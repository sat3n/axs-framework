# AXS Test Protocol

## Overview

This document describes how to run a standardised AXS assessment against a service. The protocol is designed to be reproducible: two assessors scoring the same service should arrive at similar results.

## Prerequisites

- Access to an AI coding agent (Claude Code, Cursor, or equivalent)
- Free/developer tier account on the service being tested
- The [scoring methodology](scoring-methodology.md) for reference
- The [scorecard template](../templates/axs-scorecard.md) for recording results
- A timer (for latency measurements)
- Approximately 45–90 minutes per service

## Step 1: Define Standardised Tasks (One-Time Per Category)

Before testing any service, define 3 standardised tasks for the category. These tasks must be:

- Representative of common agent use cases for that service type
- Achievable on a free/developer tier
- Identical across all services in the category

**Example tasks for Project Management category:**

1. Create a project called "AXS Test Project" with 5 tasks, each with a title, description, and due date
2. Move a task to a different status and add a comment to it
3. Query all tasks assigned to a specific user that are due within the next 7 days

**Example tasks for CRM category:**

1. Create a contact with name, email, phone, and company fields
2. Create a deal/opportunity linked to that contact with a value and stage
3. Query all contacts added in the last 30 days, filtered by company

Document your category tasks in `benchmarks/[category]/tasks.md` before starting.

## Step 2: Score Discoverability (Manual Review, ~15 min)

Without signing up or using the API, review the service's public-facing documentation:

- [ ] Search for the service + "API documentation". Can you find it within 2 clicks?
- [ ] Is there a published OpenAPI spec, MCP manifest, or GraphQL schema?
- [ ] Read 5 endpoint descriptions. Are they unambiguous? Could an agent infer correct usage from the description alone?
- [ ] Is there a machine-readable changelog or versioning?
- [ ] Ask your AI agent: "Based on [service] documentation, what can this API do?" Score the accuracy of its response.

Record scores per criterion in the scorecard.

## Step 3: Score Schema Quality (Manual Review, ~15 min)

Review the API specification or documentation:

- [ ] Count total endpoints. How many have fully typed request/response schemas?
- [ ] Check for internal consistency: do related endpoints use the same data types for the same concepts?
- [ ] Identify the error response format. Is it consistent across endpoints?
- [ ] Run the same GET request twice. Compare response structure.

Record scores per criterion in the scorecard.

## Step 4: Score Auth & Access (~15 min)

Sign up and get API credentials:

- [ ] Time the process from "click sign up" to "have a working API key/token"
- [ ] Record every step and friction point (CAPTCHA, phone verification, email confirmation, dashboard navigation to find key)
- [ ] Check: is machine-to-machine auth documented?
- [ ] Check: are scoped permissions available?
- [ ] Check: is token refresh documented?

Record scores per criterion in the scorecard.

## Step 5: Run Agent Tasks (~20–40 min)

This is the core test. For each of the 3 standardised tasks:

1. Open your AI agent in a clean session
2. Give it the prompt: "[Task description]. Use the [Service] API. Here is my API key: [key]."
3. Start the timer
4. Observe and record:
   - Did the agent complete the task on the first attempt?
   - How many iterations/retries were needed?
   - Did you need to intervene? (If yes, record what you did)
   - What errors did the agent encounter? Were error messages helpful?
   - Total time to task completion
5. Stop the timer

Repeat each task 3 times to test consistency.

Record all observations in the scorecard.

## Step 6: Score Reliability (From Task Results)

Using the data from Step 5:

- [ ] Calculate task completion rate across all attempts
- [ ] Note any availability issues during testing
- [ ] Deliberately hit the rate limit. Record the response.
- [ ] Test idempotency: send a duplicate write request. Record the behaviour.

Record scores per criterion in the scorecard.

## Step 7: Score Recoverability (~10 min)

Deliberately trigger errors:

- [ ] Send a request with a missing required field. Is the error actionable?
- [ ] Send a request with the wrong data type. Is the error actionable?
- [ ] Send a request to a non-existent resource. Is the error actionable?
- [ ] Send a request with an expired/invalid token. Is the error actionable?
- [ ] If the API supports batch operations, include one invalid item. What happens to the valid items?

Record the exact error messages received and score per criterion.

## Step 8: Score Latency & Efficiency (From Task Results + Additional Tests)

Using timing data from Step 5, plus:

- [ ] Record p50 and p95 response times for standard GET operations (run 10 requests)
- [ ] Check for pagination support. Test retrieving a list that exceeds one page.
- [ ] Check for bulk/batch endpoints
- [ ] Check for streaming or webhook support for long-running operations

Record scores per criterion in the scorecard.

## Step 9: Score Determinism (~10 min)

- [ ] Send 5 identical GET requests. Compare responses field by field (exclude timestamps, request IDs)
- [ ] Review documentation for write operation side effects. Are they documented?
- [ ] If testing over multiple days, compare endpoint behaviour between sessions

Record scores per criterion in the scorecard.

## Step 10: Compile Results

1. Total all dimension scores
2. Assign tier classification
3. Check minimum dimension floor (no dimension below 40% of its max)
4. Write a per-service summary (2–3 paragraphs): what worked well, what broke, specific friction points
5. Save the completed scorecard to `benchmarks/[category]/results/[service-name].md`
6. Submit via PR

## Recording Standards

- All scores must reference specific observations, not impressions
- Error messages should be quoted verbatim (redact API keys)
- Timing data should include the method used (wall clock, programmatic)
- Agent model and version should be recorded (e.g. "Claude Code, Claude Sonnet 4.6")
- Date of assessment should be recorded (results age; services change)

## Assessor Notes

- Test during business hours in the service's primary region where possible
- Use the free/developer tier unless the free tier is too limited to complete tasks
- If a task is impossible on the free tier, record this as a finding
- Do not contact the service's support team during testing. The point is to measure unassisted experience.
