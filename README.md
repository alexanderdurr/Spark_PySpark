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
(credit to Michael Galarnyk)<br>
3) press `ESC` to exit insert mode, enter `:wq` to exit VIM. *[You could fine more VIM commands here](http://www.radford.edu/~mhtay/CPSC120/VIM_Editor_Commands.htm)*<br>
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

## 3. Common Errors
### Unable to load native-hadoop library
This error seems to be quite common for people who's trying to install Hadoop. Basically it means you are running Hadoop on 64bit OS wile Hadoop library is only compiled on 32bit OS. I also had this error and tried several methods, it seems I still have the error but after I did the above steps to call Jupyter Notebook, the error is gone and it didn't have any impact on using SparkConext in the Jupyter Notebook. _If anyone knows any other methods, please do let me know._<br>
#### Possible solution 1: download and install Hadoop binary, add the following codes to your bash_profile:<br>
```
export HADOOP_HOME=~/hadoop-2.8.0
```
#### Possible solution 2: add "native" to HADOOP_OPTS:<br>
```
export HADOOP_OPTS="$HADOOP_OPTS -Djava.library.path=$HADOOP_HOME/lib/native"
```
#### Possible solution 3: similar to solution 2, but add one more line:<br>
```
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib"
```
<br>

### Java gateway process exited before sending the driver its port number
This error is usually caused by JAVA_HOME is not set, so add the following codes to your bash_profile shoud do the trick, remember to change the spark version to the version you have:<br>
```
export JAVA_HOME=/Library/Java/Home
```
But also [Julius Wang](https://medium.com/data-science-canvas/configuring-ipython-notebook-for-spark-on-mac-os-8ec2d88ce724) shared another possible cause and fix of setting up SPARK_HOME that you could also try:<br>  
```
export SPARK_HOME=/<your spark installation location>/spark-1.6.0
export PATH=$SPARK_HOME/bin:$PATH
export PYTHONPATH=$SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.9-src.zip:$PYTHONPATH
export PYSPARK_SUBMIT_ARGS=pyspark-shell
```
## 4. Some other useful commands
#### If you want to uninstall any previous version of Java, use the following code:<br>
```
sudo rm -fr /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin 
sudo rm -fr /Library/PreferencePanes/JavaControlPanel.prefPane 
sudo rm -fr ~/Library/Application\ Support/Java
```

#### If you want to uninstall Spark, use `$ brew remove --force apache-spark`

If you have any questions on the steps, or if you encountered any other errors, you could let me know and I will try to help. Anyway, I'm just a newbie who's only being studying Python, Spark, or machine learning for not very long time, but I'm more than willing to discuss these topics and learn from all of you. By the way, I had experience working as a technical support when I was in college, so at least I'm confident in working in cmd and I'm very good at looking for sulotions from google search. :)
