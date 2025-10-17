public interface MessageProcessorFactory {
    MessageProcessor getProcessor(MessageType messageType);
}

@RequiredArgsConstructor(onConstructor = @__(@Inject))
public class IncomingMessageProcessorFactory implements MessageProcessorFactory {

    @NonNull
    private final IncomingCallMessageProcessor callMessageProcessor;

    @NonNull
    private final IncomingCallResultMessageProcessor callResultMessageProcessor;

    @NonNull
    private final IncomingCallErrorMessageProcessor callErrorMessageProcessor;
    
    @Override
    public MessageProcessor getProcessor(@NonNull final MessageType messageType) {
        if (MessageType.CALL.equals(messageType)) {
            return callMessageProcessor;
        } else if (MessageType.CALL_RESULT.equals(messageType)) {
            return callResultMessageProcessor;
        } else if (MessageType.CALL_ERROR.equals(messageType)) {
            return callErrorMessageProcessor;
        }
        throw new IllegalArgumentException("Received invalid messageType: " + messageType);
    }
}

@RequiredArgsConstructor(onConstructor = @__(@Inject))
public class OutgoingMessageProcessorFactory implements MessageProcessorFactory {

    @NonNull
    private final OutgoingCallMessageProcessor callMessageProcessor;
    
    @NonNull
    private final OutgoingCallResultMessageProcessor callResultMessageProcessor;

    @NonNull
    private final OutgoingCallErrorMessageProcessor callErrorMessageProcessor;

    @Override
    public MessageProcessor getProcessor(@NonNull final MessageType messageType) {
        if (MessageType.CALL.equals(messageType)) {
            return callMessageProcessor;
        } else if (MessageType.CALL_RESULT.equals(messageType)) {
            return callResultMessageProcessor;
        } else if (MessageType.CALL_ERROR.equals(messageType)) {
            return callErrorMessageProcessor;
        }
        throw new IllegalArgumentException("Received invalid messageType: " + messageType);
    }
}



#### Generic Factory
public interface ConnectionFactory<T> {

    /**
     * Creates a new connection instance.
     *
     * @return A newly created connection instance.
     * @throws UnsupportedEncodingException or MqttException if there is an error during connection creation.
     */
    T create() throws UnsupportedEncodingException, MqttException;

    /**
     * Destroys the given connection, releasing any resources associated with it.
     *
     * @param connection The connection instance to be destroyed.
     * @throws Exception if there is an error while destroying the connection.
     */
    void destroy(T connection) throws Exception;
}
