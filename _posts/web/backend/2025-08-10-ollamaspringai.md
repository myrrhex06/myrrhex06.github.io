---
title: "Spring AI Ollama Embedding Model 연동에 대해 알아보자."
date: 2025-08-10 11:00:00 +0900
categories: [Web, Backend]
tags: [backend, spring, restful, ollama, embedding, model, ai]
---

최근들어 가장 핫한 주제인 AI에 대해 Spring 진영에서도 관련 프레임워크가 나왔다.

Spring AI라는 것으로 LLM, Embedding Model, Vector DB 등을 연결하기 쉽고 관련 인터페이스들을 다양하게 제공해준다.

## **Ollama를 선택한 이유**
Ollama를 선택한 이유는 아래와 같다.
- 무료 Embedding Model 제공
- Spring AI와 통합하기 쉬움
- 로컬에서 쉽게 구동하여 테스트 가능

## **Ollama 임베딩 모델 환경 구성**
1. Ollama 다운로드 웹 사이트 접속하여 운영체제에 맞게 설치

Ollama 다운로드 웹 페이지: [https://ollama.com/download](https://ollama.com/download)

2. 설치 후 터미널에 아래 커맨드를 입력하여 정상적으로 설치되었는지 확인
```bash
ollama --version
```

3. 사용할 Embedding Model pull

오늘의 목표는 Spring AI와 연동 후 임베딩이 제대로 되는지 테스트해보는 것이 목적이기 때문에 가벼운 nomic-embed-text 모델을 사용

```bash
ollama pull nomic-embed-text
```

> pull이 완료되면 자동으로 localhost에 11434 포트로 구동된다.

4. Embedding Model 테스트

잘 구동되었는 아래 커맨드로 테스트 진행
```bash
curl http://localhost:11434/api/embeddings \
  -d '{
    "model": "nomic-embed-text",
    "prompt": "Hello, world!"
  }'
```

실행 후 임베딩 값이 잘 나오면 성공적으로 구성이 끝난 것이다.

## **Spring AI 연동**
1. Spring 프로젝트의 아래 의존성을 추가해준다.
```groovy
dependencies {
  implementation 'org.springframework.ai:spring-ai-starter-model-ollama'
}
```

2. `application.yml`(또는 properties) 파일에 아래 내용을 추가해준다.
```yaml
spring:
  ai:
    ollama:
      base-url: YOUR_OLLAMA_BASE_URL
      embedding:
        model: YOUR_EMBEDDING_MODEL
```

- `spring.ai.ollama.base-url`: Ollama가 구동되고 있는 baseurl
- `spring.ai.ollama.embedding.model`: 사용할 embedding model

3. Embedding 테스트

`EmbeddingController.java` 예시
```java
@RestController
@RequiredArgsConstructor
public class EmbeddingController {

    private final EmbeddingModel embeddingModel;

    @GetMapping("/embedding")
    public ResponseEntity<Map<String, Object>> embedding(
            @RequestParam(name = "message") String message
    ) {
        float[] embed = embeddingModel.embed(message);

        Map<String, Object> result = new HashMap<>();
        result.put("embeddingValue", embed);

        return ResponseEntity.ok(result);
    }
}
```

- `EmbeddingModel` 인터페이스의 `embed()` 메서드를 사용해서 텍스트 데이터를 Embedding Model로 전달하여 Embedding 처리

실행 결과
![result](/assets/img/embedding_value.png)

위 이미지처럼 Embedding된 데이터는 벡터값으로 반환된다.

> 조금 더 많은 정보를 알고 싶다면 Spring AI 공식 문서를 읽어보는 것도 좋은 방법이다.<br>
[Spring AI 공식 문서](https://docs.spring.io/spring-ai/reference/index.html)

### **참고: EmbeddingModel 인터페이스**
`EmbeddingModel` 인터페이스는 Embedding 처리 기능을 표준화한 인터페이스이다.

여러 Embedding 서비스(ex. OpenAI, HuggingFace, Ollama, 자체 모델 등)를 추상화해서 동일한 방법으로 처리가 가능하다.

`EmbeddingModel` 내부 코드
```java
public interface EmbeddingModel extends Model<EmbeddingRequest, EmbeddingResponse> {
    EmbeddingResponse call(EmbeddingRequest request);

    default float[] embed(String text) {
        Assert.notNull(text, "Text must not be null");
        List<float[]> response = this.embed(List.of(text));
        return (float[])response.iterator().next();
    }

    float[] embed(Document document);

    default List<float[]> embed(List<String> texts) {
        Assert.notNull(texts, "Texts must not be null");
        return this.call(new EmbeddingRequest(texts, EmbeddingOptionsBuilder.builder().build())).getResults().stream().map(Embedding::getOutput).toList();
    }

    default List<float[]> embed(List<Document> documents, EmbeddingOptions options, BatchingStrategy batchingStrategy) {
        Assert.notNull(documents, "Documents must not be null");
        List<float[]> embeddings = new ArrayList(documents.size());
        List<List<Document>> batch = batchingStrategy.batch(documents);
        Iterator var6 = batch.iterator();

        while(var6.hasNext()) {
            List<Document> subBatch = (List)var6.next();
            List<String> texts = subBatch.stream().map(Document::getText).toList();
            EmbeddingRequest request = new EmbeddingRequest(texts, options);
            EmbeddingResponse response = this.call(request);

            for(int i = 0; i < subBatch.size(); ++i) {
                embeddings.add(((Embedding)response.getResults().get(i)).getOutput());
            }
        }

        Assert.isTrue(embeddings.size() == documents.size(), "Embeddings must have the same number as that of the documents");
        return embeddings;
    }

    default EmbeddingResponse embedForResponse(List<String> texts) {
        Assert.notNull(texts, "Texts must not be null");
        return this.call(new EmbeddingRequest(texts, EmbeddingOptionsBuilder.builder().build()));
    }

    default int dimensions() {
        return this.embed("Test String").length;
    }
}
```

| 메서드명                               | 설명                                                                                                            |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `call(EmbeddingRequest request)`       | 임베딩 요청 객체를 받아서 전체 응답을 돌려줌.<br>요청 옵션, 모델명, 텍스트 리스트 등을 담음.                    |
| `embedForResponse(List<String> texts)` | 텍스트 리스트를 받아서 임베딩 결과와 메타정보를 포함한 응답 객체를 편리하게 반환.<br> 내부적으로 `call()` 호출. |
| `embed(List<String> texts)`            | 텍스트 리스트 받아서 임베딩 벡터 리스트만 단순하게 반환(가장 자주 쓰임).                                        |
| `embed(String text)`                   | 단일 텍스트에 대한 임베딩 벡터 반환.                                                                            |
| `dimensions()`                         | 임베딩 벡터의 차원을 반환.<br> 구현체에서 모델에 따라 다름.                                                     |
