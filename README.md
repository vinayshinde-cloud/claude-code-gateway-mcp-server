# Integrate Claude Code with MCP using AWS AgentCore Gateway

## Overview

This project demonstrates how to integrate Claude Code with AWS AgentCore Gateway to build a centralized MCP (Model Context Protocol) server.

Instead of managing multiple MCP connections, all requests are routed through a single gateway. This reduces configuration complexity, optimizes context usage, and improves scalability in enterprise environments.

### Usecase details

| Information          | Details                                                   |
|:---------------------|:----------------------------------------------------------|
| Tutorial type        | Interactive (Jupyter Notebook)                            |
| AgentCore components | AgentCore Gateway, AgentCore Identity                     |
| Gateway Target type  | MCP server                                                |
| Inbound Auth IdP     | Amazon Cognito (can use others)                           |
| Tutorial vertical    | Cross-vertical                                            |
| Complexity           | Easy                                                      |
| SDK used             | boto3    

### Usecase Architecture
Claude Code ──(OAuth token)──▶ AgentCore Gateway ──▶ AWS Knowledge MCP Server ──▶ Tools
│
Amazon Cognito (auth)
AWS IAM Role (backend access)

![Solution Architecture](images/claude_code_agentcore_gateway_architecture_new.png)

### Use case key Features
Claude Code hits the gateway with an OAuth Bearer token
Gateway validates the token through Cognito
Gateway figures out which backend tools are needed (semantic search)
Gateway forwards the request to the right MCP target
Response comes back through the gateway to Claude Code

## What's in the Repo

├── claude-code-gateway-mcp-server.ipynb # Main setup notebook
├── utils.py # Helper functions for AWS resources
├── requirements.txt # Python deps
├── images/ # Architecture + screenshots
└── README.md # This file

## Prerequisites

**IAM permissions needed:**
- `iam:CreateRole`, `iam:PutRolePolicy`, `iam:PassRole`
- `cognito-idp:*` (for user pools and OAuth)
- `bedrock-agentcore:*` (for creating gateways and targets)

## Environment

An AWS account with admin permissions
Python 3.10+
Jupyter Notebook
Claude Code CLI installed and logged in

## Usage

**Install dependencies**
python3 -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -U -r requirements.txt
jupyter notebook claude-code-gateway-mcp-server.ipynb

## Sample Prompts

Once the gateway is registered in Claude Code, try:

- `/mcp` → select `my-tools-gw` → View tools to see available tools
- Ask Claude Code questions that leverage the AWS Knowledge MCP Server tools exposed through the gateway


