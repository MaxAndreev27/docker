# Contributing

Thank you for considering a contribution to Time Application.

## Before You Start

- Search existing issues and pull requests before opening a new one.
- For security vulnerabilities, follow [the security policy](SECURITY.md)
  instead of opening a public issue.
- Keep changes focused and explain the user-facing or operational impact.

## Development Setup

The project contains a Vue frontend, a Node.js API, and a MySQL database.
Install dependencies in both application directories:

```sh
cd api && npm install
cd ../frontend && npm install
```

Use the Docker Compose files when you need the complete local stack:

```sh
docker compose up --build
```

## Making Changes

1. Create a focused branch from `main`.
2. Make the smallest change that solves the problem.
3. Update documentation when behavior or setup changes.
4. Run the relevant checks before opening a pull request:

```sh
cd api && npm start
cd frontend && npm run build
```

5. Open a pull request with a clear summary, testing notes, and any required
   configuration or database changes.

## Pull Requests

Pull requests should:

- Describe the problem and the proposed solution.
- Include steps to verify the change.
- Avoid unrelated formatting or refactoring.
- Include screenshots for visible frontend changes when useful.
- Call out breaking changes and deployment considerations.

Maintainers may ask for revisions before merging. Contributions are expected
to follow the [Code of Conduct](CODE_OF_CONDUCT.md).
