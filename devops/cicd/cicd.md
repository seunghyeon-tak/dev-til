# CI/CD

📅 작성일: 2025-11-02  
📂 분류: devops/cicd  
🔖 태그: #ci #cd #파이프라인

---

## CI/CD란?

- CI/CD는 개발자가 작성한 코드를 자동으로 빌드(Build), 테스트(Test), 배포(Deploy)하는 과정을 말한다.
- 즉, 수동으로 하던 통합과 배포를 자동화 하여 개발 효율성과 안정성을 높이는 DevOps 핵심 개념이라 할 수 있다.

## 필요한 이유?

- 개발을 팀단위로 하게된다면 코드를 합치고 배포를 수동으로 계속하면 이러한 상황들이 발생할 수 있다.
    - dev 서버에 누가 배포했는가?
    - 내 PC 환경에선 안돌아간다?
    - 테스트 안하고 배포했는가?
    - 잘되던 부분에서 배포 후 에러가 뜬다?
- 이와 같은 문제를 수동으로 하나하나 해결할 수 는 없다
- 이를 위해 CI/CD라는 개념이 생겼고 이를 쉽게 해주는 툴이 나오게 되었다.
    - Github Actions
    - Jenkins
    - GitLab CI/CD
    - CircleCI
    - Travis CI

## CI/CD 파이프라인

- 코드구축 부터 시작해서 배포까지 일련의 과정들을 CI/CD 파이프라인이라고 한다.
- 총 3가지 단계로 구성됨
    - continuous integration (지속적 통합) : 코드를 `빌드`하고 `테스트`하고 `합친다.`(merge)
    - continuous delivery (지속적 제공) : 레퍼지토리에 릴리스
    - continuous deployment (지속적 배포) : 운영서버에 배포
- delivery, deployment의 차이
    - delivery : 자동 배포 직전 단계까지 준비
    - deployment : 실제 서비스(운영) 환경까지 자동 배포
- 이러한 파이프 라인이 주는 장점은 코드 배포까지 체계적으로 만드는 점과 테스트가 강제된다는 점이 있다.


## 예시 (Github Actions)

- .github/workflows/ci-cd.yml

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
      - name: Build and Test
        run: ./gradlew build
      - name: Deploy to Server
        run: |
          echo "배포 스크립트 실행 중..."
```