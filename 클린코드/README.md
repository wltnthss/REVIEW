# 클린 코드 - 로버트C.마틴

## 1장 깨끗한 코드

**깨끗한 코드란**

* 깨끗한 코드란 무엇일까? 프로그래머의 생각마다 다르게 생각할거라고 생각한다. 이 책에서 많은 유명한 프로그래머들은 깨끗한 코드를 아래와 같이 정의하고 있다.
* **단순하고 직접적이며 잘 쓸 문장처럼 읽히는 코드**
* **중복이 없으며, 클래스, 메서드, 함수 등을 최대한 줄인 코드**
* **짐작했던 기능이 각 루틴 그대로 수행되는 코드**

* 보통 코드를 읽는 시간대 코드를 짜는 시간 비율은 10대 1 새 코드를 짜면서 끊임없이 기존 코드를 읽는다.
* 때문에 읽기 쉬운 코드는 매우 중요하며 기존 코드를 읽어야 새 코드를 짜므로 읽기 쉽게 만들면 코드를 짜기도 쉬워지며 이 논리는 빠져나갈 방법은 없다.
* 변수 이름 하나의 개선, 조금 긴 함수 하나를 분할, 약간의 중복 제거, 복잡한 if문 하나 개선과 같이 조금씩 시간과 노력을 투자해 코드를 정리하자.
* **체크아웃할 때보다 좀 더 깨끗한 코드를 체크인하면 코드는 절대 나빠지지 않는다.**

> 1장에서 가장 와닿았던 내용은, 중복과 표현력을 언급했던 Extreme Programming의 저자 론 제프리스의 말이었다.
 
1. 객체가 여러 기능을 수행한다면 여러 객체로 나눈다.
2. 메서드 추출 리팩터링 기법을 적용해 기능을 명확히 기술하는 메서드 하나와 기능을 실제로 수행하는 메서드 여러 개로 나눈다.
3. 집합에서 특정 항목을 찾아낼 필요의 경우 추상 메서드나 추상 클래스를 만듬으로써 실제 구현은 언제든지 바꿀 수 있게 설계한다.

## 2장 의미 있는 이름

**변수, 함수, 클래스, 패키지, 소스파일들의 이름 잘 짓는 규칙**

* **의도가 분명하게 이름을 지어라**
```java
// 나쁜 코드
List<int[]> list1 = new ArrayList<int[]>();

for(int[] x : theList)
    if(x[0] == 4)
        list1.add(x)

// 좋은 코드
List<int[]> flaggedCells = new ArrayList<int[]>();

for(int[] cell : gameBoard)
    if(cell[STATUS_VALUE] == FLAGGED)
        flaggedCells.add(cell);
```

* 개념에 대한 이름만 붙여도 코드가 상당히 나아진다.
* 좋은 이름을 지으려면 시간이 걸리지만 좋은 이름으로 절약하는 시간이 훨씬 더 많다.
  
* **의미 있게 구분하라**
```
moneyAmount - money
customerInfo - customer
accountData - account
```
* 위의 예제와 같이 명확한 관례가 없다면 구분이 안되기에 읽는 사람이 차이를 알도록 이름을 사용하자.

* **클래스이름과 객체 이름은 명사나 명사구**
* 동사는 사용하지 않는다.
```
좋은 코드 - Customer, WikiPage, Account, AddressParser
나쁜 코드 - Manager, Processor, Data, Info
```

* **메서드 이름은 동사나 동사구**
* 접근자, 변경자, 조건자는 get, set, is 를 붙인다.
```
postPayment, deletePage, save
```

* **한 단어를 두 가지 목적으로 사용하지마라**
* add라는 메서드를 만들었다해서 일관성을 고려해 add 메서드로 하나 더 만들지 말고 insert나 append 메서드 사용해라.

* **발음하기 쉬운 이름을 사용해라**
* **검색하기 쉬운 이름을 사용해라**
```js
// 나쁜 코드
const fiveDays = 5;

// 좋은 코드
const int WORK_DAYS_PER_WEEK = 5;
```

* **한 개념에 한 단어만 사용해라**

## 3장 함수

**함수를 만드는 첫째 규칙은 작게! 만드는 것이다**

* 각 함수가 하나의 이야기를 표현할 수 있도록 함수를 작성하자.
* 이 책에서 이상적인 함수의 크기는 아래와 같이 말하고 있다.

```java
publis static String renderPageWithSetupsAndTeardowns(PageData pageData, boolean isSuite) throws Exception{
    if(isTestPage(pageData)){
        includeSetupAndTeardownPages(pageData, isSuite);
    }
    return pageData.getHtml();
}
```

* 쉽게 말해 if/else/while문 등에 들어가는 블록은 한 줄이어야 한다는 뜻이다.

**함수는 한 가지의 기능만 하고, 한 가지를 잘 해야하며 한 가지만을 해야한다.**

**서술적인 이름을 사용해라**

* testableHtml -> SetupTeardownIncluderrender 
* 책의 저자는 위와 같이 메서드 이름을 변경하였다.
* 이름이 길어도 겁먹지말자. 길고 서술적인 이름은 짧고 어려운 이름보다 좋다.
* 이름을 붙일 때는 모듈 내에서 함수 이름은 같은 문구, 명사, 동사를 사용하자.
* includeSetupAndTeardownPages, includeSetupPages, includeSuiteSetupPage 등등

**함수에서 이상적인 인수 개수는 적을 수록 좋다**

* 함수에서 이상적인 인수 개수는 0개, 1개, 2개 .. 3개 이상은 가능한 피하는 편이 좋다.

**오류 코드보다 예외를 사용하고, Try/Catch 블록은 뽑아내라**

```java
try{
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
} catch(Exception e){
    logger.log(e.getMessage());
}
```

* try/catch 블록은 정상 동작과 오류 처리 동작을 뒤섞으므로 별도 함수로 뽑아내는 편이 좋다.

```java
try{
    deletePageAndAllReferences(page);
} catch(Exception e){
    logError(e)
}

private void deletePageAndAllReferences(Page page){
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e){
    logger.log(e.getMessagE());
}
```

* 함수는 한 가지 작업만 하듯이 오류 처리도 한 가지 작업에 속한다.
* 오류를 처리하는 함수는 오류만 처리해야 마땅하다.

## 4장 주석

**나쁜 코드에 주석을 달지마라. 새로 짜라**

* 잘 달린 주석은 그 어떤 정보보다 유용하나, 근거 없는 주석은 코드를 이해하기 어렵게 만든다.
* 표현력이 풍부하고 깔끔하며 주석이 거의 없는 코드가, 복잡하고 어수선하며 주석이 많이 달린 코드보다 훨씬 좋다. 

**코드로 의도를 표현해라**

 ```java
// 나쁜 코드
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if((employee.flags & HOURLY_FLAG) && (employee.age > 65)){

}

// 좋은 코드 
if(employee.isEligibleForFullBenefites()){

}

//
 ```

**주석으로 처리한 코드**

 ```java
InputStreamResponse response = new InputStreamResponse();
// InputStream resultsStream = formatter.getResultStream();
 ```

 * 주석으로 처리된 코드는 협업 상황에서 최악이기에 삭제하자.

**HTML 주석**

* HTML 주석은 혐오 그 자체이다.

**모호한 관계**

```java
/*
*   모든 픽셀을 담을 만큼 충분한 배열로 시작한다.
*   그리고 헤더 정보를 위해 200바이트를 더한다.
*/
this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200]
```

* 주석을 다는 목적은 코드만으로 설명이 부족해서이다.
* 주석 자체로 다시 설명을 요구하는 주석은 좋은 주석이 아니다.
* 이왕 공들여 주석을 달았다면 무슨 소리인지 명확하게 알 수 있는 주석을 달아두자.

> 정말로 좋은 주석은, 주석을 달지 않을 방법을 찾아낸 주석이라는게 핵심이였다. 주석 대신 메서드나 클래스로는 나타낼 수는 없는지, 알아보기 쉬운 변수명을 활용해보자.

## 5장 형식 맞추기

**형식을 맞추는 목적**

* 코드 형식은 개발자들간의 의사소통의 일환이며 기본적인 의무이고 유지보수에 계속 영향을 미친다.
* 때문에 코드 형식은 기본적으로 갖춰야하는 중요한 부분이다.
* 대부분 자바 소스 파일의 평균 크기는 약 65줄이며, 대부분 200~500줄 사이의 코드로도 커다란 시스템을 구축할 수 있다는 사실이다.

**수직거리**

* 서로 밀집한 개념은 세로로 가까이 둬야 한다.
* 두 개념이 서로 다른 파일에 속한다면 규칙이 통하지 않으나, 타당한 근거가 없다면 서로 밀접한 개념은 한 파일에 속해야 마땅하다.
* 변수 선언은 변수를 사용하는 위치에 가까이 선언하자.

> 기본적인 들여쓰기, 새로운 개념을 시작할 때는 빈 행, 밀접한 관계가 있는 변수와 메서드는 가까운 위치에 맞춰서 사용하자는 내용이 5장의 핵심이었다.

## 6장 객체와 자료구조 

**자료 추상화**

* 변수를 비공개하여 변수에 의존하지 않게 만들기위해 사용한다. 하지만 getter와 setter를 public으로 설정하거나 interface를 사용하여 비공개 변수를 외부에 노출하는 이유는 뭘까?
* 밑에 예제를 통해서 알아보자.

```java
// 구체적인 Point 클래스
public class Point{
    public double x;
    public double y;
}

//추상적인 Point 클래스
public interface Point{
    double getX();
    double getY();
    void setCartesian(double x, double y);
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}
```

* 추상 인터페이스를 통해 사용자가 구현을 모른 채 자료의 핵심을 조작하는 것이 추상적인 클래스이다.
* 그렇다고 무조건적으로 interface를 사용하는 것이 좋은 코드인가?
* 결론은 **아니다.**

> 절차적인 코드는 기존 자료 구조를 변경하지 않으면서 새 함수를 추가하기 쉽고, 새로운 자료 구조는 추가하기 어렵다.
> 
> 객체지향 코드는 기존 함수를 변경하지 않으면서 새 클래스를 추가하기 쉽고, 새로운 함수는 추가하기 어렵다.
>
> 즉, **객체 지향 코드에서 어려운 변경은 절차적인 코드에서는 쉽고, 절차적인 코드에서 어려운 변경은 객체 지향 코드에서는 쉽다.**

## 7장 오류 처리

* 깨긋한 코드와 오류 처리는 확실히 연관성이 잇다.
* 상당수 코드 기반은 전적으로 오류 처리 코드에 좌우된다.
* 오류 처리로 인해 프로그램 논리를 이해하기 어려워진다면 깨끗한 코드라 부르기 어렵다.

**오류 코드보다 예외를 사용해라**

 ```java
public void sendShutDown(){
    try{
        tryToShutDown();
    }catch(DeviceSHutDownError e){
        logger.log(e);
    }
}

private void tryToShutDown() throws DeviceShutDownError{
    DeviceHandle handle = getHande(DEV1);
    Device Recofed record = retrieveDeviceRecord(handle);
    ...
}
 ```

 * 오류가 발생하면 예외를 던지는 편이 논리와 오류 처리 코드가 뒤섞이지 않아 간단하다.

**미확인 예외를 사용해라**

* checked, unchecked의 논란이 있었지만 이 책에서는 unchecked 사용을 권한다.
* 왜냐하면 checked 는 메서드에서 확인된 예외를 던졌을 때 catch 블록이 세 단계 위에 있다면 그 사이 메서드 모두가 해당 예외를 정의해야하기에 OCP를 위반하기 때문이다.
* 때로는 확인된 예외도 유용하기에 용도에 맞게 사용해라.

**예외에 의미를 제공해라**

* 오류 메세지에 정보를 담아 예외와 함께 던져라.
* 로깅 기능을 통해 catch 블록에서 오류를 기록하도록 충분한 정보를 넘겨주어라.

**호출자를 고려해 예외 클래스를 정의해라**

* 외부 라이브러리가 던질 예외를 감싸는 예외 유형 하나를 반환해라.
* 외부 API를 감싸면 의존성이 크게 줄어들어 테스트도 쉽고 비용도 적으며, API를 설계한 방식에 발목을 잡히지 않는다.
* 아래는 예외 클래스를 정의한 예제이다.

```java
public class LocalPort {
    try{
        innerPort.open();
    }catch (DeviceResponseException e){
        throw new PortDeviceFailure(e);
    }catch (ATM1212UnlockedException e){
        throw new PortDeviceFailure(e);
    }catch (GMXError e){
        throw new PortDeviceFailure(e);
    }...
}
```

**null을 반환하지 마라**

**null을 전달하지 마라**

> 오류 처리를 프로그램 논리와 분리해 안정적이고 유지보수가 높은 깨끗한 코드를 작성하자.

## 8장 경계

* 시스템에 들어가는 모든 소프트웨어를 직접 개발하는 경우는 드물다.
* 때로는 오픈 소스나, 패키지를 이용하며 외부 코드를 우리 코드에 깔끔하게 통합해야한다.  
* 소프트웨어 경계를 깔끔하게 처리하는 기법과 기교를 살펴보자

**외부 코드 사용하기**

* 인터페이스 제공자와 사용자 사이에는 특유의 긴장이 존재한다.
* 사용자는 자신의 요구에 집중하는 인터페이스를 바란다 예로 java.util.Map을 살펴보자.
* Map은 다양한 인터페이스를 제공하지만 Map 사용자라면 누구나 Map에 접근할 권한이 생긴다.

```java
Map sensors = new HashMap();

Sensor s = (Sensor) sensors.get(sensorId);
```

* Sensor 라는 객체를 담는 Map을 만들려면 위와 같이 Map을 생성한다.
* Sensor 객체가 필요한 코드는 get을 통해 Sensor 객체를 가져온다.
* 여기서 중요한 점은, **Map이 반환하는 Object를 유형으로 변환할 책임은 Map을 사용하는 사용자에 있다는 것**이다.
* 그래도 동작은 하기에 괜찮지만 의도도 분명히 드러나지 않아, 깨끗한 코드라 보기는 어렵다.
* 위의 코드를 컴파일시 타입을 미리 체크하는 제네릭스를 사용하여 리팩토링하자면 아래와 같다.

```java
Map<String, Sensor> sensors = new HashMap();
...
Sensor s = sensors.get(sensorId);
```

* 위의 코드는 좀 더 가독성이 좋지만 사용자에게 필요하지 않은 기능까지 제공한다는 문제는 해결하지 못한다.
* Map<String, Sensor> 인스턴스를 여기저기로 넘긴다면 Map 인터페이스가 변하는 경우에 수정할 코드가 상당히 많아지므로 유지보수에 불리하다.
* 아래 코드를 통해 Map을 좀 더 깔끔하게 사용한 코드를 알아보자.

```java
puiblic class Sensors{
    private Map sensors = new HashMap();

    public Sensor getById(String id){
        return (Sensor) sensors.get(id);
    }
}
```

* 위와 같이 경계 인터페이스인 Map을 Sensors 클래스 안으로 숨김으로써 Map 인터페이스가 변하더라도 나머지 프로그램에는 영향을 미치지 않는다. (제네릭스를 사용하든 하지 않든 더 이상 문제가 되지 않음)

> 필자가 말하고자 하는 것은 **Map 클래스를 사용할 때마다 위와 같은 캡술화를 하라는 것이 아닌 Map객체를 여기저기 넘기지 않도록(클래스나 클래스 계열 밖으로) 주의하라는 것.** 이 핵심이었다.

**경계 살피고 익히기**

* 외부 코드를 사용하면 적은 시간에 더 많은 기능을 출시하기는 쉬우나, 어디서 어떻게 시작하면 좋을까 생각해보자.
* 외부 코드를 익히기는 어렵고, 통합하기도 어렵기에 간단한 테스트 케이스를 통해 외부 코드를 검증해보자. 이를 **학습 테스트**라고 부른다.
* 간단한 학습 테스트를 통해 검증 후에 메인 코드를 작성하는 습관을 가지자.
* 테스트 케이스(학습 테스트)는 패키지 새 버전의 호환성 검증을 해줘 사실을 바로 알아낼 수 있는 좋은 장점을 가졌다.

> 경계에 위치하는 코드는 깔끔하게 분리하며, 이를 테스트 케이스로 검증하자. 또한 외부 패키지를 호출하는 코드를 새로운 클래스로 감싸거나 Adapter 패턴을 사용하여 코드 가독성을 기르고, 일관성을 높이자.

# 9장 단위테스트 

**TDD 법칙 세 가지**

* TDD가 실제 코드를 짜기 전에 단위 테스트부터 짜라고 요구하는 사실을 점점 더 중요해지고 있는 사실이다.
* 이 규칙은 세 가지 법칙에 의거한다.

1. 첫째 법칙 : 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
2. 둘째 법칙 : 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
3. 셋째 법칙 : 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

* 위의 법칙에 따르면 실제 코드를 사실상 전부 테스트하는 테스트 케이스가 나온다.
* 하지만 실제 코드와 맞먹을 정도로 방대한 테스트 코드는 심각한 관리 문제를 유발하기도 한다.

**깨끗한 테스트 코드**

* 테스트 코드는 잘 설계하거나 변수명을 신경쓰지않고 돌아가게만 실제 코드를 테스트하면 그만이었다.
* 하지만 지저분한 테스트 코드일수록 변경하기 어려워지고 보수하는 비용도 늘어가며 개발자 사이에서 큰 불만으로 자리 잡게된다.
* 테스트 코드를 사용하지 않으면 결함 수가 많아지기에 개발자는 변경을 주저하게되고 코드는 망가지게 된다.

> **결국 테스트 코드는 실제 코드 못지 않게 중요하다** 라는 말을 전하고 있다.

```java

public void testGetPageHierarchyAsXml() throws Exception{
    // 테스트 자료 생성 (given)
    makePages("PageOne", "PageOne.ChildOne", "PageTwo");

    // 테스트 자료 조작 (when)
    submitRequest("root", "type:pages");

    // 테스트 자료 검증 (then)
    assertResponseIsXML();
    assertResponseContains(
        "<names>PageOne</names>", "<name>PageTwo</name>", "<name>ChildOne</name>"
    );
```

* 위의 코드와 같이 given, when, then 규칙에 맞춘 규격으로 테스트코드를 작성하는 습관을 기르자.

**테스트 당 assert 하나**

* JUnit으로 테스트 코드를 짤 때는 함수마다 assert문을 단 하나만 사용하자.
* assert문이 단 하나인 함수는 결론이 하나라서 코드를 이해하기 쉽고 빠르다.
* 꼭 하나가 아니여도 assert문 개수는 최대한 줄이는게 좋다.
* 밑에 예제와 같이 작성하면 알아보기도 쉬울뿐더러 유지보수에도 장점이 있다.

```java
@Test
public void turnOnCoolerAndBlowerIfTooHot() throws Exception{
    tooHot();
    assertEquals("hBChl", hw.getState());
}

@Test
public void turnOnHeaterAndBlowerIfTooCold() throws Exception{
    tooCold();
    assertEquals("HBchl", hw.getState());
}
```

**F.I.R.S.T**

1. F : Fast 빠르게 - 테스트는 빨리 돌아야한다.
2. I : Indepencend 독립적으로 - 각 테스트는 서로 의존하면 안 된다.
3. R : Repeatble - 어떤 환경에서도 반복 가능해야 한다.
4. S : Self-Validating - 부울값으로 결과를 내야 한다. (성공 아니면 실패)
5. T : Timely - 테스트는 적시게 작성해야 한다. 단위 테스트는 테스트하려는 실제 코드를 구현하기 직전에 구현하는 습관을 기르자.

> 테스트 코드는 실제 코드만큼이나 프로젝트 건강에 중요하다. 테스트 케이스를 작성하는 습관이 길러져 있지 않는 것 같으니, 실제 코드 작성전에 깨끗한 테스트 케이스 코드를 작성하는 습관을 기르자.

## 10장 클래스

**클래스 체계**

* 클래스를 정의하는 표준 자바 관례에 따르면, 가장 먼저 변수 목록이 나온다.
* 다음으로는 비공개(private) 변수가 나오며 다음에는 공개 함수가 나오고, 비공개 함수는 자신을 호출하는 공개 함수 직후에 넣는다.
* 즉, 추상화 단계가 순차적으로 내려간다.

**클래스는 작아야 한다!**

* 클래스를 만들 때 첫 번째 규칙은 크기다. 클래스를 설계할 때도 함수와 마찬가지로, 작게가 기본 규칙이라는 의미를 담고 있다.
* 클래스 이름은 해당 클래스 책임을 기술해야 한다.
* SRP(단일 책임 원칙) 로 클래스나 모듈을 변경할 이유는 하나에 의거하여 클래스를 작성하자.
* 단일 책임 클래스가 많아지면 큰 그림을 이해하기 어렵다고 우려워하지만, 규모가 커져도 부품의 수는 비슷하기에 큰 서랍에 모두 던져 넣지 말고 작은 서랍을 많이 두고 명확한 컴포넌트를 나눠 넣자

**응집도**

* 클래스는 인스턴스 변수 수가 작아야 한다.
* 일반적으로 메서드가 변수를 더 많이 사용할수록 메서드와 클래스는 응집도가 더 높다.
* 아래는 응집도가 높은 클래스의 예제인 Stack 클래스이다.

```java
public class Stack{
    private int topOfStack = 0;
    List<Integer> elements = new LinkedList<Integer>();

    public int size(){
        return topOfStack;
    }

    public void push(int element){
        topOfStack++;
        elements.add(element)
    }

    public int pop() throws PoppedWhenEmpty(){
        if(topOfStack == 0){
            throw new PoppedWhenEmpty();
        }
        int element = elements.get(--topOfStack);
        elements.remove(topOfStack);
        return element;
    }
}
```

* 몇몇 메서드만이 사용하는 인스턴스가 아주 많아진다는 것은 클래스를 쪼개야 한다는 신호이다.
* 응집도가 높아질수록 변수와 메서드를 적절히 분리해 새로운 클래스 두 세개로 쪼개준다.

## 11장 시스템