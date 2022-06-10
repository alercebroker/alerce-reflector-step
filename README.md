# Alerce Reflector Step

Copies one or more Kafka topics into a new one, essentially a custom made 
[mirrormaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330). 
Used to replicate messages from an external source into the current Kafka 
cluster.

This step does nothing with the data. Custom consumers/producers completely
skip the (de)serialization stages and messages are copied just as they are.
There is no database connection.

## Environment variables

Unless noted, the following environment variables are required

## Local installation

Install required packages using:
```commandline
pip install -r requirements.txt
```

The step itself can be run with:
```commandline
python scripts/run_step.py
```

## Development and testing

Additional dependencies for testing without the deployment of the full 
infrastructure are required. these can be installed using:
```commandline
pip install -r dev-requirements.txt
```

To run all tests, use:
```commandline
pytest
```

## Previous conditions

No special conditions, only connection to Kafka.

## Version

* 1.0.0

## Libraries used

* [APF](https://github.com/alercebroker/APF)

## Environment variables

### Consumer setup

* `CONSUMER_SERVER`: Kafka host with ports, e.g., `localhost:9092`
* `CONSUMER_GROUP_ID`: Name for the consumer group, e.g., `cmirrormaker-step`
* `CONSUME_TIMEOUT`: Maximum time in seconds to wait for a message. Defaults to `10`
* `CONSUME_MESSAGES`: Number of messages to consume per operation. Defaults to `1000`
* `TOPIC_STRATEGY_FORMAT`: Format of topics that change daily, e.g., `ztf_{}_pid` or `ztf_{}_pid1,ztf_{}_pid2`. The `{}` will be replaced by the date formatted as `%Y%m%d`, set to change every day at 23:00 UTC
* `CONSUMER_TOPICS`: List of topics to consume as a string separated by commas, e.g., `topic` or `topic1,topic2`

Note that one of `TOPIC_STRATEGY_FORMAT` or `CONSUMER_TOPICS` *must* be set. 
If both are set, then `CONSUMER_TOPICS` will be ignored.

### Producer setup

* `PRODUCER_SERVER`: Kafka host with ports, e.g., `localhost:9092`
* `PRODUCER_TOPIC`: Topic to which messages will be produced, e.g., `topic`

### Step metadata

* `STEP_VERSION`: Current version of the step, e.g., `1.0.0`. Defaults to `dev`
* `STEP_ID`: Unique identifier for the step. Defaults to `cmirrormaker`
* `STEP_NAME`: Name of the step. Defaults to `cmirrormaker`
* `STEP_COMMENTS`: Comments on the specific version of the step