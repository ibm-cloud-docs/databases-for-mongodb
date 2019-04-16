---

Copyright:
  years: 2019
lastupdated: "2019-04-08"

subcollection: cloud-databases

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Using IP Whitelists
{: #whitelisting}

If you want to restrict access to your databases, you can whitelist specific IP addresses or ranges of IP addresses on your deployment. When you create a whitelist, only IP addresses that match the whitelist or are in the range of IP addresses in the whitelist can connect to your deployment. Whitelists can be enabled for both public endpoints and private endpoints. When no IP addresses are in the whitelist, the whitelist is disabled and the deployment accepts connections from any IP address.

Even if not explicitly whitelisted, {{site.data.keyword.cloud_notm}} management services are still able to connect.
{: .tip}

## Setting a Whitelist

The UI for managing whitelists is on the _Settings_ tab of your _Deployment Overview_.

### IP addresses

The *IP* field can take a single complete IPv4 address or IPv6 address with or without a netmask. Without a netmask, incoming connections must come from exactly that IP address. 

Although the *IP* field allows for IPv6, no deployments are currently available to IPv6 networking and so these addresses cannot be filtered on.
{: tip}

### Netmasks

To allow a connection from a specified range of IP addresses, use a netmask. The IP address must be fully specified. That means entering, for example, 192.168.1.0/24 rather than 192.168.1/24.

### Description

The *Description* can be any user-significant text for identifying the whitelist entry - a customer name, project identifier, or employee number, for example. The description field is required.

## Setting a Whitelist through the CLI

The {{site.data.keyword.databases-for}} CLI Plug-in offers a set of commands for managing whitelists. Use the [`cdb deployment-whitelist-add`](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-whitelist-add) to add a whitelist and [`cdb deployment-whitelist-list`](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference#deployment-whitelist-list) to view the current whitelist.

More information is on the {{site.data.keyword.databases-for}} CLI Plug-in [reference page](/docs/databases-cli-plugin?topic=cloud-databases-cli-cdb-reference).

## Setting a Whitelist through the API

To use the {{site.data.keyword.databases-for}} API to manage your whitelist with the [`/deployments/{id}/whitelists/`](https://cloud.ibm.com/apidocs/cloud-databases-api#retrieve-the-whitelisted-addresses-and-ranges-for-) endpoint. You can retrieve the current whitelist, add entries to the whitelist, and also bulk upload IP addresses to the whitelist from the API. 

More information is in the [API Reference](https://cloud.ibm.com/apidocs/cloud-databases-api)

## Removal

From the UI remove an IP address or netmask from the Whitelist by clicking *Remove*. You can also use CLI command is `cdb deployment-whitelist-delete` or send a `DELETE` request to the API endpoint. When all entries on the whitelist are removed, the whitelist is disabled and all IP addresses are accepted by your deployment.