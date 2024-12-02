# API Documentation: Get Articles

## GET /api/articles

Retrieve a list of articles with optional filters.

### Query Parameters

| Parameter  | Type     | Description                                         |
|------------|----------|-----------------------------------------------------|
| search     | string   | Search in title or content.                         |
| author     | string   | Filter by author.                                   |
| category   | string   | Filter by category.                                 |
| source     | string   | Filter by source.                                   |
| page       | integer  | Page number for pagination (default: 1).            |
| per_page   | integer  | Number of articles per page (default: 10).          |

### Request Example

```http
GET /api/articles?search=laravel&author=Jane&page=1
