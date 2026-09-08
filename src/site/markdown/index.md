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

Ghostwriter MCP Server is a runnable MCP server that combines Ghostwriter’s project-aware code-assistance workflows with Bindex Core metadata retrieval, registration, and library-recommendation capabilities. The Machai MCP Server runtime exposes these capabilities as MCP tools over STDIO or HTTP, allowing AI assistants, IDEs, and automation processes to invoke them through a single server.

The server is assembled as an executable Java 17 artifact. Its runtime registers Ghostwriter tools for code-assistance workflows and Bindex Core services for discovering and working with library metadata; Ghostwriter can use the configured generative-AI provider when a workflow requires model assistance. The project contains no `package-info.java` files, so this overview is derived from the project’s Maven metadata and component design.

![Ghostwriter MCP Server component diagram](./images/c4-diagram.png)

The diagram is maintained in [`src/site/puml/c4-diagram.puml`](../puml/c4-diagram.puml) and shows the MCP client, Machai runtime, Ghostwriter tools, Bindex Core services, configured AI provider, and metadata sources.

## Supported AI providers

This module delegates AI-provider integration to its Ghostwriter and Machai dependencies. Its own Maven configuration selects **CodeMie** as the configured provider; no additional provider-specific implementations or provider settings are declared in this project. To use another provider supported by the underlying runtime, configure that runtime’s provider identifier and model according to its documentation.

| Provider | Configuration in this project |
| --- | --- |
| CodeMie | `genai.serverId=CodeMie` selects the provider; `gw.model=CodeMie:gpt-5.5-2026-04-24` selects its default model. |

### Common configuration parameters

| Parameter | Description | Default value |
| --- | --- | --- |
| `genai.serverId` | Identifier of the generative-AI provider server used by Ghostwriter. | `CodeMie` |
| `gw.model` | Provider-qualified model identifier used for Ghostwriter model requests. | `CodeMie:gpt-5.5-2026-04-24` |
| `maven.compiler.release` | Java release used to compile the server artifact. | `17` |

Set Maven properties with `-D`, or override them in the Maven configuration used to build or run the server. Provider credentials and any other connection settings are supplied by the selected underlying provider/runtime configuration rather than by this module’s `pom.xml`.

## Resources

- [Machai platform](https://machai.machanism.org/)
- [Machai GitHub repository](https://github.com/machanism-org/machai)
- [Ghostwriter MCP Server artifact on Maven Central](https://central.sonatype.com/artifact/org.machanism.machai/gw-mcp-server)
- [Machai MCP Server artifact on Maven Central](https://central.sonatype.com/artifact/org.machanism.machai/machai-mcp-server)
- [Ghostwriter documentation](https://machai.machanism.org/ghostwriter/)
- [Bindex Core documentation](https://machai.machanism.org/bindex-core/)
