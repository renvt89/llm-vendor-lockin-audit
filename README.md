# LLM Lock-in Scan (GitHub Action)

**Answer "Which LLM providers does your code call?" on a vendor-risk or security questionnaire with evidence from your own repo, not from memory.** This free Action lists every LLM SDK import and hard-coded model name on each pull request, so the list stays current. It is a starting list for review, not an audit or a certification.

A read-only, heuristic scan that lists:

- LLM provider SDK imports and requires (openai, anthropic, google-genai / generative-ai, vertexai, cohere, mistralai, groq, together, replicate, Bedrock client, LangChain provider packages)
- hard-coded model names (gpt-*, o1/o3/o4, claude-*, gemini-*, mistral-*, llama*, command-r*, deepseek-*)

It prints a short summary to the job log.

The Action does not change files. It does not upload your code and makes no network calls. It only runs `grep` on the checked-out workspace.

## Usage

```yaml
name: lock-in-scan
on: [pull_request, workflow_dispatch]
jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: renvt89/llm-vendor-lockin-audit@v1
        with:
          path: .   # optional, default is the repo root
```

## Honest limits

This is pattern matching, not a parser:

- It can miss indirect usage, such as wrappers, dynamic imports or model names built at runtime.
- It can flag false positives, such as comments, docs strings or unrelated packages with similar names.
- It is not an audit, a certification or a compliance attestation.
- Treat the output as a starting list to review by hand.

## Need a written report?

The paid CLI is a separate product. It scores portability from 0 to 100, writes a per-file migration checklist and includes a refactor playbook.

Full written audit: https://renevibe76.gumroad.com/l/vdmrtn?ref=gh-mkt

License: MIT
