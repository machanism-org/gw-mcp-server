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

[![Download](https://img.shields.io/sourceforge/dt/machanism/machai%2Fgw-mcp-server%2Freleases)](https://sourceforge.net/projects/machanism/files/machai/gw-mcp-server/releases/) [![M8ven Score](https://m8ven.ai/badge/mcp/machanism-org/gw-mcp-server)](https://m8ven.ai/mcp/machanism-org/gw-mcp-server) [![bindex](https://img.shields.io/badge/bindex-blue.svg)](https://raw.githubusercontent.com/machanism-org/gw-mcp-server/refs/heads/main/bindex.json)

## Overview

Ghostwriter MCP Server is an executable Java 17 Model Context Protocol (MCP) server. The Maven project is named **Ghostwriter MCP Server** and publishes the `org.machanism.machai:gw-mcp-server` artifact. It combines Ghostwriter's project-aware code-assistance workflows with Bindex Core's metadata retrieval, registration, schema, and library-recommendation capabilities, and exposes them through the Machai MCP Server runtime.

The executable entry point is `org.machanism.machai.mcp.server.McpServer`. Launching the assembled JAR without a port uses STDIO; `--port` enables HTTP, and `--session` enables streamable HTTP sessions. The server can also receive a project directory, server name, advertised version, and properties configuration through the runtime command-line options. Its tools support project-aware assistance, model-assisted workflows, dependency metadata discovery, Bindex descriptor retrieval and registration, and library recommendations.

No `package-info.java` files are present in this source tree. This overview therefore uses the Maven project description, its declared Ghostwriter, Bindex Core, and Machai MCP Server dependencies, the executable entry point recorded in the project metadata, and the Bindex descriptor. Ghostwriter model-assisted workflows use the configured generative-AI provider; Bindex workflows use configured metadata sources and registry services.

![Ghostwriter MCP Server component diagram](./images/c4-diagram.png)

The diagram is maintained in [`src/site/puml/c4-diagram.puml`](../puml/c4-diagram.puml). It shows the MCP client, runtime and transport adapters, runtime configuration, Ghostwriter tools, Bindex Core services, the configured AI provider, and external metadata and registry systems.

## Supported AI providers

The project metadata identifies **CodeMie** as the provider used by Ghostwriter's model-assisted workflows. No other provider is declared in this module, so no additional provider is claimed as supported here. Configure provider credentials outside source control through the runtime properties or environment supplied by the Ghostwriter/Machai configuration system.

| Provider | Supported configuration |
| --- | --- |
| CodeMie | Set `genai.serverId=CodeMie` when selecting the provider and use the provider-qualified model `gw.model=CodeMie:gpt-5.6-luna-2026-07-09`. Supply the required CodeMie user credentials or OAuth client credentials through the runtime configuration; do not commit them to the project. |

### Common configuration parameters

The following values are documented by this project or its executable-server metadata. A value marked **not set** is intentionally selected at launch or by the runtime rather than hard-coded in this POM.

| Parameter | Description | Default or project value |
| --- | --- | --- |
| `gw.model` | Provider-qualified model identifier used for Ghostwriter model-assisted requests. | `CodeMie:gpt-5.6-luna-2026-07-09` (project metadata) |
| CodeMie credentials | User/password or OAuth client credentials required to authenticate with CodeMie. | No default; supply through runtime configuration or environment variables. |
| `--projectDir` / `-d` | Project directory supplied to project-aware server workflows. | Runtime-defined; commonly the current directory. |
| `--config` | Optional properties configuration file loaded before startup. | Runtime default when omitted; no project-specific file is committed. |
| `--name` / `-n` | Server name advertised by the MCP runtime. | Runtime-defined. |
| `--version` / `-v` | Version advertised by the MCP runtime. | Runtime-defined. |
| `--port` / `-p` | Selects HTTP server mode. | Not set: the server uses STDIO. |
| `--session` / `-s` | Enables streamable HTTP sessions when HTTP mode is active. | Disabled. |
| `maven.compiler.release` | Java release used to compile the artifact. | `17` |

Maven properties can be overridden with `-D` options. The optional `pack` profile assembles the runnable release JAR and writes it to the directory represented by `${env.MACHANISM_PACK_DIR}`. See the Machai MCP Server CLI documentation for the complete runtime option reference.

## Resources

- [Machai platform](https://machai.machanism.org/)
- [Machai GitHub repository](https://github.com/machanism-org/machai)
- [Ghostwriter MCP Server GitHub repository](https://github.com/machanism-org/gw-mcp-server)
- [Ghostwriter documentation](https://machai.machanism.org/ghostwriter/)
- [Bindex Core documentation](https://machai.machanism.org/bindex-core/)
- [Machai MCP Server CLI documentation](https://machai.machanism.org/machai-mcp-server/index.html#CLI)
- [Ghostwriter MCP Server artifact on Maven Central](https://central.sonatype.com/artifact/org.machanism.machai/gw-mcp-server)
- [Machai MCP Server artifact on Maven Central](https://central.sonatype.com/artifact/org.machanism.machai/machai-mcp-server)
- [Project Bindex descriptor](https://raw.githubusercontent.com/machanism-org/gw-mcp-server/refs/heads/main/bindex.json)
