# Glossary

This document defines the key terms used throughout the Support OS project.

---

## AI Agent

An AI-powered assistant that answers customer queries using the organization's knowledge base before escalating to a human support agent.

---

## Audit Log

A chronological record of important actions performed in the system, such as user login, ticket updates, role changes, and document modifications.

---

## Customer

An end user who interacts with an organization by creating support tickets, chatting with AI, or communicating with support agents.

---

## Embedding

A numerical vector representation of a document or text used for semantic search in the AI knowledge base.

---

## Knowledge Base

A collection of articles, FAQs, manuals, and documentation that AI and support agents use to answer customer questions.

---

## Organization

A company or business that uses Support OS to manage its customer support operations.

An organization can have:
- Multiple teams
- Multiple users
- Multiple support agents
- Multiple customers

---

## Permission

A specific action a user is allowed to perform, such as creating tickets, deleting users, or managing documents.

---

## RAG (Retrieval-Augmented Generation)

An AI architecture where relevant documents are retrieved from the knowledge base and provided as context to a Large Language Model before generating a response.

---

## Role

A predefined set of permissions assigned to users.

Examples:
- Owner
- Manager
- Support Agent
- Customer

---

## Team

A group of users within an organization responsible for handling a specific category of support requests.

Examples:
- Billing
- Technical Support
- Sales
- Returns

---

## Ticket

A support request created by a customer to report an issue, ask a question, or request assistance.

Each ticket has:
- Status
- Priority
- Category
- Assigned Agent
- Messages
- Attachments

---

## Ticket Category

A classification used to organize tickets.

Examples:
- Billing
- Refund
- Technical Issue
- Account Access
- Feature Request

---

## Ticket Priority

Defines how urgently a ticket should be handled.

Levels:
- Low
- Medium
- High
- Critical

---

## Ticket Status

Represents the current stage of a support request.

Possible statuses:
- Open
- In Progress
- Waiting for Customer
- Resolved
- Closed

---

## User

A person with access to the Support OS platform.

Examples:
- Owner
- Manager
- Support Agent

Customers are not considered internal users of the platform.

---

## Vector Database

A specialized database that stores embeddings and enables semantic similarity searches for AI-powered document retrieval.

---

## Workspace

The isolated environment where an organization manages its users, tickets, settings, and knowledge base.

Each organization has one workspace.

---

## Workflow

A predefined sequence of actions performed to complete a business process, such as ticket creation, assignment, resolution, and closure.