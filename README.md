# ELK

> ELK: ElasticSearch, Logstash, Kibana

---

- 참고 사이트

  - [Elastic 공식 사이트](https://www.elastic.co/kr/elasticsearch/)

  - [Elastic 가이드북](https://esbook.kimjmin.net/)

  - [유용한 자료 모음](https://docs.google.com/document/d/1XJsI2mHlihP8sOKJRn_MgMY4STovOnd4IlaAkwyxbys/edit)

  - [Youtube 강의](https://www.youtube.com/watch?v=Ks0P49B4OsA)

  - [자주 사용되는 리눅스& vi 명령어](https://docs.google.com/document/d/1kv4VlXsF1fMvfZRx-0r7hADNI2Bitx7b9DAdSI0DFCY/edit)

    

---

### 0. 환경설정

**1) GCP 인스턴스 구성**

- Google Cloud Platform
  - VM 인스턴스 생성
  - SSH 접속

**2) 터미널 도구에서 SSH를 이용한 GCP VM 접속**

- 인증 키 생성
  - ssh-keygen을 이용한 인증 키 파일 생성
  - rsa 공개 키 파일 생성
- 원격 터미널로 GCP 접속
  - GCP metadata에 ssh 정보 등록
  - 터미널 프로그램에서 ssh -i ...명령으로 GCP VM 접속

**3) 자주 사용하는 리눅스, vi 명령어**

---

### 1. ElasticSearch 환경설정

**1) ElasticSearch 다운로드 및 실행**

- elasticsearch 다운로드 및 압축 해제
- :9200(http), :9300(tcp) 사용 포트 설명
- bin/elasticsearch를 이용한 실행
- config/elasticsearch.yml 설정 파일 설명
- curl 명령을 이용한 프로세스 실행 확인
- start.sh, stop.sh 시작/ 종료 스크립트 만들기



**2) Elastic 환경 설정**

- jvm.options 파일에서 Java 힙메모리 설정
- elasticsearch.yml 환경 설정 파일
  - YAML 문법 설명
  - cluster.name : 클러스터명
  - node.name : 노드명
  - path.data, path.log : 데이터, 로그 저장 경로
- 실행 명령에서 -E 옵션으로 환경 설정



**3) ElasticSearch 클러스터와 노드 바인딩**

- elasticsearch 클러스터 구성도
  - 여러 서버에서 각 노드 실행
  - 하나의 서버에서 여러 노드 실행
- 다수의 노드가 차례대로 바인딩되는 과정 데모
- 노드 디스커버리 과정 설명



**4) network.host 설정과 부트스트랩 체크**

- 클러스터 구성을 위한 네트워크 설정
  - network.host: [ "_local_", "_site_" ]
  - discovery.seed_hosts: [ {호스트명 | ip주소} ]
  - cluster.initial_master_nodes: [ {마스터노드명} ]
- max file descriptors 설정
- max virtual memory areas 설정
- Google Cloud Platform 설정
  - 인스턴스 태그 설정
  - http, tcp 통신을 위한 방화벽 포트 설정

**5) elasticsearch 클러스터 구성**

- 클러스터 아키텍쳐 구성도
- 추가 노드를 위한 GCP 인스턴스 생성
    - VM 이미지 스냅샷을 이용
    - 클러스터링을 위한 방화벽 태그 설정
    - /etc/hosts 에서 호스트명 설정
- 3개 노드로 클러스터 구성
    - 실행 로그에서 바인딩 되는 과정 확인
    - _ cat/nodes 명령으로 구성된 노드 정보 확인

**6-1) 클러스터 보안 설정 (1): 노드간 통신에 TLS 적용**

- elasticsearch.yml 에서 보안 기능 설정
    - xpack.security - 보안 기능 활성
    - transport.ssl - 노드들간의 통신에 TLS 보안 적용
- 암호화 / 복호화 키 생성
    - 키를 만들기 위해 elastic 에서 제공하는 도구
        elasticsearch-certutil 사용
    - PKCS#12 (.p12) 방식의 키 저장소 생성
    - 키 저장소를 이용한 private 키 생성
- elasticsearch-keystore 사용
    - elasticsearch.yml 에 입력할 수 없는 민감한 환경변수 값 (패스워드 등)을 저장하기 위한 도구


**6-2) 클러스터 보안 설정 (2): 사용자 - 암호 설정**

- node-2, node-3 노드에 보안 적용
    - node-1에서 만든 키 파일을 scp 명령을 통해 전송
    - elasticsearch-keystore 등을 이용하여 node-1과 동일하게 보안 설정 적용
- user / password 설정
    - elastic-setup-password 기능을 이용하여 사용자 계정/ 암호 정보를 클러스터 설정에 저장 (Native Realm)
    - elasticsearch-users 기능을 이용하여 사용자 계정/암호 정보를 파일에 저장 (File Realm)

**7-1) Kibana 시작하기: 설치 및 실행**

- 전체 클러스터 아키텍쳐 리뷰
- Kibana 환경 설정
    - server.host: KB 네트워크 정보
    - server.name: KB 프로세스 네임
    - elasticsearch.hosts: ES 클러스터 접속 정보
    - kibana-keystore를 이용한 ES 사용자/암호 입력
- GCP 방화벽 설정
    - KB 접속 포트 (:5601) 오픈
- GCP 인스턴스 다시 생성
    - 문제 원인 파악과 해결을 위해 진행한 과정 설명
- Kibana UI 에서 새로운 사용자 생성

**7-2) Kibana 시작하기: pm2를 이용한 데몬 실행**

- ES를 백그라운드로 실행하는 방법 복습
- Kibana를 시작하는 다양한 방법
    - bin/kibana(.bat) 스크립트 실행
    - node.js를 이용하여 src/cil/cil.js 실행
- pm2를 이용해서 kibana를 데몬으로 실행하기
    - nvm을 이용한 노드 버전 관리
    - pm2 설치 및 실행 옵션 설명
- kibana를 데몬으로 실행/종료 하기 위한 스크립트 파일 생성



