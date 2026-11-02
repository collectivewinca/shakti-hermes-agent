# Changelog

## 2026-11-02

- **Mistral `think` omit:** With reasoning off, Hermes was sending `extra_body.think = false`, which Mistral rejects (HTTP 422 `extra_forbidden` on `body.think`) and caused cron fallback to DeepSeek. The live fix (applied on ve-shakti against NousResearch/hermes-agent @ f14059fad2) omits `think` for model names starting with mistral, magistral, devstral, codestral, ministral, or pixtral; `reasoning_effort: none` is still sent. This fork has no `plugins/model-providers/` tree, so the change is tracked as a reapplicable patch in `patches/mistral-think-skip/` until upstreamed.
- **Mistral `think` omit NameError:** The first live skip used bare `model` in `build_api_kwargs_extras`, which is not in scope (the model name arrives via `**ctx`). The patch now uses `str(ctx.get("model") or "")`.
