# Workspaces

## Purpose

A Workspace is the highest organizational container within Theory OS. It represents an isolated knowledge environment that groups one or more related theories. A Workspace does not contain scientific knowledge itself; instead it provides the persistent structure in which theories evolve.

The purpose of a Workspace is to separate large domains of work while maintaining a consistent architecture across all projects. Examples include scientific research, software development, personal knowledge, or business strategy.

A Workspace is therefore a STORE-level object. It changes infrequently and provides stability for all lower organizational layers.

## Hierarchy

The organizational hierarchy begins with the Workspace.

Workspace
→ Theory
→ Layer
→ Branch
→ Focus
→ Claim
→ Reasoning Session

Every object below a Workspace belongs to exactly one Workspace through its parent Theory. This ensures that every piece of knowledge always exists within a well-defined context.

## Philosophy

A Workspace is not a folder.

Folders organize files.

A Workspace organizes knowledge.

Knowledge inside a Workspace forms a coherent graph whose relationships are independent of documents or conversations. Documents are outputs generated from the graph rather than the source of truth.

This distinction reflects the core architectural principles of Theory OS.

STORE provides persistence.

FLOW provides reasoning.

COMPRESSION produces abstractions.

NAVIGATION provides understanding.

The Workspace is therefore the persistent substrate upon which these processes operate.

## Scope

A Workspace defines the global scope for reasoning. It contains shared configuration, navigation state, AI integrations, global settings and all theories belonging to that domain. Users interact with multiple Workspaces in the same application, but reasoning always occurs inside one active Workspace at a time.

Example:

Workspace
Research

├── SMP Theory
├── Biology
├── AI Research
├── Software Architecture

Each Theory maintains its own internal graph while inheriting the broader organizational context of the Workspace.

## Responsibilities

The Workspace is responsible for managing global navigation, shared configuration, user permissions, AI providers, reusable templates, cross-theory references and persistent storage. It should remain lightweight and stable, while allowing the knowledge graph beneath it to evolve continuously.

## Design Principles

A Workspace should satisfy the following principles.

It contains theories, not documents.

It defines context, not content.

It persists over long periods while theories evolve within it.

It enables navigation without constraining scientific exploration.

It remains independent of any specific AI model, database implementation or user interface.

## Future Evolution

Future versions of Theory OS may allow Workspaces to synchronize across devices, share selected theories with collaborators and establish controlled relationships between independent Workspaces. These capabilities extend the organizational layer without changing its fundamental role as the highest persistent container in the system.
