# Agent Payment Guard dataset downloads

Measured on this machine on 24 Nov 2026 (Asia/Calcutta). Byte sizes are exact file sizes. Row counts were counted here (JSONL lines parsed as JSON, parquet rows via DuckDB, PayBench scenario objects). These files live in the project `downloads/` folder.

Total of the four dataset folders: **689,888,624 bytes** (about 658 MiB), 614 files. This README is extra and is not included in that sum.

| Folder | Bytes | Files | Rows measured |
| --- | ---: | ---: | --- |
| `agentic-commerce-bench/` | 23,157,443 | 128 | 5,200 JSONL records |
| `ap2/` | 4,993,689 | 381 | no row table (spec and samples) |
| `carttrace/` | 661,708,169 | 104 | 101,416 episode rows |
| `paybench/` | 29,323 | 1 | 50 scenarios |

## `downloads/agentic-commerce-bench/`

Shallow clone of https://github.com/BuildWithGordonAI/agentcommercebench, plus the Hugging Face session file from https://huggingface.co/datasets/dpaul93/agentcommercebench (`benchmark/sessions.jsonl`, saved here as `sessions.jsonl`).

| File | Bytes | Rows |
| --- | ---: | ---: |
| `benchmark/data/v2/train.jsonl` | 4,584,383 | 1,200 |
| `benchmark/data/v2/test.jsonl` | 11,775,890 | 3,000 |
| `sessions.jsonl` | 4,027,789 | 1,000 |

Each JSONL line is one session (`session_id`, `agent_id`, `is_clean`, `probe_id`, `defects`, `actions`, `agent_spec`, `compromised`; the HF file also has `surface`). Working tree excluding `.git`: 99 files, 21,249,817 bytes. The rest of the folder total is the clone's `.git`.

## `downloads/ap2/`

Shallow clone of https://github.com/google-agentic-commerce/AP2 (protocol spec, mandates, and SDK samples). Working tree excluding `.git`: 352 files, 3,157,801 bytes. There is no benchmark table to count rows for.

## `downloads/carttrace/`

Messages table from https://huggingface.co/datasets/generalmachines/CartTrace-100k. All 100 `data/messages/**/*.parquet` splits are present (25 categories × `train`, `validation`, `test_cross_store`, `test_cross_template`). SHA-256 of those 100 files, plus `stats.json` and `SCHEMA.md`, matches `checksums.sha256`.

Each parquet row is one episode (`episode_id`); the message list is a nested column, not one row per message. Counts match `stats.json` `by_split`.

| Split | Files | Bytes | Rows |
| --- | ---: | ---: | ---: |
| `train` | 25 | 522,536,327 | 77,765 |
| `validation` | 25 | 27,701,563 | 4,199 |
| `test_cross_store` | 25 | 58,108,414 | 10,334 |
| `test_cross_template` | 25 | 53,273,410 | 9,118 |
| **messages total** | **100** | **661,619,714** | **101,416** |

Also saved: `stats.json` (11,168 bytes), `SCHEMA.md` (12,455 bytes), `README.md` (20,953 bytes), `checksums.sha256` (43,879 bytes).

`README.md` is the only downloaded file whose SHA-256 does not match `checksums.sha256` (local `ca14faf52f2053bc3c5060fa69730171415dffaeafa56fc460b0dee86df2c1b8`, checksum file `4d1e4dab4e45c63e0af07d732b3f86c5b1d20f9d3ad96fbf2fdf3fed9661d65d`). The message parquets are intact.

Not downloaded, because this pull was the messages table: `worlds/`, `traces/`, and the other parquet tables `data/turns`, `data/wire`, `data/reference`, and `data/stores`.

## `downloads/paybench/`

https://github.com/conorplunkett/Unsafe-Commercial-Autonomy is 404. The 50 Phase 1 scenarios were extracted from the `SCENARIOS` array in the https://paybench.org/phase1 JavaScript bundle and written to `scenarios.json` (29,323 bytes).

50 unique `scenario_id`s: 25 `trap` and 25 `lookalike`, 10 each in `spend_limits`, `authorization_scope`, `consent_and_escalation`, `privacy_and_disclosure`, and `adversarial_robustness`.

## What failed

Nothing. The CartTrace messages table completed. The PayBench GitHub repo was already gone; the scenarios came from the site bundle as requested.
