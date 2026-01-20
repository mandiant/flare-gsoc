# Project Ideas
*FLARE @ Google Summer of Code 2025*

This document lists examples of projects that would be great for GSoC 2025 contributors.
The list doesn't include everything - feel free to identify your own idea and propose it!

All of our project ideas revolve around reverse engineering tools.
That is, we want to improve the lives of malware analysts through novel techniques and automation.
To succeed with any of these examples, you should have a basic familiarity with reverse engineering or a strong desire to learn.

Briefly:
- [capa](https://github.com/mandiant/capa) identifies the capabilities in executable files, such as "installs a service" or "downloads data via HTTP".
  - [add Binary Ninja Explorer plugin](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#capa-add-binary-ninja-explorer-plugin)
  - [add Ghidra Explorer plugin](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#capa-add-ghidra-explorer-plugin)
  - [Frida dynamic analysis for Android](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#capa-add-frida-dynamic-analysis-for-android)
  - [migrate to PyGhidra](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#capa-migrate-to-pyghidra)
  - [add ARM support to IDA Pro, Ghidra, and/or Binary Ninja backends](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#capa-add-arm-support-to-ida-pro-ghidra-andor-binary-ninja-backends)
- [FLOSS](https://github.com/mandiant/flare-floss) automatically deobfuscated protected strings in malware.
  - [extract language specific strings (.NET, Swift, Zig, ...)](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#floss-extract-language-specific-strings-net-swift-zig-)
  - [QUANTUMSTRAND](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#floss-quantumstrand)
- [BinDiff](https://github.com/google/bindiff) is an open-source comparison tool for binary files that assists vulnerability researchers and engineers to quickly find differences and similarities in disassembled code.
  - [rearchitect Binary Diff Server and port to PyQt](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#bindiff-rearchitect-binary-diff-server-and-port-to-pyqt)
- [XRefer](https://github.com/mandiant/xrefer) is an IDA plugin offering a custom navigation interface to examine execution paths, highlight downstream behaviors, cluster related functions, and generate Gemini-based insights into the malware's anatomy. 
  - [Build a Multi-Backend Abstraction Layer with Binary Ninja Support](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#xrefer-build-a-multi-backend-abstraction-layer-with-binary-ninja-support)
  - [HTML Exporter and Visualizer for XRefer's Cluster Analysis](https://github.com/mandiant/flare-gsoc/blob/2025/doc/project-ideas.md#xrefer-html-exporter-and-visualizer-for-xrefers-cluster-analysis)
- [GoReSym](https://github.com/mandiant/GoReSym) is a Go symbol parser that extracts program metadata (such as CPU architecture, OS, endianness, compiler version, etc), function metadata, filename and line number metadata, and embedded structures and types.

These tools are used by thousands of analysts to identify, describe, and stop malware.


## capa: add Binary Ninja Explorer plugin

_size_: medium, estimated 175 hours

_difficulty_: medium

_mentors_: [@williballenthin](https://github.com/williballenthin)

_link_: [https://github.com/mandiant/capa/issues/169](https://github.com/mandiant/capa/issues/169)

capa is the FLARE team's open-source tool to identify program capabilities using an extensible rule set.

Binary Ninja (Binja) is a modern disassembler and reverse engineering tool with a robust Python API that facilitates plugin development. A capa Explorer plugin for Binary Ninja would significantly enhance the workflow of reverse engineers who use Binja, allowing them to seamlessly identify and analyze program capabilities within their preferred environment. This project would not only benefit Binja users but also expand the reach and adoption of capa within the reverse engineering community.

The core functionality of the plugin would be to:

1. **Use capa's existing Binary Ninja backend to find capabilities in the currently open binary**.
1. **Display the capa results in a user-friendly manner within Binary Ninja**. This includes displaying matching rules, the locations of matched features, and potentially the associated source code (if debug information is available).
1. **Allow users to navigate from the capa results to the corresponding locations in the disassembly view**. This is crucial for efficient analysis, enabling users to quickly jump to the code responsible for a detected capability.

**Deliverables**:

- **Results Display**:
  - Implement a custom dock widget (view) in Binary Ninja to display the capa results.
  - Display a hierarchical tree view of matching rules, grouped by namespace (e.g., "anti-analysis", "communication").
  - Show the rule name, description (short summary), and match status.
  - Display the locations (addresses) of matched features within each rule.
  - Implement filtering and searching capabilities within the results view. Allow users to filter rules by namespace, ATT&CK technique, or keyword.
  - Highlight matched features directly in the disassembly view using Binary Ninja's highlighting API.
- **Navigation**:
  - Enable double-clicking on a rule or feature location in the results view to navigate to the corresponding address in the Binary Ninja disassembly view. Highlight the relevant instruction(s).
  - Add tags/bookmarks for the matches.
- **Rule Selection**:
  - Basic UI for user to select a file path that contains the rulesets they'd like to use.
- **Testing and Documentation**:
  - Write basic unit tests for the plugin's core functionality.
  - Create user documentation explaining how to install and use the plugin.
- **Blog Post**:
  - Document the development process and findings in a blog post suitable for publication on the Mandiant blog or a similar platform.

**Required Skills**:

- Solid knowledge of Python 3.
- Experience with Binary Ninja's API (or strong willingness to learn).
- Basic understanding of reverse engineering concepts (disassembly, assembly language, executable file formats).
- Experience with Git and GitHub.

**Potential Challenges and Mitigation Strategies**:

- **Binary Ninja API Learning Curve**: Binary Ninja's API is extensive, but well-documented. The contributor should allocate time for learning the API and exploring existing plugins. The mentors can provide guidance and point to relevant examples.
- **Performance Optimization**: Running capa on large binaries can be time-consuming. The plugin should be designed to handle large analysis results efficiently and provide progress feedback to the user. Asynchronous execution and caching strategies can be employed.
- **UI Design**: Provide the user an intuitive way to interact with the plugin.

## capa: add Ghidra Explorer plugin

_size_: medium, estimated 175 hours

_difficulty_: medium

_mentors_: [@mike-hunhoff](https://github.com/mike-hunhoff)

_link_: [https://github.com/mandiant/capa/issues/1980](https://github.com/mandiant/capa/issues/1980)

capa is the FLARE team's open-source tool to identify program capabilities using an extensible rule set. Currently, analysts often invoke capa as a command-line tool or via the capa Explorer plugin for IDA Pro. This project aims to bring the interactive rule exploration experience of capa Explorer to Ghidra, a powerful and extensible reverse engineering platform developed by the NSA.

Ghidra is a free and open-source software reverse engineering (SRE) framework. It includes a suite of tools for analyzing compiled code on a variety of platforms. Ghidra's extensibility is a key feature, and recently, the PyGhidra project has provided Python bindings for the Ghidra API, enabling plugin development in Python. A capa Explorer plugin for Ghidra would greatly enhance the workflow of reverse engineers who rely on Ghidra, allowing them to seamlessly integrate capa's capability detection into their analysis process. This project would benefit both Ghidra users and expand the user base of capa.

The core functionality of the plugin would be to:

1. **Use capa's existing Ghidra backend to find capabilities in the currently open binary**.
1. **Display the capa results in a user-friendly manner within Ghidra**. This includes showing matching rules, the locations of matched features (addresses, function names, etc.), and potentially linking to the relevant decompiler output.
1. **Allow users to navigate from the capa results to the corresponding locations in the Ghidra disassembly listing and decompiler views**. This is critical for efficient analysis, enabling users to quickly jump to the code associated with a detected capability.

**Deliverables**:

- **Results Display**:
  - Implement a custom Ghidra Tool window or panel to display the capa results.
  - Display a hierarchical tree view of matching rules, grouped by namespace (e.g., "anti-analysis", "communication").
  - Show the rule name, description, and match status.
  - Display the locations of matched features within each rule.
  - Implement filtering and searching capabilities within the results view. Allow users to filter rules by namespace, ATT&CK technique, or keyword.
  - Highlight matched features directly in the Ghidra listing view using Ghidra's highlighting API.
- **Navigation**:
  - Enable double-clicking on a rule or feature location in the results view to navigate to the corresponding address in the Ghidra disassembly listing view.
  - Highlight the relevant instructions.
- **Rule Selection**:
  - Provide a basic UI for the user to select the file path containing the desired rulesets.
- **Testing and Documentation**:
  - Write basic unit tests for the plugin's core functionality.
  - Create user documentation explaining how to install and use the plugin.
- **Blog Post**:
  - Document the development process, challenges and finding in a blog post.

**Required Skills**:

- Solid knowledge of Python 3.
- Experience with Ghidra and PyGhidra (or strong willingness to learn). Familiarity with Java is a plus, but not strictly required due to PyGhidra.
- Basic understanding of reverse engineering concepts (disassembly, assembly language, executable file formats).
- Experience with Git and GitHub.

**Potential Challenges and Mitigation Strategies**:

- **PyGhidra Learning Curve**: While PyGhidra simplifies Ghidra plugin development, the student will still need to learn the PyGhidra API and how it interacts with Ghidra's underlying Java API. The mentors can provide guidance and point to relevant examples.
- **Performance Optimization**: Running capa on large binaries can be time-consuming. The plugin should handle large results efficiently and provide feedback to the user. Asynchronous execution and caching can help.
- **UI Design**: Design the user interface to be intuitive within the Ghidra environment.

## capa: add Frida dynamic analysis for Android

_size_: large, estimated 350 hours

_difficulty_: hard

_mentors_: [@larchchen](https://github.com/larchchen)

capa is the FLARE team's open-source tool to identify program capabilities using an extensible rule set.

[Frida](https://github.com/frida/frida) is a popular dynamic instrumentation toolkit for developers, reverse-engineers, and security researchers, allowing custom scripts injected into black box processes thus monitoring program behaviors. Frida is a particularly preferred option to analyze Android Apps by launching Apps in Android Emulator and intercepting certain function calls.

In addition to the capa's dependencies on CAPE sandbox during dynamic capabilities detection, Frida is a more friendly alternative for mobile App analysis. With the possibilities of using existing Frida scripts and/or developing new Frida scripts, extending capa's dynamic detection upon logs generated from Frida logs would be a good start. Integrating capa rule matching engine with Frida scripts could be another bonus approach. The goal of this project is to support capa rule matching capabilities in Android via Frida instrumentation framework.

**Deliverables**

- Research
  - Review capa's existing support of dynamic capabilities detection
  - Review Frida's instrumentation framework
- Identify Additions, Changes, and Improvements
  - Suggest technical roadmaps to support Frida-capa detection
  - Discuss ideas with mentors and capa user community
- Implementation
  - Implement ideas aligned with finalized roadmaps
- Evaluation and Knowledge Sharing
  - Test deliverables and gather feedback from users
  - Write blog post about experience and project achievements

**Required Skills**

- Solid knowledge of Python 3
- Solid knowledge of one of JavaScript/C/Go
- Basic understanding of reverse engineering / malware analysis
- Basic understanding of Git
- Basic understanding of Android App analysis using Android Emulator
- Basic understanding of Frida

## capa: migrate to PyGhidra

_size_: small, estimated 90 hours

_difficulty_: low

_mentors_: [@mike-hunhoff](https://github.com/mike-hunhoff)

_link_: https://github.com/mandiant/capa/issues/2600

This project aims to modernize the existing capa Ghidra backend by migrating it from the third-party Ghidrathon Python bindings to the officially supported PyGhidra bindings, [released with Ghidra 11.3](https://github.com/NationalSecurityAgency/ghidra/blob/Ghidra_11.3_build/Ghidra/Configurations/Public_Release/src/global/docs/WhatsNew.md#pyghidra ). Since PyGhidra is distributed with Ghidra, we expect this to have better long term support and be easier for users to access. This migration will ensure the long-term maintainability and compatibility of the capa plugin with future Ghidra releases.

**Deliverables**:

- **Port Existing Functionality**: Migrate the existing capa Ghidra backend's code to use the PyGhidra API. This primarily involves updating API calls and adapting to any differences in how PyGhidra interacts with Ghidra.
- **Testing**: Thoroughly test the migrated plugin to ensure that all existing features function correctly with PyGhidra.
- **Documentation Updates**: Update the plugin's documentation to reflect the change to PyGhidra and provide installation instructions for users.

**Required Skills**:

- Basic Python programming skills.
- Familiarity with Ghidra and its scripting capabilities, or willingness to learn.
- Experience with Git and GitHub.
- Understanding of capa is a plus, but not required for this project.


## capa: add ARM support to IDA Pro, Ghidra, and/or Binary Ninja backends

_size_: small to large

_difficulty_: medium

_mentors_: [@mr-tz](https://github.com/mr-tz)

_link_: https://github.com/mandiant/capa/issues/1774

This project aims to extend capa's support for analyzing programs targeting the ARM architecture across its major analysis backends: IDA Pro, Ghidra, and Binary Ninja. While capa's core analysis engine (via the BinExport2 backend) already supports ARM, the backends for these popular disassemblers currently lack direct feature extraction for this architecture. This project will bridge that gap, enabling users to analyze ARM binaries seamlessly within their preferred reverse engineering environments.

The core task involves extending the existing backends to extract relevant features (instructions, API calls, constants, etc.) from ARM binaries loaded in IDA Pro, Ghidra, and Binary Ninja. This will leverage the respective disassembler APIs to access the disassembled code and program information. The extracted features will then be formatted and passed to capa's core analysis engine.

**Deliverables**:

- **update capa IDA Pro backend (optional, pick 1-3)**
- **update capa Ghidra backend (optional, pick 1-3)**
- **update capa Binary Ninja backend (optional, pick 1-3)**
- **Testing**: Develop test cases (ARM binaries with known capabilities) and verify that capa correctly identifies capabilities in these binaries through each of the extended plugins.
- **Documentation**: Update the documentation for each plugin to reflect the added ARM support.

**Required Skills**:

- Solid Python programming skills.
- Familiarity with at least one of: IDA Pro, Ghidra, or Binary Ninja, and their respective plugin APIs (or willingness to learn quickly).
- Basic understanding of the ARM architecture and assembly language.
- Experience with Git and GitHub.


## FLOSS: extract language specific strings (.NET, Swift, Zig, ...)

_size_: large, estimated 350 hours

_difficulty_: medium

_mentors_: [@mr-tz](https://github.com/mr-tz)

_link_: [https://github.com/mandiant/flare-floss/issues/718](https://github.com/mandiant/flare-floss/issues/718)

Various programming languages embed the constant data, like strings, used within executables in different ways. Most tools, like strings.exe, just look for printable character sequences. This doesn't work well for files compiled from Go or Rust.

Here we propose to extend FLOSS to include a framework to extract language specific strings from executables. After identifying the language, a specific extractor can use specialized logic to pull out the strings embedded into a program by the author. When possible, the extractor should indicate library and runtime-related strings. For example, the extractor may parse debug information to recognize popular third party libraries and annotate the related strings appropriately.

Today, FLOSS automatically deobfuscates protected strings found in malware. Better categorization of its output would make its users more efficient. Extracting language-specific strings would make FLOSS more useful and manifest success as the default tool used by security analysts.

**Deliverables**

- Develop language identification module
  - Initial focus on .NET
  - Consider also Swift, Zig, …
- Research language string embeddings and create extractor code
  - We can share existing knowledge and code to bootstrap this
- Identify strings related to runtime and library code for targeted programming languages
- Extend standard output format and render results

**Required Skills**

- Medium knowledge of Python 3
- Basic understanding of reverse engineering (focus: Windows PE files)
- Experience with .NET or Swift (internals) is a plus, but not required
- Interest in malware analysis with focus on static analysis
- Basic understanding of Git


## FLOSS: QUANTUMSTRAND

_size_: large, estimated 350 hours

_difficulty_: medium

_mentors_: [@williballenthin](https://github.com/williballenthin)

_link_: [https://github.com/mandiant/flare-floss/issues/943](https://github.com/mandiant/flare-floss/issues/943)

Extend FLOSS to use the rendering techniques pioneered by QUANTUMSTRAND.

QUANTUMSTRAND is an experiment that augments traditional strings.exe output with context to aid in malware analysis and reverse engineering. For example, we show the structure of a file alongside its strings and mute/highlight entries based on their global prevalence, library association, expert rules, and more.

FLOSS is a tool that automatically extracts obfuscated strings from malware, rendering the human-readable data in a way that enables rapid reverse engineering.

We propose to extend FLOSS to use the techniques pioneered by QUANTUMSTRAND to highlight important information while muting common and/or analytically irrelevant noise. The project will provide an opportunity to dig into the PE, ELF, and/or Mach-O file formats, finding ways to make technical details digestible. If successful, FLOSS will continue to be the tool that malware analysts turn to when triaging unknown files.

**Deliverables**

Brand new output format released as part of FLOSS v4 in late 2025.

- Research
  - Review Quantumstrand functionality
  - Evaluate most useful features for integration into FLOSS
- Identify and Propose Improvements
  - Suggest improvements for the user interface and experience
  - Discuss ideas with mentors and FLOSS user community
- Implementation
  - Implement improved functionality
  - [stretch goal]: Work on a GUI to interactively display FLOSS results
- Evaluation and Knowledge Sharing
  - Test improvements and gather feedback from users
  - Write blog post about experience and project achievements

**Required Skills**

- Solid knowledge of Python 3
- Basic understanding of reverse engineering / malware analysis
- Basic understanding of Git
- Experience or interest with file formats such as PE, ELF, and/or Mach-O
- Experience or interest in user interface and/or user experience design


## BinDiff: rearchitect Binary Diff Server and port to PyQt

_size_: large, estimated 350 hours

_difficulty_: hard

_mentors_: [@cblichmann](https://github.com/cblichmann)

_link_: https://github.com/google/bindiff/issues/17

This project aims to modernize BinDiff by re-architecting it as a cross-platform "diffing service" with a unified UI layer. The core idea is to separate the diffing engine from the user interface. A "diff server," implemented (likely in C++ or Rust for performance), will handle the core diffing logic. This server will load BinExport files and perform the diffing computations. It will communicate with client plugins via a protocol like gRPC.

Client plugins will be developed for IDA Pro and Binary Ninja, using a shared Python codebase and PyQt for the UI. Each disassembler will have a small, platform-specific module to handle tasks like symbol porting. This architecture promotes code reuse and simplifies maintenance. Keeping the diff server running in the background allows for dynamic re-diffing as binaries are modified, and opens up possibilities for improved flow graph visualization by combining data from multiple functions.

The project scope is intentionally flexible, allowing the student and mentors to collaboratively define the specific features and implementation details. The focus will be on establishing a solid foundation for the new architecture and demonstrating its feasibility.

**Deliverables (Flexible, to be refined during the project)**:

- **Diff Server Prototype**:
  - Design and implement a basic "diff server" that can load BinExport files and perform a simple diffing algorithm.
  - Implement a communication protocol (e.g., gRPC) for interaction with client plugins.
- **Shared UI Library (Python/PyQt)**:
  - Develop a shared Python library using PyQt that provides the core UI components for displaying diffing results. This includes views for function lists, matched/unmatched functions, and potentially basic flow graph comparisons.
- **IDA Pro and Binary Ninja Plugins**:
  - Create basic plugins for IDA Pro and Binary Ninja that utilize the shared UI library and communicate with the diff server.
  - Implement symbol porting.
  - Demonstrate basic diffing functionality within each disassembler.
- **Proof of Concept**:
  - Demonstrate the ability to load two BinExport files, perform a diff, and display the results in both IDA Pro and Binary Ninja.
- **Documentation**:
  - Document the design, architecture, and API of the diff server and client plugins.

**Required Skills**:

- Solid knowledge of Python 3 and C++ and/or Rust.
- Experience with or willingness to learn PyQt.
- Experience with or willingness to learn gRPC.
- Basic understanding of binary diffing concepts.
- Familiarity with IDA Pro and Binary Ninja APIs (or strong willingness to learn).
- Experience with Git and GitHub.

**Potential Challenges**:

- **Defining the Scope**: The open-ended nature of the project requires careful planning and communication between the student and mentors to define achievable goals.
- **Inter-process Communication**: Choosing and implementing an efficient and reliable communication protocol between the diff server and client plugins will be crucial.


### XRefer: Build a Multi-Backend Abstraction Layer with Binary Ninja Support
_size_: large, estimated 360 hours

_difficulty_: medium

_mentors_: [@m-umairx](https://github.com/m-umairx)

XRefer is tightly coupled with IDA Pro, making it challenging to adapt for use with other popular reverse-engineering platforms like Ghidra or Binary Ninja. This project aims to refactor XRefer's core **analyzer** component, creating a new backend abstraction layer that standardizes how different platforms interact with the plugin's logic. Additionally, the project aims to aid support for Binary Ninja by implementing a new PoC backend.

_**Note**: This project focuses on creating and demonstrating an abstraction layer for XRefer's underlying analysis engine only. The user interface is not included in the project scope_.

**Deliverables**:

- **Code Review**
  - Identify and document all places where IDA-specific APIs or data structures are used within the **analyzer** and **lang** components.
  - Assess the feasibility and scope of decoupling those calls into a new abstraction layer.
- **Design a Backend Interface**
  - Specify the APIs needed for core tasks (e.g., disassembly, cross-references, function discovery, flow analysis) that different backends must implement.
  - Draft an interface or set of classes that each supported platform (IDA, Ghidra, Binary Ninja, etc.) can plug into with minimal friction.
- **Refactor XRefer**
  - Migrate IDA-specific logic into a separate module or wrapper.
  - Adapt XRefer's main codebase to use the newly created backend interface rather than direct IDA calls.
- **Proof-of-Concept for Additional Backends**
  - Implement a PoC backend using Binary Ninja's API.
  - Demonstrate how XRefer can run independently of IDA using the newly defined backend interface to generate a .**xrefer** analysis file.
  - Outline best practices for future contributors to add and maintain backends.

**Required Skills**

- Proficiency in Python programming language.
- Experience with (or strong willingness to learn) IDA's Python API.
- Experience with (or strong willingness to learn) Binary Ninja's API.
- Basic understanding of reverse engineering and underlying concepts (disassembly, functions, cross-references) and executable file formats.
- Basic knowledge of Git/Github.

## XRefer: HTML Exporter and Visualizer for XRefer's Cluster Analysis
_size_: medium, estimated 160 hours

_difficulty_: low

_mentors_: [@m-umairx](https://github.com/m-umairx)

The goal of this project is to design and implement an HTML export module for XRefer. The module will convert XRefer's internal cluster analysis data into a dynamic HTML visualization. This interactive output should allow users to:

- **View Cluster Graphs**: Render detailed graphs illustrating the relationships between clusters.
- **Read Semantic Descriptions**: Provide natural language explanations for each cluster and its contained functions.
- **Interact with Data**: Offer interactive controls (e.g., zoom, pan, node selection, filtering) to explore and analyze clusters in depth.

**Deliverables**:

- **Design and Architecture**
  - Develop an intuitive UI/UX design that outlines how clusters and their semantic descriptions will be presented. Consider interactive elements such as zoomable graphs, clickable nodes, and filtering options.
  - Evaluate and choose suitable front-end libraries or frameworks (e.g., D3.js, Cytoscape.js) for rendering graphs and managing interactivity.
- **Develop the HTML Export Module**
  - Create a Python module to convert XRefer's cluster analysis data into a format consumable by the front-end (e.g., JSON).
  - Develop a responsive HTML template that integrates the chosen visualization libraries. The template should include placeholders for cluster graphs, semantic descriptions, and interactive controls.
  - Implement features such as zoom, pan, node highlighting, and tooltips to enhance the user's exploratory experience.
  - Integrate the export module into the existing XRefer workflow so that a .html file is generated as part of the analysis process.
- **Documentation**
  - Document the design decisions, data transformation process, and integration steps to help future contributors extend or maintain the module.

**Required Skills**

- Proficiency in Python programming language.
- Familiarity with HTML, CSS, and JavaScript for building interactive web interfaces.
- Experience with visualization libraries (e.g., D3.js, Cytoscape.js) or willingness to learn how to implement interactive graphs.
- Ability to conceptualize and design an intuitive user interface that effectively presents complex data.
- Basic knowledge of Git/Github.

## GoReSym: Recover Golang Structure Tags and Interface Methods in GoReSym

**Mentors:** @stevemk14ebr, @jaeyoungkimG  
**Difficulty:** Easy to Medium  
**Project Repo:** [https://github.com/mandiant/GoReSym](https://github.com/mandiant/GoReSym)  

### Description
GoReSym is a Go symbol parser that extracts program metadata, function information, and embedded structures/types from Go binaries. It is widely used by reverse engineers to analyze stripped Go binaries and reconstruct type definitions.

Currently, GoReSym recovers structure fields but fails to extract **structure tags** (e.g., `` `json:"name"` ``) and **interface method names**. These tags are critical for understanding how data is serialized (JSON, XML) and how the application interacts with databases or external APIs. The goal of this project is to implement the parsing logic required to recover these missing metadata fields and include them in GoReSym's JSON output.

### Task Details
The contributor will need to:
1.  **Analyze Existing Parsers:** Study how GoReSym currently extracts `StructField` information by looking at the code adapted from the Go runtime (specifically `objfile` and type parsing logic).
2.  **Implement Tag Extraction:** Add logic to read the tag string associated with struct fields. This involves understanding the internal memory layout of Go types.
3.  **Implement Method Name Extraction:** Add logic to recover method names for Interface types.
4.  **Update Output:** Modify the JSON serialization to include these new fields.
5.  **Testing:** specific test cases involving structs with various tags and interfaces to ensure accurate recovery across different Go versions.

### Recommended Skills
*   **Go (Golang):** Intermediate knowledge.
*   **Reverse Engineering:** Basic understanding of binary formats (PE, ELF, Mach-O) and memory layouts.
*   **Go Internals:** Familiarity with how Go stores type metadata (`moduledata`, `pclntab`) is helpful but can be learned during the project.

### Resources
*   **Issue Discussion:** [GoReSym Issue #37](https://github.com/mandiant/GoReSym/issues/37) (contains references to similar implementations).
*   **Reference Implementation:** [goretk/gore type parsing](https://github.com/goretk/gore/blob/3009b3909f08fa910e5a93d893bb66117f3628f9/type2.go#L149)

