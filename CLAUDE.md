# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Common Development Commands

### Development Setup
```bash
# Install dependencies for all workspaces
npm install

# Start development servers
npm run backend:dev  # Start backend with hot reload
npm run frontend:dev # Start frontend dev server (in separate terminal)

# Build production
npm run frontend     # Build all packages and client
npm run backend      # Start production backend
```

### Testing
```bash
# Run tests
npm run test:api     # Test API/backend
npm run test:client  # Test client/frontend
npm run e2e          # Run E2E tests locally
npm run e2e:headed   # Run E2E tests with browser visible

# Linting & Formatting
npm run lint         # Check code style
npm run lint:fix     # Fix linting issues
npm run format       # Format code with Prettier
```

### Database & User Management
```bash
# User management
npm run create-user      # Create new user
npm run reset-password   # Reset user password
npm run list-users       # List all users
npm run ban-user         # Ban a user
npm run delete-user      # Delete a user

# Balance management (for token usage)
npm run add-balance      # Add tokens to user balance
npm run set-balance      # Set user balance
npm run list-balances    # List all user balances
```

### Docker Development
```bash
npm run update:docker    # Update dependencies in Docker
npm run start:deployed   # Start with deploy-compose.yml
npm run stop:deployed    # Stop deployed services
```

## Architecture Overview

LibreChat is a monorepo with three main components:

### 1. API Backend (`/api`)
- **Express.js** server with MongoDB database
- **Entry**: `api/server/index.js`
- **Routes**: Modular route handlers in `api/server/routes/`
  - Auth, conversations, messages, AI endpoints, files, agents
- **Models**: MongoDB/Mongoose schemas in `api/models/`
- **Authentication**: Passport.js with JWT, social logins, LDAP support

### 2. Client Frontend (`/client`)
- **React 18** application with Vite
- **Entry**: `client/src/main.jsx`
- **State Management**: 
  - React Query for server state
  - Recoil for local UI state
- **Routing**: React Router with main chat at `/c/:conversationId`
- **Key Features**: Real-time streaming, file uploads, multi-language support

### 3. Shared Packages (`/packages`)
- **@librechat/data-provider**: API client and React Query hooks
- **@librechat/data-schemas**: Shared schemas and validation
- **@librechat/api**: Core utilities, encryption, MCP support

### Communication Flow
- Frontend uses React Query hooks to call backend APIs
- Authentication via JWT tokens in httpOnly cookies
- Real-time AI responses via Server-Sent Events (SSE)
- File uploads handled with multipart forms

### Key Patterns
1. **Modular Routes**: Each feature has dedicated route file
2. **Shared Types**: TypeScript definitions shared via packages
3. **Provider Pattern**: Extensive React Context usage
4. **Middleware Architecture**: Express middleware for auth, validation
5. **Database Abstraction**: Model methods encapsulate DB operations

### Configuration
- Environment variables for API keys and services
- MongoDB for data persistence
- Optional: Meilisearch for search, various AI providers
- Docker support with compose files