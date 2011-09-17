# Turing Machine Optimizations

## Prune unreachable states

The most basic optimization is to remove any rules which depend on an unreachable state. For a given rule, if the condition state is neither the entry state for the program nor the target state of any reachable rule, the rule can be pruned without changing the behavior of the program.

## Remove functionally equivalent states

If all rules for a pair of condition states are identical, aside from the condition state itself, then one may be removed and all references to the removed state may be replaced with references to the remaining state without changing the behavior of the program.

## Remove "state change runs"

In situations in which "no-move" operations occur (usually transitory, during optimization), The rule 

> (condition state, condition symbol) -> (written symbol, no seek, destination state)

given

> (destination state, written symbol) -> (another written symbol, seek instruction, another destination state)

can be replaced with

> (condition state, condition symbol) -> (another written symbol, seek instruction, another destination state)

## Remove "seekbacks"


