# 第 28 章 Cache

## java memcached client

```
wget http://img.whalin.com/memcached/jdk6/log4j/java_memcached-release_1.5.1.tar.gz
tar zxvf java_memcached-release_1.5.1.tar.gz
cd java_memcached-release_1.5.1
cp java_memcached-release_1.5.1.jar /usr/local/memcached/api/java

```

export CLASSPATH="./:/usr/local/java/lib:/usr/local/java/jre/lib:/usr/local/memcached/api/java/java_memcached-release_1.5.1.jar:/usr/local/memcached/api/java/log4j-1.2.14.jar"

例 28.1. memcached.java

```

import com.danga.MemCached.*;
import org.apache.log4j.*;
public class memcached {

	public static void main(String[] args) {
		try{
			BasicConfigurator.configure();

			String[] serverlist = { "127.0.0.1:11211"  };

			// initialize the pool for memcache servers
			SockIOPool pool = SockIOPool.getInstance( "test" );
			pool.setServers( serverlist );
			pool.setInitConn( 10 );
			pool.setMinConn( 5 );
			pool.setMaxConn( 250 );
			pool.setMaintSleep( 30 );
			pool.setNagle( false );
			pool.setSocketTO( 3000 );
			pool.initialize();

	        MemCachedClient mc = new MemCachedClient();

	        // compression is enabled by default
	        mc.setCompressEnable(true);

	        // set compression threshhold to 4 KB (default: 15 KB)
	        mc.setCompressThreshold(4096);

	        // turn on storing primitive types as a string representation
	        // Should not do this in most cases.
	        mc.setPrimitiveAsString(true);

	    	mc.setPoolName( "test" );

			for ( int i = 0; i < 10; i++ ) {
				boolean success = mc.set( "" + i, "Hello!" );
				String result = (String)mc.get( "" + i );
				System.out.println( String.format( "set( %d ): %s", i, success ) );
				System.out.println( String.format( "get( %d ): %s", i, result ) );
			}

			System.out.println( "\n\t -- sleeping --\n" );
			try { Thread.sleep( 10000 ); } catch ( Exception ex ) { }

			for ( int i = 0; i < 10; i++ ) {
				boolean success = mc.set( "" + i, "Hello!" );
				String result = (String)mc.get( "" + i );
				System.out.println( String.format( "set( %d ): %s", i, success ) );
				System.out.println( String.format( "get( %d ): %s", i, result ) );
			}

		}
		catch (Exception e)
		{
			System.out.println("[Exception] - " + e.toString());
		}
		finally {}

	}

}

```

test memcache

```
javac memcached.java
java memcached

```

## Jedis

```

<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.7.2</version>
    <type>jar</type>
    <scope>compile</scope>
</dependency>		

```

```

package cn.netkiller.redis;

import redis.clients.jedis.Jedis;

public class JedisTest {

	private static Jedis jedis;

	public static void main(String[] args) {
		jedis = new Jedis("192.168.2.1");
		System.out.println("Server is running: "+jedis.ping());
		jedis.set("foo", "bar");
		String value = jedis.get("foo");
		System.out.println(value);
	}

}

```

### 认证

```

jedis = new Jedis("localhost", 6379, 15000);
jedis.auth("foobared");

```

### jedis.keys

```

	public Map<String, String> getCaptcha() {

		Map<String, String> captchas = new HashMap<String, String>();
		this.jedis = new Jedis(this.config.getProperty("captcha.redis"));

		System.out.printf("%s: Redis server is running %s\n", this.getClass().getName(), this.jedis.ping());

		Set<String> keys = jedis.keys("captcha::phone::*");
		if (!keys.isEmpty()) {
			for(String key:keys){
				if (this.jedis.exists(key)) {
					captchas.put(key.replace("captcha::phone::", ""), this.jedis.get(key)+" ["+ String.valueOf(this.jedis.ttl(key)) + "s]");
				}
			}
		}

		this.jedis.close();

		return captchas;
	}		

```

```

import redis.clients.jedis.Jedis;
public class RedisKeys {
   public static void main(String[] args) {

      Jedis jedis = new Jedis("localhost");
      System.out.println("Connection to server sucessfully");

      List<String> list = jedis.keys("*");
      for(int i=0; i<list.size(); i++) {
         System.out.println("List of stored keys:: "+list.get(i));
      }
   }
}

```

## Ehcache