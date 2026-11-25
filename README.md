# agent-payment-guard

Agent Payment Guard A risk layer that checks every payment an AI agent makes on a user's behalf

Agent Payment Guard checks a payment an AI agent is about to make for a person and returns one decision: approve it, ask the person to confirm, or decline it. This repository is a data drop for the proof of concept. It is not the guard implementation.

## What is in this repo

- `downloads/carttrace` — the CartTrace messages table only: 101,416 journeys, including 8,071 on the AP2 rail.
- `downloads/agentic-commerce-bench` — 1,200 train sessions and 3,000 test sessions, plus a 1,000-session slice.
- `downloads/ap2` — the AP2 protocol (spec, schemas, and samples). It is not a payment table.
- `downloads/paybench/scenarios.json` — 50 scenarios. The upstream GitHub repository returns 404.
- `AGENT_PAYMENT_GUARD_DATASET_STUDY.md` — a measurement note on the files above.

## What is not included

CartTrace turns, wire, reference, stores, and traces.

## Sources

- https://huggingface.co/datasets/generalmachines/CartTrace-100k
- https://github.com/BuildWithGordonAI/agentcommercebench
- https://huggingface.co/datasets/dpaul93/agentcommercebench
- https://github.com/google-agentic-commerce/AP2
- https://paybench.org/phase1
