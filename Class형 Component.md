#### Class형 Component

---

1. 클래스형 컴포넌트란?

   리액트에서 컴포넌트를 만드는 방법은 함수형이 기본이다. 이는 리액트 v16.8에서 추가된 것이다.. 이전에는 클래스형 컴포넌트를 정의해야만 state를 관리할 수 있었다. 16.8에서 추가된 함수형 컴포넌트는 클래스형 컴포넌트를 대체하면서, 리액트의 "훅"또한 추가되었다. 클래스형 컴포넌트의 생김새는 다음과 같다.

   ```react
   class Sample extends Component {
   	render() {
   		return <h1>Sample</h1>
   	}
   }
   ```

   클래스형 컴포넌트는 어디까지나 함수형 컴포넌트의 대체재일 뿐이다. 요즘은 대부분 함수형 컴포넌트를 사용하고 있는데, 이유는 클래스형 컴포넌트에서 훅을 사용할 수 없기 때문이다. 클래스형에서 훅을 사용하기 위해서는 적절한 변환이 필요하다.

   그럼에도 불구하고, 클래스형 컴포넌트를 학습해야 하는 이유는, third-party 라이브러리나 레거시 코드를 가져와 사용할 때 여전히 클래스형 컴포넌트가 많이 활용되기 때문이다.



 2. 함수형 컴포넌트를 클래스형으로 바꿔보기

    ```react
    const User = (props) => {
      return <li className={classes.user}>{props.name}</li>;
    };
    ```

    ```react
    class User extends Component {
      render() {
        return <li className={classes.user}>{this.props.name}</li>;
      }
    }
    ```

    * 클래스형 컴포넌트에서는 this를 통해 props에 접근해야 한다.

    ```react
    const Users = () => {
      const [showUsers, setShowUsers] = useState(true);
    
      const toggleUsersHandler = () => {
        setShowUsers((curState) => !curState);
      };
    
      const usersList = (
        <ul>
          {DUMMY_USERS.map((user) => (
            <User key={user.id} name={user.name} />
          ))}
        </ul>
      );
    
      return (
        <div className={classes.users}>
          <button onClick={toggleUsersHandler}>
            {showUsers ? "Hide" : "Show"} Users
          </button>
          {showUsers && usersList}
        </div>
      );
    };
    ```

    ```react
    class Users extends Component {
      constructor() {
        super();
        this.state = {
          showUsers: true,  
        }; // state는 언제나 object이다! 개중요함.
      }
    
      toggleUsersHandler = () => {
        this.setState((curState) => {
          return { showUsers: !curState.showUsers };
        });
      };
    
      render() {
        const usersList = (
          <ul>
            {DUMMY_USERS.map((user) => (
              <User key={user.id} name={user.name} />
            ))}
          </ul>
        );
    
        return (
          <div className={classes.users}>
            <button onClick={this.toggleUsersHandler.bind(this)}>
              {this.state.showUsers ? "Hide" : "Show"} Users
            </button>
            {this.state.showUsers && usersList}
          </div>
        );
      }
    }
    ```

    * state는 어떻게 관리하나?

      this.state에 관리해야하는 state 정의, 언제나 오브젝트형태로 정의한다.

      state를 업데이트 할때 주의 해야하는 사항: 오브젝트 형태로 반환

      ```
      클래스형 컴포넌트와 함수형 컴포넌트의 state 관리 차이점
      
      * 함수형은 덮어쓰기
      * 클래스형은 수정
      ```

    * bind(this)

      위의 예제에서 bind(this)를 사용하지 않으면, 오류가 발생한다. 이는 this가 동적으로 해당 함수를 호출한 객체에 바인딩되기 때문이다.

      https://academind.com/tutorials/this-keyword-function-references
      
      
