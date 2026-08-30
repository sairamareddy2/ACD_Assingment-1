# Automata Theory — DFA, NFA & NFA to DFA

A Python-based implementation of three fundamental **Automata Theory** concepts:

1. Deterministic Finite Automaton (DFA)
2. Nondeterministic Finite Automaton (NFA)
3. NFA to DFA conversion using **Subset Construction**

The complete implementations and sample outputs are provided in the Jupyter/Google Colab notebook.

---

## 📌 Project Overview

This project demonstrates how finite automata can be represented, simulated, and converted programmatically using Python.

### Programs Included

| # | Program | Main Concept |
|---|---|---|
| 1 | DFA Simulator | Deterministic state transition and string acceptance |
| 2 | NFA Simulator | Multiple active states and nondeterministic paths |
| 3 | NFA → DFA Converter | Subset construction / powerset method |

---

# 1. Deterministic Finite Automaton (DFA)

## Question

Implement a universal DFA simulator that accepts:

- A finite set of states
- An input alphabet
- A start state
- Accept states
- A transition function

The simulator should process an input string and determine whether it is accepted or rejected.

## DFA Definition

The DFA is represented using the 5-tuple:

**M = (Q, Σ, δ, q₀, F)**

Where:

- **Q** — finite set of states
- **Σ** — input alphabet
- **δ : Q × Σ → Q** — transition function
- **q₀** — start state
- **F** — set of accepting states

## Features

- Interactive DFA configuration
- Transition table generation
- Input string simulation
- Visual state-flow trace
- Accepted/rejected verdict
- Invalid-symbol detection
- No external Python libraries required

## Test Case 1 — Accepted

### DFA Configuration

```text
Alphabet: 0 1
States: q0 q1 q2
Start State: q0
Accept State: q2
```

### Transition Table

| State | 0 | 1 |
|---|---|---|
| q0 | q1 | q0 |
| q1 | q1 | q2 |
| q2 | q2 | q2 |

### Input

```text
11010
```

### State Flow

```text
q0 → q0 → q0 → q1 → q2 → q2
```

### Result

```text
ACCEPTED (Valid Member)
```

## Test Case 2 — Rejected

### Input

```text
0000
```

### State Flow

```text
q0 → q1 → q1 → q1 → q1
```

The final state is `q1`, which is not an accepting state.

### Result

```text
REJECTED (Invalid)
```

## Test Case 3 — Invalid Input Symbol

### Input

```text
exist
```

The alphabet is:

```text
0 1
```

Therefore, the symbol `e` is invalid.

### Result

```text
Invalid symbol 'e' not in alphabet.
```

---

# 2. Nondeterministic Finite Automaton (NFA)

## Question

Implement a universal NFA simulator that allows multiple possible transitions for the same state and input symbol.

The simulator should determine whether **at least one possible path** reaches an accepting state.

## NFA Definition

The NFA is represented using:

**M = (Q, Σ, δ, q₀, F)**

where the transition function is:

**δ : Q × (Σ ∪ {ε}) → P(Q)**

This allows a state and input symbol to have multiple destination states.

## Features

- Multiple transitions for the same input
- Parallel branch execution
- Epsilon (`ε`) transition support
- Epsilon closure calculation
- Active-state tracking
- Transition table generation
- Accepted/rejected verdict
- Invalid-symbol detection

## Test NFA Configuration

```text
Alphabet: 0 1
States: q0 q1 q2 q3 q4
Start State: q0
Accept States: q2 q4
```

### Transition Table

| State | 0 | 1 |
|---|---|---|
| q0 | {q0,q1} | {q0,q3} |
| q1 | ∅ | {q2} |
| q2 | ∅ | ∅ |
| q3 | {q4} | ∅ |
| q4 | ∅ | ∅ |

## Test Case 1 — Accepted

### Input

```text
1110
```

### Active-State Flow

```text
{q0}
  ↓ 1
{q0,q3}
  ↓ 1
{q0,q3}
  ↓ 1
{q0,q3}
  ↓ 0
{q0,q1,q4}
```

Since `q4` is an accepting state:

```text
ACCEPTED
```

## Test Case 2 — Rejected

### Input

```text
000
```

### Active-State Flow

```text
{q0}
  ↓ 0
{q0,q1}
  ↓ 0
{q0,q1}
  ↓ 0
{q0,q1}
```

No active path reaches an accepting state.

### Result

```text
REJECTED
```

---

# 3. NFA to DFA Conversion

## Question

Convert a given NFA into an equivalent DFA using the **Subset Construction** method.

The program creates DFA states from sets/subsets of NFA states.

## Conversion Method

The converter performs the following main operations:

1. Calculate the epsilon closure of the NFA start state.
2. Use this closure as the DFA start state.
3. Apply the `move` operation for each input symbol.
4. Calculate the epsilon closure of the resulting states.
5. Treat every unique subset as a DFA state.
6. Mark a DFA state as accepting if its subset contains an NFA accepting state.
7. Continue until all reachable subsets have been processed.

## Test Case

### NFA Configuration

```text
Alphabet: 0 1
States: q0 q1 q2 q3
Start State: q0
Accept State: q3
```

### NFA Transitions

```text
δ(q0, 0) → q0
δ(q0, 1) → q0
δ(q0, ε) → q1

δ(q1, 0) → q2
δ(q1, 1) → ∅
δ(q1, ε) → ∅

δ(q2, 0) → ∅
δ(q2, 1) → q3
δ(q2, ε) → ∅

δ(q3, 0) → q3
δ(q3, 1) → q3
δ(q3, ε) → ∅
```

## Equivalent DFA

### DFA States

```text
A, B, C, D
```

### DFA Alphabet

```text
0 1
```

### DFA Start State

```text
A
```

### DFA Accept States

```text
C, D
```

## Subset Mapping

| DFA State | NFA State Subset |
|---|---|
| A | {q0, q1} |
| B | {q0, q1, q2} |
| C | {q0, q1, q3} |
| D | {q0, q1, q2, q3} |

## DFA Transition Table

| DFA State | 0 | 1 |
|---|---|---|
| A | B | A |
| B | B | C |
| C | D | C |
| D | D | C |

The notebook generates this equivalent DFA automatically using subset construction.

---

# 🎥 Demo Video

A demonstration video explaining/showing the project is available here:

**[▶ Watch Project Demonstration](https://drive.google.com/file/d/1Ti3azL2f0G2pET_95u557Wzj4WPNuDYN/view?usp=sharing)**

The video can be used to see the programs and their execution in practice.

---

# 📂 Project Structure

```text
ACD/
│
├── ACD_1.ipynb
└── README.md
```

---

# ▶️ How to Run

## Google Colab

1. Open `ACD_1.ipynb` in Google Colab.
2. Run the DFA cell for DFA simulation.
3. Run the NFA cell for NFA simulation.
4. Run the NFA → DFA cell for subset construction.
5. Enter the requested automaton details when prompted.
6. Enter test strings where applicable.

## Jupyter Notebook

Install Jupyter Notebook if required:

```bash
pip install notebook
```

Then run:

```bash
jupyter notebook ACD_1.ipynb
```

---

# 🛠️ Technologies Used

- **Python 3**
- **Google Colab**
- **Jupyter Notebook**
- Automata Theory
- Finite Automata
- Subset Construction

No external libraries are required for the DFA and NFA simulators; the NFA→DFA converter uses Python's standard library components.

---

# 📚 Concepts Demonstrated

- Deterministic Finite Automata
- Nondeterministic Finite Automata
- DFA transition functions
- NFA transition relations
- Epsilon transitions
- Epsilon closure
- State simulation
- String acceptance
- Parallel NFA paths
- Subset Construction
- NFA to DFA equivalence

---

# 👤 Author

**Mallidi Sscvv Ramakrishna Reddy**

Student Project — Automata & Compiler Design
