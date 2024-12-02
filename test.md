
# **News Aggregator API Documentation**

## **Project Overview**

The **News Aggregator API** is a backend system built with Laravel that aggregates news articles from multiple sources, processes them, and serves them to a frontend application via API endpoints. It provides filtering, searching, and live updates for news articles. 

### **Key Features**
- Aggregates news from **News API (newsapi.org)**, **The Guardian**, and **The New York Times**.
- Allows filtering by author, category, and source.
- Supports full-text search in article titles and content.
- Uses queues for asynchronous fetching and processing of news.
- Includes live updates using **Laravel Scheduler** and **Pusher**.
- Handles rate limits of API keys for developer accounts.

---

## **News Sources**
1. **News API (newsapi.org)**: A free API for breaking news and headlines across various categories.
2. **The Guardian**: Provides access to Guardian articles with filtering capabilities.
3. **The New York Times**: Offers news content with detailed metadata.

> **Note:** These APIs have request limits for free developer accounts. You may encounter rate limit errors during high usage.

---

## **API Endpoints**

### **1. Get Articles**
Retrieve a list of articles with optional filters and search parameters.

#### **Endpoint**
```
GET /api/articles
```

#### **Query Parameters**

| Parameter    | Type    | Required | Description                                                                     | Example                       |
|--------------|---------|----------|---------------------------------------------------------------------------------|-------------------------------|
| `search`     | String  | No       | Search for articles by title or content (case-insensitive).                     | `search=breaking news`        |
| `author`     | String  | No       | Filter articles by author name.                                                 | `author=John Doe`             |
| `category`   | String  | No       | Filter articles by category.                                                    | `category=Technology`         |
| `source`     | String  | No       | Filter articles by source.                                                      | `source=NYTimes`              |
| `page`       | Integer | No       | Specify the page number for paginated results. Defaults to 1.                   | `page=2`                      |
| `per_page`   | Integer | No       | Number of articles per page. Defaults to 10.                                    | `per_page=5`                  |

---

#### **Example Request**

```http
GET /api/articles?search=tech&author=Jane Doe&category=Technology&source=NYTimes&page=1
```

---

#### **Response Example**

**Status Code**: `200 OK`

```json
{
    "current_page": 1,
    "data": [
        {
            "id": 1,
            "title": "Breaking News: Laravel 11 Released",
            "content": "Laravel 11 brings significant changes to task scheduling...",
            "author": "Jane Doe",
            "category": "Technology",
            "source": "NYTimes",
            "date": "2024-12-01",
            "url": "https://nytimes.com/article"
        }
    ],
    "first_page_url": "http://example.com/api/articles?page=1",
    "from": 1,
    "last_page": 2,
    "last_page_url": "http://example.com/api/articles?page=2",
    "next_page_url": "http://example.com/api/articles?page=2",
    "path": "http://example.com/api/articles",
    "per_page": 10,
    "prev_page_url": null,
    "to": 10,
    "total": 15
}
```

#### **Error Responses**
- **400 Bad Request**: Invalid query parameters.
- **500 Internal Server Error**: Server-related issues.

---

## **Installation and Setup**

### **1. Clone the Repository**
```bash
git clone https://github.com/your-repository.git
cd your-repository
```

### **2. Install Dependencies**
```bash
composer install
npm install
```

### **3. Configure the Environment**
Copy the `.env.example` file to `.env` and update the necessary configurations:
- Database connection (`DB_` settings).
- API keys for `NEWS_API`, `GUARDIAN_API`, and `NY_TIMES_API`.

### **4. Run Migrations**
Set up the database schema:
```bash
php artisan migrate
```

### **5. Queue and Scheduler Setup**
#### **Queue Worker**
Start the queue worker to handle jobs for fetching and processing news:
```bash
php artisan queue:work
```

#### **Scheduler**
Ensure the Laravel scheduler is running to trigger periodic updates:
```bash
php artisan schedule:run
```
> **Note:** In production, set up a cron job for the scheduler:
```bash
* * * * * php /path-to-your-project/artisan schedule:run >> /dev/null 2>&1
```

### **6. Serve the Application**
Run the Laravel development server:
```bash
php artisan serve
```

---

## **Live Updates with Pusher**
- Live updates are pushed to the frontend using **Pusher**.
- Configure the `.env` file with your Pusher credentials:
  ```env
  PUSHER_APP_ID=your_app_id
  PUSHER_APP_KEY=your_app_key
  PUSHER_APP_SECRET=your_app_secret
  PUSHER_APP_CLUSTER=your_app_cluster
  ```
- Events are broadcasted when new articles are fetched.

---

## **Key Notes**
- **Rate Limits**: The free developer accounts for APIs used in this project are limited. Monitor usage carefully.
- **Extensibility**: Add more sources easily by implementing new service classes that adhere to the existing design pattern.
- **Testing**: Test the API with tools like Postman or Swagger.

---

## **Example Workflow**
1. The scheduler runs periodically to check for new articles.
2. If new articles are detected, jobs are queued to fetch and store them.
3. Frontend clients can call the `/api/articles` endpoint to retrieve and display the latest articles.
