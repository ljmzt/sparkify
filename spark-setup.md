# Set up spark on local network

### Purpose
The dataset for this exercise is not super large (12G). I also don't want to pay the extra bucks for AWS, which seems an over-kill for this project.  I do have two macbooks at home, so I set up a spark session with them. This file documents the steps to do so for those who are interested in doing similar things. 

### Package download
The package spark-3.4.1-bin-hadoop3 is from [here](https://spark.apache.org/downloads.html). This is pre-built and since it requires the paths from the two machines are the same. I unpacked it under /Applications. 

### Setup the spark session
[The offical guide](https://spark.apache.org/docs/latest/spark-standalone.html) is very easy to follow. I do have extra tweaks need to do.

1. The user names on my two laptops are different. So I need to set up the ssh config file following [this](https://www.howtogeek.com/75007/stupid-geek-tricks-use-your-ssh-config-file-to-create-aliases-for-hosts/), and [this](https://stackoverflow.com/questions/65573249/spark-ssh-password-less-with-different-username), which creates an alias for the second machine.

2. Use ssh-keygen to prepare the ssh keys for password-less login, follow [this](https://www.ssh.com/academy/ssh/keygen), which is a pretty standward way for ssh. 

3. Add the worker info in the conf/workers file. If you don't have the file, please create one. 

4. My default JAVA is too old, so I downloaded a newer version and add the JAVA_HOME path in my conf/spark-env.sh file. The command looks like this: JAVA_HOME='/Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home' 

5. Finally, you can start the server by using sbin/start-all.sh script. If everything is successsful, you will see in your localhost:8080 a monitoring page for the spark. (sometimes I forgot to close my spark before startinga new one, then i find the page on localhost:8081). It should display two machines are connected.

6. Now download pyspark from anaconda, open a jupyter notebook, import pyspark. To connect to the spark session, I actually do this

```
SparkSession.builder.master("local[*]").getOrCreate().stop()

spark = SparkSession \
        .builder \
        .enableHiveSupport() \
        .master('spark://192.168.0.103:7077') \
        .appName('etl-spark') \
        .getOrCreate()
```

The ones from the official [pyspark connect](https://spark.apache.org/docs/latest/api/python/getting_started/quickstart_connect.html) doesn't really work for me. The url is the ip address for your master machine and can be found either on the monitor page (localhost:8080) or your system preferences-sharing.

7. I also need to have an environment variable set up to avoid a forking error (see [here](https://medium.com/r/?url=https%3A%2F%2Fstackoverflow.com%2Fquestions%2F50168647%2Fmultiprocessing-causes-python-to-crash-and-gives-an-error-may-have-been-in-progr)) by adding this line at the top to the notebook.

```
%set_env OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
```

Now the spark is ready to go! I connect the two machines on wifi at home, and can handle this dataset without trouble.
