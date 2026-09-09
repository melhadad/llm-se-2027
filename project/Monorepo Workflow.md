When dealing with large-scale projects, organized in independent modules (front-end with many components, multiple micro-services), two approaches can be used to organize the Github repository of the project:
- Mono-repo: a single repository is used for the whole project with all its sub-components.
- Multi-repo: multiple Git repositories are created for each deployable unit (for example, one repo per micro-service).

For courseLLM, we will use a mono-repo approach.  This has some benefits and some costs.  This page describes the trade-off and the workflow to be applied to minimize the costs of using a mono-repo approach.

Our architecture combines a Next/React frontend with Python micro-service backend deployed within the Firebase ecosystem.

A **Monorepo** (Monolithic Repository) with this technology stack allows us to treat  infrastructure, frontend, and microservices as a single cohesive unit, rather than isolated silos.


### Part 1: Monorepo vs. Multi-repo

To understand the best approach, we must visualize how the code is stored and accessed.
#### The Multi-repo Approach

In a traditional **Multi-repo** setup, the Next.js app lives in one Git repository, and each Python micro-service in another, and perhaps the shared configurations and persistence models in a third.

- **Friction:** If the Python team changes an API endpoint, they merge their PR. The Frontend team might not know this happened until their app breaks in the staging environment.
    
- **Version Drift:** The Frontend might be expecting `v1.2` of the API, but the backend is running `v1.3`.
    
- **Context Switching:** Developers have to clone, install, and update multiple repositories to run the full stack locally.

#### The Monorepo Approach

In a **Monorepo**, the Next.js frontend and Python backend live side-by-side in distinct folders within the _same_ Git repository.

**Key Benefits for our Stack:**

1. **Atomic Changes:** You can submit a single Pull Request (PR) that updates the Python API endpoint _and_ the React component that consumes it simultaneously. The app does not break because the changes land together.
    
2. **Unified Firebase Context:** Since we are using Firebase Emulators, a monorepo allows a single root configuration (`firebase.json`) to spin up Firestore, Auth, and Cloud Run emulators that both the Python service and Next.js app connect to instantly.
    
3. **Shared Truth:** We keep our API definitions (e.g., OpenAPI specs or Data Connect schemas) in a shared folder. If the backend changes the schema, the frontend build can be configured to fail immediately, catching errors before deployment.
    
4. **Simplified CI/CD:** We have one pipeline. We don't need to coordinate complex webhooks between repositories to run integration tests.

| **Feature**         | **Multi-repo**                     | **Monorepo**                           |
| ------------------- | ---------------------------------- | -------------------------------------- |
| **Code Visibility** | Siloed (Team A sees Repo A)        | Transparent (Everyone sees everything) |
| **Dependency Mgmt** | Hard (requires package publishing) | Easy (direct file imports/workspaces)  |
| **Refactoring**     | Slow (requires coordination)       | Fast (atomic commits)                  |
| **Dev Environment** | Complex (multiple terminals/repos) | Unified (one command startup)          |

### Part 2: Organizational Structure

For a mixed-language monorepo (TypeScript/JavaScript + Python), structure is critical. We want to separate concerns while allowing integration.

**Recommended Directory Tree:**



```
ClassLLM-monorepo/
├── firebase.json              # Config for Emulators, Firestore indexes, Hosting
├── firestore.rules            # Security rules
├── dataconnect.yaml           # Firebase Data Connect config
├── package.json               # Root scripts (Turborepo/Nx)
├── pnpm-workspace.yaml        # JS Workspace config
├── .github/                   # CI/CD workflows (CODEOWNERS lives here)
│
├── apps/
│   ├── web/                   # Next.js Application
│   │   ├── package.json
│   │   └── src/...
│
├── services/
│   ├── api/                   # Python Microservice (Cloud Run)
│   │   ├── Dockerfile
│   │   ├── main.py
│   │   └── requirements.txt
│
└── packages/                  # Shared JS/TS Libraries
    ├── ui/                    # Shared React UI components
    ├── ts-types/              # Shared Types (generated from API specs)
    └── configs/               # Shared ESLint/TSConfig
```

- **`apps/`**: Contains deployable frontend units.
    
- **`services/`**: Contains deployable backend containerized services.
    
- **`packages/`**: Contains internal logic shared between frontend apps (or config files). _Note: You cannot directly import Python code into JS, but you can share config or generated types here._


### Part 3: Practical Workflow & Synchronization

Managing multiple teams (Frontend Team vs. Backend Team) in one repo requires specific tooling and rules.

#### 1. Tooling: The "Build System"

Since we have a mixed language stack, we need a tool that understands the dependency graph. **Nx** or **Turborepo** are the industry standards here.

- **For our stack:** We will use **Nx**. Nx has good support for generic commands. We can configure Nx so that "building the python app" means running a docker build, while "building the web app" means `next build` (which works in TypeScript).
    
- **Smart Rebuilds:** If a developer only touches `apps/web`, the CI system (powered by Nx) will realize the Python code hasn't changed and _skip_ rebuilding/redeploying the Python container. This saves massive amounts of time.

#### 2. The "One Command" Local Dev

This is the superpower of the monorepo with Firebase. We can script a single command (e.g., `npm run dev`) that does the following:

1. **Starts Firebase Emulators:** Spins up Auth, Firestore, and the Emulator UI.
   
2. **Starts Python Service:** Runs the Python service locally (using `uvicorn`), mapped to the Emulator's function/run ports.
   
3. **Starts Next.js:** Runs `next dev`.

**Synchronization Benefit:** When a backend developer updates the Python logic, the frontend developer (running the same command) sees the change instantly without needing to pull a different repo or restart their environment.

#### 3. Defining Ownership (`CODEOWNERS`)

In a large project, we could use the CODEOWNERS configuration in GitHub to automatically map PR review requests to specific teams based on the folder layout of the monorepo.  This would prevent "too many cooks in the kitchen." This enforces that specific teams must review changes in specific directories before a merge.


```
# .github/CODEOWNERS

# Frontend team owns the web app and UI packages
/apps/web/ @my-org/frontend-team
/packages/ui/ @my-org/frontend-team

# Backend team owns the Python service and Data Connect schemas
/services/api/ @my-org/backend-team
/dataconnect.yaml @my-org/backend-team
/firestore.rules @my-org/backend-team

# Both teams must agree on shared types or contracts
/packages/ts-types/ @my-org/frontend-team @my-org/backend-team
```

We will not use this in our project and instead rely on discipline.

#### 4. The Integration Bridge: Data Connect & API Specs

Since we cannot import Python classes into React, we need a "Contract."

- **Approach:** Use the Python service to auto-generate an **OpenAPI (Swagger) spec** or use the **Firebase Data Connect** schema (GraphQL based).  We will use Data Connect for all services that expose data in a rich schema (for example, list of courses, teachers, students, student assessment results, student learning trajectories, conversation analytics) and OpenAPI for all the other services (chatbot service, file management).
    
- **Workflow:**
    
    1. Backend dev updates Python model.
        
    2. Backend dev runs a script: `npm run gen-types`.
        
    3. This script inspects the Python code/Data Connect schema and generates TypeScript `interfaces` into the `packages/ts-types` folder.
        
    4. Frontend dev immediately sees type errors in Next.js if the API contract was broken.
        

### Part 4: CI/CD Workflow (Google Cloud Build / GitHub Actions)

In a multi-team monorepo, your CI pipeline acts as the traffic controller.

1. **Pull Request Opened:**
    
    - Pipeline checks which files changed.
        
    - If `services/api` changed -> Run Python Linting & Unit Tests.
        
    - If `apps/web` changed -> Run React Tests & Linting.
        
    - **Crucially:** Run an _Integration Test_ suite where the Next.js app hits the Python service running in the Firebase Emulator inside the CI runner.
        
2. **Merge to Main:**
    
    - The pipeline detects changes.
        
    - **Frontend:** Builds Next.js and deploys to Firebase Hosting.
        
    - **Backend:** Builds the Python Docker container and deploys to Google Cloud Run.
        
    - **Firebase:** Deploys updated security rules or Data Connect schemas.

### Summary of Benefits for Your Team

- **Frontend Team:** No mocking APIs that don't match reality. Teams develop against a running local instance of the actual Python backend.
- **Backend Team:** They can see exactly how the frontend uses their data. If they want to deprecate a field, they can globally search the repo to see usage.
- **Project Manager:** Faster releases. No "waiting for the backend PR to merge" before the frontend PR can be merged.

### 5. NX Configuration for TS/Python

The `nx.json` file controls the "Task Graph" and Caching. Since we are mixing Python and Node.js, the most important part is defining **`namedInputs`**. This tells Nx exactly which file changes should trigger a rebuild for  each language.

**Location:** `nx.json` (Root)


```
{
  "$schema": "./node_modules/nx/schemas/nx-schema.json",
  "tasksRunnerOptions": {
    "default": {
      "runner": "nx/tasks-runners/default",
      "options": {
        "cacheableOperations": ["build", "test", "lint", "type-check", "docker-build"]
      }
    }
  },
  "targetDefaults": {
    "build": {
      "dependsOn": ["^build"],
      "inputs": ["production", "^production"],
      "cache": true
    },
    "docker-build": {
      "inputs": ["python", "sharedGlobals"],
      "cache": true
    },
    "test": {
      "inputs": ["default", "^production", "{workspaceRoot}/jest.config.ts"],
      "cache": true
    },
    "lint": {
      "inputs": ["default", "{workspaceRoot}/.eslintrc.json"],
      "cache": true
    }
  },
  "namedInputs": {
    "default": ["{projectRoot}/**/*", "sharedGlobals"],
    "production": [
      "default",
      "!{projectRoot}/**/?(*.)+(spec|test).[jt]s?(x)?(.snap)",
      "!{projectRoot}/tsconfig.spec.json",
      "!{projectRoot}/.eslintrc.json"
    ],
    "python": [
      "{projectRoot}/**/*.py",
      "{projectRoot}/requirements.txt",
      "{projectRoot}/Dockerfile"
    ],
    "sharedGlobals": [
      "{workspaceRoot}/babel.config.json",
      "{workspaceRoot}/firebase.json",
      "{workspaceRoot}/nx.json"
    ]
  }
}
```

#### 5.1. Connecting Python to Nx (`project.json`)

While the Next.js app will be auto-detected by Nx (via `package.json`), the Python service requires a manual configuration to "teach" Nx how to handle it. You create a `project.json` inside your Python folder.

**Location:** `services/api/project.json`

JSON

```
{
  "name": "api",
  "projectType": "application",
  "sourceRoot": "services/api",
  "targets": {
    "serve": {
      "executor": "nx:run-commands",
      "options": {
        "command": "uvicorn main:app --reload",
        "cwd": "services/api"
      }
    },
    "build": {
      "executor": "nx:run-commands",
      "options": {
        "command": "docker build -t my-api .",
        "cwd": "services/api"
      },
      "inputs": ["python"] 
    },
    "test": {
      "executor": "nx:run-commands",
      "options": {
        "command": "pytest",
        "cwd": "services/api"
      },
      "inputs": ["python"]
    }
  }
}
```

### Why this setup is good

1. **Smart Caching (The `inputs` magic):**
    
    - In `nx.json`, we defined a `python` input set.
        
    - In the `project.json`, the `build` target uses `"inputs": ["python"]`.
        
    - **Result:** If a developer changes a React component in `apps/web`, Nx knows that `services/api` (which only cares about `.py` files) **did not change**. It will skip the Docker build entirely in CI, saving you minutes of build time.
        
2. **Unified Commands:**
    
    - Developers don't need to remember `uvicorn` for python and `next dev` for web.
        
    - They just run `nx serve api` or `nx serve web`.
        
    - Or better yet, `nx run-many --target=serve --all` to start the whole stack.
