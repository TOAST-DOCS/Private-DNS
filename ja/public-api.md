<!-- machine_translated: true -->

<!-- pre-align:aligned sig=797776c94fa3 -->

{% if "gov" in build_flags -%}
  {%- set identity_env   = "" -%}
  {%- set token_suffix   = "-gov" -%}
  {%- set region_names   = "韓国(板橋)リージョン" -%}
  {%- set region_urls    = "https://kr1-api-network-infrastructure.gov-nhncloudservice.com" -%}
{%- elif "ncgn" in build_flags -%}
  {%- set identity_env   = "ncgn" -%}
  {%- set region_names   = "韓国(板橋)リージョン" -%}
  {%- set region_urls    = "https://kr1-api-network-infrastructure.gncloud.go.kr" -%}
{%- elif "ngsc" in build_flags -%}
  {%- set identity_env   = "ngsc" -%}
  {%- set region_names   = "韓国(大邱)リージョン" -%}
  {%- set region_urls    = "https://kr4-api-network-infrastructure.ngsc.go.kr" -%}
{%- elif "ngovc" in build_flags -%}
  {%- set identity_env   = "ngovc" -%}
  {%- set region_names   = "韓国(大邱)リージョン" -%}
  {%- set region_urls    = "https://kr4-api-network-infrastructure.ngovc.com" -%}
{%- elif "ngoic" in build_flags -%}
  {%- set identity_env   = "ngoic" -%}
  {%- set region_names   = "韓国(大邱)リージョン" -%}
  {%- set region_urls    = "https://kr4-api-network-infrastructure.ngoic.com" -%}
{%- elif "ninc" in build_flags -%}
  {%- set identity_env   = "ninc" -%}
  {%- set region_names   = "韓国(大邱)リージョン" -%}
  {%- set region_urls    = "https://kr4-api-network-infrastructure.ninc.go.kr" -%}
{%- else -%}
  {%- set identity_env   = "" -%}
  {%- set token_suffix   = "" -%}
  {%- set region_names   = "韓国(板橋)リージョン<br>韓国(坪村)リージョン<br>韓国(光州)リージョン" -%}
  {%- set region_urls    = "https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://kr3-api-network-infrastructure.nhncloudservice.com" -%}
{%- endif -%}

<a id="network-private-dns-api-v2-guide"></a>
## Network > Private DNS > API v2ガイド { #network-private-dns-api-v2-guide }

{% if identity_env -%}
API を使用するには、API エンドポイントとトークンなどが必要です。[API 使用準備](/Compute/Compute/ja/identity-api-$[ identity_env ]$/) を参照して、API の使用に必要な情報を準備します。
{%- else -%}
NHN Cloud Network サービスは、API 呼び出し時の認証/認可に IaaS トークンを使用します。IaaS トークンは、NHN Cloud の OpenStack ベースのインフラサービス (IaaS) で使用する認証トークンです。IaaS トークンの発行および使用の詳細については、[IaaS トークン](/nhncloud/ja/public-api/iaas-token$[ token_suffix ]$) を参照してください。
{%- endif %}

Private DNS Zoneとレコードセットは`network`タイプエンドポイントを利用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| network | $[ region_names ]$ | $[ region_urls ]$ |


APIレスポンスにガイドに記載されていないフィールドが表示される場合があります。このようなフィールドは、NHN Cloudの内部用途に使用され、事前告知なしに変更される可能性があるため、使用しないでください。

<a id="private-dns-zone"></a>
## Private DNS Zone { #private-dns-zone }
<a id="view-the-list-of-private-dns-zones"></a>
### Private DNS Zoneリスト表示 { #view-the-list-of-private-dns-zones }

```
GET /v2.0/privatedns/zones 
X-Auth-Token: {tokenId} 
```

<a id="view-the-list-of-private-dns-zones-request"></a>
#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明                    |
| --- | --- | --- | --- |-------------------------|
| tokenId | Header | String | O | トークンID                   |
| id | Query | UUID | - | 照会するPrivate DNS Zone ID |
| name | Query | String | - | 照会するPrivate DNS Zone名 |
| status | Query | String | - | 照会するPrivate DNS Zone状態     |


<a id="view-the-list-of-private-dns-zones-response"></a>
#### レスポンス

| 名前              | 種類 | 形式    | 説明                            |
|-------------------| --- |---------|---------------------------------|
| zones             | Body | Array   | Private DNS Zoneオブジェクトリスト        |
| zones.id          | Body | UUID    | Private DNS Zone ID            |
| zones.name        | Body | String  | Private DNS Zone名          |
| zones.description | Body | String  | Private DNS Zone説明          |
| zones.status      | Body | String  | Private DNS Zone状態          |
 | zones.networks    | Body | Array   | Private DNS Zoneに属するNetworkリスト |
| zones.created_at  | Body | Date    | Private DNS Zone作成した時間      |
| zones.updated_at  | Body | Date    | Private DNS Zone修正した時間      |
| zones.rrset_count | Body | Integer | Private DNS Zoneが持っているレコードセット数 |

<details>
  <summary>例</summary> 

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

<a id="view-private-dns-zone"></a>
### Private DNS Zone表示 { #view-private-dns-zone }

```
GET /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

<a id="view-private-dns-zone-request"></a>
#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前    | 種類 | 形式 | 必須 | 説明                |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | トークンID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |

| 名前              | 種類 | 形式    | 説明                            |
|-------------------| --- |---------|---------------------------------|
| zones             | Body | Array   | Private DNS Zoneオブジェクトリスト        |
| zones.id          | Body | UUID    | Private DNS Zone ID            |
| zones.name        | Body | String  | Private DNS Zone名          |
| zones.description | Body | String  | Private DNS Zone説明          |
| zones.status      | Body | String  | Private DNS Zone状態          |
 | zones.networks    | Body | Array   | Private DNS Zone使用するNetwork UUIDリスト |
| zones.created_at  | Body | Date    | Private DNS Zone作成した時間      |
| zones.updated_at  | Body | Date    | Private DNS Zone修正した時間      |
| zones.rrset_count | Body | Integer | Private DNS Zoneが持っているレコードセット数 |

<a id="create-a-private-dns-zone"></a>
### Private DNS Zone作成 { #create-a-private-dns-zone }

```
POST /v2.0/privatedns/zones
X-Auth-Token: {tokenId}
```

<a id="create-a-private-dns-zone-request"></a>
#### リクエスト

| 名前             | 種類 | 形式   | 必須 | 説明                              |
|------------------| --- |--------|-----|-----------------------------------|
| tokenId          | Header | String | O   | トークンID                             |
| zone             | Body | Object | O   | Private DNS Zone情報オブジェクト          |
| zone.name        | Body | String | O   | Private DNS Zone名             |
| zone.description | Body | String | -   | Private DNS Zone説明             |
| zone.networks    | Body | Arrray | O   | Private DNS Zoneを使用するVPC UUIDリスト |

<details>
  <summary>例</summary>

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

<a id="create-a-private-dns-zone-response"></a>
#### レスポンス

| 名前          | 種類 | 形式    | 説明                                   |
|---------------| --- |---------|----------------------------------------|
| zone          | Body | Array   | Private DNS Zone情報オブジェクト               |
| zone.id       | Body | UUID    | Private DNS Zone ID                   |
| zone.name     | Body | String  | Private DNS Zone名                 |
| zone.description | Body | String  | Private DNS Zone説明                 |
| zone.status   | Body | String  | Private DNS Zone状態                 |
 | zone.networks | Body | Array   | Private DNS Zone使用するNetwork UUIDリスト |
| zone.created_at | Body | Date    | Private DNS Zone作成した時間             |
| zone.updated_at | Body | Date    | Private DNS Zone修正した時間             |

<details>
  <summary>例</summary>

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

<a id="modifying-a-private-dns-zone"></a>
### Private DNS Zone修正 { #modifying-a-private-dns-zone }

```
PUT /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

<a id="modifying-a-private-dns-zone-request"></a>
#### リクエスト

| 名前             | 種類 | 形式   | 必須 | 説明                                                                                       |
|------------------| --- |--------|-----|--------------------------------------------------------------------------------------------|
| tokenId          | Header | String | O   | トークンID                                                                                      |
| zoneId           | URL | UUID | O   | Private DNS Zone ID                                                                        |
| zone             | Body | Object | O   | Private DNS Zone情報オブジェクト                                                                   |
| zone.description | Body | String | -   | Private DNS Zone説明                                                                      |
| zone.networks    | Body | Arrray | O   | Private DNS Zoneを使用するVPC UUIDリスト <br> VPC UUIDリストを修正する場合、既存の値は全て削除され、修正リクエストしたレコードに適用されます。

<details>
  <summary>例</summary>

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

<a id="modifying-a-private-dns-zone-response"></a>
#### レスポンス

| 名前          | 種類 | 形式    | 説明                                   |
|---------------| --- |---------|----------------------------------------|
| zone          | Body | Array   | Private DNS Zone情報オブジェクト               |
| zone.id       | Body | UUID    | Private DNS Zone ID                   |
| zone.name     | Body | String  | Private DNS Zone名                 |
| zone.description | Body | String  | Private DNS Zone説明                 |
| zone.status   | Body | String  | Private DNS Zone状態                 |
 | zone.networks | Body | Array   | Private DNS Zone使用するNetwork UUIDリスト |
| zone.created_at | Body | Date    | Private DNS Zone作成した時間             |
| zone.updated_at | Body | Date    | Private DNS Zone修正した時間             |

<details>
  <summary>例</summary>

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

<a id="deleting-a-private-dns-zone"></a>
### Private DNS Zone削除 { #deleting-a-private-dns-zone }

```
DELETE /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

<a id="deleting-a-private-dns-zone-request"></a>
#### リクエスト


このAPIはリクエスト本文を要求しません。

| 名前    | 種類 | 形式 | 必須 | 説明                |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | トークンID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |

<a id="deleting-a-private-dns-zone-response"></a>
#### レスポンス

このAPIはレスポンス本文を返しません。

<a id="record-set"></a>
## レコードセット { #record-set }
<a id="view-a-list-of-recordsets"></a>
### レコードセットリスト表示 { #view-a-list-of-recordsets }


```
GET /v2.0/privatedns/zones/{zoneId}/rrsets
X-Auth-Token: {tokenId}
```

<a id="view-a-list-of-recordsets-request"></a>
#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前    | 種類   | 形式    | 必須 | 説明                            |
|---------|--------|---------| --- |---------------------------------|
| tokenId | Header | String  | O | トークンID                           |
| zoneId  | URL    | UUID    | O | 照会するレコードセットPrivate DNS Zone ID |
| id      | Query  | UUID    | - | 照会するレコードセットID                  |
| name    | Query  | String  | - | 照会するレコードセット名                 |
| type    | Query  | String  | - | 照会するレコードタイプ                   |
| ttl     | Query  | Integer | - | 照会するレコードttl                    |

<a id="view-a-list-of-recordsets-response"></a>
#### レスポンス

| 名前                   | 種類 | 形式    | 説明                                                                                    |
|------------------------| --- |---------|-----------------------------------------------------------------------------------------|
| rrsets                 | Body | Array   | レコードセットオブジェクトリスト                                                                          |
| rrsets.id              | Body | UUID    | レコードセットID                                                                              |
| rrsets.name            | Body | String  | レコードセット名                                                                            |
| rrsets.description     | Body | String  | レコードセット説明                                                                            |
| rrsets.zone_id         | Body | String  | レコードセットが属するPrivate DNS Zone ID                                                          |
| rrsets.ttl             | Body | Integer | レコードセットttl                                                                             |
| rrsets.type            | Body | String  | レコードセットタイプ。  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
 | rrsets.records         | Body | Array   | レコードセットに属するレコードオブジェクトリスト                                                                  |
| rrsets.records.content | Body | String  | レコードセットレコード値                                                                          |
| rrsets.created_at      | Body | Date    | レコードセット作成した時間                                                                        |
| rrsets.updated_at      | Body | Date    | レコードセット修正した時間                                                                        |

<details>
  <summary>例</summary>

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

<a id="view-a-recordset"></a>
### レコードセット表示 { #view-a-recordset }

```
GET /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

<a id="view-a-recordset-request"></a>
#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前    | 種類 | 形式 | 必須 | 説明                |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | トークンID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |
| rrsetId | URL | UUID | O | レコードセットID           |

<a id="view-a-recordset-response"></a>
#### レスポンス

| 名前           | 種類 | 形式    | 説明                                                                                    |
|----------------| --- |---------|-----------------------------------------------------------------------------------------|
| rrset          | Body | Array   | レコードセットオブジェクトリスト                                                                          |
| rrset.id       | Body | UUID    | レコードセットID                                                                              |
| rrset.name     | Body | String  | レコードセット名                                                                            |
| rrset.description | Body | String  | レコードセット説明                                                                            |
| rrset.zone_id  | Body | String  | レコードセットが属するPrivate DNS Zone ID                                                          |
| rrset.ttl      | Body | Integer | レコードセット情報更新周期。デフォルト値は`300`                                                                             |
| rrset.type     | Body | String  | レコードセットタイプ。  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
 | rrset.records  | Body | Array   | レコードセットに属するレコードオブジェクトリスト                                                                  |
| rrset.records.content | Body | Array   | レコードセットレコード値                                                                          |
| rrset.created_at | Body | Date    | レコードセット作成した時間                                                                        |
| rrset.updated_at | Body | Date    | レコードセット修正した時間                                                                        |

<details>
  <summary>例</summary>

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

<a id="create-a-recordset"></a>
### レコードセット作成 { #create-a-recordset }

```
POST /v2.0/privatedns/zones/{zoneId}/rrsets
X-Auth-Token: {tokenId}
```

<a id="create-a-recordset-request"></a>
#### リクエスト

| 名前                  | 種類 | 形式    | 必須 | 説明                                                                                    |
|-----------------------| --- |---------| --- |-----------------------------------------------------------------------------------------|
| tokenId               | Header | String  | O | トークンID                                                                                   |
| zoneId                | URL | UUID    | O | Private DNS Zone ID                                                                     |
| rrset                 | Body | Object  | O   | レコードセット情報オブジェクト                                                                          |
| rrset.name            | Body | String  | O   | レコードセット名                                                                             |
| rrset.records         | Body | Array   | O   | レコードセットレコードリスト                                                                        |
| rrset.records.content | Body | String  | O   | レコード値                                                                                 |
| rrset.ttl             | Body | Integer | -   | レコードセット情報更新周期。デフォルト値は`300`                                                                             |
| rrset.type            | Body | String  | O   | レコードセットタイプ。  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
| rrset.description     | Body | String  | O   | レコードセット説明                                                                            |


<details>
  <summary>例</summary>

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

<a id="create-a-recordset-response"></a>
#### レスポンス

| 名前           | 種類 | 形式    | 説明                                                                                    |
|----------------| --- |---------|-----------------------------------------------------------------------------------------|
| rrset          | Body | Array   | レコードセットオブジェクトリスト                                                                          |
| rrset.id       | Body | UUID    | レコードセットID                                                                              |
| rrset.name     | Body | String  | レコードセット名                                                                            |
| rrset.description | Body | String  | レコードセット説明                                                                            |
| rrset.zone_id  | Body | String  | レコードセットが属するPrivate DNS Zone ID                                                          |
| rrset.ttl      | Body | Integer | レコードセット情報更新周期。デフォルト値は`300`                                                                             |
| rrset.type     | Body | String  | レコードセットタイプ。  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
 | rrset.records  | Body | Array   | レコードセットに属するレコードオブジェクトリスト                                                                  |
| rrset.records.content | Body | Array   | レコードセットレコード値                                                                          |
| rrset.created_at | Body | Date    | レコードセット作成した時間                                                                        |
| rrset.updated_at | Body | Date    | レコードセット修正した時間                                                                        |

<details>
  <summary>例</summary>

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

<a id="modify-a-recordset"></a>
### レコードセット修正 { #modify-a-recordset }

```
PUT /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

<a id="modify-a-recordset-request"></a>
#### リクエスト

| 名前                  | 種類 | 形式    | 必須 | 説明                                                                                   |
|-----------------------| --- |---------| --- |----------------------------------------------------------------------------------------|
| tokenId               | Header | String  | O | トークンID                                                                                  |
| zoneId                | URL | UUID    | O | Private DNS Zone ID                                                                    |
| rrsetId                | URL | UUID    | O | 修正するレコードセットID                                                                          |
| rrset                 | Body | Object  | O   | レコードセット情報オブジェクト                                                                         |
| rrset.records         | Body | Array   | O   | レコードセットレコードリスト <br> レコードリストを修正する場合、既存の値は全て削除され、修正リクエストされたレコードに適用されます。                      |
| rrset.records.content | Body | String  | O   | レコード値                                                                                |
| rrset.ttl             | Body | Integer | -   | レコードセット情報更新周期。デフォルト値は`300`                                                            |
| rrset.type            | Body | String  | O   | レコードセットタイプ。  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
| rrset.description     | Body | String  | O   | レコードセットの説明                                                                            |

<details>
  <summary>例</summary>

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

<a id="modify-a-recordset-response"></a>
#### レスポンス

| 名前           | 種類 | 形式    | 説明                                                                               |
|----------------| --- |---------|------------------------------------------------------------------------------------|
| rrset          | Body | Array   | レコードセットオブジェクトリスト                                                                     |
| rrset.id       | Body | UUID    | レコードセットID                                                                          |
| rrset.name     | Body | String  | レコードセット名                                                                        |
| rrset.description | Body | String  | レコードセットの説明                                                                        |
| rrset.zone_id  | Body | String  | レコードセットが属するPrivate DNS Zone ID                                                     |
| rrset.ttl      | Body | Integer | レコードセット情報更新周期。デフォルト値は`300`                                                                         |
| rrset.type     | Body | String  | レコードセットタイプ。  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
 | rrset.records  | Body | Array   | レコードセットに属するレコードオブジェクトリスト                                                             |
| rrset.records.content | Body | Array   | レコードセットレコード値                                                                      |
| rrset.created_at | Body | Date    | レコードセットを作成した時間                                                                   |
| rrset.updated_at | Body | Date    | レコードセットを修正した時間                                                                   |

<details>
  <summary>例</summary>

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

<a id="delete-a-recordset"></a>
### レコードセット削除 { #delete-a-recordset }

```
DELETE /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

<a id="delete-a-recordset-request"></a>
#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前    | 種類 | 形式 | 必須 | 説明           |
|---------| --- | --- | --- |----------------|
| tokenId | Header | String | O | トークンID          |
| zoneId  | URL | UUID | O | Private DNS Zone ID |
| rrsetId | URL | UUID    | O | 修正するレコードセットID  |

<a id="delete-a-recordset-response"></a>
#### レスポンス

このAPIはレスポンス本文を返しません。
