---
layout: single
title:  "[React] useEffect()"
---

![](https://velog.velcdn.com/images/eenooyos/post/26c6325e-e0d1-46bf-8b6d-a3c6f37e9a82/image.jpeg)

## useEffect 란?
> ```useEffect(function, deps)```
**useEffect()**는 component 가 렌더링 될 때마다, 
특정 작업(effects)을 수행 할 수 있도록 해 주는 함수이다.
React class의 **componentDidMount, componentDidUpdate,
componentWillUnmount** 와 같은 목적으로 사용된다.

```useEffect()``` 함수는 컴포넌트가 렌더링 될 때마다 실행되기때문에,
두번째 인자에 값을 넘겨줌으로써 원하는 시점에 실행 될 수 있도록 구현 할 수 있다. **하지만 무조건적으로 최초 렌더링 후(mount되었을 때) 한번은 동작한다.**

```jsx

1. 컴포넌트가 렌더링 될 때마다 실행되는 함수.

useEffect(() => {})

2. 컴포넌트가 처음 렌더링 되었을때 실행되는 함수.

useEffect(() => {}, [])

3. 특정 props, state 가 변경되었을때 실행되는 함수.

useEffect(() => {}, [deps])

```

## Clean-up 란?
> **clean up함수**는 클래스형 컴포넌트의 ```componentWillUnmount``` 역할과 같다. 목적은 메모리 누수가 발생하지 않도록 정리하는 것이다.

#### cleau-up 

```jsx
useEffect(() => {
  console.log("hello")
  return ()=>{
    console.log("clean-up")
  };
}, []);

```
1. clean-up 함수는 ```useEffect()에서 콜백함수의 return 함수``` 이다.
2. clean-up함수를 반환하여 컴포넌트가 ```unmount되거나 update되기 직전에 원하는 작업을 실행``` 시킬 수 있다.
* ```unmount``` 될때 cleanup 실행 -> ```deps``` 빈 배열 [ ]
빈배열일경우에 effect 내부의 props, state 값은 최신의 상태는 갖지 않는다.
* ```update``` 되기 직전 cleanup 실행 -> ```deps``` 원하는 값
3. clean-up 함수를 사용하면 리렌더링 후 ```이전의 effect들을 정리하고 effect를 실행```한다.

#### 클로저(Closure)란?
```...이건 솔직히 아직 뭔소린지 모르겠습니다.```
> 함수가 자신의 내부가 아닌 외부에서 자신 내부 안에서 선언된 변수에 접근하는 것.

```javascript
let l0 = "l0";

function fn1() {
  function fn2() {
    function fn3() {
      let l3 = "l3"
      console.log(l0, l1, l2, l3)
    }
    let l2 = "l2";
    console.log(l0, l1, l2)
    fn3();
  }
  let l1 = "l1"
  console.log(l0, l1)
  fn2();
}

fn1();
```
![](https://velog.velcdn.com/images/eenooyos/post/7cad7822-d368-445d-b9d0-058ab6e80619/image.png)

이렇듯 클로저는 함수가 만들어지는 시점에서, 그 함수의 부모함수(외부함수)가 가지고있는 변수를 언제든지 호출만 하면 불러올 수 있는 것을 클로저라고 한다.

React 함수형 컴포넌트는 **렌더링이 될 때마다 함수를 다시 호출**하게되는데, 
상태관리를 하기 위해 함수가 다시 호출되었을때, **이전의 상태를 기억**하고있어야 하기 때문에 **클로저를 통해 해결**한다고한다.

useEffect는 보통 익명함수를 넣어 사용되고, 이 익명한수는 렌더링시마다 새로 생성되며, 각자 생성 될 당시의 state값을 바라본다. 이것이 클로저 이다.

