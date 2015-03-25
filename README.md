# Spark + iPython

## Spark

### Setup cluster

Spark have a master-worker cluster architecture. So first we have to setup the cluster. If you have already a cluster
then you can skip this step.

Download a Spark distribution that meets your system configuration or Spark standalone from [Spark download webpage](http://spark.apache.org/downloads.html).
Also you can build yourself Spark from sources (it takes several minutes). You have to deploy the distribution
in the master machine and in workers.

Start master executing the command

    ./sbin/start-master.sh
    
Then in each worker execute

    ./bin/spark-class org.apache.spark.deploy.worker.Worker spark://<master ip>:<master port>
    
Then test the cluster setup checking `http://<master ip>:8080` or by executing the shell client:

    ./bin/spark-shell --master spark://<master ip>:<master port>
    
## iPython

### Setup iPython

1. virtualenv

2. pip install ipython[all]

3. MASTER=spark://<master ip>:<master port> IPYTHON_OPTS="notebook" ./bin/pyspark

## Try it

Open a notebook and execute the following code:

    NUM_SAMPLES = 10000000
    def sample(p):
        from random import random
        x, y = random(), random()
        return 1 if x*x + y*y < 1 else 0
    count = sc.parallelize(xrange(0, NUM_SAMPLES)).map(sample) \
                 .reduce(lambda a, b: a + b)
    print "Pi is roughly %f" % (4.0 * count / NUM_SAMPLES)
     