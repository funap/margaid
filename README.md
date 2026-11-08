# Margaid ⚡

A blazing-fast, 100% dependency-free Diagrams-as-Code compiler and CLI implemented in Rust & WebAssembly.

**English** | [日本語](README.ja.md)

**[✨ Try it Live!](https://funap.github.io/margaid/)**

---

## 🌟 What is Margaid?

**Margaid** is a high-performance Diagrams-as-Code compiler built to streamline how developers generate diagrams from plain text.

By eliminating heavy external runtimes like Java, Graphviz, or Node.js in favor of a pure, standalone Rust engine, Margaid compiles specifications directly into crisp vector graphics (SVG) in milliseconds. It runs both as a native CLI binary and directly in web browsers via WebAssembly—100% locally and offline.

### Key Highlights:

- **🚀 Zero External Dependencies**:
  - Standalone native binary CLI (`margaid input.txt -o output.svg`).
  - Web version runs by opening `index.html` directly in any web browser without needing a local web server.
- **🔒 Fully Private & Local**:
  - The entire parsing and SVG generation pipeline executes locally on your machine. No diagrams or sensitive architectural data leave your computer.
- **⚡ Blazing Fast**:
  - Native Rust execution delivers instant SVG compilation.

---

## 🎨 Supported Diagrams

Margaid supports 4 diagram formats and automatically detects the diagram type from the source text.

### 1. Sequence Diagrams
Visualize inter-system communications and API interaction lifecycles clearly.
- Actors (`actor`), participants (`participant`), boundaries (`boundary`)
- Control flow & branching (`alt` / `else`, `loop`, `opt`, `par`, `critical`, `group`)
- Lifeline activation (`activate` / `deactivate`)
- Notes (`note left`, `note right`, `note over`) and dividers (`== Phase ==`)

```
participant User
participant API
database DB

User -> API: GET /users
API -> DB: Query User
DB --> API: User Data
API --> User: 200 OK
```

### 2. Component Diagrams
Model software architecture, system boundaries, and module relationships.
- Components (`[Name]` or `component Name`)
- Package & group hierarchies (`package`, `node`, `folder`, `frame`, `cloud`, `database`)
- Ports (`portin`, `portout`) and typed relationships

```
package "Web Application" {
    [Frontend] --> [API Gateway]
}
database "Storage" {
    [API Gateway] --> [PostgreSQL]: SQL
}
```

### 3. GUI Wireframes (Salt UI)
Rapidly sketch application layouts and UI wireframes using plain text.
- Window frames, buttons (`[OK]`), text inputs (`"Input text"`)
- Checkboxes (`[X]`), radio buttons (`(X)`), dropdowns (`^Option^`)
- Grid layouts and tree views (`{T ... }`)

```
@startsalt
{
  [Login] | [Cancel]
  () Keep me signed in
  "Username"
  "Password"
}
@endsalt
```

### 4. Hardware & DIY Circuit Schematics (WireLayout)
Design microcontrollers, pinouts, and breadboard connections with orthogonal routing.
- Board definitions (`board <name>`), microcontrollers/ICs (`MCU`)
- Edge pin connectors (`@ edge.top`, `@ edge.right`)
- Bus lines (`-> bus <name>`) and automatic orthogonal wire routing

```
board UNO "Arduino Uno" {
  MCU "ATmega328P"
  USB "Type-B" @ edge.top
  LED "Red LED" @ edge.right
  USB -> MCU.top : power(5V)
  MCU.right -> LED.left : gpio(D13)
}
```

---

## 🛠️ Getting Started

### 1. Native CLI (No npm required)

Install using Cargo to make `margaid` accessible globally from any directory:

```bash
# Install to ~/.cargo/bin
cargo install --path crates/margaid-cli

# Compile file to SVG
margaid input.txt -o output.svg

# Output to stdout
margaid input.txt > output.svg

# Pipe from stdin
cat diagram.txt | margaid > output.svg

# Explicitly specify diagram type
margaid -t sequence input.txt -o output.svg
```

#### Run directly with Cargo:
```bash
cargo run -p margaid -- input.txt -o output.svg
```

#### Using repository runner scripts:
- **Windows:** `.\margaid.cmd input.txt -o output.svg`
- **macOS / Linux:** `./margaid input.txt -o output.svg`
- Or run the prebuilt binary `target/release/margaid.exe` / `target/release/margaid` directly.

#### Rebuilding browser bundles (dist/margaid.js):
If you modify Rust source code and want to recompile the browser WebAssembly & JavaScript bundles, no npm is needed:
```bash
cargo run -p margaid-build
# Or Windows: .\margaid.cmd --build-web / Unix: ./margaid --build-web
```

---

### 2. In the Browser (No npm required)

Double-click `index.html` to open the live editor and gallery in your browser. The WebAssembly module is bundled directly inside `dist/margaid.min.js`, so you don't even need a local HTTP server.

#### Embedding in HTML:
```html
<!-- Load Margaid -->
<script src="dist/margaid.min.js"></script>

<!-- Initialize auto-rendering -->
<script>
  margaid.initialize();
</script>

<!-- Plain text diagrams -->
<pre class="margaid">
Alice -> Bob: Hello Bob
Bob --> Alice: Hi Alice
</pre>
```

---

## 📄 License

MIT License
