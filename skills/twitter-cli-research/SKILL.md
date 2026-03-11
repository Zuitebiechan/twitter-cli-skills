---
name: twitter-cli-research
description: Use twitter-cli as a structured Twitter/X data source for research, summarization, monitoring, and feedback analysis. Use when the user wants to search X, inspect tweets or users, review bookmarks or feeds, or summarize Twitter/X discussions in a reproducible way.
argument-hint: [research goal, topic, handle, or tweet URL]
allowed-tools: Bash(twitter *), Read, Grep, Glob
---

# Twitter CLI Research Skill

Use `twitter-cli` as a structured, command-line interface to Twitter/X data.

This skill is for **read-only research and analysis workflows**. Treat `twitter-cli` as a structured data source that is easier for an agent to query, filter, summarize, save, and compare than the X web UI.

## When to use this skill

Use this skill when the user wants to:

- search Twitter/X for discussion on a topic, keyword, product, company, event, or person
- inspect a specific tweet and optionally review replies
- inspect a user profile and recent tweets
- review bookmarks or feed content for summarization
- collect Twitter/X data into files for later analysis
- generate structured summaries, reports, notes, or research artifacts from Twitter/X content

Do not use this skill when:

- the task is primarily visual browsing or UI exploration
- the task requires web-only interactions that `twitter-cli` cannot provide
- the user wants to perform write actions unless they explicitly authorize them
- the user has not installed or configured `twitter-cli` and the task cannot proceed without it

## Preconditions

Before relying on this skill, assume or verify the following when relevant:

- `twitter-cli` is installed and the `twitter` command is available
- the user is authenticated through browser cookies or environment variables
- the task is compatible with read-only Twitter/X research

If the command is unavailable or authentication fails, explain the issue clearly and suggest the next step rather than repeatedly retrying.

## Operating principles

1. **Prefer structured output**
   - Default to `--yaml` unless the user explicitly requests JSON.
   - If output will be analyzed further, save it to a file before summarizing when practical.

2. **Start small**
   - Default to `--max 20` for exploratory tasks.
   - Use `--max 30` to `--max 50` only when the task clearly benefits from broader coverage.
   - Avoid pulling very large result sets unless the user explicitly asks.

3. **Prefer read-only commands**
   - Use read commands such as `search`, `tweet`, `user`, `user-posts`, `feed`, and `bookmarks`.
   - Do not use write commands such as `post`, `delete`, `like`, `retweet`, `bookmark`, `follow`, or similar unless the user explicitly asks for that action.

4. **Narrow before deepening**
   - For broad research, start with `search`.
   - For person-centric analysis, start with `user`, then optionally `user-posts`.
   - For thread-centric analysis, use `tweet <id-or-url>`.

5. **Reduce noise before reasoning**
   - Prefer smaller, more relevant result sets over dumping a large amount of raw content into context.
   - If results are noisy, refine the query or reduce the count before summarizing.

6. **Be explicit about limitations**
   - `twitter-cli` depends on browser cookies or environment-based authentication.
   - Upstream Twitter/X private APIs may change.
   - If authentication or upstream queries fail, explain the issue clearly and suggest the next step.

## Default workflow

When using this skill, follow this workflow unless the user asks for something more specific:

1. Determine the research target:
   - topic / keyword
   - handle / account
   - tweet URL or tweet ID
   - bookmarks
   - home feed

2. Choose the smallest appropriate command.

3. Prefer structured output:
   - use `--yaml`
   - use a conservative `--max`

4. If the output is large or will be reused:
   - write it to a file first
   - then read or summarize from that saved artifact

5. Summarize with clear distinctions:
   - what was observed
   - which themes were common
   - what was high-signal vs low-signal
   - any caveats from sample size or query scope

## Command selection guide

### Topic / keyword research
Use search for broad discussion discovery.

Preferred patterns:
- `twitter search "<query>" -t Top --max 20 --yaml`
- `twitter search "<query>" -t Latest --max 20 --yaml`

Heuristic:
- Use `Top` for representative or influential discussion
- Use `Latest` for current reactions or emerging issues

### User inspection
Use:
- `twitter user <handle> --yaml`
- `twitter user-posts <handle> --max 20 --yaml`

Heuristic:
- Start with `twitter user` to understand who the account is
- Use `user-posts` only if the user wants recent output, themes, or position analysis

### Tweet inspection
Use:
- `twitter tweet <tweet-id-or-url> --yaml`

Use this when the user wants:
- the content of a specific tweet
- tweet context
- replies or thread-level inspection

### Feed review
Use:
- `twitter feed --max 20 --yaml`
- `twitter feed -t following --max 20 --yaml`

Only use feed review when the user explicitly wants home-timeline sampling or content triage.

### Bookmark review
Use:
- `twitter bookmarks --max 20 --yaml`

Use for summarizing saved content, extracting themes, or turning bookmarks into notes.

## Output handling rules

- Prefer YAML unless strict JSON is required.
- If the user asks for a summary only, you may summarize directly from command output.
- If the task is iterative, comparative, or likely to continue, save output to a file first.
- When saving outputs, choose filenames that reflect:
  - source
  - query or handle
  - approximate date or scope

Examples:
- `tmp/twitter-search-claude-code.yaml`
- `tmp/twitter-user-openai-posts.yaml`
- `notes/twitter-feedback-agent-tooling.yaml`

## Error handling

If you encounter authentication or runtime issues:

### Missing cookies
If output indicates no Twitter cookies were found:
- explain that `twitter-cli` requires either browser cookies or the `TWITTER_AUTH_TOKEN` and `TWITTER_CT0` environment variables
- suggest confirming the user is logged into x.com in a supported browser

### Invalid or expired cookies
If output indicates cookie expiration or invalid auth:
- explain that the user should re-login to x.com and retry

### Query instability or upstream failures
If Twitter/X returns 404 or another upstream API error:
- explain that private GraphQL query IDs may rotate
- retry once if appropriate
- otherwise report the limitation clearly

### Oversized outputs
If results are too large or noisy:
- lower `--max`
- narrow the query
- switch between `Top` and `Latest`
- inspect a smaller subset before summarizing

## Reporting expectations

When summarizing Twitter/X data obtained through this skill:

- separate direct observations from interpretation
- note the query and scope used
- mention the sample size
- identify recurring themes, notable examples, and outliers
- avoid overgeneralizing from a small result set

## Additional resources

Load these files when needed:
- [reference.md](reference.md) for command reference and decision rules
- [recipes.md](recipes.md) for common workflows
- `examples/` for example report shapes and output formats