# WireShield — Voice Banking Fraud-Confirmation Agent

WireShield is a voice-driven fraud-confirmation agent built for secure high-risk wire transfers. It combines Deepgram speech processing, Twilio telephony, LangGraph orchestration, and FastAPI tooling to verify callers, assess risk, and approve or cancel payments with a clear audit trail.

## Overview

WireShield verifies identity with a multi-factor voice workflow, confirms human liveness, and executes payment decisions through tool calls. The system is designed for fast, phone-first interactions with strong safety controls and explicit state management.

## Key Features

- 3-step identity verification before transaction disclosure
- Dynamic liveness phrase and deepfake risk scoring
- Tool-based payment approval/cancellation
- Mock FastAPI sandbox for wire transfers
- SQLite audit logging for call events
- Modular architecture with clear session isolation

## Getting Started

### Requirements

- Python 3.10 or newer
- Deepgram API key
- OpenAI API key
- Optional Twilio account for real calls

### Run the sandbox API

```bash
uv run uvicorn api.bank_sandbox:app --host 0.0.0.0 --port 8000 --reload
```

### Start the voice bridge

```bash
uv run python main.py
```

### Optional: expose via ngrok

```bash
ngrok http 5000
```

Then point Twilio Media Streams to your public WebSocket URL.

## How It Works

1. Collect the caller’s phone number
2. Verify card last 4 digits
3. Verify customer ID last 4 digits
4. Ask the caller to repeat a dynamic liveness phrase
5. Perform a deepfake risk check
6. Read back payment details
7. Approve or cancel the transfer

## Configuration

WireShield uses environment variables with the `WIRESHIELD_` prefix and keys for Deepgram/OpenAI.

Example variables:

- `DEEPGRAM_API_KEY`
- `OPENAI_API_KEY`
- `WIRESHIELD_DB_URL`
- `WIRESHIELD_BANK_API_BASE_URL`
- `WIRESHIELD_REDIS_URL`
- `WIRESHIELD_SIGNING_SECRET`
- `WIRESHIELD_REQUIRE_SIGNED_REQUESTS`

## Project Structure

- `main.py` — WebSocket-based voice bridge and Deepgram relay
- `config.json` — LLM prompt, tool definitions, STT/TTS settings
- `api/bank_sandbox.py` — Mock banking API and payment endpoints
- `wireshield/bank_tools.py` — Payment and verification tool implementations
- `wireshield/bank_data.py` — In-memory payment and identity data
- `wireshield/graph.py` — LangGraph state machine and verification flow
- `wireshield/session.py` — Per-call session management
- `wireshield/storage.py` — SQLite event logging
- `wireshield/dfdetect.py` — Deepfake risk scoring stub
- `tests/` — Unit tests for core behavior

## Testing

Run the test suite:

```bash
uv run pytest -q
```

## Contributing

Contributions are welcome. Please open an issue or submit a pull request with bug fixes, enhancements, or documentation improvements.
