
# 🐳 Docker Image for TDLib

[![Docker Pulls](https://img.shields.io/docker/pulls/airani/tdlib.svg)](https://hub.docker.com/r/airani/tdlib)
[![GitHub issues](https://img.shields.io/github/issues/airani/tdlib)](https://github.com/airani/tdlib/issues)
[![GitHub license](https://img.shields.io/github/license/airani/tdlib)](https://github.com/airani/tdlib/blob/main/LICENSE)

A lightweight and up-to-date Docker image for the **[Telegram Database Library (TDLib)](https://github.com/tdlib/td)**. 

Compiling TDLib from source can be incredibly resource-intensive (often requiring lots of RAM and taking a long time). This repository automates the build process via GitHub Actions, providing you with ready-to-use, pre-compiled TDLib binaries inside a Docker container.

## ✨ Features

- **Pre-compiled & Ready to Use**: Skip the long compilation times.
- **Automated Builds**: Keeps up to date with the latest TDLib releases via GitHub Actions.
- **Multi-Stage Build Friendly**: Easily copy the built binaries into your own application's Docker image (Python, Node.js, Go, PHP, etc.).
- **Optimized Size**: Built to keep the final image footprint as small as possible.

## 🚀 Usage

### 1. Pulling the Image

You can pull the latest built image directly from Docker Hub (or GitHub Container Registry, depending on your setup):

```bash
docker pull airani/tdlib:latest

```

### 2. Using as a Base Image

You can use this image as a base for your own Telegram bots or clients.

```dockerfile
FROM airani/tdlib:latest

# Set your working directory
WORKDIR /app

# Copy your bot code
COPY . .

# Run your application (Example for a Python wrapper)
CMD ["python3", "main.py"]

```

### 3. Using in a Multi-Stage Build (Recommended)

If you only need the compiled `.so` (shared object) files to keep your final image minimal, use a multi-stage build to copy the binaries into your preferred runtime environment:

```dockerfile
# Stage 1: Get TDLib from the pre-built image
FROM airani/tdlib:latest AS tdlib

# Stage 2: Your actual application environment (e.g., Python, Node.js)
FROM python:3.10-slim

# Copy the compiled TDLib binary from the first stage
# (Adjust the source path depending on where it is built in the Dockerfile)
COPY --from=tdlib /usr/local/lib/libtdjson.so /usr/local/lib/libtdjson.so

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python3", "bot.py"]

```

## 🛠 Building the Image Locally

If you want to build the Docker image yourself on your local machine:

1. Clone the repository:
```bash
git clone https://github.com/airani/tdlib.git
cd tdlib

```


2. Build the Docker image:
```bash
docker build -t airani/tdlib:local .

```



*(Note: Ensure your Docker daemon has access to at least 4GB to 8GB of RAM, as `g++` compilation for TDLib is highly memory-intensive.)*

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!
Feel free to check the [issues page](https://www.google.com/url?sa=E&source=gmail&q=https://github.com/airani/tdlib/issues) if you want to contribute.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 🌐 Resources

- [TDLib Official Documentation](https://core.telegram.org/tdlib)
- [TDLib GitHub Repository](https://github.com/tdlib/td)

## 📝 License

Distributed under the MIT License. See `LICENSE` for more information. Note that TDLib itself is licensed under the Boost Software License.

---

**Maintained by [airani](https://github.com/airani)**


