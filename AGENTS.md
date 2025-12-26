# Repository Guidelines

## Important
- Never change texts in /content/posts folder without permission
- If there was anything wrong about the text like typos or more complex issues, please let me know before execute any change

## Project Structure & Module Organization
- Markdown-first repository; each post lives as a standalone `.md` file at the root or under `posts/` when you want to stage drafts separately.
- Existing article `01_functional_programming.md` uses numbered prefixes to keep reading order; follow the same `NN_topic.md` pattern with lowercase, underscores between words.
- Keep assets lightweight; prefer linking to external diagrams. If you must add images later, place them in `assets/` (create it if needed) and reference with relative paths.

## Build, Test, and Development Commands
- There is no build toolchain checked in; the repo tracks source Markdown only. Use your editor’s Markdown preview to review layout and links before committing.
- Optional local check for broken links from the shell: `rg -o \"https?://\\S+\" | xargs -n1 -I{} curl -I -L -o /dev/null -w \"%{http_code} %{url_effective}\\n\" {}`.

## Writing Style & Naming Conventions
- Language is Portuguese with a technical, diary-like tone; prefer concise paragraphs that explain reasoning over tutorial-style step lists.
- Use Markdown headings to structure sections; emphasize key terms with bold instead of all caps. Admonitions like `[!NOTE]` are welcome for context.
- Code blocks should be fenced with language hints (e.g., ```elixir```, ```ts```) and kept minimal; wrap lines around 100 characters to ease diff review.

## Testing Guidelines
- Manually scan for rendering issues in a Markdown preview and verify links resolve (see optional command above).
- Ensure code snippets compile in isolation when practical, or mark them as illustrative when they are partial.
- When introducing a new numbering prefix, ensure surrounding files still read in the intended order.

## Commit & Pull Request Guidelines
- No history yet; use short, imperative commit messages (e.g., `Add notes on explicit state`), keeping the subject under ~72 characters.
- Pull requests should include: purpose, scope of changes (new/updated posts), and a quick preview note or screenshot if layout is relevant.
- Link any related issues or TODOs; call out breaking content changes (renamed files, retitled posts) so readers can adjust bookmark.
