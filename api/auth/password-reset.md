# Endpoint: `/auth/password-reset`

## Description

This endpoint is for operations related to password reset.

#### Methods:

- [POST](#method-post)
- [PATCH](#method-patch)

## Method: `POST`

## Description

This method allows users to request a password reset link to their email.

## Request

### Headers

| Key          | Value            | Required |
| ------------ | ---------------- | -------- |
| Content-Type | application/json | Yes      |

### Request Body

```json
{
  "username": "exampleUser"
}
```

| Parameter | Type   | Description | Required |
| --------- | ------ | ----------- | -------- |
| username  | string | Username    | Yes      |

## Validations

`username`:

- If `username` does not exist return [`400`](#400-bad-request) with message: `Invalid Username`

## Response

### Success Response (201 Created)

```json
{
  "success": true
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

## Notes:

- Endpoint is rate limited to 1 request per second.
- A email with a base64 encoded password reset token is sent to the user's email.
- The password reset token is generated securely (SHA-256 hash of random bytes).
- If applicable, the previous password reset token is removed.

## Method: `PATCH`

## Description

This method allows users to change their password.

## Request

### Headers

| Key          | Value            | Required |
| ------------ | ---------------- | -------- |
| Content-Type | application/json | Yes      |

### Request Body

```json
{
  "token": "NjAyNjY2ZjNmMzM1ZWI4M2YyZTExYzQyODEzYzIxYTdkYTQzODljZmIxNmI5M2MzYzVkNWIzMjkyMWU1YTc5Mg==",
  "password": "SecurePassword123!",
  "confirm_password": "SecurePassword123!"
}
```

| Parameter        | Type   | Description                         | Required |
| ---------------- | ------ | ----------------------------------- | -------- |
| token            | string | Base64 encoded password reset token | Yes      |
| password         | string | Password                            | Yes      |
| confirm_password | string | Confim Password                     | Yes      |

## Validations

`token`:

- If `token` does not exist return [`400`](#400-bad-request) with message: `Invalid Token`
- If `token` has expired return [`400`](#400-bad-request) with message: `Expired Token`

`password`: A minimum of 8 characters, 1 uppercase, 1 lowercase and 1 special character (`` ~`! @#$%^&*()-_+={}[]|\;:"<>,./? ``).

- If `password` doesnt match requirements, return [`400`](#400-bad-request) with message: `Password does not match expected critera`.

- If `password` is compromised [^password], return [`409`](#409-conflict) with message: `This password has been compromised`.

`confirm_password`: A minimum of 8 characters, 1 uppercase, 1 lowercase and 1 special character (`` ~`! @#$%^&*()-_+={}[]|\;:"<>,./? ``).

- If `password` doesnt match requirements, return [`400`](#400-bad-request) with message: `Password does not match expected critera`.

- If `confirm_password` does not match `password`, return [`400`](#400-bad-request) with message: `Passwords do not match`.

## Response

### Success Response (200 OK)

```json
{
  "success": true
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

## Notes:

- Endpoint is rate limited to 1 request per second.
- The password is hashed using scrypt. [^scrypt_min]

[^password]: Refer to Checking for compromised passwords in the Copenhagen book (https://thecopenhagenbook.com/password-authentication#checking-for-compromised-passwords).
[^scrypt_min]: Refer to Scrypt in the Copenhagen book for the minimum parameters for scrypt (https://thecopenhagenbook.com/password-authentication#scrypt).
