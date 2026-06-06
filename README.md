![ADR](https://img.shields.io/badge/framework-Architecture%20Decision%20Records-blue)
![Examples](https://img.shields.io/badge/examples-included-green)
![Docs](https://img.shields.io/badge/docs-architecture%20guidance-purple)
![Status](https://img.shields.io/badge/status-active-success)
![Stars](https://img.shields.io/github/stars/EventLoom/CodeWeaveDev)

# CodeWeave ADR Framework (Public Sample)

You've had the meeting. Someone wants to change how errors are 
handled. Or the logging format. Or why you're not using Kubernetes.

And you spend 40 minutes explaining a decision that was made 
18 months ago by someone who has since left.

That's what this is for.

---

## What CodeWeave is

A set of architectural decisions written down, with the reasoning 
attached.

Not a methodology. Not a maturity model. Just the decisions 
your team keeps having to make again, already made, with enough 
context that someone can actually understand why.

Built for teams who run their own systems. No dedicated platform 
team. No SRE. You wrote it, you operate it, you fix it at 2am.

---

## What's in this repo

Five decisions from the full set:

| ADR | Title |
|-----|-------|
| ADR-0-001 | Default Execution Model |
| ADR-3-012 | Context Preservation Strategy |
| ADR-4-015 | Configuration Management |
| ADR-4-024 | Error Handling & Classification |
| ADR-4-025 | Structured Application Logging |

Trying to figure out where to start:

Deployment and runtime shape → ADR-0-001  
Why decisions were made the way they were → ADR-3-012  
Environment config and how it's handled → ADR-4-015  
What happens when something breaks → ADR-4-024  
What your logs should actually look like → ADR-4-025

Plus two example system context documents showing how they 
apply to real systems — a sensor platform and a financial 
reconciliation system.

---

## What's not in this repo

The full framework has 35+ decisions covering deployment, 
observability, service boundaries, incident response, 
and scaling.

These five are the easy ones to read out of context.
The rest are the decisions that actually hurt when 
you get them wrong.

---

## The full pack

**→ [eventloomtech.com/codeweave](https://www.eventloomtech.com/codeweave)**

One-time download. Drop it in `/docs/adr`.
Next time someone wants to relitigate the logging format,
point them at ADR-4-025 and get back to work.
