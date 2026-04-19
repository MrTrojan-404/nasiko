# LLM Router Gateway Integration Design

## Rationale: LiteLLM vs Portkey
For Track 2, we have selected **LiteLLM** as the platform-managed LLM gateway. 

**Trade-offs & Rationale:**
* **LiteLLM Advantages:** It is open-source, easily containerized via `ghcr.io`, and provides a seamless unified API format (OpenAI format) for over 100+ providers. It runs natively in-cluster without external dependencies.
* **Portkey Disadvantages:** While Portkey offers superior enterprise observability UI, its self-hosted open-source version is heavier and requires more complex database provisioning compared to LiteLLM's lightweight stateless proxy design.
* **Decision:** LiteLLM aligns perfectly with Nasiko's need for a lightweight, in-cluster routing layer that won't bloat the local deployment.

## Implementation Steps (Track 2)
1. **Infra Deployment:** `litellm-gateway` added to `docker-compose.local.yml` running on port 4000.
2. **Provider Credentials:** Managed centrally via `litellm_config.yaml`.
3. **WARNING TO AGENT DEVELOPERS:** Do NOT hardcode provider keys (like `OPENAI_API_KEY`) in your agent source code. You must route requests to `http://litellm-gateway:4000` using the platform-injected virtual key.
