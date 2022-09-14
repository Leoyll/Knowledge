# ModelMapper
```java
import org.modelmapper.ModelMapper;

@Autowired
ModelMapper modelMapper;

CustomDtoClass customDtoClass = modelMapper.map(sourceDtoObject, CustomDtoClass.class);    
```
# Pair
```java
import org.modelmapper.internal.Pair;
Pair<L, R> pair = Pair.of(lTypeObject, rTypeObject);
Pair<Long, List<XxxDTO>> pair = Pair.of(page.getTotalElements(), xxxDTOList);
```
