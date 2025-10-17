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

#### Implmentations

```
// OCPP 1.6 Abstract Factory - creates OCPP 1.6 factories
public class OCPP16AbstractFactory implements OCPPAbstractFactory {
    @Override
    public MessageProcessorFactory createProcessorFactory() {
        return new OCPP16ProcessorFactory(); // Returns a factory
    }
    
    @Override
    public MessageValidatorFactory createValidatorFactory() {
        return new OCPP16ValidatorFactory(); // Returns a factory
    }
    
    @Override
    public MessageSerializerFactory createSerializerFactory() {
        return new OCPP16SerializerFactory(); // Returns a factory
    }
}

// OCPP 2.0 Abstract Factory - creates OCPP 2.0 factories
public class OCPP20AbstractFactory implements OCPPAbstractFactory {
    @Override
    public MessageProcessorFactory createProcessorFactory() {
        return new OCPP20ProcessorFactory(); // Returns a factory
    }
    
    @Override
    public MessageValidatorFactory createValidatorFactory() {
        return new OCPP20ValidatorFactory(); // Returns a factory
    }
    
    @Override
    public MessageSerializerFactory createSerializerFactory() {
        return new OCPP20SerializerFactory(); // Returns a factory
    }
}
```
