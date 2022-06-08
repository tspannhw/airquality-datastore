### airquality-datastore

AirQuality DataStore - ScyllaDB


### Create data schemas

````


CREATE KEYSPACE IF NOT EXISTS airquality WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1};

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
PRIMARY KEY (readingID));

INSERT INTO airquality.reading (readingID, 
 avg_ozone ,min_ozone ,max_ozone ,avg_pm10 ,min_pm10 ,max_pm10 ,avg_pm25 ,min_pm25 ,max_pm25 ,local_time_zone ,state_code ,reporting_area ,   hour_observed ,date_observed,latitude ,longitude) 
 VALUES ('TEST',20,19,21,15,14,16,25,20,30,'EST','NJ', 'Newark', 10,'2022-06-08',37.48,-122.22);

select * from airquality.reading;

desc airquality.reading;

````

### Example JSON Data

````

key:[45f12e82-326b-41de-8ea6-322cb00826a8], properties:[], content:{"dateObserved":"2022-06-08 ","hourObserved":11,"localTimeZone":"PST","reportingArea":"Redwood City","stateCode":"CA","latitude":37.48,"longitude":-122.22,"parameterName":"PM2.5","aqi":20,"category":{"number":1,"name":"Good","additionalProperties":{}},"additionalProperties":{}}

````


### References

* https://github.com/tspannhw/ScyllaFLiPSTheStream

