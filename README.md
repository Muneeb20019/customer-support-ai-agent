# Customer Support AI Voice Agent 

## Project Overview

This project implements an AI-powered voice assistant designed to provide customer support and information for the Burj Khalifa Residences & Hotel. Leveraging robust automation and AI services, it handles guest inquiries about hotel rooms, prices, amenities, and reservation policies, providing instant, conversational responses.

## Key Features

* **Voice-Enabled Interactions:** Utilizes ElevenLabs for natural language understanding and text-to-speech, enabling a seamless voice experience.
* **Automated Workflows:** Powered by n8n, orchestrating complex interactions between AI models, knowledge bases, and external tools.
* **Intelligent Information Retrieval:** Integrates with a Pinecone vector database to efficiently search and retrieve relevant information from a comprehensive hotel knowledge base.
* **Dynamic Responses:** Leverages OpenAI's chat models for intelligent reasoning and generating context-aware responses.

## Technologies Used

* **ElevenLabs:** AI Voice Synthesis and Agent Platform.
* **n8n:** Workflow Automation (Self-hosted or Cloud).
* **Pinecone:** Vector Database for scalable information retrieval.
* **OpenAI:** For Embeddings (text-to-vector conversion) and Chat Models (AI reasoning).
* **Google Drive:** As the source for the knowledge base document.

## System Architecture

Here's a high-level overview of how the Customer Support AI Agent works:

1.  **Voice Input (ElevenLabs):** A guest speaks their query to the ElevenLabs voice agent.
2.  **Tool Invocation (ElevenLabs):** ElevenLabs' LLM identifies the need for information and invokes a configured tool to send the user's question to n8n.
3.  **Workflow Trigger (n8n - Voice Agent):** An n8n workflow (`voice-agent-workflow.json`) receives the user's question via a webhook.
4.  **AI Reasoning & Knowledge Retrieval (n8n - AI Agent Node):**
    * The n8n AI Agent node uses an OpenAI Chat Model for reasoning.
    * It accesses a Pinecone Vector Store tool to search for relevant information within the hotel's knowledge base.
    * OpenAI Embeddings are used to convert query text into vectors for Pinecone search.
5.  **Response Generation (n8n):** The AI Agent node formulates a concise answer based on the retrieved information.
6.  **Voice Output (n8n & ElevenLabs):** The n8n workflow sends the AI's textual response back to ElevenLabs, which converts it into natural-sounding speech for the guest.

### Data Ingestion Workflow:

The knowledge base is maintained and updated via a separate n8n workflow:

1.  **Knowledge Base Source (Google Drive):** Hotel information is stored in a Google Doc.
2.  **Data Loader (n8n - Info Loader):** The `info-loader-workflow.json` is triggered manually (or on a schedule) to download the Google Doc content from Google Drive.
3.  **Text Processing (n8n):** The document content is processed by a Default Data Loader and then split into smaller, manageable chunks using a Recursive Character Text Splitter.
4.  **Embedding (n8n):** Each text chunk is converted into numerical vector embeddings using OpenAI Embeddings.
5.  **Vector Storage (Pinecone):** These embeddings are then uploaded to the Pinecone vector database, making them searchable by the AI agent.

## Project Setup & Files

This repository contains all the necessary files to understand and recreate the Customer Support AI Agent.

* `workflows/`: Contains the n8n workflow `.json` files.
    * [`voice-agent-workflow.json`](workflows/voice-agent-workflow.json): The primary workflow for handling live user interactions.
    * [`info-loader-workflow.json`](workflows/info-loader-workflow.json): The workflow responsible for loading and embedding knowledge base content into Pinecone.
* `images/`: Visual aids for understanding the n8n workflows.
    * [`n8n-voice-agent-overview.png`](images/n8n-voice-agent-overview.png): Overview of the voice agent workflow.
    * [`n8n-info-loader-overview.png`](images/n8n-info-loader-overview.png): Overview of the information loader workflow.
    * [`demo-thumbnail.png`]: (Optional) Thumbnail image for the demo video.
* `knowledge-base/`: The textual content of the hotel's knowledge base.
    * [`burj-khalifa-guide.md`](knowledge-base/burj-khalifa-guide.md): The comprehensive guide for Burj Khalifa Residences & Hotel information.
* `elevenlabs-prompts/`: Configuration details for the ElevenLabs AI agent.
    * [`elevenlabs-setup-guide.md`](elevenlabs-prompts/elevenlabs-setup-guide.md): A guide detailing the ElevenLabs agent setup, including the system prompt, initial greeting, and n8n tool configuration.

## Demonstration

Watch a quick demonstration of the Customer Support AI Agent in action:

[![Demo Video Thumbnail](images/YOUR_DEMO_THUMBNAIL_FILENAME.png)](YOUR_SCREENREC_SHAREABLE_LINK_HERE)
*(Remember to replace `YOUR_DEMO_THUMBNAIL_FILENAME.png` with the actual filename if you include one in your `images` folder, and `YOUR_SCREENREC_SHAREABLE_LINK_HERE` with your video's public link from ScreenRec).*

## How to Set Up (High-Level Steps)

1.  **n8n Setup:**
    * Import `info-loader-workflow.json` into your n8n instance.
    * Configure the Google Drive node to access your knowledge base document.
    * Configure OpenAI Embedding and Pinecone nodes with your API keys and index name.
    * Run this workflow once to load your knowledge base into Pinecone.
    * Import `voice-agent-workflow.json` into your n8n instance.
    * Configure the OpenAI Chat Model and Pinecone Vector Store tool nodes with your API keys and index name.
    * Activate the webhook for this workflow (its URL will be used in ElevenLabs).
2.  **ElevenLabs Setup:**
    * Follow the detailed steps in `elevenlabs-prompts/elevenlabs-setup-guide.md` to configure your ElevenLabs agent, including the system prompt, initial greeting, and the tool integration pointing to your n8n URL.
3.  **Testing:** Interact with your ElevenLabs agent to test the end-to-end functionality.
