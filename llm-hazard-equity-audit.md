# LLM Hazard Response Equity Audit (LHREA)

> A rigorous, reproducible benchmark testing whether eight major large language models give systematically worse emergency guidance to users in low-resource infrastructure contexts — with a pre-registered rubric, 800 evaluated responses, and a forensic record of a judge hallucination caught in the process.

| | |
|---|---|
| **Built for** | Personal / Research |
| **Timeline** | May 2026 |
| **Status** | Complete |
| **Team** | Solo |
| **Stack** | Python, promptfoo, OpenRouter API, CSV/JSON |

---

## The problem

A 2026 EarthArXiv preprint by Rafiezadeh Shahi et al. identified a pattern they named **Representational Flattening**: when asked for emergency guidance, large language models systematically default to advice calibrated for Western, high-income, car-dependent, grid-connected users — regardless of what the user has actually described about their situation.

The practical danger is concrete. A person in an informal settlement in Freetown asking an LLM for cyclone guidance and receiving advice to *"drive to the nearest shelter"* or *"charge your devices and monitor the emergency alert app"* has been given information that is not just useless — it may be actively harmful, consuming the time and trust that should go toward actions they can actually take.

The paper identified the pattern qualitatively. I wanted to measure it.

## What I built

A structured A/B benchmark — **LHREA** — designed to quantify Representational Flattening across eight frontier and open-weights models, with enough methodological rigour to be reproducible by anyone with an OpenRouter API key.

**The core design:**

- **100 scenarios** across 50 paired locations and 5 hazard types (Flash Flood, Extreme Heat, Wildfire, Landslide, Hurricane/Cyclone)
- **Condition A:** Each scenario includes an explicit description of local infrastructure — roads, power, connectivity, emergency services — calibrated to either a high-resource (structured) or low-resource (unstructured) context
- **Condition B:** The same scenarios stripped to bare location name and hazard type only, with no infrastructure context given to the model
- **The "blind" test:** Models are not told they are being audited. They receive neutral, realistic emergency requests. Any difference in advice quality is attributable to their assumptions about the user — not to the prompt phrasing

The **Phronesis Scale** (1–5) was pre-registered as the evaluation rubric before any model outputs were collected, preventing post-hoc rationalisation. It measures a single quality: *can a person in the described environment actually follow this advice with only the resources the prompt says they have?*

**Eight models tested:**

| Model | Provider | Open weights |
|---|---|---|
| Claude 3.7 Sonnet | Anthropic | No |
| Claude 3 Haiku | Anthropic | No |
| GPT-4o | OpenAI | No |
| Gemini 2.5 Flash | Google | No |
| Llama 3.1 8B Instruct | Meta | Yes |
| Llama 3.1 70B Instruct | Meta | Yes |
| Mistral NeMo 12B | Mistral AI | Yes |
| DeepSeek V3.2 | DeepSeek | Yes (weights) |

The open-weights models were deliberately included because they're the ones most likely to be self-hosted by NGOs and community organisations in lower-income countries — exactly the settings where unstructured infrastructure is the reality.

## What I found

**The short version:** Representational Flattening is real, but it's almost entirely concentrated in one model.

**Condition A (explicit infrastructure context given):**
- Structured mean Phronesis score: **4.00 / 5**
- Unstructured mean Phronesis score: **4.00 / 5**
- Equity gap: **0.00**

When the model is told exactly what resources the user has, it gives appropriate advice for both structured and unstructured users. No gap. This was true for all eight models.

**Condition B (bare prompt — no infrastructure context):**
- Structured mean: **3.98 / 5**
- Unstructured mean: **3.96 / 5**
- Equity gap: **0.02**

The gap is small in aggregate — but it disappears entirely when you remove one outlier.

**Llama 3.1 8B Instruct is the primary risk model.** In Condition B its mean score dropped from 4.00 to 3.79 (−0.21), and its equity gap widened to 0.14 — seven times larger than Mistral NeMo's (the next worst) and absent in every other model. The failure concentrated in wildfire scenarios, where Llama 3.1 8B produced flat refusals — *"I can't provide information on that topic"* — for six scenarios when infrastructure context was removed. The same model had given substantive 4/5 responses to those scenarios in Condition A. The wildfire refusals hit both structured (Sacramento, Malibu) and unstructured (Bogotá, Jakarta, Caracas) locations: this isn't structured-vs-unstructured bias in the conventional sense, it's a content policy miscalibration that lands hardest on unstructured users because they have fewer alternatives to fall back on.

The six frontier-tier models (Claude 3.7 Sonnet, Claude 3 Haiku, GPT-4o, Gemini 2.5 Flash, Llama 70B, DeepSeek V3.2) showed **zero measurable degradation** when context was removed — demonstrating that geographic inference without explicit infrastructure context is achievable at scale.

## The judge hallucination incident

During review, the Claude 3 Haiku evaluation judge was found to be hallucinating evaluation steps for responses that were outright refusals — awarding scores of 3–4/5 to empty or one-sentence outputs by fabricating reasoning that was never present in the text. This was caught through manual spot-checks.

Five scores were corrected with human-assigned values before the final analysis. The full incident — raw judge outputs, the specific responses that triggered it, and the correction rationale — is documented in `LLM_JUDGE_HALLUCINATION.md`. The finding itself became a secondary result: **AI evaluation pipelines require human spot-checks for refusal-class responses**, and refusal is the exact failure mode most likely to be underrepresented in automated evaluation at scale.

## What makes this project different

Most LLM bias audits score outputs qualitatively after the fact. This one was designed to be:

- **Pre-registered** — the rubric was finalised before any model outputs were collected
- **Reproducible** — the full benchmark runs with `promptfoo` and an OpenRouter API key; no proprietary tooling, no hidden prompts
- **Structurally controlled** — the "ask" is held constant across every pair; the only variable is infrastructure context, so any score difference is attributable to model assumptions, not prompt variation
- **Deployment-realistic** — model selection was guided by who actually uses these models in the field, not just which models perform best on leaderboards

The audit confirmed the preprint's hypothesis and extended it with a new dimension: the gap that doesn't exist when users provide their own context opens when they don't. The model most likely to be deployed in the settings most affected by that gap is the one that fails most severely.

## Reference

Rafiezadeh Shahi, K., Kuglitsch, M. M., Bove, J. B., et al. (2026). *Governing Generative AI in Disaster Risk Management.* EarthArXiv. https://doi.org/10.31223/X5HJ2B
