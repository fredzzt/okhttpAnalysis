OkHttp
======

An HTTP & HTTP/2 client for Android and Java applications. For more information see [the website][1] and [the wiki][2].

Download
--------

Download [the latest JAR][3] or configure this dependency:

```kotlin
implementation("com.squareup.okhttp3:okhttp:3.12.0")
```

Snapshots of the development version are available in [Sonatype's `snapshots` repository][snap].


MockWebServer
-------------

A library for testing HTTP, HTTPS, and HTTP/2 clients.

MockWebServer coupling with OkHttp is essential for proper testing of HTTP/2 so that code can be shared.

### Download

Download [the latest JAR][4] or configure this dependency:
```xml
testImplementation("com.squareup.okhttp3:mockwebserver:3.12.0")
```

R8 / ProGuard
-------------

If you are using R8 or ProGuard add the options from
[this file](https://github.com/square/okhttp/blob/master/okhttp/src/main/resources/META-INF/proguard/okhttp3.pro).

You might also need rules for Okio which is a dependency of this library.

### Usage
-------
```java
private final OkHttpClient client = new OkHttpClient();

  public void run() throws Exception {
    Request request = new Request.Builder()
        .url("https://publicobject.com/helloworld.txt")
        .build();

    try (Response response = client.newCall(request).execute()) {
      if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);

      Headers responseHeaders = response.headers();
      for (int i = 0; i < responseHeaders.size(); i++) {
        System.out.println(responseHeaders.name(i) + ": " + responseHeaders.value(i));
      }

      System.out.println(response.body().string());
    }
  }
```
```java
private final OkHttpClient client = new OkHttpClient();

  public void run() throws Exception {
    Request request = new Request.Builder()
        .url("http://publicobject.com/helloworld.txt")
        .build();

    client.newCall(request).enqueue(new Callback() {
      @Override public void onFailure(Call call, IOException e) {
        e.printStackTrace();
      }

      @Override public void onResponse(Call call, Response response) throws IOException {
        try (ResponseBody responseBody = response.body()) {
          if (!response.isSuccessful()) throw new IOException("Unexpected code " + response);

          Headers responseHeaders = response.headers();
          for (int i = 0, size = responseHeaders.size(); i < size; i++) {
            System.out.println(responseHeaders.name(i) + ": " + responseHeaders.value(i));
          }

          System.out.println(responseBody.string());
        }
      }
    });
  }
```

```java
public static final MediaType JSON
    = MediaType.parse("application/json; charset=utf-8");

OkHttpClient client = new OkHttpClient();

String post(String url, String json) throws IOException {
  RequestBody body = RequestBody.create(JSON, json);
  Request request = new Request.Builder()
      .url(url)
      .post(body)
      .build();
  Response response = client.newCall(request).execute();
  return response.body().string();
}
```
samples:https://github.com/square/okhttp/wiki/Recipes

OkHttp is an HTTP client that’s efficient by default:

* HTTP/2 support allows all requests to the same host to share a socket.
* Connection pooling reduces request latency (if HTTP/2 isn’t available).
* Transparent GZIP shrinks download sizes.
* Response caching avoids the network completely for repeat requests.


License
-------

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.


 [1]: https://square.github.io/okhttp
 [2]: https://github.com/square/okhttp/wiki
 [3]: https://search.maven.org/remote_content?g=com.squareup.okhttp3&a=okhttp&v=LATEST
 [4]: https://search.maven.org/remote_content?g=com.squareup.okhttp3&a=mockwebserver&v=LATEST
 [snap]: https://oss.sonatype.org/content/repositories/snapshots/
