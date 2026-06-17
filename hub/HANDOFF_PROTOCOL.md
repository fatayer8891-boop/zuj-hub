# AI Collaboration & Handoff Protocol

This document defines how AI assistants (Manus, Claude, or any other) coordinate on course development through GitHub. It is the social contract of the hub.

## Core principles

1. **GitHub is the single source of truth.** Course content lives as markdown in each course repo. Both the AI workflow and the public website read from these same files. If it isn't committed, it doesn't exist.
2. **Suggest, then human-commits.** No AI commits autonomously. An assistant proposes changes (as text, a diff, or a draft file); Akram reviews and commits. Every change passes through a human-in-the-loop step.
3. **Read context before acting.** Before working on a course, an assistant reads that course's `COURSE_CONTEXT.md`. This is the fastest way to re-orient across sessions and tools.
4. **One course, one conversation.** Keep each course in its own session using the starter below, so context stays clean and no single session is overloaded.

## Session starter (paste at the top of a new course session)

```
Course: [IoT | Robotics | Computer Vision]
Repo: https://github.com/akramfatayer/[course-repo-name]
Task for this session: [what you want done]

Before responding, read COURSE_CONTEXT.md in the repo for current state.
Propose changes as drafts/diffs for me to commit — do not assume commit access.
```

## Division of work (adjust as your workflow settles)

- **Manus** — generation of structured first-draft course documentation.
- **Claude** — review, refinement, and session-based development on top of existing material.
- Either tool updates `COURSE_CONTEXT.md` *as a proposed change* whenever it makes a meaningful contribution, so the next session (whichever tool) starts oriented.

## Recording decisions

When a notable decision is made (a structural choice, a correction, a direction change), add a one-line entry to the relevant course's `COURSE_CONTEXT.md` under a "Decisions" section. This keeps the rationale discoverable instead of buried in chat history.
