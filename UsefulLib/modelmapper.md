# ModelMapper
```java
import org.modelmapper.ModelMapper;

@Autowired
ModelMapper modelMapper;

CustomDtoClass customDtoClass = modelMapper.map(sourceDtoObject, CustomDtoClass.class);    
```
