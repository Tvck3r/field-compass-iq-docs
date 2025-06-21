# Getting Started

This guide will help you set up the Field-Compass-IQ development environment and get all services running locally.

## Prerequisites

### Required Software
- **Git** - Version control
- **Docker Desktop** - Containerization platform
- **Node.js 18+** - JavaScript runtime
- **.NET 9 SDK** - Backend development
- **Visual Studio Code** - Recommended IDE

### Optional Software
- **Visual Studio 2022** - Alternative IDE for .NET development
- **Android Studio** - For mobile development
- **Xcode** - For iOS development (macOS only)
- **Postman** - API testing

## Repository Setup

### 1. Clone All Repositories

```bash
# Create a workspace directory
mkdir field-compass-iq-workspace
cd field-compass-iq-workspace

# Clone all repositories
git clone https://github.com/Tvck3r/field-compass-iq.git
git clone https://github.com/Tvck3r/field-compass-iq-api.git
git clone https://github.com/Tvck3r/field-compass-iq-web.git
git clone https://github.com/Tvck3r/field-compass-iq-mobile.git
git clone https://github.com/Tvck3r/field-compass-iq-docs.git
git clone https://github.com/Tvck3r/field-compass-iq-infra.git
```

### 2. Workspace Structure

Your workspace should look like this:
```
field-compass-iq-workspace/
├── field-compass-iq/          # Main project repository
├── field-compass-iq-api/      # Backend API
├── field-compass-iq-web/      # Web application
├── field-compass-iq-mobile/   # Mobile application
├── field-compass-iq-docs/     # Documentation
└── field-compass-iq-infra/    # Infrastructure
```

## Development Environment Setup

### 1. Backend API Setup

```bash
cd field-compass-iq-api

# Install .NET dependencies
dotnet restore

# Set up local database with Docker
docker-compose up -d postgres redis

# Run database migrations
dotnet ef database update -p src/FieldCompassIQ.Infrastructure -s src/FieldCompassIQ.Api

# Start the API
dotnet run --project src/FieldCompassIQ.Api
```

The API will be available at:
- HTTP: `http://localhost:5000`
- HTTPS: `https://localhost:5001`
- Swagger: `https://localhost:5001/swagger`

### 2. Web Application Setup

```bash
cd ../field-compass-iq-web

# Install dependencies
npm install

# Copy environment file
cp .env.example .env.local

# Start development server
npm run dev
```

The web app will be available at `http://localhost:3000`

### 3. Mobile Application Setup

```bash
cd ../field-compass-iq-mobile

# Install dependencies
npm install

# iOS setup (macOS only)
cd ios && pod install && cd ..

# Copy environment file
cp .env.example .env

# Start Metro bundler
npm start

# In another terminal, run on device/emulator
npm run android  # For Android
npm run ios      # For iOS (macOS only)
```

## Environment Configuration

### Backend API (.env)
```env
# Database
ConnectionStrings__DefaultConnection=Host=localhost;Database=fieldcompassiq;Username=postgres;Password=postgres
ConnectionStrings__RedisConnection=localhost:6379

# JWT
JwtSettings__SecretKey=your-super-secret-key-here
JwtSettings__Issuer=FieldCompassIQ
JwtSettings__Audience=FieldCompassIQ

# External Services
GoogleMaps__ApiKey=your-google-maps-api-key
SendGrid__ApiKey=your-sendgrid-api-key
Twilio__AccountSid=your-twilio-account-sid
Twilio__AuthToken=your-twilio-auth-token
```

### Web Application (.env.local)
```env
VITE_API_URL=http://localhost:5000
VITE_WS_URL=ws://localhost:5000/hubs
VITE_APP_NAME=Field-Compass-IQ
VITE_ENABLE_ANALYTICS=false
```

### Mobile Application (.env)
```env
API_URL=http://localhost:5000
WS_URL=ws://localhost:5000/hubs
MAPS_API_KEY=your-google-maps-api-key
PUSH_SENDER_ID=your-fcm-sender-id
DEV_MODE=true
```

## Database Setup

### 1. Using Docker (Recommended)

```bash
# Start PostgreSQL and Redis
cd field-compass-iq-api
docker-compose up -d postgres redis

# Verify containers are running
docker ps
```

### 2. Manual Setup

If you prefer manual database setup:

1. Install PostgreSQL 15+
2. Create database: `fieldcompassiq`
3. Install Redis 7+
4. Update connection strings in appsettings.json

### 3. Run Migrations

```bash
# Apply database migrations
dotnet ef database update -p src/FieldCompassIQ.Infrastructure -s src/FieldCompassIQ.Api

# Seed sample data (optional)
dotnet run --project src/FieldCompassIQ.Api -- --seed
```

## External Service Setup

### Google Maps API
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select existing
3. Enable Maps JavaScript API, Geocoding API, Directions API
4. Create credentials (API Key)
5. Add API key to environment variables

### SendGrid (Email)
1. Sign up at [SendGrid](https://sendgrid.com)
2. Create API key
3. Add to environment variables

### Twilio (SMS)
1. Sign up at [Twilio](https://twilio.com)
2. Get Account SID and Auth Token
3. Add to environment variables

## Development Workflow

### 1. Daily Development

```bash
# Start all services
./scripts/start-dev.sh  # If available, or manually start each service

# API (Terminal 1)
cd field-compass-iq-api
dotnet run --project src/FieldCompassIQ.Api

# Web (Terminal 2)
cd field-compass-iq-web
npm run dev

# Mobile (Terminal 3)
cd field-compass-iq-mobile
npm start
```

### 2. Testing

```bash
# Backend tests
cd field-compass-iq-api
dotnet test

# Frontend tests
cd field-compass-iq-web
npm test

# Mobile tests
cd field-compass-iq-mobile
npm test
```

### 3. Code Quality

```bash
# Backend linting
cd field-compass-iq-api
dotnet format

# Frontend linting
cd field-compass-iq-web
npm run lint:fix

# Mobile linting
cd field-compass-iq-mobile
npm run lint:fix
```

## IDE Configuration

### Visual Studio Code

Recommended extensions:
- C# (Microsoft)
- TypeScript and JavaScript Language Features
- React Native Tools
- GitLens
- Docker
- REST Client

### Workspace Settings

Create `.vscode/settings.json` in workspace root:
```json
{
  "typescript.preferences.importModuleSpecifier": "relative",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.exclude": {
    "**/node_modules": true,
    "**/bin": true,
    "**/obj": true
  }
}
```

## Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 5000, 5001, 3000 are available
2. **Database connection**: Verify PostgreSQL is running and accessible
3. **Node version**: Ensure Node.js 18+ is installed
4. **Docker issues**: Restart Docker Desktop if containers won't start

### Getting Help

1. Check the [troubleshooting guide](../operations/troubleshooting.md)
2. Review logs in each service
3. Create an issue in the main repository

## Next Steps

1. [Architecture Overview](../architecture/overview.md)
2. [API Documentation](../api/)
3. [Coding Standards](coding-standards.md)
4. [Testing Guidelines](testing.md)