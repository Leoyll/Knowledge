# ModelMapper
```java
import org.modelmapper.ModelMapper;

@Autowired
ModelMapper modelMapper;

CustomDtoClass customDtoClass = modelMapper.map(paymentWithdrawEntity, CustomDtoClass.class);    
```
