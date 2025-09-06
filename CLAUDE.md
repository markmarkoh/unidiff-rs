# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **unidiff-rs**, a Rust library for parsing unified diff files and extracting metadata. The library provides a clean API to parse diffs from various version control systems (Git, SVN, Mercurial, Bazaar) and work with the structured data.

## Architecture

### Core Components

- **PatchSet**: Top-level container representing an entire diff/patch
- **PatchedFile**: Represents a single file's changes within a patch 
- **Hunk**: Individual modification blocks within a file
- **Line**: Individual lines of changes with metadata (added/removed/context)

### Key Data Flow
1. Raw diff text → `PatchSet::parse()` → structured data
2. PatchSet contains multiple PatchedFiles
3. Each PatchedFile contains multiple Hunks  
4. Each Hunk contains multiple Lines

The library uses regex patterns to parse diff headers and hunk content, with lazy static compilation for performance.

## Common Commands

### Building and Testing
```bash
# Check code without building
cargo check

# Build the library
cargo build

# Run all tests
cargo test

# Run specific test
cargo test test_parse_sample0_diff

# Run tests with output
cargo test -- --nocapture

# Format code
cargo fmt

# Run benchmarks
cargo bench
```

### Development Workflow
```bash
# Check formatting
cargo fmt -- --check

# Run all CI checks locally
cargo check && cargo test && cargo fmt -- --check
```

## Test Structure

Tests are located in `tests/` directory with fixture files in `tests/fixtures/`:
- Various diff formats: `git.diff`, `svn.diff`, `hg.diff`, `bzr.diff`
- Sample test cases: `sample0.diff` through `sample5.diff`
- Test modules: `test_patchset.rs`, `test_patchedfile.rs`, `test_hunk.rs`

## Features

The crate supports optional features:
- `encoding` (default): Enables encoding detection/conversion via `encoding_rs`
- `unstable`: Enables unstable/experimental features

## Library Usage Patterns

When working with this codebase, note that:
- All parsing is done through regex patterns defined as lazy statics
- The library handles various edge cases in diff formats
- File paths are normalized (strips `a/` and `b/` prefixes)
- Line numbers are tracked for both source and target files
- The API supports both owned and borrowed data patterns