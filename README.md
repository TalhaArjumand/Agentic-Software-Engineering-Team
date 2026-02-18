# Engineering Team

A multi-agent code generation workflow built with [CrewAI](https://github.com/crewAIInc/crewAI).  
This project takes high-level product requirements and produces:

- a backend Python module,
- a simple Gradio demo UI,
- and unit tests,

using a sequential team of specialized agents.

## Overview

The workflow is designed to simulate an engineering team:

- **Engineering Lead**: creates a detailed design from requirements.
- **Backend Engineer**: implements the Python backend module.
- **Frontend Engineer**: builds a minimal Gradio UI that uses the backend.
- **Test Engineer**: writes unit tests for the backend.

Artifacts are written to the `output/` directory.
<img width="1920" height="917" alt="image" src="https://github.com/user-attachments/assets/2d3d15f6-53c4-4a2e-9f0a-c75cdba03081" />


## How It Works

Execution starts in `src/engineering_team/main.py`, which passes input variables (requirements, module name, class name) to the Crew:

1. `design_task` -> writes `output/{module_name}_design.md`
2. `code_task` -> writes `output/{module_name}`
3. `frontend_task` -> writes `output/app.py`
4. `test_task` -> writes `output/test_{module_name}`

Task and agent behavior is configured in:

- `src/engineering_team/config/agents.yaml`
- `src/engineering_team/config/tasks.yaml`

## Repository Structure

```text
.
├── src/engineering_team/
│   ├── main.py                 # Entry point
│   ├── crew.py                 # Crew and task wiring
│   ├── config/
│   │   ├── agents.yaml         # Agent prompts, roles, models
│   │   └── tasks.yaml          # Task definitions and output files
│   └── tools/
│       └── custom_tool.py      # Optional custom CrewAI tool scaffold
├── knowledge/
├── output/                     # Generated artifacts (created at runtime)
├── example_output_*            # Sample generated outputs
└── pyproject.toml
```

## Prerequisites

- Python `>=3.10,<3.13`
- Docker (recommended/required when using CrewAI safe code execution mode)
- API keys for the LLM providers referenced in `agents.yaml`

Typical environment variables:

- `OPENAI_API_KEY`
- `ANTHROPIC_API_KEY`

## Installation

### Option 1: Using `uv` (recommended)

```bash
uv sync
```

### Option 2: Using `pip`

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -e .
```

## Usage

Run the crew:

```bash
uv run engineering_team
```

Alternative:

```bash
python3 -m engineering_team.main
```

After a run, check generated files in `output/`.

## Configuration

### Update the product brief

Edit the values in `src/engineering_team/main.py`:

- `requirements`
- `module_name`
- `class_name`

### Change model/provider behavior

Edit `src/engineering_team/config/agents.yaml` to update:

- agent goals and prompts,
- LLM providers/models per agent.

### Change task flow/output locations

Edit `src/engineering_team/config/tasks.yaml` to update:

- task descriptions,
- context dependencies,
- output file paths.

## Notes

- `output/` is created automatically if missing.
- Example generated outputs are included under `example_output_4o/`, `example_output_new/`, and `example_output_mini/`.
- Only the `run` entry point is implemented in `main.py`.

## Troubleshooting

- **`ModuleNotFoundError: crewai`**
  - Ensure dependencies are installed (`uv sync` or `pip install -e .`).
- **Provider authentication errors**
  - Confirm required API keys are exported in your shell/session.
- **Code execution issues in safe mode**
  - Verify Docker is installed and running.

## License

Add your project license information here.
