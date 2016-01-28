# commit-lint

Lint your commit messages.

**Status**: *Design* (please contribute!)

## Goals

- Sensible defaults, should be as easy as running in the `commit-msg` hook
- Should support the git and angular styles out of the box
- Configurable to support required lines (eg, ticket reference or change-id)
- Maybe, configurable enough to support a custom syntax

## Design

- Configurability suggests that each lint should be a set of functions run against the text. If any throws, we have a fail (but all must be allowed to finish)
- Within that, helpers to assist in matching line formats would be useful. Grammars are a little heavy, regex is a little hard and not very composable. But regex might work initially.

Here's one idea:

```bash
# .git/hooks/commit-msg
node_modules/.bin/commit-lint $1 --config config/commit-lint.js
```

The config file would look like:

```js
// config/commit-lint.js
import { subjectLine, body, anyLine, matching } from 'commit-lint';

export default {
  checks: [
    subjectLine({ length: 50, capitalized: true, period: false }),
    body({ wrap: 72, startsAtLine: 3 }),
    anyLine(matching(/^Jira: .+/)),
    anyLine(matching(/^Change-Id: .+/))
  ]
};
```
