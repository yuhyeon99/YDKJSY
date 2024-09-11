# CHAPTER 2. 자바스크립트 조망하기

### 2.1 파일은 프로그램입니다

- JS에서는 파일 각각이 별도의 프로그램
    - 그러므로 각 파일별 파싱/컴파일 과정 중에 오류가 발견되면 다음 이후에 작업할 파일들은 처리되지 않는다.
- 웹 프로젝트는 빌드 도구를 써서 분리된 파일들을 하나로 합친 후 배포를 진행, 이 경우 JS는 합쳐진 단일 파일을 전체 프로그램으로 취급
    - ex) Webpack, vite
- 독립적인 .js 파일 여러 개를 하나의 프로그램으로 작동시키는 유일한 방법
    - **전역 스코프**를 사용해 파일 간 상태를 공유 및 접근
- ES6 이후로 JS는 모듈 포맷도 지원하기 시작
    - 이전에는 독립형 JS 프로그램 포맷을 지원했음(예: )
    - 모듈로 분리한 파일 사용 방법
        - `import` 문을 사용하여 읽기
        - `<script type=module>` 태그를 추가하여 읽기
    - JS는 각 모듈을 별도로 처리

### 2.2 값

- JS에서 값은 크게 **원시 타입**과 **객체 타입**으로 분류
    - 원시 타입: **문자열**(`’abc’`), **숫자**(`123`), **불리언**(`true, false`) 그리고 `null`과 `undefined`
    - 객체 타입: 배열([]), 객체({})
- 리터럴(literal) 이란?
    - alert(’안녕하세요’) 에서 ‘안녕하세요’ 와 같이 변수를 통하지 않고 코드 그 자체로 프로그램에 값을 주입할 때 사용
    - 큰 따음표(””), 작은 따음표(’’), 백틱(``) 사용 가능
        - 백틱(``)은 **변수 표현식**(`${…}`) 을 사용해서 문자열 내 **변수로 보간(사이를 채운다는 뜻)**이 가능 **(※ 변수 표현식이 필요 없다면 굳이 백틱을 쓰지 않는게 덜 헛갈림!)**
        - 코드 가독성과 유지 보수를 위해 프로그램 전반에 걸쳐 일관성 있는 표기를 사용해야함
- JS는 문자열, 숫자, 불리언 이외에도 null과 undefined라는 원시 타입을 지원
    - 둘 다 **비어 있음(혹은 존재하지 않음)** 을 나타낼 때 사용
    - 정확하게 구분지어 일관성 있게 사용해야함
    - 비어 있는 단일 값을 나타낼 때는 undefined를 사용하는 게 가장 안전한 최선의 방법
        - 하지만 `코어 자바스크립트` 책에서는 이와 반대로 말한다.
            
            > var 변수를 사용할 때 ‘값을 대입하지 않은 변수’ 는 JS엔진에서 
            직접 undefined를 할당합니다.
            이 경우는 우리의 통제 범위를 벗어나므로 직접 undefined를 할당해 줄 경우 혼란을 느낄 여지가 있다.(내가 undefined를 할당한 건지 아님 엔진에서 할당한 건지..)
            
            **고로 비어 있음을 의미하는 상황에선 null로 통일하자~**
            > 
            
            개인적인 생각으론 let 과 const를 사용할거면 굳이 위 사례를 신경쓰지 않아도 된다고 생각함.
            
    - **null 주의사항**
        - typeof null이 object 라는 점.(null은 **객체 타입**이 아닌 **원시 타입**인데?)
            - `typeof null`이 'object'로 나오는 이유는 초창기 자바스크립트 설계에서 `null`이 객체의 타입 태그와 결합된 "null 포인터"(메모리 상 **특정 위치(0)**를 가리킴)로 처리되었기 때문.
        - 이는 자바스크립트 자체 버그이며, 
        (참조:  https://2ality.com/2013/10/typeof-null.html?ck_subscriber_id=727966771)
        - 기존 코드와의 호환성을 위해 수정되지 않았고, 앞으로도 수정되지 않을 듯…

### 2.2.1 배열과 객체

- 배열은 인덱스라는 숫자를 통해서 요소에 접근
    
    ```jsx
    // 예시
    const arr = [1, 2, 3];
    console.log(arr[0]); // 1
    ```
    
- 객체는 요소 위치를 알려주는 문자열(키 OR 프로퍼티라고 부름)을 사용해 요소에 접근
    
    ```jsx
    // 예시
    const obj = {'fisrt' : 1, 'second': 2, 'third': 3};
    console.log(obj.first, obj['third']) // 1 3
    ```
    

### 2.2.2 값의 타입

- typeof 연산자를 사용해 원시 타입 값과 객체 타입 값을 구분

```jsx
typeof 42; // "number"
typeof "abc" // "string"
typeof true; // "boolean"
typeof undefined // "undefined"
typeof null // "object" => 버그!
typeof { "a": 1 } // "object"
typeof [1, 2, 3] // "object"
typeof function hello(){}; // "function"
```

### 2.3 변수 선언과 사용

- **let 과 var**
    - **`var`**
        - **함수 스코프**
            
            ```jsx
            function example() {
              if (true) {
                var x = 10;
              }
              console.log(x);  // 10 (블록 외부에서도 접근 가능)
            }
            example();
            ```
            
    - **`let`**
        - **블록 스코프**
        
        ```jsx
        function example() {
          if (true) {
            let y = 10;
          }
          console.log(y);  // ReferenceError: y is not defined (블록 외부에서 접근 불가)
        }
        example();
        ```
        
    - 변수의 접근 범위를 블록으로 한정 지으면 프로그램 내 변수의 영향 범위를 제한해 변수 이름 중복을 막아준다는 측면에서 유용
        
        ```jsx
        function example() {
            let x = 10;  // 함수 내에서 사용되는 변수 x
        
            if (true) {
                let x = 20;  // 블록 내에서만 유효한 변수 x (이름이 같아도 문제 없음)
                console.log("블록 내부의 x:", x);  // 20
            }
        
            console.log("블록 외부의 x:", x);  // 10 (외부의 x에 영향을 주지 않음)
        }
        
        example();
        ```
        
    - `var` 은 **‘변수를 좀 더 넓은 범위에서 사용’** 할 때 유용
- **const**
    - 선언한 변수를 재할당 불가 ( 값을 바꿀 수는 있음 ex. 객체, 배열 그러나 혼란스러울 수 있으니 지양하는게 좋음)
        
        ```jsx
        const person = {
          name: 'John',
          age: 30
        };
        
        // 객체의 내부 값은 변경 가능
        person.age = 31;
        console.log(person.age);  // 31 (객체 내부 속성 값 변경)
        
        // 하지만 객체 자체를 재할당할 수는 없음
        // person = {};  // TypeError: Assignment to constant variable.
        
        const numbers = [1, 2, 3];
        
        // 배열의 내부 값은 변경 가능
        numbers.push(4);
        console.log(numbers);  // [1, 2, 3, 4] (배열에 값 추가)
        
        // 하지만 배열 자체를 재할당할 수는 없음
        // numbers = [5, 6];  // TypeError: Assignment to constant variable.
        ```
        
    - const는 간단한 원싯값에 이름을 부여할 때 적절

### 2.4 함수

- **함수 선언문:** 함수 선언으로 정의한 함수는 식별자와 실제 함수를 나타내는 값의 연관이 코드 실행 단계가 아닌 컴파일 단계에서 맺어짐.
    - **호이스팅(Hoisting)** 이라는 개념 덕분에 컴파일 단계에서 미리 함수의 식별자(함수 이름)과 함수 값(함수 본체)이 연결됨.
    - 이로 인해 어느 위치에서든 함수 선언 전에 해당 함수를 선언할 수 있음.
    
    ```jsx
    greet();  // "Hello, World!" (함수 선언 전에 호출 가능)
    
    function greet() {
      console.log("Hello, World!");
    }
    
    greet();  // "Hello, World!" (함수 선언 후에도 호출 가능)
    ```
    
- **함수 표현식**: 함수 식별자가 코드가 실행되기 전까지는 함수 값(함수 본체)관계를 맺지 않음
    
    ```jsx
    // greetExpression();  // TypeError: greetExpression is not a function
    
    const greetExpression = function() {
      console.log("표현식 하이!");
    };
    
    greetExpression();  // "Hello from expression!"
    ```
    
- 함수는 오로지 한 개의 값만 반환 가능. 여러 데이터를 반환하고 싶다면 객체나 배열로 감싸서 반환
- 함수는 값이므로 함수를 객체의 프로퍼티로 할당 가능
    
    ```jsx
    const person = {
      name: 'Alice',
      age: 25,
      
      farewell() {
        console.log('바이!');
      }
    };
    
    // 함수 호출
    person.farewell();  // "바이!"
    ```
    

### 2.5 비교

- 일치 비교(===):
    - **타입과 값**이 모두 값아야 `true` 를 반환함. 즉, **형 변환 없이** 값을 비교
    
    ```jsx
    1 === '1';  // false (타입이 다름)
    1 === 1;    // true (타입과 값이 모두 같음)
    ```
    
- 동등 비교(==):
    - **값**만 비교하며, **타입이 다를 경우** 자바스크립트가 **암묵적 형 변환**을 수행한 후 비교. 형 변환으로 인한 예기치 않은 결과가 발생할 수 있어서 주의!
    
    ```jsx
    1 == '1';   // true (타입이 다르지만 형 변환 후 비교)
    0 == false; // true (0과 false는 형 변환 후 같다고 간주됨)
    ```
    
- `NaN === NaN`이 `false`인 이유
    - JavaScript에서 **NaN은 자기 자신과도 같지 않다**는 규칙이 있습니다. 즉, `NaN`은 어떤 값과도 (심지어 자기 자신과도) **일치하지 않습니다**.
    - NaN과 비교할 때는 거짓말을 하지 않는 `Number.isNaN()`을 사용해라
- `0 === -0` 이 `true` 인 이유
    - **IEEE 754 부동소수점 표준**을 따르는데, 이 표준에서는 `+0`과 `-0`을 구별하지만, 대부분의 연산에서 둘을 동일하게 처리합니다. 그래서 **일치 비교(`===`)**나 **동등 비교(`==`)**를 할 때, `0`과 `-0`은 같다고 간주되어 `true`를 반환합니다.
- 비교 대상이 객체인 경우에는 값의 본질이나 내용이 아닌 **구조적 일치**를 비교하게 됨
    - 객체의 **내용과 구조**가 동일한지 비교. **서로 다른 객체**라도 **구조와 값이 동일**하면 `true` 반환
- JS에서는 **독자성 일치**를 비교함
    - **객체의 값이나 내용이 아닌, 객체의 메모리 참조**(즉, 객체가 메모리 상에서 저장된 위치)
        1. **데이터 공간**(Data Space)
            - **데이터 공간**은 실제 데이터가 저장되는 **메모리 영역**입니다.
        
          2. **데이터 참조 공간**(Reference Space)
        
        - **데이터 참조 공간**은 데이터가 저장된 **메모리 위치를 가리키는 주소**입니다.
        1. **객체 비교 시 중요한 개념:**
            - **객체 비교**는 **데이터 공간**을 비교하는 것이 아니라, **데이터 참조 공간**을 비교합니다. 즉, 객체가 저장된 실제 데이터가 아닌, 그 데이터가 저장된 위치(메모리 주소)를 비교하는 것입니다.
- 구조적 일치와 독자성 일치의 차이

| 비교 방식 | 설명 | 예시 결과 |
| --- | --- | --- |
| **독자성 일치** | 두 객체가 **같은 메모리 참조(주소)**를 가리키는지 비교. 값과 상관없이 **같은 객체**를 참조할 때만 `true` 반환 | `obj1 === obj2`는 `false` |
| **구조적 일치** | 객체의 **내용과 구조**가 동일한지 비교. **서로 다른 객체**라도 **구조와 값이 동일**하면 `true` 반환 | `deepEqual(obj1, obj2)`는 `true` |

### 2.5.2 강제 변환

- == 연산자는 강제 변환을 먼저 실행해 피연산자의 타입을 맞춘 이후에 === 연산자처럼 작동
- 동등 연산자는 설계가 불실하고 위험하며 버그가 많다는 의견이 다분 사용을 지양하는게 좋을듯.
- >=과 <= 또한 == 연산자와 동일하게 피연산자들의 타입이 같으면 ===처럼 작동

### 2.6 코드 구조화 패턴(업데이트중)

- JS 생태계 전반에 걸쳐 데이터와 행동 관점에서 코드를 구조화하는 패턴은 크게 **클래스**와 **모듈** 두가지가 있음.

### 2.6.1 클래스

- 클래스는 사용자가 정의한 데이터 ‘타입’으로 데이터와 이 데이터를 조작하는 동작이 들어감.
- 프로그램에서 사용할 수 있는 구체적인 값이 필요하다면 `new` 키워드를 사용해서 인스턴스를 만들어야 함
    
    ```jsx
    // 가구 클래스
    class Furniture {
    	constructor(name, width, height) {
    		this.name = name;
    		this.width = width;
    		this.height = height;
    	}
    	
    	print(){
    		console.log(`가구명: ${this.name}`);
    		console.log(`가구 너비: ${this.width}`);
    		console.log(`가구 높이: ${this.height}`);
    	}
    }
    
    // 집 클래스
    class House {
    	constructor(name) {
    		this.name = name;
    		// 가구 인스턴스를 담을 배열 필드 정의
    		this.furnitures = [];
    	}
    	
    	addFurniture(name, width, height) {
    		// 가구 인스턴스를 생성
    		const furniture = new Furniture(name, width, height);
    		// 가구 목록 배열에 인스턴스 추가
    		this.furnitures.push(furniture);
    	}
    	
    	print(){
    		console.log(`
    			우리 집 가구 개수는 ${this.furnitures.length}개 이고,
    			목록은 아래와 같아.
    		`);
    		for(let furniture of this.furnitures){
    			furniture.print();
    		}
    	}
    }
    
    let myHouse = new House('망푸집');
    myHouse.addFurniture('TV', 500, 100);
    myHouse.addFurniture('침대', 1000, 200);
    
    myHouse.print();
    // 우리 집 가구 개수는 2개 이고,
    // 목록은 아래와 같아
    // 가구명: TV
    // 가구 너비: 500
    // 가구 높이: 100
    // 가구명: 침대
    // 가구 너비: 1000
    // 가구 높이: 200
    ```
    
- 클래스의 동작(메서드)은 클래스만 가지고는 사용할 수 없음. ⇒ 인스턴스를 통해서만 호출
    - `클래스명.print()` 는 불가하지만, `인스턴스를담은변수.print()` 는 가능하다~

- **상속:** 클래스 지향 설계의 핵심 중 하나
    
    ```jsx
    class Cooking {
    	constructor(name, ingredients, recipe) {
    		this.name = name;
    		this.ingredients = ingredients;
    		this.recipe = recipe;
    	}
    	
    	print() {
    		console.log(`
    			요리명: ${this.name}
    			재료 목록: ${this.ingredients.join(', ')}
    			레시피: ${this.recipe}
    		`);
    	}
    }
    
    class Curry extends Cooking {
    	constructor(name, ingredients, recipe, spicyLevel) {
    		// 부모 클래스의 생성자를 호출하여 name, ingredients, recipe를 초기화
    		super(name, ingredients, recipe);
    		
    		// 자식 클래스만의 고유 속성
    		this.spicyLevel = spicyLevel; // 매운 정도 (1~5)
    	}
    	
    	// print 메서드 오버라이딩
    	print() {
    		// 부모 클래스의 print 메서드를 호출
    		super.print();
    		console.log(`매운 정도: ${this.spicyLevel}`);
    	}
    }
    
    class Ramen extends Cooking {
    	constructor(name, ingredients, recipe, noodleThickness) {
    		// 부모 클래스의 생성자를 호출하여 name, ingredients, recipe를 초기화
    		super(name, ingredients, recipe);
    		
    		// 자식 클래스만의 고유 속성
    		this.noodleThickness = noodleThickness; // 면의 굵기
    	}
    	
    	// print 메서드 오버라이딩
    	print() {
    		// 부모 클래스의 print 메서드를 호출
    		super.print();
    		console.log(`면의 굵기: ${this.noodleThickness}`);
    	}
    }
    
    const curry = new Curry(
    	"Spicy Curry", 
    	["카레 가루", "감자", "당근", "양파", "고기"], 
    	"1. 재료를 썬다. 2. 재료를 볶는다. 3. 물을 붓고 끓인다. 4. 카레 가루를 넣는다.", 
    	4
    );
    curry.print();
    
    const ramen = new Ramen(
    	"Shoyu Ramen", 
    	["라면", "물", "라면 스프", "계란", "파"], 
    	"1. 물을 끓인다. 2. 라면과 스프를 넣는다. 3. 계란을 풀어 넣고, 파를 추가한다.", 
    	"얇음"
    );
    ramen.print();
    ```
    
    - Curry와 Ramen 클래스 모두 extends라는 키워드를 사용해 Cooking 클래스에서 정의한 동작을 **확장**해서 사용하고 있음.
    - `super()` 는 부모 클래스인 Cooking의 생성자를 자식 클래스에서도 사용할 수 있도록 동일한 작업을 하는 코드를 다시 작성하지 않아도 초기화할 수 있음.
    - 두 자식 클래스 인스턴스를 통해 부모 클래스인 Cooking에서 상속받아 새롭게 **재정의(`overriding`)**한 메서드인 print()를 호출할 수 있다는 점 ⇒ 객체의 특성 중 **다형성**에 해당

### 2.6.2 모듈

- **클래식 모듈**
    - 클래식 모듈의 주요 특징은 최소한 한 번 이상 실행되는 외부 함수입니다.
    - **외부 함수**는 모듈 **인스턴스 내부의 숨겨진 데이터**를 **대상으로 작동하는 함수**가 있는 ‘인스턴스’를 반환합니다. **⇒ 모듈 패턴**과, **클로저**를 활용한 **데이터 은닉**에 대한 설명
        - 즉, 데이터는 **숨겨져 있고** (private), 그 데이터에 접근하거나 조작할 수 있는 함수만 **외부에 노출**되는 방식입니다.
    - **인스턴스**: 모듈은 클로저로 감싸진 함수들을 반환하는데, 이 함수들이 포함된 객체(또는 함수 자체)를 **인스턴스**라고 할 수 있습니다.
    
    ```jsx
    // 모듈 패턴 예제
    const CounterModule = (function() {
        // 외부에서 접근할 수 없는 숨겨진 데이터 (private 변수)
        let count = 0;
    
        // 모듈이 반환하는 객체
        return {
            // 숨겨진 count 변수를 조작하는 함수
            increment: function() {
                count++;
                console.log(`현재 카운트: ${count}`);
            },
            decrement: function() {
                count--;
                console.log(`현재 카운트: ${count}`);
            },
            getCount: function() {
                return count;
            }
        };
    })();
    
    // 사용 예
    CounterModule.increment(); // 현재 카운트: 1
    CounterModule.increment(); // 현재 카운트: 2
    CounterModule.decrement(); // 현재 카운트: 1
    
    // count 변수에 직접 접근할 수 없음 (외부에서 count는 undefined)
    console.log(CounterModule.count); // undefined
    
    // count 값을 얻는 방법은 오직 getCount 함수를 사용하는 것
    console.log(CounterModule.getCount()); // 1
    ```
    
    ### 설명:
    
    1. **숨겨진 데이터**: `let count = 0;`라는 변수는 `CounterModule` 모듈 내부에서만 존재하며 외부에서는 접근할 수 없습니다. 외부에서 `count`를 직접 조회하려고 해도 `undefined`입니다.
    2. **클로저**: 반환된 객체 (`increment`, `decrement`, `getCount` 메서드를 가진 객체) 내부의 함수들은 클로저를 통해 `count` 변수에 접근할 수 있습니다. 즉, 이 함수들은 자신이 생성될 때의 환경(즉, 모듈 내부의 `count` 변수)을 계속 기억합니다.
    3. **모듈 인스턴스**: `CounterModule`은 함수 실행의 결과로 하나의 객체를 반환합니다. 이 객체가 "모듈 인스턴스"로서 작동하며, 외부에서 이 인스턴스의 메서드만 호출할 수 있고, 인스턴스 내부의 데이터 (`count`)에는 접근할 수 없습니다.
    
    **⭐ 모듈 인스턴스는 호출되자마자 생성되고 이후에는 최초로 생성된 인스턴스만을 가지고 활용된다. (예시 코드에서는 즉시 실행 함수로 CounterModule 변수에서 정의와 동시에 인스턴스가 생성되고 이후 활용됨)**
    
- **ES 모듈(이하 ESM)**
    - ES 모듈에는 **모듈을 정의**하는 **래핑 함수**가 없음.
    - ES 모듈을 사용할 때 모듈 API와 직접 상호작용 하지 않음
    - export 키워드를 사용해 변수나 메서드를 퍼블릭 API로 정의
    - ESM을 인스턴스화하지 않아도 `import 키워드를 사용해 가져오기만 한다면 단일 인스턴스처럼 사용할 수 있다`. 프로그램 내에서 import 키워드를 사용해 처음 모듈을 가져온 순간 인스턴스가 생기고, 동일한 모듈을 다른 곳에서 import 할 때는 이미 생성된 모듈의 참조만 가져옵니다. 이런 관점에서 ES 모듈은 사실상 싱글턴singleton이라 할 수 있습니다. 인스턴스 여러 개가 필요한 경우에는 ES 모듈을 정의할 때 클래식 모듈 스타일의 팩토리 함수를 작성하면 됩니다.
    - ES 모듈과 클래식 모듈을 합쳐놓은 예시
    
    ```jsx
    // 클래식 모듈 패턴 부분
    function printDetails(title, author, pubDate) {
    	console.log(`
    		제목: ${title}
    		저자: ${author}
    		발행일: ${pubDate}
    	`);
    }
    
    // ES 모듈: create 함수를 export 하여 외부에서 사용할 수 있게 함
    export function create(title, author, pubDate) {
    	// 클래식 모듈 패턴 부분
    	var publicAPI = {
    		print() {
    			printDetails(title, author, pubDate); // 내부 데이터를 클로저로 은닉
    		}
    	};
    	
    	return publicAPI; // 클래식 모듈 패턴: publicAPI 객체를 반환하여 외부에서 함수만 접근 가능
    }
    ```
    
    ```jsx
    // ES 모듈: publication.js에서 create 함수 import (모듈 시스템 활용)
    import { create as createPub } from './publication.js';
    
    // 클래식 모듈 패턴 부분
    function printDetails(pub, URL) {
    	pub.print(); // publication 모듈의 print 함수 호출
    	console.log(URL); // 추가 정보 출력
    }
    
    // ES 모듈: create 함수를 export 하여 외부에서 사용할 수 있게 함
    export function create(title, author, pubDate, URL) {
    	// ES 모듈의 함수를 호출하여 클래식 모듈 생성
    	var pub = createPub(title, author, pubDate); 
    	
    	// 클래식 모듈 패턴 부분
    	var publicAPI = {
    		print() {
    			printDetails(pub, URL); // 내부 데이터와 함수를 숨기고 외부에서 접근 가능하게 설정
    		}
    	};
    	
    	return publicAPI; // 클래식 모듈 패턴: 객체를 반환하여 외부에서 print 함수만 접근 가능
    }
    ```
    
    ```jsx
    // ES 모듈: blogpost.js에서 create 함수 import (모듈 시스템 활용)
    import { create as newBlogPost } from './blogpost.js';
    
    // 클래식 모듈 인스턴스 생성
    var forAgainstLet = newBlogPost(
    	"For and against let",
    	"카일 심슨",
    	"2014년 10월 27일",
    	"https:// ~~~"
    );
    
    // 클래식 모듈의 public 메서드 호출
    forAgainstLet.print(); // 내부 데이터(title, author, pubDate, URL)가 printDetails를 통해 출력됨
    ```

# 문제

### [슈슉님]

**Q1. 자바스크립트의 원시 타입(Primitive Type) 값을 기술하시오 (총 7개)**

A1. `number`, `string`, `boolean`, `null`, `undefined`, `symbol`, `bigint`

**Q2. var로 선언하는 경우 프로그램 내 변수의 영향 범위를 제한해 변수 이름 중복을 막아준다는 측면에서 유용하다. (O / X)**

A2. `X`

**Q3. 함수는 여러 개의 값을 반환할 수 있다. (O / X) + 이유를 함께 서술하시오**

A3. `O`. 함수는 기본적으로 하나의 값만 반환할 수 있지만, 배열이나 객체로 감싸서 반환하는 경우 여러 개의 값을 반환할 수 있다.

**Q4. 한 타입의 값이 다른 타입의 값으로 변하는 것을 의미하는 용어는?**

A4. 타입 강제변환 (`type coercion`)

---

### [은도님]

**Q1. typeof Number(“a”) === typeof Number(“a”)**

A1. `false` (NaN === NaN이 되기 때문)

**Q2 ~ Q4 참고**

```
const new_arr = [1, 2, 3, 4, 5];

```

**Q2. console.log(typeof new_arr) ?**

A2. `object`

**Q3. new_arr.splice(0,1); console.log(new_arr) ?**

A3. `[2, 3, 4, 5]`

**Q4. new_arr = new_arr((el) => el / 2 === 0) 배열값 가능?**

A4. 불가능. 이유: `const` 키워드를 가진 변수는 재할당 불가.

---

### [호두님]

**Q1. JS에서 값은 크게 ___ 타입과 ___ 타입으로 분류된다. (p55)**

A1. 원시타입, 객체타입

---

### [폴님]

**Q2. JS에서 백틱(``)으로 감싼 문자열에 변수 표현식을 사용하는 방식을 뭐라고 하나요?**

A2. 보간법

**Q4. 아래의 코드의 console.log의 결과값은?**

```
var myName = "폴";
var yourName = myName;
myName = "망푸";

```

1. `console.log(myName);`
A4.1. 망푸
2. `console.log(yourName);`
A4.2. 폴
\- 원시타입의 값이 독립적인 것에 대한 증명 (부록 A.1)

**Q5. JS의 변수 선언 시 `var`과 `let`의 접근 범위는 어떻게 되나요?**

A5. 함수스코프, 블록스코프

---

### [망푸님]

**Q1. JS에서는 독립적인 .js 파일 여러 개를 하나의 프로그램으로 작동시키기 위한 방법은 무엇인가요?**

A1. 전역 스코프를 사용해 파일 간 상태를 공유 및 접근한다.

**Q2. NaN === NaN이 false인 이유는 무엇인가요?**

A2. JavaScript에서 NaN은 자기 자신과도 같지 않다는 규칙이 있기 때문입니다.

**Q3. 객체 비교에서 "구조적 일치"와 "독자성 일치"의 차이는 무엇인가요?**

A3.

- **구조적 일치**는 객체의 **내용과 구조**가 동일하면 `true`를 반환합니다.
- **독자성 일치**는 객체가 **같은 메모리 참조**를 가리킬 때만 `true`를 반환합니다.

---
