# Lecture Plan

1. GCP IAM
2. GCP redis storage
3. GCP Storage
4. GCP BigQuery


---

# IAM (Identity and access management)

org --> folders--> project --> resource

![policy inheritance](./images/policy-inheritance.svg)


Google Account: any user email address that is used to access google cloud

Service Account: for services or applications

Google Group: collection of google accounts and service accounts

Google workspace account: collection of users specific to org

Cloud Identity domain: similar to google workspace account but does not provide workspace access

All Authnticated users: all users and service accounts 

All users: anyone on the internet

1. Principal : user or service account
2. Role: collection of permisions
    - basic
    - predefined
    - custom
3. Permission: operation
4. Resource: any gcp service like compute, storage, gke, bigquery
5. Allow policy: used to grant roles to users

IAM policy APIs

1. setIamPolicy
2. getIamPolicy
3. testIamPemissions


# Redis memorystore

memorystore: redis and memcached

memcached: simple, volatile cache server. key value suitable for simple objects and strings upto 1MB.

redis: cache memory, upto 512 MB, persistance, cluster support.

shards: 1 primary + 0-2 replicas

   - high availability
   - disaster recovery
   - low latency

redis cluster: set of shards

redis tier: basic and standard tiers


1. basic: primary, Ephemeral cache, 
2. standard: primary + replica
3. standard + read replica: standard + multiple replaicas

best practices: 


few design patterns: 

1. Lazy Loading/ Cache aside/ Lazy Polulation
2. Write-through
3. TTL


use cases: DNS, OS, CDN and webpage caching, session management, database caching.

# Storage

bucket: containers that hold the data. specific to region or multi region. globally unique name.


[storage quotas](https://cloud.google.com/storage/quotas)


**storage classes:**


1. standard: frequently accessed
2. nearline: infrequently accssed, 30 day minimum storage, low cost. (once a month access)
3. coldline: very low cost, highly durable, infrequntky accessed data. 90 days minimum (once a quater retreival)
4. archive: lowest cost, highly durable storage, backup, available in milli seconds, 1 year min storage

other: multi regional, regional, durable reduced availability storage.


**implementation:** bucket creation, upload and delete objects and delete bucket, bucket IAM access : Uniform access and ACLs.



# Big Query


DWH vs Data Lakes


DWH: enterprise system used for storage and analysis of structured and semi-structured data from different sources. vertically scaled

Data lakes: store, process and secure large ammounts of structure, semi structured and unstructured data. horizantally scaled


DWH architecture:

sourcing--> stagging --> storage
 --> Integration --> metadata --> data access 

- ETL (extract, transform and load)


Vendors:

- gcp bigquery, aws redshift, snowflake, acure synapse alalytics, IBM Db2 warehouse

BigQuery: fully managed, pat-byte scale, serverless dwh from gcp.


datasets: collection of tables

tables: records organised in rows. table is associated to schema (similar to SQL)

    - standard
    - external

views: vrtual table defined by SQL query.

materialized view: cache the results of a query

