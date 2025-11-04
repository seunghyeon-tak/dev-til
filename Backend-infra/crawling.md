# Crawling

📅 작성일: 2025-11-04  
📂 분류: Backend-Infra / crawling  
🔖 태그: #beautifulsoup #requests #selenium #lxml #크롤링

---

> python을 기반으로 정리

## 크롤링이란?

- 웹 크롤링이란 웹 사이트의 HTML 구조를 분석해 데이터를 자동으로 수집하는 과정이다.

---

## 기본 문법 예시

### HTML 요청

```python
import requests

url = "https://example.com"
response = requests.get(url)
print(response.status_code)
print(response.text)
```

### BeautifulSoup을 이용한 파싱

```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(response.text, "html.parser")
title = soup.find("title").text
links = [a["href"] for a in soup.find_all("a", href=True)]
```

### CSS 선택자 사용

```python
soup.select_one("div.article h1").text
soup.select("ul.list > li > a")
```

### XPath 사용(lxml)

```python
from lxml import html

tree = html.fromstring(response.text)
titles = tree.xpath('//div[@class="title"]/text()')
```

### 동적 페이지 처리 (Selenium)

```python
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get("https://example.com")

elements = driver.find_elements(By.CSS_SELECTOR, "div.item > span")
for e in elements:
    print(e.text)

driver.quit()
```

### API 크롤링

```python
import requests

url = "https://api.example.com/items"
response = requests.get(url)
data = response.json()

print(data)
```

---

## 자주 쓰는 문법 요약

|기능 |	BeautifulSoup 문법 |	XPath 문법 |
|----|-------------------|--------------|
|태그 찾기 |	soup.find("div") |	//div |
|클래스 찾기 |	soup.find("div", class_="box") |	//div[@class="box"] |
|아이디 찾기 |	soup.find(id="main") |	//*[@id="main"] |
|텍스트 추출 |	.text.strip() |	/text() |
|속성값 추출 |	tag["href"] |	/@href |
|여러 개 |	soup.find_all("li") |	//li |

---

## 주의할 점

- `robots.txt` 정책을 반드시 확인 (크롤링 허용 여부)
- `User-Agent`를 지정하지 않으면 차단될 수 있음
- 너무 빠른 요청은 IP가 차단될 수 있으므로 `time.sleep()` 으로 조절
- `try-except`로 HTTP 오류 처리

---

## 어떤 크롤링 기법으로 해야하는지 구분 하는 법

> 내가 확인하는 방법을 정리

### 방법 1. view source로 확인

사이트 코드를 보면 body 초반부터 script 태그가 많다 -> 높은 확률로 JS 렌더링 기반

**정적(HTML) 기반**

```
<body>
  <header>...</header>
  <main>
    <h1>상품 목록</h1>
    <div class="product">상품A</div>
    <div class="product">상품B</div>
  </main>
</body>
```

- BeautifulSoup 가능

**동적(js) 기반**

```
<body>
  <script src="/_next/static/..."></script>
  <script src="/_next/static/chunks/..."></script>
  <script id="__NEXT_DATA__" type="application/json">
    {"props":{"pageProps":{"items":[],"category":"..."}}}
  </script>
  <div id="__next"></div>
</body>
```

- JSON API or Selenium 필요

### 방법 2. Network 탭 -> Fetch/XHR 확인

- 정적은 아무것도 안뜨거나 headers에 Content-Type이 text/html이라고 표시됨
- 동적은 /api/, search, list, data, .json 이러한 요청들이 있고 Content-Type에 application/json 이라고 표시됨  

### 방법 3. JS 끄기 테스트

- 브라우저 개발자 도구에서 우측 상단에 톱니바퀴 모양 누르고 Preferences에서 Disable JavaScript (자바스크립트 비활성화) 체크 후 새로고침 해보기 (크롬 기준)
    - 유지되면 HTML 기반
    - 깨지면 JSON 기반