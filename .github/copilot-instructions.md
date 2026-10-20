# Copilot instructions

## Project structure and architecture

The quiz is a standalone static page in the root `index.html`. It has no package manifest or build system: the document contains the markup, CSS, and JavaScript, and can be opened directly in a browser.

The quiz's question bank and zero-based correct-answer indexes live in the `questions` array in the inline script. The script owns the current question index, score, and answered state. `renderQuestion()` builds each set of answer buttons from the question data; `selectAnswer()` locks the choices, updates score and feedback, and advances the progress bar; `showResults()` replaces the quiz with the score-based result. Restart resets the state and renders the first question again.

## Commands and verification

There are no build, test, or lint commands configured. Open `index.html` directly in a browser to run the app. For a manual smoke test, check both correct and incorrect answers, score and progress updates, the final results at the end of all 10 questions, and restarting the quiz.

## Browser testing

For an interactive smoke test, open `index.html` in the integrated browser. Verify that answer buttons and the enabled Next button can be reached by keyboard, correct answers increment the score, incorrect answers do not, feedback and progress update, and completing all 10 questions shows the score-based result and emoji. Verify that replay resets the quiz.

Playwright is not a project dependency or configured test runner. If browser automation is needed, use the Playwright MCP server through the environment's MCP settings rather than adding dependencies to this standalone page. In a VS Code MCP configuration such as `.vscode/mcp.json`, the server entry is:

```json
{
  "servers": {
    "playwright": {
      "type": "http",
      "url": "https://api.mcp.github.com/v0.1/servers/microsoft%2Fplaywright-mcp/versions/0.0.1-seed"
    }
  }
}
```

## Project conventions

- Keep the app dependency-free and self-contained in `index.html`; there is no bundler, module system, or server requirement.
- Add or edit questions in the `questions` array. Each entry has a prompt, four answer strings, and a zero-based `correct` index. Keep score and feedback logic in the shared answer handler rather than embedding behavior in question data.
- Use CSS custom properties for shared colors and surfaces. The existing palette supports dark mode through `prefers-color-scheme`; preserve that behavior when introducing styles.
- Honor `prefers-reduced-motion` for animations. Use native buttons for answer and navigation controls, and retain the progress bar's accessible label and value attributes when changing quiz state.
- Answer choices are created dynamically. Keep inserted answer text as text content rather than treating question data as HTML.
