# TypeScript 7 <!-- omit in toc -->

<a href="https://github.com/microsoft/typescript-go" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/Project-Active-green.svg" alt="Project Status: Active"></a>
<a href="https://opensource.org/licenses/Apache-2.0" target="_blank" rel="noopener noreferrer"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="License: Apache 2.0"></a>

<a href="https://devblogs.microsoft.com/typescript/typescript-native-port/" target="_blank" rel="noopener noreferrer">Not sure what this is? Read the announcement post!</a>

This repo is very much under active development; as such there are no published artifacts at this time.
Interested developers can clone and run locally to try out things as they become available.

## Table of Contents <!-- omit in toc -->
- [How to Build and Run](#how-to-build-and-run)
  - [Prerequisites](#prerequisites)
  - [Setup Instructions](#setup-instructions)
  - [Development Commands](#development-commands)
  - [Running `tsgo`](#running-tsgo)
  - [Running LSP Prototype](#running-lsp-prototype)
- [What Works So Far?](#what-works-so-far)
  - [Status Overview](#status-overview)
  - [Status Definitions](#status-definitions)
- [Other Notes](#other-notes)
- [Contributing](#contributing)
- [Trademarks](#trademarks)

## How to Build and Run

### Prerequisites

This repo uses:
- <a href="https://go.dev/dl/" target="_blank" rel="noopener noreferrer">Go 1.24 or higher</a>
- <a href="https://nodejs.org/" target="_blank" rel="noopener noreferrer">Node.js with npm</a>
- <a href="https://www.npmjs.com/package/hereby" target="_blank" rel="noopener noreferrer">`hereby`</a>

### Setup Instructions

For tests and code generation, this repo contains a git submodule to the main TypeScript repo pointing to the commit being ported.
When cloning, you'll want to clone with submodules:

```sh
git clone --recurse-submodules https://github.com/microsoft/typescript-go.git
```

If you have already cloned the repo, you can initialize the submodule with:

```sh
git submodule update --init --recursive
```

### Development Commands

With the submodule in place and `npm ci`, you can run tasks via `hereby`, similar to the TypeScript repo:

```sh
hereby build
```
Verify that the project builds

```sh
hereby test
```
Run all tests

```sh
hereby install-tools
```
Install additional tools such as linters

```sh
hereby lint
```
Run all linters

```sh
hereby format
```
Format all code

```sh
hereby generate
```
Generate all Go code (e.g. diagnostics, committed to repo)

Additional tasks are a work in progress.

> **Note:** `hereby` is not required to work on the repo; the regular `go` tooling (e.g., `go build`, `go test ./...`) will work as expected.
> `hereby` tasks are provided as a convenience for those familiar with the TypeScript repo.

### Running `tsgo`

After running `hereby build`, you can run `built/local/tsgo`, which behaves mostly the same as `tsc` (respects tsconfig, but also prints out perf stats).
This is mainly a testing entry point; for higher fidelity with regular `tsc`, run `tsgo tsc [flags]`, which behaves more similarly to `tsc`.

### Running LSP Prototype

To try the prototype LSP experience:

1. Run VS Code in the repo workspace (`code .`)
2. Copy `.vscode/launch.template.json` to `.vscode/launch.json`
3. <kbd>F5</kbd> (or `Debug: Start Debugging` from the command palette)

This will launch a new VS Code instance which uses the Corsa LS as the backend. If correctly set up, you should see "typescript-go" as an option in the Output pane:

![LSP Prototype Screenshot](.github/ls-screenshot.png)


## What Works So Far?

This is still a work in progress and is not yet at full feature parity with TypeScript. Bugs may exist. Please check this list carefully before logging a new issue or assuming an intentional change.

### Status Overview

| Feature | Status | Description |
|---------|--------|-------------|
| **Program creation** | **Done** | Read `lib`, `target`, `reference`, `import`, `files`, `include`, and `exclude`. You should see the *same files*, with modules resolved to the *same locations*, as in a TypeScript 5.8 (TS5.8) invocation. Not all resolution modes are supported yet. |
| **Parsing/scanning** | **Done** | Read source text and determine syntax shape. You should see the exact same *syntax errors* as in a TS5.8 invocation. |
| **Commandline and `tsconfig.json` parsing** | **Mostly Done** | Note that the entry point is slightly different (for now). |
| **Type resolution** | **Done** | Resolve computed types to a concrete internal representation. You should see the same types as in TS5.8. |
| **Type checking** | **Done** | Check for problems in functions, classes, and statements. You should see the same errors, in the same locations, with the same messages, as TS 5.8. Types printback in errors may display slightly differently; this is in progress. |
| **JavaScript-specific inference and JS Doc** | **Not Ready** | |
| **JSX** | **Not Ready** | |
| **Declaration emit** | **Not Ready** | Coming soon! |
| **Emit (JS output)** | **In Progress** | `target: esnext` (minimal downleveling) is well-supported but other targets may have gaps. |
| **Watch mode** | **Prototype** | Watches the correct files and rebuilds, but doesn't do incremental rechecking. |
| **Build mode / project references** | **Not Ready** | |
| **Incremental build** | **Not Ready** | |
| **Language service (LSP)** | **Prototype** | Expect minimal functionality (errors, hover, go to def). More features soon! ASCII files only for now. |
| **API** | **Not Ready** | |

### Status Definitions

- **Done** (aka "believed done"): We're not currently aware of any deficits or major left work to do. OK to log bugs.
- **In Progress**: Currently being worked on; some features may work and some might not. OK to log panics, but nothing else please.
- **Prototype**: Proof-of-concept only; do not log bugs.
- **Not Ready**: Either haven't even started yet, or far enough from ready that you shouldn't bother messing with it yet.

## Other Notes

Long-term, we expect this repo is that its contents will be merged into `microsoft/TypeScript`.
As a result, the repo and issue tracker for typescript-go will eventually be closed, so treat discussions/issues accordingly.

For a list of intentional changes with respect to TypeScript 5.7, see CHANGES.md.

## Contributing

This project welcomes contributions and suggestions. Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit <a href="https://cla.opensource.microsoft.com" target="_blank" rel="noopener noreferrer">Contributor License Agreements</a>.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the <a href="https://opensource.microsoft.com/codeofconduct/" target="_blank" rel="noopener noreferrer">Microsoft Open Source Code of Conduct</a>.
For more information see the <a href="https://opensource.microsoft.com/codeofconduct/faq/" target="_blank" rel="noopener noreferrer">Code of Conduct FAQ</a> or
contact <a href="mailto:opencode@microsoft.com" target="_blank" rel="noopener noreferrer">opencode@microsoft.com</a> with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft
trademarks or logos is subject to and must follow
<a href="https://www.microsoft.com/legal/intellectualproperty/trademarks/usage/general" target="_blank" rel="noopener noreferrer">Microsoft's Trademark & Brand Guidelines</a>.
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.

<script>
// Force all links to open in new tabs
document.addEventListener('DOMContentLoaded', function() {
  document.querySelectorAll('a[href^="http"]').forEach(link => {
    link.setAttribute('target', '_blank');
    link.setAttribute('rel', 'noopener noreferrer');
  });
});
</script>
