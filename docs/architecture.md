# Project Architecture

This document provides a high-level overview of the project's architecture.

## Monorepo Structure

The project is structured as a monorepo, which is a single repository containing multiple distinct packages. This approach is defined in the root `package.json` file, which specifies the `workspaces` configuration as `packages/*`. This means that each subdirectory within the `packages` directory is treated as a separate package.

## Packages

The project is composed of two main packages:

*   **`@google/gemini-cli-core`**: This package contains the core logic of the Gemini CLI. It is responsible for handling the main functionalities, such as processing commands, managing authentication, and interacting with the Gemini API.

*   **`@google/gemini-cli`**: This package serves as the user-facing command-line interface (CLI). It is responsible for parsing user input, displaying output, and managing the overall user experience.

## Dependencies

The `@google/gemini-cli` package has a dependency on the `@google/gemini-cli-core` package. This means that the CLI package relies on the core package to perform its functions. This separation of concerns allows for a more modular and maintainable codebase.

## Conclusion

The project's architecture is designed to be modular and scalable. The use of a monorepo allows for easy management of the different packages, while the separation of concerns between the core and CLI packages ensures a clean and maintainable codebase.
