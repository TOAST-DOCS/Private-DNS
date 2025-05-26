## Network > Private DNS > API v2 Guide

To use the API, API endpoint and token are required. Refer to [API usage preparations](/Compute/Compute/en/identity-api/) to prepare the information required to use the API.

Private DNS zones and recordsets use the endpoints of the `network` type. See the `serviceCatalog`in the token issuance response for the exact endpoint.

| Type | Region | Endpoint |
|---|---|---|
| network | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com |


Fields not specified in the guide may appear in the API response. Do not use these fields as they are for NHN Cloud internal use and are subject to change without notice.

## Private DNS Zone
### View the list of Private DNS Zones

```
GET /v2.0/privatedns/zones 
X-Auth-Token: {tokenId} 
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description                      |
| --- | --- | --- | --- |-------------------------|
| tokenId | Header | String | O | Token ID                   |
| id | Query | UUID | - | Private DNS Zone ID to retrieve |
| name | Query | String | - | Private DNS Zone name to retrieve |
| String | Query | String | - | Private DNS Zone status to retrieve       |


#### Response

| Name                | Type | Format      | Description                              |
|-------------------| --- |---------|---------------------------------|
| zones             | Body | Array   | List of Private DNS Zone objects          |
| zones.id          | Body | UUID    | Private DNS Zone ID            |
| zones.name        | Body | String  | Private DNS Zone name            |
| zones.description | Body | String  | Private DNS Zone description            |
| zones.status      | Body | String  | Private DNS Zone status            |
 | zones.networks    | Body | Array   | List of networks that belong to the Private DNS Zone |
| zones.created_at  | Body | Date    | Private DNS Zone Created Time        |
| zones.updated_at  | Body | Date    | Private DNS Zone Modified Time        |
| zones.rrset_count | Body | Integer | Number of recordsets the Private DNS Zone has |

<details>
  <summary>Example</summary> 

```json
{
  "zones": [
    {
      "id": "ecb6de30-5665-4f97-9b96-3ff06e90ecd8",
      "name": "example.net.",
      "description": "",
      "status": "ACTIVE",
      "networks": [
        "8970f625-0b41-4e04-8ece-64a7d5651113",
        "8a725559-671f-486f-96f0-8d77a21f6394"
      ],
      "created_at": "2025-05-11 06:44:23",
      "updated_at": "2025-05-11 06:44:23",
      "rrset_count": 10
    }
  ]
}
```

</details>

***

### View Private DNS Zone

```
GET /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name      | Type | Format | Required | Description                  |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | Token ID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |

| Name                | Type | Format      | Description                              |
|-------------------| --- |---------|---------------------------------|
| zones             | Body | Array   | List of Private DNS Zone objects          |
| zones.id          | Body | UUID    | Private DNS Zone ID            |
| zones.name        | Body | String  | Private DNS Zone name            |
| zones.description | Body | String  | Private DNS Zone description            |
| zones.status      | Body | String  | Private DNS Zone status            |
 | zones.networks    | Body | Array   | List of Network UUIDs used by Private DNS Zone |
| zones.created_at  | Body | Date    | Private DNS Zone Created Time        |
| zones.updated_at  | Body | Date    | Private DNS Zone Modified Time        |
| zones.rrset_count | Body | Integer | Number of recordsets the Private DNS Zone has |

### Create a Private DNS Zone

```
POST /v2.0/privatedns/zones
X-Auth-Token: {tokenId}
```

#### Request

| Name               | Type | Format     | Required  | Description                                |
|------------------| --- |--------|-----|-----------------------------------|
| tokenId          | Header | String | O   | Token ID                             |
| zone             | Body | Object | O   | Private DNS Zone Information Object            |
| zone.name        | Body | String | O   | Private DNS Zone name               |
| zone.description | Body | String | -   | Private DNS Zone description               |
| zone.networks    | Body | Arrray | O   | List of VPC UUIDs that will use the Private DNS Zone |

<details>
  <summary>Example</summary>

```json
{
  "zone": {
    "name": "example.net.",
    "description": "",
    "networks": [
      "8970f625-0b41-4e04-8ece-64a7d5651113",
      "8a725559-671f-486f-96f0-8d77a21f6394"
    ]
  }
}
```

</details>

#### Response

| Name            | Type | Format      | Description                                     |
|---------------| --- |---------|----------------------------------------|
| zone          | Body | Array   | Private DNS Zone Information Object                 |
| zone.id       | Body | UUID    | Private DNS Zone ID                   |
| zone.name     | Body | String  | Private DNS Zone name                   |
| zone.description | Body | String  | Private DNS Zone description                   |
| zone.status   | Body | String  | Private DNS Zone status                   |
 | zone.networks | Body | Array   | List of Network UUIDs used by Private DNS Zone |
| zone.created_at | Body | Date    | Private DNS Zone Created Time               |
| zone.updated_at | Body | Date    | Private DNS Zone Modified Time               |

<details>
  <summary>Example</summary>

```json
{
  "zone": {
    "id": "ecb6de30-5665-4f97-9b96-3ff06e90ecd8",
    "name": "example.net.",
    "description": "",
    "status": "BUILD",
    "networks": [
      "8970f625-0b41-4e04-8ece-64a7d5651113",
      "8a725559-671f-486f-96f0-8d77a21f6394"
    ],
    "created_at": "2025-05-11 06:44:23",
    "updated_at": "2025-05-11 06:44:23"
  }
}
```

</details>

### Modifying a Private DNS Zone

```
PUT /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

#### Request

| Name               | Type | Format     | Required  | Description                                                                                         |
|------------------| --- |--------|-----|--------------------------------------------------------------------------------------------|
| tokenId          | Header | String | O   | Token ID                                                                                      |
| zoneId           | URL | UUID | O   | Private DNS Zone ID                                                                        |
| zone             | Body | Object | O   | Private DNS Zone Information Object                                                                     |
| zone.description | Body | String | -   | Private DNS Zone description                                                                        |
| zone.networks    | Body | Arrray | O   | List of VPC UUIDs that will use the Private DNS Zone <br> If you modify the VPC UUID list, all existing values are removed and applied to the record you requested to modify. |

<details>
  <summary>Example</summary>

```json
{
  "zone": {
    "description": "test",
    "networks": [
      "2c590fdf-993d-4377-a49b-a54f66759909"
    ]
  }
}
```

</details>

#### Response

| Name            | Type | Format      | Description                                     |
|---------------| --- |---------|----------------------------------------|
| zone          | Body | Array   | Private DNS Zone Information Object                 |
| zone.id       | Body | UUID    | Private DNS Zone ID                   |
| zone.name     | Body | String  | Private DNS Zone name                   |
| zone.description | Body | String  | Private DNS Zone description                   |
| zone.status   | Body | String  | Private DNS Zone status                   |
 | zone.networks | Body | Array   | List of Network UUIDs used by Private DNS Zone |
| zone.created_at | Body | Date    | Private DNS Zone Created Time               |
| zone.updated_at | Body | Date    | Private DNS Zone Modified Time               |

<details>
  <summary>Example</summary>

```json
{
  "zone": {
    "id": "ecb6de30-5665-4f97-9b96-3ff06e90ecd8",
    "name": "example.net.",
    "description": "test",
    "status": "ACTIVE",
    "networks": [
      "2c590fdf-993d-4377-a49b-a54f66759909"
    ],
    "created_at": "2025-05-11 08:44:23",
    "updated_at": "2025-05-11 08:44:23"
  }
}
```

</details>

### Deleting a Private DNS Zone

```
DELETE /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

#### Request


This API does not require a request body.

| Name      | Type | Format | Required | Description                  |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | Token ID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |

#### Response

This API does not return a response body.

## Record set 
### View a list of recordsets


```
GET /v2.0/privatedns/zones/{zoneId}/rrsets
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name      | Type     | Format      | Required | Description                              |
|---------|--------|---------| --- |---------------------------------|
| tokenId | Header | String  | O | Token ID                           |
| zoneId  | URL    | UUID    | O | Recordset to lookup Private DNS Zone ID |
| id      | Query  | UUID    | - | Recordset ID to retrieve                  |
| name    | Query  | String  | - | Name of the recordset to retrieve                   |
| String    | Query  | String  | - | Record types to retrieve                     |
| ttl     | Query  | Integer | - | TTL of the record to retrieve                    |

#### Response

| Name                     | Type | Format      | Description                                                                                      |
|------------------------| --- |---------|-----------------------------------------------------------------------------------------|
| rrsets                 | Body | Array   | Recordset object list                                                                            |
| rrsets.id              | Body | UUID    | Record set ID                                                                              |
| rrsets.name            | Body | String  | Record set name                                                                              |
| rrsets.description     | Body | String  | Recordset description                                                                              |
| rrsets.zone_id         | Body | String  | The Private DNS Zone ID to which the recordset belongs                                                          |
| rrsets.ttl             | Body | Integer | Recordset TTL                                                                             |
| rrsets.type            | Body | String  | Recordset type. `a`, `aaaa`, `caa` `cname` `mx` `naptr` `ns` `ptr` `soa` `spf` `srv` `txt` |
 | rrsets.records         | Body | Array   | List of record objects in a recordset                                                                    |
| rrsets.records.content | Body | String  | Recordset record values                                                                            |
| rrsets.created_at      | Body | Date    | Time the recordset was created                                                                          |
| rrsets.updated_at      | Body | Date    | Time the recordset was modified                                                                          |

<details>
  <summary>Example</summary>

```json
{
  "rrsets": [
    {
      "id": "db12b85e-0b41-4c58-816f-dfb833ae098f",
      "name": "test.example.net.",
      "description": "DESCRIPTION",
      "zone_id": "ecb6de30-5665-4f97-9b96-3ff06e90ecd8",
      "ttl": 1000,
      "type": "A",
      "records": [
        {
          "content": "192.168.59.9"
        }
      ],
    "created_at": "2025-05-11 06:44:23",
    "updated_at": "2025-05-11 06:44:23"
    }
  ]
}
```

</details>

### View a Recordset

```
GET /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name      | Type | Format | Required | Description                  |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | Token ID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |
| rrsetId | URL | UUID | O | Record set ID           |

#### Response

| Name             | Type | Format      | Description                                                                                      |
|----------------| --- |---------|-----------------------------------------------------------------------------------------|
| rrset          | Body | Array   | Recordset object list                                                                            |
| rrset.id       | Body | UUID    | Record set ID                                                                              |
| rrset.name     | Body | String  | Record set name                                                                              |
| rrset.description | Body | String  | Recordset description                                                                              |
| rrset.zone_id  | Body | String  | The Private DNS Zone ID to which the recordset belongs                                                          |
| rrset.ttl      | Body | Integer | Recordset information update frequency. The default is `300`                                                                             |
| rrset.type     | Body | String  | Recordset type. `a`, `aaaa`, `caa` `cname` `mx` `naptr` `ns` `ptr` `soa` `spf` `srv` `txt` |
 | rrset.records  | Body | Array   | List of record objects in a recordset                                                                    |
| rrset.records.content | Body | Array   | Recordset record values                                                                            |
| rrset.created_at | Body | Date    | Time the recordset was created                                                                          |
| rrset.updated_at | Body | Date    | Time the recordset was modified                                                                          |

<details>
  <summary>Example</summary>

```json
{
  "rrsets": {
     "id": "db12b85e-0b41-4c58-816f-dfb833ae098f",
     "name": "test.example.net.",
     "description": "DESCRIPTION",
     "zone_id": "ecb6de30-5665-4f97-9b96-3ff06e90ecd8",
     "ttl": 1000,
     "type": "A",
     "records": [
       {
         "content": "192.168.59.9"
       }
     ],
   "created_at": "2025-05-11 06:44:23",
   "updated_at": "2025-05-11 06:44:23"
  }
}
```

</details>

### Create a recordset

```
POST /v2.0/privatedns/zones/{zoneId}/rrsets
X-Auth-Token: {tokenId}
```

#### Request

| Name                    | Type | Format      | Required | Description                                                                                      |
|-----------------------| --- |---------| --- |-----------------------------------------------------------------------------------------|
| tokenId               | Header | String  | O | Token ID                                                                                   |
| zoneId                | URL | UUID    | O | Private DNS Zone ID                                                                     |
| rrset                 | Body | Object  | O   | Recordset information objects                                                                            |
| rrset.name            | Body | String  | O   | Record set name                                                                               |
| rrset.records         | Body | Array   | O   | Recordsets Records list                                                                          |
| rrset.records.content | Body | String  | O   | Record values                                                                                   |
| rrset.ttl             | Body | Integer | -   | Recordset information update frequency. The default is `300`                                                                             |
| rrset.type            | Body | String  | O   | Recordset type. `a`, `aaaa`, `caa` `cname` `mx` `naptr` `ns` `ptr` `soa` `spf` `srv` `txt` |
| rrset.description     | Body | String  | O   | Recordset description                                                                              |


<details>
  <summary>Example</summary>

```json
{
  "rrset": {
    "name": "test.example.net.",
    "records": [
      {
        "content": "192.168.39.9"
      },
      {
        "content": "192.168.49.10"
      }
    ],
    "ttl": 86400,
    "type": "A",
    "description": "test"
  }
}
```

</details>

#### Response

| Name             | Type | Format      | Description                                                                                      |
|----------------| --- |---------|-----------------------------------------------------------------------------------------|
| rrset          | Body | Array   | Recordset object list                                                                            |
| rrset.id       | Body | UUID    | Record set ID                                                                              |
| rrset.name     | Body | String  | Record set name                                                                              |
| rrset.description | Body | String  | Recordset description                                                                              |
| rrset.zone_id  | Body | String  | The Private DNS Zone ID to which the recordset belongs                                                          |
| rrset.ttl      | Body | Integer | Recordset information update frequency. The default is `300`                                                                             |
| rrset.type     | Body | String  | Recordset type. `a`, `aaaa`, `caa` `cname` `mx` `naptr` `ns` `ptr` `soa` `spf` `srv` `txt` |
 | rrset.records  | Body | Array   | List of record objects in a recordset                                                                    |
| rrset.records.content | Body | Array   | Recordset record values                                                                            |
| rrset.created_at | Body | Date    | Time the recordset was created                                                                          |
| rrset.updated_at | Body | Date    | Time the recordset was modified                                                                          |

<details>
  <summary>Example</summary>

```json
{
  "rrset": {
    "id": "db12b85e-0b41-4c58-816f-dfb833ae098f",
    "name": "test.example.net.",
    "description": "test",
    "zone_id": "ecb6de30-5665-4f97-9b96-3ff06e90ecd8",
    "ttl": 86400,
    "type": "A",
    "records": [
      {
        "content": "192.168.39.9"
      },
      {
        "content": "192.168.49.10"
      }
    ],
   "created_at": "2025-05-11 06:44:23",
   "updated_at": "2025-05-11 06:44:23"
  }
}
```

</details>

### Modify a recordset

```
PUT /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

#### Request

| Name                    | Type | Format      | Required | Description                                                                                     |
|-----------------------| --- |---------| --- |----------------------------------------------------------------------------------------|
| tokenId               | Header | String  | O | Token ID                                                                                  |
| zoneId                | URL | UUID    | O | Private DNS Zone ID                                                                    |
| rrsetId                | URL | UUID    | O | Recordset ID to modify                                                                          |
| rrset                 | Body | Object  | O   | Recordset information objects                                                                           |
| rrset.records         | Body | Array   | O   | Recordsets Records list <br> If you modify a list of records, all existing values are removed and replaced with the records you requested to modify.                      |
| rrset.records.content | Body | String  | O   | Record values                                                                                  |
| rrset.ttl             | Body | Integer | -   | Recordset information update frequency. The default is `300`                                                            |
| rrset.type            | Body | String  | O   | Recordset type. `a`, `aaaa`, `caa` `cname` `mx` `naptr` `ns` `ptr` `soa` `spf` `srv` `txt` |
| rrset.description     | Body | String  | O   | Recordset description                                                                              |

<details>
  <summary>Example</summary>

```json
{
  "rrset": {
    "records": [
      {
        "content": "192.168.59.9"
      }
    ],
    "ttl": 1000,
    "description": "change"
  }
}
```

</details>

#### Response

| Name             | Type | Format      | Description                                                                                 |
|----------------| --- |---------|------------------------------------------------------------------------------------|
| rrset          | Body | Array   | Recordset object list                                                                       |
| rrset.id       | Body | UUID    | Record set ID                                                                          |
| rrset.name     | Body | String  | Record set name                                                                          |
| rrset.description | Body | String  | Recordset description                                                                          |
| rrset.zone_id  | Body | String  | The Private DNS Zone ID to which the recordset belongs                                                     |
| rrset.ttl      | Body | Integer | Recordset information update frequency. The default is `300`                                                                         |
| rrset.type     | Body | String  | Recordset type. `a`, `aaaa`, `caa` `cname` `mx` `naptr` `ns` `ptr` `soa` `spf` `srv` `txt` |
 | rrset.records  | Body | Array   | List of record objects in a recordset                                                               |
| rrset.records.content | Body | Array   | Recordset record values                                                                        |
| rrset.created_at | Body | Date    | Time the recordset was created                                                                     |
| rrset.updated_at | Body | Date    | Time the recordset was modified                                                                     |

<details>
  <summary>Example</summary>

```json
{
  "rrset": {
    "id": "db12b85e-0b41-4c58-816f-dfb833ae098f",
    "name": "test.example.net.",
    "description": "change",
    "zone_id": "ecb6de30-5665-4f97-9b96-3ff06e90ecd8",
    "ttl": 1000,
    "type": "A",
    "records": [
      {
        "content": "192.168.59.9"
      }
    ],
   "created_at": "2025-08-11 06:44:23",
   "updated_at": "2025-08-11 06:44:23"
  }
}
```

</details>

### Delete a recordset

```
DELETE /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name      | Type | Format | Required | Description             |
|---------| --- | --- | --- |----------------|
| tokenId | Header | String | O | Token ID          |
| zoneId  | URL | UUID | O | Private DNS Zone ID |
| rrsetId | URL | UUID    | O | Recordset ID to modify  |

#### Response

This API does not return a response body.