# GLM Model Configuration Example

This example configures Grok Build to use Z.AI's GLM models through the
OpenAI-compatible GLM Coding endpoint.

## 1. Store the API key

Set `ZAI_API_KEY` in your shell environment. For example, add the following to
`~/.zshrc`:

```zsh
export ZAI_API_KEY="your-zai-api-key"
```

For a private, non-versioned environment file:

```zsh
umask 077
printf 'export ZAI_API_KEY=%q\n' "$ZAI_API_KEY" > "$HOME/.zai_env"
chmod 600 "$HOME/.zai_env"

# Load it from ~/.zshrc:
source "$HOME/.zai_env"
```

Never commit a real API key.

## 2. Configure Grok

Add this to `~/.grok/config.toml`. Model settings are user-global; they do not
belong in a project-local `.grok/config.toml`.

```toml
[models]
default = "zai-glm-5.3"
default_reasoning_effort = "max"

[model."zai-glm-5.3"]
model = "glm-5.3"
name = "Z.AI GLM 5.3"
description = "GLM-5.3 via Z.AI Coding Plan"
base_url = "https://api.z.ai/api/coding/paas/v4"
api_backend = "chat_completions"
env_key = "ZAI_API_KEY"
context_window = 1000000
max_completion_tokens = 128000
reasoning_efforts = [
  { value = "low", label = "Low", description = "Lightweight reasoning" },
  { value = "high", label = "High", description = "Enhanced reasoning" },
  { value = "max", label = "Max", description = "Deep reasoning", default = true },
]

[model."zai-glm-5.3-flash"]
model = "glm-5.3-flash"
name = "Z.AI GLM 5.3 Flash"
description = "GLM-5.3 Flash via Z.AI Coding Plan"
base_url = "https://api.z.ai/api/coding/paas/v4"
api_backend = "chat_completions"
env_key = "ZAI_API_KEY"
context_window = 1000000
max_completion_tokens = 128000
reasoning_efforts = [
  { value = "low", label = "Low", description = "Lightweight reasoning" },
  { value = "high", label = "High", description = "Enhanced reasoning" },
  { value = "max", label = "Max", description = "Deep reasoning", default = true },
]
```

## 3. Use the models

```zsh
grok -m zai-glm-5.3 --effort max -p "Review this change"
grok -m zai-glm-5.3-flash --effort low -p "Summarize this repository"
```

Inside an interactive session, use:

```text
/model zai-glm-5.3
/effort max
```

GLM-5.3 supports `low`, `high`, and `max` reasoning effort. It always has
reasoning enabled; disabling reasoning is not supported.
