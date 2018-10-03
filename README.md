# Percona with Grafana
### Percona client installation
Reference : [Installing PMM Client on Ubuntu](https://www.percona.com/doc/percona-monitoring-and-management/deploy/client/apt.html#install-client-apt)
1.Configure Percona repositories as described in Percona Software Repositories Documentation.
- Fetch the repository package:
```sh
wget https://repo.percona.com/apt/percona-release_0.1-6.$(lsb_release -sc)_all.deb
```
- Install the repository package:
```sh
sudo dpkg -i percona-release_0.1-6.$(lsb_release -sc)_all.deb
```
- Update local apt cache:
```sh
sudo apt-get update
```
- Make sure that Percona packages are available:
```sh
sudo apt-cache search percona
```

2.Install the PMM Client package:
```sh
apt-get install pmm-client
```

### Percona Server installation via Docker
Reference : [Percona installation on Docker](https://www.percona.com/doc/percona-monitoring-and-management/deploy/server/docker.html).

```sh
sudo docker pull percona/pmm-server:latest
```
```sh
sudo docker create    -v /opt/prometheus/data    -v /opt/consul-data    -v /var/lib/mysql    -v /var/lib/grafana    --name pmm-data    percona/pmm-server:latest /bin/true
```
```sh
sudo docker run -d -p <port>:80 --volumes-from pmm-data --name pmm-server -e SERVER_USER=<username> -e SERVER_PASSWORD=<pwd> -e METRICS_MEMORY=131072 -e METRICS_RESOLUTION=5s --restart always percona/pmm-server:latest
```
- pmm-admin configuration
```sh
sudo pmm-admin config --server <server-ip>:<port> --server-user <username> --server-password <pwd>
```
- adding clients
```sh
sudo pmm-admin add mysql --user <username> --password <pwd> --host <hostname> <alias>
```
- Checking
```sh
sudo pmm-admin list
sudo pmm-admin check-network
```
### Grafana Installation
Follow the steps  [Grafana Installation on Ubuntu 14.04](http://docs.grafana.org/installation/debian/).

#### Add prometheus data source
```sh
Name: Prometheus
Type: Prometheus
url: http://<server-ip>:<port>/prometheus/
```
### Git Repository for templates
https://github.com/percona/grafana-dashboards
#### Note
While creating RDS DB user grant mysql as well as replication permissions.
