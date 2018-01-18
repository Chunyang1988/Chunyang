---
title: Dagger2简单使用
date: 2017-9-24 12:38:59
tags: [Android]
---


Dagger2是基于JSR—330标准的依赖注入框架，也就是在编译期间自动生成代码，来创建相应的对象。

## Dagger2相应注释讲解
此文讲解案例使用`Retrofit`讲解，讲解前先把常规写法写出来大家根据此对比，来发现Dagger2的便利之处。

```
        //创建OkHttpClient
        OkHttpClient.Builder client = new OkHttpClient.Builder();
        client.connectTimeout(15, TimeUnit.SECONDS);
        client.writeTimeout(30, TimeUnit.SECONDS);
        client.readTimeout(30, TimeUnit.SECONDS);
        
        //添加Interceptor
        HttpLoggingInterceptor logging = new HttpLoggingInterceptor(new HttpLoggingInterceptor.Logger() {
            @Override
            public void log(String message) {
                LogUtil.i(TAG, message);
            }
        });
        logging.setLevel(HttpLoggingInterceptor.Level.BODY);
        client.addInterceptor(logging);
        client.addNetworkInterceptor(new StethoInterceptor());
        client.addNetworkInterceptor(new NetworkInterceptor());
        
        //创建Retrofit，传递OkHttpClient、HostUrl
        Retrofit retrofit = new Retrofit.Builder().client(client.build())
                .addConverterFactory(GsonConverterFactory.create())
                .addCallAdapterFactory(RxJavaCallAdapterFactory.create())
                .baseUrl(HOST_URL).build();
                
        //创建ApiService
        mService = retrofit.create(ApiService.class);
```
大家都知道`Retrofit`中Api的调用都会放在`ApiService`中，那么使用`Dagger2`如何来获取到`ApiService`？如何创建`Retrofit`对象？如何传递`OkHttpClient`对象？大家先带着这样的问题来看此文。

### @Inject
`@Inject`负责标记那些需要被依赖注入自动创建出来，说白一点就是看见`@Inject`就代表这个对象在依赖注入中被引用或者被new出来。

在`ApiModule`中代码如下，先不用看非`@Inject`地方，后续会讲到。

```
@Module
public class ApiModule {
    String host;

    @Inject
    public ApiModule(String host) {
        this.host = host;
    }
    
...

}

```

在`ApiManager`属性中添加`@Inject ApiService`

```
public class ApiManager {

    private static class Holder {
        private static ApiManager IN = new ApiManager();
    }

    public static ApiManager getInstance() {
        return Holder.IN;
    }

    @Inject
    ApiService apiService;

    public ApiManager() {
        DaggerApiComponent.builder().apiModule(new ApiModule("")).build().inject(this);
    }
}
```


### @Module

通过`@Module`注解，Dagger才知道怎么去依赖注入，也就是上文中的`@Inject`的对象在此处查找并构造出来。

```
@Module
public class ApiModule {
    String host;

    @Inject
    public ApiModule(String host) {
        this.host = host;
    }

    @Provides
    @SoApp
    public OkHttpClient okHttpClient() {
          //创建OkHttpClient
        OkHttpClient.Builder client = new OkHttpClient.Builder();
        client.connectTimeout(15, TimeUnit.SECONDS);
        client.writeTimeout(30, TimeUnit.SECONDS);
        client.readTimeout(30, TimeUnit.SECONDS);
        
        //添加Interceptor
        HttpLoggingInterceptor logging = new HttpLoggingInterceptor(new HttpLoggingInterceptor.Logger() {
            @Override
            public void log(String message) {
                LogUtil.i(TAG, message);
            }
        });
        logging.setLevel(HttpLoggingInterceptor.Level.BODY);
        client.addInterceptor(logging);
        client.addNetworkInterceptor(new StethoInterceptor());
        client.addNetworkInterceptor(new NetworkInterceptor());
        return client.build();
    }

    @Provides
    @SoApp
    public ApiService apiService(String host, OkHttpClient client) {
     //创建Retrofit，传递OkHttpClient、HostUrl
        Retrofit retrofit = new Retrofit.Builder().client(client)
                .addConverterFactory(GsonConverterFactory.create())
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
                .baseUrl(host)
                .build();
        return retrofit.create(ApiService.class);
    }


    @Provides
    String providesHost() {
        return host;
    }
}

```

### @Provides

`@Provides`会标记Module中那些返回依赖的方法，通过此注解查找依赖引用，尤其是有些构造器会有很多参数，而这些参数必须在Module中添加`@Provides`的依赖方法，否则无法编译通过。

```
@Module
public class ApiModule {
    String host;

    @Inject
    public ApiModule(String host) {
        this.host = host;
    }

    @Provides
    @SoApp
    public OkHttpClient okHttpClient() {
          //创建OkHttpClient
        OkHttpClient.Builder client = new OkHttpClient.Builder();
        client.connectTimeout(15, TimeUnit.SECONDS);
        client.writeTimeout(30, TimeUnit.SECONDS);
        client.readTimeout(30, TimeUnit.SECONDS);
        
        //添加Interceptor
        HttpLoggingInterceptor logging = new HttpLoggingInterceptor(new HttpLoggingInterceptor.Logger() {
            @Override
            public void log(String message) {
                LogUtil.i(TAG, message);
            }
        });
        logging.setLevel(HttpLoggingInterceptor.Level.BODY);
        client.addInterceptor(logging);
        client.addNetworkInterceptor(new StethoInterceptor());
        client.addNetworkInterceptor(new NetworkInterceptor());
        return client.build();
    }

    @Provides
    @SoApp
    public ApiService apiService(String host, OkHttpClient client) {
     //创建Retrofit，传递OkHttpClient、HostUrl
        Retrofit retrofit = new Retrofit.Builder().client(client)
                .addConverterFactory(GsonConverterFactory.create())
                .addCallAdapterFactory(RxJava2CallAdapterFactory.create())
                .baseUrl(host)
                .build();
        return retrofit.create(ApiService.class);
    }


    @Provides
    String providesHost() {
        return host;
    }
}

```

### @Component

在`@Component`里面定义了在哪些Module中去依赖注入，`@Component`通常是`@Module`和`@Inject`之间的桥梁。`@Component`也可以依赖其它的component，在此处先不讲解，后续讲解。

```
@SoApp
@Component(modules = {ApiModule.class})
public interface ApiComponent {

    void inject(ApiManager manager);

}
```


### 使用

在`ApiManager`属性中需要依赖注入的对象例如添加`@Inject ApiService`，Build Make Module 一下即可编译生成` DaggerApiComponent.builder().apiModule(new ApiModule("")).build().inject(this);`

```
public class ApiManager {

    private static class Holder {
        private static ApiManager IN = new ApiManager();
    }

    public static ApiManager getInstance() {
        return Holder.IN;
    }

    @Inject
    ApiService apiService;

    public ApiManager() {
        DaggerApiComponent.builder().apiModule(new ApiModule("")).build().inject(this);
    }
}
```

到此基本的Dagger也就讲解完成了，文中还有`@SoApp`注解在下一个文章中讲解。



