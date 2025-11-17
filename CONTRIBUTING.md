# Contributing to Anima Locus Documentation

See the main [Contributing Guide](../CONTRIBUTING.md) for general guidelines.

---

## Documentation-Specific Requirements

**License:** Creative Commons BY-SA 4.0 (content), AGPLv3 (code examples)

### Content Guidelines

**Writing Style:**
- Clear and concise
- Active voice preferred
- Target audience: Technical users (developers, musicians, hardware hackers)
- Avoid jargon unless defined

**Structure:**
- H2 (`##`) for main sections
- H3 (`###`) for subsections
- H4 (`####`) sparingly (prefer reorganizing content)

### Markdown Standards

**Code Blocks:**
````markdown
```python
from anima_locus import AnimaClient

client = AnimaClient("http://anima.local:8080")
```
````

Always specify language for syntax highlighting.

**Links:**
```markdown
<!-- Internal links: Use relative paths -->
See [API Overview](../api/overview.md) for details.

<!-- External links: Use full URLs -->
See [FastAPI documentation](https://fastapi.tiangolo.com/).
```

**Images:**
```markdown
![PCB Assembly](../images/pcb-assembly.jpg)
```

- Store in `public/images/`
- Optimize (WebP format, < 500 KB)
- Add descriptive alt text (accessibility)

**Tables:**
```markdown
| Parameter | Type | Range | Description |
|-----------|------|-------|-------------|
| grain_size | float | 0.01-0.5 | Grain duration in seconds |
```

### Frontmatter

```yaml
---
title: "Hardware Assembly Guide"
description: "Step-by-step guide for assembling the Anima Locus PCB"
author: "Ken Tsugi"
date: 2025-11-17
tags: [hardware, assembly, pcb]
---
```

### Interactive Elements

**API Playground:**
```markdown
<ApiExplorer spec="/openapi-spec.yaml" />
```

**Code Examples (Runnable):**
````markdown
```python:run
from anima_locus import AnimaClient

client = AnimaClient("http://anima.local:8080")
presets = client.presets.list()
print(f"Found {len(presets)} presets")
```
````

**Video Embeds:**
```markdown
<VideoEmbed
  src="https://youtube.com/watch?v=..."
  title="Hardware Assembly Guide"
/>
```

### Screenshots

- **Resolution:** 1920x1080 or higher
- **Format:** WebP (fallback PNG)
- **File size:** < 500 KB
- **Naming:** Descriptive (`conductor-ui-xy-pad.webp`, not `screenshot1.png`)

### Accessibility

- **Alt text** for all images
- **Transcripts** for videos (or link to YouTube auto-captions)
- **Headings** in logical order (no skipping levels)
- **Color contrast** meets WCAG AA standards

### Testing

**Link Validation:**
```bash
npm run check-links
```

**Spell Check:**
```bash
npm run spellcheck
```

**Build Test:**
```bash
npm run build
# Verify no errors in dist/
```

### Pull Request Checklist

- [ ] Content is clear and accurate
- [ ] Code examples tested
- [ ] Links validated (internal and external)
- [ ] Images optimized (< 500 KB)
- [ ] Alt text for all images
- [ ] Frontmatter complete
- [ ] Spell check passed
- [ ] DCO sign-off

### Commit Message Format

```
[docs] Add hardware assembly guide

Comprehensive guide for PCB assembly:
- Soldering instructions
- Component placement order
- Reflow profile
- Testing procedures

Includes 12 photos and 2 video tutorials.

Fixes #15

Signed-off-by: Your Name <your.email@example.com>
```

### Code Examples

**Python:**
```python
from anima_locus import AnimaClient
import asyncio

async def main():
    async with AnimaClient("ws://anima.local:8080") as client:
        await client.set_param("granular", "grain_size", 0.25)

asyncio.run(main())
```

**TypeScript:**
```typescript
import { AnimaClient } from '@anima-locus/sdk';

const client = new AnimaClient('ws://anima.local:8080');
await client.connect();
await client.setParam('granular', 'grain_size', 0.25);
```

**Bash:**
```bash
# List presets
anima-ctl presets list

# Load preset
anima-ctl load preset-001
```

Test all code examples before committing.

### Localization (Future)

Prepare for translation:
- Use simple, clear language
- Avoid idioms or culturally-specific references
- Keep sentences concise

Translation structure:
```
docs/
├── en/          # English (primary)
├── ja/          # Japanese
└── es/          # Spanish
```

### Style Guide

- **Headings:** Sentence case ("Hardware assembly guide", not "Hardware Assembly Guide")
- **Lists:** Use bullet points for unordered, numbers for steps
- **Emphasis:** *Italic* for emphasis, **bold** for strong emphasis, `code` for technical terms
- **Dates:** ISO 8601 format (2025-11-17)

---

## DCO Sign-Off

All commits must include:

```
Signed-off-by: Your Name <your.email@example.com>
```

Use `git commit -s` to add automatically.

---

## Questions?

Open a GitHub Discussion or see [main Contributing Guide](../CONTRIBUTING.md).

---

*Documentation licensed under CC BY-SA 4.0. Code examples licensed under AGPLv3. See [LICENSE](./LICENSE).*
