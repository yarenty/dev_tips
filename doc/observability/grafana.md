# grafana

To start grafana now and restart at login:

brew services start grafana

Or, if you don't want/need a background service you can just run:

/usr/local/opt/grafana/bin/grafana server --config /usr/local/etc/grafana/grafana.ini --homepath /usr/local/opt/grafana/share/grafana --packaging\=brew cfg:default.paths.logs\=/usr/local/var/log/grafana cfg:default.paths.data\=/usr/local/var/lib/grafana cfg:default.paths.plugins\=/usr/local/var/lib/grafana/plugins
