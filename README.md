# Initial installation procedure for [THiNX OpenSource IoT management platform](https://thinx.cloud)

## Setup Firebase Notifications

Save keyfile generated by Google to conf/fcm.json

## Customize configuration

Edit conf/conf.json (leave couchdb username and password unchanged, it will be overwritten).

## SPF/DKIM

Your domain DNS and mailserver should have valid SPF/DKIM setup to prevent activation and password-reset e-mails being rejected as spam.

## Run

    docker run -ti -e THINX_HOSTNAME='staging.example.com' -e THINX_OWNER='admin@example.com' thinx-docker

# TODOs

    sudo ufw allow 'Nginx HTTP'
    sudo systemctl start nginx

## Startup

    sudo update-rc.d mosquitto remove
    sudo nano /etc/systemd/system/mosquitto.service

    Paste and save this script
    
    ```
    [Unit]
    Description=Mosquitto MQTT Broker
    Documentation=man:mosquitto(8)
    Documentation=man:mosquitto.conf(5)
    ConditionPathExists=/etc/mosquitto/mosquitto.conf
    After=xdk-daemon.service
    
    [Service]
    ExecStart=/usr/sbin/mosquitto -c /etc/mosquitto/mosquitto.conf
    ExecReload=/bin/kill -HUP $MAINPID
    User=mosquitto
    Restart=on-failure
    RestartSec=10
    
    [Install]
    WantedBy=multi-user.target
    sudo systemctl enable mosquitto.service
    sudo reboot
    
    check if mosquitto is running
    
    sudo mosquitto -v
    ```

## Device API

    ufw allow 7442
    ufw allow 7443
    ufw allow 7444
    # and many others... (1883/8883 for mqtt, 9000/9001 for githooks,...???)
        
        
## Redis
        
    ```
    [Unit]
    Description=Redis In-Memory Data Store
    After=network.target
    
    [Service]
    User=redis
    Group=redis
    ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
    ExecStop=/usr/local/bin/redis-cli shutdown
    Restart=always
    
    [Install]
    WantedBy=multi-user.target
    ```

## Web UI

## CRONs and background jobs

## Logs

    # todo: don't use pm2 logrotate anymore!
    sudo chmod 775 thinx.log 
    root@thinx:/var/log# sudo chown syslog:adm thinx.log 

