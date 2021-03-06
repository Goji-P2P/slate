## *Instruments*

The Instruments API allows an Investment Manager to:
 
 (1) [create new instruments](/#settlement-equity-instrument-creation) and (2) [issue/ allocate shares](/#settlement-equity-issuing-allocating-shares) of that instrument.

A registered instrument can then be used for:

 * [Trade Settlement](/#settlement-equity-trade-settlement)
 * [Corporate Actions](/#settlement-equity-corporate-actions)

## Instrument Creation

Instrument creation allows registering a new [Instrument](/#settlement-equity-instrument-model).

This instrument would then be referenced in requests to endpoints e.g. [Trade Settlement](/#settlement-equity-trade-settlement) by instrument `symbol`.  

Currently only equity instruments are supported.

## Instrument Model

The Instrument model used for endpoints: 

(1) [`POST /instruments`](/#settlement-equity-post-instruments) 

(2) [`GET /instruments/{symbol}`](/#settlement-equity-get-instruments-symbol)

⚠️ Please note that before registering instruments, BankAccountIds have to be registered at the endpoint: 

[`POST /platformApi/bankAccountDetails`](/#payments-manager-post-platformapi-bankaccountdetails)

before they can be used for the following instrument attributes: 

(1) `paymentInstructions.primaryMarketBankAccountId` 

(2) `feeTypes[].bankAccountId`

⚠️ Please note that before registering instruments, ownerPartyIds have to be registered at the endpoint:

[`POST /companies`](/#payments-manager-post-companies)

before they can be used for the following instrument attributes
the company is required to be in a <br>
`REGISTERED` state:

(1) `ownerPartyId`



### Required Attributes

| Key                                             | JSON Type | Value Type       | Value Description                                                                                                    |
|-------------------------------------------------|-----------|------------------|----------------------------------------------------------------------------------------------------------------------|
| symbol                                          | String    | InstrumentSymbol | Unique symbol used to identify this instrument. Max 100 characters.                                                  |
| name                                            | String    | String           | Human readable name of the instrument.                                                                               |
| assetClass                                      | String    | AssetClass       | Values: EQUITY                                                                                                       |
| ownerPartyId                                    | String    | String           | The partyId of the legal entity that owns the instrument as previously registered.                                                                                                       |
| paymentInstructions                             | Object    | Object           | Contains the attributes listed below.                                                                                |
| paymentInstructions. primaryMarketBankAccountId | String    | BankAccountId    | The BankAccountId to send primary market sale funds to.                                                              |


### Optional Attributes

| Key                                            | JSON Type | Value Type       | Value Description                                                                                                    |
|------------------------------------------------|-----------|------------------|----------------------------------------------------------------------------------------------------------------------|
| paymentInstructions. feeTypes[]                | Array     | List             | A list of all additional fees associated with this instrument. Can be empty.                                         |
| feeTypes[].symbol                              | String    | FeeSymbol        | A symbolic representation of the type of fee, i.e. STAMP_DUTY. Required if a feeType is specified.                   |
| feeTypes[].bankAccountId                       | String    | BankAccountId    | The BankAccountId associated with the FeeSymbol.  Required if a feeType is specified.                                |

## `POST /instruments`

```http
POST /instruments HTTP/1.1
Host: api-sandbox.goji.investments
Content-Type: application/json
Authorization: Basic ...
X-GOJI-CLIENT-ID: 79f33f3c-86e0-4613-ba49-9fac3c6f0eab

{
  "symbol": "SPV1",
  "name": "Happy Parade - SPV1",
  "assetClass": "EQUITY",
  "ownerPartyId": "COM~d28360c5-07a3-4d78-ade4-bddcdd8b5502"
  "paymentInstructions": {
    "primaryMarketBankAccountId": "01700ae3-1b61-41c8-87d9-c59e82f33a41",
    "feeTypes": [
      {
        "symbol": "COMMISSION",
        "bankAccountId": "01700ae3-1b61-41c8-87d9-c59e82f33a41"
      }
    ]
  }
}
```

```http
HTTP/1.1 201 Created
Content-Type: application/json
X-GOJI-REQUEST-ID: 79f33f3c-86e0-4613-ba49-9fac3c6f0eab

{
  "symbol": "SPV1",
  "name": "Happy Parade - SPV1",
  "assetClass": "EQUITY",
  "ownerPartyId": "COM~d28360c5-07a3-4d78-ade4-bddcdd8b5502",
  "paymentInstructions": {
    "primaryMarketBankAccountId": "01700ae3-1b61-41c8-87d9-c59e82f33a41",
    "feeTypes": [
      {
        "symbol": "COMMISSION",
        "bankAccountId": "01700ae3-1b61-41c8-87d9-c59e82f33a41"
      }
    ]
  }
}
```

### Description

Registers a new instrument which can then be referenced by its symbol in other APIs which require an `InstrumentSymbol`.


A registered instrument can be referenced by the `InstrumentSymbol` in the following APIs:

 * [Trade Settlement API](/#settlement-equity-trade-settlement)
 * [Corporate Actions API](/#settlement-equity-corporate-actions)
 
In addition to defining the instrument, a list of fees that will apply to any trade settlement for this instrument 
must be specified.  The `FeeSymbol`s that have been registered will later be specified in the
[Trade Settlement API](/#settlement-equity-trade-settlement) to charge fees to the investors at settlement time.

### Request

Body: [Instrument Model](/#settlement-equity-instrument-model)

### Response

Body: [Instrument Model](/#settlement-equity-instrument-model)

Http Status:

| Code             | Description                                      | Body       | Content-Type     |
|------------------|--------------------------------------------------|------------|------------------|
| 201 Created      | Instrument created successfully                  | Instrument | application/json |
| 400 Bad Request  | The request was malformed.  See response body    | None       | n/a              |
| 401 Unauthorized | No auth provided, auth failed, or not authorized | None       | n/a              |

## `GET /instruments/{symbol}`

### Description

```http
GET /instruments/SPV99 HTTP/1.1
Host: api-sandbox.goji.investments
Content-Type: application/json
Authorization: Basic ...
X-GOJI-CLIENT-ID: 79f33f3c-86e0-4613-ba49-9fac3c6f0eab
```

```http
HTTP/1.1 200 OK
Content-Type: application/json
X-GOJI-REQUEST-ID: 79f33f3c-86e0-4613-ba49-9fac3c6f0eab

{
  "symbol": "SPV99",
  "name": "Happy Parade - SPV99",
  "assetClass": "EQUITY",
  "ownerPartyId": "COM~d28360c5-07a3-4d78-ade4-bddcdd8b5502",
  "paymentInstructions": {
    "primaryMarketBankAccountId": "01700ae3-1b61-41c8-87d9-c59e82f33a41",
    "feeTypes": [
      {
        "symbol": "COMMISSION",
        "bankAccountId": "01700ae3-1b61-41c8-87d9-c59e82f33a41"
      }
    ]
  }
}
```

Retrieves the details held on an instrument given its `symbol` is supplied.

### Request

Body: None

### Response

Body: [Instrument Model](/#settlement-equity-instrument-model)

Http Status:

| Code             | Description                                      | Body       | Content-Type     |
|------------------|--------------------------------------------------|------------|------------------|
| 200 OK           | Instrument exists and is returned in the body    | Instrument | application/json |
| 400 Bad Request  | The request was malformed.  See response body    | None       | n/a              |
| 401 Unauthorized | No auth provided, auth failed, or not authorized | None       | n/a              |
| 404 Not Found    | No instrument with this symbol is registered     | None       | n/a              |



## Issuing / Allocating Shares

After an instrument has been created, a number of shares must be issued to the nominee company.  This can be achieved
using [`POST /allocations`](/#settlement-equity-post-allocations).

Alternatively, if this instrument is created during migration of asset custody to Goji Nominee Ltd, and shares have 
already been sold to investors, the [`POST /allocations`](/#settlement-equity-post-allocations) can be used to allocate 
each investor's shares.

## Allocation Model

### Required Attributes

All of the following attributes of the Allocation Model are considered 'required' attributes. 

⚠️ In the case of the `investor` and `nominee` attributes specified below however, it may be more apt to call them 'conditional' attributes. 

If an allocation request is specified as being to an `investor`, it cannot simultaneously be to the `nominee` company. 

Please specify either one or the other when sending an allocation instruction.

⚠️ When allocating initial shares to the `nominee` the allocation will be placed in a `PENDING` state until the necessary paperwork has been
completed by our operations team to fulfil the CASS 6 regulations. Once approved the allocation will be moved into
an `ALLOCATED` status and a webhook and email will be triggered notifying the share allocation is now eligible for trading.

For integration purposes, endpoints have been made available in the test environments to manually move the `nominee allocation` into one of the
terminal statuses. 

These endpoints are:
 
 (1) [`PUT /allocations/test/{id}/approve`](#settlement-equity-put-allocations-test-id-approve) 
 (2) [`PUT /allocations/test/{id}/reject`](#settlement-equity-put-allocations-test-id-reject)

The webhook in question is further described [`here`](#webhooks-nominee_allocation_status_update). 


| Key                  | JSON Type | Value Type       | Value Description                                                                                                 |
|----------------------|-----------|------------------|-------------------------------------------------------------------------------------------------------------------|
| id                   | String    | AllocationId     | The platform generated unique ID of the allocation. Up to 100 characters.                                         |
| quantity             | Number    | Number           | The (whole) number of shares to allocate.                                                                         |
| instrumentSymbol     | String    | InstrumentSymbol | Platform generated unique ID of the instrument.                                                                   |
| investor             | Object    | Object           | Set to null for non-investor allocations, else populated with the fields below.                                   |
| investor.clientId    | String    | ClientId         | Either the client ID for the investor. Required if `investor` specified on the allocation.                        |
| investor.accountType | String    | Enum             | The investor's account to allocate to. Values: `GIA`, `ISA`. Required if `investor` specified on the allocation.  |
| nominee              | Object    | Object           | Set to null when not allocating to a nominee, else fields below.                                                  |
| nominee.accountType  | String    | Enum             | The nominee involved in the alloc. Value: `ORIGINATOR`. Required if `nominee` specified on the allocation.        |
| status               | String    | Enum             | `PENDING`, `CANCELLED`, `ALLOCATED`                                                                               |

## `POST /allocations`

```http
POST /allocations HTTP/1.1
Host: api-sandbox.goji.investments
Content-Type: application/json
Authorization: Basic ...
X-GOJI-CLIENT-ID: 59425d3d-cf73-44ff-aecb-590cd198a4bc

{
  "id": "5dd40510-810e-4a55-a395-04819fd915b9",
  "quantity": 1000000,
  "instrumentSymbol": "446ca5fc-4d38-4706-a50b-5b3a64d3f703",
  "investor": null,
  "nominee": {
    "accountType": "ORIGINATOR"
  }
}
```

```http
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": "5dd40510-810e-4a55-a395-04819fd915b9",
  "quantity": 1000000,
  "instrumentSymbol": "446ca5fc-4d38-4706-a50b-5b3a64d3f703",
  "investor": null,
  "nominee": {
    "accountType": "ORIGINATOR"
  },
  "status": "PENDING"
}
```

### Description

Creates a new allocation of shares.

When initially issuing shares, the `nominee` must be specified.
When migrating shares, the `investor` must be specified.

⚠️ When allocating initial shares to the `nominee` the allocation will be placed in a `PENDING` state until the necessary paperwork has been
completed by our operations team to fulfil the CASS 6 regulations. Once approved the allocation will be moved into
an `ALLOCATED` status and a webhook and email will be triggered notifying the share allocation is now eligible for trading.

For integration purposes, endpoints have been made available in the test environments to manually move the `nominee allocation` into one of the
terminal statuses. These endpoints are [`PUT /allocations/test/{id}/approve`](#settlement-equity-put-allocations-test-id-approve) and [`PUT /allocations/test/{id}/reject`](#settlement-equity-put-allocations-test-id-reject).

The webhook in question is further described [`here`](#webhooks-nominee_allocation_status_update). 


### Request

Body: [Allocation Model](/#settlement-equity-allocation-model)

### Response

Body: [Allocation Model](/#settlement-equity-allocation-model)

Http Status: 

| HTTP Status Code | Description                                      | Body       | Content-Type     |
|------------------|--------------------------------------------------|------------|------------------|
| 201 Created      | Allocation created successfully                  | Allocation | application/json |
| 400 Bad Request  | The request was malformed.  See response body    | None       | n/a              |
| 401 Unauthorized | No auth provided, auth failed, or not authorized | None       | n/a              |

## `GET /allocations/{id}`

```http
GET /allocations/5dd40510-810e-4a55-a395-04819fd915b9 HTTP/1.1
Host: api-sandbox.goji.investments
Content-Type: application/json
Authorization: Basic ...
X-GOJI-CLIENT-ID: 59425d3d-cf73-44ff-aecb-590cd198a4bc
```

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "5dd40510-810e-4a55-a395-04819fd915b9",
  "quantity": 1000000,
  "instrumentSymbol": "446ca5fc-4d38-4706-a50b-5b3a64d3f703",
  "investor": null,
  "nominee": {
    "accountType": "ORIGINATOR"
  },
  "status": "ALLOCATED"
}
```

### Description

Retrieves the details of an existing allocation given an allocation `id` is supplied.

### Request

Body: [Allocation Model](/#settlement-equity-allocation-model)

### Response

Body: [Allocation Model](/#settlement-equity-allocation-model)

Http Status: 

| Code             | Description                                      | Body       | Content-Type     |
|------------------|--------------------------------------------------|------------|------------------|
| 200 OK           | Allocation exists and is return in the body      | Allocation | application/json |
| 400 Bad Request  | The request was malformed.  See response body    | None       | n/a              |
| 401 Unauthorized | No auth provided, auth failed, or not authorized | None       | n/a              |
| 404 Not Found    | No allocation with this UUID exists              | None       | n/a              |


## `🧪 PUT /allocations/test/{id}/approve`

```http
PUT /allocations/test/5dd40510-810e-4a55-a395-04819fd915b9/approve HTTP/1.1
Host: api-sandbox.goji.investments
Content-Type: application/json
Authorization: Basic ...
X-GOJI-CLIENT-ID: 59425d3d-cf73-44ff-aecb-590cd198a4bc
```

```http
HTTP/1.1 202 ACCEPTED
Content-Type: application/json

```

### Description

⚠️ This endpoint is only available in the test and sandbox environments for integration purposes. 

It will approve and action an existing allocation towards a nominee.

Off the back of a call to this endpoint, the [`NOMINEE_ALLOCATION_STATUS_UPDATE`](#webhooks-nominee_allocation_status_update) webhook is triggered. 
Post receipt of that webhook, the allocation instruction will have been executed and shares will have been allocated to the nominee.

### Request

No body. The `id` specified as a path parameter refers to the one described here: [Allocation Model](/#settlement-equity-allocation-model)

### Response

No body.

Http Status: 

| Code             | Description                                      | Body       | Content-Type     |
|------------------|--------------------------------------------------|------------|------------------|
| 202 Accepted     | Allocation exists and has been updated.          | None       | n/a              |
| 400 Bad Request  | The request was malformed.  See response body    | None       | n/a              |
| 401 Unauthorized | No auth provided, auth failed, or not authorized | None       | n/a              |
| 404 Not Found    | No allocation with this UUID exists              | None       | n/a              |




## `🧪 PUT /allocations/test/{id}/reject`

```http
PUT /allocations/test/5dd40510-810e-4a55-a395-04819fd915b9/reject HTTP/1.1
Host: api-sandbox.goji.investments
Content-Type: application/json
Authorization: Basic ...
X-GOJI-CLIENT-ID: 59425d3d-cf73-44ff-aecb-590cd198a4bc
```

```http
HTTP/1.1 202 ACCEPTED
Content-Type: application/json

```

### Description

⚠️ This endpoint is only available in the test and sandbox environments for integration purposes. 

It will cancel a pending allocation instruction towards a nominee.

Off the back of a call to this endpoint, the [`NOMINEE_ALLOCATION_STATUS_UPDATE`](#webhooks-nominee_allocation_status_update) webhook is triggered. 

### Request

No body. The `id` specified as a path parameter refers to the one described here: [Allocation Model](/#settlement-equity-allocation-model)

### Response

No body.

Http Status: 

| Code             | Description                                      | Body       | Content-Type     |
|------------------|--------------------------------------------------|------------|------------------|
| 202 Accepted     | Allocation exists and has been updated.          | None       | n/a              |
| 400 Bad Request  | The request was malformed.  See response body    | None       | n/a              |
| 401 Unauthorized | No auth provided, auth failed, or not authorized | None       | n/a              |
| 404 Not Found    | No allocation with this UUID exists              | None       | n/a              |
