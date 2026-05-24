# COMPONENTS DESCRIPTIONS

# Parent architectural element (repeat as needed)

  **Parent:** [Parent ID] - [parent name]

  **Parent purpose:** [short summary of the parent architectural role]

## Assumptions

  + [assumptions]

## Constraints

  + [important constraints: inherited from the architecture, inherent to tech choices, etc.]

## Component breakdown (repeat as needed for this parent element)

### [ARCHITECTURAL_ELEMENT_ID].[COMPO_ID] — [Component name]

#### Description

  **Category:** [Frontend system / API service / worker / database / queue / adapter / storage / external integration / observability / other]

  **Purpose:** [why this component exists]

  **Technology choice:** [specific framework / product / runtime / service / library]

  **Responsibilities**

    + [responsibilities]

#### Interfaces

##### Incoming

  + [requests / commands / events / data]

##### Outgoing

  + [requests / commands / events / data]

##### Data / state

  + [what this component owns, persists, caches, or reads]

#### Dependencies

##### Internal

  + [other x.y components]

##### External

  + [architectural elements outside the parent scope, if relevant]

#### Observability / operational considerations

  + [logs / metrics / tracing / admin / scaling / failure visibility]

#### Constraints / notes

  + [important implementation constraints]

#### Principal alternative (optional)

  + [close second option and why it was not chosen]


## Open questions

  + [question requiring human/component-design decision]
