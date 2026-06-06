![ADR](https://img.shields.io/badge/framework-Architecture%20Decision%20Records-blue)
![Examples](https://img.shields.io/badge/examples-included-green)
![Docs](https://img.shields.io/badge/docs-architecture%20guidance-purple)
![Status](https://img.shields.io/badge/status-active-success)
![Stars](https://img.shields.io/github/stars/EventLoom/CodeWeaveDev)

# CodeWeave ADR Framework (Public Sample)

We've all been in that meeting. Someone questions how errors are handled, wants to change the logging format, or asks why the team isn't using Kubernetes. Before you know it, you've lost 40 minutes explaining a design choice made 18 months ago by an engineer who has since moved on.

CodeWeave is designed to stop you having to repeat yourself.

## What CodeWeave is
It is a curated collection of Architectural Decision Records (ADRs). This isn't a heavy methodology or a corporate maturity model to implement—it’s simply a set of practical, pre-written decisions with the reasoning clearly laid out. 

It is aimed squarely at "you build it, you run it" teams. If you don't have the luxury of a dedicated platform team or an SRE department to handle the fallout of bad architecture, these records provide a solid baseline to work from.

## What's in this repo
This repository contains five sample decisions from the wider framework.

* **[ADR-0-001 Default Execution Model](adr/ADR-0-001-default-execution-model.md):** The baseline for deployment and runtime shape.
* **[ADR-3-012 Context Preservation Strategy](adr/ADR-3-012-context-preservation-strategy.md):** How and why architectural reasoning is captured.
* **[ADR-4-015 Configuration Management](adr/ADR-4-015-configuration-management.md):** The standard for handling environment-specific config.
* **[ADR-4-024 Error Handling & Classification](adr/ADR-4-024-error-handling-and-classification.md):** What happens when something inevitably breaks.
* **[ADR-4-025 Structured Application Logging](adr/ADR-4-025-structured-application-logging.md):** What logs actually need to look like to be useful.

### Real-World Examples
To see how these decisions apply in practice, have a look at the example documents included in this repository:
* **Financial Reconciliation System:** Check out the [System Context](examples/financial-reconciliation/system-context.md) and its specific [ADR Index](examples/financial-reconciliation/adr-index.md).
* **Sensor Platform:** Review how the framework maps to a hardware/software [System Context](examples/sensor-platform/system-context.md).

## What isn't in this repo
The complete CodeWeave pack contains over 35 decisions covering service boundaries, incident response, scaling, and deployment strategy. 

I've shared these five here because they make sense out of context. The full pack tackles the bigger architectural calls—the ones and brings it all together to a full system you can deploy in a commercial team in one go.

## The full framework
You can grab the complete set at [eventloomtech.com/codeweave](https://eventloomtech.com/codeweave). 

It is a one-off download. You just drop the Markdown files straight into your repository's `/docs/adr` folder. The next time someone wants to relitigate your logging format, just point them to ADR-4-025 and get back to work.
