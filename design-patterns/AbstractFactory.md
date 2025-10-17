#### Factories
```
// Factory for Message Processors
public interface MessageProcessorFactory {
    MessageProcessor createProcessor(MessageType type);
}

// Factory for Message Validators  
public interface MessageValidatorFactory {
    MessageValidator createValidator(MessageType type);
}

// Factory for Message Serializers
public interface MessageSerializerFactory {
    MessageSerializer createSerializer(MessageType type);
}
```


#### Abstract Factory
```
// This is the "factory of factories"
public interface OCPPAbstractFactory {
    MessageProcessorFactory createProcessorFactory();
    MessageValidatorFactory createValidatorFactory(); 
    MessageSerializerFactory createSerializerFactory();
}
```
