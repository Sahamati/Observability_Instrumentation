## API Events Specification

FIU's have to raise the API events for all AA APIs including callbacks with the structure describe in the [API Event Type](/specs/telemetry.md#api) section. Few of the APIs can have additional data as described in the below table and those API sections are expaned in the next few sections.

### Consent Request

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

### Consent Fetch

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

### Consent Notification

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

### FI Request

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

### FI Notification

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

### FI Fetch

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