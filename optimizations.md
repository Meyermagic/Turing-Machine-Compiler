# Turing Machine Optimizations

In the following examples, turing machine instructions are presented as follows:

> (condition state, condition symbol) -> (written symbol, seek instruction, destination state)

## Prune unreachable states

The most basic optimization is to remove any rules which depend on an unreachable state. For a given rule, if the condition state is neither the entry state for the program nor the target state of any reachable rule, the rule can be pruned without changing the behavior of the program.

## Remove functionally equivalent states

If all rules for a pair of condition states are identical, aside from the condition state itself, then one may be removed and all references to the removed state may be replaced with references to the remaining state without changing the behavior of the program.

## Remove "no-seek" instructions

In situations in which "no-seek" operations occur (usually transitory, during optimization), the pair of instructions

```
(st_a, sy_a) -> (sy_b, no seek, st_b)
(st_b, sy_b) -> (sy_c, direction, st_c)
```

can be replaced with

> (st_a, sy_a) -> (sy_c, direction, st_c)

> (st_b, sy_b) -> (sy_c, direction, st_c)

in order to remove an unnecessary state change, and potentially enable further optimization.

## Remove "seekbacks"

If a situation exists where all the instructions for a given condition state are of the form

> (st_a, sy_a) -> (sy_a, direction, st_b)

for constant st_a, st_b, and direction across all the instructions (that is, if the instructions all seek in the same direction, end in the same state, and make no change to the tape) and an additional rule exists of the form

> (st_c, sy_c) -> (sy_d, opposite direction, st_a)

then the rule 

> (st_c, sy_c) -> (sy_d, opposite direction, st_a)

can be replaced with 

> (st_c, sy_c) -> (sy_d, no seek, st_b)

with no change to the behavior of the program. This optimization should obviously be followed by removing the resultant "no-seek" operation.
