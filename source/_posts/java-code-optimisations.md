---
title: Java Code Optimisations
date: 2024-11-10 18:03:15
tags: [Java]
---

# Java Codes Optimisations

## Enum Operation

### Original

```java
// Operation.java
public enum Operation {
  ADD,
  SUBTRACT;
}

// Calculator.java
public class Calculator {
  public double eval(double a, double b, Operation operation) {
    if (Operation.ADD.equals(operation)) {
      return a + b;
    } else if (Operation.SUBTRACT.equals(operation)) {
      return a - b;
    } else {
      throw new IllegalStateException("Illegal operand.");
    }
  }
}
```

In the code shown above, behaviour and state are separated, not easy to maintain, a branch needs to be added for each additional state, poor readability and high cost of understanding. After adding abstract methods in the enumeration class, the logic is converged through the method overloading of the enumeration object to improve the readability and maintainability of the code.

### Optimised

```java
// Operation.java
public enum Operation {
  ADD() {
    @Override
    double eval(final double a, final double b) {
      return a + b;
    }
  },
  SUBTRACT() {
    @Override
    double eval(final double a, final double b) {
      return a - b;
    }
  };
  
  abstract double eval(double a, double b);
}

// Calculator.java
public class Calculator {
  public double eval(double a, double b, Operation operation) {
    return operation.eval(a, b);
  }
}
```


