# Link Tree Clone

A simple Link Tree clone built with HTML, CSS, and served via Nginx in a Docker container. This project demonstrates deploying a static website to Kubernetes using GitHub Actions.

## Features

- Static HTML/CSS website
- Docker containerized with Nginx
- Automated deployment to Kubernetes via GitHub Actions
- HTTPS enabled with Let's Encrypt (cert-manager)

## Local Development

1. Clone the repository
2. Run a local server from the `src/` directory:
   ```bash
   cd src
   python3 -m http.server 8000
   ```
3. Visit `http://localhost:8000`

## Deployment

The project is automatically deployed to Kubernetes when changes are pushed to the `main` branch via GitHub Actions.

### Prerequisites

- Kubernetes cluster with:
  - NGINX Ingress Controller
  - cert-manager installed
- Docker Hub account
- Domain pointing to your ingress IP (currently configured for `linktree.ja-zum-leben.at`)

### GitHub Secrets

Set the following secrets in your GitHub repository (Settings > Secrets and variables > Actions):

- `DOCKER_USERNAME`: Your Docker Hub username
- `DOCKER_PASSWORD`: Your Docker Hub password or access token
- `KUBE_CONFIG`: Base64-encoded kubeconfig (run `cat ~/.kube/config | base64 -w 0` locally)

### Manual Deployment

If needed, you can deploy manually:

1. Build and push the Docker image:
   ```bash
   docker build -t blisha234/link-tree-clone:latest .
   docker push blisha234/link-tree-clone:latest
   ```

2. Apply Kubernetes manifests:
   ```bash
   kubectl apply -f k8s/
   kubectl rollout restart deployment/link-tree-clone
   ```

## Project Structure

- `src/`: Source files
  - `index.html`: Main HTML file
  - `style.css`: CSS styles
- `k8s/`: Kubernetes manifests
  - `deployment.yaml`: Kubernetes deployment
  - `service.yaml`: Kubernetes service
  - `ingress.yaml`: NGINX ingress configuration
- `Dockerfile`: Docker build configuration
- `.github/workflows/deploy.yml`: GitHub Actions workflow
- `README.md`: This file
- `.gitignore`: Git ignore rules

## License

This project is open source. Feel free to use and modify as needed.