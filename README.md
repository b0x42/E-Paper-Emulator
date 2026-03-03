# E-Paper Emulator

[![Python 3](https://img.shields.io/badge/Python-3-blue.svg)](https://www.python.org/)
[![Pillow](https://img.shields.io/badge/Pillow-Image%20Processing-yellow.svg)](https://python-pillow.org/)
[![Flask](https://img.shields.io/badge/Flask-Web%20Server-lightgrey.svg)](https://flask.palletsprojects.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A drop-in replacement for the Waveshare E-Paper Display (EPD) Python library that renders to a Tkinter window or Flask web server instead of hardware. Develop, test, and demo e-paper UIs on any machine — no display module required. Supports 37+ models in color and monochrome.

- **Drop-in Replacement**: Use the same API as the Waveshare EPD library for seamless development
- **37+ EPD Models**: Pre-configured JSON files for a wide range of Waveshare displays
- **Dual Rendering Modes**: Tkinter (native GUI window) or Flask (web-based, accessible via browser)
- **Color and Monochrome**: Simulate both color and monochrome e-paper displays
- **Drawing API**: Text, rectangles, lines, ellipses, and image pasting

| Flask (Browser) | Tkinter (Desktop) |
|:---:|:---:|
| ![Flask](screenshots/screenshot_flask.png) | ![Tkinter](screenshots/screenshot_tkinter.png) |

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Configuration](#configuration)
- [Supported Display Models](#supported-display-models)
- [File Structure](#file-structure)
- [Credits](#credits)
- [License](#license)


## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/b0x42/E-Paper-Emulator.git
cd E-Paper-Emulator
```

### 2. Install Dependencies

```bash
pip install .
```

For development (includes pytest and flake8):

```bash
pip install -e ".[dev]"
```


## Usage

### Run the Demo

```bash
python demo.py
```

The demo script shows how to initialize the emulator, draw shapes and text, and display images.

### Use in Your Own Project

```python
from epaper_emulator.emulator import EPD

# Initialize with your preferred settings
epd = EPD(
    config_file="epd2in13",
    use_tkinter=False,
    use_color=True,
    update_interval=5,
    reverse_orientation=False
)
epd.init()
epd.Clear(255)

# Draw content
draw = epd.draw
draw.text((10, 10), "Hello EPD!", font=font, fill=0)
draw.rectangle((0, 0, epd.width - 1, epd.height - 1), outline=0)

# Display
image_buffer = epd.get_frame_buffer(draw)
epd.display(image_buffer)
```

### Rendering Modes

- **Flask (default)**: Opens `http://127.0.0.1:5000/` in your browser. Set `use_tkinter=False`.
- **Tkinter**: Opens a native desktop window. Set `use_tkinter=True`.


## Configuration

All EPD parameters are set when initializing the emulator:

| Parameter | Type | Description | Default |
|-----------|------|-------------|---------|
| `config_file` | `str` | EPD model name (matches JSON filename in `config/`) | `epd2in13` |
| `use_tkinter` | `bool` | `True` for native GUI, `False` for Flask web server | `False` |
| `use_color` | `bool` | `True` for RGB color, `False` for monochrome | `False` |
| `update_interval` | `int` | Refresh delay in seconds | `2` |
| `reverse_orientation` | `bool` | Swap width and height | `False` |
| `port` | `int` | Flask server port number | `5000` |

### EPD Model Configuration

Each display model is defined by a JSON file in `epaper_emulator/config/`:

```json
{
    "name": "epd2in13",
    "width": 122,
    "height": 250,
    "color": "white",
    "text_color": "black"
}
```


## Supported Display Models

| Model | Resolution | Size |
|-------|-----------|------|
| `epd1in02` | 128x80 | 1.02" |
| `epd1in54` | 200x200 | 1.54" |
| `epd1in54b` | 200x200 | 1.54" (B, red/black/white) |
| `epd1in54c` | 152x152 | 1.54" (C, yellow/black/white) |
| `epd2in13` | 122x250 | 2.13" |
| `epd2in13bc` | 122x250 | 2.13" (B/C) |
| `epd2in13d` | 104x212 | 2.13" (D) |
| `epd2in13v2` | 122x250 | 2.13" (V2) |
| `epd2in66` | 296x152 | 2.66" |
| `epd2in7` | 176x264 | 2.7" |
| `epd2in9` | 128x296 | 2.9" |
| `epd2in9b` | 128x296 | 2.9" (B, red/black/white) |
| `epd3in52` | 320x320 | 3.52" |
| `epd3in7` | 280x480 | 3.7" |
| `epd4in2` | 400x300 | 4.2" |
| `epd4in2b` | 400x300 | 4.2" (B, red/black/white) |
| `epd4in26` | 800x480 | 4.26" |
| `epd4in3` | 800x600 | 4.3" |
| `epd5in65` | 600x448 | 5.65" (F, 7-color) |
| `epd5in79` | 792x272 | 5.79" |
| `epd5in79b` | 792x272 | 5.79" (B, red/black/white) |
| `epd5in83` | 600x448 | 5.83" |
| `epd6in0` | 800x600 | 6.0" |
| `epd6in2` | 640x384 | 6.2" |
| `epd7in3f` | 800x480 | 7.3" (F, 7-color ACeP) |
| `epd7in5` | 800x480 | 7.5" |
| `epd7in5b` | 800x480 | 7.5" (B, red/black/white) |
| `epd7in5hd` | 880x528 | 7.5" HD |
| `epd7in8` | 1872x1404 | 7.8" |
| `epd9in7` | 1200x825 | 9.7" |
| `epd10in2g` | 960x640 | 10.2" (G, color) |
| `epd10in3` | 1872x1404 | 10.3" |
| `epd11in6` | 960x640 | 11.6" |
| `epd12in48` | 1304x984 | 12.48" |
| `epd13in3` | 1600x1200 | 13.3" |
| `epd13in3b` | 960x680 | 13.3" (B, red/black/white) |
| `epd13in3k` | 960x680 | 13.3" (K) |


## File Structure

```
E-Paper-Emulator/
├── epaper_emulator/              # Main package
│   ├── __init__.py               # Package entry point
│   ├── emulator.py               # Core EPD emulator class
│   └── config/                   # EPD model JSON configurations
│       ├── epd1in54.json
│       ├── epd2in13.json
│       ├── ...
│       └── epd12in48.json
├── tests/                        # Test suite
│   ├── __init__.py
│   ├── test_config.py
│   └── test_epd.py
├── screenshots/                  # Generated screenshot assets
│   └── generate_cat_screenshots.py
├── .github/                      # GitHub templates and workflows
├── demo.py                       # Demo script
├── pyproject.toml                # Python packaging configuration
├── AGENTS.md                     # Project context for AI agents
├── CONTRIBUTING.md               # Contribution guide
├── SECURITY.md                   # Security policy
└── LICENSE                       # MIT License
```


## Credits

- Original project: [infinition/EPD-Emulator](https://github.com/infinition/EPD-Emulator)
- E-Paper hardware: [Waveshare](https://www.waveshare.com/)

## License

MIT License - see [LICENSE](LICENSE)
