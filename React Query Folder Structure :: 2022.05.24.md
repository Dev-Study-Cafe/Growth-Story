# React Query Folder Structure // 2022.05.24

### Background

React Query를 도입하려고 현재 공부를 했는데, React Query를 사용하게 되면 기존에 고민했던 아키텍처가 흐트러지게 된다.

기존 아키텍처에 대한 글은 React Folder Structure 글에서 확인할 수 있다.

React Query는 API Call에 대하여 로딩, 결과 데이터 등의 Result에 대하여 state처럼 관리할 수 있게 해준다.  
이 말은 즉 API Call에 대한 Result 값의 변경에 대하여 useEffect 등으로 감지할 수 있다는 말이다.  
프론트엔드 특성상 현재 로딩중인지(데이터를 Fetch하는 중인지) 확인하여 사용자에게 로딩 화면을 보여줘야 한다.(가장 기본적인 UX)  
때문에 이런 데이터를 state 형태로 제공하는 것은 코드를 줄일 수 있고, 생산성을 향상 시킬 수 있는 좋은 라이브러리이다.

### Problem

그런데 문제는 이 React Query 라이브러리를 기존 아키텍처에 적용하려 할 때 발생한다.  
기존 아키텍처에서 비즈니스 로직은 다음과 같이 관리된다.

```text
Pages(React Component 형태) -> Service(Class 형태) -> Helper(Function 형태)
```

React Query는 Hook으로 구현되어 있기 때문에, React Component 이외의 코드에선 호출이 불가하다.

### Solving

아직 명확한 답을 찾지 못했다. 계속해서 개선해나가야 할 것 같다.

-----

2022.05.24 기준 해결책  
다음과 같은 구조로 관리하면 기존에 발생했던 문제는 해결할 수 있다.

```text
Pages(React Component 형태) -> Service(Class 형태)
                           -> Helper(Custom Hook 형태)
```

API Call(React Query 사용 부분)을 React custom hook으로 만든다면 내부에서 hook 사용이 가능하고, 이 API Call을 Pages에서 호출하게 되면 기타 에러는 발생하지 않는다.  
하지만 여기서 중요하게 봐야 할 것은, 위와 같은 구조로 개발을 하게 되면 API Call을 재호출하여 로직을 처리할 경우 어떻게 할 지에 대하여 고민해봐야 한다.  
예를 들어, 컴포넌트 렌더링 초기에 API Call을 하고 추후에 특정 이벤트 발생 시 동일한 API Call을 한 번 더 하여 데이터를 갱신해야 하는 경우 코드의 가독성이 저하되고 일관성 없는 코드가 될 여지가 있다.

이 문제는 React Query에서 제공하는 Key 기반 API Call 캐싱 기능을 통해 해결할 수 있지 않을까?  
더 찾아보고 고민해봐야 할 것 같다.
