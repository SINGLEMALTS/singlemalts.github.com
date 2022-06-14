---

title: "[EffectiveC#] Item01 지역변수를 선언할 때는 var를 사용하는 것이 낫다"
excerpt: C# 언어요소
categories:
- Csharp
tags:
- - C#
toc: true
toc_sticky: true
published: true

---

# 지역변수를 선언할 떄는 var를 사용하는 것이 낫다

var는 동적 타이핑이 아니라, 할당 연산자의 오른쪽의 타입을 확인하여 왼쪽 변수의 타입을 결정.

## 장점

1. C#의 익명 타입(anonymous type)을 지원하기 위해 암시적 타입 선언을 할 수 있는 손쉬운 방법을 제공하기 때문 (C# 3.0~)
    - 익명타입 : 클래스를 미리 정의하지 않고 사용할 수 있게 함.
    new { ... } 와 같이 사용

    ~~~Csharp
    // 익명 타입 : new { 속성1=값, 속성2=값; }
    var t = new { Name="홍길동", Age=20 };
    string s = t.Name;
    ~~~

2. 코드 가독성에도 var를 사용하여 선언한 코드가 더 잘 읽힘.
3. 정확한 반환타입을 알지 못한채 올바르지 않은 타입을 명시적으로 지정하지 않기위해.

    ```Csharp
    IEnumerable<string> q = form c in db.Customers
                            select c.ContactName;
    var q2 = q.Where(s=>s.StartsWith(start))
    ```

    - DB 쿼리 수행시, LINQ 쿼리는 실제로 IQueryable< string> 타입을 반환하는데 IQueryable이 IEnumerable을 상속하기 때문에 컴파일러는 암시적 변환을 허용하여 IEnumerable로 암시적형변환이 됨 -> 성능 문제 유발

## 단점

1. 과도한 var의 사용으로 인해 가독성이 나빠지는 경우가 있음
    - var를 사용하여 특정 반환값을 저장할 변수를 선언하는 경우

    ```Csharp
    // 반환 값의 변수명에서 타입을 추측하기 어려움
    var result = someObject.DoSomeWork();
    ```

2. 내부 자동 타입 변환으로 인해 버그를 만드는 경우가 있음 -> 유지보수를 더 어렵게 하는 경우

    ```Csharp
    // 메서드의 반환인 f의 타입에 따라 total의 타입이 결정됨.
    // f가 double, int32 등 다른경우 결과값이 달라짐
    var f = GetMagicNum();
    var total = 100 * f / 6; // 컴파일러가 literal 상수들을 f와 동일한 타입으로 변환 후 계산하기때문에 결과값에 차이 발생
    ```

# 요약

코드를 읽을 때 타입을 명시적으로 드러내야하는 경우가 아니면  var가 더 좋다. (변수명을 통해 역할을 명확하게 드러내도록 작성)
내장 숫자 타입(int, float, double 등) 선언시는 명시적인 타입 선언이 더 좋다.



