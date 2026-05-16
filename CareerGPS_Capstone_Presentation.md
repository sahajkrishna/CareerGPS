# CareerGPS: AI-Driven Diagnostic Engine & Roadmap Architect
**Capstone Project Presentation & Executive Summary**

---

## 1. The Problem Statement: Meet Priya
Priya is an aspiring tech professional. Like millions of other job seekers, she is ambitious but completely lost. 

**Where was Priya?**
* **Stuck in "Tutorial Hell":** She spends hours watching coding tutorials but never builds anything tangible. 
* **The Resume Black Hole:** She sends out hundreds of generic resumes to jobs she isn't fully qualified for, only to receive automated rejections. 
* **The Delusion Gap:** Priya believes she is ready for a Mid-Level/Senior role based on the tutorials she watched, but she lacks the *Proof of Work* that hiring managers actually look for. She has no idea what her actual, tangible skill gaps are.

**The Bottom Line:** Priya didn't need another generic Udemy course. She needed a brutally honest reality check and an anchored, actionable path forward.

---

## 2. The Solution: CareerGPS
We built **CareerGPS**—a headless, AI-driven diagnostic engine designed to tell Priya exactly where she stands. 

Instead of generic career advice, CareerGPS mathematically calculates the "Delta" (the gap) between her current resume and the *Gold Standard Job Description* she wants to achieve. It then generates a customized, 90-day roadmap that forces her to build real projects ("Proof of Work") rather than just watching more tutorials.

---

## 3. The Architecture (Start to End Workflow)

We built a fully automated, production-ready SaaS pipeline using a modern No-Code/AI stack: **HTML/CSS/JS (Frontend) ➡️ n8n (Orchestration) ➡️ Groq/Llama 3.1 (AI Logic) ➡️ Supabase (Database).**

### Step 1: The Input (The User Interface)
Priya visits the CareerGPS web app. She inputs three critical pieces of data:
1. Her current **Resume**.
2. The exact **Job Description (JD)** she is aiming for.
3. Her **GitHub Repository / Portfolio URL** (Her Proof of Work).

### Step 2: The Validation Pipeline (n8n & Webhooks)
The moment she clicks submit, a webhook fires the data into our **n8n automation pipeline**. 
* The system standardizes her data.
* It triggers an **Upsert User** protocol with our **Supabase** database. If Priya is a new user, it creates an account. If she is returning, it links to her existing UUID to track her progress over time.

### Step 3: The "Lie Detector" (GitHub Integration)
Before making any judgments, our custom **GitHub Parser** automatically scrapes the README of the GitHub URL she provided. 
* If Priya's resume claims she is a "React Expert," but her GitHub only contains a basic Python script, the system logs this discrepancy. 

### Step 4: The Diagnostic Engine (AI Agent 1)
All this data (Resume + JD + GitHub Code) is fed into our first AI Agent powered by the **Llama 3.1** language model.
* **The Readiness Score:** The AI calculates a brutal, honest percentage (e.g., 40%) of how ready she is for the job.
* **Critical Gaps:** It explicitly lists exactly what she is missing, directly calling out any lack of evidence found in her GitHub.

### Step 5: The Roadmap Architect (AI Agent 2)
The raw assessment data is passed to a second AI Agent. This agent constructs a customized **90-Day Learning Roadmap**. 
* **The Catch:** It does not assign tutorials. It assigns **Actionable Deliverables** (e.g., "Deploy an E-commerce App"). It locks the next phase of the roadmap behind a required "Proof of Work."

### Step 6: The Output (PDF Generation)
The final JSON payload is sent perfectly back to the UI. The web app renders a beautiful, premium dashboard showing Priya her Readiness Score, her Critical Gaps, and her Roadmap. With one click, she can download this as a stylized **PDF Report** to anchor her journey for the next 90 days.

---

## 4. The Future Scope: The Gamified Loop
CareerGPS is not a one-off PDF generator; it is designed as a continuous feedback loop. 

**What happens next?**
Priya takes her PDF and goes to work. Four weeks later, after successfully building her first project, she returns to CareerGPS. She enters her email and pastes her *new* GitHub URL. 

Our database recognizes her. The AI analyzes her new code, verifies she completed the "Proof of Work," mathematically **increases her Readiness Score**, and unlocks Phase 2 of her career journey. We have turned career development into a gamified, data-driven engine.
