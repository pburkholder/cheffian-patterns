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
* Roles
* Environments
* Databags
  * Are they really a code smell?
  * 
* "Dynamic execution": What do I mean here? Most cookbook's consume attributes and apply those attributes to resources. But what if that attribute value is not known at cookbook creation time or chef-client initiation time?
  * Here's one set of answers I generate : There are four ways to feed data into a Chef cookbook recipe, each with their following use cases:
    * From a cookbook attribute: This is probably the go-to answer for most Chef coders, but attributes should only be used as an API into the cookbook. If we don't want these data to change at the role or environment level, then we shouldn't use an attribute.
    * From a data bag: Data bags are global, and not subject to the vagaries of attribute precedence levels, but they're also unversioned and don't leave an audit trail.
    * From code internal to the cookbook: This is my preference here, since the data are tied closely to the functionality of the cookbook, and we have the VCS and Chef versioning tools to track changes to these data.
    * From an Ohai plugin on the node: If the data were being pulled from a database or other external source, this would also be a feasible option.
  * But the above only addresses data that are known before the client run starts, what about data that aren't available until the run starts?
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


The Chef Ecosystem
-------------------
* Commmunity cookbooks
  * How to assess and consume them
  * How to wrap them
  * When to rewind and when to give up
* Running Supermarket internally
  * Exposes Berkshelf API for berks 3.0
* Communication and community building
  * Automation Champions
* Getting and offering help
  * Pull requests and code reviews
* How drive up  end user engagement beyond IT/OPs?
  * Hold a 'Meetup' internally for Cheffians to share use cases, success stories, garner enthusiasm
  * Hold regular brownbags, cookbook sessions
  * Share success stories
  * Start small


Disclaimer
==========

These views are my own and are not those of Chef, Inc.
Copyright 2014 Peter Burkholder
