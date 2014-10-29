Technically, there's no way to enforce that currently with Chef. I discussed the idea with some community members at the Chef Summit a few weeks ago, and consensus response was that forcing the server to return a particular run-list ran counter to the Chef model and would probably violate your ability to reason about your infrastructure at the least convenient times.

The generally suggested alternatives were a) auditing, b) a cron jobs, or c) non-root chef-client for the application side of your organization. In more detail:
a.1) on the server side, the Chef Analytics add-on will provide reports or alerts on nodes that are not applying certain recipes are part of their run_list
a.2) their are client-side tools to look for 'configuration drift', such as ScriptRock guardrail and Metafor. I cannot vouch for either of these products.
b) have as part of your server build a cron job to run Chef that has only the minimum mandatory run_list, then let the app developers manage the run_list of the daemon (not sure if this is really feasible, but it might work)
c) have the root owner run the chef-client cron or daemon, and a less-privileged user, to manage the part of their stack that is their area of responsibility.
