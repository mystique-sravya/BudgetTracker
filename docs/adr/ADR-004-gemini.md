# ADR-004 — Adopt Gemini Flash for AI Workloads

Status: Accepted  
Date: 2026-07-02  
Decision: Use Google Gemini Flash as the primary AI inference provider.

Alternatives Considered:
- GPT-4o
- Claude 3.5
- Local LLMs

Consequences:
- AI flows must validate provider output strictly
- Fallback and caching remain mandatory
- Provider dependence exists but is isolated behind the AI service layer

## Context

FinanceTracker requires low-cost, fast AI generation for summaries, anomaly detection support, and recommendation flows without letting AI cost dominate the platform.

## Rationale

- Supports high-context prompts for financial summaries
- Keeps cost low relative to heavier general-purpose models
- Fits optional AI enrichment rather than critical write paths