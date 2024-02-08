# This is documentation about Smartshield Transmitter API

## Smartshield Listener is a set list of API developed by Chainsmart

## Client need to call these APIs to submit data change and verification check to the blockchain

\
Record API

## I. Rest API

- [ ] Authentication

```YAML
  path: /auth/signin
  method: POST
  input:
    - body:
      type: JSON
      data:
        - name: username
          type: string
          nullable: true
        - name: password
          type: string
          nullable: false
  output:
    - code: 201
      type: JSON
      data:
        - name: refreshToken
          type: string
          nullable: false
          format: JWT
        - name: accessToken
          type: string
          nullable: false
          format: JWT
        - name: accessTokenExpireAt
          type: string
          nullable: false
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
          example: "2021-11-01T14:48:00.000Z"
        - name: refreshTokenExpireAt
          type: string
          nullable: false
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
          example: "2021-11-01T14:48:00.000Z"
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Extend Authentication

```YAML
  path: /auth/signin
  method: PUT
  input:
    - body:
      type: JSON
      data:
        - name: refreshToken
          type: string
          nullable: false
  output:
    - code: 201
      type: JSON
      data:
        - name: refreshToken
          type: string
          nullable: false
          format: JWT
        - name: accessToken
          type: string
          nullable: false
          format: JWT
        - name: accessTokenExpireAt
          type: string
          nullable: false
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
          example: "2021-11-01T14:48:00.000Z"
        - name: refreshTokenExpireAt
          type: string
          nullable: false
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
          example: "2021-11-01T14:48:00.000Z"
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Delete Authentication

```YAML
  path: /auth/signin
  method: DELETE
  input:
    - body:
      type: JSON
      data:
        - name: refreshToken
          type: string
          nullable: false
  output:
    - code: 204
      description: 204 code returns no content
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Create/Update/Delete Record

Create: before is null, after has value\
Update: both before and after has value\
Delete: before has value, after is null

```YAML
  path: /record
  method: POST
  header:
    - name: Authorization
      type: string
      format: Bearer {accessToken}
    - name: Content-Type
      type: string
  input:
    - body:
      type: JSON
      data:
        - name: recordId
          type: string
          nullable: false
        - name: ownerType
          type: string
          nullable: false
          description: the type of owner id, eg  NIM, NIK, social number, space character is not allowed.
        - name: ownerId
          type: string
          nullable: false
          description: submitter has responsibility to make sure owner id match owner type format.
        - name: before
          type: JSON
          nullable: true
          description: both before and update must not be null
        - name: after
          type: JSON
          nullable: true
        - name: timestamp
          type: string
          nullable: true
          example: "2021-11-01T14:48:00.000Z"
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
          description: if not provided, API will add it automatically
    - body:
      type: multipart/form-data
      data:
        - name: recordId
          type: string
          nullable: false
        - name: ownerType
          type: string
          nullable: false
          description: the type of owner id, eg  NIM, NIK, social number, space character is not allowed.
        - name: ownerId
          type: string
          nullable: false
          description: submitter has responsibility to make sure owner id match owner type format.
        - name: before
          type: array
          nullable: true
          values:
            - name: file
              type: binary
              nullable: false
            - name: metadata
              type: JSON
        - name: after
          type: array
          nullable: true
          values:
            - name: file
              type: binary
              nullable: false
            - name: metadata
              type: JSON
        - name: timestamp
          type: string
          nullable: true
          example: "2021-11-01T14:48:00.000Z"
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
          description: if not provided, API will add it automatically
  output:
    - code: 201
      type: JSON
      data: UUIDV4
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Record verification check

```YAML
  path: /record/validate
  method: POST
  header:
    - name: Authorization
      type: string
      format: Bearer {accessToken}
  input:
    - body:
      type: JSON
      data:
        - name: recordId
          type: string
          nullable: false
        - name: before
          type: JSON
          nullable: false
        - name: timestamp
          type: string
          nullable: true
          example: "2021-11-01T14:48:00.000Z"
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
          description: if not provided, API will add it automatically
    - body:
      type: multipart/form-data
      data:
        - name: recordId
          type: string
          nullable: false
        - name: before
          type: array
          nullable: true
          values:
            - name: file
              type: binary
              nullable: false
            - name: metadata
              type: JSON
        - name: timestamp
          type: string
          nullable: true
          example: "2021-11-01T14:48:00.000Z"
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
          description: if not provided, API will add it automatically
  output:
    - code: 201
      type: JSON
      data: UUIDV4
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Load Single Record

```YAML
  path: /record/:recordId
  method: GET
  header:
    - name: Authorization
      type: string
      format: Bearer {accessToken}
  input:
    - path:
      name: recordId
      type: string
      nullable: false
  output:
    - code: 200
      type: JSON
      data:
        - name: recordId
          type: string
          nullable: false
        - name: ownerType
          type: string
          nullable: false
          description: the type of owner id, eg  NIM, NIK, social_number, space are not allowed.
        - name: ownerId
          type: string
          nullable: false
          description: submitter responsible to make sure owner id match owner type format.
        - name: valid
          type: boolean
          nullable: false
          description: Indication if record has pass verification check.
        - name: record
          type: JSON
          nullable: false
        - name: invalids
          type: array
          nullable: false
          values:
            - name: path
              type: string
              nullable: false
              description: location where verification check failed
              example: /a/b/c/d/e
            - name: operation
              type: enum string(“add”, “replace”, “remove”)
              nullable: false
              description: operation applied to json
            - name: oldValue
              type: any
              nullable: true
              description: record before operation applied
            - name: newValue
              type: any
              nullable: true
              description: record after operation applied
        - name: deltaList
          type: array
          nullable: false
          values:
            - name: path
              type: string
              nullable: false
              description: location where verification check failed
              example: /a/b/c/d/e
            - name: operation
              type: enum string(“add”, “replace”, “remove”)
              nullable: false
              description: operation applied to json
            - name: oldValue
              type: any
              nullable: true
              description: record before operation applied
            - name: newValue
              type: any
              nullable: true
              description: record after operation applied
        - name: timestamp
          type: string
          nullable: true
          example: "2021-11-01T14:48:00.000Z"
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 404
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```

- [ ] Load Records

```YAML
  path: /record
  method: GET
  header:
    - name: Authorization
      type: string
      format: Bearer {accessToken}
  output:
    - code: 200
      type: JSON
      data:
        - type: array
          nullable: false
          values:
            - name: recordId
              type: string
              nullable: false
            - name: ownerType
              type: string
              nullable: false
              description: the type of owner id, eg  NIM, NIK, social_number, space are not allowed.
            - name: ownerId
              type: string
              nullable: false
              description: submitter responsible to make sure owner id match owner type format.
            - name: valid
              type: boolean
              nullable: false
              description: Indication if record has pass verification check.
            - name: invalidPathList
              type: array
              nullable: false
              values:
                - name: path
                  type: string
                  nullable: false
                  description: location where verification check failed
                  example: /a/b/c/d/e
                - name: operation
                  type: enum string(“add”, “replace”, “remove”)
                  nullable: false
                  description: operation applied to json
                - name: oldValue
                  type: any
                  nullable: true
                  description: record before operation applied
                - name: newValue
                  type: any
                  nullable: true
                  description: record after operation applied
            - name: timestamp
              type: string
              nullable: true
              example: "2021-11-01T14:48:00.000Z"
              format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
    - code: 401
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 403
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
    - code: 500
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
```
