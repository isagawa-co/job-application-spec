# /kernel/fix

Run impact assessment before implementing any fix to kernel components.

## Usage

```
/kernel/fix the hook isn't blocking correctly
/kernel/fix session-start creates new domain when one exists
/kernel/fix
```

## Instructions

### Step 1: Understand the Fix

If `$ARGUMENTS` provided, that's the fix description.
If empty, ask user: "What needs to be fixed?"

### Step 2: Log Defect

Before analysis, log in `DEFECT_LOG.md`:
- DEF-XXX with brief description
- Date, severity, status: OPEN
- What happened, expected vs actual

### Step 3: Impact Assessment (MANDATORY)

Before writing ANY code, complete ALL four:

#### 3.1 Who calls this code?

```bash
grep -r "function_or_file_name" .claude/
```

#### 3.2 What depends on current behavior?

- List commands that use this
- List hooks that check this
- List state files affected

#### 3.3 What will break?

Explicit list:
- Commands affected
- Hooks affected
- State contracts broken

#### 3.4 Migration path?

- Backward compatible? Yes/No + why

### Step 4: Present Assessment

```
## Impact Assessment: [Fix description]

### 1. Callers
- `command.md:N` - references this

### 2. Dependencies
- Commands: X, Y, Z
- Hooks: A, B
- State: field_name in workflow.json

### 3. What Breaks
- [item] - reason (or "None identified")

### 4. Migration
- Backward compatible: Yes/No

**Proceed with fix?**
```

### Step 5: Approval Gate

- If cycling mode is active: auto-proceed (no breaking changes)
- If NOT cycling: wait for user approval

### Step 6: Implement Fix

After approval:
1. Make the changes
2. Update any dependent commands/hooks

### Step 7: Invoke /kernel/learn

After fix is complete, record the lesson.

## When to Invoke

- Before ANY fix to kernel components
- Before ANY fix to hooks
- Before ANY change to state contracts
