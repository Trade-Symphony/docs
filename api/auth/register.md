## Endpoint

**POST** `/auth/register`

## Description

This endpoint allows users to register a new account by providing the required details.

## Request

### Headers

| Key          | Value            | Required |
| ------------ | ---------------- | -------- |
| Content-Type | application/json | Yes      |

### Request Body

```json
{
  "username": "exampleUser",
  "email": "user@example.com",
  "password": "SecurePassword123!",
  "confirm_password": "SecurePassword123!"
}
```

| Parameter        | Type   | Description     | Required |
| ---------------- | ------ | --------------- | -------- |
| username         | string | Username        | Yes      |
| email            | string | Email address   | Yes      |
| password         | string | Password        | Yes      |
| confirm_password | string | Confim Password | Yes      |

## Validations

`username`: A minimum of 6 characters with a maximum of 16 characters.

- If `username` does not match requirments return [`400`](#400-bad-request) with message: `Username does not match requirements`
- If `username` is already taken return [`409`](#409-conflict) with message: `Username already exists`

`email`: Valid email address.

- If `email` is already in the database return [`409`](#409-conflict) with message: `Email already exists`

[TODO: Verify email is not a disposable email]: #

`password`: A minimum of 8 characters, 1 uppercase, 1 lowercase and 1 special character (`` ~`! @#$%^&*()-_+={}[]|\;:"<>,./? ``).

- If `password` doesnt match requirements, return [`400`](#400-bad-request) with message: `Password does not match expected critera`.

- If `password` is compromised [^password], return [`409`](#409-conflict) with message: `This password has been compromised`.

`confirm_password`: A minimum of 8 characters, 1 uppercase, 1 lowercase and 1 special character (`` ~`! @#$%^&*()-_+={}[]|\;:"<>,./? ``).

- If `password` doesnt match requirements, return [`400`](#400-bad-request) with message: `Password does not match expected critera`.

- If `confirm_password` does not match `password`, return [`400`](#400-bad-request) with message: `Passwords do not match`.

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

#### 409 Conflict

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
