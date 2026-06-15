# Contribution 1: Label the TLS in vmmap

**Contribution Number:** 1
**Student:** Chenyan Zhu  
**Issue:** GitHub issue link](https://github.com/pwndbg/pwndbg/issues/1570)
**Status:** Phase I Complete

---

## Why I Chose This Issue

I chose this issue because it is a focused enhancement in a Python-based debugging tool, and it connects well with my interest in systems, infrastructure, and low-level debugging. 
The issue asks for improving the `vmmap` command so that Thread Local Storage (TLS) information is visible in the memory map output, 
which feels specific enough for a first open-source contribution while still giving me a chance to learn how pwndbg represents process memory. 

This issue also matches my current skill level because pwndbg is mainly written in Python, 
and the relevant behavior seems to involve reading existing command logic rather than designing a large new feature from scratch. 
I hope to learn how a real debugging tool structures commands, memory map data, 
and output formatting, and I want to practice making a small but useful contribution to a widely used developer tool.

---

## Understanding the Issue

### Problem Description

The `tls` command in pwndbg can already find and print the Thread Local Storage (TLS) base address. However, when the user runs `vmmap`, the memory mapping that contains the TLS address is displayed only as a normal anonymous mapping, without any label showing that it contains TLS. This makes it harder for users to quickly identify where TLS is located in the process memory layout.

### Expected Behavior

When `vmmap` displays the memory map, it should annotate the exact TLS base address inside the relevant mapping, rather than relabeling the entire mapping as TLS. This avoids implying that the whole page is TLS when the TLS base is not page-aligned and the page may contain non-TLS padding or other data.

### Current Behavior

Currently, `tls` can show the TLS base address, but `vmmap` does not include that information in its output. The issue example shows TLS at `0x7ffff7cb3800`, while `vmmap` only shows a nearby anonymous memory mapping such as `[anon_7ffff7cb3]`.

### Affected Components

The likely affected components are the `tls` command, the `vmmap` command, and the memory map/page display logic. I will start by reading the implementation of the relevant command files and memory map helpers.

---

## Reproduction Process

### Environment Setup

[Notes on setting up your local development environment - challenges you faced, how you solved them]

### Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Observed result]

### Reproduction Evidence

- **Commit showing reproduction:** [Link to commit in your fork]
- **Screenshots/logs:** [If applicable]
- **My findings:** [What you discovered during reproduction]

---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
