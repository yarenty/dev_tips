# redpanda

Redpanda Keeper (rpk) is Redpanda's command line interface (CLI)
utility. The rpk commands let you configure, manage, and tune
Redpanda clusters. They also let you manage topics, groups,
and access control lists (ACLs).
Start a three-node docker cluster locally:

    rpk container start -n 3

Interact with the cluster using commands like:

    rpk topic list

When done, stop and delete the docker cluster:

    rpk container purge

For more examples and guides, visit: https://docs.redpanda.com

fish completions have been installed to:
/usr/local/share/fish/vendor_completions.d