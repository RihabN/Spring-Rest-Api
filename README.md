# Spring Boot REST API - Article Management System

A complete RESTful API built with Spring Boot for managing Providers and Articles with a many-to-one relationship.

## Features

- **Provider Management** - Full CRUD operations for providers
- **Article Management** - Full CRUD operations for articles
- **Relationship Management** - Many-to-one relationship between Articles and Providers
- **Validation** - Input validation with Bean Validation
- **CORS Support** - Cross-Origin Resource Sharing enabled
- **MySQL Database** - Persistent data storage
- **Auto-generated Database** - Database created automatically if not exists

## Technologies Used

- Java 11
- Spring Boot 2.7.14
- Spring Data JPA
- MySQL 8.0
- Maven
- Bean Validation

## Prerequisites

- Java 11 or higher
- Maven 3.6+
- MySQL 8.0+
- Postman (for API testing)

## Database Setup

The application will automatically create the database `amsAPI` if it doesn't exist. Just make sure MySQL is running.

If you want to create it manually:
```sql
CREATE DATABASE amsAPI;
```

## Configuration

Update `src/main/resources/application.properties` with your MySQL credentials:

```properties
spring.datasource.username=root
spring.datasource.password=YOUR_PASSWORD
```

## Running the Application

1. **Clone/Extract the project**

2. **Build the project:**
```bash
mvn clean install
```

3. **Run the application:**
```bash
mvn spring-boot:run
```

4. **Access the application:**
- Homepage: http://localhost:8080
- API Base URL: http://localhost:8080

## Project Structure

```
spring-rest-api/
├── src/
│   ├── main/
│   │   ├── java/com/example/demo/
│   │   │   ├── controllers/
│   │   │   │   ├── ArticleController.java
│   │   │   │   └── ProviderController.java
│   │   │   ├── entities/
│   │   │   │   ├── Article.java
│   │   │   │   └── Provider.java
│   │   │   ├── repositories/
│   │   │   │   ├── ArticleRepository.java
│   │   │   │   └── ProviderRepository.java
│   │   │   └── SpringRestApiApplication.java
│   │   └── resources/
│   │       ├── static/
│   │       │   └── index.html
│   │       └── application.properties
│   └── test/
└── pom.xml
```

## API Endpoints

### Provider Endpoints

#### 1. Get All Providers
```http
GET http://localhost:8080/providers/list
```
**Response:**
```json
[
    {
        "id": 1,
        "name": "Provider Name",
        "address": "123 Main St",
        "email": "provider@email.com"
    }
]
```

#### 2. Get Provider by ID
```http
GET http://localhost:8080/providers/{providerId}
```
**Example:**
```http
GET http://localhost:8080/providers/1
```

#### 3. Create Provider
```http
POST http://localhost:8080/providers/add
Content-Type: application/json

{
    "name": "Tech Supplies Inc",
    "address": "456 Tech Avenue",
    "email": "contact@techsupplies.com"
}
```

#### 4. Update Provider
```http
PUT http://localhost:8080/providers/{providerId}
Content-Type: application/json

{
    "name": "Updated Provider Name",
    "address": "789 New Street",
    "email": "updated@email.com"
}
```
**Example:**
```http
PUT http://localhost:8080/providers/1
```

#### 5. Delete Provider
```http
DELETE http://localhost:8080/providers/{providerId}
```
**Example:**
```http
DELETE http://localhost:8080/providers/1
```

### Article Endpoints

#### 1. Get All Articles
```http
GET http://localhost:8080/articles/list
```
**Response:**
```json
[
    {
        "id": 1,
        "label": "Laptop",
        "price": 999.99,
        "picture": "laptop.jpg",
        "provider": {
            "id": 1,
            "name": "Tech Supplies Inc",
            "address": "456 Tech Avenue",
            "email": "contact@techsupplies.com"
        }
    }
]
```

#### 2. Create Article
```http
POST http://localhost:8080/articles/add/{providerId}
Content-Type: application/json

{
    "label": "Laptop",
    "price": 999.99,
    "picture": "laptop.jpg"
}
```
**Example:**
```http
POST http://localhost:8080/articles/add/1
Content-Type: application/json

{
    "label": "Laptop Dell XPS",
    "price": 1299.99,
    "picture": "dell-xps.jpg"
}
```

#### 3. Update Article
```http
PUT http://localhost:8080/articles/update/{providerId}/{articleId}
Content-Type: application/json

{
    "label": "Updated Laptop",
    "price": 1199.99,
    "picture": "updated-laptop.jpg"
}
```
**Example:**
```http
PUT http://localhost:8080/articles/update/1/1
Content-Type: application/json

{
    "label": "MacBook Pro",
    "price": 2499.99,
    "picture": "macbook-pro.jpg"
}
```

#### 4. Delete Article
```http
DELETE http://localhost:8080/articles/delete/{articleId}
```
**Example:**
```http
DELETE http://localhost:8080/articles/delete/1
```

## Testing with Postman

### Step-by-Step Testing Guide

1. **Create a Provider First**
   - Method: POST
   - URL: `http://localhost:8080/providers/add`
   - Body (raw JSON):
   ```json
   {
       "name": "Electronics World",
       "address": "123 Tech Street, Silicon Valley",
       "email": "info@electronicsworld.com"
   }
   ```
   - Note the `id` in the response (e.g., 1)

2. **List All Providers**
   - Method: GET
   - URL: `http://localhost:8080/providers/list`

3. **Create an Article for the Provider**
   - Method: POST
   - URL: `http://localhost:8080/articles/add/1` (use the provider ID)
   - Body (raw JSON):
   ```json
   {
       "label": "iPhone 15 Pro",
       "price": 1199.99,
       "picture": "iphone15pro.jpg"
   }
   ```

4. **List All Articles**
   - Method: GET
   - URL: `http://localhost:8080/articles/list`

5. **Update an Article**
   - Method: PUT
   - URL: `http://localhost:8080/articles/update/1/1` (providerId/articleId)
   - Body (raw JSON):
   ```json
   {
       "label": "iPhone 15 Pro Max",
       "price": 1399.99,
       "picture": "iphone15promax.jpg"
   }
   ```

6. **Delete an Article**
   - Method: DELETE
   - URL: `http://localhost:8080/articles/delete/1`

## Postman Collection

Import this collection into Postman for quick testing:

```json
{
    "info": {
        "name": "AMS API",
        "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
    },
    "item": [
        {
            "name": "Providers",
            "item": [
                {
                    "name": "Get All Providers",
                    "request": {
                        "method": "GET",
                        "url": "http://localhost:8080/providers/list"
                    }
                },
                {
                    "name": "Create Provider",
                    "request": {
                        "method": "POST",
                        "header": [{"key": "Content-Type", "value": "application/json"}],
                        "body": {
                            "mode": "raw",
                            "raw": "{\n    \"name\": \"Tech Supplies Inc\",\n    \"address\": \"456 Tech Avenue\",\n    \"email\": \"contact@techsupplies.com\"\n}"
                        },
                        "url": "http://localhost:8080/providers/add"
                    }
                }
            ]
        }
    ]
}
```

## Entity Relationships

```
Provider (1) -------- (*) Article
```

- One Provider can have multiple Articles
- Each Article must belong to one Provider
- Cascade delete: Deleting a Provider will delete all associated Articles

## Validation Rules

### Provider
- **name**: Required, cannot be blank
- **address**: Required, cannot be blank
- **email**: Required, cannot be blank

### Article
- **label**: Required, cannot be blank
- **price**: Optional, float value
- **picture**: Optional, string value
- **provider**: Required, must reference existing provider

## Database Tables

### provider
| Column  | Type         | Constraints    |
|---------|-------------|----------------|
| id      | BIGINT      | PRIMARY KEY    |
| name    | VARCHAR(255)| NOT NULL       |
| address | VARCHAR(255)| NOT NULL       |
| email   | VARCHAR(255)| NOT NULL       |

### article
| Column      | Type         | Constraints                    |
|-------------|-------------|--------------------------------|
| id          | BIGINT      | PRIMARY KEY                    |
| label       | VARCHAR(255)| NOT NULL                       |
| price       | FLOAT       |                                |
| picture     | VARCHAR(255)|                                |
| provider_id | BIGINT      | FOREIGN KEY → provider(id)     |

## Common Issues & Solutions

### 1. Port 8080 Already in Use
**Solution:** Change the port in `application.properties`:
```properties
server.port=8081
```

### 2. Database Connection Failed
**Solution:** 
- Verify MySQL is running
- Check credentials in `application.properties`
- Ensure MySQL allows connections from localhost

### 3. Cannot Create Article
**Solution:** 
- Ensure the provider exists first
- Verify you're using the correct provider ID in the URL

### 4. Validation Errors
**Solution:** 
- Check that all required fields are provided
- Ensure field names match exactly (case-sensitive)

## Development Tips

1. **Enable SQL Logging**: Already enabled in `application.properties` to see all SQL queries

2. **Test with Sample Data**: Create a few providers and articles to test relationships

3. **Use Postman Environment Variables**: Set base URL as a variable for easier testing

4. **CORS is Enabled**: The API can be accessed from any origin (good for development, configure properly for production)

## Next Steps / Enhancements

1. Add pagination for list endpoints
2. Implement search/filter functionality
3. Add authentication and authorization
4. Implement DTOs (Data Transfer Objects)
5. Add exception handling with custom error responses
6. Add Swagger/OpenAPI documentation
7. Implement unit and integration tests
8. Add file upload for pictures
9. Implement caching
10. Add audit fields (createdAt, updatedAt)

## Production Checklist

Before deploying to production:

- [ ] Configure proper CORS settings
- [ ] Change `ddl-auto` from `update` to `validate`
- [ ] Add proper exception handling
- [ ] Implement authentication/authorization
- [ ] Add request logging
- [ ] Configure production database
- [ ] Add health check endpoints
- [ ] Implement rate limiting
- [ ] Add API documentation
- [ ] Set up monitoring and alerts

## License

Educational project for learning Spring Boot REST APIs.

## Author

TP7 - REST Layer Tutorial
