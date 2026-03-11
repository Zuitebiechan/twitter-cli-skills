# twitter-cli research recipes

Use these recipes for common Twitter/X research tasks.

## Recipe: summarize discussion on a topic

Goal:
- understand what people are saying about a topic on Twitter/X

Recommended process:
1. Search `Top` results for representative discussion
2. Search `Latest` results for recent reactions
3. Compare overlap and differences
4. Summarize themes, notable examples, and uncertainties

Suggested commands:
- `twitter search "<topic>" -t Top --max 20 --yaml`
- `twitter search "<topic>" -t Latest --max 20 --yaml`

When to save to file:
- if the user wants a report
- if you expect follow-up questions
- if you want a reproducible artifact

## Recipe: inspect a user’s public positioning

Goal:
- understand how an account presents itself and what it has been posting recently

Recommended process:
1. Inspect the profile
2. Fetch recent tweets
3. Identify repeated themes, topics, cadence, and tone
4. Note whether conclusions are based on profile metadata, tweet content, or both

Suggested commands:
- `twitter user <handle> --yaml`
- `twitter user-posts <handle> --max 20 --yaml`

## Recipe: inspect a tweet and its discussion

Goal:
- understand the content and response pattern around a specific tweet

Recommended process:
1. Fetch the tweet by URL or ID
2. Read the main tweet
3. Review replies and identify response clusters
4. Distinguish between the original claim and audience reaction

Suggested command:
- `twitter tweet <tweet-url-or-id> --yaml`

## Recipe: turn bookmarks into notes

Goal:
- convert saved tweets into a compact knowledge artifact

Recommended process:
1. Fetch a limited bookmark sample
2. Group tweets by theme
3. Extract action items, ideas, or references
4. Save the result into notes or docs

Suggested command:
- `twitter bookmarks --max 20 --yaml`

## Recipe: sample the home feed for signals

Goal:
- quickly understand what content is appearing in the user’s timeline

Recommended process:
1. Fetch a small feed sample
2. Separate likely high-signal items from casual noise
3. Identify recurring subjects, creators, and formats
4. Report that this is feed-sampling, not a comprehensive trend view

Suggested commands:
- `twitter feed --max 20 --yaml`
- `twitter feed -t following --max 20 --yaml`

## Recipe: product or brand feedback scan

Goal:
- find complaints, praise, and recurring requests related to a product or brand

Recommended process:
1. Search for the product or brand name
2. If needed, search for combinations such as:
   - `<product> bug`
   - `<product> feature`
   - `<product> broken`
   - `<product> love`
3. Keep result sets small and targeted
4. Summarize by issue type, sentiment direction, and frequency cues

Suggested commands:
- `twitter search "<product>" -t Latest --max 20 --yaml`
- `twitter search "<product> bug" -t Latest --max 20 --yaml`
- `twitter search "<product> feature" -t Latest --max 20 --yaml`

## Recipe: competitive intelligence snapshot

Goal:
- compare how multiple accounts or products are being discussed

Recommended process:
1. Inspect each account profile if needed
2. Fetch recent posts for each account
3. Optionally search by brand name for external discussion
4. Compare themes, tone, and engagement cues
5. Avoid overclaiming based on small samples

Suggested commands:
- `twitter user <handle> --yaml`
- `twitter user-posts <handle> --max 20 --yaml`
- `twitter search "<brand>" -t Top --max 20 --yaml`

## Recipe: save data first, then analyze

Goal:
- preserve a reproducible intermediate artifact

Recommended pattern:
1. Run `twitter-cli` command with `--yaml`
2. Save to a file under `tmp/`, `data/`, or `notes/`
3. Use the saved file as the analysis input for subsequent steps

Examples:
- `twitter search "Claude Code" -t Latest --max 30 --yaml > tmp/twitter-claude-code-latest.yaml`
- `twitter user-posts anthropicai --max 20 --yaml > tmp/twitter-anthropic-posts.yaml`

Use this pattern when:
- the task may continue over multiple turns
- the user wants a report artifact
- the result should be auditable or compared later

## Recipe: compare two accounts

Goal:
- compare messaging, themes, and recent activity between two accounts

Recommended process:
1. Fetch profile data for both
2. Fetch recent posts for both
3. Compare:
   - topics
   - tone
   - posting cadence
   - audience cues from engagement
4. State explicitly that conclusions come from a small recent sample unless more data was collected

Suggested commands:
- `twitter user <handle-a> --yaml`
- `twitter user <handle-b> --yaml`
- `twitter user-posts <handle-a> --max 20 --yaml`
- `twitter user-posts <handle-b> --max 20 --yaml`
