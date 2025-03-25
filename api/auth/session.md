## Endpoint

**POST** `/auth/session`

## Description

This endpoint allows the webapp to verify the validity of the session.

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
  "token": "c457757d84a000a6776cd5dc97b63d3f244493fec71cc4aabf6dbbbcab7ccaaa"
}
```

| Parameter | Type   | Description   | Required |
| --------- | ------ | ------------- | -------- |
| token     | string | Session Token | Yes      |

## Validations

`token`:

- If `token` does not exist in databse return [`400`](#400-bad-request) with message: `Invalid token`

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

- If either the User agent or the IP address is different than the one stored, delete the session from the server side and return [`409`](#409-conflict) with message: `Conflicting User agent/IP`.
