# Termait

Wait, what was that command again? **Termait** is an AI-powered terminal command generator that translates natural language into executable shell commands.

It supports **Anthropic (Claude)**, **OpenAI (GPT)**, and **Local Models** (via Ollama, vLLM, etc.).

## Installation

Install using `uv`:

```bash
uv tool install termait
# or from source
uv tool install .
```

## Configuration

Termait is designed to check for configuration in the following order of precedence:
1.  **CLI Arguments** (highest priority)
2.  **Environment Variables**
3.  **Configuration File** (`~/.termait` or `~/.config/termait/config.json`)
4.  **Defaults** (Anthropic)

### Configuration File

You can create a configuration file at `~/.termait` (or `~/.config/termait/config.json`) to persist your settings. This allows you to define multiple providers and switch between them easily.

**Example `~/.termait`**:

```json
{
  "provider": "anthropic",
  "anthropic": {
    "api_key": "sk-ant-...",
    "model": "claude-3-5-sonnet-20241022"
  },
  "openai": {
    "api_base": "http://localhost:11434/v1",
    "model": "llama3",
    "api_key": "ollama"
  }
}
```

To switch providers, simply change the top-level `"provider"` key to `"openai"` or `"anthropic"`.

### Environment Variables

If you prefer environment variables:

**Anthropic**
*   `ANTHROPIC_API_KEY`: Your API key.
*   `ANTHROPIC_MODEL`: Model to use (default: `claude-3-5-sonnet-20241022`).

**OpenAI / Local**
*   `OPENAI_API_KEY`: Your API key (use "ollama" or dummy value for local).
*   `OPENAI_API_BASE`: Base URL (e.g., `http://localhost:11434/v1`).
*   `OPENAI_MODEL`: Model to use (e.g., `gpt-4o`, `llama3`).

**Global**
*   `TERMAIT_PROVIDER`: Set default provider (`anthropic` or `openai`).

## Usage

### Basic Usage
Run `tm` followed by your request:

```bash
tm list files sorted by size
tm "find all python files larger than 1kb"
```

### Using Local Models (Ollama)

1.  Ensure [Ollama](https://ollama.com/) is running and you have pulled a model (e.g., `ollama pull llama3`).
2.  Configure your `~/.termait` as shown above, OR run directly:

```bash
tm --provider openai --api-base http://localhost:11434/v1 --model llama3 "list docker containers"
```

### CLI Options

*   `--provider`: Force a specific provider (`anthropic`, `openai`).
*   `--model`: Override the model (e.g., `gpt-4-turbo`, `gemma:2b`).
*   `--api-base`: Override the API base URL (useful for pointing to local servers).

```bash
# Force Anthropic even if config says generic
tm --provider anthropic "check git status"

# Use a specific model
tm --model gpt-3.5-turbo "echo hello"
```
