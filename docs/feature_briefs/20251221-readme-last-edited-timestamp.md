# Feature Brief: README Last Edited Timestamp

## Overview

### What the feature does
Automatically appends a "Last edited on [date]" line to the end of README.md whenever the file is modified and committed.

### Why it matters
- Provides visibility into when documentation was last updated
- Helps contributors and users understand the freshness of the README content
- Automates a manual task, ensuring consistency

### Who uses it
- Repository maintainers (DJ)
- Contributors reviewing the README
- Users checking documentation currency

---

## Requirements & Scope

### Functional Requirements
1. When README.md is staged for commit, automatically update the "last edited" timestamp
2. The timestamp must reflect the local machine's date at commit time
3. The format must be: `---\n\n_Last edited on Month DD, YYYY_`
4. If the timestamp line already exists, it must be updated (not duplicated)
5. If the timestamp line does not exist, it must be appended

### Non-Functional Requirements
1. The solution must use Node.js (consistent with project tooling)
2. No external dependencies required
3. The hook must be lightweight and fast (< 1 second execution)
4. Must work on Windows (primary development environment)

### Out of Scope
- Updating timestamps for any file other than README.md
- Tracking edit history (only most recent edit date)
- CI/CD integration
- Timezone conversion (uses local time only)

### Feature-Level Acceptance Criteria
- [ ] Committing a change to README.md results in the timestamp being updated
- [ ] Committing changes that do NOT include README.md leaves the timestamp unchanged
- [ ] The timestamp format matches `_Last edited on Month DD, YYYY_`
- [ ] Running the hook multiple times does not create duplicate timestamp lines
- [ ] The hook works correctly on Windows with Git Bash / MINGW

---

## Technical Design

### Architecture Approach
A two-part solution:
1. **Node.js script** (`scripts/update-readme-timestamp.js`) - Handles the logic of reading, modifying, and writing the README
2. **Git pre-commit hook** (`.git/hooks/pre-commit`) - Triggers the script only when README.md is staged

### Key Components & Responsibilities

#### 1. `scripts/update-readme-timestamp.js`
- Reads README.md content
- Parses to detect existing timestamp line (regex: `^_Last edited on .+_$`)
- Generates new timestamp in format "Month DD, YYYY"
- Either updates existing line or appends new section
- Writes updated content back to README.md
- Re-stages README.md so the timestamp change is included in the commit

#### 2. `.git/hooks/pre-commit`
- Checks if README.md is in the staged files
- If yes, executes the Node.js script
- If no, exits silently (allows commit to proceed)

#### 3. `scripts/install-hooks.js` (setup helper)
- Copies/creates the pre-commit hook into `.git/hooks/`
- Makes the hook executable
- Can be run via `npm run prepare` or manually

### Data Models and Schema Changes
None - this feature only modifies README.md content.

### APIs or Interfaces
None - purely local file system operations.

### UI/UX Considerations
None - this is a developer tooling feature.

---

## Dependencies & Risks

### External Libraries/Services
- None required (uses Node.js built-in `fs` and `child_process` modules)

### Potential Blockers
- Git hooks are not automatically shared via git clone (hooks live in `.git/` which is not tracked)
- Solution: Provide `scripts/install-hooks.js` and document setup

### Compatibility Concerns
- Windows path handling (use `path` module for cross-platform compatibility)
- Git Bash on Windows may have different behavior than Unix shells
- Solution: Use Node.js for all logic, minimal shell scripting in hook

### Unknowns Requiring Investigation
- None identified

---

## Implementation Summary

This feature will be implemented in 4 small tasks:

1. Create the timestamp update script
2. Create the git pre-commit hook
3. Create the hook installation helper script
4. Update README.md with initial timestamp and setup documentation

Each task is independently testable and can be completed in sequence.

---

## Task List

### Status Legend
- Status: TODO        # Not started
- Status: IN_PROGRESS # Currently being worked on by an agent
- Status: DONE        # Completed and validated

## Tasks

- [ ] TASK-001 – Create README timestamp update script
  - Status: TODO
  - Owner: unassigned
  - Description: Create a Node.js script that reads README.md, updates or appends the "last edited" timestamp line, writes the file back, and re-stages it for commit. The script should be idempotent (safe to run multiple times).
  - Acceptance Criteria:
    - Script exists at `scripts/update-readme-timestamp.js`
    - Running the script updates README.md with current date in format `_Last edited on Month DD, YYYY_`
    - If timestamp line exists, it is replaced (not duplicated)
    - If timestamp line does not exist, `---\n\n_Last edited on ..._` is appended
    - Script re-stages README.md after modification using `git add README.md`
    - Script exits with code 0 on success, non-zero on failure
  - Files to change or create:
    - `scripts/update-readme-timestamp.js` – new file, main script logic
  - Tests:
    - Manually run `node scripts/update-readme-timestamp.js` and verify README.md is updated with correct format
    - Run script twice in a row and verify only one timestamp line exists
    - Verify date format matches "Month DD, YYYY" (e.g., "December 21, 2025")
  - Dependencies: none

- [ ] TASK-002 – Create git pre-commit hook
  - Status: TODO
  - Owner: unassigned
  - Description: Create a pre-commit hook script that checks if README.md is staged and, if so, runs the timestamp update script. The hook should be a shell script compatible with Git Bash on Windows.
  - Acceptance Criteria:
    - Hook file exists at `scripts/hooks/pre-commit`
    - Hook only runs timestamp script when README.md is in staged files
    - Hook exits cleanly (code 0) when README.md is not staged
    - Hook calls `node scripts/update-readme-timestamp.js` when README.md is staged
    - Hook is executable (has proper shebang line)
  - Files to change or create:
    - `scripts/hooks/pre-commit` – new file, the hook script
  - Tests:
    - Stage a non-README file, commit, verify timestamp is NOT updated
    - Stage README.md (make a small change), commit, verify timestamp IS updated
    - Verify commit succeeds in both scenarios
  - Dependencies: TASK-001

- [ ] TASK-003 – Create hook installation helper script
  - Status: TODO
  - Owner: unassigned
  - Description: Create a Node.js script that installs the pre-commit hook by copying it to `.git/hooks/pre-commit`. Add an npm script `prepare` that runs this installer automatically after `npm install`.
  - Acceptance Criteria:
    - Script exists at `scripts/install-hooks.js`
    - Running the script copies `scripts/hooks/pre-commit` to `.git/hooks/pre-commit`
    - Script handles case where `.git/hooks/` directory exists
    - Script outputs success/failure message to console
    - `package.json` has `"prepare": "node scripts/install-hooks.js"` script
    - Running `npm run prepare` successfully installs the hook
  - Files to change or create:
    - `scripts/install-hooks.js` – new file, installation logic
    - `package.json` – add "prepare" script
  - Tests:
    - Delete `.git/hooks/pre-commit` if it exists
    - Run `npm run prepare`
    - Verify `.git/hooks/pre-commit` exists and matches `scripts/hooks/pre-commit`
  - Dependencies: TASK-002

- [ ] TASK-004 – Update README with initial timestamp and setup docs
  - Status: TODO
  - Owner: unassigned
  - Description: Add the initial timestamp line to README.md and add a brief section documenting the auto-timestamp feature for future contributors.
  - Acceptance Criteria:
    - README.md ends with `---\n\n_Last edited on [current date]_`
    - README.md includes a brief note about the auto-timestamp feature (in setup instructions section)
    - Documentation explains that `npm install` will set up the hook automatically
  - Files to change or create:
    - `README.md` – append timestamp, add documentation note
  - Tests:
    - Visually verify README.md has the timestamp at the end
    - Verify the setup documentation is clear and accurate
  - Dependencies: TASK-001, TASK-002, TASK-003
