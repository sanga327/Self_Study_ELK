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

