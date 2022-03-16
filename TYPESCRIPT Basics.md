### TYPESCRIPT Basics

---

* static typing[정적 타이핑]

  타입스크립트를 사용하는 가장 큰 이유라고 할 수 있다. 자바스크립트는 dynamic typing[동적 타이핑] 언어이다. 다음 예시를 보자

  ```javascript
  function add(a, b) {
      return a + b;
  }
  
  const result = add(2, 5);
  
  console.log(result);
  ```

  콘솔에는 다음과 같이 출력된다.

  ```
  7
  ```

  이번에는 인수로 전달되는 값을 문자열로 바꿔 보자

  ```javascript
  function add(a, b) {
      return a + b;
  }
  
  const result = add("2", "5");
  
  console.log(result);
  ```

  콘솔에는 다음과 같이 출력된다. 자바스크립트가 문자열을 합쳤다.

  ```
  25
  ```

  두번째 예시에서 볼 수 있듯이, 자바스크립트는 동적 타이핑을 지원하기 때문에 인수로 문자열이 전달되든, 숫자가 전달되든 에러 없이 함수가 실행된다. 하지만 개발자가 의도한 것은 숫자를 더하는 기능일 수 있다. 따라서, 문자열이 전달되는 상황은 원천차단해야 한다.

  이와 같은 문제를 타입스크립트를 통해 해결할 수 있다. 타입스크립트로 '의도'에 맞게 코드를 작성해보자.

  ```typescript
  function add(a: number, b: number) {
    return a + b;
  }
  
  const result = add(2, 5);
  
  console.log(result);
  
  ```

  add 함수에 전달되는 인자 a, b에 number 라는 타입을 명시해주었다. 코드에디터에서 인자를 '2', '5'와 같이 문자열로 전달해주면 에러가 발생한다.

* Types

  타입스크립트에서는 다음과 같이 타입을 명시할 수 있다.

  ```typescript
  // Primitive(number, string, boolean)
  
  let age: number = 24;
  
  let userName: string;
  
  userName = "joe"
  
  let isStudent: boolean;
  
  isStudent = true;
  
  // Complex(Array, Object)
  
  let hobbies: string[]; // 문자열의 배열
  
  hobbies = ["tennis", "game"]
  
  let person: {
      age: number;
      name: string;
  } // 객체
  
  person = { age: 27, name: 'scarlet'}
  
  // 객체의 배열은?
  
  let people: {
      age: number;
      name: string;
  }[];
  ```

* Type Inference[타입 참조]

  타입을 명시하지 않아도 타입스크립트가 "알아서" 타입을 참조한다.

  ```typescript
  let course = 'computer science'
  ```

  위 코드에서 course는 string 타입을 참조하게 된다. 일반적으로, 타입 참조를 적극 활용하는 것이 권장된다.

* Type Union[타입 유니온]

  굳이 타입을 한가지로 한정할 필요는 없다.

  ```typescript
  let person: string | string[];
  
  person = 'john'
  
  person = ['john', 'david']
  ```

* Type Alias[타입 별칭]

  직접 타입을 정의하여 사용할 수 있다.

  ```typescript
  type Person = {
  	name: string;
  	age: number;
  }
  
  let person: Person;
  
  person = { name: 'jackson', age: 30}
  
  let people: Person[] = [
      {
          name: 'john',
          age: 27
      },
      {
          name: 'david',
          age: 28
      }
  ]
  ```

* 함수

  ```typescript
  // function add(a: number, b: number): number
  
  function add(a: number, b: number) {
  	return a + b;
  }
  ```

  add 함수에서 return 값에 대한 type을 명시하지 않았음에도 불구하고 타입스크립트가 '알아서' number 값을 참조하고 있다.

  return 값이 없다면 어떻게 될까?

  ```typescript
  // function printOutput(value: any): void
  
  function printOutput(value: any) {
    console.log(value);
  }
  ```

  'void'라는 타입이 참조되었음을 알 수 있다.

* <strong>Generics</strong>

  ```typescript
  function insertAtBeginning(array: any[], value: any) {
    const newArray = [value, ...array];
    return newArray;
  }
  
  const demoArray = [1, 2, 3];
  
  // const updatedArray: any[]
  const updatedArray = insertAtBeginning(demoArray, -1);
  ```

  updatedArray 변수는 [-1, 1, 2, 3]이라는 배열을 참조하게 된다.

  여기서 어떤 문제가 발생할까?

  updatedArray에 커서를 가져가 보면, 배열 요소의 타입이 'any'로 되어있는 것을 확인할 수 있다. any라는 타입을 가지게 되는 것은 typescript를 사용하는 목적에 정면으로 충돌된다. 그렇다면 다음과 같이 문제를 해결하면 되지 않을까?

  ```typescript
  function insertAtBeginning(array: string[], value: string) {
  	...
  }
  ```

   insertAtBeginning 함수의 인자에 타입을 string으로 명시했다. 여기서 다른 문제가 생긴다. 만약 개발자가 이 함수를 문자열의 배열에도 사용하고 싶다면? 아예 새로운 함수를 만들어야 할 것이다.

  이와 같이, 범용성과 안정성의 문제를 모두 해결하기 위해서 사용되는 것이 Generics이다. 사용 방법은 다음과 같다.

  ```typescript
  function insertAtBeginning<T>(array: T[], value: T) {
    const newArray = [value, ...array];
    return newArray;
  }
  ```

  이제 문자열로 이루어진 배열에서도 해당 함수를 '안전'하게 사용할 수 있게 되었다.

