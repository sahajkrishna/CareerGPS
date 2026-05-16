# CareerGPS: n8n Technical Architecture & Node Workflow
**System Documentation & Presenter Script**

---

This document outlines the exact step-by-step data flow through the n8n orchestration engine. You can use this as your technical script to explain the system architecture during your Capstone presentation.

---

### Phase 1: Data Ingestion & Pre-Processing

**1. Webhook_Trigger**
*   **Purpose:** The entry point of the entire application. 
*   **Action:** It continuously listens for the `POST` payload sent from the custom HTML Frontend when the user clicks "Run Honest Assessment."

**2. Input Normalizer**
*   **Purpose:** Data sanitation and structure.
*   **Action:** It ensures that the incoming data (Resume text, Job Description, Location, GitHub URL) is formatted correctly and mapped to safe variables so the rest of the workflow operates smoothly.

**3. GitHub Parser**
*   **Purpose:** URL extraction logic.
*   **Action:** A custom JavaScript node that takes the raw URL the user pasted (e.g., `https://github.com/username/project`) and mathematically slices it to extract just the exact `username/repository` string required by the GitHub API.

---

### Phase 2: The Routing Logic (The "Lie Detector")

**4. IF Node (Router)**
*   **Purpose:** Conditional workflow branching.
*   **Action:** It checks if the user actually provided a valid GitHub link.
    *   *If True:* Routes traffic to the **HTTP Request** node, which securely connects to the public GitHub API to scrape the raw text of their `README.md`.
    *   *If False:* Bypasses the HTTP request entirely, ensuring the workflow doesn't break if a user doesn't submit a portfolio link.

**5. Merge Node**
*   **Purpose:** Data convergence.
*   **Action:** It re-combines both pathways from the IF node. This guarantees that the AI agents will receive a clean, standardized package of data regardless of which path was taken.

---

### Phase 3: The First AI Agent (Diagnosis)

**6. Diagnostic Engine (AI Agent)**
*   **Purpose:** The core analytical brain powered by Groq (Llama 3.1).
*   **Action:** It analyzes the Resume, the JD, and the GitHub Data. Acting as a strict hiring manager, it mathematically calculates the **Readiness Score** and explicitly lists the **Critical Gaps**.

**7. JSON Parser**
*   **Purpose:** Data structuring.
*   **Action:** Because LLMs output raw text strings, this custom code node converts that text into a highly structured JSON object so the database can read it.

**8. Schema Validator**
*   **Purpose:** Quality control and safety checkpoint.
*   **Action:** It double-checks that the AI didn't hallucinate. It ensures all required database fields (like `readiness_score` and `career_pivot_logic`) exist before attempting to save the data.

---

### Phase 4: Database Persistence (Supabase)

**9. Upsert User (Supabase API)**
*   **Purpose:** Identity management.
*   **Action:** Connects to the Supabase database and checks the `users` table to see if the candidate's email already exists. If yes, it updates their profile. If no, it creates a brand new user and generates a unique `UUID`.

**10. Database Insert (Supabase API)**
*   **Purpose:** Record keeping.
*   **Action:** Takes the Readiness Score and Critical Gaps and permanently saves them to the `assessments` table, securely linking them to that specific user's `UUID` for future tracking.

---

### Phase 5: The Second AI Agent (Action Plan)

**11. Roadmap Architect (AI Agent)**
*   **Purpose:** The prescriptive brain (Llama 3.1).
*   **Action:** Instead of looking at the resume, this agent looks *only* at the Critical Gaps found by the first agent. It generates a customized, 3-phase, 90-Day Learning Roadmap specifically designed to close those exact gaps.

**12. Roadmap JSON Parser**
*   **Purpose:** Data structuring.
*   **Action:** Similar to step 7, it cleans the Roadmap Architect's raw text output and forces it into a strict JSON array so the HTML UI can render the timeline beautifully.

---

### Phase 6: The Delivery

**13. Merge Results**
*   **Purpose:** Payload aggregation.
*   **Action:** Takes the output from the Diagnostic Engine (Score/Gaps) and the output from the Roadmap Architect (Milestones), and combines them into one final, master JSON payload.

**14. Respond to Webhook**
*   **Purpose:** The final handshake.
*   **Action:** It sends the master JSON payload all the way back to the frontend browser, completing the API call and triggering the UI animations to render the final dashboard!
