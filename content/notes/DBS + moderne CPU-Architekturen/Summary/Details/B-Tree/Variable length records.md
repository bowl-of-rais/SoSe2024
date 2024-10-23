> problem: tree can degenerate (separator space consumption/fanout)

## When bulkloading

- pick smallest value as separator -> resulting group sizes within constant factor
- difficult tho

## Minimal
= modify split logic

1. overflow: build sorted list of all values
2. separator = smallest within 20% of median (as close to median as possible)

can still degenerate tho :c

