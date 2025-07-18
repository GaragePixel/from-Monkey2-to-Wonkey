# Package System Specification: From Modules to Ecosystem
**Author:** iDkP from GaragePixel  
**Date:** 2025-07-16  

## Purpose

This document proposes a hierarchical package management system for Monkey3 (Wonkey2) that extends the current module architecture into a three-tier ecosystem: **Package** ← **Library** ← **Module**. The system maintains backward compatibility with existing Monkey2/Wonkey modules while introducing modern package management capabilities inspired by proven ecosystems like Go modules, Rust crates, and Node.js packages.

## List of Functionality

- **Three-Tier Hierarchy:**
	- **Module:** Individual compilation unit with namespaces (current Monkey2/Wonkey concept)
	- **Library:** Collection of related modules in a single folder, independently precompilable
	- **Package:** Distribution bundle containing multiple libraries with installer/wizard

- **Enhanced Module Discovery:**
	- Extends current ini-based module search beyond "modules" folder
	- Allows arbitrary folder registration for module discovery
	- Maintains JSON metadata for module definitions

- **Independent Precompilation:**
	- Each module compiles independently within a library
	- Entire libraries precompile as cohesive units
	- Supports incremental builds and dependency optimization

- **Standard Library Decomposition:**
	- Breaks monolithic stdlib into discrete, focused modules
	- Organizes stdlib modules within "stdlib" library folder
	- Maintains namespace hierarchy while improving modularity

- **Package Distribution System:**
	- Wizard-based installer for package deployment
	- URL-based library discovery and download
	- Version management with upgrade capabilities
	- Automatic ini file management for library registration

- **Backward Compatibility:**
	- Existing Monkey2/Wonkey modules continue functioning
	- Current module import syntax remains valid
	- Gradual migration path for legacy projects

## Notes on Implementation Choices

- **Hierarchical Design Philosophy:**
	The three-tier approach mirrors successful package ecosystems while respecting Monkey2/Wonkey's namespace-centric architecture. This isn't revolutionary but represents thoughtful adaptation of proven patterns.

- **Precompilation Strategy:**
	Independent module precompilation within libraries enables better build performance and dependency management, addressing current stdlib compilation bottlenecks.

- **Folder-Based Organization:**
	Using filesystem hierarchy for package organization aligns with developer expectations and simplifies tooling implementation.

- **JSON Metadata Approach:**
	Extending existing JSON module definitions provides familiar configuration while enabling richer metadata for package management.

## Technical Advantages and Detailed Explanations

- **Ecosystem Coherence with Existing Systems:**
	This design parallels established patterns:
	- **Go:** modules → packages → repositories
	- **Rust:** modules → crates → registries  
	- **Node.js:** modules → packages → npm registry
	- **Python:** modules → packages → PyPI
	
	This system's originality lies in its namespace-first approach and precompilation focus, rather than the hierarchical structure itself.

- **Namespace Preservation:**
	The friend-like namespace integration mechanism maintains Monkey2/Wonkey's powerful namespace system while enabling modular distribution.

- **Performance Optimization:**
	Independent precompilation eliminates the need to recompile entire stdlib for small changes, significantly improving development iteration speed.

- **Distribution Flexibility:**
	Package-level distribution enables ecosystem growth while maintaining developer control over dependency selection.

- **Version Management Integration:**
	URL-based distribution with version checking provides modern package management without requiring centralized registries.

## Ecosystem Comparison and Originality Assessment

### Similar to Existing Systems:
- **Hierarchical Structure:** Nearly universal in modern languages
- **Metadata-Driven Configuration:** Standard practice (package.json, Cargo.toml, go.mod)
- **URL-Based Distribution:** Used by Go modules, some Rust registries

### Original Aspects:
- **Namespace-First Design:** Most systems prioritize modules over namespaces
- **Precompilation at Library Level:** Unique focus on compilation unit optimization
- **Friend-Like Namespace Integration:** Sophisticated namespace sharing mechanism
- **Wizard-Based Installation:** User-friendly approach vs. command-line tools

### Well-Considered Elements:
- **Backward Compatibility Strategy:** Essential for ecosystem migration
- **Standard Library Decomposition:** Addresses real performance pain points
- **Flexible Folder Discovery:** Enables diverse project organizations

## Implementation Roadmap

### Phase 1: Library System
```
Current: modules/mymodule/
Proposed: libraries/mylib/module1/
                        /module2/
                        /mylib.json
```

### Phase 2: Package Distribution
```
Package Wizard:
- Download library from URL
- Extract to libraries/libname/
- Update ini with library path
- Verify dependencies
```

### Phase 3: Version Management
```
Version Detection:
- Check local library versions
- Query remote version info
- Offer upgrade/downgrade options
- Handle dependency conflicts
```

## Coexistence with Legacy System

The proposed system maintains full compatibility through:

- **Dual Path Support:** Both "modules/" and "libraries/" paths remain valid
- **Import Syntax Preservation:** Existing `Import` statements continue working
- **Gradual Migration:** Developers can migrate modules individually
- **Tooling Compatibility:** Existing build tools function unchanged

## Technical Specification

### Library Structure:
```
libraries/
├── stdlib/
│   ├── core/
│   ├── collections/
│   ├── io/
│   └── stdlib.json
├── graphics/
│   ├── rendering/
│   ├── ui/
│   └── graphics.json
└── mylib/
    ├── module1/
    ├── module2/
    └── mylib.json
```

### Module Discovery INI Enhancement:
```ini
[modules]
path=modules
path=libraries/stdlib
path=libraries/graphics
path=libraries/mylib
```

### Package Wizard Workflow:
```
1. Parse package manifest
2. Check library dependencies
3. Download missing libraries
4. Extract to libraries/
5. Update module discovery ini
6. Precompile libraries
7. Verify installation
```

## Conclusion

This package system represents a well-considered evolution of Monkey2/Wonkey's module architecture. While the hierarchical approach follows established patterns, the namespace-first design and precompilation focus provide innovation. The system addresses real pain points (stdlib compilation, distribution complexity) while maintaining the language's architectural strengths.

The three-tier hierarchy is both familiar to developers from other ecosystems and respectful of Monkey2/Wonkey's unique characteristics. Most importantly, the backward compatibility strategy ensures ecosystem continuity during the transition to Monkey3 (Wonkey2).

This system positions the language ecosystem for growth while preserving its distinctive namespace-centric philosophy-a balance that demonstrates thoughtful design rather than blind imitation of other systems.
