# LangGraph Agent with Bedrock AgentCore Integration

| Information         | Details                                                                      |
|---------------------|------------------------------------------------------------------------------|
| Agent type          | Synchronous                                                                 |
| Agentic Framework   | Langgraph                                                                    |
| LLM model           | Anthropic Claude 3 Haiku                                                     |
| Components          | AgentCore Runtime                                |
| Example complexity  | Easy                                                                 |
| SDK used            | Amazon BedrockAgentCore Python SDK                                           |

This example demonstrates how to integrate a LangGraph agent with AWS Bedrock AgentCore, enabling you to deploy a web search-capable agent as a managed service.

## Prerequisites

- Python 3.10+
- [uv](https://github.com/astral-sh/uv) - Fast Python package installer and resolver
- AWS account with Bedrock access


The agent will:
1. Process your query
2. Use DuckDuckGo to search for relevant information
3. Provide a comprehensive response based on the search results


## How It Works

This agent uses LangGraph to create a directed graph for agent reasoning:

1. The user query is sent to the chatbot node
2. The chatbot decides whether to use tools based on the query
3. If tools are needed, the query is sent to the tools node
4. The tools node executes the search and returns results
5. Results are sent back to the chatbot for final response generation

The Bedrock AgentCore framework handles deployment, scaling, and management of the agent in AWS.

## Additional Resources

- [LangGraph Documentation](https://github.com/langchain-ai/langgraph)
- [LangChain Documentation](https://python.langchain.com/docs/get_started/introduction)
- [Bedrock AgentCore Documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-core.html)
