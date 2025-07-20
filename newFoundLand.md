### A Complete Overview of the Gemini CLI Project

This document provides a comprehensive understanding of the Gemini CLI project, synthesizing findings from a high-level architectural analysis and a detailed feature review.

#### **1. High-Level Architecture**

The project is structured as a **monorepo**, which is a single repository containing multiple distinct packages. This approach is often used to simplify dependency management and code sharing between related projects. The key packages are:

*   **`@google/gemini-cli-core` (`packages/core`):** This is the heart of the application. It contains all the core logic for interacting with the Google Gemini API, managing tools, handling session state, and orchestrating the backend operations. It is designed to be a standalone engine that could potentially be used by other clients, not just the CLI.
*   **`@google/gemini-cli` (`packages/cli`):** This package contains the user-facing command-line interface. It is built using React and Ink, and it provides the interactive REPL (Read-Eval-Print Loop), command parsing, and user interface components. It is a client that depends on the `@google/gemini-cli-core` package for its functionality.

The `cli` package has a direct dependency on the `core` package, meaning the user interface is built on top of the foundational logic provided by the core engine.

#### **2. Detailed Features and Capabilities**

The Gemini CLI is an interactive and scriptable tool for developers to leverage Google's Gemini models from their terminal.

**Key Purpose:**

The main goal of the Gemini CLI is to provide a seamless and powerful command-line experience for interacting with Gemini. It supports both conversational, interactive sessions and non-interactive execution for automation and scripting.

**Core Functionalities (from `@google/gemini-cli-core`):**

*   **API Interaction:** Manages all communication with the Gemini API.
*   **Tool Orchestration:** A powerful system allows the model to request the execution of predefined tools. The core package validates these requests, runs the tools, and sends the results back to the model.
*   **State and Context Management:** It maintains the history of the conversation, enabling coherent, multi-turn interactions.
*   **Automatic Token Compression:** To stay within the model's token limits, it automatically summarizes and compresses long conversation histories.
*   **Model Fallback:** It can automatically switch from the "pro" model to the "flash" model if it detects rate-limiting, ensuring a more resilient user experience.
*   **Hierarchical Memory:** The CLI can be customized with instructions provided in `GEMINI.md` files. It discovers these files hierarchically, from the current directory up to the user's home directory, allowing for global, per-project, and local instructions.

**User-Facing Features (from `@google/gemini-cli`):**

The CLI provides a rich set of commands for users:

*   **Slash Commands (`/`):** For controlling the session and environment.
    *   Session: `/chat`, `/clear`, `/compress`, `/quit`
    *   Context: `/memory`, `/restore`
    *   Introspection: `/help`, `/tools`, `/stats`
    *   Configuration: `/theme`, `/editor`, `/auth`
*   **At Commands (`@`):** To easily include the content of files (`@file.txt`) or entire directories (`@src/`) in a prompt.
*   **Shell Passthrough (`!`):** To execute shell commands directly from the CLI, either as one-off commands (`!ls -l`) or by entering a persistent shell mode (`!`).

**Available Tools for the Model:**

The model can use a variety of tools to interact with the local environment:

*   **File System:** `read_file`, `read_many_files`, `write_file`, `edit`, `ls`, `glob`, `grep`
*   **Execution:** `run_shell_command`
*   **Web:** `web_fetch`, `google_web_search`
*   **Memory:** `save_memory`
*   **Extensibility:** It supports the **Model Context Protocol (MCP)**, allowing it to connect to external servers and use third-party tools.