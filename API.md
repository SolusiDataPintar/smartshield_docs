# This is documentation about Data Validator API

\
Record Validator API

## I. Rest API

- [ ] Authentication

```YAML
  path: /auth
  method: POST
  input:
    - body:
      type: JSON
      data:
        - name: username
          type: string
          nullable: true
        - name: passphrase
          type: string
          nullable: false
  output:
    - code: 201
      type: JSON
      data:
        - name: refreshToken
          type: string
          nullable: false
        - name: accessToken
          type: string
          nullable: false
        - name: refreshTokenExpiredAt
          type: int64
          format: unixepoch
          nullable: false
        - name: accessTokenExpiredAt
          type: int64
          format: unixepoch
          nullable: false
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

- [ ] Token Authentication

```YAML
  path: /auth
  method: POST
  input:
    - body:
      type: JSON
      data:
        - name: token
          type: string
          nullable: false
  output:
    - code: 201
      type: JSON
      data:
        - name: refreshToken
          type: string
          nullable: false
        - name: accessToken
          type: string
          nullable: false
        - name: refreshTokenExpiredAt
          type: int64
          format: unixepoch
          nullable: false
        - name: accessTokenExpiredAt
          type: int64
          format: unixepoch
          nullable: false
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

 [ ] Create Authentication Token

```YAML
  path: /authtoken
  method: POST
  header:
    - name: Authorizations
      type: string
      format: JWT
  input:
    - body:
      type: JSON
      data:
        - name: note
          type: string
          nullable: true
        - name: expiration
          type: string
          nullable: true
          example: "2021-11-01T14:48:00.000Z"
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
  output:
    - code: 201
      type: string
      description: use this token to sign in at Token Authentication, please save this token because this is the only time the token shown, we do not store your token.
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

- [ ] Create/Update Record

```YAML
  path: /record
  method: POST
  header:
    - name: Authorizations
      type: string
      format: JWT
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
          description: the type of owner id, eg  NIM, NIK, social_number, space are not allowed.
        - name: ownerId
          type: string
          nullable: false
          description: submitter responsible to make sure owner id match owner type format.
        - name: before
          type: JSON
          nullable: true
        - name: after
          type: JSON
          nullable: false
        - name: timestamp
          type: string
          nullable: true
          example: "2021-11-01T14:48:00.000Z"
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
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

- [ ] Record webhook from blockchain

```YAML
  path: /record or client request
  method: POST
  header:
    - name: Authorizations
      type: string
      format: JWT
  input:
    - body:
      type: JSON
      data:
        - name: taskId
          type: string
          format: UUIDV4
          example: 020d0353-af93-4935-bb4d-54de04aaed57
          description: UUIDV4 returned when client submit record to blockchain
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
        - name: status
          type: int
          nullable: false
          descritpion: status from blockchain
        - name: before
          type: JSON
          nullable: true
        - name: after
          type: JSON
          nullable: false
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
  output:
    - code: 200-299
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
    - name: Authorizations
      type: string
      format: JWT
  input:
    - body:
      type: JSON
      data:
        - name: recordId
          type: string
          nullable: false
        - name: record
          type: JSON
          nullable: false
        - name: timestamp
          type: string
          nullable: true
          example: "2021-11-01T14:48:00.000Z"
          format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
  output:
    - code: 200
      type: JSON
      data:
        - name: valid
          type: boolean
          nullable: false
          description: Indication if record has pass verification check.
        - name: before
          type: JSON
          nullable: true
        - name: after
          type: JSON
          nullable: false
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

- [ ] Load One Record

```YAML
  path: /record/:recordId
  method: GET
  header:
    - name: Authorizations
      type: string
      format: accessClaim from create access
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

- [ ] Load Record

```YAML
  path: /record
  method: GET
  header:
    - name: Authorizations
      type: string
      format: accessClaim from create access
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
            - name: record
              type: JSON
              nullable: false
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
