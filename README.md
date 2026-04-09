🚀 Integrate Claude Code with Mcp using AgentCore Gateway 
Overview

This project integrates Claude Code with AWS AgentCore Gateway to create a centralized MCP server. Instead of managing multiple MCP connections, all requests are routed through a single gateway—reducing complexity, saving context window, and improving scalability.


**Context window overhead**: Every MCP server adds tool definitions to the context window, leaving less room for code reasoning.
- **Configuration sprawl**: In enterprise environments, each developer must individually configure and authenticate against every server.

AgentCore Gateway solves both by acting as a single, central MCP server that aggregates multiple backend MCP servers behind one endpoint with dynamic tool loading via semantic search.

##Usecase details

| Information          | Details                                                   |
|:---------------------|:----------------------------------------------------------|
| Tutorial type        | Interactive (Jupyter Notebook)                            |
| AgentCore components | AgentCore Gateway, AgentCore Identity                     |
| Gateway Target type  | MCP server                                                |
| Inbound Auth IdP     | Amazon Cognito (can use others)                           |
| Tutorial vertical    | Cross-vertical                                            |
| Complexity           | Easy                                                      |
| SDK used             | boto3    

## Architecture

Claude Code ──(OAuth token)──▶ AgentCore Gateway ──▶ AWS Knowledge MCP Server ──▶ Tools
│
Amazon Cognito (auth)
AWS IAM Role (backend access)


![Solution Architecture](images/claude_code_agentcore_gateway_architecture_new.png)

**Flow:**
1. Claude Code hits the gateway with an OAuth Bearer token
2. Gateway validates the token through Cognito
3. Gateway figures out which backend tools are needed (semantic search)
4. Gateway forwards the request to the right MCP target
5. Response comes back through the gateway to Claude Code

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

### Environment

- An AWS account with admin permissions
- Python 3.10+
- Jupyter Notebook
- Claude Code CLI installed and logged in


## Usage

1. **Install dependencies**
python3 -m venv .venv
source .venv/bin/activate
pip install -U pip
pip install -U -r requirements.txt
jupyter notebook claude-code-gateway-mcp-server.ipynb

## Sample Prompts

Once the gateway is registered in Claude Code, try:

- `/mcp` → select `my-tools-gw` → View tools to see available tools
- Ask Claude Code questions that leverage the AWS Knowledge MCP Server tools exposed through the gateway

## Clean Up

The notebook includes cleanup cells to:

1. Delete the AgentCore Gateway and its targets
2. Remove the MCP server from Claude Code (`claude mcp remove my-tools-gw`)

Additional resources you may need to manually delete:
- IAM role and policies (`sample-claude-code-mcp-gateway`)
- Cognito user pool (`sample-agentcore-gateway-pool`)




