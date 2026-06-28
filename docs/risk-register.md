# Clinical AI Risk Register
**Project:** AI-Assisted Triage Decision Support — Caribbean Hospital Deployment
**Version:** 1.0
**Last updated:** June 2026

> **How to read this table:** Each risk has a Likelihood and Impact rating of H (High), M (Medium), or L (Low). If a mitigation cannot be put into practice, the risk is flagged — and deployment is not recommended until it is resolved.

---

## Quick summary

| # | Risk name | Category | Likelihood | Impact | Flagged? |
|---|-----------|----------|-----------|--------|----------|
| R1 | Training data is skewed toward non-Caribbean patients | AI-Technical | H | H | No |
| R2 | The model quietly stops working well over time | AI-Technical | H | H | No |
| R3 | The model gives a wrong answer with high confidence | AI-Technical | M | H | Conditional |
| R4 | Nobody can explain why the model made a decision | AI-Technical | H | M | No |
| R5 | Clinicians start ignoring all alerts, including real ones | Operational | H | H | No |
| R6 | The AI adds extra work instead of reducing it | Operational | M | M | No |
| R7 | Patients don't know AI is being used in their care | Ethical | H | M | No |
| R8 | Clinicians stop thinking for themselves and just follow the AI | Ethical | H | H | No |
| R9 | The model works poorly for rural and low-income patients | Equity | H | H | No |
| R10 | Patients without smartphones or digital records get worse assessments | Equity | M | M | No |

---

## AI-Technical risks

### R1 — Training data is skewed toward non-Caribbean patients

**Category:** AI-Technical

**Likelihood:** H — Almost all the data the model learned from came from hospitals in North America, Europe and beyond, but not the Caribbean. Caribbean patients, especially Afro-Caribbean patients, are barely represented.

**Impact:** H — The model may regularly mis-score the urgency of a patient's condition if that patient doesn't resemble who it was trained on. This means some patients could wait too long for care.

**Mitigation:** Before the system goes live, check how well it performs across different groups — by age, sex, ethnicity, and existing health conditions. Collect labelled local cases to retrain the model on Caribbean patients. Set a firm rule: no patient group can have a detection rate more than 10 percentage points below the overall average. If that rule can't be met, don't go live.

**Signal of success:** A monthly report that breaks down accuracy by patient group. If any group falls more than 10 percentage points below the overall average for two months in a row, the system is paused for review.

---

### R2 — The model quietly stops working well over time

**Category:** AI-Technical

**Likelihood:** H — Caribbean hospitals see surges in diseases like dengue, and staff document patient information differently than hospitals elsewhere. These differences can slowly cause the model's performance to slip without anyone noticing.

**Impact:** H — Clinicians keep trusting the model even though it has drifted from what it was tested on. By the time anyone notices, patients may have been harmed.

**Mitigation:** Run an automatic monthly check that compares what the model is seeing today against what it learned from. If the difference is large (something like more than 2 standard deviations), a clinical team reviews the outputs before they are used again. Set aside some local patient data specifically to test the model every month.

**Signal of success:** A monthly accuracy score on the reserved local test data. If the score drops by more than 3 percentage points compared to the starting point over any 30-day window, the model must be re-calibrated before continuing.

---

### R3 — The model gives a wrong answer with high confidence

**Category:** AI-Technical

**Likelihood:** M — More likely if the system includes a tool that generates free-text summaries like a chatbot-style feature. Less likely if it only outputs a triage score.

**Impact:** H — A nurse sees a confident-looking wrong recommendation and overrides their own correct instinct, especially likely on night shifts when they are tired.

**Mitigation:** Do not allow the model to produce open-ended text outputs. Every output should be a structured score with a confidence range shown beside it (e.g., "Level 2 — 68% confident"). Add a clearly labelled "I disagree" button that clinicians can use without friction. Log every disagreement for weekly review.

> **Conditional flag:** If the system is ever updated to include a free-text generative feature and this mitigation cannot be guaranteed by the technical design, this risk must be flagged as a **deployment blocker** until resolved.

**Signal of success:**
Track the "I disagree" rate per clinician per shift. Also track how often the model scores below 60% confidence — that should not exceed 20% of cases. Run a monthly review of cases where the model was highly confident but wrong.

---

### R4 — Nobody can explain why the model made a decision

**Category:** AI-Technical

**Likelihood:** H — Most AI triage tools are "black boxes" by default. Explainability features are rarely built in unless specifically required.

**Impact:** M — Without knowing the reason behind a recommendation, clinicians cannot check whether it makes sense, teach from it, or defend it if something goes wrong.

**Mitigation:** Display the top 3 reasons behind each recommendation in plain language (e.g., "Oxygen level 91%, breathing rate 26, new confusion"). Validate monthly that these reasons make clinical sense by having a clinician review 20 randomly selected cases.

**Signal of success:** A quarterly staff survey asking: "Do you understand why the model made this recommendation?" Target: average score of 4 out of 5 or higher. Zero formal complaints about unexplained decisions in any governance review.

---

## Operational risks

### R5 — Clinicians start ignoring all alerts, including real ones

**Category:** Operational

**Likelihood:** H — Research consistently shows that 49 – 96% of clinical alerts are dismissed within weeks of a new system being introduced. This is even more likely in Caribbean hospitals where there are fewer staff to manage ongoing system oversight.

**Impact:** H — When clinicians are trained to ignore alerts, they start dismissing the serious ones too. A real emergency gets the same reaction as a false alarm.

**Mitigation:** Limit interrupting alerts to only the most urgent cases (immediate danger). Lower-priority flags should appear quietly in a sidebar — not as pop-ups or sounds. Cap the number of active alerts any one clinician sees at 3 at a time. Report the weekly dismissal rate to the clinical lead. Review and tighten alert thresholds every quarter.

**Signal of success:** Weekly alert dismissal rate per clinician (target: below 30%). Time from alert to clinical action for the most urgent cases (target: no slower than before the AI was introduced). Quarterly threshold review documented and on file.

---

### R6 — The AI adds extra work instead of reducing it

**Category:** Operational

**Likelihood:** M — Triage nurses at under-resourced Caribbean hospitals often manage more patients than is ideal. Adding a new screen or login step could make their job harder, not easier.

**Impact:** M — Nurses spend time switching between the AI tool and paper forms, slowing down patient flow and increasing the chance of missing something.

**Mitigation:** Run a time-and-task study in the pilot ward before rolling out more widely. Build the AI directly into the existing triage screen — no extra login, no separate tool. Train all clinical staff for at least 4 hours before go-live. Appoint one person per shift as the team's AI point-of-contact.

**Signal of success:** Average time to complete triage before and after deployment (target: no more than 10% longer). A staff workload survey at 1 month and 3 months after launch (target: no increase in reported workload compared to before).

---

## Ethical risks

### R7 — Patients don't know AI is being used in their care

**Category:** Ethical

**Likelihood:** H — There is currently no rule in the project scope requiring patients to be told that AI is supporting their triage. Caribbean health regulations around medical AI are still being developed.

**Impact:** M — Patients have a right to know what tools are being used in their care. If they are not told, and something goes wrong, the hospital may face legal and trust challenges.

**Mitigation:** Create a short, plain-language information notice explaining that AI helps the nurse assess urgency, that all final decisions are made by the clinician, and that patients can ask questions. Display this at the triage desk. Formal written consent is not required (the AI only supports decisions, not makes them) — but the process must be written into hospital policy and reviewed annually.

**Signal of success:** A survey every six months asking patients whether they were aware AI tools were used during their visit (target: 70% or more saying yes within 6 months of launch). Zero negative findings relating to patient notification in the 12-month governance review.

---

### R8 — Clinicians stop thinking for themselves and just follow the AI

**Category:** Ethical

**Likelihood:** H — Studies show that clinicians are more likely to agree with a wrong AI recommendation when they are tired, or when they see the AI's answer before forming their own view.

**Impact:** H — Senior nurses defer to the model even when their instinct would have been right. Junior staff never develop independent triage skills because the AI is always there to tell them the answer.

**Mitigation:** Show the AI recommendation only after the clinician has recorded their own assessment. Track cases where the clinician and the model disagreed, and use these for training. Run a quarterly triage skills test that is paper-based and does not involve the AI. Describe the AI in all training materials as "one additional source of information," not as the final word.

**Signal of success:** Monthly rate of clinician-vs-model disagreements (target: stays stable at 15% or above — a drop below 10% is a warning sign of over-reliance). Clinical skills test pass rate: 90% or above maintained.

---

## Equity risks

### R9 — The model works poorly for rural and low-income patients

**Category:** Equity
**Likelihood:** H — Most AI clinical tools have been trained on data from large city hospitals. Patients from rural Jamaican clinics or smaller Eastern Caribbean islands present differently — often with more advanced illness, less prior medical history, and different common conditions.
**Impact:** H — The model performs best for patients who look like its training data: urban, with prior records, and with access to regular healthcare. The patients who need the best triage most — rural, first-time visitors, uninsured — are the ones it is least prepared to help.

**Mitigation:**
At least 20% of the local testing data must come from rural clinic referrals and patients who have never been to this hospital before. Work with the Ministry of Health to collect structured triage records from at least 2 rural health centres before the system is used hospital-wide. All performance reports must be broken down by where the patient came from (urban facility vs. rural referral).

**Signal of success:**
The accuracy gap between urban-hospital patients and rural-referral patients must stay below 8 percentage points. Rural and first-visit patients must make up at least 20% of the validation dataset. This is reviewed at every quarterly governance meeting.

---

### R10 — Patients without smartphones or digital records get worse assessments

**Category:** Equity
**Likelihood:** M — If the system relies on patients providing their own digital data (symptom check-in apps, wearable device readings), patients who cannot or do not use these tools will have thinner records — and potentially lower-confidence model outputs.
**Impact:** M — If the system quietly deprioritises cases where it has less data, the patients who are already least connected to the healthcare system get the least benefit — or are actively disadvantaged.

**Mitigation:**
Confirm that the AI triage tool works entirely from information recorded by the clinician at the bedside — vitals, observations, clinical notes. No patient-facing app or device should be required. If any patient-facing data collection is added in a future update, a full equity review must be completed before that feature is switched on.

**Signal of success:**
A data completeness check at 3 months and 6 months, comparing patients by insurance status and home area code. If there is a completeness gap of more than 15% between groups, patient-facing data features must be frozen and escalated to the clinical governance board.

---

## Research papers referenced
 
All five papers are freely available online and contain recent, relevant evidence on AI in clinical and healthcare settings.
 
---
 
### Paper 1 — Fairness of artificial intelligence in healthcare: review and recommendations
 
Ueda, D., Kakinuma, T., Fujita, S., Kamagata, K., Fushimi, Y., Ito, R., Matsui, Y., Nozaki, T., Nakaura, T., Fujima, N., Tatsugami, F., Yanagawa, M., Hirata, K., Yamada, A., Tsuboyama, T., Kawamura, M., Fujioka, T., & Naganawa, S. (2024). Fairness of artificial intelligence in healthcare: review and recommendations. *Japanese Journal of Radiology*, *42*(1), 3–15. https://doi.org/10.1007/s11604-023-01474-3
 
[https://pmc.ncbi.nlm.nih.gov/articles/PMC10764412/](https://pmc.ncbi.nlm.nih.gov/articles/PMC10764412/)
**Relevant to:** R1, R4, R9
 
A review by a large team of Japanese radiologists covering how AI systems in healthcare can produce unfair results for different groups of patients. It walks through the specific ways bias can enter a model — through the data it was trained on, the way the algorithm is built, and the way clinicians interact with it. It also offers concrete strategies for auditing and correcting those problems, and presents the FAIR statement: a set of recommended best practices for responsible AI deployment in clinical settings.
 
---
 
### Paper 2 — AI-driven clinical decision support systems: an ongoing pursuit of potential
 
Elhaddad, M., & Hamam, S. (2024). AI-driven clinical decision support systems: an ongoing pursuit of potential. *Cureus*, *16*(4), e57728. https://doi.org/10.7759/cureus.57728
 
[https://pmc.ncbi.nlm.nih.gov/articles/PMC11073764/](https://pmc.ncbi.nlm.nih.gov/articles/PMC11073764/)
**Relevant to:** R3, R5, R6, R8
 
A review of how AI decision support tools are being used in healthcare today — covering the underlying technologies (machine learning, natural language processing, deep learning) and the practical challenges that come with putting these tools into real clinical settings. It pays particular attention to issues of clinician trust, alert fatigue, workflow fit, and the ethical and legal responsibilities that come with AI-assisted decision-making.
 
---
 
### Paper 3 — Automation bias in AI-decision support: results from an empirical study
 
Kücking, F., Hübner, U., Przysucha, M., Hannemann, N., Kutza, J.-O., Moelleken, M., Erfurt-Berge, C., Dissemond, J., Babitsch, B., & Busch, D. (2024). Automation bias in AI-decision support: results from an empirical study. *Studies in Health Technology and Informatics*, *317*, 298–304. https://doi.org/10.3233/SHTI240871
 
[https://pubmed.ncbi.nlm.nih.gov/39234734/](https://pubmed.ncbi.nlm.nih.gov/39234734/)
**Relevant to:** R8
 
A study of 210 clinicians — nurses and physicians across German hospitals — that directly measured how often participants agreed with AI recommendations they should have questioned. It found that stronger diagnostic training and wound care certification significantly reduced the rate of blind agreement with wrong AI outputs. This is direct evidence that how clinicians are trained and assessed matters as much as how well the AI performs.
 
---
 
### Paper 4 — Patient perspectives on informed consent for medical AI: a web-based experiment
 
Park, H. J. (2024). Patient perspectives on informed consent for medical AI: a web-based experiment. *Digital Health*, *10*, 20552076241247938. https://doi.org/10.1177/20552076241247938
 
[https://pmc.ncbi.nlm.nih.gov/articles/PMC11064747/](https://pmc.ncbi.nlm.nih.gov/articles/PMC11064747/)
**Relevant to:** R7
 
A single-author experimental study from Hanyang University (South Korea) that tested what patients actually want to know when AI tools are used in their care — and found that most patients are currently unaware this is happening at all. The study explores the level of disclosure patients expect, and whether they feel they should be able to opt out. It is directly relevant to designing a patient notification and consent policy for this project.
 
---
 
### Paper 5 — Recent advances, applications and open challenges in machine learning for health: reflections from research roundtables at ML4H 2024 symposium
 
Adibi, A., Cao, X., Ji, Z., Kaur, J. N., Chen, W., Healey, E., Nuwagira, B., Ye, W., Woollard, G., Xu, M. A., Cui, H., Xi, J., Chang, T., Bikia, V., Zhang, N., Noori, A., Xia, Y., Hossain, M. B., Frank, H. A., Peluso, A., Pu, Y., Shen, S. Z., Wu, J., Fallahpour, A., Mahbub, S., Duncan, R., Zhang, Y., Cao, Y., Xu, Z., Craig, M., Krishnan, R. G., Beheshti, R., Rehg, J. M., Karim, M. E., Coffee, M., Celi, L. A., Fries, J. A., Sadatsafavi, M., Shung, D., McWeeney, S., Dafflon, J., & Jabbour, S. (2025). *Recent advances, applications and open challenges in machine learning for health: reflections from research roundtables at ML4H 2024 symposium*. arXiv. https://doi.org/10.48550/arXiv.2502.06693
 
[https://arxiv.org/abs/2502.06693](https://arxiv.org/abs/2502.06693)
**Relevant to:** R1, R2, R9, R10
 
A reflective paper from the fourth Machine Learning for Health (ML4H) symposium, held in Vancouver in December 2024, drawing on discussions across 13 research roundtables. It addresses the specific challenges of deploying health AI in low- and middle-income country settings — including the absence of standardised electronic records, small or biased local datasets, infrastructure gaps, and the risk that models trained in high-income countries perform poorly for the very patients who need them most. It also discusses what it takes to build trustworthy, locally relevant, and sustainable AI partnerships.
 
---

*Risk register prepared for internal governance review. To be updated following pilot evaluation and in advance of any expansion to additional wards or facilities.*