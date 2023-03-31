In order to monitor your Postgres DBs need to install postgresql_exporter in each db nodes.

Here are the commands:
$ mkdir /opt/postgres_exporter
$ cd /opt/postgres_exporter 
$ wget https://github.com/wrouesnel/postgres_exporter/releases/download/v0.5.1/postgres_exporter_v0.5.1_linux-amd64.tar.gz
$ tar -xzvf postgres_exporter_v0.5.1_linux-amd64.tar.gz
$ cd postgres_exporter_v0.5.1_linux-amd64

we are in /postgres_exporter_v0.5.1_linux-amd64:

$ sudo cp postgres_exporter /usr/local/bin
$ cd /opt/postgres_exporter
$ sudo vim postgres_exporter.env
# Inside env-file add new line
DATA_SOURCE_NAME="postgresql://'username':'password'@localhost:5432/?sslmode=disable"

And now create service:

$sudo vim /etc/systemd/system/postgres_exporter.service

Put configuration below inside postgres_exporter.service file and save it

[Unit]
Description=Prometheus exporter for Postgresql
Wants=network-online.target
After=network-online.target
[Service]
User=postgres
Group=postgres
WorkingDirectory=/opt/postgres_exporter
EnvironmentFile=/opt/postgres_exporter/postgres_exporter.env
ExecStart=/usr/local/bin/postgres_exporter
Restart=always 
[Install]
WantedBy=multi-user.target

Finally enable service and add it to the auto-load:

sudo systemctl daemon-reload
sudo systemctl start postgres_exporter
sudo systemctl enable postgres_exporter
