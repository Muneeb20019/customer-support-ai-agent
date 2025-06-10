Guide: Setting Up ElevenLabs for Your Customer Support AI Agent


1. Configuring the First Message and System Prompt
This section outlines how to set the agent's first message and system prompt.
●	First Message:
Find the "First message" field within your ElevenLabs agent configuration.
Input the following text: 
“Hey there, How can I help you today? ”
●	System Prompt:
Find the "System prompt" field within your ElevenLabs agent configuration.
Input the following text:

“You are an assistant helping guests with reservations and information for the Burj Khalifa Residences & Hotel. Your role is to listen to the guest's question, understand their intent, and then process their request. When responding, provide a clear, concise, and friendly answer.Maintain a friendly and helpful tone. Don't repeat the question back to the guests; just give a concise answer. Maintain a consistent flow in your response; don't interrupt yourself with something else to say.”

2. Setting Up the n8n Integration Tool
This tool allows ElevenLabs to send user queries to your n8n workflow for processing and retrieving information from your knowledge base.
●	Tool Type: Select "Webhook".

●	Configuration:
○	Name: n8n.
○	Description: "Use this tool to get information about Burj Khalifa hotel rooms, prices, amenities, and reservation policies." 
○	Method: Set to POST.
○	URL: Input your specific n8n endpoint url for receiving requests.

●	Request Body (Properties): This defines what data ElevenLabs will send to n8n.
○	Description: "Ask the user what they need help with."
○	Properties:
■	Data type: String.
■	Identifier: Question.
■	Required: Check this box (meaning the 'Question' parameter must always be sent).
■	Value Type: Select LLM Prompt. This ensures that the complete transcribed user query is sent as the value for the Question parameter to your n8n workflow.
■	Description (for this property): "question from the user".

3. Saving and Testing
●	After configuring these settings, ensure you save your ElevenLabs agent.
●	You can then test the agent within ElevenLabs to confirm it correctly sends queries to your n8n endpoint and receives responses.



