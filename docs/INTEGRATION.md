# Rule_Graph Integration Guide

## Consumer pattern: pin a constitution

A downstream contract can store:

```text
rulebook_id
expected_rule_graph_hash
```

Before a sensitive transition, it calls Rule_Graph:

```python
rule_graph = IRule_Graph(rule_graph_address)
valid = rule_graph.view().is_consistent_for(rulebook_id, expected_rule_graph_hash)
```

If `valid` is false, the consumer can fail closed because either the rulebook is inconsistent or active rule_graphical state changed since the consumer pinned it. For a resolved-conflict rule_graph, also inspect `rule_graph_status` and `get_rule_graph_relations` before executing an action in an affected scope.

## Consumer pattern: inspect current rule_graph

`get_rule_graph(rulebook_id)` returns active rules with normalized semantics and priority.

`get_rule_graph_relations(rulebook_id)` returns the bounded active-active graph,
including relation kind, semantic hashes, and deterministic conflict resolution.
Consumers must read this view when resolved conflicts are meaningful to execution.

`rule_graph_status(rulebook_id)` makes the distinction explicit:

- `COHERENT`: no active conflict or ambiguity;
- `RESOLVED_CONFLICTS`: conflicts exist, but every active conflict has deterministic precedence;
- `UNRESOLVED`: at least one active conflict has no deterministic winner;
- `AMBIGUOUS`: at least one active relation is semantically ambiguous.

`consistent=true` means no unresolved or ambiguous active relation. It does not
mean that no conflict edge exists.

A consumer can use this as trusted shared context for a separate adjudication contract without repeating normalization work.

## Consumer pattern: inspect a conflict

`relation_between(left_rule_id, right_rule_id)` exposes semantic relation, conflict subtype, overlap description, deterministic resolution, and semantic hashes used by the edge.

This is useful for governance tooling or another Intelligent Contract deciding whether a candidate action enters a disputed part of the rule graph.

## Ownership composition

The rulebook owner is an address. It can be a normal account or another contract-controlled address depending on the governance architecture.

Rule_Graph itself does not implement voting. It is intentionally a primitive for semantic consistency and rule_graph state.

## Recommended integration invariant

For high-stakes consumers, pin all of:

1. Rule_Graph contract address;
2. rulebook ID;
3. expected rule_graph hash.

Do not trust only a human-readable rulebook name.

## Version handling

`revision` changes for any persisted rulebook governance change.

`rule_graph_version` changes only when active rule_graphical state changes.

Consumers interested only in operative rules should track `rule_graph_version` and `rule_graph_hash`. Audit systems interested in failed or blocked proposals may also track `revision`.
