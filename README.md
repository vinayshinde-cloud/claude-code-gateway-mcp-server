# Integrate Claude Code with MCP using AWS AgentCore Gateway

## Overview

This project demonstrates how to integrate Claude Code with AWS AgentCore Gateway to build a centralized MCP (Model Context Protocol) server.

Instead of managing multiple MCP connections, all requests are routed through a single gateway. This reduces configuration complexity, optimizes context usage, and improves scalability in enterprise environments.

---

## Problem Statement

In multi-MCP environments, the following challenges are observed:

- Context window overhead: Each MCP server adds tool definitions, reducing space for model reasoning  
- Configuration sprawl: Developers must configure and authenticate against multiple MCP servers  

---

## Solution

AWS AgentCore Gateway acts as a centralized MCP server:

- Provides a single endpoint for MCP communication  
- Dynamically routes requests to backend MCP servers  
- Uses semantic search for tool selection  
- Integrates with OAuth authentication using Amazon Cognito  

---

## Architecture
