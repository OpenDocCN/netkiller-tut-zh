# 第 20 章 Play

https://www.playframework.com/

安装 Play

```
curl -s https://raw.githubusercontent.com/oscm/shell/master/lang/java/framework/play.sh | bash	

```

或者

```

cd /usr/local/src/

wget http://downloads.typesafe.com/typesafe-activator/1.3.2/typesafe-activator-1.3.2-minimal.zip

unzip typesafe-activator-1.3.2-minimal.zip 

mv activator-1.3.2-minimal /srv/
ln -s activator-1.3.2-minimal /srv/activator

cat >> /etc/profile.d/activator.sh <<'EOF'
export PATH=$PATH:/srv/activator
EOF

```

首次运行会下载所需的包

```
# activator
Getting com.typesafe.activator activator-launcher 1.3.2 ...
downloading https://repo.typesafe.com/typesafe/ivy-releases/com.typesafe.activator/activator-launcher/1.3.2/jars/activator-launcher.jar ...
	[SUCCESSFUL ] com.typesafe.activator#activator-launcher;1.3.2!activator-launcher.jar (3004ms)
downloading https://repo1.maven.org/maven2/org/scala-lang/scala-library/2.11.5/scala-library-2.11.5.jar ...
	[SUCCESSFUL ] org.scala-lang#scala-library;2.11.5!scala-library.jar (49602ms)
downloading https://repo.typesafe.com/typesafe/ivy-releases/com.typesafe.activator/activator-props/1.3.2/jars/activator-props.jar ...
	[SUCCESSFUL ] com.typesafe.activator#activator-props;1.3.2!activator-props.jar (2091ms)
downloading https://repo.typesafe.com/typesafe/ivy-releases/com.typesafe.activator/activator-ui-common/1.3.2/jars/activator-ui-common.jar ...
	[SUCCESSFUL ] com.typesafe.activator#activator-ui-common;1.3.2!activator-ui-common.jar (2480ms)
downloading https://repo.typesafe.com/typesafe/ivy-releases/org.scala-sbt/launcher-interface/0.13.8-M5/jars/launcher-interface.jar ...
	[SUCCESSFUL ] org.scala-sbt#launcher-interface;0.13.8-M5!launcher-interface.jar (2489ms)
downloading https://repo.typesafe.com/typesafe/ivy-releases/org.scala-sbt/completion_2.11/0.13.8-M5/jars/completion_2.11.jar ...
	[SUCCESSFUL ] org.scala-sbt#completion_2.11;0.13.8-M5!completion_2.11.jar (6234ms)
downloading https://repo.typesafe.com/typesafe/ivy-releases/com.typesafe.activator/activator-templates-cache/1.0-a0afb008ea619bf9d87dc010156cddffa8a6f880/jars/activator-templates-cache.jar ...
	[SUCCESSFUL ] com.typesafe.activator#activator-templates-cache;1.0-a0afb008ea619bf9d87dc010156cddffa8a6f880!activator-templates-cache.jar (8632ms)
downloading https://repo.typesafe.com/typesafe/ivy-releases/com.typesafe.activator/activator-common/1.0-a0afb008ea619bf9d87dc010156cddffa8a6f880/jars/activator-common.jar ...
	[SUCCESSFUL ] com.typesafe.activator#activator-common;1.0-a0afb008ea619bf9d87dc010156cddffa8a6f880!activator-common.jar (23184ms)
downloading https://repo1.maven.org/maven2/org/scala-lang/modules/scala-xml_2.11/1.0.1/scala-xml_2.11-1.0.1.jar ...
	[SUCCESSFUL ] org.scala-lang.modules#scala-xml_2.11;1.0.1!scala-xml_2.11.jar(bundle) (8692ms)
downloading https://repo1.maven.org/maven2/org/scala-lang/modules/scala-parser-combinators_2.11/1.0.1/scala-parser-combinators_2.11-1.0.1.jar ...
	[SUCCESSFUL ] org.scala-lang.modules#scala-parser-combinators_2.11;1.0.1!scala-parser-combinators_2.11.jar(bundle) (5927ms)
downloading https://repo1.maven.org/maven2/org/apache/lucene/lucene-core/4.3.0/lucene-core-4.3.0.jar ...
	[SUCCESSFUL ] org.apache.lucene#lucene-core;4.3.0!lucene-core.jar (22313ms)
downloading https://repo1.maven.org/maven2/org/apache/lucene/lucene-analyzers-common/4.3.0/lucene-analyzers-common-4.3.0.jar ...
	[SUCCESSFUL ] org.apache.lucene#lucene-analyzers-common;4.3.0!lucene-analyzers-common.jar (12576ms)
downloading https://repo1.maven.org/maven2/org/apache/lucene/lucene-queryparser/4.3.0/lucene-queryparser-4.3.0.jar ...
	[SUCCESSFUL ] org.apache.lucene#lucene-queryparser;4.3.0!lucene-queryparser.jar (6739ms)
downloading https://repo1.maven.org/maven2/com/typesafe/akka/akka-actor_2.11/2.3.9/akka-actor_2.11-2.3.9.jar ...
	[SUCCESSFUL ] com.typesafe.akka#akka-actor_2.11;2.3.9!akka-actor_2.11.jar (14051ms)
downloading https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk/1.3.29/aws-java-sdk-1.3.29.jar ...

```