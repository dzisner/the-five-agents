# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This project is a multi-agent content-creation system. A CEO orchestrator agent manages a team of specialized sub-agents, each responsible for a distinct part of the content pipeline. The team composition and agent roles will be defined as the project evolves.

## Project-specific Claude configuration

Custom agents, skills, and commands for this project live under `.claude/`:

```
.claude/
├── agents/    # Sub-agent definitions
├── skills/    # Reusable skill prompts
└── commands/  # Custom slash commands
```
