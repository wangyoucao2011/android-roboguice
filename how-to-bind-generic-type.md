#如何绑定generic类型

如果需要注入某个参数化类型，比如 List<String>:

```
class Example {
 @Inject
 void setList(List<String> list) {
 ...
 }
}

```

可以使用 TypeLiteral 来创建这个绑定。TypeLiteral 为一特殊类型可以用于表示参数化类型。

```
@Override public void configure() {
bind(new TypeLiteral<List<String>>() {})
.toInstance(new ArrayList<String>());   }

```

或者使用 @Provides 方法：

```@Provides List<String> providesListOfString() {
 return new ArrayList<String>();
}

```

到目前为止，基本介绍了 Google Guice 的用法，上面用法也适用于 Java SE，Java EE 平台，更详细的可以参见 [英文文档](http://code.google.com/p/google-guice/wiki/Motivation) ，后面接着介绍和Android 平台相关的 Dependency Injection (Roboguice) 的用法。