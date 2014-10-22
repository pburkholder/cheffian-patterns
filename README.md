chef-patterns
=============

Chef Patterns and Anti-patterns

Chef has no "best practices" and many good practices are still emerging, or are yet to be built. Nonetheless, Chef can do more to steer users towards common pattern and away from anti-patterns. This repo is stub of what that might look like.

Contents
========

* Attributes
  * Lazy attributes: Use a variable if you can
  * Naming/Namespacing
  * Scoping
  * Arrays are to be avoided at all costs
  * Use 'default' except when you don't
  * When to use run_state
* Workflows, cookbook promotion and versioning
  * knife-spork
  * berks apply
  * CI/CD plugins
* Catching errors
  * Analytics
  * Handlers
* Commmunity cookbooks
  * How to assess and consume them
  * How to wrap them
  * When to rewind and when to give up
* Server scaling and availability
* Secrets and security
* Service discovery
  * Search often lags behind reality
  * Use services that don't require discovery (e.g. Sensu instead of Nagios)
  * Complementary tools (e.g. Consul, Zookeeper)
* Roles
* Environments
* Databags
  * Are they really a code smell?

Disclaimer
==========

These views are my own and are not those of Chef, Inc.
