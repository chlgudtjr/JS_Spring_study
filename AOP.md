# 2021-02-15(AOP)

### 1. AOP에 대해 설명하시오.

> AOP(Aspect Oriented Programming)

- 관점 지향 프로그래밍
- 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 각각 모듈화 하는 것이다.

---

### 2. AOP를 적용하기위한 두가지 방법은?

**첫번째 XML을 이용한 AOP**

- servlet-context.xml

    ```java
    <bean id="logAdvice" class="edu.bit.ex.board.aop.LogAdvice" />
    <bean id="logAop" class="edu.bit.ex.board.aop.LogAop" />

    <aop:config>
         <aop:aspect ref="logAdvice">
             <aop:pointcut id="publicM" expression="within(edu.bit.ex.board.service.*)"/>
             <aop:before pointcut-ref="publicM" method="printLogging" />
         </aop:aspect>
    </aop:config>
       
    <aop:config>
         <!-- aspect id는 logger이고, logAop를 참조함 -->
         <aop:aspect  ref="logAop">
             <!-- pointcut(핵심 기능)의 id는 publicM이고, 해당 패키지에 있는 모든 클래스에 공통 기능을 적용 -->
             <aop:pointcut id="publicM" expression="within(edu.bit.ex.board.service.*)"/>
             <!-- loggerAop()라는 공통 기능을 publicM라는 pointcut에 적용 -->
             <aop:around pointcut-ref="publicM" method="loggerAop" />          
         </aop:aspect>
    </aop:config>
    ```

- Log Advice

    ```java
    public class LogAdvice {
    	public void printLogging() {
    		System.out.println("=========로그기록Mk.1=========");
    	}
    }
    ```

- Log Aop

    ```java
    package edu.bit.ex.board.aop;

    import org.aspectj.lang.ProceedingJoinPoint;

    public class LogAop {
    	public Object loggerAop(ProceedingJoinPoint joinpoint) throws Throwable {
    		// LogAdvice 객체 작동 시간을 측정하기 위해 Jointpoint를 이용한 변수 선언
    		String signatureStr = joinpoint.getSignature().toShortString();
    		System.out.println(signatureStr + " is start.");
    		// 시간 설정
    		// 시작 시간을 설정한다(start time)
    		long st = System.currentTimeMillis();

    		// try ~ finally로 성능체크 기능을 구현한다.
    		try {
    			Object obj = joinpoint.proceed();
    			return obj;
    		} finally {
    			// 종료 시간을 설정한다(end time)
    			long et = System.currentTimeMillis();

    			System.out.println(signatureStr + " is finished.");
    			System.out.println(signatureStr + " 경과시간 : " + (et - st));
    		}

    	}
    }
    ```

**두번째 Annotation을 이용한 AOP**

- servlet-context.xml
- Log Advice2

    ```java
    package edu.bit.ex.board.aop;

    import org.aspectj.lang.annotation.Aspect;
    import org.aspectj.lang.annotation.Before;
    import org.springframework.stereotype.Component;

    // context xml파일에서 auto proxy 기능이 되었는지 확인한다!
    @Component
    @Aspect
    public class LogAdvice2 {
    	@Before("within(edu.bit.ex.board.service.*)")
    	public void printLogging() {
    		System.out.println("=========로그기록Mk.2=========");
    	}
    }
    ```

- Log Aop2

    ```java
    package edu.bit.ex.board.aop;

    import org.aspectj.lang.ProceedingJoinPoint;
    import org.aspectj.lang.annotation.Around;
    import org.aspectj.lang.annotation.Aspect;
    import org.springframework.stereotype.Component;

    // context xml파일에서 auto proxy 기능이 되었는지 확인한다!
    @Component
    @Aspect
    public class LogAop2 {
    	@Around("within(edu.bit.ex.board.service.*)")
    	public Object loggerAop(ProceedingJoinPoint joinpoint) throws Throwable {
    		String signatureStr = joinpoint.getSignature().toShortString();
    		System.out.println(signatureStr + " is start.");

    		long st = System.currentTimeMillis();

    		try {
    			Object obj = joinpoint.proceed();
    			return obj;
    		} finally {

    			long et = System.currentTimeMillis();

    			System.out.println(signatureStr + " is finished.");
    			System.out.println(signatureStr + " 경과시간 : " + (et - st));
    		}

    	}
    }
    ```
