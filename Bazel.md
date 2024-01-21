# Bazel 정리 


---


# Build System 전반적인 내용 정리


## CMake, Meson, Make, Ninja



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline drawings not supported directly from Docs. You may want to copy the inline drawing to a standalone drawing and export by reference. See <a href="https://github.com/evbacher/gd2md-html/wiki/Google-Drawings-by-reference">Google Drawings by reference</a> for details. The img URL below is a placeholder. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![drawing](https://docs.google.com/drawings/d/12345/export/png)

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


## Bucks

bazel 에 영향을 많이 받음. 

[https://github.com/airbnb/BuckSample/blob/master/BuckLocal/BUCK](https://github.com/airbnb/BuckSample/blob/master/BuckLocal/BUCK)

Pants

[https://www.pantsbuild.org/2.18/docs/introduction/how-does-pants-work](https://www.pantsbuild.org/2.18/docs/introduction/how-does-pants-work)

거의 비슷하다. 

remote execution 존재. 


---


# Bazel 정리


## cache difference

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


## Bazel’s ways to increase performance build 


### distributed build. 

with remote build(execution)

TODO

[https://google-engtools.blogspot.com/2011/09/build-in-cloud-distributing-build-steps.html](https://google-engtools.blogspot.com/2011/09/build-in-cloud-distributing-build-steps.html)

4개 시리즈 전부 읽을 것. 배경을 다루었음. 배경.. 구글의 빌드 artifacts 는 하나에 1G 까지 됨. 

bazel 블로그 .docs읽을 것. 


### Google 의 Build 처리 과정

총 4편의 글, 그리고 강의가 있습니다. [^1][^2]


### Caching

remote caching


### Build without the bytes?

bazel 7.0 feature


### local repository?


### Bazel grpc


### Bazel basic concepts


### Bazel BzlMod

transitive 안써도 되는지?

글 읽고 정리해보기

diamond collision 그래서 어떻게 해결했다는 건지? shaded?


### rule 짜보기


---


# Gradle 정리


## Remote System 글 요약[^3]


### 앞 내용들

gradle 은 cache 시스템 2017년에 제공 시작. 

gradle, remote execution? distributed execution?

현재 제공하고 있지 않음.  그렇지만 다른 툴들이 존재함.

[https://blog.gradle.org/remote-and-distributed-build-patterns](https://blog.gradle.org/remote-and-distributed-build-patterns)

remote execution vs distributed execution  미묘한 개념 차이가 있는데, 정리해 보자. 

 First of all, Both are designed for Performance. 

Remote build : 원격으로 build

remote execution : 원격으로 연결. vs code - remote IDE, IntelliJ IDEA, Fleet

Cloud Development Environment : 빌드를 로컬에서 할 필요가 없음. 중앙 집중형. : vs code 로 뭐 있었던 것 같은디? Github codespaces, JetBrains Sapce, Gitpod

remote build 에서 remote execution, cloud development Environment

distributed build : 분산 build , 앞에서 다루었던 것들은 single remote machine. distributed build 는 multi machine 개념임. 

**Don’t forget about the backing infrastructure needs of distributed builds. **

**A build could actually be slower if sufficient remote build agents are not available.**

구글의 system 은 어떻게 되어 있는가? 

구글 엔지니어는 이렇게 일한다에서 참고. 

gradle 에서는 build, 특히 test build 에 대하여 다음 4단계로 단계를 나누어서 구분하고 있음. [^4]



1. **[Build Cache](https://gradle.com/gradle-enterprise-solutions/build-cache/)**. Avoid unnecessarily running components of builds and tests when inputs have not changed.
2. **[Predictive Test Selection](https://gradle.com/gradle-enterprise-solutions/predictive-test-selection/)**. Run only tests that are likely to provide useful feedback using machine learning.
3. **[Test Distribution](https://gradle.com/gradle-enterprise-solutions/test-distribution/)**. Run the necessary and relevant remaining tests in parallel to minimize build time.
4. **[Performance Continuity](https://gradle.com/gradle-enterprise-solutions/performance-continuity/)**. Sustain Test Distribution and other performance improvements over time with data analytic and performance profiling capabilities.



no parallelism - single machine parallelism(자원 공유 문제) - CI Fanout - Modern test distribution(TD)


### CI Fanout

build 를 여러 CI jobs 로 나누는 것. manually configured 되어야 함. 그리고 CI platform 마다 unique 하게 할당이 되어야 함. 


### Test Distribution

특히 tests 가 JVM ecosystem 상에서 빌드 시간을 느리게 만드는 가장 큰 원인임. 

gradle 상용화. 테스트를 나누어서 수행함. 

로컬, ci 모두 가능. 


### General distribution

challenges : 환경 격리 + overhead time 

bazel, pants

challenges : 어떻게 빌드를 split 할 것인지

언어에 따라서 성능 차이가 클 수 있음. 


### Common Factors of Remote and Distributed Builds

Network Traffic : serializing, Network proximity ( hosts, storage)

Managing the pool of remote hosts or distributed agents


### bazel 과의 비교 글들

gradle 이 bazel 과 비교해놓은 글들이 있음.[^5] [^6] 

TODO 읽기. 


## Gradle work

어떻게 돌아가는지에 대한 글임. [^7]

TODO 읽기. 3편. 


<!-- Footnotes themselves at the bottom. -->
## Notes

[^1]:
     https://google-engtools.blogspot.com/2011/06/build-in-cloud-accessing-source-code.html

[^2]:
     https://www.youtube.com/watch?v=b52aXZ2yi08

[^3]:
     [https://blog.gradle.org/remote-and-distributed-build-patterns](https://blog.gradle.org/remote-and-distributed-build-patterns)

[^4]:
     https://gradle.com/gradle-enterprise-solutions/test-distribution/#advantages-section

[^5]:
     https://blog.gradle.org/our-approach-to-faster-compilation

[^6]:
     https://blog.gradle.org/gradle-vs-bazel-jvm#the-granularity-of-build-files

[^7]:
     https://blog.gradle.org/how-gradle-works-3
