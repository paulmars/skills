---
name: new-app-product-doc
description: Use when starting or standardizing a product with web and iOS applications.
---

# New App Product and Tech Spec

Use these defaults to create a product brief, set up a new project, or align an existing one. Add optional libraries only when a requirement needs them.

## Default stack

### Web

| Concern | Default |
| --- | --- |
| Runtime, package manager, scripts, and tests | Bun; do not use Node.js, npm, Yarn, or pnpm |
| Language | TypeScript with strict type checking |
| Framework | Next.js with the App Router |
| UI | React, Tailwind CSS, and shadcn/ui |
| Database | PostgreSQL |
| Database access and migrations | Drizzle ORM with the PostgreSQL driver |
| Authentication tokens | `jose` when token-based authentication is required |
| Realtime | `ws` when WebSockets are required |
| Object storage | AWS SDK S3 client for any S3-compatible provider |
| Image processing | Sharp when server-side image work is required |

Keep route handlers thin. Separate UI, workflows, domain rules, and database access.

### iOS

| Concern | Default |
| --- | --- |
| Language | Swift |
| UI | SwiftUI; use UIKit only for platform gaps |
| Local database | GRDB over SQLite; this is the iOS equivalent of using Drizzle |
| Concurrency | Swift structured concurrency with `async`/`await` and actors |
| Networking | `URLSession`, including its WebSocket support when needed |
| Credentials | Keychain through the Security framework |
| Dependencies | Swift Package Manager |
| Tests | XCTest |

Keep views, UI state, workflows, domain rules, and platform adapters separate.

## Product brief

Before setup or implementation, write a short brief with:

- Product, users, problem, and measurable outcome.
- Smallest complete first release and explicit non-goals.
- Platforms: web, iOS, or both.
- Data source of truth, local persistence, offline needs, retention, and deletion.
- Required integrations such as authentication, realtime, storage, notifications, analytics, or payments.
- Any stack deviation and its reason.
- One vertical slice through UI, workflow, persistence, and deployment.
- Assumptions and unresolved decisions.

Do not invent features to justify infrastructure. For an existing project, inspect it first, list deviations, and make the smallest changes needed.

## Testing expectations

- Unit-test domain rules, state transitions, policies, and local persistence.
- Integration-test APIs, authorization, PostgreSQL, migrations, transactions, and network boundaries.
- Verify both saved data and visible results when a feature affects both.
- Cover important rejection, retry, duplicate, and data-preservation cases.
- Keep tests deterministic and isolate each test client's identity and data.
- Run targeted tests locally; let CI run all groups. Any nonzero exit, forbidden warning, crash, or unexpected skip fails.

## CI expectations

Expose simple root commands for unit and integration tests. CI runs separate visible steps to:

1. Install locked dependencies with Bun in frozen mode.
2. Type-check and build the web application.
3. Build and run iOS unit tests.
4. Run web unit tests.
5. Run API integration tests with isolated PostgreSQL.
6. Run HTTP or iOS end-to-end tests when those boundaries exist.
7. Clean up owned processes and resources after success or failure.

The brief or setup plan must name the selected defaults, justified optional libraries, environment and deployment needs, validation commands, and uncovered boundaries. Pull requests report each command and outcome.
