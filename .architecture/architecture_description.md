# ARCHITECTURE DESCRIPTION

The response must provide one or more architectural elements, adhering to this format.


# PROBLEM STATEMENT

## Objective

  **System:** [Description of what system is being designed]

  **Users:** [Description of who it serves/uses it]

  **Primary outcomes**

    + [what it must enable]

## Scope boundaries

**In scope**

  + [items]

**Out of scope**

  + [items]

## Assumptions

  + [assumptions]

# Architectural elements

Repeat & fill this template as needed. Follow the numbering convention.

## 10 — [Element name]

### Description

  **Category:** [client / domain service / orchestration / identity and access / data persistence / messaging / external integration / observability / other]

  **Purpose:** [why this exists]

  **Responsibilities**

    + [responsibilities]

### Interfaces

#### Incoming

  + [requests / commands / events / user actions / upstream inputs]

#### Outgoing

  + [responses / commands / events / downstream outputs]

#### Data & state:

  + [what data or state this element owns, reads, writes, persists, or exposes]

### Interactions

+ **User-facing**
  + [if applicable]

+ **Internal synchronous**
  + [if applicable]

+ **Internal asynchronous**
  + [if applicable]

### Security

  + [authentication / authorization / trust boundary / sensitive data notes]

### Observability

  + [logging / monitoring / auditability / failure visibility / admin concerns]

### Dependencies

  + [Element IDs]

### Constraints / notes:

  + [important remarks]

### Principal alternative (optional)

  + 1-2 sentences. Do not systematically include with all elements. Only when there are strong reasons to consider 2 options as very close, describe your 2nd best choice here.


# System interaction summary
## Primary request / control paths:
  + [items]
## Main data flows:
  + [items]
## Main event flows:
  + [items]


# System-wide concerns
## Security and access control:
  + [items]
## Reliability and recovery:
  + [items]
## Observability and operations:
  + [items]
## Performance and scalability:
  + [items]
## Compliance / audit / governance:
  + [items if relevant]

# Open questions
- [question requiring architectural decision]
