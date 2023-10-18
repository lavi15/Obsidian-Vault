
naver.properties
```properties
ncp.accessKey=
ncp.secretKey=
ncp.regionName=kr-standard  
ncp.endPoint=https://kr.object.ncloudstorage.com
```

NaverConfiguration.java
```java
package com.spring.conf;  
  
import lombok.Getter;  
import lombok.Setter;  
import org.apache.commons.dbcp2.BasicDataSource;  
import org.apache.ibatis.session.SqlSession;  
import org.apache.ibatis.session.SqlSessionFactory;  
import org.mybatis.spring.SqlSessionFactoryBean;  
import org.mybatis.spring.SqlSessionTemplate;  
import org.mybatis.spring.annotation.MapperScan;  
import org.springframework.beans.factory.annotation.Value;  
import org.springframework.context.ApplicationContext;  
import org.springframework.context.annotation.Bean;  
import org.springframework.context.annotation.Configuration;  
import org.springframework.context.annotation.PropertySource;  
import org.springframework.jdbc.datasource.DataSourceTransactionManager;  
import org.springframework.transaction.annotation.EnableTransactionManagement;  
  
import javax.sql.DataSource;  
  
@Configuration  
@PropertySource("classpath:naver.properties")  
@Getter  
@Setter  
public class NaverConfiguration {  
    @Value("${ncp.accessKey}")  
    private String accessKey;  
    @Value("${ncp.secretKey}")  
    private String secretKey;  
    @Value("${ncp.regionName}")  
    private String regionName;  
    @Value("${ncp.endPoint}")  
    private String endPoint;  
  
  
}
```

UserController
```java
package user.controller;  
  
import org.springframework.beans.factory.annotation.Autowired;  
import org.springframework.stereotype.Controller;  
import org.springframework.web.bind.annotation.*;  
import org.springframework.web.multipart.MultipartFile;  
import s3.service.NcpObjectStorageService;  
import s3.service.S3Service;  
import user.bean.UserImgDTO;  
import user.service.UserService;  
  
import javax.servlet.http.HttpSession;  
import java.io.File;  
import java.io.IOException;  
import java.util.ArrayList;  
import java.util.List;  
  
@Controller  
@RequestMapping(value = "user")  
public class UserController {  
    @Autowired  
    private UserService userService;  
    @Autowired  
    private S3Service s3Service;  
  
    private String bucketName = "bitcamp-test-01";  

    @PostMapping(value = "upload", produces = "application/json;charset=UTF-8")  
    @ResponseBody  
    public String upload(@ModelAttribute UserImgDTO userImgDTO,  
                         @RequestParam("img[]") List<MultipartFile> list,  
                         HttpSession session) {  
        String filePath = session.getServletContext().getRealPath("/WEB-INF/storage");  
  
        File file;  
        String originlFailName;  
        String fileName;  
  
        List<UserImgDTO> userImgList = new ArrayList<>();  
  
        for(MultipartFile img : list) {  
            originlFailName = img.getOriginalFilename();  
            System.out.println(originlFailName);  
  
            fileName = s3Service.uploadFile(bucketName, "storage/", img);  
  
            file = new File(filePath, originlFailName);  
  
            try{  
                img.transferTo(file);  
            } catch (IOException e) {  
                e.printStackTrace();  
            }  
            UserImgDTO dto = new UserImgDTO();  
            dto.setImageName(userImgDTO.getImageName());  
            dto.setImageContent(userImgDTO.getImageContent());  
            dto.setImageFileName(fileName);  
            dto.setImageOriginalName(originlFailName);  
  
            userImgList.add(dto);  
        }  
        userService.upload(userImgList);  
  
  
        return "이미지 등록 완료";  
    }  

```

S3Service.java
```java
package s3.service;  
  
import org.springframework.web.multipart.MultipartFile;  
  
public interface S3Service {  
    public String uploadFile(String bucketName, String directoryPath, MultipartFile img);  
}
```

NcpObjectStorageService.java
```java
package s3.service;  
  
import com.amazonaws.auth.AWSStaticCredentialsProvider;  
import com.amazonaws.auth.BasicAWSCredentials;  
import com.amazonaws.client.builder.AwsClientBuilder;  
import com.amazonaws.services.s3.AmazonS3;  
import com.amazonaws.services.s3.AmazonS3ClientBuilder;  
import com.amazonaws.services.s3.model.CannedAccessControlList;  
import com.amazonaws.services.s3.model.ObjectMetadata;  
import com.amazonaws.services.s3.model.PutObjectRequest;  
import com.spring.conf.NaverConfiguration;  
import org.springframework.stereotype.Service;  
import org.springframework.web.multipart.MultipartFile;  
  
import java.io.InputStream;  
import java.util.UUID;  
  
@Service  
public class NcpObjectStorageService implements S3Service{  
    final AmazonS3 s3;  
  
    NcpObjectStorageService(NaverConfiguration naverConfiguration) {  
        s3 = AmazonS3ClientBuilder.standard().withEndpointConfiguration(  
                new AwsClientBuilder.EndpointConfiguration(naverConfiguration.getEndPoint(), naverConfiguration.getRegionName())  
        ).withCredentials(  
                new AWSStaticCredentialsProvider(  
                        new BasicAWSCredentials(naverConfiguration.getAccessKey(), naverConfiguration.getSecretKey())  
                )        ).build();  
    }  
    @Override  
    public String uploadFile(String bucketName, String directoryPath, MultipartFile img) {  
        if(img.isEmpty()) return null;  
  
        try(InputStream fileIn = img.getInputStream()) {  
  
            String fileName = UUID.randomUUID().toString();  
  
            ObjectMetadata objectMetadata = new ObjectMetadata();  
            objectMetadata.setContentType(img.getContentType());  
  
            PutObjectRequest objectRequest = new PutObjectRequest(  
                    bucketName, directoryPath + fileName, fileIn, objectMetadata  
            ).withCannedAcl(CannedAccessControlList.PublicRead);  
  
            s3.putObject(objectRequest);  
  
            return fileName;  
        }catch (Exception e) {  
            throw new RuntimeException("파일 업로드 오류", e);  
        }    
	}
}
```