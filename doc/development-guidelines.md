# A Craftsman's Development Guidelines

## Core Philosophy
Software is not just code, it’s a living expression of business intent, human collaboration, and evolving understanding. These guidelines exist not to impose rigid rules, but to cultivate a shared mindset that balances clarity, adaptability, and responsibility across every layer of our systems.

We believe that great software emerges when:

* The domain is modeled with precision and respect for business reality (DDD),  
* The architecture protects core logic from volatility and external dependencies (Clean Architecture),  
* Design is guided by principles that promote change without fear (SOLID, Package Principles),  
* Behavior is defined collaboratively and verified relentlessly (BDD, TDD),  
* And every line of code is written with humility, simplicity, and care (Clean Code, DRY, YAGNI, KISS).

These practices are not dogma. Apply them thoughtfully favoring outcomes over ceremony, understanding over compliance, and value over vanity. When in doubt, ask: “Does this make the system easier to understand, safer to change, and more aligned with what our users truly need?”

That is the heart of sustainable software craftsmanship.

## Architecture and Modeling

### 1. Clean Architecture
Clean Architecture organizes your system into concentric layers: Domain (core business logic), Application (use cases), Interface Adapters (controllers, presenters), and Frameworks & Drivers (databases, UI, external services) - **_with dependencies pointing inward_**. It ensures your business rules are independent of frameworks, UIs, or databases, making the system testable, maintainable, and adaptable to change.

> _Avoid over-engineering simple CRUD apps or prototypes where architectural overhead outweighs benefits. Start lightweight and evolve toward Clean Architecture as complexity grows._

* Structure your code into clearly separated layers: Domain → Application → Infrastructure/UI. 
* Never let outer layers (e.g., database, web framework) dictate the design of your core domain. 
* Inject dependencies (e.g., repositories, services) into use cases; never instantiate them directly in domain code. 
* Keep business logic out of controllers, API routes, or data access code. 
* Ensure all tests for core logic can run without databases, networks, or UIs.

### 2. Domain Driven Design (DDD)
DDD aligns software structure with business reality by modeling the core domain using a shared language (ubiquitous language), bounded contexts, entities, value objects, and aggregates. It prevents miscommunication and ensures the system reflects real-world rules.

> _Avoid DDD for simple domains (e.g., admin panels, basic forms) where the overhead of modeling outweighs clarity gains._

* Collaborate with domain experts to define a **ubiquitous language** - use it in code, tests, and documentation.
* Identify and isolate **bounded contexts**; avoid letting one context bleed into another. 
* Model **entities** (with identity) and **value objects** (immutable, identityless) explicitly.
* Enforce business invariants within **aggregates**. Only allow changes through the aggregate root.
* Place domain logic in domain objects and not in services or controllers.

## Design and Modularity

### 1. SOLID Principles
SOLID is a set of five object-oriented design principles (Single Responsibility - SRP, Open/Closed - OCP, Liskov Substitution - LSP, Interface Segregation - ISP, Dependency Inversion - DIP) that promote flexible, maintainable, and testable code.

> _Don’t force abstractions prematurely (violating YAGNI) just to satisfy SOLID. Apply them when change or testing becomes painful._

* **Single Responsibility:** Give each class one reason to change - split large classes.
* **Open/Closed:** Extend behavior via new code (e.g., strategies, plugins), not by modifying existing code.
* **Liskov Substitution:** Ensure subclasses can replace parent classes without breaking behavior.
* **Interface Segregation:** Create small, role-specific interfaces - not monolithic ones.
* **Dependency Inversion:** Depend on abstractions (interfaces), not concrete implementations; inject dependencies.

### 2. Principles of Package Cohesion
These principles define what classes should belong together in the same package. They ensure packages are focused, reusable, and easy to understand by grouping classes that change for the same reasons or serve the same high-level purpose.

> _Avoid rigidly enforcing these in early-stage projects with unstable domains. Let the package structure emerge as the design stabilizes. Don’t create artificial packages just to satisfy the rules._

* **REP (Reuse/Release Equivalence Principle):** Group classes that form a reusable, versioned unit. Release them together.
* **CCP (Common Closure Principle):** Place classes that change for the same reason in the same package. Minimize the number of packages touched per change.
* **CRP (Common Reuse Principle):** Avoid forcing users to depend on classes they don’t need. Split packages if some classes are rarely used together.
* Favor CCP over CRP in application code
* Favor CRP over CCP in shared libraries.

  > 💡 _**CCP** favors stability (fewer packages changed per feature), while **CRP** favors reusability (smaller, focused packages)._

### 3. Principles of Package Coupling
These principles govern dependencies between packages to ensure the system remains buildable, testable, and evolvable without ripple effects. They prevent cyclic dependencies and enforce a stable, acyclic dependency graph.

> _In monolithic scripts or throwaway prototypes, strict package coupling rules may add unnecessary friction. Apply them when modularity matters._

* **ADP (Acyclic Dependencies Principle):** Never allow dependency cycles between packages. Refactor to break cycles (e.g., via interfaces or new packages).
* **SDP (Stable Dependencies Principle):** Depend in the direction of stability. Unstable packages should depend on stable ones, not vice versa.
* **SAP (Stable Abstractions Principle):** Stable packages should be abstract (easy to extend); unstable packages should be concrete. A package should be either abstract and stable or concrete and unstable - not both concrete and stable (rigid) or abstract and unstable (useless).

  > _💡 Measure stability: A package with many dependents but few dependencies is stable; one with few dependents but many dependencies is unstable. Use tools (e.g., phpmetrics) to detect violations._ 

## Process and Practices

### 1. Test-Driven Development (TDD)
TDD is a development cycle where you write a failing test first, then write minimal code to pass it, and finally refactor. It ensures correctness, drives modular design, and builds a safety net for refactoring.

> _Avoid strict TDD for exploratory coding, spikes, or UI-heavy work where test feedback is slow or unclear. Use it selectively for logic-heavy components._

* Write a failing unit test before writing any new functionality.
* Make the test pass with the simplest possible code (no extra logic).
* Refactor only after the test passes (keep behavior unchanged).
* Keep tests fast, isolated, and deterministic (no external dependencies).
* Use TDD primarily for domain logic, algorithms, and critical business rules.

### 2. Behavior-Driven Development (BDD)
BDD extends TDD by framing tests as user-centric behaviors using natural language (e.g., “Given-When-Then”). It bridges business and technical teams, ensuring features deliver real value.

> _Don’t use BDD for low-level unit tests or internal utilities. Reserve it for user-facing features and acceptance criteria._

* Write acceptance criteria as executable specifications using Gherkin syntax (Given/When/Then).
* Collaborate with product owners to define scenarios before coding begins.
* Automate BDD scenarios as end-to-end or integration tests (not unit tests).
* Use the ubiquitous language from DDD in your BDD scenarios.
* Keep scenarios focused on business outcomes (not implementation details).

## Code Craftsmanship

### 1. Clean Code
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

### 2. DRY (Don’t Repeat Yourself)
DRY states that every piece of knowledge or logic should have a single, unambiguous representation in your system. It reduces duplication, minimizes inconsistency, and makes changes easier-since you only need to update one place.

> _Don’t extract duplicated code prematurely if the similarity is coincidental or the contexts are likely to evolve independently. Forced reuse can create hidden coupling and violate YAGNI-duplication is often cheaper than the wrong abstraction._
* Identify true duplication (same logic, same intent), not just similar-looking code.
* Extract repeated logic into well-named functions, classes, or modules; but only when the abstraction clarifies intent.
* Avoid sharing code across different bounded contexts (from DDD). Duplicated concepts in separate domains may have divergent meanings.
* Prefer composition over inheritance. Sharing behavior-inheritance often spreads changes too widely.
* If removing duplication makes the code harder to understand or test, leave it duplicated and document why.

### 3. YAGNI (You Aren’t Gonna Need It)
YAGNI urges you to implement only what is needed now, not what you might need later. It prevents overengineering, reduces complexity, and accelerates delivery.

> _Don’t ignore clear, near-term requirements or architectural seams (e.g., plugin points) that are known to be needed soon._

* Implement features only when they are requested, not because they “might be useful.”
* Avoid building generic frameworks or abstractions without concrete use cases.
* Delete speculative code - even if it’s “just in case.”
* Postpone performance or scalability optimizations until metrics prove they’re needed.
* Favor concrete, specific solutions over flexible-but-complex ones.

### 4. KISS (Keep It Simple, Stupid)
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
