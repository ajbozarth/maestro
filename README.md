# Maestro

Maestro is a tool for managing and running AI agents and workflows.

## Installation

```bash
pip install git+https://github.com/AI4quantum/maestro.git@v0.1.0
```

Note: If using scoring or crewai agents, install:
```bash
pip install "maestro[crewai] @ git+https://github.com/AI4quantum/maestro.git@v0.1.0"
```

## Usage

1. Run a workflow:
```bash
maestro run <workflow_path>
```

2. Create an agent:
```bash
maestro create <agent_path>
```

3. Validate a workflow or agent:
```bash
maestro validate <path>
```

## Development

1. Clone the repository:
```bash
git clone https://github.com/AI4quantum/maestro.git
cd maestro
```

2. Install development dependencies:
```bash
uv pip install -e .
```

3. Run the formatter:
```bash
uv run ruff format
```

4. Run the linter:
```bash
uv run ruff check
```


## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## License

This project is licensed under the Apache license- see the [LICENSE](LICENSE) file for details.
