# 🌐 AWS Route 53 개념 정리

📅 작성일: 2025-06-19  
📂 분류: devops/aws  
🔖 태그: #AWS #SSL #TLS #ELB #Nginx #HTTPS

---

## ✅ ELB (Elastic Load Balancer)란?

> 트래픽(부하)을 여러 대상(EC2 등)에 `적절히 분산`시켜주는 AWS 서비스

### 📌 ELB의 주요 유형

- ALB (Application Load Balancer)
    - **HTTP/HTTPS 기반**
    - URL path, host 기반 라우팅 지원
    - WebSocket, HTTP/2 지원
- NLB (Network Load Balancer)
    - **TCP/UDP, TLS 기반**
    - 초고속 처리 (수백만 동시 연결에 최적화)
    - 고정 IP 제공
- CLB (Classic Load Balancer)
    - HTTP/HTTPS/TCP
    - 현재는 레거시, 가급적 ALB/NLB 사용 권장

### 📌 ELB의 역할

- 고가용성 : 다수의 AZ에 걸쳐 트래픽 분산
- 무중단 배포 : EC2 교체/업데이트 시 트래픽 자연 전환 가능

---

## ✅ SSL/TLS란?

HTTPS의 기반이 되는 **암호화 및 인증 기술**
HTTPS는 단순히 `인증서`만으로 구성되는것이 아니라, **암호화 통신(SSL/TLS 프로토콜)**을 의미 한다.

### 📌 SSL vs TLS

- SSL은 초기 버전 (현재는 보안 취약점으로 인해 더 이상 사용하지 않음)
- TLS 1.2, 1.3이 현재 널리 사용되고 있음

### 📌 SSL/TLS 주요 기능

- 암호화 : 데이터 도청 방지
- 무결성 : 데이터 위/변조 방지
- 인증 : 서버의 신원 확인

---

## ✅ HTTPS와 보안

### 📌 HTTPS 필요성 요약

- 민감한 정보 (비밀번호, 카드번호 등) 보호
- SEO 점수 향상 (google은 HTTPS 우선순위)
- 브라우저 경고 회피 (이 사이트는 안전하지 않음)
- CSRF, MITM 공격 대응 가능

---

## ✅ HTTPS 연결 시: ELB vs Nginx

| 항목     | ELB 사용                         | Nginx + Certbot 사용     |
| ------ | ------------------------------ | ---------------------- |
| 비용     | **추가 비용 발생** (트래픽 + 인증서 관리 포함) | 무료 (Let's Encrypt 인증서) |
| 설정 난이도 | 매우 쉬움 (콘솔 클릭 몇 번)              | 리눅스 서버 지식 필요           |
| 인증서 갱신 | **자동** 갱신                      | 수동 또는 스크립트 자동화 필요      |
| 확장성    | **Auto Scaling에 적합**           | 수동 설정 필요               |
| 적용 위치  | 로드밸런서 앞단                       | EC2 내부 (웹서버 레벨)        |


---

## ✅ AWS Certificate Manager (ACM)

- AWS에서 제공하는 무료 SSL/TLS 인증 서비스
- ELB, CloudFront, API Gateway등과 쉽게 연동 가능
- 유효기간 13개월, 자동 갱신 지원
- 단점 : **EC2 내 Nginx에서는 직접 사용 불가**