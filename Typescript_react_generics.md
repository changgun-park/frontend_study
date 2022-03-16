### 타입스크립트 컴포넌트에 props 전달하기

---

* 리액트에서 컴포넌트를 일반적으로 만드는 것처럼 컴포넌트를 만들어 보자.

  만들어 볼 컴포넌트는 props를 통해 todo 리스트를 받아서 출력하는 간단한 컴포넌트이다.

  (todo는 문자열의 배열이다. ['study react', 'study typescript'])

  ```typescript
  import React from "react";
  
  const Todos = (props) => {
    return (
      <ul>
        {props.items.map((item) => (
          <li>{item}</li>
        ))}
      </ul>
    );
  };
  
  export default Todos;
  ```

  props에 type을 지정해주지 않았기 때문에 IDE에서 오류를 확인할 수 있다.

  -> 'Parameter 'props' implicitly has an 'any' type.'

* 이번에는 props에 타입을 지정해서 인자로 넘겨주자.

  ```typescript
  const Todos = (props: { items: string[] }) => {
    return (
      <ul>
        {props.items.map((item) => (
          <li>{item}</li>
        ))}
      </ul>
    );
  };
  
  export default Todos;
  ```

  일단은 멀쩡해보인다. 다만 문제가 있다.

  <strong>리액트 컴포넌트에는 언제나 children이라는, 특수한 prop이 존재한다.</strong> :sweat: 어떤 컴포넌트던지 간에, children은 모든 컴포넌트에서 props를 통해 전달 받는다. 따라서 children도 명시해 주어야 한다.

  ```
  const Todos = (props: { items: string[], children }) => {
    return (
      <ul>
        {props.items.map((item) => (
          <li>{item}</li>
        ))}
      </ul>
    );
  };
  
  export default Todos;
  ```

  다시 문제가 발생했다. children은 타입을 뭘로 지정해 주어야 하는 걸까??

  children의 타입을 지정할 수 있겠으나, 이런 작업을 모든 컴포넌트에서 반복할 수는 없다.



* React.FC를 통해 리액트 컴포넌트(함수)를 제너릭으로 선언하기

  ```typescript
  import React from "react";
  
  const Todos: React.FC<{ items: string[] }> = (props) => {
    return (
      <ul>
        {props.items.map((item) => (
          <li>{item}</li>
        ))}
      </ul>
    );
  };
  
  export default Todos;
  
  ```

  React.FC를 통해 함수 선언을 하게 되면, 해당 함수가 리액트 컴포넌트로서, Generic 타입임을 알려줄 수 있다.

  FC는 Functional Component의 줄임말이다. 이와 같이 선언하게 되면, children prop을 자동으로 받게 된다.

  그리고, props를 통해 전달할 것이 추가적으로 있다면(위의 경우 items) 괄호를 열어 따로 선언하면 된다.

* <strong>위의 방법은 모든 문제를 과연 해결해 주는걸까....?</strong>

  ~~또 문제가 발생했다.~~ :cry:

  해당 방법은 컴포넌트가 '암묵적'으로 props를 받게 해버린다. 즉, 타입스크립트를 사용하는 목적과 정면으로 위배된다. 하지만 더 깊게 들어가버리면 현재 학습하고 있는 곳에서 진도를 못나가니까....여기까지만 하도록 하자.

  