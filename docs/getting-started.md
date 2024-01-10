# 🔎 Querying

In this section, we'll learn how to query and filter our ingested data. Let's get __qryn__!

Let's **explore** our data using [Grafana](guide/datasources.md) and its _amazing visualizations_

<!-- tabs:start -->

#### ** Loki **

[arcade-loki](https://demo.arcade.software/GKaq4SJRBPYyCZgoWBoM?embed ':include :type=iframe width=100% height=550px border=0')

### [🔎 Start using LogQL](logs/query) 

Connect Grafana to **qryn** using the _built-in_ `Loki` datasource


#### Manual Provisioning
* Log into your Grafana or Grafana Cloud instance
* Browse to `Configuration` > `Data Sources` via the ⚙️ icon on the left sidebar
* Click the big `+ Add` data source button
* Choose `Loki` from the list
  * _Optional_ configure derivate fields to Tempo
* Use your **qryn** URL and `save`

?> qryn:oss uses port **3100**, qryn:cloud uses port **3200**

-----

![image](https://user-images.githubusercontent.com/1423657/184536845-31638c7f-30bd-4416-ae99-4486024367f8.png)


#### Auto Provisioning
Use the following sample as reference for provisioning your datasource:
```
# grafana config file
apiVersion: 1
datasources:
  - name: Loki-qryn
    type: loki
    access: proxy
    uid: loki
    url: http://qryn:3100
    editable: true
    jsonData:
      derivedFields:
        - datasourceUid: tempo
          matcherRegex: ".*/traces/(?\u003ctraceId\u003e.*)"
          name: "traceId"
          url: "${__value.raw}"
        - datasourceUid: tempo
          matcherRegex: "^.*?traceI[d|D]=(\w+).*$"
          name: TraceID
          url: "${__value.raw}"
```

#### ** Prometheus **

[arcade-prom](https://demo.arcade.software/r2A4ia93hPs96pyiaFQx?embed ':include :type=iframe width=100% height=550px border=0')

### [🔎 Start using PromQL](metrics/query)

Connect Grafana to **qryn** using the _built-in_ `Prometheus` datasource

#### Manual Provisioning
* Log into your Grafana or Grafana Cloud instance
* Browse to `Configuration` > `Data Sources` via the ⚙️ icon on the left sidebar
* Click the big `+ Add` data source button
* Choose `Prometheus` from the list
* Use your **qryn** URL and `save`

?> qryn:oss uses port **3100**, qryn:cloud uses port **3200**

-----

![image](https://user-images.githubusercontent.com/1423657/184536921-b3ff7fab-f71f-4f07-93c3-40a5b9915c64.png)

#### Auto Provisioning
Use the following sample as reference for provisioning your datasource:
```
# grafana config file
apiVersion: 1
datasources:
  - name: Prom-qryn
    type: prometheus
    access: proxy
    uid: prometheus
    url: http://qryn:3100
    basicAuth: false
    jsonData:
     graphiteVersion: "1.1"
     tlsAuth: false
     tlsAuthWithCACert: false
```

#### ** Tempo **

[arcade-tempo](https://demo.arcade.software/G43G6ja4CHJYpF9rc3V2?embed ':include :type=iframe width=100% height=550px border=0')

### [🔎 Start using Tempo](telemetry/query)

Connect Grafana to **qryn** using the _built-in_ `Tempo` datasource

#### Manual Provisioning
* Log into your Grafana or Grafana Cloud instance
* Browse to `Configuration` > `Data Sources` via the ⚙️ icon on the left sidebar
* Click the big `+ Add` data source button
* Choose `Tempo` from the list
  * _Optional_ configure Loki search datasource
* Use your **qryn** URL and `save`

?> qryn:oss uses port **3100**, qryn:cloud uses port **3200/tempo**

-----

![image](https://user-images.githubusercontent.com/1423657/184536886-7a1eb428-9671-4a36-a93b-173fcfcd775d.png)

#### Auto Provisioning
Use the following sample as reference for provisioning your datasource:
```
# grafana config file
apiVersion: 1
datasources:
  - name: Tempo-qryn
    type: tempo
    access: proxy
    uid: tempo
    url: http://qryn:3100
    jsonData:
      nodeGraph:
        enabled: true
      tracesToLogs:
        datasourceUid: loki
        filterByTraceID: false
        spanEndTimeShift: "2000ms"
        spanStartTimeShift: "-2000ms"
        tags: ['job']
```

#### ** Flux **

### [🔎 Start using Flux](https://docs.influxdata.com/flux/v0.x/)

Connect Grafana to **qryn** using the _built-in_ `Flux` datasource

`Flux` is a lightweight scripting language for querying databases and working with data

#### Manual Provisioning
* Log into your Grafana or Grafana Cloud instance
* Browse to `Configuration` > `Data Sources` via the ⚙️ icon on the left sidebar
* Click the big `+ Add` data source button
* Choose `InfluxDB` from the list
* Choose `Flux` as the query language
* Use your **qryn** URL and `save`

#### Auto Provisioning
Use the following sample as reference for provisioning your datasource:
```
# grafana config file
apiVersion: 1
datasources:
 - name: Flux
    type: influxdb
    uid: flux
    version: 2
    access: proxy
    url: http://fluxpipe:8086
    basicAuth: false
    jsonData:
      defaultBucket:
      httpMode: POST
      organization: qryn
      version: Flux

```

### ** View **

### [🔎 Start using View](https://github.com/metrico/qryn-view)

Connect to **qryn** using **view**, our _built-in_ data exploration tool!

<img src="https://user-images.githubusercontent.com/1423657/200136242-f4133229-ee7c-45e0-8228-8734cf56140a.gif">

_**view** is a browser app, and requires no installation or server components._

#### Supported Protocols
* LogQL _to explore Logs and Metrics_
* PromQL _to explore Metrics_
* Tempo _to explore Traces_
* Flux _for Anything_


<!-- tabs:end -->
