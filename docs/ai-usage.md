# AI Usage Disclosure

AI tools used: ChatGPT/Codex.

I used AI as a structured development assistant, not as a replacement for technical judgement. My main use was to accelerate organisation, challenge my own coverage of the brief, tighten wording, and check whether the response addressed the engineering risks that matter most: authorization, deployment safety, review discipline and production incident handling.

Relevant prompts included:

```text
Read the Third Wave BBQ exercise requirements. Create a document-based repository structure and prepare concise written answers for each requested section. Keep the response focused on reasoning, AI-assisted development workflow, review process, deployment risk and production incident handling rather than building the feature.
```

```text
Review the written response for submission readiness and check whether each requested section is answered directly. Identify wording that should be made more specific, more concise, or more professional.
```

```text
Help combine my previous AInsight full-stack AI aggregation project with my lead intelligence automation project into one truthful previous-project example. Do not invent technical details. Emphasise what I personally built, how AI was used, what AI initially got wrong, and how I tested and corrected it.
```

I rejected or changed several AI suggestions. First, I did not build a sample Next.js/NestJS/PostgreSQL application, because the brief explicitly says the exercise is not asking for the feature to be built. Creating unnecessary code would have looked busy but would not have answered the assessment as well. Second, I simplified the repository from a code-style folder structure into a document-based structure because that better matches the deliverable. Third, I rewrote generic AI wording into more concrete engineering language, especially around backend authorization, venue scoping, migration safety, rollback and incident response.

The most useful part of using AI was speed of iteration. I used it to produce an initial structure quickly, then reviewed and narrowed the result myself. I checked that the answer stayed aligned with the scenario, refined weak wording, split the response into focused documents, and made sure the most serious issue in the code review section was clearly identified as broken authorization rather than just a query bug.

My final decisions remained human-controlled: what scope to submit, what not to build, which risks to prioritise, how to describe the previous project truthfully, and which AI-generated suggestions to discard.
