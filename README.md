# TruthTrace

> Follow the logic. Defeat the AI.

TruthTrace is a small Python tool that checks whether a conclusion *necessarily* follows from a set of premises. When an argument fails, it shows the exact truth assignment (the counterexample) where the premises hold but the conclusion is false, so you can trace the logic step by step.

It also includes a quiz mode where an "AI" presents arguments and you try to spot the flawed ones. Beat it enough times and you win.

## Features

- Parses propositional logic: `not`, `and`, `or`, `->`, `<->`, and parentheses
- Checks argument validity with a full truth table
- Prints a counterexample trace when an argument is invalid
- Quiz mode with classic valid and invalid forms (modus ponens, affirming the consequent, and so on)
- Zero dependencies, Python 3.8+

## Installation

```bash
git clone https://github.com/<your-username>/truthtrace.git
cd truthtrace
```

No packages to install.

## Usage

### Check an argument

```bash
python truthtrace.py check -p "P -> Q" -p "P" -c "Q"
```

Output:

```
VALID: the conclusion 'Q' follows from the premises.
```

An invalid argument:

```bash
python truthtrace.py check -p "P -> Q" -p "Q" -c "P"
```

Output:

```
INVALID: 'P' does not follow from the premises.
Counterexample: P=F, Q=T
  Premise 'P -> Q' is True
  Premise 'Q' is True
  Conclusion 'P' is False
```

### Play the quiz

```bash
python truthtrace.py quiz --rounds 5
```

The AI presents an argument. You answer whether the conclusion necessarily follows. Each correct call scores a point against the AI.

## Formula syntax

| Operator | Meaning           | Example       |
|----------|-------------------|---------------|
| `not`    | negation          | `not P`       |
| `and`    | conjunction       | `P and Q`     |
| `or`     | disjunction       | `P or Q`      |
| `->`     | implication       | `P -> Q`      |
| `<->`    | biconditional     | `P <-> Q`     |
| `( )`    | grouping          | `(P or Q) -> R` |

Precedence from highest to lowest: `not`, `and`, `or`, `->`, `<->`. Implication is right-associative, so `P -> Q -> R` means `P -> (Q -> R)`.

Variables are any identifier starting with a letter or underscore.

## Running tests

```bash
python -m unittest discover tests
```

## How it works

An argument is valid if there is no assignment of truth values in which every premise is true and the conclusion is false. TruthTrace enumerates all `2^n` assignments of the `n` variables to check this. That is exhaustive, so it is exact for propositional logic, but it grows exponentially with the number of variables, so it suits small arguments.

Note that an argument with contradictory premises is vacuously valid, because no assignment makes all premises true. TruthTrace reports this as valid, as standard logic does.

## Project structure

```
truthtrace/
├── README.md
├── truthtrace.py
└── tests/
    └── test_truthtrace.py
```

## License

MIT
