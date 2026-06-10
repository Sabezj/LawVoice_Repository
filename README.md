# LawVoice

LawVoice is a prototype real-time voice assistant for adolescent legal literacy. The project shows how a browser-based voice interface can be combined with backend-controlled security, dialog-state management, retrieval-augmented planning and operational monitoring.

The system is designed as an educational assistant, not as a substitute for professional legal advice. Its purpose is to help teenagers describe a difficult situation, identify important facts, understand possible rights and risks, and choose safe next steps.

## Main Use Cases

The prototype focuses on common legal-literacy scenarios:

* cyberbullying and online harassment;
* school-related conflicts;
* online purchases, returns and complaints;
* peer pressure and personal boundaries;
* contact with law enforcement or detention-related situations;
* urgent safety situations requiring escalation.

## Core Features

* Real-time browser voice interaction using WebRTC.
* Backend-based session creation for realtime model access.
* Temporary client credentials so that server API keys are not exposed to the browser.
* Hybrid intent routing for supported LawVoice scenarios.
* Scenario-aware dialog manager with risk level, missing facts and dialog stage.
* Retrieval-augmented knowledge search and evidence-bound action planning.
* PostgreSQL and pgvector support for knowledge documents and vector search.
* Cost and token usage tracking for realtime sessions.
* Health and readiness endpoints.
* Prometheus-compatible metrics endpoint.
* Grafana and Prometheus monitoring support.
* Admin interface for logs and diagnostic information.
* Automated tests for API routing, knowledge planning and dialog management.

## Architecture Overview

The project separates the realtime voice channel from domain-specific backend logic.

The browser handles microphone access, WebRTC connection management, audio visualization and user interaction. The backend handles session creation, authentication, API routing, database access, retrieval, action planning, logging and monitoring.

A simplified flow is:

1. The browser requests a temporary session credential from the backend.
2. The backend creates a realtime session using the protected server API key.
3. The browser establishes a WebRTC connection for audio exchange.
4. User transcripts are routed through intent classification and dialog-state logic.
5. If knowledge support is required, the backend searches indexed documents.
6. Retrieved evidence is passed to the action-planning module.
7. The response is generated under safety, evidence and cost constraints.
8. Runtime metrics are exposed for monitoring and diagnostics.

## Technology Stack

| Layer             | Technologies                                      |
| ----------------- | ------------------------------------------------- |
| Frontend          | JavaScript, WebRTC, Web Audio API, Chart.js       |
| Backend           | Node.js, Express                                  |
| Database          | PostgreSQL, pgvector, JSONB                       |
| Search            | Vector search with pgvector, full-text fallback   |
| Security          | Helmet, CORS, rate limiting, JWT, CSRF protection |
| Monitoring        | Prometheus, Grafana, Alertmanager                 |
| Testing           | Jest, Supertest                                   |
| Auxiliary service | FastAPI-based search service                      |

## Project Structure

```text
.
├── public/                  # Browser UI and frontend logic
├── profiles/                # LawVoice assistant profiles
├── scripts/                 # Setup and helper scripts
├── docs/                    # Project documentation
├── server.js                # Main Express backend
├── docker-compose.yml       # Application and monitoring services
├── package.json             # Node.js dependencies and scripts
└── README.md
```

## Monitoring

The project includes an observability layer for development, demonstration and prototype evaluation.

The backend exposes:

* `/metrics` for Prometheus-compatible metrics;
* `/api/health` for basic liveness checks;
* `/api/ready` for readiness checks.

The main metric groups include:

* HTTP request throughput and latency;
* knowledge search latency and hit counts;
* action-plan generation metrics;
* LLM and embedding request latency;
* component health;
* number of indexed knowledge documents and chunks.

Grafana dashboards and alert rules are used to inspect backend behavior, retrieval quality and action-planning latency during demonstration and testing.

## Admin Interface

The admin interface is intended for controlled diagnostics. Depending on configuration, it can provide access to:

* application logs;
* monitoring logs;
* session-level diagnostic records;
* observability links;
* internal debugging information.

Admin access should be protected and separated from the public user interface.

## API Overview

| Group            | Purpose                                                   |
| ---------------- | --------------------------------------------------------- |
| Realtime session | Create realtime sessions and exchange connection metadata |
| Intent routing   | Classify user requests into controlled intents            |
| Knowledge        | Ingest and search knowledge documents                     |
| Planning         | Generate grounded action plans from retrieved evidence    |
| Profiles         | Manage assistant profiles                                 |
| Analytics        | Store and inspect session metrics                         |
| Operations       | Expose monitoring, liveness and readiness checks          |
| Admin            | Provide diagnostics and log access                        |

## Testing

The automated tests cover:

* API integration behavior;
* LawVoice intent and scenario routing;
* knowledge-planning fallback behavior;
* filtering of invalid evidence identifiers;
* dialog-manager state transitions.

## Security Notes

The project uses several security controls:

* server-side storage of sensitive API keys;
* temporary client credentials;
* rate limiting;
* security headers;
* CORS configuration;
* JWT-based authentication;
* CSRF protection for selected pages;
* admin-only diagnostic surfaces.

Before real deployment, the following should be reviewed:

* environment variables and secrets;
* HTTPS configuration;
* admin access policy;
* database access rules;
* log retention policy;
* privacy and data-handling requirements for minors.

## Limitations

LawVoice is a prototype. It is not a production legal service and does not provide official legal advice.

Current limitations include:

* no completed user study with teenagers;
* no final legal-domain expert validation;
* limited adversarial testing;
* dependence on the quality of the knowledge base;
* prototype-level deployment and monitoring configuration.

Further work should include expert review, user testing, stronger privacy governance, adversarial evaluation and production hardening.
