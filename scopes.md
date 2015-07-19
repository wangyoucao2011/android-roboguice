#Scopes

缺省情况下，Guice 每次都创建类的一个新的实例对象给需要该类实例的地方。可以使用 Scopes 来修改这个缺省行为，Scope 允许在一定范围内重用类实例。Roboguice 中常用的有两种：

+ @Singleton 整个 Application 生命周期中使用同一实例对象
+ @ContextScoped 同一个Context（如 Activity）中共享某一实例对象。

使用 Scope 的方法为使用相应的标记，如：

```
@Singleton
public class InMemoryTransactionLog implements TransactionLog {
 // everything here should be threadsafe!
 }

```

或者在 Module 中使用 bind 语句：

```
bind(TransactionLog.class)
 .to(InMemoryTransactionLog.class)
 .in(Singleton.class);

```

如果使用 @Provides，可以有：

```
@Provides @Singleton
TransactionLog provideTransactionLog() {
...
}

```

如果某个类型使用某个你不想使用的 Scope 标记，可以将其绑定到 Scopes.NO_SCOPE 取消这个 Scope 定义。