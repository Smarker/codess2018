# Workshop Setup

If you would like to set up this workshop on your own, follow the below steps.

## Prerequisites

You will need the following accounts and resources:

* [Twitter Account](https://help.twitter.com/en/create-twitter-account)
* [Azure Account](https://azure.microsoft.com/en-us/free/)
  * [Azure Databricks Account](https://docs.microsoft.com/en-us/azure/azure-databricks/quickstart-create-databricks-workspace-portal)
  * [Azure EventHub](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-create)

## Environment Setup

After signing up for the prerequisite accounts and creating an `EventHub` to
store incoming twitter posts, login to your `databricks account`. You will need to
[create two clusters](http://),
one for produce and one for consume. Then, attach the [corresponding libraries](http://)
to each cluster. Import `ConsumeEvents.html` as a notebook and attach it to your
`consume cluster`. **Make sure to set a consumer group in `ConsumeEvents.html`'s notebook.**
Import `ProduceEvents.html` as a notebook and attach it to
your `produce cluster`. Then, set your clusters' [environment variables](http://) by
substituting your `Twitter` and `EventHub` credentials
into `Environment.html` and execute the `Environment` script with `shift + enter`.

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
```

## Execute Code

`shift + enter`

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

### I cannot execute my code

* Make sure that your notebook is attached to a cluster (produce or consume).
* Try restarting the cluster and then re-executing all the code in the notebook

### Other

* Email `stephanie.marker93@gmail.com` and I'll get back to you!
