#Untargetted Bindings

在创建 Bindings 时，也可以不给出绑定的目标，通常用于含有 @ImplementedBy 和 @ProvidedBy （后面介绍）的实类 (Concrete classes 或 type)。 Untargeted bindings 目的是通知 Injector 某个类类型，从而 Injector 可以预先准备某个依赖。Untargetted Bindings 不含 to 语句。

例如：

```
bind(MyConcreteClass.class);
bind(AnotherConcreteClass.class).in(Singleton.class);

```

但如果此时需要同时使用 [binding annotations](http://www.imobilebbs.com/wordpress/?p=2510) 时，需要为绑定添加目标，即使是绑定到同一个实类，如：

```
bind(MyConcreteClass.class)
 .annotatedWith(Names.named("foo"))
 .to(MyConcreteClass.class);
bind(AnotherConcreteClass.class)
 .annotatedWith(Names.named("foo"))
 .to(AnotherConcreteClass.class)
 .in(Singleton.class);

```
