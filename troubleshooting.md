---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-19"

subcollection: discovery-data

---

{:shortdesc: .shortdesc}
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:pre: .pre}
{:important: .important}
{:deprecated: .deprecated}
{:codeblock: .codeblock}
{:screen: .screen}
{:download: .download}
{:hide-dashboard: .hide-dashboard}
{:apikey: data-credential-placeholder='apikey'} 
{:url: data-credential-placeholder='url'}
{:curl: .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:ruby: .ph data-hd-programlang='ruby'}
{:swift: .ph data-hd-programlang='swift'}
{:go: .ph data-hd-programlang='go'}

# Getting help
{: #troubleshoot}

If you cannot find a solution to the issue you are having, try the resources available from the **Developer community** section of the table of contents.

You can also get help by creating a case here: [{{site.data.keyword.cloud_notm}} Support](https://cloud.ibm.com/unifiedsupport/supportcenter){: external}.

## Clearing a lock state
{: #troubleshoot-ls}

![Cloud Pak for Data only](images/cpdonly.png) When the `gateway` pod restarts it runs a database validation plugin that checks for changes and applies the latest changesets to the shared database. If the pod is restarted while this check is in process, the plugin may remain in a lock state preventing the service from starting. Manual database intervention may be needed to clear the lock.

If the Discovery API doesn't come online, or if the `gateway-0` pod looks like it is in a constant crash loop, you can try checking the Liberty server logs for the API service located here: `/opt/ibm/wlp/output/wdapi/logs/messages.log`

The logs would indicate if liquidbase is failing and unable to run. If the system is locked you may see something like this:

```
[11/7/19 5:07:51:491 UTC] 0000002f liquibase.executor.jvm.JdbcExecutor I SELECT LOCKED FROM public.databasechangeloglock WHERE ID=1
[11/7/19 5:07:51:593 UTC] 0000002f liquibase.lockservice.StandardLockService I Waiting for changelog lock....
[11/7/19 5:08:01:601 UTC] 0000002f liquibase.executor.jvm.JdbcExecutor I SELECT LOCKED FROM public.databasechangeloglock WHERE ID=1
[11/7/19 5:08:02:091 UTC] 0000002f liquibase.lockservice.StandardLockService I Waiting for changelog lock....
[11/7/19 5:08:12:097 UTC] 0000002f liquibase.executor.jvm.JdbcExecutor I SELECT LOCKED FROM public.databasechangeloglock WHERE ID=1
[11/7/19 5:08:12:197 UTC] 0000002f liquibase.lockservice.StandardLockService I Waiting for changelog lock....
[11/7/19 5:08:22:203 UTC] 0000002f liquibase.executor.jvm.JdbcExecutor I SELECT ID,LOCKED,LOCKGRANTED,LOCKEDBY FROM public.databasechangeloglock WHERE ID=1
[11/7/19 5:08:22:613 UTC] 0000002f com.ibm.ws.logging.internal.impl.IncidentImpl I FFDC1015I: An FFDC Incident has been created: "org.jboss.weld.exceptions.DeploymentException: WELD-000049: Unable to invoke public void liquibase.integration.cdi.CDILiquibase.onStartup() on liquibase.integration.cdi.CDILiquibase@7f02a07 com.ibm.ws.container.service.state.internal.ApplicationStateManager 31" at ffdc\_19.11.07\_05.08.22.0.log
```

It is possible to manually unlock the plugin by executing the following command on the postgres database the `gateway-0` pod is looking at:

`psql dadmin`
`UPDATE DATABASECHANGELOGLOCK SET LOCKED=FALSE, LOCKGRANTED=null, LOCKEDBY=null where ID=1;`

If you can then restart the gateway pod everything should resume normally.

## Environment variable settings for Smart Document Understanding
{: #troubleshoot-sdu}

![Cloud Pak for Data only](images/cpdonly.png) There are two environment variables that need to be adjusted for Smart Document Understanding in {{site.data.keyword.discoveryfull}} version 2.1.0. This was resolved in version 2.1.1, see [2.1.1 release, 24 Jan 2020](/docs/discovery-data?topic=discovery-data-release-notes#24jan2020).

`SDU_PYTHON_REST_RESPONSE_TIMEOUT_MS`
`SDU_YOLO_TIMEOUT_SEC`

Both of these should be set to their respective `hour` value. These values must be set in the `<release-name>-watson-discovery-hdp` ConfigMap.

The values should be:

`SDU_PYTHON_REST_RESPONSE_TIMEOUT_MS` should be set to `3600000`

`SDU_YOLO_TIMEOUT_SEC` should be set to `3600`

These values should be set when the software is installed or reinstalled.