
Lamont wrote:  

I'd advise for starting out that you begin with a role_base cookbook with multiple recipes in it.  The structure then looks like:


metadata.rb
=======

name "role_base"
depends "openssh"
depends "resolver"

attributes/default.rb
=============

# ssh settings
default["openssh"]["server"]["port"] = 22

# resolver settings
default[:resolver][:nameservers] = [ "8.8.8.8", "8.8.4.4" ]

recipes/default.rb
===========

include_recipe "openssh"
include_recipe "resolver"
include_recipe "#{cookbook_name}::packages"

recipes/packages.rb
============

%w{lsof tcpdump zsh dmidecode}.each do |pkg|
  package pkg
end

I would still use an actual base role:

roles/base.json
==========
{
  "name": "base",
  "description": "It's all about the base",
  "run_list": [
    "recipe[role_base]"
  ]
}

This keeps things fairly tidy.  You still have a role in your run_list but its pretty much a no-op pointer to the role cookbook.  The role cookbook can serve multiple purposes and you should break it up into sub-recipes that each do one thing with a default recipe that runs them all.  You can start to break out other cookbooks from the base cookbook and make it all smaller and more composable, but I think you should do that as you decide that you start wanting to do more test-driven-development or one of the recipes starts to give you obvious pain.  One thing that you cannot do with this approach is have multiple base roles that share code between them.  You cant have a single cookbook with multiple roles in it (the attributes and libraries files don't really allow it -- whenever you include a cookbook you include all its attribute files so there no way to switch behavior there between different kinds of hosts).

For your apps, I'd have simpler roles, they should have one default recipe and should depend on library cookbooks that export LWRPs to set up applications.  You can use cookbooks like application_ruby to provide those, or you can roll your own LWRPs for "what it means to be an app at MYORG"

So:

cookbooks/role_foo/metadata.rb
====================

name "role_foo"
depends "application_ruby"
depends "build-essentials"

cookbooks/role_foo/attributes/default.rb
==========================

default['build-essential']['compile_time'] = true

cookbooks/role_foo/recipes/default.rb
========================

include_recipe "build-essentials"

package "libv8-dev"

# ... more stuff ...

application "foo" do
  path "/srv/foo"
  owner "root"
  group "root"
  rails do
     # ... stuff ...
  end
end

You'd probably call this an "application" cookbook, but its also a "wrapper" cookbook around the build-essentials cookbook, which I think is where the terminology can wind up tying your brain into knots.   The important point is that your application needs:

- the compiler toolchain installed (at compile_time for the sake of argument [*])
- the libv8-dev package installed
- a bunch of typical stuff you do to install and deploy a rails app in you organization

If you start duplicating a lot of code in your application cookbooks you can start to extract those out to either other cookbooks which may be recipes or may be LWRPs so that you might wind up with a 'base' application cookbook that all your application cookbooks depend upon and use include recipe:

cookbooks/app_base/metadata.rb
=====================

name "app_base"
depends "application_ruby"
depends "build-essentials"

cookbooks/app_base/attributes/default.rb
==========================

default['build-essential']['compile_time'] = true

cookbooks/app_base/recipes/default.rb
========================
include_recipe "build-essentials"

package "libv8-dev"

cookbooks/role_foo/metadata.rb
====================

name "role_foo"
depends "app_base"

cookbooks/role_foo/recipes/default.rb
========================

include_recipe "role_foo::default.rb"

application "foo" do
  [....stuff...]
end

As "app_base" grows larger you can keep your role_foo (and role_bar and role_baz) cookbooks smaller now.

And there's no pattern which you should definitely adopt.  For small needs what I've outlined here will be more than enough.  As you grow you probably need to refactor and wrap more.   At the same time, for my own use I wound up overengineering my small personal needs and had a few dozen cookbooks composing my base role and recently refactored them into this one-base-cookbook-many-recipes pattern which greatly simplified things for me.

I do suggest that your run_list still looks like 'role[base], role[foo]' and that those are independent.  That way you can build your app in a test harness and not have to install every user account and all their dotfiles that your base role should be responsible for -- the application role should have all of its dependencies and only its dependencies.  If you slurp your whole base role into your app cookbooks then you'll miss out on being able to quickly build apps in test harnesses.

HTH

[*] also I don't think you should ever need to force build-essentials to compile-time for apps that you install so maybe thats a bad example to pick, but I couldn't find a more relevant one offhand and it nearly makes sense that you might want to do that for native gem installs.
