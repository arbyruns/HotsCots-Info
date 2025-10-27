# Hots&Cots API Documentation

Welcome to the Hots&Cots API. This REST API provides access to quality-of-life feedback data for U.S. service members across dining facilities (Hots) and barracks/housing (Cots).

## Overview

The Hots&Cots API allows authorized users to retrieve, filter, and paginate through anonymized reviews and feedback data collected from service members across installations.

**Base URL:** `https://api.hotscots.app/functions/v1/api`

## Authentication

All API requests require an API key. Pass it in any of these ways:

```bash
# Option 1: Authorization header (recommended)
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=cots"

# Option 2: Query parameter
curl "https://api.hotscots.app/functions/v1/api/reviews?endpoint=cots&apikey=YOUR_API_KEY"

# Option 3: Custom header
curl -H "apikey: YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=cots"
```

**API Key Format:** `hc_live_xxxxxxxxxxxxxxxxxxxxxxxx`

Contact the Hots&Cots team to obtain your API credentials.

## Getting Started

### Basic Request

All endpoints require the `endpoint` query parameter to specify which data type you want (`cots` or `hots`).

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/health?endpoint=cots"
```

## Endpoints

### Health Check

**Endpoint:** `GET /health`

**Description:** Check API status and authentication.

**Query Parameters:**
- `endpoint` — Required. One of: `cots`, `hots`, `other`

**Example:**
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/health?endpoint=cots"
```

**Response:**
```json
{
  "success": true,
  "status": "ok",
  "timestamp": "2025-10-27T14:30:00Z"
}
```

---

### Reviews

**Endpoint:** `GET /reviews`

**Description:** Retrieve reviews for the specified endpoint type (Hots or Cots).

**Query Parameters:**
- `endpoint` — Required. `cots` or `hots`
- `limit` — Optional. Number of records to return (default: 100, max: 1000)
- `offset` — Optional. Number of records to skip (default: 0)

**Example - Get HOTS (Dining) Reviews:**
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=hots&limit=50&offset=0"
```

**Example - Get COTS (Housing) Reviews:**
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=cots&limit=50&offset=0"
```

**Response:**
```json
{
  "success": true,
  "endpoint": "cots",
  "data": [
    {
      "review_id": "rev_123",
      "created_at": "2025-10-15T14:30:00Z",
      "title": "Great housing conditions",
      "installation": "Fort Hood",
      "rating": 4,
      "feedback": "Clean, well-maintained barracks",
      "tags": ["clean", "comfortable"],
      "branch": "Army",
      "is_housing": true,
      "mold_issues": false,
      "pest_issues": false,
      "working_air_heat": true,
      "resolved": true
    }
  ],
  "pagination": {
    "limit": 50,
    "offset": 0,
    "returned": 50
  }
}
```

---

### Installations

**Endpoint:** `GET /installations`

**Description:** List all unique installations in the dataset for a given endpoint type.

**Query Parameters:**
- `endpoint` — Required. `cots` or `hots`

**Example:**
```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/installations?endpoint=hots"
```

**Response:**
```json
{
  "success": true,
  "endpoint": "hots",
  "data": [
    { "installation": "Fort Hood" },
    { "installation": "Fort Bragg" },
    { "installation": "Fort Lewis" }
  ]
}
```

---

## Hots Data Fields (Dining Reviews)

Available fields when querying `/reviews?endpoint=hots`:

- `review_id` — ID of review
- `created_at` — Date of review creation
- `title` — Title of the review
- `installation` — Installation location
- `dfac_name` — DFAC name
- `rating` — Overall rating (1-10)
- `feedback` — Detailed feedback description
- `tags` — Array of tags highlighting key aspects
- `branch` — Military branch
- `breakfast_ranking` — Breakfast quality ranking
- `lunch_ranking` — Lunch quality ranking
- `dinner_ranking` — Dinner quality ranking
- `meal_type` — Type of meal
- `dfac_choices` — Number of available choices
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

## Cots Data Fields (Barracks & Housing Reviews)

Available fields when querying `/reviews?endpoint=cots`:

- `review_id` — ID of review
- `created_at` — Date of review creation
- `title` — Title of the review
- `installation` — Installation location
- `rating` — Overall rating (1-10)
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
- `dpw_satisfaction_rating` — DPW satisfaction rating (1-10)
- `resolved` — Is the issue resolved
- `image_name` — Name of the image
- `photo_album` — Photo album associated with the review
- `bldg_number` — Building number

---

## Pagination

Use `limit` and `offset` parameters to paginate through results.

```bash
# Get first 100 records
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=cots&limit=100&offset=0"

# Get next 100 records (page 2)
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=cots&limit=100&offset=100"

# Get records 200-299
curl -H "Authorization: Bearer YOUR_API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=cots&limit=100&offset=200"
```

**Pagination Response:**
```json
{
  "pagination": {
    "limit": 100,
    "offset": 0,
    "returned": 100
  }
}
```

---

## Rate Limiting

Every response includes rate limit headers:

```
X-RateLimit-Limit: 1000        # Total requests allowed
X-RateLimit-Used: 234          # Requests used
X-RateLimit-Remaining: 766     # Requests remaining
X-RateLimit-Percentage: 23     # Usage percentage
```

**Rate Limits:**
- Varies by subscription plan
- Check response headers for your current limits
- `429 Too Many Requests` returned when exceeded

---

## Error Handling

The API returns standard HTTP status codes:

- `200 OK` — Request successful
- `400 Bad Request` — Invalid query parameters
- `401 Unauthorized` — Missing or invalid API key
- `403 Forbidden` — Endpoint access denied for your subscription
- `404 Not Found` — Endpoint not found
- `429 Too Many Requests` — Rate limit exceeded
- `500 Internal Server Error` — Server error

**Error Response Examples:**

Missing API key:
```json
{
  "success": false,
  "error": "Missing API key"
}
```

Invalid endpoint access:
```json
{
  "success": false,
  "error": "Access denied to hots endpoint",
  "allowedEndpoints": ["cots"]
}
```

Rate limit exceeded:
```json
{
  "success": false,
  "error": "Rate limit exceeded"
}
```

---

## Code Examples

### JavaScript/Node.js

```javascript
const API_KEY = "hc_live_xxxxx";
const API_URL = "https://api.hotscots.app/functions/v1/api";

async function getCotsReviews(limit = 100, offset = 0) {
  const response = await fetch(
    `${API_URL}/reviews?endpoint=cots&limit=${limit}&offset=${offset}`,
    {
      headers: {
        "Authorization": `Bearer ${API_KEY}`
      }
    }
  );

  if (!response.ok) {
    throw new Error(`API error: ${response.status}`);
  }

  const data = await response.json();
  
  // Check rate limit
  const remaining = response.headers.get("X-RateLimit-Remaining");
  console.log(`Requests remaining: ${remaining}`);

  return data;
}

// Get first page
const page1 = await getCotsReviews(100, 0);
console.log(`Got ${page1.pagination.returned} reviews`);

// Get next page
const page2 = await getCotsReviews(100, 100);
```

### Python

```python
import requests

API_KEY = "hc_live_xxxxx"
API_URL = "https://api.hotscots.app/functions/v1/api"

def get_reviews(endpoint, limit=100, offset=0):
    headers = {
        "Authorization": f"Bearer {API_KEY}"
    }
    
    params = {
        "endpoint": endpoint,
        "limit": limit,
        "offset": offset
    }
    
    response = requests.get(f"{API_URL}/reviews", headers=headers, params=params)
    
    if response.status_code != 200:
        raise Exception(f"API error: {response.status_code} - {response.text}")
    
    # Check rate limit
    remaining = response.headers.get("X-RateLimit-Remaining")
    print(f"Requests remaining: {remaining}")
    
    return response.json()

# Get reviews
data = get_reviews("cots", limit=100, offset=0)
print(f"Got {data['pagination']['returned']} reviews")
```

### cURL

```bash
API_KEY="hc_live_xxxxx"

# Get COTS reviews
curl -H "Authorization: Bearer $API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=cots&limit=50"

# Get HOTS reviews
curl -H "Authorization: Bearer $API_KEY" \
  "https://api.hotscots.app/functions/v1/api/reviews?endpoint=hots&limit=50"

# Get installations
curl -H "Authorization: Bearer $API_KEY" \
  "https://api.hotscots.app/functions/v1/api/installations?endpoint=cots"

# Health check
curl -H "Authorization: Bearer $API_KEY" \
  "https://api.hotscots.app/functions/v1/api/health?endpoint=cots"
```

---

## Data Privacy & OPSEC

All data returned by this API is anonymized and OPSEC-scrubbed. Personal identifying information (PII), unit designations, and sensitive operational details are redacted. Reviews represent aggregated feedback to help leaders improve quality-of-life conditions.

---

## Support

For questions, issues, or to request API access, contact the Hots&Cots team:

**Email:** support@hotscots.app  
**Website:** https://www.hotscots.app

---

**Last Updated:** October 2025
**API Version:** v1
**Base URL:** https://api.hotscots.app/functions/v1/api
