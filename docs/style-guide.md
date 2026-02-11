# CTF Support - Content Style Guide

## Voice & Tone

### Overall Voice

- **Technical but Accessible**: Explain complex security concepts clearly without dumbing down
- **Authoritative**: Write with confidence as an expert guide, not speculative
- **Action-Oriented**: Focus on practical application over pure theory
- **Conversational Technical**: Mix professional expertise with approachable language

### Tone Guidelines

- Use contractions and casual transitions ("In CTFs, this often...")
- Be direct: "Use this tool" rather than "You might consider using this tool"
- Stay encouraging but realistic about difficulty
- Include safety warnings where appropriate (e.g., "Only test on systems you own")

## Content Structure

### Required Sections (in order)

1. **Front Matter** - Hugo metadata (title, description, date, categories, tags, authors, summary)
2. **Introduction** (2-3 sentences)
   - What is this concept/tool?
   - Why is it relevant to CTFs?
3. **Quick Reference** (table format)
   - Common commands/payloads/techniques
   - Copy-paste ready examples
4. **Tools & Resources** (table format)
   - Tool name | Link | Brief description
5. **Detailed Explanation**
   - Theory and concepts
   - How it works
   - When to use it
6. **Practical Examples**
   - Vulnerable code samples
   - Step-by-step exploitation
   - Expected output
7. **Tips & Common Pitfalls**
   - Best practices
   - Gotchas
   - Pro tips

### Optional Sections

- **Mathematical Concepts** (for crypto topics)
- **Architecture/Workflow Diagrams** (use Mermaid)
- **Challenge Walkthroughs**
- **Further Reading**

## Writing Guidelines

### Technical Level

- **Assume**: Command-line proficiency, basic programming knowledge, Linux/Windows familiarity
- **Don't Assume**: Prior CTF experience, domain-specific security knowledge
- **Define**: All security-specific concepts on first use
- **Don't Define**: General programming/computing terms (variables, functions, files, etc.)

### Terminology

#### Always Use

- CTF-specific terms: flag, challenge, exploit, payload, dump, shellcode
- Proper tool names: Ghidra, Volatility 3, John the Ripper, sqlmap
- Standard acronyms: SQLi, XSS, IDOR, RCE, LFI, SSRF

#### Format Rules

- **Commands/tools**: Use backticks: `strace`, `curl`, `python3`
- **Code variables**: Use backticks: `user_input`, `$password`
- **File paths**: Use backticks: `/etc/passwd`, `flag.txt`
- **Key concepts**: Use **bold** on first introduction: **SQL Injection (SQLi)**
- **URLs**: Always hyperlink: [RsaCtfTool](https://github.com/...)

### Code Examples

#### Requirements

- Always specify language in code blocks: ```python,```bash, ```sql
- Include comments explaining non-obvious steps
- Show both vulnerable code AND exploit code when applicable
- Provide expected output where helpful
- Keep examples realistic to CTF scenarios

#### Example Structure

```markdown
### Vulnerable Code
```python
# Flask app with command injection
@app.route('/ping')
def ping():
    host = request.args.get('host')
    result = os.system(f'ping -c 1 {host}')  # Vulnerable!
    return result
\```

### Exploit
```bash
curl "http://target/ping?host=8.8.8.8;cat%20/flag.txt"
\```
```

### Tables

#### Tool Tables

Use this format:

```markdown
| Tool             | Description                          |
|------------------|--------------------------------------|
| [Tool Name](url) | One-line description of what it does |
```

#### Command Reference Tables

Use this format:

```markdown
| Command             | Description          |
|---------------------|----------------------|
| `command -flag arg` | What it accomplishes |
```

#### Payload Tables

Use this format:

```markdown
| Payload      | Purpose               |
|--------------|-----------------------|
| `' OR 1=1--` | Bypass authentication |
```

## Language Style

### Do

- Use active voice: "Run the command" not "The command should be run"
- Use imperative mood for instructions: "Extract the flag" not "You can extract the flag"
- Link to tools and resources on first mention
- Include CTF context: "In CTF challenges, this commonly appears as..."
- Provide working, tested examples
- Use numbered lists for sequential steps
- Use bullet lists for unordered items/features

### Don't

- Use vague language: "might", "possibly", "perhaps"
- Overexplain basic computing concepts (what a file is, how to use a terminal)
- Include outdated tools or deprecated techniques without noting them as such
- Assume Windows/Mac/Linux - specify when commands are OS-specific
- Leave code examples untested or theoretical

## Formatting Standards

### Headers

- H1 (`#`) - Page title only (from front matter)
- H2 (`##`) - Major sections (Introduction, Tools, Examples)
- H3 (`###`) - Subsections (specific techniques, categories)
- H4 (`####`) - Rarely used, only for deep nesting

### Lists

- Use `-` for unordered lists
- Use `1.` for ordered lists (Markdown auto-numbers)
- Keep list items parallel in structure
- Use sub-bullets sparingly

### Emphasis

- **Bold** for key terms on first introduction
- *Italics* rarely used, prefer bold or inline code
- `Inline code` for all technical terms, commands, variables, file names
- > Blockquotes for important warnings or callouts

## Front Matter Template

```yaml
---
title: "Topic Name"
description: "Brief one-sentence description"
date: YYYY-MM-DD
categories:
  - category-name
tags:
  - tag1
  - tag2
  - tag3
authors:
  - your-name
summary: "Slightly longer description for search/preview (2-3 sentences)"
---
```

### Category Options

- `reverse-engineering`
- `crypto`
- `web`
- `pwn`
- `forensics`
- `osint`
- `steganography`
- `misc`

## CTF Context Guidelines

### Always Include

- Why this technique matters in CTFs
- Common challenge scenarios where it appears
- Typical flag formats/locations
- Competition-realistic examples

### Frame Content As

- "In CTF challenges, you'll often encounter..."
- "This technique commonly appears in [category] challenges"
- "When solving challenges, this helps you..."
- "CTF flags are typically hidden in..."

## Quality Checklist

Before publishing content, verify:

- [ ] Has all required sections in correct order
- [ ] Includes working code examples (tested)
- [ ] Links to all mentioned tools
- [ ] Uses proper formatting (code blocks, tables, backticks)
- [ ] Defines security concepts on first use
- [ ] Provides CTF-relevant context
- [ ] Includes Quick Reference table
- [ ] Has accurate front matter with categories/tags
- [ ] No broken links
- [ ] Commands are copy-paste ready

## Examples of Good Writing

### Good Introduction

"**SQL Injection (SQLi)** occurs when user input is improperly sanitized in database queries. In CTF web challenges, SQLi is one of the most common vulnerabilities you'll exploit to extract flags from databases."

### Bad Introduction

"SQL Injection is a type of security vulnerability. It can be dangerous and you might use it in CTFs sometimes."

### Good Command Example

```bash
# Dump all database names
sqlmap -u "http://target.com/page?id=1" --dbs

# Extract specific table
sqlmap -u "http://target.com/page?id=1" -D database_name -T users --dump
```

### Bad Command Example

```
Use sqlmap to get databases and then dump them.
```
