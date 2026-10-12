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

## Maintaining this file

- Keep this file for knowledge useful to almost every future agent session in this project.
- Do NOT repeat what the codebase already shows; point to the authoritative file or command instead.
- Prefer rewriting or pruning existing entries over appending new ones.
- When updating this file, preserve this bar for all agents and keep entries concise.
