# Event Sourcing

Event sourcing is an architectural pattern where changes to application state are stored as a sequence of events. Instead of storing just the current state, the system captures all changes as immutable events, which can be used to reconstruct the state at any point in time.

## Core Concepts

### 1. Events
- **Immutable**: Once created, events never change
- **Sequential**: Events have a defined order
- **Complete**: Events contain all data needed to apply the change
- **Descriptive**: Events describe what happened, not how state changed

### 2. Event Store
- **Append-only**: New events are added, never modified
- **Persistent**: Events are stored permanently
- **Queryable**: Events can be retrieved by ID, type, or time range
- **Optimized**: Often optimized for append operations

### 3. Projections
- **Derived Views**: Current state built from events
- **Purpose-specific**: Different projections for different needs
- **Rebuildable**: Can be reconstructed from event history
- **Optimized for Queries**: Structured for efficient reading

### 4. Command-Query Responsibility Segregation (CQRS)
- **Commands**: Change state by creating events
- **Queries**: Read from optimized projections
- **Separation**: Different models for writing and reading

## Benefits

- **Complete Audit Trail**: Every change is recorded
- **Temporal Queries**: State can be reconstructed for any point in time
- **Debugging**: Easier to understand how state evolved
- **Event Replay**: System can be rebuilt from event history
- **Business Insights**: Event stream provides valuable data

## Challenges

- **Complexity**: More complex than traditional CRUD
- **Event Schema Evolution**: Handling changes to event structure
- **Eventual Consistency**: Projections may lag behind events
- **Learning Curve**: Requires different thinking about state

## Implementation Considerations

- **Event Schema Design**: Structure events for longevity
- **Versioning Strategy**: Plan for event schema evolution
- **Snapshotting**: Optimize rebuilding with periodic snapshots
- **Concurrency**: Handle concurrent commands appropriately
- **Storage**: Choose appropriate event store technology

## Examples

*Coming soon*

## Sources

- [Martin Fowler on Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [Greg Young's CQRS Documents](https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf)
- [Event Store DB](https://www.eventstore.com/eventstoredb)

[Back to Table of Contents](/README.md)