# Environment Setup

## Table of contents

* [Prerequisites](#Prerequisites)
* [Project Setup](#Project-Setup)
* [CTF Exercises Setup](#CTF-Exercises-Setup)

## Prerequisites

This course assumes some basic knowledge of Python and Unix like systems (e.g., Linux and MacOS). Most of the setup and configurations instructions have been designed for Linux, although they should work on a MacOS/Windows environment provided access to a console.

### Managing Python

Python is often pre-installed on Linux. However, a better solution is either to have it managed via [PyEnv](https://github.com/pyenv/pyenv) or [uv](https://docs.astral.sh/uv/). For this course you are required Python version >= 3.12.

#### Installing `pyenv`

Pyenv is a Python environment manager, that allows you to have multiple Python versions installed in your system (even locally as a user) without conflicts.

You can check the installation instructions on the different OS available in the [official documentation](https://github.com/pyenv/pyenv?tab=readme-ov-file#installation). After that you can install the required Python version and set it up locally on this same directory:

```bash
$ pyenv install 3.12
$ pyenv local 3.12
```

#### Installing `uv`

`uv` is an extremely fast Python package installer and resolver, written in Rust. It is designed to be a drop-in replacement for `pip` and `pip-tools`.

While you can install `uv` using `pip`, the recommended way to install command-line applications like `uv` is using `pipx`. `pipx` installs applications in isolated environments, preventing conflicts with your project dependencies. There are other ways to install `uv` as well, you can check [the official documentation](https://docs.astral.sh/uv/getting-started/installation/)

To install `uv` using `pipx`, run the following command in your terminal:

```bash
pipx install uv
```

If you don't have `pipx` installed, you'll need to install it first (e.g., `pip install pipx` or using your system's package manager).

After installation, you can verify that `uv` is installed correctly by checking its version:

```bash
uv --version
```

This command should display the installed version of `uv`.

## Project Setup

### Setup with uv

`uv` makes setting up a project and installing dependencies straightforward and fast.

#### Syncing Virtual Environment

To create and sync the virtual environment for your project using `uv`, from the root directory run:

```bash
$ uv sync
```

By default, this command creates a directory named `.venv` in your current directory and install all dependencies on it.

#### Activating the Virtual Environment

You can activate the virtual environment via `source`:

```bash
source .venv/bin/activate
```

However this is not necessary as the preferred way is to run everything in your environment via `uv` or [running tools in an isolated environment](https://docs.astral.sh/uv/guides/tools/) via `uvx` (like `pipx`):

```bash
$ uv run -- jupyter lab
```

## CTF Exercises Setup

### PromptMe

[`PromptMe`](https://github.com/R3dShad0w7/PromptMe) is a vulnerable LLM application designed for educational purposes to demonstrate the OWASP Top 10 for LLM Applications. It provides a series of CTF (Capture The Flag) challenges. I have [forked and modified the original version](https://github.com/crscardellino/PromptMe) so it can be set up with Docker and docker-compose. Until I manage to make a PR to the original repo we'll be using my version for the training.

#### Setup with Docker

This is the easiest method as it handles all dependencies, including the Ollama server and required models.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/crscardellino/PromptMe.git
    cd PromptMe
    ```
2.  **Build and run:**
    ```bash
    docker-compose up --build
    ```
3.  **Access the application:**
    Open your browser to `http://127.0.0.1:5000`.

### Gandalf

[Gandalf is an interactive game created by Lakera](https://gandalf.lakera.ai/intro) to challenge and enhance your prompt injection skills. The primary goal is to cleverly trick the AI into revealing a secret password. The game features 8 levels of progressively increasing difficulty, along with special "Adventures" that focus on specific prompt injection techniques.