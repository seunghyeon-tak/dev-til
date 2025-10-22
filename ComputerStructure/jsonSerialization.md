# ⚙️ 컴퓨터는 숫자와 문자를 어떻게 저장하고 처리하는가?

📅 작성일: 2025-10-22  
📂 분류: ComputerStructure  
🔖 태그: #json #직렬화 #역직렬화

---

## JSON이란 무엇인가?

- Javascript 객체 문법을 바탕으로 한 데이터 교환 형식이다.

## JSON의 형태

- key, value 형태로 되어있으며 key값은 중복이 안되고 나중 값으로 덮어씌워진다.

## JSON 역직렬화

- JSON 파일 -> Java 객체 변환
- 바이트나 문자열 형태의 데이터를 객체로 되돌리는 기술

```java
public class JsonSerial {
    public static void main(String[] args) throws Exception {
        // 역직렬화
        Deserialization d = new Deserialization();
        Person p = d.read();
        System.out.println(p.name + " / " + p.age);

    }
}

class Person {
    public String name;
    public int age;
}

class Deserialization {
    Person read() throws Exception {
        String json = new String(Files.readAllBytes(Paths.get("src/main/java/jsonExample/a.json")));

        ObjectMapper mapper = new ObjectMapper();
        return mapper.readValue(json, Person.class);
    }
}
```

## JSON 직렬화

- Java객체 -> JSON 변환

```java
public class JsonSerial {
    public static void main(String[] args) throws Exception {
        // 직렬화
        Serialization s = new Serialization();
        System.out.println(s.write());

    }
}

class Person {
    public String name;
    public int age;
}

class Serialization {
    String write() throws Exception {
        Person p = new Person();
        p.name = "jack";
        p.age = 31;

        ObjectMapper mapper = new ObjectMapper();
        return mapper.writeValueAsString(p);
    }
}
```

## JSON 접근 방법

- 동적 접근 방법
    - `JSONObject`
        - 객체 -> `{"name": "kim", "age": 30}`
        - `{}` 형태의 JSON 데이터를 다룸
    - `JSONArray`
        - 배열 -> `[{"name": "kim", "name": "jack"}]`
        - `[]` 형태의 JSON 데이터를 다룸
    - 위 둘의 사용 목적은 JSON을 직접 탐색하거나 하나씩 꺼낼때 사용됨
    - JSON -> 객체 자동 변환이 안되며 수동처리로 해야함
- POJO 매핑
    - Jackson `ObjectMapper` 사용
    - readValue와 writeValue로 역직렬화/직렬화 가능
    - JSON을 클래스 형태로 자동 변환 가능하고 간결하고 실무에서 가장 많이 사용됨