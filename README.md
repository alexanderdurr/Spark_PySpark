# Installing Spark/PySpark on Mac and Ways to Fix Some of the Common Errors
I followed the majority steps from Michael Galarnyk's post [Install Spark on Mac (PySpark)](https://medium.com/@GalarnykMichael/install-spark-on-mac-pyspark-453f395f240b). But I've shortened the installation part by using Homebrew <br>
I will also share my methods on fixing some of the common errors that you might encounter:
- Unable to load native-hadoop library for your platform… using builtin-java classes where applicable
- Java gateway process exited before sending the driver its port number

## 1. Install Spark/PySpark
### Prerequisites: Anaconda, Python3
The Spark installation also requires specific version of Java (java 8), but we can also install it using Homebrew.
1) open terminal, enter `$ brew install apache-spark`
2) once you see this error message ![alt text](https://github.com/yajieli912/Spark_PySpark/blob/master/images/fullsizeoutput_f7c.jpeg?raw=true) enter `$ brew cask install caskroom/versions/java8` to install Java8
3) check if pyspark is properly install by `$ pyspark`, you shuold see something like this: ![alt text](https://github.com/yajieli912/Spark_PySpark/blob/master/images/pyspark.jpg?raw=true)

## 2. Open Jupyter Notebook with PySpark Ready
### Prerequisites: PySpark works fine when calling `$ pyspark`
Jupyter Notebook is a very convenient tool to write and save codes, so in this instruction, I will share the steps of how to create a global profile in order to create Jupyter Notebook automatically initialized with SparkContext. <br>
In order to create a global profile for your terminal session, you will need to create or modify your .bash_profile or .bashrc file. Here, I will use .bash_profile as my example<br>
1) check if you have .bash_profile in your system `$ ls -a`, if you don't have one, create one using `$ touch ~/.bash_profile`
2) if you already have a .bash_profile, open it by `$ vim ~/.bash_profile`, press `I` in order to insert, and paste the following codes in any location (DO NOT delete anything in your file):
```
export SPARK_PATH=~/spark-1.6.0-bin-hadoop2.6 
export PYSPARK_DRIVER_PYTHON="jupyter" 
export PYSPARK_DRIVER_PYTHON_OPTS="notebook" 

#For python 3, You have to add the line below or you will get an error
# export PYSPARK_PYTHON=python3
alias snotebook='$SPARK_PATH/bin/pyspark --master local[2]'
```
(credit to Michael Galarnyk)
3) press `ESC` to exit insert mode, enter `:wq` to exit VIM. *[You could fine more VIM commands here](http://www.radford.edu/~mhtay/CPSC120/VIM_Editor_Commands.htm)*
4) refresh terminal profile by `$ source ~/.bash_profile`
5) you should be able to open Jupyter Notebook by simply calling `$ jupyter notebook`
6) to check if your notebook is initialized with SparkContext, you could try the following codes in your notebook:
```
sc = SparkContext.getOrCreate()
import numpy as np
TOTAL = 1000000
dots = sc.parallelize([2.0 * np.random.random(2) - 1.0 for i in range(TOTAL)]).cache()
print("Number of random points:", dots.count())
stats = dots.stats()
print('Mean:', stats.mean())
print('stdev:', stats.stdev())
```
