---
title: "maven deploy 到 archiva 上面的相关配置"
date: 2014-10-31 11:41:27 +0800
categories: java
tags: [tool, java]
---

## ~/.m2/settings.xml文件的配置

### 配置deploy时的用户名，密码

id必需与distributionManagement下的id匹配

``` xml
<servers>
	<server>
		<id>snapshots</id>
		<username>username</username>
		<password>password</password>
	</server>
	<server>
		<id>internal</id>
		<username>username</username>
		<password>password</password>
	</server>
</servers>
```

<!-- more -->
### 配置下载的mirror地址

snapshots的mirrorOf必需通过profile特别配置

``` xml
<mirrors>
	<mirror>
		<id>archiva.internal</id>
		<url>http://localhost:8080/repository/internal</url>
		<mirrorOf>*</mirrorOf>
	</mirror>
	<mirror>
		<id>archiva.snapshots</id>
		<url>http://localhost:8080/repository/snapshots</url>
		<mirrorOf>snapshots</mirrorOf>
	</mirror>
</mirrors>
```

### 配置mirrorOf所需的profile

``` xml
<profiles>
	<profile>
		<activation>
			<activeByDefault>true</activeByDefault>
		</activation>
		<repositories>
			<repository>
				<id>internal</id>
				<name>Archiva Managed Internal Repository</name>
				<url>http://localhost:8080/repository/internal</url>
				<releases>
					<enabled>true</enabled>
				</releases>
				<snapshots>
					<enabled>false</enabled>
				</snapshots>
			</repository>
			<repository>
				<id>snapshots</id>
				<name>Archiva Managed Snapshots Repository</name>
				<url>http://localhost:8080/repository/snapshots</url>
				<releases>
					<enabled>false</enabled>
				</releases>
				<snapshots>
					<enabled>true</enabled>
				</snapshots>
			</repository>
		</repositories>
	</profile>
</profiles>
```

## 项目的pom.xml文件配置

``` xml
<project>
	<distributionManagement>
		<repository>
			<id>internal</id>
			<name>Archiva Managed Internal Repository</name>
			<url>http://localhost:8080/repository/internal</url>
		</repository>
		<snapshotRepository>
			<id>snapshots</id>
			<name>Archiva Managed Snapshot Repository</name>
			<url>http://localhost/repository/snapshots</url>
		</snapshotRepository>
	</distributionManagement>
</project>
```
上面配置了release和snapshots两类version的上传和下载；version以`-SNAPSHOT`(必需是大写)结尾的是snapshots的的version;
