# Pull request scope check

The `Check Pull Request Scope` workflow uses a configurable OpenAI-compatible
provider and a deterministic fallback.

## Configure the AI provider

Set repository **Actions secrets** and **variables**:

- Gemini: secret `GEMINI_API_KEY`; variable `AI_PROVIDER=gemini`; variable
  `AI_MODEL_GEMINI` set to a currently supported model ID.
- OpenRouter: secret `OPENROUTER_API_KEY`; variable `AI_PROVIDER=openrouter`;
  optional variable `AI_MODEL_OPENROUTER` set to a currently available model
  ID ending in `:free`. It defaults to
  `nvidia/nemotron-3-ultra-550b-a55b:free`.

If `AI_PROVIDER` is unset, the workflow defaults to Gemini. Model IDs are not
hardcoded; verify the current free-tier model names and availability in the
provider documentation before setting these variables. Provider rate limits
and free-tier terms can change.

The workflow runs on `pull_request_target` so fork pull requests can use the
API secret and a write-capable token to post comments. This is safe only while
the workflow follows these rules:

- Never check out or execute code from the pull request head.
- Keep pull request title and body in environment variables only; never
  interpolate PR text into `run:` scripts.

If the API key is missing or the provider fails, the rule-based fallback runs.
It only logs a notice and never posts a comment; it cannot judge semantic scope.
Free OpenRouter models rotate and have daily limits, so the model variable may
need updating.

## Fallback and disabling AI

Set `AI_PROVIDER=none` to disable API calls and run only the rule-based
fallback. It logs a notice and never posts a comment. Its objective checks
cannot determine whether code changes are semantically related to an issue.

To disable the entire workflow, disable **Check Pull Request Scope** from the
repository's Actions settings or remove its `pull_request` trigger.
