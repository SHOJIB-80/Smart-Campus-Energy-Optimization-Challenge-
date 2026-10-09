# Smart Campus Energy Optimization Challenge

A Python-based FastAPI service for interpreting informal operator notes into structured energy management directives for a smart campus energy system.

## Overview

This repository contains a lightweight energy management API that converts operator instructions written in natural language into structured JSON directives. The service is designed for smart campus or grid operations scenarios where staff may issue ad hoc notes such as maintenance windows, discharge restrictions, or generation expectations.

The implementation uses FastAPI, Pydantic models, and an optional OpenAI client to parse operator notes into directive objects. If no OpenAI API key is configured, the system falls back to safe, structured responses instead of failing.

## Features

- FastAPI-based REST API for note interpretation
- Pydantic request/response validation
- Batch processing of multiple operator notes
- Structured directive generation with `directive_type`, `parameters`, and `reasoning`
- Optional OpenAI-powered interpretation using `OPENAI_API_KEY`
- Graceful fallback behavior when the LLM is unavailable
- Health endpoint for service monitoring
- Example validation scripts for API testing

## Tech Stack

- Python 3.9+
- FastAPI
- Pydantic
- Uvicorn
- OpenAI Python SDK
- JSON-based API payloads

## Project Structure

```text
Smart-Campus-Energy-Optimization-Challenge-/
├── main.py                 # Main FastAPI application
├── Optimize.py             # Alternative async FastAPI implementation
├── Server.py               # Additional API implementation with schedule optimization logic
├── test_api.py             # Simple API validation client
├── test_optimizer.py       # API validation script for the optimization service
├── project_readme.txt      # Original project documentation
├── README.md              # Project documentation
├── requirements.txt       # Present in the repo, but currently empty
└── .gitignore             # Repository configuration file (if present in GitHub project metadata)
```

Notes:
- The repository contains more than one FastAPI implementation variant (`main.py`, `Optimize.py`, and `Server.py`), which suggests the project was built and iterated across a few prototypes.
- The primary API behavior is the conversion of operator notes into interpreted directives.

## Requirements / Prerequisites

Before running the project, make sure you have:

- Python 3.9 or newer
- `pip` installed
- Access to a terminal or command prompt
- An OpenAI API key if you want real LLM interpretation enabled

Important:
- The repository currently includes a `requirements.txt` file, but it is empty in the checked-in state.
- The code imports `fastapi`, `pydantic`, `uvicorn`, and `openai`, so these packages should be installed before running the project.

## Installation

Create a virtual environment and install the required packages:

```bash
python -m venv .venv
source .venv/bin/activate
pip install fastapi uvicorn pydantic openai
```

On Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install fastapi uvicorn pydantic openai
```

TODO:
- Populate `requirements.txt` with the exact package versions used by the project.

## Configuration

The service supports optional OpenAI-based parsing.

Set the environment variable:

```bash
export OPENAI_API_KEY="your_openai_api_key"
```

On Windows:

```powershell
set OPENAI_API_KEY=your_openai_api_key
```

Optional model override:

```bash
export OPENAI_MODEL="gpt-4o-mini"
```

If no API key is provided, the app returns structured fallback responses rather than crashing.

## Running the Project

Start the main FastAPI service:

```bash
python -m uvicorn main:app --reload
```

If you want to run the alternative implementation in `Optimize.py`, use:

```bash
python -m uvicorn optimize:app --reload
```

Note:
- On Linux/macOS, the file name is case-sensitive. The repository contains `Optimize.py`, not `optimize.py`.
- The app typically runs on:

```text
http://127.0.0.1:8000
```

## Usage

Send a POST request to:

```http
POST /api/v1/interpret-notes
```

Example request body:

```json
{
  "scenario_id": "SCENARIO-01",
  "operator_notes": [
    {
      "id": "NOTE-101",
      "text": "Maintenance scheduled from 14:00 to 16:00, stop discharging battery."
    }
  ]
}
```

Example response:

```json
{
  "scenario_id": "SCENARIO-01",
  "status": "PROCESSED_SUCCESSFULLY",
  "interpreted_directives": [
    {
      "note_id": "NOTE-101",
      "directive_type": "BATTERY_DISCHARGE_RESTRICTION",
      "parameters": {
        "start_hour": 14,
        "end_hour": 16,
        "max_discharge_kw": 0.0
      },
      "reasoning": "Processed via OpenAI LLM"
    }
  ]
}
```

## API Documentation

### Health check

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET | `/health` | Verifies the service status and availability |

Example response:

```json
{
  "status": "healthy"
}
```

### Interpret operator notes

| Method | Endpoint | Purpose |
| --- | --- | --- |
| POST | `/api/v1/interpret-notes` | Parses a batch of operator notes into structured directives |

Request fields:

- `scenario_id` (string): Scenario identifier
- `operator_notes` (array): list of note objects
  - `id` (string): note ID
  - `text` (string): human-written operator instruction

Response fields:

- `scenario_id` (string)
- `status` (string)
- `interpreted_directives` (array)
  - `note_id`
  - `directive_type`
  - `parameters`
  - `reasoning`

## How It Works

```text
Operator note
   ↓
FastAPI endpoint
   ↓
Pydantic validation
   ↓
OpenAI LLM (optional)
   ↓
Structured JSON directive output
```

If the LLM is not configured, the app returns a structured fallback response (`no_op` or equivalent safe value) instead of raising an exception.

## Testing

The repository includes validation scripts for the API.

Run the API test client:

```bash
python test_api.py
```

Run the optimization validation script:

```bash
python test_optimizer.py
```

These scripts assume the API server is already running on:

```text
http://127.0.0.1:8000
```

## Known Limitations

- The repository contains multiple app implementations (`main.py`, `Optimize.py`, `Server.py`) without a single clearly standardized entry point.
- `requirements.txt` is empty, so package dependencies are not pinned or documented in the repository.
- The service is focused on natural-language directive parsing and not a full production energy optimization platform.
- Without an OpenAI API key, directive interpretation falls back to safe default responses rather than true AI parsing.
- There is no database, authentication layer, or deployment configuration in the checked-in code.

## TODO

- Add the exact project dependencies to `requirements.txt`
- Standardize a single canonical app entry point
- Document exact production deployment steps if the project expands beyond the prototype stage

## Contributing

Contributions are welcome if you want to improve the API, clean up the project structure, or standardize the service entry points.

Suggested workflow:

```bash
git checkout -b feature/your-change
# make changes
python -m pytest
git commit -am "Add your improvement"
git push origin feature/your-change
```

## License

No explicit license file was found in the repository, so no license is declared for this project.

##
MD Sirajul Islam 
Hadiul Hridoy
