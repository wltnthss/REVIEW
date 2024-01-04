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

**예외에 의미를 제공해라**

**호출자를 고려해 예외 클래스를 정의해라**

**null을 반환하지 마라**

**null을 전달하지 마라**

> 오류 처리를 프로그램 논리와 분리해 안정적이고 유지보수가 높은 깨끗한 코드를 작성하자.