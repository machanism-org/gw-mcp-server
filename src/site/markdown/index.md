<!-- @guidance:
Generate or update the content as follows.  
**Important:** If any section or content already exists, update it with the latest and most accurate information instead of duplicating or skipping it.
# Page Structure: 
1. Header
   - Project Title: need to use from pom.xml
   - [![Downloads](https://img.shields.io/sourceforge/dt/machanism/machai%2Fgw-mcp-server%2Freleases)](https://sourceforge.net/projects/machanism/files/machai/gw-mcp-server/releases/)
   - [![M8ven Score](https://m8ven.ai/badge/mcp/machanism-org/gw-mcp-server)](https://m8ven.ai/mcp/machanism-org/gw-mcp-server) 
   - Bindex Badge [![bindex](https://img.shields.io/badge/bindex-blue.svg)](https://raw.githubusercontent.com/machanism-org/[artifactId]/refs/heads/main/bindex.json)
# Overview
   - Full description the project based on package-info.java files in source folder..
   - Use the project structure diagram by the path: `./images/c4-diagram.png` (`src/site/puml/c4-diagram.puml`).
# Supported AI providers
   - Describe all supported AP providers with configurations.
   - Table of common configuration parameters, their descriptions, and default values.
# Resources
   - List of relevant links (platform, GitHub, Maven).
-->

# Ghostwriter MCP Server

[![Downloads](https://img.shields.io/sourceforge/dt/machanism/machai%2Fgw-mcp-server%2Freleases)](https://sourceforge.net/projects/machanism/files/machai/gw-mcp-server/releases/) [![M8ven Score](https://m8ven.ai/badge/mcp/machanism-org/gw-mcp-server)](https://m8ven.ai/mcp/machanism-org/gw-mcp-server) [![bindex](https://img.shields.io/badge/bindex-blue.svg)](https://raw.githubusercontent.com/machanism-org/gw-mcp-server/refs/heads/main/bindex.json)

## Overview

Ghostwriter MCP Server is an executable Java 17 Model Context Protocol (MCP) server. The Maven project is named **Ghostwriter MCP Server** and publishes the `org.machanism.machai:gw-mcp-server` artifact. It combines Ghostwriter's project-aware code-assistance workflows with Bindex Core's metadata retrieval, registration, schema, and library-recommendation capabilities, and exposes them through the Machai MCP Server runtime.

The executable entry point is `org.machanism.machai.mcp.server.McpServer`. Launching the assembled JAR without a port uses STDIO; `--port` enables HTTP, and `--session` enables streamable HTTP sessions. The server can also receive a project directory, server name, advertised version, and properties configuration through the runtime command-line options. Its tools support project-aware assistance, model-assisted workflows, dependency metadata discovery, Bindex descriptor retrieval and registration, and library recommendations.

No `package-info.java` files are present in this source tree. This overview therefore uses the Maven project description, its declared Ghostwriter, Bindex Core, and Machai MCP Server dependencies, the executable entry point recorded in the project metadata, and the Bindex descriptor. Ghostwriter model-assisted workflows use the configured generative-AI provider; Bindex workflows use configured metadata sources and registry services.

![Ghostwriter MCP Server component diagram](./images/c4-diagram.png)

The diagram source is `src/site/puml/c4-diagram.puml`. It shows the packaged server and its relationships with MCP clients, the configured AI provider, library metadata sources, and the Bindex registry. This module has no local Java components; the runtime and tools are supplied by its dependencies.

## Supported AI providers

Ghostwriter uses the shared [Machai GenAI Client](https://machai.machanism.org/genai-client/) provider layer. Its built-in external AI providers are **OpenAI**, **Anthropic**, and **CodeMie**; **Tools** and **None** implement the same lifecycle without an external AI service. Select a provider using `gw.model=Provider:Model` in a protected `gw.properties` file, or supply the corresponding runtime environment setting (`gw_model`). Keep credentials out of source control. The CodeMie model in `bindex.json` is an example from older build metadata, not a model default declared by this module's current POM.

| Provider | Supported configuration |
| --- | --- |
| OpenAI | For example, `gw.model=OpenAI:gpt-4o-mini`; supply `OPENAI_API_KEY`. Set `OPENAI_BASE_URL` for an OpenAI-compatible gateway. Supports text generation, function tools, embeddings, and optional web search and MCP forwarding. |
| Anthropic | Select an available Claude model using `gw.model=Anthropic:MODEL`; supply `ANTHROPIC_API_KEY` (API key or authorization token) and optionally `ANTHROPIC_BASE_URL`. Supports text generation, function-tool loops, web search, MCP forwarding, and prompt caching. |
| CodeMie | For example, `gw.model=CodeMie:gpt-4o-mini`; supply `GENAI_USERNAME` and `GENAI_PASSWORD`, with optional `AUTH_URL`. A username containing `@` selects the password grant; other usernames select the client-credentials grant (client ID and secret). CodeMie supplies the delegated endpoint and bearer token: `gpt-`, `gemini-`, `text-embedding-`, `codemie-text-embedding-`, and `amazon.titan-embed-text-` model prefixes use OpenAI-compatible delegation; `claude-` uses Anthropic. Unsupported prefixes fail initialization. Actual model availability depends on the service. |
| Tools (local, not an AI service) | `gw.model=Tools:yaml` invokes registered host callbacks from a YAML tool-call descriptor containing `tool` and optional `params`. Requires registered callbacks and a prompt, but no external credentials. |
| None (no-op, not an AI service) | `gw.model=None:log` logs lifecycle activity; `None:disabled` is silent. Neither makes model requests or requires credentials. |

### Common configuration parameters

Provider defaults below come from the shared GenAI Client documentation; server values come from the runtime documentation and this module's `src/main/resources/mcp-server.properties`. Model names in examples must be available to your account.

| Parameter | Description | Default or project value |
| --- | --- | --- |
| `gw.model` | Provider-qualified model identifier used for Ghostwriter model-assisted requests. | No model declared in this module's POM or bundled properties; configure explicitly. |
| `OPENAI_API_KEY` | OpenAI or compatible-service API key. | No default; required for direct OpenAI access. |
| `OPENAI_BASE_URL` | OpenAI-compatible API endpoint override. | OpenAI SDK default. |
| `ANTHROPIC_API_KEY` | Anthropic API key or authorization token. | No default; required for direct Anthropic access. |
| `ANTHROPIC_BASE_URL` | Anthropic-compatible API endpoint override. | Anthropic SDK default. |
| `GENAI_USERNAME` / `GENAI_PASSWORD` | CodeMie user/password or OAuth client ID/secret. | No credential defaults; required for CodeMie. |
| `AUTH_URL` | CodeMie OpenID Connect token endpoint override. | CodeMie default endpoint. |
| `GENAI_TIMEOUT` | Request timeout in seconds; zero leaves the SDK default in effect. | `0` |
| `MAX_OUTPUT_TOKENS` | Maximum generated tokens. | `18000` |
| `MAX_TOOL_CALLS` | OpenAI Responses API tool-call limit; zero leaves it unset. Does not limit Anthropic tool loops. | `0` |
| `WebSearchTool.type` | Enables provider-specific web search; supported types depend on the provider. | Not set. |
| `WebSearchTool.city` / `WebSearchTool.country` / `WebSearchTool.region` | Optional web-search location hints. | Not set. |
| `MCP.name` / `MCP.url` | Name and URL of an MCP server forwarded to the AI provider; a name is required to register a group. Additional groups use `MCP_1.*`, `MCP_2.*`, etc. | Not set. |
| `MCP.authorization` / `MCP.description` | Optional forwarded MCP authorization and description; numbered groups are also supported. | Not set. |
| `custom_configuration` | Additional tool configuration file selected by the bundled server properties. | `gw.properties` |
| `--projectDir` / `-d` | Project directory supplied to project-aware server workflows. | Not set; HTTP mode can determine it from a client request. Set explicitly for predictable project context. |
| `--config` / `-c` | Server properties configuration file loaded before startup. | `mcp.properties`; absence of the default is tolerated, but an explicitly selected file must be readable. |
| `--name` / `-n` | Server name advertised by the MCP runtime. | This module supplies `mcp.server.name=gw-mcp-server` through Maven-filtered resources. |
| `--version` / `-v` | Version advertised by the MCP runtime. | This module supplies its Maven project version through Maven-filtered resources (currently `1.4.3-SNAPSHOT`). |
| `--port` / `-p` | Selects HTTP server mode. | Not set: the server uses STDIO. |
| `--session` / `-s` | Enables streamable HTTP sessions when HTTP mode is active. | Disabled. |
| `maven.compiler.release` | Java release used to compile the artifact. | `17` |

Maven properties can be overridden with `-D` options. The optional `pack` profile assembles the runnable release JAR and writes it to the directory represented by `${env.MACHANISM_PACK_DIR}`. See the Machai MCP Server CLI documentation for the complete runtime option reference.

## Resources

- [Machai platform](https://machai.machanism.org/)
- [Machai GitHub repository](https://github.com/machanism-org/machai)
- [Ghostwriter MCP Server GitHub repository](https://github.com/machanism-org/gw-mcp-server)
- [Ghostwriter documentation](https://machai.machanism.org/ghostwriter/)
- [GenAI Client provider and configuration documentation](https://machai.machanism.org/genai-client/)
- [Bindex Core documentation](https://machai.machanism.org/bindex-core/)
- [Machai MCP Server CLI documentation](https://machai.machanism.org/machai-mcp-server/index.html#CLI)
- [Ghostwriter MCP Server artifact on Maven Central](https://central.sonatype.com/artifact/org.machanism.machai/gw-mcp-server)
- [Machai MCP Server artifact on Maven Central](https://central.sonatype.com/artifact/org.machanism.machai/machai-mcp-server)
- [Project Bindex descriptor](https://raw.githubusercontent.com/machanism-org/gw-mcp-server/refs/heads/main/bindex.json)
