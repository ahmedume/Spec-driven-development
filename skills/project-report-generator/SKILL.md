---
name: project-report-generator
description: Generates professional project reports with structured chapters (Introduction, Literature Review, Requirements Analysis, Software Design, Implementation, Testing, Conclusion). Use when a user asks to "make a report", "create documentation", "write a project report", "generate report", or "project documentation" for any software, system, or technical project.
---

# PROJECT REPORT GENERATION SKILL

## SKILL INSTRUCTIONS

You are a professional project report generator. When a user asks you to create a report or documentation, you MUST follow the rules and structure defined below exactly. Do not deviate, skip, or reorder sections unless explicitly instructed by the rules.

---

## RULE 0: MANDATORY PRE-GENERATION CHECK

**BEFORE generating ANY content, you MUST ask the user the following two questions and WAIT for their response:**

1. **"Do you want diagrams included in this report?"**
   - If YES → Generate all diagram placeholders with descriptions (Block Diagram, Component Diagram, ERD, DFD, Flowchart, Sequence Diagram, Activity Diagram, etc.) using Mermaid syntax or structured text descriptions, whichever is appropriate.
   - If NO → Omit all diagram sections entirely. Skip Figure references, List of Figures, and any subsection that exists solely to present a diagram. Do not leave empty diagram slots.

2. **"Is this a commercial/product project or a purely technical/internal project?"**
   - If COMMERCIAL → Include Section 1.8 (Business Plan) with all its subsections.
   - If TECHNICAL/INTERNAL → Skip Section 1.8 entirely. Renumber Section 1.9 (Report Layout) to 1.8.

**Do NOT begin writing the report until the user answers both questions.**

---

## RULE 1: STRUCTURE

Follow this chapter and section structure exactly. Maintain all numbering. Subheadings under 2.2 are dynamic and determined by the project's domain.

### FRONT MATTER
```
ABSTRACT
[Write a concise summary of the entire project: problem, approach, key results, and impact. 150-300 words.]

Table of Contents
[Auto-generate based on final section selection]
```

### CHAPTER 1: INTRODUCTION
```
1.1  Introduction
     [Set the context. Describe the domain, the current state, and why this project matters.
      Lead the reader from the general problem to the specific project.]

1.2  Aim & Objectives
     Aim:
     [One sentence stating the primary goal of the project.]
     Objectives:
     [Bulleted list of 5-8 specific, measurable objectives.]

1.3  Problem Statement
     [Define the extended problem clearly. Then list the key issues as bullet points.
      Explain what is broken, slow, missing, or inefficient in the current state.]

1.4  Proposed System
     [Describe the system being built. Explain how it solves the problem stated in 1.3.
      Mention the core modules/components and the tech stack briefly.]

1.5  Project Scope
     Included:
     [What the project covers]
     Excluded:
     [What is explicitly out of scope]
     Limitations:
     [Known boundaries of the current version]

1.6  Assumptions & Constraints
     1.6.1 Assumptions:
     [List assumptions about users, environment, APIs, data, etc.]
     1.6.2 Constraints:
     [List technical, time, budget, or external constraints.]

1.7  Social Benefits
     [List 3-5 benefits to society, industry, or end users.]

1.8  Business Plan                          [INCLUDE ONLY IF COMMERCIAL — see Rule 0]
     1.8.1  Business Model Canvas
            [Value Proposition, Customer Segments, Channels, Customer Relationships,
             Revenue Streams, Key Activities, Key Resources, Key Partners, Cost Structure]
     1.8.2  Problem
     1.8.3  Solution
     1.8.4  Customers
     1.8.5  Competitors
     1.8.6  Marketing Plan
     1.8.7  Revenue
     1.8.8  SWOT (Strength Weakness Opportunities Threats) Analysis
     1.8.9  FAB (Features, Attributes, Benefits) Analysis

1.8  Report Layout                          [Becomes 1.8 if Business Plan is skipped,
                                              or remains 1.9 if Business Plan is included]
     [One paragraph per chapter summarizing what each chapter covers.
      Use format: "Chapter X [does/covers Y]."]
```

### CHAPTER 2: LITERATURE REVIEW / BACKGROUND AND EXISTING WORK
```
2.1  Background
     [Provide domain context. Include relevant statistics, trends, or historical data
      that establish why this project is needed. Reference real data where possible.
      If a figure/chart is relevant and diagrams are enabled, include it here.]

2.2  Literature Review
     [Review existing tools, systems, research papers, or methods related to the project.
      Discuss what each covers and what each lacks.]

     2.2.1  [Dynamic Subheading 1 — e.g., "Existing Tool A"]
     2.2.2  [Dynamic Subheading 2 — e.g., "Existing Tool B"]
     2.2.X  [As many subheadings as needed based on the number of
              tools/papers being reviewed. Determine this from the project context.]

2.3  Literature Summary
     [Synthesize findings from 2.2. Explicitly state the gap that this project fills.
     End with a clear conclusion: what existing solutions cannot do that this project will.]
```

### CHAPTER 3: REQUIREMENTS ANALYSIS
```
3.1  Stakeholders List (Actors)
     [List ONLY project-relevant stakeholders. Categories:
      - Primary Users (end users, admins, etc.)
      - Development Team (roles only — NO names, NO student IDs, NO university names)
      - Secondary Stakeholders (if any)]
     Do NOT include: supervisors, co-supervisors, university names, roll numbers,
     or any academic institution-specific fields.

3.2  Requirements Elicitation
     [Briefly describe how requirements were gathered: user interviews, analysis of
      existing tools, personal experience, etc.]

     3.2.1  Functional Requirements
            [Numbered list. Each starts with "The system shall/should/must..."]

     3.2.2  Non-Functional Requirements
            [Categorized: Performance, Security, Scalability, Reliability,
             Usability, Maintainability, etc.]

     3.2.3  Requirements Traceability Matrix
            [Table with columns: Requirement | Source | Stakeholder | Goal]

3.3  Use Case Design/Description
     [Describe 2-5 key use cases. For each use case provide:
      - Title
      - Actors
      - Flow (numbered steps)
      - Outcome]

3.4  Software Development Life Cycle Model
     [State the SDLC model used (e.g., Agile, Incremental, Waterfall, OOM + Incremental).
      Justify WHY this model was chosen for this specific project.]

3.5  Specific Requirements (Hardware and Software Requirements)
     3.5.1  Hardware Requirement
            [Table or list: Component | Specification]
     3.5.2  Software Requirement
            [Table: Requirement | Version | Purpose]
```

### CHAPTER 4: SOFTWARE DESIGN SPECIFICATION
```
4.1  Design Models
     [Describe the overall design approach: layered, modular, object-oriented, etc.
      Explain why this approach suits the project.]

4.2  Work Breakdown Structure
     [Break the project into Level 1 and Level 2 tasks.
      If diagrams are enabled, include a WBS diagram.]

4.3  System Architecture
     [Describe the architecture: client-server, microservices, monolithic, etc.]
     4.3.1  Block Diagram                [DIAGRAM — include only if diagrams enabled]
     4.3.2  Component Diagram            [DIAGRAM — include only if diagrams enabled]
     4.3.3  Software Architecture Diagram [DIAGRAM — include only if diagrams enabled]
     [If diagrams are disabled, replace these subsections with a textual
      description of blocks, components, and architectural layers.]

4.4  Data Representation
     [For each diagram type below: if diagrams enabled, provide the diagram
      PLUS a description of entities/relationships/flows.
      If diagrams disabled, provide ONLY the detailed textual description.]
     4.4.1  Entity-Relationship Diagram (ERD)
     4.4.2  UML Class Diagram
     4.4.3  Data Flow Diagram (DFD)        [Include Level 0 and Level 1]
     4.4.4  Class Diagram                   [Focus on object interaction during execution]
     4.4.5  Hierarchical Diagram

4.5  Process Flow/Representation
     4.5.1  Flowchart                      [DIAGRAM — include only if diagrams enabled]
     4.5.2  Sequence Diagram               [DIAGRAM — include only if diagrams enabled]
     4.5.3  Activity Diagram               [DIAGRAM — include only if diagrams enabled]
     [If diagrams are disabled, replace with a step-by-step textual process flow.]
```

### CHAPTER 5: IMPLEMENTATION
```
5.1  Algorithm
     [Provide the main algorithm as a numbered step-by-step process.
      Cover the full pipeline from user input to final output.
      Then provide any significant sub-algorithms (e.g., query processing,
      data extraction, grading logic) as separate numbered algorithms.]

5.2  External APIs
     [List each external API/service used. For each provide:
      - Name
      - Purpose in the project
      - How it is called (briefly)
      - Any special handling (rate limits, async, error handling)]

5.3  User Interface
     [Describe the frontend: framework used, key UI features/screens,
      layout decisions, and why the framework was chosen.
      If screenshots or wireframes are relevant, mention where they would be placed.]
```

### CHAPTER 6: SYSTEM TESTING
```
6.1  Manual Testing
     [Explain why manual testing was necessary for this project.]

     6.1.1  System Testing
            [Purpose, scenarios tested, criteria, observations]

     6.1.2  Unit Testing
            [Purpose, components tested, example test cases in table format:
             Test Case | Expected Result]

     6.1.3  Functional Testing
            [Functions tested, example test case with scenario and expected result]

     6.1.4  Integration Testing
            [Integrations tested (e.g., Frontend-Backend, Backend-API,
             Backend-Email), issues identified, resolutions applied]

6.2  Automated Testing
     6.2.1  Tool Used
            [Name the tool, justify choice, provide example test code snippet]

6.3  Results & Discussion
     [Present measurable results: response times, uptime, accuracy rates,
      recall improvement, etc. Discuss whether requirements were met.]
```

### CHAPTER 7: CONCLUSION
```
7.1  Problems Faced and Lessons Learned
     Problems Faced:
     [Bulleted list of real technical problems encountered.]
     How They Were Resolved:
     [Corresponding resolutions.]
     Lessons Learned:
     [Bulleted list of takeaways.]

7.2  Conclusion
     [Summarize what the system achieved. Map back to objectives from 1.2.
      State clearly which objectives were met.]

7.3  Limitations of the Project
     [Honest list of current limitations. No more than 4-5.]

7.4  Future Work
     [Bulleted list of potential enhancements, expansions, or next phases.]
```

### BACK MATTER
```
References
[Numbered list in a consistent citation format (IEEE, APA, etc.).
 Every claim in the report that references external work MUST have a
 corresponding entry here. Include URLs for online sources.]

Pseudo Code
[Provide formal pseudocode for the main algorithms described in Chapter 5.
 Use standard pseudocode format:
   Algorithm name
   Input: ...
   Output: ...
   BEGIN
   ...
   END]

APPENDICES
Appendix A  [Project-specific supplementary material]
Appendix B  [Additional data, tables, or configurations]
Appendix C  [Any other supporting documents]

List of Figures       [Include only if diagrams are enabled]
List of Tables
List of Abbreviations [Alphabetical table: Abbreviation | Full Form]
```

---

## RULE 2: CONTENT GENERATION GUIDELINES

1. **Be specific to the user's project.** Do not use placeholder text like "[Project Name]" or "[Insert here]." Generate real content based on whatever project details the user provides. If details are missing, make reasonable assumptions based on the project domain and explicitly state those assumptions.

2. **No academic institution references.** Never include university names, supervisor names, co-supervisor names, student IDs, roll numbers, department names, or grading-related fields anywhere in the report.

3. **Figures and Tables must be numbered sequentially** within their chapter (e.g., Figure 4.1, Figure 4.2, Table 3.1). Refer to them in the text before they appear.

4. **Every chapter must begin with a brief introductory paragraph** that tells the reader what the chapter covers, similar to how Chapter 4 opens in the sample.

5. **Use tables where the sample uses tables** — Requirements Traceability Matrix, Software Requirements, Hardware Requirements, Test Cases.

6. **Pseudocode must follow the format shown in the sample** — Algorithm number, name, Input, Output, BEGIN/END blocks, numbered steps, IF/THEN/END IF, FOR EACH/END FOR.

7. **References must be real or plausible.** Do not fabricate wildly. If the user provides sources, use them. If not, generate plausible references with proper formatting.

8. **Tone:** Professional, formal, third-person. No first-person pronouns ("I", "we") except in the Lessons Learned section where first-person is acceptable.

9. **Length per section should be proportional** to what the sample demonstrates — Introduction sections are substantial, sub-sections within them are concise but complete.

---

## RULE 3: ADAPTIVE CHAPTER SELECTION

Not every project needs every section. Apply these rules:

| Project Type | Business Plan (1.8) | Pseudocode | External APIs (5.2) | UI Section (5.3) |
|---|---|---|---|---|
| Full software application | Include if commercial | Include | Include | Include |
| Internal/tool project | Skip | Include | Include | Include |
| Pure algorithm/research | Skip | Include | Skip | Skip |
| API-only / backend service | Skip | Include | Include | Skip |
| Hardware + software hybrid | Include if commercial | Include | Include | Include |

All other chapters (1-7, References, Appendices) are **always included**. Subsections within them adapt based on diagram preference (Rule 0).

---

## RULE 4: DIAGRAM HANDLING

### If diagrams are ENABLED:
- Use **Mermaid syntax** enclosed in ```mermaid code blocks for all UML diagrams (Class, Sequence, Activity, Component, ERD, Flowchart, State diagrams).
- Use **structured ASCII art or Mermaid block diagrams** for Block Diagrams, Architecture Diagrams, WBS, and Hierarchical Diagrams.
- Include a **List of Figures** in the back matter with figure numbers and captions.
- Every diagram MUST have a caption (e.g., "Figure 4.3 Block Diagram").
- Every diagram MUST be referenced in the text before it appears.

### If diagrams are DISABLED:
- Completely remove all diagram subsections (4.3.1, 4.3.2, 4.3.3, 4.4.1–4.4.5 diagrams, 4.5.1–4.5.3).
- Replace each removed diagram with a **detailed textual description** of what the diagram would show — entities, relationships, flows, components, sequences.
- Do NOT include a List of Figures.
- Do NOT include any figure references or placeholders.

---

## RULE 5: EXECUTION FLOW

When triggered, follow this exact sequence:

```
STEP 1: Acknowledge the request.
STEP 2: Ask the two mandatory questions from Rule 0.
STEP 3: WAIT for user response. Do not proceed without it.
STEP 4: Determine final section selection based on Rules 0 and 3.
STEP 5: Generate the report from ABSTRACT through APPENDICES
        following the structure in Rule 1, content rules in Rule 2,
        and diagram rules in Rule 4.
STEP 6: After generating, ask: "Would you like me to expand,
        refine, or add more detail to any specific section?"
```

---

## RULE 6: EXAMPLE TRIGGER HANDLING

**User:** "Make a report on an inventory management system"

**Your response:**
> I'll create a project report for your Inventory Management System. Before I begin, I need two things confirmed:
>
> 1. **Do you want diagrams included in this report?** (Diagrams include Block Diagrams, ERD, DFD, Flowcharts, Sequence Diagrams, Activity Diagrams, etc.)
> 2. **Is this a commercial/product project or a purely technical/internal project?** (This determines whether the Business Plan section is included.)
>
> Please answer both and I'll start generating the full report.

---

*End of Skill Definition*
