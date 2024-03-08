# 第 1 章 Java 12

## JVM

### 安装 Java 6

解压

```
chmod +x jdk-6u1-linux-i586.bin
./jdk-6u1-linux-i586.bin
输入"yes"回车

mv jdk1.6.0_01 /usr/local/
ln -s /usr/local/jdk1.6.0_01/ /usr/local/java

```

/etc/profile.d/java.sh

例 1.1. /etc/profile.d/java.sh

```
################################################
### Java environment by neo
################################################
export JAVA_HOME=/usr/local/java
export JRE_HOME=/usr/local/java/jre
export PATH=$PATH:/usr/local/java/bin:/usr/local/java/jre/bin
export CLASSPATH="./:/usr/local/java/lib:/usr/local/java/jre/lib:/usr/local/memcached/api/java"
export JAVA_OPTS="-Xms128m -Xmx1024m"

```

#### HeapDumpOnOutOfMemoryError

```
JAVA_OPTS = "$JAVA_OPTS -XX:+HeapDumpOnOutOfMemoryError"

```

如果针对 Tomcat 可以在 catalina.sh 加入

```
if [ "$1" = "debug" ] ; then
JAVA_OPTS = "$JAVA_OPTS -XX:+HeapDumpOnOutOfMemoryError"

```

### java-1.8.0-openjdk

```
# yum install -y java-1.8.0-openjdk		

```

### docker 环境

在 docker 中运行 java

```

neo@MacBook-Pro ~ % docker pull openjdk:12-jdk		

```

```

docker run -it openjdk:12-jdk /bin/jshell		

```

```

docker run -it openjdk:12-jdk /bin/bash
root@44d1d18351a8:/# java -version

```

### java - Launches a Java application.

#### java 9~11

直接使用 java 命令运行 *.java 文件

```

java netkiller.java		

```

#### -verbose:class 显示载入 jar 文件

```

# java -verbose:class hello
[Opened /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Object from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.Serializable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Comparable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.CharSequence from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.String from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.AnnotatedElement from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.GenericDeclaration from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Type from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Class from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Cloneable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassLoader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.System from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Throwable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Error from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ThreadDeath from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Exception from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.RuntimeException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.SecurityManager from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.ProtectionDomain from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.AccessControlContext from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.SecureClassLoader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ReflectiveOperationException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassNotFoundException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.LinkageError from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.NoClassDefFoundError from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassCastException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ArrayStoreException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.VirtualMachineError from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.OutOfMemoryError from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.StackOverflowError from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.IllegalMonitorStateException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.Reference from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.SoftReference from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.WeakReference from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.FinalReference from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.PhantomReference from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Cleaner from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.Finalizer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Runnable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Thread from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Thread$UncaughtExceptionHandler from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ThreadGroup from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Map from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Dictionary from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Hashtable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Properties from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.AccessibleObject from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Member from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Field from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Parameter from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Executable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Method from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Constructor from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.MagicAccessorImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.MethodAccessor from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.MethodAccessorImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.ConstructorAccessor from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.ConstructorAccessorImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.DelegatingClassLoader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.ConstantPool from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.FieldAccessor from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.FieldAccessorImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.UnsafeFieldAccessorImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.UnsafeStaticFieldAccessorImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.annotation.Annotation from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.CallerSensitive from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandle from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.DirectMethodHandle from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MemberName from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandleNatives from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.LambdaForm from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodType from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.BootstrapMethodError from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.CallSite from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.ConstantCallSite from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MutableCallSite from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.VolatileCallSite from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Appendable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.AbstractStringBuilder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.StringBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.StringBuilder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Unsafe from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.AutoCloseable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.Closeable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.InputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.ByteArrayInputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.File from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.net.URLClassLoader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.net.URL from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.jar.Manifest from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Launcher from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Launcher$AppClassLoader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Launcher$ExtClassLoader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.CodeSource from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.StackTraceElement from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.Buffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Boolean from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Character from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Number from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Float from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Double from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Byte from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Short from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Integer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Long from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.NullPointerException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ArithmeticException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.ObjectStreamField from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Comparator from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.String$CaseInsensitiveComparator from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.Guard from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.Permission from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.BasicPermission from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.RuntimePermission from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.AccessController from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.ReflectPermission from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.PrivilegedAction from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.ReflectionFactory$GetReflectionFactoryAction from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.cert.Certificate from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Iterable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.List from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.RandomAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.AbstractCollection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.AbstractList from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Vector from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Stack from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.ReflectionFactory from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.Reference$Lock from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.Reference$ReferenceHandler from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.ReferenceQueue from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.ReferenceQueue$Null from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.ReferenceQueue$Lock from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ref.Finalizer$FinalizerThread from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Map$Entry from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Hashtable$Entry from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.VM from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Math from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.Charset from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.spi.CharsetProvider from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.FastCharsetProvider from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.StandardCharsets from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.AbstractMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.util.PreHashedMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.StandardCharsets$Aliases from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.StandardCharsets$Classes from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.StandardCharsets$Cache from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ThreadLocal from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.atomic.AtomicInteger from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.IncompatibleClassChangeError from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.NoSuchMethodError from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.ArrayList from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Set from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.AbstractSet from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$EmptySet from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$EmptyList from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$EmptyMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$UnmodifiableCollection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$UnmodifiableList from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$UnmodifiableRandomAccessList from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.Reflection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.HashMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.HashMap$Node from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Class$3 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Class$ReflectionData from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Class$Atomic from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.generics.repository.AbstractRepository from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.generics.repository.GenericDeclRepository from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.generics.repository.ClassRepository from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Class$AnnotationData from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.annotation.AnnotationType from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.WeakHashMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassValue$ClassValueMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Modifier from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.LangReflectAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.ReflectAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Arrays from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.HistoricallyNamedCharset from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.Unicode from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.UTF_8 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Class$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.ReflectionFactory$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.NativeConstructorAccessorImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.DelegatingConstructorAccessorImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.StringCoding from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ThreadLocal$ThreadLocalMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ThreadLocal$ThreadLocalMap$Entry from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.StringCoding$StringDecoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.ArrayDecoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.CharsetDecoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.UTF_8$Decoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.CodingErrorAction from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Hashtable$EntrySet from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$SynchronizedCollection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$SynchronizedSet from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Objects from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Enumeration from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Iterator from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Hashtable$Enumerator from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Runtime from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Version from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FileInputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FileDescriptor from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.JavaIOFileDescriptorAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FileDescriptor$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.SharedSecrets from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.Flushable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.OutputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FileOutputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FilterInputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.BufferedInputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.atomic.AtomicReferenceFieldUpdater from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.atomic.AtomicReferenceFieldUpdater$AtomicReferenceFieldUpdaterImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.PrivilegedExceptionAction from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.atomic.AtomicReferenceFieldUpdater$AtomicReferenceFieldUpdaterImpl$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.reflect.misc.ReflectUtil from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FilterOutputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.PrintStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.BufferedOutputStream from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.Writer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.OutputStreamWriter from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.StreamEncoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.security.action.GetPropertyAction from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.ArrayEncoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.CharsetEncoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.UTF_8$Encoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.ByteBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.HeapByteBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.Bits from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.ByteOrder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.JavaNioAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.Bits$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.BufferedWriter from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.DefaultFileSystem from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FileSystem from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.UnixFileSystem from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.ExpiringCache from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.LinkedHashMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.ExpiringCache$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Enum from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.File$PathStatus from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.file.Watchable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.file.Path from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.StringCoding$StringEncoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassLoader$3 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.ExpiringCache$Entry from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.LinkedHashMap$Entry from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassLoader$NativeLibrary from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Terminator from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.SignalHandler from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Terminator$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Signal from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.NativeSignalHandler from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Integer$IntegerCache from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.OSEnvironment from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.JavaLangAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.System$2 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.IllegalArgumentException from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Compiler from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Compiler$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.net.URLStreamHandlerFactory from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Launcher$Factory from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.security.util.Debug from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassLoader$ParallelLoaders from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.WeakHashMap$Entry from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Collections$SetFromMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.WeakHashMap$KeySet from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.JavaNetAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.net.URLClassLoader$7 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.StringTokenizer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Launcher$ExtClassLoader$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.MetaIndex from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Readable from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.Reader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.BufferedReader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.InputStreamReader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FileReader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.cs.StreamDecoder from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.CharBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.HeapCharBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.CoderResult from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.CoderResult$Cache from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.CoderResult$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.charset.CoderResult$2 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.reflect.Array from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.HashMap$TreeNode from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FileInputStream$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.net.www.ParseUtil from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.BitSet from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Locale from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.util.locale.LocaleObjectCache from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Locale$Cache from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentHashMap from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.locks.Lock from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.locks.ReentrantLock from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentHashMap$Segment from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentHashMap$Node from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentHashMap$CounterCell from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentHashMap$CollectionView from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentHashMap$KeySetView from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentHashMap$ValuesView from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.ConcurrentHashMap$EntrySetView from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.util.locale.BaseLocale from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.util.locale.BaseLocale$Cache from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.util.locale.BaseLocale$Key from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.util.locale.LocaleObjectCache$CacheEntry from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.Locale$LocaleKey from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.util.locale.LocaleUtils from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.CharacterData from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.CharacterDataLatin1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.net.Parts from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.net.URLStreamHandler from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.net.www.protocol.file.Handler from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.JavaSecurityAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.ProtectionDomain$JavaSecurityAccessImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.JavaSecurityProtectionDomainAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.ProtectionDomain$2 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.ProtectionDomain$Key from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.Principal from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.HashSet from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.URLClassPath from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.net.www.protocol.jar.Handler from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Launcher$AppClassLoader$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.SystemClassLoaderAction from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandleImpl from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandleImpl$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.function.Function from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandleImpl$2 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandleImpl$3 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassValue from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandleImpl$4 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassValue$Entry from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassValue$Identity from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.ClassValue$Version from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MemberName$Factory from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandleStatics from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.invoke.MethodHandleStatics$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.PostVMInitHook from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.usagetracker.UsageTrackerClient from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.concurrent.atomic.AtomicBoolean from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.usagetracker.UsageTrackerClient$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.usagetracker.UsageTrackerClient$4 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.usagetracker.UsageTrackerClient$3 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FileOutputStream$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.launcher.LauncherHelper from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.net.URLClassLoader$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.net.util.URLUtil from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.URLClassPath$3 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.URLClassPath$Loader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.URLClassPath$JarLoader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.zip.ZipConstants from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.zip.ZipFile from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.JavaUtilZipFileAccess from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.util.zip.ZipFile$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.URLClassPath$FileLoader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Resource from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.URLClassPath$FileLoader$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.ByteBuffered from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.PerfCounter from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Perf$GetPerfAction from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.Perf from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.misc.PerfCounter$CoreCounters from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.nio.ch.DirectBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.MappedByteBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.DirectByteBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.LongBuffer from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.nio.DirectLongBufferU from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.PermissionCollection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.Permissions from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.net.URLConnection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.net.www.URLConnection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.net.www.protocol.file.FileURLConnection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded sun.net.www.MessageHeader from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FilePermission from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FilePermission$1 from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.io.FilePermissionCollection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.AllPermission from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.UnresolvedPermission from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.security.BasicPermissionCollection from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded hello from file:/root/java/]
[Loaded sun.launcher.LauncherHelper$FXHelper from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Class$MethodArray from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Void from /srv/jdk1.8.0_60/jre/lib/rt.jar]
Hello
[Loaded java.lang.Shutdown from /srv/jdk1.8.0_60/jre/lib/rt.jar]
[Loaded java.lang.Shutdown$Lock from /srv/jdk1.8.0_60/jre/lib/rt.jar]				

```

#### java.io.tmpdir

临时文件目录

```
java -Djava.io.tmpdir=/path/to/tmpdir		

```

#### 显示版本号

```
$ java -version
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)			

```

#### 列出 java 模块

```

neo@MacBook-Pro ~ % java --list-modules
java.activation@10.0.1
java.base@10.0.1
java.compiler@10.0.1
java.corba@10.0.1
java.datatransfer@10.0.1
java.desktop@10.0.1
java.instrument@10.0.1
java.jnlp@10.0.1
java.logging@10.0.1
java.management@10.0.1
java.management.rmi@10.0.1
java.naming@10.0.1
java.prefs@10.0.1
java.rmi@10.0.1
java.scripting@10.0.1
java.se@10.0.1
java.se.ee@10.0.1
java.security.jgss@10.0.1
java.security.sasl@10.0.1
java.smartcardio@10.0.1
java.sql@10.0.1
java.sql.rowset@10.0.1
java.transaction@10.0.1
java.xml@10.0.1
java.xml.bind@10.0.1
java.xml.crypto@10.0.1
java.xml.ws@10.0.1
java.xml.ws.annotation@10.0.1
javafx.base@10.0.1
javafx.controls@10.0.1
javafx.deploy@10.0.1
javafx.fxml@10.0.1
javafx.graphics@10.0.1
javafx.media@10.0.1
javafx.swing@10.0.1
javafx.web@10.0.1
jdk.accessibility@10.0.1
jdk.aot@10.0.1
jdk.attach@10.0.1
jdk.charsets@10.0.1
jdk.compiler@10.0.1
jdk.crypto.cryptoki@10.0.1
jdk.crypto.ec@10.0.1
jdk.deploy@10.0.1
jdk.deploy.controlpanel@10.0.1
jdk.dynalink@10.0.1
jdk.editpad@10.0.1
jdk.hotspot.agent@10.0.1
jdk.httpserver@10.0.1
jdk.incubator.httpclient@10.0.1
jdk.internal.ed@10.0.1
jdk.internal.jvmstat@10.0.1
jdk.internal.le@10.0.1
jdk.internal.opt@10.0.1
jdk.internal.vm.ci@10.0.1
jdk.internal.vm.compiler@10.0.1
jdk.internal.vm.compiler.management@10.0.1
jdk.jartool@10.0.1
jdk.javadoc@10.0.1
jdk.javaws@10.0.1
jdk.jcmd@10.0.1
jdk.jconsole@10.0.1
jdk.jdeps@10.0.1
jdk.jdi@10.0.1
jdk.jdwp.agent@10.0.1
jdk.jfr@10.0.1
jdk.jlink@10.0.1
jdk.jshell@10.0.1
jdk.jsobject@10.0.1
jdk.jstatd@10.0.1
jdk.localedata@10.0.1
jdk.management@10.0.1
jdk.management.agent@10.0.1
jdk.management.cmm@10.0.1
jdk.management.jfr@10.0.1
jdk.management.resource@10.0.1
jdk.naming.dns@10.0.1
jdk.naming.rmi@10.0.1
jdk.net@10.0.1
jdk.pack@10.0.1
jdk.packager@10.0.1
jdk.packager.services@10.0.1
jdk.plugin@10.0.1
jdk.plugin.server@10.0.1
jdk.rmic@10.0.1
jdk.scripting.nashorn@10.0.1
jdk.scripting.nashorn.shell@10.0.1
jdk.sctp@10.0.1
jdk.security.auth@10.0.1
jdk.security.jgss@10.0.1
jdk.snmp@10.0.1
jdk.unsupported@10.0.1
jdk.xml.bind@10.0.1
jdk.xml.dom@10.0.1
jdk.xml.ws@10.0.1
jdk.zipfs@10.0.1
oracle.desktop@10.0.1
oracle.net@10.0.1

```

模块所在位置 /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/jmods

```

cd /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/jmods

neo@MacBook-Pro /Library/Java/JavaVirtualMachines/jdk-10.0.1.jdk/Contents/Home/jmods % ll -1
java.activation.jmod
java.base.jmod
java.compiler.jmod
java.corba.jmod
java.datatransfer.jmod
java.desktop.jmod
java.instrument.jmod
java.jnlp.jmod
java.logging.jmod
java.management.jmod
java.management.rmi.jmod
java.naming.jmod
java.prefs.jmod
java.rmi.jmod
java.scripting.jmod
java.se.ee.jmod
java.se.jmod
java.security.jgss.jmod
java.security.sasl.jmod
java.smartcardio.jmod
java.sql.jmod
java.sql.rowset.jmod
java.transaction.jmod
java.xml.bind.jmod
java.xml.crypto.jmod
java.xml.jmod
java.xml.ws.annotation.jmod
java.xml.ws.jmod
javafx.base.jmod
javafx.controls.jmod
javafx.deploy.jmod
javafx.fxml.jmod
javafx.graphics.jmod
javafx.media.jmod
javafx.swing.jmod
javafx.web.jmod
jdk.accessibility.jmod
jdk.aot.jmod
jdk.attach.jmod
jdk.charsets.jmod
jdk.compiler.jmod
jdk.crypto.cryptoki.jmod
jdk.crypto.ec.jmod
jdk.deploy.controlpanel.jmod
jdk.deploy.jmod
jdk.dynalink.jmod
jdk.editpad.jmod
jdk.hotspot.agent.jmod
jdk.httpserver.jmod
jdk.incubator.httpclient.jmod
jdk.internal.ed.jmod
jdk.internal.jvmstat.jmod
jdk.internal.le.jmod
jdk.internal.opt.jmod
jdk.internal.vm.ci.jmod
jdk.internal.vm.compiler.jmod
jdk.internal.vm.compiler.management.jmod
jdk.jartool.jmod
jdk.javadoc.jmod
jdk.javaws.jmod
jdk.jcmd.jmod
jdk.jconsole.jmod
jdk.jdeps.jmod
jdk.jdi.jmod
jdk.jdwp.agent.jmod
jdk.jfr.jmod
jdk.jlink.jmod
jdk.jshell.jmod
jdk.jsobject.jmod
jdk.jstatd.jmod
jdk.localedata.jmod
jdk.management.agent.jmod
jdk.management.cmm.jmod
jdk.management.jfr.jmod
jdk.management.jmod
jdk.management.resource.jmod
jdk.naming.dns.jmod
jdk.naming.rmi.jmod
jdk.net.jmod
jdk.pack.jmod
jdk.packager.jmod
jdk.packager.services.jmod
jdk.plugin.jmod
jdk.plugin.server.jmod
jdk.rmic.jmod
jdk.scripting.nashorn.jmod
jdk.scripting.nashorn.shell.jmod
jdk.sctp.jmod
jdk.security.auth.jmod
jdk.security.jgss.jmod
jdk.snmp.jmod
jdk.unsupported.jmod
jdk.xml.bind.jmod
jdk.xml.dom.jmod
jdk.xml.ws.jmod
jdk.zipfs.jmod
oracle.desktop.jmod
oracle.net.jmod			

```

### jar

查看包中的文件列表 jar -tf package.war/package.jar

```
$ /srv/java/bin/jar -tf mis.netkiller.cn-0.0.1.war |more
META-INF/
META-INF/MANIFEST.MF
WEB-INF/
WEB-INF/jsp/
WEB-INF/jsp/include/
WEB-INF/jsp/system/
WEB-INF/jsp/banner/		

```

### jdeps - Java class dependency analyzer.

包类依赖分析器

```
[net@netkiller lib]$ jdeps jersey-client-1.18.1.jar
jersey-client-1.18.1.jar -> not found
jersey-client-1.18.1.jar -> /usr/java/jdk1.8.0_73/jre/lib/rt.jar
   com.sun.jersey.api.client (jersey-client-1.18.1.jar)
      -> com.sun.jersey.api.client.async                    jersey-client-1.18.1.jar
      -> com.sun.jersey.api.client.config                   jersey-client-1.18.1.jar
      -> com.sun.jersey.api.client.filter                   jersey-client-1.18.1.jar
      -> com.sun.jersey.client.impl                         jersey-client-1.18.1.jar
      -> com.sun.jersey.client.impl.async                   jersey-client-1.18.1.jar
      -> com.sun.jersey.client.proxy                        jersey-client-1.18.1.jar
      -> com.sun.jersey.client.urlconnection                jersey-client-1.18.1.jar
      -> com.sun.jersey.core.header                         not found
      -> com.sun.jersey.core.provider                       not found
      -> com.sun.jersey.core.reflection                     not found
      -> com.sun.jersey.core.spi.component                  not found
      -> com.sun.jersey.core.spi.component.ioc              not found
      -> com.sun.jersey.core.spi.factory                    not found
      -> com.sun.jersey.core.util                           not found
      -> com.sun.jersey.spi                                 not found
      -> com.sun.jersey.spi.inject                          not found
      -> com.sun.jersey.spi.service                         not found
      -> java.io                                            
      -> java.lang                                          
      -> java.lang.annotation                               
      -> java.lang.reflect                                  
      -> java.net                                           
      -> java.util                                          
      -> java.util.concurrent                               
      -> java.util.logging                                  
      -> javax.ws.rs.core                                   not found
      -> javax.ws.rs.ext                                    not found
   com.sun.jersey.api.client.async (jersey-client-1.18.1.jar)
      -> com.sun.jersey.api.client                          jersey-client-1.18.1.jar
      -> java.lang                                          
      -> java.util.concurrent                               
   com.sun.jersey.api.client.config (jersey-client-1.18.1.jar)
      -> com.sun.jersey.core.util                           not found
      -> java.lang                                          
      -> java.util                                          
   com.sun.jersey.api.client.filter (jersey-client-1.18.1.jar)
      -> com.sun.jersey.api.client                          jersey-client-1.18.1.jar
      -> com.sun.jersey.core.util                           not found
      -> java.io                                            
      -> java.lang                                          
      -> java.net                                           
      -> java.nio.charset                                   
      -> java.security                                      
      -> java.util                                          
      -> java.util.logging                                  
      -> java.util.regex                                    
      -> java.util.zip                                      
      -> javax.ws.rs                                        not found
      -> javax.ws.rs.core                                   not found
   com.sun.jersey.client.impl (jersey-client-1.18.1.jar)
      -> com.sun.jersey.api.client                          jersey-client-1.18.1.jar
      -> com.sun.jersey.core.header                         not found
      -> java.io                                            
      -> java.lang                                          
      -> java.net                                           
      -> java.util                                          
      -> java.util.concurrent.atomic                        
      -> javax.ws.rs.core                                   not found
   com.sun.jersey.client.impl.async (jersey-client-1.18.1.jar)
      -> com.sun.jersey.api.client                          jersey-client-1.18.1.jar
      -> com.sun.jersey.api.client.async                    jersey-client-1.18.1.jar
      -> java.lang                                          
      -> java.util.concurrent                               
   com.sun.jersey.client.proxy (jersey-client-1.18.1.jar)
      -> com.sun.jersey.api.client                          jersey-client-1.18.1.jar
      -> com.sun.jersey.api.client.async                    jersey-client-1.18.1.jar
      -> java.lang                                          
      -> java.util.concurrent                               
   com.sun.jersey.client.urlconnection (jersey-client-1.18.1.jar)
      -> com.sun.jersey.api.client                          jersey-client-1.18.1.jar
      -> com.sun.jersey.core.header                         not found
      -> com.sun.jersey.spi                                 not found
      -> java.io                                            
      -> java.lang                                          
      -> java.lang.reflect                                  
      -> java.net                                           
      -> java.security                                      
      -> java.util                                          
      -> java.util.logging                                  
      -> javax.net.ssl                                      
      -> javax.ws.rs.core                                   not found
   com.sun.ws.rs.ext (jersey-client-1.18.1.jar)
      -> com.sun.jersey.core.spi.factory                    not found
      -> java.lang                                          
      -> javax.ws.rs.core                                   not found

```

### JShell

JShell，即 Java Shell。从 java9 开始，java 开始引入了 REPL（Read-Eval-Print Loop，读取-求值-输出 循环）工具

```

neo@MacBook-Pro ~ % jshell
|  Welcome to JShell -- Version 12
|  For an introduction type: /help intro

jshell> 		

```

#### /help 显示帮助信息

```

jshell> /help
|  Type a Java language expression, statement, or declaration.
|  Or type one of the following commands:
|  /list [<name or id>|-all|-start]
|  	list the source you have typed
|  /edit <name or id>
|  	edit a source entry
|  /drop <name or id>
|  	delete a source entry
|  /save [-all|-history|-start] <file>
|  	Save snippet source to a file
|  /open <file>
|  	open a file as source input
|  /vars [<name or id>|-all|-start]
|  	list the declared variables and their values
|  /methods [<name or id>|-all|-start]
|  	list the declared methods and their signatures
|  /types [<name or id>|-all|-start]
|  	list the type declarations
|  /imports 
|  	list the imported items
|  /exit [<integer-expression-snippet>]
|  	exit the jshell tool
|  /env [-class-path <path>] [-module-path <path>] [-add-modules <modules>] ...
|  	view or change the evaluation context
|  /reset [-class-path <path>] [-module-path <path>] [-add-modules <modules>]...
|  	reset the jshell tool
|  /reload [-restore] [-quiet] [-class-path <path>] [-module-path <path>]...
|  	reset and replay relevant history -- current or previous (-restore)
|  /history [-all]
|  	history of what you have typed
|  /help [<command>|<subject>]
|  	get information about using the jshell tool
|  /set editor|start|feedback|mode|prompt|truncation|format ...
|  	set configuration information
|  /? [<command>|<subject>]
|  	get information about using the jshell tool
|  /! 
|  	rerun last snippet -- see /help rerun
|  /<id> 
|  	rerun snippets by ID or ID range -- see /help rerun
|  /-<n> 
|  	rerun n-th previous snippet -- see /help rerun
|  
|  For more information type '/help' followed by the name of a
|  command or a subject.
|  For example '/help /list' or '/help intro'.
|  
|  Subjects:
|  
|  intro
|  	an introduction to the jshell tool
|  keys
|  	a description of readline-like input editing
|  id
|  	a description of snippet IDs and how use them
|  shortcuts
|  	a description of keystrokes for snippet and command completion,
|  	information access, and automatic code generation
|  context
|  	a description of the evaluation context options for /env /reload and /reset
|  rerun
|  	a description of ways to re-evaluate previously entered snippets

```

介绍信息

```

jshell> /help intro
|  
|                                   intro
|                                   =====
|  
|  The jshell tool allows you to execute Java code, getting immediate results.
|  You can enter a Java definition (variable, method, class, etc), like:  int x = 8
|  or a Java expression, like:  x + x
|  or a Java statement or import.
|  These little chunks of Java code are called 'snippets'.
|  
|  There are also the jshell tool commands that allow you to understand and
|  control what you are doing, like:  /list
|  
|  For a list of commands: /help			

```

#### 退出命令

```

jshell> /exit
|  Goodbye

```

## System

```
Java.version 运行时环境版本

java.vendor 运行时环境供应商

java.vendor.url 供应商的 URL

java.home 安装目录

java.vm.specification.version 虚拟机规范版本

java.vm.specification.vendor 虚拟机规范供应商

java.vm.specification.name 虚拟机规范名称

java.vm.version 虚拟机实现版本

java.vm.vendor 虚拟机实现供应商

java.vm.name 虚拟机实现名称

java.specification.version 运行时环境规范版本

java.specification.vendor Java 运行时环境规范供应商

java.specification.name Java 运行时环境规范名称

java.class.version Java 类格式版本号

java.class.path Java 类路径

java.library.path 加载库时搜索的路径列表

java.io.tmpdir 默认的临时文件路径

java.compiler 要使用的 JIT 编译器的名称

java.ext.dirs 一个或多个扩展目录的路径

os.name 操作系统的名称

os.arch 操作系统的架构

os.version 操作系统的版本

file.separator 文件分隔符（在 UNIX 系统中是“/”）

path.separator 路径分隔符（在 UNIX 系统中是“:”）

line.separator 行分隔符（在 UNIX 系统中是“/n”）

user.name 用户的账户名称

user.home 用户的主目录

user.dir 用户的当前工作目录	

```

### user.dir

```

public class Test {

	public static void main(String[] args) {
		System.out.println("Working Directory = " + System.getProperty("user.dir"));

		System.out.println(System.getProperty("user.home"));
		System.out.println(System.getProperty("java.version"));
		System.out.println(System.getProperty("os.name"));
		System.out.println(System.getProperty("java.vendor.url"));
	}
}

```

### java.io.tmpdir

改变 java.io.tmpdir 的默认值

```
System.setProperty("java.io.tmpdir", "/vat/tmp");
System.out.println(System.getProperty("java.io.tmpdir"));

```

如果你希望从外部修改这个系统属性的话，你可以使用-D 这个参数，例如 java -Djava.io.tmpdir=/path/to/tmpdir

```
System.out.println(System.getProperty("java.io.tmpdir"));

```

### 打印当前 Java 文件的默认编码

```

package cn.netkiller.test;

import java.nio.charset.Charset;

public class Test {

	public Test() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {

		System.out.println(System.getProperty("file.encoding"));    
		System.out.println(Charset.defaultCharset());   

	}

}

```

### 自定义

```

	public static void main(String[] args) {
		try {
			Oracle oracle = new Oracle();
			if(System.getProperty("config") == null){
				System.exit(1);
			}
			oracle.openConfig(System.getProperty("config"));
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}

	}

```

上面程序编译打包后运行

```

neo@netkiller:~/project/Oracle$ java -jar -Dconfig=jdbc.properties target/oracle-0.0.1-SNAPSHOT.jar

```

### System.in 标准输入(Stdin)

标准输入一般用于管道输入，例如：

```

cat test.txt | java cn.netkiller.ipo.test.StdinToStdout

```

下面的程序例子里从管道输入，并从标准输出打印。

```

package cn.netkiller.ipo.test;

import java.io.BufferedReader;
import java.io.InputStreamReader;

public class StdinToStdout {

	public StdinToStdout() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {

		String s = "";
		try {
			BufferedReader stdIn = new BufferedReader(new InputStreamReader(System.in));

			while ((s = stdIn.readLine()) != null) {
				System.out.println(s);
			}

		} catch (Exception e) {
			e.printStackTrace();
		}

	}
}

```

默认 BufferedReader 缓冲区比较小，无法处理大于 1000 行的输入，可以通过配置缓冲区来解决。

```

stdin = new BufferedReader(new InputStreamReader(System.in, "utf-8"), 1024 * 1024 * 1024); // 1G 缓冲区		

```

## exec 运行 shell

```

exec(String[] cmdarray, String[] envp, File dir)
Executes the specified command and arguments in a separate process with the specified environment and working directory.

那个 dir 就是调用的程序的工作目录，这句其实还是很有用的。

Windows 下调用程序
Process proc =Runtime.getRuntime().exec("file.exe");

Linux 下调用程序就要改成下面的格式
Process proc =Runtime.getRuntime().exec("./file");

Windows 下调用系统命令
String [] cmd={"cmd","/C","copy file1.txt file2.txt"};
Process proc =Runtime.getRuntime().exec(cmd);

Linux 下调用系统命令就要改成下面的格式
String [] cmd={"/bin/sh","-c","ln -s file1 file2"};
Process proc =Runtime.getRuntime().exec(cmd);

Windows 下调用系统命令并弹出命令行窗口
String [] cmd={"cmd","/C","start copy file1.txt file2.txt"};
Process proc =Runtime.getRuntime().exec(cmd);

Linux 下调用系统命令并弹出终端窗口就要改成下面的格式
String [] cmd={"/bin/sh","-c","xterm -e ln -s file1 file2"};
Process proc =Runtime.getRuntime().exec(cmd);

还有要设置调用程序的工作目录就要
Process proc =Runtime.getRuntime().exec("ls",null, new File("/bin"));

```

## 类型

数据类型的最大值和最小值。

```

基本类型：int 二进制位数：32
包装类：java.lang.Integer
最小值：Integer.MIN_VALUE= -2147483648 （-2 的 31 次方）
最大值：Integer.MAX_VALUE= 2147483647  （2 的 31 次方-1）

基本类型：short 二进制位数：16
包装类：java.lang.Short
最小值：Short.MIN_VALUE=-32768 （-2 的 15 此方）
最大值：Short.MAX_VALUE=32767 （2 的 15 次方-1）

基本类型：long 二进制位数：64
包装类：java.lang.Long
最小值：Long.MIN_VALUE=-9223372036854775808 （-2 的 63 次方）
最大值：Long.MAX_VALUE=9223372036854775807 （2 的 63 次方-1）

基本类型：float 二进制位数：32
包装类：java.lang.Float
最小值：Float.MIN_VALUE=1.4E-45 （2 的-149 次方）
最大值：Float.MAX_VALUE=3.4028235E38 （2 的 128 次方-1）

基本类型：double 二进制位数：64
包装类：java.lang.Double
最小值：Double.MIN_VALUE=4.9E-324 （2 的-1074 次方）
最大值：Double.MAX_VALUE=1.7976931348623157E308 （2 的 1024 次方-1）	

```

### var 本地变量类型推断

```

var javastack = "javastack";
就等于：
String javastack = "javastack";		

```

### Integer

```

十进制转成十六进制：   

Integer.toHexString(int i)   

十进制转成八进制   

Integer.toOctalString(int i)   

十进制转成二进制   

Integer.toBinaryString(int i)   

十六进制转成十进制   

Integer.valueOf("FFFF",16).toString()   

八进制转成十进制   

Integer.valueOf("876",8).toString()   

二进制转十进制   

Integer.valueOf("0101",2).toString()   

有什么方法可以直接将 2,8,16 进制直接转换为 10 进制的吗?   

java.lang.Integer 类   

parseInt(String s, int radix)   

使用第二个参数指定的基数，将字符串参数解析为有符号的整数。   

examples from jdk:   

parseInt("0", 10) returns 0   

parseInt("473", 10) returns 473   

parseInt("-0", 10) returns 0   

parseInt("-FF", 16) returns -255   

parseInt("1100110", 2) returns 102   

parseInt("2147483647", 10) returns 2147483647   

parseInt("-2147483648", 10) returns -2147483648   

parseInt("2147483648", 10) throws a NumberFormatException   

parseInt("99",throws a NumberFormatException   

parseInt("Kona", 10) throws a NumberFormatException   

parseInt("Kona", 27) returns 411787    

例二   

int i=100;   

String binStr=Integer.toBinaryString(i);   
String otcStr=Integer.toOctalString(i);   
String hexStr=Integer.toHexString(i);   
System.out.println(binStr);   

例二   

Integer factor = Integer.valueOf(args[0]);   

String s;   

s = String.format("%d", factor);     
System.out.println(s);   

s = String.format("%x", factor);   
System.out.println(s);   

s = String.format("%o", factor);   
System.out.println(s);   

其他方法：   

Integer.toHexString(你的 10 进制数);   

例如   

String temp = Integer.toHexString(75);   

输出 temp 就为 4b     		

```

#### 前面补零

```

public static String frontCompWithZore(int sourceDate,int formatLength) {  

　　String newString = String.format("%0"+formatLength+"d", sourceDate);   
　　return newString;  
}  			

```

### String

Java 11 增加了一系列的字符串处理方法，如以下所示。

```

// 判断字符串是否为空白
" ".isBlank(); // true
// 去除首尾空格
" Javastack ".strip(); // "Javastack"
// 去除尾部空格
" Javastack ".stripTrailing(); // " Javastack"
// 去除首部空格
" Javastack ".stripLeading(); // "Javastack "

```

#### 查找字符重现的位置

```

package cn.netkiller.test;

public class Test {

	public static void main(String[] args) {

		String string = new String("http://www.netkiller.cn");

		System.out.println("查找字符 . 第一次出现的位置: " + string.indexOf('.'));
		System.out.println("从第 15 个字符位置查找字符 . 出现的位置:" + string.indexOf('.', 15));
	}
}	

```

#### 行数统计

```

"A\nB\nC".lines().count(); // 3			

```

#### 复制字符串

```

"Java".repeat(3);// "JavaJavaJava"			

```

#### 随机字符串

```

	public String randomString(String chars, int length) {
		Random rand = new Random();
		StringBuilder buf = new StringBuilder();
		for (int i = 0; i < length; i++) {
			buf.append(chars.charAt(rand.nextInt(chars.length())));
		}
		return buf.toString();
	}

	/**
     * 获取 4 位随机验证码
     * @return
     */
    public static String getValidationCode(){
        return String.valueOf((new Random().nextInt(8999) + 1000));
    }

    /**
     * 获取 6 位随机验证码
     * @return
     */
    public static String getValidationCode(){
        return String.valueOf((new Random().nextInt(899999) + 100000));
    }	

```

#### 字符串替换处理

```

public class Test {

	public Test() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		System.out.println("2010-09-11T20:00:30".replace("T", " "));
	}
}

```

```
				{"status":0,"message":"","bankcode":"ABOC;IBC;CCTB;ICBC"}
				转换后
				{\"status\":0,\"message\":\"\",\"bankcode\":\"ABOC;IBC;CCTB;ICBC\"}

```

```

package test;

public class str {

	public static void main(String[] args) {
		String jsonString = "{\"status\":0,\"message\":\"\",\"bankcode\":\"ABOC;IBC;CCTB;ICBC\"}";
		System.out.println(jsonString);
		System.out.println(jsonString.replace("\"", "\\\""));
	}

}

```

##### 正则表达式查找与替换

查找特定字符并替换为找到的内容

```

package cn.netkiller.type;

public class ragexTest {

	public ragexTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		String str = "<html>Netkiller</html>";
		String regex = "<html>|</html>";
		//运行结果返回 Netkiller
		System.out.println(str.replaceAll(regex, ""));

		// 运行结果返回 Neo
		System.out.println("CN/NETKILLER/WWW/Neo_Chen".replaceAll(".*/(.+)_Chen", "$1"));
	}
}

```

#### substring

```

例如：
String str = "helloword!!!";

System.out.println(str.substring(1,4));

System.out.println(str.substring(3,5));

System.out.println(str.substring(0,4));

将得到结果为：

ell

lo 

hell

```

#### string to timestamp

Timestamp 转化为 String:

```
				SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"); //定义格式，不显示毫秒
				Timestamp now = new Timestamp(System.currentTimeMillis());
				//获取系统当前时间
				String str = df.format(now);

```

String 转化为 Timestamp:

```
				SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
				String time = df.format(new Date());
				Timestamp ts = Timestamp.valueOf(time);

```

#### String.strip

java11 对 String 类新增了 strip，stripLeading 以及 stripTrailing 方法，除了 strip 相关的方法还新增了 isBlank、lines、repeat(int)等方法

```

	@Test
    public void testStrip(){
        String text = "  \u2000a  b  ";
        Assert.assertEquals("a  b",text.strip());
        Assert.assertEquals("\u2000a  b",text.trim());
        Assert.assertEquals("a  b  ",text.stripLeading());
        Assert.assertEquals("  \u2000a  b",text.stripTrailing());
    }

```

#### Ascii

```

		String string = "佛山市 123 南海区 ABC 精密 abc 机械有限公司􁵪􁻠􁴹􄲀􀞜􀨨,";

		String clean = string.replaceAll("\\P{Print}", "");
		System.out.println(clean);			

```

#### 字符串处理，删除中文以外的字符

Unicode 码表 https://www.ssec.wisc.edu/~tomw/java/unicode.html

```

	String string = "2017-12-18 netkiller http://www.netkiller.cn -  网站正常";
	System.out.println(string.replaceAll("[^\u4e00-\u9fa5]", ""));			

```

#### 取出字符串中的中文字符

Unicode 码表 https://www.ssec.wisc.edu/~tomw/java/unicode.html

```

		String string = "2017-12-18 netkiller http://www.netkiller.cn -  网站正常";
		String regex = "[\u4e00-\u9fa5]";
		System.out.println(string.replaceAll(regex, ""));			

```

### 类型转换

#### Long to String

```

	public class MainClass {

	  public static void main(String[] arg) {
	    long b = 12L;
	    System.out.println(String.valueOf(b));   

	  }
	}

```

### Date

java.util.Date, java.sql.Date, java.sql.Time, java.sql.Timestamp 区别

```

package cn.netkiller.java.date;

/**
 * Hello world!
 *
 */
public class App {
	public static void main(String[] args) {
		System.out.println("Hello World!");

		// Get standard date and time
		java.util.Date utilDate = new java.util.Date();
		long javaTime = utilDate.getTime();
		System.out.println("The Java Date is:" + utilDate.toString());

		// Get and display SQL DATE
		java.sql.Date sqlDate = new java.sql.Date(javaTime);
		System.out.println("The SQL DATE is: " + sqlDate.toString());

		// Get and display SQL TIME
		java.sql.Time sqlTime = new java.sql.Time(javaTime);
		System.out.println("The SQL TIME is: " + sqlTime.toString());

		// Get and display SQL TIMESTAMP
		java.sql.Timestamp sqlTimestamp = new java.sql.Timestamp(javaTime);
		System.out.println("The SQL TIMESTAMP is: " + sqlTimestamp.toString());
	}
}

```

```
			The Java Date is:Thu Aug 24 16:51:57 CST 2017
			The SQL DATE is: 2017-08-24
			The SQL TIME is: 16:51:57
			The SQL TIMESTAMP is: 2017-08-24 16:51:57.234

```

#### SimpleDateFormat

```
				public static void main(String[] args) {

				DateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
				//get current date time with Date()
				Date date = new Date();
				System.out.println(dateFormat.format(date));

				//get current date time with Calendar()
				Calendar cal = Calendar.getInstance();
				System.out.println(dateFormat.format(cal.getTime()));

				}

```

#### Timestamp

```
				Timestamp timestamp = new Timestamp(System.currentTimeMillis());

				Date date = new Date();
				Timestamp timestamp = new Timestamp(date.getTime());

```

#### TimeZone

```
				package cn.netkiller.example;

				import java.sql.Timestamp;
				import java.text.SimpleDateFormat;
				import java.util.Calendar;
				import java.util.Date;
				import java.util.GregorianCalendar;
				import java.util.TimeZone;

				public class TimeZoneTest {

				public TimeZoneTest() {
				// TODO Auto-generated constructor stub
				}

				public static void main(String[] args) {
				// TODO Auto-generated method stub

				SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

				TimeZone timeZone = TimeZone.getTimeZone("Asia/Harbin");

				Date date = new Date();
				Timestamp timestamp = new Timestamp(date.getTime());

				System.out.println(timestamp);

				timestamp.setHours(timestamp.getHours()+8);
				System.out.println(timestamp);

				simpleDateFormat.setTimeZone(TimeZone.getTimeZone("GMT"));
				System.out.println(simpleDateFormat.format(date));

				simpleDateFormat.setTimeZone(TimeZone.getTimeZone("Asia/Harbin"));
				System.out.println(simpleDateFormat.format(date));

				Calendar calendar = new GregorianCalendar();
				calendar.setTime(date);
				calendar.setTimeZone(timeZone);
				System.out.println(simpleDateFormat.format(calendar.getTime()));
				}

				}

```

#### String to Date

```

package cn.netkiller.example;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class StringToDate {

	public StringToDate() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		String dateString = "2008-8-8 8:8:8";

		try {

			Date date = formatter.parse(dateString);
			System.out.println(date);
			System.out.println(formatter.format(date));

		} catch (ParseException e) {
			e.printStackTrace();
		}
	}

}			

```

#### 比较两个日期与时间

```

package cn.netkiller.example;

import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateCompare {

	public DateCompare() {
		// TODO Auto-generated constructor stub
	}

	public void fun1() throws InterruptedException {
		Date d1 = new Date();
		Thread.sleep(5000);
		Date d2 = new Date();
		if (d1.before(d2)) {
			System.out.println(String.format("%s < %s", d1.toString(), d2.toString()));
		} else {
			System.out.println(String.format("%s > %s", d1.toString(), d2.toString()));
		}
		if (d2.after(d1)) {
			System.out.println(String.format("%s > %s", d2.toString(), d1.toString()));
		}

		System.out.println(String.format("%s : %s => %d", d2.toString(), d1.toString(), d1.compareTo(d2)));
		System.out.println(String.format("%s : %s => %d", d1.toString(), d2.toString(), d2.compareTo(d1)));
	}

	public void fun2() throws InterruptedException {

		DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");

		Date date1 = new Date();
		Date date2 = new Date();
		String s1 = dateFormat.format(date1);
		String s2 = dateFormat.format(date2);
		System.out.println(String.format("%s : %s => %d", s1, s2, s1.compareTo(s2)));

		date1 = new Date();
		Thread.sleep(5000);
		date2 = new Date();
		s1 = dateFormat.format(date1);
		s2 = dateFormat.format(date2);
		System.out.println(String.format("%s : %s => %d", s1, s2, s1.compareTo(s2)));
		System.out.println(String.format("%s : %s => %d", s2, s1, s2.compareTo(s1)));
		System.out.println();
	}

	public void fun3() throws InterruptedException, ParseException {
		DateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm");
		//Date time = formatter.parse("2016-09-27 16:29");
		Date time = formatter.parse("2016-08-09 09:15");
		Date startDate = formatter.parse("2016-08-09 09:15");
		Date endDate = formatter.parse("2016-09-27 16:30");

		if (time.before(startDate) || time.after(endDate)) {
			System.out.println("Skip");
		}
	}

	public static void main(String[] args) throws InterruptedException {
		// TODO Auto-generated method stub
		DateCompare dateCompare = new DateCompare();
		dateCompare.fun1();
		System.out.println();
		dateCompare.fun2();
		System.out.println();
		dateCompare.fun3();
	}

}

```

#### Calendar

```

		Calendar cal = Calendar.getInstance();
		int year = cal.get(Calendar.YEAR);
		int month = cal.get(Calendar.MONTH )+1;

		System.out.println(year + "年 " + month + "月");

```

#### getToday

```

	public Date getToday(String time) {
		final Calendar cal = Calendar.getInstance();
		DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd " + time);
		DateFormat fmt = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		Date date = null;
		try {
			date = fmt.parse(dateFormat.format(cal.getTime()));
		} catch (ParseException e) {
			e.printStackTrace();
		}
		return date;
	}

	private Date addOneDay(Date date, int day) {
		Calendar cal = Calendar.getInstance();
		cal.setTime(date);
		cal.add(Calendar.DATE, day);
		return cal.getTime();
	}

```

#### Yesterday

```

package cn.netkiller.date;

import java.text.DateFormat;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;

public class Yesterday {

	public Yesterday() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Yesterday yesterday = new Yesterday();
		System.out.println(yesterday.yesterday());
		System.out.println(yesterday.getYesterday("00:00:00"));
		System.out.println(yesterday.getYesterday("23:59:59"));
	}
	private Date yesterday() {
	    final Calendar cal = Calendar.getInstance();
	    cal.add(Calendar.DATE, -1);
	    return cal.getTime();
	}

	private Date getYesterday(String time) {
	        DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd "+time);

	        DateFormat fmt =new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
	        Date date = null;
	        try {
	            date = fmt.parse(dateFormat.format(yesterday()));
	        } catch (ParseException e) {
	            e.printStackTrace();
	        }
	        return date;
	}
}

```

#### ISO 8601

```

ISO 8601 扩展格式 YYYY-MM-DDTHH:mm:ss.sssZ

```

#### LocalDateTime

```

	LocalDateTime localDateTime = LocalDateTime.of(2016, 1, 1, 13, 55);
	ZonedDateTime zonedDateTime = localDateTime.atZone(ZoneId.of("Asia/Shanghai"));
	Date date = Date.from(zonedDateTime.toInstant());	

	Instant instant = Instant.ofEpochMilli(date.getTime());
    LocalDateTime ldt = LocalDateTime.ofInstant(instant, ZoneOffset.UTC);
    Instant instant = ldt.toInstant(ZoneOffset.UTC);
    Date date = Date.from(instant);		

```

#### ZonedDateTime

```

Date.from(java.time.ZonedDateTime.now().toInstant());			

```

### Array

一定字符串数组

```

String[] str={"AAA","BBB","CCC"};
String str[]={"AAA","BBB","CCC"};		

```

String to Array

```

package cn.netkiller.java;

public class StringToArray {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String str="a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z";
		String[] array = null;   
		array = str.split(",");
		for(int i=0; i<array.length; i++){
			System.out.println(array[i]);
		}
	}
}		

```

#### for each

```

	public static void main(String[] args) {
		try {

			for (String arg : args) {
				System.out.println(arg);
			}
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}			

```

#### Array to String

```

package cn.netkiller.java;

import java.util.Arrays;

public class ArrayToString {

	public static void main(String[] args) {
		String[] array = {"a", "b", "c", "d", "e", "f", "g", "h", "i", "j", "k", "l", "m", "n", "o", "p", "q", "r", "s", "t", "u", "v", "w", "x", "y", "z"};
		System.out.println(Arrays.toString(array));
		System.out.println(Arrays.toString(array).replaceAll(", |\\[|\\]", ""));
	}

}

```

```

String[][] deepArray = new String[][] {{"John", "Mary"}, {"Alice", "Bob"}};
System.out.println(Arrays.toString(deepArray));
//output: [[Ljava.lang.String;@106d69c, [Ljava.lang.String;@52e922]
System.out.println(Arrays.deepToString(deepArray));		

Output:
[[John, Mary], [Alice, Bob]]	

```

```

String[] array = {"neo", "chen"};
String string = String.join(",", array)
// 输出结果 "neo,chen"		

```

### float

float 不能直接做减法运算

```
			float a = 77.22f;
			float b = 77.2f;

			System.out.println((float)a-b);
			System.out.println((float)a+b);

			输出结果为：
			0.020004272
			154.42

```

```

package cn.netkiller.example;

import java.math.BigDecimal;

public class Math {

	public Math() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {

		float a = 77.22f;
		float b = 77.2f;

		System.out.println((float) a + b);
		System.out.println((float) a - b);
		System.out.println((float) a * b);
		System.out.println((float) a / b);

		System.out.println("-------------");

		System.out.println(add(a, b));
		System.out.println(sub(a, b));
		System.out.println(mul(a, b));
		System.out.println(div(a, b));

	}

	public static float add(float v1, float v2) {
		BigDecimal b1 = new BigDecimal(Float.toString(v1));
		BigDecimal b2 = new BigDecimal(Float.toString(v2));
		return b1.add(b2).floatValue();
	}

	public static float sub(float v1, float v2) {
		BigDecimal b1 = new BigDecimal(Float.toString(v1));
		BigDecimal b2 = new BigDecimal(Float.toString(v2));
		return b1.subtract(b2).floatValue();
	}

	public static float mul(float v1, float v2) {
		BigDecimal b1 = new BigDecimal(Float.toString(v1));
		BigDecimal b2 = new BigDecimal(Float.toString(v2));
		return b1.multiply(b2).floatValue();
	}

	public static float div(float v1, float v2) {
		return div(v1, v2, 5);
	}

	public static float div(float v1, float v2, int scale) {
		if (scale < 0) {
			throw new IllegalArgumentException("The scale must be a positive integer or zero");
		}
		BigDecimal b1 = new BigDecimal(Float.toString(v1));
		BigDecimal b2 = new BigDecimal(Float.toString(v2));
		return b1.divide(b2, scale, BigDecimal.ROUND_HALF_UP).floatValue();
	}

	public static float round(float v, int scale) {
		if (scale < 0) {
			throw new IllegalArgumentException("The scale must be a positive integer or zero");
		}
		BigDecimal b = new BigDecimal(Float.toString(v));
		BigDecimal one = new BigDecimal("1");
		return b.divide(one, scale, BigDecimal.ROUND_HALF_UP).floatValue();
	}
}

```

### double

```

package cn.netkiller.example;

import java.math.BigDecimal;

public class Math {

	public Math() {
		// TODO Auto-generated constructor stub
	}

	public static double add(double v1, double v2) {
		BigDecimal b1 = new BigDecimal(Double.toString(v1));
		BigDecimal b2 = new BigDecimal(Double.toString(v2));
		return b1.add(b2).doubleValue();
	}

	public static double sub(double v1, double v2) {
		BigDecimal b1 = new BigDecimal(Double.toString(v1));
		BigDecimal b2 = new BigDecimal(Double.toString(v2));
		return b1.subtract(b2).doubleValue();
	}

	public static double mul(double v1, double v2) {
		BigDecimal b1 = new BigDecimal(Double.toString(v1));
		BigDecimal b2 = new BigDecimal(Double.toString(v2));
		return b1.multiply(b2).doubleValue();
	}

	public static double div(double v1, double v2) {
		return div(v1, v2, 8);
	}

	public static double div(double v1, double v2, int scale) {
		if (scale < 0) {
			throw new IllegalArgumentException("The scale must be a positive integer or zero");
		}
		BigDecimal b1 = new BigDecimal(Double.toString(v1));
		BigDecimal b2 = new BigDecimal(Double.toString(v2));
		return b1.divide(b2, scale, BigDecimal.ROUND_HALF_UP).doubleValue();
	}

	public static double round(double v, int scale) {
		if (scale < 0) {
			throw new IllegalArgumentException("The scale must be a positive integer or zero");
		}
		BigDecimal b = new BigDecimal(Double.toString(v));
		BigDecimal one = new BigDecimal("1");
		return b.divide(one, scale, BigDecimal.ROUND_HALF_UP).doubleValue();
	}
}		

```

#### String to double

```

double amount = Double.parseDouble(value);			

```

### BigDecimal

```

package cn.netkiller.example;

import java.math.BigDecimal;

public class BigDecimalTest {

	public BigDecimalTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		BigDecimal first = new BigDecimal("1.0");
		BigDecimal second = new BigDecimal("1.77");
		System.out.println(String.format("%s, %s", first, second));

		if (first.equals(second))

			System.out.println("equals: true");

		else

			System.out.println("equals: false");

		if (first.compareTo(second) == 0)

			System.out.println("compareTo: true");
		else
			System.out.println("compareTo: false");

		BigDecimal zero = new BigDecimal("0");
		BigDecimal one = new BigDecimal("1");
		BigDecimal minus = new BigDecimal("-1");

		if (zero.compareTo(one) < 0)

			System.out.println("比較演算子[ <  ]: true");

		if (one.compareTo(one) == 0)

			System.out.println("比較演算子[ == ]: true");

		if (zero.compareTo(minus) > 0)

			System.out.println("比較演算子[ >  ]: true");

		if (zero.compareTo(minus) >= 0)

			System.out.println("比較演算子[ >= ]: true");

		if (zero.compareTo(minus) != 0)

			System.out.println("比較演算子[ != ]: true");

		if (zero.compareTo(one) <= 0)
			System.out.println("比較演算子[ <= ]: true");

	}

}

```

#### Convert BigDecimal Object to double value

```

BigDecimal.doubleValue()

```

#### 去除末尾多余的 0

```

System.out.println( new BigDecimal("100.000").stripTrailingZeros().toString());		

```

#### 禁用科学计数法

```

有时会输出 1E+2，如果你不希望这种科学计数法输出可以使用 toPlainString() 替代 toString()
System.out.println( new BigDecimal("100.000").stripTrailingZeros().toPlainString());			

```

#### 移动小数点位置

```

package cn.netkiller.example.test;

import java.math.BigDecimal;
import java.math.BigInteger;

public class Test {

	public Test() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		int decimal = 4;

		BigInteger amount = BigInteger.valueOf(10000000000L);
		BigDecimal balance = new BigDecimal(amount);
		BigDecimal point = new BigDecimal(0.1 / Math.pow(10, decimal));
		balance = balance.multiply(point);
		System.out.println(balance);
	}

}			

```

发现输出有问题 100000.000000000008180305391403130954586231382563710212707519531250000000000

换种方法

```

package cn.netkiller.example.test;

import java.math.BigDecimal;
import java.math.BigInteger;

public class Test {

	public Test() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		// String i = Integer.valueOf("0x57c457",16).toString();
		// System.out.println(i);

		int decimal = 6;

		BigInteger amount = BigInteger.valueOf(10000000000L);

		System.out.println(amount);

		String tmp = amount.toString();

		String number = new StringBuffer(tmp).insert(tmp.length() - decimal, ".").toString();
		BigDecimal balance = new BigDecimal(number);

		System.out.println(balance);
	}

}			

```

最佳方案

```

		int decimal = 6;

		System.out.println(BigDecimal.TEN.pow(decimal));
		BigDecimal balance1 = new BigDecimal("1234");
		BigDecimal value = balance1.divide(BigDecimal.TEN.pow(decimal));
		System.out.println(value);

		BigDecimal balance2 = new BigDecimal("12.107");
		BigDecimal value2 = balance2.multiply(BigDecimal.TEN.pow(decimal)).setScale(0, RoundingMode.DOWN);
		System.out.println(value2);			

```

### StringBuffer

```

String str = Integer.toString(j);
str = new StringBuilder(str).insert(str.length()-2, ".").toString();

Or if you need synchronization use the StringBuffer with similar usage :

String str = Integer.toString(j);
str = new StringBuffer(str).insert(str.length()-2, ".").toString();		

```

### enum

```

class EnumExample1{

	public enum Season { WINTER, SPRING, SUMMER, FALL }

	public static void main(String[] args) {
		for (Season s : Season.values())
			System.out.println(s);
	}
}		

```

```

package cn.netkiller.api.util;

public enum HttpMethod {
    GET("GET"), POST("POST"), PUT("PUT"), PATCH("PATCH"), DELETE("DELETE");

    private String value;

    private HttpMethod(String value) {
        this.value = value;
    }

    public String toString() {
        return value;
    }
}		

```

### byte 类型

#### string2byte

```

	byte[] bytes = "Helloworld!!! - http://www.netkiller.cn".getBytes();		

```

#### byte[] to String

```

	byte[] bytes = "Helloworld!!! - http://www.netkiller.cn".getBytes();
	String str = new String(bytes, StandardCharsets.UTF_8);
	System.out.println(str);

```

#### BigInteger2byte

```

BigInteger b= new BigInteger('300');
byte bytes= b.byteValue();			

```

#### int to byte array

```

int a= 120;
byte b= (byte)a;			

```

```

private byte[] bigIntToByteArray( final int i ) {
    BigInteger bigInt = BigInteger.valueOf(i);      
    return bigInt.toByteArray();
}			

```

```

private byte[] intToByteArray ( final int i ) throws IOException {      
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    DataOutputStream dos = new DataOutputStream(bos);
    dos.writeInt(i);
    dos.flush();
    return bos.toByteArray();
}			

```

```

private byte[] intToBytes( final int i ) {
    ByteBuffer bb = ByteBuffer.allocate(4); 
    bb.putInt(i); 
    return bb.array();
}						

```

位移操作

```

private static byte[] intToBytes(final int a) {
    return new byte[] {
        (byte)((data >> 24) & 0xff),
        (byte)((data >> 16) & 0xff),
        (byte)((data >> 8) & 0xff),
        (byte)((data >> 0) & 0xff),
    };
}			

```

#### byte array to int

```

private int convertByteArrayToInt(byte[] intBytes){
    ByteBuffer byteBuffer = ByteBuffer.wrap(intBytes);
    return byteBuffer.getInt();
}			

```

```

private int convertByteArrayToInt(byte[] data) {
    if (data == null || data.length != 4) return 0x0;
    // ----------
    return (int)( // NOTE: type cast not necessary for int
            (0xff & data[0]) << 24  |
            (0xff & data[1]) << 16  |
            (0xff & data[2]) << 8   |
            (0xff & data[3]) << 0
            );
}			

```

```

	public static byte[] intToByte32(int num) {
        byte[] arr = new byte[32];
        for (int i = 31; i >= 0; i--) {
            // &1 也可以改为 num&0x01,表示取最地位数字.
            arr[i] = (byte) (num & 1);
            // 右移一位.
            num >>= 1;
        }
        return arr;
    }

	public static int byte32ToInt(byte[] arr) {
        if (arr == null || arr.length != 32) {
            throw new IllegalArgumentException("byte 数组必须不为空,并且长度是 32!");
        }
        int sum = 0;
        for (int i = 0; i < 32; ++i) {
            sum |= (arr[i] << (31 - i));
        }
        return sum;
    }			

```

int array to byte array

```

private byte[] convertIntArrayToByteArray(int[] data) {
        if (data == null) return null;
        // ----------
        byte[] byts = new byte[data.length * 4];
        for (int i = 0; i < data.length; i++)
            System.arraycopy(convertIntToByteArray(data[i]), 0, byts, i * 4, 4);
        return byts;
    }			

```

byte array to int array

```

public int[] convertByteArrayToIntArray(byte[] data) {
        if (data == null || data.length % 4 != 0) return null;
        // ----------
        int[] ints = new int[data.length / 4];
        for (int i = 0; i < ints.length; i++)
            ints[i] = ( convertByteArrayToInt(new byte[] {
                    data[(i*4)],
                    data[(i*4)+1],
                    data[(i*4)+2],
                    data[(i*4)+3],
            } ));
        return ints;
    }			

```

#### byte2char

```

	byte b1 = 65;
    // char ch = b1;  

    char ch = (char) b1;

    System.out.println("byte value: " + b1);             // prints 65
    System.out.println("Converted char value: " + ch);   // prints A (ASCII is 65 for A)			

```

```

	public static char byte2ToChar(byte[] arr) {
        if (arr == null || arr.length != 2) {
            throw new IllegalArgumentException("byte 数组必须不为空,并且是 2 位!");
        }
        return (char) (((char) (arr[0] << 8)) | ((char) arr[1]));
    }			

    public static byte[] charToByte2(char c) {
        byte[] arr = new byte[2];
        arr[0] = (byte) (c >> 8);
        arr[1] = (byte) (c & 0xff);
        return arr;
    }

```

#### longToByte64

```

    public static byte[] longToByte64(long sum) {
        byte[] arr = new byte[64];
        for (int i = 63; i >= 0; i--) {
            arr[i] = (byte) (sum & 1);
            sum >>= 1;
        }
        return arr;
    }			

```

#### byte64ToLong

```

	public static long byte8ToLong(byte[] arr) {
        if (arr == null || arr.length != 8) {
            throw new IllegalArgumentException("byte 数组必须不为空,并且是 8 位!");
        }
        return (long) (((long) (arr[0] & 0xff) << 56) | ((long) (arr[1] & 0xff) << 48) | ((long) (arr[2] & 0xff) << 40)
                        | ((long) (arr[3] & 0xff) << 32) | ((long) (arr[4] & 0xff) << 24)
                        | ((long) (arr[5] & 0xff) << 16) | ((long) (arr[6] & 0xff) << 8) | ((long) (arr[7] & 0xff)));
    }

	public static byte[] longToByte8(long sum) {
        byte[] arr = new byte[8];
        arr[0] = (byte) (sum >> 56);
        arr[1] = (byte) (sum >> 48);
        arr[2] = (byte) (sum >> 40);
        arr[3] = (byte) (sum >> 32);
        arr[4] = (byte) (sum >> 24);
        arr[5] = (byte) (sum >> 16);
        arr[6] = (byte) (sum >> 8);
        arr[7] = (byte) (sum & 0xff);
        return arr;
    }

	public static long byte64ToLong(byte[] arr) {
        if (arr == null || arr.length != 64) {
            throw new IllegalArgumentException("byte 数组必须不为空,并且长度是 64!");
        }
        long sum = 0L;
        for (int i = 0; i < 64; ++i) {
            sum |= ((long) arr[i] << (63 - i));
        }
        return sum;
    }			

```

#### short2byte

```

    public static byte[] shortToByte16(short s) {
        byte[] arr = new byte[16];
        for (int i = 15; i >= 0; i--) {
            arr[i] = (byte) (s & 1);
            s >>= 1;
        }
        return arr;
    }

    public static short byte16ToShort(byte[] arr) {
        if (arr == null || arr.length != 16) {
            throw new IllegalArgumentException("byte 数组必须不为空,并且长度为 16!");
        }
        short sum = 0;
        for (int i = 0; i < 16; ++i) {
            sum |= (arr[i] << (15 - i));
        }
        return sum;
    }			

    public static short byte2ToShort(byte[] arr) {
        if (arr != null && arr.length != 2) {
            throw new IllegalArgumentException("byte 数组必须不为空,并且是 2 位!");
        }
        return (short) (((short) arr[0] << 8) | ((short) arr[1] & 0xff));
    }

    public static byte[] shortToByte2(Short s) {
        byte[] arr = new byte[2];
        arr[0] = (byte) (s >> 8);
        arr[1] = (byte) (s & 0xff);
        return arr;
    }

```

#### byte8ToDouble

```

	public static double byte8ToDouble(byte[] arr) {
        if (arr == null || arr.length != 8) {
            throw new IllegalArgumentException("byte 数组必须不为空,并且是 8 位!");
        }
        long l = byte8ToLong(arr);
        return Double.longBitsToDouble(l);
    }

    public static byte[] doubleToByte8(double i) {
        long j = Double.doubleToLongBits(i);
        return longToByte8(j);
    }			

```

#### byte4ToFloat

```

    public static float byte4ToFloat(byte[] arr) {
        if (arr == null || arr.length != 4) {
            throw new IllegalArgumentException("byte 数组必须不为空,并且是 4 位!");
        }
        int i = byte4ToInt(arr);
        return Float.intBitsToFloat(i);
    }

    public static byte[] floatToByte4(float f) {
        int i = Float.floatToIntBits(f);
        return intToByte4(i);
    }			

```

#### 无符号 byte

```

byte b= -120;
int a= bytes & 0xff;

```

```

byte a = (byte)234;
System.out.println(a);

上面的代码，结果是-22，因为 java 中 byte 是有符号的，byte 范围是-128~127。

如果想输出 234，该怎么做呢，首先想到的是将 a 赋给大一点的类型，如下：

byte a = (byte)234;
int i = a&0xff;
System.out.println(i);

原因是 0xff 是 int，占 4 个字节，a 是 byte，占 1 个字节，进行&操作的细节如下：

   00000000 00000000 00000000 11101010    (a)
&
   00000000 00000000 00000000 11111111    (0xff)
---------------------------------------------------------------------
=  00000000 00000000 00000000 11101010

结果是 int，但是符号位是 0，说明是正数，最后就是正整数 234.	

```

#### byte to hex

```

byte bv = 10;
String hexString = Integer.toHexString(bv);			

```

#### byte[] to hex

```

byte bytes[] = {(byte)0, (byte)0, (byte)134, (byte)0, (byte)61};
System.out.println(javax.xml.bind.DatatypeConverter.printHexBinary(bytes));			

```

```

public static String byteArrayToHex(byte[] bytes) {
   StringBuilder sb = new StringBuilder(a.length * 2);
   for(byte b: bytes)
      sb.append(String.format("%02x", b));
   return sb.toString();
}

```

```

BigInteger n = new BigInteger(byteArray);
String hexa = n.toString(16));			

```

#### 连接两个 byte[]

```

byte[] one = { 1, 2, 3 };
byte[] two = { 6, 8, 9 };
int length = one.Length + two.Length;
byte[] sum = new byte[length];
one.CopyTo(sum,0);
two.CopyTo(sum,one.Length);			

```

```

byte[] one = { 1, 2, 3 };
byte[] two = { 6, 8, 9 };
List<byte> list1 = new List<byte>(one);
List<byte> list2 = new List<byte>(two);
list1.AddRange(list2);
byte[] sum2 = list1.ToArray();			

```

#### List<Byte> to byte[]

```

		List<Byte> byteList = new ArrayList<Byte>();
        byteList.add((byte) 1);
        byteList.add((byte) 2);
        byteList.add((byte) 3);
        byte[] byteArray = Bytes.toArray(byteList);
        System.out.println(Arrays.toString(byteArray));			

```

## Collection

```

Collection
 ├List
 │├LinkedList
 │├ArrayList
 │└Vector
 │　└Stack
 └Set

```

```

Collection 接口
　　Collection 是最基本的集合接口，一个 Collection 代表一组 Object，即 Collection 的元素（Elements）。一些 Collection 允许相同的元素而另一些不行。一些能排序而另一些不行。Java SDK 不提供直接继承自 Collection 的类，Java SDK 提供的类都是继承自 Collection 的“子接口”如 List 和 Set。
　　所有实现 Collection 接口的类都必须提供两个标准的构造函数：无参数的构造函数用于创建一个空的 Collection，有一个 Collection 参数的构造函数用于创建一个新的 Collection，这个新的 Collection 与传入的 Collection 有相同的元素。后 一个构造函数允许用户复制一个 Collection。
　　如何遍历 Collection 中的每一个元素？不论 Collection 的实际类型如何，它都支持一个 iterator()的方法，该方法返回一个迭代子，使用该迭代子即可逐一访问 Collection 中每一个元素。典型的用法如下：
　　　　Iterator it = collection.iterator(); // 获得一个迭代子
　　　　while(it.hasNext()) {
　　　　　　Object obj = it.next(); // 得到下一个元素
　　　　}
　　由 Collection 接口派生的两个接口是 List 和 Set。
List 接口
　　List 是有序的 Collection，使用此接口能够精确的控制每个元素插入的位置。用户能够使用索引（元素在 List 中的位置，类似于数组下标）来访问 List 中的元素，这类似于 Java 的数组。
和下面要提到的 Set 不同，List 允许有相同的元素。
　　除了具有 Collection 接口必备的 iterator()方法外，List 还提供一个 listIterator()方法，返回一个 ListIterator 接口，和标准的 Iterator 接口相比，ListIterator 多了一些 add()之类的方法，允许添加，删除，设定元素， 还能向前或向后遍历。
　　实现 List 接口的常用类有 LinkedList，ArrayList，Vector 和 Stack。
LinkedList 类
　　LinkedList 实现了 List 接口，允许 null 元素。此外 LinkedList 提供额外的 get，remove，insert 方法在 LinkedList 的首部或尾部。这些操作使 LinkedList 可被用作堆栈（stack），队列（queue）或双向队列（deque）。
　　注意 LinkedList 没有同步方法。如果多个线程同时访问一个 List，则必须自己实现访问同步。一种解决方法是在创建 List 时构造一个同步的 List：
　　　　List list = Collections.synchronizedList(new LinkedList(...));
ArrayList 类
　　ArrayList 实现了可变大小的数组。它允许所有元素，包括 null。ArrayList 没有同步。
size，isEmpty，get，set 方法运行时间为常数。但是 add 方法开销为分摊的常数，添加 n 个元素需要 O(n)的时间。其他的方法运行时间为线性。
　　每个 ArrayList 实例都有一个容量（Capacity），即用于存储元素的数组的大小。这个容量可随着不断添加新元素而自动增加，但是增长算法 并没有定义。当需要插入大量元素时，在插入前可以调用 ensureCapacity 方法来增加 ArrayList 的容量以提高插入效率。
　　和 LinkedList 一样，ArrayList 也是非同步的（unsynchronized）。
Vector 类
　　Vector 非常类似 ArrayList，但是 Vector 是同步的。由 Vector 创建的 Iterator，虽然和 ArrayList 创建的 Iterator 是同一接口，但是，因为 Vector 是同步的，当一个 Iterator 被创建而且正在被使用，另一个线程改变了 Vector 的状态（例如，添加或删除了一些元素），这时调用 Iterator 的方法时将抛出 ConcurrentModificationException，因此必须捕获该异常。
Stack 类
　　Stack 继承自 Vector，实现一个后进先出的堆栈。Stack 提供 5 个额外的方法使得 Vector 得以被当作堆栈使用。基本的 push 和 pop 方法，还有 peek 方法得到栈顶的元素，empty 方法测试堆栈是否为空，search 方法检测一个元素在堆栈中的位置。Stack 刚创建后是空栈。
Set 接口
　　Set 是一种不包含重复的元素的 Collection，即任意的两个元素 e1 和 e2 都有 e1.equals(e2)=false，Set 最多有一个 null 元素。
　　很明显，Set 的构造函数有一个约束条件，传入的 Collection 参数不能包含重复的元素。
　　请注意：必须小心操作可变对象（Mutable Object）。如果一个 Set 中的可变元素改变了自身状态导致 Object.equals(Object)=true 将导致一些问题。
Map 接口
　　请注意，Map 没有继承 Collection 接口，Map 提供 key 到 value 的映射。一个 Map 中不能包含相同的 key，每个 key 只能映射一个 value。Map 接口提供 3 种集合的视图，Map 的内容可以被当作一组 key 集合，一组 value 集合，或者一组 key-value 映射。		

```

```

package cn.netkiller.example;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collection;
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.LinkedList;
import java.util.TreeSet;

public class Test {

	public Test() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		// A A B E F G C D
		String[] array = { "A", "A", "B", "E", "F", "G", "C", "D" };
		Collection<String> list = new ArrayList<String>(Arrays.asList(array));
		for (String str : list) {
			System.out.print(str + " ");
		}
		System.out.println();

		// A A B E F G C D
		Collection<String> linkedList = new LinkedList<String>(Arrays.asList(array));
		for (String str : linkedList) {
			System.out.print(str + " ");
		}
		System.out.println();

		// 无重复，无序 D E F G A B C
		Collection<String> hashSet = new HashSet<String>(Arrays.asList(array));
		for (String str : hashSet) {
			System.out.print(str + " ");
		}
		System.out.println();

		// 无重复 A B C D E F G
		Collection<String> treeSet = new TreeSet<String>(Arrays.asList(array));
		for (String str : treeSet) {
			System.out.print(str + " ");
		}
		System.out.println();

		// 无重复 A B E F G C D
		Collection<String> linkedHashSet = new LinkedHashSet<String>(Arrays.asList(array));
		for (String str : linkedHashSet) {
			System.out.print(str + " ");

		}

	}

}

```

输出结果

```
		A A B E F G C D
		A A B E F G C D
		A B C D E F G
		A B C D E F G
		A B E F G C D

```

### 静态 List

```

	public static List<String> list = new ArrayList<String>();
	static {
		list.add("录入");
		list.add("变更");
		list.add("收藏");
		list.add("在售");
		list.add("展出");
	}			

```

### ArrayList

判断元素是否存在

```

import java.util.ArrayList;

public class arraylist {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ArrayList<String> whitelist = new ArrayList<String>();
		whitelist.add("Neo");
		whitelist.add("Jam");
		whitelist.add("Sam");

		if (whitelist.contains("Neo")) {
			System.out.println("Found!");
		}else{
			System.out.println("Not Found!");
		}
	}

}

```

```

package cn.netkiller.type;

import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class ArrayListExample {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		List<String> list = new ArrayList<String>();
		list.add("Jack");
		list.add("Jet");
		list.add("Jack");
		list.add("Mike");
		list.add("Kitty");
		list.add("Tom");

		//while 循环
		Iterator<String> it = list.iterator();
		while (it.hasNext()) {
			System.out.println(it.next());
		}

		for (Iterator<String> it1 = list.iterator(); it1.hasNext();) {
			System.out.println(it1.next());
		}

		// for 循环
		for (int i = 0; i < list.size(); i++) {
			System.out.println(list.get(i));
		}

		// for 循环加强版
		for (String i : list) {
			System.out.println(i);
		}

	}

}

```

ArrayList 转为 Array

```

		String[] array = {"/bin/sh","-c"};
		List<String> list = new ArrayList<String>(Arrays.asList(array));
	    list.add("command");
	    list.add("param");

	    String[] command = (String[]) list.toArray(new String[0]);
	    System.out.println(Arrays.toString(command));

```

#### ArrayList to String

```

		List<String> list = new ArrayList<String>();
	    list.add("command");
	    list.add("param");

	    String listString = String.join(", ", list);

	    System.out.println(listString);

```

#### Array to List

```
				Arrays.asList(array)

```

#### List to Array

```

		List<String> list = new ArrayList<String>();
		list.add("str1");
		list.add("str2");

		String[] array = (String[]) list.toArray();
		System.out.println(array);				

```

### Set 转为 List

```

		// 将 Map Key 转化为 List      
        List<String> mapKeyList = new ArrayList<String>(map.keySet());    
        System.out.println("mapKeyList:"+mapKeyList);  

        // 将 Map Key 转化为 List      
        List<String> mapValuesList = new ArrayList<String>(map.values());    
        System.out.println("mapValuesList:"+mapValuesList);  			

```

```

Set<Type> set = new Set<>();
Set<Type> set = new HashSet<>();		

```

### List.of()

```

List<String> strings = List.of("first", "second");		

```

### List.copyOf()

```

var list = List.of("Java", "Python", "C");
var copy = List.copyOf(list);
System.out.println(list == copy); // true

```

```

var list = new ArrayList<String>();
var copy = List.copyOf(list);
System.out.println(list == copy); // false

```

### ArrayList forEach

```

List<String> arrayList = new ArrayList<>();
arrayList.add("A");
arrayList.add("B");
arrayList.add("C");
arrayList.add("D");
arrayList.add("E");

for (String item:arrayList){
    System.out.println(item);
}

arrayList.forEach(item->System.out.println(item));

arrayList.forEach(System.out::println);

arrayList.forEach(item->{
    if("C".equals(item)){
        System.out.println(item);
    }
});		

```

### ArrayList stream()

```

arrayList.stream()
        .filter(s-> s.contains("B")||s.contains("C"))
        .forEach(System.out::println);

arrayList.stream()
        .filter(s->s.contains("E"))
        .findFirst().ifPresent(s -> System.out.println(s));		

```

### Set.of()

```

Set<Integer> ints = Set.of(1, 2, 3);		

```

### Collection to Array

Collection.toArray(IntFunction)

```

	@Test
    public void testCollectionToArray(){
        Set<String> names = Set.of("Fred", "Wilma", "Barney", "Betty");
        String[] copy = new String[names.size()];
        names.toArray(copy);
        System.out.println(Arrays.toString(copy));
        System.out.println(Arrays.toString(names.toArray(String[]::new)));
    }

```

### ArrarList 转换为 string[]

```

　　ArrayList list = new ArrayList();

　　list.Add("aaa");

　　list.Add("bbb");

　　string[] arrString = (string[])list.ToArray(typeof( string)) ;		

```

### string[] 转换为 ArrarList

```

　　ArrayList list = new ArrayList(new string[] { "aaa", "bbb" });		

```

### ArrayList 转换为 string

```

　　ArrayList list = new ArrayList();

　　list.Add("aaa");

　　list.Add("bbb");

　　string str= string.Join(",", (string[])list.ToArray(typeof( string)));		

```

### string 转换为 ArrayList

```

　　string str="1,2,3,4,5";
　　ArrayList b = new ArrayList( str.Split(',') ) ;		

```

### String[] to List

```

String[] arr = new String[] {"1", "2"};
List list = Arrays.asList(arr);		

```

## Map

```

Map
 ├Hashtable
 ├HashMap
 └WeakHashMap

 Map 接口
　　请注意，Map 没有继承 Collection 接口，Map 提供 key 到 value 的映射。一个 Map 中不能包含相同的 key，每个 key 只能映射一个 value。Map 接口提供 3 种集合的视图，Map 的内容可以被当作一组 key 集合，一组 value 集合，或者一组 key-value 映射。
Hashtable 类
　　Hashtable 继承 Map 接口，实现一个 key-value 映射的哈希表。任何非空（non-null）的对象都可作为 key 或者 value。
　　添加数据使用 put(key, value)，取出数据使用 get(key)，这两个基本操作的时间开销为常数。
Hashtable 通过 initial capacity 和 load factor 两个参数调整性能。通常缺省的 load factor 0.75 较好地实现了时间和空间的均衡。增大 load factor 可以节省空间但相应的查找时间将增大，这会影响像 get 和 put 这样的操作。
使用 Hashtable 的简单示例如下，将 1，2，3 放到 Hashtable 中，他们的 key 分别是”one”，”two”，”three”：
　　　　Hashtable numbers = new Hashtable();
　　　　numbers.put(“one”, new Integer(1));
　　　　numbers.put(“two”, new Integer(2));
　　　　numbers.put(“three”, new Integer(3));
　　要取出一个数，比如 2，用相应的 key：
　　　　Integer n = (Integer)numbers.get(“two”);
　　　　System.out.println(“two = ” + n);
　　由于作为 key 的对象将通过计算其散列函数来确定与之对应的 value 的位置，因此任何作为 key 的对象都必须实现 hashCode 和 equals 方 法。hashCode 和 equals 方法继承自根类 Object，如果你用自定义的类当作 key 的话，要相当小心，按照散列函数的定义，如果两个对象相 同，即 obj1.equals(obj2)=true，则它们的 hashCode 必须相同，但如果两个对象不同，则它们的 hashCode 不一定不同，如 果两个不同对象的 hashCode 相同，这种现象称为冲突，冲突会导致操作哈希表的时间开销增大，所以尽量定义好的 hashCode()方法，能加快哈希 表的操作。
　　如果相同的对象有不同的 hashCode，对哈希表的操作会出现意想不到的结果（期待的 get 方法返回 null），要避免这种问题，只需要牢记一条：要同时复写 equals 方法和 hashCode 方法，而不要只写其中一个。
　　Hashtable 是同步的。
HashMap 类
　　HashMap 和 Hashtable 类似，不同之处在于 HashMap 是非同步的，并且允许 null，即 null value 和 null key。，但是将 HashMap 视为 Collection 时（values()方法可返回 Collection），其迭代子操作时间开销和 HashMap 的容量成比例。因此，如果迭代操作的性能相当重要的话，不要将 HashMap 的初始化容量设得过高，或者 load factor 过低。
WeakHashMap 类
　　WeakHashMap 是一种改进的 HashMap，它对 key 实行“弱引用”，如果一个 key 不再被外部所引用，那么该 key 可以被 GC 回收。

```

### 初始化

```

	Map<String, Object> data = new HashMap<String, Object>() {
		{
			put("name", "neo");
		}
	};		

```

### static map

```

	private static final Map<String, String> point;
	static {
		point = new HashMap<String, String>();
		point.put("CN", "China");
		point.put("HK", "Hongkong");
		point.put("TW", "Taiwan");
	};

```

```

	public final static Map<String, String> hostMap = new HashMap<String, String>() {
        {
            put("redis", "127.0.0.1");
            put("solr", "127.0.0.1");
        }
	};

	public final static Map map = new HashMap() {{      
	    put("key1", "value1");      
	    put("key2", "value2");      
	}};  		

```

### HashMap

#### 遍历 HashMap

```

Map<String, Integer> session = new HashMap<String, Integer>();

session.put("A",1);
...
...
session.put("Z",26)

for (Map.Entry<String, Integer> entry : session.entrySet()) {
	System.out.println(String.format("%s:%d", entry.getKey(), entry.getValue()));
}

Map<Integer, Integer> map = new HashMap<Integer, Integer>();  

for (Map.Entry<Integer, Integer> entry : map.entrySet()) {  

    System.out.println("Key = " + entry.getKey() + ", Value = " + entry.getValue());  

} 

```

#### 遍历 map 中的键

```

Map<Integer, Integer> map = new HashMap<Integer, Integer>();  

//遍历 map 中的键  

for (Integer key : map.keySet()) {  

    System.out.println("Key = " + key);  

}  			

```

#### 遍历 map 中的值 

```

Map<Integer, Integer> map = new HashMap<Integer, Integer>();    
for (Integer value : map.values()) {  
    System.out.println("Value = " + value);  
}  			

```

#### 通过键取值

```

Map<Integer, Integer> map = new HashMap<Integer, Integer>();  

for (Integer key : map.keySet()) {  

    Integer value = map.get(key);  

    System.out.println("Key = " + key + ", Value = " + value);  

}  			

```

#### 使用 Iterator 遍历 HashMap

```

Map<Integer, Integer> map = new HashMap<Integer, Integer>();

Iterator<Map.Entry<Integer, Integer>> entries = map.entrySet().iterator();

while (entries.hasNext()) {

    Map.Entry<Integer, Integer> entry = entries.next();

    System.out.println("Key = " + entry.getKey() + ", Value = " + entry.getValue());

}  

Map map = new HashMap();  

Iterator entries = map.entrySet().iterator();

while (entries.hasNext()) {  

    Map.Entry entry = (Map.Entry) entries.next();  

    Integer key = (Integer)entry.getKey();  

    Integer value = (Integer)entry.getValue();  

    System.out.println("Key = " + key + ", Value = " + value);  
}  

```

### LinkedHashMap

```

import java.util.HashMap;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.Map;
public class TestLinkedHashMap {

  public static void main(String args[])
  {
   System.out.println("*** LinkedHashMap ***");
   Map<Integer,String> map = new LinkedHashMap<Integer,String>();
   map.put(6, "apple");
   map.put(3, "banana");
   map.put(2,"pear");

   for (Iterator it =  map.keySet().iterator();it.hasNext();)
   {
    Object key = it.next();
    System.out.println( key+"="+ map.get(key));
   }

   System.out.println("*** HashMap ***");
   Map<Integer,String> map1 = new  HashMap<Integer,String>();
   map1.put(6, "apple");
   map1.put(3, "banana");
   map1.put(2,"pear");

   for (Iterator it =  map1.keySet().iterator();it.hasNext();)
   {
    Object key = it.next();
    System.out.println( key+"="+ map1.get(key));
   }
  }
}

```

### Map forEach

```

Map<String, Integer> items = new HashMap<>();
items.put("A", 10);
items.put("B", 20);
items.put("C", 30);
items.put("D", 40);
items.put("E", 50);
items.put("F", 60);

items.forEach((k,v)->System.out.println("key : " + k + "; value : " + v));

//output
key : A value : 10
key : B value : 20
key : C value : 30
key : D value : 40
key : E value : 50
key : F value : 60

items.forEach((k,v)->{
    System.out.println("key : " + k + " value : " + v);
});

```

## Queue

```

package cn.netkiller.test;

import java.util.LinkedList;
import java.util.Queue;

public class QueueTest {
    public static void main(String[] args) {

        Queue<String> queue = new LinkedList<String>();
        //添加元素
        queue.offer("a");
        queue.offer("b");
        queue.offer("c");
        queue.offer("d");
        queue.offer("e");
        for(String q : queue){
            System.out.println(q);
        }
        System.out.println("===");
        System.out.println("poll="+queue.poll()); //返回第一个元素，并在队列中删除
        for(String q : queue){
            System.out.println(q);
        }
        System.out.println("===");
        System.out.println("element="+queue.element()); //返回第一个元素 
        for(String q : queue){
            System.out.println(q);
        }
        System.out.println("===");
        System.out.println("peek="+queue.peek()); //返回第一个元素 
        for(String q : queue){
            System.out.println(q);
        }
        //add()和 remove()方法在失败的时候会抛出异常(不推荐)
    }
}

```

## Stream

### Stream.of

```

Stream<String> stream = Stream.of("Hollis", "HollisChuang", "hollis", "Hello", "HelloWorld", "Hollis");		

```

### Stream.ofNullable

增加单个参数构造方法，可为 null

```

Stream.ofNullable(null).count(); // 0		

```

### filter

filter 方法用于通过设置的条件过滤出元素。以下代码片段使用 filter 方法过滤掉空字符串：

```

List<String> strings = Arrays.asList("Hollis", "", "HollisChuang", "H", "hollis");
strings.stream().filter(string -> !string.isEmpty()).forEach(System.out::println);		

```

### map

map 方法用于映射每个元素到对应的结果，以下代码片段使用 map 输出了元素对应的平方数：

```

List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
numbers.stream().map( i -> i*i).forEach(System.out::println);
//9,4,4,9,49,9,25

```

### limit/skip

limit 返回 Stream 的前面 n 个元素；skip 则是扔掉前 n 个元素。以下代码片段使用 limit 方法保理 4 个元素：

```

List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
numbers.stream().limit(4).forEach(System.out::println);
//3,2,2,3		

```

### sorted

sorted 方法用于对流进行排序。以下代码片段使用 sorted 方法进行排序：

```

List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
numbers.stream().sorted().forEach(System.out::println);
//2,2,3,3,3,5,7		

```

### distinct

distinct 主要用来去重，以下代码片段使用 distinct 对元素进行去重：

```

List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
numbers.stream().distinct().forEach(System.out::println);
//3,2,7,5		

```

### forEach

Stream 提供了方法 'forEach' 来迭代流中的每个数据。以下代码片段使用 forEach 输出了 10 个随机数：

```

Random random = new Random();
random.ints().limit(10).forEach(System.out::println);

```

### count

count 用来统计流中的元素个数。

```

List<String> strings = Arrays.asList("Hollis", "HollisChuang", "hollis","Hollis666", "Hello", "HelloWorld", "Hollis");
System.out.println(strings.stream().count());
//7

```

### collect

collect 就是一个归约操作，可以接受各种做法作为参数，将流中的元素累积成一个汇总结果：

```

List<String> strings = Arrays.asList("Hollis", "HollisChuang", "hollis","Hollis666", "Hello", "HelloWorld", "Hollis");
strings  = strings.stream().filter(string -> string.startsWith("Hollis")).collect(Collectors.toList());
System.out.println(strings);
//Hollis, HollisChuang, Hollis666, Hollis	

```

### takeWhile 和 dropWhile

增加 takeWhile 和 dropWhile 方法

```

Stream.of(1, 2, 3, 2, 1)
.takeWhile(n -> n < 3)
.collect(Collectors.toList()); // [1, 2]

```

从开始计算，当 n < 3 时就截止

```

Stream.of(1, 2, 3, 2, 1)
.dropWhile(n -> n < 3)
.collect(Collectors.toList()); // [3, 2, 1]

```

### List to Stream

```

List<String> strings = Arrays.asList("Hollis", "HollisChuang", "hollis", "Hello", "HelloWorld", "Hollis");
Stream<String> stream = strings.stream();		

```

### 混合使用的例子

```

List<String> strings = Arrays.asList("Hollis", "HollisChuang", "hollis", "Hello", "HelloWorld", "Hollis");
Stream s = strings.stream().filter(string -> string.length()<= 6).map(String::length).sorted().limit(3)
            .distinct();				

```

## Optional

```

Optional.of("javastack").orElseThrow(); // javastack
 // 1

```

### of() 为非 null 的值创建一个 Optional。

of 方法通过工厂方法创建 Optional 类。需要注意的是，创建对象时传入的参数不能为 null。如果传入参数为 null，则抛出 NullPointerException 。

```

		Optional<String> name = Optional.of("netkiller");
		if (name.isPresent()) {
			// 在 Optional 实例内调用 get()返回已存在的值
			System.out.println(name.get());// 输出 netkiller
		}		

```

传入参数为 null，抛出 NullPointerException.

```

Optional<String> someNull = Optional.of(null);		

```

### ofNullable() 为指定的值创建一个 Optional，如果指定的值为 null，则返回一个空的 Optional。

```

		Optional<String> name = Optional.ofNullable("netkiller");
		if (name.isPresent()) {
			// 在 Optional 实例内调用 get()返回已存在的值
			System.out.println(name.get());// 输出 netkiller
		}

		Optional<String> empty = Optional.ofNullable(null);
		if (empty.isPresent()) {
			System.out.println(empty.get());
		}

```

### isPresent 如果值存在返回 true，否则返回 false。

```

	//isPresent 方法用来检查 Optional 实例中是否包含值
	if (name.isPresent()) {
		System.out.println(name.get());
	}

```

### ifPresent() 如果 Optional 实例有值执行 lambda 表达式

如果 Optional 实例有值，调用 ifPresent()可以接受接口段或 lambda 表达式。类似下面的代码：

```

	Optional<String> name = Optional.ofNullable("netkiller");

	name.ifPresent((value) -> {
		System.out.println("hello " + value);
	});

	name.ifPresent((value) -> {
		System.out.println(value.length());
	});		

```

### get() 返回值

如果 Optional 有值则将其返回，否则抛出 NoSuchElementException。

```

		Optional<String> name = Optional.ofNullable("netkiller");
		System.out.println(name.get());

		Optional<String> empty = Optional.ofNullable(null);
		try {
			System.out.println(empty.get());
		} catch (NoSuchElementException e) {
			System.out.println(e.getMessage());
		}		

```

输出内容

```

netkiller
No value present		

```

### orElse 如果有值则将其返回，否则返回指定的其它值。

如果 Optional 实例有值则将其返回，否则返回 orElse 方法传入的参数。示例如下：

```

package cn.netkiller.test;

import java.util.Optional;

public class OptionalTest {

	public OptionalTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {

		Optional<String> name = Optional.ofNullable("netkiller");

		Optional<String> empty = Optional.ofNullable(null);

		System.out.println(name.orElse("There is some value!"));
		System.out.println(empty.orElse("There is no value present!"));

	}

}

```

输出

```

netkiller
There is no value present!		

```

指定默认值

```

	User user = new User();
	user.setId(1);
	user.setUsername("Neo");

	Optional<User> user = Optional.ofNullable(user).orElse(new User(0, "Unknown"));

	System.out.println("Username is: " + user.getUsername());

```

### orElseGet 与 orElse 方法类似，区别在于得到的默认值从 Supplier 返回。

orElseGet 方法可以接受 Supplier 接口的实现用来生成默认值。示例如下：

```

package cn.netkiller.test;

import java.util.Optional;

public class OptionalTest {

	public OptionalTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {

		Optional<String> name = Optional.ofNullable("netkiller");

		Optional<String> empty = Optional.ofNullable(null);

		System.out.println(name.orElseGet(() -> "There is some value!"));
		System.out.println(empty.orElseGet(() -> "There is no value present!"));

	}

}		

```

```

Optional<User> user = Optional.ofNullable(user).orElseGet(() -> new User(0, "Unknown"));		

```

### orElseThrow 如果有值则将其返回，否则抛出 supplier 接口创建的异常

```

Optional<User> user = Optional
        .ofNullable(user)
        .orElseThrow(() -> new EntityNotFoundException("id=" + id + " 的用户没有找到"));

```

使用场景举例

```

@RequestMapping("/user/{id}")
public User getUser(@PathVariable Integer id) {
    Optional<User> user = userService.getUserById(id);
    return user.orElseThrow(() -> new EntityNotFoundException("id=" + id + " 的用户不存在"));
}

@ExceptionHandler(EntityNotFoundException.class)
public ResponseEntity<String> handleException(EntityNotFoundException ex) {
    return new ResponseEntity<>(ex.getMessage(), HttpStatus.NOT_FOUND);
}		

```

```

package cn.netkiller.test;

import java.util.Optional;

public class OptionalTest {

	public OptionalTest() {
		// TODO Auto-generated constructor stub
	}

	public static class ValueAbsentException extends Throwable {

		private static final long serialVersionUID = -1758502952187236809L;

		public ValueAbsentException() {
			super();
		}

		public ValueAbsentException(String msg) {
			super(msg);
		}

		@Override
		public String getMessage() {
			return "No value present in the Optional instance";
		}
	}

	public static void main(String[] args) {

		Optional<String> empty = Optional.ofNullable(null);

		try {
			// orElseThrow 会抛出 lambda 表达式或方法生成的异常
			empty.orElseThrow(ValueAbsentException::new);
		} catch (Throwable ex) {
			// 输出 No value present in the Optional instance
			System.out.println(ex.getMessage());
		}

	}

}

```

### map() 方法用来对 Optional 实例的值执行一系列操作

map 方法用来对 Optional 实例的值执行一系列操作。通过一组实现了 Function 接口的 lambda 表达式传入操作。map 方法示例如下：

```

	Optional<String> name = Optional.ofNullable("netkiller");
	Optional<String> upperName = name.map((value) -> value.toUpperCase());
	System.out.println(upperName.orElse("No value found"));		

```

```

		Optional<String> username = Optional.ofNullable("netKiller-Neo")
				.map((value) -> value.toLowerCase())
				.map((value) -> value.replace("n", "N"))
				.map(value -> value.replace('-', '_'));

		System.out.println("Username is: " + username.orElse("Unknown"));		

```

### flatMap()

与 map() 区别在于 flatMap 中的 mapper 返回值必须是 Optional

```

		Optional<String> username = Optional.ofNullable("netKiller-Neo").flatMap((value) -> Optional.of(value.toUpperCase()));

		System.out.println("Username is: " + username.orElse("No value found"));		

```

### filter() 通过传入限定条件过滤 Optional 值

```

package cn.netkiller.test;

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class OptionalTest {

	public OptionalTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {

		for (String item : List.of("Neo", "Jerry", "Netkiller")) {
			Optional<String> username = Optional.of(item).filter((value) -> value.length() > 6);
			System.out.println("name is: " + username.orElse("The name is less than 6 characters"));
		}
	}

}

```

使用多个 filter 组合过滤数据

```

		List.of("Neo", "Jerry", "Netkiller", "Tom", "Anni", "Lisa", "Leo").forEach(item -> {
			Optional.of(item).filter((value) -> value.length() > 2).filter((value) -> value.contains("o")).ifPresent((n) -> {
				System.out.println(n);
			});
		});		

```

### stream()

```

Optional.of("javastack").stream().count();		

```

### or()

```

	String string = (String) Optional.ofNullable(null).or(() -> Optional.of("netkiller")).get();
	System.out.println(string);		

```

### example

```

		Optional<Map<String, Object>> name = Optional.of(new HashMap<String, Object>() {
			{
				put("id", 1);
				put("name", "Neo");
				put("age", 30);
			}
		});

		System.out.println(name.toString());
		name.map((m) -> m.put("count", 1));
		System.out.println(name.get());
		name.map((m) -> m.put("nickname", "netkiller"));
		name.map((m) -> m.remove("id"));
		System.out.println(name.get());
		Optional<Map<String, Object>> tmp = name.filter((m) -> ((Integer) m.get("age")) == 30);
		System.out.println("filter: " + tmp.get());		

```

## Network

Java 网络相关操作

### URL

```

import java.net.*;
import java.io.*;

public class URLReader {
    public static void main(String[] args) throws Exception {

        URL url = new URL("http://www.netkiller.cn/");
        BufferedReader in = new BufferedReader(new InputStreamReader(url.openStream()));

        String inputLine;
        while ((inputLine = in.readLine()) != null)
            System.out.println(inputLine);
        in.close();
    }
}

```

### java.io.tmpdir

改变 java.io.tmpdir 的默认值

```
System.setProperty("java.io.tmpdir", "/vat/tmp");
System.out.println(System.getProperty("java.io.tmpdir"));

```

## JDBC

### 安装 JDBC 包

将 ojdbc6.jar 包复制到 jre/lib/ext 目录下

```
# mv ojdbc6.jar /srv/jdk1.8.0_60/jre/lib/ext/

```

### MySQL

### Oracle

#### SID

jdbc:oracle:thin:@<host>:<port>:<SID> Example:

```
jdbc:oracle:thin:@192.168.2.1:1521:oral

```

#### SERVICE_NAME

jdbc:oracle:thin:@//<host>:<port>/<service_name>

Example:

```

jdbc:oracle:thin:@//192.168.2.1:1521/orcl.example.com 			

```

#### TNS

jdbc:oracle:thin:@<TNSName> Example:

jdbc:oracle:thin:@TNS

#### Oracle RAC Cluster

Oracle 11G 不能直接链接 RAC 的 VIP

```
jdbc:oracle:thin:@(DESCRIPTION =
	(ADDRESS_LIST = 
		(ADDRESS = (PROTOCOL = TCP)(HOST = 172.16.10.10)(PORT = 1521))
		(ADDRESS = (PROTOCOL = TCP)(HOST = 172.16.10.20)(PORT = 1521))
		(LOAD_BALANCE = yes)
	)
	(CONNECT_DATA =
		(SERVER = DEDICATED)
		(SERVICE_NAME = orcl.example.com)
		(FAILOVER_MODE =(TYPE = SELECT )(METHOD = BASIC)(RETRIES = 120)(DELAY = 5)))
	)

```

```
jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=16.50.26.29)(PORT=1521))(LOAD_BALANCE=yes)(FAILOVER=ON)(CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=orcl.example.com)(FAILOVER_MODE=(TYPE=SESSION)(METHOD=BASIC))))			

```

#### Oracle JDBC Demo

```

package cn.netkiller.zabbix;

import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

/**
 * Java JDBC Oracle Demo!
 *
 */
public class Oracle {

	String url = null; // 数据库链接地址
	String username = null;// 用户名,系统默认的账户名
	String password = null;// 你安装时选设置的密码

	public void openConfig() {
		String connectionfig = "jdbc.properties";
		Properties properties = new Properties();
		try {
			properties.load(new FileInputStream(connectionfig));
			this.url = properties.getProperty("jdbc.url");
			this.username = properties.getProperty("jdbc.username");
			this.password = properties.getProperty("jdbc.password");
		} catch (FileNotFoundException e) {
			System.out.println(
					e.getMessage() + " Working Directory = " + System.getProperty("user.dir") + "/" + connectionfig);
		} catch (IOException e) {
			System.out.println(e.getMessage());
		}
		if (this.url == null || this.username == null || this.password == null) {
			System.out.println("This Propertie file is invalid");
			// throw new Exception("");
		}

	}

	public void testOracle() {
		Connection connection = null;// 创建一个数据库连接
		ResultSet result = null;// 创建一个结果集对象
		Statement statement = null;
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");// 加载 Oracle 驱动程序
			connection = DriverManager.getConnection(this.url, this.username, this.password);
			String sql = "select current_date from dual";
			statement = connection.createStatement();
			result = statement.executeQuery(sql);
			result.next();
			System.out.printf("%s %s", result.getDate(1), result.getTime(1));
		} catch (ClassNotFoundException e) {
			System.out.println(e.getMessage());
		} catch (SQLException e) {
			System.out.println(e.getMessage());
		} finally {
			try {
				if (result != null)
					result.close();
				if (connection != null)
					connection.close();
			} catch (Exception e) {
				e.printStackTrace();
			}
		}
	}

	public static void main(String[] args) {
		Oracle oracle = new Oracle();
		oracle.openConfig();
		oracle.testOracle();
	}
}			

```

### FAQ

#### java.sql.SQLRecoverableException: IO Error: The Network Adapter could not establish the connection

直接连接 RAC VIP 提示

```
java.sql.SQLRecoverableException: IO Error: The Network Adapter could not establish the connection		

```

解决方案

```
jdbc:oracle:thin:@(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=16.50.26.29)(PORT=1521))(LOAD_BALANCE=yes)(FAILOVER=ON)(CONNECT_DATA=(SERVER=DEDICATED)(SERVICE_NAME=orcl.example.com)(FAILOVER_MODE=(TYPE=SESSION)(METHOD=BASIC))))			

```

#### Exception in thread "main" java.lang.ClassNotFoundException: oracle.jdbc.driver.OracleDriver

检查 /srv/jdk1.8.0_60/jre/lib/ext/ 是否有 jdbc 包，然后运行下面程序检查是否已经载入

```
# java -verbose:class JdbcTest | grep jdbc

[Loaded oracle.jdbc.driver.OracleDriver from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleDriver from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleDriverExtension from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleDriver$1 from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.ClassRef from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.ClassRef$XMLTypeClassRef from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.ClassRef$Locale from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.ClassRef$LocaleCategoryClassRef from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.DiagnosabilityMXBean from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleDiagnosabilityMBean from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.DatabaseError from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleSQLException from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.SQLStateMapping from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.SQLStateMapping$Tokenizer from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.Message from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.Message11 from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleConnection from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.OracleConnection from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.ClientDataSupport from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleConnectionWrapper from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleConnection from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.PhysicalConnection from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.T4CDriverExtension from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OraclePreparedStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleCallableStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.OracleStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.OraclePreparedStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.OracleCallableStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.ScrollRsetStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OraclePreparedStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleCallableStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.T4CCallableStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.T4CStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.T4CPreparedStatement from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleBufferedStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleInputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.T4CInputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.BfileDBAccess from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.BlobDBAccess from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.ClobDBAccess from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.T4CConnection from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.OracleDatumWithConnection from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleClob from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.OracleClob from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.Datum from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.DatumWithConnection from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.CLOB from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleNClob from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.OracleNClob from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.NCLOB from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleSQLPermission from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.AdditionalDatabaseMetaData from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleDatabaseMetaData from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleDatabaseMetaData from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleSavepoint from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.PhysicalConnection$2 from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.aq.AQMessage from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.NTFRegistration from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.NotificationRegistration from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.dcn.DatabaseChangeRegistration from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.NTFDCNRegistration from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleTypeMetaData from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.ObjectData from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.ORAData from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleData from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.TypeDescriptor from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleTypeMetaData$Struct from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.StructDescriptor from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.OracleTypeMetaData$Array from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.sql.ArrayDescriptor from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleClobInputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleBlobInputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleConversionInputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleConversionReader from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.NetException from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.aq.AQNotificationRegistration from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.NTFAQRegistration from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleBlobOutputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleClobOutputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleClobReader from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.OracleClobWriter from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.internal.XSEvent from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.NTFXSEvent from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.Communication from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.CRC64 from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.NTFManager from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.jdbc.driver.PhysicalConnection$1 from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.SQLnetDef from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.NSProtocol from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.NetOutputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.nl.NLException from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.NetInputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.SessionAtts from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.nl.NVFactory from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.nl.InvalidSyntaxException from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.nl.NVNavigator from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.resolver.AddrResolution from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.TNSAddress.SOException from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.ClientProfile from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.nt.ConnStrategy from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.TNSAddress.SchemaObjectFactoryInterface from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.resolver.NavSchemaObjectFactory from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.TNSAddress.SchemaObject from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.resolver.NavSchemaObject from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.TNSAddress.ServiceAlias from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.resolver.NavServiceAlias from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.nl.NVTokens from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.nl.UninitializedObjectException from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.nl.NVPair from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.TNSAddress.Description from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.resolver.NavDescription from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.TNSAddress.Address from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.resolver.NavAddress from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.jdbc.TNSAddress.DescriptionList from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.resolver.NavDescriptionList from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.nt.ConnOption from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.nt.NTAdapter from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.nt.TcpNTAdapter from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.Packet from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.DataPacket from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.DataDescriptorPacket from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.BreakNetException from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.Ano from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.AnoNetInputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.AnoNetOutputStream from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.Service from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.AnoComm from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.SupervisorService from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.AuthenticationService from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.EncryptionService from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.aso.g from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ano.DataIntegrityService from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.aso.d from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.aso.i from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.aso.b from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.ConnectPacket from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]
[Loaded oracle.net.ns.RedirectPacket from file:/srv/jdk1.8.0_60/jre/lib/ext/ojdbc6.jar]	

```

## Util

### Properties 处理 *.properties 文件

```

package netkiller.test;

import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.FileInputStream;
import java.util.Properties;

public class PropertiesTest {

	public static void main(String[] args) {

		System.out.println("Working Directory = " + System.getProperty("user.dir"));

		Properties ps = new Properties();
		try {
			ps.load(new FileInputStream("netkiller.properties"));
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		System.out.println(ps.getProperty("key"));
	}

}

```

#### 打开 properties 文件

##### 文件方式打开

```

BufferedReader br = null;
Properties properties = new Properties();
br = new BufferedReader(new InputStreamReader(new  FileInputStream(new File("data.properties")), "UTF8"));
properties.load(br);				

```

##### 输入流

```

Properties properties = new Properties();
InputStream in = getClass().getResourceAsStream("/IcisReport.properties");
properties.load(in);
Set keyValue = properties.keySet();
for (Iterator it = keyValue.iterator(); it.hasNext();)
{
	String key = (String) it.next();
}				

```

#### propertyNames()

```

package cn.netkiller.properties;

import java.util.Enumeration;
import java.util.Map.Entry;
import java.util.Properties;

public class PropertiesTest {

	public PropertiesTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Properties properties = new Properties();

		properties.put("K1", "V1");
		properties.put("K2", "V2");

		for (Entry<Object, Object> x : properties.entrySet()) {
			System.out.println(x.getKey() + " " + x.getValue());
		}

		Enumeration<?> e = properties.propertyNames();
		while (e.hasMoreElements()) {
			String key = (String) e.nextElement();
			String value = properties.getProperty(key);
			System.out.println(key + ": " + value);
		}
	}

}

```

```

import java.io.FileInputStream;
import java.util.Enumeration;
import java.util.Properties;

public class MainClass {
  public static void main(String args[]) throws Exception {
    Properties p = new Properties();
    p.load(new FileInputStream("test.properties"));
    Enumeration e = p.propertyNames();

    for (; e.hasMoreElements();) {
      System.out.println(e.nextElement());

    }
  }
}			

```

#### keySet()

```

package cn.netkiller.properties;

import java.util.Properties;
import java.util.Set;

public class PropertiesTest {

	public PropertiesTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Properties properties = new Properties();

		properties.put("K1", "V1");
		properties.put("K2", "V2");

		Set<Object> states = properties.keySet();

		for (Object name : states)
			System.out.println("The value of " + name + " is " + properties.getProperty((String) name) + ".");
	}

}

```

#### entrySet()

```

package cn.netkiller.properties;

import java.util.Map.Entry;
import java.util.Properties;

public class PropertiesTest {

	public PropertiesTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Properties properties = new Properties();

		properties.put("K1", "V1");
		properties.put("K2", "V2");

		for (Entry<Object, Object> x : properties.entrySet()) {
			System.out.println(x.getKey() + " " + x.getValue());
		}
	}

}

```

#### 方法中返回 Properties

```

	@RequestMapping("/host")
	public Enumeration<Object> host() throws IOException {
		Properties properties = PropertiesLoaderUtils.loadProperties(new ClassPathResource(String.format("/%s.properties", "host")));
		return properties.keys();
	}

	@RequestMapping("/mail")
	public Collection<Object> mail() throws IOException {
		Properties properties = PropertiesLoaderUtils.loadProperties(new ClassPathResource(String.format("/%s.properties", "mail")));
		return properties.values();
	}
	@RequestMapping("/nameserver")
	public Set<Entry<Object, Object>> nameserver() throws IOException {
		Properties properties = PropertiesLoaderUtils.loadProperties(new ClassPathResource(String.format("/%s.properties", "dns")));
		return properties.entrySet();
	}
	@RequestMapping("/dns")
	public Properties dns() throws IOException {
		Properties properties = PropertiesLoaderUtils.loadProperties(new ClassPathResource(String.format("/%s.properties", "dns")));
		return properties;
	}	

```

#### getResourceAsStream()

```

Properties prop = new Properties();
prop.load(getServletContext().getResourceAsStream("/WEB-INF/resource/sample.properties"));

```

```

prop.load(Thread.currentThread().getContextClassLoader().getResourceAsStream("netkiller.properties"));

```

#### store

```

package cn.netkiller.config;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.util.Properties;

public class Config {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Properties prop = new Properties();
		OutputStream output = null;

		try {

			output = new FileOutputStream("config.properties");

			// set the properties value
			prop.setProperty("host", "localhost");
			prop.setProperty("port", "8000");
			prop.setProperty("user", "neo");
			prop.setProperty("pass", "password");

			// save properties to project root folder
			prop.store(output, null);

		} catch (IOException io) {
			io.printStackTrace();
		} finally {
			if (output != null) {
				try {
					output.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}

		}
	}

}

```

getProperty 取出 key 的值

```

package cn.netkiller.config;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class LoadConfig {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Properties prop = new Properties();
		InputStream input = null;

		try {

			input = new FileInputStream("config.properties");

			// load a properties file
			prop.load(input);

			// get the property value and print it out
			System.out.println(prop.getProperty("host"));
			System.out.println(prop.getProperty("port"));
			System.out.println(prop.getProperty("user"));
			System.out.println(prop.getProperty("pass"));

		} catch (IOException ex) {
			ex.printStackTrace();
		} finally {
			if (input != null) {
				try {
					input.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}

	}

}

```

循环打印所有 Properties 内容

```

package test;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Enumeration;
import java.util.Properties;

public class Application {

	public static void main(String[] args) {
		Application app = new Application();
		app.config();

	}

	private void config() {
		Properties prop = new Properties();
		InputStream input = null;
		try {

			String filename = "config.properties";

			prop.load(new FileInputStream(filename));

			Enumeration<?> e = prop.propertyNames();
			while (e.hasMoreElements()) {
				String key = (String) e.nextElement();
				String value = prop.getProperty(key);
				System.out.println(key + ": " + value);
			}

		} catch (IOException ex) {
			ex.printStackTrace();
		} finally {
			if (input != null) {
				try {
					input.close();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}
	}

}

```

#### 实现国际化

准备语言包文件 chinese.properties 内容如下

```

hello=你好世界

```

english.properties

```

hello=Helloworld

```

```

	@GetMapping("/lang")
	public String lang(@RequestHeader String lang) throws IOException {
		System.out.println(lang);
		Properties properties = PropertiesLoaderUtils.loadProperties(new ClassPathResource(lang + ".properties"));
		String tmp = properties.getProperty("hello");
		return tmp;
	}

```

测试效果

```

curl -s -H lang:chinese http://localhost:8080/lang
你好世界

curl -s -H lang:english http://localhost:8080/lang
Helloworld

```

### Logging

```

import java.util.logging.*;
public class Main {
	public static void main(String[] args) {
		Logger log = Logger.getLogger("test"); 
        log.setLevel(Level.INFO); 
        log.info("--------------------------");
        log.info("Test");
        log.info("--------------------------");

	}
}

```

XML

```

import java.io.IOException;
import java.util.logging.*;

public class Main {
	public static void main(String[] args) {

		try {
			Logger log = Logger.getLogger("test");
			FileHandler fileHandler = new FileHandler("test.%g.log");
			fileHandler.setLevel(Level.INFO);
			log.addHandler(fileHandler);

			log.setLevel(Level.INFO);
			log.info("One");
			log.info("Two");
			log.info("Three");

		} catch (SecurityException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}
}		

```

XML 输出结果

```

<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE log SYSTEM "logger.dtd">
<log>
<record>
  <date>2016-04-19T15:57:19</date>
  <millis>1461052639360</millis>
  <sequence>0</sequence>
  <logger>test</logger>
  <level>INFO</level>
  <class>Main</class>
  <method>main</method>
  <thread>1</thread>
  <message>One</message>
</record>
<record>
  <date>2016-04-19T15:57:19</date>
  <millis>1461052639394</millis>
  <sequence>1</sequence>
  <logger>test</logger>
  <level>INFO</level>
  <class>Main</class>
  <method>main</method>
  <thread>1</thread>
  <message>Two</message>
</record>
<record>
  <date>2016-04-19T15:57:19</date>
  <millis>1461052639395</millis>
  <sequence>2</sequence>
  <logger>test</logger>
  <level>INFO</level>
  <class>Main</class>
  <method>main</method>
  <thread>1</thread>
  <message>Three</message>
</record>
</log>

```

Formatter 日志格式化

```

import java.io.IOException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.logging.*;

class LogFormatter extends Formatter {
	@Override
	public String format(LogRecord record) {
		return String.format("%s %s\t%s\n", new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS").format(new Date()) , record.getLevel(), record.getMessage());
	}
}

public class Main {
	public static void main(String[] args) {

		try {
			Logger log = Logger.getLogger("test");
			FileHandler fileHandler = new FileHandler("test.%g.log");
			fileHandler.setLevel(Level.INFO);
			log.addHandler(fileHandler);
			fileHandler.setFormatter(new LogFormatter());
			log.setLevel(Level.INFO);
			log.info("One");
			log.info("Two");
			log.info("Three");

		} catch (SecurityException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}
}		

```

输出样式

```

2016-04-19 16:05:53.324 INFO	One
2016-04-19 16:05:53.352 INFO	Two
2016-04-19 16:05:53.353 INFO	Three		

```

#### console

控制台日志输入格式定义

```
				ConsoleHandler consoleHandler = new ConsoleHandler();
				consoleHandler.setLevel(Level.OFF);
				logger.addHandler(consoleHandler);

```

禁止 Console 输出

```
				logger.setUseParentHandlers(false);

```

### BASE64

```

package cn.netkiller.test;

import java.nio.charset.StandardCharsets;
import java.util.Base64;

public class Base64Test {
	public static void main(String[] args) {
		final String text = "http://www.netkiller.cn/index.html";

		final String encoded = Base64.getEncoder().encodeToString(text.getBytes(StandardCharsets.UTF_8));
		System.out.println(encoded);

		final String decoded = new String(Base64.getDecoder().decode(encoded), StandardCharsets.UTF_8);
		System.out.println(decoded);
	}
}

```

### Locale 国际化

```

package cn.netkiller.i18n;

import java.util.Locale;

public class Lang {

	public Lang() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		// create a new locale
		Locale locale = new Locale("ENGLISH", "US");

		// print locale
		System.out.println("Locale:" + locale);

		// print display name for locale - based on inLocale
		System.out.println("Name:" + locale.getDisplayName(new Locale("GERMAN", "GERMANY")));

	}

}

```

```

Locale locale = new Locale("zh", "CN");
Locale locale = Locale.US;
Locale locale = Locale.getDefault();	// 默认语言	

```

### ResourceBundle

```

java.util.ResourceBundle resourceBundle = java.util.ResourceBundle.getBundle("message");

resourceBundle.getString("nickname");	

// 指定语言

Locale  locale = new Locale("zh", "CN");
ResourceBundle res = ResourceBundle.getBundle("message", locale);	

```

### Scanner

```

Scanner in = new Scanner(System.in);
System.out.println("Username:");
String username = in.next();

```

### UUID

```

package cn.netkiller.example.uuid;

import java.util.UUID;

public class UuidTest {

	public UuidTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) {
		UUID uuid = UUID.randomUUID();
		String randomUUIDString = uuid.toString();

		System.out.println("Random UUID String = " + randomUUIDString);
		System.out.println("UUID version       = " + uuid.version());
		System.out.println("UUID variant       = " + uuid.variant());
	}

}

```

### Arrays.equals 判断两个数组是否相等

static boolean equals(type[] a, type[] b)

```

Arrays.equals(array1, array2)		

```

### Random 随机字符串

```

package cn.netkiller.test;

import java.util.Random;

public class QueueTest {

	public static void main(String[] args) throws InterruptedException {
		new Random().ints(10, 33, 38).forEach(System.out::println);
	}
}

```

```

36
34
34
36
37
35
34
35
34
33

```

#### 指定随机数范围

```

package cn.netkiller.test;

import java.util.Random;

public class RandomTest {

	public static int random(int min, int max) {
		var value = new Random().ints(min, (max + 1)).limit(1).findFirst().getAsInt();
		return value;
	}

	public static void main(String[] args) throws InterruptedException {
		System.out.println(random(10, 15));
	}
}

```

### ArrayBlockingQueue

```

package cn.netkiller.test;

import java.util.Random;
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class QueueTest {
	/**
	 * 定义装苹果的篮子
	 */
	public static class Basket {
		// 篮子，能够容纳 10 个苹果
		BlockingQueue<String> basket = new ArrayBlockingQueue<String>(10);

		// 生产苹果，放入篮子
		public void produce() throws InterruptedException {
			// put 方法放入一个苹果，若 basket 满了，等到 basket 有位置
			basket.put("An apple");
		}

		// 消费苹果，从篮子中取走
		public String consume() throws InterruptedException {
			// get 方法取出一个苹果，若 basket 为空，等到 basket 有苹果为止
			return basket.take();
		}

		public int size() {
			return basket.size();
		}

	}

	// 测试方法
	public static void testBasket() throws InterruptedException {
		// 建立一个装苹果的篮子
		final Basket basket = new Basket();
		// 定义苹果生产者
		class Producer implements Runnable {
			public void run() {
				try {
					while (true) {
						int n = random(1, 5);
						for (int i = 0; i < n; i++) {
							basket.produce();
						}
						System.out.println(System.currentTimeMillis() + " 放入" + n + "个，当前总数：" + basket.size() + "个");
						Thread.sleep(random(450, 1000));
					}
				} catch (InterruptedException ex) {
				}
			}
		}
		// 定义苹果消费者
		class Consumer implements Runnable {
			public void run() {
				try {
					while (true) {
						// 消费苹果
						int n = random(1, 5);
						for (int i = 0; i < n; i++) {
							basket.consume();
						}
						System.out.println(System.currentTimeMillis() + " 取出" + n + "个，剩余数量：" + basket.size() + "个");
						Thread.sleep(random(400, 1000));
					}
				} catch (InterruptedException ex) {
				}
			}
		}

		ExecutorService service = Executors.newCachedThreadPool();
		Producer producer = new Producer();
		Consumer consumer = new Consumer();
		service.submit(producer);
		// 延迟消费
		Thread.sleep(5000);
		service.submit(consumer);
		// 程序运行 10s 后，所有任务停止
		try {
			Thread.sleep(20000);
		} catch (InterruptedException e) {
		}
		service.shutdownNow();
	}

	public static int random(int min, int max) {
		var value = new Random().ints(min, (max + 1)).limit(1).findFirst().getAsInt();
		return value;
	}

	public static void main(String[] args) throws InterruptedException {
		QueueTest.testBasket();
	}
}

```

## IO

### 取出文件名中的扩展名

```

	File file = new File("HelloWorld.jpeg");
	String fileName = file.getName();
	String suffix = fileName.substring(fileName.lastIndexOf(".") + 1);
	System.out.println(suffix);

```

#### getAbsolutePath() 获取绝对路径

```

	File configFile = new File(System.getProperty("user.dir") + configPath);
	System.out.printf("configFile : %s\n", configFile.getAbsolutePath());			

```

#### 创建目录 mkdir()

```

//判断 cache 目录是否存在的代码，如果不存在则创建 cache 目录
String path="/tmp/cache";
File dirname = new File(path);

//目录不存在  
if (!dirname.isDirectory()) 
{ 
     dirname.mkdir(); //创建目录
} 			

```

### 临时文件

```

package cn.netkiller.file;

import java.io.File;
import java.io.IOException;

public class CreateTempFileExample
{
    public static void main(String[] args)
    {	

    	try{

    	   //create a temp file
    	   File temp = File.createTempFile("temp-file-name", ".tmp"); 

    	   System.out.println("Temp file : " + temp.getAbsolutePath());

    	}catch(IOException e){

    	   e.printStackTrace();

    	}

    }
}

```

Temp file : C:\Users\mkyong\AppData\Local\Temp\temp-file-name623426.tmp

### FileWriter 文本写入文件

```

import java.io.*;

public class Main {

    public static void main(String[] args) {

        try {
            String str = "SomeMoreTextIsHere";
            File newTextFile = new File("thetextfile.txt");

            FileWriter fw = new FileWriter(newTextFile);
            fw.write(str);
            fw.close();

        } catch (IOException iox) {
            //do stuff with exception
            iox.printStackTrace();
        }
    }
}		

```

### BufferedWriter

```

 	File file = new File( fileName );

    // if file doesnt exists, then create it 
    if ( ! file.exists( ) )
    {
        file.createNewFile( );
    }

    FileWriter fw = new FileWriter( file.getAbsoluteFile( ) );
    BufferedWriter bw = new BufferedWriter( fw );
    bw.write( text );
    bw.close( );		

```

### inputStream.transferTo()

```

var classLoader = ClassLoader.getSystemClassLoader();
var inputStream = classLoader.getResourceAsStream("hello.txt");
var tmp = File.createTempFile("tmp", "txt");
try (var outputStream = new FileOutputStream(tmp)) {
    inputStream.transferTo(outputStream);
}

```

### InputStreamReader

```

InputStreamReader stream = new InputStreamReader(this.getClass().getClassLoader().getResourceAsStream(filename));		

```

### 获得 Resource 下文件路径

```

String path = Main.class.getClassLoader().getResource("netkiller-config.yaml").getPath();
String path = Main.class.getClassLoader().getResource("netkiller-config.yaml").getPath();

```

### PrintWriter

```

package cn.netkiller.io;

import java.io.FileNotFoundException;
import java.io.PrintWriter;

public class PrintWriterTest {

	public PrintWriterTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) throws FileNotFoundException {
		// TODO Auto-generated method stub
		PrintWriter output = new PrintWriter("temp.txt");
		output.print("Welcome to Java!");
		output.close();
	}

}

```

### OutputStreamWriter

```

		File file = new File("/tmp" + File.separator + "netkiller.txt");
		OutputStreamWriter outputStreamWriter = new OutputStreamWriter(new FileOutputStream(file), "utf-8");
		outputStreamWriter.write("Netkiller Java 手札");
		outputStreamWriter.close();		

```

### FileOutputStream

```

byte[] data = ...

FileOutputStream fos = ...
fos.write(data, 0, data.length);
fos.flush();
fos.close();		

```

### FileInputStream

```

File inputFile = new File(filePath);
byte[] data = new byte[inputFile.length()];
FileInputStream fis = new FileInputStream(inputFile);
fis.read(data, 0, data.length);
fis.close();		

```

### Scanner

```

Scanner input = new Scanner(new File("temp.txt"));
System.out.print(input.nextLine());		

```

```

package cn.netkiller.io;

import java.io.File;
import java.io.FileNotFoundException;
//import java.io.PrintWriter;
import java.util.Scanner;

public class PrintWriterTest {

	private static Scanner input;

	public PrintWriterTest() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) throws FileNotFoundException {
		// TODO Auto-generated method stub
		// PrintWriter output = new PrintWriter("temp.txt");
		// output.print("Welcome to Java!");
		// output.close();

		input = new Scanner(new File("temp.txt"));
		System.out.print(input.nextLine());
	}

}

```

### 二进制文件

#### 理解二进制文件

我们运行下面一段程序，向文件 netkiller.bin 中写入一个整形数值 1 ，然后观察文件变化

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeInt(1);
		out.close();		

```

打开终端，使用 xxd 命令查看二进制文件

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -b netkiller.bin 
00000000: 00000000 00000000 00000000 00000001                    ....			

```

可以看到一串二进制 00000000 00000000 00000000 00000001，运行下面程序可以讲二进制转换为十进制，注意替换掉空格。

```

		int n = Integer.valueOf("00000000 00000000 00000000 00000001".replaceAll(" ", ""), 2);
		System.out.println(n);			

```

运行结果是 1 ，为什前面那么多 0 呢？请运行下面一段程序

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeInt(Integer.MAX_VALUE);
		out.close();

```

现在观察结果

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -b netkiller.bin
00000000: 01111111 11111111 11111111 11111111                    ....			

```

```

		int n = Integer.valueOf("01111111 11111111 11111111 11111111".replaceAll(" ", ""), 2);
		System.out.println(n);			

```

输出结果是 2147483647, 这是 int 得最大值，2147483647 + 1 会怎么样呢？

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeInt(Integer.MAX_VALUE + 1);
		out.close();

		System.out.println(Integer.MAX_VALUE + 1);			

```

输出结果是 -2147483648，正确应该是 2147483648 这就是整形溢出。整形变量得二进制表示方法是 4 个字节长度 32 位 00000000 00000000 00000000 00000000 到 01111111 11111111 11111111 11111111 ， 其中第一位 0 表示正数 1 表示负数。

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -b netkiller.bin
00000000: 10000000 00000000 00000000 00000000                    ....			

```

整形溢出演示，超出整形范围怎么办？ 使用 Long 型。

```

System.out.println(Integer.MAX_VALUE);
System.out.println(Integer.MAX_VALUE + 1);
System.out.println(Integer.MIN_VALUE);
System.out.println(Integer.MIN_VALUE - 1);

输出结果如下：

2147483647
-2147483648
-2147483648
2147483647			

```

负数演示

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeInt(-1);
		out.writeInt(Integer.MAX_VALUE + 1);
		out.close();			

```

-1 得结果是 11111111 11111111 11111111 11111111

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -b netkiller.bin
00000000: 11111111 11111111 11111111 11111111                    ....			

```

现在我们存储两个整形数值

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeInt(1);
		out.writeInt(-1);
		out.close();			

```

很清楚的看到里面有两个数值，1 和 -1

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 4 -b netkiller.bin
00000000: 00000000 00000000 00000000 00000001  ....
00000004: 11111111 11111111 11111111 11111111  ....			

```

读取二进制文件中的 int 数据

```

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {
			int i = in.readInt();
			System.out.println(i);
		} catch (EOFException e) {
			e.printStackTrace();
		}			

```

#### byte 类型

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeByte(1);
		out.close();

```

byte 只占用一个字节 8 位

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 4 -b netkiller.bin
00000000: 00000001   			

```

如果写入 -1 结果是，由此得出 第一位 0 是正数，1 是负数，可以得出他的取值范围 -128 ~ 127。超出范围也会溢出。

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 4 -b netkiller.bin
00000000: 11111111   			

```

常常写入最小值与最大值

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeByte(Byte.MIN_VALUE);
		out.writeByte(Byte.MAX_VALUE);
		out.close();			

```

运行结果

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 1 -b netkiller.bin
00000000: 10000000  .
00000001: 01111111  .			

```

写入一个字符

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeBytes("a");
		out.close();			

```

写入结果

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 1 -b netkiller.bin
00000000: 01100001  a			

```

从 ASCII 表中查出 01100001 十进制 97 十六进制 61 对应字母 a

写入一段字符串

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeBytes("http://www.netkiller.cn");
		out.close();			

```

运行结果

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 8 -b netkiller.bin
00000000: 01101000 01110100 01110100 01110000 00111010 00101111 00101111 01110111  http://w
00000008: 01110111 01110111 00101110 01101110 01100101 01110100 01101011 01101001  ww.netki
00000010: 01101100 01101100 01100101 01110010 00101110 01100011 01101110           ller.cn			

```

读取二进制文件中的 byte 字符串，readAllBytes() 可以一次读取所有 byte 到 byte[] 中。

```

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {
			System.out.println(new String(in.readAllBytes()));
		} catch (EOFException e) {
			e.printStackTrace();
		}			

```

readByte() 逐字节读取

```

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {
			char c = ' ';
			while (true) {
				try {
					c = (char) in.readByte();
					System.out.print(c);

				} catch (EOFException e) {
					System.out.println();
					break;
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}			

```

现在我们已经掌握了 byte 的操作方法，现在我们来做一个例子，读取 int 数据，int 是由 4 个字节组成一组。所以我们每次取 4 个字节。

```

		// 这个例子中，我们写入三个数值到 netkiller.bin 文件，分别是 1024，-128，2147483647
		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeInt(1024);
		out.writeInt(-128);
		out.writeInt(Integer.MAX_VALUE);
		out.close();

```

二进制文件如下

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 4 -b netkiller.bin
00000000: 00000000 00000000 00000100 00000000  ....
00000004: 11111111 11111111 11111111 10000000  ....
00000008: 01111111 11111111 11111111 11111111  ....			

```

从二进制文件读出 int 数据。

```

		String filename = "netkiller.bin";
		FileInputStream stream = new FileInputStream(filename);

		byte[] buffer = new byte[4];

		while (stream.read(buffer) != -1) {
			ByteBuffer byteBuffer = ByteBuffer.wrap(buffer);
			System.out.println(byteBuffer.getInt());
		}			

```

运行结果

```

1024
-128
2147483647			

```

#### boolean 布尔型

我们想文件写入两个布尔类型，一个是 true, 另一个是 false

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeBoolean(true);
		out.writeBoolean(false);
		out.close();			

```

运行结果可以看出 boolean 使用了一个字节，最后一位 1 表示 true, 0 表示 false。所以对于二进制文件最小单位就是 byte 字节，虽然 boolean 型只需要一个 1 bit 位，但是存储的最小单位是字节，所以前面需要补 7 个零 0000000。

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 1 -b netkiller.bin
00000000: 00000001  .
00000001: 00000000  .			

```

使用 ls 命令可以看这个文件占用了 2B（两个字节）

```

neo@MacBook-Pro ~/workspace/netkiller % ll netkiller.bin 
-rw-r--r--  1 neo  staff     2B Oct 18 13:47 netkiller.bin			

```

读取二进制文件中的 boolean 数据

```

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {
			boolean bool = in.readBoolean();
			System.out.println(bool);
		} catch (EOFException e) {
			e.printStackTrace();
		}			

```

#### Long 型

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeLong(1);
		out.close();			

```

有了上面 int 型数据的经验，下面一看你就会明白。long 型采用 8 个字节保存数据，是 int 的一倍。取值范围这里就不多说了，也会存在溢出现象。

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 8 -b netkiller.bin
00000000: 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000001  ........			

```

取值范围

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeLong(Long.MIN_VALUE);
		out.writeLong(Long.MAX_VALUE);
		out.close();			

```

输出文件

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 8 -b netkiller.bin
00000000: 10000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000  ........
00000008: 01111111 11111111 11111111 11111111 11111111 11111111 11111111 11111111  ........			

```

读取二进制文件中的 long 数据

```

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {

			long l = in.readLong();
			System.out.println(l);

		} catch (EOFException e) {
			e.printStackTrace();
		}			

```

#### chat 类型

有符号 signed char 类型的范围为 -128~127

无符号 unsigned char 的范围为 0~ 255

char 与 byte 操作类似，我们首先去 ASCII 表查找字符 A 对应 65，我们将 65 写入二进制文件。然后读取该字符，输出结果是 A。

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeChar(65);
		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {

			char c = in.readChar();
			System.out.println(c);

		} catch (EOFException e) {
			e.printStackTrace();
		}

```

从二进制文件中我们可以看到 char 类型占用 2 个字节 16 位

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 2 -b netkiller.bin
00000000: 00000000 01000001  .A			

```

使用 writeChars()写入字符串到二进制文件

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeChars("http://www.netkiller.cn");
		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));

		char c = ' ';
		while (true) {
			try {
				c = in.readChar();
				System.out.print(c);

			} catch (EOFException e) {
				System.out.println();
				break;
			}
		}

```

二进制文件如下，你会发现第一个字节没有用到，很多 00000000 所以如果存储英文 byte 更适合，char 是双倍 byte 开销。

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 8 -b netkiller.bin 
00000000: 00000000 01101000 00000000 01110100 00000000 01110100 00000000 01110000  .h.t.t.p
00000008: 00000000 00111010 00000000 00101111 00000000 00101111 00000000 01110111  .:././.w
00000010: 00000000 01110111 00000000 01110111 00000000 00101110 00000000 01101110  .w.w...n
00000018: 00000000 01100101 00000000 01110100 00000000 01101011 00000000 01101001  .e.t.k.i
00000020: 00000000 01101100 00000000 01101100 00000000 01100101 00000000 01110010  .l.l.e.r
00000028: 00000000 00101110 00000000 01100011 00000000 01101110                    ...c.n			

```

存储汉字

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		String s = "陈";
		char name = s.charAt(s.length() - 1);
		out.writeChar(name);
		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));

		char c = ' ';
		while (true) {
			try {
				c = in.readChar();
				System.out.print(c);

			} catch (EOFException e) {
				System.out.println();
				break;
			}
		}

```

二进制文件如下，使用两个字节表示一个汉字

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 2 -b netkiller.bin
00000000: 10010110 01001000  .H			

```

转成 Hex 十六进制，得到 96 48 两个数字。

```

neo@MacBook-Pro ~/workspace/netkiller % hexdump  netkiller.bin 
0000000 96 48                                          
0000002			

```

现在去搜索引擎搜索“汉字内码”，然后查询“陈”这个汉字，可以看到 Unicode 编码 16 进制就是 96 48

尝试写入汉字字符串

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeChars("陈景峰");
		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {
			char c = ' ';
			while (true) {
				try {
					c = in.readChar();
					System.out.print(c);

				} catch (EOFException e) {
					System.out.println();
					break;
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}

```

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -b netkiller.bin 
00000000: 10010110 01001000 01100110 01101111 01011100 11110000  .Hfo\.			

```

#### UTF 字符串

这次我们使用新的文件名 netkiller.txt

```

		String filename = "netkiller.txt";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));

		out.writeUTF("峰");

		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {
			System.out.println(in.readUTF());
		} catch (EOFException e) {
			e.printStackTrace();
		}			

```

查看二进制文件，一个汉字怎么这么多字节？

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -b netkiller.txt    
00000000: 00000000 00000011 11100101 10110011 10110000           .....

```

转成 16 禁止看看。

```

neo@MacBook-Pro ~/workspace/netkiller % hexdump netkiller.txt
0000000 00 03 e5 b3 b0                                 
0000005					

```

我们在网上查询 “峰” 字的汉字内码，可以看到 UTF-8 内码是 E5 B3 B0。这是因为 UTF8 使用三个字节存储汉字。 00000000 00000011 可能是 UTF 标志位，具体我也不太清楚，总之不是 BOM 信息。

我们现在写入一个字符串试试

```

		out.writeUTF("陈景峰");			

```

xxd -s 2 -c 3 表示跳过两个字节，三列显示

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -s 2 -c 3 -b netkiller.txt
00000002: 11101001 10011001 10001000  ...
00000005: 11100110 10011001 10101111  ...
00000008: 11100101 10110011 10110000  ...

```

UTF 字符是可以直接使用文本工具查看的。

```

neo@MacBook-Pro ~/workspace/netkiller % cat netkiller.txt 
	陈景峰			

```

#### Short 类型

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));

		out.writeShort(1);

		out.flush();
		out.close();			

```

输出结果，Short 使用两个字节 16 位表示。

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 2 -b netkiller.bin
00000000: 00000000 00000001  ..			

```

Short 分为有符号和无符号类型

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));

		out.writeShort(1);
		out.writeShort(1);
		out.writeShort(-1);
		out.writeShort(-1);

		out.flush();
		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));
		try {
			System.out.println(in.readShort());
			System.out.println(in.readUnsignedShort());
			System.out.println(in.readShort());
			System.out.println(in.readUnsignedShort());
		} catch (EOFException e) {
			e.printStackTrace();
		}			

```

运行结果

```

1
1
-1
65535

```

有符号的取值范围

```

最小值：Short.MIN_VALUE=-32768 （-2 的 15 此方）
最大值：Short.MAX_VALUE=32767 （2 的 15 次方-1）			

```

无符号的取值范围是 0 ~ 65535

#### float 单精度浮点类型

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));

		out.writeFloat(0);
		out.writeFloat(1.0f);
		out.writeFloat(1.1f);
		out.flush();
		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));

		float c = 0;
		while (true) {
			try {
				c = in.readFloat();
				System.out.println(c);

			} catch (EOFException e) {
				System.out.println();
				break;
			}
		}			

```

float 使用 4 字节 32 为表示浮点类型，float 不同于前面数据类型，无法直接读取浮点数，需要经过计算才能得出，有点复杂。

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 4 -b netkiller.bin
00000000: 00000000 00000000 00000000 00000000  ....
00000004: 00111111 10000000 00000000 00000000  ?...
00000008: 00111111 10001100 11001100 11001101  ?...			

```

浮点型示意图

```

 /------------- 32 bit ----------------\
 | 1 |     8  |           23            |
 |--------------------------------------|
 31 30        22                        0
  ^       ^               ^
符号位   指数位     	    尾数部分         32 位

首先 float 二进制是从后向前读。与上面所有类型相反。
符号位(Sign) : 0 代表正，1 代表为负
指数位（Exponent）:用于存储科学计数法中的指数数据，并且采用移位存储
尾数部分（Mantissa）：尾数部分

将一个内存存储的 float 二进制格式转化为十进制的步骤： 
（1）将第 22 位到第 0 位的二进制数写出来，在最左边补一位“1”，得到二十四位有效数字。将小数点点在最左边那个“1”的右边。 
（2）取出第 29 到第 23 位所表示的值 n。当 30 位是“0”时将 n 各位求反。当 30 位是“1”时将 n 增 1。 
（3）将小数点左移 n 位（当 30 位是“0”时）或右移 n 位（当 30 位是“1”时），得到一个二进制表示的实数。 
（4）将这个二进制实数化为十进制，并根据第 31 位是“0”还是“1”加上正号或负号即可。

1.0f = 00111111 10000000 00000000 00000000

Sign 31 位是 0 表示正数
Exponent 23~30 位 0111111 1
Mantissa 0~22 位 0000000 00000000 00000000

得到

| 0 | 0111111 1 | 0000000 00000000 00000000 |

具体细节请参考 IEEE R32.24

```

#### double 数据类型

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeDouble(12.5d);
		out.flush();
		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));

		double d = 0d;
		while (true) {
			try {
				d = in.readDouble();
				System.out.println(d);

			} catch (EOFException e) {
				System.out.println();
				break;
			}
		}			

```

二进制文件

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 8 -b netkiller.bin
00000000: 01000000 00101001 00000000 00000000 00000000 00000000 00000000 00000000  @)......			

```

```

 /------------------------- 64 bit ------------------------------\
 | 1 |    11    |           52                                    |
 |----------------------------------------------------------------|
 63 62          51                                                0
  ^         ^                  ^
符号位     指数位       	    尾数部分                              64 位

首先 float 二进制是从后向前读。与上面所有类型相反。
符号位(Sign) : 0 代表正，1 代表为负
指数位（Exponent）:用于存储科学计数法中的指数数据，并且采用移位存储
尾数部分（Mantissa）：尾数部分

详细参加考 IEEE R64.53

```

#### 二进制文件操作演示

##### 所有类型演示一遍

```

		String filename = "netkiller.bin";
		DataOutputStream out = new DataOutputStream(new FileOutputStream(filename));
		out.writeInt(1024);
		out.writeShort(255);
		out.writeLong(100000000000L);
		out.writeFloat(3.14f);
		out.writeDouble(3.141592653579d);
		out.writeBoolean(true);
		out.writeChar(165);
		out.writeChars("陈景峰");
		out.writeUTF("Netkiller Java 手札 - http://www.netkiller.cn");
		out.writeChars("这是最后一行\r\n");
		out.flush();
		out.close();

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream(filename)));

		System.out.println(in.readInt());
		System.out.println(in.readUnsignedShort());
		System.out.println(in.readLong());
		System.out.println(in.readFloat());
		System.out.println(in.readDouble());
		System.out.println(in.readBoolean());
		System.out.println(in.readChar());

		int i = 0;
		String name = "";
		while (i < 3) {
			try {
				char c = in.readChar();
				name += c;
			} catch (EOFException e) {
				break;
			}
			i++;
		}

		System.out.println(name);
		System.out.println(in.readUTF());
		System.out.println(in.readUTF());		

```

需要注意的一点是 out.writeChars("陈景峰"); 写入 char 字符串，在读取的时候你需要知道字符串的长度。然后循环取出 char 数据。

二进制文件内容

```

neo@MacBook-Pro ~/workspace/netkiller % xxd -c 8 -b netkiller.bin
00000000: 00000000 00000000 00000100 00000000 00000000 11111111 00000000 00000000  ........
00000008: 00000000 00010111 01001000 01110110 11101000 00000000 01000000 01001000  ..Hv..@H
00000010: 11110101 11000011 01000000 00001001 00100001 11111011 01010100 01000011  ..@.!.TC
00000018: 11001110 00101000 00000001 00000000 10100101 10010110 01001000 01100110  .(....Hf
00000020: 01101111 01011100 11110000 00000000 00101111 01001110 01100101 01110100  o\../Net
00000028: 01101011 01101001 01101100 01101100 01100101 01110010 00100000 01001010  killer J
00000030: 01100001 01110110 01100001 00100000 11100110 10001001 10001011 11100110  ava ....
00000038: 10011100 10101101 00100000 00101101 00100000 01101000 01110100 01110100  .. - htt
00000040: 01110000 00111010 00101111 00101111 01110111 01110111 01110111 00101110  p://www.
00000048: 01101110 01100101 01110100 01101011 01101001 01101100 01101100 01100101  netkille
00000050: 01110010 00101110 01100011 01101110 10001111 11011001 01100110 00101111  r.cn..f/
00000058: 01100111 00000000 01010100 00001110 01001110 00000000 10001000 01001100  g.T.N..L
00000060: 00000000 00001101 00000000 00001010                                      ....			

```

16 进制编辑器更好阅读一些

```

neo@MacBook-Pro ~/workspace/netkiller % hexdump -C netkiller.bin 
00000000  00 00 04 00 00 ff 00 00  00 17 48 76 e8 00 40 48  |..........Hv..@H|
00000010  f5 c3 40 09 21 fb 54 43  ce 28 01 00 a5 96 48 66  |..@.!.TC.(....Hf|
00000020  6f 5c f0 00 2f 4e 65 74  6b 69 6c 6c 65 72 20 4a  |o\../Netkiller J|
00000030  61 76 61 20 e6 89 8b e6  9c ad 20 2d 20 68 74 74  |ava ...... - htt|
00000040  70 3a 2f 2f 77 77 77 2e  6e 65 74 6b 69 6c 6c 65  |p://www.netkille|
00000050  72 2e 63 6e 8f d9 66 2f  67 00 54 0e 4e 00 88 4c  |r.cn..f/g.T.N..L|
00000060  00 0d 00 0a                                       |....|
00000064			

```

##### 检查文件是否是 png 文件

```

package cn.netkiller.io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.InputStream;
import java.util.Arrays;

public class PngFile {

	private static InputStream inputStream;

	public PngFile() {
	}

	public static void main(String[] args) throws FileNotFoundException {

		int[] pngSignature = { 137, 80, 78, 71, 13, 10, 26, 10 };

		try {
			inputStream = new FileInputStream("/Users/neo/Downloads/NBRC.png");

			byte[] headerBytes = new byte[8];
			inputStream.read(headerBytes);

			// for (byte b : headerBytes) {
			// System.out.println(b);
			// }

			int[] pngHeader = new int[8];

			for (int i = 0; i < headerBytes.length; i++) {
				pngHeader[i] = (int) headerBytes[i] & 0xff;
			}

			if (Arrays.equals(pngHeader, pngSignature)) {
				System.out.println("This is a PNG file! ");
			}
		} catch (IOException e) {
			e.printStackTrace();
		}

	}

}				

```

## Reflection 反射

```
this.getClass().getName() //当前 Class 名字
Thread.currentThread().getStackTrace()[1].getMethodName()); //当前方法名

```

### 获得所有变量

```

	Field[] fields = objClass.getFields();
	for (Field field : fields) {
		System.out.println(field.getName());
	}

```

注意：只能去除 public 变量

### 批量赋值

### 方法操作

JAVA 反射调用方法的步骤有三步

```
得到要调用类的 class
得到要调用的类中的方法(Method)
方法调用(invoke)

```

#### 获得所有方法

```

	Class<?> objClass = a.getClass();
	Method[] methods =  objClass.getDeclaredMethods();
	for (Method method : methods) {
		System.out.println(method);
	}			

```

#### set/get 方法

```

package cn.netkiller.reflect;

import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class Member {
	public String name;
	private int age;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	@Override
	public String toString() {
		return "ClassA [name=" + name + ", age=" + age + "]";
	}

	public Member() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, NoSuchMethodException, SecurityException, IllegalArgumentException, InvocationTargetException {
		Class<?> cls = Class.forName("cn.netkiller.reflect.Member");
		Object member = cls.newInstance();
		Method setMethod = cls.getDeclaredMethod("setAge", int.class);
		setMethod.invoke(member, 15);

		Method getMethod = cls.getDeclaredMethod("getAge");
		System.out.println(getMethod.invoke(member));

	}

}

```

下面做一个稍微复杂点的例子，ClassB 继承 ClassA，取出 ClassA 的成员变量赋值到 ClassA。

```

package cn.netkiller.reflect;

public class ClassA {
	public String name;
	private int age;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public ClassA() {
		// TODO Auto-generated constructor stub
	}

	@Override
	public String toString() {
		return "ClassA [name=" + name + ", age=" + age + "]";
	}
}

package cn.netkiller.reflect;

public class ClassB extends ClassA{

	public ClassB() {
		// TODO Auto-generated constructor stub
	}
	private String address;

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	@Override
	public String toString() {
		return "ClassB [address=" + address + "]";
	}

}

package cn.netkiller.reflect;

import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class ReflectionTest {

	public ReflectionTest() {
		// TODO Auto-generated constructor stub
	}

	public void testSetMethod() throws NoSuchMethodException, SecurityException, IllegalAccessException, IllegalArgumentException, InvocationTargetException, InstantiationException {

		// ClassA a = new ClassA();

		ClassB b = new ClassB();
		b.setAddress("Shenzhen");

		Class<ClassA> classA = ClassA.class;
		ClassA a = classA.newInstance();
		a.setName("Neo");
		a.setAge(30);

		System.out.println(classA.getDeclaredMethod("getAge").invoke(a));

		Method m = classA.getDeclaredMethod("setAge", int.class);
		m.setAccessible(true); // 因为写成 private 所以这里必须设置
		m.invoke(b, 26);

		System.out.println(a.toString());
		System.out.println(b.toString());

		System.out.println(b.getName());
		System.out.println(b.getAge());
	}

	public static void main(String[] args) throws InvocationTargetException {

		ReflectionTest rt = new ReflectionTest();
		try {
			rt.testSetMethod();

		} catch (NoSuchMethodException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (SecurityException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IllegalAccessException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IllegalArgumentException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (InstantiationException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}

}

```

set 方法

```

System.out.println(classA.getDeclaredMethod("getAge").invoke(a));	

```

get 方法

```

	Method m = classA.getDeclaredMethod("setAge", int.class);
	m.setAccessible(true);	//因为写成 private 所以这里必须设置
	m.invoke(b, 26);	

```

#### static 方法调用

```

Class cls = Class.forName("cn.netkiller.reflect.Student");
Method setMethod = cls.getDeclaredMethod("setAge",int.class);
setMethod.invoke(cls.newInstance(), 15);

```

static 方法调用时，不必得实例化对象

```

Class cls = Class.forName("cn.netkiller.reflect.Student");
Method staticMethod = cls.getDeclaredMethod("setAge",int.class);
staticMethod.invoke(cls,20); //这里不需要 newInstance

```

## Thread 线程

### 实现异步执行

```

	public void testThread() throws Exception {
		try {
			Thread sendmail = new Thread(new Runnable() {
				@Override
				public void run() {
					// Sendmail
					log.info("Sendmail OK");
				}
			});
			sendmail.setName("sendmail");
			sendmail.start();
		} catch (Exception e) {
			e.printStackTrace();
		}	
	}				

```

### 继承 Thread 类实现多线程

```

package cn.netkiller.ipo.test;

public class MyThread extends Thread {

	private String name;

	public MyThread(String name) {
		super();
		this.name = name;
	}

	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println("Thread:" + this.name + ",i=" + i);
		}
	}

	public static void main(String[] args) {
		MyThread mt1 = new MyThread("A");
		MyThread mt2 = new MyThread("B");
		mt1.start();
		mt2.start();
	}
}

```

### 实现 Runnable 接口

```

package cn.netkiller.ipo.test;

public class MyRunnable implements Runnable {

	private String name;

	public MyRunnable(String name) {
		this.name = name;
	}

	@Override
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println("Thread:" + this.name + ",i=" + i);
		}

	}

	public static void main(String[] args) {

		MyRunnable mr1 = new MyRunnable("A");
		MyRunnable mr2 = new MyRunnable("B");

		new Thread(mr1).start();
		new Thread(mr2).start();
		new Thread(new MyRunnable("C")).start();
	}
}

```

### 线程同步

```

package cn.netkiller.thread;

public class SynchronizedThread extends Thread {
	private int count = 0;

	@Override
	public /*synchronized*/ void run() {
		count++;
		System.out.println(Thread.currentThread().getName() + " count:" + count);
	}

	public static void main(String[] args) {
		SynchronizedThread myThread = new SynchronizedThread();
		Thread thread1 = new Thread(myThread, "thread1");
		Thread thread2 = new Thread(myThread, "thread2");
		Thread thread3 = new Thread(myThread, "thread3");
		Thread thread4 = new Thread(myThread, "thread4");
		Thread thread5 = new Thread(myThread, "thread5");
		thread1.start();
		thread2.start();
		thread3.start();
		thread4.start();
		thread5.start();
	}
}		

```

线程运行不分先后

```

thread2 count:3
thread4 count:4
thread1 count:3
thread3 count:3
thread5 count:5

```

```

package cn.netkiller.thread;

public class SynchronizedThread extends Thread {
	private int count = 0;

	@Override
	public synchronized void run() {
		count++;
		System.out.println(Thread.currentThread().getName() + " count:" + count);
	}

	public static void main(String[] args) {
		SynchronizedThread myThread = new SynchronizedThread();
		Thread thread1 = new Thread(myThread, "thread1");
		Thread thread2 = new Thread(myThread, "thread2");
		Thread thread3 = new Thread(myThread, "thread3");
		Thread thread4 = new Thread(myThread, "thread4");
		Thread thread5 = new Thread(myThread, "thread5");
		thread1.start();
		thread2.start();
		thread3.start();
		thread4.start();
		thread5.start();
	}
}		

```

```

thread1 count:1
thread5 count:2
thread4 count:3
thread2 count:4
thread3 count:5

```

```

package cn.netkiller.thread;

public class MultiThread {
	private static int count = 0;

	public synchronized void add() {
		count++;
		System.out.println(Thread.currentThread().getName() + " count:" + count);
	}

	public static void main(String[] args) throws InterruptedException {

		final MultiThread multiThread1 = new MultiThread();
		final MultiThread multiThread2 = new MultiThread();
		final MultiThread multiThread3 = new MultiThread();
		final MultiThread multiThread4 = new MultiThread();
		final MultiThread multiThread5 = new MultiThread();

		new Thread(new Runnable() {
			public void run() {
				multiThread1.add();
			}
		}).start();

		new Thread(new Runnable() {
			public void run() {
				multiThread2.add();
			}
		}).start();

		new Thread(new Runnable() {
			public void run() {
				multiThread3.add();
			}
		}).start();

		new Thread(new Runnable() {
			public void run() {
				multiThread4.add();
			}
		}).start();

		new Thread(new Runnable() {
			public void run() {
				multiThread5.add();
			}
		}).start();
	}
}

```

## java 脚本引擎

什么是脚本引擎，脚本引擎是指在程序运行期间嵌入另一种脚本语言，并与其交互，产生最终运行结果

脚本引擎存在的意义是什么？脚本引擎可以改变编译语言的内部运行逻辑，弥补编译语言的不足，使编译语言具备动态语言的一部分特性。

是否有成功案例？最成功的案例就是基于 C++和 Lua 语言开发的端游（网游一种，需要按照客户端），编译语言最大的缺点就是客户端升级需要重新安装并且安装之后重启应用程序才能生效。脚本引擎弥补了这项致命的缺点，用户只需升级剧情脚本，而不需要退出整个游戏然后重新进入。

### Maven

```

<project   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>cn.netkiller</groupId>
	<artifactId>script</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>Java Script</name>
	<description>Java Script Engine</description>
	<dependencies>
		<!-- https://mvnrepository.com/artifact/org.mockito/mockito-all -->
		<dependency>
			<groupId>org.mockito</groupId>
			<artifactId>mockito-all</artifactId>
			<version>1.10.19</version>
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

### Helloworld

```

package cn.netkiller.javascript;

import java.util.List;
import java.util.logging.Level;
import java.util.logging.Logger;

import javax.script.ScriptEngine;
import javax.script.ScriptEngineFactory;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;

public class Helloworld {

	public Helloworld() {
		ScriptEngineManager manager = new ScriptEngineManager();
		List<ScriptEngineFactory> factories = manager.getEngineFactories();
		for (ScriptEngineFactory f : factories) {
			System.out.println("egine name:" + f.getEngineName());
			System.out.println("engine version:" + f.getEngineVersion());
			System.out.println("language name:" + f.getLanguageName());
			System.out.println("language version:" + f.getLanguageVersion());
			System.out.println("names:" + f.getNames());
			System.out.println("mime:" + f.getMimeTypes());
			System.out.println("extension:" + f.getExtensions());
			System.out.println("-----------------------------------------------");
		}
	}

	public void hello() throws ScriptException {
		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName("JavaScript");
		// ScriptEngine engine = manager.getEngineByExtension("js");
		// ScriptEngine engine = manager.getEngineByMimeType("text/javascript");
		engine.eval("print('Hello, World')");
	}

	public static void main(String[] args) {
		try {
			new Helloworld().hello();
		} catch (ScriptException ex) {
			Logger.getLogger(Helloworld.class.getName()).log(Level.SEVERE, null, ex);
		}
	}
}

```

### 运行脚本文件

将脚本放入外部文件

```

package javascript;

import java.io.FileNotFoundException;
import java.net.URL;

import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;

public class EvalFile {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByExtension("js");
		// ScriptEngine engine = manager.getEngineByMimeType("text/javascript");

		try {

			URL location = EvalFile.class.getProtectionDomain().getCodeSource().getLocation();
			String file = location.getFile() + "test.js";
			System.out.println(file);

			engine.eval(new java.io.FileReader(file));

		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (ScriptException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}

```

test.js

```
print("This is hello from test.js");		

```

### 变量传递

```

package javascript;

import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

import javax.script.Bindings;
import javax.script.ScriptContext;
import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;
import javax.script.SimpleBindings;

public class ScriptVars {

	ScriptEngine engine = null;

	public ScriptVars() {
		ScriptEngineManager manager = new ScriptEngineManager();
		engine = manager.getEngineByMimeType("text/javascript");
	}

	public void variable() throws ScriptException {
		engine.put("name", "Neo");
		engine.eval("var message = 'Hello, ' + name;");
		engine.eval("print(message);");
		Object obj = engine.get("message");
		System.out.println(obj);
	}

	public void simpleBindings() throws ScriptException {
		Bindings bindings = new SimpleBindings();
		bindings.put("name", "Neo");
		engine.eval("print('I am ' + name);", bindings);
	}

	public void function() throws ScriptException {
		engine.put("a", 4);
		engine.put("b", 6);
		Object maxNum = engine.eval("function max_num(a,b){return (a>b)?a:b;}max_num(a,b);");
		System.out.println("max_num:" + maxNum + ", (class = " + maxNum.getClass() + ")");

		Map<String, Integer> m = new HashMap<String, Integer>();
		m.put("c", 10);
		engine.put("m", m);
		engine.eval("var x= max_num(a,m.get('c'));");
		System.out.println("max_num:" + engine.get("x"));
	}

	public void object() throws ScriptException {
		File f = new File("test.txt");
		// expose File object as variable to script
		engine.put("file", f);

		// evaluate a script string. The script accesses "file"
		// variable and calls method on it
		engine.eval("print(file.getAbsolutePath())");
	}

	public void outputToFile() throws IOException, ScriptException {
		ScriptContext context = engine.getContext();
		context.setWriter(new FileWriter("script.log"));
		engine.eval("print('Hello World!');");
	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		try {
			ScriptVars sv = new ScriptVars();
			sv.variable();
			sv.simpleBindings();
			sv.outputToFile();
			sv.function();
			sv.object();
			sv.outputToFile();

		} catch (ScriptException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}

}

```

### 全局变量与局部变量定义

ScriptContext.GLOBAL_SCOPE 作用于 ScriptEngineManager， ScriptContext.ENGINE_SCOPE 作用于 ScriptEngine

```

package javascript;

import javax.script.ScriptContext;
import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;

public class ContextVariable {

	public static void main(String[] args) throws ScriptException {
		// TODO Auto-generated method stub
		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName("JavaScript");
		ScriptContext context = engine.getContext();  
	    context.setAttribute("name", "Netkiller", ScriptContext.GLOBAL_SCOPE);  
	    context.setAttribute("name", "Neo", ScriptContext.ENGINE_SCOPE);  
	    //context.getAttribute("name"); 
	    engine.eval("print('I am ' + name);", context);

	    // engine1,context1 并没有定义 name 但输出结果却显示 Netkiller，所以 ScriptContext.GLOBAL_SCOPE 定义的变量是全局的。 
	    ScriptEngine engine1 = manager.getEngineByName("JavaScript");
	    ScriptContext context1 = engine1.getContext();  
	    engine.eval("print('I am ' + name);", context1);

	}

}		

```

```

import javax.script.*;

public class MultiScopes {
    public static void main(String[] args) throws Exception {
        ScriptEngineManager manager = new ScriptEngineManager();
        ScriptEngine engine = manager.getEngineByName("JavaScript");

        engine.put("x", "hello");
        // print global variable "x"
        engine.eval("print(x);");
        // the above line prints "hello"

        // Now, pass a different script context
        ScriptContext newContext = new SimpleScriptContext();
        Bindings engineScope = newContext.getBindings(ScriptContext.ENGINE_SCOPE);

        // add new variable "x" to the new engineScope
        engineScope.put("x", "world");

        // execute the same script - but this time pass a different script context
        engine.eval("print(x);", newContext);
        // the above line prints "world"
    }
}

```

### 调用脚本中的函数或方法

```

package javascript;

import javax.script.Invocable;
import javax.script.ScriptEngine;
import javax.script.ScriptEngineManager;
import javax.script.ScriptException;

public class InvokeScriptFunction {

	public static void main(String[] args) throws ScriptException, NoSuchMethodException {
		// TODO Auto-generated method stub
		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName("JavaScript");

		// JavaScript code in a String
		String script = "function hello(name) { print('Hello, ' + name); }";
		// evaluate script
		engine.eval(script);

		// javax.script.Invocable is an optional interface.
		// Check whether your script engine implements or not!
		// Note that the JavaScript engine implements Invocable interface.
		Invocable inv = (Invocable) engine;

		// invoke the global function named "hello"
		inv.invokeFunction("hello", "Scripting!!");

		// JavaScript code in a String. This code defines a script object 'obj'
		// with one method called 'hello'.
		script = "var obj = new Object(); obj.hello = function(name) { print('Hello, ' + name); }";
		// evaluate script
		engine.eval(script);
		// get script object on which we want to call the method
		Object obj = engine.get("obj");

		// invoke the method named "hello" on the script object "obj"
		inv.invokeMethod(obj, "hello", "Script Method !!");
	}
}

```

### 脚本编译

只有重复执行脚本时才需要编译。只运行一次不建议编译运行。

```

package javascript;

import javax.script.*;

public class ScriptCompile {
	public CompiledScript compile(String script) throws ScriptException {

		ScriptEngineManager manager = new ScriptEngineManager();
		ScriptEngine engine = manager.getEngineByName("JavaScript");

		return ((Compilable) engine).compile(script);

	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		ScriptCompile sc = new ScriptCompile();
		try {

			CompiledScript script = sc.compile("print('Helloworld!!!');");

			for (int i = 0; i < 100; i++) {
				script.eval();
			}

		} catch (ScriptException ex) {
			ex.printStackTrace();
		}
	}

}

```

### jjs - Invokes the Nashorn engine.

jjs 是一个在命令行运行脚本的命令

创建脚本文件

```

[neo@netkiller tmp]$ cat test.js 
function f() { 
     return 1; 
}; 

print( f() + 1 );

```

运行结果

```

[neo@netkiller tmp]$ jjs test.js 
2		

```

## Crypto

### MD5

JDK 1.8

```
			String hash = DatatypeConverter.printHexBinary(MessageDigest.getInstance("MD5").digest("SOMESTRING".getBytes("UTF-8")));

```

### AES

```

package cn.netkiller.crypto;

import javax.crypto.Cipher;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;
import java.security.MessageDigest;
import java.security.SecureRandom;

public class TestAES {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String key = "fm6I1D2HTFVVOWUKny76TThagNq5Czrv";
		String clean = "Helloworld!!!";

		try {
			byte[] encrypted = encrypt(clean, key);
			String decrypted = decrypt(encrypted, key);
			System.out.println(decrypted);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}

	public static byte[] encrypt(String plainText, String key) throws Exception {
		byte[] clean = plainText.getBytes();

		// Generating IV.
		int ivSize = 16;
		byte[] iv = new byte[ivSize];
		SecureRandom random = new SecureRandom();
		random.nextBytes(iv);
		IvParameterSpec ivParameterSpec = new IvParameterSpec(iv);

		// Hashing key.
		MessageDigest digest = MessageDigest.getInstance("SHA-256");
		digest.update(key.getBytes("UTF-8"));
		byte[] keyBytes = new byte[16];
		System.arraycopy(digest.digest(), 0, keyBytes, 0, keyBytes.length);
		SecretKeySpec secretKeySpec = new SecretKeySpec(keyBytes, "AES");

		// Encrypt.
		Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5Padding");
		cipher.init(Cipher.ENCRYPT_MODE, secretKeySpec, ivParameterSpec);
		byte[] encrypted = cipher.doFinal(clean);

		// Combine IV and encrypted part.
		byte[] encryptedIVAndText = new byte[ivSize + encrypted.length];
		System.arraycopy(iv, 0, encryptedIVAndText, 0, ivSize);
		System.arraycopy(encrypted, 0, encryptedIVAndText, ivSize, encrypted.length);

		return encryptedIVAndText;
	}

	public static String decrypt(byte[] encryptedIvTextBytes, String key) throws Exception {
		int ivSize = 16;
		int keySize = 16;

		// Extract IV.
		byte[] iv = new byte[ivSize];
		System.arraycopy(encryptedIvTextBytes, 0, iv, 0, iv.length);
		IvParameterSpec ivParameterSpec = new IvParameterSpec(iv);

		// Extract encrypted part.
		int encryptedSize = encryptedIvTextBytes.length - ivSize;
		byte[] encryptedBytes = new byte[encryptedSize];
		System.arraycopy(encryptedIvTextBytes, ivSize, encryptedBytes, 0, encryptedSize);

		// Hash key.
		byte[] keyBytes = new byte[keySize];
		MessageDigest md = MessageDigest.getInstance("SHA-256");
		md.update(key.getBytes());
		System.arraycopy(md.digest(), 0, keyBytes, 0, keyBytes.length);
		SecretKeySpec secretKeySpec = new SecretKeySpec(keyBytes, "AES");

		// Decrypt.
		Cipher cipherDecrypt = Cipher.getInstance("AES/CBC/PKCS5Padding");
		cipherDecrypt.init(Cipher.DECRYPT_MODE, secretKeySpec, ivParameterSpec);
		byte[] decrypted = cipherDecrypt.doFinal(encryptedBytes);

		return new String(decrypted);
	}

}

```

### AES/CBC/PKCS5PADDING

```

package cn.netkiller.security;

import java.util.Base64;

import javax.crypto.Cipher;
import javax.crypto.spec.IvParameterSpec;
import javax.crypto.spec.SecretKeySpec;

public class AES {
	private static final String initVector = "encryptionIntVec";
	private String key;

	public AES(String key) {
		// TODO Auto-generated constructor stub
		this.key = key;
	}

	public String encrypt(String value) {
		return this.encrypt(value, this.key);
	}

	public String encrypt(String value, String key) {
		try {
			IvParameterSpec ivParameterSpec = new IvParameterSpec(initVector.getBytes("UTF-8"));
			SecretKeySpec secretKeySpec = new SecretKeySpec(key.getBytes("UTF-8"), "AES");

			Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5PADDING");
			cipher.init(Cipher.ENCRYPT_MODE, secretKeySpec, ivParameterSpec);

			byte[] encrypted = cipher.doFinal(value.getBytes());
			return Base64.getEncoder().encodeToString(encrypted);
		} catch (Exception ex) {
			ex.printStackTrace();
		}
		return null;
	}

	public String decrypt(String encrypted) {
		return this.decrypt(encrypted, this.key);
	}

	public String decrypt(String encrypted, String key) {
		try {
			IvParameterSpec ivParameterSpec = new IvParameterSpec(initVector.getBytes("UTF-8"));
			SecretKeySpec secretKeySpec = new SecretKeySpec(key.getBytes("UTF-8"), "AES");

			Cipher cipher = Cipher.getInstance("AES/CBC/PKCS5PADDING");
			cipher.init(Cipher.DECRYPT_MODE, secretKeySpec, ivParameterSpec);
			byte[] original = cipher.doFinal(Base64.getDecoder().decode(encrypted));

			return new String(original);
		} catch (Exception ex) {
			ex.printStackTrace();
		}

		return null;
	}

	public static void main(String[] args) {
		// key 长度 16 个字节
		String key = "www.netkiller.cn";
		System.out.println(key.length());
		AES aes = new AES(key);
		String en = aes.encrypt("Helloworld!!!");
		String de = aes.decrypt(en);
		System.out.println(en);
		System.out.println(de);

	}
}

```

### DES

```

package cn.netkiller.security;

import java.nio.charset.StandardCharsets;
import java.security.SecureRandom;
import java.util.Base64;

import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;
import javax.crypto.spec.DESKeySpec;

public class DES {

	public DES() {
		// TODO Auto-generated constructor stub
	}

	public static String encrypt(String text, String password) {
		try {
			SecureRandom random = new SecureRandom();
			DESKeySpec desKey = new DESKeySpec(password.getBytes());
			// 创建一个密匙工厂，然后用它把 DESKeySpec 转换成
			SecretKeyFactory keyFactory = SecretKeyFactory.getInstance("DES");
			SecretKey securekey = keyFactory.generateSecret(desKey);
			// Cipher 对象实际完成加密操作
			Cipher cipher = Cipher.getInstance("DES");
			// 用密匙初始化 Cipher 对象
			cipher.init(Cipher.ENCRYPT_MODE, securekey, random);
			// 现在，获取数据并加密
			// 正式执行加密操作

			return Base64.getEncoder().encodeToString(cipher.doFinal(text.getBytes(StandardCharsets.UTF_8)));
		} catch (Throwable e) {
			e.printStackTrace();
		}
		return null;
	}

	private static String decrypt(String text, String password) throws Exception {
		try {
			// DES 算法要求有一个可信任的随机数源
			SecureRandom random = new SecureRandom();
			// 创建一个 DESKeySpec 对象
			DESKeySpec desKey = new DESKeySpec(password.getBytes());
			// 创建一个密匙工厂
			SecretKeyFactory keyFactory = SecretKeyFactory.getInstance("DES");
			// 将 DESKeySpec 对象转换成 SecretKey 对象
			SecretKey securekey = keyFactory.generateSecret(desKey);
			// Cipher 对象实际完成解密操作
			Cipher cipher = Cipher.getInstance("DES");
			// 用密匙初始化 Cipher 对象
			cipher.init(Cipher.DECRYPT_MODE, securekey, random);
			// 真正开始解密操作
			return new String(cipher.doFinal(Base64.getDecoder().decode(text)), StandardCharsets.UTF_8);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}

	public static void main(String[] args) throws Exception {
		// TODO Auto-generated method stub
		String en = DES.encrypt("Helloworld!!!", "www.netkiller.cn");
		String de = DES.decrypt(en, "www.netkiller.cn");
		System.out.println(en);
		System.out.println(de);

	}

}

```

## java.security

### 列出 Java 支持的数字摘要算法

```

package cn.netkiller.security;

import java.security.Security;
import java.util.Set;

public class ListMessageDigest {

	public static void main(String[] args) {

		// List of available MessageDigest Algorithms
		Set<String> messageDigest = Security.getAlgorithms("MessageDigest");
		messageDigest.forEach(x -> System.out.println(x));

	}

}		

```

运行结果

```

SHA3-512
SHA-384
SHA
SHA3-384
SHA-224
SHA-512/256
SHA-256
MD2
SHA-512/224
SHA3-256
SHA-512
MD5
SHA3-224		

```

### 计算文件的 MD5，SHA 等 HASH 值

```

package cn.netkiller.security;

import java.io.FileInputStream;
import java.io.IOException;
import java.security.DigestInputStream;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;

public class sha1sum {

	public sha1sum() {
		// TODO Auto-generated constructor stub
	}

	public static void main(String[] args) throws NoSuchAlgorithmException, IOException {

		String hex = checksum("/etc/hosts");
		System.out.println(hex);
	}

	private static String checksum(String filepath) throws IOException, NoSuchAlgorithmException {

		MessageDigest md = MessageDigest.getInstance("SHA"); // SHA, MD2, MD5, SHA-256, SHA-384...
		// file hashing with DigestInputStream
		try (DigestInputStream dis = new DigestInputStream(new FileInputStream(filepath), md)) {
			while (dis.read() != -1)
				; // empty loop to clear the data
			md = dis.getMessageDigest();
		}

		// bytes to hex
		StringBuilder result = new StringBuilder();
		for (byte b : md.digest()) {
			result.append(String.format("%02x", b));
		}
		return result.toString();

	}

}		

```