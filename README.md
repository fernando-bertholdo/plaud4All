# plaud4All

Open-source self-hosted alternative to Plaud Note for recording, transcribing, and organizing voice recordings using your phone.

## Overview

plaud4All is a pipeline that:
1. Records audio on your iPhone using Apple Shortcuts
2. Automatically saves recordings to iCloud Drive
3. Monitors the folder and transcribes new recordings using Gemini API (with support for Whisper/Parakeet)
4. Processes and organizes transcriptions in Notion

## Features

- **Category-based Recording**: Choose recording type (meeting, class, lecture, chat, etc.)
- **Automatic Transcription**: Powered by Gemini API with diarization support
- **BYOK (Bring Your Own Key)**: Use your own API keys or local models
- **Notion Integration**: Organized database with searchable transcriptions
- **Flexible Architecture**: Extensible to support multiple transcription backends

## Project Structure

```
plaud4All/
├── src/
│   ├── watcher/           # Folder monitoring service
│   ├── transcription/     # Transcription services (Gemini, Whisper, etc.)
│   ├── notion/            # Notion integration
│   ├── queue/             # Processing queue management
│   └── utils/             # Shared utilities
├── config/
│   ├── config.yaml        # Application configuration
│   └── .env.example       # Environment variables template
├── shortcuts/             # iPhone Shortcut documentation and files
├── docs/                  # Documentation
├── tests/                 # Test suite
└── scripts/               # Utility scripts
```

## Quick Start

### Prerequisites

- Python 3.9+
- iPhone with iCloud Drive
- Notion account
- Gemini API key (or OpenAI/local model)

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/plaud4All.git
cd plaud4All

# Install dependencies
pip install -r requirements.txt

# Copy and configure environment variables
cp config/.env.example config/.env
# Edit config/.env with your API keys and settings
```

### Configuration

1. Set up the iPhone Shortcut (see `shortcuts/README.md`)
2. Configure your API keys in `config/.env`
3. Set up Notion database (see `docs/notion-setup.md`)
4. Configure folder paths in `config/config.yaml`

### Running

```bash
# Start the watcher service
python -m src.main
```

## Roadmap

### MVP (Current Focus)
- [x] Project structure
- [ ] iPhone Shortcut setup
- [ ] Folder watcher implementation
- [ ] Gemini transcription integration
- [ ] Notion database creation and publishing
- [ ] Basic error handling and logging

### Future Enhancements
- [ ] Local Whisper support
- [ ] Parakeet TTS integration
- [ ] Advanced LLM summaries and insights
- [ ] Category-specific processing templates
- [ ] Web dashboard
- [ ] Android support (Tasker/Routines)
- [ ] Multiple storage backends (Google Drive, Dropbox)

## License

MIT License - see LICENSE file

## Contributing

Contributions welcome! Please read CONTRIBUTING.md for guidelines.
