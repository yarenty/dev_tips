# hbase
To start hbase now and restart at login:
brew services start hbase
Or, if you don't want/need a background service you can just run:
HBASE_HOME="/usr/local/opt/hbase/libexec" HBASE_IDENT_STRING="root" HBASE_LOG_DIR="/usr/local/var/hbase" HBASE_LOG_PREFIX="hbase-root-master" HBASE_LOGFILE="hbase-root-master.log" HBASE_MASTER_OPTS=" -XX:PermSize=128m -XX:MaxPermSize=128m" HBASE_NICENESS="0" HBASE_OPTS="-XX:+UseConcMarkSweepGC" HBASE_PID_DIR="/usr/local/var/run/hbase" HBASE_REGIONSERVER_OPTS=" -XX:PermSize=128m -XX:MaxPermSize=128m" HBASE_ROOT_LOGGER="INFO,RFA" HBASE_SECURITY_LOGGER="INFO,RFAS" /usr/local/opt/hbase/bin/hbase --config /usr/local/opt/hbase/libexec/conf master start