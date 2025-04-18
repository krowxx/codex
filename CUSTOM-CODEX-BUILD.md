# Building Your Custom Codex

This guide walks you through forking the Codex repository, adding your own features, and building a standalone custom version—isolated from the official codebase.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Fork & Clone](#fork--clone)
3. [Branching Strategy](#branching-strategy)
4. [Developing New Features](#developing-new-features)
5. [Building Your Custom Version](#building-your-custom-version)
6. [Version Isolation](#version-isolation)
7. [Syncing with Official Without Merging](#syncing-with-official-without-merging)
8. [Best Practices & Design Patterns](#best-practices--design-patterns)
9. [Testing & Deployment](#testing--deployment)
10. [Troubleshooting](#troubleshooting)
11. [References](#references)

## 1. Prerequisites
- **Git** installed and configured
- **Node.js** >= 14.x and npm (or Yarn)
- Optional: **Docker** for containerized builds

## 2. Fork & Clone
1. On GitHub, fork `openai/codex` to your own account.
2. Clone your fork locally:
   ```bash
   git clone git@github.com:YOUR_USERNAME/codex.git
   cd codex
   ```
3. Add the official repo as an upstream remote:
   ```bash
   git remote add upstream git@github.com:openai/codex.git
   ```

## 3. Branching Strategy
- Create a branch for your custom distribution:
  ```bash
  git checkout -b custom-distribution
  ```
- For each feature, spin off feature branches:
  ```bash
  git checkout -b feature/my-new-feature
  ```

## 4. Developing New Features
- **Single Responsibility Principle (SRP):** Keep each module focused on one task.
- **Open/Closed Principle (OCP):** Add new functionality via extensions or plugins without modifying core code.
- **Program to an Interface:** Define abstract interfaces for new components.
- **Favor Composition Over Inheritance:** Build features as composable modules.

**Example: Strategy Pattern for Tokenizers**
```ts
// Define an interface
interface Tokenizer {
  tokenize(input: string): string[];
}

// Concrete strategy
class MyTokenizer implements Tokenizer {
  tokenize(input: string): string[] {
    // ...custom logic...
  }
}

// Register strategy
registry.register('custom', new MyTokenizer());
```

## 5. Building Your Custom Version
1. Install dependencies:
   ```bash
   npm install
   ```
2. Run the build:
   ```bash
   npm run build
   ```
3. Produce a distributable bundle:
   ```bash
   npm run bundle
   ```
4. Configure and link your custom CLI
   - Ensure your `package.json` has the correct `bin` field:
     ```json
     {
       "name": "codex",
       "version": "1.0.0-custom",
       "bin": {
         "codex": "./bin/codex.js"
       }
     }
     ```
   - Link your package globally so that you can invoke `codex` anywhere:
     ```bash
     npm link
     ```
   - Or use `npx` within your project directory without global linking:
     ```bash
     npx codex <command>
     ```

## 6. Version Isolation
- Tag your custom release:
  ```bash
  git tag v1.0.0-custom
  git push origin v1.0.0-custom
  ```
- Keep your `custom-distribution` branch isolated; avoid merging `upstream/main` directly.

## 7. Syncing with Official Without Merging
- Fetch upstream changes:
  ```bash
  git fetch upstream
  ```
- Cherry-pick or rebase individual commits if desired:
  ```bash
  git checkout custom-distribution
  git cherry-pick <commit-hash>
  ```

## 8. Best Practices & Design Patterns
- **Strategy Pattern:** Encapsulate algorithms (e.g., different tokenization or ranking schemes).
- **Mediator Pattern:** Centralize communication between core components.
- **Bridge Pattern:** Separate abstractions (interfaces) from implementations for independent evolution.
- **Encapsulate What Varies:** Isolate change-prone components.
- **Extensibility & Reuse:** Design plugins so new features can be enabled/disabled easily.

## 9. Testing & Deployment
- Run unit tests:
  ```bash
  npm test
  ```
- Lint and format:
  ```bash
  npm run lint
  npm run format
  ```
- Start a local server:
  ```bash
  npm start
  ```

## 10. Troubleshooting
- **Build errors:** Check Node.js version and dependencies.
- **Merge conflicts:** Use `git cherry-pick --no-commit` to inspect patches.
- **Plugin failures:** Ensure new modules implement the required interfaces.

## 11. References
- **Dive Into Design Patterns** for SOLID and patterns
- **Official Codex repo:** https://github.com/openai/codex 