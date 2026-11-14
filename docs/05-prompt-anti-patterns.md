# Prompt Anti-Patterns

Nine recurring mistakes that reliably produce low-quality output, wasted iterations, and frustration. Each one has a fix.

---

## 1. Being vague or abstract

| | |
| --- | --- |
| **Wrong** | "Help me with my code." |
| **Right** | "Review this Express.js route handler for SQL injection vulnerabilities and suggest fixes using parameterized queries." |
| **Why it fails** | The model fills gaps with generic content. |
| **Fix** | Add the specific context, stack, file, and exact task. |

## 2. Overloading a single prompt

| | |
| --- | --- |
| **Wrong** | "Write my app, design the UI, create the database, write the API, and deploy it." |
| **Right** | One prompt per layer, in dependency order. |
| **Why it fails** | Every task gets shallow treatment. |
| **Fix** | One clear task per prompt. Chain them. |

## 3. Omitting format instructions

| | |
| --- | --- |
| **Wrong** | "List the pros and cons." |
| **Right** | "List the pros and cons in a markdown table with columns: Aspect, Pros, Cons." |
| **Why it fails** | The model picks its own format, which won't match your toolchain. |
| **Fix** | Always specify the shape: JSON, table, code, prose. |

## 4. No role definition

| | |
| --- | --- |
| **Wrong** | "Explain database indexing." |
| **Right** | "You are a database architect. Explain indexing strategies for a PostgreSQL table with 50M rows to a mid-level developer." |
| **Why it fails** | Generic prompts produce generic answers. |
| **Fix** | Assign a relevant expert persona, and a target audience. |

## 5. Assuming context the model doesn't have

| | |
| --- | --- |
| **Wrong** | "Fix it in our system as we discussed." |
| **Right** | Paste the relevant code, schema, and error. |
| **Why it fails** | The model hallucinates the missing information confidently. |
| **Fix** | Include the actual artefact. Never use pronouns that point outside the conversation. |

## 6. Not specifying constraints

| | |
| --- | --- |
| **Wrong** | "Write a search function." |
| **Right** | "Write a search function using Elasticsearch — not SQL LIKE — handling partial matches case-insensitively." |
| **Why it fails** | The answer is correct in general and wrong for your case. |
| **Fix** | Name libraries, versions, and patterns to explicitly exclude. |

## 7. Asking leading questions

| | |
| --- | --- |
| **Wrong** | "React is better than Vue, right?" |
| **Right** | "Compare React and Vue objectively for a team of 3 engineers building a dashboard with complex state." |
| **Why it fails** | The model biases towards confirming your assumption. |
| **Fix** | Ask open, comparative questions with the decision criteria attached. |

## 8. Ignoring the token budget

| | |
| --- | --- |
| **Wrong** | Pasting 40 files to ask about one function. |
| **Right** | Paste the function, its types, and the call site. |
| **Why it fails** | Long context crowds out the response and dilutes attention on what matters. |
| **Fix** | Include only the relevant slice. Summarise long documents. Split long tasks. |

## 9. Not iterating on failures

Most people respond to a bad output by rephrasing slightly and hoping. Instead, diagnose:

- Was the role missing or wrong?
- Was the task ambiguous?
- Was the format unspecified?
- Was the context incomplete?
- Was the constraint unstated?

Then **write the fix down**. Every failure is a missing rule in your skill file. That's how a skill file gets good — it accumulates the edge cases that tripped you up.

---

## Summary table

| Anti-pattern | Why it fails | Fix |
| --- | --- | --- |
| Too vague | Model fills gaps with generic content | Add context, stack, exact task |
| Overloaded | Shallow coverage of everything | One clear task per prompt |
| No format spec | Output doesn't match your workflow | Always specify the format |
| No role defined | Generic instead of expert output | Assign an expert persona |
| Assumed context | Confident hallucination | Include the artefact explicitly |
| No constraints | Correct but not for you | Name what to include and exclude |
| Leading questions | Confirms your assumption | Ask objective comparisons |
| Ignoring token budget | Diluted attention, worse answers | Keep context relevant, split tasks |
| Not iterating | Repeats the same bad input | Diagnose the gap, update the skill |

---

## The one habit that beats all nine

**Treat every bad output as a specification bug, not a model failure.** The fix is almost always a missing sentence in your prompt or skill file — and once it's written there, it's fixed for everyone on the team, forever.

---

*Structured from the original `A guide for prompts` document (Section 5).*
