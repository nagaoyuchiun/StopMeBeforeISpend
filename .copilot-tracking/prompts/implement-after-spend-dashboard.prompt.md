---
agent: agent
model: Claude Sonnet 4.5 (copilot)
---

<!-- markdownlint-disable-file -->

# Implementation Prompt: 實際大額消費影響儀表板

## Implementation Instructions

### Step 1: Create Changes Tracking File

You WILL create `20260107-after-spend-dashboard-changes.md` in .copilot-tracking/changes/ if it does not exist.

### Step 2: Execute Implementation

You WILL follow .github/instructions/task-implementation.instructions.md
You WILL systematically implement .copilot-tracking/plans/20260107-after-spend-dashboard-plan.instructions.md task-by-task
You WILL follow ALL project standards and conventions

**CRITICAL**: If ${input:phaseStop:true} is true, you WILL stop after each Phase for user review.
**CRITICAL**: If ${input:taskStop:false} is true, you WILL stop after each Task for user review.

### Step 3: Cleanup

When ALL Phases are checked off (`[x]`) and completed you WILL do the following:

1. You WILL provide a markdown style link and a summary of all changes from .copilot-tracking/changes/20260107-after-spend-dashboard-changes.md to the user:

   - You WILL keep the overall summary brief
   - You WILL add spacing around any lists
   - You MUST wrap any reference to a file in a markdown style link

2. You WILL provide markdown style links to:
   - .copilot-tracking/plans/20260107-after-spend-dashboard-plan.instructions.md
   - .copilot-tracking/details/20260107-after-spend-dashboard-details.md
   - .copilot-tracking/research/20260107-after-spend-dashboard-research.md
   
   You WILL recommend cleaning these files up as well.

3. **MANDATORY**: You WILL attempt to delete .copilot-tracking/prompts/implement-after-spend-dashboard.prompt.md

## Success Criteria

- [ ] Changes tracking file created
- [ ] All plan items implemented with working code
- [ ] All detailed specifications satisfied
- [ ] Project conventions followed
- [ ] Changes file updated continuously
- [ ] Dashboard displays all core metrics
- [ ] Cumulative calculations correct
- [ ] Expense history list functional
- [ ] Timeline visualization clear
- [ ] Firestore real-time listeners working
- [ ] Loading and empty states friendly
- [ ] Error handling complete
- [ ] Responsive design working
