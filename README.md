->Main repo private while in dev<-

# HR CARE

**Compliance And Rights Engine**

HR CARE is an AI-powered HR assistant designed to provide accurate, policy-grounded workplace guidance for employees, managers, and HR professionals.
The system combines company policy documents with employment law baselines to deliver structured answers, cite sources, and recommend escalation when human HR review is appropriate.
Unlike generic chatbots, HR CARE is designed to prioritize accuracy, documentation, and compliance over confident guessing.


## Architecture

- **Frontend**: React + Wouter routing + TanStack Query + Shadcn UI + Tailwind CSS
- **Backend**: Express.js with SSE streaming for AI responses
- **Database**: PostgreSQL via Drizzle ORM
- **AI**: OpenAI gpt-5.2
- **Document Parsing**: pdf-parse (PDF), mammoth (DOCX), jszip (ODT), native (TXT/MD/HTML)


## Key Features

- **Role-aware guidance**
  - Employees receive plain-language explanations of rights and processes
  - Managers receive compliance obligations and documentation steps
  - HR professionals receive detailed statutory context and risk flags

- **Policy-based answers**
  Upload company policy documents (PDF, DOCX, ODT, TXT, Markdown, HTML).  
  The system parses, chunks, and indexes documents so responses reference the organization's real policies.

- **Knowledge base retrieval (RAG)**
  Upload company policy documents → they are parsed, chunked, and indexed in PostgreSQL full-text search. Every message query searches the knowledge base and injects relevant chunks into the AI system prompt with citations.
  
- **Streaming AI chat**
  Real-time responses delivered via Server-Sent Events (SSE).

- **Conversation persistence**
  Conversations are stored in PostgreSQL, auto-titled, and accessible from the sidebar.

- **Channel-agnostic architecture**
  Currently supports parsing company policies on inter/intranet and uploaded documentation. 


  ## Planned Features

  ## Channel Integrations
- **Slack** — bot integration via Bolt SDK, slash commands, DMs
- **Email** — inbound email parsing and reply via SendGrid/Postmark
- **Telegram** — bot API, simple to implement
- **Microsoft Teams** — Azure Bot Framework, app manifest
- **WhatsApp** — Meta Cloud API, requires business account approval
- **Embeddable web widget** — iframe/script tag for third-party portals

- Doc Export via SSG capability - Generate a static documentation site or Markdown file from knowledge base documents and conversation insights

- Build out workflow logic and decision-tree models to prevent the model from giving advice that *might* violate specific State or Federal statutes. After all, if it can't do what it's supposed to then what's the point?
    - Legal checklist
    - Federal checklist
    - HR decision workflow
    - RAG workflow *** I'm actually the most excited about this one ;)
    - Documentation workflow
    - Escalation workflow
    - Role-based Access workflow
    - Investigation workflow
    - Policy Interpretation workflow
        
- Document generation
    - Incident reports
    - Warning letters
    - Term summaries
    - Investigation notes

- Change AI from 5.2 -> OpenRouter API -> routing layer -> most appropriate model.

- Admin config panel
- Conversation export (PDF, Markdown)
- Jurisdiction tagging on uploaded documents
- HR-only document access controls (audience tagging per doc)
- Audit log viewer






## Architecture Diagram

                    ┌──────────────────────┐
                    │      User Input      │
                    │ employee / manager / │
                    │ HR question or case  │
                    └──────────┬───────────┘
                               │
                               ▼
                   ┌────────────────────────────┐
                   │   HR Workflow Engine       │
                   │ - classify issue type      │
                   │ - detect risk level        │
                   │ - apply role logic         │
                   │ - apply escalation rules   │
                   └──────────┬─────────────────┘
                              │
              ┌───────────────┼────────────────┐
              │               │                │
              ▼               ▼                ▼
  ┌─────────────────┐ ┌─────────────────┐ ┌──────────────────┐
  │ Company Policy  │ │ Legal Checklist │ │ Conversation /   │
  │ Retrieval (RAG) │ │ + Compliance    │ │ Case Context     │
  │ docs + handbook │ │ workflow rules  │ │ memory           │
  └────────┬────────┘ └────────┬────────┘ └─────────┬────────┘
           │                   │                    │
           └───────────────────┴────────────────────┘
                               │
                               ▼
                  ┌──────────────────────────┐
                  │  LLM via OpenRouter      │
                  │ - explanation            │
                  │ - policy interpretation  │
                  │ - drafting               │
                  │ - structured guidance    │
                  └──────────┬───────────────┘
                             │
                             ▼
                ┌──────────────────────────────┐
                │ Structured HR Response       │
                │ - answer                     │
                │ - what it's based on         │
                │ - next steps                 │
                │ - escalation note            │
                │ - optional documentation     │
                └──────────────────────────────┘


  
