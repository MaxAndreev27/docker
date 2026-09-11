# Time Application

A small full-stack application that displays the current time and lets users
save and delete time records in a MySQL database.

The project consists of a Vue.js frontend, a Node.js/Express API, MySQL, and
Adminer for database administration.

## Features

- Displays a live clock in the browser.
- Saves the current time to MySQL.
- Lists previously saved times.
- Deletes saved records.
- Runs as a complete stack with Docker Compose.

## Architecture

```text
Browser -> Vue frontend :3000 -> Express API :5555 -> MySQL :3306
																			^
																			|
																Adminer :8888
```

The frontend calls the API at `http://localhost:5555`. The API creates the
`times` table automatically when it connects to MySQL for the first time.

## Requirements

For the Docker workflow:

- Docker Engine
- Docker Compose v2 (`docker compose`)

For development without Docker:

- Node.js and npm
- A MySQL server configured with the environment variables described below

## Run with Docker Compose

### Build and run locally

This builds the frontend and API images from the local source code:

```sh
docker compose up --build
```

Open the application at <http://localhost:3000>.

The available services are:

| Service  | Address                                  | Purpose              |
| -------- | ---------------------------------------- | -------------------- |
| Frontend | <http://localhost:3000>                  | Vue application      |
| API      | <http://localhost:5555>                  | Express API          |
| Adminer  | <http://localhost:8888>                  | MySQL administration |
| MySQL    | `localhost:3306` from the Docker network | Database             |

Stop the stack with:

```sh
docker compose down
```

The database is stored in the `mysql_data` Docker volume. To remove the
database data as well, run `docker compose down -v`.

### Run published images

To run the images referenced by `docker-compose.pub.yml`:

```sh
docker compose -f docker-compose.pub.yml up -d
```

Open <http://localhost:3000> after the services start.

## Adminer

Open <http://localhost:8888> and use the following development credentials:

| Field    | Value      |
| -------- | ---------- |
| System   | `MySQL`    |
| Server   | `mysql`    |
| Username | `root`     |
| Password | `password` |
| Database | `time_db`  |

These credentials are intended for local development only. Change them before
using the project in a public or production environment.

## Run Without Docker

### API

Install dependencies and start the API in development mode:

```sh
cd api
npm install
npm run dev
```

The API listens on `http://localhost:5000` when run directly. Set the MySQL
connection variables before starting it if your database is not available at
the defaults:

```sh
export MYSQL_HOST=localhost
export MYSQL_PORT=3306
export MYSQL_USER=root
export MYSQL_PASSWORD=password
export MYSQL_DATABASE=time_db
```

### Frontend

In a second terminal:

```sh
cd frontend
npm install
npm run dev
```

Vite prints the development URL when it starts. The frontend still expects the
API at `http://localhost:5555`, so use the Docker Compose workflow or expose
the API on that port when running both parts locally.

To create a production frontend build:

```sh
npm run build
```

## API

The API runs on port `5000` inside the container and is exposed as port `5555`
by Docker Compose.

| Method   | Endpoint    | Description                    |
| -------- | ----------- | ------------------------------ |
| `GET`    | `/`         | API health message             |
| `GET`    | `/times`    | Returns saved time records     |
| `POST`   | `/times`    | Saves `{ "time": "HH:mm:ss" }` |
| `DELETE` | `/time/:id` | Deletes a saved record         |

Example request:

```sh
curl -X POST http://localhost:5555/times \
	-H 'Content-Type: application/json' \
	-d '{"time":"12:34:56"}'
```

## Project Structure

```text
.
├── api/                    # Express API and MySQL access
├── frontend/               # Vue.js application
├── docker-compose.yml      # Local builds and development stack
├── docker-compose.pub.yml  # Stack using published images
└── README.md
```

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.
Security issues should be reported according to [SECURITY.md](SECURITY.md).

## License

This project is distributed under the [ISC License](LICENSE).
