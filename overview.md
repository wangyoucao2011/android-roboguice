#概述
在開發應用時一個基本原則是模塊化，並且近最大可能性地降低模塊之間的耦合性。在 Java 平台上 Spring Framework 以及 Net 平台 CAB ,SCSF 和 Prism (WPF,Silverlight) 中都有對 Dependency injection 的支持。

Dependency injection 大大降低了類之間的依賴性，可以通過annotation (Java) 或是 SeviceDepdendcy (.Net) 描述類之間的依賴性，避免了直接調用類似的構造函數或是使用 Factory 來參加所需的類，從而降低類或模塊之間的耦合性，以提高代碼重用並增強代碼的可維護性。

Google Guice 提供了 Java 平台上一個輕量級的 Dependency injection 框架，並可以支持開發 Android 應用。本指南將使用 Android 平台來說明 Google Guice 的用法。

簡單的來說：Guice 降低了 Java 代碼中使用 new 和 Factory 函數的調用。可以把 Guice 的 @Inject 看作 new 的一個替代品。使用 Guice可能還需要寫一些 Factory 方法，但你的代碼不會依賴這些 Factory 方法來創建實例。 使用 Guice 修改代碼，單元測試已經代碼重用變得更容易。

RoboGuice 為 Android 平台上基於 Google Guice 開發的一個庫，可以大大簡化 Android 應用開發的代碼和一些繁瑣重複的代碼。比如代碼中可能需要大量使用 findViewById 在 XML 中查找一個 View，並將其強制轉換到所需類型，onCreate 中可能有大量的類似代碼。 RoboGuice 允許使用 annotation 的方式來描述 id 於 View 之間的關係，其餘的工作由 roboGuice 庫來完成。比如：

```

class AndroidWay extends Activity {
 TextView name;
 ImageView thumbnail;
 LocationManager loc;
 Drawable icon;
 String myName;

 public void onCreate(Bundle savedInstanceState) {
 super.onCreate(savedInstanceState);
 setContentView(R.layout.main);
 name      = (TextView) findViewById(R.id.name);
 thumbnail = (ImageView) findViewById(R.id.thumbnail);
 loc       = (LocationManager) getSystemService(Activity.LOCATION_SERVICE);
 icon      = getResources().getDrawable(R.drawable.icon);
 myName    = getString(R.string.app_name);
 name.setText( "Hello, " + myName );
 }
}

```

如果使用 roboguice 來寫：

```

class RoboWay extends RoboActivity {
 @InjectView(R.id.name)             TextView name;
 @InjectView(R.id.thumbnail)        ImageView thumbnail;
 @InjectResource(R.drawable.icon)   Drawable icon;
 @InjectResource(R.string.app_name) String myName;
 @Inject                            LocationManager loc;

 public void onCreate(Bundle savedInstanceState) {
 super.onCreate(savedInstanceState);
 setContentView(R.layout.main);
 name.setText( "Hello, " + myName );
 }
}

```

只需使用 @InjectView 來描述 view 和 Id 之間的關係，RoboGuice 自動完成餘下的工作，代碼簡潔易讀。在介紹完 Google Guice ，再接著介紹 RoboGuice 在 Android 平台上使用方法。 