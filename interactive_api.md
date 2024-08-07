# This is documentation about Smartshield Transmitter API

## Smartshield Listener is a set list of API developed by Chainsmart

## Client need to call these APIs to verify data change and verification check to the blockchain without update the blockchain

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

- [x] Record Interactive Verification Check

```YAML
openapi: 3.1.0
info:
  title: Swagger SmartShield - OpenAPI 3.1
  description: |-
    This is a SmartShield Interactive(Web) Verification API based on the OpenAPI 3.1 specification.
  contact:
    email: riyanto@chainsmart.id
  version: 1.0.0
tags:
  - name: Record
    description: Access to ChainSmart Transmitter API
paths:
  /interactive/verify:
    post:
      tags:
        - Record
      summary: Check a record integrity againsts blockchain without update the blockchain
      operationId: VerifyRecordInteractive
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
                name:
                  type: string
                email:
                  type: string
                before:
                  type: object
              required:
                - recordId
                - name
                - email
          multipart/form-data:
            schema:
              type: object
              properties:
                recordId:
                  type: string
                name:
                  type: string
                email:
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
              required:
                - recordId
                - name
                - email
      responses:
        '200':
          description: Successful operation
          content:
            application/json:
              schema:
                type: object
                properties:
                  valid:
                    type: boolean
                  err:
                    type: object
                    properties:
                      code:
                        type: integer
                      message:
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
