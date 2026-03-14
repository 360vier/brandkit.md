# FAQ

## Is `brandkit.md` a file format, a convention, or a standard?

It is currently a convention with a formal specification in progress (standard-in-progress).

## Why a single file instead of many files?

One file reduces complexity and makes adoption easier. Teams can still manage multiple profiles via multiple `*.brandkit.md` files.

## Does `brandkit.md` replace brand guidelines?

No. `brandkit.md` covers AI- and text-oriented brand context, not a full visual identity rulebook.

## Do I need YAML frontmatter?

No. Frontmatter is optional. The specification remains valid without it.

## Why is `brand expressions` required?

AI and terminal contexts need reduced renderings. Without explicit fallbacks, brand identity degrades in text-heavy channels.

## Is this stable for production-grade parsers?

Partially. v0.x is intentionally semi-structured. Strict validators may require additional normalization.

## What about `brand.md` and `styles.md`?

They are legacy artifacts from the early project phase. For new implementations, `SPEC.md` plus `brandkit.md` is the canonical path.

## Non-goals

- Not a marketing manifesto
- Not a heavyweight metadata framework
- Not a claim of immediate universal tooling support
