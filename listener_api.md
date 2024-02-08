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
  description: client can change path or methods, for pagination support, if cursor is string, lexical order should be used.
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
          nullable: true
          values: string
          description: if value is provided, after, before, and size should be ignored
        - name: after
          type: any
          nullable: true
          description: value to get the next page, usually it will be recordId
        - name: before
          type: any
          nullable: false
          description: value to get the previous page, usually it will be recordId
        - name: size
          type: int
          nullable: true
          description: "client can decide the default page size, eg: 200 items per page"
      description: if nothing is provided, the first page will be returned
  output:
    - code: 200
      type: JSON
      data:
        - name: links
          type: JSON
          nullable: false
          description: if recordIds is provided, this value should be null
          values:
            - name: prev
              type: string
              nullable: true
              description: link to previous page, if recordIds is provided, this should be null
              example: /records?before=yyy&size=2
            - name: next
              type: string
              nullable: true
              description: link to next page, if recordIds is provided, this should be null
              example: /records?after=zzz&size=2
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
        - name: meta
          type: JSON
          nullable: false
          values:
            - name: page
              type: JSON
              nullable: false
              values:
                - name: cursor
                  type: any
                  nullable: true
                  description: the value of cursor that will be used to fetch the next or previous page, usually it will be recordId
    - code: 400
      type: JSON
      data:
        - name: code
          type: int
          nullable: false
        - name: message
          type: string
          nullable: false
          example: size must be a positive integer; got 0
        - name: source
          type: JSON
          values:
            - name: parameter
              type: string
              example: size
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
