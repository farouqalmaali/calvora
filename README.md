<div align="center">

CALVORA
A scientific calculator built around a structured expression engine.
A browser-based calculator with a dedicated lexer, parser, AST, validator, and evaluator — designed to process mathematical expressions without executing them as JavaScript.

Live Demo · Source Code





</div>

Table of Contents
- Overview
- Highlights
- Expression Processing
- Supported Operations
- Error Handling
- Security Design
- History and Persistence
- Accessibility and Interface
- Keyboard Shortcuts
- Technology Stack
- Getting Started
- Project Structure
- Testing
- Roadmap
- Author
- License
Overview
CALVORA is a browser-based scientific calculator built with Vue 3 and TypeScript. It is designed as a calculation workspace rather than a simple collection of calculator buttons.

At its core is an expression-processing pipeline that tokenizes input, parses its structure into an Abstract Syntax Tree (AST), validates it, and evaluates the resulting tree. The calculation engine is kept separate from the Vue interface so its logic can be tested independently.

Project principles:
- Structured expression processing instead of executing user input as JavaScript.
- Separation between the calculation engine and the user interface.
- Explicit validation and structured errors.
- Predictable mathematical precedence and controlled complexity.
- Responsive design, keyboard support, and persistent local preferences.
Highlights
- Standard and scientific calculator modes.
- A dedicated Lexer, Parser, AST, Validator, and Evaluator.
- Arithmetic, powers, percentages, implicit multiplication, and parentheses.
- Scientific functions, constants, and degree/radian angle modes.
- Previous-answer support through ans.
- Live expression feedback and contextual error reporting.
- Calculation history with search, reuse, copy, and deletion.
- Local persistence for history and preferences.
- Light and dark themes.
- Keyboard shortcuts and responsive layouts.
- Reduced-motion support and accessibility considerations.
- No eval() or new Function() in the expression-processing pipeline.
Expression Processing
CALVORA processes expressions through a controlled sequence:
Input
  ↓
Lexer
  ↓
Tokens
  ↓
Parser
  ↓
Abstract Syntax Tree (AST)
  ↓
Validator
  ↓
Evaluator
  ↓
Formatted Result
Lexer
Converts the input string into tokens and records token positions. It also normalizes supported alternative mathematical symbols into representations understood by the engine.
Parser
Reads the tokens and constructs an AST. It handles parentheses, operator precedence, functions, implicit multiplication, and postfix operators.
Abstract Syntax Tree
Represents the structure of an expression as typed nodes. This lets CALVORA validate and evaluate the mathematical structure rather than treating the input as executable source code.
Validator
Checks expression structure, identifiers, function availability, function argument counts, and other constraints before evaluation.
Evaluator
Walks the validated AST and evaluates its nodes in the intended order, returning a numerical result or a structured error.
Formatter
Formats results for display, including trimming unnecessary trailing zeros and using scientific notation where appropriate.
Supported Operations
Standard mode
- Addition, subtraction, multiplication, and division.
- Exponentiation using ^.
- Percentages using %.
- Parentheses and decimal numbers.
- Unary positive and negative signs.
- Implicit multiplication, such as 2(3 + 4) and (1 + 2)(3 + 4).
Scientific mode
The scientific mode supports functions including:
Category	Functions
Trigonometric	sin, cos, tan
Inverse trigonometric	asin, acos, atan, atan2
Hyperbolic	sinh, cosh, tanh
Logarithmic	log, ln, log2
Exponential	exp
Roots	sqrt, cbrt
Powers	pow
Other	abs, floor, ceil, round, min, max, mod
Factorial	fact and postfix !


Supported mathematical constants include π, pi, e, τ, and tau.
Angle modes
Trigonometric calculations can use either degrees or radians. The selected mode affects how angle-based functions interpret their arguments.
Expression syntax
The expression engine supports:
- Integers and decimal numbers, including .5.
- Scientific notation, such as 1e3 and 1.5E-3.
- Greek mathematical symbols, including π and τ.
- Alternative operators, including ×, ÷, and −.
- Function names without case sensitivity.
- ans to reference the previous result.
Operator precedence
The parser applies defined precedence rules for postfix operators, exponentiation, unary signs, multiplication and division, and addition and subtraction.

Examples:
2^3^2 = 512
-2^2 = -4
(-2)^2 = 4
2^-2 = 0.25
Percentage behavior
Percentages are supported in ordinary expressions and in calculator-style addition and subtraction contexts. Examples include:
50% = 0.5
200*10% = 20
200+10% = 220
200-10% = 180
Error Handling
CALVORA uses structured errors rather than relying only on generic failure messages. Errors can identify the processing stage and relevant position in the expression.

Handled error categories include:
- Empty or overlong expressions.
- Unknown tokens and invalid numbers.
- Missing or unexpected parentheses.
- Unknown functions and identifiers.
- Incorrect function argument counts.
- Division by zero.
- Mathematical domain errors.
- Numeric limits and excessive nesting depth.
Security Design
The expression engine does not use eval() or new Function() to execute calculator input. Instead, expressions pass through dedicated parsing, validation, and evaluation stages.

The implementation also uses controlled function and constant lookup, rejects dangerous identifiers such as constructor and __proto__, and applies limits to expression length and nesting depth.

These are design measures, not a claim that any software is completely secure. Review the source code and tests before relying on the project in a security-sensitive environment.
History and Persistence
The calculator can keep a local history of successful calculations. Users can search previous entries, reuse an expression, copy a result, remove individual entries, or clear the history.

History and preferences are stored in the browser's localStorage. Stored data is validated when loaded, and the application is designed to tolerate unavailable or incompatible local storage without treating it as a fatal application error.

Local browser storage is not cloud synchronization: data is associated with the browser and origin where it was saved.
Accessibility and Interface
CALVORA includes interface features intended to support different devices and interaction preferences:
- Responsive layouts for desktop and mobile.
- Light and dark themes.
- Semantic interface elements and clearly labelled controls.
- Visible keyboard focus.
- Keyboard navigation.
- A skip-to-content link.
- Reduced-motion support.
- Screen-reader announcements for selected interface updates.
Keyboard Shortcuts
Shortcut	Action
/	Focus the expression field
Enter	Evaluate the expression
Esc	Clear the expression
↑ / ↓	Navigate calculation history
Alt + 1	Switch to Standard mode
Alt + 2	Switch to Scientific mode
Alt + D	Toggle degrees/radians


Technology Stack
- Vue 3 — interface components.
- TypeScript — calculation engine, application logic, and typed state.
- Vite — development server and production build.
- Vitest — automated tests.
- Vue TSC — Vue and TypeScript type checking.
- CSS — responsive styling and themes.
- localStorage — browser-local history and preferences.
Getting Started
Prerequisites
Install a supported version of Node.js and npm.
Clone the repository
git clone https://github.com/farouqalmaali/calvora.git
cd calvora
npm install
Run the development server
npm run dev
Open the local URL printed by Vite in your terminal.
Run tests
npm run test
If the repository defines a different test script, use the exact script listed in package.json.
Type-check and build
npm run type-check
npm run build
The available scripts are defined in package.json; check that file if a command is not present in your checkout.
Project Structure
The following is a high-level guide to the main areas of the codebase. Exact filenames may change as the project evolves.
frontend/
├── core/
│   ├── lexer/          # Tokenization
│   ├── parser/         # Expression parsing
│   ├── ast/            # AST node definitions
│   ├── validation/     # Expression validation
│   ├── evaluator/      # Mathematical evaluation
│   ├── errors/         # Structured errors
│   └── format/         # Result formatting
├── modules/
│   ├── standard/       # Standard calculator mode
│   └── scientific/     # Scientific calculator mode
├── stores/             # Calculator state, history, preferences
├── services/           # Persistence and related services
├── components/         # Vue interface components
└── composables/        # Reusable Vue composition logic

tests/                  # Automated tests
docs/                   # Architecture and expression-language documentation
Refer to the current repository for the authoritative, up-to-date file structure.
Testing
The project includes tests for areas such as tokenization, parsing, AST behavior, operator precedence, scientific functions, error handling, decimal calculations, persistence, history, and state stores.

A previous project check reported successful Vue/TypeScript type checking, 357 passing tests, and a successful production build. These are historical results, not a guarantee that the current checkout passes; run the relevant commands to verify the current version.
Roadmap
Potential future improvements may include:
- Expanding expression-language documentation and examples.
- Continuing to improve edge-case and regression test coverage.
- Refining keyboard navigation and accessibility.
- Improving calculator history workflows.
- Maintaining performance and input-complexity limits as the expression engine evolves.
Author
Farouq Al-Maali
- GitHub: farouqalmaali
- Project repository: calvora
- Live demo: calvora.vercel.app
License
CALVORA is proprietary software owned by Farouq Al-Maali.

Personal, non-commercial use and private personal modifications are permitted only under the terms of the accompanying LICENSE file.

Commercial use, redistribution, and publication of modified versions require prior written permission from the copyright holder.

Copyright (c) 2026 Farouq Al-Maali. All rights reserved.

Important: This README assumes that a matching LICENSE file exists in the repository. Add and review the custom license before publishing this README. The license is a draft and should be reviewed by a qualified legal professional before final adoption.
<div align="center">

CALVORA

Calculation as a workflow, not a row of buttons.
</div>
