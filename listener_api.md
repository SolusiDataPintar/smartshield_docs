# This is documentation about Smartshield Listener API

## SmartShield Listener is a set list of API should be developed by Client

## SmartShield will call this api if there is a verification request from SmartShield UI

The Authentication, Extend Authentication, and Delete Authentication is api to secure record endpoints that uses JWT for authentication.\
\
The alternatives are basic authentication, long token, or OAUTH 2.0.

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
        - name: accessToken
          type: string
          nullable: false
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

- [ ] Records Request

```YAML
  path: /records(any)
  method: GET(ANY)
  description: client can change path or methods
  header:
    - name: Authorization
      type: string
      format: Bearer {accessToken}/Basic ${base64(username:password)}/Token
  input:
    - body:
      type: JSON
      data:
        - name: recordIds
          type: array
          nullable: false
          values: string
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
            - name: before
              type: JSON
              nullable: false
            - name: timestamp
              type: string
              nullable: true
              example: "2021-11-01T14:48:00.000Z"
              format: RFC3339(YYYY-MM-dd’T’HH:mm:ss.sss’Z’)
              descrption: optional
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
