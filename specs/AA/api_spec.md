## API Events Specification

AA's have to raise the API events for all AA APIs to FIP/FIUs including callbacks with the structure describe in the [API Event Type](/specs/telemetry.md#api) section. Few of the APIs can have additional data as described in the below table and those API sections are expaned in the next few sections.

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

### Consent Request by AA

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Consent`| `FIU` | `AA` |

#### Additional Attributes
```js
{
  "events":[{
    "name": "AdditionalAttributes",
    "time": "2023-06-26T17:51:18.412Z",
    "attributes": [
      {
        "key":"Purpose",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "code", "value": {"stringValue": "req.ConsentDetail.Purpose.code"}},
              {"key": "text", "value": {"stringValue": "req.ConsentDetail.Purpose.text"}}
            ]
          }
        }
      },
      {
        "key":"FIDataRange",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "from", "value": {"stringValue": "req.ConsentDetail.FIDataRange.from"}},
              {"key": "to", "value": {"stringValue": "req.ConsentDetail.FIDataRange.to"}}
            ]
          }
        }
      },
      {
        "key":"Frequency",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "unit", "value": {"stringValue": "req.ConsentDetail.Frequency.from"}},
              {"key": "value", "value": {"intValue": "req.ConsentDetail.Frequency.value"}}
            ]
          }
        }
      },
      {
        "key":"ConsentHandle", "value": {"stringValue": "res.ConsentHandle"}
      }
    ]
  }]
}
```
#### Key Attributes
```json
{
    "spanId": "req.txnid",
    "traceId": "hash(req.ConsentDetail.Customer.id)",
    "sender.id": "<FIU>",
    "recipient.id": "<AA>"
}
```

### Consent Handle

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Consent/handle`| `FIU` | `AA` |

#### Additional Attributes
```js
{
  "events":[{
    "name": "AdditionalAttributes",
    "time": "2023-06-26T17:51:18.412Z",
    "attributes": [
      {"key":"consentId","value": {"stringValue": "res.ConsentStatus.id"}},
      {"key":"consentStatus","value": {"stringValue": "res.ConsentStatus.status"}}
    ]
  }]
}
```
#### Key Attributes
```json
{
    "spanId": "req.txnid",
    "traceId": "req.ConsentHandle",
    "sender.id": "<FIU>",
    "recipient.id": "<AA>"
}
```

### Consent Request to FIP

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

### Consent Fetch By AA

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Consent/fetch`| `FIU` | `AA` |

#### Additional Attributes
```js
None
```
#### Key Attributes
```json
{
    "spanId": "req.txnid",
    "traceId": "req.consentId",
    "sender.id": "<FIU>",
    "recipient.id": "<AA>"
}
```

### Consent Notification to FIU

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/Consent/Notification`| `AA` | `FIU` |

#### Additional Attributes
```js
{
  "events":[{
    "name": "AdditionalAttributes",
    "time": "2023-06-26T17:51:18.412Z",
    "attributes": [
      {"key":"consentId","value": {"stringValue": "req.ConsentStatusNotification.consentId"}},
      {"key":"consentHandle","value": {"stringValue": "req.ConsentStatusNotification.consentHandle"}},
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
    "recipient.id": "<FIU>"
}
```

### Consent Notification to FIP

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

### FI Request by FIU

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/FI/request`| `FIU` | `AA` |

#### Additional Attributes
```js
{
  "events":[{
    "name": "AdditionalAttributes",
    "time": "2023-06-26T17:51:18.412Z",
    "attributes": [
      {"key":"consentId","value": {"stringValue": "req.Consent.id"}},
      {
        "key":"FIDataRange",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "from", "value": {"stringValue": "req.FIDataRange.from"}},
              {"key": "to", "value": {"stringValue": "req.FIDataRange.to"}}
            ]
          }
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
    "traceId": "res.sessionId",
    "sender.id": "<FIU>",
    "recipient.id": "<AA>"
}
```

### FI Request to FIP

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

### FI Notification to AA

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

### FI Notification to FIU

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/FI/Notification`| `AA` | `FIU` |

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
    "sender.id": "<AA>",
    "recipient.id": "<FIU>"
}
```

### FI Fetch by AA

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

### FI Fetch by FIU

| API | Txn Originator | Txn Receiver |
|:----------|:-------------|:------|
|`/FI/fetch`| `FIU` | `AA` |

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
    "sender.id": "<FIU>",
    "recipient.id": "<AA>"
}
```