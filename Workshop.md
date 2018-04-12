# Workshop Setup

If you would like to set up this workshop on your own, follow the below steps.

## Prerequisites

* Twitter Account
* Azure Account
  * Databricks Account
  * EventHub Resource

## Set up Clusters

* 1 cluster for producing twitter events with attached libraries: `azure-eventhubs-databricks_2.11-3.4.0`, `spark-streaming-twitter_2.11-2.2.0`
* 1 cluster for consuming twitter events with attached libraries: `azure-eventhubs-spark_2.11-2.3.0`, `spark-streaming-twitter_2.11-2.2.0`

## Set up Environment Variables

These environment variables should be shared between the produce and consume
clusters.

```sh
sudo echo TWITTER_CONSUMER_KEY=... >> /etc/environment
sudo echo TWITTER_CONSUMER_SECRET=... >> /etc/environment
sudo echo TWITTER_ACCESS_TOKEN=... >> /etc/environment
sudo echo TWITTER_ACCESS_TOKEN_SECRET=... >> /etc/environment

sudo echo EVENTHUB_NAMESPACE_NAME=... >> /etc/environment
sudo echo EVENTHUB_NAME=... >> /etc/environment
sudo echo EVENTHUB_SAS_KEY_NAME=... >> /etc/environment
sudo echo EVENTHUB_SAS_KEY=... >> /etc/environment
sudo echo EVENTHUB_CONSUMER_GROUP=... >> /etc/environment
```

## About these Files

| File        | Description | Library Dependencies |
| ----------- | ----------- | ---------------------|
| Environment | Contains `environment variables` used to connect to `EventHub` and `Twitter` | none |
| Produce Events | Produces `twitter events` and streams them to `EventHub` | `azure-eventhubs-databricks_2.11-3.4.0`, `spark-streaming-twitter_2.11-2.2.0` |
| ConsumeEvents | Consume `twitter events` from `EventHub` with structured spark streaming and perform operations on `twitter posts`. | `azure-eventhubs-spark_2.11-2.3.0`, `spark-streaming-twitter_2.11-2.2.0` |

## FAQ

### Why are there two eventhubs libraries (spark, databricks)

Using `spark-streaming-twitter_2.11-2.2.0` to produce twitter events results in
a `DStream[twitter4j.Status]`. To work with `DStreams`, I needed
`azure-eventhubs-databricks_2.11-3.4.0`. Since I wanted to use
`structured spark streaming` to consume twitter statuses, I needed to use
`azure-eventhubs-spark_2.11-2.3.0`, since this library is for structured
streaming.

### Why use two clusters

I needed two clusters to avoid `azure-eventhubs-databricks_2.11-3.4.0` and
`azure-eventhubs-spark_2.11-2.3.0` library collisions.