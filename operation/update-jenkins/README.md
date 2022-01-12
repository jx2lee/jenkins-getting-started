Jenkins 활용을 위해 version update 를 계획하고 수행한다.

## _Update 계획_
* AS-IS
    * `2.222.4` (2020-05-24)
* TO-BE
    * `2.303.3`
    * ![image.png](image/upgrade-jenkins-001.png)
    * [https://www.jenkins.io/changelog-stable/](https://www.jenkins.io/changelog-stable/) 에서 반응을 확인할 수 있다. (좋은지, 나쁜지, 롤백했는지..)
    * Upgrade Guide 에 의하면 업그레이드 시 LTS release 를 건너뛰는 경우 그 사이의 모든 release 섹션을 읽는 것이 좋다고 한다.
      * https://www.jenkins.io/doc/upgrade-guide/

## _step-by-step 영향_
* 2.235.x
* 2.249.x
* 2.263.x
* 2.277.x
    * [https://www.jenkins.io/doc/upgrade-guide/2.277](https://www.jenkins.io/doc/upgrade-guide/2.277)
    * 확인 사항
      * installation wizard
          * install selected plugins -> none 으로 해결한다.
      * Breaking changes
          * Configuration From Modernization (aka Tables-to-Divs)
          * user interface 개선 때문에 플러그인들도 같이 최신 버전으로 업데이트가 필요하다.
    * 관련 이슈
        * [https://issues.jenkins.io/browse/JENKINS-65581](https://issues.jenkins.io/browse/JENKINS-65581)
    * 권장 절차
        * 업그레이드 전에 사용하지 않는 플러그인을 제거한다.
        * 업그레이드 전에 플러그인을 업데이트 한다.
        * Jenkins 를 업그레이드 한다.
        * 플러그인을 업그레이드 한다.
* 2.289.x
* 2.303.x
    * [https://www.jenkins.io/doc/upgrade-guide/2.303](https://www.jenkins.io/doc/upgrade-guide/2.303)
    * 2.303.1
      * _Default Docker images use Java 11_
        * Docker image 의 Java 태그가 없는 경우 default 버젼 번경 (java8 -> java11)
      * _Plugins that do not support Java 11_
          * Java 11 을 지원하지 않는 플러그인은 jenkins 시작 전 삭제가 필요하다.
          * 플러그인이 java 11 을 지원하는지 확인 & 지원하지 않는다면 삭제한다.
      * _New RPM package manager dependency_
      * _Removed Apache Commons Digester from the Jenkins core_
        * Apache Commons Digester v2.1이 Jenkins 코어에서 삭제되었다.
        * 영향받은 플러그인
          * Visual Studio Code Metrics
          * Blame Subversion 
          * JavaTest report 
          * Visual SourceSafe 
          * Synergy 
          * Config Rotator 
          * Harvest SCM 
          * CMVC
        * 영향을 받아 중지된 플러그인
          * TFS
          * svn-release-mgr 
          * cpptest 
          * CFLint
        * 위 두 항목의 플러그인이 설치되어 있는지 확인하고 영향받은 플러그인이 설치되어 있는 경우 삭제가 필요하다.
      * _Stop bundling External Monitor Job Type, LDAP, and PAM plugins with Jenkins_
      * _Removed JEP-200 compatibility workarounds_
        * 다음 플러그인 중 하나를 사용하는 경우 지정된 버전 이상을 사용해야 한다.
        * Maven Integration 3.1 (released Jan 2018)
        * Job DSL 1.67 (released Jan 2018)
        * Monitoring 1.71.0 (released Feb 2018)
        * Git Client 2.7.1 (released Jan 2018)
        * Pipeline: Supporting APIs 2.17 (released Jan 2018)
        * OWASP Dependency-Check 3.1.1 (released Jan 2018)
      * _Removed jna-posix from Jenkins core_
        * jna-posix dependency 가 Jenkins-core 에서 제거되었다. 아래 플러그인이 설치되어있다면 삭제해야한다.
        * Maven Repository Scheduled Cleanup
        * SICCI for Xcode
        * java.io.tmpdir
      * _Removed JTidy from Jenkins core_
        * JTidy dependency 가 Jenkins 코어에서 제거되었다.
        * JDepend 플러그인 사용자는 최신 버전으로 업그레이드해야 한다.
        * JTidy 기능을 사용하는 다른 플러그인은 Jenkins 코어에 의존하는 대신 JTidy에 대한 dependency 를 명시적으로 선언하도록 업데이트해야 한다.
      * _Removed Bytecode Compatibility Transformer from Jenkins core_
        * Slave Prerequisites 플러그인 및 vertx 플러그인을 포함하여 hudson.model.Queue$Item#id 또는 hudson.model.AbstractProject#triggers 필드에 의존하는 플러그인에 대한 지원이 중단되었다.
        * Jenkins 를 업그레이드하기 전에 이러한 플러그인을 제거해야 한다.
        * hudson.ClassicPluginStrategy.noBytecodeTransformer 시스템 속성을 customize 한 경우 제거해야 한다.
    * 2.303.2
      * _Redefinition of build cause description property_
    * 2.303.3
      * _Agent-to-controller security hardening (role checks)_
      * _Agent-to-controller file path filter security fixes_
      * _Agent-to-controller security fix related to build directory access_
      * 이 버전의 경우 agent <-> controller 노드 간 보안 강화에 의한 패치로 보여진다.

## _Check List_
* 업그레이드 문서에 나와있는 플러그인들이 삭제되었는지 확인이 필요하다.
  * 대부분 삭제를 권하거나 특정 버젼을 사용해야 한다.
  * 삭제가 필요한 플러그인
    * [ ] Java 11 을 지원하지 않는 플러그인
    * [ ] Visual Studio Code Metrics
    * [ ] Blame Subversion
    * [ ] JavaTest report
    * [ ] Visual SourceSafe
    * [ ] Synergy
    * [ ] Config Rotator
    * [ ] Harvest SCM
    * [ ] CMVC
    * [ ] TFS
    * [ ] svn-release-mgr
    * [ ] cpptest
    * [ ] CFLint
    * [ ] Maven Repository Scheduled Cleanup
    * [ ] SICCI for Xcode
    * [ ] java.io.tmpdir
    * [ ] Slave Prerequisites 플러그인
    * [ ] vertx 플러그인을 포함하여 hudson.model.Queue$Item#id hudson.model.AbstractProject#triggers 필드에 의존하는 플러그인
  * 특정 버젼을 사용해야 하는 플러그인 (혹은 업데이트)
    * [ ] Maven Integration 3.1 (released Jan 2018)
    * [ ] Job DSL 1.67 (released Jan 2018)
    * [ ] Monitoring 1.71.0 (released Feb 2018)
    * [ ] Git Client 2.7.1 (released Jan 2018)
    * [ ] Pipeline: Supporting APIs 2.17 (released Jan 2018)
    * [ ] OWASP Dependency-Check 3.1.1 (released Jan 2018)
    * [ ] JDepend
    * [ ] JTidy 기능을 사용하는 다른 플러그인은 Jenkins 코어에 의존하는 대신 JTidy에 대한 dependency 를 명시적으로 선언하도록 업데이트

## _확인 결과_
* 삭제가 필요한 플러그인은 없는 것으로 확인
* 특정 버젼을 사용하거나 사용하지 않은 Plugin 은 없는 것으로 확인
* 테스트
  1. centos8 EOS 로 lts 이미지 사용 시 centos7 으로 설치되는데 이슈는 없는지
  2. ascode 로 구성한 shared library 가 정상적으로 동작하는지
     * 다양한 팀에서 사용하기 때문에 모든 job 에 대한 테스트는 불가능
     * 특별한 job 과 매번 테스트 했던 job 을 그대로 가져와 테스트 예정

## Reference
* [https://www.jenkins.io/doc/upgrade-guide/](https://www.jenkins.io/doc/upgrade-guide/)
* [https://www.jenkins.io/changelog-stable/](https://www.jenkins.io/changelog-stable/)
