In this project, for processing streaming data in real time, one of the first requirements is to get access to the streaming data; in this case, real­time tweets. Twitter provides a very convenient API to fetch tweets in a streaming manner. In addition, this project will also be using Kafka to buffer the tweets before processing. Kafka provides a distributed queuing service which can be used to store the data when the data creation rate is more than processing rate.

Installing Required Python LibrariesThis text file contains the required python packages: requirements.txt. 
To install all of these at once, simply run (only missing packages will be installed):$ sudo pip install -r requirements.txt
Installing and Initializing KafkaDownload and extract the latest binaryfrom https://kafka.apache.org/downloads.html

Start zookeeper service:$ bin/zookeeper-server-start.sh config/zookeeper.properties 

Start kafka service:$ bin/kafka-server-start.sh config/server.properties 

Create a topic named twitterstream in kafka:
$ bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 —-partitions 1 --topic twitterstream
Check what topics you have with:$ bin/kafka-topics.sh --list --zookeeper localhost:2181


In order to download the tweets from twitter streaming API and push them to kafka queue, run the python script  twitter_to_kafka.py. The script will need your twitter authentication tokens (keys). Once you have your authentication tokens, create or update the  twitter.txt file with these credentials.

After updating the text file with your twitter keys, you can start downloading tweets from the twitter stream API and push them to the twitterstream topic in Kafka. Do this by running our program as follows:$ python twitter_to_kafka.py

To check if the data is landing in Kafka:
$ bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic twitterstream --from-beginning
Running the Stream Analysis Program$ $SPARK_HOME/bin/spark-submit --packages org.apache.spark:spark-streaming-kafka_2.10:1.5.1 twitterStream.py