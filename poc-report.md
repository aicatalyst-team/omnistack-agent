# PoC Report: omnistack-agent

## Executive Summary

omnistack-agent is a platform-agnostic prompt engineering build tool that compiles a single source of AI agent instructions into adapter files for 6 platforms (ChatGPT, Claude, Copilot, Gemini, Cursor, and generic LLMs). The PoC successfully containerized the project with a UBI9 Node.js image, built and pushed to Quay.io, deployed three Kubernetes Jobs on OpenShift, and validated all scenarios. The build script compiled 9 adapters, the validation script confirmed zero drift, and the test suite passed all 10 unit tests.

## Project Analysis

- **Repository:** https://github.com/Ricar66/omnistack-agent
- **Fork:** https://github.com/aicatalyst-team/omnistack-agent
- **Summary:** A prompt compilation build tool that transforms markdown source files (core/ + knowledge/) into platform-specific adapter files for AI agents.
- **Classification:** infrastructure (build tooling)

| Component | Language | Build System | ML Workload | Port |
|-----------|----------|-------------|-------------|------|
| omnistack-agent | JavaScript (ESM) | npm | No | None |

## PoC Objectives

1. Containerize the build and validation pipeline using UBI Node.js image
2. Verify prompt compilation produces correct adapter files
3. Validate compiled adapters match source with zero drift
4. Run the built-in test suite (10 tests) inside the container

## Pipeline Execution

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#EE0000', 'primaryTextColor': '#fff', 'primaryBorderColor': '#A30000', 'lineColor': '#6A6E73', 'secondaryColor': '#F0F0F0', 'tertiaryColor': '#0066CC'}}}%%
graph LR
    A["Intake ✓"] --> B["Evaluate ✓"]
    B --> C["Fork ✓"]
    C --> D["PoC Plan ✓"]
    D --> E["Containerize ✓"]
    E --> F["Build ✓"]
    F --> G["Deploy ✓"]
    G --> H["Apply ✓"]
    H --> I["Execute ✓"]
    I --> J["Report ✓"]
```

- **Intake**: Single JavaScript component. Zero npm dependencies. Existing GitHub Actions CI.
- **Evaluate**: Score 25/100. Distant relationship to Red Hat AI strategy. Build tooling for prompt engineering.
- **Fork**: `https://github.com/aicatalyst-team/omnistack-agent` with AutoPoC topics.
- **PoC Plan**: Infrastructure type. Three CLI test scenarios. Job-based deployment.
- **Containerize**: `Dockerfile.ubi` using `registry.access.redhat.com/ubi9/nodejs-22`. First-attempt build succeeded.
- **Build**: Image built via OpenShift BuildConfig. Pushed to `quay.io/aicatalyst/omnistack-agent:latest`.
- **Deploy**: Three Kubernetes Job manifests created.
- **Apply**: All Jobs deployed to `poc-omnistack-agent` namespace. All completed within 8 seconds.
- **Execute**: All 3 scenarios passed.

## Test Results

| Scenario | Status | Duration | Details |
|----------|--------|----------|---------|
| build-adapters | PASS | < 1s | Built 9 adapters across 6 platforms |
| validate-adapters | PASS | < 1s | All 9 adapters in sync with source |
| run-tests | PASS | < 1s | 10/10 unit tests passed (normalizeEol, contentHash, assembleCore, assembleKnowledge, renderTarget, TARGETS) |

## Infrastructure Deployed

- **Namespace:** `poc-omnistack-agent`
- **Container Image:** `quay.io/aicatalyst/omnistack-agent:latest`
- **Base Image:** `registry.access.redhat.com/ubi9/nodejs-22`
- **K8s Resources:** 3 Jobs (build, validate, test)
- **Resource Requests:** 256Mi memory, 250m CPU per Job
- **Services/Routes:** None (batch jobs)

## Recommendations

### Production Readiness
- **Low**: This is a build tool, not a production service. Useful for CI/CD pipelines.

### Performance
- All jobs completed in under 8 seconds. The build and validation scripts are fast and efficient.

### Security
- Container runs as non-root (UID 1001). Zero npm dependencies reduces supply chain risk.

### Next Steps
1. Integrate the build/validate pipeline into a Tekton pipeline on OpenShift
2. Use as a CI step in prompt engineering workflows

## Open Data Hub / OpenShift AI Considerations

- **Tekton Pipelines**: The build/validate/test workflow maps naturally to a Tekton Task for CI/CD of AI agent prompts.
- No direct ODH component integration needed.

## Appendix

### Artifacts
- PoC Plan: [`poc-plan.md`](https://github.com/aicatalyst-team/omnistack-agent/blob/autopoc-artifacts/poc-plan.md)
- Test Script: [`poc_test.py`](https://github.com/aicatalyst-team/omnistack-agent/blob/autopoc-artifacts/poc_test.py)
- Dockerfile: [`Dockerfile.ubi`](https://github.com/aicatalyst-team/omnistack-agent/blob/main/Dockerfile.ubi)
- Kubernetes Manifests: [`kubernetes/`](https://github.com/aicatalyst-team/omnistack-agent/tree/main/kubernetes)
- Container Image: `quay.io/aicatalyst/omnistack-agent:latest`

### Build Errors
None. First build attempt succeeded.

### Retry Counts
- Build retries: 0/3
- Deploy retries: 0/3
- Container fix retries: 0/2
