## API Events Specification

FIP's have to raise the API events for all FIP APIs originating from AA including callbacks with the structure describe in the [API Event Type](/specs/telemetry.md#api) section. Few of the APIs can have additional data as described in the below table and those API sections are expaned in the next few sections.

### Account Discovery

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Account/discover`| `AA` | `FIP` |

#### Additional Attributes

```js
None
```

#### Key Attributes
```js
{
  "spanId": "request.txnid",
  "traceId": "hash(request.Customer.id)",
  "sender.id": "<AA>",
  "recipient.id": "<FIP>"
}
```

### Account Linking

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Account/link`| `AA` | `FIP` |

#### Additional Attributes

```js
{
  "events":[{
    "name": "AdditionalAttributes",
    "time": "2023-06-26T17:51:18.412Z",
    "attributes": [
      {"key":"accounts_req_count","value": {"intValue": 1}} // Number of accounts requested. Count of resp.Customer.Accounts array
    ]
  }]
}
```

#### Key Attributes
```js
{
  "spanId": "request.txnid",
  "traceId": "hash(request.Customer.id)",
  "sender.id": "<AA>",
  "recipient.id": "<FIP>"
}
```

### Verify Account Linking

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Account/link/verify`| `AA` | `FIP` |

#### Additional Attributes
```js
None
```

#### Key Attributes
```json
{
    "spanId": "request.txnid",
    "traceId": "hash(resp.AccLinkDetails.customerAddress)",
    "sender.id": "<AA>",
    "recipient.id": "<FIP>"
}
```

### Account Delink

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Account/delink`| `AA` | `FIP` |

#### Additional Attributes
```js
None
```
#### Key Attributes
```json
{
    "spanId": "request.txnid",
    "traceId": "hash(Account.customerAddress)",
    "sender.id": "<AA>",
    "recipient.id": "<FIP>"
}
```

### Consent

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Consent`| `AA` | `FIP` |

#### Additional Attributes
```js
None
```
#### Key Attributes
```json
{
    "spanId": "req.txnid",
    "traceId": "req.consentId",
    "sender.id": "<AA>",
    "recipient.id": "<FIP>"
}
```

### Consent Notification

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Consent/Notification`| `AA` | `FIP` |

#### Additional Attributes
```js
{
  "events":[{
    "name": "AdditionalAttributes",
    "time": "2023-06-26T17:51:18.412Z",
    "attributes": [
      {"key":"consentId","value": {"stringValue": "req.ConsentStatusNotification.consentId"}},
      {"key":"consentStatus","value": {"stringValue": "req.ConsentStatusNotification.consentStatus"}}
    ]
  }]
}
```
#### Key Attributes
```json
{
    "spanId": "req.txnid",
    "traceId": "req.ConsentStatusNotification.consentId",
    "sender.id": "<AA>",
    "recipient.id": "<FIP>"
}
```

### FI Request

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/FI/request`| `AA` | `FIP` |

#### Additional Attributes
```js
None
```
#### Key Attributes
```json
{
    "spanId": "req.txnid",
    "traceId": "res.sessionId",
    "sender.id": "<AA>",
    "recipient.id": "<FIP>"
}
```

### FI Notification

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/FI/Notification`| `FIP` | `AA` |

#### Additional Attributes
```js
{
  "events":[{
    "name": "AdditionalAttributes",
    "time": "2023-06-26T17:51:18.412Z",
    "attributes": [
      {"key":"sessionId","value": {"stringValue": "req.FIStatusNotification.sessionId"}},
      {
        "key":"Accounts",
        "value": {
          "arrayvalue": {"values":[{
            "kvlistValue": {
              "values": [
                {"key": "linkRefNumber", "value": {"stringValue": "req.FIStatusNotification.Accounts[*].linkRefNumber"}},
                {"key": "FIStatus", "value": {"stringValue": "req.FIStatusNotification.Accounts[*].FIStatus"}}
              ]
            }
          }]}
        }
      }
    ]
  }]
}
```
#### Key Attributes
```json
{
    "spanId": "req.txnid",
    "traceId": "req.FIStatusNotification.sessionId",
    "sender.id": "<FIP>",
    "recipient.id": "<AA>"
}
```

### FI Fetch

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/FI/fetch`| `AA` | `FIP` |

#### Additional Attributes
```js
{
  "events":[{
    "name": "AdditionalAttributes",
    "time": "2023-06-26T17:51:18.412Z",
    "attributes": [
      {"key":"countOfLRNs","value": {"stringValue": "count(req.linkRefNumber"}}
    ]
  }]
}
```
#### Key Attributes
```json
{
    "spanId": "req.txnid",
    "traceId": "req.sessionId",
    "sender.id": "<AA>",
    "recipient.id": "<FIP>"
}
```