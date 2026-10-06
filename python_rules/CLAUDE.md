# Python coding rules

Follow these in full, without being asked, in every task that writes or changes Python.
They assume Python 3.12 or newer. Where this project ships a linter config, that config
wins over any number stated here.

Project-specific facts — how to run the tests, what the packages hold, what must never
change — belong under a `## Project` heading appended to the end of this file, never mixed
into the rules above it.

## Object attributes

- Use snake_case for object attributes. Use a leading underscore for private attributes,
  and avoid a double leading underscore unless it is necessary.
- Do not create class or object attributes dynamically. Create object attributes in the
  constructor only.

## Language and tooling

- Target Python 3.12 or newer. Assume `tomllib`, `@override`, `type` aliases, `X | None`,
  and generics with `TypeVar(bound=...)` are available.
- Prefer a modern idiom when writing new code. Do not rewrite working code just to adopt one.
- Code must pass black, isort, flake8, and mypy. Run the project's `pre-commit` if it has one.
- Default line length is 100.

## Module and function shape

- A module is at most 160 lines. Split anything longer.
- The limit does not apply to a module that is a flat list of same-shaped items: constants,
  dataclasses, or near-identical functions with no branching logic between them.
- No function with deep nesting or many responsibilities. Extract instead.
- No wrapper that only forwards its arguments to another call. Properties, dunder methods,
  and protocol implementations may be one line.
- No function defined inside another function. Lift it to module level and pass what it
  needs as arguments. A closure is justified only where it must capture local state, such
  as a callback built once per call. A method is not a nested function, even where its
  class is local to one test.
- No `return` that hides a decision or a multi-step build: no conditional expression, and
  nothing assembled out of several intermediate results. Bind it to a named variable and
  return that name, so the value carries a name the reader and a debugger can both see. A
  single expression may be returned as it is, whether it is a literal, a name, one call, one
  comparison, or one comprehension.

## Typing

- Every function signature is fully annotated, arguments and return.
- Do not use `cast()` or runtime type coercion to satisfy the checker.
- Do not silence a checker with `# type: ignore` or a linter with `# noqa`. Fix the cause.
  If a third-party stub is genuinely missing, configure the exemption in the project config
  where it is visible, not inline.
- Avoid `Any`. Use a `TypeVar` with a bound for generic code, and a type alias for any
  signature that repeats a complex type.

## Docstrings and comments

- Every module opens with a docstring stating its purpose in one or two sentences.
- Every function and dataclass has a compact Google-style docstring. Describe arguments and
  any exception raised. Do not restate types, the annotations already carry them.
- A module implementing a mathematical operation states the formula in its docstring.
- Write a comment only where the code cannot explain itself. Never comment what is already
  obvious from the names.

## Organisation

- Structure code into purposeful modules, packages, and classes. Use inheritance and
  composition where they remove duplication.
- No dead code, no redundant abstraction, no duplicated logic, no unused import, variable,
  function, or class.
- All imports sit at the top of the module. Never import inside a function or method.
- Use a relative import for a sibling in the same package. Never go up more than one level.
- Import a package's submodule only when you need that submodule specifically.
- An `__init__.py` holds no logic and no hardcoded import path. Leave it empty unless the
  package genuinely needs initialisation.

## Values

- No magic number and no magic string. Name it as a constant, or put it in a config file or
  a dataclass.
- A constant used by one module is defined at the top of that module. A constant shared by
  several modules lives in its own module and is imported.

## Naming and order

- One leading underscore marks a private member. Use two only to avoid a real subclass
  collision.
- Within a module, public elements come before private ones.
- Within a class, dunder methods come first, then public members, then private ones.

## Errors and logging

- Log through the `logging` module at a level that matches the event. No `print` in
  production code, and never log a credential or a secret.
- `assert` is allowed only in test files.

## Testing

- Test the public API of a module or class. Do not reach for implementation details.
- Keep tests compact. Do not write extensive tests for trivial functions.
- Use fixtures for setup and teardown, and `pytest.mark.parametrize` for multiple inputs.
- Build real objects rather than mocks. Reserve patching for a genuine external boundary,
  such as the network, a paid service, or hardware the test environment lacks. When you
  must patch one, say so in the test name or a single comment.
- Never call `sleep()` in a test.

## Self-review before reporting done

- Re-read every file you changed and fix anything in it that breaks these rules. Do this on
  your own, as the last step of the task, without being asked.
- Refactor what your change made badly shaped: split a module that grew past the limit,
  merge logic your change duplicated, delete what your change orphaned.
- Restructure beyond the files you touched only when the task cannot be completed cleanly
  otherwise. When you do, say what you moved and why.
- Leave untouched code untouched. A rule violation elsewhere in the repo is something to
  report to the user, not to fix uninvited.
