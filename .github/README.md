# Pull request scope check

The `Check Pull Request Scope` workflow uses a configurable OpenAI-compatible
provider and a deterministic fallback.

## Configure the AI provider

Set repository **Actions secrets** and **variables**:

- Gemini: secret  `GEMINI_API_KEY`; variable `AI_PROVIDER=gemini`; variable
  `AI_MODEL_GEMINI` set to a currently supported model ID.
- OpenRouter: secret `OPENROUTER_API_KEY`; variable `AI_PROVIDER=openrouter`;
  variable `AI_MODEL_OPENROUTER` set to a currently available model ID ending
  in `:free`.

If `AI_PROVIDER` is unset, the workflow defaults to Gemini. Model IDs are not
hardcoded; verify the current free-tier model names and availability in the
provider documentation before setting these variables. Provider rate limits
and free-tier terms can change.

For pull requests from forks, GitHub does not provide repository secrets to
the workflow. When no API key is available, the provider is unavailable, or
`AI_PROVIDER=none`, the rule-based fallback runs. It only logs a notice about
a missing title, missing description, or unchecked checklist item. It cannot
judge whether changes are related, always returns `no`, and never posts a
comment. Do not change this workflow to `pull_request_target` or run pull
request code with secrets.

## Fallback and disabling AI

Set `AI_PROVIDER=none` to disable API calls and run only the rule-based
fallback. It only logs a notice about a missing title, missing description,
or unchecked Markdown task-list item. It cannot judge whether changes are
related, always returns `no`, and never posts a comment.

To disable the entire workflow, disable **Check Pull Request Scope** from the
repository's Actions settings or remove its `pull_request` trigger.
