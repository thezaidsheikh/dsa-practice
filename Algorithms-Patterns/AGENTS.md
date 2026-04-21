# AGENTS.md - Algorithms-Patterns

This folder contains core algorithm pattern explanations and conceptual learning material.

## Folder Structure

- `graph/` - Graph-related concepts (traversal, topological sort, Kahn's algorithm, etc.)
- Other pattern files at root level

---

## Content Style - Required Sections

Every pattern file MUST include these sections in order:

### 1. Definition
Clear one-liner explaining what the pattern is. Should be understandable in one sentence.

### 2. When To Use (Recognition Cues)
Problem recognition cues and keywords that signal this pattern:
- Specific phrases that appear in problem statements
- Problem types (array, string, tree, graph, etc.)
- Constraints that hint at the pattern

### 3. Why It Works
Intuition and reasoning. Explain the fundamental insight that makes this pattern work. This is crucial - don't just state what to do, explain WHY it works.

### 4. Real-World Example (Single Example Throughout)
Use ONE consistent real-world example to explain ALL concepts within the pattern file. The same example should be used from start to finish.

Examples:
- **Graph**: Metro system with stations and tracks
- **Two Pointers**: Two people walking towards each other
- **Sliding Window**: Moving window in a house while cleaning
- **Prefix Sum**: Bank account running balance
- **Kadane's Algorithm**: Stock price fluctuation over days
- **Stack**: Stack of plates in a cafeteria
- **Queue**: People waiting in line at a ticket counter

### 5. Algorithm Steps
Numbered step-by-step procedure for implementing the approach.

### 6. Visual Example
ASCII diagrams, tables, or illustrations showing how the algorithm works on the real-world example.

### 7. Variations / Different Cases
Common variations of the pattern:
- When the standard approach doesn't apply
- Edge cases
- Common modifications

### 8. Code Implementation
Java code block showing the approach:
- Use `class Solution` for LeetCode-style code
- Include helper methods if needed
- Comment key logic briefly

### 9. Complexity Analysis
Time and space complexity with explanation:
- Best/Average/Worst case when different
- Why this complexity is optimal or necessary

### 10. Comparison (When Relevant)
Compare with related patterns:
- When to choose this vs alternatives
- Trade-offs

### 11. Common Problems
List of LeetCode or similar problems that use this pattern:
- Problem name/number
- What variation of the pattern they use

### 12. Interview Key Points
What interviewers want to hear:
- Edge cases to mention
- Optimizations to discuss
- Follow-up questions to anticipate
- Time to mention (if asked)

### 13. Way To Remember
Memory aid or mental shortcut:
- One-liner that captures the essence
- Visual mental image
- Analogy

### 14. Common Pitfalls
Mistakes to avoid:
- Off-by-one errors
- Edge cases forgotten
- Incorrect complexity assumptions

---

## Naming Conventions

- Use kebab-case: `graph.md`, `kahns-algorithm.md`, `two-pointers.md`
- Use descriptive names that match the pattern name
- Match existing style in sibling files

---

## Code Standards

- Use Java for implementations (unless folder suggests otherwise)
- Use `class Solution` for LeetCode-style code
- Include meaningful variable names (left, right, sum, prev, etc.)
- Format inside fenced code blocks with `java` language tag
- Keep code readable and interview-appropriate

---

## Quality Checklist

Before finishing a pattern file, verify:

- [ ] Definition is clear and concise
- [ ] Recognition cues are specific and actionable
- [ ] "Why it works" explains the insight, not just the steps
- [ ] ONE real-world example is used consistently
- [ ] Algorithm steps are numbered and clear
- [ ] Visual example includes ASCII diagram
- [ ] All variations are covered
- [ ] Code compiles mentally and follows Java conventions
- [ ] Complexity analysis is complete with Big-O
- [ ] Comparison section exists for related patterns
- [ ] At least 3 common problems are listed
- [ ] Interview key points are specific to this pattern
- [ ] "Way to remember" is memorable
- [ ] Common pitfalls are listed

---

## What To Avoid

- Do not add single problem notes in this folder
- Do not use overly casual language
- Do not skip complexity analysis
- Do not omit the "why it works" explanation
- Do not change the real-world example mid-file
- Do not include multiple unrelated examples
- Do not skip interview key points section

---

## Section Template

Use this template when creating a new pattern file:

```md
# Pattern Name

## Definition
<1-2 sentence definition>

## When To Use
- Keywords: <list of problem keywords>
- Problem types: <types that hint at this pattern>
- Recognition: <how to identify this pattern in a problem>

## Why It Works
<The fundamental insight that makes this pattern work>

## Real-World Example
<ONE consistent example used throughout>
<Explain all concepts using this example>

## Algorithm Steps
1. <Step 1>
2. <Step 2>
3. <Step 3>

## Visual Example
```
<ASCII diagram showing the example>
```

## Variations
- <Variation 1>: <description>
- <Variation 2>: <description>

## Code Implementation
```java
// code here
```

## Complexity Analysis
- Time: O(<complexity>)
- Space: O(<complexity>)

## Comparison
| This Pattern | Alternative | When To Choose |
| --- | --- | --- |
| | | |

## Common Problems
- <Problem 1> - <what variation it uses>
- <Problem 2> - <what variation it uses>
- <Problem 3> - <what variation it uses>

## Interview Key Points
- <Point 1>
- <Point 2>
- <Point 3>

## Way To Remember
<One-liner or analogy to remember>

## Common Pitfalls
- <Pitfall 1>
- <Pitfall 2>
```
