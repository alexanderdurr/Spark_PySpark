# Installing Spark on Mac and Ways to Fix Some of the Common Errors
I followed the majority steps from Michael Galarnyk's post [Install Spark on Mac (PySpark)](https://medium.com/@GalarnykMichael/install-spark-on-mac-pyspark-453f395f240b). But I've shortened the installation part by using Homebrew <br>
I will also share my methods on fixing some of the common errors that you might encounter:
- Unable to load native-hadoop library for your platform… using builtin-java classes where applicable
- Java gateway process exited before sending the driver its port number

## 1. Install Spark
### Prerequisites: Anaconda, Python3
The Spark installation also requires specific version of Java (java 8), but we can also install it using Homebrew.
1) open terminal, enter `$ brew install apache-spark`
2) once you see this error message ![alt text](https://github.com/yajieli912/Spark_PySpark/blob/master/images/6B23E333-CDAC-494A-9AE7-7D22F370A39D-49128-0000211C7B7233CD_tmp.jpg?raw=true)
