#Just-in-time Bindings

Injector 通过检查 bindings 定义来创建某个类型的实例对象。定义在Module 中的绑定称为“明确声明绑定（Explicit bindings”。Injector 会首先使用带有 Explicit Bindings 为某个类型创建实例对象。 当但某个类型没有明确定义绑定时，Injector 试图构造“即时绑定 (Just-in-time Bindings),JIT Bindings 也成为隐含绑定 (implicit bindings).

##Eligible Constructor

Injector 通过使用类的 injectable constructor 来创建该类的实例对象。injectable constructor 可以为该类定义的 public 不带参数的构造函数或是带有 @Injector 标记的构造函数。

比如 [Android RoboGuice 使用指南(4):Linked Bindings](http://www.imobilebbs.com/wordpress/archives/2537?p=2503) 中MyRectangle的无参数构造函数：

```
public class MyRectangle extends Rectangle{
 public MyRectangle(){
 super(50,50,100,120);
 }
 ...
}

```

和 [Android RoboGuice 使用指南(6):Instance Bindings](http://www.imobilebbs.com/wordpress/archives/2537?p=2517) 定义的含@Injector 标记的构造函数：

```
public class MySquare extends MyRectangle {
 @Inject public MySquare(@Named("width") int width){
 super(width,width);
 }
}

```

##@ImplementedBy

该标记通知Injector某个类型的缺省实现，其功能和 [Linked Bindings](http://www.imobilebbs.com/wordpress/?p=2503) 类似，例如：

```
@ImplementedBy(PayPalCreditCardProcessor.class)
public interface CreditCardProcessor {
 ChargeResult charge(String amount, CreditCard creditCard)
 throws UnreachableException; }

```

和

```
bind(CreditCardProcessor.class)
 .to(PayPalCreditCardProcessor.class);

```

等效。 如果某个类型同时含有 @ImplementedBy 和 bind 定义，将优先使用 bind 中的定义。

注： @ImplementedBy 定义了从 Interface 到实现的依赖，一般不建议使用。

##@ProvidedBy

@ProvidedBy 通知 Injector 某个类型使用那个缺省 Provider 来创建实例对象，例如：

```
@ProvidedBy(DatabaseTransactionLogProvider.class)
public interface TransactionLog {
 void logConnectException(UnreachableException e);
 void logChargeResult(ChargeResult result);
}

```

和下面 Binding 等效：

```
bind(TransactionLog.class)
 .toProvider(DatabaseTransactionLogProvider.class);

```

和 @ImplementedBy 一样，如果同时定义了 @ProvidedBy 和 bind，模块中定义的 bind 优先