# NaijaPulse Product Requirements

**Status:** Draft v0.1. Not validated with real customers.
**Last updated:** 2026-10-16

## 1. Purpose

NaijaPulse is a web-based customer intelligence platform that analyzes customer feedback written in Nigerian Pidgin, English, slang, and mixed-language text, and classifies its sentiment as positive, neutral, or negative. It gives Nigerian businesses, starting with fintech customer experience teams, a stored history of analyzed feedback and a dashboard showing how customer sentiment is distributed and how it changes over time.

The long-term vision is customer intelligence infrastructure for African languages and informal customer communication. This document covers only the MVP.

## 2. Primary user (persona, unvalidated)

> **This persona is a hypothesis. It was written from general assumptions about the role, not from interviews or research. It must be validated before any claim about real customer needs is made.**

**Ada, Customer Experience Manager** at a mid-sized Nigerian fintech (for example a payments app or digital wallet).

- Leads a support team of roughly 8 to 15 agents and reports to Operations or the COO. (Assumed.)
- Customer feedback likely arrives through several places: in-app chat, email, app store reviews, and social media mentions. (Assumed.)
- Customers likely write the way they speak: Pidgin, English, and mixtures of both, with informal spelling. Fintech complaints probably cluster around failed transfers, delayed reversals, and card or app problems. (Assumed.)
- She is asked by leadership whether customers are happier or angrier than before, and she likely answers with sampled readings and manual tagging rather than measured data. (Assumed.)
- She is likely to check on a phone as well as a laptop. (Assumed.)

**Other intended users, not designed for in the MVP:** Customer Support Managers, Product Managers, Operations Managers, business owners and founders.

## 3. Core workflow

1. Ada registers and creates a company account.
2. She logs in and sees an empty dashboard.
3. She submits one piece of customer feedback at a time through a form (the same capability is available through the API).
4. The system classifies it as positive, neutral, or negative, and stores the feedback with its prediction.
5. She opens the feedback history and filters by sentiment and date range, with paginated results.
6. She opens the dashboard and sees the total number of analyzed feedback items, the sentiment distribution, and a basic sentiment trend over time.
7. Users from a different company can never view her company's data.

**Illustrative examples (invented for explanation, not from a dataset):**

| Feedback text | Expected sentiment |
|---|---|
| "Una don try, my money enter inside one minute. Thank you!" | Positive |
| "I don send the money since morning, e never reach una still dey owe me." | Negative |
| "I wan change my phone number on the app." | Neutral |

Expected labels for real data will come from dataset research, not from this table. Some expressions will be ambiguous, such as exclamations whose sentiment depends on context. Handling these is part of the ML work.

## 4. MVP scope

### In scope

1. User registration and authentication
2. Company accounts with company-level data separation
3. Submission of customer feedback for analysis
4. Sentiment classification: positive, neutral, negative
5. Storage of feedback and its prediction
6. Feedback history with filtering and pagination
7. Dashboard: total analyzed feedback and sentiment distribution
8. Basic sentiment trend over time
9. API documentation
10. Responsive user interface (usable on phone and desktop)
11. Automated tests and deployment

### Out of scope for now

- Confidence scores, issue categorization, priority detection, and advanced recommendations (to be added only if their technical validity and usefulness are demonstrated)
- WhatsApp, social media, and other channel integrations
- Chatbots
- Mobile applications
- Advanced predictive analytics
- Bulk file upload (to be decided after the single-submission workflow works)
- Support for Yoruba, Igbo, Hausa, and other languages beyond Pidgin and English
- Billing and subscription management
- Claims that any large language model is integrated (none is planned for the MVP)

## 5. Success criteria

Each criterion must be verified by a test or a documented measurement before it is marked as met.

**Functional**
- A new user can register, log in, and create a company account.
- An authenticated user can submit feedback and receive a sentiment label, and the feedback and label are stored.
- Feedback history can be filtered by sentiment and date range, and results are paginated.
- Dashboard totals equal the count of stored records for that company.
- Sentiment distribution percentages sum to 100 percent (within rounding).
- Trend data is grouped by day or week using the Africa/Lagos time zone.

**Security and privacy**
- A user from Company A cannot read, list, or modify Company B's feedback. This is verified by explicit automated tests, including direct requests for another company's record IDs.
- Unauthenticated requests to protected endpoints are rejected.
- Passwords are hashed using Django's standard mechanisms, and no secrets are committed to the repository.
- Demonstrations use only synthetic or properly authorized and anonymized feedback.

**Machine learning**
- The dataset's source, license, and label definitions are documented before training.
- Class distribution, duplicates, and missing values are audited, and a train, validation, and test split is created without leakage.
- A baseline model is trained reproducibly and evaluated on a held-out test set, reporting per-class precision, recall, and F1.
- Errors are inspected and documented, including difficult Pidgin and mixed-language examples.
- Numeric performance targets are set only after the baseline has been measured. No performance figure is claimed before then.

**Quality and delivery**
- The API is documented.
- The interface is usable on a phone-sized screen and a desktop screen.
- The automated test suite passes, and the application is deployed with HTTPS.

## 6. Assumptions and open questions

**Not yet validated**
- No customer interviews or market research have been done. Everything about Ada's role, tools, workflow, and pain points is assumed.
- It is a **hypothesis** that conventional sentiment analysis tools handle Nigerian Pidgin, slang, and code-switched text poorly. This has not been tested in this project.
- It is unknown whether three-class sentiment is enough for a CX team, or whether they would need issue categories sooner.
- It is unknown which feedback channels matter most to the target users, and whether single-item submission is a workable starting point.
- Willingness to use or pay for the product has not been tested.

**Technical and data questions**
- Which datasets exist for Nigerian Pidgin sentiment, and what are their licenses, label definitions, and quality? (Dataset research is a prerequisite for training.)
- How will neutral feedback be defined consistently?
- How should data be handled in line with Nigerian data protection requirements (for example the Nigeria Data Protection Act)? Applicable obligations need to be verified before real customer data is processed.

**Validation plan (not yet started)**
- Find and interview customer experience or support professionals when access becomes available.
- Test the chosen model against a small set of hand-labeled, authorized or synthetic Pidgin examples.
- Revise this document after each round of evidence.

## 7. Change history

| Date | Version | Change |
|---|---|---|
| 2026-10-16 | 0.1 | Initial draft |