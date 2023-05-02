## 파일 업로드



### File

```java
package com.cnu.taleteller.backend.domain.tool.domain;

import com.cnu.taleteller.backend.domain.book.domain.Book;
import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.*;

@Entity
@Getter
@Table(name = "files")
@NoArgsConstructor
public class File {

    @Id
    @GeneratedValue(strategy= GenerationType.IDENTITY)
    private Long fileId;

    @Column(length = 100)
    private String fileName;

    @Column(length = 10)
    private String fileSize;

    @Column(length = 100)
    private String fileOriginName;

    @ManyToOne
    @JoinColumn(name = "bookId")
    private Book bookId;

    @Builder
    public File(Long fileId, String fileName, String fileSize, String fileOriginName, Book bookId) {
        this.fileId = fileId;
        this.fileName = fileName;
        this.fileSize = fileSize;
        this.fileOriginName = fileOriginName;
        this.bookId = bookId;
    }
}

```



### FileDto

```java
package com.cnu.taleteller.backend.domain.tool.dto;

import com.cnu.taleteller.backend.domain.book.domain.Book;
import com.cnu.taleteller.backend.domain.tool.domain.File;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;

@Getter
@Setter
@NoArgsConstructor
public class FileDto {

    private Long fileId;

    private String fileName;

    private String fileSize;

    private String fileOriginName;

    private Long bookId;

    public File toEntity(){
        return File.builder()
                .fileId(this.fileId)
                .fileName(this.fileName)
                .fileSize(this.fileSize)
                .fileOriginName(this.fileOriginName)
                .bookId(Book.builder().bookId(this.bookId).build())
                .build();
    }
}

```



### Controller

``` java
package com.cnu.taleteller.backend.domain.tool.controller;

import com.cnu.taleteller.backend.domain.tool.dto.FileDto;
import com.cnu.taleteller.backend.domain.tool.service.FileService;
import com.cnu.taleteller.backend.domain.tool.service.ScenarioService;
import lombok.AllArgsConstructor;
import lombok.NoArgsConstructor;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;

import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.UUID;

@RestController
@RequestMapping("/api/tool")
@AllArgsConstructor
public class ToolController {

    private final ScenarioService scenarioService;

    private final FileService fileService;

    @PostMapping(value = "/image", consumes = {MediaType.MULTIPART_FORM_DATA_VALUE})
    public ResponseEntity<String> saveImage (@RequestParam("image") MultipartFile img,
                                             @RequestParam("menu") String menu,
                                             @RequestParam("bookId") Long bookId) {
        System.out.println(bookId);
        try {
            if (img.isEmpty()) {
                throw new IllegalArgumentException("이미지 파일 오류");
            }
            if (menu == null || menu.isEmpty()) {
                throw new IllegalArgumentException("메뉴 오류");
            }
            if (bookId == null) {
                throw new IllegalArgumentException("book_id 오류");
            }
            String imageFileName = fileService.saveImage(img, menu, bookId);
            if (imageFileName == null) {
                return ResponseEntity.badRequest().body("이미지 파일 업로드 실패");
            }
            return ResponseEntity.ok(imageFileName);
        }
        catch (Exception e) {
            String msg = "사진 업로드 실패: " + e.getMessage();
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(msg);
        }
    }
}
```

###  

application.properties

```properties
spring.servlet.multipart.max-file-size=5MB
file.dir=D:/project/cnu-taleteller/frontend/public/images/
```





### service

```java
package com.cnu.taleteller.backend.domain.tool.service;

import com.cnu.taleteller.backend.domain.book.domain.Book;
import com.cnu.taleteller.backend.domain.book.repository.BookRepository;
import com.cnu.taleteller.backend.domain.tool.domain.File;
import com.cnu.taleteller.backend.domain.tool.dto.FileDto;
import com.cnu.taleteller.backend.domain.tool.repository.FileRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.web.multipart.MultipartFile;

import java.io.IOException;
import java.util.UUID;

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class FileService {

    @Value("${file.dir}")
    private String fileDir;

    private final FileRepository fileRepository;
    private final BookRepository bookRepository;

    public String saveImage(MultipartFile img, String menu, Long bookId) {
        Book book = bookRepository.findById(bookId)
                .orElseThrow(() -> new IllegalArgumentException("존재하지 않는 Book ID입니다."));

        String uuid = UUID.randomUUID().toString();
        String imageFileName = uuid + "_" + img.getOriginalFilename();

        if(menu.equals("background")){
            imageFileName = "B_"+ uuid + "_" + img.getOriginalFilename();
        }
        else if(menu.equals("character")){
            imageFileName = "C_"+ uuid + "_" + img.getOriginalFilename();
        }

        // 파일을 로컬에 저장
        String savedPath = fileDir + imageFileName;
        try {
            img.transferTo(new java.io.File(savedPath));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

        // 이미지 파일을 FileDto 객체로 변환
        FileDto fileDto = new FileDto();
        fileDto.setFileName(imageFileName);
        fileDto.setFileSize(String.valueOf(img.getSize()));
        fileDto.setFileOriginName(img.getOriginalFilename());
        fileDto.setBookId(book.getBookId());

        // FileDto 객체를 이용하여 File 엔티티 객체 생성
        File file = fileDto.toEntity();
        fileRepository.save(file);

        return imageFileName;
    }
}

```





### repository

```java
package com.cnu.taleteller.backend.domain.tool.repository;

import com.cnu.taleteller.backend.domain.tool.domain.File;
import org.springframework.data.jpa.repository.JpaRepository;

public interface FileRepository extends JpaRepository<File, Long> {
}

```

