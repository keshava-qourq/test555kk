# Product Requirements

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start narrative -->
## Summary

### REQ-SCOPE-001 · draft · source: requirements:scope

Defines the lightweight MVP delivery scope for TEST555 and the capabilities explicitly excluded from this build.

Delivery scope: lightweight (MVP), Requirements version 1. In-scope capabilities: document-grounded question answering over uploaded content; standalone chat page for visitors; multi-turn conversation within a session; source citations with answers; 'I don't know' fallback with no invented answers; admin upload and management of PDF/Word/text documents; document chunking, embedding and indexing; admin sign-in; admin content dashboard; OpenAI language model for answers; OpenAI embeddings for document search. Explicitly out of scope: website crawling and external-system connectors (Zendesk, Notion, Confluence, Google Drive); real-time live-agent handoff / human live chat.

**Related:** EPIC-001, EPIC-002, EPIC-003, EPIC-004, EPIC-005

- ⚠ AC-003: the embeddable website widget is assumed deferred beyond this MVP (only the standalone chat page ships) -- marked 'assumption to confirm' by the source and not yet confirmed.

### REQ-STACK-001 · approved · source: requirements:tech-stack

Technology stack approved by the product owner for TEST555: FastAPI backend, AWS cloud, PostgreSQL 16 database, React 19 frontend.

Backend: FastAPI 0.115 -- chosen because the RAG workload (LLM calls, embedding, vector search, document parsing) lives in Python's richest-maintained libraries, and FastAPI handles streamed token-by-token LLM replies for a responsive chat feel. Cloud: AWS -- runs the FastAPI service, Postgres database and S3 in one account, pinned to an EU region to support the GDPR-level privacy promise with audit trail, without operating own servers. Database: PostgreSQL 16 with pgvector -- holds documents/metadata, conversation logs, follow-up requests, admin accounts, and document embeddings in one store, simplifying backup/restore and GDPR delete-on-request at a few-thousand-conversations/month scale. Frontend: React 19 -- serves the embeddable chat widget, standalone chat page, and authenticated admin dashboard from one codebase/team. These are to be designed against exactly as specified, no substitutions.

**Related:** EPIC-001, EPIC-003, NFR-006

### REQ-SERVICES-001 · approved · source: requirements:services

Runtime services and required API keys/credentials approved by the product owner.

AI provider: OpenAI, saas, env OPENAI_API_KEY -- generates chatbot answers. Embeddings: OpenAI embeddings, saas, env OPENAI_API_KEY -- turns documents into searchable vectors, same provider/key as chat. Vector store: pgvector inside PostgreSQL (known) -- avoids a second database, keeps deletion-on-request in one place. File storage: Amazon S3, saas, env AWS_ACCESS_KEY_ID / AWS_SECRET_ACCESS_KEY / AWS_REGION / S3_BUCKET -- durable storage for uploaded PDF/Word/text files. Email: SendGrid, saas, env SENDGRID_API_KEY -- delivers staff notification on captured follow-up requests. Queue: Redis (container redis:7-alpine) -- background queue so document parsing/embedding doesn't block the admin upload UI. Full required env/API key list: AWS_ACCESS_KEY_ID, AWS_REGION, AWS_SECRET_ACCESS_KEY, OPENAI_API_KEY, REDIS_URL, S3_BUCKET, SENDGRID_API_KEY.

**Related:** US-003, US-007, US-010, US-012, NFR-001

- ⚠ AC-016: email-sending credential (SMTP or SendGrid) and configurable staff inbox address are marked 'assumption to confirm'.
- ⚠ AC-007: the specific vector store and LLM API credential (OPENAI_API_KEY) are marked 'assumption to confirm' pending engineering-team sign-off.

### EPIC-001 · draft · source: requirements:EPIC-001

Core visitor-facing chatbot: a standalone chat page delivering retrieval-augmented, multi-turn, cited answers with an honest fallback when unsure.

US-001 [critical] Standalone chat page for visitors: anonymous access, no login (AC-001); responsive layout for mobile/desktop, web-only (AC-002); MVP delivers only the standalone page, embeddable widget assumed deferred (AC-003, assumption to confirm). US-002 [high] Multi-turn conversation within a session: bot uses session context for follow-up questions (AC-004); a new session carries no prior context (AC-005). US-003 [critical] Grounded answers via RAG: retrieves most relevant passages and generates answers grounded only in retrieved content (AC-006); OpenAI LLM generates answers and OpenAI embeddings power search, vector store choice and LLM credential (e.g. OPENAI_API_KEY) flagged as assumption to confirm (AC-007). US-004 [high] Source citations: citations shown when an answer derives from retrieved passages (AC-008); no fabricated/misleading citation shown when no clear source exists (AC-009). US-005 [critical] 'I don't know' fallback: bot states it doesn't know rather than inventing an answer when it cannot confidently answer (AC-010); offers the visitor the option to leave contact details for staff follow-up (AC-011).

**Related:** US-001, US-002, US-003, US-004, US-005, EPIC-002, NFR-003

- ⚠ AC-003: embeddable widget deferral is an unconfirmed assumption.
- ⚠ AC-007: vector store choice and LLM API credential are an unconfirmed assumption.

### EPIC-002 · draft · source: requirements:EPIC-002

Captures visitor contact details and question when the bot cannot answer, stores the request, and notifies staff by email; admins can review and mark requests handled.

US-006 [high] Capture on fallback: form collects contact detail and question after fallback is triggered (AC-012); on submit, contact detail, question and timestamp are stored for admin review (AC-013); without visitor consent, contact details are not stored and submission is blocked (AC-014). US-007 [high] Email notification to staff: stored follow-up triggers an email with question and contact detail to a configured staff inbox (AC-015); requires an email-sending credential (SMTP or SendGrid) and configurable inbox address, flagged as assumption to confirm (AC-016); if email delivery fails, the request remains stored and the failure is logged (AC-017). US-008 [high] Admin review and action: admin view lists each request with contact detail, question, timestamp and status (AC-018); admin can mark a request handled and the status updates in the list (AC-019).

**Related:** US-006, US-007, US-008, EPIC-001, EPIC-005

- ⚠ AC-016: email credential and configurable staff inbox are an unconfirmed assumption.

### EPIC-003 · draft · source: requirements:EPIC-003

Authenticated admin area for signing in and uploading, replacing, removing, and automatically processing source documents (PDF, Word, text) into the vector store.

US-009 [critical] Admin sign-in: valid credentials grant admin area access (AC-020); invalid/unauthenticated attempts are denied (AC-021); anonymous visitors never require or are offered admin access, flagged as assumption to confirm (AC-022). US-010 [critical] Upload source documents: PDF/Word/plain-text uploads are stored and queued for processing (AC-023); unsupported formats are rejected with a clear message listing supported formats, with the PDF/Word/plain-text-only list flagged as an assumption (AC-024). US-011 [high] Replace and remove documents: removing a document deletes it and its indexed content so it no longer informs answers (AC-025); replacing a document supersedes previous indexed content with the reprocessed new content (AC-026). US-012 [critical] Chunking, embedding, indexing: uploaded documents are split into chunks, embedded with OpenAI embeddings, and stored in the vector store (AC-027); processing status (processing/indexed/failed) is visible to the admin (AC-028).

**Related:** US-009, US-010, US-011, US-012, NFR-001, NFR-005

- ⚠ AC-022: visitors-never-get-admin-access is an unconfirmed assumption.
- ⚠ AC-024: PDF/Word/plain-text as the exhaustive supported-format list is an unconfirmed assumption.

### EPIC-004 · draft · source: requirements:EPIC-004

Admin dashboard listing managed documents and status, plus a conversation-history view for reviewing visitor interactions and answer quality.

US-013 [high] Admin content dashboard: lists uploaded documents with name, format and processing/indexing status (AC-029); list reflects the current document set after additions/removals (AC-030). US-014 [medium] Review conversation history: admin can list past conversations with messages and timestamps (AC-031); selecting a conversation shows the full multi-turn visitor/bot exchange (AC-032).

**Related:** US-013, US-014, EPIC-003

### EPIC-005 · draft · source: requirements:EPIC-005

GDPR-level privacy standard: a visible privacy notice, explicit consent before capturing contact details, and admin-driven deletion of a visitor's personal data on request.

US-015 [high] Privacy notice: a privacy notice is accessible from the chat interface explaining how conversation and contact data is handled (AC-033). US-016 [high] Explicit consent: the follow-up form requires explicit consent before submission; submission is blocked without it (AC-034); if a visitor declines or skips the form, no contact details are stored (AC-035). US-017 [medium] Data deletion on request: a signed-in admin can delete a visitor's personal data (contact details and relevant conversation records) (AC-036); after deletion, that personal data no longer appears in follow-up requests or conversation history (AC-037).

**Related:** US-015, US-016, US-017, NFR-006, NFR-007, EPIC-002

- ⚠ GDPR: only notice, consent, and deletion-on-request are specified. The requirements do not mention a data-subject access/export (portability) request process, a data-breach notification procedure, a documented lawful basis / record of processing, or a defined data-retention period -- confirm with the product owner whether these are in scope for a 'GDPR-level' standard.

### REQ-NFR-001 · draft · source: requirements:non-functional

Seven non-functional requirements covering background processing, scale, responsiveness, availability, security, data residency and GDPR compliance.

NFR-001 [performance] Uploads process in the background (applies to US-010, US-012): admin area returns immediately; parsing/embedding/indexing run async. NFR-002 [scalability] Modest single-organisation workload (whole system): target up to a few thousand conversations/month. NFR-003 [usability] Responsive chat on any screen (US-001): responsive web app, no native app. NFR-004 [availability] Follow-up survives email failure (US-007): request stays stored and failure is logged if notification email fails. NFR-005 [security] Admin area restricted to staff (US-009): authenticated-only access; visitors stay anonymous. NFR-006 [privacy] Data kept in an EU region (whole system): documents, conversation logs and personal data stored/processed in-EU. NFR-007 [compliance] GDPR-level privacy standard (US-015, US-016, US-017): visible notice, explicit consent, deletion on request.

**Related:** US-001, US-007, US-009, US-010, US-012, US-015, US-016, US-017

- ⚠ NFR-006 names 'an EU region' generically; no specific AWS region is designated -- needs architecture-phase decision.
<!-- sdlc:auto:end narrative -->

<!-- sdlc:auto:start summary -->
**Requirements version:** v1  
**Delivery scope:** lightweight  
**Approved:** 2026-11-04

**Capabilities in scope:** Document-grounded question answering over uploaded content, Standalone chat page for visitors, Multi-turn conversation within a session, Source citations shown with answers, "I don't know" fallback with no invented answers, Admin upload and management of source documents (PDF, Word, text), Document chunking, embedding and indexing for retrieval, Admin sign-in, Admin content dashboard, OpenAI language model for generating answers, OpenAI embeddings for document search

**Approved technology stack:**

| Layer | Choice | Rationale |
|---|---|---|
| backend | FastAPI | The core of this product is retrieval-augmented generation -- calling an LLM, embedding documents, searching the vector store -- and that work lives in Python, where the AI and document-parsing libraries (PDF/Word extraction, chunking) are richest and best maintained. FastAPI gives you a clean API for the widget and admin area and handles the LLM's streamed, token-by-token replies well, which is what makes the chat feel responsive. |
| cloud | AWS | AWS runs the whole product in one account: the FastAPI service, the Postgres database, and S3 for the uploaded PDFs and Word files. For a GDPR-level standard it lets you pin all storage and processing to an EU region and gives you the data-handling controls and audit trail you'd want to stand behind that privacy promise, without you operating your own servers. |
| database | PostgreSQL | One database can hold everything this product records -- documents and their metadata, chat conversation logs, captured follow-up requests, admin accounts -- and, through the pgvector extension, the document embeddings the chatbot searches. At a few thousand conversations a month that keeps your whole system in a single, backup-and-restore-in-one-place store, which also simplifies the GDPR 'delete a person's data on request' obligation. |
| frontend | React | Your product has two public faces that must feel instant to anonymous visitors -- an embeddable chat widget that drops into any page of the organization's existing site, and a standalone chat page -- plus a separate authenticated admin area for uploading documents, reviewing conversations and handling follow-up requests. React handles both: a small, self-contained widget bundle that other sites can load, and a richer dashboard, from one codebase and one team. |
<!-- sdlc:auto:end summary -->

<!-- sdlc:auto:start epics -->
## EPIC-001: Document-Grounded Conversational Answering

The core visitor-facing chatbot: a standalone chat page where public visitors ask questions and receive answers generated strictly from the organization's uploaded documents using retrieval-augmented generation (OpenAI language model and OpenAI embeddings over a vector store). Conversations are multi-turn within a session, answers show source citations where possible, and the bot never invents answers.

### US-001: Standalone chat page for visitors

**Priority:** critical

> As a public website visitor, I want to open a standalone chat page and ask questions in natural language without logging in, so that I can get help from the organization's documentation quickly and anonymously.

| ID | Given | When | Then |
|---|---|---|---|
| AC-001 | a visitor navigates to the standalone chat page URL | the page loads | a chat interface with a message input and send control is displayed and no login or account is required to use it |
| AC-002 | a visitor opens the chat page on a mobile or desktop browser | the page renders | the layout is responsive and remains usable across screen sizes (responsive web application only, no native app) |
| AC-003 | the Lightweight MVP delivery scope | the chat experience is delivered | only the standalone chat page is provided and the embeddable website widget is assumed deferred beyond this MVP version (assumption to confirm) |

### US-002: Multi-turn conversation within a session

**Priority:** high

> As a public website visitor, I want the bot to remember what we have already discussed during my current session, so that I can ask follow-up questions without repeating context each time.

| ID | Given | When | Then |
|---|---|---|---|
| AC-004 | a visitor has already exchanged one or more messages in the current session | the visitor asks a follow-up question that references the earlier exchange | the bot uses the session conversation context to produce a relevant answer |
| AC-005 | a visitor starts a new session | they send their first message | no conversation context from any previous session is carried over |

### US-003: Grounded answers via retrieval-augmented generation

**Priority:** critical

> As a public website visitor, I want answers that are drawn only from the organization's uploaded documents, so that I can trust the responses reflect the organization's actual content and not invented information.

| ID | Given | When | Then |
|---|---|---|---|
| AC-006 | documents have been uploaded and indexed | a visitor asks a question that is answerable from those documents | the bot retrieves the most relevant passages and generates an answer grounded only in the retrieved content |
| AC-007 | the system architecture uses an LLM and a vector store | answers and retrieval are performed | an OpenAI language model generates answers and OpenAI embeddings power document search, using a vector store chosen by the engineering team and requiring an LLM API credential such as OPENAI_API_KEY (assumption to confirm) |

### US-004: Source citations shown with answers

**Priority:** high

> As a public website visitor, I want to see which source documents an answer came from, so that I can verify the answer and read the original material if needed.

| ID | Given | When | Then |
|---|---|---|---|
| AC-008 | an answer is derived from one or more retrieved document passages | the answer is displayed to the visitor | source citation references to the originating document(s) are shown alongside the answer |
| AC-009 | an answer has no clearly identifiable source passage | the answer is displayed | no fabricated or misleading citation is shown |

### US-005: "I don't know" fallback with no invented answers

**Priority:** critical

> As a public website visitor, I want the bot to admit when it cannot answer confidently rather than guessing, so that I am not misled by invented information.

| ID | Given | When | Then |
|---|---|---|---|
| AC-010 | a visitor's question cannot be confidently answered from the uploaded documents | the bot responds | it clearly states that it does not know and does not invent an answer beyond the documents |
| AC-011 | the bot has stated it cannot answer | it responds | it offers the visitor the option to leave their contact details and question for staff follow-up |

## EPIC-002: Follow-up Capture and Staff Notification

When the bot cannot answer, visitors can leave their contact details and question for staff follow-up. Captured follow-up requests are stored for admin review and trigger an email notification to a configured staff inbox. Contact capture is only performed with the visitor's consent.

### US-006: Capture visitor contact details and question on fallback

**Priority:** high

> As a public website visitor whose question could not be answered, I want to leave my contact details and question, so that staff can follow up with me directly.

| ID | Given | When | Then |
|---|---|---|---|
| AC-012 | the bot has triggered the fallback and offered follow-up | the visitor chooses to leave their details | a form collects the visitor's contact detail (e.g. email) and their question |
| AC-013 | the visitor completes the follow-up form | they submit it | the follow-up request (contact detail, question and timestamp) is stored for admin review |
| AC-014 | the visitor does not provide consent to be contacted | they attempt to submit the form | their contact details are not stored and submission is blocked |

### US-007: Email notification to configured staff inbox

**Priority:** high

> As a staff member, I want to be emailed when a new follow-up request is captured, so that I can respond promptly without constantly checking the admin area.

| ID | Given | When | Then |
|---|---|---|---|
| AC-015 | a follow-up request has been stored | the storage completes | an email notification containing the question and contact detail is sent to a configured staff inbox |
| AC-016 | the system sends email | notifications are configured | an email-sending credential such as SMTP or SendGrid is required and the staff inbox address is configurable (assumption to confirm) |
| AC-017 | the email notification fails to send | the follow-up request is processed | the request remains stored for admin review and the delivery failure is logged |

### US-008: Admin review and action of follow-up requests

**Priority:** high

> As a signed-in admin, I want to see and act on captured follow-up requests, so that I can manage which visitor questions have been handled.

| ID | Given | When | Then |
|---|---|---|---|
| AC-018 | a signed-in admin opens the follow-up requests view | the view loads | it lists each request with the visitor contact detail, question, timestamp and current status |
| AC-019 | a signed-in admin is viewing a follow-up request | they mark it as handled | its status updates and the change is reflected in the list |

## EPIC-003: Admin Document Management

An authenticated admin area where staff sign in to upload, replace and remove source documents (PDF, Word and plain text). Uploaded documents are chunked, embedded with OpenAI embeddings and indexed in the vector store so they become available to the chatbot.

### US-009: Admin sign-in

**Priority:** critical

> As a staff member, I want to sign in to a protected admin area, so that only authorized staff can manage documents and view captured data.

| ID | Given | When | Then |
|---|---|---|---|
| AC-020 | a staff member provides valid admin credentials | they sign in | they are granted access to the admin area |
| AC-021 | a user provides invalid credentials or is not authenticated | they attempt to access the admin area | access is denied and admin functions are not available |
| AC-022 | an anonymous public visitor | they use the chat | they never require or are offered admin access (visitors remain anonymous) (assumption to confirm) |

### US-010: Upload source documents

**Priority:** critical

> As a signed-in admin, I want to upload source documents in PDF, Word and plain text formats, so that the chatbot has content to answer from.

| ID | Given | When | Then |
|---|---|---|---|
| AC-023 | a signed-in admin selects a PDF, Word or plain text file | they upload it | the document is stored and queued for processing into the knowledge base |
| AC-024 | a signed-in admin selects a file in an unsupported format | they attempt to upload it | the upload is rejected with a clear message listing the supported formats (assumption: PDF, Word, plain text only) |

### US-011: Replace and remove source documents

**Priority:** high

> As a signed-in admin, I want to replace or remove previously uploaded documents, so that the chatbot's answers stay accurate and up to date.

| ID | Given | When | Then |
|---|---|---|---|
| AC-025 | a document exists in the knowledge base | a signed-in admin removes it | the document and its indexed content are deleted so it no longer informs chatbot answers |
| AC-026 | a document exists in the knowledge base | a signed-in admin replaces it with a new file | the previous indexed content is superseded by the re-processed new content |

### US-012: Document chunking, embedding and indexing

**Priority:** critical

> As a signed-in admin, I want uploaded documents to be automatically chunked, embedded and indexed, so that their content can be retrieved to answer visitor questions.

| ID | Given | When | Then |
|---|---|---|---|
| AC-027 | a document has been uploaded in a supported format | processing runs | the document is split into chunks, embedded using OpenAI embeddings and stored in the vector store for retrieval |
| AC-028 | a document is being or has been processed | an admin views it | its processing status (e.g. processing, indexed, failed) is visible |

## EPIC-004: Admin Dashboard and Conversation Review

A content dashboard for staff showing the managed documents and their status, and a view of logged conversation history so staff can review how visitors are using the bot and the quality of answers.

### US-013: Admin content dashboard

**Priority:** high

> As a signed-in admin, I want a dashboard listing the source documents and their status, so that I can see at a glance what content powers the chatbot.

| ID | Given | When | Then |
|---|---|---|---|
| AC-029 | a signed-in admin opens the dashboard | it loads | it lists the uploaded documents with their name, format and processing/indexing status |
| AC-030 | documents have been added or removed | the admin returns to the dashboard | the list reflects the current set of documents in the knowledge base |

### US-014: Review conversation history

**Priority:** medium

> As a signed-in admin, I want to review logged visitor conversations, so that I can understand common questions and check answer quality.

| ID | Given | When | Then |
|---|---|---|---|
| AC-031 | visitors have had conversations with the bot | a signed-in admin opens the conversation history view | past conversations are listed with their messages and timestamps for review |
| AC-032 | a signed-in admin selects a conversation | it is displayed | the full multi-turn exchange between the visitor and the bot is shown |

## EPIC-005: Privacy and Data Protection (GDPR-level)

The product is designed to a GDPR-level privacy standard for a globally mixed visitor audience: a visible privacy notice, explicit consent before capturing contact details, and the ability to delete personal data on request.

### US-015: Privacy notice for visitors

**Priority:** high

> As a public website visitor, I want to see a privacy notice explaining how my data is handled, so that I understand what happens to any information I provide.

| ID | Given | When | Then |
|---|---|---|---|
| AC-033 | a visitor is using the chat | they view the chat interface | a privacy notice is accessible (e.g. linked) explaining how conversation and contact data is handled |

### US-016: Explicit consent before capturing contact details

**Priority:** high

> As a public website visitor, I want to give explicit consent before my contact details are stored, so that my personal data is only captured with my agreement.

| ID | Given | When | Then |
|---|---|---|---|
| AC-034 | the follow-up contact capture form is shown | the visitor submits their details | they must give explicit consent and submission is not allowed without it |
| AC-035 | a visitor declines consent | they close or skip the form | no contact details are stored |

### US-017: Data deletion on request

**Priority:** medium

> As a signed-in admin acting on a visitor request, I want to delete a visitor's personal data on request, so that the organization can meet GDPR-level data deletion obligations.

| ID | Given | When | Then |
|---|---|---|---|
| AC-036 | a data deletion request is received for a visitor's data | a signed-in admin processes the request | the associated personal data (captured contact details and relevant conversation records) is deleted from the system |
| AC-037 | personal data has been deleted | an admin views follow-up requests or conversation history afterwards | the deleted personal data is no longer present |
<!-- sdlc:auto:end epics -->
