#### 拦截器

拦截器作为OKHttp的核心，贯穿整个网络请求操作的始终。

OKHttp使用List集合保存所有的拦截器，我们通过addInterceptor方式添加进来的拦截器也会被保存到这个集合中，在调用时，我们越晚添加进去的拦截器，会越早被调用。

每一个被调用的拦截器，执行完intercept方法的逻辑之后，都需要调用chain.proceed\(\)将当前执行的request和其他数据传递给下一个拦截器。proceed方法最终也是调用下一个拦截器的intercept方法。

#### addInterceptor和addNetworkInterceptor的区别

从源码上看，两个方法将添加的拦截器放到不同的集合中。

```
  Response getResponseWithInterceptorChain() throws IOException {
    // Build a full stack of interceptors.
    List<Interceptor> interceptors = new ArrayList<>();
    // addInterceptor添加的拦截器
    interceptors.addAll(client.interceptors());
    interceptors.add(retryAndFollowUpInterceptor);
    interceptors.add(new BridgeInterceptor(client.cookieJar()));
    interceptors.add(new CacheInterceptor(client.internalCache()));
    interceptors.add(new ConnectInterceptor(client));
    if (!forWebSocket) {
      // addNetworkInterceptor添加的拦截器
      interceptors.addAll(client.networkInterceptors());
    }
    interceptors.add(new CallServerInterceptor(forWebSocket));

    // 这里指明了从第0个拦截器开始执行
    Interceptor.Chain chain = new RealInterceptorChain(interceptors, null, null, null, 0,
        originalRequest, this, eventListener, client.connectTimeoutMillis(),
        client.readTimeoutMillis(), client.writeTimeoutMillis());

    return chain.proceed(originalRequest);
  }
```

从调用顺序上来看，通过addInterceptor添加到拦截器最先被执行，然后是OkHttp自带拦截器，然后才是addNetworkInterceptor添加的拦截器。

从执行的效果上看，在没有网络的情况下，通过addNetworkInterceptor添加的拦截器不会被执行到。因为在断网的情况下，执行到ConnectInterceptor时已经结束，不会继续往下调用拦截器。

> 利用addInterceptor和addNetworkInterceptor的特点，我们可以进行组合使用，来控制客户端的缓存策略。
>
> 1、在服务器不支持缓存的情况下，我们可以通过addNetworkInterceptor实现自己添加缓存策略。
>
> * 因为addNetworkInterceptor添加到拦截器在response返回时先于CacheInterceptor\(真正执行缓存操作的拦截器\)被执行，我们可以在addNetworkInterceptor添加的拦截器中向Response Header中添加进缓存策略\(Cache-Control: max-age=xxx\)，这样在执行到CacheInterceptor读取到了缓存策略，就会进行本地缓存。
>
> 2、在断网的情况下，强制走缓存。
>
> * 在有网的情况下，我们强制走网络请求数据。同时，将请求的数据缓存一段时间。
> * 在断网时，我们通过addInterceptor添加拦截器，强制请求走缓存\(Cache-Control: only-if-cached\)，这样即使在断网的情况下，我们也能够有数据。
> * 注意必须是使用addInterceptor添加的拦截器，因为在断网情况下是不会走到addNetworkInterceptor添加的拦截器中运行的。



