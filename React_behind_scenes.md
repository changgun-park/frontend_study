#### React의 작동 방식

---

1. React & React-DOM

   React: jsx 코드 평가(evaluation). state, context에 변화가 있으면 재평가하여 snapshot을 찍고, 가장 마지막의 snapshot과 비교(diffing)한다.

   React-DOM: snapshot을 비교하여 바뀌어야 하는 부분만 "real" DOM에 render한다.



2. Evaluating vs Rendering

   Evaluating(평가, 혹은 코드 실행)은 Rendering과 다르다. React-DOM은 차이점이 있는 부분만 Rendering한다.



3. 자식 Component관련 헷갈릴 수 있는 것..

   ```react
   import React, { useState } from "react";
   import Button from "./components/UI/Button/Button";
   import "./App.css";
   import DemoOutput from "./components/Demo/DemoOutput";
   
   function App() {
     const [showParagraph, setShowParagraph] = useState(false);
   
     const toggleParagraph = () => {
       setShowParagraph((prev) => !prev);
     };
   
     console.log('parent running')
     return (
       <div className="app">
         <h1>Hi there!</h1>
         <DemoOutput show={false}></DemoOutput>
         <Button onClick={toggleParagraph}>Show Paragraph</Button>
       </div>
     );
   }
   
   export default App;
   
   ```

   ```react
   import React from "react";
   
   const DemoOutput = (props) => {
     console.log('Child running')
     return <p>{props.show ? "This is new!" : ""}</p>;
   };
   
   export default DemoOutput;
   
   ```

   ​	:question:자식 Component에서 state를 props로 전달 받지 않는다면, 자식 component는 실행되는 걸까 실행되지 않는 걸까? 예를 들		어, DemoOutput에 show prop으로 showParagraph가 아니라, false를 전달한다면?

   ​	:bulb: 자식 Component도 <b>부모 Component 코드의 함수</b> 중 하나 이기 때문에, 실행된다.

   ​	:bulb: Dom 요소의 변화는 없기 때문에, Rendering 되는 것은 아무것도 없다.

   

4. 불필요한 코드 실행을 막는 방법: 최적화

   ```react
   import React from "react";
   
   const DemoOutput = (props) => {
     console.log('Child running')
     return <p>{props.show ? "This is new!" : ""}</p>;
   };
   
   export default React.memo(DemoOutput); 
   ```

   * prop의 state가 변경되었을 때만 코드가 실행되고 재평가 될 수 있게 해준다.
   * class형 component에선 통하지 않는다.
   * component를 재평가 ---> props를 비교하는 문제로 바꾸어 버림.
   * 큰 프로젝트 일수록 memo를 사용할 이점이 커짐.

   

5. <b>props가 안바뀌면 절대 재평가 되지 않나?</b>

   NO. 함수를 지정한 경우를 살펴보자

   ```react
   import React, { useState } from "react";
   import Button from "./components/UI/Button/Button";
   import "./App.css";
   import DemoOutput from "./components/Demo/DemoOutput";
   
   function App() {
     const [showParagraph, setShowParagraph] = useState(false);
   
     const toggleParagraph = () => {
       setShowParagraph((prev) => !prev);
     };
   
     return (
       <div className="app">
         <h1>Hi there!</h1>
         <DemoOutput show={showParagraph}></DemoOutput>
         <Button onClick={toggleParagraph}>Show Paragraph</Button>
       </div>
     );
   }
   
   export default App;
   
   ```

   * App.js의 toggleParagraph는 App.js가 실행될때마다 다시 생성된다.

   * 따라서, onClick prop이 항상 '바뀐 것'으로 간주되어, Button도 재평가된다.

   * 앞선 예시와의 차이점은 무엇인가??

     ```react
     props === props.previous....를 비교할 때
     
     primitive value의 특징!
     false === false // true
     'hi'  === 'hi'  // true
     ['a', 'b'] === ['a', 'b'] // false
     ```

   * 그럼 도대체 Memo()의 쓸모가 뭔데?? props에 object가 있으면 무조건 실행되는데!

     => useCallback

     

6. useCallback

   ```react
   let obj1 = {};
   let obj2 = {};
   obj1 === obj2; // false
   
   obj2 = obj1;
   obj1 === obj2; // true
   ```

   ```react
   import React, { useState } from "react";
   import Button from "./components/UI/Button/Button";
   import "./App.css";
   import DemoOutput from "./components/Demo/DemoOutput";
   
   function App() {
       
     const [showParagraph, setShowParagraph] = useState(false);
   
     const toggleParagraph = useCallback(() => {
       setShowParagraph((prev) => !prev);
     }, []);
   
     return (
       <div className="app">
         <h1>Hi there!</h1>
         <DemoOutput show={showParagraph}></DemoOutput>
         <Button onClick={toggleParagraph}>Show Paragraph</Button>
       </div>
     );
   }
   
   export default App;
   ```

   * useCallback에 첫번째 인자로 object를 넘겨주면, react가 메모리 상 어딘가에 해당 object를 저장하고, component가 실행될때마다 해당 object를 참조하도록 한다.
   * 두번째 인자로는 useEffect와 유사하게 배열을 전달한다. 배열은 dependecy를 포괄한다. 빈 배열을 전달하면 항상 똑같은 object가 저장된다.





