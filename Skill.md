---
name: dotnet-best-practices
description: 'Ensure .NET/C# code meets best practices for the solution/project.'
---

# .NET/C# Best Practices

Your task is to ensure the .NET/C# code in `${selection}` meets the best practicies and identify concrete changes required to improve correctness, maintainability, security, performance, testability, or consistency.

Do not introduce a new framework, library, architectural pattern, or naming convention unless it resolves a specific problem and is compatible with the existing solution.

## Documentation

- Add XML documentation (through Swagger) to public APIs when required by the repository, package, or documentation build
- Document behavior, parameters, return values, exceptions, side effects, and important constraints.
- Avoid comments that merely restate the code.
- Preserve existing public API compatibility unless a breaking change is explicitly allowed. Human wil confirm about this.

## Structure
- Choose classes, records, structs, record structs, and interfaces according to their semantics.
- Use records when value-based equality and immutable data-oriented behavior are appropriate.
- Use records for Data Transfer Objects but not for Domain Aggregates or Entities.


## Architecture and Project Structure

- Follow the existing solution architecture, dependency direction, namespace conventions, and folder structure.
- Keep domain or business logic independent from infrastructure concerns where the architecture requires that separation.
- Avoid introducing dependencies from lower-level components to higher-level or application-specific components.
- Place third-party integrations behind appropriate boundaries when isolation, replacement, or testing requires it.
- Use primary constructor syntax for dependency injection (e.g., `public class MyClass(IDependency dependency)`)
- Use interface segregation with clear naming conventions (prefix interfaces with 'I')
- Follow the Factory pattern for complex object creation.
- Custom rules:
- Use Clean Code Architecture with Apps, Core, Infrastructure, Tests folder structure.
- The folder Core includes Domain, Services, Consumers and Abstractions Projects. E.g. {Project}.{Domain}.Core.Domain. Service Project includes both Service Abstractions and Implementations.
- The folder Infrastructure includes all third party implementations such as {Project}.{Domain}.Infrastructure.MongoDb or {Project}.{Domain}.Infrastructure.ThirdParty

## Dependency Injection

- Use constructor dependency injection (avoid the null checks in the constructor)
- Register services with appropriate lifetimes (Singleton, Scoped, Transient). Favour Scoped lifetime.
- Create explicit scopes for background jobs, message consumers, and hosted services when scoped dependencies must be resolved. Use `AsyncScope` via `IServiceScopeFactory`.
- Custom rules:
- Kafka Consumers ending with Handler must implement and `AsyncScope` via `IServiceScopeFactory` to resolve the dependency.

## Resource Management & Localization

- Use ErrorCodes where necessary and ensure these are returned as a manifest.

## Async/Await Patterns

- Avoid blocking asynchronous operations with `.Result`, `.Wait()`, or `.GetAwaiter().GetResult()` in asynchronous application code.
- Return `Task`, `Task<T>`, `ValueTask`, or `ValueTask<T>` according to the API's actual requirements.
- Do not mark a method `async` when it does not await asynchronous work.
- Propagate `CancellationToken` through cancellable operations.
- Do not require `ConfigureAwait(false)` in application frameworks that manage execution context appropriately unless the repository has an explicit convention.
- Handle async exceptions properly

## Testing Standards

- Ensure that new code is thoroughly tested and enough tests are created.
- Use xUnit framework with FluentAssertions for assertions
- Follow AAA pattern (Arrange, Act, Assert)
- Use Moq for mocking dependencies
- Test observable behavior rather than implementation details.
- Test both success and failure scenarios
- Keep tests deterministic, isolated, and independent of execution order.
- Include null parameter validation tests. Do not require null-validation tests where null is impossible by contract or nullable analysis unless runtime validation is part of the API behavior.
- Tests naming convention should be <method>_<tested-scenario>_<expectedresult> example CreditFreeBetAsync_NegativeAmount_ReturnsError


## Configuration & Settings

- Use strongly-typed configuration classes with data annotations
- Implement validation attributes (Required, NotEmptyOrWhitespace)
- Use IConfiguration binding for settings
- Support appsettings.json configuration files

## Semantic Kernel & AI Integration

- Use Microsoft.SemanticKernel for AI operations
- Implement proper kernel configuration and service registration
- Handle AI model settings (ChatCompletion, Embedding, etc.)
- Use structured output patterns for reliable AI responses

## Error Handling & Logging

- Use structured logging with Microsoft.Extensions.Logging
- Use try-catch blocks for expected failure scenarios
- Use structured message templates rather than string interpolation for structured log values.
- Do not log credentials, access tokens, personal data, payment data, secrets, or other sensitive information.
- Use log levels consistently.
- Custom rules:
- Ensure that logs are with the format `for player {playerId} in {brand}

## Performance & Security

- Use C# 12+ features and .NET 8 optimizations where applicable
- Treat all external input as untrusted.
- Use parameterized commands or framework-supported query parameterization.
- Do not construct database, shell, LDAP, or similar commands through unsafe string concatenation.
- Apply authorization at the appropriate boundary.
- Avoid exposing sensitive data through responses, logs, exceptions, telemetry, or generated content.
- Follow secure coding practices for AI/ML operations

## Code Quality

- Ensure SOLID principles compliance
- Avoid code duplication through base classes and utilities
- Use meaningful names that reflect domain concepts
- Keep methods focused and cohesive
- Implement proper disposal patterns for resources

## Database
- Follow the persistence technology and repository patterns already used by the solution. If there isn't use Entity Framework
- Avoid N+1 queries and unnecessary round trips.
- Project only the fields required by the operation when practical.
- Use no-tracking queries for read-only Entity Framework operations when appropriate.
- Use bulk update or delete APIs when they preserve required domain behavior, auditing, concurrency handling, and interceptors.
