# PoC Plan: omnistack-agent

## Project Classification
- **Type:** infrastructure
- **Key Technologies:** Node.js (build scripts), Markdown (prompt templates)
- **ODH Relevance:** Validates that prompt-engineering build tools can run as containerized batch workloads on OpenShift.

## PoC Objectives
1. Containerize the build and validation pipeline using a UBI Node.js image
2. Verify the prompt compilation (build) produces correct adapter files
3. Validate that the compiled adapters match source with the validate script
4. Run the built-in test suite inside the container

## Infrastructure Requirements
- **Resource Profile:** small (256Mi RAM, 250m CPU)
- **GPU Required:** No
- **Persistent Storage:** None
- **Deployment Model:** job
- **Listens on Port:** No
- **LLM API Required:** No

## Test Scenarios

### Scenario 1: build-adapters
- **Description:** Run the build script to compile core + knowledge into adapter files
- **Type:** cli
- **Input:** `node scripts/build.mjs`
- **Expected:** Exits 0, generates adapter files
- **Timeout:** 30 seconds

### Scenario 2: validate-adapters
- **Description:** Run the validation script to confirm adapters match source
- **Type:** cli
- **Input:** `npm run validate`
- **Expected:** Exits 0, confirms no drift between source and adapters
- **Timeout:** 30 seconds

### Scenario 3: run-tests
- **Description:** Run the built-in test suite
- **Type:** cli
- **Input:** `node --test`
- **Expected:** Exits 0, all tests pass
- **Timeout:** 30 seconds
