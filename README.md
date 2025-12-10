# CI-CD Demo

A Node.js application demonstrating a complete CI/CD pipeline with Docker and GitHub Actions.

## Project Structure

```
.
├── .github/workflows/    # GitHub Actions workflows
├── __tests__/            # Jest test files
├── src/                  # Source code
├── server.js             # Main application entry point
├── Dockerfile            # Docker image configuration
├── docker-compose.yml    # Docker Compose for local development
├── package.json          # NPM dependencies and scripts
├── jest.config.js        # Jest testing configuration
├── .eslintrc.json        # ESLint configuration
├── .gitignore            # Git ignore file
└── README.md             # This file
```

## Prerequisites

- Node.js 20+
- Docker and Docker Compose
- Git

## Local Development

### Without Docker

1. Install dependencies:
   ```bash
   npm install
   ```

2. Start the application:
   ```bash
   npm start
   ```

3. Run linter:
   ```bash
   npm run lint
   ```

4. Run tests:
   ```bash
   npm test
   ```

The application will be available at `http://localhost:3000`

### With Docker Compose

1. Start the application with Docker Compose:
   ```bash
   docker-compose up --build
   ```

2. The application will be available at `http://localhost:3000`

3. Stop the application:
   ```bash
   docker-compose down
   ```

## API Endpoints

- `GET /` - Welcome message
- `GET /api/status` - Application status
- `POST /api/echo` - Echo the request body
- `GET /health` - Health check endpoint

## NPM Scripts

| Script | Description |
|--------|-------------|
| `npm start` | Start the application |
| `npm run dev` | Start the application (development) |
| `npm test` | Run tests with coverage |
| `npm run test:watch` | Run tests in watch mode |
| `npm run lint` | Run ESLint |
| `npm run lint:fix` | Run ESLint and fix issues |

## Docker Build

Build the Docker image:

```bash
docker build -t ci-cd-demo:latest .
```

Run the Docker container:

```bash
docker run -p 3000:3000 ci-cd-demo:latest
```

## CI/CD Pipeline

The GitHub Actions workflow (`.github/workflows/ci.yml`) automatically:

1. Checks out the code
2. Sets up Node.js v20
3. Installs dependencies
4. Runs ESLint
5. Runs tests
6. Logs into GitHub Container Registry (GHCR)
7. Builds the Docker image
8. Pushes the image to GHCR

### Required GitHub Secrets

The workflow requires the following GitHub repository secrets:

- `GHCR_USERNAME` - Your GitHub username
- `GHCR_TOKEN` - GitHub Personal Access Token with `write:packages` scope

**Note:** The current workflow uses `secrets.GITHUB_TOKEN` (which is automatically available), so no additional secrets configuration is needed. However, if you want to use custom credentials, you can add the above secrets.

### Workflow Trigger

The workflow is triggered on every push to the `main` branch.

### Docker Image Tags

Images are pushed to GitHub Container Registry with the following tags:

- `ghcr.io/<username>/ci-cd-demo:latest` - Latest version
- `ghcr.io/<username>/ci-cd-demo:<commit-sha>` - Specific commit version

## Testing

The project uses Jest for testing. Tests are located in the `__tests__` directory.

Run tests:

```bash
npm test
```

Run tests in watch mode:

```bash
npm run test:watch
```

## Linting

The project uses ESLint for code quality.

Run linter:

```bash
npm run lint
```

Fix linting issues:

```bash
npm run lint:fix
```

## GitHub Container Registry (GHCR)

The built Docker image is automatically pushed to GitHub Container Registry.

### Pull the image:

```bash
docker pull ghcr.io/<your-github-username>/ci-cd-demo:latest
```

### Run the image:

```bash
docker run -p 3000:3000 ghcr.io/<your-github-username>/ci-cd-demo:latest
```

Replace `<your-github-username>` with your actual GitHub username.

## Environment Variables

- `NODE_ENV` - Application environment (`development`, `production`)
- `PORT` - Server port (default: 3000)

## License

MIT

## Getting Started with GitHub

1. Initialize a git repository (if not already done):
   ```bash
   git init
   git add .
   git commit -m "Initial commit: Add CI/CD pipeline"
   ```

2. Create a new repository on GitHub

3. Add the remote repository:
   ```bash
   git remote add origin https://github.com/<your-username>/ci-cd-demo.git
   git branch -M main
   git push -u origin main
   ```

4. Push to the main branch to trigger the CI/CD pipeline automatically

## Troubleshooting

### Docker builds fail
- Ensure you have Node.js 20 installed locally
- Clear Docker cache: `docker system prune -a`

### Tests fail
- Run `npm install` to ensure all dependencies are installed
- Check that all test files follow the naming convention (`*.test.js` or `*.spec.js`)

### Linting fails
- Run `npm run lint:fix` to automatically fix most issues

### GHCR push fails
- Verify that your GitHub Personal Access Token has `write:packages` scope
- Ensure you're logged in to GHCR with correct credentials
