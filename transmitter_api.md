# This is documentation about Smartshield Transmitter API

## Smartshield Listener is a set list of API developed by Chainsmart

## Client need to call these APIs to submit data change and verification check to the blockchain

## I. Authentication

### Authentication is needed to call the API, SmartShield is use OpenID for authentication

## The following url can be used to authenticate, replace {org} with your organization

- Development: <https://sso.dev.chainsmart.id/realms/{org}/.well-known/openid-configuration>
- Production: <https://sso.chainsmart.id/realms/{org}/.well-known/openid-configuration>

- [x] Authentication with Password using OpenID

```YAML
openapi: 3.1.0
info:
  title: Swagger SSO SmartShield - OpenAPI 3.1
  description: |-
    This is a SSO Authentication API based on the OpenAPI 3.1 specification.
  contact:
    email: riyanto@chainsmart.id
  version: 1.0.0
tags:
  - name: Auth
    description: Access to ChainSmart SSO Authentication
paths:
  /realms/{org}/protocol/openid-connect/token#1:
    post:
      tags:
        - Auth
      summary: Authenticate to ChainSmart
      description: Authenticate using OpenID
      operationId: SignIn
      parameters:
        - name: org
          in: path
          description: organization
          required: true
          schema:
            type: string
      requestBody:
        content:
          application/x-www-form-urlencoded:
            schema:
              type: object
              properties:
                grant_type:
                  type: string
                  examples: ['password']
                client_id:
                  type: string
                client_secret:
                  type: string
                username:
                  type: string
                password:
                  type: string
                scope:
                  type: string
                  description: space delimited list of scope requests
                  examples: ['openid email']
              required:
                - grant_type
                - client_id
                - client_secret
                - username
                - password
                - scope
      responses:
        '200':
          description: Successful operation
          content:
            application/json:
              schema:
                type: object
                properties:
                  token_type:
                    type: string
                  id_token:
                    type: string
                  refresh_token:
                    type: string
                  access_token:
                    type: string
                  refresh_expires_in:
                    type: integer
                  expires_in:
                    type: integer
                  not-before-policy:
                    type: integer
                  session_state:
                    type: string
                  scope:
                    type: string
                required:
                  - token_type
                  - id_token
                  - refresh_token
                  - access_token
                  - refresh_expires_in
                  - expires_in
                  - 'not-before-policy'
                  - session_state
                  - scope
  /realms/{org}/protocol/openid-connect/token#2:
    post:
      tags:
        - Auth
      summary: Refresh Authentication to ChainSmart
      description: Refresh Session using OpenID
      operationId: RefreshSession
      parameters:
        - name: org
          in: path
          description: organization
          required: true
          schema:
            type: string
      requestBody:
        content:
          application/x-www-form-urlencoded:
            schema:
              type: object
              properties:
                grant_type:
                  type: string
                  examples: ['refresh_token']
                client_id:
                  type: string
                client_secret:
                  type: string
                refresh_token:
                  type: string
              required:
                - grant_type
                - client_id
                - client_secret
                - refresh_token
      responses:
        '200':
          description: Successful operation
          content:
            application/json:
              schema:
                type: object
                properties:
                  token_type:
                    type: string
                  id_token:
                    type: string
                  refresh_token:
                    type: string
                  access_token:
                    type: string
                  refresh_expires_in:
                    type: integer
                  expires_in:
                    type: integer
                  not-before-policy:
                    type: integer
                  session_state:
                    type: string
                  scope:
                    type: string
                required:
                  - token_type
                  - id_token
                  - refresh_token
                  - access_token
                  - refresh_expires_in
                  - expires_in
                  - 'not-before-policy'
                  - session_state
                  - scope
  /realms/{org}/protocol/openid-connect/logout:
    post:
      tags:
        - Auth
      summary: Remove Authentication
      description: Remove Session using OpenID
      operationId: SignOut
      parameters:
        - name: org
          in: path
          description: organization
          required: true
          schema:
            type: string
      requestBody:
        content:
          application/x-www-form-urlencoded:
            schema:
              type: object
              properties:
                client_id:
                  type: string
                client_secret:
                  type: string
                refresh_token:
                  type: string
              required:
                - client_id
                - client_secret
                - refresh_token
      responses:
        '204':
          description: Successful operation No Content
```

## II. Rest API

- [x] Create/Update/Delete Record

Create: before is null, after has value\
Update: both before and after has value\
Delete: before has value, after is null

```YAML
openapi: 3.1.0
info:
  title: Swagger SmartShield - OpenAPI 3.1
  description: |-
    This is a SmartShield Transmitter API based on the OpenAPI 3.1 specification.
  contact:
    email: riyanto@chainsmart.id
  version: 1.0.0
tags:
  - name: Record
    description: Access to ChainSmart Transmitter API
paths:
  /api/record:
    post:
      tags:
        - Record
      summary: Create/Update/Delete Record
      description: |
        Create: before is null and after has value;
        Update: both before and after has value;
        Delete: before has value and after is null
      operationId: CreateUpdateDeleteRecord
      security:
        - bearerAuth: []
      requestBody:
        content:
          json:
            schema:
              type: object
              properties:
                recordId:
                  type: string
                owenerType:
                  type: string
                  description: record owner type like NIK, NIM, NIP, ID, etc
                ownerId:
                  type: string
                  description: record owner id content of owner type
                before:
                  type: object
                after:
                  type: object
                timestamp:
                  type: string
                  format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
                  description: if not provided, API will add it automatically
                  examples: ["2021-11-01T14:48:00.000Z"]
              required:
                - recordId
          multipart/form-data:
            schema:
              type: object
              properties:
                recordId:
                  type: string
                owenerType:
                  type: string
                  description: record owner type like NIK, NIM, NIP, ID, dll
                ownerId:
                  type: string
                  description: record owner id content of owner type
                before.file:
                  type: array
                  items:
                    type: string
                    format: binary
                before.metaData[]:
                  type: array
                  items:
                    type: object
                after.file:
                  type: array
                  items:
                    type: string
                    format: binary
                after.metaData[]:
                  type: array
                  items:
                    type: object
                timestamp:
                  type: string
                  format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
                  description: if not provided, API will add it automatically
                  examples: ["2021-11-01T14:48:00.000Z"]
              required:
                - recordId
      responses:
        '201':
          description: Successful operation
          content:
            application/json:
              schema:
                type: string
        '400':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
        '401':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
        '500':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
security:
  - bearerAuth: []
```

- [x] Record verification check

```YAML
openapi: 3.1.0
info:
  title: Swagger SmartShield - OpenAPI 3.1
  description: |-
    This is a SmartShield Transmitter API based on the OpenAPI 3.1 specification.
  contact:
    email: riyanto@chainsmart.id
  version: 1.0.0
tags:
  - name: Record
    description: Access to ChainSmart Transmitter API
paths:
  /record/validate:
    post:
      tags:
        - Record
      summary: Check a record integrity againsts blockchain
      operationId: ValidateRecord
      security:
        - bearerAuth: []
      requestBody:
        content:
          json:
            schema:
              type: object
              properties:
                recordId:
                  type: string
                before:
                  type: object
                timestamp:
                  type: string
                  format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
                  description: if not provided, API will add it automatically
                  examples: ["2021-11-01T14:48:00.000Z"]
              required:
                - recordId
          multipart/form-data:
            schema:
              type: object
              properties:
                recordId:
                  type: string
                before.file:
                  type: array
                  items:
                    type: string
                    format: binary
                before.metaData[]:
                  type: array
                  items:
                    type: object
                timestamp:
                  type: string
                  format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
                  description: if not provided, API will add it automatically
                  examples: ["2021-11-01T14:48:00.000Z"]
              required:
                - recordId
      responses:
        '201':
          description: Successful operation
          content:
            application/json:
              schema:
                type: string
        '400':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
        '401':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
        '500':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
security:
  - bearerAuth: []
```

- [x] Load Single Record

```YAML
openapi: 3.1.0
info:
  title: Swagger SmartShield - OpenAPI 3.1
  description: |-
    This is a SmartShield Transmitter API based on the OpenAPI 3.1 specification.
  contact:
    email: riyanto@chainsmart.id
  version: 1.0.0
tags:
  - name: Record
    description: Access to ChainSmart Transmitter API
paths:
  /record/{recordId}:
    get:
      tags:
        - Record
      summary: Load a record rom the blockchain
      description: Load a record with data from the blockchain
      operationId: GetRecord
      security:
        - bearerAuth: []
      parameters:
        - name: recordId
          in: path
          description: Record ID
          required: true
          schema:
            type: string
      responses:
        '200':
          description: Successful operation
          content:
            application/json:
              schema:
                type: object
                properties:
                  recordId:
                    type: string
                  ownerType:
                    type: string
                  ownerId:
                    type: string
                  hash:
                    type: string
                  height:
                    type: integer
                  epoch:
                    type: integer
                  valid:
                    type: boolean
                  data:
                    type: object
                  invalids:
                    type: array
                    items:
                      type: object
                      properties:
                        path:
                          type: string
                          description: location where verification check failed
                          examples: ['/a/b/c/d/e']
                        operation:
                          type: string
                          enum: [add, replace, remove]
                        oldValue:
                          type: object
                        newValue:
                          type: object
                  deltaList:
                    type: array
                    items:
                      type: object
                      properties:
                        path:
                          type: string
                          description: location where verification check failed
                          examples: ['/a/b/c/d/e']
                        operation:
                          type: string
                          enum: [add, replace, remove]
                        oldValue:
                          type: object
                        newValue:
                          type: object
                  txInfos:
                    type: array
                    items:
                      type: object
                      properties:
                        txId:
                          type: string
                        timestamp:
                          type: string
                          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
                          description: if not provided, API will add it automatically
                          examples: ["2021-11-01T14:48:00.000Z"]
                required:
                  - recordId
                  - valid
                  - record
                  - invalids
                  - deltaList
                  - timestamp
        '400':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
        '401':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
        '500':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
security:
  - bearerAuth: []
```

- [ ] Load Records

```YAML
openapi: 3.1.0
info:
  title: Swagger SmartShield - OpenAPI 3.1
  description: |-
    This is a SmartShield Transmitter API based on the OpenAPI 3.1 specification.
  contact:
    email: riyanto@chainsmart.id
  version: 1.0.0
tags:
  - name: Record
    description: Access to ChainSmart Transmitter API
paths:
  /record:
    get:
      tags:
        - Record
      summary: List Record with metadata and without data
      description: List Record
      responses:
        '200':
          description: Successful operation
          content:
            application/json:
              schema:
                type: object
                properties:
                  pageMeta:
                    type: object
                    properties:
                      currentPage:
                        type: integer
                      itemsPerPage:
                        type: integer
                      totalPages:
                        type: integer
                      totalItems:
                        type: integer
                  data:
                    type: array
                    items:
                      type: object
                      properties:
                        id:
                          type: string
                        hash:
                          type: string
                        meta:
                          type: object
                          properties:
                            hash:
                              type: string
                            height:
                              type: integer
                            epoch:
                              type: integer
                            invalids:
                              type: array
                              items:
                                type: object
                                properties:
                                  path:
                                    type: string
                                    description: location where verification check failed
                                    examples: ['/a/b/c/d/e']
                                  operation:
                                    type: string
                                    enum: [add, replace, remove]
                                  oldValue:
                                    type: object
                                  newValue:
                                    type: object
                            tx_infos:
                              type: object
                              properties:
                                tx_infos:
                                  type: array
                                  items:
                                    type: object
                                    properties:
                                      tx_id:
                                        type: string
                                      timestamp:
                                        type: object
                                        properties:
                                          seconds:
                                            type: integer
                                          nanos:
                                            type: integer
                          required:
                            - recordId
                            - valid
                            - record
                            - invalids
                            - deltaList
                            - timestamp
                        authorizedCount:
                          type: integer
                        timestamp:
                          type: string
                          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
                          description: if not provided, API will add it automatically
                          examples: ["2021-11-01T14:48:00.000Z"]
                        createdAt:
                          type: string
                          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
                          description: if not provided, API will add it automatically
                          examples: ["2021-11-01T14:48:00.000Z"]
                        updatedAt:
                          type: string
                          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
                          description: if not provided, API will add it automatically
                          examples: ["2021-11-01T14:48:00.000Z"]
                      required:
                        - id
                        - hash
                        - meta
                        - authorizedCount
                        - timestamp
                        - createdAt
                        - updatedAt
        '400':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
        '401':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
        '500':
          description: Access token is missing or invalid
          content:
            application/json:
              schema:
                type: object
                properties:
                  code:
                    type: integer
                  message:
                    type: string
                required:
                  - code
                  - message
security:
  - bearerAuth: []
```
