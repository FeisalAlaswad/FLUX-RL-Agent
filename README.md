# Latent Dynamics Inference: Sequence Modeling vs World Modeling in LLMs

## Abstract

Large language models achieve strong performance in language generation and knowledge-intensive tasks, yet remain limited in settings requiring causal reasoning, persistent state tracking, and long-horizon planning. We argue that these limitations may arise from an objective-level mismatch between sequence prediction and reasoning over latent environment dynamics. To formalize this distinction, we introduce **Latent Dynamics Inference (LDI)**, a conceptual perspective that interprets language and multimodal observations as partial evidence of underlying transition dynamics.

To empirically investigate this perspective, we introduce **Flux**, a sequential reasoning environment specified entirely through natural-language rules. As a proof-of-concept case study, the rules are first compiled into an explicit state-transition simulator, illustrating that structured latent transition dynamics can, in some cases, be operationally extracted from textual rule descriptions. This enables a controlled comparison between LLMs operating purely over textual observations and reinforcement-learning agents trained directly within the extracted latent state space.

Within this case study, agents operating with explicit access to the latent state space exhibit substantially more stable behavior in long-horizon gameplay, achieving an aggregate win rate of approximately **79% versus 11% for LLMs**. Qualitative analysis reveals failure modes consistent with unstable persistent state tracking, including invalid actions, state-tracking errors, and short-horizon reasoning failures.

The complete implementation of the Flux environment is available here:  
https://github.com/FeisalAlaswad/FLUX-RL-Agent

These results suggest that strong sequence prediction alone may struggle to support robust long-horizon dynamic reasoning without explicit state tracking and transition modeling.

---

## Keywords

Artificial General Intelligence, World Models, Large Language Models (LLMs), Latent Dynamics Inference, Causal Reasoning, Long-Horizon Planning

---

## 1. Formal Analysis: Sequence Modeling vs World Modeling

### 1.1 Sequence Modeling Objective

Autoregressive language models maximize:

\[
\max_{\theta} \; \mathbb{E}_{x \sim D} \sum_{t} \log P_{\theta}(x_t \mid x_{<t})
\]

This factorizes sequence probability into token-level predictions.

### 1.2 World Model Objective

A world model learns latent dynamics:

\[
s_{t+1} = f_{\theta}(s_t, a_t), \quad a_t \sim \pi(a_t \mid s_t)
\]

where \(s_t\) is the latent state and \(a_t\) is an action.

### Key Difference

- Sequence models: learn **observations**
- World models: learn **state transitions**

---

## 2. Observation Space vs Latent State Space

LLMs operate in token space \( \mathcal{X} \), while world models operate in latent state space \( \mathcal{S} \).

\[
x_t \sim P(x_t \mid s_t)
\]

This mapping is many-to-one and lossy, making inversion \(P(s \mid x)\) fundamentally ill-posed.

---

## 3. Structural Limitations of Next-Token Prediction

### 3.1 Hallucination as Objective Effect

Hallucination arises because models optimize:

\[
P(x_t \mid x_{<t})
\]

not truth consistency.

### 3.2 Causal vs Observational Learning

- LLMs learn \(P(Y|X)\)
- Agents require \(P(Y|\mathrm{do}(X))\)

---

## 4. Latent Dynamics Inference (LDI)

We propose:

> Natural language is a lossy observation channel of latent environment dynamics.

Formally:

\[
x_t \sim P(x_t \mid s_t)
\]

LDI aims to recover:

\[
P(s \mid x)
\]

### Core Insight

Language encodes:
- causal structure
- temporal structure
- intent

but not precise physical state.

---

## 5. Multimodal Extension

Video and text are both observation channels:

\[
v_t \sim P(v_t \mid s_t)
\]

Thus:
- Text → abstract dynamics
- Video → physical dynamics

---

## 6. Experimental Design: Flux

We construct **Flux**, a natural-language-defined game compiled into a simulator.

### Key Idea

- LLMs operate in observation space \( \mathcal{X} \)
- RL agents operate in latent space \( \mathcal{S} \)

---

## 7. Results

### Win Rates

- RL agent: **~79%**
- LLM agent: **~11%**

### Failure Modes

- Sum constraint violations
- State tracking errors
- Invalid actions
- Horizon myopia

---

## 8. Discussion

### Key Finding

Explicit latent state modeling improves long-horizon consistency.

### Interpretation

Performance gap arises from:
- absence of persistent state tracking in LLMs
- accumulation of observation-space errors

---

## 9. Implications of LDI

LDI reframes learning as:

> inference over latent processes, not prediction over observations

---

## 10. Architectural Implications

A full system may include:

1. Perceptual encoder (language/vision)
2. Latent world model
3. Planning module in state space

---

## 11. Limitations

- Simplified environment
- Fully observable state in Flux
- Not a universal performance claim

---

## Citation / Repository

Flux implementation:  
https://github.com/FeisalAlaswad/FLUX-RL-Agent