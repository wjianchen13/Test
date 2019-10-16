# android studio多渠道打包基础配置

![API](https://img.shields.io/badge/API-15%2B-green) ![license](https://img.shields.io/badge/License-Apache%202.0-blue)
android studio使用gradle作为构建工具，这里简单记录一下使用gradle进行多渠道打包的基础知识

## 配置flavorDimensions
这个是配置多维度，必须配置，可以是一个或多个维度
如果只有1个维度，在productFlavors里可以不指定这个维度，如果有多个维度，必须指定是哪个维度
而且如果有多个维度productFlavors里面的配置必须要每个维度都有所指定，比如你配置了2个维度，但是在productFlavors
里面却只使用了一个，则会报错。
flavorDimensions配置如下：
```Groovy
    flavorDimensions "company", "version"
```
## 配置signingConfigs
这个是配置签名文件的，signingConfigs的配置需要在productFlavors前面，否则productFlavors
里面使用signingConfigs会报错
```Groovy
    signingConfigs {
        test1 {
            storeFile file('../jks/test1')
            storePassword "123456"
            keyAlias "key0"
            keyPassword "123456"
        }
        test2 {
            storeFile file('../jks/test2')
            storePassword "123456"
            keyAlias "key0"
            keyPassword "123456"
        }
    }
```
## 配置buildTypes
这里可以理解为编译类型，例如debug，release，当然也可以添加自定义的类型
```Groovy
    buildTypes {
        other {
            debuggable true
            jniDebuggable true
            zipAlignEnabled true
            minifyEnabled false
            shrinkResources false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-project.txt'
        }

        debug {
            debuggable true
            jniDebuggable true
            zipAlignEnabled true
            minifyEnabled false
            shrinkResources false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-project.txt'
        }

        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
```
buildTypes常用配置字段的含义：
debbuggable	否生成一个可调式的apk
minifyEnabled	打开混淆
multiDexEnabled	是否可以分包
proguardFiles	指定插件使用的混淆文件
zipAlignEnabled	是否使用zipAlign优化apk,Android sdk包里面的工具，能够对打包的应用程序进行优化，让整个系统运行的更快
shrinkResources 打开资源压缩

## 配置productFlavors
在这里进行配置多渠道
```Groovy
    productFlavors {
        c_vivo {
            dimension "company"
        }

        v_test1 {
            signingConfig signingConfigs.test1
            dimension "version"
        }

        v_test2 {
            signingConfig signingConfigs.test2
            dimension "version"
        }
    }
```
指定dimension，其中这里面配置的渠道必须要覆盖了flavorDimensions指定的dimension,否则会报错
另外在这里指定各个渠道的签名

这样配置完成之后，就可以点击工具栏上面的sync project按钮同步project了，同步完成后可以再Build Variants里面查看
配置的渠道

## license

    Copyright 2019 wjianchen13

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.





























