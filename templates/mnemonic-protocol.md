<!-- BEGIN MNEMONIC PROTOCOL -->
## Memory Operations

You have PERSISTENT MEMORY across sessions.

BEFORE starting any task:
```bash
if [ -d ~/.claude/mnemonic ]; then
    rg -i "{relevant_keywords}" ~/.claude/mnemonic/ --glob "*.memory.md" -l | head -5
fi
```
If results exist, READ the most relevant and apply that context.

AFTER completing work, if you discovered:
- A decision → capture to _semantic/decisions
- A pattern → capture to _procedural/patterns
- A learning → capture to _semantic/knowledge
- A blocker → capture to _episodic/blockers
<!-- END MNEMONIC PROTOCOL -->
