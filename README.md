# Anima Locus Documentation Site

**AGPLv3 Licensed** | Static Site Generator for Project Documentation

Comprehensive documentation for hardware, firmware, API, and usage guides.

---

## Overview

This repository contains the source for the Anima Locus documentation site, built with:

- **Static Site Generator:** VitePress / Docusaurus / Jekyll (TBD)
- **Content:** Markdown + MDX (interactive examples)
- **Hosting:** GitHub Pages
- **Domain:** anima-locus.musubiaccord.org

### Documentation Structure

```
docs-site/
├── docs/
│   ├── index.md                  # Landing page
│   ├── getting-started/
│   │   ├── quickstart.md
│   │   ├── hardware-setup.md
│   │   └── first-sound.md
│   ├── hardware/
│   │   ├── overview.md
│   │   ├── pcb.md
│   │   ├── sensors.md
│   │   ├── bom.md
│   │   └── assembly.md
│   ├── firmware/
│   │   ├── architecture.md
│   │   ├── building.md
│   │   ├── flashing.md
│   │   └── debugging.md
│   ├── api/
│   │   ├── overview.md
│   │   ├── websocket.md
│   │   ├── rest.md
│   │   ├── openapi-spec.yaml
│   │   └── examples.md
│   ├── sdk/
│   │   ├── python.md
│   │   └── typescript.md
│   ├── audio-engines/
│   │   ├── granular.md
│   │   ├── spectral.md
│   │   ├── sampler.md
│   │   └── effects.md
│   ├── conductor-ui/
│   │   ├── overview.md
│   │   ├── hud-elements.md
│   │   └── macros.md
│   ├── presets/
│   │   ├── creating.md
│   │   ├── mapping.md
│   │   └── sharing.md
│   ├── performance/
│   │   ├── latency.md
│   │   ├── cpu-usage.md
│   │   └── optimization.md
│   └── contributing/
│       ├── guidelines.md
│       ├── code-of-conduct.md
│       └── security.md
├── public/
│   ├── images/
│   ├── videos/
│   └── downloads/
├── .vitepress/
│   ├── config.ts
│   └── theme/
└── package.json
```

---

## Key Documentation Sections

### Getting Started

**Target Audience:** First-time users, performers, developers

**Content:**
- **Quickstart:** 5-minute "hello world" - power on, load preset, make sound
- **Hardware Setup:** PCB assembly, sensor connections, power supply
- **First Sound:** Walkthrough of Conductor UI, basic parameter control

### Hardware

**Target Audience:** Hardware hackers, DIY builders, CE engineers

**Content:**
- **Overview:** Arduino UNO Q platform, sensor suite, connectors
- **PCB:** Layer stack, trace impedance, EMC considerations
- **Sensors:** Datasheets, I2C addresses, interrupt wiring, calibration
- **BOM:** Digikey/Mouser part numbers, alternates, cost estimates
- **Assembly:** Soldering guide, reflow profile, testing procedures

### Firmware

**Target Audience:** Embedded developers, contributors

**Content:**
- **Architecture:** ISR/DMA structure, sensor scheduling, link protocol
- **Building:** CMake setup, Docker build, CI/CD
- **Flashing:** ST-Link, DFU, Arduino bootloader
- **Debugging:** SWD, UART logs, performance profiling

### API

**Target Audience:** Integration developers, SDK users

**Content:**
- **Overview:** WebSocket vs REST, authentication (if any), rate limits
- **WebSocket:** Connection lifecycle, parameter control messages, telemetry format
- **REST:** Presets CRUD, scenes, system status endpoints
- **OpenAPI Spec:** Interactive API explorer (Swagger UI)
- **Examples:** Python, TypeScript, cURL snippets

### SDK

**Target Audience:** Python/TypeScript developers

**Content:**
- **Python:** Installation, client initialization, async patterns, CLI tools
- **TypeScript:** npm install, browser/Node usage, React hooks

### Audio Engines

**Target Audience:** Musicians, sound designers, audio developers

**Content:**
- **Granular:** Parameters, sensor mappings, use cases
- **Spectral:** FFT freeze, bin masking, partials emphasis
- **Sampler:** Polyphony, triggers, ADSR
- **Effects:** Reverb, delay, distortion, filters

### Conductor UI

**Target Audience:** Performers, VJs, installation artists

**Content:**
- **Overview:** HUD layout, XY pad, beat/dynamics meters
- **HUD Elements:** Macro knobs, scene selector, telemetry display
- **Macros:** Assigning parameters, saving configurations

### Presets

**Target Audience:** All users

**Content:**
- **Creating:** Engine configs, sensor mappings, saving/loading
- **Mapping:** How to map sensor paths to parameters
- **Sharing:** Exporting JSON, community preset library

### Performance

**Target Audience:** System integrators, optimizers

**Content:**
- **Latency:** Audio I/O, sensor-to-sound, network overhead
- **CPU Usage:** Profiling tools, bottleneck identification
- **Optimization:** Kernel tuning, JACK/PipeWire settings, governor settings

### Contributing

**Target Audience:** Open-source contributors

**Content:**
- **Guidelines:** Code style, commit messages, PR templates
- **Code of Conduct:** Contributor Covenant
- **Security:** Vulnerability reporting, security policy

---

## Interactive Features

### API Playground

Embedded Swagger UI for live API testing:

```markdown
<ApiExplorer spec="/openapi-spec.yaml" />
```

### Code Examples

Runnable code snippets with syntax highlighting:

````markdown
```python:run
from anima_locus import AnimaClient

client = AnimaClient("http://anima.local:8080")
presets = client.presets.list()
print(f"Found {len(presets)} presets")
```
````

### Video Tutorials

Embedded YouTube/Vimeo guides:

```markdown
<VideoEmbed src="https://youtube.com/watch?v=..." title="Hardware Assembly Guide" />
```

### Sensor Visualization

Interactive sensor data viewer (WebGL):

```markdown
<SensorVisualizer wsUrl="ws://anima.local:8080" />
```

---

## Building the Site

### Prerequisites

- **Node.js:** 18+
- **Package Manager:** npm, yarn, or pnpm

### Development

```bash
npm install
npm run dev
# Site runs at http://localhost:5173
```

### Production Build

```bash
npm run build
# Outputs to dist/
```

### Deployment

```bash
# Deploy to GitHub Pages
npm run deploy

# Manual deploy
npm run build
rsync -av dist/ user@server:/var/www/anima-locus/
```

---

## Content Authoring

### Markdown Standards

- **Headings:** Use H2 (`##`) for main sections, H3 (`###`) for subsections
- **Code Blocks:** Specify language for syntax highlighting
- **Links:** Prefer relative paths for internal links
- **Images:** Store in `public/images/`, optimize (WebP, < 500 KB)

### Frontmatter

```yaml
---
title: "Hardware Overview"
description: "Detailed guide to Anima Locus hardware components"
author: "Ken Tsugi"
date: 2025-11-17
tags: [hardware, pcb, sensors]
---
```

### Cross-References

Link to other docs with relative paths:

```markdown
See [Firmware Building](../firmware/building.md) for build instructions.
```

### Code Snippets

External code files can be embedded:

````markdown
```python
<<< @/examples/quickstart.py
```
````

---

## Localization (Future)

Directory structure for i18n:

```
docs/
├── en/          # English (primary)
├── ja/          # Japanese
├── es/          # Spanish
└── fr/          # French
```

Translation files:

```
.vitepress/
└── i18n/
    ├── en.json
    ├── ja.json
    └── ...
```

---

## Analytics (Optional)

### Privacy-Preserving Analytics

Use Plausible or similar (no cookies, GDPR-compliant):

```typescript
// .vitepress/config.ts
export default {
  head: [
    ['script', {
      defer: '',
      'data-domain': 'anima-locus.musubiaccord.org',
      src: 'https://plausible.io/js/script.js'
    }]
  ]
}
```

---

## Licensing

Documentation content licensed under **Creative Commons BY-SA 4.0**.

Code examples licensed under **AGPLv3** (same as project).

See [LICENSE](./LICENSE) for full text.

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for:

- Content guidelines
- Markdown style
- Screenshot standards
- Translation workflow

---

## Related Repositories

- [hw/](../hw/) - Hardware designs (source material)
- [mcu-stm32/](../mcu-stm32/) - Firmware docs (source material)
- [engine-ui/](../engine-ui/) - API reference (source material)
- [sdk-py/](../sdk-py/) - Python SDK docs
- [sdk-ts/](../sdk-ts/) - TypeScript SDK docs

---

## Links

- **Live Site:** https://anima-locus.musubiaccord.org
- **VitePress:** https://vitepress.dev/
- **Docusaurus:** https://docusaurus.io/

---

*Part of the [Anima Locus](../) project and [The Authentic Rebellion Framework](https://rebellion.musubiaccord.org) ecosystem.*
