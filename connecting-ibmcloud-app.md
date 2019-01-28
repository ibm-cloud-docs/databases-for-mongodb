---

Copyright:
  years: 2018, 2019
lastupdated: "2019-01-28"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Connecting an {{site.data.keyword.cloud_notm}} application
{: #ibmcloud-app}

Applications running in {{site.data.keyword.cloud_notm}} can be bound to your {{site.data.keyword.databases-for-mongodb_full}} deployment. 

{{site.data.keyword.cloud_notm}} uses a manifest file - `manifest.yml` to associate an application with a service. Follow these steps to create your manifest file.
- In an editor, open a new file and add the text:
  ```
  ---
  applications:
  - name:    example-helloworld-application
    routes:
    - route: example-helloworld-application.us-south.cf.appdomain.cloud
    memory:  128M
    services:
      - example-mongo
  ```

- Change the route value to something unique. The route that you choose determines the subdomain of your application's URL. The format is `<host>.{region}.cf.appdomain.cloud`. Be sure the `{region}` matches where your application is deployed.
- Change the name value. The value that you choose is the name of the app as it appears in your {{site.data.keyword.cloud_notm}} dashboard.
- Update the services value to match the name or [Cloud Foundry alias](#create-alias) of your {{site.data.keyword.databases-for-mongodb}} service.

You can verify that the services are connected by navigating to the _Connections_ panel. If the deployment and the application are connected, the connection shows up in both services.

## Creating a Cloud Foundry alias
{: #create-alias}

If your application is running on Cloud Foundry, you need to create an alias for your {{site.data.keyword.databases-for-mongodb}} service so that it is discoverable by the Cloud Foundry application. Log in to the {{site.data.keyword.cloud_notm}} CLI and use the command:

`ibmcloud resource service-alias-create alias-name --instance-name instance-name`

The alias name can be the same as the database service instance name. So, for a {{site.data.keyword.databases-for-mongodb}} service named "example-mongo", use the following command:

`ibmcloud resource service-alias-create example-mongo --instance-name example-mongo`





