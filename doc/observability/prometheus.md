# prometheus

When run from `brew services`, `prometheus` is run from
`prometheus_brew_services` and uses the flags in:
/usr/local/etc/prometheus.args

To start prometheus now and restart at login:
brew services start prometheus
Or, if you don't want/need a background service you can just run:
/usr/local/opt/prometheus/bin/prometheus_brew_services