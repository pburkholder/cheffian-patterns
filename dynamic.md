# How do I get data into my Chef cookbooks/resources?

## Static data:

There are four ways to feed data into a Chef cookbook recipe, each with their following use cases:
* From a cookbook attribute: This is probably the go-to answer for most Chef coders, but attributes should only be used as an API into the cookbook. If we don't want these data to change at the role or environment level, then we shouldn't use an attribute.
* From a data bag: Data bags are global, and not subject to the vagaries of attribute precedence levels, but they're also unversioned and don't leave an audit trail.
* From code internal to the cookbook: This is my preference here, since the data are tied closely to the functionality of the cookbook, and we have the VCS and Chef versioning tools to track changes to these data.
* From an Ohai plugin on the node: If the data were being pulled from a database or other external source, this would also be a feasible option.

## Dynamic data:

But the above only addresses data that are known before the client run starts, what about data that aren't available until the run starts?



1. If the data are going to be available at the start of the Chef run, such as DB lookups, service discovery, use Ohai.
1. If the resource providing the datum needs to be available at compile time, then using the `resource.run_action()` let you execute a resource during the compile phase
1. A variable can be set at compile time and then utilized at run time(?)
1. An ephemeral datum can be stored in the node.run_state[] Mash and then accessed later with `lazy` evaluation

Here are some resources addressing the points above:

1. Fast-forward some resource actions into the compilation stage with `.run_action()` e.g. [Julian Dunn's Common Idioms post](https://www.getchef.com/blog/2013/09/04/demystifying-common-idioms-in-chef-recipes/)
2. Dynamically modifying resource methods with  the `resource(some_resource)` as in [Seth Vargo's Changing Resources at Runtime](https://sethvargo.com/changing-chef-resources-at-runtime/)
1. Delay some compilation/evaluation steps until the execution phase using `lazy` and/or `node.run_state` per [The Recipe Docs](https://docs.getchef.com/essentials_cookbook_recipes.html#node-run-state)
2. For node attributes, use [Noah Kantrowitz's Derived Attributes](https://coderanger.net/derived-attributes/) suggestion.
3. Avoid use of `execute` and `ruby_block`. Per FC014: Consider extracting long rubyblock to library (http://acrmp.github.io/foodcritic/#FC014) and `ruby_block` doesn't handle notify well.

## About rewind

Rewinding makes cookbooks hard to reason about. Generally should only be necessary when you're consuming community cookbooks that need a patch/functionality/override that has not yet been accepted upstream. When you control dependent cookbook code you generally won't need to `rewind`.

## About this document

Questions around this come up all the time. A few examples:
* http://lists.opscode.com/sympa/arc/chef/2014-10/msg00408.html
* http://lists.opscode.com/sympa/arc/chef/2014-11/msg00006.html:
  * What if you need to modify an attribute only when including a recipe? E.g., I have a third_party.rb recipe on my company_openssh cookbook that enables third_party system group to login via ssh, this is done by adding that grop to the [openssh]['allowed_groups'] attribute. I cant do this on attribute files because that way, the group access is always allowed. (OR): I want to open 443 on my node firewall only when i enable the http_ssl, by including that recipe on a securewebserer role
* http://lists.opscode.com/sympa/arc/chef/2014-11/msg00092.html:
  * LG: For what you're trying to do this is probably the worst available option. If you need to pass attributes from one recipe to another the first choice is using attribute files (particularly if they are consumed by later resources and you're not trying to dep inject computed attributes into earlier recipes). To pass state between recipes you can also use the node.run_state instead of using attributes. And if all you are doing is writing some DSL sauce around setting attributes you should probably use a definition (compile time macro) instead of a provider. The other direction you could go is to not consume the attributes directly in compile mode in recipes and wrap the later consumers of the node attributes in a provider in which case they'll also be called at converge time after the earlier provider sets the value. You can also use lazy {} on the later attribute, although if you adopt this pattern generally like it seems like you might be trying to do this tends to wind up with lazy {} everywhere in your recipes which bypasses the resource input validation (and in some cases we have bugs where the input validation on the resource fails on a lazy block). 
