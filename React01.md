#### 0. React
웹페이지를 동적으로 구현하는 대표적인 방식인 MVC 모델에서
V를 통한 사용자의 입력에 의해 새로운 웹페이지를 구성하여 
다시 렌더링하는 방식을 사용했다.

하지만 페이지 전체를 다시 구성하고 로드하는 방식은 코스트가 너무크다.

그대안으로 나온 방식이 React 와 같은 반응형 웹 프론트엔드 라이브러리 or 프레임워크에서 사용하는

가상 DOM 이다.

가상 DOM에 새로 추가된 요소들을 추가하고, 바로 이전의 DOM과 비교하여 변경된
Element들만 반영하여 업데이트 하는 방식이다.

#### 1. JS 표현식, React Element
JSX 표현식 : JSX 문법으로 작성한 표현식은 
		React Element 객체를 반환한다.

React Element 객체        JSX 표현식
```javascript
const element       =    <h1>Hello, World!</h1>;
```

### 1.JSX에 표현식 포함하기
HTML
```html
<div id='root'></div>
```

```javascript
const name = "Im kk";
const elment = <h1>Hello, {name}</h1>;
			//{}이 내부에 자바스크립트 코드 삽입 가능.
			//함수호출, 변수, 기타계산,표현식등 모두가능.
ReactDOM.render(
	element,
	document.getElementById('root');
)
```

### 2. JSX에 함수식 넣기
```javascript
function formatName(user){
	return user.firstname + user.lastname;
}

const user = {
	firstname:"Lee",
	lastname:"KK"
};

const element = (
	<h1>
		Hello, {formatName(user)}!
	</h1>
);

ReactDOM.render(
	element,
	docu..('root')
);
```

### 3. 함수 인자, 리턴값으로 사용가능. JS의 표현식의 일종이므로 가능.
```javascript
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### 4. property
```javascript
const element = <div tabIndex="0" className={user.avatarUrl}></div>;
```
기존의 html 프로퍼티 이름과 조금씩 다르다.
		id는 동일, class -> className 같이..
    Java와 동일한 Camel 로 변환해야 한다.


### 5. ReactElement 객체
```javascript
const element = ( //이 표현식 자체가 실제 객체의 모양을 나타내는 것이 아니다.
	<h1 className="greeting" ...>
	Hello,World!
	</h1>	
); // 이건 사실 내부적으로는 이런 React클래스,객체의 메서드를 호출하여

const element = React.createElement(
	'h1',
	{className:'greeting' ...},
	'Hello World!	
);
```

이것과 동일한 과정임. 즉 클래스의 객체 생성자를 사용하는 것과 동일하다.
그결과로 생성되는 실제 객체의 모양은 다음과 같다.

```javascript
const element = {
	type: 'h1',
	props: {
		className : 'greeting',
		children : 'Hello, World'
	}
}; //이것이 실제ReactElement 객체이다.
```
즉 JSX 표현식은 내부적으로 React.createElement 객체 생성자 함수를 호출하여 
	ReactElement 객체를 생성하는 함수이다.




#### 2. 컴포넌트

- Element : 화면에 표현할 가장 기본적인 단위, js에서 다루는 객체 단위.

- Component : js의 함수와 유사. props라는 입력을 받은후 화면에 어떻게 표현할지에 대한 ReactElement를 Return


### 1. 컴포넌트 구현

```javascript
//1. 함수 컴포넌트
function Welcome(props){
	return <h1>Hello, {props.name}</h1>;
}

//2. 클래스 컴포넌트 (ES6 이상부터)

class Welcome extends React.Component {
	render(){
		return <h1>Hello, {this.props.name}</h1>;
	}
}
```
근본적으로 두 코드는 동일한 동작을 수행하지만,
클래스에 부가적인 기능이 더 추가된 형태이다.

### 2.컴포넌트 사용
이제 이 컴포넌트를 JSX 표현식으로 사용 가능하다.

```javascript
const element = <Welcome name = "Sara" />;
```


### 3. props

이렇게 하면 Welcome 이라는 컴포넌트의 리턴Element객체에
name부분에 Sara를 인자로 넘겨 완성한다.

여기서 name 과같은 프로퍼티들을 여러개 생성할 수 있다.
name = "Sara" id = "23123" ....

이러한 모든 값들은
props라는 객체(json) 에 저장된다.
JS의 일반적인 함수의
arguments와 유사하다.

```javascript
function Clock(props){
	return(
		<div><h1>{props.name} {props.date.toLocaleTimeString()}</h1></div>
	);
}

function tick() {
	ReactDOM.render(
	<Clock date = {new Date()} name="Lee"/>,
	document.getElementById('root')	
	);
}

setInterval(tick,1000);
```
여기서 컴포넌트(함수)를 호출하는 쪽에서 넘겨주는 arguments 들을 props 라고한다.
HTML 태그의 프로퍼티 형식으로 넘겨주는 것들이 그것이다.

	<컴포넌트명  prop1 = " " prop2 = " "....> 여기에 넣은게 child 이다.</컴포넌트명>

이 jsx식은 컴포넌트내부의 리턴 jsx식을 포함한 ReactElement 객체를 반환한다.


### 4. state

props와는 다르게, 호출하는쪽에서 넘겨주는게 아니라, 내부적으로 값을 지정하여 외부로 들어내지 않는 인자들을 state라고한다.
현재 function component에선 사용못하고 이를 단계적으로 class component로 전환해보자

```javascript
class Clock extends React.Component {
	render(){   //기존 함수형 컴포넌트 내부코드를 render(){...}에 넣어준다.
		return(   // 기존 함수형 컴포넌트에서 props.prop1 을 this.props.prop1 로 바꿔준다.
			<div><h1>{this.props.name} {this.props.date.toLocaleTimeString()}</h1></div>
		);
	}
}
```
ReactDOM.render는 업데이트가 발생할때마다 호출되지만,
동일한 DOM노드로  <Clock />을 렌더링하는 경우, Clock클래스로 생성한
단일 인스턴스만 동작.

즉 DOM 전체에 대한 렌더링작업을 하는게아니라, 
특정 DOM노드일부중, 이미 한번 렌더링된 노드로 렌더링을 하는것은
Clock 클래스에 정의된 render메서드만 실행하는것.
즉 렌더링업데이트 코스트가 현저히 낮다고 볼 수 있다.


### 5. props -> state

현재까지는 date라는게 props에 있는데 이것을  state로 옮겨보자.
1. render() {...} 내부의 this.props 를 this.state로 변경
2. class constructor 추가.
3. <Clock /> 에서 해당 프로퍼티 삭제
```javascript
class Clock extends React.Component {
	constructor(props){
		super(props); //상속받은 React.Component의 constructor에 props를 삽입하는 동작
		this.state = {date : new Date()	};
	render(){   //기존 함수형 컴포넌트 내부코드를 render(){...}에 넣어준다.
		return(   // 기존 함수형 컴포넌트에서 props.prop1 을 this.props.prop1 로 바꿔준다.
			<div><h1>{this.state.name} {this.state.date.toLocaleTimeString()}</h1></div>
		);
	}
}
```

### 6. state
1. setState() : state를 수정할 때 사용.

this.state.comment = 'Hello';
이런식의 수정은 렌더링에 반영되지 않는다.

```javascript
this.setState({
	comment: "Hello"
});
```
2. setState() 내부에 this.state, this.props 등을 사용하면 안된다. 해당 값들은 비동기적으로 업데이트 되기 때문에 다른방식사용

```javascript
this.setState({counter : this.state.counter + this.props.increment,}); //Wrong
```

3. setState메서드에 함수를 인자로 넘기는 방식으로 동작시키면 된다.
```javascript
this.setState((state,props)=>({ //화살표 함수 사용
	counter: state.counter + props.increment
}));

this.setState(function(state,props){ //일반 함수 사용
	return {
		counter: state.counter + props.increment
	};
});

```
4. setState를 호출시마다, 사용된 state가 현재 state로 병합된다.
```javascript  
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
``` 
다음과 같이 state에 posts와 comments라는 독립적인 변수가 있고, 이를 setState상에서 각각을 독립적으로 업데이트가 가능하다.
업데이트 된 결과는 병합된다.

5. 단방향성이다.
state 값을 자식에게 props로 전달 가능하다.

<h2>{this.state.date.toLocaleTimerString()}</h2>



#### 3. 생명주기
```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState(
	date:new Date()
    );
  }
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```
함수형 컴포넌트를 사용했단 Clock 예제에서는 타이머를 직접 호출하여 타이머 인터럽트가 발생시
이벤트핸들러를 직접 지정하여 사용했는데, 클래스 컴포넌트를 사용하면 다음 처럼 사용 가능하다.

```javascript
//생명주기 메서드
componentDidMount(){}  //Clock 출력물이 DOM에 렌더링된 후에 실행.
componentWillUnmount(){}
```
Clock이 DOM에 렌더링 될때마다 타이머를 설정, 이것을 마운팅,
타이머를 해제-> unmount.


#### 4. 이벤트처리
1.
```html
<button onClick="activateLasers()">
	Activate Lasers
</button>
```

```javascript - jsx
<button onClick="{activateLasers}>
	Activate Lasers
</button>
```
"함수명()" 대신에  {함수명} 으로 대체.

2. onCLick이 false 이더라도, 기본동작을 수행해야 하므로 preventDefault 를 호출해야 디폴트행동이 자동실행되지 않는다.

```javascript
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}

reactDOM.render(<ActionLink />, documen...("root"));

이렇게 되면 자동으로 onClick 이벤트 핸들러가 내장된 함수형 컴포넌트가 사용 가능하다.

```
ES6 클래스 컴포넌트 사용
```javascript
class Toggle extends React.Component{
	constructor(props){
		super(props);
		this.state = {isToggleOn:true};
		this.handleClick = this.handleClick.bind(this);
	}
	handleClick(){
		this.setState(state=>({
			isToggleOn:!state.isToggleOn
		}));
	}
	render() {
		return(
		<button onClick={this.handleClick}>
			{this.state.isToggleOn ? 'ON':'OFF'}
		</button>
		);
	}
}

ReactDOM.render(<Toggle/>, .. ('root'));

```

#### 5.조건부 렌더링
1. if,else문
```javascript
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;
    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

ReactDOM.render(
  <LoginControl />,
  document.getElementById('root')
);

```


2. {논리표현식 &&jsx}
if 대신에 다음과 같은 방식도 가능.
논리표현식이 거짓이면 jsx도 실행안됨.
{unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>}

3. 3항 연산자
  조건식 ? 참일때 결정값 : 거짓일때 결정값.
  {isLoggedIn
        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} 
4. 조건문을 활용하여 렌더링 제어.
if(!props.warn) return null;
렌더링 return이 아니라 null을 반환.


	
