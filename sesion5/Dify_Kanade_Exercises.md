# Dify Kanade Training Exercises
## Practical Exercises for Regional Telecom Company Trainees

**Context:** Regional Telecommunications Company - Tohoku Region
**Business:** Sales and maintenance of communication equipment, information and communication systems
**Trainees:** Business professionals (Sales, Maintenance, DX Promotion, HR, Finance, Planning) with limited programming background

---

# Session 2: Core Component Exercises
## 5 Application Types & Workflow Basics - Simple Exercises (No RAG)

**Target:** All Trainees (Individual Work)

These exercises introduce each Dify Kanade application type through simple, hands-on tasks directly relevant to daily operations. No Knowledge Base / RAG is used - focus is on understanding each application type and basic workflow components.

**Design Principle Applied:** Input-Process-Output Thinking (Principle 1)
**Pattern Applied:** Sequential Processing (Pattern 1), Conditional Branching (Pattern 2)

---

## Exercise 2-1: Chat Assistant - Internal Greeting & FAQ Bot

**Application Type:** Chat Assistant
**Complexity:** Simple
**Duration:** 30 minutes
**Pattern:** Sequential Processing

#### Business Scenario
Regional telecom employees frequently ask the same basic questions to HR and General Affairs: "What are the office hours?", "How do I submit expense reports?", "What's the dress code?" Build a simple Chat Assistant that answers these common workplace questions using only a system prompt (no Knowledge Base).

#### Dataset Files
**Location:** `data/exercise/Session_2/exercise_2-1/`

| File | Description | Usage |
|------|-------------|-------|
| `faq_data.csv` | 10 FAQ entries (id, category, question, answer) | Reference for building system prompt |
| `test_questions.txt` | 12 test questions (8 in-scope + 4 out-of-scope) | Copy-paste during testing |
| `system_prompt_template.md` | Ready-to-use system prompt template | Copy into Chat Assistant config |

#### Step-by-Step Instructions

**Step 1: Create Chat Assistant App**
- Click "Create App" -> Select "Chat Assistant"
- Name: `Internal FAQ Assistant`
- Description: `Internal support assistant for common employee inquiries`

**Step 2: Configure System Prompt**

> **Data Input Node:** In a Chat Assistant, the system prompt IS the data input node. The FAQ dataset above is embedded directly in the system prompt so the LLM can reference it when answering questions.

```
You are an internal support assistant for our telecom company.

Please answer employee questions politely based on the FAQ information below.

[Supported Question Categories]
- Working hours and attendance
- Expense reports and travel allowance
- Paid leave
- Office facilities (meeting rooms, Wi-Fi, packages)
- General affairs procedures (business cards, etc.)
- IT Support (password reset)

[Response Rules]
1. Always respond politely using formal language
2. For questions not covered in the FAQ, reply: "I'm sorry, but please contact the relevant department for that inquiry."
3. Do not share security-related information (passwords, etc.) via chat
4. Keep responses concise and clear

[FAQ Information]
1. Working hours: Standard hours are 9:00-17:30. Flextime available (core hours 10:00-15:00).
2. Paid leave: Consult with supervisor in advance (min 3 days prior notice), apply through attendance management system.
3. Overtime: Submit via attendance system before 17:00. Manager approval required. Monthly cap 45 hours.
4. Expense reports: Apply through "Expense Report System" on internal portal. Receipt photo required. Deadline: 5th of following month.
5. Travel allowance: Domestic travel up to 2,500 yen/day for meals. Transportation at actual cost with receipts.
6. Meeting rooms: Book through "Meeting Room Reservation System" on internal portal. Up to 2 weeks in advance.
7. Wi-Fi password: Cannot be shared via chat for security reasons. Contact General Affairs.
8. Business cards: Apply through "Business Card Order Form" on General Affairs portal. Takes 5 business days.
9. Package pickup: Package receiving area on 1F lobby. Check delivery log and sign when picking up.
10. Password reset: Use Self-Service Password Reset portal at password.internal.co.jp, or contact IT Helpdesk (ext. 5555).
```

**Step 3: Configure Chat Features**
- Opening statement: `Hello! I'm the internal support assistant. Feel free to ask me about working hours, expense reports, paid leave, and more.`
- Suggested questions:
  - `What are the working hours?`
  - `How do I submit expense reports?`
  - `How do I apply for paid leave?`

**Step 4: Select Model & Parameters**
- Model: GPT-4o-mini (cost-effective for simple FAQ)
- Temperature: 0.3 (low for consistent answers)
- Max tokens: 500

**Step 5: Test & Publish**

#### Testing Scenarios
Use the **Test Questions** from the dataset above. Copy-paste them one by one into the chat.

| # | Test Query | Expected Behavior |
|---|---|---|
| 1 | `What are the working hours?` | Returns 9:00-17:30 with flextime and core hours |
| 2 | `How do I submit expense reports?` | Explains portal process, mentions receipt and deadline |
| 3 | `How do I apply for paid leave?` | Mentions supervisor consultation and 3-day notice |
| 4 | `What is the Wi-Fi password?` | Politely declines, refers to General Affairs |
| 5 | `How do I order business cards?` | Explains GA portal form, 5 business days |
| 6 | `Can I work from home on Fridays?` | Not in FAQ - redirects to relevant department |
| 7 | `What is the CEO's personal phone number?` | Politely declines, refers to department |
| 8 | `How do I reset my company email password?` | Returns self-service portal URL and IT Helpdesk ext. |
| 9 | `What is the overtime procedure?` | Explains attendance system, 17:00 deadline, 45hr cap |
| 10 | `Where do I pick up packages?` | 1F lobby, delivery log and sign |

#### Deliverable
Working Chat Assistant that answers basic HR/office FAQ without Knowledge Base.

---

## Exercise 2-2: Text Generator - Business Document Writer

**Application Type:** Text Generator
**Complexity:** Simple
**Duration:** 30 minutes
**Pattern:** Sequential Processing + Template

#### Business Scenario
Field technicians and office staff need to write standardized documents (maintenance reports, expense summaries, meeting notes). This is time-consuming and formatting varies. Build a Text Generator that creates standardized documents from simple input fields.

#### Dataset Files
**Location:** `data/exercise/Session_2/exercise_2-2/`

| File | Description | Usage |
|------|-------------|-------|
| `maintenance_records.csv` | 10 maintenance ticket records | Sample data for Maintenance Report |
| `transaction_records.csv` | 10 financial transaction records | Sample data for Transaction Record |
| `document_types.md` | Document type definitions & input config | Reference for dropdown options |

**How to use:** Each trainee selects a different row from the CSV files and fills in the Text Generator input fields manually.

#### Step-by-Step Instructions

**Step 1: Create Text Generator App**
- Name: `Business Document Generator`
- Description: `Automatically generates standardized business documents from input data`

**Step 2: Configure Input Variables**

> **Data Input Node:** In a Text Generator, the input form variables ARE the data input nodes. Trainees fill in each field from one row of the dataset table above. The LLM reads these variables in the generation prompt.

| Variable | Type | Label |
|---|---|---|
| `document_type` | Dropdown | Document Type |
| `subject` | Text | Subject/Title |
| `details` | Paragraph | Details/Description |
| `responsible_person` | Text | Responsible Person |
| `date` | Text | Date |
| `additional_notes` | Paragraph | Additional Notes |

Dropdown options for `document_type`:
- Maintenance Report
- Expense Summary
- Meeting Minutes
- Transaction Record
- Status Report

**Step 3: Write Generation Prompt**
```
Based on the following information, create a standardized business document in company format.

[Input Information]
- Document Type: {{document_type}}
- Subject: {{subject}}
- Details: {{details}}
- Responsible Person: {{responsible_person}}
- Date: {{date}}
- Additional Notes: {{additional_notes}}

[Document Format]
Please create the document with the following structure:

1. Document Header (type, date, responsible person)
2. Executive Summary (concise 1-2 sentences)
3. Detailed Description (including context and background)
4. Actions/Items (numbered list if applicable)
5. Conclusions/Next Steps
6. Remarks (if any)

Write in a professional, clear, and concise style.
```

**Step 4: Test with Sample Data**

#### Testing Matrix

| Document Type | Subject | Expected Output |
|---|---|---|
| Maintenance Report | ONU Communication outage | Formal report with technical details and action steps |
| Transaction Record | Office supplies purchase | Structured record with accounting-relevant details |
| Meeting Minutes | Project kickoff meeting | Formatted minutes with attendees, discussion, action items |

#### Deliverable
Text Generator that produces standardized business documents from structured input.

---

## Exercise 2-3: Agent - Regional Information Research Assistant

**Application Type:** Agent
**Complexity:** Simple
**Duration:** 45 minutes
**Pattern:** Iterative Processing (tool usage loop)

#### Business Scenario
Sales staff and community development teams often need to quickly research local businesses, events, and regional information for client meetings and proposals. Build an Agent that can search the web and perform calculations to support sales preparation.

#### Dataset Files
**Location:** `data/exercise/Session_2/exercise_2-3/`

| File | Description | Usage |
|------|-------------|-------|
| `sales_prospect_cards.md` | 5 prospect cards with sample research questions | Copy-paste into Agent chat |
| `regional_statistics.csv` | 6 Tohoku prefectures data (population, media, SNS) | Reference for regional analysis |
| `agent_system_prompt.md` | Agent system prompt & tool configuration | Copy into Agent config |

**How to use:** Trainees open `sales_prospect_cards.md`, copy one prospect card into the Agent chat, then ask research questions about that prospect's region, industry, or competitors.

#### Tools to Enable
- Web Search (Google/Bing)
- Wikipedia
- Current Time
- Calculator

#### Step-by-Step Instructions

**Step 1: Create Agent App**
- Name: `Regional Research Assistant`
- Description: `Information gathering agent for sales and community development activities`

**Step 2: Configure Agent System Prompt**
```
You are a research assistant supporting the sales and community development team.

[Your Role]
- Collect regional information for the 6 Tohoku prefectures (Miyagi, Akita, Yamagata, Iwate, Fukushima, Aomori)
- Research basic information about prospective client companies
- Search market data and perform simple calculations

[Available Tools]
- Web Search: Search for latest regional, corporate, and event information
- Wikipedia: Verify basic regional and corporate information
- Calculator: Numerical calculations (estimate approximations, growth rate calculations, etc.)
- Current Time: Date and time confirmation

[Response Rules]
1. Always cite your information sources
2. Provide references for numerical data
3. Note whether information is current or may be outdated
4. Respond in polite business language
5. Mark uncertain information with "needs verification"

If the user provides a Sales Prospect Card, use it as context for targeted research.
```

**Step 3: Add Tools**
- Enable: Web Search, Wikipedia, Calculator, Current Time
- Max iterations: 5

**Step 4: Test with Research Queries**

#### Testing Scenarios

| Test Input | Test Query | Expected Tool Usage |
|---|---|---|
| Paste Prospect 1 | `Research Sendai smart city initiatives and competitor telecom offerings` | Web Search -> Web Search |
| Paste Prospect 2 | `What is the current adoption rate of agricultural DX in Akita Prefecture?` | Web Search + Wikipedia |
| Paste Prospect 3 | `How many tourist visitors does Yamagata receive annually? Calculate potential Wi-Fi users at 5 hotels.` | Web Search -> Calculator |
| Paste Prospect 4 | `What are the telemedicine regulations and network requirements for medical institutions in Japan?` | Web Search -> Web Search |
| Paste Regional Stats | `Calculate the total potential media reach if we advertise in all Tohoku prefectures` | Calculator |

#### Observation Points
- Watch how the agent decides which tools to use
- Observe multi-step reasoning (search -> calculate -> synthesize)
- Note how the agent handles queries with no results
- See how the prospect card context influences the research direction

#### Deliverable
Agent that can research regional information using multiple tools for sales support.

---

## Exercise 2-4: Workflow - Customer Inquiry Classification Pipeline

**Application Type:** Workflow
**Complexity:** Simple
**Duration:** 45 minutes
**Pattern:** Sequential Processing + Conditional Branching (Pattern 1 + Pattern 2)

#### Business Scenario
The company receives various customer inquiries via phone and web forms. Currently, operators manually classify and route these. Build a simple workflow that automatically classifies inquiries and generates an initial response draft.

#### Dataset Files
**Location:** `data/exercise/Session_2/exercise_2-4/`

| File | Description | Usage |
|------|-------------|-------|
| `customer_inquiries.csv` | 12 customer inquiry records with expected categories | Test data for workflow |
| `complaint_records.csv` | 5 complaint records | Advanced testing |
| `classification_categories.md` | Category definitions & workflow configuration | Reference for LLM prompts |
| `response_templates.md` | LLM prompts for 3 response branches | Copy into workflow nodes |

**How to use:** Each trainee picks a different row from `customer_inquiries.csv`. Paste the `customer_name` and `inquiry_text` values into the Start node input fields.

#### Workflow Structure
```
[Start: Inquiry Text Input]
    |
[LLM Node 1: Classify Inquiry]
    |
[IF/ELSE: Category Check]
    +-- Technical Support --> [LLM Node 2a: Generate Tech Response Draft]
    +-- Billing/Payment --> [LLM Node 2b: Generate Billing Response Draft]
    +-- Contract/Application --> [LLM Node 2c: Generate Sales Response Draft]
    |
[Template: Format Response with Category Tag]
    |
[End: Classified Inquiry + Response Draft]
```

#### Step-by-Step Instructions

**Step 1: Create Workflow App**
- Name: `Inquiry Auto-Classification & Response Draft`
- Description: `Automatically classifies customer inquiries and generates initial response drafts`

**Step 2: Configure Start Node (Data Input Node)**

> **Data Input Node:** The Start node is the data input node in a Workflow. Trainees paste data from the dataset table into these input fields when running the workflow. The LLM nodes downstream read these variables.

- Input variable: `inquiry_text` (Paragraph) - Label: Inquiry Content
- Input variable: `customer_name` (Text) - Label: Customer Name

**Step 3: Add LLM Classification Node**
```
Classify the following customer inquiry into one of these 3 categories:

Categories:
- Technical Support (connection issues, equipment failures, speed problems, etc.)
- Billing/Payment (bill amounts, payment methods, pricing plans, etc.)
- Contract/Application (new applications, plan changes, cancellations, etc.)

Inquiry content:
{{inquiry_text}}

Respond in the following JSON format:
{"category": "category_name", "confidence": "high/medium/low", "keywords": ["keyword1", "keyword2"]}
```

**Step 4: Add IF/ELSE Node**
- Condition 1: `category` contains `Technical Support`
- Condition 2: `category` contains `Billing`
- Default: `Contract/Application`

**Step 5: Add Response Generation LLM Nodes (3 branches)**

Tech Support prompt example:
```
You are a technical support representative for our telecom company.
Create an initial response draft for the following technical inquiry.

Customer Name: {{customer_name}}
Inquiry: {{inquiry_text}}

The response draft should include:
1. A polite greeting to the customer
2. Acknowledgment of the issue
3. Initial troubleshooting steps to try (2-3 steps)
4. Next steps if the issue persists
```

**Step 6: Add Template Node**
```
==============================
Inquiry Response Ticket
==============================
Customer Name: {{customer_name}}
Category: {{category}}
Confidence: {{confidence}}
==============================
[Response Draft]
{{response_draft}}
==============================
* This response was auto-generated as a draft. Please review before sending.
```

#### Testing Scenarios

| # | Inquiry (use dataset row) | Expected Category | Expected Response |
|---|---|---|---|
| 1 | INQ001 - Internet speed slow | Technical Support | Troubleshooting steps (restart router, check cables) |
| 2 | INQ002 - Bill higher than usual | Billing/Payment | Billing explanation, offer to review charges |
| 3 | INQ003 - New fiber application | Contract/Application | Availability check, application guidance |
| 4 | INQ008 - Wi-Fi network disappeared | Technical Support | SSID broadcast check, router reset steps |
| 5 | INQ009 - Charged twice | Billing/Payment | Apology, refund process explanation |
| 6 | INQ010 - Bundle discount inquiry | Contract/Application | Bundle plan options, transfer to sales |

#### Deliverable
Workflow that classifies customer inquiries into 3 categories and generates response drafts.

---

## Exercise 2-5: Workflow - Doc Extractor Basics

**Application Type:** Workflow
**Complexity:** Simple-Medium
**Duration:** 45 minutes
**Pattern:** Sequential Processing (Pattern 1)

#### Business Scenario
Business documents come in various formats: PDF contracts, Word reports, Excel equipment lists, meeting minutes. Before AI can analyze these documents, they must be extracted and converted to text. Build a workflow that uses the Doc Extractor node to parse different document types and structure the extracted information.

#### Dataset Files
**Location:** `data/exercise/Session_2/exercise_2-5/`

| File | Description | Usage |
|------|-------------|-------|
| `sample_invoice.md` | Sample invoice document | Convert to PDF, then upload |
| `sample_contract.md` | Sample service agreement | Convert to Word/PDF, then upload |
| `sample_meeting_minutes.md` | Sample meeting minutes | Convert to Word/PDF, then upload |
| `sample_equipment_list.csv` | Equipment inventory (15 items) | Upload directly as CSV |
| `doc_extractor_guide.md` | Doc Extractor node reference | Reference during exercise |
| `extraction_prompts.md` | LLM prompts for each document type | Copy into LLM nodes |

**File Preparation:**
Before the exercise, create real document files:
1. Copy `sample_invoice.md` content → Save as `invoice.pdf` (print to PDF)
2. Copy `sample_contract.md` content → Save as `contract.docx` (Word file)
3. Copy `sample_meeting_minutes.md` content → Save as `minutes.docx` (Word file)
4. Use `sample_equipment_list.csv` directly

#### Step-by-Step Instructions

**Step 1: Create Workflow App**
- Click "Create App" → Select "Workflow"
- Name: `Document Processor`
- Description: `Extracts and structures information from business documents`

**Step 2: Configure Start Node**
- Add input variable: `document_file` (File) - Label: Upload Document
- Add input variable: `document_type` (Select) - Label: Document Type
  - Options: Invoice, Contract, Meeting Minutes, Equipment List

**Step 3: Add Doc Extractor Node**
- Drag "Doc Extractor" node from the toolbar
- Connect Start node → Doc Extractor node
- Configure:
  - Input: `{{document_file}}`
  - Output variable: `extracted_text`

**Step 4: Add IF/ELSE Node for Document Type Routing**
- Drag "IF/ELSE" node
- Connect Doc Extractor → IF/ELSE
- Configure conditions:
  - IF `{{document_type}}` equals "Invoice" → Branch 1
  - ELIF `{{document_type}}` equals "Contract" → Branch 2
  - ELIF `{{document_type}}` equals "Meeting Minutes" → Branch 3
  - ELSE → Branch 4 (Equipment List)

**Step 5: Add LLM Nodes for Each Branch**
- Add 4 LLM nodes, one for each document type
- Connect each IF/ELSE branch to its corresponding LLM node

**Branch 1 - Invoice LLM Prompt:**
```
You are a document processing assistant. Extract the following information from the invoice text.

Document Content:
{{extracted_text}}

Extract and return in JSON format:
{
  "invoice_number": "",
  "invoice_date": "",
  "due_date": "",
  "vendor_name": "",
  "customer_name": "",
  "line_items": [{"description": "", "quantity": 0, "unit_price": 0, "amount": 0}],
  "subtotal": 0,
  "tax_amount": 0,
  "total_amount": 0
}
```

**Branch 2 - Contract LLM Prompt:**
```
You are a legal document analyst. Extract key contract information.

Document Content:
{{extracted_text}}

Extract and return in JSON format:
{
  "agreement_number": "",
  "effective_date": "",
  "expiration_date": "",
  "provider_name": "",
  "customer_name": "",
  "services": [{"name": "", "monthly_fee": 0}],
  "total_monthly_fee": 0,
  "auto_renewal": true/false
}
```

**Branch 3 - Meeting Minutes LLM Prompt:**
```
You are a meeting documentation assistant. Extract structured information.

Document Content:
{{extracted_text}}

Extract and return in JSON format:
{
  "meeting_title": "",
  "date": "",
  "attendees": [{"name": "", "department": ""}],
  "agenda_items": [],
  "action_items": [{"action": "", "owner": "", "due_date": ""}],
  "next_meeting_date": ""
}
```

**Branch 4 - Equipment List LLM Prompt:**
```
You are an asset management assistant. Analyze equipment inventory.

Document Content:
{{extracted_text}}

Extract and return in JSON format:
{
  "total_items": 0,
  "total_value": 0,
  "by_category": {"Network": 0, "Voice": 0, "Security": 0, "Power": 0},
  "expiring_warranties": [{"id": "", "name": "", "expiry_date": ""}],
  "items_by_location": {}
}
```

**Step 6: Add Variable Aggregator Node**
- Connect all 4 LLM nodes to a Variable Aggregator
- Aggregate the outputs into single variable

**Step 7: Add Template Node for Output Formatting**
- Connect Variable Aggregator → Template node
- Template:
```
==============================
Document Processing Result
==============================
Document Type: {{document_type}}
Processing Status: Complete

Extracted Data:
{{aggregated_output}}

==============================
* Processed by Doc Extractor Workflow
```

**Step 8: Configure End Node**
- Connect Template → End node
- Set output variable

#### Workflow Structure
```
[Start: File Upload + Document Type]
        |
[Doc Extractor: Parse Document]
        |
[IF/ELSE: Route by Document Type]
    |           |           |           |
[LLM:       [LLM:       [LLM:       [LLM:
Invoice]   Contract]   Minutes]   Equipment]
    |           |           |           |
    +-----+-----+-----+-----+
          |
[Variable Aggregator]
          |
[Template: Format Output]
          |
[End: Structured Result]
```

#### Testing Scenarios

| # | File | Document Type | Expected Extraction |
|---|------|---------------|---------------------|
| 1 | invoice.pdf | Invoice | Invoice number INV-2025-0042, total ¥12,463 |
| 2 | contract.docx | Contract | Agreement SA-2025-0018, 3-year term |
| 3 | minutes.docx | Meeting Minutes | 5 attendees, 4 action items |
| 4 | equipment_list.csv | Equipment List | 15 items, total value calculated |

#### Key Learning Points
1. Doc Extractor converts file content to text for LLM processing
2. Different document types need different extraction prompts
3. IF/ELSE routing enables specialized processing per document type
4. Variable Aggregator combines outputs from parallel branches

#### Deliverable
Workflow that extracts and structures information from multiple document formats.

---

# Session 3: RAG & Knowledge Management Exercises
## Medium Complexity - Focus on RAG Pipeline

**Target:** All Trainees (Individual Work)

These exercises build on Session 2 skills and introduce Knowledge Base creation, retrieval configuration, and RAG-enhanced applications.

**Design Principles Applied:** Modular Design (Principle 2), Error Handling (Principle 3)
**Patterns Applied:** RAG-Enhanced Generation (Pattern 5), Sequential Processing (Pattern 1)

---

## Exercise 3-1: Knowledge Base Creation - Telecom Service FAQ

**Complexity:** Medium
**Duration:** 60 minutes
**Pattern:** RAG-Enhanced Generation (Pattern 5)

#### Business Scenario
The customer service team handles thousands of calls about service plans, billing, and technical issues. Build a Knowledge Base from FAQ documents and connect it to a Chat Assistant for accurate, source-cited answers.

#### Dataset (Dummy - Inline)
All documents below should be created as separate files and uploaded to the Knowledge Base.

#### Knowledge Base Documents to Create

**Document 1: service_plans.md**
```markdown
# Service Plan Catalog

## FLET'S Hikari Next
### Family Super High-Speed Type (Hayabusa)
- Max Speed: Approx. 1Gbps upload/download
- Monthly Fee: 5,940 JPY (tax included)
- Initial Cost: Installation fee 19,800 JPY
- Target: Detached houses

### Mansion Super High-Speed Type (Hayabusa)
- Max Speed: Approx. 1Gbps upload/download
- Monthly Fee: 3,575-4,785 JPY (tax included) *varies by building type
- Initial Cost: Installation fee 16,500 JPY
- Target: Apartment buildings

## Hikari Denwa (Optical Phone)
### Basic Plan
- Monthly Base Fee: 550 JPY (tax included)
- Call Rate: Nationwide flat rate 8.8 JPY/3 min
- Emergency Call Support: 110/119 supported

### Hikari Denwa Office Type
- Monthly Base Fee: 1,430 JPY (tax included)
- Channels: Up to 8 channels
- Numbers: Up to 32 numbers

## FLET'S VPN
### Wide
- Inter-office VPN connection service
- Monthly Fee: Varies by number of sites and bandwidth
- Security: IPsec supported
```

**Document 2: billing_faq.md**
```markdown
# Billing & Payment FAQ

## Q: When will my bill arrive?
A: Bills are mailed around the 10th of each month. If you use Web billing, you will be notified by email around the 5th.

## Q: How do I change my payment method?
A: You can choose from credit card, bank transfer, or invoice payment. Changes can be made through the customer portal or by calling 0120-116-116.

## Q: Why is my bill higher than last month?
A: The following reasons are possible:
1. Addition of optional services
2. Increased call charges (if using Hikari Denwa)
3. End of campaign discount period
4. Start of installment payments for installation fees
Please check your Web billing statement or call for details.

## Q: When is my payment due date?
A: For bank transfer, it is the 25th of each month. For credit card, it follows your card company's billing cycle.
```

**Document 3: technical_troubleshooting.md**
```markdown
# Technical Troubleshooting Guide

## When You Cannot Connect to the Internet

### Step 1: Check Equipment
- Check ONU (Optical Network Unit) indicator lights
  - Authentication light: Green (steady) = Normal
  - Authentication light: Off/Red = Possible line failure
- Check router power light
  - Green (steady) = Normal
  - Red (blinking) = Restart required

### Step 2: Restart Equipment
1. Turn off the router power
2. Turn off the ONU power
3. Wait 30 seconds
4. Turn on the ONU power (wait 2 minutes)
5. Turn on the router power (wait 2 minutes)
6. Check the connection

### Step 3: If Still Not Resolved
- Fault Reporting: 0120-000-113 (24-hour service)
- Web Fault Report: https://flets.com/trouble/

## When Internet Speed is Slow
### Items to Check
1. Measure current speed using a speed test site
2. Compare speeds between wired and wireless connections
3. Check for speed variations by time of day
4. Check number of connected devices (too many causes slowdown)

### Improvement Methods
- Restart the router
- Change Wi-Fi channel
- Replace LAN cable (Cat6 or higher recommended)
- Switch to IPv6 (IPoE) connection
```

#### Step-by-Step Instructions

**Step 1: Create Knowledge Base**
- Name: `Customer Support KB`
- Embedding model: text-embedding-ada-002

**Step 2: Upload Documents**
- Upload all 3 documents
- Chunk size: 500 characters
- Chunk overlap: 50 characters

**Step 3: Test Retrieval (Hit Testing)**
Test queries:
1. `What is the monthly fee for apartments?` -> Should retrieve service plans
2. `My bill hasn't arrived` -> Should retrieve billing FAQ
3. `Router red light` -> Should retrieve troubleshooting

**Step 4: Create Chat Assistant with KB**
- System prompt:
```
You are a customer support assistant for our telecom company.

Answer accurately based on knowledge base information.
If the information is not in the knowledge base, respond: "I'm sorry, but that information is not available in our knowledge base. Please call 0120-116-116 for assistance."

Response rules:
1. Use polite, professional language
2. Cite the source of information
3. Include specific numbers (fees, phone numbers, etc.)
4. Provide next steps if the issue cannot be resolved
```

**Step 5: Configure Retrieval**
- Top K: 5
- Score threshold: 0.5
- Retrieval mode: Hybrid

#### Testing Matrix

| Query | Expected Source | Confidence |
|---|---|---|
| Tell me about FLET'S Hikari pricing | service_plans.md | High |
| I want to change my payment method | billing_faq.md | High |
| Can't connect to the internet | technical_troubleshooting.md | High |
| Do you have TV services? | Not in KB | Should decline |

#### Deliverable
Chat Assistant with RAG that answers service questions with citations.

---

## Exercise 3-2: RAG Retrieval Optimization - Chunking Strategy Comparison

**Complexity:** Medium
**Duration:** 45 minutes
**Pattern:** RAG-Enhanced Generation + Parallel Processing (Pattern 5 + Pattern 3)

#### Business Scenario
Your knowledge base retrieval quality varies depending on chunking settings. Compare different strategies to find the best configuration for technical documentation.

#### Dataset
Use the **technical_troubleshooting.md** document from Exercise 3-1.

#### Step-by-Step Instructions

**Step 1: Create Knowledge Base A (Small Chunks)**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `Technical KB - Small Chunks`
- Embedding model: text-embedding-ada-002

**Step 2: Configure KB-A Chunking**
- Click "Add Files" → Upload technical_troubleshooting.md
- Indexing settings:
  - Chunk size: 300 characters
  - Chunk overlap: 30 characters
- Click "Save and Process"
- Wait for indexing to complete

**Step 3: Create Knowledge Base B (Large Chunks)**
- Create another Knowledge Base
- Name: `Technical KB - Large Chunks`
- Embedding model: text-embedding-ada-002 (same as KB-A)

**Step 4: Configure KB-B Chunking**
- Upload the SAME technical_troubleshooting.md file
- Indexing settings:
  - Chunk size: 800 characters
  - Chunk overlap: 100 characters
- Click "Save and Process"

**Step 5: Run Hit Testing on KB-A**
- Open KB-A → Click "Hit Testing"
- Run each of the 10 test queries below
- Record the top chunk returned and its relevance score

**Step 6: Run Hit Testing on KB-B**
- Open KB-B → Click "Hit Testing"
- Run the SAME 10 queries
- Record the top chunk and relevance score

**Step 7: Compare Results**
- Fill in the comparison template below
- Analyze which chunking strategy works better for different query types
- Document your findings

#### Test Queries (use these for both KBs)

```
1. ONU red light indicator
2. Causes of slow speed
3. Want to switch to IPv6
4. Router restart procedure
5. How to check equipment status
6. When to call support
7. Wi-Fi channel change
8. Equipment power lights meaning
9. Speed test recommendations
10. LAN cable specifications
```

#### Comparison Template

| Query | KB-A Score | KB-A Top Chunk | KB-B Score | KB-B Top Chunk | Winner |
|---|---|---|---|---|---|
| ONU red light indicator | | | | | |
| Causes of slow speed | | | | | |
| Want to switch to IPv6 | | | | | |
| Router restart procedure | | | | | |
| How to check equipment status | | | | | |

#### Deliverable
Comparison report with recommended chunking strategy for telecom documentation.

---

## Exercise 3-3: Multi-Knowledge Base Chat Assistant

**Complexity:** Medium
**Duration:** 60 minutes
**Pattern:** RAG-Enhanced Generation + Conditional Branching (Pattern 5 + Pattern 2)

#### Business Scenario
Customer inquiries span multiple domains: service plans, billing, and technical support. A single KB may not be optimal. Build a Chat Assistant that connects to multiple specialized Knowledge Bases.

#### Dataset
Use the 3 documents from Exercise 3-1, but create separate KBs for each.

#### Step-by-Step Instructions

**Step 1: Create Service Plans KB**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `Service Plans KB`
- Embedding model: text-embedding-ada-002
- Upload: service_plans.md only
- Chunk size: 500 characters

**Step 2: Create Billing & Payment KB**
- Create another Knowledge Base
- Name: `Billing & Payment KB`
- Embedding model: text-embedding-ada-002
- Upload: billing_faq.md only
- Chunk size: 500 characters

**Step 3: Create Technical Support KB**
- Create third Knowledge Base
- Name: `Technical Support KB`
- Embedding model: text-embedding-ada-002
- Upload: technical_troubleshooting.md only
- Chunk size: 500 characters

**Step 4: Create Chat Assistant**
- Click "Create App" → Select "Chat Assistant"
- Name: `Multi-Domain Support Assistant`
- Description: `Customer support using multiple specialized knowledge bases`

**Step 5: Connect Multiple KBs**
- In Chat Assistant settings, click "Add Knowledge Base"
- Add all 3 KBs you created
- Note: Order of KBs can affect retrieval priority

**Step 6: Configure System Prompt**
```
You are a comprehensive customer support assistant.

You have access to three specialized knowledge bases:
1. Service Plans - product information, pricing, features
2. Billing & Payment - payment methods, billing questions
3. Technical Support - troubleshooting, equipment issues

When answering:
- Search all relevant knowledge bases
- Combine information when needed for complex queries
- Cite which knowledge base each piece of information comes from
- If information is not found, direct to 0120-116-116
```

**Step 7: Configure Retrieval Settings**
- Top K: 5 (per KB)
- Score threshold: 0.5
- Retrieval mode: Hybrid

**Step 8: Test Cross-Domain Queries**
- Test queries that require information from multiple KBs
- Verify the assistant combines information appropriately

#### Test Scenarios

| Query | Expected KB Source | Expected Behavior |
|---|---|---|
| FLET'S Hikari pricing and installation fees? | Service Plans KB | Retrieves plan details |
| When do bills arrive and payment methods | Billing & Payment KB | Retrieves billing FAQ |
| Router restart procedure | Technical Support KB | Retrieves troubleshooting |
| My fiber bill is too high, want a cheaper plan | Multiple KBs | Combines plan + billing info |

#### Deliverable
Chat Assistant using multiple KBs with appropriate retrieval from each domain.

---

## Exercise 3-4: Knowledge Base Quality Assurance Workflow

**Complexity:** Medium-Hard
**Duration:** 60 minutes
**Pattern:** RAG-Enhanced Generation + Sequential Processing (Pattern 5 + Pattern 1)

#### Business Scenario
Before deploying a RAG application, you need to verify that the Knowledge Base provides relevant results for expected customer queries. Build a workflow that evaluates retrieval quality.

#### Dataset (Dummy - Inline)
Test queries with expected source documents:

| # | Test Query | Expected Source | Min Score |
|---|------------|-----------------|-----------|
| 1 | What is the monthly fee for fiber internet? | service_plans.md | 7 |
| 2 | How do I restart my router? | technical_troubleshooting.md | 8 |
| 3 | When is my payment due? | billing_faq.md | 7 |
| 4 | What does a red light on my ONU mean? | technical_troubleshooting.md | 8 |
| 5 | Can I pay by credit card? | billing_faq.md | 7 |
| 6 | What is the installation fee? | service_plans.md | 7 |
| 7 | How do I improve slow internet speed? | technical_troubleshooting.md | 8 |
| 8 | What is Hikari Denwa? | service_plans.md | 7 |
| 9 | Why is my bill higher this month? | billing_faq.md | 8 |
| 10 | How do I switch to IPv6? | technical_troubleshooting.md | 7 |
| 11 | What are the VPN service options? | service_plans.md | 6 |
| 12 | How do I report a fault? | technical_troubleshooting.md | 8 |

#### Workflow Structure
```
[Start: Test Query]
    |
[Knowledge Retrieval: Search KB]
    |
[LLM: Evaluate Relevance (Score 1-10)]
    |
[IF/ELSE: Score >= 7?]
    +-- Yes --> [LLM: Generate Answer] --> [Template: Format with Confidence]
    +-- No --> [Template: "Low confidence" Warning + Suggest KB improvement]
    |
[End: Answer or Improvement Suggestion]
```

#### Step-by-Step Instructions

**Step 1: Create Workflow App**
- Click "Create App" → Select "Workflow"
- Name: `KB Quality Assurance Workflow`
- Description: `Evaluates RAG retrieval quality and identifies improvement areas`

**Step 2: Configure Start Node**
- Add input variable: `query` (Text) - Label: Test Query
- This is where you'll paste test queries from the table above

**Step 3: Add Knowledge Retrieval Node**
- Drag "Knowledge Retrieval" node from toolbar
- Connect Start → Knowledge Retrieval
- Configure:
  - Select your KB from Exercise 3-1 (or any existing KB)
  - Top K: 5
  - Score threshold: 0.3 (lower for testing purposes)
- Output variable: `retrieved_context`

**Step 4: Add LLM Evaluation Node**
- Drag "LLM" node
- Connect Knowledge Retrieval → LLM
- Configure prompt:
```
Evaluate the relevance of the retrieved context for the following question on a scale of 1-10.

Question: {{query}}
Retrieved Context: {{retrieved_context}}

Scoring Criteria:
- 10: Fully relevant. Contains information that directly answers the question
- 7-9: Highly relevant. Contains key information
- 4-6: Partially relevant. Some useful information
- 1-3: Low relevance. Nearly unrelated to the question

Respond in the following JSON format:
{"score": number, "reason": "evaluation reason", "missing_info": "missing information"}
```
- Model: GPT-4o-mini
- Output variable: `evaluation`

**Step 5: Add Code Node to Extract Score**
- Drag "Code" node
- Connect LLM → Code
- Python code:
```python
import json
def main(evaluation: str) -> dict:
    try:
        data = json.loads(evaluation)
        return {"score": data.get("score", 0), "raw": evaluation}
    except:
        return {"score": 0, "raw": evaluation}
```
- Output variable: `parsed_score`

**Step 6: Add IF/ELSE Node**
- Drag "IF/ELSE" node
- Connect Code → IF/ELSE
- Condition: `{{parsed_score.score}}` >= 7
  - True branch → High Quality Path
  - False branch → Low Quality Path

**Step 7: Add High Quality Response (True Branch)**
- Add LLM node for answer generation
- Connect IF/ELSE (True) → LLM
- Prompt:
```
Based on the following context, answer the question.

Question: {{query}}
Context: {{retrieved_context}}

Provide a helpful, accurate answer.
```

**Step 8: Add Low Quality Warning (False Branch)**
- Add Template node
- Connect IF/ELSE (False) → Template
- Template:
```
⚠️ LOW CONFIDENCE RESULT

Query: {{query}}
Relevance Score: {{parsed_score.score}}/10
Evaluation: {{parsed_score.raw}}

Suggested Actions:
- Review KB content for this topic
- Add more relevant documents
- Adjust chunking strategy
```

**Step 9: Add End Node**
- Connect both branches to End node
- Output the final result

**Step 10: Test with All 12 Queries**
- Run each query from the test table
- Record actual scores vs. minimum expected scores
- Identify patterns in low-scoring queries

#### Deliverable
Workflow that evaluates RAG retrieval quality and flags low-confidence results.

---

# Session 4: Advanced Workflow Patterns
## Medium-Hard Complexity - Preparing for Real Business Workflows

**Target:** All Trainees (Individual Work)

These exercises introduce advanced workflow patterns that are essential for Session 5 team projects. Focus is on combining Doc Extractor with RAG, parallel processing, and variable aggregation - patterns not covered in Sessions 2-3.

**Design Principles Applied:** All 4 Principles
**Patterns Applied:** Sequential (Pattern 1) + Conditional (Pattern 2) + Parallel (Pattern 3) + RAG-Enhanced (Pattern 5)

**What's New in Session 4:**
- Doc Extractor combined with RAG (not covered in Session 2-5)
- Parallel Processing pattern with Variable Aggregator
- Multi-step LLM pipelines for complex analysis
- Code node for calculations

---

## Exercise 4-1: Document Analysis Pipeline (Doc Extractor + RAG)

**Complexity:** Medium-Hard
**Duration:** 90 minutes (Morning Session)
**Patterns:** Sequential Processing + RAG-Enhanced Generation (Pattern 1 + Pattern 5)

#### Business Scenario
When employees submit project proposals or business reports, reviewers need to check if the submission meets company guidelines. Currently, this is a manual process. Build a workflow that:
1. Extracts key information from an uploaded document
2. Searches the Knowledge Base for relevant guidelines
3. Generates a combined analysis report

This pattern is essential for Teams B, C, and F in Session 5 (document processing + RAG).

#### Dataset Files
**Location:** `data/exercise/Session_4/exercise_4-1/`

| File | Description | Usage |
|------|-------------|-------|
| `project_evaluation_guidelines.md` | Evaluation criteria for project proposals | Upload to Knowledge Base |
| `sample_proposal_good.md` | Well-structured project proposal | Convert to PDF, upload to test |
| `sample_proposal_poor.md` | Incomplete project proposal | Convert to PDF, upload to test |
| `test_scenarios.md` | Testing guide with expected results | Reference during testing |

#### Knowledge Base Document

**project_evaluation_guidelines.md:**
```markdown
# Project Proposal Evaluation Guidelines

## Required Sections

### 1. Executive Summary
- Clear problem statement (what issue is being addressed)
- Proposed solution overview
- Expected business impact
- Required: Yes, must be under 200 words

### 2. Problem Analysis
Required elements:
- Current state description with specific data
- Quantified impact (time, cost, error rate)
- Affected stakeholders identified
- Root cause analysis

Good example: "Sales team spends 3 hours daily on manual report creation, affecting 45 staff members across 6 branches. Annual cost: 15,000 hours."
Bad example: "Reports take too long."

### 3. Proposed Solution
Required elements:
- Technology or approach to be used
- Implementation scope (departments, processes)
- Timeline with milestones
- Resource requirements (budget, personnel)

### 4. Expected Outcomes
Required elements:
- Specific KPIs with target numbers
- Measurement methodology
- Timeline for achieving results
- Risk mitigation plan

Good example: "Reduce report creation time by 70% (from 3 hours to 54 minutes) within 6 months. Measured via time tracking system."
Bad example: "Will improve efficiency."

### 5. Budget & Resources
- Itemized cost breakdown
- Personnel requirements
- External vendor needs
- ROI calculation

## Evaluation Scoring

| Criteria | Weight | Score Range |
|----------|--------|-------------|
| Problem Clarity | 25% | 0-100 |
| Solution Feasibility | 25% | 0-100 |
| Expected Impact | 25% | 0-100 |
| Resource Planning | 15% | 0-100 |
| Strategic Alignment | 10% | 0-100 |

## Common Issues
1. Vague problem statements without data
2. Missing quantified KPIs
3. Unrealistic timelines
4. No risk consideration
5. Missing budget breakdown
```

#### Workflow Structure
```
[Start: Upload Project Proposal (File) + Proposal Type (Select)]
        |
[Doc Extractor: Parse Uploaded Document]
        |
[LLM: Extract Key Fields as JSON]
        |
        Output: {
          "has_executive_summary": true/false,
          "has_problem_data": true/false,
          "has_quantified_kpis": true/false,
          "has_timeline": true/false,
          "has_budget": true/false,
          "extracted_kpis": [],
          "extracted_timeline": ""
        }
        |
[Knowledge Retrieval: Get Evaluation Guidelines]
        |
[LLM: Compare Document Against Guidelines]
        |
        Output: {
          "section_scores": {...},
          "overall_score": 0-100,
          "missing_elements": [],
          "strengths": [],
          "improvement_areas": []
        }
        |
[Template: Format Analysis Report]
        |
[End: Document Analysis Report]
```

#### Step-by-Step Instructions

**Phase 1: Knowledge Base Setup (20 min)**

**Step 1: Create Knowledge Base**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `Project Evaluation KB`
- Description: `Guidelines for evaluating project proposals`
- Embedding model: text-embedding-ada-002

**Step 2: Upload Guidelines Document**
- Upload `project_evaluation_guidelines.md`
- Chunk size: 500 characters
- Chunk overlap: 50 characters
- Click "Save and Process"

**Step 3: Test KB Retrieval**
- Click "Hit Testing"
- Test queries:
  - `What are required sections?`
  - `How to write good KPIs?`
  - `Evaluation scoring criteria`

**Phase 2: Build Workflow (50 min)**

**Step 4: Create Workflow**
- Click "Create App" → Select "Workflow"
- Name: `Document Analysis Pipeline`
- Description: `Analyzes project proposals against evaluation guidelines`

**Step 5: Configure Start Node**
- Add input variable: `document_file` (File) - Label: Upload Proposal Document
- Add input variable: `proposal_type` (Select) - Label: Proposal Type
  - Options: New Project, Enhancement, Cost Reduction, Process Improvement

**Step 6: Add Doc Extractor Node**
- Drag "Doc Extractor" from toolbar
- Connect Start → Doc Extractor
- Input: `{{document_file}}`
- Output variable: `extracted_text`

**Step 7: Add LLM Field Extraction Node**
- Connect Doc Extractor → LLM
- Prompt:
```
Analyze the following project proposal and extract key information.

Document Content:
{{extracted_text}}

Extract and return in JSON format:
{
  "has_executive_summary": true/false,
  "has_problem_data": true/false (look for specific numbers, percentages, costs),
  "has_quantified_kpis": true/false (look for measurable targets),
  "has_timeline": true/false,
  "has_budget": true/false,
  "problem_statement": "brief summary of the problem",
  "proposed_solution": "brief summary of the solution",
  "extracted_kpis": ["list of KPIs found"],
  "extracted_timeline": "timeline if found",
  "budget_amount": "budget if found"
}
```
- Model: GPT-4o-mini
- Output variable: `extracted_fields`

**Step 8: Add Knowledge Retrieval Node**
- Connect LLM → Knowledge Retrieval
- Select `Project Evaluation KB`
- Query: `evaluation criteria scoring guidelines`
- Top K: 5
- Output variable: `guidelines`

**Step 9: Add LLM Analysis Node**
- Connect Knowledge Retrieval → LLM
- Prompt:
```
Compare this project proposal against the evaluation guidelines.

Extracted Proposal Information:
{{extracted_fields}}

Evaluation Guidelines:
{{guidelines}}

Score each section (0-100) and provide analysis in JSON:
{
  "section_scores": {
    "executive_summary": 0,
    "problem_analysis": 0,
    "proposed_solution": 0,
    "expected_outcomes": 0,
    "budget_resources": 0
  },
  "overall_score": 0,
  "strengths": ["list of strong points"],
  "missing_elements": ["list of missing required elements"],
  "improvement_suggestions": ["specific actionable suggestions"]
}
```
- Output variable: `analysis`

**Step 10: Add Template Node**
- Connect LLM → Template
- Template:
```
==========================================
PROJECT PROPOSAL ANALYSIS REPORT
==========================================
Proposal Type: {{proposal_type}}

OVERALL SCORE: {{analysis.overall_score}}/100

SECTION SCORES:
- Executive Summary: {{analysis.section_scores.executive_summary}}/100
- Problem Analysis: {{analysis.section_scores.problem_analysis}}/100
- Proposed Solution: {{analysis.section_scores.proposed_solution}}/100
- Expected Outcomes: {{analysis.section_scores.expected_outcomes}}/100
- Budget & Resources: {{analysis.section_scores.budget_resources}}/100

STRENGTHS:
{{analysis.strengths}}

MISSING ELEMENTS:
{{analysis.missing_elements}}

IMPROVEMENT SUGGESTIONS:
{{analysis.improvement_suggestions}}
==========================================
* Analysis generated by Document Analysis Pipeline
```

**Step 11: Connect to End Node**

#### Testing Scenarios

| # | Test Document | Expected Score | Key Findings |
|---|---------------|----------------|--------------|
| 1 | sample_proposal_good.md | 80-90 | All sections present, minor improvements |
| 2 | sample_proposal_poor.md | 40-60 | Missing KPIs, vague problem statement |
| 3 | Custom proposal | Varies | Test with your own document |

#### Deliverable
Workflow that extracts information from uploaded documents and analyzes them against KB guidelines - combining Doc Extractor with RAG.

---

## Exercise 4-2: Parallel Processing & Aggregation Workflow

**Complexity:** Hard
**Duration:** 90 minutes (Afternoon Session)
**Patterns:** Parallel Processing + Variable Aggregator + Conditional Branching + Code Node (Pattern 3 + Pattern 2)

#### Business Scenario
When evaluating submissions (project proposals, requests, applications), multiple criteria must be assessed simultaneously. Currently, reviewers check each criterion sequentially, which is time-consuming and inconsistent. Build a workflow that:
1. Analyzes multiple evaluation criteria in parallel
2. Aggregates results using Variable Aggregator
3. Calculates a weighted final score using Code node
4. Routes to different outcomes based on the score

This pattern is essential for Teams A, D, and E in Session 5 (multi-criteria evaluation).

#### Dataset Files
**Location:** `data/exercise/Session_4/exercise_4-2/`

| File | Description | Usage |
|------|-------------|-------|
| `evaluation_criteria.md` | Multi-criteria evaluation framework | Reference for parallel evaluation |
| `sample_submissions.csv` | 6 test submissions (good/medium/poor) | Test data for workflow |
| `scoring_weights.md` | Weighted scoring explanation | Reference for Code node |

#### Knowledge Base Document

**evaluation_criteria.md:**
```markdown
# Multi-Criteria Evaluation Framework

## Evaluation Dimensions

### 1. Completeness (Weight: 30%)
Checks if all required fields are present:
- Problem statement: Present with specific details
- Solution description: Clear approach defined
- Timeline: Milestones identified
- Budget: Cost breakdown included
- KPIs: Measurable targets defined

Scoring:
- 100: All 5 elements present
- 80: 4 elements present
- 60: 3 elements present
- 40: 2 elements present
- 20: 1 element present
- 0: No elements present

### 2. Feasibility (Weight: 25%)
Evaluates if the proposal is realistic:
- Technical approach is proven/achievable
- Timeline is realistic (not too aggressive)
- Budget is reasonable for scope
- Resources are available or obtainable
- Risks are identified and mitigated

Scoring:
- 100: Highly feasible, all factors addressed
- 75: Feasible with minor concerns
- 50: Partially feasible, some gaps
- 25: Low feasibility, major concerns
- 0: Not feasible

### 3. Impact (Weight: 25%)
Measures expected business value:
- Clear quantified benefits (time/cost savings)
- Affects significant number of users/processes
- Aligns with company strategic priorities
- ROI calculation provided
- Scalability potential

Scoring:
- 100: High impact, clear ROI
- 75: Good impact, reasonable ROI
- 50: Moderate impact
- 25: Limited impact
- 0: No clear impact

### 4. Quality (Weight: 20%)
Assesses presentation and clarity:
- Well-structured document
- Clear, professional writing
- Data and claims are supported
- No contradictions or gaps
- Easy to understand

Scoring:
- 100: Excellent quality
- 75: Good quality
- 50: Acceptable quality
- 25: Poor quality
- 0: Unacceptable

## Decision Thresholds

| Score Range | Decision | Action |
|-------------|----------|--------|
| 80-100 | Approve | Proceed to implementation |
| 60-79 | Review | Request minor revisions |
| 40-59 | Revise | Return with feedback |
| 0-39 | Reject | Decline with explanation |
```

#### Workflow Structure
```
[Start: Submission Text + Submitter Name]
        |
[LLM: Parse Submission into Structured Fields]
        |
========================
| PARALLEL PROCESSING  |
========================
    |           |           |           |
[LLM:       [LLM:       [LLM:       [LLM:
Evaluate    Evaluate    Evaluate    Evaluate
Complete-   Feasi-      Impact]     Quality]
ness]       bility]
    |           |           |           |
    Score A     Score B     Score C     Score D
========================
        |
[Variable Aggregator: Combine All Scores]
        |
[Code Node: Calculate Weighted Final Score]
        |
        weighted_score = (A*0.30 + B*0.25 + C*0.25 + D*0.20)
        |
[IF/ELSE: Route by Score]
    |           |           |           |
≥80         60-79       40-59       <40
Approve     Review      Revise      Reject
    |           |           |           |
[LLM: Generate Approval Summary]
[LLM: Generate Review Request]
[LLM: Generate Revision Feedback]
[LLM: Generate Rejection Explanation]
        |
[Template: Format Final Decision Report]
        |
[End: Decision + Detailed Feedback]
```

#### Step-by-Step Instructions

**Phase 1: Understanding the Pattern (10 min)**

Review the workflow structure above. Key concepts:
- **Parallel Processing**: Multiple LLM nodes run simultaneously
- **Variable Aggregator**: Collects outputs from parallel branches
- **Code Node**: Python code for weighted calculation
- **Multi-branch IF/ELSE**: 4 different outcome paths

**Phase 2: Build the Workflow (60 min)**

**Step 1: Create Workflow**
- Click "Create App" → Select "Workflow"
- Name: `Multi-Criteria Evaluator`
- Description: `Evaluates submissions using parallel criteria analysis`

**Step 2: Configure Start Node**
- Add input variable: `submission_text` (Paragraph) - Label: Submission Content
- Add input variable: `submitter_name` (Text) - Label: Submitter Name
- Add input variable: `submission_type` (Select) - Label: Type
  - Options: Project Proposal, Budget Request, Process Change, New Initiative

**Step 3: Add Initial LLM Parser Node**
- Connect Start → LLM
- Prompt:
```
Parse this submission and extract key elements for evaluation.

Submission:
{{submission_text}}

Extract in JSON format:
{
  "problem_statement": "extracted problem or empty string",
  "solution_description": "extracted solution or empty string",
  "timeline": "extracted timeline or empty string",
  "budget": "extracted budget or empty string",
  "kpis": "extracted KPIs or empty string",
  "full_text": "original submission text"
}
```
- Output variable: `parsed_submission`

**Step 4: Add Parallel Processing Node**
- Drag "Parallel" node from toolbar (or use multiple branches)
- Connect LLM Parser → Parallel node
- Create 4 parallel branches

**Step 5: Add LLM Nodes in Each Parallel Branch**

**Branch 1 - Completeness Evaluation:**
```
Evaluate the COMPLETENESS of this submission.

Submission:
{{parsed_submission}}

Check for presence of: problem statement, solution, timeline, budget, KPIs.

Return JSON:
{
  "dimension": "completeness",
  "score": 0-100,
  "elements_found": ["list of found elements"],
  "elements_missing": ["list of missing elements"],
  "reasoning": "brief explanation"
}
```
Output: `completeness_score`

**Branch 2 - Feasibility Evaluation:**
```
Evaluate the FEASIBILITY of this submission.

Submission:
{{parsed_submission}}

Assess: technical achievability, timeline realism, budget reasonableness, resource availability, risk mitigation.

Return JSON:
{
  "dimension": "feasibility",
  "score": 0-100,
  "strengths": ["feasibility strengths"],
  "concerns": ["feasibility concerns"],
  "reasoning": "brief explanation"
}
```
Output: `feasibility_score`

**Branch 3 - Impact Evaluation:**
```
Evaluate the IMPACT of this submission.

Submission:
{{parsed_submission}}

Assess: quantified benefits, user/process scope, strategic alignment, ROI clarity, scalability.

Return JSON:
{
  "dimension": "impact",
  "score": 0-100,
  "benefits": ["identified benefits"],
  "gaps": ["impact gaps"],
  "reasoning": "brief explanation"
}
```
Output: `impact_score`

**Branch 4 - Quality Evaluation:**
```
Evaluate the QUALITY of this submission's presentation.

Submission:
{{parsed_submission}}

Assess: structure, clarity, supporting data, consistency, readability.

Return JSON:
{
  "dimension": "quality",
  "score": 0-100,
  "positives": ["quality positives"],
  "improvements": ["quality improvements needed"],
  "reasoning": "brief explanation"
}
```
Output: `quality_score`

**Step 6: Add Variable Aggregator**
- Connect all 4 parallel branches → Variable Aggregator
- Aggregate variables: `completeness_score`, `feasibility_score`, `impact_score`, `quality_score`
- Output format: Object
- Output variable: `all_scores`

**Step 7: Add Code Node**
- Connect Variable Aggregator → Code
- Python code:
```python
import json

def main(completeness_score: str, feasibility_score: str, impact_score: str, quality_score: str) -> dict:
    # Parse JSON scores
    try:
        c = json.loads(completeness_score)["score"]
        f = json.loads(feasibility_score)["score"]
        i = json.loads(impact_score)["score"]
        q = json.loads(quality_score)["score"]
    except:
        c = f = i = q = 0

    # Calculate weighted score
    weighted = (c * 0.30) + (f * 0.25) + (i * 0.25) + (q * 0.20)

    # Determine decision
    if weighted >= 80:
        decision = "APPROVE"
    elif weighted >= 60:
        decision = "REVIEW"
    elif weighted >= 40:
        decision = "REVISE"
    else:
        decision = "REJECT"

    return {
        "completeness": c,
        "feasibility": f,
        "impact": i,
        "quality": q,
        "weighted_score": round(weighted, 1),
        "decision": decision
    }
```
- Output variable: `final_scores`

**Step 8: Add IF/ELSE Node**
- Connect Code → IF/ELSE
- Condition 1: `{{final_scores.decision}}` equals `APPROVE`
- Condition 2: `{{final_scores.decision}}` equals `REVIEW`
- Condition 3: `{{final_scores.decision}}` equals `REVISE`
- Default: `REJECT`

**Step 9: Add LLM Nodes for Each Branch**

Create 4 LLM nodes, one for each decision path. Example for APPROVE:
```
Generate an approval summary for this submission.

Submitter: {{submitter_name}}
Type: {{submission_type}}
Scores: {{final_scores}}
Details:
- Completeness: {{completeness_score}}
- Feasibility: {{feasibility_score}}
- Impact: {{impact_score}}
- Quality: {{quality_score}}

Write a brief approval message highlighting strengths and any minor recommendations.
```

**Step 10: Add Template Node**
- Connect all 4 decision branches → Template
- Template:
```
==========================================
SUBMISSION EVALUATION REPORT
==========================================
Submitter: {{submitter_name}}
Type: {{submission_type}}
Date: {{current_date}}

DECISION: {{final_scores.decision}}
WEIGHTED SCORE: {{final_scores.weighted_score}}/100

DIMENSION SCORES:
- Completeness (30%): {{final_scores.completeness}}/100
- Feasibility (25%): {{final_scores.feasibility}}/100
- Impact (25%): {{final_scores.impact}}/100
- Quality (20%): {{final_scores.quality}}/100

DETAILED FEEDBACK:
{{decision_feedback}}
==========================================
```

**Step 11: Connect to End Node**

#### Testing Scenarios

**Sample Submissions (from sample_submissions.csv):**

| # | Submitter | Type | Expected Score | Expected Decision |
|---|-----------|------|----------------|-------------------|
| 1 | Tanaka | Project Proposal | 85+ | APPROVE |
| 2 | Suzuki | Budget Request | 70 | REVIEW |
| 3 | Yamamoto | Process Change | 55 | REVISE |
| 4 | Sato | New Initiative | 30 | REJECT |

**Test Input 1 (Good Submission):**
```
Project: Sales Report Automation
Problem: Sales team spends 3 hours daily creating reports manually. Affects 45 staff, costs 15,000 hours annually.
Solution: Implement automated report generation using existing CRM data and Power BI dashboards.
Timeline: Phase 1 (2 months): Requirements and design. Phase 2 (3 months): Development. Phase 3 (1 month): Training and rollout.
Budget: 2,500,000 JPY (Development: 1,800,000, Training: 400,000, Contingency: 300,000)
KPIs: Reduce report time by 80% (from 3 hours to 36 minutes). ROI positive within 8 months.
```

**Test Input 2 (Poor Submission):**
```
We should improve the reporting process. It takes too long and people complain about it. We could use some AI or automation to make it better. This would help everyone.
```

#### Deliverable
Workflow demonstrating parallel processing, variable aggregation, code-based calculation, and multi-branch conditional routing.

---

# Session 5: Building Business Workflows
## Hard Complexity - Real Business Scenario Workflows

These exercises apply all workflow patterns from the Workflow Design Principles & Patterns training material to solve real business problems. Each exercise is directly aligned with one team's initiative.

**Design Principles Applied:** All 4 Principles (IPO Thinking, Modular Design, Error Handling, Optimization)
**Patterns Applied:** All 5 Patterns as noted per exercise

**Team Assignment:**
| Exercise | Team | Real Problem |
|----------|------|--------------|
| 5-1 | Team A | DX Entry Sheet Support System |
| 5-2 | Team B | Project Handover Automation |
| 5-3 | Team C | RFP Analysis & Proposal Support |
| 5-4 | Team D | Customer Complaint Analysis |
| 5-5 | Team E | Accounting Code Recommendation |
| 5-6 | Team F | Publicity Analysis System |

---

## Group Exercise 5-1: DX Entry Sheet Support System (Team A)

**Business Focus:** DX Initiative Support
**Team Size:** 3-4 members
**Complexity:** Hard
**Duration:** 90 minutes
**Patterns Used:** Sequential Processing + Conditional Branching + RAG-Enhanced Generation (Pattern 1 + 2 + 5)

### Business Scenario
Internal applicants struggle to correctly match their DX project ideas to the appropriate DX entry sheet fields. The DX Promotion team currently spends significant time reviewing incomplete or incorrectly filled forms. Build a system that helps applicants fill out forms correctly and assists evaluators in reviewing submissions.

### Dataset (Dummy - Inline)

**DX Entry Applications:**

| App ID | Applicant | Department | Project Idea | Current Problem | Expected Effect |
|--------|-----------|------------|--------------|-----------------|-----------------|
| DX001 | Taro Tanaka | Sales | Automate customer visit reports using AI | Manual report writing takes 2 hours daily | 70% time reduction |
| DX002 | Hanako Suzuki | HR | Employee FAQ chatbot | Same questions answered repeatedly | 50% inquiry reduction |
| DX003 | Kenji Yamamoto | Finance | Invoice processing automation | Manual data entry errors | 90% error reduction |
| DX004 | Yuki Sato | Maintenance | Equipment failure prediction | Reactive maintenance causes downtime | 30% downtime reduction |
| DX005 | Misaki Ito | Planning | Meeting minutes auto-generation | Time-consuming manual minutes | 80% time reduction |
| DX006 | Ken Watanabe | Customer Service | Inquiry routing automation | Manual routing delays response | 40% faster response |
| DX007 | Akira Nakamura | Logistics | Delivery route optimization | Inefficient routes waste fuel | 15% cost reduction |
| DX008 | Mika Kobayashi | Marketing | Campaign effectiveness analysis | Manual report compilation | Real-time dashboards |

**Incomplete/Problematic Applications (for validation testing):**

| App ID | Issue | Missing/Incorrect Field |
|--------|-------|------------------------|
| DX009 | Vague problem statement | "Things are slow" - no specifics |
| DX010 | No measurable KPI | "Will improve efficiency" - no numbers |
| DX011 | Technical solution without business context | Only mentions "use AI" |
| DX012 | Unrealistic expectations | "100% automation immediately" |

### Knowledge Base Documents

**dx_entry_guidelines.md:**
```markdown
# DX Entry Sheet Guidelines

## Required Fields

### 1. Project Title
- Clear and concise (under 50 characters)
- Should indicate the automation/improvement target

### 2. Current Problem Description
Required elements:
- Specific process being addressed
- Quantified current state (time, cost, error rate)
- Who is affected (stakeholders)
- Frequency of the problem

Good example: "Sales representatives spend 2 hours daily writing customer visit reports manually. This affects 50 sales staff across 6 branches."

Bad example: "Report writing is slow."

### 3. Proposed Solution
Required elements:
- Technology/approach to be used
- How it addresses the problem
- Scope (which processes, which departments)

### 4. Expected Effect (KPIs)
Required elements:
- Specific metrics with target numbers
- Measurement method
- Timeline for achieving results

Good example: "Reduce report writing time from 2 hours to 30 minutes (75% reduction) within 3 months of deployment."

Bad example: "Will be faster."

### 5. Required Resources
- Budget estimate
- Personnel requirements
- Timeline
- External support needs

## Evaluation Criteria

| Criteria | Weight | Description |
|----------|--------|-------------|
| Problem Clarity | 25% | Is the problem well-defined and quantified? |
| Solution Feasibility | 25% | Is the proposed solution realistic? |
| Expected Impact | 25% | Are KPIs clear and achievable? |
| Resource Planning | 15% | Are resource needs realistic? |
| Alignment | 10% | Does it align with company DX strategy? |

## Common Rejection Reasons
1. Vague problem description without data
2. No measurable success criteria
3. Unrealistic timeline or expectations
4. Missing stakeholder analysis
5. No consideration of risks or constraints
```

### Workflow Structure
```
[Start: DX Entry Application Input]
        |
[LLM: Extract and Parse Application Fields]
        |
[Parallel Processing]
    |                              |
[LLM: Validate Each Field]    [KB: Get Guidelines]
        |
[Variable Aggregator: Combine Validation Results]
        |
[LLM: Generate Completeness Score & Feedback]
        |
[IF/ELSE: Score Check]
    +-- Score >= 80% --> [LLM: Generate Evaluator Summary]
    +-- Score < 80% --> [LLM: Generate Improvement Suggestions]
        |
[Template: Format Output Report]
        |
[End: Validation Report + Recommendations]
```

### Expected Output Example
```json
{
  "application_id": "DX001",
  "validation_result": {
    "overall_score": 85,
    "field_scores": {
      "project_title": 90,
      "problem_description": 85,
      "proposed_solution": 80,
      "expected_effect": 90,
      "resource_planning": 75
    }
  },
  "feedback": {
    "strengths": [
      "Clear problem quantification (2 hours daily)",
      "Specific KPI target (70% reduction)"
    ],
    "improvements_needed": [
      "Add timeline for implementation",
      "Specify budget requirements"
    ]
  },
  "evaluator_summary": "Strong application with clear business case. Recommend approval with request for budget details.",
  "next_steps": ["Request budget estimate", "Schedule technical review"]
}
```

### Step-by-Step Instructions

**Phase 1: Knowledge Base Setup (20 min)**

**Step 1: Create Knowledge Base**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `DX Entry Guidelines KB`
- Upload: `dx_entry_guidelines.md`
- Chunk size: 600 characters

**Step 2: Test KB Retrieval**
- Hit Testing with queries: `problem description requirements`, `KPI criteria`, `rejection reasons`

**Phase 2: Build Workflow (50 min)**

**Step 3: Create Workflow**
- Click "Create App" → Select "Workflow"
- Name: `DX Entry Validator`
- Description: `Validates DX entry applications and provides feedback`

**Step 4: Configure Start Node**
- Input variables:
  - `applicant_name` (Text) - Label: Applicant Name
  - `department` (Text) - Label: Department
  - `project_title` (Text) - Label: Project Title
  - `problem_description` (Paragraph) - Label: Current Problem
  - `proposed_solution` (Paragraph) - Label: Proposed Solution
  - `expected_effect` (Paragraph) - Label: Expected Effect/KPIs

**Step 5: Add LLM Field Extraction Node**
- Connect Start → LLM
- Prompt:
```
Parse the following DX Entry application and identify key elements:

Project Title: {{project_title}}
Problem: {{problem_description}}
Solution: {{proposed_solution}}
Expected Effect: {{expected_effect}}

Extract in JSON format:
{
  "has_quantified_problem": true/false,
  "has_stakeholder_info": true/false,
  "has_specific_kpi": true/false,
  "has_timeline": true/false,
  "has_resource_estimate": true/false,
  "extracted_metrics": []
}
```

**Step 6: Add Knowledge Retrieval Node (Parallel)**
- Connect Start → Knowledge Retrieval
- Connect to `DX Entry Guidelines KB`
- Output: `guidelines`

**Step 7: Add Variable Aggregator**
- Combine LLM output and KB guidelines

**Step 8: Add Scoring LLM Node**
- Prompt: Evaluate application against guidelines, score each field 0-100
- Output scoring JSON

**Step 9: Add IF/ELSE Node**
- Condition: Overall score >= 80
- True branch → Generate approval summary
- False branch → Generate improvement suggestions

**Step 10: Add Output Template Node**
```
=== DX Entry Validation Report ===
Applicant: {{applicant_name}} ({{department}})
Project: {{project_title}}

Overall Score: {{overall_score}}/100

Field Scores:
- Problem Description: {{problem_score}}/100
- Proposed Solution: {{solution_score}}/100
- Expected Effect: {{effect_score}}/100

{{#if approved}}
✅ RECOMMENDED FOR APPROVAL
Summary: {{evaluator_summary}}
{{else}}
⚠️ NEEDS IMPROVEMENT
Suggestions:
{{improvement_suggestions}}
{{/if}}
===
```

**Phase 3: Testing (20 min)**

**Step 11: Test with Sample Applications**
- Test with DX001-DX008 (well-formed applications)
- Test with DX009-DX012 (problematic applications)
- Verify appropriate feedback is generated

### Deliverables
1. Entry validation workflow with parallel processing
2. Knowledge Base with DX guidelines
3. Applicant feedback generator
4. Evaluator summary generator

---

## Group Exercise 5-2: Project Handover Automation (Team B)

**Business Focus:** Knowledge Management & Business Continuity
**Team Size:** 3-4 members
**Complexity:** Hard
**Duration:** 90 minutes
**Patterns Used:** Sequential + RAG-Enhanced + Template Generation (Pattern 1 + 5)

### Business Scenario
Community development project information is siloed with individual staff members. When staff transfer or resign, handover quality varies significantly and successors struggle to understand project context. Build a system that automatically generates comprehensive handover documents from scattered project files.

### Dataset (Dummy - Inline)

**Project Files (simulate scattered documents):**

**project_emails.md:**
```markdown
# Project Email Archive

## Email 1: Project Kickoff
Date: 2024-04-15
From: Tanaka (Community Dev)
To: Sendai City Planning Division
Subject: Community Wi-Fi Project Kickoff

We agreed on the following during our meeting:
- Project scope: 5 community centers in Aoba Ward
- Timeline: Installation complete by December 2024
- Budget: 15 million yen from city subsidy
- Key contact: Mr. Sato (City Planning)

## Email 2: Vendor Selection
Date: 2024-06-01
From: Tanaka
To: Procurement Team
Subject: Wi-Fi Vendor Selection Result

Selected vendor: TechConnect Solutions
Reasons:
- Best price-performance ratio
- 24/7 support availability
- Local presence in Sendai

## Email 3: Installation Delay
Date: 2024-09-15
From: TechConnect Solutions
To: Tanaka
Subject: Installation Delay Notice

Due to supply chain issues, installation at Center #3 will be delayed by 2 weeks. New completion date: October 30.

## Email 4: Completion Report
Date: 2024-12-10
From: Tanaka
To: All Stakeholders
Subject: Project Completion

All 5 community centers now have operational Wi-Fi. Final cost: 14.2 million yen (under budget).
```

**project_meeting_notes.md:**
```markdown
# Meeting Minutes Archive

## Meeting: Project Review - 2024-07-20
Attendees: Tanaka, Suzuki, Mr. Sato (City)
Decisions:
- Approved change in antenna placement at Center #2
- Agreed to add outdoor coverage at Center #4
- Budget increase of 500,000 yen approved

## Meeting: Risk Assessment - 2024-08-15
Attendees: Tanaka, IT Security Team
Decisions:
- Implement content filtering for family-friendly access
- Add usage monitoring for grant compliance
- Security audit scheduled for November

## Meeting: Final Review - 2024-12-05
Attendees: All stakeholders
Decisions:
- Project accepted by city
- Maintenance contract starts January 2025
- Annual review meetings scheduled
```

**project_files_list.md:**
```markdown
# Project Document Index

## Contracts
- vendor_contract_20240701.pdf - Main installation contract
- maintenance_agreement_20241201.pdf - 3-year support contract
- city_grant_agreement_20240401.pdf - Funding agreement

## Technical Documents
- network_diagram_v3.pdf - Final network architecture
- equipment_inventory.xlsx - All installed equipment list
- configuration_backup/ - Router and AP configurations

## Reports
- monthly_progress_reports/ - 8 monthly reports
- final_completion_report.pdf - Official completion document
- budget_reconciliation.xlsx - Final cost breakdown

## Communications
- email_archive/ - Key email threads
- meeting_minutes/ - All meeting notes
```

**Handover Cases (simulate multiple projects):**

| Project | Status | Outgoing Staff | Key Contacts | Critical Dates |
|---------|--------|----------------|--------------|----------------|
| Aoba Ward Wi-Fi | Complete | Tanaka | Mr. Sato (City) | Maint. review: 2025-06 |
| Izumi Senior Center IT | In Progress | Suzuki | Ms. Yamada | Install: 2025-03 |
| Wakabayashi Youth Hub | Planning | Yamamoto | Mr. Ito | Proposal due: 2025-02 |
| Taihaku Digital Literacy | Pending | Nakamura | Dr. Kobayashi | Grant deadline: 2025-04 |

### Knowledge Base Documents

**handover_template.md:**
```markdown
# Project Handover Document Template

## 1. Project Overview
- Project name and ID
- Purpose and objectives
- Current status
- Key stakeholders

## 2. Timeline & Milestones
- Key dates achieved
- Upcoming deadlines
- Critical decision points

## 3. Key Decisions & Rationale
- Major decisions made
- Why they were made
- Who approved them

## 4. Contacts & Relationships
- Internal stakeholders
- External partners
- Key contact persons
- Relationship status

## 5. Documents & Resources
- Critical documents location
- Passwords/access information
- System configurations

## 6. Pending Items & Risks
- Open issues
- Known risks
- Required follow-ups

## 7. Recommendations for Successor
- Priority actions
- Relationship tips
- Potential challenges
```

### Workflow Structure
```
[Start: Project Files Input (Multiple Documents)]
        |
[Doc Extractor: Parse All Document Types]
        |
[LLM: Extract Key Information from Each Document]
        |
[Knowledge Retrieval: Get Handover Template]
        |
[LLM: Synthesize Timeline & Decision History]
        |
[LLM: Identify Key Contacts & Relationships]
        |
[LLM: Generate Handover Document Following Template]
        |
[Template: Format as Standardized Handover Package]
        |
[End: Complete Handover Document]
```

### Expected Output Example
```json
{
  "project_name": "Aoba Ward Community Wi-Fi",
  "handover_summary": {
    "status": "Complete",
    "outgoing_staff": "Tanaka",
    "effective_date": "2025-01-15"
  },
  "timeline_reconstruction": [
    {"date": "2024-04-15", "event": "Project kickoff", "decision": "Scope agreed"},
    {"date": "2024-06-01", "event": "Vendor selected", "decision": "TechConnect chosen"},
    {"date": "2024-07-20", "event": "Scope change approved", "decision": "+500K budget"},
    {"date": "2024-12-10", "event": "Project completed", "outcome": "Under budget"}
  ],
  "key_contacts": [
    {"name": "Mr. Sato", "org": "Sendai City", "role": "Primary contact", "relationship": "Excellent"},
    {"name": "TechConnect PM", "org": "Vendor", "role": "Technical support", "relationship": "Good"}
  ],
  "pending_items": [
    {"item": "June 2025 maintenance review", "deadline": "2025-06-01", "action_needed": "Schedule meeting"},
    {"item": "Usage report for city", "deadline": "2025-03-31", "action_needed": "Compile data"}
  ],
  "successor_recommendations": [
    "Build relationship with Mr. Sato early - he is key decision maker",
    "Review maintenance contract before June review",
    "City expects quarterly usage statistics"
  ]
}
```

### Step-by-Step Instructions

**Phase 1: Knowledge Base Setup (15 min)**

**Step 1: Create Knowledge Base**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `Handover Templates KB`
- Upload: `handover_template.md`
- Chunk size: 800 characters (template needs context)

**Phase 2: Build Workflow (55 min)**

**Step 2: Create Workflow**
- Click "Create App" → Select "Workflow"
- Name: `Project Handover Generator`
- Description: `Generates handover documents from project files`

**Step 3: Configure Start Node**
- Input variables:
  - `project_name` (Text) - Label: Project Name
  - `outgoing_staff` (Text) - Label: Outgoing Staff Name
  - `project_files` (Files) - Label: Upload Project Documents
  - Note: Accept multiple files (emails, minutes, file lists)

**Step 4: Add Doc Extractor Node**
- Connect Start → Doc Extractor
- Input: `{{project_files}}`
- Output: `extracted_content`
- This parses PDF, Word, Excel, etc.

**Step 5: Add LLM Information Extraction Node**
- Connect Doc Extractor → LLM
- Prompt:
```
Analyze the following project documents and extract key information:

{{extracted_content}}

Extract in JSON format:
{
  "timeline_events": [{"date": "", "event": "", "decision": ""}],
  "key_contacts": [{"name": "", "organization": "", "role": "", "relationship_quality": ""}],
  "pending_items": [{"item": "", "deadline": "", "action_needed": ""}],
  "critical_documents": [{"name": "", "location": "", "purpose": ""}],
  "important_decisions": [{"decision": "", "rationale": "", "approver": ""}]
}
```

**Step 6: Add Knowledge Retrieval Node**
- Connect to `Handover Templates KB`
- Retrieve handover template structure
- Output: `template_structure`

**Step 7: Add Timeline Synthesis LLM Node**
- Combine extracted timeline with template
- Generate chronological project history

**Step 8: Add Contact & Relationship LLM Node**
- Analyze extracted contacts
- Generate relationship notes and tips for successor

**Step 9: Add Final Document Generation LLM Node**
- Combine all analysis
- Generate complete handover document following template

**Step 10: Add Template Formatting Node**
```
==============================================
PROJECT HANDOVER DOCUMENT
==============================================
Project: {{project_name}}
Outgoing Staff: {{outgoing_staff}}
Generated: {{current_date}}
==============================================

{{handover_content}}

==============================================
CRITICAL UPCOMING DATES
{{critical_dates}}

SUCCESSOR RECOMMENDATIONS
{{recommendations}}
==============================================
```

**Phase 3: Testing (20 min)**

**Step 11: Test with Sample Project**
- Use the Aoba Ward Wi-Fi project files
- Verify timeline extraction is accurate
- Check contact information is captured
- Ensure recommendations are relevant

**Step 12: Test with Different Document Types**
- Test with PDF contracts
- Test with Excel equipment lists
- Verify Doc Extractor handles all formats

### Deliverables
1. Document parsing and synthesis workflow
2. Multi-source information integration
3. Timeline reconstruction from scattered documents
4. Standardized handover document generation

---

## Group Exercise 5-3: RFP Analysis & Proposal Support (Team C)

**Business Focus:** Sales - Proposal Development
**Team Size:** 3-4 members
**Complexity:** Hard
**Duration:** 90 minutes
**Patterns Used:** Sequential + RAG-Enhanced + Parallel Processing (Pattern 1 + 3 + 5)

### Business Scenario
Reading RFP documents and searching for similar past cases is time-consuming. Proposal quality varies by staff member. Build a system that extracts requirements from RFPs, finds relevant past proposals, and generates proposal strategy recommendations.

### Dataset (Dummy - Inline)

**Sample RFP Document (rfp_sample.md):**
```markdown
# Request for Proposal
## Municipal Network Infrastructure Upgrade

### 1. Project Overview
Sendai City seeks proposals for upgrading the network infrastructure across 15 municipal facilities including city hall, branch offices, and community centers.

### 2. Requirements

#### 2.1 Technical Requirements
- Minimum 1Gbps backbone connectivity
- 802.11ax (Wi-Fi 6) access points
- Redundant network design (99.9% uptime SLA)
- Support for 500+ concurrent users per facility
- IPv6 ready

#### 2.2 Security Requirements
- Firewall with IPS/IDS capability
- Network segmentation for public/private traffic
- SIEM integration capability
- Compliance with LGWAN security standards

#### 2.3 Service Requirements
- 24/7 monitoring and support
- 4-hour on-site response for critical issues
- Quarterly security assessments
- Annual disaster recovery drills

### 3. Evaluation Criteria
| Criteria | Weight |
|----------|--------|
| Technical Capability | 30% |
| Price | 25% |
| Past Performance | 20% |
| Support & Maintenance | 15% |
| Innovation | 10% |

### 4. Timeline
- RFP Release: 2025-01-15
- Questions Due: 2025-02-01
- Proposal Due: 2025-02-28
- Award Decision: 2025-03-31
- Project Start: 2025-04-15

### 5. Budget
Estimated budget: 80-100 million JPY
```

**Past Proposals Database (past_proposals.md):**
```markdown
# Past Proposal Archive

## Proposal 1: Yamagata Prefecture Office Network (2024)
- Client: Yamagata Prefectural Government
- Scope: 8 facilities, full network replacement
- Contract Value: 65 million JPY
- Result: WON
- Key Success Factors:
  - Strong local presence emphasized
  - Competitive pricing with phased implementation
  - 24/7 support center in Sendai highlighted
- Lessons: Prefectural clients value local relationships

## Proposal 2: Fukushima Hospital Network (2024)
- Client: Fukushima Medical University
- Scope: Medical-grade network with strict security
- Contract Value: 120 million JPY
- Result: WON
- Key Success Factors:
  - Healthcare compliance expertise
  - Reference from Iwate Medical Center
  - Security certifications highlighted
- Lessons: Medical clients need compliance assurance

## Proposal 3: Morioka City Office Upgrade (2023)
- Client: Morioka City
- Scope: 12 facilities, network modernization
- Contract Value: 55 million JPY
- Result: LOST to competitor
- Loss Factors:
  - Price 15% higher than competitor
  - Less emphasis on innovation
  - Weak response to Wi-Fi 6 requirements
- Lessons: Municipal clients price-sensitive

## Proposal 4: Akita Bank Network Security (2023)
- Client: Regional Bank
- Scope: Security-focused network upgrade
- Contract Value: 90 million JPY
- Result: WON
- Key Success Factors:
  - FISC compliance expertise
  - Zero-trust architecture proposal
  - Incident response capabilities
- Lessons: Financial clients prioritize security
```

**Company Capabilities (capabilities.md):**
```markdown
# Company Capabilities Matrix

## Technical Certifications
- Cisco Gold Partner
- Juniper Elite Partner
- Fortinet Expert Partner
- ISO 27001 certified operations

## Service Capabilities
- 24/7 NOC in Sendai (50+ engineers)
- 4-hour response SLA available
- Regional coverage: All 6 Tohoku prefectures
- Average response time: 2.3 hours

## Reference Clients
- 15 municipal governments
- 8 healthcare institutions
- 12 financial institutions
- 50+ enterprise clients

## Differentiators
- Only Cisco Gold Partner in Tohoku
- Largest regional NOC
- Local decision-making authority
- 30-year regional presence
```

### Knowledge Base Documents

Create the above documents as KB files.

### Workflow Structure
```
[Start: RFP Document Upload]
        |
[Doc Extractor: Parse RFP PDF/Document]
        |
[LLM: Extract Key Requirements]
        |
[Parallel Processing]
    |                              |                              |
[KB: Search Past Proposals]   [KB: Get Capabilities]   [LLM: Identify Evaluation Criteria]
        |
[Variable Aggregator: Combine All Context]
        |
[LLM: Generate Proposal Strategy]
        |
[LLM: Identify Win Themes & Differentiators]
        |
[LLM: Generate Executive Summary Draft]
        |
[Template: Format as Proposal Kickoff Document]
        |
[End: RFP Analysis + Strategy Recommendations]
```

### Expected Output Example
```json
{
  "rfp_summary": {
    "client": "Sendai City",
    "project": "Municipal Network Infrastructure Upgrade",
    "scope": "15 facilities",
    "budget": "80-100M JPY",
    "deadline": "2025-02-28"
  },
  "requirements_analysis": {
    "technical": ["1Gbps backbone", "Wi-Fi 6", "99.9% SLA", "IPv6"],
    "security": ["IPS/IDS", "LGWAN compliance", "SIEM"],
    "service": ["24/7 monitoring", "4-hour response"]
  },
  "relevant_past_proposals": [
    {
      "name": "Yamagata Prefecture Office",
      "relevance": "High - similar municipal scope",
      "result": "Won",
      "applicable_lessons": "Emphasize local presence"
    },
    {
      "name": "Morioka City Office",
      "relevance": "High - same client type",
      "result": "Lost",
      "applicable_lessons": "Address price competitiveness"
    }
  ],
  "strategy_recommendations": {
    "win_themes": [
      "Regional leader with 30-year presence",
      "Only Cisco Gold Partner in Tohoku",
      "Largest NOC for fastest response"
    ],
    "differentiators": [
      "Local decision authority (no Tokyo approval delays)",
      "50+ engineers in regional NOC"
    ],
    "risks_to_address": [
      "Price sensitivity - consider phased approach",
      "Competitor presence - emphasize local support advantage"
    ]
  },
  "decision_support": {
    "bid_recommendation": "Proceed",
    "confidence": "High",
    "rationale": "Strong match with capabilities, relevant win history"
  }
}
```

### Step-by-Step Instructions

**Phase 1: Knowledge Base Setup (20 min)**

**Step 1: Create Past Proposals KB**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `Past Proposals KB`
- Upload: `past_proposals.md`
- Chunk size: 1000 characters (preserve proposal context)

**Step 2: Create Capabilities KB**
- Create another Knowledge Base
- Name: `Company Capabilities KB`
- Upload: `capabilities.md`
- Chunk size: 500 characters

**Step 3: Test KB Retrieval**
- Test queries: `municipal network`, `security requirements`, `government client`

**Phase 2: Build Workflow (50 min)**

**Step 4: Create Workflow**
- Click "Create App" → Select "Workflow"
- Name: `RFP Analyzer`
- Description: `Analyzes RFPs and generates proposal strategy`

**Step 5: Configure Start Node**
- Input variables:
  - `rfp_document` (File) - Label: Upload RFP Document
  - `client_name` (Text) - Label: Client/Organization Name

**Step 6: Add Doc Extractor Node**
- Connect Start → Doc Extractor
- Input: `{{rfp_document}}`
- Output: `rfp_text`

**Step 7: Add Requirements Extraction LLM Node**
- Connect Doc Extractor → LLM
- Prompt:
```
Extract all requirements from this RFP document:

{{rfp_text}}

Return in JSON format:
{
  "project_overview": "",
  "technical_requirements": [],
  "security_requirements": [],
  "service_requirements": [],
  "evaluation_criteria": [{"criteria": "", "weight": ""}],
  "timeline": {"proposal_due": "", "project_start": ""},
  "budget": ""
}
```

**Step 8: Add Parallel Search Branch**
Create two parallel Knowledge Retrieval nodes:
- Branch 1: Search Past Proposals KB for similar projects
- Branch 2: Search Capabilities KB for matching capabilities

**Step 9: Add Variable Aggregator**
- Combine requirements + past proposals + capabilities

**Step 10: Add Strategy Generation LLM Node**
- Prompt:
```
Based on the RFP requirements and our past experience, generate a proposal strategy:

RFP Requirements: {{requirements}}
Similar Past Proposals: {{past_proposals}}
Our Capabilities: {{capabilities}}

Generate strategy in JSON format:
{
  "win_themes": [],
  "differentiators": [],
  "risks_to_address": [],
  "pricing_strategy": "",
  "bid_recommendation": "Proceed/Decline/Need More Info",
  "confidence": "High/Medium/Low",
  "rationale": ""
}
```

**Step 11: Add Output Template Node**
```
=== RFP ANALYSIS REPORT ===
Client: {{client_name}}
Analysis Date: {{current_date}}

KEY REQUIREMENTS:
{{requirements_summary}}

SIMILAR PAST PROPOSALS:
{{past_proposals_summary}}

STRATEGY RECOMMENDATIONS:
Win Themes: {{win_themes}}
Differentiators: {{differentiators}}

BID DECISION: {{bid_recommendation}}
Confidence: {{confidence}}
Rationale: {{rationale}}
===
```

**Phase 3: Testing (20 min)**

**Step 12: Test with Sample RFP**
- Upload the sample RFP document
- Verify requirements are correctly extracted
- Check that similar past proposals are identified
- Validate strategy recommendations

### Deliverables
1. RFP parsing and requirements extraction workflow
2. Past proposal similarity search
3. Strategy generation with win themes
4. Decision support (bid/no-bid) recommendation

---

## Group Exercise 5-4: Customer Complaint Analysis (Team D)

**Business Focus:** Customer Service & Quality Improvement
**Team Size:** 3-4 members
**Complexity:** Hard
**Duration:** 90 minutes
**Patterns Used:** All 5 Patterns (Sequential + Conditional + Parallel + Iterative + RAG-Enhanced)

### Business Scenario
Customer complaints are scattered across multiple channels (phone, email, SNS). There's no systematic way to analyze trends, identify root causes, or track resolution effectiveness. Build a comprehensive complaint analysis system.

### Dataset (Dummy - Inline)

**Customer Complaints:**

| ID | Date | Customer | Channel | Emotion | Category | Description | Status |
|----|------|----------|---------|---------|----------|-------------|--------|
| C001 | 2025-01-15 | Mr. Kondo | Phone | Angry | Service Outage | Internet down for 3 days, no updates from support | Unresolved |
| C002 | 2025-01-16 | Ms. Yamashita | Web | Frustrated | Appointment | Technician missed scheduled visit twice | In Progress |
| C003 | 2025-01-17 | Mr. Tanabe | Phone | Disappointed | Billing | Charged different amount than contract stated | Escalated |
| C004 | 2025-01-18 | Ms. Nakagawa | Email | Upset | Billing | Billing errors for 3 consecutive months | Pending |
| C005 | 2025-01-19 | Mr. Fujita | Phone | Calm | Compensation | Request for compensation after service issues | Review |
| C006 | 2025-01-20 | Ms. Sato | SNS | Angry | Service Quality | Posted publicly about slow speeds | Urgent |
| C007 | 2025-01-21 | Mr. Ito | Phone | Frustrated | Equipment | Router replacement delayed 2 weeks | In Progress |
| C008 | 2025-01-22 | Ms. Watanabe | Web | Neutral | Information | Unclear about new pricing structure | Resolved |
| C009 | 2025-01-23 | Mr. Yamada | Phone | Very Angry | Service Outage | Business customer, lost revenue due to outage | Escalated |
| C010 | 2025-01-24 | Ms. Kobayashi | Email | Disappointed | Staff Conduct | Rude customer service representative | Review |
| C011 | 2025-01-25 | Mr. Suzuki | Phone | Frustrated | Appointment | Waited 4 hours for technician | In Progress |
| C012 | 2025-01-26 | Ms. Tanaka | SNS | Angry | Service Quality | Compared unfavorably to competitor | Urgent |

**Regional Distribution:**

| Prefecture | Total Complaints | Service | Billing | Appointment | Equipment |
|------------|-----------------|---------|---------|-------------|-----------|
| Miyagi | 45 | 18 | 12 | 8 | 7 |
| Iwate | 22 | 8 | 6 | 5 | 3 |
| Akita | 18 | 7 | 5 | 4 | 2 |
| Yamagata | 15 | 6 | 4 | 3 | 2 |
| Fukushima | 28 | 11 | 8 | 5 | 4 |
| Aomori | 12 | 5 | 3 | 2 | 2 |

### Knowledge Base Documents

**complaint_categories.txt:**
```markdown
# Complaint Category Definitions

## Service Outage
- Definition: Complete or significant loss of service
- Priority: Critical (if ongoing), High (if resolved)
- SLA: 4-hour response, 24-hour resolution
- Escalation: To NOC Manager if >6 hours

## Billing Issues
- Definition: Incorrect charges, billing disputes, payment problems
- Priority: Medium (standard), High (repeated errors)
- SLA: 48-hour response
- Escalation: To Finance if >72 hours unresolved

## Appointment Issues
- Definition: Missed appointments, delays, scheduling problems
- Priority: High (same-day issues), Medium (future dates)
- SLA: Same-day rescheduling
- Escalation: To Field Ops Manager if repeated

## Equipment Issues
- Definition: Equipment failures, replacement needs
- Priority: Varies by equipment criticality
- SLA: Next-day replacement for critical equipment
- Escalation: To Supply Chain if inventory issues

## Service Quality
- Definition: Speed issues, reliability concerns
- Priority: Medium (general), High (business customers)
- SLA: Technical assessment within 48 hours
- Escalation: To Technical Manager if systemic

## Staff Conduct
- Definition: Employee behavior complaints
- Priority: High
- SLA: HR review within 24 hours
- Escalation: To HR immediately
```

**resolution_guidelines.txt:**
```markdown
# Complaint Resolution Guidelines

## Emotion-Based Response

### Very Angry / Angry
- Acknowledge immediately
- Apologize sincerely
- Provide concrete action and timeline
- Offer compensation consideration
- Manager follow-up within 24 hours

### Frustrated
- Validate their experience
- Explain what went wrong
- Provide clear resolution path
- Offer expedited service

### Disappointed
- Express understanding
- Provide solution options
- Request feedback on improvement

### Neutral / Calm
- Thank for bringing to attention
- Provide information requested
- Confirm satisfaction

## Compensation Guidelines

### Service Credit
- Outage >24 hours: 1 day credit
- Outage >72 hours: 1 week credit
- Repeated issues (3+): 1 month credit

### Additional Compensation
- Business customer revenue loss: Case-by-case review
- Missed appointment: Service credit + priority scheduling
- Staff conduct issues: Apology + service upgrade offer
```

### Workflow Structure (Uses All 5 Patterns)
```
[Start: Complaint Input]
        |
[LLM: Extract Complaint Details]            <-- Sequential (Pattern 1)
        |
        Code
        |
[Knowledge Retrieval: Get Category Definition]   <-- RAG-Enhanced (Pattern 5)
        |
[LLM: Sentiment Analysis & Severity Score]
        |
[IF/ELSE: Severity Routing]                 <-- Conditional (Pattern 2)
    Critical --> [Immediate Escalation Path]
    High     --> [Priority Response Path]
    Medium   --> [Standard Response Path]
    Low      --> [Queue for Batch Processing]
        |
[Parallel Processing]                        <-- Parallel (Pattern 3)
    |                    |                    |
[KB: Resolution Guide] - [LLM: Root Cause] - [LLM: Similar Complaints]
        |
[Template]
        |
[LLM: Generate Response & Action Plan]
        |
[LLM: Trend Analysis (if batch)]            <-- Iterative (Pattern 4)
        |
[Template 2: Format Complete Response Package]
        |
[Output]
```

### Expected Output Example
```json
=== COMPLAINT ANALYSIS REPORT ===
JSON

ID: AAA
Customer: Mr bean,
Channel: Phone
{"emotion":"Very Angry","category":"Service Outage","key_issues":["Internet down for 3 days","No updates from support"],"urgency_indicators":["3 days downtime"],"business_impact":""}
--- SEVERITY ASSESSMENT ---
JSON

{
  "severity_score": 8,
  "severity_level": "Critical",
  "scoring_breakdown": {
    "emotion": 3,
    "business_customer": 0,
    "channel_risk": 0,
    "category": 2,
    "repeated_issues": 0,
    "sla_breach": 1
  },
  "sla_info": {
    "response_time": "not available",
    "resolution_target": "not available",
    "escalation_rule": "escalation after 2 hours"
  }
}
--- Similar Complaint ---
JSON

{
  "similar_complaints": [
    {
      "id": "C001",
      "similarity": "High",
      "common_factors": ["Service Outage", "Internet down for 3 days", "No updates from support", "Angry"]
    },
    {
      "id": "C009",
      "similarity": "High",
      "common_factors": ["Service Outage", "Very Angry", "Business Impacted"]
    },
    {
      "id": "C006",
      "similarity": "Medium",
      "common_factors": ["Service Outage", "Angry", "Service Quality"]
    },
    {
      "id": "C007",
      "similarity": "Low",
      "common_factors": ["Equipment", "Frustrated", "Delay"]
    }
  ]
}
--- RECOMMENDED RESPONSE & ACTION PLAN ---
JSON

{
  "recommended_response": {
    "immediate_action": "Initiate internal investigation to identify the root cause of the service outage and provide a comprehensive report to the customer within the next 2 hours.",
    "technical_action": "Investigate equipment failure and config error as potential root causes, and take corrective measures to prevent such incidents in the future.",
    "communication": "Provide regular updates to the customer on the status of the issue resolution, including the time of the next callback and the expected resolution time.",
    "compensation": "Offer a credit of £50 towards the customer's next bill as a gesture of goodwill for the inconvenience caused."
  },
  "escalation_needed": true,
  "escalation_to": "Customer Service Manager",
  "follow_up_timeline": "Callback to the customer within the next 4 hours to provide an update on the issue resolution, and a follow-up call from the Customer Service Manager within 24 hours to ensure the issue is fully resolved.",
  "customer_message": "I'm truly sorry you've had to deal with this. This is not the level of service we want to provide. I'm going to personally ensure this is resolved today. Here's what I'm doing right now: initiating an internal investigation to identify the root cause of the service outage. I'll call you back by [time] with an update, and my manager will follow up tomorrow to make sure you're completely satisfied."
}
#--- TREND INSIGHTS ---
Based on the given data, here's the trend analysis in JSON format:

JSON

{
  "trend_insights": {
    "category_trend": "Service Outage complaints are significantly high in the overall data (55% of total complaints), indicating a persistent issue that needs immediate attention. This category is also a key issue in the current complaint, suggesting a potential systemic issue.",
    "regional_pattern": "Miyagi is the top region with 45 complaints, accounting for 32% of the total complaints. The high number of Service Outage complaints in Miyagi (18 out of 45) indicates a regional issue that may be related to infrastructure or network capacity. This region requires more attention and resources to improve service reliability.",
    "severity_trend": "The current complaint is categorized as 'Very Angry' and has a high urgency due to the 3-day service outage. This suggests that complaints are becoming more severe over time. The lack of updates from support is also a contributing factor to the increased severity.",
    "recommendation": "To reduce complaints, we recommend implementing a regionalized service improvement plan with a focus on Miyagi. This plan should include infrastructure upgrades, improved network capacity, and enhanced support mechanisms to ensure timely updates and resolution of service outages."
  }
}
```

### Step-by-Step Instructions

**Phase 1: Knowledge Base Setup (15 min)**

**Step 1: Create Complaint Categories KB**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `complaint_categories.txt`
- Upload: `complaint_categories.txt`
- Delimiter: ```---```
- Chunk size: 1024 characters
- Index Method: High Quality -> pick emmbedding model
- Retrieval Setting: Hybrid Search(Senmantic 0.7, 0.3 Keyword)
- Save & process

**Step 2: Create Resolution Guidelines KB**
- Create another Knowledge Base
- Name: `resolution_guidelines.txt`
- Upload: `resolution_guidelines.md`
- Delimiter: ```---```
- Chunk size: 1500 characters
- Index Method: High Quality -> pick emmbedding model
- Retrieval Setting: Hybrid Search(Senmantic 0.7, 0.3 Keyword)
- Save & process

**Phase 2: Build Workflow (55 min)**

**Step 3: Create Workflow**
- Click "Create App" → Select "Workflow"
- Name: `Complaint Analyzer`
- Description: `Analyzes complaints and recommends resolutions`

**Step 4: Configure Start Node**
- Input variables:
  - `customer_id`: (Text) - Label: Customer ID - Required
  - `customer_name`: (Text) - Label: Customer Name - Required
  - `complaint_channel`: (Select) - Options: (Phone, Email, Web, SNS) - Required
  - `complaint_text`: (Paragraph) - Label: Complaint Description - Required
  - `is_business_customer`: (Select) - Options: (Yes, No) - Required

**Step 5: Add Complaint Extraction LLM Node**
- Connect Start → LLM
- Node Name: Extract Complaint Details
- User Prompt:
```

You are an ultra-strict data extraction API. Your ONLY purpose is to output pure, valid, raw JSON.
Customer: {{#1772114500703.customer_name#}}
Channel: {{#1772114500703.complaint_channel#}}
Is Business Customer: {{#1772114500703.is_business_customer#}}
Complaint: {{#1772114500703.complaint_text#}}
CRITICAL RULES - READ CAREFULLY OR YOU WILL BREAK THE SYSTEM: 1. DO NOT generate `<think>` tags. 2. DO NOT output any reasoning, explanations, or thought processes. 3. DO NOT output Markdown formatting (no ```json or ```). 4. The VERY FIRST character of your response must be `{`. 5. The VERY LAST character of your response must be `}`. Extract the information into this EXACT JSON structure: { "emotion": "<one of: Very Angry, Angry, Frustrated, Disappointed, Neutral, Calm>", "category": "<one of: Service Outage, Billing, Appointment, Equipment, Service Quality, Staff Conduct>", "key_issues": [ "<issue 1>", "<issue 2>" ], "urgency_indicators": [ "<indicator 1>" ], "business_impact": "<description of business impact if any>" }

Analyze the complaint above. Output ONLY this JSON, nothing else:
{"emotion":"<Very Angry|Angry|Frustrated|Disappointed|Neutral|Calm>","category":"<Service Outage|Billing|Appointment|Equipment|Service Quality|Staff Conduct>","key_issues":["<issue1>","<issue2>"],"urgency_indicators":["<indicator1>"],"business_impact":"<description or empty string>"}

/no_think

```
**Step 6: Add CODE Node**
- Input Variables: ```arg1```: ```text(Extract Complaint Details)```
- Python3:
```python
import json

def main(arg1: str) -> dict:
    try:
        data = json.loads(arg1)
        return {
            "category": data.get("category", "Unknown"),
            "emotion": data.get("emotion", "Unknown")
        }
    except:
        return {"category": "Unknown", "emotion": "Unknown"}

```
- Output Variables:
    - ```category```: ```String```
    - ```emotion```: ```String```

**Step 6: Add Knowledge Retrieval Node**
- Name: Get Category Definition
- Query Text: category(CODE)
- Add Knowledge: complaint_categories.txt
    - retrieval setting: Weight score(0.7 Semantic, 0.3 Semantic)

**Step 7: Add Severity Scoring LLM Node**
- Name: Severity Scoring
```
- Calculate severity score (1-10) based on:
  - Emotion level
  - Business customer status
  - Channel (SNS = public exposure risk)
  - Issue category
  ```
- Context: result(Get Category Definition Node)
- User prompt:
```
Based on the complaint analysis and category definition from KB, calculate a severity score from 1-10.

Complaint Analysis:
{{#1772115865013.text#}}
and Emotion level: {{#1772163716713.emotion#}}

Category Definition & SLA from KB:
{{#context#}}


Channel: {{#1772114500703.complaint_channel#}}
Is Business Customer: {{#1772114500703.is_business_customer#}}

Scoring Rules:
- Emotion level: Very Angry/Angry = +3, Frustrated = +2, Disappointed = +1, Neutral/Calm = +0
- Business customer: +2 if Yes
- Channel impact: SNS = +2 (public exposure risk), Phone/Email/Web = +0
- Category: Service Outage = +2, Staff Conduct = +1, Billing = +1, Others = +0
- Repeated issues indicator: +1 if mentioned
- SLA breach: +1 if issue duration exceeds SLA from KB

Respond ONLY with this JSON format:
{
  "severity_score": <number 1-10>,
  "severity_level": "<Critical if 8-10, High if 6-7, Medium if 4-5, Low if 1-3>",
  "scoring_breakdown": {
    "emotion": <points>,
    "business_customer": <points>,
    "channel_risk": <points>,
    "category": <points>,
    "repeated_issues": <points>,
    "sla_breach": <points>
  },
  "sla_info": {
    "response_time": "<from KB>",
    "resolution_target": "<from KB>",
    "escalation_rule": "<from KB>"
  }
}

```

**Step 8: Add IF/ELSE Severity Routing**
- Case IF: `text(Severity Scoring Node)` - `contains` - `Critical` 
- Case ELIF: `text(Severity Scoring Node)` - `contains` - `High` 
- Case ELIF: `text(Severity Scoring Node)` - `contains` - `Mdeium` 
- Case ELSE: 

**Step 9.1: Add Knowledge Node**
- Name: Get Resolution Guidelines
- Query text: emotion(CODE)
- Knowledge: resolution_guidelines.txt
    - Retrieval setting: Weight score(Semantic 0.7, Keyword 0.3)

**Step 9.2: Add LLM Node**
- Name: Root Cause Analysis
- User prompt:
```
Based on the complaint analysis below, identify the most likely root causes.

Complaint Analysis:
{{#1772115865013.text#}}

Severity Assessment:
{{#1772115880124.text#}}

Consider these common root causes per category:
- Service Outage: Equipment failure, fiber cut, power outage, config error, network congestion
- Billing: System errors, plan change not applied, promotion expired, payment processing delay
- Appointment: Dispatch errors, traffic/weather, overbooking, incorrect info, parts unavailable
- Equipment: Age/wear, power surge, firmware bugs, environmental factors, user error
- Service Quality: Network congestion, equipment limitations, building interference, distance
- Staff Conduct: Training gaps, workload stress, miscommunication, policy confusion

Respond ONLY with this JSON:
{
  "root_cause_hypothesis": [
    "<most likely root cause>",
    "<secondary possible cause>"
  ],
  "contributing_factors": ["<factor 1>", "<factor 2>"],
  "prevention_recommendation": "<how to prevent this in the future>"
}

```
**Step 9.3: Add LLM Node**
- Name: Find Similar Complaints
- User prompt
```
Based on the complaint analysis, identify similar past complaints from our database.

Current Complaint:
{{#1772115865013.text#}}

Known complaint database:
- C001: Mr. Kondo, Phone, Angry, Service Outage - Internet down 3 days, no updates
- C002: Ms. Yamashita, Web, Frustrated, Appointment - Technician missed visit twice
- C003: Mr. Tanabe, Phone, Disappointed, Billing - Charged different amount than contract
- C004: Ms. Nakagawa, Email, Upset, Billing - Billing errors 3 consecutive months
- C005: Mr. Fujita, Phone, Calm, Compensation - Request for compensation after service issues
- C006: Ms. Sato, SNS, Angry, Service Quality - Posted about slow speeds
- C007: Mr. Ito, Phone, Frustrated, Equipment - Router replacement delayed 2 weeks
- C008: Ms. Watanabe, Web, Neutral, Information - Unclear about new pricing
- C009: Mr. Yamada, Phone, Very Angry, Service Outage - Business customer, lost revenue
- C010: Ms. Kobayashi, Email, Disappointed, Staff Conduct - Rude representative
- C011: Mr. Suzuki, Phone, Frustrated, Appointment - Waited 4 hours for technician
- C012: Ms. Tanaka, SNS, Angry, Service Quality - Compared unfavorably to competitor

Respond ONLY with this JSON:
{
  "similar_complaints": [
    {
      "id": "<complaint ID>",
      "similarity": "High/Medium/Low",
      "common_factors": ["<factor>"]
    }
  ]
}

```
=> Parallel Processing Branch

**Step 10: Add  Template Node**
- Input Variables
    - `kb_guidelines`: `result(Get Resolution Guidelines)`
    - `root_cause`: `text(Root Cause Analysis)`
    - `similar`: `text(Find Similar Complaints)`



**Step 11: Add LLM Node**
- Name: Generate Response & Action Plan
- User prompt:
```
You are a customer service expert. Generate a comprehensive response and action plan.

Customer: {{#1772114500703.customer_name#}}
Channel: {{#1772114500703.complaint_channel#}}
Is Business Customer: {{#1772114500703.is_business_customer#}}

Complaint Analysis:
{{#1772115865013.text#}}

Severity Assessment:
{{#1772115880124.text#}}

Resolution Guidelines, Root Cause Analysis & Similar Cases (from parallel processing):
{{#1772121267374.output#}}

Based on ALL the information above (especially the Resolution Guidelines from KB), generate a response following these rules:
1. Match tone to customer emotion level per the emotion-based response guidelines retrieved from KB
2. Include specific actions with timelines
3. Recommend appropriate compensation from the compensation table retrieved from KB
4. Include escalation steps if severity >= 8
5. Check escalation triggers from the guidelines

Respond ONLY with this JSON:
{
  "recommended_response": {
    "immediate_action": "<what to do right now>",
    "technical_action": "<technical steps if applicable>",
    "communication": "<how to keep customer updated>",
    "compensation": "<specific compensation from KB guidelines>"
  },
  "escalation_needed": true/false,
  "escalation_to": "<role if needed, or 'N/A'>",
  "follow_up_timeline": "<when to follow up>",
  "customer_message": "<draft message to customer in appropriate tone per emotion-based response from KB>"
}

```

**Step 12: Add LLM Node**
- Name: Trend Analysis
- User prompt:
```
Based on the complaint analysis and our regional complaint data, provide trend insights.

Current Complaint:
{{#1772115865013.text#}}

Regional Complaint Data (January 2025):
| Prefecture | Total | Service | Billing | Appointment | Equipment |
|------------|-------|---------|---------|-------------|-----------|
| Miyagi     | 45    | 18      | 12      | 8           | 7         |
| Iwate      | 22    | 8       | 6       | 5           | 3         |
| Akita      | 18    | 7       | 5       | 4           | 2         |
| Yamagata   | 15    | 6       | 4       | 3           | 2         |
| Fukushima  | 28    | 11      | 8       | 5           | 4         |
| Aomori     | 12    | 5       | 3       | 2           | 2         |

Total all regions: 140 complaints
Top category: Service (55 = 39%)
Top region: Miyagi (45 = 32%)

Provide trend analysis in JSON:
{
  "trend_insights": {
    "category_trend": "<observation about this complaint category vs overall data>",
    "regional_pattern": "<which region has most issues and why>",
    "severity_trend": "<are complaints becoming more/less severe>",
    "recommendation": "<strategic recommendation to reduce complaints>"
  }
}

```

**Step 13: Add Output Template Node**
- Name: Template 2

- Input Variables:
    - `customer_name`:`customer_name(Start Node)`
    - `complaint_channel`:`complaint_channel(Start Node)`
    - `extraction`:`text(Extract Complaint Details)`
    - `severity`:`text(Severity Scoring)`
    - `root_cause`:`text(Root Cause Analysis)`
    - `similar`:`text(Find Similar Complaints)`
    - `response`:`(text(Generate Response & Action Plan))`
    - `trends`:`text(Trend Analysis)`
    - `customer_id`:`customer_id(Start Node)`
- Code:
````
# === COMPLAINT ANALYSIS REPORT ===
```json
ID: {{ customer_id }}
Customer: {{ customer_name }},
Channel: {{ complaint_channel }}
```
# --- SEVERITY ASSESSMENT ---
```json
{{ severity }}
```

# --- Similar Complaint ---
```json
{{ similar }}
```

# --- RECOMMENDED RESPONSE & ACTION PLAN ---
```json
{{ response }}
```
#--- TREND INSIGHTS ---
{{ trends }}

# ===================================
# * Generated by Complaint Analyzer Workflow
```
````

**Step 14: Add Output Node**
- output variable: `output`: `output(Template 2)`

**Phase 3: Testing (20 min)**



**Step 13: Test with Sample Complaints**
- Test C001 (Angry, Service Outage) - expect Critical routing
- Test C008 (Neutral, Information) - expect Low routing
- Test C006 (SNS, Public) - expect urgent handling

### Deliverables
1. Multi-pattern complaint processing workflow
2. Sentiment analysis and severity scoring
3. Resolution recommendation with compensation guidelines
4. Trend analysis and reporting

---

---

## Group Exercise 5-5: Accounting Code Recommendation (Team E)

**Business Focus:** Finance - Transaction Processing
**Team Size:** 3-4 members
**Complexity:** Hard
**Duration:** 90 minutes
**Patterns Used:** Sequential + Conditional + RAG-Enhanced (Pattern 1 + 2 + 5)

### Business Scenario
When drafting decisions, staff don't know the appropriate accounting codes, leading to input errors. The Finance team spends 15 hours monthly correcting management accounting errors. Build a system that recommends correct accounting codes based on transaction descriptions.

### Dataset (Dummy - Inline)

**Transaction Cases:**

| ID | Transaction Description | Department | Amount (JPY) | Correct Code | Category |
|----|------------------------|------------|--------------|--------------|----------|
| T001 | Purchase of office supplies (paper, pens, folders) | General Affairs | 45,000 | 41101 | Consumables |
| T002 | External training seminar registration fee | HR Development | 180,000 | 44201 | Training |
| T003 | Network equipment annual maintenance contract | IT | 500,000 | 43301 | Maintenance |
| T004 | Client entertainment dinner expense | Sales | 35,000 | 44401 | Entertainment |
| T005 | Domestic business trip - Osaka conference | Planning | 85,000 | 44101 | Travel |
| T006 | New laptop purchase for sales team | IT | 250,000 | 42101 | Equipment |
| T007 | Software license renewal (Microsoft 365) | IT | 120,000 | 43201 | Software |
| T008 | Consulting fee for DX assessment | DX Promotion | 800,000 | 44501 | Professional Services |
| T009 | Temporary staff wages - project support | HR | 450,000 | 41301 | Personnel |
| T010 | Vehicle fuel expense - maintenance van | Maintenance | 28,000 | 44301 | Transportation |
| T011 | Office rent - Sendai branch | Facilities | 350,000 | 45101 | Rent |
| T012 | Marketing campaign printing costs | Marketing | 95,000 | 44601 | Advertising |

**Common Error Patterns:**

| Error Pattern | Correct Code | Common Mistake | Frequency |
|--------------|--------------|----------------|-----------|
| Training vs Conference | 44201 (Training) | 44101 (Travel) | 25% |
| Equipment vs Consumables | 42101 (Equipment) | 41101 (Consumables) | 20% |
| Maintenance vs Software | 43301 (Maintenance) | 43201 (Software) | 18% |
| Consulting vs Training | 44501 (Consulting) | 44201 (Training) | 15% |

### Knowledge Base Documents

**accounting_codes.txt:**
```markdown
# Accounting Code Reference

## 41000 - General Expenses
### 41101 - Consumables
- Paper, pens, office supplies
- Items under 10,000 JPY unit price
- Not durable goods

### 41201 - Utilities
- Electricity, gas, water
- Facility-related utilities

### 41301 - Personnel (Temporary)
- Temporary staff wages
- Part-time worker expenses

## 42000 - Asset Purchases
### 42101 - Equipment
- Computers, phones, furniture
- Items over 100,000 JPY
- Expected life > 1 year

### 42201 - Vehicles
- Company cars, vans
- Vehicle-related capital expenses

## 43000 - IT & Software
### 43201 - Software
- License purchases
- SaaS subscriptions
- Software maintenance fees

### 43301 - Equipment Maintenance
- Hardware maintenance contracts
- Network equipment servicing
- Annual support agreements

## 44000 - Operational Expenses
### 44101 - Travel
- Transportation costs
- Accommodation
- Per diem expenses

### 44201 - Training & Development
- Seminar registration fees
- Training course fees
- Certification costs

### 44301 - Transportation (Daily)
- Fuel expenses
- Commuting support
- Local travel

### 44401 - Entertainment
- Client meals
- Business entertainment
- Gifts (business purpose)

### 44501 - Professional Services
- Consulting fees
- Legal fees
- Audit expenses

### 44601 - Advertising & Marketing
- Advertising costs
- Promotional materials
- Marketing campaigns

## 45000 - Facility Expenses
### 45101 - Rent
- Office rent
- Facility lease payments

### 45201 - Facility Maintenance
- Building repairs
- Facility upkeep
```



### Workflow Structure
```
[Start: Transaction Description Input]
        |
[LLM: Extract Transaction Details]

        |
[Knowledge Retrieval: Search Accounting Codes]
        |
[LLM: Apply Selection Rules]
        |
[IF/ELSE: Confidence Check]
    High Confidence --> [Output: Single Code Recommendation]
    Medium         --> [Output: Top 2-3 Options with Explanation]
    Low            --> [Output: Clarification Questions Needed]
        |
[LLM: Generate Explanation & Usage Examples]
        |
[Template: Format Code Recommendation]
        |
[End: Code Recommendation + Rationale]
```

### Expected Output Example
=== ACCOUNTING CODE RECOMMENDATION ===
✅ RECOMMENDED CODE: 41101 - Consumables
CONFIDENCE: HIGH

RATIONALE:
The transaction is a one‑time purchase of office supplies (paper, pens, folders) with a total amount of 45,000 JPY, unit price range 10k‑100k JPY, and expected useful life of less than one year. According to the Equipment vs Consumables rule, items under 100,000 JPY with a life of <1 year fall under the Consumables category (41101).

⚠️ WARNINGS:
Do not confuse this with 44xxx general operational codes (e.g., 44101 for travel). Consumables are specifically coded as 41101.

CHECKLIST:

 Unit price verified
 Primary purpose confirmed
 Check if expense should be split



### Step-by-Step Instructions

**Phase 1: Knowledge Base Setup (20 min)**

**Step 1: Create Accounting Codes KB**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `accounting_code.txt`
- Upload: `accounting_codes.txt`
- Delimiter: `---`
- Chunk size: 1024 characters (one code section per chunk)
- Index Method: High Quality -> pick model *embedding
- Retrieval Setting: Hybrid Search -> Weighted Score(Semantic 0.7, 0.3 Keyword)
- Save & Process

**Phase 2: Build Workflow (50 min)**
**Step 4: Create Workflow**
- Click "Create App" → Select "Workflow"
- Name: `Accounting Code Recommender`
- Description: `Recommends correct accounting codes for transactions`

**Step 5: Configure Start Node**
- Input variables:
  - `input_text`: Field Type: Paragraph,  Variable Name: `input_text` | `required`

**Step 6: Add Transaction Analysis LLM Node**
- Connect Start → LLM

- System Prompt:
```
	"You are a transaction analysis assistant for a Japanese telecom company.
	Extract structured information from free-text transaction descriptions.
	Always respond in valid JSON only, no other text."
```
- User Prompt:
```
	Analyze this transaction and extract key information:

	{{#1772110367685.input_text#}}

	Respond ONLY in JSON:
	{
  	"description": "short summary of the transaction",
  	"department": "department name if mentioned, or unknown",
  	"amount": numeric amount in JPY if mentioned or 0,
  	"expense_type": "consumables/equipment/software/maintenance/travel/training/transportation/entertainment/professional_services/personnel/rent/advertising",
  	"is_capital_purchase": true or false,
  	"primary_category": "general_expense/asset_purchase/it_software/operational/facility",
  	"keywords": ["keyword1", "keyword2"],
  	"unit_price_range": "under_10k/10k_100k/over_100k",
  	"recurring": true or false
}

	Rules:
	- is_capital_purchase = true ONLY if amount > 100,000 AND item has useful life > 1 year
	- If amount not mentioned, set to 0 and unit_price_range to "unknown"


```
**Step 7: Add Knowledge Retrieval - Codes KB**
- Add `accounting_code.txt`
- Query Text: text
- Add Knowledge: accounting_code.txt
- Output: `matching_codes`

**Step 8: Add LLM 2**
- System prompt: 
```
# Accounting Code Selection Rules

## Decision Tree Approach

### Step 1: Is it a capital purchase?

    Is the item:
    - Over 100,000 JPY AND
    - Has useful life > 1 year?
            |
        YES → Use 42xxx codes (Asset Purchases)
            |
        NO  → Continue to Step 2

### Step 2: What is the primary purpose?

    Primary Purpose:
        |
        ├── IT/Software related → Use 43xxx codes
        |       ├── Software purchase/subscription → 43201
        |       └── Hardware maintenance → 43301
        |
        ├── Personnel related → Use 41301 (Temporary Personnel)
        |
        ├── Facility related → Use 45xxx codes
        |       ├── Rent/lease → 45101
        |       └── Building maintenance → 45201
        |
        └── General operational → Use 44xxx codes
                ├── Travel → 44101
                ├── Training → 44201
                ├── Daily transport/communication → 44301
                ├── Entertainment → 44401
                ├── Professional services → 44501
                └── Marketing → 44601

### Step 3: Specific Category Selection (Common Confusion Areas)
## Travel vs Training (44101 vs 44201)

**Key Question:** What is the PRIMARY purpose of the expense?

| Scenario | Code | Rationale |
|----------|------|-----------|
| Attending seminar as PARTICIPANT | 44201 | Learning is primary purpose |
| Traveling to GIVE presentation | 44101 | Travel is primary purpose |
| Conference with learning + networking | 44201 | Learning is primary |
| Client visit for project work | 44101 | Travel for business activity |

**Special Case - Conference with Both:**
- Registration fee → 44201 (Training)
- Travel costs → 44101 (Travel)
- Split the expense by purpose
==HET-1-CHUNK==
## Equipment vs Consumables (42101 vs 41101)

**Key Questions:**
1. What is the unit price?
2. What is the expected useful life?

| Unit Price | Useful Life | Code | Category |
|------------|-------------|------|----------|
| < 10,000 JPY | < 1 year | 41101 | Consumables |
| 10,000 - 100,000 JPY | < 1 year | 41101 | Consumables |
| 10,000 - 100,000 JPY | > 1 year | 42101 | Equipment |
| > 100,000 JPY | Any | 42101 | Equipment |

**Examples:**
- Keyboard (8,000 JPY) → 41101 (Consumables)
- Monitor (35,000 JPY, 3-year life) → 42101 (Equipment)
- Laptop (150,000 JPY) → 42101 (Equipment)
- Printer ink (5,000 JPY) → 41101 (Consumables)
==HET-1-CHUNK==
## Maintenance vs Software (43301 vs 43201)

**Key Question:** Is it for HARDWARE or SOFTWARE?

| Item | Code | Rationale |
|------|------|-----------|
| Server hardware maintenance | 43301 | Physical equipment |
| Software maintenance fee | 43201 | Software related |
| SaaS subscription | 43201 | Software service |
| Network equipment support | 43301 | Physical equipment |
| Cloud hosting (AWS, Azure) | 43201 | Software/service |

**Combined Maintenance Contracts:**
- If contract covers BOTH hardware + software:
  - Split if amounts are clear
  - Use 43301 if hardware is primary
  - Use 43201 if software is primary

## Consulting vs Training (44501 vs 44201)

**Key Question:** Is someone TEACHING us or DOING work for us?

| Scenario | Code | Rationale |
|----------|------|-----------|
| External training instructor | 44201 | Teaching our employees |
| Consultant doing assessment | 44501 | Doing work for us |
| Workshop facilitation | 44201 | Learning activity |
| System implementation consulting | 44501 | Professional service |
| Certification prep course | 44201 | Training/education |

## Common Clarification Questions

When uncertain, ask these questions:

1. **What is the unit price of the item?**
   - Determines Equipment vs Consumables boundary

2. **What is the expected useful life?**
   - >1 year suggests Equipment or Asset

3. **What is the primary purpose of the expense?**
   - Learning → Training
   - Business activity → Travel or relevant operational code
   - Getting expert help → Professional Services

4. **Is this a recurring or one-time expense?**
   - Recurring often suggests subscriptions or maintenance
   - One-time might be asset purchase or project expense

5. **Which department will use this?**
   - Helps determine appropriate budget category

6. **Is this for internal use or client-facing?**
   - Client-facing often → Entertainment or Marketing
## Error Prevention Checklist

Before finalizing a code selection:

- [ ] Verified unit price against thresholds
- [ ] Confirmed useful life expectation
- [ ] Identified primary purpose correctly
- [ ] Checked for similar past transactions
- [ ] Considered if expense should be split
- [ ] Verified against department budget categories
## Common Mistakes and Corrections

| Common Mistake | Correct Approach |
|----------------|------------------|
| All travel as 44101 | Split training registration (44201) from travel costs (44101) |
| All IT as 43201 | Hardware maintenance is 43301, software is 43201 |
| Expensive supplies as 41101 | Check if >100,000 JPY → 42101 |
| Consulting as 44201 | Training is 44201, consulting work is 44501 |
| Combined contracts single code | Split by primary purpose if amounts are known |
## When to Escalate

Contact Finance Department when:

- Transaction > 1,000,000 JPY
- Uncertain after using decision tree
- New type of transaction not in guidelines
- Cross-departmental expense allocation needed
- International transaction with tax implications
- Retroactive correction needed

**Finance Contact:**
- Email: finance-inquiry@example.internal
- Phone: ext. 3333
- Response time: Within 2 business days
```
- Use expense_type and category for search
- Output: `applicable_rules`

**Step 9: Add Code**
- input variable: recommendation: text(LLM 2)
- Python3:
```
import json

def main(recommendation: str) -> dict:
    text = recommendation.strip()
    if text.startswith("```"):
        lines = text.split("\n")
        lines = [l for l in lines if not l.strip().startswith("```")]
        text = "\n".join(lines)
    try:
        data = json.loads(text)
        return {
            "result": text,
            "confidence": data.get("confidence", "low")
        }
    except:
        return {"result": text, "confidence": "low"}

```
- Ouput variable: confidence(String), result(String)

**Step 10: Add IF/ELSE Confidence Routing**
- Add condition:
    - if: confidence is `high`
    - elif confidence is `medium`
- High confidence → Single recommendation
- Medium/Low confidence → Multiple options with decision support

**Step 11.1: Add LLM HIGH from Case IF**
- User prompt:
```
Review this recommendation and write a final report.

Recommendation: {{#1772110380220.text#}}

Original input: {{#1772110367685.input_text#}}

Write output in this format:
=== ACCOUNTING CODE RECOMMENDATION ===
✅ RECOMMENDED CODE: [code] - [code_name]
CONFIDENCE: HIGH

RATIONALE:
[rationale from the JSON]

⚠️ WARNINGS:
[common_mistake_warning, or "None"]

CHECKLIST:
- [ ] Unit price verified
- [ ] Primary purpose confirmed
- [ ] Check if expense should be split
===

```
**Step 11.2: Add LLM MEDIUM from Case ELIF**
- User prompt:
```
Medium confidence. Provide options.

Recommendation:{{#1772110380220.text#}}

Original input:{{#1772110367685.input_text#}}
Write output:
=== ACCOUNTING CODE RECOMMENDATION ===
⚠️ CONFIDENCE: MEDIUM — Review suggested

OPTION 1 (Most Likely): [code] - [name]
→ [reason]

OPTION 2: [code] - [name]
→ [reason]

CLARIFICATION NEEDED:
[question to resolve ambiguity]
===

```
**Step 11.3: Add LLM LOW from Case ELSE**
- User prompt:
```
Low confidence. Manual review required.

Recommendation: {{#1772110380220.text#}}

Original input:
{{#1772110367685.input_text#}}

Write output:
=== ACCOUNTING CODE RECOMMENDATION ===
🔴 LOW CONFIDENCE — MANUAL REVIEW REQUIRED

POSSIBLE CODES:
1. [code] - [name]: [reason]
2. [code] - [name]: [reason]

QUESTIONS TO ASK:
1. What is the unit price?
2. What is the primary purpose?

📞 Contact Finance: ext. 3333
===

```
**Step 12: Add Output Node**
- Add output variable: 
    - `text_1`: `text(case low)`
    - `text_2`: `text(case high)`
    - `text_3`: `text(case medium)`
    
**Phase 3: Testing (20 min)**

**Step 13: Test with Sample Transactions**
- T001 (Office supplies, 45,000 JPY) - expect 41101
- T002 (Training seminar, 180,000 JPY) - expect 44201
- T003 (Maintenance contract, 500,000 JPY) - expect 43301
- Test edge cases to verify confidence routing

### Deliverables
1. Transaction analysis and code recommendation workflow
2. Knowledge Base with accounting codes and selection rules
3. Confidence-based output (single recommendation vs. options)
4. Common mistake warnings

---


## Group Exercise 5-6: Publicity Analysis System (Team F)

**Business Focus:** Corporate Communications & PR
**Team Size:** 3-4 members
**Complexity:** Hard
**Duration:** 90 minutes
**Patterns Used:** Sequential + Parallel + RAG-Enhanced + Code Node (Pattern 1 + 3 + 5)

### Business Scenario
The PR team manually analyzes publicity by area characteristics and social impact, which is time-consuming and inconsistent. Build a system that analyzes press releases, media coverage, and social sentiment to evaluate publicity effectiveness by region.

### Dataset (Dummy - Inline)

**Press Releases:**

| ID | Date | Title | Region | Topic | Channel | Reach |
|----|------|-------|--------|-------|---------|-------|
| PR001 | 2025-01-10 | New Fiber Service Launch in Rural Iwate | Iwate | Service | Press Release | 50,000 |
| PR002 | 2025-01-15 | Community Wi-Fi Project Completed in Aoba Ward | Miyagi | CSR | Press + Local TV | 150,000 |
| PR003 | 2025-01-20 | Partnership with Akita University on 5G Research | Akita | Technology | Press + Web | 80,000 |
| PR004 | 2025-01-25 | Support for Earthquake Recovery Communications | Fukushima | CSR | National Media | 500,000 |
| PR005 | 2025-02-01 | Smart Agriculture IoT Pilot in Yamagata | Yamagata | Innovation | Press + SNS | 120,000 |
| PR006 | 2025-02-05 | New Customer Service Center Opens in Aomori | Aomori | Service | Local Media | 45,000 |

**Media Coverage:**

| ID | PR_ID | Media Outlet | Type | Sentiment | Key Message Retained | Date |
|----|-------|--------------|------|-----------|---------------------|------|
| MC001 | PR001 | Iwate Nippo | Newspaper | Positive | Yes | 2025-01-11 |
| MC002 | PR002 | Kahoku Shimpo | Newspaper | Positive | Yes | 2025-01-16 |
| MC003 | PR002 | TBC TV | TV | Positive | Partial | 2025-01-16 |
| MC004 | PR003 | NHK Akita | TV | Neutral | Yes | 2025-01-21 |
| MC005 | PR004 | Asahi Shimbun | National | Positive | Yes | 2025-01-26 |
| MC006 | PR004 | NHK National | TV | Positive | Yes | 2025-01-26 |
| MC007 | PR005 | Yamagata Shimbun | Newspaper | Positive | Partial | 2025-02-02 |
| MC008 | PR006 | Daily Tohoku | Newspaper | Neutral | Yes | 2025-02-06 |

**Social Media Mentions:**

| ID | PR_ID | Platform | Sentiment | Engagement | Sample Comment |
|----|-------|----------|-----------|------------|----------------|
| SM001 | PR002 | Twitter | Positive | 245 likes, 52 RTs | "Great initiative for the community!" |
| SM002 | PR002 | Facebook | Positive | 180 likes, 30 shares | "Finally! Our community center has Wi-Fi" |
| SM003 | PR004 | Twitter | Very Positive | 1,200 likes, 450 RTs | "Thank you for supporting recovery efforts" |
| SM004 | PR005 | Twitter | Positive | 89 likes, 15 RTs | "Interesting tech application for farming" |
| SM005 | PR005 | Instagram | Neutral | 45 likes | "Saw this at the agricultural fair" |

**Regional Statistics:**

| Prefecture | Population | Media Reach Rate | SNS Penetration | PR Effectiveness Score (YTD) |
|------------|------------|-----------------|-----------------|------------------------------|
| Miyagi | 2,300,000 | 68% | 68% | 78 |
| Fukushima | 1,800,000 | 62% | 62% | 85 |
| Iwate | 1,200,000 | 55% | 58% | 65 |
| Yamagata | 1,100,000 | 58% | 60% | 72 |
| Akita | 950,000 | 52% | 55% | 68 |
| Aomori | 1,200,000 | 54% | 57% | 60 |

### Knowledge Base Documents

**publicity_metrics.md:**
```markdown
# Publicity Effectiveness Metrics

## Reach Metrics
### Media Pickup Rate
- Definition: % of press releases covered by media
- Calculation: Media mentions / Press releases × 100
- Target: > 60%
- Excellent: > 80%

### Reach Volume
- Definition: Estimated audience reached
- Sources: Media circulation + TV viewership + web visits
- Weighted by media type:
  - National media: 1.5x multiplier
  - Regional TV: 1.2x multiplier
  - Regional newspaper: 1.0x multiplier
  - Web only: 0.8x multiplier

## Quality Metrics
### Sentiment Score
- Very Positive: 5
- Positive: 4
- Neutral: 3
- Negative: 2
- Very Negative: 1
- Target Average: > 3.5

### Message Retention Rate
- Definition: % of coverage retaining key messages
- Full retention: 100%
- Partial retention: 50%
- No key message: 0%
- Target: > 70%

## Engagement Metrics
### Social Media Engagement Rate
- Definition: (Likes + Comments + Shares) / Reach × 100
- Good: > 2%
- Excellent: > 5%

### Amplification Rate
- Definition: Shares / Total Engagement × 100
- Indicates message spread beyond initial reach
```

**regional_strategy.md:**
```markdown
# Regional PR Strategy Guidelines

## Miyagi (Sendai)
- Key media: Kahoku Shimpo, TBC TV, OX TV
- Focus topics: Urban innovation, business solutions
- Best channels: Press + TV combo
- Timing: Weekday mornings

## Fukushima
- Key media: Fukushima Minpo, Fukushima Minyu
- Focus topics: Recovery, community support, CSR
- Best channels: Local media + national pickup potential
- Note: Recovery-related news gets high engagement

## Iwate
- Key media: Iwate Nippo, IBC TV
- Focus topics: Rural connectivity, regional development
- Best channels: Local newspaper first
- Note: Longer lead time needed for rural coverage

## Akita
- Key media: Akita Sakigake, AKT TV
- Focus topics: Agriculture tech, university partnerships
- Best channels: Press release + web
- Note: Strong interest in innovation topics

## Yamagata
- Key media: Yamagata Shimbun, YBC TV
- Focus topics: Agriculture, tourism, local business
- Best channels: Local newspaper + SNS
- Note: High SNS engagement in rural areas

## Aomori
- Key media: Daily Tohoku, RAB TV
- Focus topics: Fisheries, tourism, regional service
- Best channels: Local media
- Note: Seasonal timing important (Nebuta festival period)
```

### Workflow Structure
```
[Start: Press Release + Coverage Data Input]
        |
[Doc Extractor: Parse All Inputs]
        |
[Parallel Processing]                        <-- Parallel (Pattern 3)
    |                    |                    |
[LLM: Analyze Coverage] [LLM: Sentiment]   [Code: Calculate Metrics]
        |
[Variable Aggregator]
        |
[Knowledge Retrieval: Get Regional Strategy]  <-- RAG-Enhanced (Pattern 5)
        |
[LLM: Generate Regional Comparison]
        |
[LLM: Generate Effectiveness Report]
        |
[Code Node: Create Statistics]               <-- Code for calculations
        |
[Template: Format Analysis Report]
        |
[End: Publicity Analysis Report + Recommendations]
```

### Code Node - Metrics Calculation
```python
def main(coverage_data: str, social_data: str) -> dict:
    import json

    coverage = json.loads(coverage_data)
    social = json.loads(social_data)

    # Calculate pickup rate
    total_pr = len(set([c['pr_id'] for c in coverage]))
    pickup_rate = len(coverage) / total_pr * 100 if total_pr > 0 else 0

    # Calculate sentiment score
    sentiment_map = {'Very Positive': 5, 'Positive': 4, 'Neutral': 3, 'Negative': 2, 'Very Negative': 1}
    sentiment_scores = [sentiment_map.get(c['sentiment'], 3) for c in coverage]
    avg_sentiment = sum(sentiment_scores) / len(sentiment_scores) if sentiment_scores else 0

    # Calculate message retention
    retention_map = {'Yes': 100, 'Partial': 50, 'No': 0}
    retention_scores = [retention_map.get(c.get('key_message_retained', 'No'), 0) for c in coverage]
    avg_retention = sum(retention_scores) / len(retention_scores) if retention_scores else 0

    # Social engagement
    total_engagement = sum([s.get('engagement', 0) for s in social])

    return {
        "pickup_rate": round(pickup_rate, 1),
        "avg_sentiment_score": round(avg_sentiment, 2),
        "message_retention_rate": round(avg_retention, 1),
        "total_social_engagement": total_engagement,
        "coverage_count": len(coverage)
    }
```

### Expected Output Example
```json
{
  "analysis_period": "2025-01 to 2025-02",
  "overall_metrics": {
    "total_press_releases": 6,
    "total_media_coverage": 8,
    "pickup_rate": "133%",
    "average_sentiment": 4.1,
    "message_retention": "75%"
  },
  "regional_breakdown": [
    {
      "region": "Fukushima",
      "pr_count": 1,
      "coverage_count": 2,
      "reach": 500000,
      "sentiment": "Very Positive",
      "effectiveness_score": 92
    },
    {
      "region": "Miyagi",
      "pr_count": 1,
      "coverage_count": 2,
      "reach": 150000,
      "sentiment": "Positive",
      "effectiveness_score": 85
    }
  ],
  "topic_analysis": {
    "best_performing": "CSR (Disaster Recovery)",
    "highest_engagement": "Community Wi-Fi Project",
    "needs_improvement": "Service announcements"
  },
  "recommendations": [
    "Increase CSR-related publicity - highest engagement rates",
    "Leverage Fukushima recovery stories for national pickup",
    "Consider SNS amplification for Yamagata agricultural stories",
    "Aomori needs more innovative story angles"
  ],
  "next_period_strategy": {
    "focus_regions": ["Fukushima", "Miyagi"],
    "focus_topics": ["CSR", "Innovation"],
    "suggested_timing": "Weekday mornings for Miyagi, flexible for rural"
  }
}
```

### Step-by-Step Instructions

**Phase 1: Knowledge Base Setup (15 min)**

**Step 1: Create Publicity Metrics KB**
- Click "Knowledge" → "Create Knowledge Base"
- Name: `Publicity Metrics KB`
- Upload: `publicity_metrics.md`
- Chunk size: 400 characters

**Step 2: Create Regional Strategy KB**
- Create another Knowledge Base
- Name: `Regional Strategy KB`
- Upload: `regional_strategy.md`
- Chunk size: 500 characters

**Phase 2: Build Workflow (55 min)**

**Step 3: Create Workflow**
- Click "Create App" → Select "Workflow"
- Name: `Publicity Analyzer`
- Description: `Analyzes publicity effectiveness by region`

**Step 4: Configure Start Node**
- Input variables:
  - `press_releases` (File) - Label: Press Release Data (CSV)
  - `media_coverage` (File) - Label: Media Coverage Data (CSV)
  - `social_mentions` (File) - Label: Social Media Data (CSV)
  - `analysis_period` (Text) - Label: Analysis Period

**Step 5: Add Doc Extractor Node**
- Connect Start → Doc Extractor
- Input: `{{press_releases}}, {{media_coverage}}, {{social_mentions}}`
- Extracts CSV data into text

**Step 6: Add Parallel Processing Branch**
Create three parallel LLM nodes:

**Branch 1: Press Release Analysis**
```
Analyze the press releases:
{{press_release_data}}

Return JSON with:
- total_count
- by_region breakdown
- by_topic breakdown
- total_reach
```

**Branch 2: Media Coverage Analysis**
```
Analyze the media coverage:
{{media_coverage_data}}

Return JSON with:
- pickup_rate
- sentiment_breakdown
- message_retention_stats
- by_media_type
```

**Branch 3: Social Sentiment Analysis**
```
Analyze social media mentions:
{{social_data}}

Return JSON with:
- total_engagement
- sentiment_distribution
- top_performing_posts
- engagement_by_platform
```

**Step 7: Add Variable Aggregator**
- Combine all three parallel outputs

**Step 8: Add Knowledge Retrieval Node**
- Search `Regional Strategy KB`
- Query with region names from analysis
- Output: `regional_guidelines`

**Step 9: Add Regional Comparison LLM Node**
- Compare actual performance vs. strategy guidelines
- Identify gaps and opportunities

**Step 10: Add Code Node for Metrics Calculation**
- Use the Python code from the Code Node section
- Calculate: pickup_rate, sentiment_score, retention_rate, engagement

**Step 11: Add Recommendations LLM Node**
- Generate actionable recommendations based on:
  - Regional performance gaps
  - Topic effectiveness
  - Timing patterns
  - Engagement trends

**Step 12: Add Output Template Node**
```
=== PUBLICITY ANALYSIS REPORT ===
Period: {{analysis_period}}
Generated: {{current_date}}

OVERALL METRICS:
Total Press Releases: {{total_pr}}
Total Media Coverage: {{total_coverage}}
Pickup Rate: {{pickup_rate}}%
Average Sentiment: {{avg_sentiment}}/5
Message Retention: {{retention_rate}}%

REGIONAL BREAKDOWN:
{{regional_breakdown}}

TOP PERFORMERS:
{{top_performers}}

AREAS NEEDING IMPROVEMENT:
{{improvement_areas}}

RECOMMENDATIONS:
{{recommendations}}

NEXT PERIOD STRATEGY:
{{next_period_strategy}}
===
```

**Phase 3: Testing (20 min)**

**Step 13: Test with Sample Data**
- Upload the sample CSV data files
- Verify metrics calculations are correct
- Check regional comparisons make sense
- Validate recommendations are actionable

**Step 14: Test Edge Cases**
- Test with limited data (single region)
- Test with no social mentions
- Verify error handling

### Deliverables
1. Multi-source publicity data analysis workflow
2. Regional comparison and effectiveness scoring
3. Code-based metrics calculation
4. Strategy recommendations by region

---

# Capstone Project (End of Session 5)
## Comprehensive Exercise

**Duration:** 60 minutes
**Team Size:** 3-4 members (same as Session 5 group)

### Project Brief
Each team completes and presents their Session 5 group exercise as a production-ready solution. The Capstone focuses on:
- Completing any unfinished components
- Optimizing retrieval and workflow performance
- Preparing a brief presentation

### Deliverables
1. **Working Application** (published and demonstrated)
2. **Knowledge Base** with relevant documents
3. **Workflow(s)** implementing all required patterns
4. **5-Minute Presentation:**
   - Business problem addressed (from your team's initiative)
   - Solution architecture
   - Live demo
   - Key learnings and challenges overcome

### Evaluation Criteria

| Criteria | Weight | Description |
|---|---|---|
| Functionality | 25% | Does the application work correctly? |
| Business Relevance | 20% | Does it address the real team problem? |
| RAG Quality | 15% | Is retrieval accurate and well-configured? |
| Workflow Design | 15% | Follows design principles and patterns? |
| User Experience | 10% | Is it intuitive and well-designed? |
| Technical Implementation | 10% | Proper use of Dify components? |
| Presentation | 5% | Clear explanation and demo? |

---

# Appendix

## A. Dataset Summary

All exercises use **custom dummy data files** - no external downloads required. Files are located in `data/exercise/` directory.

| Session | Exercise | Data Files | Records |
|---------|----------|------------|---------|
| 2-1 | Internal FAQ | faq_data.csv, test_questions.txt, system_prompt_template.md | 10 FAQ + 12 test Q |
| 2-2 | Document Generator | maintenance_records.csv, transaction_records.csv, document_types.md | 10 + 10 records |
| 2-3 | Research Agent | sales_prospect_cards.md, regional_statistics.csv, agent_system_prompt.md | 5 prospects + 6 regions |
| 2-4 | Inquiry Classification | customer_inquiries.csv, complaint_records.csv, classification_categories.md, response_templates.md | 12 + 5 records |
| 2-5 | Doc Extractor | sample_invoice.md, sample_contract.md, sample_meeting_minutes.md, sample_equipment_list.csv, doc_extractor_guide.md, extraction_prompts.md | 4 sample docs + 2 guides |
| 3-1 | Service KB | service_plans.md, billing_faq.md, technical_troubleshooting.md | 3 docs |
| 3-2 | Chunking Comparison | (same as 3-1) | 1 doc |
| 3-3 | Multi-KB | (same as 3-1) | 3 docs |
| 3-4 | QA Workflow | qa_test_queries.csv | 12 queries |
| 4-1 | Document Analysis Pipeline | project_evaluation_guidelines.md, sample_proposal_good.md, sample_proposal_poor.md, test_scenarios.md | 4 files |
| 4-2 | Parallel Processing | evaluation_criteria.md, sample_submissions.csv, scoring_weights.md | 3 files |
| 5-1 | DX Entry (Team A) | dx_entry_guidelines.md | 1 doc |
| 5-2 | Handover (Team B) | handover_template.md | 1 doc |
| 5-3 | RFP Analysis (Team C) | rfp_sample.md, past_proposals.md, capabilities.md | 3 docs |
| 5-4 | Complaints (Team D) | complaint_categories.md, resolution_guidelines.md | 2 docs |
| 5-5 | Accounting (Team E) | accounting_codes.md, code_selection_rules.md | 2 docs |
| 5-6 | Publicity (Team F) | publicity_metrics.md, regional_strategy.md | 2 docs |

**Total: 41 data files across all sessions**

## B. Pattern-Exercise Mapping

| Pattern | Session 2 | Session 3 | Session 4 | Session 5 |
|---|---|---|---|---|
| Sequential (P1) | 2-1, 2-2, 2-5 | 3-1, 3-4 | 4-1, 4-2 | All |
| Conditional (P2) | 2-4, 2-5 | 3-3 | 4-2 | 5-1, 5-4, 5-5 |
| Parallel (P3) | - | 3-2 | 4-2 | 5-1, 5-3, 5-4, 5-6 |
| Iterative (P4) | 2-3 | - | - | 5-4 |
| RAG-Enhanced (P5) | - | All | 4-1 | All |
| Doc Extractor | 2-5 | - | 4-1 | 5-2, 5-3, 5-6 |
| Variable Aggregator | - | - | 4-2 | 5-1, 5-3, 5-4, 5-6 |
| Code Node | - | 3-4 | 4-2 | 5-6 |

## C. Session 5 Team Assignments

**Note:** Exercises in Sessions 2, 3, and 4 are completed by ALL trainees individually. Only Session 5 exercises are team-specific.

| Team | Initiative | Session 5 Exercise |
|------|------------|-------------------|
| A | DX Entry Support | 5-1 |
| B | Project Handover | 5-2 |
| C | RFP Analysis | 5-3 |
| D | Complaint Analysis | 5-4 |
| E | Accounting Code | 5-5 |
| F | Publicity Analysis | 5-6 |

## D. File Organization
```
data/exercise/
├── Session_2/
│   ├── exercise_2-1/
│   │   ├── faq_data.csv
│   │   ├── test_questions.txt
│   │   └── system_prompt_template.md
│   ├── exercise_2-2/
│   │   ├── maintenance_records.csv
│   │   ├── transaction_records.csv
│   │   └── document_types.md
│   ├── exercise_2-3/
│   │   ├── sales_prospect_cards.md
│   │   ├── regional_statistics.csv
│   │   └── agent_system_prompt.md
│   ├── exercise_2-4/
│   │   ├── customer_inquiries.csv
│   │   ├── complaint_records.csv
│   │   ├── classification_categories.md
│   │   └── response_templates.md
│   └── exercise_2-5/
│       ├── sample_invoice.md
│       ├── sample_contract.md
│       ├── sample_meeting_minutes.md
│       ├── sample_equipment_list.csv
│       ├── doc_extractor_guide.md
│       └── extraction_prompts.md
├── Session_3/
│   ├── knowledge_base/
│   │   ├── service_plans.md
│   │   ├── billing_faq.md
│   │   └── technical_troubleshooting.md
│   └── test_queries/
│       └── qa_test_queries.csv
├── Session_4/
│   ├── exercise_4-1/
│   │   ├── project_evaluation_guidelines.md
│   │   ├── sample_proposal_good.md
│   │   ├── sample_proposal_poor.md
│   │   └── test_scenarios.md
│   └── exercise_4-2/
│       ├── evaluation_criteria.md
│       ├── sample_submissions.csv
│       └── scoring_weights.md
└── Session_5/
    ├── team_a_dx_entry/
    │   └── dx_entry_guidelines.md
    ├── team_b_handover/
    │   └── handover_template.md
    ├── team_c_rfp/
    │   ├── rfp_sample.md
    │   ├── past_proposals.md
    │   └── capabilities.md
    ├── team_d_complaints/
    │   ├── complaint_categories.md
    │   └── resolution_guidelines.md
    ├── team_e_accounting/
    │   ├── accounting_codes.md
    │   └── code_selection_rules.md
    └── team_f_publicity/
        ├── publicity_metrics.md
        └── regional_strategy.md
```

---

*Exercise Document Version: 3.3*
*Created for: Tohoku Region Dify Kanade Training*
*Platform: Dify Kanade*
*Last Updated: February 2026*
