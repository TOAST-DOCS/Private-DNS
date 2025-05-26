## Network > Private DNS > API v2ガイド

APIを使用するにはAPIエンドポイントとトークンなどが必要です。 [API使用準備](/Compute/Compute/ko/identity-api/)を参考にしてAPI使用に必要な情報を準備します。

Private DNS Zoneとレコードセットは`network`タイプエンドポイントを利用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| network | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com |


APIレスポンスにガイドに記載されていないフィールドが表示される場合があります。このようなフィールドは、NHN Cloudの内部用途に使用され、事前告知なしに変更される可能性があるため、使用しないでください。

## Private DNS Zone
### Private DNS Zoneリスト表示

```
GET /v2.0/privatedns/zones 
X-Auth-Token: {tokenId} 
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明                    |
| --- | --- | --- | --- |-------------------------|
| tokenId | Header | String | O | トークンID                   |
| id | Query | UUID | - | 照会するPrivate DNS Zone ID |
| name | Query | String | - | 照会するPrivate DNS Zone名 |
| status | Query | String | - | 照会するPrivate DNS Zone状態     |


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

### Private DNS Zone表示

```
GET /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

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

### Private DNS Zone作成

```
POST /v2.0/privatedns/zones
X-Auth-Token: {tokenId}
```

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

### Private DNS Zone修正

```
PUT /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

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

### Private DNS Zone削除

```
DELETE /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

#### リクエスト


このAPIはリクエスト本文を要求しません。

| 名前    | 種類 | 形式 | 必須 | 説明                |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | トークンID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |

#### レスポンス

このAPIはレスポンス本文を返しません。

## レコードセット 
### レコードセットリスト表示


```
GET /v2.0/privatedns/zones/{zoneId}/rrsets
X-Auth-Token: {tokenId}
```

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

### レコードセット表示

```
GET /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前    | 種類 | 形式 | 必須 | 説明                |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | トークンID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |
| rrsetId | URL | UUID | O | レコードセットID           |

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

### レコードセット作成

```
POST /v2.0/privatedns/zones/{zoneId}/rrsets
X-Auth-Token: {tokenId}
```

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

### レコードセット修正

```
PUT /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

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

### レコードセット削除

```
DELETE /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前    | 種類 | 形式 | 必須 | 説明           |
|---------| --- | --- | --- |----------------|
| tokenId | Header | String | O | トークンID          |
| zoneId  | URL | UUID | O | Private DNS Zone ID |
| rrsetId | URL | UUID    | O | 修正するレコードセットID  |

#### レスポンス

このAPIはレスポンス本文を返しません。
