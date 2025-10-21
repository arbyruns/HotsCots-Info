# Hots&Cots-Info In Progress

This is general infomation on the table structure of Hots&Cots data fields.

# Hots&Cots API Documentation

Welcome to the Hots&Cots API. This REST API provides access to quality-of-life feedback data for U.S. service members across dining facilities (Hots) and barracks/housing (Cots).

## Overview

The Hots&Cots API allows authorized users to retrieve, filter, and paginate through anonymized reviews and feedback data collected from service members across installations.

**Base URL:** `https://api.hotsandcots.com/rest/v1`

## Authentication

All API requests require an API key passed in the request headers.

```bash
-H "apikey: YOUR_API_KEY"
-H "Authorization: Bearer YOUR_API_KEY"
```

Contact the Hots&Cots team to obtain your API credentials.

## Getting Started

### Basic GET Request

```bash
curl -X GET "https://api.hotsandcots.com/rest/v1/reviews" \
  -H "apikey: YOUR_API_KEY"
```

## Endpoints

### Hots Data (Dining Reviews)

**Endpoint:** `/hots`

**Description:** Retrieve dining facility (DFAC) reviews and feedback.

**Example:**
```bash
curl -X GET "https://api.hotsandcots.com/rest/v1/hots" \
  -H "apikey: YOUR_API_KEY"
```

**Available Fields:**
- `[HOTS_DATA_FIELDS_PLACEHOLDER]`

---

### Cots Data (Barracks & Housing Reviews)

**Endpoint:** `/cots`

**Description:** Retrieve barracks and on-post housing reviews and feedback.

**Example:**
```bash
curl -X GET "https://api.hotsandcots.com/rest/v1/cots" \
  -H "apikey: YOUR_API_KEY"
```

**Available Fields:**
- `[COTS_DATA_FIELDS_PLACEHOLDER]`

---

## Pagination

Use `limit` and `offset` parameters to paginate through results.

```bash
# Get first 10 records
curl -X GET "https://api.hotsandcots.com/rest/v1/hots?limit=10&offset=0" \
  -H "apikey: YOUR_API_KEY"

# Get next 10 records (page 2)
curl -X GET "https://api.hotsandcots.com/rest/v1/hots?limit=10&offset=10" \
  -H "apikey: YOUR_API_KEY"
```

### Getting Total Count

```bash
curl -X GET "https://api.hotsandcots.com/rest/v1/hots?select=count" \
  -H "apikey: YOUR_API_KEY" \
  -H "Prefer: count=exact"
```

The response header `Content-Range` will show the total count and range (e.g., `0-9/100`).

## Filtering

Filter results by adding query parameters:

```bash
# Filter by installation
curl -X GET "https://api.hotsandcots.com/rest/v1/hots?installation=eq.Fort%20Hood" \
  -H "apikey: YOUR_API_KEY"

# Filter by rating
curl -X GET "https://api.hotsandcots.com/rest/v1/hots?rating=gte.4" \
  -H "apikey: YOUR_API_KEY"

# Combine filters
curl -X GET "https://api.hotsandcots.com/rest/v1/hots?installation=eq.Fort%20Hood&rating=gte.4" \
  -H "apikey: YOUR_API_KEY"
```

### Filter Operators

- `eq` — equals
- `neq` — not equals
- `gt` — greater than
- `gte` — greater than or equal
- `lt` — less than
- `lte` — less than or equal
- `like` — pattern match
- `in` — in list

## Ordering

Sort results by adding the `order` parameter:

```bash
# Sort by date (newest first)
curl -X GET "https://api.hotsandcots.com/rest/v1/hots?order=created_at.desc" \
  -H "apikey: YOUR_API_KEY"

# Sort by rating (highest first)
curl -X GET "https://api.hotsandcots.com/rest/v1/hots?order=rating.desc" \
  -H "apikey: YOUR_API_KEY"
```

Use `.asc` for ascending or `.desc` for descending order.

## Response Format

Responses are returned as JSON arrays:

```json
[
  {
    "id": 12345,
    "installation": "Fort Hood",
    "rating": 4,
    "created_at": "2025-10-15T14:30:00Z",
    "tags": ["clean", "food_variety"],
    "feedback": "Great meal options this week"
  },
  {
    "id": 12346,
    "installation": "Fort Hood",
    "rating": 2,
    "created_at": "2025-10-14T09:15:00Z",
    "tags": ["mold", "hvac"],
    "feedback": "HVAC not working in barracks"
  }
]
```

## Error Handling

The API returns standard HTTP status codes:

- `200 OK` — Request successful
- `400 Bad Request` — Invalid query parameters
- `401 Unauthorized` — Missing or invalid API key
- `404 Not Found` — Endpoint not found
- `429 Too Many Requests` — Rate limit exceeded
- `500 Internal Server Error` — Server error

Error responses include a message:

```json
{
  "error": "Invalid API key"
}
```

## Rate Limiting

API requests are rate-limited to prevent abuse. Current limits:

- **100 requests per minute** per API key

If you exceed the limit, you'll receive a `429` response. Wait before retrying.

## Data Privacy & OPSEC

All data returned by this API is anonymized and OPSEC-scrubbed. Personal identifying information (PII), unit designations, and sensitive operational details are redacted. Reviews represent aggregated feedback to help leaders improve quality-of-life conditions.

## Support

For questions, issues, or to request API access, contact the Hots&Cots team:

**Email:** support@hotsandcots.com  
**Website:** https://www.hotsandcots.com

---

**Last Updated:** October 2025
