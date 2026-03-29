# Contributing

![Open Source](https://img.shields.io/badge/collaboration-welcome-1f6feb)
![Privacy](https://img.shields.io/badge/privacy-sanitized-2ea043)
![Docs](https://img.shields.io/badge/focus-documentation-8250df)

Thanks for helping improve this project.

## Contribution flow

```mermaid
flowchart LR
    A[Identify problem or improvement] --> B[Prepare focused change]
    B --> C[Sanitize screenshots, logs, and examples]
    C --> D[Explain what changed]
    D --> E[Open pull request]
```

## Before you contribute

Please keep contributions:

- practical
- reproducible
- privacy-conscious
- easy to review

Good contributions include:

- installation improvements
- troubleshooting fixes
- clearer wording
- support for additional safe configurations
- documentation corrections
- Linux and Windows path clarification

## Please do not submit

- private keys
- credentials
- tokens
- sensitive internal IPs or hostnames
- logs containing secrets
- destructive automation without clear warnings

## Preferred contribution style

- keep changes focused
- explain what problem the change solves
- include exact reproduction steps when reporting a bug
- sanitize screenshots and logs before uploading

## Pull request checklist

When opening a pull request, include:

1. what you changed
2. why you changed it
3. how you tested it
4. whether the change affects security or long-running command behavior

## Documentation standards

When adding commands:

- explain what the command is trying to verify
- explain expected output when possible
- warn users before risky or system-changing actions

## Code of conduct

Be respectful and constructive. The goal is to help people build a working setup safely.
