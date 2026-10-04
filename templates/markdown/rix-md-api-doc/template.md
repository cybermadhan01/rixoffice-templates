# <METHOD> /v1/<resource>

<One sentence: what this endpoint does.>

**Base URL:** `https://api.example.com`

**Auth:** `Authorization: Bearer <token>`

**Rate limit:** <n> requests per minute per key

## Path parameters

| Name | Type | Required | Description |
| --- | --- | --- | --- |
| `id` | string | yes | <description> |

## Query parameters

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| `limit` | integer | `20` | <description> |
| `cursor` | string | - | Opaque pagination cursor |

## Request body

```json
{
  "field": "value"
}
```

## Responses

### 200 OK

```json
{
  "id": "res_123",
  "created_at": "2026-01-01T00:00:00Z"
}
```

### 4xx errors

| Status | Code | When it happens | What to do |
| --- | --- | --- | --- |
| 400 | `invalid_request` | <condition> | <fix> |
| 404 | `not_found` | <condition> | <fix> |
| 429 | `rate_limited` | <condition> | <fix> |

## Example

```bash
curl -X GET 'https://api.example.com/v1/resource/res_123' \
  -H 'Authorization: Bearer $TOKEN'
```

## Notes

- <anything that trips people up>
- <deprecation or version note>
