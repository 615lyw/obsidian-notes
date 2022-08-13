# 链表实现

```java
public interface Context {

}

public interface Handler {
    
    boolean handle(Context context);
}

public interface Pipeline {
    
    void start();

    Context getContext();

    void addFirst(Handler... handlers);

    void addLast(Handler... handlers);
}

public interface PipelineFactory {
    
    Pipeline createPipeline(Context context);
}

public class DefaultLinkedPipelineImpl implements Pipeline {
    private static final Logger LOGGER = LoggerFactory.getLogger(DefaultLinkedPipelineImpl.class);

    private HandlerNode headNode = new HandlerNode();

    /**
     * 尾指针利用对象初始化过程顺序完成赋值
     */
    private HandlerNode tailNode = headNode;

    private Context context;

    public DefaultLinkedPipelineImpl(Context context) {
        if (context == null) {
            throw new IllegalArgumentException("context can't be null");
        }
        this.context = context;
    }

    @Override
    public void start() {
        if (headNode.next == null) {
            throw new IllegalStateException("must add handler before start");
        }
        headNode.next.exec(context);
    }

    @Override
    public Context getContext() {
        return context;
    }

    /**
     * 在节点链表首部依次添加多个 handler
     *
     * @param handlers 处理器
     */
    @Override
    public void addFirst(Handler... handlers) {
        if (handlers == null || handlers.length == 0) {
            throw new IllegalArgumentException("handlers can't be null or empty");
        }
        for (Handler handler : handlers) {
            // 头插法, handler 逆序排列
            headNode.next = new HandlerNode(handler, headNode.next);
        }
        // 更新尾指针
        HandlerNode p = headNode.next;
        while (p.next != null) {
            p = p.next;
        }
        tailNode = p;
    }

    @Override
    public void addLast(Handler... handlers) {
        if (handlers == null || handlers.length == 0) {
            throw new IllegalArgumentException("handlers can't be null or empty");
        }
        for (Handler handler : handlers) {
            tailNode.next = new HandlerNode(handler, null);
            tailNode = tailNode.next;
        }
    }

    private static class HandlerNode {
        Handler handler;
        HandlerNode next;

        HandlerNode() {

        }

        HandlerNode(Handler handler, HandlerNode next) {
            this.handler = handler;
            this.next = next;
        }

        void exec(Context context) {
            boolean success = handler.handle(context);
            if (success) {
                if (next != null) {
                    next.exec(context);
                }
            } else {
                LOGGER.info("流水线中断");
            }
        }
    }
}
```

# 补充

[[Pipeline 模式 VS 责任链模式]]
