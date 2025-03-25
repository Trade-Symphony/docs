# Endpoint: `/your-endpoint`

## Description

Provide a brief description of the endpoint.

#### Methods:

- [POST](#method-post)
- [PATCH](#method-patch)
- [GET](#method-get)
- [DELETE](#method-delete)

---

## Method: `METHOD`

## Description

Describe what this method does.

## Request

### Headers

| Key          | Value            | Required |
| ------------ | ---------------- | -------- |
| Content-Type | application/json | Yes      |

### Request Body

```json
{
  "parameter": "value"
}
```

| Parameter | Type | Description | Required |
| --------- | ---- | ----------- | -------- |
| param1    | type | description | Yes/No   |

## Validations

`param1`:

- If `param1` does not exist, return [`400`](#400-bad-request) with message: `Invalid param1`

## Response

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

#### 409 Conflict

```json
{
  "success": false,
  "message": "<message>"
}
```

## Notes:

- Include any relevant details, such as rate limits or security measures.
