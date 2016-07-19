## Tinker
[![license](http://img.shields.io/badge/license-apache_2.0-red.svg?style=flat)](http://git.code.oa.com/tinker/tinker/blob/master/LICENSE)

Tinker is a hot-fix solution library for Android, it supports dex, library and resources update without reinstall apk.

![tinker.png](assets/tinker.png) 

## Getting started
Add tinker-gradle-plugin as a dependency in your main `build.gradle` in the root of your project:

```gradle
buildscript {
    dependencies {
        classpath ('com.tencent.tinker:tinker-patch-gradle-plugin:1.2.0')
    }
}
```

Then you need to "apply" the plugin and add dependencies by adding the following lines to your `app/build.gradle`.

```gradle
dependencies {
	//Optional, help to gen the final application 
	compile('com.tencent.tinker:tinker-android-anno:1.2.0')
    //tinker's main Android lib
    compile('com.tencent.tinker:tinker-android-lib:1.2.0') 
}
...
...
apply plugin: 'com.tencent.tinker.patch'
```

If your app has a class that subclasses android.app.Application, then you need to modify that class, and move all its implements to DefaultApplicationLifeCycle rather than Application:

```java
-public class YourApplication extends Application {
+public class SampleApplicationLifeCycle extends DefaultApplicationLifeCycle 
```

Now you should change your `Application` class, which will be a subclass of [TinkerApplication](http://git.code.oa.com/tinker/tinker/blob/master/tinker-android/tinker-android-loader/src/main/java/com/tencent/tinker/loader/app/TinkerApplication.java). As you can see from its API, it is an abstract class that does not have a default constructor, so you must define a no-arg constructor as follows:

```java
public class SampleApplication extends TinkerApplication {
    public SampleApplication() {
      super(
        //tinkerFlags, which types is supported
        //dex only, library only, all support
        TinkerApplication.TINKER_ENABLE_ALL,
        // This is passed as a string so the shell application does not
        // have a binary dependency on your ApplicationLifeCycle class. 
        "tinker.sample.android.SampleApplicationLifeCycle");
    }  
}
```

Use `tinker-android-anno` to generate your `Application` is more recommended, you can just add an annotation for your [ApplicationLifeCycle](http://git.code.oa.com/tinker/tinker/blob/master/tinker-sample-android/app/src/main/java/tinker/sample/android/SampleApplicationLifeCycle.java) class

```java
@DefaultLifeCycle(
application = ".SampleApplication",                       //application name to generate
flags = TinkerApplication.TINKER_ENABLE_ALL)              //tinkerFlags above
public class SampleApplicationLifeCycle extends DefaultApplicationLifeCycle 
```

How to install tinker? learn more at the sample [SampleApplicationLifeCycle](http://git.code.oa.com/tinker/tinker/blob/master/tinker-sample-android/app/src/main/java/tinker/sample/android/SampleApplicationLifeCycle.java).

for proguard, we have already change the proguard config automatic, and also generate the multiDex keep proguard file for you.

for more tinker plugin configurations, learn more at the sample [app/build.gradle](http://git.code.oa.com/tinker/tinker/blob/master/tinker-sample-android/app/build.gradle).
## Support
Any problem?

1. learn more from [tinker-sample-android](http://git.code.oa.com/tinker/tinker/tree/master/tinker-sample-android).
2. read the [source code](http://git.code.oa.com/tinker/tinker/tree/master).
3. read the [wiki](http://git.code.oa.com/tinker/tinker/wikis/home) or [FAQ](http://git.code.oa.com/tinker/tinker/wikis/faq) for help.
4. contact shwenzhang for help.

## License
    Copyright (C) 2016 Tencent Wechat, Inc.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.