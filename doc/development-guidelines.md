# Development Guidelines

## Core Philosophy
Write code that is easy to read, easy to change, and easy to verify.
Every line should serve a clear purpose, express intent explicitly, and minimize cognitive load for future maintainers (including your future self).

## 1. Clean Architecture
Clean Architecture organizes your system into concentric layers: Domain (core business logic), Application (use cases), Interface Adapters (controllers, presenters), and Frameworks & Drivers (databases, UI, external services) - **_with dependencies pointing inward_**. It ensures your business rules are independent of frameworks, UIs, or databases, making the system testable, maintainable, and adaptable to change.

> _Avoid over-engineering simple CRUD apps or prototypes where architectural overhead outweighs benefits. Start lightweight and evolve toward Clean Architecture as complexity grows._

* Structure your code into clearly separated layers: Domain → Application → Infrastructure/UI. 
* Never let outer layers (e.g., database, web framework) dictate the design of your core domain. 
* Inject dependencies (e.g., repositories, services) into use cases; never instantiate them directly in domain code. 
* Keep business logic out of controllers, API routes, or data access code. 
* Ensure all tests for core logic can run without databases, networks, or UIs.

## 2. Clean Code
Clean Code is the practice of writing code that is easy to read, understand, and change even by someone unfamiliar with the codebase. It emphasizes clarity over cleverness, reducing cognitive load and bug risk.

> _Don’t sacrifice performance-critical optimizations (e.g., in embedded or HFT systems) purely for readability. Profile first, then balance._

* Use intention-revealing names for variables, functions, and classes (e.g., `calculateTotalPrice()` instead of `calc()`).
* Keep functions/methods short
  * Does one thing only.
  * Ideally less than 10 lines, but no more than 30 lines _(excluding comments and empty lines)_.
  * Avoid long parameters lists, more than 3 parameters is a code smell and requires strong justification.
* A class should contain an average of less than 30 methods, resulting in up to 900 lines of code.
* A package shouldn’t contain more than 30 classes, thus comprising up to 27,000 code lines.
* Subsystems with more than 30 packages should be avoided.
* A system with more than 30 subsystems should be avoided
* Avoid comments by making code self-explanatory; if you must comment, explain why, not what.
* Remove dead code, unused parameters, and redundant logic immediately.
* Format code consistently. Use linters and shared style guides.

## 3. Domain Driven Design (DDD)
DDD aligns software structure with business reality by modeling the core domain using a shared language (ubiquitous language), bounded contexts, entities, value objects, and aggregates. It prevents miscommunication and ensures the system reflects real-world rules.

> _Avoid DDD for simple domains (e.g., admin panels, basic forms) where the overhead of modeling outweighs clarity gains._

* Collaborate with domain experts to define a **ubiquitous language** - use it in code, tests, and documentation.
* Identify and isolate **bounded contexts**; avoid letting one context bleed into another. 
* Model **entities** (with identity) and **value objects** (immutable, identityless) explicitly.
* Enforce business invariants within **aggregates**. Only allow changes through the aggregate root.
* Place domain logic in domain objects and not in services or controllers.

## 4. SOLID Principles
SOLID is a set of five object-oriented design principles (Single Responsibility - SRP, Open/Closed - OCP, Liskov Substitution - LSP, Interface Segregation - ISP, Dependency Inversion - DIP) that promote flexible, maintainable, and testable code.

> _Don’t force abstractions prematurely (violating YAGNI) just to satisfy SOLID. Apply them when change or testing becomes painful._

## 5. Test-Driven Development (TDD)
TDD is a development cycle where you write a failing test first, then write minimal code to pass it, and finally refactor. It ensures correctness, drives modular design, and builds a safety net for refactoring.

> _Avoid strict TDD for exploratory coding, spikes, or UI-heavy work where test feedback is slow or unclear. Use it selectively for logic-heavy components._

* Write a failing unit test before writing any new functionality.
* Make the test pass with the simplest possible code (no extra logic).
* Refactor only after the test passes (keep behavior unchanged).
* Keep tests fast, isolated, and deterministic (no external dependencies).
* Use TDD primarily for domain logic, algorithms, and critical business rules.

## 6. Behavior-Driven Development (BDD)
BDD extends TDD by framing tests as user-centric behaviors using natural language (e.g., “Given-When-Then”). It bridges business and technical teams, ensuring features deliver real value.

> _Don’t use BDD for low-level unit tests or internal utilities. Reserve it for user-facing features and acceptance criteria._

* Write acceptance criteria as executable specifications using Gherkin syntax (Given/When/Then).
* Collaborate with product owners to define scenarios before coding begins.
* Automate BDD scenarios as end-to-end or integration tests (not unit tests).
* Use the ubiquitous language from DDD in your BDD scenarios.
* Keep scenarios focused on business outcomes (not implementation details).

## 7. KISS (Keep It Simple, Stupid)
KISS prioritizes simplicity in design and implementation. Simple code is easier to debug, test, and maintain and often more reliable than clever alternatives.

> _Don’t oversimplify when domain complexity genuinely requires nuanced modeling (e.g., financial regulations) - clarity ≠ naivety._

* Choose the simplest algorithm or data structure that solves the problem correctly.
* Avoid unnecessary design patterns, layers, or indirections.
* Prefer flat code structures over deeply nested ones.
* If a junior developer can’t understand your code in 5 minutes, simplify it.
* Favor readability and predictability over brevity or “elegance.”

> 💡 _These principles are guides, not dogma. Apply them thoughtfully based on context, team maturity, and system complexity. When in doubt, prioritize **clarity**, **testability**, and **business value**._

## Shell Scripts
* Use POSIX compliant shell syntax
* Follow [Google Shell Style Guide](https://google.github.io/styleguide/shell.xml)
