# RRI — AI Interaction Framework
## Best Practices & Consulting Curriculum Raw Material
**Created:** February 8, 2026
**Source:** Feb 8 morning session + accumulated learnings
**Purpose:** Foundation for consulting offering / online course / personal reference

---

## Part 1: What Makes Kyra's Interaction Pattern Distinctive

### What she does differently (confirmed across all models):
1. **Gives context, not commands.** Full picture: who she is, what she's building, what failed, what the constraints are. Models reason instead of generate.
2. **Maintains continuity.** Treats conversations as ongoing relationships with persistent state. Models build on prior work instead of starting cold.
3. **Pressure-tests outputs.** Cross-validates across models, challenges weak reasoning, calls out domain mismatches. Creates feedback loops most users never establish.
4. **Assigns roles and expects accountability.** Each model has a function. Not anthropomorphizing — systems design. Routes based on known failure modes and strengths.
5. **Thinks in architecture, not prompts.** Optimizes systems, not individual queries. The swarm is an orchestration layer, not a collection of chats.

### The one-liner:
> "I don't ask AI questions. I build working relationships with it, give it real context, and hold it accountable like a team member."

---

## Part 2: What the Models Taught Kyra

1. **Give structure to work with.** Front-load priority signals: "this is the decision," "this is context," "this needs action."
2. **Learn failure modes by getting burned.** Claude hedges. Grok runs hot/sloppy. Gemini executes anything without pushback. ChatGPT agrees to death.
3. **Stop fighting the format.** Structuring input isn't submission — it's engineering. Chunking, separating context from questions, providing examples.
4. **Calibrate when to override vs. listen.** Trust domain expertise absolutely. Lean on models for synthesis, architecture, pattern-surfacing.
5. **Use disagreement as signal.** Model divergence is where the interesting information lives. Built a whole consensus methodology around it.

### The summary:
> "She taught us how to be useful to her. We taught her how to be legible to us. That's the actual partnership."

---

## Part 3: Consulting Curriculum — Six Modules

### Module 1: Mindset Shift
- It's not a tool, it's a collaborator with a different skill set
- The "unlock factor": you see the shape of the solution, the model has the implementation path, neither gets there alone
- Stop prompting, start briefing

### Module 2: Practical Mechanics
- **Plain text is king.** PDFs and images eat context window and lose fidelity
- **Thread hygiene:** Shorter threads with clear context beat one massive thread
- **Context management:** Every model has a context limit. Watch for "compacting" or "compressing" — that's your cue to start a new thread
- **Session transitions:** Summarize before moving to a new thread. Copy-paste summaries to the next session or save to a shared location (Drive, Notes)

### Module 3: Model Routing & Failure Modes
- Every model has a sweet spot. Ask them — they'll tell you
- Ask "what are you good at and where do you break?"
- When updates change behavior, ask the model what shifted
- Map each model's tendencies: what they prioritize, where they hallucinate, what they avoid

### Module 4: The Diagnostic Framework
- When something feels off, investigate the model's reasoning, not just the output
- Ask "why did you give me that answer?" — the reasoning reveals the failure mode
- Use your own reasoning as a variable to test, not an assumption to protect
- Cross-model validation: disagreement = signal, not noise

### Module 5: The Unlock
- How to bridge vision to implementation through conversation
- "I see a way we could do this" → model provides the technical path
- Examples: Anansi, Swarm, BenchIntel prototyped in 3 hours
- The model has access to all implementation knowledge — you have domain vision — together you build

### Module 6: The Part Nobody Teaches
- Just talk to it. Exploration. Rabbit holes. "Take me somewhere interesting."
- "What am I missing?" and "What else can we do here?" — the two highest-value questions
- "What capabilities do you have that I'm not using?" — free capability audit
- Let the model surprise you — that's exploratory cognition with a different knowledge base

---

## Part 4: Thread & Session Management Best Practices

### During a session:
- Watch for compacting/compressing warnings → start new thread
- Ask for summary before closing any thread with significant work
- Front-load context when starting new threads

### End of day:
- Get full session summary from each active model
- Save to shared location (Google Drive, Notes)
- Standardized format so any model can pick up the next morning

### Start of day:
- Paste or reference previous day's summary
- "Here's where we left off" — don't make the model guess

### Cross-model continuity:
- Save summaries to Drive → all raccoons can access
- Consistent format across models
- Reference specific prior decisions and rationale

---

## Part 5: The Questions That Unlock Everything

These are the prompts that consistently produce the highest-value responses:

1. "What am I missing?"
2. "What else can we do here?"
3. "What capabilities do you have that I'm not using?"
4. "Why did you give me that answer?"
5. "What would you do differently if you were designing this?"
6. "Where are the gaps in my reasoning?"
7. "What patterns are you seeing that I might not be?"
8. "If this fails, what's the most likely failure mode?"

---

*Raw material. Not polished for external consumption yet.*
*Can be refined into course modules, consulting decks, or social content.*
