**Upload file exception:**
    org.springframework.web.multipart.MultipartException: Failed to parse multipart servlet request; nested exception is java.io.IOException: The temporary upload location [/tmp/tomcat.6239989728636105816.19530/work/Tomcat/localhost/ROOT] is not valid


https://spring.hhui.top/spring-blog/2019/02/13/190213-SpringBoot%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0%E5%BC%82%E5%B8%B8%E4%B9%8B%E6%8F%90%E7%A4%BAThe-temporary-upload-location-xxx-is-not-valid/

Solution:
1) reStart service
2) config linux server: do not tomcat folder under /tmp
  ```
  vim /usr/lib/tmpfiles.d/tmp.conf

  # add a line
  x /tmp/tomcat.*
  ```
