# Error Logging Service Architecture
Basically the idea of the service is:
- automatically reports errors, uncaught exceptions, and unhandled rejections as well as other types of errors depending on the platform
- granular error handling
- customization of functionality
  
## Client SDK
SDK will consist of
  connection config, global error handlers config

## API Backend
Tech stack:
- API Framework: Nest (OOP paradigm, dependency injection, has a vast documentation, lots of techniques and integrations),
- Database: PostgreSQL (however error payload could possibly have a different shape, there are also a lot of static properties like `capture time`, `level`, `source` etc.)

## DevOps
- GCP infrastructure (provides different storage strategies, 20% cheaper than AWS).
- GKE for Backend scalability and monitoring
