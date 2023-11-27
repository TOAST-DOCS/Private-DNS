## Network > Private DNS > Overview

Private DNS allows you to configure independent DNS per VPC. You can make DNS service accessible to instances within a VPC by configuring the settings in the web console, without separate DNS solutions or servers.

## Main Features
- Configures a separate DNS per VPC.
- You don't need a separate DNS solution or server to configure DNS services accessible to instances within a VPC.
- Connects one or more VPCs to one zone (domain area). 
- If you request for a zone from outside, you will not receive a response. 
- Instances within a VPC can receive responses to requests for domains registered through external organizations (<i>e.g.,</i> nhncloud.com) as well as zones connected to the VPC. 
- For VPCs using Private DNS, a private IP DNS (local domain) is provided. 
- You can access instances within a VPC via private IP DNS without any separate configuration.

## Service Targets

- When you need to set up a domain accessible only within a VPC
- When you need a setup to link with NHN Cloud services

## Service Terms

| Term | Descriptions |
|---|---|
| Domain | This is a way of representing the address that identify the computer on the network in a form that is easily recognizable by humans. |
| DNS Zone | The domain zone of a host that DNS serves. It is a container for a set of records and contains how to route traffic for a domain and its subdomains. |
| Record set | Host information served by DNS Zone, which is how to route traffic in domain. |
| Record set type | It is a resource type indicating how traffic on the host is routed. |
| Record value | This is to describes how to route traffic on a host, whose input depends on the type of record set (resource type) and can be registered with one or more except for a specific type. |
