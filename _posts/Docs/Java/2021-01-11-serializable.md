<!-- ---
layout: post
title: Serializable
category:
    - docs
    - java
description: >
    Java Serializable의 종류와 사용 이유에 대해
--- -->
<!-- blank -->
* toc
{:toc}

자바 시스템 내부에서 사용되는 객체 또는 데이터를 외부의 자바 시스템에서도 사용할 수 있도록 바이트 형태로 데이터 변환하는 기술과 바이트로 변환된 데이터를 다시 객체로 변환하는 기술(역직렬화)을 아울러 이야기한다.  

시스템적으로 이야기하자면 JVM(Java Virtual Machine 이하 JVM)의 메모리에 상주(힙 또는 스택)되어 있는 객체 데이터를 바이트 형태로 변환하는 기술과 직렬화된 바이트 형태의 데이터를 객체로 변환해서 JVM으로 상주시키는 형태를 같이 이야기한다.

* 기본(Primitive) 타입과 java.io.Serializable 인터페이스를 상속받은 객체는 직렬화할 수 있는 기본 조건을 가진다.
* 자바 직렬화 대상 객체는 동일한 serialVersionUID를 가지고 있어야 한다.


## 종류 

### 문자열 형태의 직렬화
* 범용적인 API나 데이터를 변환하여 추출할 때 많이 사용
* 표 형태의 다량의 데이터 직렬화 시 CSV가 많이 쓰임
* 구조적인 데이터는 이전에는 XML를 많이 사용했으며 최근에는 JSON형태를 많이 사용

#### CSV
* 데이터를 표현하는 가장 많이 사용되는 방법 중 하나
* 콤마 기준으로 데이터를 구분

#### JSON
* 최근에 가장 많이 사용하는 포맷
* 자바 스크립트에서 쉽게 사용 가능
* 다른 데이터 포맷 방식에 비해 오버헤드가 적다.

### 이진 직렬화
* 데이터 변환 및 전송 속도에 최적화
* 전송 방법에 대한 부분도 이야기하고 있지만 직렬화 부분만

#### Protocol Buffer
* 구글에서 제안한 플랫폼 독립적인 데이터 직렬화 플랫폼
* ....

## 자바 직렬화 사용 이유
* CSV, JSON, 프로토콜 버퍼 등은 시스템의 고유 특성과 상관없는 대부분의 시스템에서의 데이터 교환 시 많이 사용
* 자바 직렬화 형태의 데이터 교환은 자바 시스템 간의 데이터 교환을 위해서 존재

정답은 없다. 목적에 따라 적절하게 써야 한다.
{:.note title="그럼 자바에서도 CSV, JSON을 쓰지?"}

#### 장점
* 복잡한 데이터 구조를 가진 클래스의 객체라도 직렬화 기본 조건만 지키면 큰 작업 없이 바로 직렬화가 가능
* 역직렬화도 마찬가지
* 데이터 타입이 자동으로 맞춰지기 떄문에 관련 부분 신경쓰지 않아도 됨
* 역직렬화가 되면 기존 객체처럼 바로 사용 가능
  
### 언제, 어디서 사용할까?
* JVM의 메모리에서만 상주되어있는 객체 데이터를 그대로 영속화(Persistence)가 필요할 때 사용
* 시스템이 종료되도 없어지지 않는 장점, 네트워크로 전송도 가능

#### Servlet Session
* 서블릿 기반의 WAS(톰캣, 웹로직 등)들은 대부분 세션의 자바 직렬화를 지원
* 단순히 세션을 서블릿 메모리 위에서 운용한다면 직렬화를 필요로 하지 않지만
* 파일로 저장하거나 세션 클러스터링, DB를 저장하는 옵션 등을 선택하면 세션 자체가 직렬화가 되어 저장되어 전달

#### 캐시
* 자바 시스템에서 퍼포먼스를 위해 캐시(Ehcache, Redis, Memcached, ...) 라이브러리, 시스템을 많이 사용
* 예를 들어 DB를 조회한 후 가져온 데이터 객체 같은 경우
* 실시간 형태로 요구하는 데이터가 아니라면 메모리, 외부 저장소, 파일 등을 저장소를 이용해서 데이터 객체를 저장한 후 동일한 요청이 오면 DB를 다시 조회하는 것이 아니라 저장된 객체를 찾아서 응답하게 하는 형태
* 캐시할 부분을 자바 직렬화된 데이터를 저장해서 사용
* 사실 직렬화만을 사용하진 않지만.. 가장 간편하기 때문에 많이 사용

## Reference
<https://woowabros.github.io/experience/2017/10/17/java-serialize.html>