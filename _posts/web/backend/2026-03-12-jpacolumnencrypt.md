---
title: "Backend - Spring JPA 데이터 암호화 기능 구현하기"
date: 2026-03-12 20:23:00 +0900
categories: [Web, Backend]
tags: [backend, springboot, springjpa, aec, encrypt, decrypt]
---

Spring Data JPA에서 제공하는 `AttributeConverter` 인터페이스를 구현해서 처리함. 

`AttributeConverter.java` 

```java
public interface AttributeConverter<X, Y> {
    Y convertToDatabaseColumn(X var1);

    X convertToEntityAttribute(Y var1);
}
```

- `convertToDatabaseColumn`: `Entity`에 선언된 프로퍼티 타입을 DB Column에 저장되어 있는 프로퍼티 타입으로 변환
- `convertToEntityAttribute`: DB Column에 저장되어 있는 프로퍼티 타입을 `Entity`에 선언된 프로퍼티 타입으로 변환

`AttributeConverter`의 메서드는 저장 또는 조회할 때마다 실행됨.

AES 암호화 처리 `Converter` 구현 예시

```java
@Converter
public class AESConverter implements AttributeConverter<String, String> {

    // 비밀키
    @Value("${aes.secret.key:null}")
    private String key;

    @Override
    public String convertToDatabaseColumn(String s) {
        if(key.equals("null")){
            throw new RuntimeException("Required AES Secret Key");
        }

        // 랜덤 Salt 값 생성
        String salt = KeyGenerators.string().generateKey();

        BytesEncryptor encryptor = Encryptors.stronger(key, salt);
        byte[] encrypt = encryptor.encrypt(s.getBytes());

        // 복호화 시점에는 Salt 값을 알 수 없기 때문에 암호화된 데이터를 저장하기 전에 Salt 값을 앞에 붙여줌
        return salt + ":" + Base64.getEncoder().encodeToString(encrypt);
    }

    @Override
    public String convertToEntityAttribute(String s) {
        if(key.equals("null")){
            throw new RuntimeException("Required AES Secret Key");
        }

        // 복호화에 필요한 Salt 값 추출
        int index = s.indexOf(":");
        String salt = s.substring(0, index);

        // 암호화 데이터 추출
        String encryptedBase64Data = s.substring(index + 1);
        byte[] encryptedData = Base64.getDecoder().decode(encryptedBase64Data);

        BytesEncryptor encryptor = Encryptors.stronger(key, salt);

        // 복호화 처리
        byte[] decryptedData = encryptor.decrypt(encryptedData);

        return new String(decryptedData, StandardCharsets.UTF_8);
    }
}

```

`Entity`에는 아래와 같이 `@Convert` 어노테이션을 통해 사용할 `Converter`를 명시해주면 됨.

```java
@Entity
@Getter
@Setter
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class UserTwoFactorInfo extends BaseEntity{
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "phone_number", nullable = false)
    @Convert(converter = AESConverter.class)
    private String phoneNumber;
}
```

## 유의 사항

- `findById` 처럼 `WHERE` 절에 들어가는 값은 암호화가 되지 않기 때문에 `WHERE` 절을 통한 조회를 할 수 없음.
- `Entity`를 조회할 때 암호화된 데이터는 `Converter`에 의해 복호화된 상태로 넘어옴. → 특정 컬럼을 수정한 뒤 `repository.save`를 호출하게 되면 JPA가 복호화된 데이터도 변경 사항으로 감지하게 되서 다시 암호화한 후 `UPDATE` 처리하게 됨.
    - `Entity` 클래스에 `@DynamicUpdate` 어노테이션을 달아서 `UPDATE` 시점에 `Entity` 전체가 넘어가는 것이 아니라 변경된 데이터만 넘어가도록 처리 가능함.

## **Reference**

- [https://effortguy.tistory.com/483](https://effortguy.tistory.com/483)
- [https://multifrontgarden.tistory.com/299](https://multifrontgarden.tistory.com/299)