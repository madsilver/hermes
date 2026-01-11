# Contributing to Hermes

First of all, thank you for your interest in **Hermes**  
Hermes is an open-source project focused on **learning, clarity, and explicit design** in distributed systems. 
Contributions of all kinds are welcome.

This document explains **how to contribute**, **what we value**, and **how decisions are made**.

---

## Code of Conduct

Hermes follows a simple rule:

> Be respectful, constructive, and curious.

We expect all contributors to:
- Be kind and professional
- Assume good intent
- Focus on technical merit, not personal preference
- Keep discussions open and collaborative

Harassment, disrespectful behavior, or exclusionary language will not be tolerated.

---

## How You Can Contribute

There are many ways to help Hermes grow:

### Design & Architecture
- Propose or review **Architecture Decision Records (ADRs)**
- Suggest improvements to the Hermes Protocol (HP)
- Review protocol changes for clarity and backward compatibility

### Testing & Validation
- Add unit or integration tests
- Write test clients or simulators
- Validate protocol behavior under failure scenarios

### Code
- Implement missing components
- Improve readability and maintainability
- Optimize code **only when justified and documented**

### Documentation
- Improve README, ADRs, or protocol docs
- Add diagrams or examples
- Fix typos or clarify explanations

### Bug Reports
- Report incorrect behavior
- Suggest minimal reproductions
- Propose fixes if possible

---

## Guiding Principles

### 1. Explicitness over abstraction
Prefer clear state machines, explicit message handling, and simple data structures.

### 2. Clarity over cleverness
Readable code is more important than micro-optimizations or clever tricks.

### 3. Small, focused changes
Small pull requests with a single purpose are strongly preferred.

---

## Architecture Decision Records (ADRs)

All significant architectural decisions **must be documented** using ADRs.

- Create new ADRs under `docs/adr/`
- Describe context, decision, alternatives, and consequences
- Open a pull request for discussion

---

## Commit Messages

Use clear and descriptive commit messages, for example:

```
protocol: add HEARTBEAT message
server: refactor connection loop
docs: improve HP description
```

---

## Pull Requests

Before opening a PR:
- Ensure the project builds
- Keep changes focused
- Update documentation if behavior changes

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
