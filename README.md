# CID 702 · CM 7203 — Videoterminal Emulator

> A browser-based emulator of the first alphanumeric video terminal mass‑produced in Cuba (1978), internationally designated as the **SM‑7203 / CM 7203** within the COMECON computing system.

<p align="center"><img width="1280" src="https://github.com/maikelsz/CID702/blob/main/pictures/CID702_emu.png" /></p>

[![Single File](https://img.shields.io/badge/single--file-HTML%2FCSS%2FJS-brightgreen.svg)](index.html)
[![No Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen.svg)]()
[![Language](https://img.shields.io/badge/lang-ES%20%7C%20EN-blue.svg)](#)

---

## Table of Contents
- [About](#about)
- [Live Demo](#live-demo)
- [Features](#features)
- [Quick Start](#quick-start)
- [Technical Specifications](#technical-specifications)
- [Architecture](#architecture)
- [Documentation](#documentation)
- [Browser Compatibility](#browser-compatibility)
- [Historical Background](#historical-background)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Credits](#credits)
- [License](#license)
- [Español](#español)

---

## About

The **CID 702** was designed at the **Instituto Central de Investigación Digital (ICID)** in Havana and mass‑produced starting in 1978. Within the COMECON computing family it was catalogued as the **SM‑7203** (and later as the **CM 7203**), becoming the first Cuban technological product exported internationally.

<p align="center"><img width="1280" src="https://github.com/maikelsz/CID702/blob/main/pictures/CID702_2.jpg" /></p>

This project recreates that terminal in the browser — faithfully reproducing its 72×20 character display, KOI‑7Н Cyrillic keyboard layout, and the three original communication interfaces — so that its behaviour can be studied, preserved, and enjoyed without access to the original hardware.

---

## Historical Background

The CID 702 was developed by the **ICID** in Havana as the first peripheral designed to work with the Cuban **CID 300/10** minicomputer. In 1978 it passed joint international tests in Severodonetsk (USSR) and entered mass production. Within COMECON it was catalogued as the **SM‑7203**, and Cuba became the first socialist country to export microcomputer peripherals.

The original hardware used an **Intel 8275 CRT controller** and **8279 keyboard controller**, with a 1440‑character buffer and support for KOI‑7Н Cyrillic character generation.

For the full history, see [docs/history.md](docs/history.md).

---

## Live Demo

🔗 **[Try it online](https://[TU-USUARIO].github.io/cid-702-emulator/)** *(deploy via GitHub Pages)*

---

## Features

- **Fully faithful screen**: 72 × 20 character grid (1440 cells) with CRT scanline simulation and phosphor glow.
- **KOI‑7Н keyboard**: physical Cyrillic sub‑labels with phonetic mapping (`A → А`, `J → Й`, …).
- **LOCAL / LINE modes**: semiduplex transmission with proper buffering and flush semantics.
- **Three communication interfaces** simulated with real timing:
  - `IRPR` — parallel, instantaneous
  - `S2` — serial V.24 / RS‑232, ~9600 baud
  - `IRPS` — current loop, ~9600 baud
- **Integrated host panel** with TX/RX log and quick‑command buttons.
- **Audio feedback**: distinct sounds for key clicks and switch toggles.
- **Physical keyboard support** with visual feedback on the virtual keys.
- **Single self‑contained file** — no build step, no dependencies, works offline.
- **Bilingual documentation** (Spanish + English).

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/[TU-USUARIO]/cid-702-emulator.git

# Open it in your browser
cd cid-702-emulator
open index.html       # macOS
xdg-open index.html   # Linux
start index.html      # Windows
```

Or simply download `index.html` and open it in any modern browser. **No server required.**

---

## Technical Specifications

Emulated hardware features (from the original CM 7203 documentation):

| Specification | Value |
|---|---|
| Screen | 20 rows × 72 characters (1440 cells) |
| Character generator | 64 ASCII + 160 Latin/Cyrillic + 64 graphics |
| Attributes | Blink, reverse video, 7 colours |
| Internal buffer | 1440 characters |
| Encoding | ASCII / KOI‑7Н |
| Interfaces | IRPR (parallel), S2 (serial V.24), IRPS (current loop) |
| Original power | 110/220 V, 60/50 Hz, 200 W |
| Original weight | 22 kg (without keyboard) / 25 kg (with keyboard) |

---

## Architecture

The emulator is a **single HTML file** containing all the code:

```
index.html
├── <style>        CSS — CRT simulation, keyboard layout, panels
└── <script>       Vanilla JavaScript (no framework)
    ├── T          Global terminal state
    ├── KEY_MAP    Physical → virtual key mapping
    ├── writeChar  Character rendering with LINE‑mode buffering
    ├── render*    DOM rendering (cached cells, O(1) updates)
    └── hostSend   Simulated host → terminal transmission
```

Key design decisions:
- **Cell cache** (`CELL_CACHE`) — O(1) DOM access, no `querySelector` on the hot path.
- **Dirty‑check caching** for the status bar — avoids redundant writes.
- **Debounced transmission** (`scheduleTxFlush`) — batches keystrokes, avoiding log spam.
- **One‑shot modifiers** for physical Shift/Ctrl; **persistent toggles** only from mouse clicks.

---

## Documentation

- 📖 [**User Manual**](docs/user-manual.md) — complete walkthrough in Spanish and English.
- ⌨️ [**Keyboard Reference**](docs/keyboard-reference.md) — every key, character, and function.
- 🔌 [**Communication Protocols**](docs/communication-protocols.md) — IRPR / S2 / IRPS technical notes.
- 🕰️ [**Historical Context**](docs/history.md) — the real CID 702 / CM 7203 history.

---

## Browser Compatibility

Tested on:

| Browser | Status |
|---|---|
| Chrome / Edge (Chromium ≥ 90) | ✅ Full support |
| Firefox ≥ 88 | ✅ Full support |
| Safari ≥ 14 | ✅ Full support |
| Mobile (Chrome/Safari) | ⚠️ Works, but designed for desktop |

Requires `AudioContext` and modern ES2017+ syntax.

---

## Roadmap

- [ ] Cell‑level colour attributes (7 colours) with palette selector
- [ ] Blink and reverse‑video per‑cell rendering
- [ ] Realistic handshake (RTS/CTS/XON/XOFF) between host and terminal
- [ ] Responsive scaling for small viewports
- [ ] Integration with the [CID 201‑B emulator](https://github.com/[TU-USUARIO]/cid-201b-emulator) as a full system

---

## Contributing

Contributions are welcome. Areas that need help:

- **Historical accuracy** — if you have access to original CM 7203 manuals, schematics, or keyboard layouts, please open an issue.
- **Audio samples** — recorded sounds from the real hardware would replace the synthesized beeps.
- **Translations** — Russian and English corrections are always welcome.
- **Bug reports** — open an issue with your browser, OS, and a description of what happened.

Before submitting a PR, please read [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Credits

- **Historical research**:  
  - Instituto Central de Investigación Digital (ICID) archives  
- **Related projects**:
  - [CID 201‑B emulator](https://github.com/[TU-USUARIO]/cid-201b-emulator) — the minicomputer that originally drove the CID 702

---

## License

This project is released under the **[MIT License](LICENSE)**.

*(You may choose a different license — Apache‑2.0 or GPL‑3.0 are also common for historical preservation projects. Just update this section and the LICENSE file consistently.)*

---

## Español

**CID 702 · CM 7203 — Emulador de videoterminal en el navegador**

Emulador del primer terminal de video alfanumérico producido en serie en Cuba (1978), conocido internacionalmente como **SM‑7203 / CM 7203**. Reproduce fielmente su pantalla de 72×20 caracteres, el teclado con sub‑etiquetas cirílicas (KOI‑7Н) y las tres interfaces de comunicación originales (IRPR, S2, IRPS).

**Para usarlo**: descarga `index.html` y ábrelo en cualquier navegador moderno. No requiere instalación, servidor ni dependencias.

**Documentación completa**: [docs/user-manual.md](docs/user-manual.md)

**Contribuciones**: agradecemos cualquier aporte sobre historia, grabaciones de audio originales o correcciones del manual. Abre un *issue* o un *pull request*.

---

<p align="center">
  <sub>Hecho con respeto por la historia tecnológica cubana · 1978–2025</sub>
</p>
