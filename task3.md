# Task 3: Design Principles 2

## Is `Board3D extends Board` a Good Idea?

I would consider this a bad idea. The existing `Board` represents a two-dimensional board, it stores pieces in a 2D array and identifies a location using two coordinates. A 3D board needs a third coordinate to identify a location. Although both objects are boards conceptually, that similarity does not establish that a `Board3D` can satisfy the existing `Board` contract.

## Liskov Substitution Principle (LSP)

The main concern is the Liskov Substitution Principle. A subclass should be usable wherever its superclass is expected without breaking the superclass's behavioral contract. Inheritance must preserve the meaning of the existing operations, rather than merely reuse their names or code.

For a 2D board, `(x, y)` identifies one location. For a 3D board, the same pair could refer to several locations with different `z` coordinates. Thus the inherited `setLocation(int x, int y)` method does not provide enough information to select an arbitrary location on the 3D board.

Adding a third dimension is not automatically an LSP violation. A carefully defined 2D view of a 3D board could preserve a 2D contract. However, the assignment provides no such view or mapping, and it would represent only part of the 3D board. Extending the concrete 2D implementation solely to reuse code would therefore be a poor fit for representing the complete 3D board.

## Open-Closed Principle (OCP)

The Open-Closed Principle favors designs that can be extended without repeatedly modifying existing, working code. Making `Board3D` inherit from `Board` might appear to satisfy OCP because it introduces a subclass, but inheritance on its own does not guarantee a suitable extension.

If the change requires adding dimension checks to `Board`, changing its storage, or making existing clients check whether an object is a `Board3D`, the current abstraction is not providing a clean extension point. A better design would introduce a shared abstraction whose contract does not assume two-dimensional coordinates. 

## Better Design

I would keep the 2D and 3D boards as separate implementations. If shared client behavior is useful, I would introduce a board interface parameterized by its position type:

```java
public interface GameBoard<P> {
    Piece getPiece(P position);
    void setPiece(P position, Piece piece);
    void clearLocation(P position);
}
```

Here, `Piece` represents the existing chess-piece type. `Position2D` would contain `x` and `y`, while `Position3D` would contain `x`, `y`, and `z`. The interface makes the piece being assigned explicit because the assignment's simplified `setLocation` signature does not show how that value is supplied.

- `Board2D implements GameBoard<Position2D>` would own the 2D array.
- `Board3D implements GameBoard<Position3D>` would own a 3D array or another suitable representation.

The common contract would specify consistent behavior for reading occupied or empty locations, setting pieces, clearing locations, and rejecting invalid positions. Each implementation would fulfill that contract for its own position type. A client written specifically for `GameBoard<Position2D>` could not accidentally receive a `GameBoard<Position3D>`, and clients that do not require a particular coordinate structure could be written generically.
