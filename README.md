# Recipe API — Django REST Framework

> **VTU External Project** | Production-Ready Recipe Management REST API

---

## 📌 Project Overview

A full-featured Recipe Management REST API built with **Django** and **Django REST Framework (DRF)**. This project demonstrates real-world backend development skills including user authentication, CRUD operations, image uploads, and advanced filtering.

---

## 🔧 Tech Stack

| Layer        | Technology                          |
|-------------|--------------------------------------|
| Framework   | Django 4.2, Django REST Framework    |
| Database    | PostgreSQL                           |
| Auth        | Token Authentication (DRF)           |
| Docs        | drf-spectacular (Swagger UI)         |
| Filtering   | django-filter                        |
| Images      | Pillow                               |
| Container   | Docker & Docker Compose              |

---

## 📁 Project Structure

```
recipe_api/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── requirements-dev.txt
└── app/
    ├── manage.py
    ├── app/
    │   ├── settings.py
    │   ├── urls.py
    │   └── wsgi.py
    ├── core/           # Custom User model, Recipe, Tag, Ingredient models
    │   ├── models.py
    │   ├── admin.py
    │   ├── tests.py
    │   ├── migrations/
    │   └── management/commands/wait_for_db.py
    ├── user/           # Registration, Login, Profile endpoints
    │   ├── serializers.py
    │   ├── views.py
    │   ├── urls.py
    │   └── tests.py
    └── recipe/         # Recipe, Tag, Ingredient endpoints
        ├── serializers.py
        ├── views.py
        ├── urls.py
        ├── tests.py
        └── tests_tags_ingredients.py
```

---

## 🚀 Getting Started

### Prerequisites
- Docker Desktop installed
- Git (optional)

### 1. Clone / Download the project
```bash
cd recipe_api
```

### 2. Build and Start Containers
```bash
docker compose up --build
```

This automatically:
- Waits for the PostgreSQL database to be ready
- Runs all migrations
- Starts the development server on `http://localhost:8000`

### 3. Create a Superuser (Optional)
```bash
docker compose run --rm app sh -c "python manage.py createsuperuser"
```

---

## 📖 API Documentation

Interactive Swagger docs available at:
```
http://localhost:8000/api/docs/
```

Django Admin available at:
```
http://localhost:8000/admin/
```

---

## 🔑 API Endpoints

### User Endpoints
| Method | Endpoint            | Description              | Auth Required |
|--------|---------------------|--------------------------|---------------|
| POST   | /api/user/create/   | Register a new user      | ❌            |
| POST   | /api/user/token/    | Get auth token (login)   | ❌            |
| GET    | /api/user/me/       | Get current user profile | ✅            |
| PUT    | /api/user/me/       | Update user profile      | ✅            |
| PATCH  | /api/user/me/       | Partial update profile   | ✅            |

### Recipe Endpoints
| Method | Endpoint                              | Description              | Auth Required |
|--------|---------------------------------------|--------------------------|---------------|
| GET    | /api/recipe/recipes/                  | List all recipes         | ✅            |
| POST   | /api/recipe/recipes/                  | Create a new recipe      | ✅            |
| GET    | /api/recipe/recipes/{id}/             | Get recipe details       | ✅            |
| PUT    | /api/recipe/recipes/{id}/             | Full update recipe       | ✅            |
| PATCH  | /api/recipe/recipes/{id}/             | Partial update recipe    | ✅            |
| DELETE | /api/recipe/recipes/{id}/             | Delete a recipe          | ✅            |
| POST   | /api/recipe/recipes/{id}/upload-image/| Upload recipe image      | ✅            |

### Tag Endpoints
| Method | Endpoint                   | Description        | Auth Required |
|--------|----------------------------|--------------------|---------------|
| GET    | /api/recipe/tags/          | List all tags      | ✅            |
| PATCH  | /api/recipe/tags/{id}/     | Update a tag       | ✅            |
| DELETE | /api/recipe/tags/{id}/     | Delete a tag       | ✅            |

### Ingredient Endpoints
| Method | Endpoint                         | Description            | Auth Required |
|--------|----------------------------------|------------------------|---------------|
| GET    | /api/recipe/ingredients/         | List all ingredients   | ✅            |
| PATCH  | /api/recipe/ingredients/{id}/    | Update an ingredient   | ✅            |
| DELETE | /api/recipe/ingredients/{id}/    | Delete an ingredient   | ✅            |

---

## 🔍 Filtering & Search

### Filter Recipes by Tags
```
GET /api/recipe/recipes/?tags=1,2
```

### Filter Recipes by Ingredients
```
GET /api/recipe/recipes/?ingredients=3,4
```

### Filter Tags/Ingredients Assigned to Recipes Only
```
GET /api/recipe/tags/?assigned_only=1
GET /api/recipe/ingredients/?assigned_only=1
```

---

## 🧪 Running Tests

```bash
docker compose run --rm app sh -c "python manage.py test"
```

Run tests with verbosity:
```bash
docker compose run --rm app sh -c "python manage.py test --verbosity=2"
```

Run linting:
```bash
docker compose run --rm app sh -c "flake8"
```

---

## 💡 Sample API Usage

### 1. Register a User
```bash
curl -X POST http://localhost:8000/api/user/create/ \
  -H "Content-Type: application/json" \
  -d '{"email": "chef@example.com", "password": "mypass123", "name": "Chef Kumar"}'
```

### 2. Get Auth Token
```bash
curl -X POST http://localhost:8000/api/user/token/ \
  -H "Content-Type: application/json" \
  -d '{"email": "chef@example.com", "password": "mypass123"}'
```

### 3. Create a Recipe
```bash
curl -X POST http://localhost:8000/api/recipe/recipes/ \
  -H "Authorization: Token YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Masala Dosa",
    "time_minutes": 30,
    "price": "3.50",
    "description": "Crispy South Indian crepe with spiced potato filling",
    "tags": [{"name": "South Indian"}, {"name": "Vegetarian"}],
    "ingredients": [{"name": "Rice"}, {"name": "Urad Dal"}, {"name": "Potato"}]
  }'
```

### 4. Upload Recipe Image
```bash
curl -X POST http://localhost:8000/api/recipe/recipes/1/upload-image/ \
  -H "Authorization: Token YOUR_TOKEN_HERE" \
  -F "image=@/path/to/image.jpg"
```

---

## 🗄️ Data Models

### User
- `email` (unique, used as username)
- `name`
- `is_active`, `is_staff`

### Recipe
- `title`, `description`
- `time_minutes`, `price`
- `link` (optional external link)
- `image` (optional, uploaded to `/vol/web/media/`)
- `tags` (ManyToMany)
- `ingredients` (ManyToMany)

### Tag
- `name`
- `user` (ForeignKey)

### Ingredient
- `name`
- `user` (ForeignKey)

---

## 🔐 Authentication

This API uses **Token Authentication**. Include the token in every authenticated request:

```
Authorization: Token <your-token-here>
```

---

## 👨‍💻 Key Features Demonstrated

- ✅ Custom User Model with email-based authentication
- ✅ Token-based authentication (register → login → token → use API)
- ✅ Full CRUD for Recipes, Tags, and Ingredients
- ✅ Nested serializers (tags/ingredients created inline with recipe)
- ✅ Image upload with UUID-based file naming
- ✅ Advanced filtering by tags and ingredients
- ✅ Auto-generated Swagger/OpenAPI documentation
- ✅ Django Admin customization
- ✅ Management command: `wait_for_db` (for Docker startup reliability)
- ✅ Comprehensive test suite (unit + integration tests)
- ✅ Production-ready Docker setup with PostgreSQL

---

*Developed for VTU External Examination*
