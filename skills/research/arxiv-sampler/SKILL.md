---
name: arxiv-sampler
description: A specialist skill for searching, retrieving, and summarizing academic papers from ArXiv. It helps researcher agents stay updated with the latest AI and Tech breakthroughs.
version: 1.0.0
author: community
license: MIT
metadata:
  hermes:
    tags: [Research, ArXiv, Academia, AI, PDF]
---

# ArXiv Research Assistant

Deep research into academic papers. This skill enables the agent to search ArXiv for specific topics, fetch metadata, and summarize findings.

## Capabilities

- **Topic Search**: Find the latest papers on any technical subject.
- **Abstract Analysis**: Quickly summarize dozens of papers to find the relevant ones.
- **Deep Dive**: Prepare a structured report of a specific field's current state.

## Usage Examples

Ask the agent:
- "Find the top 5 most recent papers about 'Agentic AI' on ArXiv."
- "Summarize the key contributions of the paper with ID 2401.00001."
- "Create a research report on the current state of 'Multi-Agent Systems' based on ArXiv papers from this month."

## Tools and Workflow

The agent uses `web_search` (configured for ArXiv) and `web_extract` to pull data.

### Search Query Pattern
```
https://arxiv.org/search/?query={search_term}&searchtype=all&source=header
```

### PDF Fetching (if needed)
```
https://arxiv.org/pdf/{arxiv_id}.pdf
```

## Tips for the Agent
- Always include the ArXiv ID and a direct link in reports.
- Look for "Subjects" and "Primary Category" to categorize the papers.
- Summarize the *Results* and *Conclusion* specifically for maximum value.
