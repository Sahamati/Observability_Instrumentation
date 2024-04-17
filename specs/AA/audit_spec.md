## AUDIT Events Specification

Following are the proposed AUDIT events to be generated at the AA end. 

| Event For  | Description  |  Required? |   Share with Sahamati? |
|:----------|:-------------|:------|:------|
|Consent Artefact |Generate audit logs for all consent lifecycle|Yes|Yes|
|API Transaction Logs |Generate identity-less (non-PII) API transaction logs for all API calls|No|No|

### Consent Artefact

Generate an Audit event whenever a consent artifact (via the /Consent API using the AA client) is generated using the below structure:
```js
{
  "logRecords": [{
    "timeUnixNano": String, // Required. Map it to the API response callback timestamp
    "traceId": String, // Map it to the Consent API traceId
    "spanId": String, // Map it to the Consent API spanId
    "body": {
        "stringValue": "ConsentArtefact",
    }
    "attributes": [
      {"key":"consentStart","value": {"stringValue": "2019-12-06T11:39:57.153Z"}},
      {"key":"consentExpiry","value": {"stringValue": "2019-12-06T11:39:57.153Z"}},
      {"key":"consentMode","value": {"stringValue": "VIEW"}},
      {"key":"fetchType","value": {"stringValue": "ONETIME"}},
      {"key":"consentTypes","value": {"arrayValue": {"values":[{"stringValue": "PROFILE"}]}}},
      {"key":"fiTypes","value": {"arrayValue": {"values":[{"stringValue": "DEPOSIT"}]}}},
      {
        "key":"DataConsumer",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "id", "value": {"stringValue": "DC1"}},
              {"key": "type", "value": {"stringValue": "FIU"}}
            ]
          }
        }
      },
      {
        "key":"Customer",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "id", "value": {"stringValue": "<anonymized identifier>"}}
            ]
          }
        }
      },
      {
        "key":"Purpose",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "code", "value": {"stringValue": "101"}},
              {"key": "text", "value": {"stringValue": "Wealth management service"}}
            ]
          }
        }
      },
      {
        "key":"FIDataRange",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "from", "value": {"stringValue": "2023-07-06T11:39:57.153Z"}},
              {"key": "to", "value": {"stringValue": "2019-12-06T11:39:57.153Z"}}
            ]
          }
        }
      },
      {
        "key":"DataLife",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "unit", "value": {"stringValue": "MONTH"}},
              {"key": "value", "value": {"intValue": 0}}
            ]
          }
        }
      },
      {
        "key":"Frequency",
        "value": {
          "kvlistValue": {
            "values": [
              {"key": "unit", "value": {"stringValue": "HOUR"}},
              {"key": "value", "value": {"intValue": 1}}
            ]
          }
        }
      },
      {
        "key":"DataFilter",
        "value": {
          "arrayvalue": {
            "values": [
              {
                "kvlistValue": {
                  "values": [
                    {"key": "type", "value": {"stringValue": "TRANSACTIONAMOUNT"}},
                    {"key": "operator", "value": {"stringValue": ">="}},
                    {"key": "value", "value": {"stringValue": "20000"}}
                  ]
                }
              }
            ]
          }
        }
      }
    ] 
  }]
}
```
> **Note**: Do not send the customer PII information. Generate a unique anonymized hash of the customer id and add it as part of the event.

### API Transaction Logs

Generate the API transaction logs for all APIs using the below structure:
```js
{
  "logRecords": [{
    "timeUnixNano": String, // Required. Map it to the API response callback timestamp
    "traceId": String, // Map it to the API event traceId
    "spanId": String, // Map it to the API event spanId
    "body": {
        "stringValue": "APITransactionLog",
    }
    "attributes": [
      {"key":"request","value": {"stringValue": "<stringified-json>"}},
      {"key":"response","value": {"stringValue": "<stringified-json>"}}
    ] 
  }]
}
```
While every AA would have implemented their own mechanism to store the transaction logs, as a network facilitator sahamati is proposing to standardize the structure of the events aligned with the OpenNetwork Telemetry specification so that they can enable:

1. **Interoperability**: When systems adhere to a common structure, they can seamlessly exchange data and communicate with each other. This interoperability eliminates compatibility issues and allows for smoother integration between different systems, regardless of their underlying technologies or implementations.
2. **Consistency**: Standardization ensures consistency in data format, semantics, and behavior across multiple systems. This consistency simplifies development, maintenance, and troubleshooting processes, as developers can rely on predictable patterns and behaviors when working with standardized systems.
3. **Scalability**: Standardized structures facilitate scalability by providing a common framework for adding new systems, components, or functionalities. As organizations grow and evolve, standardized systems can easily accommodate changes and expansions without requiring significant modifications to existing infrastructure or workflows.
4. **Ecosystem Support**: Standardized structures often enjoy broader ecosystem support, including documentation, libraries, tools, and community resources. This ecosystem support fosters innovation, collaboration, and knowledge sharing among developers, further enhancing the value of standardized systems.
5. **Compliance and Regulation**: Standardization can help organizations comply with industry regulations, standards, and best practices. By adopting recognized standards and structures, organizations can demonstrate regulatory compliance, mitigate risks, and enhance trust and credibility with stakeholders.

In addition the intent of these audit events is to enable a standardized way to:

1. **Troubleshooting and Root Cause Analysis**: When issues do occur, transaction logs are invaluable for troubleshooting and root cause analysis. It provides detailed information about system behavior leading up to an incident, helping pinpoint the underlying cause and implement effective solutions.
2. **Compliance and Auditing**: In regulated industries, transaction logs may be required to demonstrate compliance with various standards and regulations. By maintaining comprehensive transaction logs records, organizations can ensure transparency and accountability in their operations.