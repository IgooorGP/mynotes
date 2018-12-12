# Google Cloud Storage

Allows the developer to store objects in containers called **intervals**.

## Creating a container 

When one creates a container, the user needs to specify three things:

* globally unique name;
* location;
* default storage class.

## Location

This is the location of your storage. There are some types:

* Regional location: specific geographic place such as London;

Multi-Region Name	Multi-Region Description
asia	Data centers in Asia
eu	Data centers in the European Union1
us	Data centers in the United States

* Multi regional location: large geographic area such as the US; (Georeduntant)

asia Data centers in Asia
eu	 Data centers in the European Union1
us	 Data centers in the United States

* Dual-regional: special type of multi-regional location which consists of two specific regional locations.

Dual-Region Name	Dual-Region Description
eur4	europe-north1 and europe-west4
nam4	us-central1 and us-east1

### Geo-redundancy:

Data that is geo-redundant is stored redundantly in at least two separate geographic places separated by at least 100 miles. Objects stored in multi-regional locations are geo-redundant, regardless of their storage class.

All Regional locations are at least 100 miles a part.

### Considerations for locations:

A good location balances latency, availability, and bandwidth costs for data consumers.

Use a regional location to help optimize latency, availability, and network bandwidth for data consumers grouped in the same region.

Store frequently accessed data, such as data used for analytics, as Regional Storage.

Store data typically accessed less than once a month, such as archived data, as Nearline Storage.

Store data typically accessed less than once a year, such as backup and disaster recovery data, as Coldline Storage.

Use a dual-regional location when you want similar performance advantages as regional locations but with added geo-redundancy.

Use a general multi-regional location when you want to serve content to data consumers that are outside of the Google network and distributed across large geographic areas, or when you need your data to be geo-redundant.

Store frequently accessed data as Multi-Regional Storage.

Store data typically accessed less than once a month as Nearline Storage.

Store data typically accessed less than once a year as Coldline Storage.

If you're not sure which location type to use or have no scenario in mind, use a regional location that is convenient or contains the majority of the users of your data.

### Compute Engine VM notes

Storing data in the same region as your Compute Engine VM instances can provide better performance and lower network costs. These advantages apply to both regional and dual-regional locations.
While you can't specify a Compute Engine zone as a bucket location, all Compute Engine VM instances in zones within a certain regional location have similar performance when accessing buckets in that regional location.v

## Storage Classes:

All storage classes offer low latency (time to first byte typically tens of milliseconds) and high durability. The classes differ by their availability, minimum storage durations, and pricing for storage and access.

There are four storage classes:

* Multi-Regional Storage:

Storing data that is frequently accessed ("hot" objects) around the world, such as serving website content, streaming videos, or gaming and mobile applications.

For Multi-Regional Storage data stored in dual-regional locations, you also get optimized performance when accessing Google Cloud Platform products that are located in one of the associated regions

* Regional Storage:

Storing frequently accessed data in the same region as your Google Cloud DataProc or Google Compute Engine instances that use it, such as for data analytics.

* Nearline Storage:

Data you do not expect to access frequently (i.e., no more than once per month). Ideal for back-up and serving long-tail multimedia content.

* Coldline:

Data you expect to access infrequently (i.e., no more than once per year). Typically this is for disaster recovery, or data that is archived and may or may not be needed at some future time.

## Object Lifecycle of Cloud Storage

You can assign a lifecycle management configuration to a bucket. The configuration contains a set of rules which apply to current and future objects in the bucket. When an object meets the criteria of one of the rules, Cloud Storage automatically performs a specified action on the object. Here are some example use cases:

Downgrade the storage class of objects older than 365 days to Coldline Storage.
Delete objects created before January 1, 2013.
Keep only the 3 most recent versions of each object in a bucket with versioning enabled.
