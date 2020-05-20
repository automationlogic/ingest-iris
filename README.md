# Ingest Iris

## Overview

The ingestion process fetches the Iris data from its [online source](https://archive.ics.uci.edu/ml/datasets/iris) as part of a proof of concept data ingestion use case. It runs as an App Engine app, which handles both fetching the data and placing it on a Pub/Sub topic, as well as reading it from Pub/Sub and inserting it into BigQuery.

## Prerequisites

1. [Platform bootstrap](https://github.com/automationlogic/platform-bootstrap)
2. [Analytics infra](https://github.com/automationlogic/analytics-infra)

## Configuration

The app configuration resides in a `app.yaml` template called `app.yaml.tmpl`. The reason for the template is to allow Cloud Build to inject environment variables into the configuration file if needed.

```
URL: https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data
PROJECT: $ANALYTICS_PROJECT    # replace in cloud build step
TOPIC: iris
SUBSCRIPTION: iris
DATASET: iris
TABLE: iris
```

The `$ANALYTICS_PROJECT` environment variable is a pipeline substitution in the pipeline trigger. It is injected as part of a Cloud Build step:

`sed -e "s/\$$ANALYTICS_PROJECT/$_ANALYTICS_PROJECT/g" app.yaml.tmpl > app.yaml`

It is passed through from the [Platform Bootstrap](https://github.com/automationlogic/platform-bootstrap) process, which is where it is originally configured.

## Run

The pipeline is automatically triggered when code is pushed. It can also be triggered manually via the console.
