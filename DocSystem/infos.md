**Author:** iDkP from GaragePixel  
**Date:** 2025-07-16  

# Purpose

The document at [NewDocSystem.md](https://github.com/GaragePixel/from-Monkey2-to-Wonkey/blob/main/DocSystem/NewDocSystem.md) proposes a new, modern documentation system for the Monkey2/Wonkey ecosystem. The focus is on in-file, richly annotated documentation, processed by a new, backward-compatible `makedoc` tool. It aims to empower both organizations and individual developers to create detailed, navigable, and locally-hosted documentation—akin to "manpages" for their libraries, modules, and projects.

# List of Functionality

- **In-file Documentation:**  
	Documentation is embedded directly within source files, using a structured metadata system inspired by Javadoc.
- **#pragma docfile Command:**  
	A preprocessor directive disables code parsing in specified files, enabling pure documentation files within the namespace hierarchy, deactivating their sourcing into the c++ project.
- **Backward Compatibility:**  
	The new system is designed to work with existing code and doc formats, ensuring older projects aren’t broken.
- **Rich Metadata:**  
	Metadata tags (author, version, history, links, etc.) are parsed and indexed for every function, class, or module.
- **Auto-generated Manpages:**  
	Developers and organizations can generate local manpages, navigating docs by function, class, module, or contributor.
- **Enhanced Linking:**  
	Docs can include rich, arbitrary links (to web resources, user profiles, etc.).
- **Achievement and History Logging:**  
	Each doc page can automatically parse and present a history log of changes, contributions, and achievements.

# Notes on Implementation Choices

- **Pedagogical Design:**  
	The system is modeled after proven documentation strategies (such as Javadoc) but tailored for Monkey2/Wonkey’s syntax and needs, focusing on usability and clarity.
- **Full In-file Approach:**  
	All documentation resides alongside code, reducing drift and guaranteeing relevance. The `#pragma docfile` allows for dedicated doc-only files inside the project’s namespace structure.
- **Local-First Philosophy:**  
	No reliance on external web services or upload platforms. Documentation is generated and consumed locally, keeping the workflow fast, private, and sustainable—similar to how Discord manages downloadable content.
- **Backward Compatibility:**  
	By preserving compatibility, existing projects and habits aren't disrupted, which is critical for community adoption.

# Technical Advantages and Detailed Explanations

- **Unified Development and Documentation:**  
	Embedding docs with code aligns with best practices seen in major languages and ecosystems (e.g., Go, Rust, Java), improving maintainability and discoverability.
- **Extensible Metadata:**  
	By allowing arbitrary metadata tags, the system can evolve alongside developer needs—enabling future-proofing for features like achievement logs and contributor histories.
- **Namespace-aware Documentation:**  
	Using the language’s namespace system ensures that docs are contextually appropriate and easy to locate, especially in large or modular projects.
- **Flexible Doc-only Files:**  
	`#pragma docfile` is a smart solution: it segregates documentation from code where necessary, without losing hierarchical structure or namespacing.
- **Local Manpage Generation:**  
	This is a strong move for sustainability, privacy, and speed. It gives users complete control over their documentation environment, and is resilient to platform changes or outages.
- **Automatic History and Achievement Logging:**  
	Automating the tracking of contributor history and module changes directly in the generated docs is a powerful feature—both for transparency and for celebrating community contributions.

# Critique

## Strengths

- **Addresses Real Pain Points:**  
	You’ve correctly identified that the old system’s design—not implementation—was lacking. This proposal goes right to the heart of those problems.
- **Pedagogically Sound:**  
	The alignment with proven documentation systems (Javadoc, etc.) is smart, but the system still innovating.
- **Local-First, Open Model:**  
	Avoiding cloud lock-in and paid upload models keeps the ecosystem open and developer-friendly.
- **Backward Compatibility:**  
	This is essential for community adoption and is well-considered.

## Concerns & Suggestions

- **Docfile Parsing and Tooling:**  
	Implementing robust parsing for mixed code/doc and doc-only files will require careful design—especially handling edge cases with the precomp pragma.
- **Metadata Overload:**  
	Too many metadata tags or inconsistent usage might fragment the doc ecosystem. Strong conventions or linting tools may be needed.
- **Community Guidelines:**  
	To avoid fragmentation, publishing clear guidelines and examples for doc format, metadata, and linking standards would be beneficial.

# Conclusion

The proposed doc system is a genuine leap forward for Monkey2/Wonkey. It’s technically sound, future-proof, and community-oriented. The key to success will be robust tooling, good conventions, and clear communication as the system is adopted. If well-executed, it could become a reference model for similar languages and ecosystems.
