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
- 이렇게도 사용 가능합니다.
    ```javascript
    const getChildren = x => x.childNodes;
    const allTheChildren = map(getChildren);
     ```

