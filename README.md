# Quantified AI Risk Register

An AI risk register that produces **loss exposure ranges** instead of heat-map colours — while still
showing coverage against NIST AI RMF 1.0 and ISO/IEC 42001 Annex A.

**Live:** https://rootcawsllc.github.io/ai-risk-register/

![The "Where these numbers come from" panel. It states that every frequency and magnitude in the register is a seeded estimate rather than a sourced one, explains that no equivalent loss data exists for first-party AI system risk because the public incident catalogues record what happened without recording what it cost, and carries a live check reporting that of twelve governed shards in the sourced corpus one is AI-related — deepfake-enabled fraud, at low confidence — which covers an attacker using AI against you rather than your own system failing, so it anchors none of these scenarios. A closing paragraph sets out what a dataset would have to report to qualify one](preview.png)

This panel is the reason to look at this tool rather than the loss figures above
it. A register that produces dollar ranges owes you an account of where the
ranges came from, and here the honest answer is that they are seeded — checked
against the real corpus on every load, so the claim fails visibly rather than
quietly if that ever stops being true.

## Why this exists

NIST AI RMF 1.0 and ISO/IEC 42001 are good at what they are for. They tell you which questions an AI
programme should be able to answer, and they give you a vocabulary an auditor recognises.

What neither does — because neither is trying to — is tell you **how much AI risk you are actually
carrying**. So most AI risk registers end up as a list of scenarios with a colour beside each one.
Red is not a unit of measurement. You cannot add two reds, you cannot compare a red to a control
budget, and you cannot walk a CFO through one.

This register keeps the framework mapping, because you do need to show coverage. It just scores every
risk the way any other risk should be scored.

## What it does

**Twelve seeded AI risk scenarios** — prompt injection driving an unauthorised action, training data
poisoning, sensitive data leaving for a third-party provider, model inversion, confabulation inside a
customer-facing decision path, harmful bias in an eligibility decision, silent model drift, agentic
action without human approval, third-party model deprecation, shadow AI, IP exposure from generated
output, and inability to explain an adverse automated decision.

Each carries a trustworthiness characteristic and a pre-built mapping to the AI RMF categories and
ISO 42001 Annex A groups it bears on. Add or remove them to shape the register.

**Three-point estimates, not a matrix.** Every risk takes a frequency range (events per year) and a
loss magnitude range (dollars per event). A 10,000-iteration Monte Carlo — triangular sampling for
magnitude, Poisson for event counts — produces an annual loss distribution per risk and, summed
across the register on every iteration, an aggregate portfolio distribution reported at the mean,
P50, P90, and P99.

**Coverage that stays honest.** The framework panel shows which of the nineteen AI RMF categories and
nine Annex A groups the register touches, and which it does not. Coverage means a risk maps to that
category — *not* that the category is satisfied. That is a control question, and a register cannot
answer it.

**Markdown export** of the whole register, the aggregate statistics, and the coverage gaps.

## The distribution matters more than the mean

With a rare, high-impact risk in isolation — model inversion, seeded at 0.02–0.15–0.8 events per year
— roughly **73% of simulated years see no loss event at all**. The expected value is being carried
entirely by the years that are not quiet. Reporting only a mean hides that completely, which is why
the tool reports the P90 and P99 next to it and says so in the panel.

## Honest limits

**The seeded ranges are illustrative and they are the weakest part of the model.** Frequency and
magnitude for AI-specific loss events are not well established — the incident base is thin and mostly
unpublished. Replace them with your own calibrated estimates before the output means anything about
your organisation. The tool is a structure for your numbers, not a source of them.

**No scenario here is offered a benchmark, and that is a finding rather than a shortcut.** The other
tools in this set can start from a source-backed shard; this one deliberately cannot. The register
models *first-party* AI system risk — injection, poisoning, inversion, confabulation, drift, agentic
action without approval. The sourced corpus at
[risk-benchmarks](https://github.com/RootCawsLLC/risk-benchmarks) contains exactly one AI-related shard,
deepfake-enabled fraud, which is an attacker using AI *against* you through social engineering: an
adjacent but different threat class, with all six of its parameters at low confidence. Anchoring
prompt injection to it would produce a citation without producing a measurement, which is worse than
an openly seeded number. The tool checks the corpus on load and says what it finds, so if a
first-party AI shard ever qualifies, this claim stops being true visibly rather than silently.

A scenario here becomes source-backed when a public dataset reports per-incident loss for that
failure mode, on a stated population, with enough events to support a range rather than an anecdote.
Incident counts are not enough. A single headline settlement is not enough.

**Mapping is at category and Annex A group level.** AI RMF subcategories and individual Annex A
controls are deliberately not claimed, because a scenario-level register cannot honestly assert that
granularity.

**Coverage and exposure answer different questions.** Coverage tells an auditor the programme is
complete. Exposure tells a board whether it is worth what it costs. You need both, and neither
substitutes for the other.

## Running locally

Single self-contained `index.html` — React 18 via UMD CDN, no build step, no dependencies.

```bash
python -m http.server 8000
```

Then open http://localhost:8000. Nothing you enter leaves the browser — no backend, no storage, no
telemetry. The page does fetch React, the webfonts, and the benchmarks corpus it checks its own
provenance claim against; none of those requests carries anything you entered, and if the corpus is
unreachable the panel says so and the register is otherwise unaffected.

## Related

- [fair-model-study](https://github.com/RootCawsLLC/fair-model-study) — the FAIR taxonomy this scoring approach comes from
- [loss-exceedance-curve](https://github.com/RootCawsLLC/loss-exceedance-curve) — reading an aggregate distribution against thresholds
- [cyber-materiality-workbench](https://github.com/RootCawsLLC/cyber-materiality-workbench) — what happens when one of these risks materialises

## Attribution

Category and control names are from [NIST AI RMF 1.0](https://www.nist.gov/itl/ai-risk-management-framework)
and ISO/IEC 42001:2023, used for mapping reference only. This project reproduces neither standard.

## License

Copyright (c) 2026 RootCaws LLC.

[GNU AGPL v3 or later](LICENSE). If you modify this and run it as a network service, the AGPL requires you to offer your users the modified source under the same terms.
