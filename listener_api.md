# This is documentation about Smartshield Listener API

## SmartShield Listener is a set list of API should be developed by Client

## SmartShield will call this api if there is a verification request from SmartShield UI

This is a SmartShield Listener API based on the OpenAPI 3.1 specification that should build by client.\
SmartShield can authenticate to SmartShield Listener using basic authentication or OpenID authentication and use bearer auth when query records

- [ ] Records Request

```YAML
openapi: 3.1.0
info:
  title: Swagger SmartShield - OpenAPI 3.1
  description: |-
    This is a SmartShield Listener API based on the OpenAPI 3.1 specification that should build by client. <br>
    SmartShield can authenticate to SmartShield Listener using basic authentication or OpenID authentication and use bearer auth when query records
  contact:
    email: riyanto@chainsmart.id
  version: 1.0.0
tags:
  - name: SmartShieldListener
    description: Access to ChainSmart Listener API
paths:
  /record:
    get:
      tags:
        - SmartShieldListener
      summary: List Record from client system
      description: List Record from client system
      operationId: SmartShieldListenerListRecord
      security:
        - basicAuth: []
        - bearerAuth: []
      parameters:
        - in: query
          name: size
          schema:
            type: integer
          description: The numbers of items to return
        - in: query
          name: recordIds
          explode: true
          schema:
            type: array
            items:
              type: string
          description: |
            a single record id or multiple record id set multiple times, eg /record?recordIds=xyz or /record?recordIds=a1&recordIds=a2&recordIds=b1&recordIds=b2
        - in: query
          name: filter
          schema:
            type: string
          description: record filter that may contains record keyword or url encoded jmespath https://jmespath.org/
        - in: query
          name: after
          schema:
            type: string
        - in: query
          name: before
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
                  links:
                    type: object
                    properties:
                      prev:
                        type: string
                        nullable: true
                        description: link to previous page, if recordIds is provided, this should be null
                        examples: ['{domain.id}/records?before=yyy&size=2']
                      next:
                        type: string
                        nullable: true
                        description: link to next page, if recordIds is provided, this should be null
                        examples: ['{domain.id}/records?after=zzz&size=2']
                  meta:
                    type: object
                    properties:
                      page:
                        type: object
                        properties:
                          cursor:
                            type: string
                            nullable: true
                            description: the value of cursor that will be used to fetch the next or previous page, usually it will be recordId
                  records:
                    type: array
                    items:
                      type: object
                      properties:
                        recordId:
                          type: string
                        before:
                          schema:
                            Record:
                              oneOf:
                                - $ref: '#/components/schemas/RecordJson'
                                - $ref: '#/components/schemas/RecordFile'
                          description: |
                            RecordJson should be use for json record <br>
                            RecordFile should be use for file record <br>
                            see schema below
                      required:
                        - recordId
                        - before
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
components:
  schemas:
    RecordJson:
      type: object
    RecordFile:
      type: array
      items:
        type: object
        properties:
        name:
          type: string
          description: file name
        file:
          type: string
          description: file url or encoded base64 file
        metaData:
          type: object
          description: file meta data in json format
  securitySchemes:
    basicAuth:
      type: http
      scheme: basic
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
security:
  - basicAuth: []
  - bearerAuth: []
```
