<p align="center">
  <img alt="demo" src="./demos/demo.gif?v=1">
</p>

# ChatGPT UI

---

A web client for ChatGPT, using OpenAI's API.

## 📢Updates

---

Version 2 is a major update that separates the backend functionality as an independent project, hosted at [chatgpt-ui-server](https://github.com/WongSaang/chatgpt-ui-server). 

If you still wish to use the old version, please visit the [v1 branch](https://github.com/WongSaang/chatgpt-ui/tree/v1).

Version 2 introduces the following new features:

- 😉 Separation of the frontend and backend, with the backend now using the Python-based Django framework.
- 😘 User authentication, supporting multiple users.
- 😀 Ability to store data in an external database (defaulting to Sqlite).
- 😎 Session persistence, allowing the API to answer questions based on your context.


## Quick start with Docker Compose

---
### Run services

Below is a docker-compose.yml template:

```yaml
version: '3'
services:
  client:
    image: wongsaang/chatgpt-ui-client:latest
    environment:
      - SERVER_DOMAIN=http://backend:8000
    depends_on:
      - backend
    volumes:
      - backend_static:/app/static
    ports:
      - '80:80'
    networks:
      - chatgpt_ui_network
  backend:
    image: wongsaang/chatgpt-ui-server:latest
    environment:
      #      - DB_URL=postgres://postgres:postgrespw@localhost:49153/chatgpt # If this parameter is not set, the built-in Sqlite will be used by default. It should be noted that if you do not connect to an external database, the data will be lost after the container is destroyed.
      - DJANGO_SUPERUSER_USERNAME=admin # default superuser name
      - DJANGO_SUPERUSER_PASSWORD=password # default superuser password
      - DJANGO_SUPERUSER_EMAIL=admin@example.com # default superuser email
    volumes:
      - backend_static:/app/static
    ports:
      - '8000:8000'
    networks:
      - chatgpt_ui_network

networks:
  chatgpt_ui_network:
    driver: bridge

volumes:
  backend_static:
```

### After running

After running the services, you can access the web client at http://localhost, and an admin panel at http://localhost/admin.

Before you can start chatting, you need to log in to the admin panel to add an OpenAI API key. In the Settings model, add a record with the name "openai_api_key" and the value as your API key.


## Development

---

### Setup

Make sure to install the dependencies:

```bash
# yarn
yarn install
```

### Development Server

Start the development server on http://localhost:3000

```bash
yarn dev
```

### Production

Build the application for production:

```bash
yarn build
```