# DisasterWatch - Community Disaster Reporting Platform

## Overview

DisasterWatch is a real-time, community-driven disaster reporting platform that enables citizens to quickly report emergencies and disasters while leveraging AI-powered severity assessment. The application combines interactive mapping with intelligent report categorization to help emergency responders prioritize and coordinate their efforts effectively.

The platform supports multiple disaster categories (floods, fires, medical emergencies, infrastructure damage, and missing persons) and uses Google's Gemini AI to automatically analyze report severity, detect spam, and suggest appropriate emergency response actions.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack:**
- React with TypeScript for type-safe component development
- Vite as the build tool and development server
- Wouter for lightweight client-side routing
- TanStack Query (React Query) for server state management and caching

**UI Framework:**
- Shadcn/ui component library built on Radix UI primitives
- Tailwind CSS with custom design system following emergency management interface patterns
- Inter font family for optimal readability under stress conditions
- Design optimized for speed of reporting and information clarity

**Key Design Decisions:**
- **Problem:** Emergency reporting requires immediate clarity and low cognitive load
- **Solution:** System-based design with emergency UI patterns inspired by crisis dashboards (Crisis24, FEMA)
- **Rationale:** Every design decision prioritizes speed of reporting and information clarity under high-stress conditions
- **Pros:** Fast user interaction, clear visual hierarchy, accessible under pressure
- **Cons:** May feel utilitarian compared to consumer applications

**Layout Strategy:**
- Dashboard uses two-column split on desktop (2/3 map, 1/3 report feed)
- Mobile-first responsive design with single-column stacking
- Full-width map with absolute positioned overlay controls
- Constrained content areas (max-w-7xl for dashboard, max-w-2xl for forms)

**State Management:**
- React Query handles all server data fetching, caching, and synchronization
- Local component state for UI interactions (filters, form inputs)
- No global state management library needed due to React Query's capabilities

### Backend Architecture

**Technology Stack:**
- Express.js for HTTP server and API routing
- TypeScript for type safety across the stack
- Drizzle ORM for database operations and schema management
- PostgreSQL (via Neon serverless) as the primary database

**API Design:**
- RESTful API with JSON payloads
- Route structure:
  - `GET /api/reports` - Fetch all disaster reports
  - `GET /api/reports/:id` - Fetch single report by ID
  - `POST /api/reports` - Create new report with AI analysis

**Storage Layer:**
- **Problem:** Need flexible storage during development with production-ready path
- **Solution:** Interface-based storage abstraction (`IStorage`) with in-memory implementation
- **Current Implementation:** `MemStorage` class for development with seeded demo data
- **Production Path:** Easy swap to database-backed implementation
- **Rationale:** Enables rapid prototyping while maintaining clean architecture

**Data Validation:**
- Zod schemas for runtime validation of all incoming data
- Drizzle-zod integration for automatic schema generation from database models
- Type-safe end-to-end from database to API responses

### Database Schema

**Reports Table:**
- Primary entity for disaster reports
- Fields: id (UUID), category (enum), title, description, location (text + lat/long coordinates)
- Severity tracking: `severityLevel` field (critical/high/medium/low/info)
- AI integration fields: `aiAnalysis` (text), `suggestedActions` (JSON array), `isSpam` (integer flag)
- Timestamp tracking with `reportedAt` field
- Optional image URL for photo evidence

**Users Table:**
- Basic authentication schema (currently unused in main flows)
- Prepared for future authentication features
- Fields: id (UUID), username (unique), password

**Design Decisions:**
- **Problem:** Need to store both structured and semi-structured AI analysis data
- **Solution:** JSON text fields for AI analysis and suggested actions
- **Alternatives Considered:** Separate tables for actions, normalized schema
- **Pros:** Flexible schema, easy to extend AI capabilities, simple queries
- **Cons:** Less queryable, potential data consistency issues

### External Dependencies

**AI Integration - Google Gemini:**
- Service: `@google/genai` SDK
- Purpose: Automated severity assessment, spam detection, and emergency response suggestions
- API Key: Configured via `GEMINI_API_KEY` environment variable
- Function: `analyzeDisasterReport()` processes category, title, description, and location
- Output: Structured JSON with severity level, spam flag, suggested actions, and analysis text

**Mapping - Leaflet.js:**
- Service: Leaflet mapping library
- Purpose: Interactive disaster location visualization
- CDN Integration: Loaded via HTML script tags
- Features: OpenStreetMap tile layer, custom severity-based markers, click interactions
- Color Coding: Severity-based marker colors (critical=red, high=orange, medium=yellow, low=blue, info=gray)

**Database - Neon PostgreSQL:**
- Service: Neon serverless PostgreSQL
- Driver: `@neondatabase/serverless` for edge-compatible connections
- Configuration: Connection via `DATABASE_URL` environment variable
- Migration Strategy: Drizzle Kit for schema migrations (`npm run db:push`)

**Session Management:**
- Package: `connect-pg-simple`
- Purpose: PostgreSQL-backed session store (prepared for authentication features)
- Current Status: Infrastructure ready, not actively used in current flows

**Development Tools:**
- Replit plugins for runtime error overlay, cartographer, and dev banner
- ESBuild for production bundling
- TypeScript compiler for type checking

**Date/Time Handling:**
- `date-fns` library for timestamp formatting and relative time display
- Used in report cards to show "X minutes ago" style timestamps