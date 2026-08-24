# Prompt Library: Senior PM 

A set of reusable prompts for the five tasks that come up most for a senior PM running offers and smart campaigns on a delivery/logistics platform. Each prompt is written to be pasted into Claude (or another LLM) with your specifics filled in.

## How to use this library
1. Paste the **Standing Context Block** (Section 0) into a fresh conversation once.
2. Follow it immediately with the task-specific prompt for what you need.
3. Treat the first response as a draft — the follow-up nudges under each prompt are usually where the real value shows up.

---

## 0. Standing Context Block

Paste this once at the top of a session, filled in, so you don't have to re-explain your product every time.

```
Context for this conversation:
- Company/product: [e.g., "Delivery/logistics platform operating in X cities, ~Y active users"]
- My focus area: Offers & smart campaigns (discounts, promos, personalized targeting, loyalty, push/SMS/email campaigns)
- Stage of this initiative: [idea / discovery / in design / in dev / in experiment / post-launch]
- Target users/segment: [e.g., "lapsed users in metro areas", "high-frequency low-AOV customers"]
- Key business metrics we optimize for: [e.g., redemption rate, CAC, order frequency, margin per order, retention]
- Tech/data context: AWS-based stack [mention if relevant: personalization via SageMaker, event data in Redshift/Kinesis, feature flags, experimentation platform]
- Team: [e.g., "1 PM (me), 2 eng, 1 data scientist, 1 designer"]
- Known constraints: [budget caps, margin thresholds, fraud/abuse history, regulatory limits on discounting, etc.]

Keep this context in mind for everything below unless I say otherwise.
```

---

## 1. Drafting a PRD

**When to use:** Kicking off a new offers/campaigns initiative — a new discount mechanic, a smart-targeting model, a loyalty tier, a campaign automation feature.

```
Act as a senior product management partner helping me draft a PRD for a delivery/logistics offers & campaigns feature.

Before drafting anything, ask me up to 6 clarifying questions about: the problem, target user, current workaround, success metrics, technical constraints (we're AWS-based), and rollout risk. Wait for my answers.

Once I've answered, draft a PRD with this structure:
1. Problem statement (what's broken/missing today, backed by the context I gave you)
2. Goals and explicit non-goals
3. Target users / segment
4. Proposed solution (narrative, not just a feature list)
5. Key user stories / flows
6. Success metrics — primary metric, guardrail metrics, and how each will be measured
7. Scope: MVP vs later phases
8. Risks & open questions (flag anything you're inferring rather than working from what I told you)
9. Rollout plan (experiment first? staged geo rollout? kill criteria?)
10. Cross-functional dependencies (data/eng/design/legal/ops — flag anything touching courier-side incentives or fraud exposure)

Write it in plain, direct language a VP of Product could skim in 3 minutes. Flag any assumption explicitly rather than presenting it as fact.
```

**Follow-ups worth running after the draft:**
- "Play devil's advocate: what's the strongest argument for *not* building this?"
- "Rewrite the success metrics section assuming leadership will only fund this if it moves [specific metric] within [timeframe]."
- "Turn section 6 into a one-pager I can put in a slide."

---

## 2. Giving Feedback on a Strategy

**When to use:** You've been handed a campaigns/offers strategy doc, quarterly plan, or GTM plan (yours, a peer's, or leadership's) and need a sharp, structured critique before it goes further.

```
Act as a skeptical but constructive peer reviewer — think a sharp VP of Product, not a cheerleader. I'm going to share a strategy document for offers/campaigns on our delivery platform.

Evaluate it against these lenses:
1. Problem/evidence: Is the problem well-defined and backed by real signal, or asserted?
2. Market & competitive context: Does it account for what competitors are doing with offers/discounting?
3. Feasibility: Given we're an AWS-based stack with [team size/data maturity from context], is this realistically buildable in the stated timeline?
4. Economics: Does it address margin impact, discount cannibalization, and CAC/LTV tradeoffs, or just top-line growth?
5. Measurability: Are success metrics specific, and do they include guardrails (fraud, margin erosion, notification fatigue/unsubscribes)?
6. Risks & blind spots: What's missing that would embarrass us in 6 months if we didn't think about it now?

Structure your feedback as:
- **Strongest parts** (2-3 things that are genuinely well-reasoned)
- **Gaps** (specific, not generic — point to the section)
- **Questions I'd ask in the room**
- **Overall recommendation**: proceed / proceed with changes / rework, with a one-line reason

Here's the strategy doc: [PASTE DOC]
```

**Follow-ups worth running:**
- "If you had to kill one initiative in this doc to protect margin, which would it be and why?"
- "Steelman the parts you just criticized — what would the author say back to you?"

---

## 3. Finding Holes in a Prototype

**When to use:** Reviewing a Figma flow, clickable prototype, or spec for an offers/campaigns feature before it goes to dev — hunting for edge cases and gaps before they become bugs or incidents.

```
Act as an adversarial reviewer whose job is to find every way this prototype breaks before we ship it — think QA lead + fraud analyst + support-escalation owner combined.

I'll describe (or paste screenshots/a link to) a prototype for [feature name] on our delivery/logistics app. Walk through it screen by screen and surface:

1. Edge cases in the core flow: expired offers, stacked/conflicting promos, partial cart eligibility, timezone/geo edge cases, delivery radius boundary cases, offer applied then item removed from cart, price changes mid-checkout
2. Failure states: payment failure, network drop mid-redemption, offer running out of budget/inventory in real time
3. Abuse/fraud vectors: multi-accounting, referral loop abuse, coupon stacking exploits, bot redemption
4. Data/instrumentation gaps: what events we would NOT be able to measure with this design that we'd want for the success metrics
5. Accessibility & comprehension: is the offer's value and terms clear at a glance, or does it need a tooltip/legal disclaimer
6. Cross-team blind spots: anything that would surprise support, ops, or courier-side teams

For each issue, rate severity (blocker / should-fix / nice-to-fix) and suggest the smallest fix.

Here's the prototype: [PASTE DESCRIPTION, SCREENSHOTS, OR LINK]
```

**Follow-ups worth running:**
- "Which 3 of these would you bet actually happen in production in month one?"
- "Write the edge cases as a test-case checklist I can hand to QA."

---

## 4. Creating an LLM Judge

**When to use:** You have an LLM generating or scoring something at scale — personalized offer copy, push/SMS notification text, campaign targeting explanations, chatbot responses about promos — and need an automated way to evaluate quality instead of manually reading every output. An "LLM judge" is just a second prompt whose only job is to score another AI's output against a rubric you define, so you can catch bad outputs before customers see them and track quality over time.

```
Help me design an LLM judge to evaluate [what's being generated — e.g., "personalized push notification copy for offer campaigns"].

Step 1 — Rubric: Propose 4-6 scoring criteria relevant to this use case. For offers/campaign copy, consider: factual accuracy about offer terms (no overpromising discount %, dates, eligibility), on-brand tone, personalization relevance to the segment, compliance/legal safety (no unauthorized claims), clarity, and call-to-action strength. Push back if you think I'm missing an important criterion.

Step 2 — Scale: For each criterion, define a simple scale (e.g., 1-5 or pass/fail) with a one-line description of what each score level looks like, so two different people scoring the same output would land on the same number.

Step 3 — Few-shot anchors: Ask me for 2-3 example outputs per criterion (a clear good one, a clear bad one, a borderline one) so the judge has anchors. If I don't have examples yet, draft plausible ones for me to sanity-check.

Step 4 — Output the judge as a ready-to-use system prompt that:
- Takes the generated content + relevant context (offer terms, target segment) as input
- Returns a structured JSON score per criterion, an overall pass/fail, and a one-sentence reason for any failing criterion

Step 5 — Flag known failure modes of LLM judges I should watch for (e.g., leniency bias, being fooled by confident-sounding but wrong claims about offer terms) and suggest one way to spot-check the judge against human ratings before I trust it.
```

**Follow-ups worth running:**
- "Now write 5 adversarial test cases designed to fool this judge."
- "I have 20 human-rated examples — here they are. Tell me where the judge and the humans disagree and why."

---

## 5. Analyzing Experiments

**When to use:** An A/B test on an offer or campaign feature has results in — a new discount threshold, a smart-targeting model, notification timing/frequency — and you need a rigorous read before deciding ship/iterate/kill.

```
Act as a data-literate strategic partner helping me read out an experiment — rigorous enough to catch a bad call, but explained in plain language rather than pure stats-speak (I have basic data literacy, not a data science background).

I'll share the experiment setup and results below. Please:

1. Restate the hypothesis and what decision this experiment is meant to inform.
2. Check statistical significance AND practical significance — a result can be "significant" but too small to matter for the business; call that out if it applies.
3. Look for segment effects worth flagging (e.g., worked for new users but not existing, one city but not others) even if not the primary cut.
4. Check guardrail metric movement — margin, refund/complaint rate, unsubscribe/opt-out rate, fraud/abuse signals — not just the primary metric.
5. Flag risks to trust in the result: novelty effects, sample ratio mismatch, seasonality, insufficient runtime, contamination between arms.
6. Give a clear recommendation: ship / iterate / kill / extend the test — with reasoning in 2-3 sentences, and name what would change your recommendation if you're missing context.
7. List the follow-up questions you'd want answered before this goes to leadership.

Here's the experiment: [PASTE HYPOTHESIS, SETUP, METRICS/RESULTS TABLE]
```

**Follow-ups worth running:**
- "Write the 3-sentence Slack summary I'd send to leadership."
- "If this shipped and margin dropped unexpectedly next month, what in these results would have predicted it?"

---

## Using this library well
- Fill in Section 0 once per session, not once per prompt — it carries context across the whole conversation.
- These prompts are starting points, not scripts — cut sections you don't need.
- Chain prompts: use the PRD output as input to the strategy-feedback prompt, or prototype-review output as input to the experiment-design conversation before you build.
