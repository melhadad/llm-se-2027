# Exercise: critique this spec

A teammate wrote the following spec and stories for the triage feature and asked a coding agent
to implement it. Your tasks:

1. Find as many problems as you can. For each, say **what a coding agent would have to guess**
   and **what could go wrong** if it guessed differently from what the team intended.
2. Classify each problem: ambiguity, incompleteness, inconsistency, untestability,
   design-in-requirements, or missing non-functional concern.
3. Rewrite any two requirements so they are testable.

Time: 25 minutes, in teams.

---

## Spec: Smart Issue Triage

### Requirements

1. The system should use AI to understand GitHub issues and figure out what they are about.
2. The system will classify issues into the right category (bug, feature, docs, etc.).
3. Severity should be determined accurately.
4. Important issues must be escalated quickly.
5. The model should output the queue, SLA and whether to escalate as JSON.
6. If the AI isn't sure, it should do its best.
7. Bugs go to engineering. Bugs without repro steps go to the clarify queue. Critical issues
   always go to the on-call queue.
8. The system should be fast and secure.
9. Use the issue's labels and comments to improve accuracy.
10. Confidence should be between 0 and 100.

### User stories

- **US-A** As a user, I want issues to be triaged automatically so that things are better.
- **US-B** As a maintainer, I want the AI to route issues so that I get the right ones.
- **US-C** As a developer, I want to use GPT-4 with temperature 0 and a regex parser so that
  the output is reliable.
- **US-D** As a maintainer, I want the system to handle weird issues gracefully.
