# Interrogating a Spec

*A review guide for any feature that claims to produce a score, a ranking, a prediction, or a grade.*

Most feature specs can be reviewed by reading them. A spec that produces a **number** has to be interrogated, because a number looks like evidence. "Returns an AI score from 0 to 100" reads as a finished design when it is actually a sentence with the mechanism deleted. The spec describes an outcome — detection, accuracy, confidence — and skips the machinery that would make the outcome true. Vagueness survives longest here, because nobody can point at a wrong number until the product is already in front of users.

The seven questions below were written about a proposed AI-content detector. They are worth reading as a worked example, but the shape generalises: each one converts a claim into a mechanism, a measurement, or a decision. A spec that cannot survive them is not a spec yet.

> If the answer to "what is the mechanism" is "AI", the spec is not written.

## What the spec said, and what it left out

The original proposal was one sentence: *detect AI-generated content and return a score.* Expanded into its actual claims, the gaps are easy to see.

| What the spec claimed | What it left out | Which question exposes it |
| --- | --- | --- |
| Detects AI content | The signal, the model, the training data | 1 and 3 |
| Returns a score | The scale, the calibration, the threshold | 2 |
| The score is about the document | Evidence spans, input contract, dependence on the input | 4 |
| Uses Langgraph | The graph, the state, the requirement it satisfies | 5 |
| Has a workflow and models | The steps, the failure paths, the model roles | 6 |
| Has a tech stack | The rejected alternatives and the accepted costs | 7 |

Six claims, six missing mechanisms. That ratio is the signature of a spec written from the outcome backwards.

---

## The seven questions

### 1. How is this app going to detect AI content?

> "How is this app going to detect AI content?"

**What it's really testing.** Whether a mechanism exists at all, or whether the feature is a verb with a model bolted to it. Detection is a measurable claim about text, and every measurable claim has a signal behind it.

**What a good answer contains.** A named signal and how it is computed: perplexity or token-likelihood distribution under a reference model, burstiness and sentence-length variance, stylometric features, a classifier trained on labelled human/machine pairs, watermark extraction, provenance metadata, or an explicit combination with weights. Then the honest details: which signal dominates, what training data the classifier saw and when, what happens on text under ~100 words, on code, on translated text, and on non-native English writers. The answer should survive the question "what does it do when the signal is absent?" — because that case is most of the traffic.

**Red flags.** "We use AI to detect AI." "Advanced algorithms." "State-of-the-art model." A single unnamed API call with no description of what it returns. No mention of short text, code, or edge inputs. An answer that describes the UI instead of the computation.

### 2. How precise is the AI score?

> "How precise is the AI score?"

**What it's really testing.** Whether the number means anything, or whether it is a decorated confidence that nobody has calibrated.

**What a good answer contains.** A definition of the scale and its semantics — is 87 a probability, a percentile, or an arbitrary index? A calibration statement: at score 87, what fraction of documents were actually machine-generated in the evaluation set? A stated threshold and what happens on each side of it. Precision and recall at that threshold, on a named test set, with the base rate of that set disclosed. A confidence interval, and a statement about how the interval widens for short or mixed-authorship documents.

**Red flags.** One accuracy figure with no test set. "99% accurate." Precision used as a synonym for confidence. No threshold, so the score is never converted into a decision. No mention of the base rate, which is the number that decides whether a positive result is meaningful at all. A precision claim that silently assumes a 50/50 split of human and machine text.

### 3. What technologies did you use for detecting the AI content?

> "What technologies did you use for detecting the AI content?"

**What it's really testing.** Whether the stack is a system or a wrapper. Naming the components is the fastest way to reveal how much of the feature is one HTTP request.

**What a good answer contains.** Concrete components with versions: the classifier and its weights, the tokenizer, the embedding or reference model used for likelihood features, the inference runtime and where it runs, the queue or batch layer if scoring is asynchronous, and the evaluation harness that produced the numbers in question 2. If a third-party detector is involved: which vendor, what the contract says about data retention, what the latency and rate limits are, and what the fallback is when it is unavailable.

**Red flags.** Technology named at the category level only — "a transformer", "an LLM", "a cloud ML service". No versions. No evaluation harness, which means the accuracy claim has no origin. A proprietary black box with no stated data handling. No answer to "what happens when the vendor is down or changes their model?"

### 4. Does the output this application produces relate to the source material?

> "Does the output this application produce, is it relevant to the source material?"

**What it's really testing.** Faithfulness. Whether the output is derived from the input or merely displayed next to it. This is the cheapest test in the set: change the input and see whether the output changes in the way the spec predicts.

**What a good answer contains.** An input contract — what counts as valid source material, minimum length, accepted formats, language assumptions. Evidence spans: which sentences drove the score, quoted from the input, not paraphrased. A description of how the explanation is produced, and specifically whether it comes from the same computation as the score or from a separate generation step. A perturbation test: swap two documents of similar length and confirm the scores diverge in the expected direction.

**Red flags.** An explanation written by a language model that never saw the classifier's features — post-hoc rationalisation dressed as evidence. The same score band for wildly different inputs. Explanations that could be pasted onto any document. No cited spans, only adjectives. No answer for empty, truncated, or non-text input.

### 5. Why did you use Langgraph?

> "Why did you use Langgraph?"

**What it's really testing.** Whether the architecture choice was made against the simplest thing that would have worked. A workflow framework is a cost, and costs need a reason attached.

**What a good answer contains.** The graph itself: nodes, edges, state, and the specific requirement that a plain function could not satisfy — branching on intermediate results, retries with different models, checkpointing a long-running run, or resuming after human review. Then the rejected alternatives by name: a single prompt, a plain function with a loop, a queue with explicit steps. Then the bill: extra dependency, harder debugging, a state schema to maintain.

**Red flags.** "It's popular." "It's the standard for agents." A graph with one node. A framework wrapped around a single API call. An author who cannot draw the graph from memory. Justification that would apply equally to every project in the company, which means it was not a decision about this one.

### 6. Define this application's logic, workflow and models used.

> "Define this application's logic, workflow and models used."

**What it's really testing.** Whether the author holds the whole system in their head. This is the coherence test, and it has a time limit: three sentences, or it is not designed.

**What a good answer contains.** Inputs, ordered steps, decision points, and outputs. Each model named with its role: what it receives, what it returns, why that model rather than a smaller or larger one. The state transitions, including the failure path — a scoring run that times out, a document that trips the classifier's input limit, a vendor returning an error. Where a human sits in the loop, if anywhere, and what they see.

**Red flags.** An answer that needs a slide deck. "The AI decides" as a step. Models referred to only as "the model". A workflow that is really one call with a retry. No failure path at all, which means failures have not been imagined.

### 7. Elaborate the tech stack you used in this product and tell why did you use it.

> "Elaborate the tech stack you used in this product and tell why did you use it."

**What it's really testing.** Whether each choice has a rejected alternative attached. A stack without alternatives is a list of preferences, not a design.

**What a good answer contains.** A layer-by-layer table: layer, choice, the constraint it satisfies, the alternative that was rejected and why, and the operational cost accepted. Frontend, backend, queue, model hosting, storage, observability. The costs should be real — cold starts, per-token pricing, vendor lock-in, the on-call burden of a self-hosted model.

**Red flags.** A list of logos with no reasons. Choices justified by familiarity alone, with no acknowledgement of the trade. No alternative named for any layer. No versions. No mention of cost or latency, which are the two constraints that decide whether the stack survives contact with users.

---

## How to run the interrogation

1. The author presents the feature **without** a demo. A demo answers question 4 for the happy path and hides the other six.
2. Ask the seven in order. Stop at the first one that cannot be answered — a broken mechanism invalidates the rest.
3. Write each answer down verbatim. Vagueness is easier to spot on the page than in the room.
4. Convert every unanswered question into an artefact with an owner and a date: a calibration report, a threshold policy, an eval set, a model card.
5. Re-run the review against the artefacts, not against the prose.

Two rules make it stick. First, the author must answer in **three sentences or fewer** for question 6; more means the system is being discovered while it is being described. Second, nobody may answer a question with a reference to a different question. "It's in the design doc" is a location, not an answer.

### Grading the answers

Every answer falls into one of three tiers. Only the first counts as designed.

| Tier | Sounds like | Verdict |
| --- | --- | --- |
| **Evidence** | A named mechanism, a test set, a number with a denominator, a rejected alternative | Designed. Review the artefact. |
| **Description** | A correct restatement of the feature's purpose, in different words | Described. Assign the artefact. |
| **Aspiration** | "It will use AI to", "advanced", "best-in-class", "we'll evaluate later" | Not designed. Stop the review. |

The middle tier is the dangerous one, because it is fluent and often longer than the evidence tier. A description that cannot be converted into an artefact is an aspiration in a better suit.

---

## The generalisable pattern

Any spec that produces a score, a ranking, a prediction, or a grade must answer these seven, whatever the domain. The AI detector is just a convenient instance.

| Question | Why it matters |
| --- | --- |
| **What is the mechanism?** | "AI-powered" is not an answer, and a spec without a mechanism cannot be reviewed or tested. |
| **What is the unit and range?** | A score of 87 out of what, meaning what — probability, percentile, and index are different products. |
| **How accurate is it, and measured how?** | Precision, recall, and the test set decide whether the number is a claim or an anecdote. |
| **What is the false-positive cost?** | Someone is accused when the model is wrong; the severity of that harm sets the required threshold. |
| **What does the user do with the number?** | If no decision changes, the feature is decoration with a compute bill. |
| **What happens at the boundary?** | 0.49 and 0.51 must not be different products; the threshold needs a stated policy and a review path. |
| **Can it be gamed?** | If yes, say so before launch — every public detector is eventually optimised against. |

Two rows carry more weight than the rest. **False-positive cost** is where the ethics and the engineering meet: a detector used to grade students and a detector used to flag spam tolerate very different error rates, and the spec should say which one it is. **What the user does with the number** is where the feature earns its place: a score that changes no decision is a dashboard, and dashboards are the most expensive way to avoid deciding anything.

> A confidence score that never crosses a threshold is a mood, not a measurement.

---

## The habit

The seven questions are one question asked from seven angles: **show me the mechanism**. Everything else — precision, stack, framework, workflow — is a way of checking whether the mechanism is real, measured, and connected to the input.

Ask them in order, and stop at the first one that fails. If the author cannot say how the score is produced, the remaining six will produce confident prose about nothing. And if the answer to "how does it work" is "it uses AI", the spec is not written yet — it is a wish with a product name.

The same discipline applies beyond detectors: any feature that outputs a judgement rather than a fact deserves this review before it gets a schedule.

---

*Structured from the original spec-review question set.*
