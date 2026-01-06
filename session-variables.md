# Session Variables

Fill these in at the start of each work session. The Generator Prompt will use these values.

```
PROJECT:        
VERSION:        
PREV_VERSION:   
BRANCH:         
SPEC_PATH:      
PR_URL:         
```

## Example

```
PROJECT:        oh.gold
VERSION:        v1.0
PREV_VERSION:   N/A
BRANCH:         main
SPEC_PATH:      docs/v1.0-spec.md
PR_URL:         https://github.com/ohjona/oh.gold/pull/1
```

## Variable Descriptions

| Variable | When to Fill | Description |
|----------|--------------|-------------|
| `PROJECT` | Session start | Project name |
| `VERSION` | Session start | Version you're building |
| `PREV_VERSION` | Session start | Previous version (or N/A) |
| `BRANCH` | Session start | Git branch name |
| `SPEC_PATH` | After spec written | Path to implementation spec |
| `PR_URL` | After PR created | Pull request URL |
