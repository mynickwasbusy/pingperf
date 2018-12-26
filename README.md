     (                      (
     )\ )                   )\ )            (
    (()/( (          (  (  (()/(   (   (    )\ )
     /(_)))\   (     )\))(  /(_)) ))\  )(  (()/(
    (_)) ((_)  )\ ) ((_))\ (_))  /((_)(()\  /(_))
    | _ \ (_) _(_/(  (()(_)| _ \(_))   ((_)(_) _|
    |  _/ | || ' \))/ _` | |  _// -_) | '_| |  _|
    |_|   |_||_||_| \__, | |_|  \___| |_|   |_|
                    |___/

# Performance tests of java [MicroProfiles](https://microprofile.io/) and Spring-Boot

## Motivation

Docker based comparision of java MicroProfiles and Spring-Boot using simple ping REST request. Docker images are based
on images from [hotswap-docklands](https://github.com/HotswapProjects/hotswap-docklands). Spring-Boot test application 
is available at [Pingperf-spring-boot](https://github.com/HotswapProjects/pingperf-spring-boot). 

## Quick start

* run `build-X.sh` to build docker image for given microprofile
* check sending REST requests using python client (single thread) : `./perf-utils/echo_test.py`
* monitor performance : `watch "docker stats --no-stream --no-trunc && echo && ./perf-utils/ping_statistics.py"`

For more accurate measurement use jmeter with prepared [jmeter](https://github.com/HotswapProjects/pingperf/jmeter_50users.jmx) script.

## JMeter load tests results

Tests were splitted into 3 groups varying in the heap size and the number of users (clients):

- Xmx128m running single user (client)
- Xmx256m running 10 users (clients)
- Xmx256 running 50 users (clients)

### Single thread (user) / Xmx128m

|Microprofile|Start time|Docker mem usage|Throughput req/s|JAVA_OPTS|
|------------|----------|----------------|----------------|---------|
|Meecrowave 1.2.4|0.7s|194MB|4659/s|-Xmx128m|
|Open Liberty 18.04|0,7s,real ~3s|252MB|3671/s|-Xmx128m|
|Payara Micro 5.184|6s|337MB|3697/s|-Xmx128m --nocluster|
|Wildfly Swarm 2018.3.3|5s|352MB|3447/s|-Xmx128m|
|Spring-Boot 2.1.1|1.4s|216MB|4869/s|-Xmx128m|

### 10 threads (users) / Xmx256m

|Microprofile|Start time|Docker mem usage|Throughput req/s|JAVA_OPTS|
|------------|----------|----------------|----------------|---------|
|Meecrowave 1.2.4|0.7s|219MB|25121/s|-Xmx256m|
|Open Liberty 18.04|3s|298MB|33070/s|-Xmx256m|
|Payara Micro 5.184|6s|399MB|24768/s|-Xmx256m --nocluster|
|Wildfly Swarm 2018.3.3|5s|390MB|22243/s|-Xmx256m|
|Spring-Boot 2.1.1|1.4s|268MB|24908/s|-Xmx256m|

### 50 threads (users) / Xmx 256

|Microprofile|Start time|Docker mem usage|Throughput req/s|JAVA_OPTS|
|------------|----------|----------------|----------------|---------|
|Meecrowave 1.2.1|0.9s|237MB|33472/s|-Xmx256m|
|Open Liberty 17.04|4s|290MB|44194/s|-Xmx256m|
|Payara Micro 5.181|5s|386MB|36042/s|-Xmx256m --nocluster|
|Wildfly Swarm 2018.3.3|5s|413MB|40889/s|-Xmx256m|
|Spring-Boot 2.0.1|1.9s|276MB|34814/s|-Xmx256m|

## RaspberryPI

Motivation of this test is to check ability to run MicroProfile application on RaspberyPi3.

|Microprofile|Start time|Mem usage|Throughput req/s|JAVA_OPTS|
|------------|----------|----------------|----------------|---------|
|Meecrowave 1.2.0|9s||250/s||
