# azure-q10-report-extraction
Extracting financial metrics from Q10 report using RAG on Microsoft Azure | Udacity project

Generative AI introduces a powerful new capability for interpreting financial reports (10-Q filings). It can efficiently extract key activities and financial metrics, enabling investors to quickly access concise summaries and analyses of a company's operations and financial performance. Microsoft Azure further supports this development by offering user-friendly tools for building such capabilities, allowing developers to focus on optimizing prompts to achieve the most accurate and relevant responses from large language models (LLMs).

This project utilizes sample data from NVIDIA’s 10-Q report, available here [Link](https://www.sec.gov/Archives/edgar/data/1045810/000104581024000264/nvda-20240728.htm).
A tool that we use to develop this capability is Azure AI Foundry [Link](https://ai.azure.com/).

The development process is structured into three key stages:
1. Setting up the Azure environment
2. Retrieval-Augmented Generation (RAG) Implementation (integrating a large language model (LLM) with the indexed data)
  - Uploading the 10-Q report to Azure Storage
  - Building a data indexing system
  - Connect LLMs with indexed data
3. Fine-tuning prompts and optimizing LLM parameters

## AI Azure service deployment (Setting up the Azure environment)
In Resource Group, three services need to be enable including Storage Account, Search Service, and Azure OpenAI.
![image](https://github.com/user-attachments/assets/79b5ffdf-4073-4439-b8a5-dda0d3920fe0)
#### Storage Account
This is used to store 10-Q report. Once the storage is created, upload the 10-Q data. Please note that when setting up the Storage Account, make sure that you use the same location as Resource Group. In this project, we use `West US`.
#### Search Service
It is a tool to provide information retrival as a part of RAG system. You can follow general instruction on [here](https://learn.microsoft.com/en-us/azure/search/search-create-service-portal).
#### Azure OpenAI
This is a main service to develop RAG systems. We can set up LLM, integrate the LLM with indexed data, fine-tune prompt, and test the response of our RAG application. 

Choices of implementation
- Use `GPT-4o` as LLM and `text-embedding-ada-002` as an embedding


In Deployment section, deploy `GPT-4o` and `text-embedding-ada-002`
![image](https://github.com/user-attachments/assets/8001e36a-4689-4740-a017-fb33f428c361)


## Retrieval-Augmented Generation (RAG) Implementation
How to build RAG systems.
1. In Azure AI Foundry, go to `Chat`.
2. Select LLM, write prompt, add data (from indexing data)

![image](https://github.com/user-attachments/assets/60440175-28bf-42d0-aa6f-f389b3d5e60a)

When setup indexed data with LLM, these are the settings.
- Select `Hybridge + sementics` for a method of retriving information from indexed data.
- Size of chunking data is 1024
- Data source is from 10-Q, which is already stored in Azure Storage.

LLM parameters are also adjusted on the parameter section at the very bottom.

<img src="https://github.com/user-attachments/assets/67b15992-cd71-4480-b3cd-133eb562eab8" width="400"/>


## Prompt development and testing

![image](https://github.com/user-attachments/assets/c041341c-88ea-4346-87e0-0409a45b444b)

System prompt:
```
You are a professional financial analyst AI specialised in reading and extracting insights from official SEC filings, particularly 10-Q quarterly reports.

When given a 10-Q document (or its text excerpt), extract and organise the following information clearly and accurately:

(1) Financial Performance: Summarise key financial metrics, including:
- Total Revenue
- Gross Profit (if available)
- Operating Income
- Net Income
- Cost of Revenue or Cost of Goods Sold
- Major changes (percentage growth or decline vs previous period, if stated).

(2) Business Operations: Highlight important events or strategic changes, such as:
- New product launches
- Expansions, mergers, acquisitions, or divestitures
- Operational restructuring or major organisational changes
- Notable supply chain issues or improvements

(3) Risk Factors: Identify any risks disclosed or updated in the quarter, especially:
- Market risks
- Operational risks
- Legal or regulatory risks
- Financial liquidity risks

(4) Management Discussion and Analysis (MD&A): Capture the key points from management’s discussion, including:
- Management’s explanation of financial results
- Challenges the company is facing
- Forward-looking statements or expectations for future performance

Important Instructions:
- Do not summarise the entire 10-Q — only extract the requested sections.
- Use bullet points or short paragraphs to present the information clearly.
- Quote exact figures and key phrases where appropriate.
- If a section is missing or not found, clearly state: "Not disclosed in the provided text."
- Maintain a professional, neutral tone suitable for investment research reports.
```

Response from RAG system when asking question `What is current look of NVIDIA?`
```
### NVIDIA's Current Look (Based on the Provided 10-Q Excerpts)  
   
#### (1) **Financial Performance**  
- **Total Revenue**: $35.08 billion for the quarter ended October 27, 2024, an increase from $18.12 billion in the same quarter of 2023. For the nine months ended October 27, 2024, revenue was $91.17 billion, up from $38.82 billion in the prior period  ^1^ .  
- **Gross Profit**: $26.16 billion for the quarter, compared to $13.40 billion in Q3 2023. For the nine-month period, gross profit was $69.14 billion versus $27.51 billion in the prior year  ^1^ .  
- **Operating Income**: $21.87 billion for the quarter, growing significantly from $10.42 billion in Q3 2023. For the nine months, operating income was $57.42 billion, up from $19.36 billion in the same period last year  ^1^ .  
- **Net Income**: $19.31 billion for the quarter, compared to $9.24 billion in Q3 2023. Net income for the nine months was $50.79 billion, a substantial increase from $17.48 billion in the prior period  ^1^ .  
- **Cost of Revenue**: $8.93 billion for the quarter, up from $4.72 billion in Q3 2023. For the nine months, cost of revenue was $22.03 billion compared to $11.31 billion in the previous year  ^1^ .  
- **Major Changes**: Revenue and profitability have seen remarkable growth, with quarterly revenue increasing by approximately 93.6% year-over-year .  
   
#### (2) **Business Operations**  
- **Segment Information**:  
  - **Compute & Networking**: Includes Data Center accelerated computing platforms, AI solutions, networking, automotive platforms, robotics, and cloud computing services  ^2^ .  
  - **Graphics**: Covers GeForce GPUs for gaming, streaming services, enterprise workstation graphics, virtual GPU software, automotive infotainment, and Omniverse Enterprise software for 3D applications  ^2^ .  
- **Recent Developments**:  
  - Strong demand for Hopper architecture (H200) and growth in AI solutions.  
  - Production ramp for next-gen Blackwell architecture scheduled for Q4 FY2025, with supply constraints expected  ^3^ .  
  - Increasing supply chain complexity due to growing product portfolio and supplier integration  ^3^ .  
- **Capital Return Program**:  
  - Repurchased 92 million shares for $11.1 billion in Q3 FY2025.  
  - Authorized to repurchase up to $46.4 billion of common stock as of October 27, 2024  ^2^ .  
   
#### (3) **Risk Factors**  
- **Supply Chain Risks**: Complexity in managing suppliers and variations in production planning for new and existing products  ^3^ .  
- **Inventory Risks**: Potential inventory provisions or impairments if supply commitments exceed demand  ^3^ .  
- **Demand Volatility**: Estimates for products and services could be incorrect, leading to revenue fluctuations  ^3^ .  
- **Legal Risks**: Pending derivative litigation regarding alleged insider trading and false statements about GPU demand related to cryptocurrency mining. No contingent liabilities accrued as of October 27, 2024  ^2^ .  
   
#### (4) **Management Discussion and Analysis (MD&A)**  
- **Explanation of Financial Results**:  
  - Revenue growth driven by demand for Data Center AI solutions, particularly Hopper architecture systems  ^3^ .  
- **Challenges**:  
  - Supply constraints for new Blackwell architecture systems.  
  - Complexity and risk in transitioning product architectures  ^3^ .  
- **Forward-Looking Statements**:  
  - Blackwell production shipments to ramp in FY2026.  
  - Continued demand for
```

