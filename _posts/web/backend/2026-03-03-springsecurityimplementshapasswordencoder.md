---
title: "Backend - Spring Security SHA-256 Password Encoder 구현하기"
date: 2026-03-03 20:23:00 +0900
categories: [Web, Backend]
tags: [backend, springboot, springsecurity, sha256, passwordencoder, hash]
---

기존에는 Spring Security에서 제공하는 `BCryptPasswordEncoder`를 사용하였으나 SHA-256 알고리즘을 통해 비밀번호를 암호화하도록 `PasswordEncoder`를 변경해야할 일이 생김.

구글링을 해보니 원래는 `MessageDigestPasswordEncoder`나 `StandardPasswordEncoder`를 통해 SHA-256 암호화를 처리할 수 있다고 했으나....확인해보니 현재 내가 사용하고는 Spring Security 버전(6.4.7)에서는 모두 `deprecated`되어 있다...

이것저것 클래스를 뒤져본 결과 `AbstractPasswordEncoder`라는 추상 클래스를 통해 쉽게 `PasswordEncoder`를 구현할 수 있다는 것을 알아내었고 바로 구현해보았다.

해당 추상 클래스는 아래와 같이 구성되어 있었다.

`AbstractPasswordEncoder.java`
```java
public abstract class AbstractPasswordEncoder implements PasswordEncoder {
    private final BytesKeyGenerator saltGenerator = KeyGenerators.secureRandom();

    protected AbstractPasswordEncoder() {
    }

    public String encode(CharSequence rawPassword) {
	      // Salt 값 랜덤 생성
        byte[] salt = this.saltGenerator.generateKey();
        // 평문 암호와 Salt 값을 통해 인코딩 및 병합 처리
        byte[] encoded = this.encodeAndConcatenate(rawPassword, salt);
        // 인코딩된 byte 배열을 16 진수로 인코딩한 후 문자열로 변환하여 반환
        return String.valueOf(Hex.encode(encoded));
    }

    public boolean matches(CharSequence rawPassword, String encodedPassword) {
        // 16 진수로 인코딩되어 있는 암호화된 문자 byte 배열로 디코딩
        byte[] digested = Hex.decode(encodedPassword);
        // 맨 앞 글자(0번 인덱스)부터 Salt의 길이만큼 digested 값에서 짤라냄
        byte[] salt = EncodingUtils.subArray(digested, 0, this.saltGenerator.getKeyLength());
        // 평문 비밀번호와 Salt 값으로 다시 인코딩한 결과를 digested 값이랑 비교
        return matches(digested, this.encodeAndConcatenate(rawPassword, salt));
    }

    protected abstract byte[] encode(CharSequence rawPassword, byte[] salt);

    protected byte[] encodeAndConcatenate(CharSequence rawPassword, byte[] salt) {
        // Salt 값과 구현한 추상 메서드를 통해 byte 배열을 얻은 후 concatenate 메서드로 두 byte 배열을 병함
        return EncodingUtils.concatenate(new byte[][]{salt, this.encode(rawPassword, salt)});
    }

    protected static boolean matches(byte[] expected, byte[] actual) {
        return MessageDigest.isEqual(expected, actual);
    }
}
```

추상 메서드로 선언된 `encode` 메서드를 아래와 같이 구현하여 SHA-256 알고리즘을 처리하였다.

`Sha256PasswordEncoder.java`
```java
@Slf4j
public class Sha256PasswordEncoder extends AbstractPasswordEncoder {

    private static final String ALGORITHM = "SHA-256";

    @Override
    protected byte[] encode(CharSequence rawPassword, byte[] salt) {
        try{
		        // SHA-256 계산을 위해 MessageDigest 인스턴스를 가져옴
            MessageDigest digest = MessageDigest.getInstance(ALGORITHM);
            
            // 평문과 Salt 값을 합침
            digest.update(Utf8.encode(rawPassword));
            digest.update(salt);

						// 해시 계산 후 결과 반환
            return digest.digest();

        } catch (NoSuchAlgorithmException e) {
            log.error(ALGORITHM + " algorithm not found.");
            throw new RuntimeException(ALGORITHM + " algorithm not available.", e);
        }
    }
}
```