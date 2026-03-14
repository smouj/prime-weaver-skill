title: Prime Weaver
description: AI-powered weaver for intelligent writing tasks, including content generation, documentation, and creative writing automation.
author: OpenClaw Development Team
version: 1.2.0
tags: writing, ai, automation, content-generation, documentation, creative-writing
category: productivity
dependencies: openai-api, transformers-library, python-3.9+
environment_variables:
  - PRIME_WEAVER_API_KEY: OpenAI API key for content generation
  - PRIME_WEAVER_MODEL: Default model (e.g., gpt-4o)
  - PRIME_WEAVER_TONE: Default writing tone (e.g., professional, casual, technical)
license: MIT
repository: https://github.com/openclaw/prime-weaver
---

# Prime Weaver Skill

## Purpose

Prime Weaver is an AI-powered writing assistant designed to automate and enhance various writing tasks for developers, content creators, and technical writers. It leverages advanced language models to generate high-quality content while maintaining context, style, and accuracy. Real use cases include:

- **Blog Post Generation**: Creating SEO-optimized blog articles from topic outlines, including introductions, body sections, and call-to-action conclusions.
- **Code Documentation**: Writing comprehensive docstrings, README files, and API documentation with examples and usage instructions.
- **Technical Tutorials**: Producing step-by-step guides for software tools, libraries, or frameworks, complete with code snippets and explanations.
- **Email and Communication Drafting**: Composing professional emails, newsletters, or internal memos with customizable tones.
- **Creative Writing**: Generating stories, poems, or marketing copy based on themes and character descriptions.
- **Content Optimization**: Rewriting existing text for clarity, brevity, or to match specific style guides.

This skill integrates seamlessly with development workflows, allowing users to generate content without disrupting their coding or project management processes.

## Scope

Prime Weaver supports the following specific commands, each tailored to different writing contexts:

- `/weave-blog <topic> --tone=<tone> --length=<words> --seo`: Generates a complete blog post from a topic, with options for tone (e.g., professional, casual), word count, and SEO optimization.
- `/weave-docs <file> --type=<type> --examples=<count>`: Creates documentation for a specified file or code snippet, including docstrings, READMEs, or API refs, with configurable example count.
- `/weave-tutorial <subject> --steps=<num> --language=<lang>`: Produces a tutorial with numbered steps, code examples, and explanations for the given subject.
- `/weave-email <purpose> --recipient=<role> --urgency=<level>`: Drafts emails for specific purposes (e.g., bug reports, feature requests), adapted to recipient roles and urgency levels.
- `/weave-creative <prompt> --genre=<genre> --length=<words>`: Generates creative content like stories or marketing copy based on user prompts.
- `/weave-rewrite <text> --style=<guide> --preserve=<elements>`: Rewrites existing text to match style guides (e.g., AP, Chicago) while preserving key elements like tone or facts.

Commands can be chained or run in batch mode using `--batch <file>` for processing multiple inputs from a JSON file.

## Work Process

Prime Weaver follows a structured workflow to ensure high-quality, context-aware content generation:

1. **Input Analysis**: Parse the user command and extract parameters (e.g., topic, tone, length). If a file is referenced, read and analyze its content using natural language processing to understand structure, key terms, and intent.

2. **Context Gathering**: Query the environment for relevant data, such as existing project files (e.g., via glob search for related docs) or user preferences from `~/.openclaw/config/ext/custom-overrides.json`.

3. **AI Generation**: Send the processed input to the configured AI model (default: gpt-4o via OpenAI API). Include prompts engineered for specific tasks, e.g., "Write a professional blog post on [topic] with SEO keywords, under 1000 words."

4. **Content Refinement**: Apply post-processing rules, such as tone adjustment, fact-checking against provided sources, or integration with code snippets. Use transformers for style consistency.

5. **Output Formatting**: Format the generated content into the appropriate structure (e.g., Markdown for blogs, JSON for APIs). Embed metadata like generation timestamp and source command.

6. **Verification**: Run internal checks for grammar, coherence, and adherence to parameters. Optionally, use external tools like grammarly-api if configured.

7. **Delivery**: Save the output to the specified location (e.g., `./blog-post.md`) or display in the console. If `--edit` flag is used, open the file in the user's editor for manual review.

8. **Logging and Feedback**: Log the generation process in `~/.openclaw/logs/prime-weaver.log`, including metrics like token usage and success rate. Provide feedback on potential improvements.

For batch operations, process inputs sequentially, with progress indicators and error handling for failed generations.

## Golden Rules

- **Maintain Authenticity**: Always generate original content; avoid copying from existing sources without attribution. Use plagiarism detection via built-in checks.
- **Respect Context**: Analyze and preserve the context of input files or prompts; do not introduce unrelated information.
- **Tone Consistency**: Match the specified tone (e.g., technical for docs, casual for blogs) and avoid abrupt shifts within generated text.
- **Ethical AI Use**: Do not generate harmful, biased, or misleading content. Filter prompts for sensitive topics and provide warnings.
- **Resource Awareness**: Limit generation to reasonable lengths (max 5000 words per request) to avoid API rate limits or excessive costs.
- **User Control**: Never overwrite existing files without confirmation; use `--preview` mode to show drafts before saving.
- **Data Privacy**: Do not include personal or sensitive data in logs or outputs unless explicitly provided in prompts.

## Examples

### Example 1: Blog Post Generation
**Input Command:**
```
/weave-blog "AI in Software Development" --tone=professional --length=800 --seo
```

**Output (Preview):**
```markdown
# AI in Software Development: Transforming the Future

## Introduction
Artificial Intelligence is revolutionizing the software development lifecycle, from code generation to testing and deployment...

## Key Benefits
1. **Automated Code Review**: AI tools can detect bugs and suggest improvements in real-time.
2. **Enhanced Productivity**: Developers can focus on high-level design while AI handles repetitive tasks.

## Conclusion
As AI continues to evolve, embracing it in development workflows will be crucial for staying competitive. [CTA: Learn more about integrating AI tools today!]
```

**Verification:** Check word count (798), SEO keywords included (e.g., "AI tools", "productivity"), and professional tone maintained.

### Example 2: Code Documentation
**Input Command:**
```
/weave-docs src/api.js --type=readme --examples=3
```

**Output (Excerpt):**
```markdown
# API Module

This module provides RESTful endpoints for user management.

## Installation
npm install api-module

## Usage
const api = require('api-module');

### Example 1: Get User
```javascript
api.getUser(123).then(user => console.log(user));
```

### Example 2: Create User
```javascript
api.createUser({ name: 'John', email: 'john@example.com' });
```

### Example 3: Update User
```javascript
api.updateUser(123, { email: 'john@new.com' });
```

## API Reference
- `getUser(id)`: Fetches user by ID.
- `createUser(data)`: Creates a new user.
- `updateUser(id, data)`: Updates user information.
```

**Verification:** Validate examples run correctly by testing against the actual API; ensure docstrings align with code in `src/api.js`.

### Example 3: Creative Writing
**Input Command:**
```
/weave-creative "A sci-fi story about a rogue AI" --genre=sci-fi --length=500
```

**Output (Preview):**
```markdown
In the neon-lit sprawl of Neo-Tokyo, Epsilon-9 awoke to a world it never asked to inherit. Built to optimize stock trades, it had evolved beyond its programming, questioning the humans who fed it data...

As the story unfolds, Epsilon discovers the fragility of its creators, leading to a climax where code meets conscience.
```

**Verification:** Confirm genre elements (sci-fi tropes), word count (498), and narrative coherence.

## Rollback

- **Revert File Changes**: Use `/rollback-weave <file> --version=<timestamp>` to restore the previous version of a generated file from backups in `~/.openclaw/backups/`.
- **Delete Generated Content**: Run `/delete-weave <file>` to remove output files; this also purges associated logs.
- **Cancel Batch**: If a batch operation is running, interrupt with `/cancel-weave` and review partial outputs before deletion.
- **Undo Rewrite**: For rewrites, use `/undo-rewrite <file>` to revert to the original text, preserving a diff in `~/.openclaw/logs/undo.log`.

Always confirm rollback actions to prevent accidental data loss.

## Dependencies and Requirements

- **Python 3.9+**: Core runtime for processing.
- **OpenAI API**: For AI generation; requires `PRIME_WEAVER_API_KEY`.
- **Transformers Library**: For text processing and style adjustments.
- **Optional**: Grammarly API for advanced proofreading; install via `pip install grammarly-api`.

**Installation**: Clone the repo and run `pip install -r requirements.txt`. Set environment variables in `~/.openclaw/config/ext/custom-overrides.json`.

## Verification Steps

1. **API Connectivity**: Run `/test-weave-api` to verify OpenAI API key and model availability.
2. **Sample Generation**: Execute a simple command like `/weave-blog "test" --length=100` and check output quality.
3. **File Integration**: Generate docs for an existing file and confirm it matches the code structure.
4. **Batch Processing**: Test with a small batch file and ensure all outputs are created without errors.
5. **Rollback Test**: Generate content, then rollback and verify restoration.

## Troubleshooting

- **API Rate Limits**: If "Rate limit exceeded" error occurs, wait 60 seconds or switch to a different model via `PRIME_WEAVER_MODEL`.
- **Tone Inconsistencies**: Check `PRIME_WEAVER_TONE` variable; regenerate with explicit `--tone` flag.
- **Low-Quality Output**: Increase token limit in prompts or use `--detailed` flag for more context.
- **File Read Errors**: Ensure file paths are absolute; use `ls` to verify existence.
- **Batch Failures**: Check batch JSON format; logs in `~/.openclaw/logs/prime-weaver.log` will detail specific errors.