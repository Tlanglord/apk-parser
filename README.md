A pure java apk parser, to read encoded xml file and get apk infos, with no need for appt/dexdump binaries.

#### Features
For now, the following is (partially) supported:
* resource table by ResourceTableParser
* binary xml file by BinaryXmlParser
* dex file by DexParser
* certificate by CertificateParser

#### Maven
If maven is used, you can add dependency:
```xml
<dependency>
    <groupId>net.dongliu</groupId>
    <artifactId>apk-parser</artifactId>
    <version>1.5.0</version>
</dependency>
```
to your pom file.

#### Usage
The easiest way is to use the ApkParser class, which contains convenient methods to get AndroidManifest.xml, apk meta infos, etc.
```java
ApkParser apkParser = new ApkParser(new File(filePath));
String xml = apkParser.getManifestXml();
System.out.println(xml);
ApkMeta apkMeta = apkParser.getApkMeta();
System.out.println(apkMeta);
Set<Locale> locales = apkParser.getLocales();
for (Locale l : locales) {
    System.out.println(l);
}
apkParser.close();
```

#### Locales
Apk may appear different infos(title, icon, etc.) for different region and language, which is determined by Locales.
If locale is not set, the default Locale of of OS is used. You can set locale like this:
```java
ApkParser apkParser = new ApkParser(new File(filePath));
apkParser.setPreferredLocale(Locale.ENGLISH);
ApkMeta apkMeta = apkParser.getApkMeta();
```
The PreferredLocale parameter work for getApkMeta, getManifestXml, and other binary xmls.
Apk parser will find best match languages with locale you specified.

If locale is set to null, ApkParser will not translate resource tag, just give the resource id.
For example, apk title will be '@string/app_name' instead of 'WeChat'.