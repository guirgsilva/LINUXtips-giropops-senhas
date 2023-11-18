# Trivy: Container Vulnerability Scanner

Trivy is an open-source tool for scanning vulnerabilities in container images. It can detect vulnerabilities in the operating system and application libraries. In this document, you will learn what Trivy is, how to install it, and how to use it to scan your container images.

## Table of Contents

- [What is Trivy?](#what-is-trivy)
- [Installing Trivy](#installing-trivy)
  - [Installation on macOS](#installation-on-macos)
  - [Installation on Ubuntu/Debian](#installation-on-ubuntudebian)
  - [Installation on CentOS/RHEL](#installation-on-centosrhel)
  - [Installation on Windows](#installation-on-windows)
- [How to use Trivy to scan container images](#how-to-use-trivy-to-scan-container-images)
- [GitHub Integration](#github-integration)

## What is Trivy?

Trivy is a simple and comprehensive vulnerability scanner for containers and other artifacts. It is capable of scanning container images to find vulnerabilities in the operating system and application libraries. Trivy is easy to use and can be integrated into your CI/CD pipeline to ensure your images are always secure.

## Installing Trivy

Follow the instructions below to install Trivy on your system:

### Installation on macOS

```bash
brew install aquasecurity/trivy/trivy

Installation on Ubuntu/Debian
sudo apt-get install -y wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo gpg --dearmor -o /usr/share/keyrings/trivy-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/trivy-archive-keyring.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy

Installation on CentOS/RHEL
sudo yum -y install epel-release
curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/install.sh | sh -s -- -b /usr/local/bin

Installation on Windows
Download the Trivy executable file for Windows from the Trivy releases page on GitHub and add it to your PATH.

How to use Trivy to scan container images
Pull the container image you want to scan. For example, to pull the giropops-senhas image:

docker pull giropops-senhas

Run Trivy to scan the image. Replace giropops-senhas with the name of the image you want to scan:

trivy image giropops-senhas

Analyze the results. Trivy will display a detailed report of the vulnerabilities found in the container image. Pay attention to high-risk vulnerabilities and follow the recommendations to fix them.

GitHub Integration
You can also integrate Trivy into your CI/CD pipeline on GitHub using the Trivy GitHub Action. This will allow you to automatically scan your container images whenever you make a change to your repository.

To add the Trivy GitHub Action to your repository, follow these steps:

Create a new file called .github/workflows/trivy.yml in your repository.

Add the following content to the trivy.yml file:

name: Trivy Vulnerability Scanner

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  scan:
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1

      - name: Build Docker image
        run: docker build -t giropops-senhas .

      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: 'giropops-senhas'
          exit-code: '1'

Make sure to replace giropops-senhas with the name of your container image.

Commit and push your changes to the repository. The Trivy GitHub Action will automatically run whenever you make a change to your repository.

Now you are ready to use Trivy to scan your container images and ensure they are free from known vulnerabilities.
