---
copyright:
  years: 2020
lastupdated: "2020-08-05"

keywords: databases, opsman, mongodbee

subcollection: databases-for-mongodb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Before you begin with the MongodDB Ops Manager

- You need to have an [{{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){:new_window}.
- And a {{site.data.keyword.databases-for-mongodb}} Enterprise Edition deployment. You can provision one from the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog/databases-for-mongodb). Give your deployment a memorable name that appears in your account's Resource List.
- [Set the Admin Password](/docs/databases-for-mongodb?topic=databases-for-mongodb-admin-password) for your deployment.
- [Create an Ops Manager username and password](/docs/databases-for-mongodb?topic=databases-for-mongodb-user-management.md) for your deployment by using the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api).


## Initial login
{: #initial-login}

The {{site.data.keyword.databases-for-mongodb_full}} Enterprise Edition (EE) service is provisioned with access to the MongodDB Ops Manager user interface.

Once you have created an OpsManager username & password via the [Cloud Databases API](https://cloud.ibm.com/apidocs/cloud-databases-api), you must follow these instructions to get access to the {{site.data.keyword.databases-for-mongodb}}EE instance within the OpsManager UI:

1. Login with the Ops Manager username & password you created for your deployment:
   
    ![The MongoDBEE Ops Manager login pane](images/opsman-login.png)

2. In the resulting view, select the 'Invitations' tab:
  
    ![The Ops Manager invitations pane](images/opsman-invitations.png)

3. Click 'Accept' for the invitation as role 'Project Data Access Admin'. This will add your Ops Manager user id to the Organization and project shown:
  
    ![The Ops Manager accepted invitations success pane](images/opsman-invite-success.png)

4. Lastly, to navigate to the instance view: 
   - Click on the 'OpsManager' logo in the top menu bar, 
   - or select the 'All Clusters' link:
    
    ![The MongoDBEE Ops Manager instance view pane](images/opsman-instance-view.png)

On subsequent logins you will directly arrive at the last view so the above procedure is only necessary on the first login.
{: .tip}

## Connecting to the MongoDB Ops Manager by using HTTPS

{{site.data.keyword.databases-for-mongodb}}EE offers an HTTPS accessible endpoint for the Ops Manager user interface. 

{{site.data.keyword.databases-for-mongodb}}EE also offers both private and public cloud service endpoints. If you choose to enable *only* private endpoints, then you must take additional steps to access the management interface over HTTPS. We recommend: 
* setting up a VSE (VM) in softlayer and SSH into it with `ssh -D 2345` 
* then, configure your browser to use socks proxy on `localhost:2345`. 
  



## Ops Manager API key creation and usage

{{site.data.keyword.databases-for-mongodb}}EE provides access to the Ops Manager API through generation of API keys. To do this: 
* Navigate to your `Account` page 
* From the `Public API Access` tab, you are able to generate up to ten API keys by using the `Generate` button. 

![API Keys pane](images/api-keys.png)
These are the 'deprecated' personal api keys.
{: .note}

Using the Ops Manager connection string found in the [Getting connection strings](/docs/databases-for-mongodb?topic=databases-for-mongodb-getting-connection-strings.md) pane of your deployment, you can then use the credentials you created for Ops Manager user access to API commands. 

### Example connection details: 
 
You can use the example credentials as follow to query the Ops Manager 'user' API:
* Example username you created via the API: 
  * `opsmanager-123`
* Example API key generated using the prior steps: 
  * `d043b2ae-bbf2-4f55-8b09-e1ce0906126f` 
* Example connection string obtained from the deployment dashboard Connections pane: 
  * `https://opsmanager-d66861a0-9ec2-4d14-9f66-15d4ad4547ac.9b48689db8eb415ca00444450bd4e589.databases.appdomain.cloud:30649`  

Combine to form the following example command: 
```
curl -k --digest --user 'opsmanager-123:d043b2ae-bbf2-4f55-8b09-e1ce0906126f'  'https://opsmanager-d66861a0-9ec2-4d14-9f66-15d4ad4547ac.9b48689db8eb415ca00444450bd4e589.databases.appdomain.cloud:30649/api/public/v1.0/users/byName/opsmanager-123'
```
{: .pre}

