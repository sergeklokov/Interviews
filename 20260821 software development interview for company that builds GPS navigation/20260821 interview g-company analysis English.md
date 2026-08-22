# G Interview Analysis (Commercial Aviation)

**Candidate:** Sergey  
**Interviewed by:** S* (Team Lead)  
**Date:** August 2026  
**Candidate’s final decision:** Decline. Full 5-day office requirement + uncovered parking in Arizona + heavy legacy VB6 + likely crowded office are unacceptable conditions.

[software development interview for company that builds GPS navigation on YouTube](https://www.youtube.com/watch?v=ZhG3kwsEnZY)

---

## 1. Interview Summary

The interviewer spent significant time describing the products (flight deck performance, load planning / weight & balance), the tech stack (VB6 → .NET / Blazor, SQL Server), and role expectations (technical leadership, mentoring SE1/SE2 engineers, identifying problem areas in complex systems, and delivering fast production fixes).

The candidate demonstrated relevant experience (aviation education background, broad Microsoft stack, work with both legacy and modern technologies). However, answers were often unstructured and rambling. Toward the end, the candidate focused heavily on return-to-office (RTO) conditions, which may have signaled lower motivation for the role itself.

---

## 2. Strengths and Weaknesses

### Candidate (Sergey)

**Strengths:**
- Relevant education (University of Civil Aviation, Kyiv) — immediately created common ground on weight & balance topics.
- Broad technical stack: VB6, Delphi, .NET (15+ years), SQL Server/Oracle, NoSQL, Blazor, Angular, MAUI.
- Practical examples (payment processing with concurrency limits, process documentation at Aetna that continued to be used after departure).
- Honesty regarding work-format preferences.


**Weaknesses:**
- Answers frequently went off-topic and lacked clear structure.
- When asked about the project he was most proud of, replied “there are many, I can’t choose one.”
- Very weak response to the weaknesses question (“I don’t know”, “I can’t speak for my colleagues”).
- At the end, focused too strongly on office downsides (RTO, parking, density), which could create an impression of low enthusiasm for the actual work.

### G / Interviewer Side

**Strengths:**
- Transparent about realities: legacy systems due to aviation regulations, many custom requirements from airlines, need for fast production fixes.
- Clearly stated expectations for a technical lead role.

**Weaknesses:**
- Talked too much at the beginning.
- Completely ignored the hint about office density.
- Did not address the practical realities of daily commuting and parking in a meaningful way.

---

## 3. Key Interview Moments and Comments

### Interviewer’s monologue about the company and role
- Microsoft stack, maintaining VB6 + modern technologies (Blazor, etc.).
- Products: flight deck performance, load planning (weight & balance), internal tools.
- Reason for legacy: aviation regulations — the industry updates very slowly.
- SQL Server, multiple data centers.
- Role expectations: technical guidance, mentoring level 1/2 engineers, system-level thinking, identifying problem areas, one-year roadmap thinking.

**Candidate’s later comment:**  
“The programs are very old, VB6. This is 30-year-old antiquity. It might be good to rewrite them to a modern stack, but I might end up just maintaining this garbage.”

**Analyst comment:**  
The interviewer was honest. Supporting legacy is an unavoidable part of the job. Modernization exists, but a significant portion of time will still be spent on old code.

### “Tell me about yourself”
The candidate listed experience with VB6, .NET, databases, Android/MAUI, Angular/TypeScript, and Blazor. He went into a long story about shaped recordsets in VB6.

**Analyst comment:**  
The answer was too long and unstructured. A strong self-introduction should fit into 60–90 seconds: Who I am → Key experience → Current strengths → Why this role is interesting.

**Improved short version:**  
“I’m a software engineer with more than 15 years of experience, primarily in the Microsoft stack (.NET, C#, earlier VB6 and Delphi). I studied at the University of Civil Aviation, so weight & balance topics are familiar to me. I’ve worked both as an individual contributor and as a lead / company owner. My strengths are backend development, databases, legacy systems and their modernization, as well as Blazor/Angular. I’m interested in your combination of old systems and modern technologies in the aviation domain.”

### Project you are most proud of
**Candidate’s answer:** “There were many of those. I cannot just point to one or two.” Then described payment processing and multi-threading (limit of ~20 concurrent threads).

**Analyst comment:**  
Saying “I can’t choose one” is a classic mistake. You should immediately name 1–2 projects and give a concrete result + your contribution (preferably using Situation → Task → Action → Result).

**Improved version:**  
“One of the strongest examples is an automated overnight payment system. We needed to process thousands of transactions. The main challenge was that the external API could stably handle only about 20 concurrent threads. I implemented throttling + a queue, monitoring, and idempotent retries. As a result, the system reliably processed the required volume without timeouts and with minimal manual intervention.”

### Challenges / multi-threading discussion
The core point about concurrency limits was correct, but the answer then drifted into general topics (lack of time, driver issues, etc.).

**Analyst comment:**  
Better to stay focused on one clear challenge and the solution.

### Leadership, mentoring, technical proposals
The candidate described experience as IC, lead, and company owner, plus the Aetna troubleshooting document that people continued using after he left.

**Analyst comment:**  
The documentation example is good. It would have been stronger with a measurable impact (fewer escalations, faster incident resolution, etc.).

### Balancing “ship fast” vs “maintainable code” (aviation context)
The candidate correctly noted the risks of quick-and-dirty approaches and the importance of product knowledge. He also asked about Copilot usage (they use it).

**Analyst comment:**  
Solid answer on substance.

### Weaknesses & Strengths
**Candidate’s answer:**  
“I don’t know… he’s too beautiful probably… I cannot speak for my colleagues.”  
“I cannot tell my weaknesses.”  
Strengths: programming and architecture knowledge, experience, ability to learn and listen, liking to create proof-of-concepts for the team.

**Candidate’s later comments:**  
“In my opinion this is a stupid psychological question.”  
“I want to refuse to answer this question.”  
“It’s not hard for me to answer. I just don’t want to play these fucked-up corporate games.”

**Analyst comment:**  
This was the weakest part of the interview. Refusing or saying “I don’t know” almost always lowers the score.  

If you have a principled objection, you can refuse politely (see below). If the goal is to pass interviews, it is better to have a short prepared answer.

**Polite ways to decline answering:**
- “I prefer not to answer this question. I find it of limited usefulness and rather formal.”
- “Honestly, I don’t answer the weaknesses question in this format. I can talk about specific mistakes or decisions I’ve made if that’s interesting.”

### Design tools (Rose, UML, etc.)
Normal exchange about old tools, UML, PlantUML, AI-generated diagrams, Miro, and the value of pen & paper. Nothing critical.

### Mentoring examples
The Aetna document example was good for knowledge transfer.

### Attitude toward returning to a corporation
Calm and adequate answer. The fast pace and multiple airlines with custom requirements did not scare the candidate.

### Candidate’s questions (RTO, parking, office density, commute)
The candidate focused mainly on:
- 5 days in the office
- Why not remote/hybrid
- Parking situation
- Office density (“the smaller the office the more tighter…”)
- His own commute (35–45 minutes)

**Interviewer responses:**
- Confirmed full RTO since March
- Reasons: collaboration, many cross-team dependencies
- Parking: “plenty of parking”, one open lot
- Completely ignored the density hint
- Said her own commute is 25 minutes

**Candidate’s later comments:**  
- “She lied about the commute. I checked the map — it’s 40-45 minutes.”
- “Uncovered parking — the car will melt under the Arizona sun.”
- “She ignored the hint about a crowded office, which means people sit on top of each other.”
- “I expected to hear ‘we have a spacious office, large cubicles, many meeting rooms…’. She didn’t say that. So it’s like sardines in a barrel.”

**Analyst comment:**  
The observations are largely accurate.  
- On commute: she spoke about *her own* 25-minute commute, not the candidate’s.  
- Open parking in Chandler in summer is a real practical problem.  
- Complete silence on office density is a red flag. Companies with good offices usually advertise them.  
Heavy focus on conditions at the end may have created the impression that this was the candidate’s primary interest.

---

## 4. How to Politely Refuse the Weaknesses Question

If you principally do not want to play this game:

**Option 1 (direct):**  
“I prefer not to answer this question. I consider it of limited usefulness and rather formal.”

**Option 2:**  
“Honestly, I don’t answer the weaknesses question in this format. I can talk about specific mistakes or decisions I’ve made if that’s of interest.”

**Option 3 (softer):**  
“I prefer to talk about real work situations rather than formulate abstract ‘weaknesses’. This question doesn’t resonate with me.”

The calmer and shorter you say it (without visible irritation), the less additional negative impact you create.

---

## 5. Candidate’s Final Decision

Definitely not accepting the role.

Reasons:
- Strict 5-day RTO
- Realistic commute of 40–45 minutes each way
- Uncovered parking under the Arizona sun
- Significant volume of legacy VB6
- High probability of a crowded office

Interesting domain and technical scope do not compensate for the daily discomfort the candidate already anticipates.

---

## 6. Short Recommendations for Future Interviews

- Keep self-introduction short and structured.
- When asked about the project you are most proud of, immediately name one and give a concrete result.
- On weaknesses — either a short prepared answer or a conscious polite refusal (with understanding of the consequences).
- Balance your own questions: not only conditions, but also technical and team aspects.
- If office / remote policy is a hard deal-breaker, state it earlier or decline promptly.

---

*Document prepared based on the interview transcript and subsequent discussion.*
