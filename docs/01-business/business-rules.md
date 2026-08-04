# Business Rules

## Overview

This document defines the business rules that govern SupportOS. These rules ensure consistent system behavior and serve as the foundation for backend validation, authorization, and workflow design.

---

# Organization Rules

## BR-001

Each organization is isolated from every other organization.

## BR-002

Users can only access data belonging to their own organization.

## BR-003

Each organization has exactly one Organization Owner.

## BR-004

An organization may have multiple administrators, managers, and support agents.

---

# User Management Rules

## BR-005

Every user must belong to an organization.

## BR-006

User email addresses must be unique across the platform.

## BR-007

Only Organization Owners and Administrators can invite new users.

## BR-008

Only authorized users can assign roles.

---

# Ticket Rules

## BR-009

Every ticket belongs to exactly one organization.

## BR-010

Every ticket has exactly one customer.

## BR-011

A ticket may be assigned to one support agent at a time.

## BR-012

Only authorized users may assign or reassign tickets.

## BR-013

Closed tickets become read-only unless reopened.

---

# Conversation Rules

## BR-014

Messages belong to exactly one ticket.

## BR-015

Messages cannot be edited after being sent.

## BR-016

Internal notes are visible only to organization members.

---

# Knowledge Base Rules

## BR-017

Knowledge documents belong to one organization.

## BR-018

AI can only access approved knowledge documents.

## BR-019

Deleting a knowledge document does not delete historical conversations.

---

# AI Rules

## BR-020

AI responses must use only organization-specific knowledge.

## BR-021

If AI confidence is below the configured threshold, the request must be assigned to a human agent.

## BR-022

AI cannot access data from another organization.

## BR-023

AI-generated replies must be clearly identified.

---

# Security Rules

## BR-024

Every authenticated request must be authorized.

## BR-025

Sensitive actions must be recorded in audit logs.

## BR-026

Passwords must never be stored in plain text.

---

# Audit Rules

## BR-027

The system must record:

- User login
- Ticket assignment
- Role changes
- Knowledge uploads
- AI actions

---

# Notifications

## BR-028

Customers receive notifications when:

- A ticket is created
- A ticket is assigned
- A reply is received
- A ticket is resolved

## BR-029

Agents receive notifications when tickets are assigned.

---

# Summary

These rules define the core operational behavior of SupportOS and will guide the implementation of authorization, validation, workflows, and backend services.