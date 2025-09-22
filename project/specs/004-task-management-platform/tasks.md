# Tasks: Task Management Platform 集中管理平台

**Input**: Design documents from `/specs/004-task-management-platform/`
**Prerequisites**: plan.md, research.md, data-model.md, contracts/openapi.yaml
**Tech Stack**: ASP.NET Core 9, Reqnroll (BDD), Entity Framework Core InMemory

## Execution Flow (main)
```
1. Load plan.md from feature directory ✅
   → Tech stack: ASP.NET Core 9, Entity Framework Core InMemory
   → Structure: Web application (backend API)
2. Load design documents ✅
   → data-model.md: 6 entities (Project, Task, User, ProjectMember, TaskComment, Notification)
   → contracts/openapi.yaml: 15 API endpoints across 5 controllers
   → research.md: Clean Architecture + BDD decisions
3. Generate tasks by category ✅
   → Setup: .NET 9 project, Clean Architecture structure
   → Tests: Reqnroll BDD specs, contract tests
   → Core: Domain entities, application services
   → Integration: API controllers, database, authentication
   → Polish: performance, logging, documentation
4. Apply task rules ✅
   → Different files marked [P] for parallel execution
   → BDD tests before implementation (TDD)
5. Number tasks sequentially (T001-T045) ✅
6. Generate dependency graph ✅
7. Validation complete ✅
```

## Format: `[ID] [P?] Description`
- **[P]**: Can run in parallel (different files, no dependencies)
- Paths assume Clean Architecture structure in backend/

## Phase 3.1: Project Setup
- [ ] T001 Create ASP.NET Core 9 solution with Clean Architecture structure (backend/src/, backend/tests/)
- [ ] T002 Configure project dependencies: Entity Framework Core, Reqnroll, Serilog, MediatR
- [ ] T003 [P] Setup EditorConfig and code formatting rules
- [ ] T004 [P] Configure InMemory database for development and testing
- [ ] T005 [P] Setup CI/CD pipeline configuration files

## Phase 3.2: BDD Specifications (Reqnroll) ⚠️ MUST COMPLETE BEFORE 3.3
**CRITICAL: These BDD specs MUST be written and MUST FAIL before ANY implementation**

### Authentication Scenarios
- [ ] T006 [P] BDD spec: User login flow in backend/tests/Specs/Authentication/UserLogin.feature
- [ ] T007 [P] BDD spec: Token refresh flow in backend/tests/Specs/Authentication/TokenRefresh.feature

### Project Management Scenarios
- [ ] T008 [P] BDD spec: Create and manage projects in backend/tests/Specs/Projects/ProjectManagement.feature
- [ ] T009 [P] BDD spec: Project member management in backend/tests/Specs/Projects/ProjectMembers.feature

### Task Management Scenarios
- [ ] T010 [P] BDD spec: Task lifecycle management in backend/tests/Specs/Tasks/TaskLifecycle.feature
- [ ] T011 [P] BDD spec: Task assignment workflow in backend/tests/Specs/Tasks/TaskAssignment.feature
- [ ] T012 [P] BDD spec: Task status transitions in backend/tests/Specs/Tasks/TaskStatusTransitions.feature

### Notification Scenarios
- [ ] T013 [P] BDD spec: Notification delivery in backend/tests/Specs/Notifications/NotificationDelivery.feature

### API Contract Tests
- [ ] T014 [P] Contract test: Authentication endpoints in backend/tests/Contract/AuthenticationContractTests.cs
- [ ] T015 [P] Contract test: Projects endpoints in backend/tests/Contract/ProjectsContractTests.cs
- [ ] T016 [P] Contract test: Tasks endpoints in backend/tests/Contract/TasksContractTests.cs
- [ ] T017 [P] Contract test: Users endpoints in backend/tests/Contract/UsersContractTests.cs
- [ ] T018 [P] Contract test: Notifications endpoints in backend/tests/Contract/NotificationsContractTests.cs

## Phase 3.3: Domain Layer (ONLY after BDD specs are failing)

### Domain Entities
- [ ] T019 [P] Project entity with business rules in backend/src/Domain/Entities/Project.cs
- [ ] T020 [P] Task entity with workflow logic in backend/src/Domain/Entities/Task.cs
- [ ] T021 [P] User entity with role management in backend/src/Domain/Entities/User.cs
- [ ] T022 [P] ProjectMember entity in backend/src/Domain/Entities/ProjectMember.cs
- [ ] T023 [P] TaskComment entity in backend/src/Domain/Entities/TaskComment.cs
- [ ] T024 [P] Notification entity in backend/src/Domain/Entities/Notification.cs

### Value Objects and Enums
- [ ] T025 [P] Domain enums (ProjectStatus, TaskStatus, etc.) in backend/src/Domain/Enums/
- [ ] T026 [P] Value objects (TaskWorkflow, NotificationSettings) in backend/src/Domain/ValueObjects/

### Domain Services
- [ ] T027 [P] Task workflow domain service in backend/src/Domain/Services/TaskWorkflowService.cs
- [ ] T028 [P] Project authorization domain service in backend/src/Domain/Services/ProjectAuthorizationService.cs

## Phase 3.4: Application Layer

### Command/Query Handlers (CQRS)
- [ ] T029 [P] Project commands/queries with MediatR in backend/src/Application/Projects/
- [ ] T030 [P] Task commands/queries with MediatR in backend/src/Application/Tasks/
- [ ] T031 [P] User commands/queries with MediatR in backend/src/Application/Users/
- [ ] T032 [P] Notification commands/queries in backend/src/Application/Notifications/

### Application Services
- [ ] T033 Authentication service with JWT in backend/src/Application/Services/AuthenticationService.cs
- [ ] T034 Notification service with SignalR in backend/src/Application/Services/NotificationService.cs

## Phase 3.5: Infrastructure Layer

### Database Configuration
- [ ] T035 Entity Framework Core DbContext in backend/src/Infrastructure/Data/TaskManagementDbContext.cs
- [ ] T036 [P] Entity configurations in backend/src/Infrastructure/Data/Configurations/
- [ ] T037 InMemory database seed data in backend/src/Infrastructure/Data/SeedData/

### Repository Implementation
- [ ] T038 [P] Generic repository pattern in backend/src/Infrastructure/Repositories/BaseRepository.cs
- [ ] T039 [P] Specific repository implementations in backend/src/Infrastructure/Repositories/

## Phase 3.6: API Layer (Controllers)

### API Controllers
- [ ] T040 Authentication controller (login, refresh) in backend/src/API/Controllers/AuthController.cs
- [ ] T041 Projects controller (CRUD operations) in backend/src/API/Controllers/ProjectsController.cs
- [ ] T042 Tasks controller (CRUD, status updates) in backend/src/API/Controllers/TasksController.cs
- [ ] T043 Users controller (profile management) in backend/src/API/Controllers/UsersController.cs
- [ ] T044 Notifications controller in backend/src/API/Controllers/NotificationsController.cs

### API Infrastructure
- [ ] T045 Program.cs configuration with dependency injection in backend/src/API/Program.cs
- [ ] T046 [P] Middleware (authentication, logging, CORS) in backend/src/API/Middleware/
- [ ] T047 [P] API response models (DTOs) in backend/src/API/Models/

## Phase 3.7: Integration & Polish

### Performance & Monitoring
- [ ] T048 [P] Structured logging with Serilog configuration
- [ ] T049 [P] Performance monitoring and health checks
- [ ] T050 [P] Caching strategy implementation (Memory + Redis preparation)

### Security & Validation
- [ ] T051 Input validation and sanitization
- [ ] T052 [P] Authorization policies and role-based access control
- [ ] T053 [P] API rate limiting and security headers

### Documentation & Testing
- [ ] T054 [P] Update OpenAPI documentation with real endpoints
- [ ] T055 [P] Performance tests (sub-200ms response time validation)
- [ ] T056 Integration test with InMemory database
- [ ] T057 [P] API documentation and deployment guide

## Dependencies
```
Setup (T001-T005) → BDD Specs (T006-T018) → Domain (T019-T028) → Application (T029-T034) → Infrastructure (T035-T039) → API (T040-T047) → Polish (T048-T057)

Key Blocking Dependencies:
- T002 blocks all other tasks (project must exist)
- T006-T018 MUST complete before T019+ (TDD requirement)
- T035 (DbContext) blocks T036-T039 (database tasks)
- T019-T028 (Domain) blocks T029+ (Application layer)
- T033 (Auth service) blocks T040, T052 (authentication)
- T045 (Program.cs) blocks T046-T047 (API infrastructure)
```

## Parallel Execution Examples

### Phase 3.2: Launch all BDD specs together
```bash
# Launch T006-T013 (BDD feature files) in parallel:
Task: "BDD spec: User login flow in backend/tests/Specs/Authentication/UserLogin.feature"
Task: "BDD spec: Token refresh flow in backend/tests/Specs/Authentication/TokenRefresh.feature"
Task: "BDD spec: Create and manage projects in backend/tests/Specs/Projects/ProjectManagement.feature"
Task: "BDD spec: Project member management in backend/tests/Specs/Projects/ProjectMembers.feature"
Task: "BDD spec: Task lifecycle management in backend/tests/Specs/Tasks/TaskLifecycle.feature"
Task: "BDD spec: Task assignment workflow in backend/tests/Specs/Tasks/TaskAssignment.feature"
Task: "BDD spec: Task status transitions in backend/tests/Specs/Tasks/TaskStatusTransitions.feature"
Task: "BDD spec: Notification delivery in backend/tests/Specs/Notifications/NotificationDelivery.feature"
```

### Phase 3.3: Launch all domain entities together
```bash
# Launch T019-T024 (Domain entities) in parallel:
Task: "Project entity with business rules in backend/src/Domain/Entities/Project.cs"
Task: "Task entity with workflow logic in backend/src/Domain/Entities/Task.cs"
Task: "User entity with role management in backend/src/Domain/Entities/User.cs"
Task: "ProjectMember entity in backend/src/Domain/Entities/ProjectMember.cs"
Task: "TaskComment entity in backend/src/Domain/Entities/TaskComment.cs"
Task: "Notification entity in backend/src/Domain/Entities/Notification.cs"
```

## BDD Test Scenarios (Reqnroll Examples)

### T006: User Login Flow
```gherkin
Scenario: Successful user login
  Given a user with email "user@example.com" and password "SecurePass123"
  When the user attempts to login
  Then the response should contain a valid JWT token
  And the token should expire in 1 hour
  And the user information should be included in the response
```

### T010: Task Lifecycle Management
```gherkin
Scenario: Task progression from creation to completion
  Given a project exists with id "project-123"
  And a user is a project member with "Contributor" role
  When the user creates a task "Implement user authentication"
  And assigns it to "developer@example.com"
  And the assignee updates status to "InProgress"
  And later updates status to "Completed"
  Then the task should have status "Completed"
  And completion timestamp should be recorded
  And project progress should be updated
```

## Performance Requirements Validation

Each task should validate these performance targets:
- 🎯 API response time < 200ms (95th percentile)
- 🎯 Database query time < 100ms
- 🎯 Support 1000+ concurrent users
- 🎯 Handle 1000 requests/second for task operations

## Notes
- **[P] tasks**: Can be executed in parallel (different files, no shared dependencies)
- **BDD-first approach**: All Reqnroll specs must be written and failing before implementation
- **Clean Architecture**: Maintain separation between Domain, Application, Infrastructure, and API layers
- **Commit strategy**: Commit after each task completion for better tracking
- **Testing strategy**: Contract tests validate API compliance, BDD specs validate business scenarios

## Task Generation Rules Applied

1. **From OpenAPI Contract**: 5 controllers → 5 contract test tasks + 5 controller implementation tasks
2. **From Data Model**: 6 entities → 6 entity creation tasks
3. **From BDD Scenarios**: 8 business flows → 8 Reqnroll feature files
4. **From Architecture**: Clean Architecture layers → Domain, Application, Infrastructure, API tasks
5. **Dependencies**: BDD tests → Domain → Application → Infrastructure → API → Polish

## Validation Checklist ✅

- [x] All OpenAPI endpoints have corresponding contract tests (T014-T018)
- [x] All domain entities have model tasks (T019-T024)
- [x] All BDD specs come before implementation (T006-T018 before T019+)
- [x] Parallel tasks are truly independent (different files, no shared state)
- [x] Each task specifies exact file path
- [x] No [P] task modifies same file as another [P] task
- [x] Performance requirements integrated into relevant tasks
- [x] Clean Architecture principles maintained throughout task sequence