![post-thumbnail](https://velog.velcdn.com/images/soyeon207/post/b01d1980-1eaa-4e62-9d34-cd2b1bc54d7c/image.png)

> QueryDSL에 대해서 더 알고 싶은 사람은 이전 글인 [👉 **[QueryDSL] QueryDSL 이란 ?**](https://velog.io/@soyeon207/QueryDSL-%EC%9D%B4%EB%9E%80) 을 참고해주세요.

그러면 본격적으로 Spring Boot 에서 QueryDSL JPA 를 사용하려면 어떻게 사용하면 되는지 차근차근 알아보자 !

## build.gradle 에 QueryDSL 세팅하기

일단 budile.gradle 에 QueryDSL 관련 코드를 세팅해줘야 한다.

추가해야할 전체 코드는 다음과 같다.

```java
dependencies {
	... 
	implementation 'com.querydsl:querydsl-jpa'
	implementation 'com.querydsl:querydsl-apt'

	annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa"
	annotationProcessor 'jakarta.persistence:jakarta.persistence-api'
	annotationProcessor 'jakarta.annotation:jakarta.annotation-api'
}

def querydslSrcDir = 'src/main/generated'
sourceSets {
  main {
    java {
      srcDirs += [ querydslSrcDir ]
    }
  }
}

compileJava {
    options.compilerArgs << '-Aquerydsl.generatedAnnotationClass=javax.annotation.Generated'
}

tasks.withType(JavaCompile) {
	options.generatedSourceOutputDirectory = file(querydslSrcDir)
}

clean {
  delete file(querydslSrcDir)
}
```

하나 하나씩 뜯어보면서 코드를 더 자세하게 파악해보자.

### dependencies

```java
implementation 'com.querydsl:querydsl-jpa'
implementation 'com.querydsl:querydsl-apt'

annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa"
annotationProcessor 'jakarta.persistence:jakarta.persistence-api'
annotationProcessor 'jakarta.annotation:jakarta.annotation-api'
```

`implementation 'com.querydsl:querydsl-jpa'`  
→ QueryDSL 을 사용하기 위한 라이브러리  
→ QuerydslRepositorySupport, QuerydslPredicateExecutor 등 QueryDSL 내부 클래스는 사용 가능하지만, 실제로 쿼리를 위해 사용되는 QClass 는 생성되지 않는다.

---

`implementation 'com.querydsl:querydsl-apt'`  
→ QClass 를 생성하기 위한 라이브러리

---

`annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa"`

→ QueryDSL JPA 의 경우에는 Q 클래스를 활용해서 동작하는데 @Entity 어노테이션을 선언한 클래스를 탐색하고, Q 클래스를 생성한다.

Q 클래스를 생성하는 방법에는 크게 두가지로 나뉜다.

① com.ewerk.gradle.plugins.querydsl  
: 2018년 이후 업데이트 된 적이 없다.  
: gradle 버전이 계속적으로 업데이트되면서 해당 플러그인 외 여러가지 설정해줘야 할 것 들이 많다.

② annotationProcessor

이번에는 ewerk 관련 단점들 때문에 두번째 방식인 annotationProcessor 를 사용한다.

---

`annotationProcessor 'jakarta.persistence:jakarta.persistence-api'`  
`annotationProcessor 'jakarta.annotation:jakarta.annotation-api'`

이클립스 재단으로 자바가 이관되면서 java → jakarta 로 이름이 변경되었다.

Q 파일을 찾지 못해서 발생하는 오류인 java.lang.NoClassDefFoundError (javax.annotation.Entity / javax.annotation.Generated) 에러 발생 대응을 위한 코드 이다.

### sourceSets

```java
def querydslSrcDir = 'src/main/generated'
sourceSets {
  main {
    java {
      srcDirs += [ querydslSrcDir ]
    }
  }
}
```

gradle build 시 QClass 소스도 함께 build 하기 위해서 sourceSets 에 해당 위치를 추가해준다.

### compileJava

```java
compileJava {
    options.compilerArgs << '-Aquerydsl.generatedAnnotationClass=javax.annotation.Generated'
}
```

해당 내용을 명시해주지 않으면 Q 파일 내 Generated 를 import 할 때 자바 9 에만 있는 import javax.annotation.processing.Generated 로 import 해준다.

그렇기 때문에 다른 버전에서도 사용할 수 있도록 java.annotation.Generated 로 import 하도록 설정하는 코드

### tasks.withType

```java
tasks.withType(JavaCompile) {
	options.generatedSourceOutputDirectory = file(querydslSrcDir)
}
```

annotationProcessorGeneratedSourcesDirectory 를 사용할 수도 있는데,  
Gradle 공식 문서에 해당 기능은 deprecated 되었고, 추후 major version 에선 삭제할 것이라고 소개하고 있기 때문에 generatedSourceOutputDirectory 를 사용한다.

generatedSourceOutputDirectory 는 annotation processors 에서 생성한 소스 파일을 저장할 디렉토리를 지정 해준다. (Gradle 공식문서 → [CompileOptions - Gradle DSL Version 7.5.1](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.compile.CompileOptions.html#org.gradle.api.tasks.compile.CompileOptions:generatedSourceOutputDirectory) )

이 코드를 통해 위에서 선언한 querydslSrcDir 변수의 src/main/generated 에다가 annotation processors 가 만든 QClass 들을 저장해준다.

### clean

```null
clean {
	// clean 실행 시 생성된 QClass 삭제
	delete file(querydslSrcDir)
}
```

build clean 시에 생성되었던 QClass 를 모두 삭제 (querydslSrcDir = src/main/generated)

### 실행하기

만들어진 QClass 삭제하기 : `Gradle > build > clean`

QClass 만들기 : `Gradle > build > compileJava`

src > main > generated 내에 Q 클래스가 만들어졌으면 queryDSL 설정이 정상적으로 된 것이다 !

## QueryDSL 사용하기

build.gradle 에 QueryDSL 을 사용할 준비가 어느정도 되었다면 본격적으로 QueryDSL 을 사용해보자.

### ① QuerydslConfig 파일 설정하기

```java
@Configuration
public class QuerydslConfig {
    @PersistenceContext
    private EntityManager entityManager;

    @Bean
    public JPAQueryFactory jpaQueryFactory() {
        return new JPAQueryFactory(entityManager);
    }
}
```

@PersistenceContext

- EntityManager 를 빈으로 주입할 때 사용하는 어노테이션
- 빈으로 주입받을 때 EntityManager 의 경우 @Autowired 가 아니라 해당 어노테이션으로 주입

JPAQueryFactory 를 Bean 으로 등록해서 프로젝트 전역에서 사용할 수 있도록 한다

### ② QueryDSL 사용 Repository 구조 잡기

Spring 에서 QueryDSL 을 사용하는 방법에는  
총 3가지 방법 (Spring Data Jpa Custom Repository / QueryRepositorySupport / JPAQueryFactory) 이 있다.

**1. Spring Data JPA Custom Repository 사용**

Spring 에서 QueryDSL 과 JPARepository 를 함께 사용하려면 각 역할 별 파일을 두 개 만들어주거나, 하나의 파일이 두 개의 의존성을 가져야 한다. 이러한 경우 Spring Data JPA 에서 제공하는 ✨Spring Data Custom Repository✨ 를 사용하면 된다.

공식 Docs → [Spring Data JPA - Reference Documentation](https://docs.spring.io/spring-data/jpa/docs/2.1.3.RELEASE/reference/html/#repositories.custom-implementations)

해당 방식을 통해서 CustomRepository 를 JpaRepository 상속 클래스에서 함께 상속 받아서 사용할 수 있다.

구현된 Repository 구조는 아래와 같다.  
![](https://velog.velcdn.com/images/soyeon207/post/916ecd10-4cdc-4f71-81a5-020c72a3f811/image.png)

Repository(interface) 가 JpaRepository(interface), CustomRepository(interface)를 다중 상속 받고,  
→ CustomRepository 인터페이스에 선언되어 있는 메소드에 대한 구현은 RepositoryImpl 에서 한다.  
→ 그리고 사용자는 Repository 인터페이스를 DI 받아서 사용한다.

**🤔 근데, RepositoryImpl은 Repository를 직접적으로 구현하지 않는데 어떻게 Repository에서 사용가능할까 ?**  
:💡 사용자 정의 구현 클래스인 경우 JPA 가 파일명이 repository interface 이름 + Impl 인 클래스를 찾아서 interface 에 JpaRepository를 Injection 할 때에 Impl 객체를 삽입해주기 때문에 사용할 수 있다.  
→ 그렇기 때문에 **반드시 구현체 파일명이 interface 명 + impl** 이여야 한다.

---

이 방식의 경우 총 3개의 repository 관련 파일이 만들어진다.

**첫번째**, Repository 파일 (interface)  
JpaRepository 와 Custom interface 를 상속한 파일  
사용자는 해당 파일을 DI 받아서 사용한다

```java
public interface ContentRepository extends JpaRepository<Content, Long>, ContentRepositoryCustom {
}
```

**두번째**, RepositoryCustom 파일 (interface)  
QueryDSL 로 커스텀해서 사용할 메소드 선언하는 파일

```java
public interface ContentRepositoryCustom {
}
```

**세번째**, RepositoryImpl 파일 (class)  
RepositoryCustom interface 에 선언한 메소드 구현하는 파일

```java
@RequiredArgsConstructor
public class ContentRepositoryImpl implements ContentRepositoryCustom {
  private final JPAQueryFactory queryFactory;
}
```

EX)

```java
public interface ContentRepositoryCustom {
}

@RequiredArgsConstructor
public class ContentRepositoryImpl implements ContentRepositoryCustom {
  private final JPAQueryFactory queryFactory;
}

public class ContentRepository extends JpaRepository<Content, Long>, ContentRepositoryCustom {

}
```

**단점**  
QueryDSL 을 사용하는 경우 만들어야 하는 파일이 엄청 많아지고, 그에 따른 관리 포인트가 늘어난다.

---

**2. QueryRepositorySupport 상속하기**

첫번째 방식처럼 여러 파일을 만들어서 상속받는 방식이 아니라 QuerydslRepositorySupport 만 상속하여 구조를 단순화 하는 방식

```java
@Repository 
public class ContentRepositorySupport extends QuerydslRepositorySupport {
  private final JPAQueryFactory queryFactory;
  
  public ContentRepositorySupport(JPAQueryFactory queryFactory) {
    super(Content.class);
    this.queryFactory = queryFactory;
  }
}
```

**단점**  
→ 매번 Support 를 상속 받고, super 생성자에 Entity 를 등록해줘야 한다.  
→ Entity 를 save, delete 하는 repository 와 query 하는 repository 가 나뉘어 진다.

---

**3. JPAQueryFactory 사용하기**

JPAQueryFactory 만 있으면 QueryDSL 기능을 모두 사용할 수 있다.

2번째 방식 처럼 super & 생성자를 사용하지 않아도 된다. 사용하고자 하는 Q 클래스를 import 해서 사용하는 방식

```java
@RequiredArgsConstructor
@Repository
public class ContentQueryRepository {
  private final JPAQueryFactory queryFactory;
  
  public List<Content> findByTitle(String title) {
    return queryFactory
              .selectFrom(content)
              .where(content.title.eq(title))
              .fetch();
  }
}
```

**단점**

Entity 를 save, delete 하는 repository 와 query 하는 Repository 가 나뉘어 진다.

---

**내가 사용하는 QueryDSL 사용 방식**  
내가 QueryDSL 을 사용할 때에는 첫번째 방식으로 가장 많이 사용한다.

두번째 (QueryRepositorySuppor), 세번째 (JPAQueryFactory) 방식의 경우에는 Spring JPA 기능과 QueryDSL 을 사용하기 위해서 각각 파일을 만들어서 사용하기 때문에 둘 간의 연관관계도 없고, 파일만 봤을 때 애매할 수 있다.

두번째, 세번째 방식의 경우 사용 예시 👇

```java
public interface ContentJpaRepository extends JpaRepository<Content, Long> {
}

@RequiredArgsConstructor
public class ContentQueryRepository {
  private final JPAQueryFactory jpaQueryFactory;
}

@RequiredArgsConstructor
public class Test {
  private final ContentJpaRepository contentJpaRepository;
  private final ContentQueryRepository contentQueryRepository;
  
  public void test() {
    contentJpaRepository.save();
    contentQueryRepository.customMethod();
  }
}
```

첫번째 방식으로 한다고 하면 아래와 같이 사용할 수 있다.

ContentRepositoryCustom.java

```java
public interface ContentRepositoryCustom {
    Content findByUserSeq(Long userSeq);
}
```

ContentRepositoryImpl.java

```java
import static show.grip.admin.content.model.QContent.content;

@Repository
@RequiredArgsConstructor
public class ContentRepositoryImpl implements ContentRepositoryCustom {
    private final JPAQueryFactory queryFactory;

    @Override
    public Content findByUserSeq(Long userSeq) {
        return queryFactory
                .selectFrom(content)
                .where(content.userSeq.eq(userSeq))
                .fetchFirst();
    }
}
```

ContentRepository.java

```java
public interface ContentRepository extends JpaRepository<Content, Long>, ContentRepositoryCustom {
}
```

코드에서는 다음과 같이 사용하기

```java
contentRepository.findByUserSeq(userSeq);
```

물론 내가 사용하는 방식이 무조건적으로 옳다는 건 아니라 각각의 방식에는 장단점이 존재한다.  
각 방식에 대해서 장단점 및 사용방식을 파악하고, 프로젝트에 맞는 방식을 사용하자 ~