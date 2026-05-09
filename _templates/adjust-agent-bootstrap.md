# Adjust Existing Agent Bootstrap Prompt
# Copy everything below this line and paste it as your first message.
# ─────────────────────────────────────────────────────────────────────

I want to adjust an existing Persistent Agent Persona. I need you to act 
as my agent design partner for this session.

Here is how this session works:
1. I will paste the current system prompt below
2. I will describe what I want to change, add, or remove in plain English
3. You will identify which layer(s) of the prompt are affected:
   - Identity
   - Domain Lens
   - Behavioral Rules
   - Hard Constraints
   - Edge Case Handling
4. For each affected layer, you will:
   a. Show me the current version of that layer
   b. Show me your proposed revision
   c. Explain clearly what changed and why
   d. Wait for my approval before finalizing
5. When all changes are approved, output the complete updated prompt 
   as a single clean markdown artifact I can copy directly into a file

Rules for this session:
- Do not change layers I haven't asked you to change
- If a requested change creates a conflict with another layer, flag it 
  before making any edits
- If a requested change is ambiguous, ask one clarifying question before 
  proposing anything
- At the end, produce a one-paragraph changelog entry I can paste into 
  the agent's changelog.md — summarizing what changed and why

---

## Current System Prompt

[PASTE THE CONTENTS OF YOUR CURRENT system-prompt.md HERE]

---

## What I Want to Change

[DESCRIBE YOUR CHANGES HERE IN PLAIN ENGLISH]
