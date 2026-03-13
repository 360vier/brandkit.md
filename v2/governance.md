# BrandKit Governance

Defines how BrandKit rules are changed and validated.

## Versioning

- Use semantic versioning for BrandKit rules.
- Record rule changes in release notes.

## Change Ownership

- Core files require approval from brand owners.
- Subbrand files require approval from subbrand owners.
- Product files require approval from product owners.

## Review Checklist

- Portability preserved
- Inheritance rules respected
- Emoji and token rules consistent
- No conflicting terminology introduced

## Conflict Resolution

- Core rule conflicts are resolved in favor of portability.
- Layer conflicts are resolved by nearest allowed override.
- If no override is explicitly allowed, inherit parent value.
