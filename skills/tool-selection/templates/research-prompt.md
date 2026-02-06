# Research-First Prompt Template

Copy and customize this prompt for Claude Chat when you need to research unfamiliar technology before building.

## Base Research Prompt

```
I'm building [PROJECT DESCRIPTION] and need to understand [TOPIC] before I start implementing.

## Context
- Project: [brief project description]
- What I'm building: [the feature/component that needs this technology]
- My current stack: [languages, frameworks, tools already in use]
- Constraints: [time, budget, team size, hosting, performance requirements]

## What I Need to Understand
1. How does [TOPIC] work at a high level?
2. What are the main approaches/libraries/tools for [TOPIC]?
3. What are the trade-offs between them?
4. What gotchas or common pitfalls should I watch for?
5. What's the simplest path to a working implementation?

## Evaluation Criteria
- Integration with my existing stack
- Learning curve and documentation quality
- Community support and maintenance status
- Performance characteristics for my use case

## Desired Output
Give me:
1. A recommended approach with justification
2. A concrete implementation outline (files, dependencies, key code patterns)
3. A list of things to test/verify early (potential blockers)
4. Copy-paste starter snippets if applicable
```

## Variant: Evaluating a Library/Tool

```
I need to choose a [CATEGORY] for my [PROJECT] project.

## Candidates
1. [Library A] — [what I know about it]
2. [Library B] — [what I know about it]
3. [Library C or "open to suggestions"]

## My Requirements
- Must support: [critical features]
- Nice to have: [optional features]
- Constraints: [size limits, license, compatibility]

## Evaluate Each On
- API ergonomics (show me a typical usage example)
- Bundle size / dependency weight
- Maintenance health (last release, open issues)
- How it handles [my specific tricky use case]

## Output
Recommendation with a concrete "getting started" plan I can hand to Claude Code.
```

## Variant: Understanding a Pattern/Architecture

```
I need to implement [PATTERN/ARCHITECTURE] in my [PROJECT].

## What I Know
- [what you currently understand]
- [what you've tried or read]

## What I Don't Understand
- [specific confusion points]
- [where existing docs/examples fall short]

## My Specific Case
- [how your project differs from typical examples]
- [constraints that might affect the standard approach]

## Output
1. Explain the pattern in terms of my specific project
2. Show how it maps to my existing code structure
3. Identify the minimal set of changes needed
4. Flag anything that might not work for my case
```
