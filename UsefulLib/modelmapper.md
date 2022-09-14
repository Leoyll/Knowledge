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
