# AWS Cloud Practitioner Repository - README Specification

This document defines the rules and guidelines for creating and maintaining README files in this repository.

---

## 1. Repository Structure

```
aws-cloud-practitioner/
├── README.md              # Main entry point - index to all modules
├── SPEC.md                # This specification file
├── .gitignore             # Git ignore file
└── docs/
    └── XX-topic-name.md   # Individual topic modules (numbered)
```

---

## 2. README File Types

### 2.1 Main README (`README.md`)

**Location:** Root directory  
**Purpose:** Main entry point and index to the entire repository  
**Required Elements:**
- Repository title with AWS Cloud Practitioner badge
- About section describing the repository purpose
- Documentation structure diagram (Mermaid)
- Study modules table with status indicators (✅ Available / 🚧 Coming Soon)
- Quick start guide
- Documentation index with links to all modules
- Tools& resources section
- License section

**Mermaid Requirements:**
- `graph TD` showing the documentation structure hierarchy

### ~~2.2 Documentation Index (`docs/README.md`)~~ — REMOVED

Consolidated into the root README.md to eliminate duplication.

### 2.3 Topic Modules (`docs/XX-topic-name.md`)

**Location:** `docs/` directory  
**Naming Convention:** `XX-topic-name.md` where XX is a two-digit number (01, 02, etc.)  
**Purpose:** Detailed content for a specific AWS topic  
**Required Elements:**
- H1 title for the topic
- Section headers (H2, H3) for content organization
- At least 2-3 Mermaid diagrams per document
- Tables for structured data comparisons
- Key takeaways section at the end
- Next/Previous navigation links
- Footer indicating part of the study materials

**Mermaid Requirements (minimum 2 diagrams):**
- `timeline` for historical content
- `mindmap` for concept organization
- `flowchart` or `graph` for processes and relationships
- `graph TD/LR` for hierarchical content

---

## 3. Mermaid Diagram Guidelines

### 3.1 Required Diagram Types by Context

| Context | Recommended Diagram Type |
|---------|--------------------------|
| History/Timeline | `timeline` |
| Concept Organization | `mindmap` |
| Hierarchical Structure | `graph TD` or `graph LR` |
| Process/Workflow | `flowchart TD` or `flowchart LR` |
| Comparison | `graph LR` with subgraphs |
| Relationships | `flowchart LR` with arrows |

### 3.2 Mermaid Best Practices

1. **Always use fenced code blocks** with ` ```mermaid ` syntax
2. **Use descriptive node labels** - avoid single letters except for root nodes
3. **Use consistent styling** - similar concepts should have similar diagram structures
4. **Keep diagrams focused** - one diagram per concept, not multiple concepts in one
5. **Include titles** where appropriate using `title` keyword

### 3.3 Recommended Diagram Count

| File Type | Minimum | Recommended |
|-----------|---------|--------------|
| Main README | 1 | 1-2 |
| Docs Index | 1 | 1-2 |
| Topic Module | 2 | 3-4 |

---

## 4. Content Organization

### 4.1 Topic Module Structure

```markdown
# Topic Title

## The Big Picture (optional)
> Opening quote or question to engage readers

---

## Section 1
### Subsection 1.1
### Subsection 1.2

## Section 2
### Subsection 2.1
### Subsection 2.2

---

## Key Takeaways
1. Point 1
2. Point 2
3. Point 3

---

## Next Steps
⬅️ Previous: [Previous Topic](./XX-previous-topic.md) | ➡️ Next: [Next Topic](./XX-next-topic.md)

---

*This documentation is part of the AWS Cloud Practitioner certification study materials.*
```

### 4.2 Table Guidelines

Use tables for:
- Feature comparisons
- Historical milestones
- Service comparisons
- Structured key-value information

Table format:
```markdown
| Header 1 | Header 2 | Header 3 |
|----------|----------|----------|
| Cell 1   | Cell 2   | Cell 3   |
```

---

## 5. Naming Conventions

### 5.1 File Names

- Use lowercase with hyphens: `01-introduction-to-aws.md`
- Use two-digit prefixes for ordering: `01-`, `02-`, `03-`, etc.
- Be descriptive but concise: `02-cloud-models-and-pricing.md`

### 5.2 Module Numbers

| Number | Topic |
|--------|-------|
| 01 | Introduction to AWS |
| 02 | Cloud Models & Pricing |
| 03 | AWS Global Infrastructure |
| 04 | Cloud Adoption Framework |
| 05 | Well-Architected Framework |
| 06 | AWS Ecosystem |

---

## 6. Navigation & Links

### 6.1 Cross-References

Each topic module must include navigation to adjacent modules:
```markdown
⬅️ Previous: [Topic Name](./XX-previous-topic.md) | ➡️ Next: [Topic Name](./XX-next-topic.md)
```

### 6.2 Link Format

- Relative paths from the file location
- Use descriptive link text, not "click here"
- Example: `[Introduction to AWS](./01-introduction-to-aws.md)`

---

## 7. Style Guidelines

### 7.1 Formatting

- Use `---` as section dividers for major sections
- Use **bold** for key terms and important concepts
- Use > for blockquotes (opening questions, definitions)
- Use emojis sparingly for visual hierarchy (📚, 🗂️, 🚀, ✅, 🚧)

### 7.2 Writing Style

- Use active voice
- Keep sentences concise
- Include practical examples where appropriate
- Explain "why" not just "what"

### 7.3 Code/Mermaid Blocks

- Always specify language for syntax highlighting
- Mermaid blocks must use ` ```mermaid ` (not ```mmd or other variants)

---

## 8. Status Indicators

| Symbol | Meaning |
|--------|---------|
| ✅ Available | Module is complete and ready to study |
| 🚧 Coming Soon | Module is planned but not yet created |

---

## 9. Checklist for New Modules

Before creating a new module, verify:

- [ ] Follows naming convention (`XX-topic-name.md`)
- [ ] Contains at least 2 Mermaid diagrams
- [ ] Has Key Takeaways section
- [ ] Has Next/Previous navigation
- [ ] Links are correctly formatted
- [ ] Tables are properly formatted
- [ ] Updates main README.md with new module entry

---

## 10. Version

**Current Version:** 1.0  
**Last Updated:** 2026-06-11
