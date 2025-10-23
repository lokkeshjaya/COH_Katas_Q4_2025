# ADR-002: Adopt a Shadow-Mode and Human-in-the-Loop Validation Strategy for AI Output

| **Field** | **Content** |
| --- | --- |
| **Title** | Adopt a Shadow-Mode and Human-in-the-Loop Validation Strategy for AI Output |
| **Status** | Accepted |
| **Context** | Non-deterministic Generative AI (like the Incentivizer) and complex ML (like the Predictor) are prone to drift and "misbehavior" in production, unlike deterministic code. Traditional testing is insufficient. |
| **Decision** | Implement a **two-pronged validation strategy**: 1) The **Dynamic Demand Predictor** will initially run in **Shadow Mode**, comparing its predictions to current outcomes before its output is actioned. 2) The **Generative Commute Incentivizer** will use **Human-in-the-Loop (HIL) validation** for newly generated incentive templates, followed by A/B testing in production to measure the effectiveness (lift). |
| **Consequences** | 
- **Pros (Justification):**
    - **Trust and Quality:** HIL prevents poor or inappropriate GenAI output from reaching customers, maintaining brand quality. **Verification:** Shadow mode provides quantifiable metrics to prove the AI is working before going live. 
- **Cons (Trade-offs):** HIL introduces a latency and cost for human review; Shadow mode adds monitoring and data processing overhead; AI results verification is a continuous, non-one-time cost. |