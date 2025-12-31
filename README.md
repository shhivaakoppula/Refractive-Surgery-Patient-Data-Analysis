# Refractive-Surgery-Patient-Data-Analysis


## Overview

This repository contains two complementary data science projects analyzing patient-facing content about PRK and LASIK refractive surgery. Both projects use the same dataset of 154 social media videos but answer different questions - one focuses on content quality, the other on patient experience.

Together, these projects demonstrate end-to-end data science capabilities applied to healthcare: from quality assessment to sentiment analysis, from predictive modeling to clinical recommendations.

---

## Project 1: Patient Education Quality Assessment

### What This Project Does

This project builds a machine learning system that predicts whether patient education content about refractive surgery meets clinical quality standards. I analyzed 154 videos using the DISCERN framework (a validated tool for assessing health information quality) and built a model that can automatically identify high-quality content with 95% accuracy.

The main finding is that only about one-third of patient-facing content actually meets basic quality standards, which means most patients are learning about eye surgery from incomplete or biased sources.

### Why This Matters

When someone is considering LASIK or PRK, they usually start by watching YouTube videos or scrolling through TikTok. The algorithm shows them whatever gets the most engagement, not what's most accurate. This creates a real problem because patients are making surgical decisions based on whatever content happens to be popular.

I found that engagement and quality are basically uncorrelated (r = -0.17). High-quality educational content doesn't necessarily go viral, and viral content isn't necessarily accurate. For companies like Alcon that manufacture the surgical equipment, this represents an opportunity - there's a huge gap in the market for evidence-based patient education.

### Technical Approach

I evaluated each video using the DISCERN framework, which has 16 questions covering source reliability, treatment information completeness, and balanced discussion of risks. Videos scoring 35 or above (out of 57 total points) were labeled as high quality.

Then I engineered about 20 features from the data:
- DISCERN subscores (reliability section, treatment section)
- Engagement metrics (likes, comments, shares, saves)
- Content characteristics (video length, procedure type)
- Source credibility (medical professional vs general user)

I tested three classification algorithms: Logistic Regression, Random Forest, and Gradient Boosting. Logistic Regression performed best, which makes sense because quality assessment is fundamentally about whether content meets certain criteria - a linear combination problem.

### Key Results

The model achieved 94.9% accuracy with a ROC-AUC of 0.991, which is near-perfect discrimination between high and low quality content.

Breaking down the quality landscape:
- Only 34.4% of videos meet quality standards
- 53.9% are "fair" quality (borderline)
- 11.7% are "poor" quality (inadequate)

The most important predictors were:
- DISCERN Section 2 (treatment quality): 23.5% of importance
- DISCERN Section 1 (reliability): 22.2% of importance
- Engagement metrics: 20% of importance
- Source credibility: only 2.6% of importance

That last finding was surprising. I expected medical professionals to produce much higher quality content, but the difference was only 2.5 points out of 57 (statistically significant but clinically modest). Content structure matters way more than who created it.

### Business Value

For medical device companies, this has direct applications:

**Content monitoring** - Deploy the model to continuously scan social media for competitor content or identify misinformation about your procedures. You can automatically flag low-quality content that might be confusing patients.

**Quality assurance** - Use it as a pre-publication check for your own patient education materials. Run everything through the model before posting to make sure it meets DISCERN standards.

**Competitive positioning** - The data shows a massive quality gap right now. Companies that provide DISCERN-compliant educational materials can differentiate themselves through trust and transparency.

The accuracy is high enough that you'd only need human review for borderline cases (scores around 33-37). This could reduce manual content review time by about 40% while improving consistency.

---

## Project 2: Patient Recovery Sentiment Analysis

### What This Project Does

This project analyzes how patients actually feel during recovery from PRK and LASIK based on what they share on social media. I used sentiment analysis to track emotional trajectories across the post-operative timeline and built a predictive model that identifies which patients are likely to have difficult recovery experiences.

The goal was to understand the real patient experience so surgeons can set better expectations and provide support when it matters most.

### Why This Matters

Clinical outcome measures focus on visual acuity and complications, but patient satisfaction is also about whether the recovery matched their expectations. When patients post about their surgery on TikTok, they're brutally honest in ways they might not be during a follow-up appointment.

I found that about 12% of patients report negative experiences, and these cluster in the first few days after surgery. More interesting is that PRK and LASIK have different recovery sentiment patterns even though both ultimately deliver similar visual results.

### Technical Approach

I manually coded sentiment for each of the 154 videos as positive, neutral, or negative. Automated sentiment analysis tends to miss medical context - for example, "my eyes are killing me" could be positive if someone is joking about how much better they see now.

For the 66 videos with timing information, I extracted the post-operative day and grouped them into recovery phases:
- Day of surgery (Day 0)
- Acute phase (Days 1-3)  
- Early recovery (Days 4-7)
- Mid recovery (Days 8-14)

I engineered about 53 features combining sentiment scores, engagement patterns, content quality (from Project 1), and temporal information. Then I built a Random Forest classifier to predict positive vs negative recovery experiences.

### Key Results

The sentiment breakdown was:
- 55.8% positive experiences
- 31.8% neutral experiences
- 12.3% negative experiences

By procedure:
- **PRK**: 50.0% positive, 13.8% negative
- **LASIK**: 62.2% positive, 10.8% negative

The difference wasn't statistically significant overall (p=0.18), but when I looked specifically at early recovery (Days 0-7), LASIK showed significantly better sentiment (p=0.008). This matches what surgeons know clinically - LASIK has gentler early recovery.

The temporal pattern was revealing. On surgery day, 88.5% of patients were positive (excited, relieved). Then sentiment drops during Days 1-3 to only 28.6% positive when pain and blur are worst. By Days 4-7 it rebounds to 40% positive.

The predictive model achieved 64.1% accuracy with ROC-AUC of 0.642. The top predictors were:
- Information quality score: 17.8% importance
- Patient engagement level: 17.4% importance
- Video length: 16.5% importance  
- Post-operative timing: 14.3% importance

The fact that information quality was the strongest predictor is interesting. Patients who created well-structured, comprehensive content about their recovery tended to report more positive experiences. This suggests that understanding what to expect correlates with better outcomes.

### Business Value

For medical device companies and surgical practices:

**Patient counseling** - The timeline data shows exactly when patients struggle most (Days 1-3). Pre-operative materials should specifically prepare patients for this window and emphasize it's temporary. Many patients seem genuinely surprised by early discomfort.

**Procedure recommendations** - Now there's quantitative evidence that LASIK has easier early recovery. When counseling anxious patients, this data supports recommending LASIK if they're eligible for both procedures.

**Early intervention** - The predictive model could flag patients at risk for poor experiences. If someone posts negative content on Day 1-2, that's a signal for proactive outreach rather than waiting for them to call with concerns.

**Continuous monitoring** - Social media sentiment could be tracked as an ongoing quality metric. If a clinic's patients start posting more negative content than baseline, that's an early warning of potential systematic issues.

Implementing these insights could reduce negative experiences by 20-25% through better expectation management and targeted support during the acute phase.

---

## Technology Stack

**Python** - All data processing, analysis, and modeling (Pandas, NumPy, Scikit-learn)

**Statistical Analysis** - Hypothesis testing, correlation analysis, comparative statistics (SciPy, Statsmodels)

**Machine Learning** - Classification models for quality prediction and outcome forecasting (Scikit-learn with Random Forest, Logistic Regression, Gradient Boosting)

**Data Visualization** - Publication-quality dashboards and charts (Matplotlib, Seaborn)

**Clinical Frameworks** - DISCERN health information quality assessment (validated instrument with 20+ years of peer-reviewed use)

## How to Run These Projects

```bash
# Clone the repository
git clone https://github.com/yourusername/refractive-surgery-analysis.git
cd refractive-surgery-analysis

# Install dependencies
pip install -r requirements.txt

# Run Project 1 (Quality Assessment)
python3 project1_complete_end_to_end.py

# Run Project 2 (Sentiment Analysis)
python3 project2_complete_end_to_end.py
```

Each script takes about 30 seconds and generates all visualizations, model outputs, and executive reports automatically. All files save to the outputs directory.

## What I'd Improve Next

**Real-time monitoring pipeline** - Right now these are static analyses. I'd build a system that continuously pulls new videos from social media APIs, runs them through both models, and alerts stakeholders to quality issues or concerning sentiment patterns. Would use Apache Airflow for scheduling.

**Integrated dashboard** - Combine both projects into a single interactive web application where non-technical users can explore the data. Flask backend with Plotly for interactive charts. Would include filters for procedure type, time period, and quality thresholds.

**Deeper NLP** - The sentiment analysis is currently pretty basic. I'd implement aspect-based sentiment that separately scores pain, vision quality, emotional state, and functional limitations. Would use BERT-based medical language models pre-trained on clinical text.

**Clinical validation** - The ultimate test would be linking these analyses to actual patient outcomes from medical records. Compare social media sentiment with clinical complication rates and patient satisfaction surveys to validate that what patients post online reflects their true experience.

**Production deployment** - Package both models as microservices with REST APIs. Medical device companies or surgical practices could integrate them into their workflows - automatically screening patient education content before publication and monitoring patient posts for early warning signs.

**Multi-language expansion** - Currently English-only. Extending to Spanish, Mandarin, German, and other major languages would significantly expand coverage. The DISCERN framework has been validated in multiple languages, so the quality assessment approach would transfer directly.

The current implementation proves both concepts work and deliver actionable insights. These improvements would transform them from research projects into production tools that could actually improve patient care at scale.

---

## Why These Projects Work Together

**Project 1** answers: What makes good patient education content?  
**Project 2** answers: What do patients actually experience?

Together they cover both sides of the patient journey:
- **Pre-operative**: Information quality assessment helps patients find reliable educational content
- **Post-operative**: Sentiment analysis reveals the real recovery experience and identifies patients needing support

The technical combination demonstrates:
- Classification algorithms (quality prediction)
- NLP and sentiment analysis (patient experience)
- Time-series analysis (recovery trajectories)
- Comparative research methods (PRK vs LASIK)
- Statistical validation (hypothesis testing, correlation analysis)
- Clinical application (DISCERN framework, evidence-based recommendations)

For companies like Alcon, this represents a complete data-driven approach to patient experience optimization - from the information patients consume before surgery to the recovery they experience after.
