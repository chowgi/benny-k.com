---
title: "Building Intelligent Agents with LangGraph and MongoDB: A Deep Dive into Vector-Powered Tool Discovery"
date: 2025-09-25
draft: false
tags: ["AI", "LangGraph", "MongoDB", "vector-search", "agents", "RAG", "VoyageAI", "Python"]
cover:
  image: "tool-discovery-2.png"
  relative: true
---

When building AI agents, one of the biggest challenges is tool selection—especially as the number of available tools increases. Evidence from OpenAI's developer community and technical documentation highlights that agents' tool selection accuracy tends to drop noticeably with larger toolsets, sometimes causing irrelevant or random tools to be invoked even with carefully designed prompts. OpenAI's internal discussions note a clear decline in single-tool call accuracy and an uptick in failure modes as the system prompt and available tool list grows, with developer complaints of incorrect or unexpected tool choices despite best practices.

Anthropic's recent publications echo this reality, noting that even strong retrieval models and non-deterministic agent selection pipelines are challenged by tool overload. Their reports show that retrieval accuracy with large tool inventories routinely drops below 35% when using traditional information retrieval methods, and overall agent task success rates fall by 10-20% in realistic settings. Anthropic's engineering retrospectives stress that as tool redundancy and overlap increase, traditional rule-based or keyword-matching approaches for tool routing simply can't keep up.

This is why more intelligent approaches are needed. I explored using vector search to semantically match user queries with the most relevant tools, rather than relying on static logic or hand-curated routes. With MongoDB providing both a memory backbone and a high-performance vector search engine, my LangGraph-based agent dynamically selects the most appropriate tools from large collections based on semantic context—not just surface cues. This unlocked a dramatic improvement in tool choice accuracy, especially as the available toolset scaled, allowing the system to maintain robust conversational memory and offer context-aware document retrieval at scale.

![Tool discovery architecture overview](tool-discovery-1.png)

### The Architecture: MongoDB as the Unified Memory Layer

The core innovation here is using MongoDB as a unified memory layer that serves multiple purposes:

- **Conversation Persistence:** LangGraph's MongoDBSaver checkpointer stores conversation state
- **Tool Registry:** Tools are stored with embeddings for semantic discovery
- **Document Knowledge Base:** Policy documents with vector search capabilities
- **Operational Data:** Orders, returns, and other business data

The system uses four main collections: tools (with embeddings), orders (business data), returns (return requests), and policies (document chunks with vector search).

![Tool discovery pipeline](tool-discovery.png)

#### Key Learning #1: Vector Search for Dynamic Tool Discovery

The most exciting part of this project was implementing semantic tool discovery. Instead of hardcoding which tools to use, the agent uses vector search to find the most relevant tools based on the user's query.

**How It Works**

- **Tool Registration:** Each tool is stored in MongoDB with its description and a vector embedding
- **Query Processing:** User queries are converted to embeddings using VoyageAI's contextualized embeddings
- **Semantic Matching:** Vector search finds the most semantically similar tools
- **Dynamic Tool Loading:** Only the relevant tools are loaded into the LangGraph agent

The system uses MongoDB's vector search capabilities to perform semantic matching between user queries and tool descriptions, returning only the most relevant tools for each interaction.

#### Key Learning #2: LangGraph's State Management is Powerful

Working with LangGraph taught me several important lessons about building conversational agents:

##### State Persistence is Seamless

LangGraph's checkpointing system makes conversation persistence trivial. The agent can continue conversations from where they left off, even after restarts, with no additional state management code required.

##### Context Integration is Key

One of the most interesting challenges was incorporating conversation context into tool selection. The agent doesn't just use the current query—it considers the entire conversation history to create enhanced queries that include relevant context.

This means that if a user says "How long do I have to return it?" after discussing a specific order, the agent understands "it" refers to that order by incorporating the conversation context into the tool selection process.

##### Tool Execution is Transparent

LangGraph's tool execution model makes it easy to see exactly what the agent is doing at each step, providing clear visibility into the decision-making process.

#### Key Learning #3: Advanced Document Retrieval with Reranking

For the policy document search, I implemented a sophisticated retrieval system that combines vector search with reranking. This two-stage approach (vector search + reranking) provides much better results than either method alone.

The system first uses MongoDB's vector search to find candidate documents, then applies VoyageAI's rerank-2.5 model to refine the results for maximum precision.

##### The Cool Use Cases

###### 1. Dynamic Tool Selection

I create a bunch of unrelated tools to showcase correct tool selection. The agent can handle completely different types of queries without any code changes.

- **Customer Service:** "I want to return my laptop from order 101"
- **General Utility:** "What time is it in Tokyo?"
- **Mathematical:** "What's the square root of 144?"
- **Entertainment:** "Roll 3 dice with 12 sides each"

Each query automatically selects the most relevant tools from the entire registry.

###### 2. Context-Aware Conversations

The agent maintains context across multiple turns:

> User: "I want to return my laptop from order 101"
> Agent: [Looks up order, finds laptop details, explains return process]
> User: "How long do I have to return it?"
> Agent: [Understands "it" refers to the laptop, searches return policy]

###### 3. Intelligent Document Retrieval

When users ask about policies, the agent doesn't just return generic information—it finds the most relevant sections using semantic search and reranking to provide precise, contextual answers.

### Lessons Learned

##### Context is Everything

The most important lesson was that context dramatically improves tool selection. A query like "return it" is meaningless without context, but with conversation history, it becomes actionable.

##### Vector Search is More Powerful Than Keywords

Semantic matching found relevant tools that keyword matching would miss. For example, "refund" queries correctly matched tools about returns and order lookups.

##### LangGraph's State Management is Elegant

The checkpointing system made conversation persistence trivial. The agent could handle complex multi-turn interactions without any additional state management code.

##### MongoDB as a Unified Platform

Using MongoDB for everything (conversations, tools, documents, data) simplified the architecture and provided a single source of truth.

#### The Result: An Intelligent, Context-Aware Agent

The final agent can:

- Dynamically select tools based on semantic similarity
- Maintain conversations across sessions with full context
- Retrieve relevant documents using advanced vector search
- Handle complex multi-turn interactions seamlessly
- Scale to hundreds of tools without performance degradation

#### What's Next?

This project opened up several exciting possibilities:

- **Tool Learning:** The agent could learn from user interactions to improve tool selection
- **Multi-Modal Tools:** Extending to tools that work with images, audio, or other data types
- **Tool Composition:** Automatically combining multiple tools for complex tasks
- **Real-Time Tool Updates:** Dynamically adding new tools without restarting the agent

### Conclusion

Building this agent taught me that the future of AI agents isn't about having more tools—it's about having smarter tool selection. By combining vector search with LangGraph's state management, we can build agents that are both powerful and contextually aware.

The key insight is that semantic understanding beats rule-based routing every time. When users can express their needs naturally, and the agent can understand and respond appropriately, we get closer to truly intelligent systems.

That said, it's worth noting that MCP (Model Context Protocol) kind of solves for the tool selection problem in a more standardized way. This vector-based approach might not be widely adopted given MCP's emergence, but it was incredibly fun to build and experiment with regardless of what the future holds for MCP.

---

This project demonstrates how modern AI tooling (LangGraph, MongoDB, VoyageAI) can be combined to create sophisticated, context-aware agents. The complete code is [available on GitHub](https://github.com/chowgi/mongodb_as_memory_for_langraph_agent_demo) for exploration and adaptation to your own use cases.
