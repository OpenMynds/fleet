# Systemd


## Run with systemd

Once you've verified that you can run Fleet in your shell, you'll likely want to keep Fleet running in the background and after the server reboots. To do that we recommend using [systemd](https://coreos.com/os/docs/latest/getting-started-with-systemd.html).

Below is a sample unit file, assuming a `fleet` user exists on the system. Any user with sufficient
permissions to execute the binary, open the configuration files, and write the log files can be
used. It is also possible to run as `root`, though as with any other web server it is discouraged
to run Fleet as `root`.

```

[Unit]
Description=Fleet
After=network.target

[Service]
User=fleet
Group=fleet
LimitNOFILE=8192
ExecStart=/usr/local/bin/fleet serve \
  --mysql_address=127.0.0.1:3306 \
  --mysql_database=fleet \
  --mysql_username=root \
  --mysql_password=toor \
  --redis_address=127.0.0.1:6379 \
  --server_cert=/tmp/server.cert \
  --server_key=/tmp/server.key \
  --logging_json

[Install]
WantedBy=multi-user.target
```

Once you created the file, you need to move it to `/etc/systemd/system/fleet.service` and start the service.

```
sudo mv fleet.service /etc/systemd/system/fleet.service
sudo systemctl start fleet.service
sudo systemctl status fleet.service

sudo journalctl -u fleet.service -f
```

## Making changes

Sometimes you'll need to update the systemd unit file defining the service. To do that, first open /etc/systemd/system/fleet.service in a text editor, and make your modifications.

Then, run

```
sudo systemctl daemon-reload
sudo systemctl restart fleet.service
```

<meta name="title" value="Systemd">
<meta name="pageOrderInSection" value="800">
<meta name="description" value="Keep Fleet running in the background with systemd">
<meta name="navSection" value="TBD">