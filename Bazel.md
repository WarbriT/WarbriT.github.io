

# Bazel 정리 


## Build System 전반적인 내용 정리


### CMake, Meson, Make, Ninja


turing-complete, non-turing complete

overview + Feature + examples simple 

Make : shell script : turing complete

CMake - Meson - Cson

CMake : Make 의 의존성을 모두 관리하지 않아도, 알아서 기록되게. 추상화. 

즉, 최종 결과물과 인풋만 주면 알아서 하게끔 함. 

예제 [https://www.tuwlab.com/ece/27234](https://www.tuwlab.com/ece/27234)

Meson : non turing complete DSL

[https://mesonbuild.com/GuiTutorial.html](https://mesonbuild.com/GuiTutorial.html)

쉽다.  

ninja 이용

scons

[https://github.com/SCons/scons-examples/blob/master/shared-lib-program/SConstruct](https://github.com/SCons/scons-examples/blob/master/shared-lib-program/SConstruct)

파이썬 기반. 느리다. 쉽다. 

webUI


### Bucks

bazel 에 영향을 많이 받음. 

[https://github.com/airbnb/BuckSample/blob/master/BuckLocal/BUCK](https://github.com/airbnb/BuckSample/blob/master/BuckLocal/BUCK)

Pants

[https://www.pantsbuild.org/2.18/docs/introduction/how-does-pants-work](https://www.pantsbuild.org/2.18/docs/introduction/how-does-pants-work)

거의 비슷하다. 

remote execution 존재. 


## Bazel 정리


### cache difference

make cache != bazel cache

Make cache : by Last-Modified Timestamps

([https://web.archive.org/web/20160813235106/http://www.conifersystems.com/whitepapers/gnu-make/](https://web.archive.org/web/20160813235106/http://www.conifersystems.com/whitepapers/gnu-make/))

Ninja cache : by Last-Modified Timestamps

(https://github.com/ninja-build/ninja/issues/1394)

Maven cache : digest based , 

  but slow and completely incremental. why? task based! hard to find

important how to make key. input, Output, plugin

([https://maven.apache.org/extensions/maven-build-cache-extension/](https://maven.apache.org/extensions/maven-build-cache-extension/))

opt in feature

[https://en.wikipedia.org/wiki/Bazel_(software)](https://en.wikipedia.org/wiki/Bazel_(software))

bazel cache : default feature, hash based: content digest

gradle cache : default feature, hash based: content digest

Q: Greenwich?


### Bazel’s ways to increase performance build 


#### distributed build. 

with remote build(execution)

TODO

[https://google-engtools.blogspot.com/2011/09/build-in-cloud-distributing-build-steps.html](https://google-engtools.blogspot.com/2011/09/build-in-cloud-distributing-build-steps.html)

4개 시리즈 전부 읽을 것. 

bazel 블로그 .docs읽을 것. 


#### Google 의 Build 처리 과정

총 4편의 글, 그리고 강의가 있습니다. [^1][^2]


#### Caching

remote caching


#### Build without the bytes?

bazel 7.0 feature


#### local repository?


#### Bazel grpc


#### Bazel basic concepts


#### Bazel BzlMod

transitive 안써도 되는지?

글 읽고 정리해보기

diamond collision 그래서 어떻게 해결했다는 건지? shaded?


#### rule 짜보기


## Gradle 정리

gradle 은 cache 시스템 2017년에 제공 시작. 

gradle, remote execution? distributed execution?

현재 제공하고 있지 않음. [^3]

[https://blog.gradle.org/remote-and-distributed-build-patterns](https://blog.gradle.org/remote-and-distributed-build-patterns)

remote execution vs distributed execution  미묘한 개념 차이가 있는데


<!-- Footnotes themselves at the bottom. -->
## Notes

[^1]:
     https://google-engtools.blogspot.com/2011/06/build-in-cloud-accessing-source-code.html

[^2]:
     https://www.youtube.com/watch?v=b52aXZ2yi08

[^3]:
     [https://blog.gradle.org/remote-and-distributed-build-patterns](https://blog.gradle.org/remote-and-distributed-build-patterns)
