# litellm

Docker Compose setup for [LiteLLM](https://github.com/BerriAI/litellm) — a proxy that exposes a unified OpenAI-compatible API across multiple model backends.

## What it is

LiteLLM sits in front of the Gemma 4 model containers running on the `llm-net` Docker network and presents them as a single API endpoint. Clients send requests to LiteLLM on port `4000` using any OpenAI-compatible SDK, and LiteLLM routes them to the right backend based on the `model` name in the request.

Includes a PostgreSQL database for usage tracking and request logging.

## Models

Configured in `config.yaml`:

| Model name | Backend container | Weights |
|---|---|---|
| `google-gemma4-26b-a4b-bf16-it-sglang` | `google-gemma4-26b-a4b-bf16-it-sglang` | google/gemma-4-26B-A4B-it (BF16 + FP8 on-the-fly) |
| `redhatai-gemma4-26b-a4b-q8-it-sglang` | `redhatai-gemma4-26b-a4b-q8-it-sglang` | RedHatAI/gemma-4-26B-A4B-it-FP8-Dynamic |

## Requirements

- Docker with NVIDIA Container Toolkit
- The `llm-net` Docker network: `docker network create llm-net`
- One or more model containers running on `llm-net` (see linked repos)

## Usage

```bash
LITELLM_MASTER_KEY=sk-xxx docker compose up -d
```

The API is available at `http://localhost:4000`. Test it:

```bash
curl http://localhost:4000/v1/models
```

## Configuration

| Variable | Description |
|---|---|
| `LITELLM_MASTER_KEY` | Master API key for authenticating requests to LiteLLM |

To add or remove models, edit `config.yaml` and restart the container.

## License

MIT — see [LICENSE](LICENSE).
