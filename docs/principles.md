# Design Principles

These principles explain why `brandkit.md` is designed the way it is.

## 1. Human-readable first

A `brandkit.md` file must remain understandable as plain Markdown, even without specialized tools.

## 2. AI-readable by design

Section names and field labels are intentionally stable so LLMs and agents can use context consistently.

## 3. Markdown-native

No new syntax, no proprietary containers, and no hard dependency on renderer-specific features.

## 4. Progressively structured

v0.x allows pragmatic free-form text plus clear section conventions. Structure can tighten in later versions without breaking the core model.

## 5. Portable across tools

Core meaning must not depend on color, SVG, or a specific UI. Essential information must stay portable in plain text.

## 6. Versionable in Git

The specification and its examples should be easy to review, diff, and version in pull requests.

## 7. Easy to author, easy to extend

The entry barrier should stay low. Extensions are allowed, but required fields stay compact and purpose-driven.

## 8. Curated identity over auto-generated noise

Core brand elements are human-curated. Tooling may propose candidates later, but does not replace editorial approval.
