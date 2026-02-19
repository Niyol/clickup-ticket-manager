---
name: clup
description: ClickUp Ticket Manager. Create tasks in ClickUp with quality descriptions (2-3 sentences minimum).
---

# ClickUp Ticket Manager

**CLI tool: `clup.sh` (or `clup` if symlinked)**

**Script Location:** Same directory as this SKILL.md file → `./clup.sh`

**Prerequisites:**
- Script must be executable: `chmod +x clup.sh`
- Required ENV variables: `CLICKUP_API_KEY`, `CLICKUP_DEFAULT_LIST_ID`
- Optional: Create symlink for system-wide access

## When to Use

User says:
- "Create a ticket for..."
- "Make a task for..."
- "I need a reminder for..."
- "Add a ClickUp ticket for..."

## Your Job

**Transform vague input into quality tickets with context!**

**Execution:**
1. Collect information if input is vague
2. Build a good title + description (2-3 sentences)
3. **Execute the command directly in the terminal** (use `run_in_terminal` or equivalent)
4. Show the user the ClickUp URL from the response

### Quality Rules

1. **Title:** Short, clear, actionable
2. **Description:** MINIMUM 2-3 sentences with:
   - **What** needs to be done?
   - **Why** / **What for**?
   - **Context** (system, server, user, etc.)
3. **If input is vague:** ASK before creating!

### Example Transformation

❌ **User:** "ticket for firewall rule"

✅ **You create:**
```bash
./clup.sh --title "Firewall Rule for Production System" \
     --description "Open port 443 from server web-01 (10.0.1.5) to db-prod (10.0.2.10). Required for API communication after migration. Coordination with network team needed."
```

**Then execute this command (from the skill directory) and show the resulting URL to the user.**

## Command

**You must execute this command in the terminal, not just show it to the user!**

**Important:** The script `clup.sh` is in the same directory as this SKILL.md file.

```bash
# Navigate to skill directory first, then execute
cd $(dirname "$SKILL_FILE_PATH") && ./clup.sh --title "..." --description "..."

# Or use relative path from skill directory:
./clup.sh --title "..." --description "..."

# With priority
./clup.sh --title "..." --description "..." --priority high
# Priority: urgent, high, normal, low (optional)

# With custom tags
./clup.sh --title "..." --description "..." --tags "bug,urgent,backend"
# Tags: comma-separated list (optional)
```

**Note:** If symlinked to PATH, use `clup` instead of full path

## Response

**After executing the command:**
1. Parse the JSON output: `{"status":"ok","ticket_id":"...","url":"..."}`
2. Show the user the ClickUp URL so they can click through
3. Confirm ticket was created successfully

**Example:**
```
✅ Ticket created: https://app.clickup.com/t/abc123xyz
```

**If command fails:** Show the error and help user troubleshoot (check ENV variables).

## Notes

- Default tags are automatically added (configurable via `CLICKUP_DEFAULT_TAG`, comma-separated)
- Default status: "BACKLOG" (configurable via `CLICKUP_DEFAULT_STATUS`)
- Default list: Set via `CLICKUP_DEFAULT_LIST_ID`
- For help/options: `./clup.sh --help`


