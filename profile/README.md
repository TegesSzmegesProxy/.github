# Tessera

A reverse proxy that sits in front of your web app and blocks attack requests before they reach it. A hosted control plane learns your app and writes the rules.

## How it works

1. **Learn.** A collector in your CI scans the environment and uploads a redacted report. The control plane reads your repository in a sandbox with no network access, lists the endpoints and matches known CVEs.
2. **Write the policy.** An LLM drafts a policy per endpoint and field. The compiler accepts only registered tools. An admin approves it, and it ships as a signed bundle.
3. **Enforce.** The proxy checks the bundle signature at startup and enforces the policy locally, even if the control plane is down. Static tools run on every request. Policy violations are blocked with no AI involved.
4. **Classify.** Suspicious requests, and a sample of safe ones, go to JEV, a classification model (not a chatbot). It returns an attack probability. The request is blocked with a `403` only if that probability is above your threshold.
5. **Adapt.** Recent attacks raise the sampling rate for that endpoint. It cools down slowly, within bounds you set.

Try it on a real CVE: [interactive demo](https://tegesszmegesproxy.github.io/landing/) (mocked data, runs in your browser).

## Repositories

| Repo | What it is |
|---|---|
| `backend` | Control plane and proxy |
| `dashboard` | Policy review and admin UI |
| `landing` | Marketing site and demo |
| `docs` | Documentation |
