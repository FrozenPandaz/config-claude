# Split Rust Monolithic Crate into Workspace

## Context
Use this skill when you have a monolithic Rust crate that needs to be split into multiple workspace crates for faster parallel compilation and better modularity.

## When to Use
- Large Rust projects with slow build times
- Codebases with clear logical boundaries between modules
- Projects where incremental builds take too long
- When you want parallel compilation of independent components

## Prerequisites
- Existing Rust project with multiple modules
- Understanding of the module dependencies
- Git repository for tracking changes

## Process

### Phase 1: Analysis & Planning

1. **Analyze the current structure**
   ```bash
   # Count Rust files and LOC
   find src -name "*.rs" | wc -l
   tokei src/
   ```

2. **Identify logical crate boundaries**
   - Look for modules with clear responsibilities
   - Identify dependency relationships
   - Aim for 8-12 crates (optimal for parallelization)
   - Group by layer: Foundation → Core → Features → Integration

3. **Create dependency graph**
   - Foundation crates (no internal deps): types, core utils
   - Mid-level crates: specialized tools, file system
   - Feature crates (can build in parallel): domain-specific functionality
   - Integration crates: high-level utilities, main entry point

### Phase 2: Create Workspace Structure

1. **Update root Cargo.toml**
   ```toml
   [workspace]
   resolver = '2'
   members = [
       "crates/my-types",
       "crates/my-utils",
       # ... more crates
   ]

   [workspace.package]
   version = "0.1.0"
   edition = "2024"

   [workspace.dependencies]
   # Move all shared dependencies here
   tokio = { version = "1.44.0", features = ["sync", "macros"] }
   serde = { version = "1.0", features = ["derive"] }
   # ... more deps
   ```

2. **Create crate directories**
   ```bash
   mkdir -p crates/{my-types,my-utils,my-core}
   ```

3. **Create Cargo.toml for each crate**
   ```toml
   [package]
   name = "my-types"
   version.workspace = true
   edition.workspace = true

   [dependencies]
   # Internal deps
   my-other-crate = { path = "../my-other-crate" }

   # Workspace deps
   serde = { workspace = true }
   tokio = { workspace = true }

   [lib]
   ```

4. **Create lib.rs for each crate**
   ```rust
   #![deny(clippy::disallowed_types)]

   // Add napi macros if using NAPI
   #[macro_use]
   extern crate napi_derive;

   pub mod my_module;
   pub use my_module::*;
   ```

### Phase 3: Move Code

1. **Copy files to new crates**
   ```bash
   # Example: moving types module
   cp -r src/native/types/* crates/my-types/src/
   ```

2. **Update import paths** (bulk operation)
   ```bash
   # Update all files to use new crate paths
   find crates -name "*.rs" -exec sed -i '' 's|crate::native::types::|my_types::|g' {} \;
   find crates -name "*.rs" -exec sed -i '' 's|use crate::native::utils::|use my_utils::|g' {} \;
   ```

3. **Fix module references**
   - Change `crate::native::*` → `crate::*` within each crate
   - Change `use crate::old_module` → `use my_crate::module` across crates
   - Update `pub(crate)` → `pub` for cross-crate APIs

### Phase 4: Resolve Issues

1. **Fix circular dependencies**
   - Split crates further if needed (e.g., utils → utils-core + utils)
   - Move shared code to foundation crates
   - Use dependency inversion if necessary

2. **Add missing dependencies**
   ```bash
   # Check for missing deps
   cargo build 2>&1 | grep "unresolved"

   # Add to Cargo.toml as needed
   ```

3. **Make private items public**
   - Change `pub(crate) fn` → `pub fn` for cross-crate functions
   - Change `pub(crate) type` → `pub type` for exported types
   - Keep internal-only items as `pub(crate)`

4. **Add macro imports**
   ```rust
   // For crates using #[napi]
   #[macro_use]
   extern crate napi_derive;
   ```

### Phase 5: Integration

1. **Update root package to use workspace crates**
   ```toml
   [package]
   name = "my-project"
   version.workspace = true
   edition.workspace = true

   [dependencies]
   my-types = { path = "crates/my-types" }
   my-utils = { path = "crates/my-utils" }
   # ... all crates

   [lib]
   crate-type = ['cdylib']  # For NAPI projects
   ```

2. **Update root lib.rs**
   ```rust
   // Re-export all crates
   pub use my_types as types;
   pub use my_utils as utils;

   // Backward compatibility layer
   pub mod native {
       pub use my_types as types;
       pub use my_utils as utils;
   }
   ```

3. **Configure linker for NAPI projects**
   ```toml
   # .cargo/config.toml at workspace root
   [target.x86_64-apple-darwin]
   rustflags = ["-C", "link-arg=-undefined", "-C", "link-arg=dynamic_lookup"]

   [target.aarch64-apple-darwin]
   rustflags = ["-C", "link-arg=-undefined", "-C", "link-arg=dynamic_lookup"]

   [target.x86_64-unknown-linux-gnu]
   rustflags = ["-Wl,--allow-shlib-undefined"]
   ```

### Phase 6: Verification

1. **Build all crates**
   ```bash
   cargo build --workspace
   ```

2. **Run tests**
   ```bash
   cargo test --workspace
   ```

3. **Verify build system integration**
   ```bash
   # For Nx projects
   nx build my-project
   ```

4. **Measure performance**
   ```bash
   # Clean build
   cargo clean
   time cargo build --workspace -j 8

   # Incremental build (change a leaf crate)
   touch crates/my-leaf/src/lib.rs
   time cargo build --workspace
   ```

## Common Issues & Solutions

### Circular Dependencies
**Problem**: Crate A depends on B, B depends on A
**Solution**:
- Split one crate into layers (A → A-core + A)
- Move shared code to foundation crate
- Use dependency inversion

### Missing NAPI Symbols
**Problem**: Linker errors about undefined NAPI functions
**Solution**:
- Move `.cargo/config.toml` to workspace root
- Add linker flags for undefined symbols (NAPI provided at runtime)

### Nested Workspace Conflicts
**Problem**: "multiple workspace roots found"
**Solution**:
- Use single workspace at repository root
- List all crates as members (even in subdirectories)
- Don't create nested `[workspace]` sections

### Version Conflicts
**Problem**: Multiple versions of same dependency
**Solution**:
- Use `workspace.dependencies` for shared deps
- Pin problematic versions (e.g., serde)
- Update all crates to use compatible versions

## Expected Results

### Performance Improvements
- **Parallel builds**: 3-5x faster (depends on crate count and dependencies)
- **Incremental builds**: 10-20x faster for leaf crate changes
- **CPU utilization**: 200-500% (2-5 cores active)

### Architecture Benefits
- Clear module boundaries
- Easier testing of individual components
- Better code organization
- Explicit dependency management

## Tips

1. **Start with foundation crates first** - they have no dependencies
2. **Use parallel agents** - spawn multiple agents to work on different crates
3. **Test frequently** - build after each crate to catch issues early
4. **Document decisions** - note why certain splits were made
5. **Keep commits atomic** - one commit per major phase

## Example Project Structure

```
my-project/
├── Cargo.toml                    # Workspace root
├── .cargo/
│   └── config.toml              # Linker configuration
├── src/
│   └── lib.rs                   # Main entry point, re-exports
└── crates/
    ├── my-types/                # Foundation
    │   ├── Cargo.toml
    │   └── src/lib.rs
    ├── my-utils-core/           # Core utilities
    │   ├── Cargo.toml
    │   └── src/lib.rs
    ├── my-feature-a/            # Features (parallel)
    │   ├── Cargo.toml
    │   └── src/lib.rs
    ├── my-feature-b/            # Features (parallel)
    │   ├── Cargo.toml
    │   └── src/lib.rs
    └── my-utils/                # High-level integration
        ├── Cargo.toml
        └── src/lib.rs
```

## Success Metrics

- ✅ All workspace crates compile independently
- ✅ Root crate builds and links successfully
- ✅ Build system integration works (nx build, cargo build, etc.)
- ✅ Tests pass
- ✅ Build time reduced by 50%+
- ✅ No logic changes, only code organization
