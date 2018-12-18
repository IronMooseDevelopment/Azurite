# Azurite
[![npm version](https://badge.fury.io/js/azurite.svg)](https://badge.fury.io/js/azurite)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)]()
[![Build Status](https://travis-ci.org/Azure/Azurite.svg?branch=master)](https://travis-ci.org/Azure/Azurite)

A lightweight server clone of Azure Blob, Queue, and Table Storage that simulates most of the commands supported by it with minimal dependencies.

# Why this fork?
~~We are maintaining this fork because the upstream has not deployed new packages since 2.6.5 despite having released code for 2.6.7
For now, we are just deploying updated Docker images. Raise an Issue if you would like to see us host the NPM package as well~~

Update: Azurite has resumed pushing out packages (see here: https://github.com/Azure/Azurite/issues/94#issuecomment-447785116)
As such we recommend using the official fork and packages and will no longer produce builds. We do not plan on removing `ironmoosedevelopment/azurite 2.6.7` from Docker Hub so as to prevent breaking any code that depends on it.

# Installation and Usage

## Docker image

### Pulling from Docker Hub
Azurite 2.6.7 is available at [Docker Hub](https://hub.docker.com/r/ironmoosedevelopment/azurite/) and ready to be pulled with:
```bash
$ docker pull ironmoosedevelopment/azurite
```
Please note that the `latest` tag will always refer to 2.6.7

### Build the Docker image 
To build the Docker image yourself, execute the following:
```bash
$ docker build -t ironmoosedevelopment/azurite .
```

### Run the Docker image
To run the Docker image, execute the following command:
```bash
$ docker run -d -t -p 10000:10000 -p 10001:10001 -p 10002:10002 -v /path/to/folder:/opt/azurite/folder ironmoosedevelopment/azurite
```

#### Configure the executable when running the container
By default, the container starts all services available (currently blob, queue, and table).
Using the environment variable `executable`, specific executables can be specifed:

 * `blob` Start the Blob Storage Emulator only
 * `queue` Start the Azure Queue Storage Emulator only
 * `table` Start the Azure Table Storage Emulator only

##### Usage example:
```bash
$ docker run -e executable=blob -d -t -p 10000:10000 -v /path/to/folder:/opt/azurite/folder ironmoosedevelopment/azurite
```

## Usage with Azure Cross-Platform CLI 2.0

To perform blob storage operations using the 2.0 Azure cross-platform CLI, you need to operate with the
appropriate connection string. The values within are based on the hardcoded Azure Storage Emulator values.

Example command to create a container:

```shell
$ az storage container create --name 'test' --connection-string 'DefaultEndpointsProtocol=http;AccountName=devstoreaccount1;AccountKey=Eby8vdM02xNOcqFlqUwJPLlmEtlCDXJ1OUzFT50uSRZ6IFsuFq2UVErCz4I6tq/K1SZFPTOtr/KBHBeksoGMGw==;BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;'

{
  "created": true
}
```

## Current List of Command Line Options

```
-a
``` 
Enables sharedkey authentication check
```
-l c:\tmp\emulatorPath
--location c:\tmp\emulatorPath
```
Allows the specification of a path
```
--blobPort 101000
```
Sets the TCP Port for blob storage to the value following the argument.
```
--queuePort 10001
```
Sets the TCP Port for queue storage to the value following the argument.
```
--tablePort 10002
```
Sets the TCP Port for table storage to the value following the argument.



# Contributions
## What do I need to know to help?
If you are interested in making a code contribution, please make it to the upstream. We will pull all new releases into this fork.


# API Support
Currently, Azurite only supports the [Blob Storage APIs](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/blob-service-rest-api), the [Queue Storage API](https://docs.microsoft.com/en-us/rest/api/storageservices/queue-service-rest-api), and the [Table Storage API](https://docs.microsoft.com/en-us/rest/api/storageservices/table-service-rest-api).  
Support for Azure Storage Files is planned, but currently not available. 

The Standard Emulator Connection String is the same as required by [Microsoft's Official Storage Emulator](https://go.microsoft.com/fwlink/?LinkId=717179):

`BlobEndpoint=http://127.0.0.1:10000/devstoreaccount1;`

`QueueEndpoint=http://127.0.0.1:10001/devstoreaccount1;`

`TableEndpoint=http://127.0.0.1:10002/devstoreaccount1;`


## Blob Storage - API Implementation Status
All DONE except:
- Account SAS Support [TODO]  
See [https://docs.microsoft.com/en-us/rest/api/storageservices/constructing-an-account-sas](https://docs.microsoft.com/en-us/rest/api/storageservices/constructing-an-account-sas) for specification

- Get Blob Service Stats [TODO]  
Retrieves statistics related to replication for the Blob service. This operation is only available on the secondary location endpoint when read-access geo-redundant replication is enabled for the storage account.

- Set Blob Tier [TODO]
The Set Blob Tier operation sets the tier on a blob.

## Queue Storage - API Implementation Status
All DONE.

## Table Storage - API Implementation Status
ALL DONE except:
- Get Table ACL [TODO]
- Set Table ACL [TODO]
- Entity Group Transaction (Batch Operation) [TODO]

