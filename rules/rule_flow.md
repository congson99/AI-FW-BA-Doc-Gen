# Flow Writing Rules

Generate the User Flow and States for a feature based on its Spec.

---

## Document Structure

1. Flow
2. States

---

## Section 1: Flow

### Format

- Start with `[Start]`
- Number each major step (1. Access, 2. Input and Submit, 3. Validation, 4. Processing, 5. Persistence, 6. Response)
- End with `[End]`
- Show branching paths with letters (1A, 1B)
- Failure branches redirect back to previous step: "→ back to **Step Name** step"

### Rules

- Keep each step short — business actions only
- Show both UI access path (1A) and direct URL path (1B) when applicable
- Show permission checks at the access step
- Show validation failure loop back to Input
- Show system error loop back to Input
- Avoid implementation details

### Standard Step Template

```
[Start]

1. Access

1A. Access via [Feature] Page (UI)
- User accesses the [Feature] Management page
- System checks [PERMISSION] permission
- If no permission → system hides the Create action → [End]
- If permission → system displays the Create action
- User clicks Create [Feature]
- System displays the Create [Feature] page

1B. Access via Direct URL
- User directly accesses the Create [Feature] page
- System checks [PERMISSION] permission
- If no permission → system shows a No Permission page → [End]
- If permission → system displays the Create [Feature] page

2. Input and Submit
- User inputs [feature] information
- User submits the creation request

3. Validation
- System trims input data before validation and persistence
- System applies validation rules
- System checks for [unique constraint]
- If validation fails → system displays validation errors (back to Input and Submit step)

4. Processing
- System prepares the [feature] data
- System sets audit fields
- If system failure → system displays error message (back to Input and Submit step)

5. Persistence
- System persists the [feature] data
- If system failure → system displays error message (back to Input and Submit step)

6. Response
- System returns a success response and redirects to [Feature] Management page

[End]
```

---

## Section 2: States

### System States
- Numbered list, one-line description each
- Standard states: Idle, Access Denied, Input Form, Submitting, Processing, Success, Validation Failed, Error

### State Transitions
- Format: `N. StateA → StateB: Trigger`
- Cover all transitions including failure paths

---

## Quality Checklist

- All AC branches are covered in the flow
- Permission checks are at access step
- Failure loops back to the correct step
- States map 1:1 to flow stages
- State transitions cover all flow branches
