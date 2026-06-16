# IA_whatsapp_Assistant
🤖 AI-powered WhatsApp appointment scheduling assistant built with n8n. handles text &amp; voice messages, integrates with a CRM backend, a real-time dashboard, and uses Google Gemini + Groq Whisper for multilingual conversation and audio transcription.

<img width="1655" height="545" alt="image" src="https://github.com/user-attachments/assets/ebbe00cf-8968-4ae5-b512-a48ab2f7eca9" />

📅 IA Assistant — AI WhatsApp Appointment Scheduling Workflow
A fully automated, end-to-end appointment scheduling system built with n8n. It engages leads directly on WhatsApp via the WAHA API, understands both text and voice messages, and coordinates across a CRM backend and a real-time dashboard — all driven by AI.

🔗 Integrations

WhatsApp (WAHA API) — Receives and sends messages via webhook. The bot handles both incoming text messages and voice notes seamlessly.
CRM Backend — Fetches lead data by WhatsApp JID, updates appointment status, creates calendar events, and patches the whatsappAnswer flag on the lead record after first contact.
Dashboard / Data Table — Maintains a live scheduling_state table that tracks each lead's conversation state: last message sent, last message received, selected date, and selected time slot.


🎙️ Voice Message Handling
When a lead replies with a WhatsApp audio message, the workflow:

Detects the presence of a media payload in the webhook body.
Extracts the audio URL and downloads the .ogg file from the WAHA API.
Sends the audio to Groq's Whisper large-v3 model for transcription.
Uses the transcribed text as the input to the AI scheduling agent — exactly like a text message.

This means leads can respond by voice and the bot understands them just as well.

🧠 AI Scheduling Logic
Powered by Google Gemini, the AI agent classifies each incoming message (text or transcribed voice) into one of the following intents:

accept — the user agrees to the proposed slot
refuse — the user refuses without suggesting another date
propose_date — the user suggests a different date
select_time — the user picks a time from a list of available slots
unknown — unclear, invalid, or off-topic message (bot asks for clarification)

The agent is also aware of the conversation context (last message sent, current date, timezone: Europe/Paris) and always responds in the user's own language.

⚙️ Workflow Logic

A webhook receives an incoming WhatsApp message.
The lead is identified in the CRM by their WhatsApp JID.
If it's a first contact (whatsappAnswer = false), the bot fetches the next available slot from the CRM and sends a personalized meeting proposal.
On subsequent replies, the AI agent classifies the intent and the workflow branches accordingly: checking availabilities, updating the selected date/time, booking the event in the CRM, or re-engaging with alternative slots.
Once a slot is confirmed, a calendar event is created in the CRM and a confirmation message is sent to the user.
Every message sent and received is logged in the dashboard state table for traceability.


🛠️ Built With

n8n — Workflow automation engine
WAHA — WhatsApp HTTP API (send/receive messages & media)
Google Gemini (PaLM) — Intent classification & multilingual response generation
Groq Whisper large-v3 — Voice message transcription
Custom CRM REST API — Lead management, availability, and event booking
n8n Data Table — Real-time scheduling state & conversation tracking dashboard
