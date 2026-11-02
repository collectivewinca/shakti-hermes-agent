# Hermes: skip `think` for Mistral models (custom provider)

File: plugins/model-providers/custom/__init__.py (hermes-agent, NousResearch/hermes-agent @ f14059fad2, main)
Applied on ve-shakti 2026-11-02 ~11:06 AM ET (backup: __init__.py.bak-20261102-1606).

## Why
With reasoning disabled, Hermes sends `reasoning_effort: "none"` plus `extra_body.think = false`.
Mistral's API rejects unknown body fields: HTTP 422 `extra_forbidden` on `body.think`,
which made Hermes crons fall back to DeepSeek.

## Change
Only add `think: false` when the model name does NOT start with (case-insensitive):
mistral, magistral, devstral, codestral, ministral, pixtral.

Read the name from `ctx.get("model")`. `build_api_kwargs_extras` takes `**ctx`; bare `model` is a NameError (hit on ve-shakti after the first live patch).

`reasoning_effort: "none"` is still sent and Mistral accepts it.

## Status
Required for Hermes on mistral-medium-latest (primary). Local patch to an upstream file —
a Hermes update/`git pull` may overwrite it; reapply `mistral-think-skip.diff` (`git apply`).

This fork does not contain `plugins/model-providers/`; do not copy the upstream plugin tree
here. The diff is the source of truth until the skip is upstreamed.
