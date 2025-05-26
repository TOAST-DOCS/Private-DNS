## Network > Private DNS > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

Private DNS Zone과 레코드 세트는 `network` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| network | 한국(판교) 리전 | https://kr1-api-network-infrastructure.gov-nhncloudservice.com |
| network | 한국(평촌) 리전 | https://kr2-api-network-infrastructure.gov-nhncloudservice.com |


API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용하며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

## Private DNS Zone
### Private DNS Zone 목록 보기

```
GET /v2.0/privatedns/zones 
X-Auth-Token: {tokenId} 
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명                      |
| --- | --- | --- | --- |-------------------------|
| tokenId | Header | String | O | 토큰 ID                   |
| id | Query | UUID | - | 조회할 Private DNS Zone ID |
| name | Query | String | - | 조회할 Private DNS Zone 이름 |
| status | Query | String | - | 조회할 Private DNS Zone 상태       |


#### 응답

| 이름                | 종류 | 형식      | 설명                           |
|-------------------| --- |---------|------------------------------|
| zones             | Body | Array   | Private DNS Zone 객체 목록       |
| zones.id          | Body | UUID    | Private DNS Zone ID          |
| zones.name        | Body | String  | Private DNS Zone 이름          |
| zones.description | Body | String  | Private DNS Zone 설명          |
| zones.status      | Body | String  | Private DNS Zone 상태          |
 | zones.networks    | Body | Array   | Private DNS Zone에 속한 네트워크 목록 |
| zones.created_at  | Body | Date    | Private DNS Zone 생성한 시간      |
| zones.updated_at  | Body | Date    | Private DNS Zone 수정한 시간      |
| zones.rrset_count | Body | Integer | Private DNS Zone의 레코드 세트 개수  |

<details>
  <summary>예시</summary> 

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

### Private DNS Zone 보기

```
GET /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름      | 종류 | 형식 | 필수 | 설명                  |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | 토큰 ID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |

| 이름                | 종류 | 형식      | 설명                              |
|-------------------| --- |---------|---------------------------------|
| zones             | Body | Array   | Private DNS Zone 객체 목록          |
| zones.id          | Body | UUID    | Private DNS Zone ID            |
| zones.name        | Body | String  | Private DNS Zone 이름            |
| zones.description | Body | String  | Private DNS Zone 설명            |
| zones.status      | Body | String  | Private DNS Zone 상태            |
 | zones.networks    | Body | Array   | Private DNS Zone에 속한 네트워크 목록 |
| zones.created_at  | Body | Date    | Private DNS Zone 생성한 시간        |
| zones.updated_at  | Body | Date    | Private DNS Zone 수정한 시간        |
| zones.rrset_count | Body | Integer | Private DNS Zone의 레코드 세트 개수 |

### Private DNS Zone 생성하기

```
POST /v2.0/privatedns/zones
X-Auth-Token: {tokenId}
```

#### 요청

| 이름               | 종류 | 형식     | 필수  | 설명                                |
|------------------| --- |--------|-----|-----------------------------------|
| tokenId          | Header | String | O   | 토큰 ID                             |
| zone             | Body | Object | O   | Private DNS Zone 정보 객체            |
| zone.name        | Body | String | O   | Private DNS Zone 이름               |
| zone.description | Body | String | -   | Private DNS Zone 설명               |
| zone.networks    | Body | Arrray | O   | Private DNS Zone을 사용할 VPC UUID 목록 |

<details>
  <summary>예시</summary>

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

#### 응답

| 이름            | 종류 | 형식      | 설명                                     |
|---------------| --- |---------|----------------------------------------|
| zone          | Body | Array   | Private DNS Zone 정보 객체                 |
| zone.id       | Body | UUID    | Private DNS Zone ID                   |
| zone.name     | Body | String  | Private DNS Zone 이름                   |
| zone.description | Body | String  | Private DNS Zone 설명                   |
| zone.status   | Body | String  | Private DNS Zone 상태                   |
 | zone.networks | Body | Array   | Private DNS Zone 사용하는 Network UUID 목록 |
| zone.created_at | Body | Date    | Private DNS Zone 생성한 시간               |
| zone.updated_at | Body | Date    | Private DNS Zone 수정한 시간               |

<details>
  <summary>예시</summary>

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

### Private DNS Zone 수정하기

```
PUT /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름               | 종류 | 형식     | 필수  | 설명                                                                                         |
|------------------| --- |--------|-----|--------------------------------------------------------------------------------------------|
| tokenId          | Header | String | O   | 토큰 ID                                                                                      |
| zoneId           | URL | UUID | O   | Private DNS Zone ID                                                                        |
| zone             | Body | Object | O   | Private DNS Zone 정보 객체                                                                     |
| zone.description | Body | String | -   | Private DNS Zone 설명                                                                        |
| zone.networks    | Body | Arrray | O   | Private DNS Zone을 사용할 VPC UUID 목록 <br> VPC UUID 목록을 수정하는 경우, 기존 값은 모두 제거되고 수정 요청한 레코드로 적용됨 |

<details>
  <summary>예시</summary>

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

#### 응답

| 이름            | 종류 | 형식      | 설명                                     |
|---------------| --- |---------|----------------------------------------|
| zone          | Body | Array   | Private DNS Zone 정보 객체                 |
| zone.id       | Body | UUID    | Private DNS Zone ID                   |
| zone.name     | Body | String  | Private DNS Zone 이름                   |
| zone.description | Body | String  | Private DNS Zone 설명                   |
| zone.status   | Body | String  | Private DNS Zone 상태                   |
 | zone.networks | Body | Array   | Private DNS Zone 사용하는 Network UUID 목록 |
| zone.created_at | Body | Date    | Private DNS Zone 생성한 시간               |
| zone.updated_at | Body | Date    | Private DNS Zone 수정한 시간               |

<details>
  <summary>예시</summary>

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

### Private DNS Zone 삭제하기

```
DELETE /v2.0/privatedns/zones/{zoneId}
X-Auth-Token: {tokenId}
```

#### 요청


이 API는 요청 본문을 요구하지 않습니다.

| 이름      | 종류 | 형식 | 필수 | 설명                  |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | 토큰 ID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.

## 레코드 세트 
### 레코드 세트 목록 보기


```
GET /v2.0/privatedns/zones/{zoneId}/rrsets
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름      | 종류     | 형식      | 필수 | 설명                              |
|---------|--------|---------| --- |---------------------------------|
| tokenId | Header | String  | O | 토큰 ID                           |
| zoneId  | URL    | UUID    | O | 조회할 레코드 세트 Private DNS Zone ID |
| id      | Query  | UUID    | - | 조회할 레코드 세트 ID                  |
| name    | Query  | String  | - | 조회할 레코드 세트 이름                   |
| type    | Query  | String  | - | 조회할 레코드 타입                     |
| ttl     | Query  | Integer | - | 조회할 레코드 ttl                    |

#### 응답

| 이름                     | 종류 | 형식      | 설명                                                                                              |
|------------------------| --- |---------|-------------------------------------------------------------------------------------------------|
| rrsets                 | Body | Array   | 레코드 세트 객체 목록                                                                                    |
| rrsets.id              | Body | UUID    | 레코드 세트 ID                                                                                       |
| rrsets.name            | Body | String  | 레코드 세트 이름                                                                                       |
| rrsets.description     | Body | String  | 레코드 세트 설명                                                                                       |
| rrsets.zone_id         | Body | String  | 레코드 세트가 속한 Private DNS Zone ID                                                                  |
| rrsets.ttl             | Body | Integer | 레코드 세트 ttl                                                                                      |
| rrsets.type            | Body | String  | 레코드 세트 타입.  `A`, `AAAA`, `CAA`, `CNAME`, `MX`, `NAPTR`, `NS`, `PTR`, `SOA`, `SPF`, `SRV`, `TXT` |
 | rrsets.records         | Body | Array   | 레코드 세트에 속한 레코드 객체 목록                                                                            |
| rrsets.records.content | Body | String  | 레코드 세트 레코드 값                                                                                     |
| rrsets.created_at      | Body | Date    | 레코드 세트 생성한 시간                                                                                   |
| rrsets.updated_at      | Body | Date    | 레코드 세트 수정한 시간                                                                                   |

<details>
  <summary>예시</summary>

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

### 레코드 세트 보기

```
GET /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름      | 종류 | 형식 | 필수 | 설명                  |
|---------| --- | --- | --- |---------------------|
| tokenId | Header | String | O | 토큰 ID               |
| zoneId  | URL | UUID | O | Private DNS Zone ID |
| rrsetId | URL | UUID | O | 레코드 세트 ID           |

#### 응답

| 이름             | 종류 | 형식      | 설명                                                                                      |
|----------------| --- |---------|-----------------------------------------------------------------------------------------|
| rrset          | Body | Array   | 레코드 세트 객체 목록                                                                            |
| rrset.id       | Body | UUID    | 레코드 세트 ID                                                                              |
| rrset.name     | Body | String  | 레코드 세트 이름                                                                              |
| rrset.description | Body | String  | 레코드 세트 설명                                                                              |
| rrset.zone_id  | Body | String  | 레코드 세트가 속한 Private DNS Zone ID                                                          |
| rrset.ttl      | Body | Integer | 레코드 세트 정보 갱신 주기. 기본값은 `300`                                                                             |
| rrset.type     | Body | String  | 레코드 세트 타입.  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
 | rrset.records  | Body | Array   | 레코드 세트에 속한 레코드 객체 목록                                                                    |
| rrset.records.content | Body | Array   | 레코드 세트 레코드 값                                                                            |
| rrset.created_at | Body | Date    | 레코드 세트 생성한 시간                                                                          |
| rrset.updated_at | Body | Date    | 레코드 세트 수정한 시간                                                                          |

<details>
  <summary>예시</summary>

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

### 레코드 세트 생성하기

```
POST /v2.0/privatedns/zones/{zoneId}/rrsets
X-Auth-Token: {tokenId}
```

#### 요청

| 이름                    | 종류 | 형식      | 필수 | 설명                                                                                      |
|-----------------------| --- |---------| --- |-----------------------------------------------------------------------------------------|
| tokenId               | Header | String  | O | 토큰 ID                                                                                   |
| zoneId                | URL | UUID    | O | Private DNS Zone ID                                                                     |
| rrset                 | Body | Object  | O   | 레코드 세트 정보 객체                                                                            |
| rrset.name            | Body | String  | O   | 레코드 세트 이름                                                                               |
| rrset.records         | Body | Array   | O   | 레코드 세트 레코드 목록                                                                          |
| rrset.records.content | Body | String  | O   | 레코드 값                                                                                   |
| rrset.ttl             | Body | Integer | -   | 레코드 세트 정보 갱신 주기. 기본값은 `300`                                                                             |
| rrset.type            | Body | String  | O   | 레코드 세트 타입.  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
| rrset.description     | Body | String  | O   | 레코드 세트 설명                                                                              |


<details>
  <summary>예시</summary>

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

#### 응답

| 이름             | 종류 | 형식      | 설명                                                                                      |
|----------------| --- |---------|-----------------------------------------------------------------------------------------|
| rrset          | Body | Array   | 레코드 세트 객체 목록                                                                            |
| rrset.id       | Body | UUID    | 레코드 세트 ID                                                                              |
| rrset.name     | Body | String  | 레코드 세트 이름                                                                              |
| rrset.description | Body | String  | 레코드 세트 설명                                                                              |
| rrset.zone_id  | Body | String  | 레코드 세트가 속한 Private DNS Zone ID                                                          |
| rrset.ttl      | Body | Integer | 레코드 세트 정보 갱신 주기. 기본값은 `300`                                                                             |
| rrset.type     | Body | String  | 레코드 세트 타입.  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
 | rrset.records  | Body | Array   | 레코드 세트에 속한 레코드 객체 목록                                                                    |
| rrset.records.content | Body | Array   | 레코드 세트 레코드 값                                                                            |
| rrset.created_at | Body | Date    | 레코드 세트 생성한 시간                                                                          |
| rrset.updated_at | Body | Date    | 레코드 세트 수정한 시간                                                                          |

<details>
  <summary>예시</summary>

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

### 레코드 세트 수정하기

```
PUT /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름                    | 종류 | 형식      | 필수 | 설명                                                                                     |
|-----------------------| --- |---------| --- |----------------------------------------------------------------------------------------|
| tokenId               | Header | String  | O | 토큰 ID                                                                                  |
| zoneId                | URL | UUID    | O | Private DNS Zone ID                                                                    |
| rrsetId                | URL | UUID    | O | 수정할 레코드 세트 ID                                                                          |
| rrset                 | Body | Object  | O   | 레코드 세트 정보 객체                                                                           |
| rrset.records         | Body | Array   | O   | 레코드 세트 레코드 목록 <br> 레코드 목록을 수정하는 경우, 기존 값은 모두 제거되고 수정 요청한 레코드로 적용됨                      |
| rrset.records.content | Body | String  | O   | 레코드 값                                                                                  |
| rrset.ttl             | Body | Integer | -   | 레코드 세트 정보 갱신 주기. 기본값은 `300`                                                            |
| rrset.type            | Body | String  | O   | 레코드 세트 타입.  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
| rrset.description     | Body | String  | O   | 레코드 세트 설명                                                                              |

<details>
  <summary>예시</summary>

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

#### 응답

| 이름             | 종류 | 형식      | 설명                                                                                 |
|----------------| --- |---------|------------------------------------------------------------------------------------|
| rrset          | Body | Array   | 레코드 세트 객체 목록                                                                       |
| rrset.id       | Body | UUID    | 레코드 세트 ID                                                                          |
| rrset.name     | Body | String  | 레코드 세트 이름                                                                          |
| rrset.description | Body | String  | 레코드 세트 설명                                                                          |
| rrset.zone_id  | Body | String  | 레코드 세트가 속한 Private DNS Zone ID                                                     |
| rrset.ttl      | Body | Integer | 레코드 세트 정보 갱신 주기. 기본값은 `300`                                                                         |
| rrset.type     | Body | String  | 레코드 세트 타입.  `A`, `AAAA`, `CAA` `CNAME` `MX` `NAPTR` `NS` `PTR` `SOA` `SPF` `SRV` `TXT` |
 | rrset.records  | Body | Array   | 레코드 세트에 속한 레코드 객체 목록                                                               |
| rrset.records.content | Body | Array   | 레코드 세트 레코드 값                                                                        |
| rrset.created_at | Body | Date    | 레코드 세트 생성한 시간                                                                     |
| rrset.updated_at | Body | Date    | 레코드 세트 수정한 시간                                                                     |

<details>
  <summary>예시</summary>

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

### 레코드 세트 삭제하기

```
DELETE /v2.0/privatedns/zones/{zoneId}/rrsets/{rrsetId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름      | 종류 | 형식 | 필수 | 설명             |
|---------| --- | --- | --- |----------------|
| tokenId | Header | String | O | 토큰 ID          |
| zoneId  | URL | UUID | O | Private DNS Zone ID |
| rrsetId | URL | UUID    | O | 수정할 레코드 세트 ID  |

#### 응답

이 API는 응답 본문을 반환하지 않습니다.