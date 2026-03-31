# SOLID Principles - Companion Q&A Book

This companion book captures **reader questions and clarifications** that come up while learning SOLID, with clean answers and practical examples.

---

## Chapter 1: Reader Q&A and Clarifications (SOLID Deep Understanding)

### Q1) LSP Confusion: “If we move `CalculateReward` out of `EmployeeFinances` but still inherit the class that declares it, aren’t we still responsible for implementing it?”

**Improved (book-ready) question:**  
If we extract `CalculateReward()` into a separate base type (for example `EmployeeRewards`) and then inherit from it, aren’t we still “responsible” for rewards? How does this help LSP?

**Answer:**  
Yes — **inheritance always means responsibility for the inherited contract**. That is *not* the problem. LSP breaks when you inherit a contract you **cannot honestly fulfill**.

The fix helps because it separates **capabilities**:

- If a type can compute rewards, it may inherit `EmployeeRewards`.
- If a type can compute pay, it should inherit a stronger contract (for example `PayableEmployeeFinances`) that promises `CalculatePay()`.

So the redesign is basically saying:  
“Contractors are reward-eligible, but they are not pay-calculable by our system.”  
That keeps contracts truthful and prevents runtime surprises.

#### Before (LSP violation: runtime failure)

```csharp
public abstract class EmployeeFinances
{
    public abstract decimal CalculatePay(Employee e);
    public virtual decimal CalculateReward(Employee e) => 100;
}

public class ContractorFinances : EmployeeFinances
{
    public override decimal CalculatePay(Employee e)
        => throw new NotImplementedException(); // breaks substitutability
}
```

#### After (LSP-aligned: capability-based base types)

```csharp
public abstract class EmployeeRewards
{
    public abstract decimal CalculateReward(Employee e);
}

public abstract class PayableEmployeeFinances : EmployeeRewards
{
    public abstract decimal CalculatePay(Employee e);
}

public class ContractorRewards : EmployeeRewards
{
    public override decimal CalculateReward(Employee e) => 120;
}
```

**Key idea:** inheriting `EmployeeRewards` is correct because rewards truly apply.  
Not inheriting “pay” prevents invalid substitution.

---

### Q2) Composition vs Inheritance: “What’s the difference, and why do people say ‘prefer composition over inheritance’?”

**Improved (book-ready) question:**  
When should we use inheritance vs composition, and why is “prefer composition over inheritance” a common guideline?

**Answer:**  
Both are tools:

- **Inheritance (“is-a”)**: Use when a subtype can **honor the full contract** of the base type without exceptions, dummy values, or weakened guarantees. (This is exactly what **LSP** enforces.)
- **Composition (“has-a”)**: Use when behavior varies independently, when you want swappable parts, or when “is-a” is not behaviorally true.

People prefer composition because it is often:

- easier to extend safely (OCP),
- easier to test (swap dependencies),
- less likely to create fragile hierarchies (LSP violations),
- less coupled to base class implementation details.

---

### Q3) “Where do we hear ‘composition’ in SOLID? Is it part of SOLID?”

**Improved (book-ready) question:**  
Composition isn’t a letter in SOLID, so where does it show up in SOLID-based design?

**Answer:**  
Composition is not one of the five letters, but it is a **common implementation style** that makes SOLID easier to apply:

- **DIP** strongly encourages composition: high-level modules *hold references to abstractions* (interfaces) and delegate work to them.
- **OCP** is often implemented using composition (Strategy pattern): add a new behavior class rather than editing an existing conditional block.
- **SRP** often results in composition: you split responsibilities into collaborators and the original module composes them.
- **ISP** supports composition by creating small capability interfaces that can be composed without forcing irrelevant methods.
- **LSP**: if inheritance becomes awkward, composition is often the safer alternative.

So in practice: **SOLID often leads to designs with more composition and less deep inheritance**.

---

### Q4) Strategy Pattern: “What is it and why is it useful?”

**Improved (book-ready) question:**  
What is the Strategy Pattern, and how does it help us reduce `if/switch` logic while keeping the code open for extension?

**Answer:**  
The **Strategy Pattern** is when you:

1. define an interface for a behavior (strategy),
2. implement multiple versions of that behavior in separate classes,
3. inject/select the right one at runtime.

It is useful because it:

- reduces branching (`if/switch`) in one method,
- makes adding new behavior easier (OCP),
- keeps code testable and modular.

#### Before (branching grows)

```csharp
public decimal CalculatePay(Employee e)
{
    if (e.Type == EmployeeType.FullTime) return e.Hours * 10;
    if (e.Type == EmployeeType.PartTime) return e.Hours * 5;
    if (e.Type == EmployeeType.Contractor) return e.Hours * 2;
    throw new Exception("Unknown type");
}
```

#### After (Strategy Pattern)

```csharp
public interface IPayStrategy
{
    decimal CalculatePay(Employee e);
}

public class FullTimePayStrategy : IPayStrategy
{
    public decimal CalculatePay(Employee e) => e.Hours * 10;
}

public class PartTimePayStrategy : IPayStrategy
{
    public decimal CalculatePay(Employee e) => e.Hours * 5;
}

public class ContractorPayStrategy : IPayStrategy
{
    public decimal CalculatePay(Employee e) => e.Hours * 2;
}

public class PayrollService
{
    private readonly IPayStrategy _payStrategy;
    public PayrollService(IPayStrategy payStrategy) => _payStrategy = payStrategy;

    public decimal CalculatePay(Employee e) => _payStrategy.CalculatePay(e);
}
```

**Where selection happens:** a factory or DI container chooses which strategy to inject based on employee type or configuration.

---

### Quick Summary Notes (for revision)

- **LSP**: inheritance must preserve behavior contracts; splitting base types by capability prevents invalid subtypes.
- **Inheritance**: great when “is-a” is true in behavior and contract.
- **Composition**: great when behavior is swappable or varies independently.
- **Strategy Pattern**: a composition-based way to implement OCP and reduce conditionals.

---

_End of Companion Chapter 1_
