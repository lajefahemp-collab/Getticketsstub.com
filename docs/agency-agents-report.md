# Agency Agents: Web Design Toolkit Report

Source: [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) (MIT license)
Pinned commit: `f99f6aa910a442b0197b768ce0ea7751e35e2060` (2026-10-07)
Installed to: `.claude/agents/` (project-scoped, so every Claude Code session in this repo gets them)

## What was added

The upstream repo has 200+ agents across 20 divisions. Only the 18 that matter for designing
and shipping websites were installed. The full catalog has many unrelated specialists, such as
WeChat, Solidity and healthcare billing agents.

### Design (all 10)
| Agent | Use it for |
|---|---|
| UI Designer | Design systems, component libraries, visual polish |
| UX Architect | CSS architecture, layout foundations, dev handoff |
| UX Researcher | Usability testing, user behavior insights |
| Brand Guardian | Brand identity and consistency |
| Visual Storyteller | Cinematic narrative, multimedia, emotional flow |
| Whimsy Injector | Delight, micro-interactions, personality |
| Image Prompt Engineer | Prompts for AI-generated photography |
| Inclusive Visuals Specialist | Imagery that is accurate and free of stereotypes |
| Persona Walkthrough Specialist | Simulated user walkthroughs and CRO reports |
| UI Finish-Gate Reviewer | Pre-ship review that catches generic, look-alike UI |

### Build
| Agent | Use it for |
|---|---|
| Frontend Developer | React/Vue/vanilla implementation, performance |
| Rapid Prototyper | Fast proof-of-concept builds |
| XR Immersive Developer | WebXR/3D immersive experiences |

### Quality & Launch
| Agent | Use it for |
|---|---|
| Accessibility Auditor | WCAG 2.2 audits (axe, Lighthouse) |
| Performance Benchmarker | Core Web Vitals and load testing |
| Evidence Collector | Screenshot-based QA proof |
| Reality Checker | Production-readiness gate |
| SEO Specialist | Technical and on-page SEO |

## How to use

In a Claude Code session in this repo, ask for an agent by name:

```
Use the UI Designer agent to create a design system for the landing page.
Have the Accessibility Auditor review index.html.
Run the UI Finish-Gate Reviewer on the new hero section before we ship.
```

A suggested pipeline for a new page:
**UX Architect → UI Designer → Visual Storyteller / Whimsy Injector → Frontend Developer →
Accessibility Auditor + Performance Benchmarker → UI Finish-Gate Reviewer → Reality Checker.**

## Verification (everything checks out)

| Check | Result |
|---|---|
| Upstream linter (`scripts/lint-agents.sh`) on all 18 files | **PASSED**: 0 errors, 3 style warnings (missing optional sections in Evidence Collector and XR Immersive Developer; harmless) |
| Valid YAML frontmatter (`name`, `description`) | All 18 files |
| Hidden/invisible Unicode (prompt-injection vector) | None found |
| Prompt-injection phrases ("ignore previous", system-prompt overrides) | None found |
| Remote-execution patterns (`curl \| sh`, `rm -rf`, base64 payloads) | None found |
| Secrets / API keys | None found (the "token" hits are design tokens and a sample k6 load-test login) |
| External URLs | Only W3C, axe-core docs, `localhost`, placeholder `site.com`, and `uizze.com` |
| Live load test in Claude Code (`claude -p --agent "UI Designer"`) | Agent loaded and responded in persona |
| License | MIT; copy kept at `.claude/agents/LICENSE-agency-agents` |

### Notes
- **UI Finish-Gate Reviewer** mentions `uizze.com` as an *optional* free reference catalogue.
  The file states that no account or API is required, so this is a link plug, not a dependency.
- **Accessibility Auditor** suggests running `npx @axe-core/cli` and `npx lighthouse`. These are
  the standard, well-known tools, and they only run if you approve them.
- The agents are plain Markdown prompts and run no code on their own.

## Updating later

```bash
git clone --depth 1 https://github.com/msitarzewski/agency-agents.git /tmp/aa
cp /tmp/aa/design/*.md .claude/agents/
# Re-run the checks above before committing upstream changes.
```
