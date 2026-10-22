# AI Support Bot Program: Plan and Optimization Playbook

This example program plan shows how a documentation program owner scopes, configures, measures, and
improves an AI support bot that answers engineers' questions in an engineering support channel from
the connected documentation set. Use it to see how program decisions, content standards, and
measurement fit together, and who owns each part.

> **Published portfolio example.** The service, teams, roles, channel, question examples, targets,
> and timelines are fictional and illustrate the method. In this example a platform team supplies
> and hosts the bot framework and retrieval integration. The program owner configures it, owns the
> content it draws on, and runs the measurement and improvement loop.

**On this page**

- [1. Program overview](#1-program-overview)
- [2. Plan](#2-plan)
- [3. Source scoping and content readiness](#3-source-scoping-and-content-readiness)
- [4. Configuration decision log](#4-configuration-decision-log)
- [5. Optimization playbook](#5-optimization-playbook)
- [6. KPIs and reporting](#6-kpis-and-reporting)
- [7. Release checklist and rollout communications](#7-release-checklist-and-rollout-communications)
- [8. How this plan connects to the documentation
  set](#8-how-this-plan-connects-to-the-documentation-set)

## 1. Program overview

### Problem

Engineers who use the Secrets Vault service ask the same questions in `#support-channel`: which
secrets belong in Vault (versus hardware security module or other secret stores), how to request access, and how to
rotate a credential. Support engineers answer them one at a time, or provide a link to the correct
place in the documentation. The documentation already covers most of these questions, but some people don't know where to look and want a fast answer.

### Goals

- Resolve routine questions in the channel from verified documentation, with citations, only flagging a support engineer for help if the question can't be answered by the documentation or needs an engineer to act.
- Give support engineers, including newer engineers covering evening and weekend shifts, a reliable
  first answer to work from.
- Turn every failed answer into a content fix, a configuration fix, or a documented gap, so the
  knowledge base improves with use.

### Success measures

| Measure | Why it matters |
| --- | --- |
| Share of in-scope questions resolved without escalation | The direct measure of support toil removed. |
| Fallback rate (no verified answer found) | A rising rate points to documentation gaps before anyone complains. |
| Failed-question recurrence | Shows whether fixes hold and carry over to related questions. |
| Source freshness coverage | The bot only answers from reviewed, published content. |

Definitions and targets are in [KPIs and reporting](#6-kpis-and-reporting).

### Scope

| In scope | Out of scope |
| --- | --- |
| Questions about the Secrets Vault service and Standard 1234 asked in `#support-channel`. | Building or hosting the bot framework and retrieval pipeline (Platform Tools team). |
| The connected documentation set: the standard, the handling guidelines, the service procedure, and the ownership records in the health dashboard. | Changes to the Secrets Vault service itself. |
| Configuration of the bot's sources, answer behavior, grounding rules, fallback, resolution check, and escalation. | Requests that require an engineer to act, such as granting access or rotating a credential on someone's behalf. The bot routes these; it does not perform them. |
| The optimization loop: feedback, triage, root cause analysis, fixes, and reruns. | External, customer-facing support. |

### Roles and responsibilities

Roles are example roles. In an operational plan, each resolves to a named person or a maintained
team contact.

| Role | Responsibilities |
| --- | --- |
| Program owner (Security Documentation Lead) | Owns this plan, content readiness, configuration decisions, triage, KPIs, and reporting. |
| Service owner (Secrets Platform Lead) | Confirms technical accuracy, approves changes to the service procedure, and provides the support rotation. |
| Control owner (Security Governance Lead) | Approves changes to Standard 1234 and confirms that grounding rules match control requirements. |
| Platform Tools team | Provides and hosts the bot framework and retrieval integration, exposes configuration options, and fixes retrieval defects. |
| Knowledge Retrieval team | Advises on chunking, indexing, and evaluation, and reviews retrieval root cause analyses. |
| Support engineers | Answer escalations, flag wrong or incomplete answers in the thread, and review fixes for their question types. |
| Engineers asking questions | Reply in the thread with what they still need, or mark the question resolved. |

| Decision or activity | Program owner | Service owner | Control owner | Platform Tools | Support engineers |
| --- | --- | --- | --- | --- | --- |
| Allowed sources | A | C | C | I | I |
| Content readiness fixes | A | R | C | I | I |
| Grounding and fallback rules | R | C | A | C | I |
| Escalation routing | R | A | I | C | C |
| Weekly triage and fixes | A | C | I | R (retrieval defects) | C |
| Pilot go/no-go | R | A | C | C | C |

R: responsible, A: accountable, C: consulted, I: informed.

## 2. Plan

### Phases

| Phase | Work | Exit criteria | Depends on |
| --- | --- | --- | --- |
| 0. Baseline | Collect the previous 60 to 90 days of channel questions. Categorize them by question type, measure volume, time to first answer, and escalations, and agree on targets. | Baseline report accepted by the service owner and support leads. | Channel history access. |
| 1. Source scoping and content readiness | Define allowed sources and exclusions. Audit the connected set against the readiness checklist. Fix structure and metadata. Confirm owners and review dates in the health dashboard. | Every in-scope page passes the checklist and shows published status in the dashboard. | Document owners' time. |
| 2. Configuration and dry run | Configure sources, answer behavior, grounding, fallback, resolution check, and escalation. Run the baseline question set in a private channel. | Pass rate meets the agreed threshold on the test set. Fallback and escalation verified. Permissions verified. | Platform Tools configuration options. |
| 3. Pilot | Enable the bot in `#support-channel` for Secrets Vault questions only. Announce it. Monitor daily and triage every miss. | Four consecutive weeks meeting pilot targets. Retro held and actions assigned. | Phase 2 exit. |
| 4. Operate and expand | Weekly triage, monthly KPI report, quarterly OKR review. Expand to adjacent services only after their content passes the readiness audit. | Ongoing. | Expansion gate in Section 3. |

### Milestones

| Milestone | Target | Owner |
| --- | --- | --- |
| Baseline report and targets agreed | Week 2 | Program owner |
| Readiness audit complete, fixes assigned | Week 4 | Program owner |
| Content fixes published, dashboard current | Week 7 | Document owners |
| Dry run passes threshold | Week 9 | Program owner, Platform Tools |
| Pilot launch | Week 10 | Program owner, service owner |
| Pilot retro | Week 14 | Program owner |
| First monthly KPI report | Week 15 | Program owner |

### Dependencies

- Platform Tools delivers configuration options for sources, grounding, fallback, and escalation
  routing before Phase 2.
- Document owners complete readiness fixes before indexing.
- The health dashboard's review status is current, because the bot indexes only published documents.

### Risks

| Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- |
| Confident wrong answers from stale pages | Medium | High | Index only pages with published status and a current review date. Grounding rule and fallback (CFG-04, CFG-05). |
| Retrieved passage omits a restriction, such as the storage eligibility note in Standard 1234 | High | High | Self-contained sections keep the restriction with the step. Test the specific questions in the dry run and the regression set. |
| Support engineers stop checking the bot's answers | Medium | Medium | Resolution check routes unresolved threads to a support engineer with context. Sample audits in weekly triage. |
| Engineers bypass the bot and message support engineers directly | Medium | Low | Channel pin and announcement. Support engineers redirect to the channel. Measure direct messages during the pilot. |
| Scope expands to other services before their content is ready | Medium | High | Expansion gate: readiness audit required before any new source is added. |
| The bot surfaces content to someone who cannot open the source | Low | High | Retrieval honors source permissions (CFG-09). Verify with a test user in the dry run. |

## 3. Source scoping and content readiness

### Allowed sources

Only pages with `status: published` and a current review date in the [Security Controls Health
Dashboard](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-controls-health-dashboard.md)
are indexed. The dashboard's review status is the gate.

| Source | Included | Excluded | Reason |
| --- | --- | --- | --- |
| [Standard 1234: Secret Storage and Handling](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-standard-1234.md) | Whole page | None | Defines control requirements and storage eligibility. |
| [Secure Data Handling](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/secure-data-handling.md) | Whole page | None | Explains the handling principles behind the standard. |
| [Secret and Key Management with Secrets Vault](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/vault-secrets-management.md) | Published sections | Sections marked as not yet published | The procedure engineers follow. Unfinished sections would produce incomplete answers. |
| [Security Controls Health Dashboard](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-controls-health-dashboard.md) | Document owners table | Revision register | Lets the bot answer "who approves a change." Revision records are workflow state, not guidance. |
| Channel history | None | All | Unverified and often outdated. |
| Design documents and tickets | None | All | Not approved guidance. |

### Content readiness checklist

Apply the checklist to every in-scope page before indexing and after any revision. The principles
are explained in [Designing content people (and AI) can find, understand, and
trust](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/designing-content-for-people-and-ai.md).

- Headings state the task or question in the reader's words.
- Each section stands on its own. The service, conditions, prerequisites, and restrictions travel
  with the step.
- A descriptive summary of one or two sentences opens each page and each major section.
- Metadata is complete: `id`, `title`, `description`, `tags`, `status`, `owner`, `last_reviewed`,
  `applies_to`, and `supports_control`.
- Link text names the destination, such as "secret storage eligibility requirements."
- Restrictions appear before the action they govern.
- Each fact has one source. Other pages reference it rather than restating it.
- Terminology matches the service interface and the standard.

### Readiness audit record

| Document | Checklist result | Fix needed | Owner | Status |
| --- | --- | --- | --- | --- |
| Standard 1234 | Pass | None | Security Governance Lead | Ready |
| Secure Data Handling | Pass with notes | Add descriptive summaries to two sections. | Security Documentation Lead | In progress |
| Secrets Vault procedure | Pass with notes | Keep the storage restriction visible in the onboarding section. Mark unfinished sections so they are excluded from indexing. | Secrets Platform Lead | In progress |
| Health dashboard (owners table) | Pass | None | Security Governance Lead | Ready |

## 4. Configuration decision log

The settings below are options the platform team exposes. The program owner selects or defines each
one and records the reason, so later changes can be traced to a decision.

| ID | Decision | Setting | Reason | Owner |
| --- | --- | --- | --- | --- |
| CFG-01 | Data sources | The allowed sources in Section 3, by path. Nothing else. | Answers come only from reviewed, published content. | Program owner |
| CFG-02 | Trigger and channel | Respond to new top-level questions in `#support-channel`. Respond in a thread only when mentioned. | Keeps the bot out of conversations that are already being handled. | Program owner |
| CFG-03 | Answer format | Direct answer first, then numbered steps where needed, then citations linking to the exact section. | Engineers scan. The citation lets them verify and read further. | Program owner |
| CFG-04 | Grounding rule | Answer only from retrieved passages in allowed sources. No general knowledge. | Prevents plausible answers that conflict with the standard. | Control owner approves |
| CFG-05 | Fallback | When no verified passage supports an answer, say so and offer escalation. Never guess. | A visible "no verified answer" is a documentation signal. A guess is a risk. | Program owner |
| CFG-06 | Resolution check | After answering, ask whether the question is resolved. The asker's reply drives routing. | Produces the resolution data behind the KPIs. | Program owner |
| CFG-07 | Escalation | Unresolved questions and action requests route to the support engineer on rotation with the thread, the bot's answer, and the sources it used. | The support engineer starts with context instead of from zero. | Service owner approves |
| CFG-08 | Tone | Direct, concise technical tone. No filler. | Matches the documentation style guide. | Program owner |
| CFG-09 | Permissions | Retrieval honors source permissions. The bot does not surface content to a user who cannot open the source. | Required for any security documentation. | Platform Tools implements |
| CFG-10 | Logging | Keep the question, retrieved passages, answer, and resolution outcome for triage. Retention per policy. | Root cause analysis needs the retrieved passages, not just the answer. | Program owner |

## 5. Optimization playbook

### Feedback loop

1. The asker replies in the thread with what they still need, or marks the question resolved.
2. Support engineers flag wrong or incomplete answers in the thread with an agreed reaction or
   keyword.
3. Each week, the program owner reviews every unresolved or flagged thread and logs it in the
   failed-question list.

### Triage categories

Every failed question gets one category. The category decides who fixes it.

| Category | Symptoms | Typical fix | Fixed by |
| --- | --- | --- | --- |
| Retrieval issue | The right content exists and is indexed, but the bot retrieved the wrong passage, or a fragment without its restriction. | Adjust section boundaries, headings, summaries, or metadata so the passage is self-contained and findable. Report a retrieval defect when the content is sound. | Program owner, document owner; Platform Tools for defects |
| Prompt or formatting issue | The right passage was retrieved, but the answer was wrong, unclear, too long, or missing its citation. | Adjust answer behavior and instructions (CFG-03, CFG-04, CFG-08). | Program owner |
| Documentation gap | No published source answers the question. | Log the gap with an owner and due date. The document owner writes or updates the content. The fallback applies until it is published. | Document owner |

### Root cause analysis template

| Field | Entry |
| --- | --- |
| Question as asked, and date | |
| Expected answer and source section | |
| What the bot retrieved and answered | |
| Category | Retrieval, prompt or formatting, or documentation gap |
| Root cause | |
| Fix and owner | |
| Verification | Rerun result and date |
| Carry-over | Other question types the fix should also resolve |

### Failed-question list and rerun cadence

- Every failed question goes on the list with its category, fix, and owner.
- After a fix ships, rerun the question and every other question in the same category.
- Close an entry after it passes two consecutive weekly reruns.
- Keep closed entries. They become the regression set for every later change.

### Change propagation

- When a source document changes, the revision register in the health dashboard records it. Rerun
  the questions that cite that document before and after re-indexing.
- When Standard 1234 changes, review the Secrets Vault procedure's restrictions and the grounding
  rule together, then rerun the eligibility questions.

## 6. KPIs and reporting

### KPIs

| KPI | Definition | Source | Cadence |
| --- | --- | --- | --- |
| Resolved without escalation | Share of in-scope questions marked resolved after the bot's answer with no support engineer involvement. | Resolution check log | Weekly |
| Escalation rate | Share of in-scope questions routed to a support engineer. | Escalation log | Weekly |
| Time to first answer | Median minutes from question to the bot's answer, and to the support engineer's answer for escalations. | Channel timestamps | Weekly |
| Fallback rate | Share of questions where the bot found no verified answer. | Bot log | Weekly |
| Failed-question recurrence | Count of previously closed question types that fail again. | Failed-question list | Weekly |
| Source freshness coverage | Share of indexed pages with a current review date. | Health dashboard | Monthly |
| Documentation gaps closed | Gaps opened and closed, and median days to close. | Failed-question list | Monthly |
| Support toil | Count of escalations, including after-hours escalations, plus a short survey of support engineers each rotation. | Escalation log, survey | Monthly |

### Reporting

- Weekly: triage summary posted in the channel for support leads. New failures by category, fixes
  shipped, reruns passed.
- Monthly: KPI report to the service owner and security leadership, with open documentation gaps by
  owner.
- Quarterly: OKR review with the service owner and control owner.

### Example quarterly OKR

Targets in brackets are illustrative. Real targets come from the Phase 0 baseline.

**Objective:** Engineers get a reliable, cited answer to routine Secrets Vault questions without
waiting on a support engineer.

- KR1: Resolve [30%] of in-scope questions without escalation by quarter end, from a baseline of
  [0%].
- KR2: Median time to first answer under [2 minutes] for in-scope questions.
- KR3: [100%] of indexed pages carry a current review date.
- KR4: Close [80%] of logged documentation gaps within [14 days].

## 7. Release checklist and rollout communications

### Pre-launch checklist

| Item | Owner | Done |
| --- | --- | --- |
| Allowed sources approved by the service owner and control owner. | Program owner | |
| Every in-scope page passes the readiness checklist and shows published status in the dashboard. | Document owners | |
| Dry run: the baseline question set passes the agreed threshold. | Program owner | |
| Fallback triggers on out-of-scope questions. | Program owner | |
| Escalation delivers the thread, answer, and sources to the support engineer on rotation. | Service owner | |
| Permissions verified with a test user who cannot open a restricted source. | Platform Tools | |
| Logging and retention confirmed. | Platform Tools | |
| Support rotation briefed and the flagging method agreed. | Service owner | |
| Announcement and channel pin drafted and approved. | Program owner | |
| Rollback plan: how to disable the bot and who can. | Platform Tools | |

### Rollout communications

- Announcement in `#support-channel` and the engineering updates channel: what the bot does, what it
  does not do, how escalation works, and how to give feedback.
- Channel pin: a one-paragraph guide with a link to this plan.
- Office hours during the first two weeks of the pilot.
- Week-4 update with the first KPIs and what changed as a result.

### After launch

- Daily review of every thread for the first two weeks, then weekly triage.
- Retro at the end of the pilot: what failed and why, what to carry into expansion, and what the
  support engineers want changed.

## 8. How this plan connects to the documentation set

- [Standard
  1234](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-standard-1234.md)
  and control SEC-1234 define what the bot may say about storage eligibility. The grounding rule
  enforces it.
- [Secure Data
  Handling](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/secure-data-handling.md)
  and the [Secrets Vault
  procedure](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/vault-secrets-management.md)
  are the answer sources. The readiness checklist applies to them.
- The [Security Controls Health
  Dashboard](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/security-controls-health-dashboard.md)
  supplies ownership and review status. The bot indexes only published, reviewed documents, and the
  revision register triggers reruns.
- [Designing content people (and AI) can find, understand, and
  trust](https://github.com/RenaVerbena/RenaVerbena/blob/main/writing-samples/designing-content-for-people-and-ai.md)
  explains the content design principles behind the readiness checklist.
  
