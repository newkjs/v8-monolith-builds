# V8 Monolith Builds 💼
[![Build V8 Static Library](https://github.com/newkjs/v8-monolith-builds/actions/workflows/build_v8.yml/badge.svg)](https://github.com/newkjs/v8-monolith-builds/actions/workflows/build_v8.yml)
<br>
➡️ Precompiled V8 static libraries for use in embedded C++ projects

> V8 is Google’s open source high-performance JavaScript and WebAssembly engine, written in C++. It is used in Chrome and in Node.js, among others. It implements ECMAScript and WebAssembly, and runs on Windows 7 or later, macOS 10.12+, and Linux systems that use x64, IA-32, ARM, or MIPS processors. V8 can run standalone, or can be embedded into any C++ application. https://v8.dev

V8's practicality is evident in the world of embedded scripting environments, allowing developers to implement a hugely powerful JavaScript / WASM engine into their C++ project. However, given V8's massive codebase and freqeuent update schedule, it isn't maintainable to manually build and deploy the library following every release. Building from source is cumbersome, time-consuming and frustrating on most platforms, especially when system resources may be scarce. In order to aid in projects which desire to include V8's capabilities within themselves, this repository has been created, to automatically compile, and release subsequent builds following the latest stable release of V8.

## Build System
**Workflow**
<br>
Our build process utilizes GitHub actions to dispatch workflows which manage the building and bumping of V8 versions. In particular, the build workflow produces monolithic libraries, meaning that you're given a single binary which can be included into your C++ project along with the provided header files. Each platform also provides a compiled executable of the d8 debugging environment. Every build makes a release with the compiled binaries.

**Project Layout**
<br>
Google's GN build system utilizes `args.gn` files to define the parameters for a build, to access the build arguments for a specific platform you can navigate to the `platforms/` directory, and then the respective subdirectory for your targeted platform. The version of V8 that will be built is mandated by the `V8_VERSION` file in the root directory of the repository.

**Manual Workflow Invocation**
<br>
The build workflow can also be manually invoked by enacting a workflow dispatch event. You can either populate the "V8 Version" input field to target a specific version of V8 for the build, or leave it empty and have it target the version specified in the `V8_VERSION` file.

**Supported Platforms**
<br>
We currently produce builds for the following platforms:
| Operating System | x86-64 / x64 | aarch64 / ARM64 |
| :-: | :-: | :-: |
| Windows | ✔️ | ❌ |
| macOS | ✔️ | w/ Rosetta 2 |
| Linux | ✔️ | ❌ |

## Commit Prefixes

The workflow responsible for building the V8 libraries recognises a couple prefixes to control the behavior of the build.

| Prefix | Behavior |
| ------ | -------- |
| [Skip] | The workflow will **not** run if this prefix is present. |

**ℹ️ Notes:**
- Only use the `[Skip]` prefix when updating documentation or non-build-impacting files. Any changes to build configurations or V8 versions must be rebuilt to assure the master branch is always working.

## Custom Build Arguments
If you need to customize the build arguments to better match your use-case feel free to fork and modify this repo! Since this repo is configured to be generally usable within our own projects, and those of a vast amount of embedders, we try not too deviate too far from what would be acceptable to everyone. If you have any suggestions however, regarding the optimization of file size, performance, or modifications to the workflow, please open a GitHub issue so we can get started!
