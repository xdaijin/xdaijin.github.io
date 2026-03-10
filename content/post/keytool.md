---
title: "Keytool使用教程"
summary: "如何使用keytool进行证书管理"
keywords: "keytool"

date: 2026-03-10T23:21:25+08:00
lastmod: 2026-03-10T23:21:25+08:00

math: false
mermaid: false

categories:
  - secure
tags:
  - keytool
---

keytool 是 Java 开发工具包 JDK1.4 之后引入的一个命令行工具，用于管理和生成 密钥对 、数字证书 以及管理 密钥库。它主要用于安全通信和身份验证，通过使用公钥/私钥对和相关证书实现自我认证。

keytool 将密钥和证书存储在所谓的 "密钥库" 中，这是一种安全的存储设施，可以保护敏感信息免受未经授权的访问。

## keytool命令

```BASH
命令:

 -certreq            生成证书请求
 -changealias        更改条目的别名
 -delete             删除条目
 -exportcert         导出证书
 -genkeypair         生成密钥对
 -genseckey          生成密钥
 -gencert            根据证书请求生成证书
 -importcert         导入证书或证书链
 -importpass         导入口令
 -importkeystore     从其他密钥库导入一个或所有条目
 -keypasswd          更改条目的密钥口令
 -list               列出密钥库中的条目
 -printcert          打印证书内容
 -printcertreq       打印证书请求的内容
 -printcrl           打印 CRL 文件的内容
 -storepasswd        更改密钥库的存储口令
 -showinfo           显示安全相关信息
 -version            输出程序版本
```

## 生成自签证书

```BASH
keytool -genkey -alias test（别名） 
-keypass 123123（私钥密码） 
-keyalg RSA（算法） 
-sigalg sha256withrsa（算法小类） 
-keysize 1024（密钥长度） 
-validity 365（有效期）
-keystore test.jks（生成密钥库文件） 
-storepass 123123（主密码）
```

执行后，会提示输入：

```BASH
What is your first and last name?
  [Unknown]:  www.test.com
What is the name of your organizational unit?
  [Unknown]:  Dev Team
What is the name of your organization?
  [Unknown]:  MyCompany
What is the name of your City or Locality?
  [Unknown]:  Beijing
What is the name of your State or Province?
  [Unknown]:  Beijing
What is the two-letter country code for this unit?
  [Unknown]:  CN
Is CN=Zhang San, OU=Dev Team, O=MyCompany, L=Beijing, ST=Beijing, C=CN correct?
  [no]:  yes
```

默认参数

```BASH
-alias "mykey"
 
-keyalg
    "DSA" (when using -genkeypair)
    "DES" (when using -genseckey)
 
-keysize
    2048 (when using -genkeypair and -keyalg is "RSA")
    2048 (when using -genkeypair and -keyalg is "DSA")
    256 (when using -genkeypair and -keyalg is "EC")
    56 (when using -genseckey and -keyalg is "DES")
    168 (when using -genseckey and -keyalg is "DESede")
 
-validity 90
  
-keystore <the file named .keystore in the user's home directory>

-destkeystore <the file named .keystore in the user's home directory>
   
 -storetype <the value of the "keystore.type" property in the
    security properties file, which is returned by the static
    getDefaultType method in java.security.KeyStore>
 
-file
    stdin (if reading)
    stdout (if writing)
 
-protected false
```

## 查看证书

```BASH
keytool -list -keystore test.jks
```

查看带base64证书内容的

```BASH
keytool -list -rfc -keystore test.jks
```

查看更详细的

```BASH
keytool -list -v -keystore test.jks
```

## 证书请求CSR和签发证书

生成一个证书请求发给证书签发单位

```BASH
keytool -certreq -alias mykey -sigalg SHA256withRSA -file certreq.csr -keystore test.jks
```

然后将certreq发给证书签发单位

假如收到一个csr，通过下面命令签发证书

```BASH
 keytool -gencert -infile certreq.csr -outfile test.crt -alias mykey -keystore test.jks
```

生成的test.crt即为证书

## 导入证书

```BASH
keytool -importcert -file test.crt -alias www.test.com -keystore test.jks
```

## 删除证书

```BASH
keytool -delete -alias www.test.com -keystore test.jks
```

## 修改证书别名

```BASH
keytool -changealias -alias mykey -destalias www.test.com -keystore test.jks
```

## 导出证书

```BASH
keytool -exportcert -alias www.test.com -file test.crt -keystore test.jks
```

## 转换pkcs12格式

```BASH
keytool -importkeystore -srckeystore test.jks -srcalias www.bo.org -destkeystore test.p12 -deststoretype pkcs12
```

## FAQ

### jks vs pkcs12

jks是java特有的证书库格式，pkcs12是通用的证书库格式

### PrivateKeyEntry vs trustedCertEntry

PrivateKeyEntry包含证书和私钥，trustedCertEntry只包含证书，而证书里面只有公钥

### genkeypair vs genseckey

genkeypair用于生成公私钥对，genseckey用于生成对称秘钥
