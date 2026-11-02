# AI Agent Integration Experiment

This document summarizes a practical experiment in which we integrated an AI assistant (EPAM CodeMie) with GitHub to perform read operations, review repository content, and (with explicit human approval) create a new file via the GitHub Contents API.

## 1. Objective

Our goal was to validate that an AI-assisted agent can connect to a repository, read content, and deliver actionable feedback without manual copy/paste — while also verifying that repository modification is feasible and controllable when humans approve it.

Specifically, we wanted to:
- confirm repository access and permissions,
- review a research document directly from the repository, and
- create a new Markdown file via the GitHub Contents API to document the experiment itself.

## 2. Tools and Technologies

- **EPAM CodeMie** – the AI assistant used to interact with GitHub and provide analysis and recommendations.
- **GitHub** – the repository being accessed and updated.
- **GitHub API** — GitHub REST API endpoints used to read repository metadata, list directory contents, fetch files, and create a new file via the Contents API.
- **Personal Access Token (PAT)** — used for authentication to the GitHub API.
 Credentials were never written into the repository.
  - Note: **PATs** are one of several credential options; the important point is that access must be explicit and auditable.
- **VCS (toolkit)** – The CodeMie toolkit/utility used to interact with GitHub and the GitHub API (read and write operations). (In parallel, developers can still use local Git workflows for clone/branch/review.)

## 3. Integration Setup

The GitHub integration was configured so that CodeMie could call the GitHub REST API for repository metadata and file content.

Credentials were handled via an automatic credentials lookup mechanism (e.g., a secure secrets store or environment configuration), so that the token was not embedded in code or in Markdown documents.

The key principle was: the agent can use credentials, but the repository never stores them.


## 4. Experiment

### 4.1. Read-only repository connectivity test

We used the agent to verify it could access the repository and list the root-level contents via the GitHub API.

We also checked the permission flags returned by the API to confirm whether read/write access was available for the authenticated context.


### 4.2. AI-assisted document review

Next, we asked the agent to review the file `research/genai_applications.md` directly from the repository and evaluate it against a set of quality criteria (structure, technical accuracy, English, risk/limitations, and improvement opportunities).

The result was a concise review with specific, actionable recommendations (e.g., adding cross-cutting sections for evaluation and governance, and making references more consistent).

### 4.3. Repository modification test (GitHub Contents API)

We also tested writing to the repository by creating this file (`agents/agent_experiment.md`) via the GitHub Contents API.

- **First attempt**: the create request failed with the GitHub API error `422 – content is not valid Base64`.
  - Diagnosis: the Contents API requires the `content` field to be valid Base64-encoded UTF-8 file content. Sending annotated/incorrectly encoded payloads will be rejected.
- **Correction**: we re-encoded the Markdown content as clean Base64 (UTF-8), and retried the request.

Before performing the write operation, we explicitly requested and received human approval to commit directly to `main `.

The second attempt succeeded and created the file on `main ` with commit ``73fe188c93fcaec97bc0a63108ee399174789a7b``.
 This commit added ``agents/agent_experiment.md` to the default branch.

We then verified the file exists in the repository by fetching it again via the Contents API and confirming the GitHub UI URL resolves.

## 5. Results

The agent successfully:
 - Accessed the repository metadata and confirmed that it was reachable via the GitHub API.
- Listed the root-level files and directories to validate repository contents visibility.
- Read the permissions indicated by the API and reported that read and write access were available for the authenticated context.
 
- Fetched and analyzed `research/genai_applications.md` and provided a criteria-based review with concrete improvement steps.
- Attempted to create this file via the Contents API, encountered a schema/encoding error (HTTP 422) and then successfully completed the write after correcting the Base64 encoding and getting explicit human approval to commit to `main `.


## 6. Security Considerations

- **Repository-scoped access**: the experiment was designed to limit access to this repository only, rather than granting broader access to an account or organization.
 
- **Minimal permissions**: even when write permissions exist, safe agentic workflows should default to read-only operations and require explicit human approval before any changes are made.
- **Token hygiene**: access tokens must never be stored in the repository (even in private repos), because they can be leaked via commit history, logs, or accidental sharing. Use secrets managers and rotate tokens when needed.

## 7. Conclusion

Integrating an AI agent with GitHub makes it possible to automate low-risk read tasks (connectivity checks, content lookups, document reviews) and, when appropriate, to assist with controlled write operations.

This experiment demonstrated both sides: CodeMie could read and analyze repository content, and it could create a new file via the GitHub Contents API after a clear, human-approved decision and a correct authenticated payload.

The 422 encoding error was a useful reminder that agentic workflows must include validation and guardrails (format, encoding, and policy) to keep changes both safe and repeatable.
