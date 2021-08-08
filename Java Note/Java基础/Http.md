# Http

## Http Server服务器端

* 处理HTTP请求，发送Http响应
* JavaEE Servlet API

## Http Client客户端

* 发送Http请求，接收Http响应
* java.net.HttpURLConnection

## 发送get请求

```java
// 得到url
URL url = new URL("http://www.example.com/");
// 获得连接
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
// 获得响应码
int code = conn.getResponseCode();
// 获得响应数据
try {
    InputStream input = conn.getInputStream();
    // 读取响应数据
} catch (Exception e) {
    e.printStackTrace();
}
// 断开连接
conn.disconnect();
```

## 发送post请求

```java
URL url = new URL("http://www.example.com/login");
HttpURLConnection conn = (HttpURLConnection) url.openConnection();
// 设置请求方式
conn.setRequestMethod("POST");
// 设置允许发送请求数据
conn.setDoOutput(true);
byte[] postData = "loginName=test&password=123456".getBytes("UTF-8");
// 设置content-type
conn.setRequestProperty("Content-Type", "application/x-www-form-urlencoded");
// 设置content-length
conn.setRequestProperty("Content-Length", String.valueOf(postData.length));
try {
    OutputStream output = conn.getOutputStream();
    output.write(postData);
} catch (Exception e) {
    throw e;
}
int code = conn.getRequestCode();
try {
    InputStream input = conn.getInputStream();
    // 读取响应数据
}
conn.disconnect();
```

## 总结

Http协议是一个基于TCP的请求/响应协议

广泛用于浏览器、手机App与服务器的数据交互

Java提供了HttpURLConnection实现了Http客户端