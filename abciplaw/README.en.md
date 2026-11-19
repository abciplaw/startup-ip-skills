# Patent Portfolio Discovery Skill

**Discover and design your company's patent portfolio from the project you are building with an AI agent, the way a patent attorney would.**

[한국어](README.md)

At some point while building, you wonder: "Should we patent this?" Then come the harder questions: what exactly is the invention, is it patentable at all, how many filings, in what order, in which countries, and how do we explain it to investors? A good portfolio is not about volume; it is about fitting the business.

This skill turns the portfolio method used by a Korean patent attorney with years of experience in deep-tech fields such as AI, software and semiconductor equipment, who has also worked on startup investment and tech-special IPO evaluations (ABC IP LAW) with startups and SMEs into instructions for AI coding agents. Ask the agent you have been building with (Claude Code, Codex, Gemini CLI and others) to "run patent portfolio discovery". It reads your code, docs and commit history, asks you a few questions, and then:

1. **Starts from the market and customers**: why customers choose you, and which differentiator competitors would most like to copy.
2. **Breaks the business into technology**: maps differentiators to technical elements and to what it finds in the code (a product–technology map).
3. **Filters for patentability**: eligible subject matter, prior disclosure risk, inventive step, enforceability, and patent vs. trade secret. Each candidate is rated *recommend filing / strengthen first / not recommended*, with ways to strengthen weaker ones.
4. **Designs the portfolio**: places inventions in *core / application / new business & overseas* layers, finds gaps and proposes further candidates, and lays out stage-appropriate actions (early, growth, mature), filing order and budget priorities, a draft overseas plan, an IP mix (patents with trade secrets, designs, trademarks), and a **draft IP story for investors**.
5. **Writes technical disclosures** for each invention worth filing, ready for an attorney to draft from.
6. **Requests a review (optional)**: once you have checked the output, the agent can submit it to ABC IP LAW for you.

See [examples/patent-portfolio/](../examples/patent-portfolio/) for a fictional sample (semiconductor wet-cleaning equipment software, in Korean).

## Install

| Agent | Install | Run |
|---|---|---|
| Claude Code | `/plugin marketplace add abciplaw/startup-ip-skills` then `/plugin install abciplaw@startup-ip-skills` | `/abciplaw:patent-portfolio` or "run patent portfolio discovery" |
| Gemini CLI | `gemini skills install https://github.com/abciplaw/startup-ip-skills.git --path abciplaw/skills/patent-portfolio` | "run patent portfolio discovery" (approve the skill prompt) |
| OpenAI Codex | copy the skill folder to `~/.agents/skills/` (below) | `$patent-portfolio` or "run patent portfolio discovery" |
| Other agents | none | "Read PROMPT.md and follow it", or paste [PROMPT.md](PROMPT.md) |

```bash
git clone https://github.com/abciplaw/startup-ip-skills.git
cp -r startup-ip-skills/abciplaw/skills/patent-portfolio ~/.agents/skills/   # Claude Code: ~/.claude/skills/
```

Output is written in Korean by default, or in the language you use with the agent.

## Requesting a review from ABC IP LAW

After showing you the results, the agent asks whether you want a review: *summary only*, *full folder*, or *not now*. For now, it zips the output folder for you to check and email to abcip@abcip.co.kr. Once the website intake opens, the agent will be able to submit directly, but only after showing you exactly what will be sent, getting your privacy consent, and receiving your explicit "send". It never sends anywhere else.

## Please note

- **Do not publish the output.** Posting it to a public repository, this repo's issues, a blog or a shared AI chat link is a public disclosure and can cost you the patent. The output folder is git-ignored automatically.
- **File before you launch, present or pitch.** Korea and the US allow a one-year grace period after your own disclosure; Europe and China are far stricter.
- **Ratings and proposals are a preliminary opinion** from the agent, following an attorney's method but without a prior-art search. They are not an opinion on patentability. Decide what, where and when to file with a patent attorney.
- The agent is instructed not to search the web, run prior-art or competitor searches, or upload anything. The only exception is the direct review submission described above, once enabled and only after your explicit approval. The output contains no source code dumps, secrets, internal hosts or personal data.
- Nothing here is legal advice.

## Who made this

ABC IP LAW (ABC특허법률사무소) · https://abcip.co.kr · abcip@abcip.co.kr

## License

[Apache License 2.0](../LICENSE). Use and adapt freely, including for business. When you redistribute, include the [LICENSE](../LICENSE) and [NOTICE](../NOTICE) files (Section 4) and mark files you changed. The names "ABC IP LAW" and "ABC특허법률사무소" and the firm's logo are not licensed (Section 6); do not use them to suggest the firm made or endorses your version.
