# Healthcare Navigation Agent

An AI agent on AWS Bedrock that helps patients navigate the *real* complexity of
US healthcare: not "which doctor is good," but **"what does my plan actually let
me do, and what's the right next step?"**

Describe your need in plain language — *"knee pain, Meridian HMO, need it looked
at this month"* — and the agent:

1. **Reads your plan's rules** (RAG over plan documents): does this HMO require a
   PCP referral for a specialist? What's the copay? Which network tier applies?
2. **Finds the right providers** — in-network for *your* plan, right specialty,
   with the facilities you need (onsite lab/imaging, accepting new patients).
3. **Explains the required path** — e.g., "your plan needs a referral first;
   here are in-network PCPs with the earliest availability."
4. **Takes one action** — creates an appointment-request record and sends a
   confirmation.

Built by someone with production healthcare-platform experience: the point is
not the chatbot, it's the **infrastructure** — guardrailed, observable,
cost-controlled, PHI-aware, and fully provisioned as code.

## Architecture

- **Amazon Bedrock Agents** — reasoning + tool orchestration (action groups → Lambda)
- **Bedrock Knowledge Base** — RAG over synthetic insurance plan documents
- **Bedrock Guardrails** — refuses medical advice; PII/PHI filtering
- **DynamoDB** — synthetic provider directory (specialty, networks, facilities) + request records
- **Lambda + API Gateway** — tools and the front door
- **Terraform** — all infrastructure as code
- **CloudWatch + AWS Budgets** — observability and cost controls from day zero

*(Architecture diagram coming with Phase 1.)*

## Status / Roadmap

- [x] Phase 0 — repo, cost guardrails ($20 budget alarm via Terraform)
- [ ] Phase 1 — synthetic data: provider directory (DynamoDB) + plan documents
- [ ] Phase 2 — Knowledge Base: plan docs → S3 → vector store, retrieval working
- [ ] Phase 3 — agent: action groups (search, availability, request), guardrails
- [ ] Phase 4 — API Gateway front door, end-to-end demo, write-up
- [ ] Phase 5 (2027) — MCP server exposing the same tools to any MCP client

## Important notes

- **All data is synthetic.** Providers, insurers ("Meridian HMO", "AcmeCare PPO"),
  plans, and availability are fictional, generated for this project. No real
  patient, provider, or payer data is used anywhere.
- **Not medical advice.** The agent navigates *process* (coverage, referrals,
  scheduling) and is explicitly guardrailed against clinical recommendations.
- Personal project, personal AWS account. Not affiliated with any employer.