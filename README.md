# Error Logging Service Architecture
Objectives:
- automatically reports errors, uncaught exceptions, and unhandled rejections as well as other types of errors depending on the platform
- granular error handling
- customization of functionality

## Entities and Relations
- Organization
- Project
- Log
- Organization otm (one to many) Project
- Project otm Log

## Business rules:
### Policies
- Free plan:
  - Retention: 14 d
  - Rate limit: 10/h, 100/d
- Pro plan:
  - Retention: 90 d
  - Rate limit: 1000/h, 10000/d  

## General architecture
- ( WEB Client, Dashboard ) -> RESTAPI K8s -> ( Mail Service, DB K8s )
- All projects use TypeScript and share dependencies via git submodules (monorepo is also possible, but I prefer factual isolation. Somebody says it is an antipattern, but for real they just don't get it. Everything is evil and everything is good from different perspectives).

## Client SDK
- NPM package.
- No extra dependencies, it is a wrapper for API calls via Fetch API with memoized preferences.
- Connection options.
- Automatic unhandled exception or rejections capturing.
- Customize error severity level.
- Manually capture error.
- Intergrations (extend automatic error handling behavior).

## API Backend
### Tech stack
- Nest (OOP paradigm, dependency injection, has a vast documentation)
- PostgreSQL (however error payload could possibly have a different shape, there are also a lot of static properties like `timestamp`, `severity level`, `source` etc.)\
  + TimescaleDB plugin for continuous aggregations, therefore for real-time processing.
  Partition logs table by project id
- Postmark SDK (rapid mailing service without marketing and other futile features)
- Redis for rate limits tracking, also possible strict tracking via Redlock (distributed locks).
- Cron for retention policy tracking job.
- Mocha/Chai (lightweight testing).

## Web Dashboard
### Tech stack
- React
- Redux (vast functionality and reach documentation, besides, it is just a standard)
- RTK, RTK Query (good architecture approach and useful tools integrated with Redux)
- Tanstack Table (headless, robust functionality, reach documentation)
- Recharts (robust functionality, reach documentation, highly popular, moderate customizability)
- Some style library like Material-ui or Tailwind if there is no custom style kit.
- Mocha/Chai (lightweight testing).

## DevOps
- Rely on GCP infrastructure (it is a matter of preference).
- GKE for RESTAPI and DB (cnpg operator).
- Cloud Store for React App Distribution.
- Had decided to implement SSR, change in favor of Firebase or Vercel would be needed.
