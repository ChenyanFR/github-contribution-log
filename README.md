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

### Environment Setup

I forked the `pwndbg/pwndbg` repository and created a working branch for this issue. Since this issue depends on Linux memory mapping behavior, I plan to use a Linux-based development environment such as Codespaces, WSL, or a Linux VM rather than macOS directly.

### Steps to Reproduce

1. Set up pwndbg in a Linux-based environment.
2. Start a simple binary with GDB and pwndbg enabled, such as `/bin/ls`.
3. Run `start` to initialize the process.
4. Run `tls` to get the Thread Local Storage base address.
5. Run `vmmap` and locate the memory mapping that contains the TLS base address.
6. Observe that the TLS base address is not annotated in the `vmmap` output.

### Reproduction Evidence

- **Commit showing reproduction:** TBD
- **Screenshots/logs:** TBD
- **My findings:** Based on the issue discussion, `tls` can identify the TLS base address, but `vmmap` only shows the surrounding anonymous mapping. The maintainer clarified that the entire mapping should not be relabeled as TLS because the TLS base is not page-aligned.
---

## Solution Approach

### Analysis

The issue is not simply that the anonymous mapping should be renamed to `[tls]`. Based on maintainer feedback, the TLS base address may be located inside a larger page rather than at the beginning of the mapping. In the example, the mapping starts at `0x...3000`, while the TLS base is `0x...3800`, which means at least `0x800` bytes before the TLS base are not TLS. Therefore, labeling the entire page as TLS would be misleading.

### Proposed Solution

The better approach is to annotate the exact TLS base address in the `vmmap` output, similar to how the `vis` command displays address annotations. I will investigate how `vmmap` formats memory mappings and how `vis` adds markers such as `<--- ...`, then adapt that pattern so `vmmap` can show something like `<--- TLS` at the TLS base address.

### Implementation Plan

Using UMPIRE framework:

**Understand:**  
The `tls` command can find the TLS base address, but `vmmap` does not display that address in a recognizable way.

**Match:**  
I will compare the `tls`, `vmmap`, and `vis` implementations to find existing patterns for retrieving addresses and annotating output.

**Plan:**
1. Read the `tls` command implementation to understand how TLS base addresses are retrieved.
2. Read the `vmmap` command implementation to understand how memory mappings are displayed.
3. Read the `vis` command implementation to understand how address annotations are rendered.
4. Reproduce the current behavior locally.
5. Implement a small change that annotates the TLS base address without relabeling the whole mapping.
6. Verify that the approach works across architectures if possible.
7. Document any SELinux-related setup caveat mentioned by the maintainer.
8. Add or update tests if the project has existing tests for `vmmap` output.

**Review:**  
I will check that the change follows pwndbg’s existing output style and does not imply that an entire page is TLS when only part of the mapping may be related to TLS.

**Evaluate:**  
I will compare the output of `tls` and `vmmap` before and after the change, and run relevant pwndbg tests if possible.

---

## Testing Strategy

### Manual Testing

- [ ] Run `tls` and record the TLS base address.
- [ ] Run `vmmap` and confirm that the TLS base address is annotated.
- [ ] Confirm that the surrounding mapping is not incorrectly renamed entirely as TLS.
- [ ] Compare the output style with the existing `vis` annotation format.

### Unit Tests

- [ ] Search for existing tests around `vmmap` output.
- [ ] Add or update a test if there is an existing pattern for command output testing.

### Integration Tests

- [ ] Run the relevant pwndbg test suite if the local environment supports it.

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
