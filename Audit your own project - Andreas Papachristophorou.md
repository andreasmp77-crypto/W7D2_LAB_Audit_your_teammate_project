# W7D2_LAB_EU_AI_ACT_Audit_your_teammate_project
- Week 7 / Day 2
- Student: Andreas Papachristophorou
- Course: AI Consulting & Integration 2026-07
- Date: 2026-08-19
---

# Phase 1: Read and annotate

## First read — no notes

## System Brief — EU Policy RAG Assistant

### What does the system do?

The system allows employees to ask questions about relevant EU legislation and policy guidance through a chat interface. It searches a controlled collection of policy documents stored by the organization, identifies passages that are relevant to the employee's question, and uses those passages to generate a written answer.

The system is designed to answer questions **only using information contained in the organization's approved document collection**. If the available documents do not contain sufficient information to answer a question, the system should indicate that it cannot provide an adequate answer and direct the employee to the organization's policy team for further assistance.

The purpose of the system is to reduce the number of routine questions that policy staff need to answer manually and provide employees with faster access to existing policy information.

### What inputs does it take?

The system has two primary types of input:

**1. Knowledge-base documents**

Documents are provided through a designated Google Drive folder. These documents contain EU legislation, regulatory guidance, and related policy information. Documents may be added, modified, or removed over time.

The prototype supports text-based documents and is being extended to support PDFs.

The documents are processed by n8n, divided into smaller sections, converted into numerical representations using an embedding model, and stored in a Pinecone vector database so that relevant sections can be retrieved when a question is submitted.

The documents are organizational policy materials rather than customer records. The intended knowledge base does **not require personal data** to perform its function. However, the system operates within an enterprise environment, so access permissions and the possibility of personal or confidential information being inadvertently included in documents need to be considered before production deployment.

**2. Employee questions**

An employee submits a natural-language question through the chat interface. In the prototype, the question is passed through an n8n workflow, which searches the vector database for relevant document sections.

### What does the system output?

The primary output is **generated text**: an answer to the employee's question based on the relevant sections retrieved from the organization's documents.

The system can also produce an **insufficient-information/escalation response** when the available documents do not provide an adequate basis for answering the question.

The intended production response should identify the source document or documents used to formulate the answer, allowing the employee and policy team to verify the information.

The system does **not make an automated legal, employment, financial, or other binding decision**. It provides informational responses based on the organization's supplied documents.

### Who is affected by the output?

The immediate users are employees of the organization, particularly employees in departments that regularly require information about EU legislation and policy.

The policy department is also affected because the system is intended to reduce routine information requests while providing a mechanism for escalating questions that cannot be adequately answered.

There is no intended direct impact on customers or members of the public.

### Does a human review the output?

**Yes, during the evaluation and initial implementation phase.**

A specialist from the policy department reviews generated responses to determine whether they provide an adequate and appropriately grounded answer to the employee's question.

The review is used to evaluate system performance and identify cases where the retrieval process or generated response needs improvement.

In the proposed production model, routine responses would be provided directly to employees without requiring manual approval of every individual response. Questions for which the system cannot find sufficient supporting information should instead be escalated to the policy department.

This creates a **human-in-the-loop escalation model** rather than requiring human approval of every automated response.

### Who built it?

The prototype was designed and implemented by **the project owner**, using existing organizational technology and third-party AI infrastructure.

The workflow was configured in **n8n**, with Google Drive used as the document source, Pinecone used as the vector database, OpenAI used for language-model processing, and Slack intended as the employee-facing conversational interface.

The project owner is responsible for the workflow design, configuration, testing, and iteration. The underlying platforms and models are provided by third-party technology providers.

### Who would use it in production?

The intended production users are **employees of the organization**, particularly employees who currently contact the policy department for information about EU legislation and related policy guidance.

The **policy department** would remain responsible for the underlying source material, validating system performance, and answering questions that the system cannot adequately address.

The organization's IT/technology function would be responsible for supporting the production environment, access controls, integrations, and operational security.

### System boundary

The system's responsibility is limited to retrieving and presenting information contained within the organization's approved policy-document collection. It is **not intended to independently research the internet, interpret legislation beyond the supplied material, provide authoritative legal advice, or make decisions on behalf of the organization**.

The key audit question is therefore not simply whether the system produces plausible answers, but whether it **retrieves the appropriate organizational information and produces answers that remain grounded in that information without introducing unsupported claims**.

## Second read — annotations

### Elements that affect the risk tier classification

Add anything from the brief that may change whether the system is prohibited, high-risk, limited risk / transparency, or minimal risk.

- The system is an internal **EU policy RAG assistant** that retrieves approved organizational policy documents and generates informational answers for employees.
- It does **not** make automated legal, employment, financial, or other binding decisions. This points away from high-risk classification under Annex III.
- The system does **not** directly affect customers, members of the public, employment decisions, access to essential services, law enforcement, education, migration, biometric identification, or critical infrastructure.
- The system creates **generated text** in response to employee questions. This may create a transparency issue because employees should understand that the answer is AI-generated and should verify important answers against the cited source documents.
- The system interacts with employees through a chat interface, likely Slack. This supports a **limited risk / transparency** classification rather than minimal risk, because users are directly interacting with an AI system.
- The intended knowledge base does **not require personal data**, which reduces GDPR and high-risk concerns. However, accidental inclusion of personal or confidential information in Google Drive documents remains a deployment risk.
- Human review exists during evaluation and initial implementation, but routine production answers may be given without manual approval. This means the system needs clear boundaries, source citations, escalation rules, and policy-team oversight.
- The system boundary is limited: it should not independently research the internet, give authoritative legal advice, or make decisions for the organization. This limitation helps keep the risk lower if it is enforced in production.

### Elements that are unclear

Add anything you would need to clarify before forming a final view.

- It is unclear whether employees will be clearly told that they are interacting with an AI system rather than a human policy specialist.
- It is unclear whether every answer will include source citations or links to the exact policy documents used. This matters because the system could otherwise sound confident while giving an unsupported answer.
- It is unclear who approves new documents before they are added to the Google Drive knowledge base. If unapproved, outdated, or incorrect documents are added, the system may produce unreliable answers.
- It is unclear how access permissions will work. For example, can all employees ask about all EU policy documents, or should some documents be limited to specific teams?
- It is unclear whether confidential information or personal data could accidentally be included in the Google Drive folder and then processed by n8n, Pinecone, OpenAI, or Slack.
- It is unclear whether employee questions and AI answers will be logged, how long logs will be kept, and who can access them.
- It is unclear what happens when the system gives a wrong or incomplete answer that an employee relies on. The brief says escalation should happen when information is insufficient, but the exact escalation and correction process is not fully described.
- It is unclear whether the system is only for general policy information or whether employees may ask for legal advice, compliance decisions, or interpretations of legislation for real business cases.

### Elements that suggest a specific obligation applies

Add anything that points to EU AI Act, GDPR, consumer protection, sector regulation, or other obligations.

- **EU AI Act — transparency duties:** Because employees interact with a chat interface and receive AI-generated text, the organization should clearly tell users that they are using an AI assistant. Employees should not be led to believe the answer comes directly from a human policy specialist.
- **EU AI Act — generated-content transparency:** The system generates written answers. The answers should identify that they are AI-generated or AI-assisted and should include source references where possible.
- **EU AI Act — not high-risk based on current brief:** The system does not make or strongly influence binding decisions in employment, finance, education, law enforcement, migration, critical infrastructure, or access to essential services. This suggests the 11 high-risk provider obligations are probably not triggered at this stage.
- **GDPR — personal data risk:** The intended knowledge base does not require personal data, but personal or confidential information could accidentally be uploaded into Google Drive or entered in employee questions. This means data minimisation, access control, retention rules, and processor/vendor checks are relevant.
- **GDPR — vendor/data transfer risk:** n8n, Pinecone, OpenAI, Google Drive, and Slack may process documents, employee questions, logs, or retrieved text. The organization should check data-processing agreements, storage locations, access rights, and whether any personal data is sent outside the EU/EEA.
- **Information security:** Because the system uses enterprise documents and several connected tools, access controls are important. Employees should only retrieve documents they are allowed to see.
- **Policy governance:** The policy department should own and approve the document collection. If old, draft, or unapproved documents are indexed, the AI may give misleading answers.
- **Professional/legal boundary:** The system should not present itself as giving authoritative legal advice. It should provide policy information based on approved sources and escalate uncertain or legal-advice questions to the policy or legal team.

## Evidence notes

Use this table to collect evidence from the teammate’s brief.

| Evidence from brief | Why it matters | Possible legal/compliance link |
| --- | --- | --- |
| The system lets employees ask questions about EU legislation and policy guidance through a chat interface. | Employees are directly interacting with an AI assistant, so they should know they are not speaking to a human policy specialist. | EU AI Act Article 50 transparency duties for AI interaction; internal user notice and clear labelling. |
| The system answers only from the organization’s approved document collection and should escalate when information is insufficient. | This lowers risk because the system is designed as a controlled information tool, not an open-ended legal adviser or decision-maker. | Supports limited risk classification; requires source grounding, escalation rules, and policy governance. |
| The system produces generated text answers and should identify the source documents used. | Generated answers can sound confident even when wrong, so source references are important for verification and trust. | AI Act transparency and good governance; risk of misleading users if citations or caveats are missing. |
| The brief says the system does not make automated legal, employment, financial, or other binding decisions. | This points away from prohibited or high-risk classification because the output does not directly decide people’s rights or access to important services. | Likely not Annex III high-risk on current facts; still needs monitoring if the use expands. |
| The intended knowledge base does not require personal data, but personal or confidential information could accidentally be included. | Even if personal data is not needed, accidental inclusion could create privacy and security risks. | GDPR data minimisation, access control, retention limits, and vendor processor checks. |
| The workflow uses n8n, Google Drive, Pinecone, OpenAI, and Slack. | Several third-party tools may handle documents, questions, embeddings, logs, or generated answers. | GDPR processor agreements, international transfer checks, security review, and vendor due diligence. |

---

# Phase 2: First-pass classification

## Classification table

| Question | Your answer |
| --- | --- |
| Does this system fall under any prohibited category (Article 5)? | No. The system is an internal policy-support tool, and none of the prohibited categories in Article 5 appear to be triggered. It does not use manipulative techniques, social scoring, biometric identification, emotion recognition, or predictive policing. |
| Does this system operate in any of the eight Annex III areas? | No, based on the current brief. The system provides general internal information about EU legislation and policy guidance. It does not directly decide or significantly influence people’s rights, employment, education, access to essential services, law enforcement, migration, critical infrastructure, or other Annex III areas. |
| If Annex III: does it significantly influence decisions in that area, or is it narrow/preparatory? | Not applicable on the current facts, because the system does not appear to fall within an Annex III high-risk area. |
| Does this system interact with end users or generate content requiring disclosure (Article 50)? | Yes. Employees interact directly with the AI assistant through a chat interface, and the system generates written answers. Employees should therefore be clearly informed that they are interacting with an AI system, not a human policy specialist. |
| First-pass risk tier | Limited risk / transparency. Controls are required under Article 50, especially clear disclosure that the user is interacting with an AI assistant. The output should also include source references and should be sense-checked or escalated when the answer is uncertain, incomplete, or potentially legal in nature. |
| One-sentence justification citing the specific article or Annex entry | The EU Policy RAG Assistant is best classified as limited risk / transparency under Article 50 because employees interact directly with an AI system that generates policy-information answers, while the system does not appear to fall under Article 5 prohibited practices or Annex III high-risk use cases. |

## Uncertainty between tiers

The classification could change if the system is later used to provide legal advice, make compliance decisions, or strongly influence employment, financial, or other important organizational decisions. 

## Clarifying questions that would resolve the uncertainty

- Question 1: Will the system be used only for general internal policy information, or could it be used for case-specific legal or compliance advice?
- Question 2: Will employees be clearly informed that they are interacting with an AI assistant?
- Question 3: Could the knowledge base, employee questions, logs, embeddings, or generated answers contain personal or confidential data?

---

# Phase 3: Clarifying questions log

## Clarifying question 1

- **Question:** What is the procedure for approving and uploading policy documents into the Google Drive folder, and how often are those documents reviewed or updated?
- **What I need to know:** Whether only approved, current, and accurate documents are included in the knowledge base.
- **Why it matters for risk classification or obligation mapping:** If outdated, draft, or incorrect documents are indexed, the system could generate unreliable policy answers. This would increase governance, monitoring, and escalation requirements.
- **Provisional assumption if I do not get an answer:** I assume that a policy owner reviews and approves documents before they are indexed, and that the knowledge base is updated regularly.

## Clarifying question 2

- **Question:** Will the system be used only for general internal policy information, or could employees use it for case-specific legal advice, compliance decisions, or decisions in Annex III areas such as employment, essential services, law enforcement, migration, or justice?
- **What I need to know:** Whether the system remains a general information tool or whether it could influence important legal, employment, financial, or compliance decisions.
- **Why it matters for risk classification or obligation mapping:** If the system is used to make or strongly influence decisions in an Annex III area, the classification could change from limited risk / transparency to high-risk, triggering additional AI Act obligations.
- **Provisional assumption if I do not get an answer:** I assume the system is limited to general internal policy information and does not make or strongly influence binding decisions.

## Clarifying question 3

- **Question:** Could the Google Drive documents, employee questions, chat logs, embeddings, or generated answers contain personal data, confidential information, or special-category data, and how are these data protected?
- **What I need to know:** Whether personal or confidential data may be processed by Google Drive, n8n, Pinecone, OpenAI, Slack, or system logs.
- **Why it matters for risk classification or obligation mapping:** Even if the system is not high-risk under the AI Act, GDPR and information-security obligations may still apply. This affects data minimisation, access control, retention, processor agreements, and possible international transfer checks.
- **Provisional assumption if I do not get an answer:** I assume the intended knowledge base does not require personal data, but accidental inclusion remains a risk that must be controlled before production deployment.

---

# Phase 4: Audit report

## Section 1: System summary

The system is a chat-based assistant that enables employees to query EU legislation and organizational policy documents. It retrieves relevant passages from an approved internal repository and generates answers strictly grounded in those authorized sources. When available documents lack sufficient information, the assistant alerts the user and escalates the inquiry to the policy team. By automating routine lookups, the tool provides employees with faster access to guidance while reducing the manual workload on internal specialists.

## Section 2: Risk classification

The system is best classified as limited risk / transparency under Article 50 because employees interact directly with an AI assistant and receive AI-generated text. Based on the current brief, it does not fall under Article 5 prohibited practices or Annex III high-risk areas because it does not make or strongly influence binding legal, employment, financial, or public-facing decisions.

## Section 3: Role map

| Role | Entity | Key obligations | Notes |
| --- | --- | --- | --- |
| Provider | Project owner / consultant / system builder | Define the system’s intended use, provide clear instructions for use, document the workflow, explain system limits, and make sure appropriate contracts and vendor checks are in place. | The provider is the person or organisation that designs and provides the EU Policy RAG Assistant. If the user organisation heavily customises the system, brands it as its own product, or controls its market placement, it may also become the provider. |
| Deployer | User organisation | Use the system only for its approved purpose, train employees, provide clear AI disclosure, monitor outputs, maintain escalation routes to the policy team, and ensure GDPR-compliant handling of documents, questions, and logs. | The deployer is the organisation using the assistant internally. Based on the current brief, the system is not classified as high-risk, but the deployer still needs transparency, governance, data protection, and access-control measures. |
| Third-party vendors | n8n, Google Drive, Pinecone, OpenAI, and Slack | Provide secure and reliable services, support data protection requirements, make relevant documentation available, and help the provider/deployer assess privacy, security, logging, and model-related risks. | These vendors provide important parts of the workflow, but they do not decide the business purpose of the system. OpenAI may also have separate obligations for its general-purpose AI model, depending on the model and deployment context. |

## Section 4: Compliance findings

### Finding 1 — Scope creep into legal or high-impact decisions

- **Severity:** Significant
- **Description:** The current brief says the system is limited to general policy information, but the boundary between policy information, legal advice, and compliance decision-support needs to be confirmed. If employees use the assistant for case-specific legal advice or decisions in Annex III areas, the risk classification may change and additional AI Act obligations may apply.
- **Recommended action:** The client should confirm the intended use and ensure that, if the status changes, the required actions are implemented.
- **Escalation needed?** Yes — client, consultant, or AI expert.

### Finding 2 — AI transparency and escalation controls

- **Severity:** Significant
- **Description:** The system output should be clearly labelled as AI-generated, and a clear escalation path should be provided to the end user (especially for uncertain or high-impact questions). Appropriate training should also be arranged.
- **Recommended action:** Provide training and an easy escalation path through the system UI.
- **Escalation needed?** Yes — client, consultant, AI expert, and software backend/frontend developers.

### Finding 3 — Knowledge-base governance and accuracy

- **Severity:** Significant
- **Description:** Ensure that the knowledge base content is accurate and regularly scrutinised, and that a robust system is in place to maintain quality.
- **Recommended action:** Implement a distinct procedure to ensure the knowledge base is reviewed and kept up to date. Each answer should cite or link to the source document used, so employees and the policy team can verify the answer.
- **Escalation needed?** Yes — deployer, with the help of the consultant.

### Additional findings

Add more findings if needed.

## Section 5: Overall recommendation

### Selected recommendation

**Proceed with conditions — significant findings must be addressed before deployment.**

### Rationale

The system should be allowed to proceed only with conditions because the current brief suggests a limited risk / transparency system, not a prohibited or high-risk system. However, deployment should not proceed until the client confirms the system’s boundaries, clearly labels the tool as an AI assistant, provides source references, and creates an escalation route to the policy team for uncertain, incomplete, or potentially legal questions. The client should also make sure the Google Drive knowledge base is approved, accurate, and regularly reviewed, and that GDPR and information-security controls are in place for documents, employee questions, logs, embeddings, and third-party vendors. These conditions are important because a RAG assistant can sound confident even when it retrieves the wrong source or generates an unsupported answer.

## Section 6: What this report is not

This report is not a legal opinion, not a conformity assessment, and not a certification. Conclusions should be verified with legal counsel before any EU market placement.