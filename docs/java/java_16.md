# 第 27 章 Apache HttpComponents

## org.apache.commons.lang3

### HTML 标签处理

```

package cn.netkiller.apache.lang;

import org.apache.commons.lang3.StringEscapeUtils;

@SuppressWarnings("deprecation")
public class LangTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String html = "<span>Neo's book</span>";
		String encode = StringEscapeUtils.escapeHtml4(html);
		String decode = StringEscapeUtils.unescapeHtml4(encode);
		System.out.println(encode);
		System.out.println(decode);

	}

}

```

### StringUtils.join 使用特定字符链接字符串

下面例子使用逗号链接字符串

```

org.apache.commons.lang.StringUtils.join(arraylist, ',') 			

```

### RandomStringUtils

```

	String project = RandomStringUtils.randomAlphanumeric(10);
	System.out.print(project);			

```

随机输出 ASCII

```

	System.out.println(RandomStringUtils.randomAscii(10));

```

随机输出数字

```

	System.out.println(RandomStringUtils.randomNumeric(10));		

```

指定字符串随机输出

```

	String project = RandomStringUtils.random(10, "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ").toString();
	System.out.println(project);

```

## commons-text

```

		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-text</artifactId>
			<version>1.6</version>
		</dependency>		

```

### 禁止转译 json

```

package cn.netkiller.test;

import org.apache.commons.text.StringEscapeUtils;

public class TestJson {

	public static void main(String[] args) {
		System.out.println(StringEscapeUtils.escapeJson("{\"name\":\"Neo\"}"));
	}

}

```

输出

```

{\"name\":\"Neo\"}

```

## Apache HttpClient

### Maven

```

<project   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>cn.netkiller</groupId>
	<artifactId>example</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<dependencies>
		<!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient -->
		<dependency>
			<groupId>org.apache.httpcomponents</groupId>
			<artifactId>httpclient</artifactId>
			<version>4.5.6</version>
		</dependency>

	</dependencies>
	<build>
		<sourceDirectory>src</sourceDirectory>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.5.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>			

```

### HTTP POST 操作

#### Post Data

```

package cn.netkiller.httpclient;

import java.io.IOException;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import javax.xml.bind.DatatypeConverter;

import org.apache.http.Consts;
import org.apache.http.HttpEntity;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

public class HttpClientPost {

	public static void main(String[] args) throws ClientProtocolException, IOException, NoSuchAlgorithmException {
		// TODO Auto-generated method stub

		String url = "http://api.netkiller.cn:8080/api/comment/list.jspx";
		String appId = "3175755150424665";
		String appKey = "yEjnjoSEOQpOP49od1IexLkyVB4HTi9c";
		String contentId = "1103";

		DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

		String date = dateFormat.format(new Date());

		String token = DatatypeConverter.printHexBinary(MessageDigest.getInstance("MD5").digest(String.format("%s&%s", appKey, date).getBytes("UTF-8"))).toLowerCase();

		CloseableHttpClient httpclient = HttpClients.createDefault();

		// appId=3175755150424665&contentId=1103&pageSize=100&timeStamp=2017-08-03
		// 10:20:00&token=e1180c0306aff7792c3e25699900dd0d
		List<NameValuePair> params = new ArrayList<NameValuePair>();
		params.add(new BasicNameValuePair("appId", appId));
		params.add(new BasicNameValuePair("contentId", contentId));
		params.add(new BasicNameValuePair("pageSize", "10"));
		params.add(new BasicNameValuePair("timeStamp", date));
		params.add(new BasicNameValuePair("token", token));

		System.out.println(params.toString());
		UrlEncodedFormEntity urlEncodedFormEntity = new UrlEncodedFormEntity(params, Consts.UTF_8);
		System.out.println(urlEncodedFormEntity.toString());

		HttpPost httpPost = new HttpPost(url);
		httpPost.setEntity(urlEncodedFormEntity);

		CloseableHttpResponse response = httpclient.execute(httpPost);

		System.out.println(response.getStatusLine());
		HttpEntity entity = response.getEntity();
		String responseBody = EntityUtils.toString(entity, "UTF-8");
		System.out.println(responseBody.toString());
		response.close();
	}

}

```

#### POST RAW 数据

```

import java.io.IOException;

import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;

@SuppressWarnings("deprecation")
public class HTTPREST {

	public static void main(String[] args) throws ClientProtocolException, IOException {
		HttpClient httpClient = new DefaultHttpClient();

		try {
			HttpPost request = new HttpPost("http://test:123456@api.netkiller.cn/v1/test/create.json");
			StringEntity params = new StringEntity("{\"name\":\"neo\", \"nickname\":\"netkiller\"}", "UTF-8");
			request.addHeader("content-type", "application/json");
			request.addHeader("Accept", "application/json");
			request.setEntity(params);
			HttpResponse response = httpClient.execute(request);
			int statusCode = response.getStatusLine().getStatusCode();
			System.out.println(statusCode);

			if (response != null) {

				String responseBody = EntityUtils.toString(response.getEntity(), "UTF-8");
				System.out.println(responseBody.toString());
			}

		} catch (Exception ex) {
			ex.printStackTrace();
		} finally {
			httpClient.getConnectionManager().shutdown();
		}

	}

}

```

#### POST GBK 编码得数据

有些国内短信运营商发送短信的接口只能接收 GBK 编码，下面就是一个 POST GBK 编码数据的范例。

```

package api.test.sms;

import java.io.IOException;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.Date;
import java.util.List;

import org.apache.http.Consts;
import org.apache.http.HttpEntity;
import org.apache.http.NameValuePair;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.entity.UrlEncodedFormEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.message.BasicNameValuePair;
import org.apache.http.util.EntityUtils;

public class SMS {

	public SMS() {
		// TODO Auto-generated constructor stub
	}

	public void sendSMS(String mobile, String message) throws ClientProtocolException, IOException {

		String url = "http://api.netkiller.cn/sms/v2/send";

		SimpleDateFormat sdf = new SimpleDateFormat("MMddHHmmss");
		String timestamp = sdf.format(Calendar.getInstance().getTime());

		CloseableHttpClient httpclient = HttpClients.createDefault();

		List<NameValuePair> params = new ArrayList<NameValuePair>();
		params.add(new BasicNameValuePair("apikey", "2863300994052170feb"));
		params.add(new BasicNameValuePair("mobile", mobile));
		params.add(new BasicNameValuePair("content", message));

		UrlEncodedFormEntity urlEncodedFormEntity = new UrlEncodedFormEntity(params, "GBK");
		System.out.println(urlEncodedFormEntity.toString());

		HttpPost httpPost = new HttpPost(url);
		httpPost.setEntity(urlEncodedFormEntity);

		CloseableHttpResponse response = httpclient.execute(httpPost);

		HttpEntity entity = response.getEntity();
		String responseBody = EntityUtils.toString(entity, "UTF-8");

		System.out.println(params.toString());
		System.out.println(response.getStatusLine());
		System.out.println(responseBody.toString());
		response.close();

	}

	public static void main(String[] args) throws ClientProtocolException, IOException {
		// TODO Auto-generated method stub
		SMS sms = new SMS();
		String message = String.format("您的验证码是%s，在%s 分钟内输入有效。如非本人操作请忽略此短信。", 8888, 10);
		sms.sendSMS("13113668800", message);
	}

}

```

### HTTPS

#### Get https 接口

环境 Nginx SSL(openssl 自颁发)，nginx 通过 proxy_pass 连接 Tomcat

下面是 nginx 配置

```

server {
	listen 443 ssl spdy;
	server_name api.netkiller.cn;

	ssl_certificate /etc/nginx/ssl/api.netkiller.cn.crt;
	ssl_certificate_key /etc/nginx/ssl/api.netkiller.cn.key;
	ssl_session_cache shared:SSL:20m;
	ssl_session_timeout 60m;
	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

	charset utf-8;
	access_log /var/log/nginx/api.netkiller.cn.access.log;
	error_log /var/log/nginx/api.netkiller.cn.error.log;

	location / {
		proxy_pass http://127.0.0.1:7000;
		proxy_http_version 1.1;
		proxy_set_header X-Forwarded-For
		$proxy_add_x_forwarded_for;
	}

}

```

下面是 Java 程序

```

package cn.netkiller.example;

import java.io.IOException;
import java.security.KeyManagementException;
import java.security.KeyStoreException;
import java.security.NoSuchAlgorithmException;

import org.apache.http.HttpEntity;
import org.apache.http.ParseException;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.conn.ssl.TrustSelfSignedStrategy;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.ssl.SSLContextBuilder;
import org.apache.http.util.EntityUtils;

public class NginxAndOpenSSLAndTomcatAndHttpclient {
	public static void main(String[] args) throws ParseException, IOException, NoSuchAlgorithmException, KeyStoreException, KeyManagementException {
		SSLContextBuilder builder = new SSLContextBuilder();
		builder.loadTrustMaterial(null, new TrustSelfSignedStrategy());
		SSLConnectionSocketFactory sslFactory = new SSLConnectionSocketFactory(builder.build());
		CloseableHttpClient httpclient = HttpClients.custom().setSSLSocketFactory(sslFactory).build();

		HttpGet httpGet = new HttpGet("https://neo:netkiller@api.netkiller.cn/v1/news/today.json");
		CloseableHttpResponse response = httpclient.execute(httpGet);
		try {
			System.out.println(response.getStatusLine());
			HttpEntity entity = response.getEntity();
			String responseBody = EntityUtils.toString(entity, "UTF-8");
			System.out.println(responseBody.toString());
		} finally {
			response.close();
		}
	}
}

```

如果遇到配置问题，可以看一下 《Netkiller Linux Web 手札》

#### POST json 数据

```

package cn.netkiller.example;

import java.io.IOException;
import java.security.KeyManagementException;
import java.security.KeyStoreException;
import java.security.NoSuchAlgorithmException;

import org.apache.http.HttpEntity;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.conn.ssl.SSLConnectionSocketFactory;
import org.apache.http.conn.ssl.TrustSelfSignedStrategy;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.ssl.SSLContextBuilder;
import org.apache.http.util.EntityUtils;

public class HttpClientSSLPost {

	public HttpClientSSLPost() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		SSLContextBuilder builder = new SSLContextBuilder();
		try {
			builder.loadTrustMaterial(null, new TrustSelfSignedStrategy());

			SSLConnectionSocketFactory sslFactory = new SSLConnectionSocketFactory(builder.build());
			CloseableHttpClient httpclient = HttpClients.custom().setSSLSocketFactory(sslFactory).build();

			HttpPost httpPost = new HttpPost("https://neo:YruuUCNXKe@api.netkiller.cn/v1/member/create.json");
			httpPost.addHeader("content-type", "application/json");
			httpPost.addHeader("Accept", "application/json");

			HttpEntity httpEntity = new StringEntity("{\"name\":\"neo\", \"nickname\":\"netkiler\",\"age\":\"18\"}", "UTF-8");

			httpPost.setEntity(httpEntity);

			CloseableHttpResponse response = httpclient.execute(httpPost);

			System.out.println(response.getStatusLine());
			HttpEntity entity = response.getEntity();
			String responseBody = EntityUtils.toString(entity, "UTF-8");
			System.out.println(responseBody.toString());
			response.close();
		} catch (NoSuchAlgorithmException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (KeyStoreException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (KeyManagementException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (ClientProtocolException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

		finally {

		}
	}

}

```

### HTTP/2

HTTP/2 实例

```

    @Test
    public void testHttp2() throws URISyntaxException, IOException, InterruptedException {
        HttpClient.newBuilder()
                .followRedirects(HttpClient.Redirect.SECURE)
                .version(HttpClient.Version.HTTP_2)
                .build()
                .sendAsync(HttpRequest.newBuilder()
                                .uri(new URI("https://http2.akamai.com/demo"))
                                .GET()
                                .build(),
                        HttpResponse.BodyHandler.asString())
                .whenComplete((resp,t) -> {
                    if(t != null){
                        t.printStackTrace();
                    }else{
                        System.out.println(resp.body());
                        System.out.println(resp.statusCode());
                    }
                }).join();
    }

```

### Java11

#### sync get

```

	/**
     * --add-modules jdk.incubator.httpclient
     * @throws IOException
     * @throws InterruptedException
     * @throws URISyntaxException
     */
    @Test
    public void testGet() throws IOException, InterruptedException, URISyntaxException {
        HttpClient httpClient = HttpClient.newHttpClient();
        HttpRequest httpRequest = HttpRequest.newBuilder()
                .uri(new URI("https://www.baidu.com"))
                .header("User-Agent", "jdk 9 http client")
                .GET()
                .build();
        HttpResponse<String> httpResponse = httpClient.send(httpRequest, HttpResponse.BodyHandler.asString());
        System.out.println(httpResponse.statusCode());
        System.out.println(httpResponse.body());
    }

```

#### async get

```

    @Test
    public void testAsyncGet() throws URISyntaxException, InterruptedException, ExecutionException {
        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(new URI("https://www.baidu.com"))
                .GET()
                .build();

        CompletableFuture<HttpResponse<String>> response = client.sendAsync(request, HttpResponse.BodyHandler.asString());

        response.whenComplete((resp,t) -> {
            if(t != null){
                t.printStackTrace();
            }else{
                System.out.println(resp.body());
                System.out.println(resp.statusCode());
            }
        }).join();
    }

```

#### post form

post form

```

	@Test
    public void testPostForm() throws URISyntaxException {
        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(new URI("http://www.w3school.com.cn/demo/demo_form.asp"))
                .header("Content-Type","application/x-www-form-urlencoded")
                .POST(HttpRequest.BodyProcessor.fromString("name1=value1&name2=value2"))
                .build();
        client.sendAsync(request, HttpResponse.BodyHandler.asString())
                .whenComplete((resp,t) -> {
                    if(t != null){
                        t.printStackTrace();
                    }else{
                        System.out.println(resp.body());
                        System.out.println(resp.statusCode());
                    }
        }).join();
    }

```

### Host name 'api.netkiller.cn' does not match the certificate subject provided

这个问题是 CN 不匹配造成的，日志如下

```

Exception in thread "main" javax.net.ssl.SSLPeerUnverifiedException: Host name 'api.netkiller.cn' does not match the certificate subject provided by the peer (EMAILADDRESS=netkiller@msn.com, CN=netkiller.cn, OU=CF, O=CF, L=Shenzhen, ST=Guangdong, C=CN)

```

解决方案一：重新做证书

```

Country Name (2 letter code) [XX]:CN
State or Province Name (full name) []:Guangdong
Locality Name (eg, city) [Default City]:Shenzhen
Organization Name (eg, company) [Default Company Ltd]:CF
Organizational Unit Name (eg, section) []:CF
Common Name (eg, your name or your server's hostname) []:api.netkiller.cn
Email Address []:netkiller@msn.com

```

解决方案二：是用 CN 上的域名链接

### HttpStatus

```

package cn.netkiller.consul.controller;

import org.apache.http.HttpStatus;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TestController {

	public TestController() {
		// TODO Auto-generated constructor stub
	}

	@RequestMapping("/health")
	public int health() {
		return HttpStatus.SC_OK;
	}
}

```