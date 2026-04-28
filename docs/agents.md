# Agents

Agents help you ask operational questions and receive clear recommendations with context.

## What agents are for

Use Agents when you need to understand:

- what is happening in your operational data
- why a change, risk, or exception matters
- what action to take next
- which evidence supports a recommendation

## Grounded answers

A grounded Annona answer should be explicit about the tenant data, evidence, and reasoning context it used. When a generic answer path is available for comparison, the product should make the difference clear so you know which answer is grounded in your uploaded data.

## Good customer prompts

Useful prompts are specific and decision-oriented, for example:

- `What should I prioritize today?`
- `Which service risks need action before the next planning window?`
- `Why did this metric change, and what should we do next?`
- `Which dataset evidence supports this recommendation?`

## Customer boundary

Agents should operate inside your tenant. They should not route you into internal admin testing screens, expose other customers' data, or ask you to choose internal model controls that are not part of the customer workflow.

## Stable help target

Use `/agents/` for in-product help from agent lists, question panels, grounded-answer views, and answer evidence sections.
