<p align="center">
<img src="https://github.com/slanatech/swagger-stats/blob/master/screenshots/logo-c-ssm.png?raw=true" alt="swagger-stats"/>
</p>

# swagger-stats


####  [http://swaggerstats.io](http://swaggerstats.io) | [Documentation](http://swaggerstats.io/docs.html) | [API DOC](http://swaggerstats.io/apidoc.html) | [API SPEC](http://swaggerstats.io/sws-api-swagger.yaml)

[![Build Status](https://travis-ci.org/slanatech/swagger-stats.svg?branch=master)](https://travis-ci.org/slanatech/swagger-stats)
[![Dependencies](https://david-dm.org/slanatech/swagger-stats.svg)](https://david-dm.org/slanatech/swagger-stats)
[![Coverage Status](https://coveralls.io/repos/github/slanatech/swagger-stats/badge.svg?branch=master&dummy)](https://coveralls.io/github/slanatech/swagger-stats?branch=master&dummy)
[![Tested on APIs.guru](https://api.apis.guru/badges/tested_on.svg)](https://APIs.guru)
[![npm version](https://badge.fury.io/js/swagger-stats.svg)](https://badge.fury.io/js/swagger-stats)



## Telemetry for your APIs

###Trace API calls and Monitor API performance, health and usage statistics in Node.js Microservices

`swagger-stats` traces REST API requests and responses in Node.js express Microservices, and collects statistics for API Operations.
swagger-stats detects API operations based on express routes, such as `/pet/:petId`. You may also provide [Swagger (Open API) specification](https://swagger.io/specification/), 
and swagger-stats will match API requests with API Operations defined in swagger specification. 
swagger-stats exposes statistcs and metrics per API Operations such as`/pet/:petId`, or `/pet/{petId}`
 
       
With data collected by swagger-stats you may spot problematic API endpoints, see where most of errors happens, 
catch long-running requests, analyze details of last errors, observe trends in requests volumes.



![swagger-stats Prometheus Dashboard](screenshots/prometheus-dashboard.png?raw=true)

TBD TBD 

![swagger-stats Built-In Monitoring](screenshots/uiscreens.gif?raw=true)


**swagger-stats** monitors REST API requests and responses in node express apps and collects statistics.
swagger-stats detects and monitors statistics for API operations based on express routes or Swagger specification.
You may retrieve statistics using swagger-stats API, as well as you may monitor statistics using built-in UI front end. 
With data collected by swagger-stats you may spot problematic API endpoints, see where most of errors happens, 
catch long-running requests, analyze details of last errors, observe trends in requests volumes.

 
**swagger-stats** collects:
* CPU and Memory Usage of Node process
* Counts of requests and responses(total and by response class), processing time (total/avg/max), 
content length(total/avg/max) for requests and responses, rates for requests and errors. 
This is baseline set of metrics. 
* Statistics by Request Method: baseline metrics collected for each request method
* Timeline: baseline metrics collected for each 1 minute interval during last 60 minutes. Timeline helps you to analyze trends.
* Errors: count of responses per each error code, top "not found" resources, top "server error" resources
* Last errors: request and response details for the last 100 errors (last 100 error responses)
* Longest requests: request and response details for top 100 requests that took longest time to process (time to send response)
* Tracing: Request and Response details - method, URLs, parameters, request and response headers, addresses, start/stop times and processing duration, matched API Operation info
* API Statistics: baseline metrics per each API Operation. API operation is path and method combination from the swagger spec. 
Swagger specification is optional. swagger-stats will detect and monitor API operations based on express routes. 
* API Operation parameters metrics: parameter passed count, mandatory parameter missing count (for API Operation parameters defined in swagger spec)


## How to Use 


### Install 

```
npm install swagger-stats --save
```

### Enable swagger-stats middleware in your app

```javascript
var swStats = require('swagger-stats');
var apiSpec = require('swagger.json');
app.use(swStats.getMiddleware({swaggerSpec:apiSpec}));
```

See /examples for sample apps 

### Get stats with API

```
$ curl http://<your app host:port>/swagger-stats/stats
{
  "startts": 1501647865959,
  "all": {
    "requests": 7,
    "responses": 7,
    "errors": 3,
    "info": 0,
    "success": 3,
    "redirect": 1,
    "client_error": 2,
    "server_error": 1,
    "total_time": 510,
    "max_time": 502,
    "avg_time": 72.85714285714286,
    "total_req_clength": 0,
    "max_req_clength": 0,
    "avg_req_clength": 0,
    "total_res_clength": 692,
    "max_res_clength": 510,
    "avg_res_clength": 98,
    "req_rate": 1.0734549915657108,
    "err_rate": 0.4600521392424475
  },
  "sys": {
    "rss": 59768832,
    "heapTotal": 36700160,
    "heapUsed": 20081776,
    "external": 5291923,
    "cpu": 0
  },
  "name": "swagger-stats-testapp",
  "version": "0.90.1",
  "hostname": "hostname",
  "ip": "127.0.0.1"
}
```

Get more statistics:

```
$ curl http://<host:port>/swagger-stats/stats?fields=method
$ curl http://<host:port>/swagger-stats/stats?fields=timeline
$ curl http://<host:port>/swagger-stats/stats?fields=lasterrors
$ curl http://<host:port>/swagger-stats/stats?fields=longestreq
$ curl http://<host:port>/swagger-stats/stats?fields=apidefs
$ curl http://<host:port>/swagger-stats/stats?fields=apistats
$ curl http://<host:port>/swagger-stats/stats?fields=errors
```

Get exactly what you need:

```
$ curl http://<host:port>/swagger-stats/stats?fields=method,timeline
$ curl http://<host:port>/swagger-stats/stats?fields=lasterrors,longestreq
$ curl http://<host:port>/swagger-stats/stats?fields=apiop&method=GET&path=/v2/pet/{petId}
$ curl http://<host:port>/swagger-stats/stats?fields=all
$ curl http://<host:port>/swagger-stats/stats?fields=*
```

[swagger-stats API specification](http://swaggerstats.io/sws-api-swagger.yaml)

Take a look at **[API Documentation](http://swaggerstats.io/apidoc.html)** for more details 


### User Interface 

Swagger-stats comes with built-in User Interface. Navigate to /swagger-stats/ui in your app to start monitoring right away
   
```
http://<your app host:port>/swagger-stats/ui
```

##### Key metrics

![swagger-stats bundled User Interface](screenshots/metrics.png?raw=true)

##### Timeline

![swagger-stats bundled User Interface](screenshots/timeline.png?raw=true)

##### Request and error rates 

![swagger-stats bundled User Interface](screenshots/rates.png?raw=true)

##### API Operations 

![swagger-stats bundled User Interface](screenshots/apitable.png?raw=true)

##### Stats By Method

![swagger-stats bundled User Interface](screenshots/methods.png?raw=true)


## Updates 

#### v0.90.1

* [feature] Added CPU and Memory Usage Stats and monitoring in UI [#8](https://github.com/slanatech/swagger-stats/issues/8)  


## Enhancements and Bug Reports

If you find a bug, or have an enhancement in mind please post [issues](https://github.com/slanatech/swagger-stats/issues) on GitHub.

## License
 
MIT
