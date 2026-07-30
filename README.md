# WorldWeaver 🌐🕸️

**A multi-agent system for the automatic generation of interactive 3D virtual worlds from text.**

WorldWeaver transforms a natural language narrative into a navigable and reactive 3D environment that runs directly in the browser. Given a text input, a pipeline of language-model-based agents (orchestrated with LangGraph) segments the story into scenes, populates them with 3D models, adds interaction logic and ambient sound, and compiles everything into a self-contained HTML document.

This repository accompanies the Master's Thesis of the same name (*Master's Degree in Applied Artificial Intelligence, Universidad Carlos III de Madrid, 2025–2026*).

## Features

* **Multi-agent pipeline** consisting of six agents: Organizer → Director → Builder → Programmer → Musician → Assembler (plus an **Examiner** in educational mode), with semantic validation and deterministic repair between stages.
* **Two operating modes**: *narrative* (exploration and immersion) and *educational* (learning content + final assessment questionnaire).
* **Bilingual generation**: worlds can be generated in Spanish or English.
* **Self-contained 3D viewer**: each world is a single HTML file that runs in any modern browser (Three.js / WebGL), with no installation required.

## Requirements

* Python 3.11 or higher.
* Three API keys are required to generate new worlds: an LLM provider, [Poly Pizza](https://poly.pizza/) (3D models), and [Freesound](https://freesound.org/) (audio). Poly Pizza and Freesound are free; the LLM can be replaced with a free local model ([Ollama](https://ollama.com/)). **API keys are not required to explore the included example worlds.**

## Getting Started

**1. Clone the repository and install dependencies:**

```bash
git clone https://github.com/aliipz/The_WorldWeaver.git
cd WorldWeaver
pip install -r worldweaver/requirements.txt
```

**2. Configure the API keys.** Copy the template file and add your credentials:

```bash
cp worldweaver/.env.example worldweaver/.env
# edit worldweaver/.env
```

| Variable (`worldweaver/.env`) | Service                                                                    | How to obtain it                                                                                                                                                                     |
| ----------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `MERCURY_API_KEY`             | LLM provider — [Mercury 2 (Inception Labs)](https://www.inceptionlabs.ai/) | Create an account on the platform and generate an API key. *(Keyless alternative: run a local model with [Ollama](https://ollama.com/) and configure `OLLAMA_*` variables instead.)* |
| `POLYPIZZA_API_KEY`           | [Poly Pizza](https://poly.pizza/) — 3D models                              | Register for free and copy your API key from the account settings (API section).                                                                                                     |
| `FREESOUND_API_KEY`           | [Freesound](https://freesound.org/) — ambient audio                        | Register for free and request credentials at [freesound.org/apiv2/apply](https://freesound.org/apiv2/apply/).                                                                        |

> Without API keys, the system still starts: you can browse the example worlds, but you cannot generate new ones (external service requests will fail). Backup audio and character assets are included, so generated worlds always contain sound and figures even if Freesound or Poly Pizza are unavailable.

## Running the Application

The system provides three ways to use the same core functionality:

**1. Web server (recommended).** Start the local interface:

```bash
cd worldweaver
uvicorn server:app --port 8000
# open http://localhost:8000
```

**2. Command line** (unattended generation):

```bash
python worldweaver/tests/test_total.py text.txt my_world
```

**3. Desktop application.** Running `python launch.py` starts the server and opens the browser in app mode. To package it as a standalone executable:

```bash
cd worldweaver
python -m PyInstaller worldweaver.spec --noconfirm
```

Generated worlds are stored in `worldweaver/outputs/<name>/` as self-contained HTML files.

## Project Structure

```
worldweaver/
  agents/      The six specialized agents in the pipeline
  config/      Configuration, prompts (ES/EN), and scene geometry
  schemas/     Pydantic schemas (agent communication contracts)
  pipeline/    LangGraph workflow, shared state, validators, assembler
  sandbox/     3D viewer template (Three.js) + assets (Quaternius, music)
  server.py    FastAPI server + landing page
  fixtures/    Example input texts
  outputs/     Example generated worlds (from evaluation)
metricas/      Raw technical evaluation data
```

## Example Worlds and Evaluation

The `worldweaver/outputs/` folder contains the worlds used in the Master's Thesis evaluation (their source texts are located in `worldweaver/fixtures/`). Raw technical evaluation data can be found in `metricas/`.

## License

The **code** is released under the **MIT License** (see [`LICENSE`](LICENSE)).

**Third-party resources** retain their own licenses: Quaternius characters and backup soundscapes are CC0; Poly Pizza models and Freesound audio retrieved at runtime are governed by the license of each individual resource.

## Authorship

Alicia Pina Zapata — Master's Thesis, Universidad Carlos III de Madrid (2025–2026).
Supervisor: Andrea Bellucci.
