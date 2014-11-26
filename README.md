chef-patterns
=============

Chef Patterns and Anti-patterns

Chef has no "best practices" and many good practices are still emerging, or are yet to be built. Nonetheless, Chef can do more to steer users towards common pattern and away from anti-patterns. This repo is stub of what that might look like.

Contents
========

Chef Language
-------------
* Attributes
  * Lazy attributes: Use a variable if you can
  * Naming/Namespacing
  * Scoping
  * Arrays are to be avoided at all costs
  * Use 'default' except when you don't
  * When to use run_state
  * pure_ruby always wins:
   ````ruby
   template expander_config do
    source "expander.rb.erb" # snip
    options = node['private_chef']['opscode-expander'].to_hash
    options['reindexer'] = false
    variables(options)
  end
````
* Roles
* Environments
* Databags
  * Are they really a code smell?
* Resources
  * LWRPs with Chef DSL
  * LWRPs with pure Ruby, http://docs.getchef.com/lwrp_custom_provider_ruby.html
  * HWRPs or plain old resources
*




Working with Chef
-----------------
* Workflows, cookbook promotion and versioning
  * knife-spork
  * berks apply
  * CI/CD plugins
* Catching errors
  * Analytics
  * Handlers
* Service discovery
  * Search often lags behind reality
  * Use services that don't require discovery (e.g. Sensu instead of Nagios)
  * Complementary tools (e.g. Consul, Zookeeper)
* Server scaling and availability
* Secrets and security
* CM doesn't mean you say goodbye to SSH/Winrm (sorry)
* Your pilot project
  * Get chef-client w/ empty runlist everywhere
  * chef-client to manage MOTD
  * Use chef to configure a new service (logging, monitoring) on existing nodes
  * Use chef in a Greenfield project on new nodes
  * Use chef to bring a manually managed service under configuration-management - **AntiPattern**
    * You're sure to miss something or break something. You'll need a full rebuild to be sane.
* Working with multiple Chef servers or orgs
  * https://github.com/greenandsecure/knife-block
  * ChefVM
  * Running supermarket internally
   * Exposes Berkshelf API for berks 3.0
   * Berks 3 no longer pulls from Chef server(?)
* "Enforcing run_list"
  * breaks the model -- see page for this.


The Chef Ecosystem
-------------------
* Commmunity cookbooks
  * How to assess and consume them
  * How to wrap them
  * When to rewind and when to give up
* Multi-org chef server with Supermarket for common cookbooks - what pattern to follow
* Communication and community building
  * Automation Champions
* Getting and offering help
  * Pull requests and code reviews
* How drive up  end user engagement beyond IT/OPs?
  * Hold a 'Meetup' internally for Cheffians to share use cases, success stories, garner enthusiasm
  * Hold regular brownbags, cookbook sessions
  * Share success stories
  * Start small
  * 
  

About this document
===================

Chef doesn't provide good guidance on practices once. e.g.:

* Mailing list, 26 Nov 2014:

  > Take - for example- the strategy of the cheap hosting provider DigitalOcean - They provide lots of nice and easy tutorials that not only rank quite well in Google but also slightly tie the users to their (commodity/kvm-vps) services. When someone searches for „chef best practices“ or „chef tutorials“, he will find https://learn.getchef.com/ which is just a tutorial about chef, not about solving real world problems (e.g., install and maintain a decent mailing list service in the cloud).
  > You’ll only reach people that are already sold to chef (or forced to use it) but not the unbiased user that want’s to level up in DevOps. Such users usually have a „real world problem they need to solve“ and will find „copy+paste“ tutorials  or trending „how I solved everything in 5 minutes with docker, go and nodejs“ blogposts somewhere else and will never learn the benefits of chef and its toolchain.
  > IMHO this is an excellent occasion to write a real-word application-cookbook for a mailing list/discourse-combination and use it as an example in your tutorial/docs/blog ;)


Disclaimer
==========

These views are my own and are not those of Chef, Inc.
Copyright 2014 Peter Burkholder
