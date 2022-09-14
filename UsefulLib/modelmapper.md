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

# diff field names between source and destination
https://www.baeldung.com/java-modelmapper
## Converter diff between source and destination
![image](https://user-images.githubusercontent.com/54012569/190035206-d95834eb-1bc4-4264-a722-973d9a8491bf.png)
![image](https://user-images.githubusercontent.com/54012569/190035469-c8a60261-b6a9-4581-abad-0cd5cbb571cf.png)
![image](https://user-images.githubusercontent.com/54012569/190036193-b8de0d32-b365-4d88-953a-c7f6e619796a.png)

## Method 2: TypeMap‘s addMappings method
![image](https://user-images.githubusercontent.com/54012569/190035768-c11225ae-96d8-4e24-a8b1-3da9d2b20c32.png)
### skip properties
![image](https://user-images.githubusercontent.com/54012569/190035913-81f328dd-0145-4695-ac73-5bc4e7c1ecb2.png)


# BeanConfiguration for ModelMapper
```java
package com.ty.config;

import com.ty.components.utils.json.JsonApi;
import com.ty.components.utils.json.jackson.JacksonImpl;
import org.modelmapper.Conditions;
import org.modelmapper.ModelMapper;
import org.modelmapper.convention.MatchingStrategies;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;

@Configuration
public class BeanConfiguration {
    @Bean
    @Primary
    public ModelMapper modelMapper() {
        ModelMapper modelMapper = new ModelMapper();
        modelMapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STRICT);
        return modelMapper;

    }

    @Bean("ignoreNullModelMapper")
    public ModelMapper ignoreNullModelMapper() {
        ModelMapper modelMapper = new ModelMapper();
        modelMapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STRICT).setPropertyCondition(Conditions.isNotNull());
        return modelMapper;

    }

    @Bean
    public JsonApi JsonApi(){
        return JacksonImpl.getInstance();
    }
}
```
