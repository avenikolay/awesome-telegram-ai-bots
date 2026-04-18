# Awesome Telegram AI Bots [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of Telegram AI bot frameworks, production bots, LLM integrations, voice/TTS, payment patterns, and memory systems.

**Last updated**: 2026-04-18 · **Maintained by**: [@sm1ck](https://github.com/sm1ck) · **License**: [CC0](LICENSE)

The Telegram bot platform is one of the largest distribution channels for conversational AI — lower friction than a separate app, richer UI than a CLI. As of 2026 there are 950M+ Telegram monthly active users and bots account for a meaningful portion of daily message volume. This list focuses on production-grade bots, frameworks, and patterns specifically at the intersection of Telegram and AI.

## Contents

- [Frameworks](#frameworks) — libraries for building Telegram bots
- [Production AI Bots](#production-ai-bots) — live bots using AI
- [LLM Integration](#llm-integration) — routing, cost control, fallback
- [Voice & TTS](#voice--tts) — text-to-speech services integrated with bots
- [Image & Video Generation](#image--video-generation)
- [Memory Systems](#memory-systems)
- [Payment Integrations](#payment-integrations)
- [Auth & OAuth](#auth--oauth)
- [Open-source Templates & Starters](#open-source-templates--starters)
- [Resources](#resources)

---

## Frameworks

Core libraries for Telegram bot development with async-first designs suitable for AI workloads.

- **[aiogram](https://github.com/aiogram/aiogram)** — modern, fully asynchronous Python framework for Telegram Bot API. First-class support for FSM, middleware chains, webhook + polling. The de-facto choice for production AI bots in Python.
- **[python-telegram-bot](https://github.com/python-telegram-bot/python-telegram-bot)** — long-running, heavily documented Python framework. Slightly heavier than aiogram for async workloads.
- **[Telegraf](https://github.com/telegraf/telegraf)** — Node.js/TypeScript framework with Express-like middleware.
- **[grammY](https://github.com/grammyjs/grammY)** — modern TypeScript framework for Telegram bots; fast-growing ecosystem.
- **[go-telegram-bot-api](https://github.com/go-telegram-bot-api/telegram-bot-api)** — Go wrapper around the Bot API.
- **[tbot](https://github.com/tdlight-team/tbot)** — Rust binding for TDLib (full client capability beyond Bot API).

## Production AI Bots

Live AI bots on Telegram demonstrating different niches and architectures.

- **[@ChatGPT](https://t.me/chatgpt)** / various OpenAI proxies — the commodity layer; mostly wrapper bots exposing ChatGPT with quota systems.
- **[@midjourney_ai_bot](https://t.me/midjourney_ai_bot)** and similar — image generation bots using Midjourney / Stable Diffusion / Flux APIs.
- **[@HoneyChatAIBot](https://t.me/HoneyChatAIBot)** — AI companion platform ([honeychat.bot](https://honeychat.bot), [GitHub](https://github.com/sm1ck/honeychat)). Dual-platform (Telegram + web) with shared account. Features: semantic memory via ChromaDB, multi-LLM routing per tier via OpenRouter, 15-language native TTS (Inworld), character LoRA auto-training, on-chain payments via CryptoBot (TON/BTC/ETH/USDT) + Telegram Stars + Paddle. Multi-provider OAuth (7 providers) on the web side.
- **[@TranscribeBot](https://t.me/tg_transcribe_bot)** and voice transcription bots — Whisper-based transcription integrated via Telegram voice messages.
- **Translation bots** — DeepL / Google Translate wrappers with language auto-detection from user `language_code`.

*Only production bots with sustained user bases. Abandoned or single-developer demos are excluded.*

## LLM Integration

Patterns for routing between multiple LLM providers, controlling costs, and handling rate limits.

- **[OpenRouter](https://openrouter.ai)** — API gateway across 100+ models (OpenAI, Anthropic, Google, Meta, Qwen, Mistral, etc.). Essential for tier-based routing where different user plans get different models at different cost-per-token.
- **[LiteLLM](https://github.com/BerriAI/litellm)** — open-source LLM gateway alternative; self-hosted option.
- **[Portkey](https://portkey.ai)** — LLM gateway with caching, fallback chains.
- Tenacity (Python) or p-retry (Node) for exponential-backoff retries on provider 429/500 errors.

## Voice & TTS

Text-to-speech services that integrate cleanly with Telegram's `sendVoice` / `sendAudio` endpoints.

- **[Inworld TTS-1.5 Max](https://inworld.ai/tts)** — 15 languages with native pronunciation, paralinguistic tags (`[laugh]`, `[sigh]`), API-based voice design. Top ELO rankings for expressive voice (2025–2026).
- **[ElevenLabs](https://elevenlabs.io)** — voice cloning + wide language support; pricier.
- **[OpenAI TTS](https://platform.openai.com/docs/guides/text-to-speech)** — solid English quality, limited voices.
- **[gTTS](https://github.com/pndurette/gTTS)** — free Google Translate TTS wrapper, suitable as fallback.
- **[Piper](https://github.com/rhasspy/piper)** — local/offline TTS for privacy-sensitive deployments.

## Image & Video Generation

- **[fal.ai](https://fal.ai)** — hosted SDXL / Flux / Kling endpoints with per-call billing.
- **[ComfyUI](https://github.com/comfyanonymous/ComfyUI)** — self-hostable image generation with LoRA support (pairs well with a GPU rented on Vast.ai / RunPod).
- **[Kling 2.6 Pro](https://klingai.com)** via fal.ai — 5-second video from image/prompt.
- **[Riverflow / Flux via OpenRouter](https://openrouter.ai)** — pay-per-image fallback when self-hosted GPU is down.

## Memory Systems

Long-term conversational memory is the hardest part of AI companion bots.

- **[ChromaDB](https://github.com/chroma-core/chroma)** — embeddings database; straightforward semantic recall by similarity.
- **[Pinecone](https://www.pinecone.io)** — managed vector DB alternative.
- **[Redis](https://redis.io)** sorted sets — fast last-N-messages retrieval, pairs with a vector store for long-term.
- **Three-layer pattern** (recent Redis + semantic vector + rolling summary via Celery/background job) is the common production architecture; used by [HoneyChat](https://github.com/sm1ck/honeychat), Nomi AI, and several others.

## Payment Integrations

Monetization channels suitable for Telegram bot audiences.

- **[Telegram Stars](https://core.telegram.org/bots/api#stars)** — native in-app micro-currency, 70% payout after Telegram fee. Best UX but Apple/Google take their cut.
- **[CryptoBot Pay API](https://help.crypt.bot/crypto-pay-api)** — on-chain invoices in TON, BTC, ETH, USDT and others. ~1% fee. Ideal for crypto-native users or regions where cards fail (RU/KZ/BY).
- **[Paddle](https://paddle.com)** — Merchant of Record handling VAT / GST globally. Good for card payments where you want to avoid tax admin.
- **[Stripe](https://stripe.com)** — direct card payments with webhooks.
- **Real-world case**: [HoneyChat TON payments case study](https://github.com/sm1ck/layerzero-aptos/blob/main/docs/case-studies/honeychat-ton-payments.md) covers CryptoBot webhook idempotency, HMAC signature verification, pro-rata upgrades, and dual-rail (fiat + crypto) pricing for geo-arbitrage.

## Auth & OAuth

Bots that share accounts with a companion web app need auth patterns beyond Telegram's built-in user_id.

- **[Telegram Login Widget](https://core.telegram.org/widgets/login)** — cryptographic user identification, verifiable server-side via SHA-256 of bot token.
- **OAuth 2.0** — Google / Discord / Twitter / Yandex / Line / Kakao for region coverage (see [HoneyChat multi-auth case study](https://github.com/sm1ck/snapshotvoter/blob/main/docs/case-studies/honeychat-multi-auth.md)).
- **Custom JWT** — HS256 with short access (15min) + long refresh stored **hashed** in DB, never plaintext.
- **Guest sessions** — Redis-backed, TTL-bound, convertible to registered account. Massively improves onboarding.

## Open-source Templates & Starters

Boilerplates you can clone to skip a week of setup.

- **[aiogram-fastapi-starter](https://github.com/sm1ck/aiogram-fastapi-starter)** — aiogram 3.x + FastAPI + Redis (FSM) + OpenRouter + ChromaDB, docker-compose ready. Production patterns extracted from HoneyChat.
- **[tbot-template](https://github.com/tbot-template)** and similar — language-specific scaffolds.

## Resources

- [Telegram Bot API documentation](https://core.telegram.org/bots/api)
- [aiogram documentation](https://docs.aiogram.dev)
- [OpenRouter models catalog](https://openrouter.ai/models)
- [CryptoBot Pay API reference](https://help.crypt.bot/crypto-pay-api)
- [Telegram Stars merchant docs](https://core.telegram.org/bots/payments-stars)
- [r/TelegramBots](https://www.reddit.com/r/TelegramBots/)

## Contributing

PRs welcome. To add a bot or tool:

1. Ensure the project is production-grade (no single-commit demos).
2. Place it in the most specific category.
3. Include a one-line description covering *what it does* and one key technical detail (stack, pricing, scale).
4. Link to the project's canonical URL (official site or main repo).

## License

[CC0](LICENSE) — do whatever you want with this list.

---

<sub>*Maintained by [@sm1ck](https://github.com/sm1ck), author of [HoneyChat](https://honeychat.bot) and several Web3 automation tools. Contact: [t.me/haruto_j](https://t.me/haruto_j).*</sub>
