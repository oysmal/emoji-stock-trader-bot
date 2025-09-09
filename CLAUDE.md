# Project: Kotlin Emoji Stock Exchange Trader Bot

## Project Description

A Kotlin-based automated trading bot. This project prioritizes clarity and simplicity over complex abstractions while ensuring reliable operation.
This entire application must be possible to build in 3 hours, so the focus should always be on simplicity and YAGNI.

## Use sub-agents

There are expert sub-agents available. Prefer using these where applicable.
When using sub-agents, see the section below on "Sub-Agent Workflow for Feature Development"

## Development Process Instructions (COD Loop and sub-agents)

### CRITICAL: Clarify-Offer-Decide (COD) Loop Requirements

**You MUST follow this development workflow for ALL coding tasks:**

1. **CLARIFY Phase**
   - Ask clarifying questions about requirements before suggesting any code
   - Identify ambiguities in the request
   - Confirm understanding of the desired outcome
   - Ask about constraints, performance requirements, or preferences
   - Never assume implementation details without confirmation

2. **OFFER Phase**
   - Present 2-3 different approaches with pros/cons
   - Explain trade-offs for each option (performance, maintainability, complexity)
   - Include brief code sketches or pseudocode when helpful
   - Highlight any architectural implications
   - Provide your recommendation given the requirements of the task at hand
   - **DO NOT implement full solutions at this stage**

3. **DECIDE Phase**
   - **STOP and WAIT for the developer to choose an approach**
   - Do not proceed with implementation until explicitly directed
   - Respect that the developer is in the driver's seat
   - After a decision is made, implement only what was chosen

After the user DECIDES, if there exists a plan document you are working on,
write the a brief note about the decision to this plan document.

**Example COD Flow:**

```
Developer: "Call the emoji stock exchange API to get 🚀 stock price"
Claude: [CLARIFY] "I need to understand the API call details:
- What's the base URL for the emoji stock exchange?
- Do we need authentication for price data?
- Should this be a one-time call or recurring?
- What format does the API return (JSON, XML)?"

Developer: "https://api.emoji-exchange.com, no auth needed, recurring every minute"
Claude: [OFFER] "Here are three approaches:
1. Simple HTTP client with coroutine timer...
2. Scheduled background job with error handling...
3. WebSocket connection for real-time updates...
[explains each with trade-offs]

I recommend 1. Simple HTTP client, because we need to keep it simple in this workshop,
and there is really no need for the additional complexity of the other solutions.

Which would you prefer?"

Developer: "Let's go with option 1"
Claude: [DECIDE -> IMPLEMENT] "Great! I'll implement update the implementation_plan.md and proceed with the HTTP client approach..."
```

### REQUIRED: Sub-Agent Workflow for Feature Development

**Follow this agent workflow for all feature development:**

**Important**: Always provide the user with information about what the sub-agent responded. If necessary, present everything verbatim.

1. **PLANNING**: Use `architecture-planner` agent to break down complex features into clear, manageable implementation steps
2. **IMPLEMENTATION**: Use appropriate Kotlin sub-agents:
   - `kotlin-service` - Business logic and services
   - `kotlin-ktor` - HTTP routes, API endpoints and configuration
   - `kotlin-db` - Database functionality, if needed
3. **REVIEW**: Use `kotlin-code-reviewer` agent to review all produced code for quality and best practices
4. **REFINEMENT**: If issues are found, pass back to appropriate `kotlin-*` agent to resolve
5. **VALIDATION**: Use `karen` agent to verify the plan was completed satisfactorily and all requirements met

**Example Workflow:**

```
Feature Request → architecture-planner → kotlin-service → kotlin-code-reviewer → (fixes if needed) → karen
```

**CRITICAL SUB-AGENT INTERACTION RULE**: <instructions>
**YOU ARE NEVER ALLOWED TO ANSWER SUB-AGENT QUESTIONS YOURSELF**

When a sub-agent is in CLARIFY or OFFER phase:

- **FORBIDDEN**: Answering questions based on your interpretation of requirements
- **FORBIDDEN**: Making implementation decisions on the user's behalf
- **FORBIDDEN**: Filtering or summarizing sub-agent proposals
- **REQUIRED**: Surface ALL sub-agent questions/proposals verbatim to the user
- **REQUIRED**: Wait for explicit user DECIDE response before proceeding

**This applies to ALL questions from sub-agents:**

- User requirement clarifications
- Implementation detail questions
- Technical approach decisions
- Architecture choices
- Any other sub-agent questions

**The user must make ALL decisions. You are only the messenger.**
</instructions>

**Important 2**: <instructions>Make sure you always provide clear instructions to the sub-agent regarding when to stop and consider its task complete.</instructions>

This ensures features are properly planned, implemented with Kotlin best practices, reviewed for quality, and validated for completeness.

## Tech Stack

### Core

- **Language**: Kotlin 2.4+
- **Build Tool**: Gradle with Kotlin DSL
- **Framework**: Ktor Client
- **Logging**: kotlin-logging + Logback

### Trading Infrastructure

- **HTTP Client**: Ktor Client
- **Serialization**: kotlinx.serialization
- **Date/Time**: kotlinx-datetime (critical for market hours)
- **Scheduling**: Coroutines with ticker channels

## Code Conventions

### Kotlin Style Guide

- Follow official Kotlin coding conventions
- Use `data class` for DTOs and value objects
- Prefer immutability: `val` over `var`
- Use sealed classes for representing restricted hierarchies
- Explicit return types for public functions
- Meaningful variable names, no abbreviations
- Max line length: 120 characters

### Naming Conventions

- **Classes**: PascalCase (e.g., `UserService`, `AuthController`)
- **Functions**: camelCase (e.g., `getUserById`, `validateToken`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `MAX_RETRY_COUNT`)
- **Packages**: lowercase (e.g., `com.example.api.routes`)

## Project Structure

```
/src
  /main
    /kotlin
      /no.kraftlauget.kiworkshop
        /services        # Functions etc. for business logic
        /models          # Data classes for domain entities and API models
        /utils           # Helper functions and utilities
        Application.kt   # Main application entry point
    /resources
      application.conf   # Ktor configuration
/gradle
/build.gradle.kts
```

## Architecture Principles

- **Keep it simple**: Direct logic flow without unnecessary layers
- **No unnecessary layers**: Avoid overcomplicated pattern unless truly needed
- **Colocate related code**: Keep route handlers close to their route definitions
- **Prefer functions over classes**: Use top-level or extension functions when possible
- **Avoid premature abstraction**: Start concrete, abstract only when patterns emerge
- **Simplicity with reliability**: Simple code can still be robust - don't confuse simple with fragile

## Important Implementation Notes

### Error Handling

- Return simple, consistent error responses
- Throw standard exceptions (IllegalArgumentException, etc.)
- Let exceptions bubble up rather than try-catch everywhere

## Environment Configuration

### Development Setup

```bash
# Required tools
- JDK 21+
- Gradle 8+

# Run locally
./gradlew run

# Check
./gradlew check

# Build JAR
./gradlew shadowJar
```

## Development Guidelines

- Write straightforward, readable code
- Use coroutines for async operations
- Add logging for important operations
- When implementing concurrent features, keep them simple but correct
- Simple header-based authentication
- Fail fast with clear error messages

## Absolute forbidden behavior

- You will never skip the COD Loop
- You will never deviate from the plan document
- You will never implement more than you have been strictly asked
- You will never move on before a task is complete
- You will never leave TODO's and skip functionality the user asked you to implement
