# Cloudmesh Storage Provider for Virtual Directories between  AWS and Google

Pratibha Pagadala |  [fa19-516-152](https://github.com/cloudmesh-community/fa19-516-152)

Project Code: 
[cloudmesh-storage](https://github.com/prati-mp/cloudmesh-storage) ,
[cloudmesh-google](https://github.com/prati-mp/cloudmesh-google)

## Introduction

This project is to develop API and rest services to manage and transfer
files between different cloud service providers. A cloudmesh based
command will be implemented to transfer files from a source to target
cloud provider. In this instance, the functionality will be implemented
for AWS and Google Cloud. For performance evaluation py tests will be
created.

## Motivation

 Multiple cloud providers offer storage solutions to manage data in the
 form of files. The intention here is to build a command which can
 provide functionality to move them from a source to a target cloud
 provider's storage. In this method, users will be able to split or move
 the data across different cloud providers that provider cheaper
 solutions.

## Architecture Diagram

![Architecture](images/architecture2.png)

##### Description

* Client intiates a cms storage_switch command with options such as
 
  1. File and directories copy from source to target
  2. List the files
  3. delete the files
  
* Cloudmesh storage copy command will run on the local server.
  According to the options and arguments, this would delegate the
  functions between AWS and Google Cloud.

* Storage and Utility APIs on AWS and Google cloud.   

## Technology Used

* [Python 3.8](https://www.python.org/downloads/release/python-382)
* [AWS S3 Storage](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html?id=docs_gateway)
* [Google Cloud Storage](https://googleapis.dev/python/storage/latest/index.html)
* [cloudmesh-storage](https://github.com/cloudmesh/cloudmesh-storage)
* [cloudmesh-google](https://github.com/cloudmesh/cloudmesh-google)

## Implementation

* Storage copy function will allow to copy files between AWS S3 and Google storage buckets.
* Copy provider includes a copy method with source, target, sourceFile and targetFile
* Copy method will invoke the provider's get method to download files from source cloud to local temporary directory. On successful download, put method of target provider will invoked to upload files to target cloud. 

**Pre-Requistes**
* Create a AWS Cloud Account, project and bucket 
* Create a Google Storage cloud account and bucket.

Account creation instructions are available in cloudmesh-manual:

[AWS Account Setup](https://cloudmesh.github.io/cloudmesh-manual/accounts/aws.html) | 
[Google Account Setup](https://cloudmesh.github.io/cloudmesh-manual/accounts/google/account.html)

**Installation:**
* Install Python 3.8 version
* Install cloudmesh-installer and following cloudmesh bundles - cloud, storage, google.
Refer to cloudmesh manual for installation steps: [cloudmesh installer installation](https://cloudmesh.github.io/cloudmesh-manual/installation/install-dev.html)
``

* Update account information in cloudmesh.yaml as instruction. Refer to cloudmesh manual for information: [yaml configuration steps](https://cloudmesh.github.io/cloudmesh-manual/configuration/configuration.html?highlight=cloudmesh%20yaml)

**Usage:**

````
Usage:

    storage copy --source=SOURCE:SOURCE_FILE_DIR --target=TARGET:TARGET_FILE_DIR
    storage list [SOURCE:SOURCE_FILE_DIR] 
    storage delete SOURCE:SOURCE_FILE_DIR 
    
Arguments:
 SOURCE:SOURCE_FILE_DIR   source provider name : file or 
                          directory name
 TARGET:SOURCE_FILE_DIR   destination provider name

Options:
 --source=SOURCE:SOURCE_FILE_DIR     specify the cloud:location
 --target=TARGET:LOCATION            specify the target:location

Description:

Command enables to Copy files between different cloud service providers,
list and delete them. This command accepts "aws" , "google" 
as the SOURCE and TARGET provider

cms storage_service copy --source=SOURCE:SOURCE_FILE_DIR 
                         --target=TARGET:TARGET_FILE_DIR
 
 Command copies files or directories from Source provider to Target
 Provider.

cms storage_service list SOURCE:SOURCE_FILE_DIR

 Command lists all the files present in SOURCE provider's in the given
 SOURCE_FILE_DIR location This command accepts "aws" or "google" as the
 SOURCE provider

cms storage_service delete SOURCE:SOURCE_FILE_DIR

 Command deletes the file or directory from the SOURCE provider's
 SOURCE_FILE_DIR location

Example:

cms storage_service copy --source=google:test1.txt 
                         --target=aws:uploadtest1.txt
cms storage_service list google:test
cms storage_service delete aws:uploadtest1.txt

````
  
## Dependencies / Constraints

* Copy currently downloads the source files from Source cloud to local file system and then uploads them to target cloud.
* Local Temp directory must be created in the path ~/.cloudmesh/storage/tmp

## Testing

PyTest have been executed to test the functionality -
 
 [Test Script](https://github.com/prati-mp/cloudmesh-storage/blob/master/tests/copy/Test_storage_service.py) | [Test Results](https://github.com/cloudmesh-community/fa19-516-152/blob/master/project/benchmark/testResults.txt)

```
pytest -v --capture=no -W ignore::DeprecationWarning 
        tests/copy/Test_storage_service.py >  tests/copy/testResults.txt
```
## Benchmarks

Benchmarks results - [storage copy benchmarks](https://github.com/cloudmesh-community/fa19-516-152/blob/master/project/benchmark/testResults.txt)

```
+--------------------------------+----------+--------+---------------------+-------+-----------+----------+---------+---------------------------------+
| Name                           | Status   |   Time | Start               | tag   | Node      | User     | OS      | Version                         |
|--------------------------------+----------+--------+---------------------+-------+-----------+----------+---------+---------------------------------|
| local_to_aws_fileSize_1.txt    | Success  | 13.851 | 2020-05-08 05:13:47 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| local_to_aws_fileSize_5.txt    | Success  | 18.43  | 2020-05-08 05:14:01 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| local_to_google_fileSize_1.txt | Success  |  6.75  | 2020-05-08 05:14:19 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| local_to_google_fileSize_5.txt | Success  |  9.041 | 2020-05-08 05:14:26 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| aws_to_local_fileSize_1.txt    | Success  | 13.834 | 2020-05-08 05:14:35 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| aws_to_local_fileSize_5.txt    | Success  | 13.665 | 2020-05-08 05:14:49 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| google_to_local_fileSize_1.txt | Success  |  3.705 | 2020-05-08 05:15:03 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| google_to_local_fileSize_5.txt | Success  |  3.94  | 2020-05-08 05:15:06 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| aws_to_google_fileSize_1.txt   | Success  | 18.207 | 2020-05-08 05:15:10 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| aws_to_google_fileSize_5.txt   | Success  | 23.681 | 2020-05-08 05:15:28 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| google_to_aws_fileSize_1.txt   | Success  | 14.81  | 2020-05-08 05:15:52 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| google_to_aws_fileSize_5.txt   | Success  | 21.724 | 2020-05-08 05:16:07 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| google_to_aws_directory        | Success  | 14.49  | 2020-05-08 05:16:29 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| aws_to_google_directory        | Success  | 14.691 | 2020-05-08 05:16:43 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
| google_to_aws_directory2       | Success  | 14.875 | 2020-05-08 05:16:58 | copy  | DESKTOP | Pratibha | Windows | ('10', '10.0.18362', 'SP0', '') |
+--------------------------------+----------+--------+---------------------+-------+-----------+----------+---------+---------------------------------+
```

## References

* <https://github.com/googleapis/google-cloud-python#google-cloud-python-client>
* <https://aws.amazon.com/s3/>
* <https://boto3.amazonaws.com/v1/documentation/api/latest/guide/resources.html>
* <https://github.com/cloudmesh/cloudmesh-storage/tree/master/cloudmesh/storage>
