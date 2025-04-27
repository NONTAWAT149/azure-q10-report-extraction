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

<img src="https://github.com/user-attachments/assets/67b15992-cd71-4480-b3cd-133eb562eab8" width="100"/>


## Prompt development and testing




