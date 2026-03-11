# twitter-cli reference

This file provides a practical command reference for research-oriented use of `twitter-cli`.

## Assumptions

- `twitter-cli` is installed and the `twitter` command is available.
- Authentication is already configured through browser cookies or environment variables.
- Use read-only commands by default.

## Preferred output format

Prefer:
- `--yaml`

Use JSON only when:
- the user explicitly asks for JSON
- a downstream parser strictly requires JSON

## Read-oriented commands

### Search
Search for tweets matching a query.

Examples:
- `twitter search "Claude Code" -t Top --max 20 --yaml`
- `twitter search "AI agent" -t Latest --max 30 --yaml`
- `twitter search "Model Context Protocol" --max 20 --yaml`

Products:
- `Top`
- `Latest`
- `Photos`
- `Videos`

Guidance:
- `Top` is better for representative, higher-signal posts
- `Latest` is better for current reactions or incident monitoring

### Tweet detail
Inspect a single tweet and its replies.

Examples:
- `twitter tweet 1234567890 --yaml`
- `twitter tweet https://x.com/user/status/1234567890 --yaml`

Use this when:
- the user already has a tweet URL
- the analysis is thread-centric
- you need replies or conversation context

### User profile
Get account metadata.

Example:
- `twitter user openai --yaml`

Use this before `user-posts` when the identity or relevance of the account matters.

### User posts
Fetch recent tweets from a user.

Examples:
- `twitter user-posts openai --max 20 --yaml`
- `twitter user-posts anthropicai --max 30 --yaml`

Use this when:
- the user wants recent themes
- the user wants to compare accounts
- the task is about positions, messaging, or output patterns

### Likes
Fetch liked tweets from a user.

Example:
- `twitter likes somehandle --max 20 --yaml`

Use carefully. Only do this when the user explicitly wants preference or interest analysis.

### Feed
Fetch home timeline samples.

Examples:
- `twitter feed --max 20 --yaml`
- `twitter feed -t following --max 20 --yaml`

Use only when the user specifically asks to inspect their feed.

### Bookmarks
Fetch bookmarks.

Example:
- `twitter bookmarks --max 20 --yaml`

Use for:
- summarizing saved content
- extracting notes from bookmarks
- reviewing curated reading material

### Followers / Following
Inspect graph-adjacent user lists.

Examples:
- `twitter followers openai --max 20 --yaml`
- `twitter following openai --max 20 --yaml`

Use only when relationship or audience analysis is relevant.

## Safe defaults

Use these defaults unless the task suggests otherwise:

- output: `--yaml`
- exploratory count: `--max 20`
- broader review: `--max 30`
- large review only with clear reason: `--max 50`

## Query refinement tips

When search results are noisy, try one or more of these:

- switch between `Top` and `Latest`
- narrow the query to a product, person, or event name
- add a second keyword such as:
  - `bug`
  - `feature`
  - `launch`
  - `review`
  - `pricing`
- reduce `--max` before expanding it

## File-saving pattern

When results will be reused, save them first.

Examples:
- `twitter search "Claude Code" -t Latest --max 30 --yaml > tmp/twitter-search-claude-code-latest.yaml`
- `twitter user-posts openai --max 20 --yaml > tmp/twitter-user-openai-posts.yaml`

Then summarize from the saved file or reference it in later steps.

## Avoid by default

Avoid these unless explicitly authorized by the user:

- `twitter post`
- `twitter delete`
- `twitter like`
- `twitter unlike`
- `twitter retweet`
- `twitter unretweet`
- `twitter bookmark`
- `twitter unbookmark`
- `twitter follow`
- `twitter unfollow`

These have external side effects and should not be used implicitly in research workflows.

## Common failures

### No cookies found
Likely causes:
- user not logged into x.com
- browser cookie extraction unavailable
- auth env vars not set

### Invalid cookies
Likely cause:
- session expired, user should re-login

### 404 or query errors
Likely cause:
- upstream Twitter/X query changes
- retry once, then report clearly

## Summary guidance

Good summaries should include:
- what was queried
- the number of items reviewed
- what patterns were common
- which items stood out
- whether the sample is broad enough to support the conclusion