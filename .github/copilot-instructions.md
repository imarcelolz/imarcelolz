# Documentation Style Guide

This file defines the tone, language, and structure for all documentation in this portfolio. Use these guidelines when writing or updating project documentation.

## Voice & Tone

- **Professional but approachable**: Write like you're explaining to a fellow engineer over coffee.
- **First person**: Use "I" when telling the story behind a project.
- **Direct and concise**: Get to the point. No fluff.
- **Show personality**: It's okay to mention that you built something "for fun" or because you "didn't want to buy the commercial option."

## Document Structure

Every project page should follow this structure:

```markdown
# Project Name

One-line description of what it is and what it's built with.

![Project Image](../images/project.png)

## The Problem / The Story

Why does this project exist? What problem does it solve?
Personal motivation is welcome here.

## The Solution / How It Works

Brief explanation of the approach.

## Architecture

ASCII diagram showing system components and data flow.

## Hardware / Tech Stack

Table listing components, technologies, and their purpose.

## Software Design (if applicable)

Key design patterns, state machines, or architectural decisions.

## API / Protocol (if applicable)

How to interact with the project. Tables for endpoints or commands.

## Getting Started

Quick setup instructions (clone, install, run).

## Future Improvements

Checkbox list of planned features.

## Links

- GitHub repository
- External references

---

[← Back to Projects](/)
```

## Formatting Guidelines

### Headers
- `#` for project title only
- `##` for main sections
- `###` for subsections

### Tables
Use tables for:
- Hardware components (Component | Model | Purpose)
- Pin configurations
- API endpoints
- Feature comparisons

### Code Blocks
- Use triple backticks with language identifier
- Keep examples short and practical
- Pseudocode is fine for illustrating concepts

### ASCII Diagrams
Use ASCII art for architecture diagrams:
```
┌──────────┐     ┌──────────┐     ┌──────────┐
│  Input   │ ──▶ │ Process  │ ──▶ │  Output  │
└──────────┘     └──────────┘     └──────────┘
```

### Emojis
- Use sparingly in README files for visual scanning
- Avoid in detailed documentation pages
- Good for: ✅ ⬆️ 📶 🔌 🔋 💡 🚧

### Links
- Always link to GitHub repos
- Link back to homepage at the bottom of each project page

## Language Patterns

### Do
- "I bought a desk without a motor and decided to add one."
- "While a simple PWM controller would have done the job, I wanted an excuse to write some C++."
- "For venues where Bluetooth is unreliable, StompLink also includes a wired fallback."

### Don't
- "This project leverages cutting-edge technology to synergize..."
- "The system utilizes a microcontroller-based solution..."
- Overly formal corporate speak

## Project Status

For work-in-progress projects, include a status checklist:

```markdown
## Project Status

🚧 **Work in Progress**

- [x] Concept and design
- [x] Bill of materials
- [ ] Hardware assembly
- [ ] Firmware development
```

## Homepage Entry

Each project on the homepage should have:
- Linked title
- Thumbnail image
- One-sentence description (what it is + key tech)

Example:
```markdown
### [ProjectName](projects/project-name)

![ProjectName](images/project.png)

One sentence describing what it does and the main technology used.
```
