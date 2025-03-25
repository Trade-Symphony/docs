## Endpoint

**POST** `/auth/login`

## Description

This endpoint allows users to login to their account by providing the required details.

## Request

### Headers

| Key             | Value            | Required |
| --------------- | ---------------- | -------- |
| Content-Type    | application/json | Yes      |
| User-Agent      | -                | Yes      |
| X-Forwarded-For | -                | Yes      |

### Request Body

```json
{
  "username": "exampleUser",
  "password": "SecurePassword123!"
}
```

| Parameter | Type   | Description | Required |
| --------- | ------ | ----------- | -------- |
| username  | string | Username    | Yes      |
| password  | string | Password    | Yes      |

## Validations

`username`:

- If `username` does not exist return [`400`](#400-conflict) with message: `Invalid Username`

`password`:

- If `password` is compromised [^password], return [`409`](#409-conflict) with message: `"This password has been compromised"`.

- If `password` doesnt match the username, return [`400`](#400-bad-request) with message: `Invalid Password`.

## Response

### Success Response (201 Created)

```json
{
  "success": true,
  "session_token": "<sessionToken>"
}
```

### Error Responses

#### 400 Bad Request

```json
{
  "success": false,
  "message": "<message>"
}
```

#### 409 Conflict

```json
{
  "success": false,
  "message": "<message>"
}
```

## Notes:

- Endpoint is rate limited to 1 request per second.
- The session token is generated securely (SHA-256 hash of random bytes).
- Both the user agent and the ip address of the user is stored to minimalise session hijacking.

[^password]: Refer to Checking for compromised passwords in the Copenhagen book (https://thecopenhagenbook.com/password-authentication#checking-for-compromised-passwords)
