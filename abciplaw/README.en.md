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
6. **Prepares a review request (optional)**: once you have checked the output, the agent zips the folder for you to email. It never sends anything itself.

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

After showing you the results, the agent asks whether you want a review. If you do, it asks for the details to include (company, contact and so on) and zips the output folder. Check it and email it to abcip@abcip.co.kr yourself; you can start with the summary file only. The agent does not send mail, upload files or contact any server.

## Please note

- **Do not publish the output.** Posting it to a public repository, this repo's issues, a blog or a shared AI chat link is a public disclosure and can cost you the patent. The output folder is git-ignored automatically.
- **File before you launch, present or pitch.** Korea and the US allow a one-year grace period after your own disclosure; Europe and China are far stricter.
- **Ratings and proposals are a preliminary opinion** from the agent, following an attorney's method but without a prior-art search. They are not an opinion on patentability. Decide what, where and when to file with a patent attorney.
- The agent is instructed not to search the web, run prior-art or competitor searches, upload anything or send mail, with no exceptions. The output contains no source code dumps, secrets, internal hosts or personal data.
- **Worried about installing it?** Read [SECURITY.md](../SECURITY.md) first. The plugin contains no executable code (text files only), and you can verify that in about five minutes before installing. Using [PROMPT.md](PROMPT.md) without installing gives the same result.
- Nothing here is legal advice.

## Invention Disclosure skill

Use this when you want **one technology** written up, without running the whole portfolio. It produces a technical disclosure you can hand to a patent attorney, or (under an NDA) to a partner, customer or investor. Ask the agent to "write a technical disclosure for this feature" (`/abciplaw:invention-disclosure` in Claude Code, `$invention-disclosure` in Codex, or have any agent read [PROMPT-invention-disclosure.md](PROMPT-invention-disclosure.md)). It

1. settles what to write about: a feature, folder, commit, document or demo file, or an invention file from a patent portfolio run;
2. reads the material and asks up to eight questions in one round (who will receive it, the prior approach and its limits, abandoned attempts, essential components, alternative implementations, measurements, disclosure status);
3. writes the disclosure: overview, the problem to solve, the core components and solution (a sub-section per component, diagrams, and a worked example with invented data), effects, alternative embodiments, and appendices with draft claim directions, drawing candidates and disclosure status;
4. keeps sources, internal names, open questions and anything withheld as a trade secret in a separate internal memo, so the disclosure itself can be shared as is;
5. optionally zips the output for you to email to ABC IP LAW.

It does not judge patentability; use the portfolio skill or an attorney for that. Before filing, share the disclosure with anyone other than your patent attorney only under an NDA.

## Trademark Pre-filing Search skill

The same plugin includes a second skill for anyone preparing a Korean trademark application, written in plain language for non-specialists. Ask the agent to "run a prior trademark search" (`/abciplaw:trademark-search` in Claude Code, `$trademark-search` in Codex, or have any agent or chat app read [PROMPT-trademark-search.md](PROMPT-trademark-search.md)). It

1. asks about the mark and the business in one round;
2. checks distinctiveness first (descriptive, generic or common terms that fail even with no prior marks);
3. builds a search plan: goods and services, Nice classes, how to look up Korean similar-group codes, 10–25 phonetic, spelling and meaning variants, and step-by-step KIPRIS instructions;
4. assesses the results **you** bring back from KIPRIS, rating each prior mark (high / caution / low) on similarity of goods and of the marks (appearance, sound, meaning), with ways around each conflict;
5. optionally zips the output for you to email to ABC IP LAW.

The agent does not search the web itself: an unfiled mark is unprotected and could be pre-empted if it leaks, and web-scraped results are hard to verify. It never invents prior marks. All conclusions are preliminary opinions, not a view on registrability.

## Who made this

ABC IP LAW (ABC특허법률사무소) · https://abcip.co.kr · abcip@abcip.co.kr

## License

[Apache License 2.0](../LICENSE). Use and adapt freely, including for business. When you redistribute, include the [LICENSE](../LICENSE) and [NOTICE](../NOTICE) files (Section 4) and mark files you changed. The names "ABC IP LAW" and "ABC특허법률사무소" and the firm's logo are not licensed (Section 6); do not use them to suggest the firm made or endorses your version.
