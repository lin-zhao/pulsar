---
id: io-overview
title: Pulsar connector overview
sidebar_label: "Overview"
---

````mdx-code-block
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
````


Messaging systems are most powerful when you can easily use them with external systems like databases and other messaging systems.

**Pulsar IO connectors** enable you to easily create, deploy, and manage connectors that interact with external systems, such as [Apache Cassandra](https://cassandra.apache.org), [Aerospike](https://www.aerospike.com), and many others.


## Concept

Pulsar IO connectors come in two types: **source** and **sink**.

This diagram illustrates the relationship between source, Pulsar, and sink:

![Pulsar IO diagram](/assets/pulsar-io.png "Pulsar IO connectors (sources and sinks)")


### Source

> Sources **feed data from external systems into Pulsar**.

Common sources include other messaging systems and firehose-style data pipeline APIs.

For the complete list of Pulsar built-in source connectors, see [source connector](io-connectors.md#source-connector).

### Sink

> Sinks **feed data from Pulsar into external systems**.

Common sinks include other messaging systems and SQL and NoSQL databases.

For the complete list of Pulsar built-in sink connectors, see [sink connector](io-connectors.md#sink-connector).

## Processing guarantee

Processing guarantees are used to handle errors when writing messages to Pulsar topics.
  
> Pulsar connectors and Functions use the **same** processing guarantees as below.

Delivery semantic | Description
:------------------|:-------
`at-most-once` | Each message sent to a connector is to be **processed once** or **not to be processed**.
`at-least-once`  | Each message sent to a connector is to be **processed once** or **more than once**.
`effectively-once` | Each message sent to a connector has **one output associated** with it.

> Processing guarantees for connectors not just rely on Pulsar guarantee but also **relate to external systems**, that is, **the implementation of source and sink**.

* Source: Pulsar ensures that writing messages to Pulsar topics respects the processing guarantees. It is within Pulsar's control.

* Sink: the processing guarantees rely on the sink implementation. If the sink implementation does not handle retries in an idempotent way, the sink does not respect the processing guarantees.

### Set

When creating a connector, you can set the processing guarantee with the following semantics:

* ATLEAST_ONCE
  
* ATMOST_ONCE
  
* EFFECTIVELY_ONCE

> If `--processing-guarantees` is not specified when creating a connector, the default semantic is `ATLEAST_ONCE`.

Take **Admin CLI** as an example. For more information about **REST API** or **JAVA Admin API**, see [here](io-use.md#create). 

````mdx-code-block
<Tabs groupId="io-choice"
  defaultValue="Source"
  values={[{"label":"Source","value":"Source"},{"label":"Sink","value":"Sink"}]}>

<TabItem value="Source">

```bash
bin/pulsar-admin sources create \
    --processing-guarantees ATMOST_ONCE \
    # Other source configs
```

For more information about the options of `pulsar-admin sources create`, see [here](reference-connector-admin.md).

</TabItem>
<TabItem value="Sink">

```bash
bin/pulsar-admin sinks create \
    --processing-guarantees EFFECTIVELY_ONCE \
    # Other sink configs
```

For more information about the options of `pulsar-admin sinks create`, see [here](reference-connector-admin.md).

</TabItem>

</Tabs>
````

### Update 

After creating a connector, you can update the processing guarantee with the following semantics:

* ATLEAST_ONCE
  
* ATMOST_ONCE
  
* EFFECTIVELY_ONCE
  
Take **Admin CLI** as an example. For more information about **REST API** or **JAVA Admin API**, see [here](io-use.md#create). 

````mdx-code-block
<Tabs groupId="io-choice"
  defaultValue="Source"
  values={[{"label":"Source","value":"Source"},{"label":"Sink","value":"Sink"}]}>

<TabItem value="Source">

```bash
bin/pulsar-admin sources update \
    --processing-guarantees EFFECTIVELY_ONCE \
    # Other source configs
```

For more information about the options of `pulsar-admin sources update`, see [here](reference-connector-admin.md).

</TabItem>
<TabItem value="Sink">

```bash
bin/pulsar-admin sinks update \
    --processing-guarantees ATMOST_ONCE \
    # Other sink configs
```

For more information about the options of `pulsar-admin sinks update`, see [here](reference-connector-admin.md).

</TabItem>

</Tabs>
````


## Work with connector

You can manage Pulsar connectors (for example, create, update, start, stop, restart, reload, delete and perform other operations on connectors) via the `Connector Admin CLI` with sources and sinks subcommands. For the latest and complete information, see [Pulsar admin docs](/tools/pulsar-admin/).

Connectors (sources and sinks) and Functions are components of instances, and they all run on Functions workers. When managing a source, sink or function via the `Connector Admin CLI` or `Functions Admin CLI`, an instance is started on a worker. For more information, see [Functions worker](functions-worker.md).

