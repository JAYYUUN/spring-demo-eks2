# Spring Boot CI Pipeline (GitHub Actions) + Argo CD Deployment

이 리포지토리는 Spring Boot 애플리케이션의 CI(빌드 & 이미지 생성) 까지를  
GitHub Actions로 처리하고,  
배포는 Argo CD가 담당하는 GitOps 구조의 애플리케이션 리포지토리입니다.

> 🔹 GitHub Actions: Build & Image Push (CI)  
> 🔹 Argo CD: Kubernetes 배포 (CD)

---

## Architecture Overview

Developer → GitHub Repository → GitHub Actions (CI) → Amazon ECR  
                                               ↓  
                                            Argo CD → Amazon EKS

---

## Repository Structure

.github/            # GitHub Actions CI workflow  
src/                # Spring Boot source code  
k8s/                # Kubernetes manifests (Argo CD가 참조)  
dockerfile          # Docker image build definition  
build.gradle        # Gradle build config  
settings.gradle  
gradlew / gradlew.bat  
README.md   

---

## CI Scope (GitHub Actions 역할)

### 포함
- 소스 코드 체크아웃
- Gradle 기반 Spring Boot 빌드
- Docker 이미지 빌드
- Amazon ECR로 이미지 Push

### 제외
- kubectl 실행
- Kubernetes 리소스 배포
- EKS 접근

➡️ 배포는 Argo CD에서만 수행됩니다.

---

## Prerequisites

- Amazon ECR Repository
- Amazon EKS Cluster
- Argo CD 설치 완료
- GitHub Actions OIDC IAM Role (ECR Push 권한 포함)

---

## CI Flow

1. main 브랜치에 push
2. GitHub Actions 실행
3. Gradle Build
4. Docker Image Build
5. Amazon ECR Push

---

## Argo CD Deployment

- Argo CD는 k8s/ 디렉터리를 감시
- 변경 사항 감지 시 자동 Sync
- 선언적(GitOps) 방식으로 EKS 배포

---

## Local Development

./gradlew bootRun  
./gradlew clean build  

---

## Docker Local Test

docker build -t demo:local -f dockerfile .  
docker run --rm -p 8080:80 demo:local  

---

## Notes

- CI/CD 역할 분리로 보안 및 운영 안정성 강화
- GitOps 기반 배포 전략
