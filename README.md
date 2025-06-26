# OCaml Trade Tax Calculator

This OCaml project implements a sophisticated **capital gains tax calculator** for stock trading operations. It simulates real-world tax calculation rules by tracking stock purchases, sales, and applying tax only on net profits above a specified threshold.

## Overview

The calculator implements key financial concepts:
- **Weighted Average Cost Basis**: Maintains the average cost of held stocks across multiple purchases
- **Loss Offsetting**: Accumulates losses to offset future gains (tax loss harvesting)
- **Threshold-based Taxation**: Only applies tax to transactions above a minimum amount
- **Stateful Tracking**: Remembers portfolio state across multiple operations

## Core Functionality

### Data Types

```ocaml
type data = { unit_cost : float ; quantity : int }
type trade = Buy of data | Sell of data
```

### Tax Calculation Rules

1. **Buy Operations**: No tax applied, updates weighted average cost
2. **Sell Operations**: Tax calculated based on:
   - Profit/loss compared to weighted average cost
   - Transaction amount must exceed threshold (default: 20,000)
   - Net profit after offsetting accumulated losses
   - Tax rate applied to net profit (default: 20%)

### Example Workflow

**Input:**
```ocaml
let input = [
    Buy  { unit_cost = 10.; quantity = 100 };  (* Purchase 100 shares at $10 *)
    Sell { unit_cost = 15.; quantity = 50  };  (* Sell 50 shares at $15 *)
    Sell { unit_cost = 15.; quantity = 50  };  (* Sell remaining 50 shares at $15 *)
]
```

**Output:**
```ocaml
let output = [None; None; None]
(* No tax because transaction amounts are below threshold *)
```

**Large Transaction Example:**
```ocaml
let input = [
    Buy  { unit_cost = 10.; quantity = 10000 };  (* $100,000 purchase *)
    Sell { unit_cost = 20.; quantity = 5000  };  (* $100,000 sale, $50,000 profit *)
]
(* Output: [None; Some 10000.] - 20% tax on $50,000 profit *)
```

## Project Structure

- **`lib/tax.ml`** - Core tax calculation logic and state management
- **`lib/tax.mli`** - Public interface definitions
- **`bin/main.ml`** - Example usage with sample trading scenarios
- **`test/`** - Comprehensive test suite covering various edge cases

## Key Features

- ✅ **Stateful Calculation**: Maintains portfolio state across operations
- ✅ **Loss Accumulation**: Tracks losses to offset future gains
- ✅ **Configurable Parameters**: Customizable tax rate and threshold
- ✅ **Error Handling**: Prevents overselling stocks
- ✅ **Comprehensive Testing**: Multiple test scenarios validate correctness

---

# Config

If you already have OCaml installed in your OS you can skip this step and jump to [Create a Switch for Your ](#create-a-switch-for-your-project) section right away.

## Install Opam

### For macOS

```sh
brew install opam
```

### For Linux

```sh
sudo apt-get install opam
```

### For Windows

```sh
winget install Git.Git OCaml.opam
```

## Global Setup

Initialize OCaml's global configuration with Opam:

```sh
opam init -y
```

Activate the **Opam** global configuration by running:

```sh
eval $(opam env)
```

**Note:** add this command to your `.bashrc` or equivalent.

### Install Platform Tools

```sh
opam install ocaml-lsp-server odoc ocamlformat utop
```

---

# Create a Switch for Your Project

Create a switch with your selected version:

```sh
opam switch create . 5.3.0 --deps-only
```

## Activate the Switch

Activate the switch every time you enter the project:

```sh
eval $(opam env)
```

## Install Switch Tools

```sh
opam install ocaml-lsp-server odoc ocamlformat alcotest
```

---

# Run

## Build and Execute

Use watch mode to build and execute the program

```sh
dune exec -w ocaml-trade-tax
```

## Run Tests

```sh
dune test -w
```
