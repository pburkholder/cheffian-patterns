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
