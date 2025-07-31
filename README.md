# airqualitydata
Air Quality Data from OpenAQ into Snowflake OpenFlow (Apache NiFi), Streamnative (Apache Kafka), PostgreSQL (Crunchy Bridge)


#### Postgresql Table

````

DROP TABLE public.air_quality_data;

GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO application;

GRANT TRUNCATE, SELECT, TRIGGER, INSERT, UPDATE, MAINTAIN, DELETE, REFERENCES ON TABLE public.air_quality_data TO application;


CREATE TABLE AIR_QUALITY_DATA (
    owner VARCHAR(255),
    city VARCHAR(512),
    timezone VARCHAR(255),
    latitude FLOAT,
    periodlabel VARCHAR(255),
    locality VARCHAR(512),
    parameterName VARCHAR(255),
    datetimefrom VARCHAR(255),
    datetimeLast VARCHAR(255),
    uuid VARCHAR(255),
    name VARCHAR(512),
    id VARCHAR(255),
    state VARCHAR(25),
    value FLOAT,
    datetimeto VARCHAR(255),
    longitude FLOAT
);
````

#### Snowflake Table and Snowflake Managed Iceberg Table

````
CREATE ICEBERG TABLE air_quality_data (
    owner STRING,
    city STRING,
    timezone STRING,
    latitude FLOAT,
    periodlabel STRING,
    locality STRING,
    parameterName STRING,
    datetimefrom TIMESTAMP,
    datetimeLast TIMESTAMP,
    uuid STRING,
    name STRING,
    id STRING, 
    state STRING,
    value FLOAT,
    datetimeto TIMESTAMP,
    longitude FLOAT
)
CATALOG = 'SNOWFLAKE'
EXTERNAL_VOLUME = 'TRANSCOM_TSPANNICEBERG_EXTVOL'
BASE_LOCATION = 'airqualitydata/';

CREATE TABLE air_quality_data_sf (
    owner STRING,
    city STRING,
    timezone STRING,
    latitude FLOAT,
    periodlabel STRING,
    locality STRING,
    parameterName STRING,
    datetimefrom TIMESTAMP,
    datetimeLast TIMESTAMP,
    uuid STRING,
    name STRING,
    id STRING, 
    state STRING,
    value FLOAT,
    datetimeto TIMESTAMP,
    longitude FLOAT
);

select * from air_quality_data_sf;

select AVG(value), (owner || ' - ' || locality || ' - ' || parametername) as sensor, datetimeto
from air_quality_data_sf 
GROUP by datetimeto, sensor
order by datetimeto desc;


````
