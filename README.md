# Copilot Chat, HIPAA, and Web Search

A plain-language educational resource for IT and security teams in regulated industries.

It answers the questions that come up constantly when a healthcare or regulated organization evaluates Microsoft 365 Copilot Chat:

- What stays inside the Microsoft 365 tenant boundary and is covered by the BAA?
- What actually happens when Copilot uses web search, and why is it outside the BAA?
- Exactly what data crosses the boundary to Bing, and what does not?
- How is all of this audited, retained, and governed in Microsoft Purview?
- Where does E5 (DLP for Copilot, DSPM for AI) add proactive PHI controls?
- What rollout best practices do mature organizations follow?

## Read it

- **PDF:** [copilot-chat-hipaa-websearch.pdf](copilot-chat-hipaa-websearch.pdf)
- **HTML source:** [copilot-chat-hipaa-websearch.html](copilot-chat-hipaa-websearch.html)

## Key takeaway

The core Copilot Chat experience (prompts and responses) stays inside your protected Microsoft 365 boundary under Enterprise Data Protection and your BAA. The one piece that reaches out is web search, and it reaches out with a few anonymized keywords over an encrypted connection, with user and tenant identifiers stripped, fully logged and auditable in Purview. That is a governable trade-off, not a reason to avoid the tool.

## Sources

Every claim is grounded in Microsoft Learn documentation, linked at the end of the document. Primary references:

- [Microsoft 365 Copilot Chat: privacy and protections](https://learn.microsoft.com/en-us/copilot/privacy-and-protections)
- [Enterprise Data Protection in Microsoft 365 Copilot and Copilot Chat](https://learn.microsoft.com/en-us/copilot/microsoft-365/enterprise-data-protection)
- [Data, privacy, and security for web search](https://learn.microsoft.com/en-us/copilot/microsoft-365/manage-public-web-access)
- [Data, privacy, and security for Microsoft 365 Copilot](https://learn.microsoft.com/en-us/copilot/microsoft-365/microsoft-365-copilot-privacy)
- [Purview audit log activities](https://learn.microsoft.com/en-us/purview/audit-log-activities)
- [Retention for Copilot and AI apps](https://learn.microsoft.com/en-us/purview/retention-policies-copilot)
- [Learn about Data Loss Prevention](https://learn.microsoft.com/en-us/purview/dlp-learn-about-dlp)
- [Data Security Posture Management (DSPM) for AI](https://learn.microsoft.com/en-us/purview/dspm-for-ai)

## Disclaimer

This is a personal educational summary created by Michael Goad. It is **not** official Microsoft guidance and does not constitute legal or compliance advice. Product behavior, licensing, and terms change over time. Always verify against the linked Microsoft Learn documentation and validate any compliance decision against your own environment, licensing, BAA, and legal counsel. Specific controls vary by Microsoft subscription plan.
