# Repository Guidelines

## Project Structure & Module Organization

This is a Rust Cargo workspace implementing the `grok` terminal AI coding agent. Main code lives in:

- `crates/codegen/xai-grok-pager-bin` — binary composition root; the artifact is `xai-grok-pager`, installed as `grok`.
- `crates/codegen/xai-grok-pager` — TUI, rendering, modals, and user-guide documentation.
- `crates/codegen/xai-grok-shell` — agent runtime and interactive/headless entry points.
- `crates/codegen/xai-grok-tools`, `xai-grok-workspace`, and sibling `xai-grok-*` crates — tools, filesystem/VCS/execution, config, MCP, and related components.
- `crates/common/`, `crates/build/`, and `prod/mc/` — shared and support crates.
- `third_party/` — vendored upstream code; do not modify it casually.

The root `Cargo.toml` is generated and should be treated as read-only. Edit individual crate manifests instead.

## Build, Test, and Development Commands

Install the pinned Rust toolchain with `rustup`, and ensure [DotSlash](https://dotslash-cli.com) is available for `bin/protoc` (or provide `protoc` via `PATH`/`PROTOC`).

- `cargo check -p <crate>` — fast validation for one crate; prefer this over full-workspace checks.
- `cargo run -p xai-grok-pager-bin` — build and launch the TUI.
- `cargo build -p xai-grok-pager-bin --release` — create the release binary.
- `cargo test -p <crate>` — run a crate’s tests.
- `cargo clippy -p <crate>` — lint with repository and `crates/codegen` configurations.
- `cargo fmt --all` — apply Rust formatting.

## Coding Style & Naming Conventions

Use standard Rust formatting (`rustfmt.toml`) and idiomatic naming: crates use `xai-*` kebab-case; modules, functions, and variables use snake_case; types use PascalCase. Keep crate boundaries focused and avoid editing generated workspace-level metadata. Respect the crate-specific Clippy rules under `crates/codegen`.

## Testing Guidelines

Most tests use Rust’s built-in `#[cfg(test)]` modules; integration tests live in crate-local `tests/` directories. Name focused tests descriptively, for example `watcher_tests.rs` or `stored_auth_session_path.rs`. Add or update tests alongside behavioral changes and run the affected crate with `cargo test -p <crate>` before handoff.

## Commit & Pull Request Guidelines

History consists of automated `Synced from monorepo` commits, so no local commit convention can be inferred. This public tree is maintained by SpaceXAI for source transparency and local builds; external pull requests and unsolicited patches are not accepted. Security issues must follow `SECURITY.md` and must not be filed publicly.

## Agent-Specific Instructions

Before editing, identify the owning crate and target its narrow package. Preserve licensing and third-party notices, keep changes minimal, and validate with `cargo fmt`, targeted Clippy, and targeted tests.
