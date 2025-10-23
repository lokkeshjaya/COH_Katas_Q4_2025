# ADR-001: Implement an AI Service Abstraction Layer


| **Field** | **Content** |
| --- | --- |
| **Title** | Implement an AI Service Abstraction Layer |
| **Status** | Accepted |
| **Context** | AI is changing fast, and models/providers are constantly shifting in performance and price. Direct integration of a specific provider (e.g., a single cloud's LLM API) into our core business logic creates significant vendor lock-in and high cost-change risk. |
| **Decision** | All core application services (Booking, Payment, etc.) will interact with AI/ML functionality through a **lightweight AI Service Abstraction Layer**. This layer exposes a uniform API for services like `get_demand_prediction(location, time)` or `generate_incentive(user_id, goal)`. |
| **Consequences** | 
- **Pros (Justification):**
    -  **Future-proof/Cost Control:** Allows MobilityCorp to swap out the underlying AI provider (e.g., move from Provider A's LLM to Provider B's) or an in-house model without changing core logic. 
   - **Increased Reliability:** Facilitates a **Circuit Breaker pattern** to fall back to a deterministic, rule-based system if the AI provider fails. 
- **Cons (Trade-offs):** Minor increase in system latency due to the extra hop; initial development cost for the abstraction layer. |