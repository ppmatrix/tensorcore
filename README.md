# TensorCore

A Docker Compose setup for running local AI models using Ollama with Open WebUI interface and NVIDIA GPU acceleration.

## Overview

This project provides a containerized environment for:
- **Ollama**: Local AI model hosting and inference
- **Open WebUI**: Modern web interface for interacting with AI models
- **NVIDIA GPU Support**: Hardware acceleration for model inference
- **Image Generation**: Integration with Automatic1111 for image generation

## Features

- 🚀 Easy deployment with Docker Compose
- 🔧 NVIDIA GPU acceleration support
- 🖼️ Image generation capabilities via Automatic1111 integration
- 🌐 Web-based interface for model interaction
- 📦 Persistent data storage for models and conversations
- 🔄 Automatic service orchestration and health checks

## Prerequisites

- Docker and Docker Compose installed
- NVIDIA GPU with CUDA support
- NVIDIA Container Runtime configured
- Linux system (tested on Ubuntu)

### NVIDIA Container Runtime Setup

Make sure you have the NVIDIA Container Runtime installed:

```bash
# Install NVIDIA Container Runtime
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update && sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```

## Quick Start

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ppmatrix/tensorcore.git
   cd tensorcore
   ```

2. **Start the services:**
   ```bash
   docker compose up -d
   ```

3. **Access the web interface:**
   - Open WebUI: http://localhost:3000
   - Ollama API: http://localhost:11434

4. **Download AI models:**
   ```bash
   # Example: Download Llama 2 model
   docker exec -it ollama ollama pull llama2
   
   # Or any other model
   docker exec -it ollama ollama pull mistral
   ```

## Services

### Ollama
- **Port**: 11434
- **Purpose**: AI model hosting and API
- **GPU**: NVIDIA GPU acceleration enabled
- **Storage**: Persistent volume for models (`ollama_data`)

### Open WebUI
- **Port**: 3000 (mapped from container port 8080)
- **Purpose**: Web interface for AI interactions
- **Features**: Chat interface, model management, conversation history
- **Storage**: Persistent volume for user data (`openwebui_data`)

### Wait-for-Ollama
- **Purpose**: Service orchestration helper
- **Function**: Ensures Ollama is ready before starting Open WebUI

## Configuration

### Image Generation

The setup includes configuration for image generation via Automatic1111:

- **Engine**: Automatic1111
- **URL**: http://host.docker.internal:7860/sdapi/v1
- **Image Size**: 512x512
- **Steps**: 20

To use image generation, make sure Automatic1111 is running on your host system.

### Environment Variables

Key environment variables in the docker-compose.yml:

- `NVIDIA_VISIBLE_DEVICES=all`: Enables all NVIDIA GPUs
- `OLLAMA_API_BASE_URL`: Points Open WebUI to Ollama API
- `ENABLE_IMAGE_GENERATION=true`: Enables image generation features

## Usage

### Managing Models

List available models:
```bash
docker exec -it ollama ollama list
```

Pull a new model:
```bash
docker exec -it ollama ollama pull <model-name>
```

Remove a model:
```bash
docker exec -it ollama ollama rm <model-name>
```

### Accessing Logs

View logs for all services:
```bash
docker compose logs -f
```

View logs for a specific service:
```bash
docker compose logs -f ollama
docker compose logs -f open-webui
```

## Data Persistence

The setup uses Docker volumes for data persistence:

- `ollama_data`: Stores downloaded AI models
- `openwebui_data`: Stores user conversations and settings

## Troubleshooting

### GPU Not Detected

If GPU acceleration isn't working:

1. Verify NVIDIA drivers are installed:
   ```bash
   nvidia-smi
   ```

2. Check Docker can access GPU:
   ```bash
   docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
   ```

3. Ensure NVIDIA Container Runtime is configured in Docker daemon.

### Port Conflicts

If ports 3000 or 11434 are already in use, modify the port mappings in `docker-compose.yml`:

```yaml
ports:
  - "3001:8080"  # Change 3000 to 3001
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project is open source. See individual service licenses for details.

## Support

For issues and questions:
- Check the troubleshooting section above
- Review Docker Compose logs
- Consult Ollama and Open WebUI documentation

---

**Note**: This setup is designed for development and personal use. For production deployments, consider additional security configurations and resource management.
