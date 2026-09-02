# FLAGS

Improvement register for this repository. Documentation handover only - do not treat Open rows as bugs fixed in the docs PR.

Status: **Open** (actionable later) or **Accepted** (known limitation).

| ID | Severity | Finding | Evidence | Suggested next step | Status |
| --- | --- | --- | --- | --- | --- |
| F1 | Low | Teaching template uses sample deploy names (`bryl`, `bryllim`, related bucket defaults). | [README.md](README.md) Runtime facts / deploy examples | Learners must rename service, Artifact Registry, and bucket args for their own project. | Accepted |
| F2 | Medium | `GEMINI_API_KEY` required for chat; without it `/api/chat` only returns a setup message. | [README.md](README.md) Phase 1 / Configuration | Set key locally or via Secret Manager on deploy before demoing chat. | Open |
