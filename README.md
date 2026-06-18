# Awesome Generative UI [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of resources for AI-generated user interfaces — systems where LLMs dynamically create, compose, and render UI components.

Generative UI represents a paradigm shift from "AI assists coding" to "AI generates interfaces directly." Instead of writing components, you describe what you need and the AI produces a working UI.

## Contents

- [What is Generative UI](#what-is-generative-ui)
- [Key Concepts](#key-concepts)
- [Research](#research)
- [Benchmarks & Datasets](#benchmarks--datasets)
- [Specifications & Protocols](#specifications--protocols)
- [Frameworks & SDKs](#frameworks--sdks)
- [Tools & Platforms](#tools--platforms)
- [Security & Sandboxing](#security--sandboxing)
- [Evaluation & Testing](#evaluation--testing)
- [Schemas & Patterns](#schemas--patterns)
- [Component Libraries](#component-libraries)
- [Examples & Demos](#examples--demos)
- [Articles & Talks](#articles--talks)
- [Related Concepts](#related-concepts)

---

## What is Generative UI

**Generative UI** is the practice of using AI models (typically LLMs) to dynamically generate user interface components at runtime, rather than having developers hand-code every interface.

### The Spectrum

| Dimension             | Traditional UI                  | AI-Assisted UI                        | Generative UI                             |
| --------------------- | ------------------------------- | ------------------------------------- | ----------------------------------------- |
| Primary workflow      | Developers hand-code components | Copilot helps developers write code   | Model generates UI components from intent |
| Developer involvement | High (author everything)        | High (developer in the loop)          | Lower per UI (developer sets constraints) |
| Runtime behavior      | Static UI                       | Static UI (generation happens in dev) | Dynamic / runtime-generated UI            |
| Typical output        | Source code                     | Source code                           | Components, schemas, or rendered UI       |
| Best for              | Predictability, control         | Faster delivery with human oversight  | Personalization and rapid UI composition  |

### Why It Matters

Generative UI enables **personalization at scale**, **rapid prototyping**, and **natural language interfaces** (e.g., "Show me sales by region" produces an actual chart), while reducing development time by shifting effort from implementation details to intent.

### Challenges

Challenges include **Consistency** (generated UIs may look different each time), **Performance** (generation takes time; streaming helps), **Safety** (arbitrary code generation has security implications), and **Hallucination** (AI might generate components that don't exist).

---

## Key Concepts

### Component Registry
A predefined set of UI components the AI can use. Instead of generating arbitrary HTML/CSS, the AI selects from known, tested components.

```
Registry: [BarChart, LineChart, DataTable, KPICard]
Prompt: "Show monthly revenue"
Output: { component: "BarChart", props: { data: [...] } }
```

### Query Registry
A predefined set of data sources the AI can query. Prevents hallucinated API calls.

```
Registry: [getUsers, getRevenue, getOrders]
Prompt: "Show top customers"
Output: { query: "getUsers", params: { sort: "revenue", limit: 10 } }
```

### Streaming UI
Rendering UI components as they're generated, rather than waiting for complete output.

### Server Components
React Server Components enable streaming UI from server to client, making generative UI more practical.

### Structured Output
Using JSON schemas or function calling to ensure AI outputs valid, parseable UI descriptions.

### Constrained vs Unconstrained Generation

A fundamental design decision in generative UI systems:

| Approach          | Description                           | Examples                                   |
| ----------------- | ------------------------------------- | ------------------------------------------ |
| **Constrained**   | AI selects from registered components | Vercel AI SDK `streamUI()`, Tambo, v0.dev  |
| **Unconstrained** | AI generates raw HTML/CSS/JS directly | Google GenUI, Claude Artifacts, MCP Apps   |

**Constrained systems** are safer and more consistent — the AI can only use pre-approved components with known behavior. Trade-off: less flexible, requires component development upfront.

**Unconstrained systems** are more powerful — the AI can create anything. Trade-off: needs sandboxing (iframe, E2B), potential for inconsistency, and security considerations.

Google's research (see below) suggests unconstrained generation may become the dominant approach as models improve. They found users preferred AI-generated HTML/CSS over markdown 83% of the time, calling it an "emergent capability" — models produce good UIs without UI-specific training.

---

## Research

### Papers

- [Generative UI: LLMs are Effective UI Generators](https://generativeui.github.io/static/pdfs/paper.pdf) - Google (2025). Foundational paper making the case for unconstrained UI generation. Key findings: (1) Users preferred AI-generated HTML/CSS/JS over markdown 83% of the time, (2) UI generation is an "emergent capability" requiring no UI-specific training, (3) Vision of "infinite ephemeral interfaces" - every query gets a custom UI. ([Project page](https://generativeui.github.io/)).
- [WebArena: A Realistic Web Environment for Building Autonomous Agents](https://arxiv.org/abs/2307.13854) - CMU (2023). Framework for evaluating web agents, relevant to UI generation and interaction.
- [Pix2Struct: Screenshot Parsing as Pretraining for Visual Language Understanding](https://arxiv.org/abs/2210.03347) - Google (2023). Vision-language model pretrained on web screenshots. Foundation for screenshot-to-code approaches.

*Note: Generative UI is an emerging field. Much knowledge lives in blog posts, SDKs, and industry practice rather than academic papers.*

---

## Benchmarks & Datasets

*Benchmarks for evaluating UI generation and datasets that enable screenshot-to-code, layout understanding, and web-agent interaction.*

- [Design2Code](https://arxiv.org/abs/2403.03163) - Benchmark for converting designs/screenshots into front-end code (Stanford/Google, 2024).
- [WebArena](https://github.com/web-arena-x/webarena) - Realistic web environment and benchmark for agents interacting with live websites.
- [VisualWebArena](https://github.com/web-arena-x/visualwebarena) - Vision-grounded WebArena variant for UI understanding and interaction.
- [Mind2Web](https://github.com/OSU-NLP-Group/Mind2Web) - Dataset and benchmark for generalist web agents grounded in real webpages.
- [MiniWoB++](https://github.com/Farama-Foundation/miniwob-plusplus) - Standard suite of web UI interaction tasks used for agent evaluation.
- [RICO](https://www.interactionmining.org/archive/rico) - Large-scale dataset of mobile app UIs (screens + view hierarchies).
- [pix2code](https://arxiv.org/abs/1705.07962) - Early reference paper on UI screenshot-to-code generation (Tony Beltramelli, 2017).

---

## Specifications & Protocols

### Open Specifications

- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/) - Anthropic. Protocol for connecting AI models to external data sources and tools.
- [Model Context Protocol (MCP) Servers](https://github.com/modelcontextprotocol/servers) - Reference implementations of MCP servers and tooling.
- [Model Context Protocol (MCP) Apps / UI Extensions](https://github.com/modelcontextprotocol/ext-apps) - SDK for the upcoming MCP Apps / UI extension.

---

## Frameworks & SDKs

### Generative UI Frameworks

Purpose-built for streaming AI-generated interfaces:

- [Vercel AI SDK](https://sdk.vercel.ai/) - Vercel. TypeScript toolkit with `streamUI()` for server-streamed components and multi-provider support.
- [Tambo](https://github.com/tambo-ai/tambo) - Generative UI SDK for React, purpose-built for streaming AI-generated components.
- [Hashbrown](https://github.com/liveloveapp/hashbrown) - Framework for building generative user interfaces in Angular and React.
- [Cuttlekit](https://cuttlekit.com) - Fully generative UI framework, framework agnostic, optimised for performance and real-time UI generation.
- [mdocUI](https://github.com/mdocui/mdocui) — Streaming generative UI using Markdoc `{% %}` tag syntax. Framework-agnostic core with React renderer, 24 theme-neutral components, and Zod schema validation.

### Supporting Libraries

Building blocks for reliable generation:

- [Instructor](https://github.com/jxnl/instructor) - Structured output extraction; useful with UI schemas for reliable generation.
- [Mitosis](https://github.com/BuilderIO/mitosis) - Builder.io. Write components once and compile to React/Vue/Svelte/etc.

---

## Tools & Platforms

### Component Generators

*"Describe a component, get code."*

- [v0.dev](https://v0.dev/) - Vercel (commercial). Generate React/Tailwind components from prompts; known for shadcn/ui integration.
- [OpenUI](https://github.com/wandb/openui) - Weights & Biases. Describe UI in natural language and see it rendered live.
- [openv0](https://github.com/raidendotai/openv0) - Open source v0 clone that generates React/Tailwind components from prompts.
- [Roblox GUI Maker](https://robloxguimaker.dev/) - Generate Roblox Studio ScreenGui layouts, HUDs, menus, and Lua UI starter code from prompts.

### App Builders

*"Describe an app, get a deployable project."*

- [Bolt.new](https://bolt.new/) - StackBlitz (commercial). Generate full-stack applications in-browser with live preview.
- [Lovable](https://lovable.dev/) - Generate and deploy web apps from descriptions (commercial).
- [Cofounder](https://github.com/raidendotai/cofounder) - Open source full-stack app generator with generative UI, databases, and APIs.

### Design-to-Code

*"Convert visuals to working code."*

- [Screenshot-to-Code](https://github.com/abi/screenshot-to-code) - Convert screenshots or designs to HTML/React/Vue code.
- [tldraw make-real](https://makereal.tldraw.com/) - Turn a wireframe into a working React component.
- [Galileo AI](https://www.usegalileo.ai/) - Generate editable UI designs from text descriptions (commercial, Figma-compatible).

### MCP Tools for UI

*"Model Context Protocol servers that improve AI UI generation."*

- [Figma-Context-MCP](https://github.com/GLips/Figma-Context-MCP) - GLips. Provides Figma layout information to AI coding agents.
- [21st.dev Magic MCP](https://github.com/21st-dev/magic-mcp) - 21st.dev. Generate UI components from prompts with design-system awareness.
- [shadcn-ui-mcp-server](https://github.com/Jpisnice/shadcn-ui-mcp-server) - Jpisnice. Helps LLMs understand shadcn/ui component structure.
- [MUI MCP](https://mui.com/x/introduction/mcp/) - MUI. MCP server for MUI docs and code examples, published as `@mui/mcp` ([npm](https://www.npmjs.com/package/@mui/mcp)).
- [Storybook MCP](https://github.com/storybookjs/mcp) - Storybook. MCP server and addon that exposes component information and workflows from your local Storybook ([npm](https://www.npmjs.com/package/@storybook/mcp)).
- [Cursor Talk to Figma MCP](https://github.com/grab/cursor-talk-to-figma-mcp) - Grab. MCP server + Figma plugin for reading and modifying Figma designs.
- [Playwright MCP](https://github.com/microsoft/playwright-mcp) - Microsoft. MCP server for browser automation and UI regression testing workflows.
- [shadcn-vue-mcp](https://github.com/HelloGGX/shadcn-vue-mcp) - HelloGGX. MCP server for shadcn-vue component knowledge.

### AI Products with UI Generation

*"Chat interfaces that can generate UIs."*

- [Claude](https://claude.ai/) - Anthropic (commercial). Artifacts generates interactive React/HTML UIs in chat.
- [ChatGPT](https://chatgpt.com/) - OpenAI (commercial). Canvas supports UI generation and editing.

---

## Security & Sandboxing

*When UIs (or UI code) are model-generated, treat outputs as untrusted. These resources cover browser sandboxing, sanitization, and LLM-app security pitfalls.*

- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) - OWASP. Threat model checklist for LLM apps (prompt injection, insecure tool use, data leakage).
- [OWASP Prompt Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html) - OWASP. Practical mitigations for prompt/tool injection in production systems.
- [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) - MDN. Core browser control for limiting script execution and exfiltration.
- [iframe sandbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/iframe) - MDN. Reference for sandboxed iframes to contain untrusted HTML/JS UI previews.
- [Trusted Types](https://web.dev/articles/trusted-types) - Mitigation against DOM XSS when inserting generated HTML into the DOM.
- [DOMPurify](https://github.com/cure53/DOMPurify) - Widely used HTML sanitizer for untrusted/generated markup.
- [Sandpack](https://github.com/codesandbox/sandpack) - CodeSandbox. In-browser code sandboxing patterns for safe previews.
- [E2B](https://github.com/e2b-dev/e2b) - Sandboxed code execution environments for running untrusted generated code.

---

## Evaluation & Testing

*Tools and practices for regression testing generated UIs, validating structured outputs, and red-teaming LLM apps.*

- [Playwright](https://playwright.dev/) - Microsoft. E2E testing and screenshot diffs for UI regression testing.
- [Storybook Test Runner](https://storybook.js.org/docs/writing-tests/integrations/test-runner) - Storybook. Automates component-level interaction testing.
- [promptfoo](https://github.com/promptfoo/promptfoo) - Prompt and tool-call regression testing across models.
- [PyRIT](https://github.com/Azure/PyRIT) - Microsoft. Red teaming toolkit for LLM apps (jailbreaks, prompt injection).
- [garak](https://github.com/NVIDIA/garak) - NVIDIA. LLM vulnerability scanner for automated probing.

---

## Schemas & Patterns

*Make model outputs reliable: constrain generation, validate payloads, and standardize tool/component contracts.*

- [JSON Schema](https://json-schema.org/) - Standard for validating structured model outputs.
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) - OpenAPI Initiative. Standard contract format for APIs and tool catalogs.
- [Zod](https://github.com/colinhacks/zod) - Colin Hacks. TypeScript schema validation for enforcing output shapes at runtime.
- [Outlines](https://github.com/dottxt-ai/outlines) - Structured generation with JSON/grammar constraints.
- [Guidance](https://github.com/guidance-ai/guidance) - Grammar- and constraint-oriented prompting for structured outputs.
- [Vercel AI SDK](https://sdk.vercel.ai/docs) - Vercel. Streaming UI and tool-calling patterns for generative UI applications.
- [shadcn/ui Registry](https://ui.shadcn.com/docs/registry) - Component registry pattern commonly targeted by UI generators.

---

## Component Libraries

### Generation Targets

*Libraries that AI generators commonly output to:*

- [shadcn/ui](https://ui.shadcn.com/) - Copy-paste components that many generators target.
- [Radix UI](https://www.radix-ui.com/) - Radix. Unstyled primitives that shadcn/ui builds on.
- [Tremor](https://tremor.so/) - Dashboard components often used for generated data UIs.
- [React Native Paper](https://reactnativepaper.com/) - Material Design components for React Native.

### For Building AI/Chat Interfaces

*Components designed for LLM-powered apps:*

- [LangUI](https://github.com/LangbaseInc/langui) - LangbaseInc. Tailwind components for chat, AI assistants, and LLM projects.
- [GPT-Vis](https://github.com/antvis/GPT-Vis) - AntV. Visualization components designed for LLM-generated outputs.

### Visualization

*For AI-generated charts and data displays:*

- [Recharts](https://recharts.org/) - Composable React charts commonly used in dashboards.
- [Vega-Lite](https://vega.github.io/vega-lite/) - Vega. Declarative JSON grammar for charts and visualizations.
- [Observable Plot](https://observablehq.com/plot/) - Observable. Grammar of graphics for JS with concise specs.
- [D3.js](https://d3js.org/) - D3. Low-level but powerful library for custom visualizations.

---

## Examples & Demos

### Open Source

- [ai-chatbot](https://github.com/vercel/ai-chatbot) - Vercel. Full-featured chatbot with generative UI using the AI SDK.
- [morphic](https://github.com/miurla/morphic) - AI-powered search engine with generative answer UI.
- [langgraphjs-gen-ui-examples](https://github.com/langchain-ai/langgraphjs-gen-ui-examples) - LangChain. Collection of generative UI agents built with LangGraph.js.
- [gen-ui](https://github.com/bracesproul/gen-ui) - Reference implementation for generative UI with LangChain.js, AI SDK, and Next.js.
- [termite](https://github.com/shobrook/termite) - Terminal-based generative UI.

---

## Articles & Talks

- [Vercel: Introducing AI SDK 3.0 with Generative UI](https://vercel.com/blog/ai-sdk-3-generative-ui) - Vercel blog post introducing Generative UI features in the AI SDK.
- [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) - Anthropic engineering post on agent design patterns relevant to tool-using UI systems.

---

## Related Concepts

### Adjacent Fields

Design-to-code converts designs (e.g., Figma) to code. Code generation is the broader field of AI-generated code. Conversational UI covers chat interfaces (related but distinct). Low-code/no-code includes visual builders that generative UI can automate.

- [LangChain.js](https://js.langchain.com/) - LangChain. LLM framework often used to build tool-using agents that can drive generative UI.
- [Mastra](https://mastra.ai/) - TypeScript framework for AI applications with patterns adaptable to generative UI.

### Evaluation tooling (adjacent)

- [OpenAI Evals](https://github.com/openai/evals) - OpenAI. Framework for evaluating model outputs with custom tasks and graders.
- [TruLens](https://github.com/truera/trulens) - TruEra. Evaluation and feedback tooling for LLM applications.
- [LangSmith Evaluation](https://docs.smith.langchain.com/evaluation) - LangChain. Evaluation patterns and workflows for LLM apps.

### Standards & Formats

JSON Schema defines valid UI structures. OpenAPI describes APIs for query registries. Web Components are a framework-agnostic component standard.

---

## Contributing

Contributions welcome! Please read the [contribution guidelines](CONTRIBUTING.md) first.

### Criteria for Inclusion

Submissions should be relevant to generative UI (not general AI/LLM), high quality (well-documented and maintained or historically significant), and accessible (open source, free tier, or detailed documentation).

### What We're Looking For

New frameworks and tools, research papers and articles, interesting examples and demos, and corrections/updates.

---

License: CC BY 4.0 (see [LICENSE](LICENSE)).
