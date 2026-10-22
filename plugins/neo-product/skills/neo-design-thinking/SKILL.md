---
name: neo-design-thinking
description:
  'Facilitate human-centered design thinking activities: stakeholder mapping, empathy mapping, persona definitions,
  problem framing, journey mapping, ideation workshops, assumption testing, and service blueprinting. Use when: running
  design workshops, defining users, mapping experiences, framing problems, brainstorming solutions, validating
  assumptions, or blueprinting services. Produces design artifacts in docs/design/.'
argument-hint: "[activity] — e.g. persona, journey-map, blueprint; omit for the menu, or describe the problem space"
---

# Design Thinking

## When to Use

- Starting a new project or entering an unfamiliar domain
- Defining who the users are and what they need
- Mapping user experiences or service operations
- Framing problems before jumping to solutions
- Generating and evaluating solution concepts
- Validating assumptions before building

## Recommended Workflow

Design thinking activities build on each other. Follow this sequence when starting from scratch, or jump to the relevant
activity if prior work exists:

```
1. Stakeholder Mapping  →  Who is involved?
2. Empathy Mapping      →  What do they think, feel, and need?
3. Persona Definitions  →  Who are the key user archetypes?
4. Problem Framing      →  What problem are we solving?
5. Journey Mapping      →  What is the current/future experience?
6. Ideation Workshop    →  What solutions could work?
7. Assumption Testing   →  Which beliefs need validation?
8. Service Blueprinting →  How does the service operate end-to-end?
```

Not every project needs all eight. Use the routing table below to pick the right activity.

## Commands

Every activity has a deterministic address. `/neo-design <activity>` (the plugin's short command) and
`/neo-design-thinking <activity>` (this skill invoked directly) behave identically. The first word of the arguments is
matched case-insensitively against the sub-command and alias columns — exact matches only.

| Sub-command            | Aliases                              | Activity             | Reference                                                       |
| ---------------------- | ------------------------------------ | -------------------- | --------------------------------------------------------------- |
| `stakeholder-mapping`  | `stakeholders`, `stakeholder-map`    | Stakeholder Mapping  | [stakeholder-mapping.md](./references/stakeholder-mapping.md)   |
| `empathy-mapping`      | `empathy`, `empathy-map`             | Empathy Mapping      | [empathy-mapping.md](./references/empathy-mapping.md)           |
| `persona-definitions`  | `persona`, `personas`                | Persona Definitions  | [persona-definitions.md](./references/persona-definitions.md)   |
| `problem-framing`      | `frame`, `problem-frame`             | Problem Framing      | [problem-framing.md](./references/problem-framing.md)           |
| `journey-mapping`      | `journey-map`, `journey`             | Journey Mapping      | [journey-mapping.md](./references/journey-mapping.md)           |
| `ideation-workshop`    | `ideate`, `ideation`                 | Ideation Workshop    | [ideation-workshop.md](./references/ideation-workshop.md)       |
| `assumption-testing`   | `assumptions`, `assumption-test`     | Assumption Testing   | [assumption-testing.md](./references/assumption-testing.md)     |
| `service-blueprinting` | `blueprint`, `service-blueprint`     | Service Blueprinting | [service-blueprinting.md](./references/service-blueprinting.md) |

Resolve the arguments in this order:

1. **No arguments → menu, never an activity.** Run the pre-flight in [Procedure step 2](#2-check-for-existing-artifacts),
   then render the table above for a human (sub-command, aliases, one-line purpose from the routing table) followed by
   **one** recommendation with its reason: the earliest activity in the [Recommended Workflow](#recommended-workflow)
   whose `docs/design/` folder has no artifact yet; if all eight have artifacts, the activity whose most recently
   changed artifact is oldest (by `git log` date, else file modification time), ties broken in workflow order. Then
   stop and wait for the user to pick. Do not start any activity.
2. **First word is a sub-command or alias → direct dispatch.** This replaces [step 1](#1-identify-the-right-activity)
   only; trigger matching is skipped. Steps 2–5 still run in full — in particular the `docs/design/` pre-flight runs
   before the reference is loaded, and you tell the user in one line what it found (for example, "Found 3 personas in
   `docs/design/personas/` — extending rather than starting over"). Any words after the sub-command are context for the
   activity (the subject, the scenario), not further sub-commands.
3. **A sub-command-shaped near-miss → menu with the nearest match.** This applies only when the arguments are a
   single word, or their first word is hyphenated like a sub-command; otherwise go to rule 4. Compare that word,
   case-insensitively, with every sub-command and alias. It is a near-miss when it is within two single-character edits
   of one of them, differs from one only by a trailing `s`, or is a prefix of one at least four characters long — for
   example `jouney-map`, `blueprints`, `stakeholder`, `assump`. The nearest match is the name with the fewest edits,
   reported as its canonical sub-command; ties go to the earlier row of the table. Do not guess and do not run
   anything: show the menu, call out the nearest match ("Did you mean `journey-mapping` (`journey-map`)?"), and wait.
4. **Anything else is a prose request → natural-language routing, unchanged.** Arguments that are neither a sub-command
   nor a near-miss — "map the onboarding experience for new admins", or a single word such as `brainstorming` — go
   through the [Activity Routing](#activity-routing) trigger table and [step 1](#1-identify-the-right-activity) exactly as
   before, including its Stakeholder Mapping default.

The sub-commands and aliases are a public surface. Adding an alias or an activity is safe; renaming or removing one
breaks users — keep the old name as an alias instead. The `/neo-design` command, this menu convention, and these
resolution rules are the shared mechanism the other Neo Product skills reuse (see
[`plugin-contract.md` § 4.1](https://github.com/skyarkitekten/neo/blob/main/docs/contributing/reference/plugin-contract.md#41-command-surfaces-for-skills)).

## Activity Routing

| Trigger                                                        | Activity             | Reference                                                       |
| -------------------------------------------------------------- | -------------------- | --------------------------------------------------------------- |
| New project, unknown stakeholders, competing priorities        | Stakeholder Mapping  | [stakeholder-mapping.md](./references/stakeholder-mapping.md)   |
| Need to understand what users think, feel, and experience      | Empathy Mapping      | [empathy-mapping.md](./references/empathy-mapping.md)           |
| Need shared user archetypes to ground design decisions         | Persona Definitions  | [persona-definitions.md](./references/persona-definitions.md)   |
| Transitioning from research to ideation, reframing requests    | Problem Framing      | [problem-framing.md](./references/problem-framing.md)           |
| Visualizing step-by-step user experiences, finding pain points | Journey Mapping      | [journey-mapping.md](./references/journey-mapping.md)           |
| Brainstorming solutions, expanding the option space            | Ideation Workshop    | [ideation-workshop.md](./references/ideation-workshop.md)       |
| Unvalidated beliefs, risky assumptions before building         | Assumption Testing   | [assumption-testing.md](./references/assumption-testing.md)     |
| Mapping frontstage/backstage operations, handoff gaps          | Service Blueprinting | [service-blueprinting.md](./references/service-blueprinting.md) |

## Procedure

### 1. Identify the Right Activity

If the arguments resolved to a sub-command (see [Commands](#commands)), the activity is already chosen — go to step 2.
Otherwise use the routing table above. If unsure, start with **Stakeholder Mapping**.

### 2. Check for Existing Artifacts

Search `docs/design/` for prior work:

- `docs/design/stakeholder-maps/`
- `docs/design/empathy-maps/`
- `docs/design/personas/`
- `docs/design/problem-frames/`
- `docs/design/journey-maps/`
- `docs/design/ideation/`
- `docs/design/assumption-tests/`
- `docs/design/service-blueprints/`

Build on existing artifacts rather than starting over. This step runs on every path — natural-language routing,
direct sub-command dispatch, and the no-argument menu. When the chosen activity's folder already holds artifacts, say
what you found, and default to extending or updating the matching artifact. Create a new one only when its subject is
genuinely distinct (a different persona, journey, or service), and say so before you write it — never silently add a
near-duplicate alongside the existing ones.

### 3. Read the Activity Reference

Load the appropriate reference file from the routing table and follow its procedure. If the reference file cannot be
found, notify the user that the reference is missing and proceed using general design thinking best practices for
that activity, clearly labeling the output as based on defaults rather than the project reference.

### 4. Save the Output

Each activity produces a markdown document in its designated `docs/design/` subfolder. Follow the output format
specified in the reference file.

### 5. Determine the Next Step

After completing an activity, recommend the logical next step from the workflow sequence. Cross-reference the routing
table to guide the user forward.

### Handling Multiple Activities

If the user requests multiple activities at once, execute them in the recommended workflow sequence, completing and
saving each artifact before starting the next. Reference the prior artifact as input to the subsequent activity.

## Shared Rules

- Ground all findings in evidence — label assumptions explicitly
- Produce one artifact per distinct subject — one file per persona, one file per journey (per user type and
  scenario), one file per service
- All artifacts are living documents — update them as new evidence emerges
- Approach every activity with empathy for the people affected — it is the foundation of design thinking, even when
  the routing table sends you directly to an activity other than Empathy Mapping
- Hand system structure questions to the `system-designer`
- Escalate pivot/persevere decisions to the `product-coach`
