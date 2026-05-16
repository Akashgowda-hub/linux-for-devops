# CI/CD Basics

CI/CD automates software build, testing, and deployment, enabling faster and more reliable releases.

---

# CI (Continuous Integration)

Continuous Integration is the practice of frequently integrating code changes into a shared repository.

## Key Practices

- **Commit code frequently** (daily or multiple times per day)
- **Automated builds** after each commit
- **Run automated tests** to detect issues early
- **Quick feedback** to developers on build success/failure
- **Maintain code quality** standards

## Benefits

- Early bug detection
- Reduced integration issues
- Faster development cycles
- Better code quality

---

# CD (Continuous Delivery/Deployment)

### Continuous Delivery

Code is automatically built and tested, ready for deployment but requires manual approval.

### Continuous Deployment

Code is automatically deployed to production without manual intervention.

## Key Practices

- **Automated testing** (unit, integration, end-to-end)
- **Automated deployment** to staging/production
- **Rollback capabilities** for failed deployments
- **Infrastructure as Code** for consistency
- **Monitoring and alerting** post-deployment

---

# Popular CI/CD Tools

| Tool | Type | Best For |
|------|------|----------|
| **Jenkins** | On-premise | Self-hosted, complex workflows |
| **GitHub Actions** | Cloud | GitHub repositories |
| **GitLab CI** | Cloud/Self-hosted | GitLab repositories |
| **CircleCI** | Cloud | Quick setup, SaaS |
| **ArgoCD** | Kubernetes | GitOps, Kubernetes deployments |
| **Travis CI** | Cloud | Open source projects |

---

# Basic CI/CD Pipeline Flow

```
Code Commit 
    ↓
Trigger Build
    ↓
Build Application
    ↓
Run Tests (Unit, Integration)
    ↓
Code Quality Check
    ↓
Build Docker Image
    ↓
Push to Registry
    ↓
Deploy to Staging
    ↓
Smoke Tests
    ↓
Deploy to Production
    ↓
Monitor & Alert
```

---

# GitHub Actions Example

### Workflow Structure

```bash
.github/workflows/build.yml
```

### Example Workflow: Build and Deploy

```yaml
name: Build and Deploy

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    # Build
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '16'

    - name: Install dependencies
      run: npm ci

    - name: Build
      run: npm run build

    # Test
    - name: Run tests
      run: npm test

    # Code quality
    - name: Run linter
      run: npm run lint

    # Docker build and push
    - name: Build Docker image
      run: docker build -t myapp:${{ github.sha }} .

    - name: Push to Docker Hub
      if: github.ref == 'refs/heads/main'
      run: |
        echo ${{ secrets.DOCKER_PASSWORD }} | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
        docker push myapp:${{ github.sha }}

    # Deploy
    - name: Deploy to production
      if: github.ref == 'refs/heads/main'
      run: |
        kubectl set image deployment/myapp myapp=myapp:${{ github.sha }}
```

---

# Jenkins Pipeline Example

### Jenkinsfile (Declarative)

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yourrepo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Code Quality') {
            steps {
                sh 'mvn sonar:sonar'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:$BUILD_NUMBER .'
            }
        }

        stage('Push to Registry') {
            when {
                branch 'main'
            }
            steps {
                sh '''
                    echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                    docker push myapp:$BUILD_NUMBER
                '''
            }
        }

        stage('Deploy to Staging') {
            when {
                branch 'main'
            }
            steps {
                sh 'kubectl apply -f k8s/staging.yaml'
            }
        }

        stage('Approval') {
            when {
                branch 'main'
            }
            steps {
                input 'Deploy to production?'
            }
        }

        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                sh 'kubectl apply -f k8s/production.yaml'
            }
        }
    }

    post {
        always {
            junit '**/test-results.xml'
        }
        failure {
            emailext(
                subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build failed. Check console output.",
                to: 'dev-team@example.com'
            )
        }
    }
}
```

---

# GitLab CI Example

### .gitlab-ci.yml

```yaml
stages:
  - build
  - test
  - package
  - deploy

variables:
  DOCKER_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA

build:
  stage: build
  image: node:16
  script:
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 hour

test:
  stage: test
  image: node:16
  script:
    - npm install
    - npm test
  coverage: '/Coverage: \d+\.\d+%/'

lint:
  stage: test
  image: node:16
  script:
    - npm install
    - npm run lint

package:
  stage: package
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $DOCKER_IMAGE .
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $DOCKER_IMAGE
  only:
    - main

deploy_staging:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    - kubectl set image deployment/myapp myapp=$DOCKER_IMAGE -n staging
  environment:
    name: staging
    url: https://staging.example.com
  only:
    - main

deploy_production:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    - kubectl set image deployment/myapp myapp=$DOCKER_IMAGE -n production
  environment:
    name: production
    url: https://example.com
  when: manual
  only:
    - main
```

---

# Common Linux Commands Used in CI/CD

```bash
# Git operations
git clone <repo>
git checkout <branch>
git pull

# Docker operations
docker build -t image:tag .
docker push image:tag
docker run -d image:tag

# Kubernetes operations
kubectl apply -f deployment.yaml
kubectl rollout status deployment/myapp
kubectl logs -f pod-name

# Build tools
mvn clean install          # Maven
gradle build               # Gradle
npm install && npm build   # Node.js

# Testing
npm test                   # Run tests
pytest                     # Python tests
mvn test                   # Maven tests

# Code quality
sonar-scanner              # SonarQube
hadolint Dockerfile        # Lint Docker files
pylint app.py              # Python linting
```

---

# Best Practices

1. **Automate everything** — builds, tests, deployments
2. **Fail fast** — catch errors early in the pipeline
3. **Keep pipelines simple** — complex logic should be in scripts/code
4. **Version your configuration** — track CI/CD changes in git
5. **Use secrets management** — never hardcode credentials
6. **Implement monitoring** — track pipeline success rates
7. **Document failure handling** — rollback procedures
8. **Parallel execution** — run independent jobs concurrently
9. **Cache dependencies** — reduce build time
10. **Notify on failures** — immediate feedback to team

---

# Summary

CI/CD is essential for modern DevOps practices. Choosing the right tool depends on your infrastructure, team size, and existing tech stack. Start simple and gradually add complexity as your needs grow.