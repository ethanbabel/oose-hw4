# Task 2: Design Principles 1

## Compromised Design Principles

### Interface Segregation Principle (ISP)

Clients should not be forced to depend on methods they do not use. The `Payment` interface combines operations for different kinds of payments into one contract, even though those operations do not apply to every implementation.

- `LoanPayment` must implement `initiatePayments()`, although the provided code identifies this as a bank-payment operation. It throws `UnsupportedOperationException` because the operation does not apply to a loan payment.
- `BankPayment` must implement `intiateLoanSettlement()` and `initiateRePayment()`, although it cannot perform either loan-related operation. Both methods throw `UnsupportedOperationException`.
- A client that only needs payment status still depends on an interface that also exposes bank-payment, loan-settlement, and repayment operations.

The issue is that the interface groups capabilities that its implementations and clients do not all need. Throwing an exception satisfies Java's requirement to implement the methods, but it does not make the interface an appropriate abstraction.

### Liskov Substitution Principle (LSP)

The design also violates the Liskov Substitution Principle under the apparent contract that a `Payment` supports the operations declared by its interface. An implementation should be usable wherever its interface is expected without breaking the behavior promised by that interface.

For example, a client could reasonably write:

```java
void processPayment(Payment payment) {
    payment.initiatePayments();
}
```

Passing a `BankPayment` would invoke its supported bank-payment operation, but passing a `LoanPayment` would throw an exception because the operation is inherently unsupported. Likewise, a client calling `intiateLoanSettlement()` through `Payment` cannot safely substitute a `BankPayment`. The problem is that entire implementations cannot support operations exposed by their shared abstraction.

## Revised Design

I would retain the common status operation in a small `Payment` interface and move the specialized operations into separate interfaces. Based on the assignment, bank payments support initiation, while loan payments support settlement and repayment.

```java
public interface Payment {
    Object status();
}

public interface BankPaymentOperations extends Payment {
    void initiatePayments();
}

public interface LoanPaymentOperations extends Payment {
    void initiateLoanSettlement();
    void initiateRePayment();
}
```

Clients would depend on the interface that provides the capabilities they actually require:

```java
class PaymentService {
    public Object readStatus(Payment payment) {
        return payment.status();
    }

    public void initiateBankPayment(BankPaymentOperations payment) {
        payment.initiatePayments();
    }

    public void settleLoan(LoanPaymentOperations payment) {
        payment.initiateLoanSettlement();
    }

    public void repayLoan(LoanPaymentOperations payment) {
        payment.initiateRePayment();
    }
}
```

A `LoanPayment` could no longer be passed to `initiateBankPayment()`, and a `BankPayment` could no longer be passed to `settleLoan()` or `repayLoan()`. These incompatible calls would be rejected at compile time instead of failing at runtime. Either class could still be passed to `readStatus()`.

This design satisfies ISP by separating the bank and loan capabilities, and it supports LSP by making each implementation responsible only for operations it can fulfill. I grouped settlement and repayment because the supplied example treats both as loan-payment capabilities. If future implementations or clients need them independently, I would split them into separate settlement and repayment interfaces as well.
