### 介绍
HttpClient是Apache Jakarta Common下的子项目，可以用来提供高效的、最新的、功能丰富的支持HTTP协议的客户端编程工具包，并且它支持HTTP协议最新的版本和建议。可以用在后端向第三方服务器发送请求。
### 使用案例
```java
//通过HttpClient发送get请求
public void testGet() throws ClientProtocolException, IOException {
    //创建httpclient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();
    //创建请求对象
    HttpGet httpGet = new HttpGet("http://127.0.0.1:8080/user/shop/status");
    //发送请求，获取响应结果
    CloseableHttpResponse response = httpClient.execute(httpGet);
    //获取服务器返回的状态码
    int statusCode = response.getStatusLine().getStatusCode();
    //关闭资源
    response.close();
    httpClient.close();
    System.out.println(statusCode);
}

//通过HttpClient发送post请求
public void testPost() throws ClientProtocolException, IOException {
    //创建httpclient对象
    CloseableHttpClient httpClient = HttpClients.createDefault();
    //创建请求对象
    HttpPost httpPost = new HttpPost("http://127.0.0.1:8080/admin/employee/login");
    //提交参数
    JSONObject jsonObject = new JSONObject();
    jsonObject.put("username", "admin");
    jsonObject.put("password", "123456");
    StringEntity entity = new StringEntity(jsonObject.toString());
    //指定请求编码格式
    entity.setContentEncoding("utf-8");
    //指定请求数据格式
    entity.setContentType("application/json");
    httpPost.setEntity(entity);
    //发送请求，获取响应结果
    CloseableHttpResponse response = httpClient.execute(httpPost);
    //获取服务器返回的状态码
    int statusCode = response.getStatusLine().getStatusCode();
    //关闭资源
    response.close();
    httpClient.close();
    System.out.println("post:" + statusCode);
}
```
1. 创建httpclient对象
   