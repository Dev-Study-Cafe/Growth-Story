# React Query Folder Structure 2 // 2022.06.08

### Problem

내가 생각했을 때 2가지의 방법이 있었다.

첫 번째 방법은, 기존 아키텍처와 똑같은 Page -> Service -> Helper 구조이다.

```
Page(React Component 형태) -> Service(Custom Hook 형태) -> Helper(Custom Hook 형태)
```

기존 아키텍처에서 Class 또는 Function 기반의 문법으로 작성하던 코드를 모두 Custom Hook으로 작성하게 되면, React Query의 useQuery와 같은 Hook을 사용하면서 기존의 아키텍처를 그대로 가져갈 수 있었다.

하지만 몇 가지 문제가 발생했다.

첫 번째로, Service는 Custom Hook 문법으로 작성할 이유가 없다.

저번 글에서도 언급했었지만, 내 개인적인 생각으로는 비즈니스 로직에서 React State를 사용하는 경우는 없어야 한다고 생각한다.  
이 전제가 잘 지켜졌다면 Service 파일을 Custom Hook으로 작성하는 이유는 단지 React Query Hook을 사용하는 Helper 함수를 호출하기 위해서일 것이다.  
이러한 목적으로 불필요하게 Service 파일을 Custom Hook으로 작성하는 것은 합리적이지 못하다.

두 번째로, React Query는 React State를 반환한다.

**이 React State는 Service 파일에 들어가면 안된다.**

위에 언급했듯이, Service 파일에는 React State가 필요가 없고 필요 해서도 안된다.  
이러한 Service 파일에서 사용하지도 않는 React State가 들어가게 되면 불필요한 데이터를 포함하고 있는 셈이다.

예를 들어, React Query의 useQuery 함수를 사용하게 되면 isLoading이라는 '현재 로딩 중인가?'에 대한 값을 반환한다.  
일반적으로 isLoading이라는 값은 비즈니스 로직에서 필요한 데이터가 아니다.  
View 부분에서 로딩 중이라면 로딩 관련 뷰 처리를 해야 하기 때문에 필요한 데이터다.

이러한 문제를 해결하기 위해 두 번째 방법으로 Page에서 Service 파일과 Helper 파일을 별도로 호출하는 구조를 생각했다.

```
Page(React Component 형태) -> Service(Class or Function 형태)
													-> Helper(Custom Hook 형태)
```

1. 코드의 일관성 (State는 결국 Page에서 모인다.)
2. Service 파일을 Custom Hook으로 작성할 필요가 없다.

생각되는 문제점

1. API Call 과정에서 발생하는 Error에 대한 처리를 어떻게 할지
2. 데이터 비즈니스 로직 처리 과정에서 API Call이 필요하다면 어떻게 할지