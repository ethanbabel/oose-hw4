# Task 4: Design Principle 3

The advisor's revised design primarily follows the Dependency Inversion Principle (DIP) and also supports the Open-Closed Principle (OCP).

## Dependency Inversion Principle

DIP states that high-level modules should not depend directly on low-level modules, both should depend on abstractions. Abstractions should not depend on implementation details, those details should depend on the abstractions.

In the original design, `Shelf` depends directly on the concrete `Book` and `DVD` classes. This couples the shelf's product-management behavior to those particular product types.

In the revised design, `Shelf` depends on the `Product` interface, and both `Book` and `DVD` implement that interface. The shelf can work through a common product contract without needing to know each product's concrete implementation. The key change is that both the shelf and the concrete products depend on the abstraction.

## Open-Closed Principle

OCP states that software entities should be open for extension but closed for modification. With the revised design, a new product type, such as `Magazine`, can implement `Product` and be used by `Shelf` without modifying the shelf's existing product-management code, provided the new type fits the existing contract.

This benefit depends on `Shelf` using the operations declared by `Product`, rather than casting products to concrete classes or branching on whether an item is a `Book` or a `DVD`. The shared interface provides the extension point that allows additional product types to be introduced independently.
