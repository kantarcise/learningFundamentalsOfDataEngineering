## 10. Security and Privacy 🛡️

Security in data engineering is not optional—it’s foundational.

As custodians of sensitive data, data engineers must prioritize security at every stage of the data lifecycle. 

Beyond protecting infrastructure, strong security builds trust, ensures regulatory compliance, and prevents damaging breaches that could derail careers and companies.

### People

Humans are the weakest link in the security chain. 

Engineers should adopt a defensive mindset, practice negative thinking to anticipate worst-case scenarios, and be cautious with sensitive data and credentials. 

Ethical concerns about data handling should be raised, not buried.

### Processes

Security must be habitual, not theatrical. Many organizations prioritize compliance checklists ([SOC-2](https://www.imperva.com/learn/data-security/soc-2-compliance/), [ISO 27001](https://www.iso.org/standard/27001)) without truly securing systems.

Embed active security thinking into the culture, regularly audit risks, and exercise the principle of least privilege by granting only necessary access, only for the time it’s needed. 

[See this doc](https://cloud.google.com/iam/docs/using-iam-securely#least_privilege) from Google Cloud as an example.

Understand your [shared responsibility](https://learn.microsoft.com/en-us/azure/security/fundamentals/shared-responsibility) when using the cloud.

### Technology

Keep your software patched and systems updated (Good Luck).

Use **encryption at rest** and **in transit** to protect against basic attacks, but remember encryption alone won’t prevent human errors. Ensure network access is locked down—never expose databases or cloud instances to public IPs without strict controls. 

Regular logging, monitoring, and alerting will help detect anomalies in **access**, **resource usage**, or **billing** that could signal a breach.

#### Data Backups and Disaster Preparedness

Always back up your data and test restore procedures regularly.

In the era of ransomware, recovery readiness is as vital as prevention. Don’t wait for disaster to find out your backups are broken.

#### Security at a Technical Level

Security risks exist even at the hardware and low-level software layer—e.g., vulnerabilities in logging libraries, microcode, or memory. 

While this book focuses on higher-level pipelines, engineers working closer to storage and processing must stay vigilant and up-to-date.

#### Internal Security Awareness

Encourage engineers to be active security contributors within their domains. Familiarity with specific tools gives them a unique vantage point to identify potential flaws. 

Security shouldn’t be siloed—it should be everyone’s responsibility.

### Conclusion

Security is not just a policy—it’s a habit. Treat data like your most valuable possession. 

While you may not be the lead on security at your company, by practicing good security hygiene, staying alert, and keeping security front of mind, you play a key role in protecting your organization’s data.

---

