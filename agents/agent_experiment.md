# AI Agent Integration Experiment

This document summarizes a practical experiment in which we integrated an AI assistant (EPAM CodeMie) with GitHub to perform read-only operations and provide developer support (e.g., document reviews).

## 1. Objective

Our goal was to validate that an AI-assisted agent can connect to a repository, read content, and deliver actionable feedback without manual copy/paste.

Specifically, we wanted to:
- confirm repository access and permissions, and
- use the agent to review a research document directly from the repository.

## 2. Tools and Technologies

- **EPAM CodeMie** — AI assistant used to interact with GitHub and amalgamate results.
- **(GitHub)** – repository hosting and source of file content.
- **(GitHub API)** – used to fetch repository metadata and file content, and to read items in the repo tree.
- **(Personal Access Token (PAT))** – used for authentication to the GitHub API (credentials were never written into the repository).
  - Note: PATS are one of several credential options; the important point is that access must be explicit and auditable.
- **VCS tools** – local Git workflows (clone, pull, branches, review) in parallel with AI-assisted operations.

> This experiment did not require creating any new repository content during the tests.

## 3. Integration Setup

The GitHub integration was configured so that the agent could call the GitHub REST API to perform read operations.

Credentials were handled via an automatic credentials lookup mechanism (e.g., a secure secrets store or environment configuration), so that the token was not embedded in code or in Markdown documents.

The key principle was: the agent can use credentials, but the repository never stores them.


## 4. Experiment

### 4.1. Read-only repository connectivity test

We used the agent to verify it could access the repository and list the root-level contents via the GitHub API.

We also checked the permission flags returned by the API to confirm whether read/write access was available for the authenticated context.


### 4.2. AI-assisted document review

Next, we asked the agent to review the file `research/genai_applications.md` directly from the repository and evaluate it against a set of quality criteria (structure, technical accuracy, English, risk/limitations, and improvement opportunities).

The result was a concise review with specific, actionable recommendations (e.g., adding cross-cutting sections for evaluation and governance, and making references more consistent).

## 5. Results

The agent successfully:
- Accessed the repository metadata and confirmed that it was reachable via the GitHub API.
- Listed the root-level files and directories to validate repository contents visibility.
- Read the permissions indicated by the API and reported that read and write access were available for the authenticated context.
- Fetched and analyzed `genai_applications.md` and provided a criteria-based review with concrete improvement steps.

## 6. Security Considerations

- **Repository-scoped access**: the experiment was designed to limit access to this repository only, rather than granting broader access to an account or organization.
- **Minimal permissions**: even when write permissions exist, safe agentic workflows should default to read-only operations and require explicit approval before any changes are made.
- **Token hygiene**: access tokens must never be stored in the repository (even in private repos), because they can be leaked via commit history, logs, or accidental sharing. Use secrets managers and rotate tokens when needed.


## 7. Conclusion

Integrating an AI agent with GitHub makes it possible to automate low-risk, read-oriented tasks (connectivity checks, content lookups, and document reviews) while keeping a strong security posture.

This experiment demonstrated that CodeMie can be used as a practical partner in a developer workflow: it can quickly verify access, summarize repository structure, and provide structured feedback on documentation – without replacing human decision-making or safeguards.
