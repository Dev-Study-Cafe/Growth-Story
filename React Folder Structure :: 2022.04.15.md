# React Folder Structure // 2022.04.15.

### Folder Naming Convention

React에서 폴더 및 파일명을 정할 때 기존에는 파스칼케이스를 사용했었는데, **케밥케이스를 사용해야 한다.**  
이유는, 파스칼 케이스를 사용한 폴더 및 파일은 운영체제의 다양성에 따라 다르게 동작할 수 있고, 다른 운영체제를 사용하는 팀원과 협업할 때 버그를 유발할 수도 있다고 한다.

※ 하지만 많은 프로젝트에서 파스칼케이스를 사용하고 있고, 파스칼케이스를 사용하였을 때 문제가 발생하는 경우는 본 적이 없다.  
만약 정말로 위와 같은 상황이 발생한다면 케밥케이스를 사용하겠지만, 현재로서는 파스칼케이스가 더 친숙하다.

### Abstraction

추상화란, 컴퓨터 공학에서 모듈, 시스템 등으로부터 핵심 개념을 간추려내는 것을 말한다.  
객체지향 언어에서 각 객체마다 가지고 있는 공통분모를 분리하여 더 상위 객체를 만든다는 개념이다.  
이런 추상화라는 개념이 폴더 구조를 잡을 때도 사용될 수 있다.  
비슷한 기능을 하는 폴더 및 파일들을 더 상위 개념으로 추상화하여 폴더 구조를 설계하는 것이다.

시니어 개발자는 추상화를 잘하는 개발자다.  
각각의 코드가 어떤 일을 하는지 파악하고, 그에 대한 공통기능을 최대한 추려서 추상화를 잘하는 것이 시니어 개발자가 되는 길이 아닌가 싶다.

현재 나는 추상화를 잘 하지 못한다.  
반대로 구체화를 잘하려고 한다.  
아직 각각의 코드가 어떤 일을 하는지 완벽하게 파악하지 못하기 때문에, 추상화보다는 구체화를 통해 코드 파악이 먼저인 것 같다.

그래서 폴더 구조를 설계할 때도 공통분모를 찾는 것보다, 최대한 각 폴더 및 파일의 역할이 명확하도록 세분화하여 나누는 것에 초점을 맞춘다.

### Folder Structure

```
- src/
--- assets/
--- components/
--- constants/
--- contexts/
--- helpers/
--- hooks/
--- layouts/
--- navigation/ (also named 'routes')
--- pages/
--- services/
--- translations/
--- types/
--- utils/
```

##### assets

assets 폴더는 내부에 이미지, 폰트, 로티, 비디오 등 정적 파일들을 포함한다.  

```
- assets/
--- images/
--- fonts/
--- lotties/
--- videos/
```

### components

components 폴더는 재사용 단위의 컴포넌트들을 포함한다.  
보통 common 폴더로 기본 컴포넌트들을 한 번 더 묶고, common과 같은 레벨에 서비스 도메인이나 라우터 등의 컴포넌트 폴더도 같이 둔다.  
그리고 컴포넌트 폴더 하위의 모듈들은 서로를 import하며, 하위 컴포넌트들을 조합하여 상위 컴포넌트를 만들기도 한다. (Atomic Design)

내가 컴포넌트 분리하는 기준은 2가지가 있다.

- 재사용성

  컴포넌트의 가장 기본적인 역할이다.  
  중복되는 뷰 코드가 있다면 이를 컴포넌트화 하여 여러 파일에서 사용할 수 있게 만든다.

- 코드 분리

  프론트엔드는 뷰 관련 코드가 많다.  
  종종 한 파일에서 뷰 관련 코드가 너무 많아 코드의 가독성이 저하되는 경우가 있다.  
  이 때 재사용성과는 관련이 없지만 코드 라인 수를 줄이고 개별 모듈로 분리하여 가독성을 높일 수 있다.

  ※ 프론트엔드는 사용자에게 더 좋은 UI/UX를 제공하는 것에 초점을 두기 때문에 뷰 관련 코드가 방대해질 수 밖에 없다고 생각한다.  
  이는 뷰와 로직의 관심사 분리를 통해 일부 해결할 수 있지만, 서비스 도메인, 타겟층, 애니메이션 등 기타 여러 요소에 의해 한계가 있다.  
  JSX 코드를 보면 해당 태그들이 실제 서비스에서 어느 역할을 하는 지 파악하기 힘들다.  
  이를 해결하기 위한 방법으로 몇 가지를 생각해보았다.

  1. JSX 코드 중간중간에 주석 달기

     클린코드에서 주석을 권장하지 않기 때문에 기각

  2. styled-components를 활용한 태그의 네이밍

     이 방법도 JSX 코드의 양이 커질수록 가독성이 떨어지는 문제를 해결할 수는 없을 것 같다.

  3. 뷰 코드의 모듈화

     현재 내가 느끼는 문제를 해결할 수 있는 최선의 방법인 듯 하다.

```
- components/
--- auth/
----- index.js
----- authLoginForm.js
--- common/
----- index.js
----- button/
------- index.js
------- Button.js
----- Input/
------- index.js
------- Input.js
--- user/
----- index.js
----- userProfile.js
```

##### constants

앱 내부에서 사용되는 상수를 포함한다.  
디자인 시스템에 정의된 앱 내부 색상 등도 여기에 포함될 수 있다.

```
- constants/
--- index.js
--- Endpoints.js
--- Keys.js
--- Theme.js
```

##### contexts

React에서 전역 상태 관리를 위해 제공하는 Context API를 포함한다.  
전역 상태 관리로 React Context를 사용하지 않는다면, 이 폴더는 제외하거나 대체 라이브러리와 관련된 네이밍으로 변경한다.

```
- contexts/
--- ThemeContext.js
--- SystemContext.js
```

##### helpers

서버와의 API 통신을 담당한다.

서버는 일반적으로 API를 도메인 단위로 분리하여 개발한다.  
클라이언트 단에서도 서버의 API 도메인 별로 파일을 생성하여 각각의 API 함수를 helper 내부에 정의한다.

```
- helpers/
--- user/
----- userHelper.js
----- payload/
------- index.js
--- auth/
----- authHelper.js
----- payload/
------- index.js
```

##### hooks

Custom Hook을 포함한다.  
Custom Hook이란 React에서 제공하는 기본 Hook 외에 서비스 내에 반복되는 React Hook을 재사용할 수 있게 모듈화한 Hook 함수다. (컴포넌트화와 유사)

React에서는 React Hook을 두 가지 경우에서만 사용할 수 있다.  
한 가지는 React Component일 때 가능하고, 다른 경우는 Custom Hook 내부에서 사용할 수 있다.

```
- hooks/
--- useLoginForm.js
--- useAuthDispatch.js
--- useAuthSelector.js
```

##### layouts

애플리케이션 내의 layout을 포함한다.

한 애플리케이션에는 사이드 바를 포함한 레이아웃, 모달 레이아웃 등 여러 레이아웃이 존재할 수 있다.  
이런 layout에 필요한 컴포넌트들은 각 페이지에서 호출해야 하는 뷰 컴포넌트와는 다르기 때문에 layouts 폴더 내에 정의한다.

```
- layouts/
--- index.js
--- components/
----- index.js
----- AuthSideBar.js
----- Header.js
--- ModalLayout.js
--- SideBarLayout.js
--- FullLayout.js
```

##### navigation

애플리케이션 내 라우팅을 담당한다.

```
- navigation
--- AuthNavigator.js
--- MainNavigator.js
```

##### pages

뷰와 뷰 관련 로직을 포함한다.

page 폴더 내부에 파일들은 기본적으로 페이지 단위의 뷰를 담당한다.  
하지만, 재사용성이나 코드 가독성 등을 고려하여 모듈 단위로 컴포넌트화하여 분리한다.  
비즈니스 로직과 관계없이 오직 뷰만 관련있는 로직같은 경우 pages 내부에서 처리하기도 한다.  
(뷰만 관련있는 뷰 로직 코드를 분리하는 건 효율이 떨어진다고 생각한다.)

```
- pages/
--- index.js
--- Login/
----- index.js
----- Login.js
--- SignUp/
----- index.js
----- SignUp.js
```

##### services

애플리케이션 내부 비즈니스 로직을 관리한다.

애플리케이션 내부 저장소에 저장하는 로직, 전역 상태를 관리하는 로직, API 함수를 호출하는 로직 등이 서비스 파일 내에 정의된다.  
앱 내부 비즈니스 로직이 존재하는 단위마다 서비스 파일을 별도로 생성한다.

```
- services/
--- AuthService.js
--- UserService.js
--- SystemService.js
--- PasswordService.js
```

##### translations

다국어 지원과 관련된 파일을 포함한다.

일반적으로 i18n을 사용하는데, 이 폴더 내에 i18n 관련 설정 및 언어팩 파일들을 저장한다.

```
- translations/
--- i18n.js
--- languages/
----- index.js
----- ko.js
----- en.js
```

##### types

애플리케이션 내부 타입을 관리한다.

테마나 시스템 등 애플리케이션 전반적으로 사용되는 Type들을 관리한다.

```
- types/
--- Navigation.js
--- System.js
--- Theme.js
```

##### utils

유틸리티 함수를 포함한다.

유틸리티 함수의 기준은 어느 프로젝트에 넣어도 동작하는 함수다.  
현재 프로젝트에서 사용하는 유틸리티 함수 코드를 그대로 복사하여 다른 프로젝트에서 실행해도 문제없을 정도로 애플리케이션과의 의존성을 제거해야 한다.  
예시로 날짜와 관련하여 날짜 형식을 변경하는 함수, 값을 계산하는 함수 등이 있을 수 있다.

```
- utils/
--- date.js
--- calculate.js
--- storage.js
```

### Conclusion

아직 만으로 1년도 되지 않은 경험이 부족한 개발자이기 때문에 위 작성한 폴더 구조가 별로일 수도 있다.  
하지만 이렇게 파일로 저장해두는 이유는 내가 개발자로서 성장하면서 생각이 어떻게 바뀌었는지, 전과 어떤 차이가 있는지 기록하기 위해서이다.  
요즘 나는 구조와 아키텍처에 관심을 가지고 있다.  
여러 프로젝트를 진행하면서 불편하거나 비효율적이라고 느꼈던 점들이 여럿 있었고, 현재의 폴더 구조는 이런 문제들을 해결하기 위한 노력의 결과물이다.  
이런 나만의 구조는 계속해서 업데이트할 생각이다.  
또한, 이런 과정을 React Folder Structure Boilerplate 프로젝트로 관리할 계획이다. 
