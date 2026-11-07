# Reapl Summary

**A flexible AI chat workspace built around provider choice, practical tools, and a focused conversation experience.**

Reapl is an early-stage, browser-based AI chat product developed by Reapl. It gives individuals a single workspace for conversations with compatible AI providers, with controls for model selection, generation behavior, conversation history, and appearance.

This document describes the product as implemented in the current client application. It is intended for prospective users, partners, and investors. It does not disclose private source code, deployment configuration, credentials, or internal network details.

## Executive Summary

Reapl combines a conversational interface with user-configurable model access and a set of practical workflow features. Users provide an API key for a compatible provider, select a model, and work in saved conversations. The application also includes optional research tools, image and audio interactions, export and data controls, and a responsive installable web experience.

The current product is a client application, not a model provider or model-training platform. It does not currently implement user accounts or cross-device conversation synchronization. Model availability, cost, rate limits, and feature compatibility depend on the provider selected by the user.

## Product Direction

Reapl is being shaped as a customizable AI workspace that keeps provider choice visible and gives users practical control over how they work. The product direction centers on:

- **Choice:** Connect to a compatible provider and choose a model rather than being limited to a single fixed model.
- **Continuity:** Keep conversations available in the user's browser and organize them into named sessions.
- **Utility:** Bring common research lookups and writing or coding interactions into one chat workflow.
- **Accessibility:** Provide a responsive interface that works across desktop and mobile browsers.

These principles describe product direction, not a commitment to a specific future feature or delivery date.

## Product Capabilities

### AI Conversations

- Start and manage multiple conversations.
- Stream responses, stop an in-progress response, and regenerate an assistant response.
- Search conversation titles, rename sessions, and remove sessions with their associated messages.
- Use suggested prompts to begin a conversation.
- Configure the provider endpoint, API key, and model identifier. The application can request a provider's available model list when that provider exposes a compatible models endpoint.
- Adjust supported generation parameters, including temperature, top-p, output-token limit, frequency and presence penalties, stop sequence, and the amount of conversation history sent with a request.

The initial configuration includes a Groq-compatible endpoint and model preset. Users can change provider settings. A compatible API shape does not guarantee that every provider supports every endpoint or parameter.

### Images, Audio, and Attachments

- **Image generation:** An image mode can submit requests to a compatible image-generation endpoint.
- **Speech generation:** An audio mode can request generated speech from a compatible provider endpoint.
- **Voice input:** Where the browser supports the Web Speech API, users can dictate text into the composer. Support varies by browser and operating system.
- **Attachments:** Users can attach a file to a message. The client accepts files up to 50 MB and processes text-like files as text, with long text truncated before submission. Other supported image content is sent as image data to the configured provider.

Attachment interpretation depends on the selected model and provider. Reapl does not currently provide general-purpose parsing for office documents, PDFs, or arbitrary audio and video files. Video chat mode is not available.

### Research Tools

Text conversations can make use of an integrated, model-invoked data tool. Depending on the request, it can retrieve information from external services for topics such as Wikipedia, weather, exchange rates and cryptocurrency, technology news, GitHub, books, country information, dictionaries, remote jobs, DNS and IP lookups, QR codes, universities, public API listings, general facts, advice, and spaceflight news.

These lookups rely on third-party services. They are not a Reapl-operated search index, and their availability, accuracy, terms, and data practices are determined by those services. The model may decide whether to call a tool; tool results should be checked against their original sources when accuracy matters.

### Conversation Display

- Render formatted assistant responses, including Markdown and mathematical notation.
- Highlight code and provide code-copy controls.
- Copy assistant responses and use response-level actions from the conversation view.

### Personalization and Notifications

- Choose from seven appearance themes: Default, Orange + White, Blue Smooth, Pink Mist, Soft Green, Lavender, and Violet Rose.
- Enable browser notifications for completed responses, subject to browser permission and support.
- Use a responsive layout with a collapsible conversation sidebar on smaller screens.

### Data Controls

- Review estimated browser storage usage where the browser provides the Storage API.
- Export conversations as JSON or CSV.
- Clear the application's IndexedDB records through the data controls in Settings.
- Submit an optional bug report through the feedback interface.

Exports contain conversation information and may include user-provided or model-generated content. Review exported files before sharing them.

## How Reapl Works

1. A user configures a compatible provider endpoint, model, and API key in Settings.
2. The browser sends the conversation request to the selected provider using an OpenAI-compatible chat-completions format.
3. If the selected mode or model supports it, the request may include image content, speech generation, or a request for the integrated data tool.
4. The response is displayed in the chat and the conversation is saved in browser storage.

Provider compatibility is determined by the endpoint and model selected by the user. Reapl does not guarantee that a third-party provider will accept a particular request format or feature.

## Technical Overview

### Client Application

Reapl is implemented as a single-page web application using Vue 3 and Vue Router. The interface is organized into views and reusable components for the welcome experience, chat workspace, composer, messages, sidebar, and settings. The application is designed for modern desktop and mobile browsers.

### Provider Integration

The client uses OpenAI-compatible request conventions for chat completions and related provider capabilities. Provider credentials and settings are configured by the user. Requests go from the browser to the configured provider; the selected provider processes prompts and any included content under its own terms and privacy practices.

### Local Persistence

Conversation sessions, messages, and application settings are stored in the browser's IndexedDB database for the current site. This supports local persistence without an account-based sync service in the current client. Settings include the API key, so access to the browser profile and device should be protected. Local browser storage is not equivalent to encrypted storage or end-to-end encryption.

### Rendering and Web App Support

The client uses established libraries for Markdown rendering, HTML sanitization, mathematical notation, and code highlighting. A service worker supports caching of application assets as a progressive web app. This can improve app-shell loading, but it does not provide offline AI inference or guarantee that external providers and research services are available offline.

## Data, Privacy, and Security

Understanding the data flow is important when using Reapl:

- **Conversation storage:** Sessions and messages are stored locally in the browser's IndexedDB for the site.
- **Provider requests:** Prompts, selected conversation history, and any submitted attachment content are sent to the configured AI provider when a request is made.
- **External research:** When a research tool is used, relevant queries are sent to the third-party service that supplies the requested data. Network or browser restrictions may cause a request to use a third-party CORS relay.
- **Bug reports:** If a user submits a report, the feedback workflow may transmit the report text, optional email address, app version, and browser information through the configured email service.
- **Data removal:** The in-app clear action removes Reapl's IndexedDB session, message, and settings records for the current site. It does not represent a guarantee that browser caches, provider records, or third-party service logs are also erased.

Reapl does not claim end-to-end encryption. Users should avoid submitting confidential, regulated, or otherwise sensitive information unless they have reviewed the selected provider's data practices and are authorized to share it. The published policies describe additional service terms and data practices; production infrastructure behavior should be evaluated against the current policies and deployment configuration.

## Current Stage and Limitations

Reapl is an experimental, early-stage product. The current published terms describe a free-tier service and do not guarantee uptime, long-term availability, technical support, or permanent retention of data. Features and provider compatibility may change.

The current client does not implement:

- User accounts or cloud-based conversation synchronization.
- A Reapl-hosted AI model or model-training workflow.
- Offline model inference.
- A guaranteed universal file parser or guaranteed support for every compatible provider feature.

Third-party model and data services may charge for use, impose rate limits, or change availability. Users are responsible for their provider credentials, usage, and compliance with applicable provider terms.

## SWOT Analysis

This assessment reflects the current client product, not a financial forecast or a promise of future delivery.

### Strengths

- **Provider choice:** Users can bring a compatible provider API key and configure the endpoint and model, reducing dependence on a single model catalog.
- **Focused workflow:** Conversations, generation controls, research lookups, image and speech interactions, and data export are available in one browser workspace.
- **Local-first conversation storage:** Sessions and messages persist in the user's browser without requiring a Reapl account in the current client.
- **Broad browser reach:** Responsive layouts and installable web-app support make the product accessible across desktop and mobile form factors.

### Weaknesses

- **Early product maturity:** The service is experimental, and current terms do not promise uptime, support, or permanent data retention.
- **No account or cloud sync:** Conversations are tied to the browser profile and site origin, limiting continuity across devices and browsers.
- **Credential handling trade-off:** The API key is stored in browser IndexedDB. This is convenient but is not encrypted or equivalent to a managed secrets vault.
- **Provider and browser dependency:** Feature availability, pricing, rate limits, and speech recognition vary by provider, browser, and operating system.
- **Limited file interpretation:** Text and supported image inputs are handled by the client, but there is no universal document parser for PDFs, office files, or arbitrary media.

### Opportunities

These are potential areas for product development, not announced features or commitments.

- Explore optional, security-conscious account and synchronization capabilities for users who need multi-device continuity.
- Improve provider capability discovery and make unsupported model features clearer before users submit requests.
- Evaluate safer credential management and more explicit controls for local data retention and deletion.
- Expand document workflows with well-scoped parsing, file-type validation, and clear source attribution.
- Develop team or workspace workflows if customer research demonstrates demand and the privacy model can support them.

### Threats

- **Third-party changes:** Provider pricing, API behavior, access policies, and model availability can change without Reapl's control.
- **Crowded category:** AI providers and other chat products can add comparable capabilities, increasing pressure on differentiation and retention.
- **Data and security exposure:** Browser compromise, shared devices, malicious extensions, or accidental exports can expose locally stored conversations or credentials.
- **External service reliability:** Research endpoints and any CORS relay services can be unavailable, change terms, or handle request data under their own policies.
- **Compliance obligations:** Handling user content, provider credentials, cross-border processing, and abuse reports requires clear policies and operational practices as the service grows.

## Policies and Contact

The current published policy texts are included below for convenience. Each block can be expanded without leaving this document. The policy files and the deployed service may be updated separately; users should check the current published version before relying on a policy. This product overview is not legal advice.

<details>
<summary><strong>Privacy Policy</strong> (Last updated: June 21, 2026)</summary>

### REAPL PRIVACY POLICY

**Last Updated: June 21, 2026**

#### 1. INTRODUCTION

REAPL respects user privacy and is committed to protecting personal data in accordance with applicable data protection laws.

#### 2. INFORMATION WE COLLECT

We may collect the following categories of data:

- Account data (if applicable)
- Device information (browser type, OS, device identifiers)
- Usage data (feature interaction, logs, performance metrics)
- Network data (IP address, access timestamps)
- Diagnostic and error logs

We do NOT intentionally collect sensitive personal data unless explicitly provided by the user.

#### 3. PURPOSE OF DATA COLLECTION

Data is used strictly for:

- Providing and maintaining the Service
- Improving performance and reliability
- Security monitoring and abuse prevention
- Debugging and system diagnostics
- Usage analytics and product development

#### 4. DATA SHARING AND DISCLOSURE

We do NOT sell personal data.

We may share limited data with:

- Infrastructure providers (hosting, storage, compute)
- Security and analytics providers
- Legal authorities if required by law

All third parties are required to maintain appropriate confidentiality and security standards.

#### 5. DATA RETENTION

Data is retained only for as long as necessary to:

- Operate the Service
- Meet legal obligations
- Maintain security and audit logs

After that, data may be deleted or anonymized.

#### 6. SECURITY MEASURES

We implement reasonable technical and organizational safeguards to protect data. However, no system is fully secure, and we cannot guarantee absolute security.

#### 7. INTERNATIONAL USERS

If you access REAPL from outside the service operator’s jurisdiction, you acknowledge that your data may be processed in jurisdictions with different data protection laws.

#### 8. USER RIGHTS

Depending on your jurisdiction, you may have rights including:

- Access to your data
- Correction of inaccurate data
- Request deletion of data
- Restriction of processing

Requests can be submitted to: company.0res@gmail.com

#### 9. THIRD-PARTY SERVICES

REAPL may rely on third-party infrastructure providers. Their privacy policies may also apply.

#### 10. CHANGES TO THIS POLICY

We may update this Privacy Policy at any time. Continued use of the Service constitutes acceptance of the updated policy.

#### 11. CONTACT

company.0res@gmail.com
</details>

<details>
<summary><strong>Terms of Service</strong> (Last updated: June 21, 2026)</summary>

### REAPL TERMS OF SERVICE

**Last Updated: June 21, 2026**

#### 1. AGREEMENT TO TERMS

By accessing or using REAPL (“Service”), you agree to be legally bound by these Terms of Service. If you do not agree, you must discontinue use immediately.

#### 2. SERVICE DESCRIPTION

REAPL is a software platform provided on a free-tier basis in its current early-stage form. The Service is experimental and may be modified, suspended, or discontinued at any time without liability or prior notice.

#### 3. ELIGIBILITY

You must be at least 13 years old or the minimum legal age in your jurisdiction. If using on behalf of an organization, you represent that you have authority to bind such entity.

#### 4. FREE TIER CONDITIONS

The Service is currently offered free of charge. Free access does NOT imply:

- Guaranteed uptime or availability
- Long-term continuity of features
- Technical support obligations
- Data permanence guarantees

Paid tiers may be introduced in the future at sole discretion of REAPL.

#### 5. USER RESPONSIBILITY

You are solely responsible for:

- Your use of the Service
- Any content, data, or actions performed via your account
- Ensuring compliance with applicable laws

#### 6. PROHIBITED CONDUCT

You agree NOT to:

- Use the Service for unlawful, fraudulent, or malicious purposes
- Attempt unauthorized access to systems or data
- Reverse engineer, decompile, or exploit the Service
- Interfere with or disrupt system integrity or performance
- Use automated abuse tools (bots, scrapers, flooding, etc.)
- Violate security or rate limits

#### 7. INTELLECTUAL PROPERTY

All rights, title, and interest in REAPL (including software, branding, design, and infrastructure) remain the exclusive property of REAPL. No rights are granted except as expressly stated.

#### 8. SERVICE MODIFICATIONS

We reserve full discretion to modify, suspend, throttle, or discontinue any part of the Service at any time without notice.

#### 9. DISCLAIMER OF WARRANTIES

THE SERVICE IS PROVIDED “AS IS” AND “AS AVAILABLE” WITHOUT WARRANTIES OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, AND NON-INFRINGEMENT.

#### 10. LIMITATION OF LIABILITY

TO THE MAXIMUM EXTENT PERMITTED BY LAW:

REAPL SHALL NOT BE LIABLE FOR ANY INDIRECT, INCIDENTAL, SPECIAL, CONSEQUENTIAL, OR EXEMPLARY DAMAGES, INCLUDING LOSS OF DATA, PROFITS, OR BUSINESS INTERRUPTION.

#### 11. INDEMNIFICATION

You agree to indemnify and hold harmless REAPL, its operators, and affiliates from any claims, damages, liabilities, or expenses arising from your use of the Service or violation of these Terms.

#### 12. TERMINATION

We may suspend or terminate access immediately, with or without notice, for any violation or suspected misuse.

#### 13. GOVERNING LAW

These Terms shall be governed by applicable laws of the jurisdiction in which the Service operator is established, without regard to conflict of law principles.

#### 14. CONTACT

company.0res@gmail.com
</details>

<details>
<summary><strong>Acceptable Use Policy</strong> (Last updated: June 21, 2026)</summary>

### REAPL ACCEPTABLE USE POLICY

**Last Updated: June 21, 2026**

#### 1. PURPOSE

This policy defines strict rules governing acceptable use of REAPL.

#### 2. GENERAL PRINCIPLE

REAPL must be used in a lawful, responsible, and non-abusive manner. Any usage that compromises system integrity, security, or other users' experience is strictly prohibited.

#### 3. STRICTLY PROHIBITED ACTIVITIES

You may NOT use REAPL to:

- Perform hacking, penetration testing, or exploitation without explicit authorization
- Distribute malware, spyware, or malicious code
- Conduct denial-of-service (DoS/DDoS) or resource exhaustion attacks
- Scrape or harvest data in violation of system limits or without permission
- Create automated abuse (bots, spam, flooding, artificial load)
- Impersonate users, services, or entities
- Facilitate fraud, scams, phishing, or identity theft
- Upload or distribute illegal content under applicable law
- Circumvent authentication, authorization, or rate-limiting systems

#### 4. FAIR USE & RATE LIMITING

REAPL operates under fair usage constraints. We reserve the right to:

- Throttle requests
- Block IPs or accounts
- Limit features dynamically without notice

#### 5. SECURITY ENFORCEMENT

Any detected abuse may result in immediate:

- Temporary suspension
- Permanent ban
- Technical blocking (IP/device fingerprinting)

#### 6. MONITORING

We may monitor usage patterns strictly for:

- Security enforcement
- Abuse prevention
- System stability

#### 7. REPORTING VIOLATIONS

Abuse reports: company.0res@gmail.com
</details>

For general inquiries, privacy requests, or abuse reports, contact **company.0res@gmail.com**.

---

Reapl is a proprietary product in active development. Product details in this document reflect the current client implementation and may change as the service evolves.
