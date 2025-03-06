# 계산기 프로젝트

Lv. 1 클래스를 사용하지 않는 계산기  
Lv. 2  클래스를 활용한 계산기  
Lv. 3  요구조건을 구현한 계산기  
**Lv ?. 추가기능을 구현한 계산기**

## 요구사항

> Lv. 1 클래스 없이 기본적인 연산을 수행할 수 있는 계산기 만들기  ✅ 완료  
- 양의 정수(0 포함)를 입력받기
- 사칙연산 기호(➕,➖,✖️,➗)를 입력받기
- 위에서 입력받은 양의 정수 2개와 사칙연산 기호를 사용하여 연산을 진행한 후 결과값을 출력하기
- 반복문을 사용하되, 반복의 종료를 알려주는 “exit” 문자열을 입력하기 전까지 무한으로 계산을 진행할 수 있도록 소스 코드를 수정하기
  
> Lv. 2  클래스를 적용해 기본적인 연산을 수행할 수 있는 계산기 만들기  ✅ 완료  
- 사칙연산을 수행 후, 결과값 반환 메서드 구현 & 연산 결과를 저장하는 컬렉션 타입 필드를 가진 Calculator 클래스를 생성
- Lv 1에서 구현한 App 클래스의 main 메서드에 Calculator 클래스가 활용될 수 있도록 수정
- App 클래스의 main 메서드에서 Calculator 클래스의 연산 결과를 저장하고 있는 컬렉션 필드에 직접 접근하지 못하도록 수정 (캡슐화)
- Calculator 클래스에 저장된 연산 결과들 중 가장 먼저 저장된 데이터를 삭제하는 기능을 가진 메서드를 구현한 후 App 클래스의 main 메서드에 삭제 메서드가 활용될 수 있도록 수정

> Lv. 3  Enum, 제네릭, 람다 & 스트림을 이해한 계산기 만들기  ✅ 완료  
- Enum 타입을 활용하여 연산자 타입에 대한 정보를 관리하고 이를 사칙연산 계산기 ArithmeticCalculator 클래스에 활용
- 실수, 즉 double 타입의 값을 전달 받아도 연산이 수행하도록 만들기
- 저장된 연산 결과들 중 Scanner로 입력받은 값보다 큰 결과값 들을 출력

> Lv ?. 추가기능 구현 ✅ 완료
- Lv. 3의 요구조건에 부합
- `"1+5*3+5"` 와 같이 계산식 문자열 입력 대응
- 연산자 우선순위 적용
  
## 주요 구현

- **Lv. 3의 요구사항 부합**  

- **`"1+5*3+5"` 와 같이 계산식 문자열 입력 대응**  

- **연산자 우선순위**에 따른 계산 결과값 출력

## 특수 상황에 대한 예외처리

<details>


<summary style="font-size: 16px;">
<strong> 1. 잘못된 연산자 입력에 대한 예외처리</strong>
</summary>


`MyOperator` Enum에서 사용자의 잘못된 연산자(예: `@`, `#`, `$`) 입력에   `IllegalArgumentException`을 발생  
→ `MyOperator.getOperator()` 메서드는 연산자에 해당하는 값을 찾을 수 없으면 예외처리

```java
public static MyOperator getOperator(char operatorChar) {     
    return Arrays.stream(values())     //MyOperator에 정의된 모든 값 = value(), 배열을 스트림으로 변환 = stream()
        .filter(check -> check.operationChar == operatorChar)  //MyOperator에 정의된 연산자와 입력받은 operatorChar를 비교
        .findFirst()                            // 필터링을 거친 스트림에서 첫 번째 요소를 반환
        .orElseThrow(() -> new IllegalArgumentException("잘못된 연산자 : " + operatorChar));    //값을 찾을 수 없는경우
}
```

`ArithmeticCalculator` 클래스에서는 try-catch문을 이용해 오류 문구 및 잘못된 연산자 안내

```java
try {
  double result = myCalculator.Calculate(input);
  System.out.println("결과값 : " + result);
  }
  catch (Exception e) {
    System.out.println(e);
    }
```

</details>

<details>

<summary style="font-size: 16px;">
<strong>2. 기록조회 메뉴의 기록삭제 가능 범위에 대한 예외처리</strong>
</summary>

사용자가 특정 계산이력을 삭제할 때, 잘못된 인덱스를 입력하는 경우에 대한 예외처리  
1. 숫자로 기록을 지정하지 않은 경우  
2. 저장된 범위를 벗어나는 경우
3. 그 외의 예외사항


`setHisory` 메소드, 
```java
public void setHistory(int idx) {
  if (idx > calHistory.size()) {
    System.out.println("저장된 범위를 벗어났습니다.");
    }
  else {
    try {
      calHistory.remove(idx);
      System.out.println(idx + " 번째 계산이 삭제되었습니다.");
      } catch (Exception e) {
        System.out.println("오류발생 내용 : " + e);
        }
      }
}
```

```java
if(inputAnswer.toUpperCase().equals("Y")) {   //대소문자 구분을 위한 toUpperCase
  System.out.println("몇번째 기록을 삭제하시겠습니까?");
  try {
    inChoiceNum = scanner.nextInt();
    scanner.nextLine();
    myCalculator.setHistory(inChoiceNum);
    }catch (NumberFormatException e) {    //숫자가 아닌경우
      System.out.println("잘못된 입력입니다. 숫자를 입력해주세요.");
      continue;
    }catch (Exception e) {    //그 외 (-1 입력 등..)
      System.out.println("잘못된 입력입니다." + e);
      continue;
    }
  }
```

### 3. 계산식의 마지막 숫자 처리
사용자의 입력을 처리하는 `Calculate` 메소드에서 `"1+1+1"` 을 입력받은 경우 마지막 숫자 1은 `numList` 에 추가되지 않기 때문에 `stringBuilder`의 길이를 확인하고, 마지막 값을 `numList`에 추가

```java
for (int i = 0; i < input.length(); i++) {
  char inputChar = input.charAt(i);
  
  if (Character.isDigit(inputChar) || inputChar == '.') // input의 i번째 문자가 숫자인 경우
  {
    stringBuilder.append(inputChar);
  }
  else                                                  // 그 외의 경우(연산기호)
  {
    numList.add(Double.parseDouble(stringBuilder.toString()));
    stringBuilder.setLength(0);
    operList.add(MyOperator.getOperator(inputChar));
  }
}
```

```java
if (stringBuilder.length() > 0)     
{
  //stringBuilder가 비어있지 않은 경우(마지막 숫자) → numList에 추가
  numList.add(Double.parseDouble(stringBuilder.toString()));
}
```

</details>

<details>

<summary style="font-size: 16px;">
<strong>3. 0으로 나누는 경우</strong>
</summary>

`Enum`인 `MyOperator`에서 `DIVISION` 연산에서 0으로 나누는 경우 `ArithmeticException` 오류발생
`ArithmeticCalculator` 클래스에서 해당 오류내용을 출력 및 예외처리

```java
//MyOperator(Enum)
PLUS('+', (a, b) -> a + b),
MINUS('-', (a, b) -> a - b),
TIMES('*', (a, b) -> a * b),
DIVISION('/', (a, b) -> {
    if (b == 0) throw new ArithmeticException("0으로 나눌 수 없습니다.");
    return a / b;
});
```

```java
// ArithmeticCalculator 클래스의 계산식을 입력받는 부분
System.out.println("계산식을 입력하세요.");
String input = scanner.nextLine();
try {
  double result = myCalculator.Calculate(input);
  System.out.println("결과값 : " + result);
} catch (Exception e) {
  System.out.println(e);
}
```

</details>

## 트러블 슈팅


<details>

<summary style="font-size: 16px;">
<strong>1. Lv. ? [ 계산식 String 으로 입력받아 분할하기 및 음수처리하기 ]</strong>
</summary>

기존에는 첫번째 숫자`num1`, 연산기호`operator`, 두번째 숫자`num2` 를 입력받아 간단하게 처리할 수 있었지만 여러 수식이 입력되는 경우 이를 분할하여 처리하는 기능을 구현하고자 했습니다.

기존 저장방식  
`inNumFirst` 숫자1  
`inNumSecond` 숫자2  
`inCalOperator` 연산자


1. 사용자의 `"1+2*3+5"` 같은 입력 형태를 숫자와 연산자로 분리하는 기능 필요  

> 숫자를 저장하는 List<Double> numList  
> 연산자를 저장하는 List<MyOperator> operList  
각각 숫자와 연산자를 분리하여 저장, 연산처리

```java
StringBuilder stringBuilder = new StringBuilder();
List<Double> numList = new ArrayList<>();   //숫자 저장
List<MyOperator> operList = new ArrayList<>();  //연산자 저장

for (int i = 0; i < input.length(); i++) {
  char inputChar = input.charAt(i);
  
  if (Character.isDigit(inputChar) || inputChar == '.') // 숫자인경우 (소수점포함)
  {
    stringBuilder.append(inputChar);
  }
  else    //연산자인 경우 numList에 추가 후 stringBuilder 초기화
  {
    numList.add(Double.parseDouble(stringBuilder.toString()));
    stringBuilder.setLength(0);
    operList.add(MyOperator.getOperator(inputChar));
  }
}

if (stringBuilder.length() > 0)     // 마지막 숫자 처리 필요
{
    numList.add(Double.parseDouble(stringBuilder.toString()));
}
```

위와 같은 코드에서는 "1+2*3+5"와 같은 수식은 제대로 처리가 되었지만, 음수를 포함한 수식은 오류가 발생했습니다.

"-5+2"와 같은 입력, "2*-5"와 같은 입력의 경우를 위해 numList에 음수를 추가하는 조건이 필요했습니다. 

1. "-5+2"와 같은 입력에서 -5 의 -를 연산자로 인식하여 음수로 처리하지 않고 operList에 추가되어 오류 발생  
> 현재 문자가 숫자인 경우와 연산자인 경우를 구분할 수 있는 `boolean isNumberOrOperator` 변수 이용  
> `-` 연산자가 입력된 경우 `isNumberOrOperator` 값이 true일때만 음수로 처리  
> 처음 입력되는 문자는 반드시 숫자이므로  `isNumberOrOperator` 를 true로 초기화

```java
// 현재 입력된 문자가 숫자(음수)인지 연산자인지 체크함, 처음 문자는 반드시 숫자이기 때문에 true로 초기화
boolean isNumberOrOperator = true;

for (int i = 0; i < input.length(); i++) {
    char inputChar = input.charAt(i);

    // 입력된 값이 숫자 또는 소수점인 경우
    if (Character.isDigit(inputChar) || inputChar == '.') {
        stringBuilder.append(inputChar);
        isNumberOrOperator = false;
    }
    // 입력된 값이 '-' 연산자이며 숫자가 오는 순서인 경우 ( 음수로 저장 되어야 하는 경우, 스트링빌더에 - append )
    else if (inputChar == '-' && isNumberOrOperator) {
        stringBuilder.append(inputChar);
    }
    // 연산자가 입력된 경우
    else {
        if (stringBuilder.length() > 0) {   //numList에 add 후 초기화
            numList.add(Double.parseDouble(stringBuilder.toString()));
            stringBuilder.setLength(0);
        }

        operList.add(MyOperator.getOperator(inputChar));
        isNumberOrOperator = true;  // 연산자가 온 후 숫자가 와야하니 true
    }
}

// 마지막 숫자 처리 필요
if (stringBuilder.length() > 0) {
    numList.add(Double.parseDouble(stringBuilder.toString()));
}
```
`boolean`타입의 `isNumberOrOperator` 변수를 이용해 다음에 입력되는 문자를 숫자 혹은 연산자로 나누어 진행할 수 있었습니다.

if문 `(Character.isDigit(inputChar) || inputChar == '.')`  
- 현재 문자가 숫자 또는 소수점인 경우 stringBuilder에 해당 문자를 추가
- `isNumberOrOperator`를 `false`로 변경하여 계속해서 숫자를 추가

else if문 `(inputChar == '-' && isNumberOrOperator)`
- `-`가 입력되고, 그 전에 연산자가 나오거나 수식의 첫 번째 값일 경우 음수처리

else문  
- 숫자와 연산자가 처리될 때 호출  
- 연산자 뒤에 오는 숫자는 문자열을 `numList`에 추가하고 `StringBuilder`를 초기화  
- 연산자는 `operList`에 저장  
- `isNumberOrOperator`를 true로 변경 다음에 올 문자를 숫자로 변경

예시  
`-5.2+3`을 입력한 경우  
 `-5.2 → numList: [-5.2], operList: []`  
 `+ → numList: [-5.2], operList: [PLUS]`  
 `3 → numList: [-5.2, 3.0], operList: [PLUS]` 결과값 : -2.2  

`-7.2-2`을 입력한 경우  
`-7.2 → numList: [-7.2], operList: []`  
`- → numList: [-7.2], operList: [MINUS]`  
`2 → numList: [-7.2, 2.0], operList: [MINUS]` 결과값 : -9.2  

`5+5*-3`을 입력한 경우  
`5 → numList: [5.0], operList: []`  
`+ → numList: [5.0], operList: [PLUS]`  
`5 → numList: [5.0, 5.0], operList: [PLUS]`  
`* → numList: [5.0, 5.0], operList: [PLUS ,TIMES]`  
`-3 → numList: [5.0, 5.0, -3.0], operList: [PLUS ,TIMES]` 결과값 : -10.0   

이로써 문자열 입력데이터 분할 및 음수처리를 완료하였습니다.

</details>


<details>

<summary style="font-size: 16px;">
<strong>2. Lv. 3 [ 입력받은 연산기호 문자(Char)를 Enum의 상수로 변환하기 ]</strong>
</summary>


연산자별 우선순위를 적용한 사칙연산을 효과적으로 처리하기 위해 Enum을 사용했습니다.

기존 Lv.2에 해당하는 클래스를 이용한 계산기 도전과제에서는 `switch`문을 이용한 연산을 진행했었기에 `Enum`을 만들고, 람다식을 활용하여 연산을 진행하도록 수정했습니다.

Enum을 활용한 코드를 작성 후 실행했으나, 계산 도중 `Enum`에서 해당하는 연산을 찾을 수 없는 오류가 발생했습니다.

1. 입력받은 연산 기호( + , - , * , / )그대로 `Enum`을 사용
   - `Enum`의 상수로 변환하는 과정이 없음
2. 연산자가 `char`로 저장되어 `Enum`에서 해당하는 연산자를 찾을 수 없음

기존에는 입력받은 연산기호를 `char`로 저장하여 `switch`문으로 계산하다보니 `Enum`을 제대로 활용할 수 없었습니다.

`Enum`에 적절한 상수로 변환해주는 과정이 필요하다는걸 알고 연산기호를 적절한 `Enum`의 상수로 변환해주는 과정을 추가했습니다.

```java
//Class를 활용한 계산기의 Calculator 클래스
switch (operator) {
  case '+':
    result = firstNum + secondNum;
    break;
  case '-':
    result = firstNum - secondNum;
    break;
  case '*':
    result = firstNum * secondNum;
    break;
  case '/':
    if (secondNum == 0) {
    System.out.println("분모가 0입니다.");
    break;
    }
    result = firstNum / secondNum;
    break;
  default:
    System.out.println("잘못된 연산자입니다.");
    result = firstNum / secondNum;
    break;
}
```

하지만 입력받은 연산자를 `Enum` 에서 변환하는것이 아닌 계산과 관련된 `Calculate` 클래스에서 처리하는것이 부적절하다 판단하여 후에 추가기능을 구현한 계산기에서는 다음과 같이 변경합니다.

```java
// Lv. ? 추가기능을 구현한 계산기
public static MyOperator getOperator(char operatorChar) {
  return Arrays.stream(values())
              .filter(check -> check.operationChar == operatorChar)
              .findFirst()
              .orElseThrow(() -> new IllegalArgumentException("잘못된 연산자 : " + operatorChar));
}
```

`Enum` 내부에 `getOperator` 메소드를 작성해 계산 처리 과정 중 `operList`에 연산자를 저장할 때 `getOperator()`를 통해 변환 후 operList에 저장 후 `Enum`을 사용하도록 수정했습니다.

</details>


<details>

<summary style="font-size: 16px;">
<strong>3. Lv. ? [ 연산자별 우선순위 연산 ]</strong>
</summary>


문자열이 입력됨에 따라 입력값을 순서대로 연산하는게 아닌 우선순위가 필요했습니다.

`1+1*2` 를 입력하는 경우 3의 결과가 나와야하는데, 우선순위를 정하지 않아 4가 출력되고 있었습니다.

우선순위에 따른 연산은 Lv. 3에서 작성한 코드가 없기때문에 새로 작성해야 했습니다.

이 사항은 `Enum` 에서 `highPriority()` 메서드를 통해 TIMES, DIVISION 연산이 높은 우선순위를 가지는지 `true` , `false` 를 리턴하여 순서에 맞게 진행되도록 작성했습니다.

```java
public boolean highPriority() {
  return this == TIMES || this == DIVISION; //TIMES, DIVISION 연산인 경우 return true
  }
```

```java
// 우선순위 연산 먼저 처리
for (int i = 0; i < operList.size(); i++) {
  if (operList.get(i).highPriority()) { //우선순위 결과가 true인 경우 실행
    double result = operList.get(i).doCalculator(numList.get(i), numList.get(i + 1));
    numList.remove(i + 1);  //i, i+1 연산 후 numList의 i+1 삭제
    numList.set(i, result); //i 번째에 결과값 설정
    operList.remove(i);     //operList의 i 번째도 계산완료에 따른 삭제
    i--;  //List를 remove 하기 때문에 List 전체 크기가 줄어듦에 따라 i--
    }
}

// 남은 +, - 계산
double result = numList.get(0);
for (int i = 0; i < operList.size(); i++) {
    result = operList.get(i).doCalculator(result, numList.get(i + 1));
}
```
작성된 코드로 `*` , `/` 연산을 먼저 실행한 후 `+` , `-` 연산을 진행할 수 있게 되었습니다.

</details>

## 아쉬운 점

- 괄호 구현 감을 못잡겠다.
- 처음부터 Stack을 구현하여 진행했으면 조금 더 편했을까? 라는 생각이 든다.
- 내가 느끼기엔 문자열로 입력을 받다보니 제네릭 활용은 크게 할만한 곳을 찾을 수 없었다.
- 음수처리를 할때 종이에다가 적어가며 고민을 했는데 알고리즘을 좀 더 풀고 공부해야겠다.
