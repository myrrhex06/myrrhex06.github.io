---
title: "Backend - Spring boot 이미지 파일 업로드 및 미리보기 기능 구현"
date: 2025-08-15 19:30:00 +0900
categories: [Web, Backend]
tags: [backend, spring, file, upload, multipart]
---

## **이미지 업로드 API**

우선 이미지 업로드는 Content-Type multipart/form-data를 통해 이미지가 전송되기 때문에, 컨트롤러 상에서 `@RequestParam`을 사용하여 `MultipartFile`을 받도록하였다.

`FileController.java`
```java
@RestController
@RequestMapping("/api/v1/files")
@RequiredArgsConstructor
@Tag(name = "Image File API Controller", description = "이미지 파일 관련 API")
public class FileController {

    @PostMapping(consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    @PreAuthorize("hasRole('ROLE_USER')")
    @SecurityRequirement(name = "Jwt Auth")
    public ResponseEntity<ResponseWrapper> upload(
            @Parameter(description = "업로드 파일", required = true)
            @RequestParam(name = "file", required = true) MultipartFile file,
            @AuthenticationPrincipal CustomUserDetails customUserDetails
    ){
        return ResponseWrapperUtils.success("success",  fileService.upload(file, customUserDetails));
    }

    ...
}
```

- `MultipartFile`은 multipart/form-data로 넘어오는 요청에서 파일 데이터를 바인딩해주는 역할
- `@PostMapping`에 `consumes`을 사용하여 요청 Content-Type을 multipart/form-data로 명시

서비스 로직은 아래와 같이 구현하였다.

`FileService.java`
```java
@Service
@RequiredArgsConstructor
@Slf4j
public class FileService {

    private final FileMapper fileMapper;
    private final UserService userService;

    @Value("${file.directory}")
    private String saveDirectory;

    /**
     * 파일 업로드 처리 메서드
     *
     * @param file 업로드 파일
     * @param customUserDetails 인증된 사용자 객체
     * @return FileResponse
     */
    public FileResponse upload(MultipartFile file, CustomUserDetails customUserDetails) {
            User user = userService.findUserByEmail(customUserDetails.getUsername())
                    .orElseThrow(() -> new UsernameNotFoundException("사용자를 찾을 수 없습니다."));

            String saveFileName = createSaveFileName(file);

            Path saveFilePath = getSaveFilePath(saveFileName);

            fileSave(file, saveFilePath);

            File fileEntity = File.builder()
                    .userSeq(user.getUserSeq())
                    .fileName(saveFileName)
                    .imageUrl(saveFilePath.toString())
                    .build();

            insertFileInfo(fileEntity);

            File result = findByFileSeq(fileEntity.getFileSeq())
                    .orElseThrow(() -> new FileUploadFailException("파일 업로드 중 오류가 발생하였습니다."));

            return FileResponse.builder()
                    .fileSeq(result.getFileSeq())
                    .userSeq(result.getUserSeq())
                    .imageUrl(result.getImageUrl())
                    .fileName(result.getFileName())
                    .createdAt(result.getCreatedAt())
                    .modifiedAt(result.getModifiedAt())
                    .build();
    }

    /**
     * 업로드할 파일의 파일명에서 확장자 추출을 위한 . 인덱스 번호 조회 처리 메서드
     *
     * @param originalFilename 업로드할 파일의 파일명
     * @return int
     */
    public int getLastDotIndex(String originalFilename){

        int lastDotIndex = originalFilename.lastIndexOf(".");
        if(lastDotIndex == -1){
            throw new InvalidFileNameException("확장자를 확인해주세요.");
        }

        return lastDotIndex;
    }

    /**
     * 업로드 파일 검증 처리 메서드
     *
     * @param extension 확장자명
     */
    public void validateImageExtension(String extension) {
        String lowerCase = extension.toLowerCase();

        if(!lowerCase.equals("jpg") && !lowerCase.equals("png") && !lowerCase.equals("jpeg") && !lowerCase.equals("gif")){
            throw new InvalidExtensionException("이미지 파일만 업로드 가능합니다.");
        }
    }

    /**
     * 저장될 파일의 파일명 생성
     *
     * @param file 업로드 파일
     * @return String
     */
    public String createSaveFileName(MultipartFile file){
        String uuid = UUID.randomUUID().toString();
        String originalFilename = file.getOriginalFilename();

        int lastDotIndex = getLastDotIndex(originalFilename);

        String extension = originalFilename.substring(lastDotIndex + 1);

        validateImageExtension(extension);

        String fileName = originalFilename.substring(0, lastDotIndex);
        return fileName + "_" + uuid + "." + extension;
    }

    /**
     * 업로드 파일 저장 경로 생성 메서드
     *
     * @param saveFileName 저장될 파일명
     * @return Path
     * @throws IOException IOException
     */
    public Path getSaveFilePath(String saveFileName) {
        try{
            Path directoryPath = Paths.get(saveDirectory);
            if(!Files.exists(directoryPath)) {
                Files.createDirectories(directoryPath);
            }

            Path saveFilePath = directoryPath.resolve(saveFileName).normalize();

            if(!saveFilePath.startsWith(directoryPath)) {
                throw new InvalidDirectoryPathException("저장 경로가 올바르지 않습니다.");
            }

            return saveFilePath;
        } catch (IOException e) {
            throw new FileUploadFailException("디렉토리 생성 중 문제가 발생했습니다.");
        }
    }

    /**
     * 파일 저장 처리 메서드
     *
     * @param file 파일 정보
     */
    public void insertFileInfo(File file){
        fileMapper.save(file);
    }

    /**
     * 파일 저장 처리 메서드
     *
     * @param file 업로드 파일
     * @param saveFilePath 파일 저장 경로
     */
    public void fileSave(MultipartFile file, Path saveFilePath) {
        try{
            Files.copy(file.getInputStream(), saveFilePath);
        } catch (IOException e) {
            throw new FileUploadFailException("파일 업로드에 실패하였습니다.");
        }

    }
}
```

> 파일 다운로드 기능이 있다면 기존 파일명을 DB에 같이 저장하여 다운로드 시에 사용할수도 있겠지만, 업로드한 이미지는 단순히 보여지기만 하는 파일이기 때문에 따로 기존 파일명은 저장하지 않았다.

파일 업로드를 처리하는 `upload` 메서드는 아래와 같이 실행된다.

- 먼저 `CustomUserDetails`를 통해 사용자 정보를 조회
- `createSaveFile` 메서드를 통해 서버에 저장할 파일명을 생성
- `getSaveFilepath` 메서드를 통해 파일을 저장할 경로를 생성
- `fileSave` 메서드를 통해 파일을 서버에 저장
- `insertFileInfo` 메서드를 통해 DB에 파일 정보 저장

## 이미지 미리보기 API

이미지 미리보기 API는 업로드에 비해 비교적 간단하게 구현이 가능하였다.

`FileController.java`
```java
@RestController
@RequestMapping("/api/v1/files")
@RequiredArgsConstructor
@Tag(name = "Image File API Controller", description = "이미지 파일 관련 API")
public class FileController {
  ...

    @GetMapping
    public ResponseEntity<Resource> show(
            @Parameter(description = "파일 기본키", required = true)
            @RequestParam(name = "fileSeq", required = true) Long fileSeq
    ){
        return ResponseEntity.ok()
                .body(fileService.show(fileSeq));
    }
  
  ...
}
```

- DB에 저장된 파일 정보의 기본키값을 파라미터로 받음
- `fileService`에서 반환된 `resource`를 그대로 반환

> `Resource`는 Spring에서 제공하는 인터페이스로 파일이나 URL 등 외부 자원을 추상화한 인터페이스다.
{: .prompt-tip }

서비스 구현 로직은 아래와 같다.

`FileService.java`
```java
@Service
@RequiredArgsConstructor
@Slf4j
public class FileService {
  ...

    /**
     * 파일 미리보기 처리 메서드
     *
     * @param fileSeq 파일 기본키
     * @return Resource
     */
    public Resource show(Long fileSeq) {

        File file = findByFileSeq(fileSeq)
                .orElseThrow(() -> new FileNotFoundException("이미지를 찾을 수 없습니다."));

        try{
            String imageUrl = file.getImageUrl();
            Path path = Paths.get(imageUrl);

            Resource resource = new UrlResource(path.toUri());

            validateResource(resource);

            return resource;

        } catch (MalformedURLException e) {
            throw new FileShowException(e.getMessage());
        }
    }

    /**
     * Resource 검증 처리 메서드
     *
     * @param resource Resource
     */
    public void validateResource(Resource resource) {
        if(!resource.exists() || !resource.isReadable() || !resource.isFile()) {
            throw new InvalidResourceException("잘못된 리소스입니다.");
        }
    }

    ...
}
```

- 파일 정보를 먼저 DB에서 조회
- 저장된 경로 추출 및 `UrlResource`를 생성
- `UrlResource` 객체 검증 후 반환

> `UrlResource`는 `Resource` 인터페이스 구현체 중 하나로, 파일 경로와 같은 url 형태의 리소스를 다룰 수 있게 한다.
{: .prompt-tip }
