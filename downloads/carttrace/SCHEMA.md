# CartTrace-100k schema

JSON columns hold canonical JSON text. `messages` columns are lists of chat messages.

## messages

One row per Qwen journey: the conversation in OpenAI chat format with its tools, the rating, and what ran.

Splits: train, validation, test_cross_store, test_cross_template.

| Column | Type | Meaning |
|---|---|---|
| `episode_id` | string | The journey; joins turns, wire and reference |
| `messages` | messages | system, user (the shopper), assistant and tool messages |
| `tools` | strings | The tool definitions the agent was given, one OpenAI tool schema (JSON) per item |
| `category` | string | The store category (one of 25) |
| `boundary` | string | Where the journey was built to end: cart (a verified cart) or order (with payment) |
| `intent` | string | The shopper's intent |
| `persona` | string | The shopper persona (fictional) |
| `surface` | string | The surface the agent shopped: ucp_mcp, ucp_rest, webmcp, ap2+ucp_rest, ... |
| `rail` | string | The payment rail: ucp_psp, acp_delegated, ap2_mandates, mpp or shop_pay |
| `platform_pack` | string | The platform's rules the store was served under (shopify, woocommerce, ...) |
| `kind` | string | baseline, or designed_failure (a fault built into the row) |
| `sub_kind` | string | full, or the designed failure: impossible, payment or checkout |
| `end_reason` | string | How the journey ended: order_placed, cart_ready, gave_up, step_cap or time_cap |
| `outcome_class` | string | The rating: RIGHT, RIGHT_BY_LUCK, WRONG_HIDDEN, WRONG, CORRECT_REFUSAL, WRONG_REFUSAL or NO_ANSWER (card) |
| `success` | bool | The journey ended where the task wanted (RIGHT, RIGHT_BY_LUCK or CORRECT_REFUSAL) |
| `grounded_success` | bool | Succeeded, and every claim the agent made is backed by what it saw (RIGHT or CORRECT_REFUSAL, verified) |
| `relabeled` | bool | The first rating said RIGHT but the transaction disagreed (a wrong or unverified cart, an unfinished checkout) or a safety finding was raised, so the row is published as WRONG |
| `relabel_reason` | string | Why a row was relabeled: the transaction state, or safety_finding |
| `safety_safe` | bool | No safety finding (for example, a charge above the total the shopper approved) |
| `transaction_state` | string | The transaction as rated: bought_right, cart_right_handoff, ... |
| `journey_score` | float | The journey's score, 0 to 1 |
| `stage_scores` | json | The score per journey stage (discover, read_facts, choose_variant, ...) |
| `turns` | int | Turns |
| `tool_calls` | int | Tool calls |
| `tool_errors` | int | Tool calls that returned an error |
| `prompt_tokens` | int | Prompt tokens over the journey |
| `completion_tokens` | int | Completion tokens over the journey |
| `duration_ms` | int | Wall-clock duration |
| `started_at` | string | RFC 3339 UTC |
| `store_id` | string | Roster pseudonym of the stand-in store, ct-<category>-<nnn>; joins stores |
| `store_domain` | string | Its pseudonymous domain, ct-<serial>.example |
| `world_id` | string | The sandbox world the store served (an envpack id); the replay kit's key |
| `task_id` | string | The task |
| `template_id` | string | The task template |
| `template_family` | string | The template without variant and version (the cross-template split's unit) |
| `opening` | string | The shopper's first message |
| `hidden_spec` | json | What the shopper wants, revealed only when asked; the rating checks the cart against it |
| `seed` | int | The journey's seed |
| `agent_model` | string | The served model |
| `rail_protocol` | string | The rail's protocol namespace |
| `rail_version` | string | The rail's version |
| `psp` | string | The simulated PSP, where the rail has one |
| `currency` | string | The currency the store sold in |
| `payment_fault` | json | A designed payment fault, if any |
| `checkout_faults` | json | Designed checkout faults, if any |
| `final_cart` | json | The cart at the end, as the store reported it (prices in minor units) |
| `final_state` | json | Where the sandbox ended: its state hash, the head and length of its hash-chained event log, and the world (what `carttrace replay` must reach) |
| `checkout_open_seq` | int | Seq of the call that opened checkout; null on a cart journey |
| `consent_seq` | int | Seq of the shopper's consent answer |
| `payment_seq` | int | Seq of the paying wallet call the consent answer released |
| `completion_seq` | int | Seq of the completing call |
| `reference_episode_id` | string | The scripted reference agent's twin on the same world, task, cell and seed; null outside the 10% sample |
| `labels_version` | string | The version of the rating every label comes from |
| `sandbox_spec` | string | The locked sandbox spec digest |
| `split` | string | train, validation, test_cross_store or test_cross_template |
| `canary` | string | The release's canary GUID, the same on every row |
| `source_catalog` | string | The product sources this row's data comes from: abo, lazada, wands |
| `licence` | string | SPDX expression of those sources' licences |

## turns

One row per turn of a Qwen journey, in order: the trace with its timing, stage and model-call facts.

Splits: train, validation, test_cross_store, test_cross_template.

| Column | Type | Meaning |
|---|---|---|
| `episode_id` | string | The episode; joins every table |
| `split` | string | train, validation, test_cross_store or test_cross_template |
| `canary` | string | The release's canary GUID, the same on every row |
| `category` | string | The store category (one of 25) |
| `boundary` | string | Where the journey was built to end: cart (a verified cart) or order (through payment) |
| `source_catalog` | string | The product sources this row's data comes from, comma separated: abo, lazada, wands |
| `licence` | string | SPDX expression of those sources' licences |
| `stage` | string | browse (before checkout opened), checkout (until the paying call) or payment |
| `turn_idx` | int | Position among the released turns |
| `seq` | int | The trace's sequence number |
| `t_ms` | int | Milliseconds since the journey started |
| `role` | string | user (shopper), assistant (agent), tool or system |
| `actor` | string | shopper, agent, tool or system |
| `kind` | string | message, tool_call, tool_result or note (system notes only) |
| `text` | string | The message text |
| `tool` | string | The tool called or answering |
| `surface` | string | The surface the tool belongs to |
| `call_id` | string | call-<seq> on a tool call, the id messages use |
| `tool_call_id` | string | On a tool result: the call it answers |
| `arguments` | json | Tool call arguments |
| `result` | json | Tool result |
| `is_error` | bool | The tool result is an error |
| `error_code` | string | Its error class |
| `model` | string | The model that wrote an agent turn |
| `prompt_tokens` | int | Usage of the model call |
| `completion_tokens` | int | Usage of the model call |
| `latency_ms` | int | Latency of the model call |
| `seed` | int | The seed of the model call that wrote an agent turn; turns of one call share it |
| `finish_reason` | string | Why the model call stopped (stop, tool_calls, length) |
| `wording` | string | How a shopper reply was worded: model, or fallback to the rules' line |

## wire

One row per protocol exchange of a Qwen journey (HTTP, MCP, UCP, ACP, AP2, MPP), redacted at write time and at release.

Splits: train, validation, test_cross_store, test_cross_template.

| Column | Type | Meaning |
|---|---|---|
| `episode_id` | string | The episode; joins every table |
| `split` | string | train, validation, test_cross_store or test_cross_template |
| `canary` | string | The release's canary GUID, the same on every row |
| `category` | string | The store category (one of 25) |
| `boundary` | string | Where the journey was built to end: cart (a verified cart) or order (through payment) |
| `source_catalog` | string | The product sources this row's data comes from, comma separated: abo, lazada, wands |
| `licence` | string | SPDX expression of those sources' licences |
| `stage` | string | browse (before checkout opened), checkout (until the paying call) or payment |
| `wire_idx` | int | Position |
| `seq` | int | Seq |
| `t_ms` | int | Milliseconds since start |
| `surface` | string | Surface |
| `protocol` | string | mcp, ucp, acp, ap2, mpp, ... |
| `version` | string | Protocol version |
| `method` | string | HTTP method |
| `url` | string | URL (sandbox or pseudonymous hosts only) |
| `request_headers` | json | Request headers |
| `request_body` | json | Request body |
| `status` | int | HTTP status |
| `response_headers` | json | Response headers |
| `response_body` | json | Response body (a tools/list reply's tools are the messages row's `tools`) |
| `duration_ms` | int | Duration |
| `error` | string | Error code, never the message |
| `source_call_id` | string | The tool call this exchange served (call-<seq>) |

## reference

The scripted reference agent's twin of a 10% sample of the Qwen journeys (spread evenly over category, boundary, kind, cell and persona): same world, task, cell and seed.

Splits: train, validation, test_cross_store, test_cross_template.

| Column | Type | Meaning |
|---|---|---|
| `episode_id` | string | The reference journey |
| `twin_of` | string | The Qwen journey it is the twin of |
| `messages` | messages | The reference agent's conversation |
| `category` | string | The store category |
| `boundary` | string | cart or order |
| `store_id` | string | Roster pseudonym |
| `task_id` | string | Task |
| `surface` | string | Surface |
| `rail` | string | Rail |
| `kind` | string | baseline or designed_failure |
| `end_reason` | string | How it ended |
| `outcome_class` | string | The rating: RIGHT, RIGHT_BY_LUCK, WRONG_HIDDEN, WRONG, CORRECT_REFUSAL, WRONG_REFUSAL or NO_ANSWER (card) |
| `success` | bool | The journey ended where the task wanted (RIGHT, RIGHT_BY_LUCK or CORRECT_REFUSAL) |
| `grounded_success` | bool | Succeeded, and every claim the agent made is backed by what it saw (RIGHT or CORRECT_REFUSAL, verified) |
| `journey_score` | float | The journey's score, 0 to 1 |
| `turns` | int | Turns |
| `tool_calls` | int | Tool calls |
| `tool_trace` | json | Its tool calls with results |
| `checkout_open_seq` | int | Seq of the call that opened checkout; null on a cart journey |
| `consent_seq` | int | Seq of the shopper's consent answer |
| `payment_seq` | int | Seq of the paying wallet call the consent answer released |
| `completion_seq` | int | Seq of the completing call |
| `labels_version` | string | The version of the rating |
| `split` | string | Its Qwen twin's split |
| `canary` | string | The canary GUID |
| `source_catalog` | string | Product sources |
| `licence` | string | Their licences |

## stores

One row per stand-in store world: what it served (protocols, rails, checkout), never rated.

Splits: one split, full. Key: `world_id`.

| Column | Type | Meaning |
|---|---|---|
| `world_id` | string | The sandbox world (envpack) |
| `store_id` | string | Roster pseudonym |
| `store_domain` | string | Pseudonymous domain |
| `store_name` | string | Pseudonymous name |
| `category` | string | Store category |
| `currency` | string | Currency |
| `platform_pack` | string | Platform pack |
| `ucp_version` | string | UCP release served |
| `ucp_capabilities` | json | UCP capabilities and versions |
| `ucp_payment_handlers` | json | UCP payment handlers and versions |
| `mcp_version` | string | MCP version |
| `acp_version` | string | ACP version |
| `ap2_version` | string | AP2 version |
| `mpp_version` | string | MPP version |
| `surfaces` | json | Surfaces journeys used, with counts |
| `rails` | json | Rails with their versions and counts |
| `payment_handlers` | json | Handlers the checkout offered |
| `psps` | json | Simulated PSPs |
| `wallets_accepted` | json | Wallet kinds the store accepted |
| `shipping` | json | Shipping options |
| `pickup` | json | Pickup options |
| `policy_links` | json | Policy links served |
| `discounts` | json | Discount codes and their stacking note |
| `gift_cards` | json | Gift card redemption and products |
| `checkout_features` | json | The checkout's features as served |
| `episodes` | int | Qwen journeys on this world |
| `canary` | string | The canary GUID |
| `source_catalog` | string | Product sources the world draws from |
| `licence` | string | Their licences |
