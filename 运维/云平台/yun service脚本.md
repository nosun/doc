## 云服务服务脚本

```shell
# !/bin/bash

# chkconfig: - 90 90
# description: Tcp service manager
# processname: yun_work # this can configure by user at project dir
# path:   /www/skylink
# script: start.php
# pidfile: /var/run/yun/yun.pid

### BEGIN 

PHP=/usr/bin/php
PATH=/www/yun_new
SCRIPT=start.php
PIDFILE=/tmp/yun_work.master.pid

case "$1" in

   start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, process is already running or crashed."
        else
                echo "Starting TCP server..."
                $PHP $SCRIPT start
        fi
        if [ "$?"="0" ]
        then
                echo "TCP server is running..."
        fi
        ;;

    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "process is not running."
        else
                PID=$(cat $PIDFILE)
                echo "Stopping..."
                $PHP $SCRIPT stop
                while [ -x $PIDFILE ]
                do
                        echo "Waiting for TCP server to shutdown..."
                        sleep 1
                done
                echo "TCP server stopped"
        fi
        ;;

    status)
        if [ ! -f $PIDFILE ]
        then
                echo "process is not running."
        else
                $PHP $SCRIPT status
        fi
        ;;

    restart)
        ${0} stop
        ${0} start
        ;;

	*)
        echo "Usage: /etc/init.d/yun {start|stop|restart}" >&2
        exit 1
esac

```