### airquality-datastore

AirQuality DataStore - ScyllaDB


### Create data schemas

````

CREATE KEYSPACE IF NOT EXISTS airquality WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1};

CREATE TABLE airquality.observation (key text PRIMARY KEY, observed text);
 
CREATE TABLE airquality.reading (readingID TEXT,
   avg_ozone DOUBLE, 
   min_ozone DOUBLE, 
   max_ozone DOUBLE, 
   avg_pm10 DOUBLE, 
   min_pm10 DOUBLE, 
   max_pm10 DOUBLE, 
   avg_pm25 DOUBLE, 
   min_pm25 DOUBLE, 
   max_pm25 DOUBLE,
   local_time_zone TEXT,
   state_code TEXT,
   reporting_area TEXT,
   hour_observed INT,
   date_observed TEXT,
   latitude FLOAT,
   longitude FLOAT,
PRIMARY KEY (reporting_area));

INSERT INTO airquality.reading (readingID, 
 avg_ozone ,min_ozone ,max_ozone ,avg_pm10 ,min_pm10 ,max_pm10 ,avg_pm25 ,min_pm25 ,max_pm25 ,local_time_zone ,state_code ,reporting_area ,   hour_observed ,date_observed,latitude ,longitude) 
 VALUES ('TEST',20,19,21,15,14,16,25,20,30,'EST','NJ', 'Newark', 10,'2022-06-08',37.48,-122.22);

select * from airquality.reading;

desc airquality.reading;

desc airquality.observation;

select * from airquality.observation;

````

### ScyllaDB Setup

````
sudo systemctl start docker
docker run -p 9042:9042 --name devscylla --hostname devscylla -d scylladb/scylla --smp 1
docker container restart devscylla
docker ps
docker logs devscylla  | tail
docker exec -it devscylla nodetool status
docker exec -it devscylla cqlsh

bin/pulsar-admin sink stop --name scylla-airquality-sink --namespace default --tenant public

bin/pulsar-admin sinks delete --tenant public --namespace default --name scylla-airquality-sink

bin/pulsar-admin sinks create --tenant public --namespace default --name "scylla-airquality-sink" --sink-type cassandra --sink-config-file conf/scyllaairquality.yml --inputs aq-ozone,aq-pm10,aq-pm25

bin/pulsar-admin sinks status --tenant public --namespace default --name scylla-airquality-sink

````

### ScyllaDB Cloud Connect

````
cqlsh -u scylla -p aPasswordThatIsLong 1.2.3.4
````

### Sink Configuration

````

configs:
    roots: "172.17.0.2:9042"
    keyspace: "airquality"
    columnFamily: "observation"
    keyname: "key"
    columnName: "observed"
    
````

### Example JSON Data

````

key:[45f12e82-326b-41de-8ea6-322cb00826a8], properties:[], content:{"dateObserved":"2022-06-08 ","hourObserved":11,"localTimeZone":"PST","reportingArea":"Redwood City","stateCode":"CA","latitude":37.48,"longitude":-122.22,"parameterName":"PM2.5","aqi":20,"category":{"number":1,"name":"Good","additionalProperties":{}},"additionalProperties":{}}

````


### References

* https://github.com/tspannhw/ScyllaFLiPSTheStream
* https://medium.com/@aamine/spring-data-for-cassandra-a-complete-example-3c6f7f39fef9
* https://dzone.com/articles/getting-started-with-spring-data-cassandra

