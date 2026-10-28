# spare-key-clock

**How long should an external AI agent's sign-in stay alive?** A Monte Carlo composition model weighs re-login friction against stolen-token exposure, and a director memo rewrites itself from the results.

- **Live demo:** https://shriyashekatkar25-byte.github.io/spare-key-clock/
- **Gallery:** [APM Lab](https://shriyashekatkar25-byte.github.io/apm-lab/)
- Built by Shriya Shekatkar · UCLA · Stats 102C Monte Carlo Methods

## The launch that inspired it

On October 27, 2026 the Databricks platform release notes announced that **default OAuth apps for external AI agents are now generally available**: ChatGPT, Codex, Claude, Claude Code and Cursor can connect to Databricks MCP servers without an account admin creating an OAuth app first.
Source: [Databricks release notes, October 2026](https://docs.databricks.com/aws/en/release-notes/product/2026/october)

## The PM decision

When connecting an agent becomes zero-setup, the default session policy becomes the policy most customers live with. The decision this prototype frames: **what default idle-session lifetime (refresh-token TTL) should the default external-agent apps ship with**, and should scheduled agents get their own machine credentials instead of sharing the human's clock?

- **Success metric:** smooth starts, the share of agent sessions that begin without a forced login (target ≥ 90%).
- **Guardrails:** human re-logins ≤ 1.5 per user-week · scheduled runs broken by expiry ≤ 0.5% · mean usable window of a leaked token ≤ 24 h · leaked tokens usable > 72 h ≤ 5%.
- **Stakeholders:** account and workspace admins, customer security teams, agent-heavy data engineers and analysts, identity / MCP / AI gateway engineering, partner teams for the agent clients.

The lifetimes and persona parameters are hypothetical and simulated; they are not Databricks' actual settings.

## What the prototype shows about product judgment

| Databricks APM skill | Where it shows up |
|---|---|
| Customer problem framing | Two failure modes (friction vs exposure) and a named user story: the nightly agent job that dies at 3 a.m. |
| Prototyping | A working simulator with sliders, seeded re-rolls, a pixel timeline and a frontier chart across six lifetime options |
| SQL / Python | Illustrative SQL to measure real refresh gaps per client app, and a NumPy version of the mixture simulation |
| Data-driven decisions | Guardrail pass/fail per option, and a recommendation that picks the shortest lifetime that clears every guardrail (or escalates the split policy) |
| Clear writing | A memo with decision, evidence, risks, rollout with kill switch, next step, and a one-sentence VP summary |

Headline finding on the default seed: no single lifetime works for both people and scheduled agents, because any lifetime close to a job's cadence breaks runs at the boundary. Splitting robots onto machine credentials lets a 3-day human session clear every guardrail.

## Methods

| Course method (Stats 102C) | Used for |
|---|---|
| Ch5 Composition: mixtures | Each user is first assigned a persona (chat, coding agent, scheduled agent; scheduled agents are a daily/weekly mixture), then sampled from that persona |
| Ch5 Composition: Box-Muller | Normal draws turned into log-normal gaps between sessions |
| Ch5 Composition: convolutions | Session times are running sums of gaps; fleet leaks are a sum of rare Bernoulli events (Binomial) |
| Ch3 Inverse CDF (discrete) | Choosing a persona by partitioning [0,1] with cumulative weights |
| Ch1 Monte Carlo basics | Seeded PRNG, KPIs as sample means, ±1.96·SE band on smooth starts |

## Design baseline

Four pins from Shriya's Pinterest home feed, redrawn from scratch in CSS/SVG/canvas (no pin images are used):

- **Hero** – [jojosbromide, "the i in my name stands for irreplaceable little angel"](https://www.pinterest.com/pin/898749669411323757/): hot-magenta field, butter-yellow ultra-condensed uppercase affirmation with a star glyph mid-sentence.
- **Decision cards** – [kauuva, "Gotta love quirky colours and spirals"](https://www.pinterest.com/pin/1000713979721328071/): orange, blush pink, crimson, teal and chartreuse palette; hand-drawn spirals and stars; painterly grain.
- **KPI badges** – [yashikahahaha, "oops yaar"](https://www.pinterest.com/pin/1050886894322654480/): stark black condensed grotesque on white, lowercase meme-caption labels.
- **Simulation room** – [Ilia Vatti, pixel desk with a green-code monitor](https://www.pinterest.com/pin/778208010658854838/): pixel-art monitor bezel, green terminal glyphs on near-black, wood-grain desk panel for the controls.

---

Unofficial concept prototype by Shriya Shekatkar. All data is simulated. Not affiliated with any company.
