![Copilot Chat, HIPAA & Web Search](https://github.com/heyitsgoad/copilot-chat-hipaa-websearch/blob/main/assets/hero-banner.png?raw=true)

# Copilot Chat, HIPAA, and Web Search

Almost every week, an IT or security team in a regulated industry asks me some version of the same question: *"Is Copilot Chat actually safe for HIPAA? And what happens when it searches the web?"*

It is a fair question, and most of the confusion comes from blending two different things together. The core Copilot Chat experience behaves one way. Web search behaves another. Once you separate them, the whole picture gets a lot clearer, and the decision gets a lot easier.

So here is the plain-language breakdown I give people, with every claim grounded in Microsoft Learn documentation. There is a short version up top, then a walk through each piece, what it means, and how it impacts a regulated environment.

> [!NOTE]
> **The short version:** Copilot Chat (the enterprise, no-cost experience) keeps your prompts and responses inside your Microsoft 365 service boundary, covered by Enterprise Data Protection and your BAA. The one component that reaches outside that boundary is **web search** (the "From the web" results powered by Bing). Web search is **not** covered by the BAA, by design, for every customer. Even so, what actually leaves the tenant is a short, de-identified keyword query over an encrypted connection, and you have several layers of control and visibility over it.

---

## 1. What stays inside your tenant (covered by your BAA)

When a user types a prompt into Copilot Chat and Copilot answers using your data or its own reasoning, that whole interaction stays inside the Microsoft 365 service boundary:

- Prompts and responses are protected by **Enterprise Data Protection (EDP)**, the same contractual terms (Data Protection Addendum plus Product Terms) that already cover your email in Exchange and your files in SharePoint.
- It is **encrypted at rest and in transit**, with tenant isolation between customers.
- It **inherits your existing governance**: your identity model and access controls, sensitivity labels, retention policies, and audit. Copilot only ever surfaces data the individual user already has permission to see.
- Your prompts and responses are **never used to train foundation models**.
- This portion is covered by the BAA.

So for the core experience, the answer to "does it stay in our tenant and inherit our DLP, audit, and access controls" is yes, depending on which controls have been implemented and the licensing level.

## 2. What happens when web search is involved (the part outside the BAA)

This is the piece that causes most of the confusion, and the instinct that web search behaves differently is correct.

When web search is enabled and Copilot decides the web would improve an answer, it does **not** send your prompt to the internet. Instead it generates a short search query (literally a few keywords, very similar to what you would type into Google) and sends only that to the **Bing search service**. The Bing search service runs separately from Microsoft 365, under the Microsoft Services Agreement, with Microsoft acting as an independent data controller. Because it sits outside the M365 boundary, the BAA does not extend to it. That is true for every customer, and it is stated plainly in Microsoft's documentation.

Here is what that actually means in practice, because the protections layered on top matter:

- **Only keywords leave, never the full picture.** The following are explicitly never sent to Bing: your entire prompt (unless it is something trivial like "local weather"), entire emails, documents, or uploaded files, entire web pages or PDFs being summarized, and any identifying information from Entra ID (username, domain, or tenant ID).
- **It is de-identified.** User and tenant identifiers are stripped before the query is sent, so there is no "this came from this organization" attached to it.
- **It is encrypted and protected in transit.** The query travels over a secure, encrypted connection specifically so it cannot be intercepted or altered in flight.
- **It is logged in the Bing search service for a limited duration** and is only accessible by authorized Microsoft employees.
- **It is not monetized or used to train.** These queries are treated as customer confidential. They are not shared with advertisers, not used to build advertising profiles, not used to improve Bing, and not used to train generative AI foundation models.

So while web search is technically outside the scope of the BAA, the data that crosses the boundary is minimized, anonymized, and encrypted.

## 3. Yes, it is fully auditable (the part most people miss)

Everything in Copilot Chat carries an audit trail in Microsoft and can be retrieved with Purview, and this works on both **E3 and E5**:

- The user's prompt, Copilot's response, and the **exact web search keywords** Copilot sent to Bing are all logged and reviewable. You can run search, audit, and eDiscovery against them using the same Purview tools your compliance team already uses.
- Users themselves see the exact web queries that were run, shown as citations in the response, so there is transparency at the point of use too.

The difference between E3 and E5 here is mostly experience, not coverage: **E5 (with DSPM for AI)** gives you a single pane where you can see the original prompt, the response, the web keywords, and the supporting resources side by side as one storyline. E3 has the underlying audit data; E5 makes the investigation faster.

## 4. Where E5 gets you proactive PHI control

This is the strongest answer to "how do we keep sensitive data from going out." With **E5 and DLP for Microsoft 365 Copilot**, you move from detective (auditing after the fact) to **preventive**:

- DLP can detect **sensitive information types** (Social Security numbers, credit card numbers, MRNs, and other PHI patterns) using deep content analysis, not a simple text scan.
- You can configure policy so that content carrying certain sensitivity labels is excluded from Copilot processing, and use DSPM for AI plus collection policies to flag or restrict sensitive material in the AI interaction itself.
- Practically, this lets you put guardrails in place so the most sensitive data is governed before it is ever part of a prompt or a generated query.

## 5. Direct answers to the common questions

**Does data stay in our tenant and inherit our DLP, audit, and access controls?**
For prompts and responses, yes, fully. For web search, a minimized, de-identified keyword query leaves the tenant by design.

**Are Bing / web searches covered under our BAA?**
No. Web search is outside the BAA for all customers. It is protected by Microsoft commitments (encryption, de-identification, no training, no ads).

**What does "web search" actually cover?**
It covers the "From the web" grounding results. It is triggered only when Copilot judges the web will improve an answer, and only a few generated keywords are sent and stored, not your prompt or response.

**Retention, PHI monitoring, compliance posture?**
Prompts, responses, and web keywords follow your retention policies and are auditable in Purview on E3 / E5. PHI monitoring is detective on E3 and can be made preventive on E5 via DLP and DSPM for AI.

## 6. Common adoption patterns in regulated environments

To put it in context, organizations in healthcare and other regulated industries tend to land in a few common places once they understand the keyword-only, de-identified, encrypted reality of web search:

- **Enable Copilot Chat and keep web search on.** The actual data crossing the boundary is minimal and anonymized, and the productivity value is high. Purview auditing provides assurance.
- **Start conservative, then expand.** Disable web search via the admin controls (or the user-level Web content toggle) during an initial rollout, then turn it on once security and compliance teams have reviewed the audit trail and DLP posture.
- **Lead with E5 controls for the most risk-averse.** Deploy E5 DLP for Copilot and DSPM for AI so PHI controls are preventive before broad adoption.

## 7. Recommended best practices for rollout

1. **Turn on Purview Audit** (it is on by default for newer tenants) and confirm Copilot interactions, including web search keywords, are being captured. This gives leadership evidence, not assurances.
2. **Decide your web search posture deliberately.** You can leave it on (recommended for value, given the protections), or scope it down with admin controls during a pilot. Either way, make it an explicit decision rather than a default.
3. **On E5, deploy DLP for Microsoft 365 Copilot** targeting your PHI sensitive information types and sensitivity labels, so the most sensitive content is governed proactively.
4. **Train users on the boundary, simply:** Copilot Chat for internal reasoning over your data is fully covered; web search returns public information and should not be treated as a place to paste PHI. Pairing the human guardrail with the technical one is what mature organizations do.
5. **Phase the rollout:** pilot group, review the audit data together, then expand. It gives the compliance team a clean before-and-after story.

> [!TIP]
> **Bottom line:** The core Copilot Chat experience stays inside your protected boundary and is covered by your BAA. Web search is the one piece that reaches out, and it reaches out with a few anonymized keywords over an encrypted connection, fully logged and auditable. This is a very governable set of trade-offs, not a reason to avoid the tool.

---

## The full write-up as a PDF

Here is the same guide as a formatted, shareable one-pager. Feel free to download it and pass it to your security and compliance team.

![Copilot Chat, HIPAA & Web Search (page 1)](https://github.com/heyitsgoad/copilot-chat-hipaa-websearch/blob/main/assets/page-1.png?raw=true)

![Copilot Chat, HIPAA & Web Search (page 2)](https://github.com/heyitsgoad/copilot-chat-hipaa-websearch/blob/main/assets/page-2.png?raw=true)

![Copilot Chat, HIPAA & Web Search (page 3)](https://github.com/heyitsgoad/copilot-chat-hipaa-websearch/blob/main/assets/page-3.png?raw=true)

**[Download the PDF: Copilot Chat, HIPAA & Web Search](https://github.com/heyitsgoad/copilot-chat-hipaa-websearch/blob/main/copilot-chat-hipaa-websearch.pdf)**

---

## Microsoft documentation for your security and compliance teams

Every claim above is grounded in Microsoft Learn. Here are the primary sources:

| Topic | Microsoft Learn |
|---|---|
| Copilot Chat: privacy and protections (architecture, what stays in-boundary, the green shield) | [learn.microsoft.com](https://learn.microsoft.com/en-us/copilot/privacy-and-protections) |
| Enterprise Data Protection (terms covering prompts/responses, plus the BAA/HIPAA note on web search) | [learn.microsoft.com](https://learn.microsoft.com/en-us/copilot/microsoft-365/enterprise-data-protection) |
| Data, privacy, and security for web search (what is and is not sent to Bing, keyword examples, logging) | [learn.microsoft.com](https://learn.microsoft.com/en-us/copilot/microsoft-365/manage-public-web-access) |
| Data, privacy, and security for Microsoft 365 Copilot (HIPAA, permissions, training commitments) | [learn.microsoft.com](https://learn.microsoft.com/en-us/copilot/microsoft-365/microsoft-365-copilot-privacy) |
| Purview audit log activities (where prompts, responses, and web queries are logged) | [learn.microsoft.com](https://learn.microsoft.com/en-us/purview/audit-log-activities) |
| Retention for Copilot and AI apps (how prompts/responses are retained and deleted) | [learn.microsoft.com](https://learn.microsoft.com/en-us/purview/retention-policies-copilot) |
| Learn about Data Loss Prevention (SSN, credit card, health records) | [learn.microsoft.com](https://learn.microsoft.com/en-us/purview/dlp-learn-about-dlp) |
| Data Security Posture Management (DSPM) for AI (proactive monitoring, full storyline, risk assessments) | [learn.microsoft.com](https://learn.microsoft.com/en-us/purview/dspm-for-ai) |

---

> [!IMPORTANT]
> This is a personal educational summary created by Michael Goad to help IT and security teams reason about Copilot Chat, HIPAA, and web search. It is **not** official Microsoft guidance and does not constitute legal or compliance advice. Product behavior, licensing, and terms change over time, so always verify against the linked Microsoft Learn documentation and validate any compliance decision against your own environment, licensing, BAA, and legal counsel. Specific controls vary by Microsoft subscription plan.

Curious how others are handling the web search conversation with their compliance teams. If you have a pattern that works, I would love to hear it.
