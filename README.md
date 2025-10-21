# Hots&Cots-Info In Progress

This is general infomation on the table structure of Hots&Cots data fields.

# Hots&Cots API Documentation

Welcome to the Hots&Cots API. This REST API provides access to quality-of-life feedback data for U.S. service members across dining facilities (Hots) and barracks/housing (Cots).

## Overview

The Hots&Cots API allows authorized users to retrieve, filter, and paginate through anonymized reviews and feedback data collected from service members across installations.

**Base URL:** `https://api.hotscots.app/rest/v1`

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
curl -X GET "https://api.hotscots.app/rest/v1/reviews" \
  -H "apikey: YOUR_API_KEY"
```

## Endpoints

### Hots Data (Dining Reviews)

**Endpoint:** `/hots`

**Description:** Retrieve dining facility (DFAC) reviews and feedback.

**Example:**
```bash
curl -X GET "https://api.hotscots.app/rest/v1/hots" \
  -H "apikey: YOUR_API_KEY"
```

**Available Fields:**
- `review_id` — ID of review
- `created_at` — Date of review creation
- `title` — Title of the review
- `installation` — Installation location
- `dfac_name` — DFAC name
- `rating` — Overall rating
- `feedback` — Detailed feedback description
- `tags` — Array of tags highlighting key aspects
- `branch` — Military branch
- `breakfast_ranking` — Breakfast quality ranking
- `lunch_ranking` — Lunch quality ranking
- `dinner_ranking` — Dinner quality ranking
- `meal_type` — Type of meal
- `dfac_choices` — Available choices in the DFAC
- `dfac_healthy` — Are there healthy choices available
- `dfac_filling` — Are the meals filling
- `dfac_visual` — Was food visually appealing
- `dfac_temp` — Was food at proper temperature
- `dfac_police` — Were you prevented from getting more food
- `dfac_special` — Was this a special holiday meal
- `has_shuttle` — Does the DFAC have a shuttle
- `walking_distance` — Within walking distance
- `kiosk_level` — How full or empty was the kiosk
- `inedible_food_issue` — Was the food inedible due to mold or undercooked
- `resolved` — Is the issue resolved
- `image_name` — Name of the image
- `photo_album` — Photo album associated with the review
- `bldg_number` — Building number

---

### Cots Data (Barracks & Housing Reviews)

**Endpoint:** `/cots`

**Description:** Retrieve barracks and on-post housing reviews and feedback.

**Example:**
```bash
curl -X GET "https://api.hotscots.app/rest/v1/cots" \
  -H "apikey: YOUR_API_KEY"
```

**Available Fields:**
- `review_id` — ID of review
- `created_at` — Date of review creation
- `title` — Title of the review
- `installation` — Installation location
- `rating` — Overall rating of review
- `feedback` — Detailed feedback description
- `tags` — Array of tags highlighting key aspects
- `branch` — Military branch
- `is_housing` — Is it housing related or barracks
- `room_amenities` — Amenities available in the room
- `mold_issues` — Presence of mold issues
- `pest_issues` — Presence of pest issues
- `working_air_heat` — Is the air conditioning/heating working
- `working_furniture` — Is the furniture in working condition
- `accessible_laundry` — Is laundry accessible
- `private_sleeping_area` — Is there a private sleeping area
- `lockable_doors` — Are there lockable doors
- `working_fire_detection` — Is the fire detection system working
- `exterior_lighting` — Is there exterior lighting
- `interior_lighting` — Is there interior lighting
- `safety_features` — Safety features available
- `dpw_satisfaction_rating` — DPW satisfaction rating
- `resolved` — Is the issue resolved
- `image_name` — Name of the image
- `photo_album` — Photo album associated with the review
- `bldg_number` — Building number

---

---

## Pagination

Use `limit` and `offset` parameters to paginate through results.

```bash
# Get first 10 records
curl -X GET "https://api.hotscots.app/rest/v1/hots?limit=10&offset=0" \
  -H "apikey: YOUR_API_KEY"

# Get next 10 records (page 2)
curl -X GET "https://api.hotscots.app/rest/v1/hots?limit=10&offset=10" \
  -H "apikey: YOUR_API_KEY"
```

### Getting Total Count

```bash
curl -X GET "https://api.hotscots.app/rest/v1/hots?select=count" \
  -H "apikey: YOUR_API_KEY" \
  -H "Prefer: count=exact"
```

The response header `Content-Range` will show the total count and range (e.g., `0-9/100`).

## Filtering

Filter results by adding query parameters:

```bash
# Filter by installation
curl -X GET "https://api.hotscots.app/rest/v1/hots?installation=eq.Fort%20Hood" \
  -H "apikey: YOUR_API_KEY"

# Filter by rating
curl -X GET "https://api.hotscots.app/rest/v1/hots?rating=gte.4" \
  -H "apikey: YOUR_API_KEY"

# Combine filters
curl -X GET "https://api.hotscots.app/rest/v1/hots?installation=eq.Fort%20Hood&rating=gte.4" \
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
curl -X GET "https://api.hotscots.app/rest/v1/hots?order=created_at.desc" \
  -H "apikey: YOUR_API_KEY"

# Sort by rating (highest first)
curl -X GET "https://api.hotscots.app/rest/v1/hots?order=rating.desc" \
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

**Email:** support@hotscots.app  
**Website:** https://www.hotscots.app

---

**Last Updated:** October 2025
