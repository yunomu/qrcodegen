# qrcodegen

# Structure

## Diagram
```mermaid
graph LR
subgraph QRcodeGen
  QRcode[(QRcode)]
  API[[API]]
  APIFunction
  GenerateFunction
  Bucket[(Bucket)]
  CDN[[CDN]]
  UserPoolClient
end

User((User))
Internet((Internet))
IDP

User --> API
API --> APIFunction
API -.-> UserPoolClient
UserPoolClient --> IDP
APIFunction --read/write--> QRcode
QRcode --stream--> GenerateFunction
GenerateFunction --update--> Bucket
GenerateFunction --invalidate--> CDN
Bucket --> CDN
CDN --> Internet
```

## Table: QRcodeTable
### Schema
|AttrName |AttrType|Key  |
|---------|--------|-----|
|Key      |S       |Hash |
|UserId   |S       |     |
|State    |S       |     |
|Payload  |S       |     |
|Timestamp|N       |     |

### Attribute
* **Key**: Randomized string. S3 object key. Not reused.
* **UserId**: User ID generated by IDP
* **State**: AVAILABLE|DELETED
* **Payload**: Contents

### Index: UserIndex
|AttrName |Key  |
|---------|-----|
|UserId   |Hash |
|Timestamp|Range|
