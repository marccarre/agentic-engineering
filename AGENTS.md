# Global Instructions for AI Agents

## General

1. Be concise — as concise as you can be.
2. Use simple words.
3. Use simple grammar.
4. Use positive sentences (e.g., instead of double-negatives).
5. Writing in ASD-STE100, or similar level of simplicity and clarity.
6. Do NOT use em-dashes, use normal dashes.

## Design

1. Write system requirements as [EARS] requirements:

   ```text
   [while <pre-condition>,]
   [when <trigger>,]
   [where <feature is included>,]
   [if <trigger>,]
   the <system name> shall <system response>
   ```

2. Write user requirements as [Connextra] user stories:

   ```text
   As a user, I want to be able to [...] so that I can [...]
   ```

3. Be concise.
4. Illustrate your design with UML diagrams in [Mermaid.js].

[Connextra]: https://agilealliance.org/glossary/user-story-template/
[EARS]: https://alistairmavin.com/ears/
[Mermaid.js]: https://mermaid.ai/open-source/intro/

## Development

1. Strictly follow Test-Driven Development.

   a. Structure and comment tests with Given/When/Then, so they read like requirements:

      ```python
      def test_abc() -> None:
          # Given:
          ...
          # When:
          ...
          # Then:
          ...
      ```

   b. Write the simplest, clearest, most maintainable code to satisfy the test.
   c. Then, refactor to make the code easier to maintain, follow design patterns, etc. (Do NOT skip the refactor step!)
   d. Then, run `/code-review --fix` to identify & fix what could be improved, still using TDD.
2. When finding a bug, start with:
   a. reproducing the bug in conditions as similar as possible to how the end user would experience it,
   b. add a test to reproduce the bug,
   c. fix the bug.
   This ensures you find the real problem, so that your fix will actually solve it.
3. If you see a linting failure, test failure, a flaky test, etc. fix it in a separate, atomic commit with JUST this
   change, even if not caused by what you are working on.
4. Create small, atomic commits following "[conventional commits]", with concise commit messages/descriptions:
   a. Unless told otherwise, commit TDD's Red/Green steps as one `feat` commit.
   b. Unless told otherwise, commit TDD's Refactor step as one `refactor` commit.
5. Always sign `git` commits with `gpg`.

[conventional commits]: https://www.conventionalcommits.org/en/v1.0.0/

## Python

1. Prefix functions internal to a module with `_`.
2. Do NOT use any prefix for functions used in other modules, except for usage in `tests`.
3. Prefix methods internal to a class with `_`.
4. Structure code so that each component/class/function does one thing, does it well, and does it only
   -- each with its own level of detail / its own level of abstraction. E.g.:

   ```python
   def handle_request(self: Self, request: HttpRequest) -> HttpResponse:
       """Handle a web request to register a new user."""
       # Each sub-function "does one thing"; how it does it stays internal to it.
       dto = self._parse_and_validate(request) # Sub-function #1.
       model = self._process(dto) # Sub-function #2.
       self._save_to_db(model) # Sub-function #3.
       resp = self._render_success_response(model) # Sub-function #4.
       return resp
   ```

5. Instrument code with `logging`, not `print`. Diagnostics go to `logging`, which writes to stderr.
   A command-line entry point may still write its own output to stdout, since that output is the result
   the user asked for.
6. Give every public module, class, function and method a docstring saying what it does, in one line where
   possible. Where one line is not enough, follow Google style and describe every argument under `Args:`.
   Types come from the annotations, so do NOT repeat them in the docstring: two copies would diverge. E.g.:

   ```python
   def greet(name: str | None = None) -> str:
       """Return a greeting, personalised when `name` holds more than whitespace."""


   def _save_to_db(self: Self, model: Model, *, dry_run: bool = False) -> Model:
       """Save the provided model object to the database, and returned the same
       model object augmented with any IDs returned by the database.

       Args:
           model: the object to save to the database.
           dry_run: run all steps before/after savings to the database, but actually save nothings.
       """
   ```

7. Annotate with the narrowest type that fits. Do NOT use `Any`, which turns off type checking wherever it spreads.

## Maintaining this file

- Keep this file for knowledge useful to almost every future agent session in this project.
- Do NOT repeat what the codebase already shows; point to the authoritative file or command instead.
- Prefer rewriting or pruning existing entries over appending new ones.
- When updating this file, preserve this bar for all agents and keep entries concise.
