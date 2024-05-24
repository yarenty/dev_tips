# node_exporter

When run from `brew services`, `node_exporter` is run from
`node_exporter_brew_services` and uses the flags in:
/usr/local/etc/node_exporter.args

To start node_exporter now and restart at login:
brew services start node_exporter
Or, if you don't want/need a background service you can just run:
/usr/local/opt/node_exporter/bin/node_exporter_brew_services