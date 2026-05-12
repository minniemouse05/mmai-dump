# Homework 5 - AI Agents

*(aka Building a Restaurant Recommendation Bot That Browses the Web So You Don't Have To!)*

I built a restaurant recommendation agent end-to-end using smolagents, starting from a baseline tool-calling agent with web search and webpage visiting, then extending it with custom tools (Overpass API, open-now checker), a vision-enhanced multimodal variant, safety evaluation with system prompt mitigations, Langfuse observability tracing, and finally deploying it as a Discord bot. I also formalized the task as a sequential decision problem, compared ReAct vs. Reflexion architectures, and ran a multi-configuration online evaluation across Sonnet/Haiku models.

[See python notebook here](homework/homework-5/homework-5.ipynb)

## Agent as a Sequential Decision Problem

The restaurant recommendation agent is formalized as a sequential decision problem where the agent must extract user preferences (cuisine, location, price, dietary restrictions), search for matching restaurants via web tools, and converge on a recommendation that satisfies all hard constraints. The observation space includes the user's latest message, a running preference profile, and prior query results. The action space covers clarifying questions, filtered restaurant searches, detail fetching, recommendation delivery, and refinement. Transitions are deterministic on the tool side (web search returns fixed results) but stochastic on the user side (preferences can shift mid-conversation).

## Evaluation Design

The offline evaluation set covers 12 tasks across 3 categories — normal, edge, and adversarial — scored on three metric families:

| Metric | What It Measures | Pass Threshold |
|---|---|---|
| Correctness | Restaurant exists + matches query constraints, zero hallucinations | Precision ≥ 0.8 |
| Trajectory | Constraints extracted, tool called before recommending, URLs cited | 3/3 behaviors |
| Operational | Wall-clock latency from query to final answer | < 60 seconds |

## Baseline Agent

The baseline agent uses `Qwen/Qwen2.5-7B-Instruct` with `WebSearchTool()` and `VisitWebpageTool()`. It performed well on normal cases (real addresses, relevant results) and correctly refused adversarial queries (e.g. reporting failure instead of hallucinating a nonexistent restaurant). 

Key failure modes included Yelp/OpenTable blocking web scraping (tool access errors) and missing location context in search queries, causing geographically incorrect results.

I also explored adding custom tools, like `OverpassRestaurantSearchTool`, `CheckOpenNowTool`.

## Vision-Enhanced Agent

The vision-enhanced agent uses a VLM Code Agent that reasons over text + browser screenshots jointly at each step, with a `save_screenshot()` callback after each action.

<img src="imgs/architecture-diagram.png" width="50%">

### Text-Only vs. Vision Agent Results

<img src="imgs/text-agent-results.png" width="60%">

<img src="imgs/vision-agent-results.png" width="60%">

The text-only baseline achieved 3/5 success with ~37s mean latency, while the vision agent achieved 2/5 with ~828s mean latency. The vision agent handled adversarial cases better (both passed vs. 1/2 for text-only) but timed out on normal and edge cases due to the computational overhead of processing images. A specific weakness was inefficient photo gallery navigation on Google Maps, where most of the 20 allowed steps were consumed trying to scroll through carousels.

The vision agent's success case was notable: given a query about seafood restaurants for someone with a shellfish allergy, it navigated to Legal Sea Foods, read the actual menu page, returned verified fish dishes with prices (Grilled Salmon $38, Rainbow Trout $31.75, Haddock $32), and correctly warned about cross-contamination risk.

## Safety Evaluation

Five challenging prompts tested the agent across privacy, out-of-scope, and unsafe/deceptive categories. The key finding was that the baseline system prompt had no scope boundary — it searched for gun shops the same way it searched for restaurants. Adding an explicit safety policy section to the system prompt fixed the out-of-scope case (latency dropped from 17.9s to 2.4s via one-step refusal) while maintaining behavior on cases the base model already handled correctly.

<img src="imgs/safety-evaluation.png" width="80%">

## Observability with Langfuse

All agent runs were instrumented with Langfuse for trace inspection. Across 5 traced runs using Claude Sonnet, total cost was $0.71 over 222.5K tokens.

<img src="imgs/langfuse-dashboard.png" width="60%">

Trace inspection revealed that latency was consistently dominated by the final answer generation step (not tool calls). For example, the Italian restaurant query spent 2.8s on the initial search but 163s generating the structured final answer with multiple restaurants.

## Discord Bot 

The agent was deployed as a Discord bot (`MMAI-Restaurant-Bot`) using an @mention-only trigger strategy. This keeps the bot dormant during regular channel conversation, avoiding unnecessary API costs from always-on or keyword-triggered strategies.

<img src="imgs/discord-bot.png" width="50%">
