# SOLID Principles - Course Book

## Table of Contents

- [Chapter 1: Introduction to SOLID and Design Rot](#chapter-1-introduction-to-solid-and-design-rot)
- [Chapter 2: Single Responsibility Principle (SRP)](#chapter-2-single-responsibility-principle-srp)
- [Chapter 3: Open/Closed Principle (OCP)](#chapter-3-openclosed-principle-ocp)
- [Chapter 4: Liskov Substitution Principle (LSP)](#chapter-4-liskov-substitution-principle-lsp)
- [Chapter 5: Interface Segregation Principle (ISP)](#chapter-5-interface-segregation-principle-isp)
- [Chapter 6: Dependency Inversion Principle (DIP), Dependency Injection (DI), and IoC](#chapter-6-dependency-inversion-principle-dip-dependency-injection-di-and-ioc)
- [Chapter 7: Other Essential Design Principles](#chapter-7-other-essential-design-principles)
- [Chapter 8: Course Conclusion and Interview Preparation](#chapter-8-course-conclusion-and-interview-preparation)
- [Book Glossary (Unified)](#book-glossary-unified)
- [Final Course Summary Note](#final-course-summary-note)

**Interview difficulty tags used throughout**

- **[Easy]**: definition-level or basic recognition
- **[Medium]**: requires reasoning, trade-offs, or light design
- **[Hard]**: scenario/design-level application or deeper nuance

## Chapter 1: Introduction to SOLID and Design Rot

**Section:** Introduction  
**Difficulty:** Beginner  
**Estimated Reading Time:** 18-22 minutes  
**Prerequisites:** Basic understanding of classes and objects in object-oriented programming

### 1) Learning Objectives

By the end of this chapter, you will be able to:

- Explain what SOLID stands for and why it matters.
- Describe how software design degrades over time ("design rot").
- Identify common symptoms of poor design in real projects.
- Connect SOLID principles to maintainability and change resilience.
- Evaluate whether a codebase is becoming harder to evolve.

### 2) Starter Context (Why This Chapter Matters)

Imagine your team gets a "small" feature request: just one field added to a form.  
It sounds simple. But after starting the task, you touch five classes, break two tests, and accidentally affect a billing flow. Suddenly, a one-hour change takes two days.

This is exactly the kind of problem SOLID was created to reduce. Good design is not about making code look fancy. It is about helping teams change software safely, quickly, and confidently.

### 3) Beginner Explanation

Think of software like a school bag.

- A well-designed bag has separate compartments: books in one place, lunch in another, water bottle in a side pocket.
- A badly designed bag has everything mixed together. To get one notebook, you pull out half the bag.

Code works the same way.

SOLID is a set of five design rules that help you organize code so:

- one change stays in one place,
- parts do not break each other,
- teams can add features without fear.

**What does SOLID mean?**

- **S** - Single Responsibility Principle (SRP)
- **O** - Open/Closed Principle (OCP)
- **L** - Liskov Substitution Principle (LSP)
- **I** - Interface Segregation Principle (ISP)
- **D** - Dependency Inversion Principle (DIP)

These principles were popularized through work by Robert C. Martin (Uncle Bob), with the SOLID acronym coined later by Michael Feathers.

At a simple level, SOLID helps answer this daily developer question:  
"How do I change this code without breaking everything?"

### 4) Technical Deep Dive

In long-lived systems, requirements and technologies keep changing. If architecture and coding practices are weak, repeated quick fixes introduce coupling, duplication, and unclear boundaries. Over time, this creates **design rot**.

**Design rot** is the gradual degradation of software structure caused by unmanaged changes. It often appears when:

- original design assumptions no longer hold,
- changes are rushed,
- maintainers do not understand existing design intent,
- teams prioritize short-term fixes over structural quality.

Robert C. Martin described recurring failure patterns in changing systems: code becomes rigid, fragile, and hard to reuse. SOLID principles were proposed as practical constraints to keep object-oriented designs:

- cohesive (each unit has clear purpose),
- loosely coupled (units depend less on implementation details),
- extendable (features added without risky edits),
- testable (behavior verified in isolation).

When SOLID is applied consistently, teams usually see:

- lower regression risk during feature updates,
- easier refactoring,
- improved onboarding for new developers,
- reduced long-term development and maintenance cost.

### 5) Key Concepts Breakdown

Key ideas from this chapter:

- **Change is constant:** software must evolve continuously.
- **Bad design amplifies change cost:** small requests become large risky edits.
- **Design rot is cumulative:** each poor shortcut adds friction.
- **SOLID is preventive:** it reduces future complexity, not just current bugs.
- **Quality is team responsibility:** maintainability is not optional work.
- **Code reviews should check design health:** not just syntax and style.

**Symptoms of poor design (design rot indicators):**

- **Rigidity:** one small change requires many dependent changes.
- **Fragility:** changing one area breaks unrelated areas.
- **Immobility:** hard to extract/reuse components.
- **Viscosity:** wrong/hacky solutions are easier than correct ones.
- **Needless complexity:** speculative structures for futures that never happen.
- **Needless repetition:** copy-paste duplication instead of abstraction.
- **Opacity:** code is hard to understand and reason about.

### 6) Code Examples

No direct code snippet was provided in this intro transcript, so here are two short examples to clarify design rot signals.

#### Example A: Rigidity + Fragility

```csharp
public class InvoiceService
{
    public void GenerateInvoice(Order order)
    {
        // business logic
        SaveToDatabase(order);
        SendEmail(order);
        LogToFile(order);
    }

    private void SaveToDatabase(Order order) { /* ... */ }
    private void SendEmail(Order order) { /* ... */ }
    private void LogToFile(Order order) { /* ... */ }
}
```

**What is wrong here?**

- One class mixes multiple responsibilities (business logic, persistence, notification, logging).
- A small email change may require touching invoice logic.
- Logging failure could affect invoice generation flow.

This increases rigidity and fragility.

#### Example B: Cleaner Separation (Intro-Level Refactor)

```csharp
public interface IInvoiceRepository { void Save(Order order); }
public interface INotifier { void Send(Order order); }
public interface ILogger { void Log(string message); }

public class InvoiceService
{
    private readonly IInvoiceRepository _repo;
    private readonly INotifier _notifier;
    private readonly ILogger _logger;

    public InvoiceService(IInvoiceRepository repo, INotifier notifier, ILogger logger)
    {
        _repo = repo;
        _notifier = notifier;
        _logger = logger;
    }

    public void GenerateInvoice(Order order)
    {
        _repo.Save(order);
        _notifier.Send(order);
        _logger.Log("Invoice generated");
    }
}
```

**Why better?**

- Responsibilities are split.
- Dependencies are abstracted.
- Future change (for example SMS instead of email) is localized.

### 7) Real-World Applications

You see these concepts in:

- enterprise applications (billing, HR, ERP) with frequent rule changes,
- product teams running rapid feature iterations,
- microservice-based systems requiring clean boundaries,
- code review standards in mature engineering organizations,
- legacy modernization efforts where rot symptoms guide refactoring.

### 8) Common Mistakes and Misconceptions

1. **Mistake:** "If code works, design is fine."  
   **Why:** short-term delivery pressure.  
   **Fix:** evaluate change effort and breakage risk, not only current correctness.

2. **Mistake:** overengineering for unknown future needs.  
   **Why:** fear of future rewrites.  
   **Fix:** design for likely change, not imagined complexity.

3. **Mistake:** copy-paste for speed.  
   **Why:** fastest immediate path.  
   **Fix:** extract shared behavior into reusable abstractions.

4. **Mistake:** treating refactoring as optional.  
   **Why:** considered "non-feature work."  
   **Fix:** include refactoring work in delivery estimates.

5. **Mistake:** adding hacks because clean change is too hard.  
   **Why:** high viscosity in codebase and tooling.  
   **Fix:** improve build speed, testability, and architecture seams.

6. **Misconception:** "SOLID is only for big systems."  
   **Clarification:** small projects benefit too; good habits scale better than bad ones.

### 9) Additional Insights (AI Enhancement)

- SOLID is best learned by refactoring existing code, not by memorizing definitions.
- You should apply principles as guardrails, not rigid laws.
- A design can be "good enough" today and still need revision tomorrow; this is normal.
- Team conventions and code reviews are just as important as individual skill.
- Technical debt is manageable when it is visible, intentional, and scheduled for cleanup.

### 10) Practice Ideas and Mini Projects

#### Beginner Exercises

1. **Symptom Hunt (Easy):**  
   Pick one existing class and identify which rot symptom it shows.  
   **Expected outcome:** one short report with symptom, proof, and one improvement.

2. **Responsibility Split (Easy-Medium):**  
   Refactor a class that does 3 or more jobs into focused parts.  
   **Expected outcome:** each class has one clear reason to change.

3. **Hack vs Clean Change (Medium):**  
   Solve a feature request in two ways: quick patch and clean design-preserving solution.  
   **Expected outcome:** compare short-term speed vs long-term maintainability.

#### Mini Projects

1. **Mini Project 1 - Refactor a Utility Module:**  
   Take a tightly coupled module and refactor it toward cleaner boundaries.

2. **Mini Project 2 - Design Health Checklist:**  
   Create a checklist (rigidity, fragility, etc.) and run it on a small project.

### 11) Interview Preparation (10 Questions with Model Answers)

1. **[Easy] Conceptual:** What does SOLID stand for?  
   **Answer:** It stands for Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion principles.

2. **[Easy] Conceptual:** Why were SOLID principles introduced?  
   **Answer:** To reduce design degradation over time and make software easier to maintain, extend, and test.

3. **[Easy] Conceptual:** What is design rot?  
   **Answer:** The gradual decline of software structure caused by unmanaged changes and quick fixes.

4. **[Medium] Conceptual:** Why is change central to software design?  
   **Answer:** Requirements evolve continuously, so design must absorb change without excessive breakage.

5. **[Medium] Practical:** How do you detect rigidity in a codebase?  
   **Answer:** If a small change requires updates in many unrelated files or modules, rigidity is likely high.

6. **[Medium] Practical:** Give one way to reduce fragility.  
   **Answer:** Separate responsibilities and use abstractions so local changes remain local.

7. **[Medium] Practical:** What is a sign of high viscosity?  
   **Answer:** Developers repeatedly choose hacks because the clean approach is too hard or slow.

8. **[Hard] Scenario-based:** Your team keeps copy-pasting similar logic to deliver fast. What do you do?  
   **Answer:** Deliver current needs, then schedule an abstraction refactor quickly to prevent duplication growth.

9. **[Hard] Scenario-based:** A "small" change unexpectedly touches many modules. What does this indicate?  
   **Answer:** Likely high coupling and rigid design; architecture review and refactoring are needed.

10. **[Hard] Scenario-based:** A new developer keeps breaking unrelated areas while making changes. Root cause?  
    **Answer:** Fragile design and weak boundaries; improve modularity, tests, and design documentation.

### 12) Quick Quiz (MCQ with Answers)

1. SOLID is primarily about:  
   A) UI design  
   B) Database tuning  
   C) Object-oriented design quality  
   D) Cloud deployment  
   **Answer:** C - It focuses on maintainable object-oriented design.

2. Which symptom means one change causes many dependent changes?  
   A) Opacity  
   B) Rigidity  
   C) Mobility  
   D) Simplicity  
   **Answer:** B - Rigidity.

3. Fragility means:  
   A) Code is hard to compile  
   B) Unrelated parts break after a change  
   C) Classes are too small  
   D) No unit tests exist  
   **Answer:** B - Fragility often shows as unrelated breakage.

4. High viscosity suggests:  
   A) Clean solutions are easier than hacks  
   B) Performance is excellent  
   C) Wrong solutions are easier than right ones  
   D) The code has no duplication  
   **Answer:** C - A key warning sign of unhealthy process and design.

5. Needless repetition is best reduced by:  
   A) More comments  
   B) Copy-paste templates  
   C) Abstraction and reuse  
   D) Bigger classes  
   **Answer:** C - Extract shared behavior into reusable components.

### 13) Glossary

_Note: For consistent definitions across the whole book, also see [Book Glossary (Unified)](#book-glossary-unified)._

- **SOLID:** Five principles for better object-oriented design.
- **SRP:** A class should have one reason to change.
- **OCP:** Software entities should be open for extension, closed for modification.
- **LSP:** Subtypes should be replaceable for base types without breaking behavior.
- **ISP:** Clients should not depend on interfaces they do not use.
- **DIP:** Depend on abstractions, not concrete implementations.
- **Design Rot:** Gradual design degradation over time.
- **Rigidity:** Difficulty of making changes.
- **Fragility:** Unrelated breakage after change.
- **Immobility:** Difficulty reusing components.
- **Viscosity:** Difficulty of doing the right design-preserving change.
- **Opacity:** Code that is hard to understand.
- **Refactoring:** Improving internal structure without changing external behavior.

### 14) Summary Notes

- SOLID is a practical response to real-world software change problems.
- Code quality should be measured by changeability, not only current correctness.
- Design rot appears gradually through repeated shortcuts.
- Rigidity, fragility, and opacity are early warning signals.
- Good design lowers long-term cost and increases team speed.
- Teams should enforce design quality in reviews, not only syntax checks.
- This chapter sets the foundation; upcoming chapters explain each SOLID principle deeply.

### 15) Bridge to Next Chapter

Now that you understand why software design degrades, the next chapter begins with the first and most foundational principle: **Single Responsibility Principle (SRP)**.  
You will learn how to design classes that are focused, predictable, and easier to change.

---

_End of Chapter 1_

---

## Chapter 2: Single Responsibility Principle (SRP)

**Section:** SRP  
**Difficulty:** Beginner  
**Estimated Reading Time:** 22-28 minutes  
**Prerequisites:** Chapter 1, basic OOP classes and methods

### 1) Learning Objectives

- Define SRP in practical terms.
- Identify "single reason to change" in real systems.
- Use decomposition, cohesion, and coupling to design better modules.
- Refactor a multi-purpose class into focused classes.
- Apply an SRP checklist in real projects.

### 2) Starter Context (Why This Chapter Matters)

A single `SaveEmployee()` method starts doing three things: save data, write logs, and produce reports.  
At first it works. Later, a logging change breaks saving. Finance asks for one update, operations breaks.  
This is exactly the problem SRP solves.

### 3) Beginner Explanation

SRP means: **one component, one job**.

If one class does many unrelated jobs, even tiny updates become risky.  
If one class does one clear job, change becomes safer.

Simple statement:

- A class should have **one responsibility**.
- A class should have **one reason to change**.

### 4) Technical Deep Dive

SRP is not only about code shape. It is about **change origin**.  
A module should be owned by one business concern (or one tightly related group), not many unrelated stakeholders.

In the transcript case:

- `CalculatePay()` -> finance concern (CFO side)
- `Save()` -> technical/persistence concern (CTO side)
- `ReportHours()` -> operations concern (COO side)

Putting all of these in one class violates SRP because requests come from different business units.

### 5) Key Concepts Breakdown

- **Single responsibility:** one focused purpose per component.
- **Single reason to change:** one source of change.
- **Decomposition:** split big problems into smaller units.
- **Cohesion:** keep strongly related behavior together.
- **Coupling:** reduce interdependence between modules.
- **SRP and people:** module boundaries should map to ownership boundaries.

### 6) Code Examples

#### Before (SRP violation)

```csharp
public class Employee
{
    public decimal CalculatePay() { /* finance logic */ return 0; }
    public void Save() { /* db + logging logic mixed */ }
    public string ReportHours() { /* operations/audit logic */ return ""; }
}
```

#### After (SRP-aligned split)

```csharp
public class EmployeeRepository
{
    public void Save(Employee employee) { /* persistence only */ }
}

public class EmployeeFinances
{
    public decimal CalculatePay(Employee employee) { return 0; }
}

public class EmployeeOperations
{
    public string ReportHours(Employee employee) { return ""; }
}
```

### 7) Real-World Applications

- Payroll systems where calculation rules change frequently.
- HR portals with separate reporting and storage lifecycles.
- Audited systems where business and technical ownership are distinct.
- Enterprise products with separate domain teams.

### 8) Common Mistakes and Misconceptions

1. "One class per method is always SRP."  
   No. Over-splitting creates class explosion and needless complexity.
2. "SRP is only a clean-code preference."  
   No. It directly affects change risk, cost, and release speed.
3. "If code compiles, design is fine."  
   No. Changeability is the real metric.
4. "Logging inside business classes is harmless."  
   It often adds hidden coupling and mixed responsibilities.
5. "SRP ignores stakeholder ownership."  
   Stakeholder ownership is central to SRP decisions.

### 9) Additional Insights (AI Enhancement)

- Use GRASP Information Expert: place behavior where relevant information already exists.
- SRP works best with strong naming: class names should reveal one purpose.
- During code review, ask: "If this changes, who asked for it?" If answers are many, split.

### 10) Practice Ideas and Mini Projects

#### Exercises

1. Pick one class in your project and list all reasons it might change.
2. Split one multi-purpose service into 2-3 cohesive services.
3. Draw stakeholder-to-module mapping for a feature.

#### Mini Projects

1. Refactor a `UserService` doing auth + profile + notification into separate modules.
2. Build a small EMS domain with `EmployeeRepository`, `EmployeeFinances`, `EmployeeOperations`.

### 11) Interview Preparation (10 Questions with Model Answers)

1. **[Easy]** What is SRP?  
   A component should have one reason to change.
2. **[Easy]** Is SRP about classes only?  
   No, it applies to methods, modules, services, and packages.
3. **[Medium]** How do you identify SRP violations?  
   Look for mixed concerns and multiple change sources.
4. **[Medium]** Why is SRP described as "about people"?  
   Because change requests come from business owners/teams.
5. **[Easy]** Can SRP increase number of classes?  
   Yes, but ideally with improved clarity and reduced risk.
6. **[Medium]** Difference between SRP and cohesion?  
   SRP sets responsibility boundaries; cohesion measures relatedness within one module.
7. **[Medium]** Why does SRP reduce bugs?  
   Changes are localized, so side effects reduce.
8. **[Hard]** When should you not split further?  
   When splitting causes artificial complexity without benefit.
9. **[Hard]** Example of SRP violation in APIs?  
   Endpoint method doing validation, persistence, logging, and analytics inline.
10. **[Medium]** Is SRP enough for good design?  
   No, it works with OCP/LSP/ISP/DIP and other principles.

### 12) Quick Quiz (MCQ with Answers)

1. SRP primarily means:  
   A) one class per file  
   B) one reason to change  
   C) one method per class  
   D) one team per project  
   **Answer:** B
2. Mixed finance + persistence + reporting in one class indicates:  
   **Answer:** SRP violation
3. High cohesion is:  
   A) undesirable  
   B) neutral  
   C) preferred  
   D) unrelated to SRP  
   **Answer:** C
4. Coupling measures:  
   **Answer:** dependency between modules
5. SRP helps most with:  
   **Answer:** safe and predictable change

### 13) Glossary

_Note: For consistent definitions across the whole book, also see [Book Glossary (Unified)](#book-glossary-unified)._

- **SRP:** Single Responsibility Principle.
- **Responsibility:** a focused behavioral concern.
- **Change origin:** who/what triggers modification.
- **Cohesion:** relatedness inside a module.
- **Coupling:** dependency between modules.
- **GRASP:** responsibility assignment principles.
- **Refactoring:** structural improvement without behavior change.
- **Class explosion:** too many tiny low-value classes.

### 14) Summary Notes

- SRP is the foundation of maintainable design.
- "One reason to change" is the key test.
- Decomposition + cohesion + low coupling support SRP.
- SRP should align with stakeholder ownership.
- Correct SRP reduces regression risk and mental load.

### 15) Bridge to Next Chapter

Now that classes are focused, the next challenge is future change.  
The Open/Closed Principle shows how to extend behavior without editing stable code repeatedly.

---

## Chapter 3: Open/Closed Principle (OCP)

**Section:** OCP  
**Difficulty:** Beginner-Intermediate  
**Estimated Reading Time:** 18-24 minutes  
**Prerequisites:** SRP basics, inheritance, polymorphism

### 1) Learning Objectives

- Define OCP with precision.
- Detect OCP violations in branching-heavy logic.
- Refactor conditional logic using polymorphism.
- Extend behavior with new types without changing core code.

### 2) Starter Context (Why This Chapter Matters)

Your payroll module supports full-time and part-time staff.  
Then contractor rules arrive. Later intern rules. Later consultant rules.  
Each time you edit one large `if/else` method. Bugs follow.

OCP helps break this cycle.

### 3) Beginner Explanation

OCP means:

- **Open for extension** -> you can add new behavior.
- **Closed for modification** -> you avoid changing already-working code.

Think of a phone charger socket: you can plug new devices in (extension) without rebuilding your house wiring (modification).

### 4) Technical Deep Dive

Classic OCP violation appears when behavior is selected with repeated conditionals (`if`, `switch`) based on type/category.

Better approach:

- define a stable abstraction,
- move varying behavior into subclasses/strategies,
- rely on polymorphism.

This reduces rebuild/retest blast radius when adding new variants.

### 5) Key Concepts Breakdown

- OCP targets **feature extension**, not bug fixes.
- Repeated type-check branching is often a smell.
- Prefer polymorphism over conditional growth.
- Stable contracts + pluggable implementations enable OCP.

### 6) Code Examples

#### Before (branch-heavy)

```csharp
public decimal CalculatePay(Employee e)
{
    if (e.Type == EmployeeType.FullTime) return e.Hours * 10;
    if (e.Type == EmployeeType.PartTime) return e.Hours * 5;
    return 0;
}
```

#### After (extension via subclasses)

```csharp
public abstract class EmployeeFinances
{
    public abstract decimal CalculatePay(Employee e);
}

public class FullTimeFinances : EmployeeFinances
{
    public override decimal CalculatePay(Employee e) => e.Hours * 10;
}

public class PartTimeFinances : EmployeeFinances
{
    public override decimal CalculatePay(Employee e) => e.Hours * 5;
}

public class ContractorFinances : EmployeeFinances
{
    public override decimal CalculatePay(Employee e) => e.Hours * 2;
}
```

### 7) Real-World Applications

- Payment gateways with different fee models.
- Discount engines with evolving campaign rules.
- Notification channels (email/SMS/push) as pluggable strategies.

### 8) Common Mistakes and Misconceptions

1. "Never modify code at all."  
   OCP is about minimizing modification for new behavior, not freezing systems.
2. Creating inheritance without shared abstraction value.  
   This causes accidental complexity.
3. Treating every `if` as bad.  
   Not always. Focus on recurring variation points.
4. Overengineering too early.  
   Apply OCP where change is frequent and predictable.
5. Ignoring test strategy.  
   OCP needs contract-level tests plus implementation-level tests.

### 9) Additional Insights (AI Enhancement)

- Strategy pattern is often a practical OCP implementation.
- Composition can achieve OCP better than deep inheritance trees.
- OCP and YAGNI should be balanced: design for likely extension, not every imagined future.

### 10) Practice Ideas and Mini Projects

- Convert one `switch`-based workflow into strategy objects.
- Add a new employee type without changing existing pay classes.
- Build a plugin-style calculator with runtime-selected strategies.

### 11) Interview Preparation (10 Questions with Model Answers)

1. **[Easy]** What is OCP?  
   Extend behavior without repeatedly modifying stable code.
2. **[Medium]** Is OCP anti-`if`?  
   Not strictly; it targets excessive change-prone branching.
3. **[Easy]** Typical OCP tools?  
   Interfaces, abstract classes, polymorphism, strategy pattern.
4. **[Medium]** OCP vs SRP?  
   SRP organizes responsibility; OCP organizes extension.
5. **[Medium]** OCP and regression risk?  
   Lower risk because old code remains untouched more often.
6. **[Hard]** How to detect OCP pain?  
   Frequent edits to the same method for new variants.
7. **[Medium]** Can composition support OCP?  
   Yes, often cleaner than inheritance.
8. **[Hard]** OCP with DI?  
   DI helps choose implementations without changing consumers.
9. **[Medium]** OCP and performance?  
   Usually acceptable tradeoff for maintainability.
10. **[Easy]** One practical rule?  
   "Add new class, not new branch" for stable variation points.

### 12) Quick Quiz

1. OCP stands for: Open/Closed Principle.  
2. Open means: open for extension.  
3. Closed means: closed for modification (for new features).  
4. Common smell: many type-based conditionals in one method.  
5. Common fix: polymorphic implementations.

### 13) Glossary

_Note: For consistent definitions across the whole book, also see [Book Glossary (Unified)](#book-glossary-unified)._

- **OCP:** Open/Closed Principle.
- **Polymorphism:** one interface, many forms.
- **Strategy:** interchangeable behavior object.
- **Variation point:** area likely to change by new types.
- **Regression:** unintended break from a change.

### 14) Summary Notes

- OCP protects stable code from repeated feature edits.
- Polymorphism is a common route to OCP.
- Repeated branching is a strong refactoring signal.
- Add new behavior by extension units.

### 15) Bridge to Next Chapter

Inheritance helps OCP, but it can still break programs if subtype behavior is invalid.  
That is where Liskov Substitution Principle becomes essential.

---

## Chapter 4: Liskov Substitution Principle (LSP)

**Section:** LSP  
**Difficulty:** Intermediate  
**Estimated Reading Time:** 20-26 minutes  
**Prerequisites:** OCP, inheritance, polymorphism

### 1) Learning Objectives

- Explain LSP in practical terms.
- Detect wrong inheritance models.
- Prevent runtime failures caused by invalid subtype behavior.
- Restructure inheritance hierarchies safely.

### 2) Starter Context (Why This Chapter Matters)

You created `ContractorFinances` under `EmployeeFinances`.  
Then `CalculatePay()` throws `NotImplementedException` for contractors.  
Everything compiles, but crashes at runtime.  
This is a classic LSP violation.

### 3) Beginner Explanation

LSP means: if `Child` is used where `Parent` is expected, the app should still work correctly.

If replacing parent with child causes breakage, that child is not a valid subtype.

### 4) Technical Deep Dive

Subtype contracts must preserve expectations of base type clients.

Violations often appear when:

- child throws for base-required methods,
- child weakens behavior guarantees,
- inheritance is chosen by shape, not by behavior compatibility.

In the transcript case, contractor had rewards logic but no pay logic, so it was wrongly modeled under the same finances base.

### 5) Key Concepts Breakdown

- "Looks like" is not enough; behavior compatibility matters.
- Invalid subtype causes runtime surprises.
- Split hierarchies when behavior contracts differ.
- Favor composition when inheritance is unnatural.

### 6) Code Examples

#### Problematic model

```csharp
public class ContractorFinances : EmployeeFinances
{
    public override decimal CalculatePay(Employee e)
        => throw new NotImplementedException();
}
```

#### Better model

```csharp
public abstract class EmployeeRewards
{
    public abstract decimal CalculateReward(Employee e);
}

public abstract class EmployeeFinances : EmployeeRewards
{
    public abstract decimal CalculatePay(Employee e);
}

public class ContractorRewards : EmployeeRewards
{
    public override decimal CalculateReward(Employee e) => 120;
}
```

### 7) Real-World Applications

- Payment providers with different capabilities.
- Storage adapters where some backends cannot support all operations.
- Domain models where partial capability should be represented explicitly.

### 8) Common Mistakes and Misconceptions

1. Modeling hierarchy by noun similarity only.
2. Returning default/empty values to hide unsupported behavior.
3. Throwing `NotImplementedException` in production subtype contracts.
4. Forcing all children to support all parent operations.
5. Missing contract tests for base/derived compatibility.

### 9) Additional Insights (AI Enhancement)

- LSP is often the "quality gate" for OCP-driven inheritance.
- Interface segregation (next chapter) often fixes LSP pain.
- Contract tests should run against every implementation.

### 10) Practice Ideas and Mini Projects

- Audit one inheritance tree for LSP violations.
- Replace one invalid subclass with separate abstraction.
- Build test suite that validates base contract across implementations.

### 11) Interview Preparation (10 Questions with Model Answers)

1. **[Easy]** Define LSP.  
   Subtypes must be substitutable for base types without breaking behavior.
2. **[Medium]** LSP and exceptions?  
   Child should not introduce unsupported behavior for required base methods.
3. **[Medium]** LSP vs OCP?  
   OCP enables extension; LSP validates subtype correctness.
4. **[Easy]** Is compile success enough for LSP?  
   No, runtime behavior is key.
5. **[Easy]** Red flag example?  
   `NotImplementedException` in subtype override of required behavior.
6. **[Hard]** How to fix?  
   Split abstractions or redesign capabilities.
7. **[Medium]** LSP and interface design?  
   Small focused interfaces improve substitutability.
8. **[Medium]** Can composition avoid LSP issues?  
   Yes, often.
9. **[Hard]** Test for LSP?  
   Shared contract tests over all implementations.
10. **[Easy]** Real-world analogy?  
   New headphone should work in existing phone port seamlessly.

### 12) Quick Quiz

1. LSP focuses on: substitutability.  
2. Runtime crash after subtype replacement suggests: LSP violation.  
3. Throwing unsupported override often indicates: wrong hierarchy.  
4. Good fix: split abstractions by capability.  
5. LSP helps reduce: runtime surprises.

### 13) Glossary

_Note: For consistent definitions across the whole book, also see [Book Glossary (Unified)](#book-glossary-unified)._

- **LSP:** Liskov Substitution Principle.
- **Subtype contract:** expected behavior inherited from base abstraction.
- **Substitutability:** safe replacement of parent with child.
- **Capability modeling:** exposing only supported operations.
- **Contract test:** shared behavior tests across implementations.

### 14) Summary Notes

- Inheritance must preserve behavior, not just signatures.
- Unsupported operations in children are major warning signs.
- Correct hierarchy design prevents runtime failures.

### 15) Bridge to Next Chapter

LSP failures often happen because interfaces are too broad.  
The next chapter (ISP) teaches how to split interfaces so clients only depend on what they use.

---

## Chapter 5: Interface Segregation Principle (ISP)

**Section:** ISP  
**Difficulty:** Beginner-Intermediate  
**Estimated Reading Time:** 16-22 minutes  
**Prerequisites:** LSP, interfaces

### 1) Learning Objectives

- Define ISP and identify forced interface implementation.
- Split "fat interfaces" into focused contracts.
- Reduce unnecessary dependency burden on clients.

### 2) Starter Context

A new requirement adds stock options for C-level employees.  
You add `CalculateStockOptions()` to a shared interface.  
Now full-time and part-time classes must implement a method they never use.

### 3) Beginner Explanation

ISP says: **do not force clients to depend on methods they do not need**.

If someone does not need a feature, do not force that feature into their contract.

### 4) Technical Deep Dive

Broad interfaces create ripple effects:

- unnecessary implementation burden,
- fake/default methods,
- needless rebuild and redeploy across consumers.

Refactor approach:

- keep base interface focused,
- create specialized interfaces for optional capabilities.

### 5) Key Concepts Breakdown

- "Fat interface" is a design smell.
- Separate mandatory vs optional capabilities.
- Interface changes can affect all implementers and clients.
- ISP improves modularity and API stability.

### 6) Code Examples

```csharp
public interface IEmployeeFinances
{
    decimal CalculatePay(Employee e);
    decimal CalculateReward(Employee e);
}

public interface IStockOptionEligible : IEmployeeFinances
{
    decimal CalculateStockOptions(Employee e);
}

public class CLevelFinances : IStockOptionEligible { /* ... */ }
public class PartTimeFinances : IEmployeeFinances { /* ... */ }
```

### 7) Real-World Applications

- SDK/API design for partners with different capability sets.
- Enterprise role-based feature contracts.
- Plugin platforms with optional advanced interfaces.

### 8) Common Mistakes and Misconceptions

1. Adding methods to popular interfaces casually.
2. Returning dummy values to satisfy irrelevant methods.
3. Breaking clients for niche new requirements.
4. Confusing ISP with "more interfaces always better."
5. Over-fragmenting without clear capability boundaries.

### 9) Additional Insights

- ISP and LSP are strongly connected: smaller interfaces improve substitutability.
- Versioned APIs benefit heavily from ISP boundaries.

### 10) Practice Ideas and Mini Projects

- Refactor one fat interface in your project.
- Introduce capability-specific interface for optional feature.
- Build simple permission-based service contracts.

### 11) Interview Questions (10)

1. **[Easy]** Define ISP.  
2. **[Easy]** What is a fat interface?  
3. **[Medium]** Why is forced implementation harmful?  
4. **[Medium]** ISP vs SRP?  
5. **[Hard]** ISP and API versioning?  
6. **[Hard]** Example of ISP violation in microservices?  
7. **[Hard]** How to refactor without breaking all consumers?  
8. **[Medium]** ISP and LSP relationship?  
9. **[Medium]** When to keep one interface instead of splitting?  
10. **[Medium]** Can default methods hide ISP issues?

### 12) Quick Quiz

1. ISP means clients depend only on used methods.  
2. Forced implementation indicates ISP violation.  
3. Splitting by capability is common ISP remedy.  
4. Over-segmentation is also a risk.  
5. ISP improves API stability.

### 13) Glossary

_Note: For consistent definitions across the whole book, also see [Book Glossary (Unified)](#book-glossary-unified)._

- **ISP:** Interface Segregation Principle.
- **Client:** class/component using a contract.
- **Fat interface:** interface with unrelated methods.
- **Capability interface:** focused contract for specific behavior.

### 14) Summary Notes

- Keep interfaces narrow and meaningful.
- Avoid forcing irrelevant behavior.
- Separate optional features cleanly.

### 15) Bridge to Next Chapter

With focused interfaces in place, we now tackle dependency direction itself.  
The next chapter covers Dependency Inversion, Dependency Injection, and IoC.

---

## Chapter 6: Dependency Inversion Principle (DIP), Dependency Injection (DI), and IoC

**Section:** DIP  
**Difficulty:** Intermediate  
**Estimated Reading Time:** 28-36 minutes  
**Prerequisites:** Interfaces, SRP/OCP/ISP basics

### 1) Learning Objectives

- Explain DIP and why high-level modules should depend on abstractions.
- Differentiate DIP, DI, and IoC clearly.
- Refactor direct object creation to injected dependencies.
- Understand container-based dependency wiring.

### 2) Starter Context

`Employee.Save()` catches exceptions and creates `new FileLogger()` or `new DatabaseLogger()` internally.  
Now business class decides infrastructure policy and is tightly coupled to concrete loggers.

### 3) Beginner Explanation

DIP says:

- Important business code should not depend directly on low-level technical details.
- Both should connect through interfaces.

Think: you use a phone camera button (interface), not lens internals (details).

### 4) Technical Deep Dive

**DIP definition (practical):**

- High-level modules should not depend on low-level modules.
- Both should depend on abstractions.
- Details should depend on abstractions.

In code, avoid this inside core modules:

```csharp
var logger = new FileLogger();
```

Use this instead:

```csharp
public class EmployeeService
{
    private readonly ILogger _logger;
    public EmployeeService(ILogger logger) { _logger = logger; }
}
```

### 5) Key Concepts Breakdown

- **High-level module:** business policy, core domain flow.
- **Low-level module:** implementation details (DB, file, network).
- **Abstraction:** interface/contract.
- **DI:** mechanism of supplying dependencies from outside.
- **IoC:** broader idea of handing control to external framework/container.

### 6) Code Examples

#### Constructor injection

```csharp
public interface ILogger { void LogError(string message); }

public class EmployeeRepository
{
    private readonly ILogger _logger;
    public EmployeeRepository(ILogger logger) => _logger = logger;

    public void Save(Employee employee)
    {
        try
        {
            // save logic
        }
        catch (Exception ex)
        {
            _logger.LogError(ex.Message);
        }
    }
}
```

#### Container registration example (conceptual)

```csharp
services.AddTransient<ILogger, DatabaseLogger>();
services.AddTransient<EmployeeRepository>();
```

### 7) Real-World Applications

- ASP.NET Core service registration and constructor injection.
- Swappable logging/metrics providers.
- Test-friendly architecture with mockable dependencies.
- Feature toggles via interface implementations.

### 8) Common Mistakes and Misconceptions

1. Confusing DIP (principle) with DI (technique).
2. Moving `new` to client code only (problem shifted, not solved).
3. Service locator misuse as hidden dependency.
4. Overusing DI for trivial value objects.
5. Container configuration leakage into domain code.

### 9) Additional Insights

- DIP improves testability and portability of business logic.
- Composition root should own concrete wiring.
- Containers are tools; architecture principles still matter.

### 10) Practice Ideas and Mini Projects

1. Refactor one class using `new` dependencies into constructor injection.
2. Add second logger implementation and switch without touching consumer class.
3. Build mini console app with DI container and 3 service layers.

### 11) Interview Questions (10)

1. **[Easy]** Define DIP in one sentence.  
2. **[Medium]** DIP vs DI vs IoC?  
3. **[Medium]** Why is `new` in domain classes a smell?  
4. **[Easy]** High-level vs low-level module examples?  
5. **[Hard]** Constructor vs property injection trade-offs?  
6. **[Medium]** How does DIP improve testing?  
7. **[Hard]** What is composition root?  
8. **[Hard]** Can DIP exist without DI container?  
9. **[Medium]** DI container drawbacks?  
10. **[Hard]** How do you avoid over-injection?

### 12) Quick Quiz

1. DIP is a principle.  
2. DI is a technique to implement DIP.  
3. IoC is broader than DI.  
4. Business modules should depend on interfaces.  
5. Containers help automate dependency resolution.

### 13) Glossary

_Note: For consistent definitions across the whole book, also see [Book Glossary (Unified)](#book-glossary-unified)._

- **DIP:** Dependency Inversion Principle.
- **DI:** Dependency Injection.
- **IoC:** Inversion of Control.
- **Container:** framework managing dependency creation/wiring.
- **Composition root:** single place where object graph is built.
- **Concrete type:** implementation class, not interface.

### 14) Summary Notes

- DIP changes dependency direction toward abstractions.
- DI operationalizes DIP in code.
- IoC is the broader control handoff pattern.
- Correct wiring location matters for loose coupling.

### 15) Bridge to Next Chapter

After SOLID, strong engineers also rely on supporting principles like DRY, KISS, YAGNI, and least astonishment.  
Next chapter covers these high-impact practical rules.

---

## Chapter 7: Other Essential Design Principles

**Section:** Other Design Principles  
**Difficulty:** Beginner  
**Estimated Reading Time:** 20-28 minutes  
**Prerequisites:** SOLID fundamentals

### 1) Learning Objectives

- Apply DRY to reduce duplication.
- Use KISS to keep systems understandable.
- Use YAGNI to avoid speculative complexity.
- Recognize supporting principles used in engineering decisions.

### 2) Starter Context

Two teams build similar features.  
Team A copies logic in ten places. Team B extracts shared code.  
After requirement change, Team A updates ten files; Team B updates one.

### 3) Beginner Explanation

These principles are practical guardrails:

- **DRY:** do not repeat the same logic everywhere.
- **KISS:** keep solutions simple.
- **YAGNI:** build what is needed now, not imagined future features.
- **Least Astonishment:** code should behave as its name suggests.
- **Avoid Premature Optimization:** optimize after correctness and evidence.

### 4) Technical Deep Dive

- **DRY / Single Source of Truth:** one authoritative representation of logic/data.
- **KISS:** complexity increases defect probability and maintenance cost.
- **YAGNI:** speculative architecture often becomes dead weight.
- **Occam's Razor:** avoid unnecessary entities.
- **Opportunity Cost:** choose solutions with highest value for cost.
- **RDUF:** enough upfront design to guide implementation, avoid extreme BDUF.

### 5) Key Concepts Breakdown

- Duplication amplifies change effort.
- Simplicity is a quality attribute.
- Unused code is future maintenance liability.
- Readability is a team productivity multiplier.
- Timing matters for optimization and architecture depth.

### 6) Code Examples

```csharp
// DRY violation: repeated tax formula in multiple methods
// Better: centralize in one TaxCalculator service
public class TaxCalculator
{
    public decimal Calculate(decimal amount) => amount * 0.18m;
}
```

### 7) Real-World Applications

- Shared domain calculators used across services.
- Centralized validation and policy engines.
- Measured optimization in high-load endpoints only.

### 8) Common Mistakes and Misconceptions

1. "DRY means no repetition at all." (Some repetition is acceptable for clarity.)
2. "KISS means simplistic architecture." (It means appropriate simplicity.)
3. "YAGNI means no planning." (It means avoid speculative implementation.)
4. "Optimization is always good." (Timing and measurement matter.)
5. "More abstractions always better." (Only if they reduce real complexity.)

### 9) Additional Insights

- DRY and SRP reinforce each other.
- KISS improves onboarding and incident response speed.
- YAGNI prevents needless complexity smell discussed earlier.

### 10) Practice Ideas and Mini Projects

- Remove duplication from one workflow.
- Simplify one over-abstracted feature.
- Build small "principle checklist" for code reviews.

### 11) Interview Questions (10)

1. **[Easy]** What is DRY and when does it help most?  
2. **[Medium]** DRY vs accidental duplication?  
3. **[Easy]** Explain KISS with engineering example.  
4. **[Medium]** YAGNI and product uncertainty relation?  
5. **[Medium]** Premature optimization risks?  
6. **[Medium]** Least astonishment in API naming?  
7. **[Hard]** Opportunity cost in architecture choices?  
8. **[Hard]** Occam's Razor in service design?  
9. **[Hard]** When is upfront design still necessary?  
10. **[Hard]** How to balance speed vs maintainability?

### 12) Quick Quiz

1. DRY reduces repetition.  
2. KISS reduces cognitive load.  
3. YAGNI avoids speculative features.  
4. Premature optimization can waste effort.  
5. Least astonishment improves readability/trust.

### 13) Glossary

_Note: For consistent definitions across the whole book, also see [Book Glossary (Unified)](#book-glossary-unified)._

- **DRY:** Don't Repeat Yourself.
- **KISS:** Keep It Simple, Stupid.
- **YAGNI:** You Aren't Gonna Need It.
- **BDUF:** Big Design Up Front.
- **RDUF:** Rough/Sufficient Design Up Front.
- **Least Astonishment:** predictable behavior by design.
- **Single Source of Truth:** one authoritative definition.

### 14) Summary Notes

- Supporting principles strengthen SOLID outcomes.
- Simplicity and clarity are competitive advantages.
- Avoid duplicate logic and speculative implementation.
- Optimize after evidence, not assumptions.

### 15) Bridge to Next Chapter

The final chapter consolidates SOLID, recap strategy, and interview preparation so you can apply these ideas confidently in projects and interviews.

---

## Chapter 8: Course Conclusion and Interview Preparation

**Section:** Conclusion  
**Difficulty:** Beginner  
**Estimated Reading Time:** 15-20 minutes  
**Prerequisites:** Entire book

### 1) Learning Objectives

- Consolidate all five SOLID principles in one mental model.
- Build an actionable review checklist for real projects.
- Prepare for common interview question patterns.

### 2) Starter Context

Most interview failures on SOLID happen not because people cannot define acronyms, but because they cannot apply principles to real code scenarios.  
This chapter closes that gap.

### 3) Beginner Explanation

SOLID is not five separate exam definitions.  
It is one design mindset for writing change-friendly software.

### 4) Technical Deep Dive

End-to-end view:

- **SRP:** split by change reason/ownership.
- **OCP:** extend behavior without repeated edits.
- **LSP:** ensure subtype correctness.
- **ISP:** keep contracts focused.
- **DIP:** depend on abstractions; inject details from outside.

Together they produce high cohesion, low coupling, and stable evolution.

### 5) Key Concepts Breakdown

- Design quality = ability to change safely.
- Keep policy independent from implementation details.
- Use principles together, not in isolation.
- Prefer practical refactoring over theoretical perfection.

### 6) Code Examples

Use this "mini audit snippet" mentally during reviews:

```text
SRP: one reason to change?
OCP: new feature needs edit or extension?
LSP: subtype swap safe?
ISP: clients forced to use unused methods?
DIP: high-level depending on concrete class?
```

### 7) Real-World Applications

- Code reviews with principle-based checklists.
- Architecture discussions for feature evolution.
- Legacy code modernization roadmaps.
- Interview whiteboard and design rounds.

### 8) Common Mistakes and Misconceptions

1. Memorizing definitions without code-level application.
2. Applying all principles blindly in tiny scripts.
3. Chasing abstraction without business need.
4. Ignoring team conventions and ownership boundaries.
5. Forgetting trade-offs (time, complexity, risk).

### 9) Additional Insights

- Interviewers often test OCP and LSP deeply because they expose design thinking.
- Strong answers use one coherent case study across all principles.
- Explain not only "what," but also "why now" and "trade-offs."

### 10) Practice Ideas and Mini Projects

1. Create a SOLID review checklist and apply it to one real project.
2. Refactor one legacy module and document before/after design decisions.
3. Mock interview: explain all SOLID principles using one case study.

### 11) Interview Questions (10)

1. **[Easy]** What are SOLID principles and why use them?  
2. **[Hard]** Explain SOLID using one practical case study.  
3. **[Hard]** Identify OCP and LSP violations in a given design.  
4. **[Medium]** DIP vs DI vs IoC differences.  
5. **[Medium]** High cohesion and low coupling in practice?  
6. **[Medium]** Signs of design rot in a project?  
7. **[Medium]** Which SOLID principle is hardest and why?  
8. **[Hard]** How to refactor without breaking production behavior?  
9. **[Hard]** What trade-offs appear while applying SOLID?  
10. **[Medium]** Which non-SOLID principles do you also use and why?

### 12) Quick Quiz

1. SOLID primarily helps long-term maintainability.  
2. OCP and LSP are often tested deeply in interviews.  
3. DI is a mechanism; DIP is a principle.  
4. Design rot signals include rigidity and fragility.  
5. Principles are tools, not rigid laws.

### 13) Glossary

_Note: For consistent definitions across the whole book, also see [Book Glossary (Unified)](#book-glossary-unified)._

- **Design rot:** structural degradation over time.
- **Maintainability:** ease of safe modification.
- **Refactoring:** restructuring without behavior change.
- **Trade-off:** balancing competing priorities.
- **Case study narrative:** one story explaining multiple principles.

### 14) Summary Notes

- SOLID is a practical framework for change-resilient software.
- Real mastery comes from applying principles to messy code.
- Supporting principles (DRY/KISS/YAGNI) amplify design quality.
- Interviews reward applied reasoning more than memorized definitions.

### 15) Bridge Forward

You now have a full baseline to design, review, and refactor object-oriented systems professionally.  
Next step: pick one active codebase and run a chapter-by-chapter implementation audit using this book.

---

## Final Course Summary Note

This book converted the full course transcript into a structured learning path:

- Introduction to design rot and motivation for SOLID.
- SRP through responsibility ownership, cohesion, and coupling.
- OCP through extension-driven design.
- LSP through safe subtype modeling.
- ISP through focused contracts.
- DIP through abstraction-first dependencies, DI, and IoC.
- Supporting engineering principles beyond SOLID.
- Interview-first preparation and practice framework.

**Practical recommendation:** revisit this book while coding, not only while studying. Use the checklists during PR review and refactoring sessions.

---

## Book Glossary (Unified)

This glossary is the **single source of truth** for terms used across all chapters.

- **Abstraction:** A stable contract that shows what is essential and hides internal details (commonly an interface in OOP).
- **BDUF (Big Design Up Front):** Completing and "perfecting" design before implementation begins (often associated with waterfall).
- **Change origin:** The stakeholder/team/business function that requests a change.
- **Cohesion:** How strongly related the responsibilities inside one module/class are. Higher cohesion is preferred.
- **Composition root:** The one place in an application where the object graph is assembled and dependencies are wired (often `Startup`/bootstrap code).
- **Concrete type:** A specific implementation class (not an interface/abstraction).
- **Coupling:** How interdependent two modules are. Lower coupling is preferred.
- **Design rot:** Gradual degradation of system structure due to unmanaged change, shortcuts, and unclear boundaries.
- **DI (Dependency Injection):** A technique where dependencies are provided from the outside (constructor/property/method injection) instead of created internally.
- **DIP (Dependency Inversion Principle):** High-level modules should not depend on low-level modules; both depend on abstractions. Details should depend on abstractions.
- **DRY:** Don't Repeat Yourself. A piece of knowledge/logic should have one unambiguous representation.
- **Fat interface:** An interface with methods that many implementers/clients do not need (an ISP smell).
- **High-level module:** Code that expresses core policy/business rules and defines the application's meaning.
- **Immobility:** Difficulty extracting/reusing parts of a system without bringing unrelated baggage.
- **IoC (Inversion of Control):** A broad concept where control over flow/object creation is inverted to a framework/container/event system.
- **ISP (Interface Segregation Principle):** No client should be forced to depend on methods it does not use; prefer focused interfaces.
- **KISS:** Keep It Simple, Stupid. Prefer the simplest solution that is still correct and maintainable.
- **LSP (Liskov Substitution Principle):** Subtypes must be substitutable for base types without breaking expected behavior.
- **Low-level module:** Technical details such as logging, persistence, networking, and external integrations.
- **Needless complexity:** Extra structures introduced for speculative futures that may never arrive.
- **Needless repetition:** Duplication of logic through copy-paste instead of reuse/abstraction.
- **OCP (Open/Closed Principle):** Software entities should be open for extension but closed for modification when adding new behavior.
- **Opacity:** Code that is difficult to understand even for maintainers.
- **Polymorphism:** Using a common abstraction to work with multiple implementations through the same interface.
- **Refactoring:** Improving internal structure without changing externally observable behavior.
- **Regression:** An unintended break introduced by a change.
- **Rigidity:** Difficulty making a change because many other dependent changes are required.
- **RDUF (Rough/Sufficient Design Up Front):** Doing enough upfront design to guide development without trying to perfect everything early.
- **SRP (Single Responsibility Principle):** A module/class should have one responsibility and one reason to change.
- **Strategy pattern:** Encapsulating interchangeable behavior behind a common interface.
- **Substitutability:** The property that a derived type can replace a base type without altering correctness.
- **Viscosity:** The tendency for the "wrong" (hacky) change path to be easier than the "right" design-preserving change.
- **Variation point:** A part of the system that changes as new cases/types/features are added.
- **YAGNI:** You Aren't Gonna Need It. Avoid implementing functionality until it is actually needed.

