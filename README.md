
# Carat BuyBack Public APIs

This document provides detailed information on the Carat BuyBack Public APIs. These APIs allow sellers to get price quotes and sell references for carats. All requests must include an API key provided to each seller.

## Authorization

All requests to the API must include a header named `x-api-key`. This API key is unique for each seller.

## Base URL

`https://dev-utility-api.diamondstandard.co`

## Endpoints

### 1. Get Price Quote

Retrieve the current price per carat in dollars.

**Endpoint**: `/v1/carat-buyback/price-quote`

**Method**: `GET`

**Query Parameters**:
- `accountId` (type: `string`, required: `true`)

**Response**:
```json
{
    "price": "0.86" // Price per Carat in Dollars
}
```

### 2. Get Sell Reference

Generate a sell reference for a specified quantity of carats at a given price.

**Endpoint**: `/v1/carat-buyback/sell-reference`

**Method**: `GET`

**Query Parameters**:
- `accountId` (type: `string`, required: `true`)
- `quantity` (type: `integer`, required: `true`)
- `price` (type: `integer`, required: `true`)

**Note**: 
- The quantity of carats should be multiplied by decimals of carat (i.e., 100).
- The price should be passed in cents.

**Response**:
```json
{
    "sellReference": "c29e0f59466b4654db8faab1778f44bca531129a5a0acc70c9e7440089895bdf",
    "spenderAccountId": "0.0.2158401"
}
```

### 3. Initiate Sell Request

Initiate a sell request for a specified quantity of carats at a given price.

**Endpoint**: `/v1/carat-buyback/initiate-sell-request`

**Method**: `POST`

**Body**:
```json
{
    "accountId": "0.0.875673",
    "price": 86, // Cents
    "quantity": 3000, // Multiplied with decimals (i.e., 100)
    "sellReference": "c29e0f59466b4654db8faab1778f44bca531129a5a0acc70c9e7440089895bdf",
    "transactionId": "0.0.97713@1718263065.501357948", // Allowance transactionId
    "signature": [10,169,152,152,39,239]
}
```
**Notes** 
- The quantity parameter should be the number of carats multiplied by 100. (e.g. 100.25 should be passed as 10025)
- The price parameter should be in cents (e.g., $0.86 should be passed as 86).
- The sell request must be initiated within 15 minutes of generating the sell reference.
- The allowance transaction memo must include the sell reference.
- Signature payload must be of below format
   - Format -  "accountId,price,quantity,transactionId"
   - Example - "0.0.875673,86,3000,0.0.97713@1718263065.501357948"
- Signature must be Array of unsigned integers.


**Response** -
```json
{
    "success": true,
    "requestId": 10
}
```



### 4. Sell Request Status

Check the status of a sell request.

**Endpoint**: `/v1/carat-buyback/sell-request-status`

**Method**: `GET`

**Query Parameters**:
- `requestId` (type: `integer`, required: `true`)

Response - 
```json
{
  "status": "REQUEST_PROCESSED" 
}
```

**Possible status** - QUOTE, REQUEST_INITIATED, INVALID_REQUEST, CARAT_TRANSFER_INITIATED , CARAT_TRANSFER_COMPLETED, CARAT_TRANSFER_FAILED, REQUEST_PROCESSED, REQUEST_REJECTED, REQUEST_CANCELLED



### 5. Sell Request History

Check the hostory of sell requests.

**Endpoint**: `/v1/carat-buyback/sell-request-history`

**Method**: `GET`

**Query Parameters**:
- `accountId` (type: `string`, required: `true`)
- `startDate` (type: `string`, required: `false`, format: `ISO`)
- `endDate` (type: `string`, required: `false`,  format: `ISO`)

Response - 

```json
[
  {
    "id": 14,
    "sellerId": 1,
    "status": "QUOTE",
    "sellReference": "dcc34e8c91051997cb32b26cca64a68b96ed4d80ea23e7a0cb334949d03a94ea",
    "quotedPrice": "0.88",
    "quantity": "20.00",
    "expiryTime": "2024-06-11T10:20:01.402Z",
    "signingPublicKey": "302a300506032b6570032100ccd39e5d6f9272bd7519d8aabb5f3b6ede130d1f49f7d385362dfc2820898a2b",
    "allowanceTxnId": "0.0.97713@1718263065.501357948",
    "transferTxnId": null,
    "sellerEmail": "johndoe@gmail.com",
    "callbackUrl": "https://www.example.com/callback",
    "requestedAt": "2024-06-11T10:05:01.402Z",
    "transferredAt": null,
    "processedAt": null,
    "sellerName": "John Doe",
    "email": "johndoe@gmail.com",
    "hederaAccountId": "0.0.3974130",
    "bankName": "First National Bank.",
    "accountName": "John Doe",
    "accountNumber": "1234567890",
    "routingNumber": "987654321"
 }
]
```



