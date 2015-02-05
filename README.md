# Introduction

My changes to make it work in Linux, no macs here. 

Slides are at [NodeJS in Production](https://speakerdeck.com/wolfeidau/nodejs-in-production).

# Setup

* Install influxdb.

```
apt-get install influxdb
```

* Configuration of influxdb will be correctly set to leveldb, but do check /opt/influxdb/current/config.toml.

```
vim /opt/influxdb/current/config.toml
```

* Grab a copy of statsd.

```
git clone https://github.com/etsy/statsd
```

* Install the influxdb plugin.

```
cd statsd
 npm install statsd-influxdb-backend
```

* Use the following configuration file as influxdbConfig.js.

```js
{
  port: 8125
, backends: [ "statsd-influxdb-backend" ]
, influxdb: {
    host: '127.0.0.1'
    , port: 8086
    , database: 'NodeJS'
    , username: 'root'
    , password: 'root'
  },
  debug: true,
  log: {
    level: "LOG_DEBUG"
  }
}
```

* Grab a copy of graphana from their website, not github version.

```
wget http://grafanarel.s3.amazonaws.com/grafana-1.9.1.tar.gz
tar zxfv grafana-1.9.1.tar.gz
```

* Install my NodeJS dashboard.

```
cp -p NodeJS.json grafana-1.9.1/app/dashboards
```

* Start all the things.

```
service influxdb start
cd statsd
node stats.js influxdbConfig.js &
cd grafana-1.9.1
python -m SimpleHTTPServer
```

Load up [http://linux-box:8000/#/dashboard/file/NodeJS.json](http://linux-box:8000/#/dashboard/file/NodeJS.json).

I cannot assume you have your browser running in the linux box, so I will assume you are in a widnows desktop, SSHed into your linux server.


