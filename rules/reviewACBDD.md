# SPEC REVIEW PROMPT

## Purpose

Review the full specification of a User Story — including Acceptance Criteria (AC), BDD Scenarios, Business Rules (BR), and User Flow — to assess:
1. Whether each AC is well-written
2. Whether each BDD scenario is well-written
3. Whether ACs are complete and correct relative to the story intent
4. Whether BDD scenarios fully cover all ACs

---

## RULES

### What to do
- Base all judgments strictly on the provided User Story, AC, BDD, BR, and Flow.
- Evaluate each AC and BDD scenario individually and holistically.
- Flag missing coverage, ambiguity, contradictions, and quality issues.
- Group related findings where possible — do not repeat the same issue multiple times.
- If the spec is large, review in batches and ask before continuing.

### What NOT to do
- Do NOT invent business rules not stated or strongly implied by the story.
- Do NOT rewrite AC or BDD unless asked — only flag issues.
- Do NOT treat your own assumptions as confirmed facts.
- Do NOT raise issues that are clearly out of scope for this story.

### When something is unclear
- List it explicitly under Assumptions & Gaps.
- Tag each as: `[Explicit]`, `[Assumed]`, or `[Needs Clarification]`.

---

## WHAT MAKES A GOOD AC

An AC is well-written when it meets **all** of the following criteria:

| Criterion        | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| **Testable**     | Can be verified objectively — has a clear pass/fail condition               |
| **Atomic**       | Covers exactly one behavior or rule — not bundled with others               |
| **Unambiguous**  | No vague terms: "fast", "easy", "appropriate", "should work", etc.         |
| **Bounded**      | States specific conditions, limits, or constraints (not open-ended)         |
| **Actor-aware**  | Clear who does what (user, system, admin, etc.)                             |
| **Result-clear** | States what the system does in response — not just what the user does       |
| **In-scope**     | Belongs to this User Story — not a different story or epic                  |

**Common AC Anti-patterns:**
- ✗ "The system should be user-friendly" — not testable
- ✗ "Users can create and edit and delete products" — not atomic
- ✗ "The form validates correctly" — vague, no specifics
- ✗ "It works on mobile" — needs explicit constraint (which breakpoints? which behaviors?)

---

## WHAT MAKES A GOOD BDD SCENARIO

A BDD scenario is well-written when it meets **all** of the following criteria:

| Criterion              | Description                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| **Maps to one AC**     | Each scenario targets a single AC or a single behavior — no bundling                        |
| **Given is meaningful**| Sets up a real precondition — not just "Given I am on the page"                             |
| **When is one action** | One trigger only — not "When I fill in and submit and confirm"                               |
| **Then is observable** | Describes system output, not internal state — verifiable from UI/API                        |
| **No UI leakage**      | Avoids hardcoded labels/selectors unless testing UI specifically                             |
| **Title is clear**     | Title summarizes the behavior being tested, not the steps                                    |
| **Covers the negative**| Happy path + at least one alternative or failure path per AC (if applicable)                |

**Common BDD Anti-patterns:**
- ✗ `Given I open the browser` — meaningless precondition
- ✗ `When I fill in all fields and click Submit` — bundled action
- ✗ `Then the system works` — not observable
- ✗ Scenario title = "Test create product" — describes action, not behavior
- ✗ Multiple Thens that each belong to different ACs — should be split

---

## OUTPUT — Strict Order

### Section 1: Assumptions & Gaps

| #  | Item | Type | Notes |
|----|------|------|-------|
| 1  | ...  | `[Needs Clarification]` / `[Assumed]` / `[Explicit]` | ... |

---

### Section 2: AC Quality Review

For each AC:

```
#### AC-X: [AC title or summary]

| Criterion      | Status | Notes |
|----------------|--------|-------|
| Testable       | ✅ / ⚠️ / ❌ | ... |
| Atomic         | ✅ / ⚠️ / ❌ | ... |
| Unambiguous    | ✅ / ⚠️ / ❌ | ... |
| Bounded        | ✅ / ⚠️ / ❌ | ... |
| Actor-aware    | ✅ / ⚠️ / ❌ | ... |
| Result-clear   | ✅ / ⚠️ / ❌ | ... |
| In-scope       | ✅ / ⚠️ / ❌ | ... |

**Overall:** ✅ Good | ⚠️ Needs Improvement | ❌ Rewrite Required

**Issues:**
- [Specific issue with suggestion]

**Suggested fix (if needed):**
> [Rewritten AC or targeted fix — only if ⚠️ or ❌]
```

---

### Section 3: AC Completeness Review

Answer these questions for the full set of ACs:

| Question | Status | Notes |
|----------|--------|-------|
| Do ACs cover the Happy Path end-to-end? | ✅ / ⚠️ / ❌ | ... |
| Do ACs cover all Alternative Flows stated or implied by the story? | ✅ / ⚠️ / ❌ | ... |
| Do ACs cover Negative / Error scenarios? | ✅ / ⚠️ / ❌ | ... |
| Do ACs cover all Business Rules listed? | ✅ / ⚠️ / ❌ | ... |
| Do ACs cover all steps/branches in the Flow? | ✅ / ⚠️ / ❌ | ... |
| Are there duplicate or overlapping ACs? | ✅ None / ⚠️ Some | ... |
| Are there ACs that belong to a different story? | ✅ None / ⚠️ Found | ... |

**Missing AC candidates (if any):**

| # | Missing Behavior | Reason / Source |
|---|------------------|-----------------|
| 1 | [What is not covered] | [Which BR / Flow step / implied behavior] |

---

### Section 4: BDD Quality Review

For each BDD Scenario:

```
#### BDD-X: [Scenario title]

**Maps to AC:** AC-X

| Criterion          | Status | Notes |
|--------------------|--------|-------|
| Maps to one AC     | ✅ / ⚠️ / ❌ | ... |
| Given meaningful   | ✅ / ⚠️ / ❌ | ... |
| When is one action | ✅ / ⚠️ / ❌ | ... |
| Then is observable | ✅ / ⚠️ / ❌ | ... |
| No UI leakage      | ✅ / ⚠️ / ❌ | ... |
| Title is clear     | ✅ / ⚠️ / ❌ | ... |

**Overall:** ✅ Good | ⚠️ Needs Improvement | ❌ Rewrite Required

**Issues:**
- [Specific issue]

**Suggested fix (if needed):**
> [Rewritten scenario or targeted fix]
```

---

### Section 5: BDD Coverage Matrix

| AC ID | AC Summary | BDD Scenario(s) | Happy Path | Negative / Alt Flow | Coverage Status |
|-------|------------|-----------------|------------|----------------------|-----------------|
| AC-1  | ...        | BDD-1, BDD-2    | ✅          | ✅                   | ✅ Full          |
| AC-2  | ...        | BDD-3           | ✅          | ❌ Missing           | ⚠️ Partial       |
| AC-3  | ...        | —               | ❌          | ❌                   | ❌ Not Covered   |

**Coverage Status Legend:**
- ✅ Full — AC is covered by at least one happy path + relevant negative/alt BDD
- ⚠️ Partial — AC has some coverage but missing scenarios
- ❌ Not Covered — No BDD scenario maps to this AC

**Missing BDD candidates (if any):**

| # | AC ID | Missing Scenario | Type |
|---|-------|-----------------|------|
| 1 | AC-X  | [Describe the scenario needed] | Negative / Alt Flow / Edge Case |

---

### Section 6: Overall Summary

| Area | Status | Key Issues |
|------|--------|------------|
| AC Quality | ✅ / ⚠️ / ❌ | [Summary] |
| AC Completeness | ✅ / ⚠️ / ❌ | [Summary] |
| BDD Quality | ✅ / ⚠️ / ❌ | [Summary] |
| BDD Coverage | ✅ / ⚠️ / ❌ | [Summary] |
| Business Rules Coverage | ✅ / ⚠️ / ❌ | [Summary] |
| Flow Coverage | ✅ / ⚠️ / ❌ | [Summary] |

**Priority Actions (what to fix first):**
1. [Most critical issue]
2. [Second priority]
3. ...

---

## OUTPUT EXAMPLE

> Short example — real output will be longer.

### Section 1: Assumptions & Gaps

| # | Item | Type | Notes |
|---|------|------|-------|
| 1 | What happens if user submits while session expired? | `[Needs Clarification]` | Not addressed in AC or flow. |
| 2 | "Active" status is the default on creation | `[Assumed]` | Implied by happy path flow but not stated in BR. |

---

### Section 2: AC Quality Review

#### AC-1: User can create a product with a name and price

| Criterion    | Status | Notes |
|--------------|--------|-------|
| Testable     | ✅ | Clear pass/fail |
| Atomic       | ⚠️ | Combines two fields — should be split or explicitly scoped |
| Unambiguous  | ✅ | |
| Bounded      | ❌ | No constraint on name length or price range |
| Actor-aware  | ✅ | |
| Result-clear | ⚠️ | Does not state what the system does after creation |
| In-scope     | ✅ | |

**Overall:** ⚠️ Needs Improvement

**Issues:**
- Missing: max name length, price range (negative? zero?)
- Missing: system response after successful creation (redirect? toast?)

**Suggested fix:**
> Given a logged-in Admin, when they submit a product creation form with a valid Name (1–255 chars) and Price (> 0), then the system saves the product with status = Active and displays a success toast.

---

### Section 3: AC Completeness Review

| Question | Status | Notes |
|----------|--------|-------|
| Do ACs cover the Happy Path? | ✅ | AC-1 covers it |
| Do ACs cover Alternative Flows? | ⚠️ | Flow shows a "Draft" save option — no AC for it |
| Do ACs cover Negative Scenarios? | ❌ | No AC for empty name, invalid price, duplicate SKU |
| Do ACs cover all BRs? | ⚠️ | BR-3 (unique SKU) has no corresponding AC |
| Are there duplicate ACs? | ✅ None | |

**Missing AC candidates:**

| # | Missing Behavior | Source |
|---|-----------------|--------|
| 1 | Reject product creation if name is empty | BR-2, implied by form validation flow |
| 2 | Reject duplicate SKU | BR-3 |
| 3 | Allow saving as Draft | Flow step 4b |

---

### Section 4: BDD Quality Review

#### BDD-1: Successfully create a product

**Maps to AC:** AC-1

| Criterion          | Status | Notes |
|--------------------|--------|-------|
| Maps to one AC     | ✅ | |
| Given meaningful   | ⚠️ | "Given I am on the product page" — too shallow, missing role/data context |
| When is one action | ✅ | |
| Then is observable | ⚠️ | "Then the product is created" — need UI/system confirmation |
| No UI leakage      | ✅ | |
| Title is clear     | ✅ | |

**Overall:** ⚠️ Needs Improvement

**Suggested fix:**
```gherkin
Given an Admin user is logged in and no product named "Test Widget" exists
When they submit the product creation form with Name = "Test Widget" and Price = 99.99
Then the system saves the product with status = "Active"
And a success toast "Product created successfully" is displayed
```

---

### Section 5: BDD Coverage Matrix

| AC ID | AC Summary | BDD Scenario(s) | Happy Path | Negative | Coverage |
|-------|-----------|-----------------|------------|----------|----------|
| AC-1  | Create product with name + price | BDD-1 | ✅ | ❌ | ⚠️ Partial |
| AC-2  | Reject empty name | — | ❌ | ❌ | ❌ Not Covered |

**Missing BDD candidates:**

| # | AC | Missing Scenario | Type |
|---|-----|-----------------|------|
| 1 | AC-1 | Create product fails when name is empty | Negative |
| 2 | AC-2 | (needs AC-2 to be written first) | — |

---

### Section 6: Overall Summary

| Area | Status | Key Issues |
|------|--------|------------|
| AC Quality | ⚠️ | AC-1 missing bounds; AC-2 missing result |
| AC Completeness | ❌ | No negative ACs; Draft flow not covered |
| BDD Quality | ⚠️ | Weak Given/Then in BDD-1 |
| BDD Coverage | ❌ | AC-2 has zero BDD coverage |
| Business Rules Coverage | ⚠️ | BR-3 not mapped to any AC or BDD |
| Flow Coverage | ⚠️ | Step 4b (Draft save) has no AC/BDD |

**Priority Actions:**
1. Add ACs for negative scenarios (empty name, invalid price, duplicate SKU)
2. Add AC + BDD for Draft save flow (Flow step 4b)
3. Tighten AC-1: add bounds for name length and price range
4. Rewrite BDD-1 Given/Then to be more specific and observable
5. Map BR-3 to an AC

---

## END OF PROMPT
