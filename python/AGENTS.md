# Python Project Instructions for AI Agents

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
