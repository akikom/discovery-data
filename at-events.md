---

copyright:
  years: 2020
lastupdated: "2020-06-12"

keywords: IBM, activity tracker, LogDNA, event, security

subcollection: discovery-data

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:important: .important}
{:note: .note}


# Activity Tracker events
{: #at_events}

![IBM Cloud only](images/cloudonly.png)</br> 
As a security officer, auditor, or manager, you can use the Activity Tracker service to track how users and applications interact with the {{site.data.keyword.discoveryshort}} service in {{site.data.keyword.cloud}}.
{: shortdesc}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see the [getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-getting-started).


## List of events
{: #at_actions}

The following table lists the {{site.data.keyword.discoveryshort}} actions that generate an event.

| Action                           | Description                        | 
|:---------------------------------|:-----------------------------------|
| `discovery.label.delete`           | Delete one or multiple documents from one or multiple collections based on a label                  |
| `discovery.document.add`           | Add one document                  |
| `discovery.document.update`        | Update one document by ingesting new or modified content for a document, given a document ID, or by changing the label for a given document ID     | 
| `discovery.document.delete`        | Delete document by ID              |
| `discovery.query.read`             | Search a collection for relevant documents |
| `discovery.notices.read`           | Get notices for a collection |
| `discovery.fields.read`            | Get fields for a collection |
| `discovery.logs.read`              | Get logs for a collection |
| `discovery.training-data.read`     | Request all training data   |
| `discovery.training-data.delete`   | Delete the training data   |
| `discovery.training-query.create`           | Add a query to the training data   |
| `discovery.training-query.delete`           | Delete a query from the training data   |
| `discovery.training-query.read`             | Read the contents of a query   |
| `discovery.training-example.create`   | Add an example to a query in the training data   |
| `discovery.training-example.update`   | Update an example of a query in the training data   |
| `discovery.training-example.delete`   | Delete an example from a query in the training data   |
| `discovery.training-example.read`     | Read the contents of a training example   |
| `discovery.training-examples.read`    | Read the contents of all examples in a query   |
| `discovery.collection-training-status.read` | Get the training status of a single-collection training  |
| `discovery.multi-collection-training-status.read` | Get the training status of a multi-collection training  |
| `discovery.metric.read`            | Request a metric   |
| `discovery.event.create`           | Add a click event to a query  |
{: caption="Table 1. Actions that generate events" caption-side="top"}


## Viewing events
{: #at_ui}

Events that are generated by an instance of the {{site.data.keyword.discoveryshort}} service are automatically forwarded to the {{site.data.keyword.at_full_notm}} service instance that is available in the same location.

{{site.data.keyword.at_full_notm}} can have only one instance per location. To view events, you must access the web UI of the {{site.data.keyword.at_full_notm}} service in the same location where your service instance is available. For more information, see [Launching the web UI through the IBM Cloud UI](/docs/Activity-Tracker-with-LogDNA?topic=Activity-Tracker-with-LogDNA-launch#launch_step2).