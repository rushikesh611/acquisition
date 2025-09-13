# Acquisition Application

This application is containerized and can be run both in development and production environments using Docker and Docker Compose. It uses Neon Database, with Neon Local for development and Neon Cloud for production.

## Prerequisites

- Docker
- Docker Compose
- Node.js 20+ (for local development outside Docker)
- Git

## Development Setup

The development environment uses Neon Local, which runs in a Docker container alongside your application.

1. Clone the repository:
   ```bash
   git clone https://github.com/rushikesh611/acquisition.git
   cd acquisition
   ```

2. Create your `.env.development` file:
   ```bash
   cp .env.development.example .env.development
   ```
   Adjust the values in `.env.development` if needed.

3. Start the development environment:
   ```bash
   docker compose -f docker-compose.dev.yml up --build
   ```

This will:
- Start Neon Local with an ephemeral branch for development
- Start your application in development mode with hot-reloading
- Connect your application to the local Neon database

The application will be available at http://localhost:3000.

### Development Database Access

- Host: localhost (or neon-local inside Docker network)
- Port: 5432
- Username: user
- Password: password
- Database: dbname

## Production Setup

For production, the application uses Neon Cloud Database.

1. Set up your production environment variables:
   ```bash
   cp .env.production.example .env.production
   ```

2. Edit `.env.production` and set your production values, particularly:
   - DATABASE_URL (from your Neon Cloud Dashboard)
   - JWT_SECRET
   - CORS_ORIGIN

3. Deploy using docker-compose:
   ```bash
   docker compose -f docker-compose.prod.yml up -d --build
   ```

## Environment Variables

### Development (.env.development)
- NODE_ENV=development
- PORT=3000
- DATABASE_URL=postgres://user:password@neon-local:5432/dbname
- JWT_SECRET=your-jwt-secret-for-development
- CORS_ORIGIN=http://localhost:3000

### Production (.env.production)
- NODE_ENV=production
- PORT=3000
- DATABASE_URL=postgres://your-production-url.neon.tech
- JWT_SECRET=your-production-jwt-secret
- CORS_ORIGIN=https://your-production-domain.com

## Database Migrations

To run database migrations:

Development:
```bash
docker compose -f docker-compose.dev.yml exec app npm run db:migrate
```

Production:
```bash
docker compose -f docker-compose.prod.yml exec app npm run db:migrate
```

## Troubleshooting

1. If Neon Local is not starting:
   - Check if port 5432 is already in use
   - Ensure Docker has enough resources allocated

2. If the application can't connect to the database:
   - In development: ensure Neon Local is running and healthy
   - In production: verify your Neon Cloud Database URL
   - Check network connectivity and firewall settings

## Security Notes

- Never commit `.env.development` or `.env.production` files
- Always use different secrets for development and production
- Regularly update dependencies and Docker images
- Monitor Neon Cloud console for database metrics and logs