# Mostly Adequate Guide to Functional programming

[book link](https://mostly-adequate.gitbook.io/mostly-adequate-guide/)  
by Professor Franklin Frisby.

## Chapter 01: What Ever Are We Doing?

- 공통적인 로직을 갖는 함수라면, 함수 이름을 일반적인 이름으로 지어서, 여러 곳에서 사용하여야 합니다.
- 함수형 프로그래밍은 범주 이론, 집합 이론, 람다 미적분학의 지식이 필요합니다.

## Chapter 02: First Class Functions

- javascript에서 함수는 First Class Citizen입니다. 이는 함수를 다른 데이터 유형 처럼 배열에 저장하거나 함수의 매개변수로 전달이 가능함을 이야기 합니다.
- 단순히 함수를 감싸는 함수는 필요 없습니다. 직접 매개변수나 다른 방법으로 함수를 넘겨주어 사용하는 것이 수정이 쉽고, 더욱 간결합니다.

## Chapter 03: Pure Happiness with Pure Functions

- 순수 함수는 동일한 입력이 주어졌을 때, 동일한 출력을 반환하고, 사이드 이펙트가 없는 함수입니다.
- 사이드 이펙트는 결과를 계산하는 동안에 발생하는 시스템(전역) 상태의 변화 또는 외부 세계와의 관찰 가능한 상호 작용을 이야기 합니다.
    - 함수의 외부에서의 어떤 것이 함수 내부에 영향을 주어서는 안됩니다.
    - 수학에서의 함수와 동일.
- 순수 함수를 사용하면 모든 요인을 함수 내에서 관리하기 떄문에, 테스트 작성이 쉽습니다.
- 참조 투명성은 순수 함수이면서, 사이드 이펙트가 없는 함수를 이야기하며, 이러한 함수를 참조 투명성이 있다라고 표현합니다.
    - 참조 투명성은 코드의 예측 가능성과 재사용성을 높이며, 병렬 처리와 최적화에 유리합니다. 하지만 모든 상황에서 참조 투명성을 유지하는 것은 쉽지 않으며, 때로는 사이드 이펙트가 필요한 경우도 있습니다. 예를 들어, 파일을 읽고 쓰거나 데이터베이스에 쿼리를 실행하는 등의 작업은 부수 효과를 가집니다. 이런 경우에는 참조 투명성을 유지하기 어렵습니다.
- 순수 함수로 구성된 함수는 외부 상태를 변화시키지도, 외부 상태에 영향을 받지도 않기 때문에, 병렬 실행이 가능합니다.

## Chapter 04: Currying
- Currying은 함수가 함수를 리턴하도록 하여 파라미터를 줄이는 방법입니다.
    - before `const add = (x, y) => x + y;`
    - after `const add = (x) => (y) => x + y;`
- 이 방식은 수학에서의 함수를 표현하기에 적합합니다. 
- 이렇게 사용 가능합니다.
    ```javascript
    const getChildren = x => x.childNodes;
    const allTheChildren = map(getChildren);
     ```

## Chapter 05: Coding by Composing
- 함수는 합성 가능해야 하며, 합성된 함수는 새로운 함수가 되어야합니다.
- 순수 함수는 파이프에서 입력과 출력이 결국 연결이 되어집니다. 만약, 이 연결이 끊어지면, 출력이 아무런 쓸모가 없어집니다.
- point-free는 작동하는 데이터를 언급하지 않는 함수를 의미합니다.
    ```javascript
    // not pointfree because we mention the data: word
    const snakeCase = word => word.toLowerCase().replace(/\s+/ig, '_');

    // pointfree
    const snakeCase = compose(replace(/\s+/ig, '_'), toLowerCase);
    ```
    - 추가 찾아본 내용,   
        여기서 “point”는 함수의 인자를 지칭합니다. 즉, “point-free”라는 용어는 함수의 인자를 명시하지 않는다는 의미이며 이러한 스타일로 함수를 정의하는 것이 “Pointfree Style”입니다.

        수학에서 이 용어를 사용하는데, 

        `f(x) = x^2` 와 같은 함수를 `f = square` 처럼 사용하여서 인사 x를 작성하지 않아도 이해 가능하도록 하는 방법입니다.

        - `f(x) = x^2` :  point-full style
        - `f = square` : point-free style

- 아래와 같은 함수를 만들어서 pipe 중간에 로그를 찍어서 확인하면 디버깅하기 편합니다.
    ```javascript
    const trace = curry((tag, x) => {
        console.log(tag, x);
        return x;
    });
    ```
    사용 예시
    ```javascript
    const dasherize = compose(
        intercalate('-'),
        toLower,
        trace('after split'), // log -> after split: blahblah
        split(' '),
        replace(/\s{2,}/ig, ' '),
    );
    ```
- 카테고리 이론   
    카테고리 이론은 집합이론, 유형 이론, 군 이론, 논리 등 여러가지 분야의 개념을 공식화할 수 있는 추상적인 수학 이론입니다. 카테고리 이론에서의 "카테고리"는 아래와 같은 구성 요소를 가지는 모음으로 정의됩니다.
    - A collection of objects
    - A collection of morphisms
    - A notion of composition on the morphisms
    - A distinguished morphism called identity   

    <b>A collection of objects</b>   
    여기서의 object는 데이터 유형을 이야기합니다. 즉, Boolean의 모음, Number의 모음들이 곧 카테고리 입니다.   
    <b>A collection of morphisms</b>   
    여기서 morphism은 기본적으로 한 유형의 값에서 다른 유형의 값으로 변환하거나, 동일한 유형 내에서 값을 변환하는 함수를 나타냅니다.   
    <b>A notion of composition on the morphisms</b>    
    위에서 이야기한 morphism의 합성 개념입니다. 즉, 함수 `f(x)`, `g(y)`가 있을 때, `h(z) = g(f(x))`로 정의 되면, `h(z)`는 `f`와 `g`의 합성 함수 입니다. 이는 코드로도 표현이 가능합니다.
    ```javascript
    const g = x => x.length;
    const f = x => x === 4;
    const isFourLetterWord = compose(f, g);
    ```   
    <b>A distinguished morphism called identity</b>   
    아래와 같은 함수를 `identity`라고 부릅니다.
    ```javascript
    const id = x => x;
    ```
    이 `identity`는 아래의 상황을 만족해야합니다. 즉, 같아야합니다.
    ```javascript
    // identity
    compose(id, f) === compose(f, id) === f;
    // true
    ```
    이 `identity`의 쓸모없음을 잘 기억해두라고 하시네요...
    * 카테고리 이론은 앞으로 계속 등장할 것 같습니다. 계속 보면서 익혀야할 내용인 것 같습니다.

## Chapter 06: Example Application
- Declarative Coding(선언형 코딩)   
    선언적 코딩은 명령형 코딩과 반대되는 개념인데, 컴퓨터에게 작업을 일일이 지시하는 것이 명령형이고, 우리가 원하는 결과물의 사양을 작성하는 것이 선언형 코딩입니다. 아래에 간단히 예시를 작성해 보겠습니다.

```javascript
    // 명령형
    const makes = [];

    for (let i = 0; i < cars.length; i += 1) {
        makes.push(cars[i].make);
    }

    // 선언형
    const makes = cars.map(car => car.make);
```
함수형 프로그래밍에서 선언형 코딩을 사용한 예시도 작성하였습니다.
```javascript
// imperative
const authenticate = (form) => {
  const user = toUser(form);
  return logIn(user);
};

// declarative
const authenticate = compose(logIn, toUser);
```
- 간단한 애플리케이션을 만들고 리팩토링하는 예시가 나와있는데, 요약 보다는 참조로 해놓겠습니다.[여기](https://mostly-adequate.gitbook.io/mostly-adequate-guide/ch06)
