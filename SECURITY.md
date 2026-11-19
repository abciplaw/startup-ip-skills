# Security and Confidentiality

## What the skill does on your machine

- **Reads** your project (code, documents, commit history) to find patent candidates.
- **Writes** only to `patent-portfolio/` inside your project. It puts a `.gitignore` inside that folder so the results are not committed. Other repository settings are left alone.
- **Network**: the skill tells the agent not to search the web, run prior-art or competitor searches, or upload anything. Direct submission to ABC IP LAW is disabled in this version. You send the results yourself if you want a review.
- **Never written to results**: source code longer than ten lines, passwords, keys, tokens, internal server addresses, or real customer data.

Review any skill before you install it, including this one: check what it reads and writes, which commands it runs, and where it sends data.

## Do not post your results

Do not open issues or pull requests that contain your project details or the generated files. Publishing an invention before filing can make it unpatentable.

## Reporting a problem

Open a GitHub issue only if the report contains no confidential information. Otherwise email abcip@abcip.co.kr.
