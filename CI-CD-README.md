# Chat App Frontend - CI/CD Pipeline

This repository includes a complete CI/CD pipeline using GitHub Actions for automated testing, building, and deployment of the chat application frontend.

## 🚀 CI/CD Pipeline Overview

The pipeline consists of three main stages:

### 1. **Test Stage**
- ✅ Code checkout
- ✅ Node.js 20 setup with npm caching
- ✅ Dependency installation
- ✅ ESLint code quality checks
- ✅ TypeScript type checking
- ✅ Application build verification
- ✅ Test execution (if available)

### 2. **Build and Push Stage**
- ✅ Docker image building with multi-platform support (AMD64/ARM64)
- ✅ Container registry authentication
- ✅ Image tagging with multiple strategies
- ✅ Push to GitHub Container Registry (ghcr.io)
- ✅ Build caching for faster subsequent builds

### 3. **Deploy Stage**
- ✅ Production deployment (main branch only)
- ✅ Deployment status reporting

## 📋 Workflow Triggers

The pipeline runs automatically on:
- **Push to main branch** - Full CI/CD pipeline including deployment
- **Pull requests to main** - CI pipeline (test + build) without deployment

## 🐳 Docker Configuration

### Multi-stage Dockerfile
- **deps**: Production dependency installation
- **builder**: Development dependencies + application build
- **runner**: Optimized production image with security best practices

### Key Features
- ✅ Alpine Linux base for smaller image size
- ✅ Non-root user execution for security
- ✅ Next.js standalone output for optimal performance
- ✅ Proper file permissions and ownership

## 📦 Container Registry

Images are automatically pushed to:
```
ghcr.io/[your-username]/[repository-name]
```

### Image Tags
- `latest` - Latest main branch build
- `main` - Current main branch
- `main-[commit-sha]` - Specific commit builds
- `pr-[number]` - Pull request builds

## 🔧 Configuration Files

### `.github/workflows/deploy.yml`
Complete CI/CD pipeline configuration with:
- Automated testing and quality checks
- Multi-platform Docker builds
- Container registry integration
- Production deployment automation

### `.dockerignore`
Optimized Docker build context excluding:
- Development files
- Documentation
- Git history
- IDE configurations

### `package.json`
Updated with required scripts:
- `type-check` - TypeScript validation
- `lint` - Code quality checks
- `build` - Production build

## 🚀 Usage

### Manual Deployment
```bash
# Build and run locally
docker build -t chatapp-fe .
docker run -p 3000:3000 chatapp-fe
```

### Automated Deployment
1. Push code to main branch
2. GitHub Actions automatically:
   - Runs tests and quality checks
   - Builds Docker image
   - Pushes to container registry
   - Deploys to production

### Pull Request Workflow
1. Create pull request to main branch
2. GitHub Actions automatically:
   - Runs tests and quality checks
   - Builds Docker image for verification
   - Provides build status feedback

## 🔍 Monitoring

### Build Status
Check the "Actions" tab in your GitHub repository to monitor:
- Pipeline execution status
- Build logs and errors
- Deployment status

### Container Registry
View published images at:
```
https://github.com/[your-username]/[repository-name]/pkgs/container/[repository-name]
```

## 🛠️ Customization

### Adding Tests
Add test scripts to `package.json`:
```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch"
  }
}
```

### Environment Variables
Add secrets in GitHub repository settings:
- `GITHUB_TOKEN` - Automatically provided
- Custom deployment tokens as needed

### Deployment Targets
Modify the deploy job in `.github/workflows/deploy.yml` to integrate with:
- AWS ECS/Fargate
- Google Cloud Run
- Azure Container Instances
- Kubernetes clusters
- Docker Swarm

## 📊 Benefits

- ✅ **Automated Quality Assurance** - Every commit is tested
- ✅ **Consistent Builds** - Reproducible Docker images
- ✅ **Multi-platform Support** - AMD64 and ARM64 compatibility
- ✅ **Security** - Non-root containers, minimal attack surface
- ✅ **Performance** - Optimized Next.js standalone builds
- ✅ **Scalability** - Ready for container orchestration platforms

## 🔗 Integration Examples

### Kubernetes Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: chatapp-frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: chatapp-frontend
  template:
    metadata:
      labels:
        app: chatapp-frontend
    spec:
      containers:
      - name: chatapp-frontend
        image: ghcr.io/[username]/[repo]:latest
        ports:
        - containerPort: 3000
```

### Docker Compose
```yaml
version: '3.8'
services:
  frontend:
    image: ghcr.io/[username]/[repo]:latest
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
```

This CI/CD pipeline provides a robust, automated workflow for your chat application frontend, ensuring code quality, security, and reliable deployments.
