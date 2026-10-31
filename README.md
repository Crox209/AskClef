# AskClef

**Ask a question. Explore your options.**

AskClef is a desktop app for structured decisions with a locally configured model server. Ask a yes/no question, compare multiple choices, or score an option against a scale. Add context and supported files, then inspect the answer and its estimated probabilities.

## Features

- **Three answer formats:** Yes/No, multiple choice (2–26 options), and score (2–10 levels).
- **Context and attachments:** Add background text and attach UTF-8 TXT, PNG, JPEG, or WebP files. On macOS, DOC and DOCX files are also supported through the system `textutil` tool. Files can be selected or dragged into the window.
- **Model connection controls:** Choose Ollama or LM Studio, set the server address, refresh its model list, and enter a model name manually.
- **Optional memory:** Save successful decisions locally and include recent decisions in later questions. Set a retention limit or wipe memory at any time.
- **Exports:** Save results as a text report, CSV, or SVG probability graph. Copy the current result to the clipboard.
- **Appearance:** Choose an accent color.

Probabilities are estimates returned by the configured model, not verified facts or calibrated confidence scores.

## Download and run

The release assets are built separately for each platform:

| Platform | Release asset | How to run |
| --- | --- | --- |
| Windows 64-bit | `AskClef-Windows-x64.exe` | Download and open the executable. |
| macOS Apple Silicon | `AskClef-macOS-AppleSilicon.zip` | Unzip, then open `AskClef.app`. |

The macOS app is not Apple notarized, so macOS may show a security prompt for a downloaded copy. The current macOS release is for Apple Silicon; an Intel Mac build is not included.

AskClef does not include a model or server. Start and configure a compatible server before asking a question.

## Server compatibility

AskClef lists models using Ollama's `/api/tags` endpoint or LM Studio's `/v1/models` endpoint. To answer questions, it sends a structured JSON request to **`/v1/systemone`** and expects a decision response containing probabilities. A standard Ollama or LM Studio installation may not provide this endpoint by itself. Use a compatible decision model or server adapter; simply selecting a listed chat model does not guarantee compatibility.

The default connection is Ollama at `http://localhost:11434`. LM Studio defaults to `http://localhost:1234`. Enter the server's base address in Settings, without appending `/v1`.

## Getting started

1. Start a compatible local server and load a decision model.
2. Open AskClef and go to **Settings**.
3. Select Ollama or LM Studio, enter the server address, and choose or type the model name.
4. Click **Check connection / refresh models**.
5. Enter a question and choose **Yes / No**, **Multiple Choice**, or **Score**. Add context or files if useful.
6. Click **Ask**. Enable exports in Settings if you want to save the result.

For multiple choice, put one unique option on each line. For scoring, put the levels in order from lowest to highest; five levels produce a 0–4 score.

## Build from source

Install a Rust toolchain that supports the 2024 edition, then run:

```sh
cargo run --release --locked
```

To build without launching:

```sh
cargo build --release --locked
```

The binary will be in `target/release/`. Windows builds require a Windows Rust target and linker. A macOS `.app` also needs a bundle containing the binary, an `Info.plist`, and an icon; the raw Cargo binary is not the release bundle. Generated build files in `target/` are excluded from Git.

Run the included tests with:

```sh
cargo test --locked
```

## Keyboard shortcuts

Use **Command** on macOS or **Ctrl** on Windows.

| Shortcut | Action |
| --- | --- |
| Command/Ctrl + Enter | Ask |
| Command/Ctrl + O | Attach files |
| Command/Ctrl + , | Open Settings |
| Command/Ctrl + E | Export result, when enabled |
| Command/Ctrl + Shift + C | Copy result |

## Data and platform notes

- Settings and optional decision memory are stored locally by the desktop app. Memory can include questions, context, answers, and extracted document text; image bytes are not retained in memory.
- Attached files are sent to the configured server as part of the current request. Use a local server address if you want requests to stay on your computer.
- On Windows, save DOC or DOCX files as UTF-8 TXT before attaching them. DOC and DOCX conversion currently requires macOS.
- Scanned documents should be attached as images; document extraction reads text and does not preserve page layout or embedded images.

## Project structure

```text
src/main.rs       Desktop app, server requests, exports, and tests
src/Icon.png      App icon
src/ClefLogo.png  In-app logo
Cargo.toml        Rust package and dependencies
Cargo.lock        Locked dependency versions
```
