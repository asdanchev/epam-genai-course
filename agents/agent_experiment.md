# AI Agent Integration Experiment

**Course:** EPAM Tech Orda — Generative AI  
**Student:** Anton Danchev  
**Assignment:** Research on Generative AI Applications  
**Topic:** AI Agent Integration with GitHub  
**Date:** November 2, 2026

---

## 1. Introduction

This report describes a practical experiment in which an AI assistant, EPAM CodeMie, was integrated with GitHub to perform repository operations, review technical documentation, create and update files, and handle API errors under human supervision.

The experiment demonstrates how AI agents can interact with external development tools and highlights the importance of reliability, security, and human oversight.

## 2. Objectives

The main objective was to explore how an AI agent can interact with external development tools and perform useful tasks with minimal manual intervention.

The specific objectives were to:

- Configure an AI assistant with access to a GitHub repository.
- Verify repository connectivity and permissions.
- Review a research document directly from the repository.
- Create a new Markdown file using the GitHub Contents API.
- Update an existing file and handle API errors.
- Evaluate the reliability and limitations of AI-assisted development workflows.

## 3. Tools and Technologies

| Tool | Purpose |
|------|---------|
| **EPAM CodeMie** | AI assistant platform for repository interaction and document analysis. |
| **GitHub** | Repository hosting and version control. |
| **GitHub REST API** | Accessing repository metadata, files, and directory contents. |
| **GitHub Contents API** | Creating and updating files in the repository. |
| **Git Data API** | Retrieving complete Git objects, including file blobs. |
| **Personal Access Token (PAT)** | Authentication and authorization for API requests. |
| **CodeMie VCS Toolkit** | Tools enabling the assistant to interact with GitHub. |
| **Git** | Local version control and repository synchronization. |

## 4. Integration Setup

We created a GitHub integration in EPAM CodeMie and configured it using a fine-grained Personal Access Token.

The token was restricted to the repository used for the experiment.

The AI assistant was configured to use the CodeMie VCS toolkit with automatic credentials lookup.

This configuration allowed the assistant to access GitHub without exposing authentication credentials in source code or Markdown documents.

**Repository:** `asdanchev/epam-genai-course`  
**Branch:** `main`  
**Research document:** `research/genai_applications.md`  
**Experiment report:** `agents/agent_experiment.md`

An important security principle was established: AI agents should use explicitly authorized credentials and require human approval before modifying repository content.

## 5. Experiment

### 5.1. Repository Connectivity Test

First, we asked the AI assistant to connect to the GitHub repository and retrieve its metadata.

The assistant successfully accessed the repository through the GitHub REST API and listed its root-level files and directories.

The API response indicated that the authenticated context had both read and write permissions.

This confirmed that the integration was configured correctly and that the assistant could interact with the repository.

### 5.2. AI-Assisted Document Review

Next, we asked the assistant to review the research document:

`research/genai_applications.md`

The assistant retrieved the document directly from GitHub and evaluated its content.

The review focused on:

- Document structure and readability.
- Technical accuracy.
- English language quality.
- Business value and practical applications.
- Technical challenges and limitations.
- Opportunities for improvement.

The assistant provided recommendations, including improvements to evaluation criteria, governance considerations, and technical references.

This demonstrated how an AI agent can assist developers with documentation reviews without requiring manual copying and pasting of repository content.

### 5.3. Repository Modification Test

We then tested whether the AI assistant could create a new file directly in the repository.

**Target file:** `agents/agent_experiment.md`

The assistant attempted to create the file using the GitHub Contents API.

However, the initial request failed with the following error:

```text
HTTP 422 — content is not valid Base64
```

The GitHub Contents API requires file content to be encoded as a valid Base64 representation of UTF-8 text.

The initial request contained an invalid payload.

After identifying the problem, we instructed the assistant to correct the encoding and retry the operation.

Before committing changes to the `main` branch, the assistant requested explicit human approval.

After receiving approval and correcting the payload, the assistant successfully created the file.

**Initial creation commit:**

```text
73fe188c93fcaec97bc0a63108ee399174789a7b
```

The assistant then verified that the file existed by performing a follow-up API request.

### 5.4. AI-Assisted Document Updates

After reviewing the generated report, we discovered several inaccuracies and formatting issues.

The document initially described only read operations and did not include the successful repository modification experiment.

We asked the assistant to update the report and document the complete workflow.

The assistant successfully performed an initial update.

**Documentation update commit:**

```text
1ed147791792b3ec783437e05e3f22d4ded31202
```

However, further attempts to correct formatting and content repeatedly failed with the same HTTP 422 Base64 error.

This revealed an additional technical limitation.

### 5.5. Tool Output Truncation and Git Data API

During the investigation, the assistant reported that the GitHub tool returned truncated file content.

The response contained a truncation marker:

```text
...[truncated]
```

As a result, the assistant could not reliably reconstruct the complete Markdown document for encoding and updating.

Attempting to encode incomplete content could result in an invalid payload or accidental loss of document content.

Instead of continuing to retry the same operation, we instructed the assistant to use an alternative approach.

The assistant retrieved the complete file through the Git Data API blob endpoint.

The response contained:

| Property | Value |
|----------|-------|
| Blob SHA | `2efcc003d3c9b82c17a9c0e3a2ebcfdab41c04cc` |
| File size | 5,801 bytes |
| Content format | Base64 |
| Truncation | Not detected |

According to the assistant's report, the blob response was complete and did not contain truncation markers.

The assistant then used the retrieved content to prepare another update.

This time, the update succeeded.

**Successful update commit:**

```text
446553cfe7df7b9b7b288dd99dcaf1fcd07a8950
```

The assistant verified the updated file through a subsequent GitHub API request.

This demonstrated the importance of selecting appropriate tools and validating data completeness before performing write operations.

### 5.6. Human Review and Final Corrections

Although the assistant successfully updated the document, the generated Markdown still contained minor formatting and language errors.

Examples included:

- Incorrect Markdown code formatting.
- Unnecessary whitespace.
- Inconsistent list indentation.
- Minor grammatical inaccuracies.

We decided to perform the final corrections manually.

This approach demonstrated a practical **Human-in-the-Loop** workflow in which an AI agent performs technical operations while a human reviews the results and ensures their quality.

## 6. Results

The experiment demonstrated several capabilities of AI-assisted development.

### 6.1. Successful Operations

| Operation | Result |
|-----------|--------|
| GitHub integration | Successful |
| Repository metadata retrieval | Successful |
| Repository permissions check | Successful |
| AI-assisted document review | Successful |
| File creation through GitHub API | Successful |
| File modification through GitHub API | Successful |
| Base64 error diagnosis | Successful |
| Alternative Git Data API retrieval | Successful |
| Human approval before writing | Implemented |
| Commit verification | Successful |

### 6.2. Identified Limitations

The experiment also revealed several limitations:

- API requests may fail because of incorrectly formatted payloads.
- Tool responses may be truncated when processing larger documents.
- AI agents may repeatedly attempt unsuccessful operations without changing their strategy.
- Generated documentation may contain formatting errors or factual inaccuracies.
- Human supervision remains necessary for reliable repository modifications.

## 7. Security Considerations

Security was an important part of the experiment.

### 7.1. Repository-Scoped Access

The Personal Access Token was configured to limit access to the repository used for the experiment.

### 7.2. Credential Protection

Authentication credentials were managed through the integration configuration and were not included in repository files.

### 7.3. Human Approval

The assistant requested explicit authorization before committing changes to the `main` branch.

### 7.4. Controlled Modifications

The assistant was instructed to modify only the specified Markdown file and avoid changing unrelated repository content.

### 7.5. Verification

Successful write operations were followed by API requests to verify that the expected files existed.

These practices help reduce risks when AI agents interact with development infrastructure.

## 8. Key Learnings

The experiment provided practical experience with several important concepts in agentic AI systems.

| Concept | Description |
|---------|-------------|
| **Tool Calling** | AI agents interact with external services through APIs and specialized tools. |
| **Error Handling** | Agents identify API errors and adjust their requests accordingly. |
| **Tool Output Truncation** | Incomplete tool responses can prevent reliable file processing. |
| **Alternative Tool Strategies** | Different API endpoints can help overcome technical limitations. |
| **Human-in-the-Loop** | Human approval and review provide additional control over agent operations. |
| **Agent Reliability** | Successful execution of one operation does not guarantee future success. |
| **Output Validation** | AI-generated content requires verification for accuracy and completeness. |

## 9. Conclusion

The experiment demonstrated that EPAM CodeMie can be integrated with GitHub to support practical software development tasks.

The AI assistant successfully accessed repository content, reviewed documentation, created files, updated existing files, and verified changes through GitHub API requests.

The experiment also revealed limitations related to Base64 encoding, truncated tool responses, repeated API failures, and documentation quality.

An important improvement was the use of the Git Data API to retrieve complete file content after the original approach encountered truncation problems.

The main conclusion is that AI agents can significantly assist developers, but their actions should remain observable, verifiable, and subject to human supervision.

**AI agents are useful development assistants, but they do not eliminate the need for human judgment, technical validation, and quality control.**