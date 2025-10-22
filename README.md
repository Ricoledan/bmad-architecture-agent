# BMAD Architecture Agent

A comprehensive cloud architecture design and analysis agent built using the BMAD (Building Multi-Agent Designs) framework.

## Overview

This agent helps with:
- Cloud architecture design and analysis
- Multi-cloud service selection and comparison
- Cost estimation and optimization
- Security and compliance assessment
- Architecture documentation and decision records

## Structure

```
bmad-architecture-agent/
├── agents/           # Agent definitions
├── templates/        # Document templates
├── tasks/           # Task procedures
├── knowledge/       # Knowledge base
├── projects/        # Active projects
├── tools/          # Python utilities
└── config/         # Configuration files
```

## Getting Started

1. Copy `.env.example` to `.env` and configure your cloud provider credentials
2. Install Python dependencies: `pip install -r tools/requirements.txt`
3. Review the configuration in `config/bmad.yaml`

## Usage

The agent provides structured templates, tasks, and knowledge base for cloud architecture work. Each component is designed to be modular and reusable across different projects.

## Components

- **Agents**: Expert agent definitions with specific capabilities
- **Templates**: Standardized document formats for architecture artifacts
- **Tasks**: Step-by-step procedures for common architecture activities
- **Knowledge**: Curated knowledge base of patterns, services, and best practices
- **Tools**: Optional Python utilities for automation and analysis

## License

See LICENSE file for details.
