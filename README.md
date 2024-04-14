# BigDataDocker

Docker Image of bigdata stack.

## Specification

|Component|Version|
|:--------|:-----:|
|Ubuntu|`22.04`|
|OpenJDK|`8`|
|Scala|`2.12.19`|
|Python|`3.10`|
|Spark|`3.1.1`|
|Hadoop|`2.7.4`|
|Python Modules|[requirements](requirements.txt)|

|Other|Value|
|:--------|:-----:|
|User|`app`|
|UID|`10000`|
|GID|`10000`|
|User home|`/app`|

## Usage

`docker run -it cyijun/bigdata_dockers:standalone_spark_3.1.1-latest bash`
