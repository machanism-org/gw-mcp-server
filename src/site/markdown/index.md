<!-- @guidance:
Generate or update the content as follows.  
**Important:** If any section or content already exists, update it with the latest and most accurate information instead of duplicating or skipping it.
# Page Structure: 
1. Header
   - Project Title: need to use from pom.xml
   - Maven Central Badge ([![Maven Central](https://img.shields.io/maven-central/v/[groupId]/[artifactId].svg)](https://central.sonatype.com/artifact/[groupId]/[artifactId])
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

[![Maven Central](https://img.shields.io/maven-central/v/org.machanism.machai/gw-mcp-server.svg)](https://central.sonatype.com/artifact/org.machanism.machai/gw-mcp-server)
[![bindex](https://img.shields.io/badge/bindex-blue.svg)](https://raw.githubusercontent.com/machanism-org/gw-mcp-server/refs/heads/main/bindex.json)

## Overview

Ghostwriter MCP Server is an executable Java 17 Model Context Protocol (MCP) server. It combines Ghostwriter's project-aware code-assistance workflows with Bindex Core's library-metadata retrieval, registration, and recommendation capabilities, and publishes those capabilities through the Machai MCP Server runtime. MCP clients, IDE integrations, and developer-automation processes can launch the assembled JAR locally over STDIO or connect to the server's HTTP mode.

The `org.machanism.machai.mcp.server.McpServer` entry point starts the unified server. With no port option it uses STDIO; `--port` enables HTTP mode, and `--session` enables streamable HTTP sessions. The server can also receive a project directory, server name, advertised version, and properties configuration through the runtime's command-line options. Its tools support project-aware assistance, model-assisted workflows, dependency metadata discovery, Bindex descriptor retrieval and registration, and library recommendations. The POM declares the Ghostwriter, Bindex Core, and Machai MCP Server runtime modules that provide these capabilities.

The source tree contains no `package-info.java` files. Accordingly, this overview is based on the Maven project description, declared dependencies, the executable entry point, and the project's Bindex metadata. Ghostwriter model-assisted workflows use the configured generative-AI provider; Bindex workflows use the configured metadata sources and registry services.

![Ghostwriter MCP Server component diagram](./images/c4-diagram.png)

The diagram is maintained in [`src/site/puml/c4-diagram.puml`](../puml/c4-diagram.puml). It shows the MCP client, Machai MCP Server runtime, Ghostwriter tools, Bindex Core services, the configured AI provider, and the external metadata and registry systems with which they interact.

## Supported AI providers

This project sets **CodeMie** as its configured AI provider. Provider integration is delegated to the Ghostwriter and Machai runtime dependencies rather than implemented in this module. The POM therefore documents CodeMie as the project's configured provider; other providers are available only when supported and configured by the selected underlying runtime. Their provider identifiers, model names, credentials, and endpoint properties must be obtained from that runtime's documentation.

| Provider | Status | Configuration |
| --- | --- | --- |
| CodeMie | Configured by this project | Set `genai.serverId=CodeMie` and `gw.model=CodeMie:gpt-5.5-2026-04-24`. Supply credentials and any endpoint settings through the Ghostwriter/Machai runtime configuration. |
| Other runtime-supported providers | Deployment-defined | Replace `genai.serverId` and `gw.model` with values supported by the selected runtime provider, then configure its required credentials and connection properties. This module does not declare a separate adapter or provider list. |

### Common configuration parameters

| Parameter | Description | Default value in this project |
| --- | --- | --- |
| `genai.serverId` | Generative-AI provider identifier used by Ghostwriter. | `CodeMie` |
| `gw.model` | Provider-qualified model identifier used for Ghostwriter model requests. | `CodeMie:gpt-5.5-2026-04-24` |
| Provider credentials and endpoint settings | Authentication, service URL, and other provider-specific connection values required by the selected runtime provider. | Not set by this module; configure them in the runtime/provider configuration. |
| `maven.compiler.release` | Java release used to compile the server artifact. | `17` |
| `--projectDir` / `-d` | Project directory supplied to project-aware server workflows. | Runtime-defined; commonly the current project directory. |
| `--port` / `-p` | Port that selects HTTP server mode. | Not set; no port starts STDIO mode. |
| `--session` / `-s` | Enables streamable sessions when HTTP mode is active. | Disabled. |

Maven properties can be overridden with `-D` options or in the Maven configuration used to build or run the server. Keep provider credentials out of the POM and source control. See the Machai MCP Server runtime documentation for the complete provider and command-line configuration reference.

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
