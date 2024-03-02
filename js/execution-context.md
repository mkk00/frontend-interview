# 🙎🏻‍♂️ 실행 컨텍스트(Execution Context)에 대해서 설명해주세요.
실행 컨텍스트(Execution Context)란 실행할 코드에 제공할 환경 정보들을 모아놓은 객체입니다.
동일한 환경에 있는 코드들을 실행할 때 필요한 정보들을 모아 컨텍스트를 구성하고, 이를 콜 스택에 쌓아 올렸다가 가장 위에 쌓여있는 컨텍스트와 관련 있는 코드들을 실행하는 식으로 전체 코드의 환경과 순서 보장합니다.
실행 컨텍스트가 활성화되는 시점에서 변수 선언, 함수 선언, 호이스팅, 외부 환경 정보 구성, this 값 설정 등의 동작을 수행합니다.(→VariableEnvironment, LexicalEnvironment)
| | | |inner| | | |
|---|:---:|:---:|:---:|:---:|:---:|---|
| | |outer|outer|**outer**| | |
| |전역|전역|전역|전역|**전역**| |

<br/>

## 실행 컨텍스트 생성되는 시점
- **전역 컨텍스트(Global Execution Context)**
  - 파일 로드 시 자동 생성
  - 전역 변수, 전역 함수 등
  - 콜 스택 제일 하단에 쌓이고, 브라우저가 닫힐 때까지 유지됨
  - VariableEnvironment, LexicalEnvironment
- **함수 컨텍스트(Functional Execution Context)**
  - 함수를 실행하여 실행 컨텍스트 구성
  - 함수 실행이 끝나면 스택에서 제거됨
  - VariableEnvironment, LexicalEnvironment
- <del>eval 함수</del>
  - <del>보안상의 이유로 권장하지 않음</del>

<br/>

## 실행 컨텍스트의 구성
실행 컨텍스트의 주요 동작들은 VariableEnvironment 와 LexicalEnvironment 라는 두 환경 컴포넌트에서 이루어집니다.
이 두 가지 환경 컴포넌트는 실행 컨텍스트가 생성될 때 함께 생성됩니다.

### `VariableEnvironment`
- 코드의 실행 환경을 초기화하는 단계에서 활용됩니다.
- `var`로 선언된 변수나 함수 선언 등의 정보를 코드가 실행되기 전에 메모리에 저장합니다.
- 변수 환경은 최초 실행 시의 스냅샷 유지합니다.
  - 변경 사항을 반영하지 않습니다.
  - 즉, 한번 설정되면 실행 컨텍스트의 생명주기 동안 변경되지 않음
  - 쉽게 말해서 함수의 호출 시점에 해당 함수의 정보를 스냅샷으로 찍어서 그 상태를 계속 유지한다는 것을 의미합니다.


### `LexicalEnvironment`
- 코드의 실행 중 발생하는 변경사항에 대한 정보를 담고 있습니다.
- EnvironmentRecord, OuterEnvironmentReference 를 통해 관리됩니다.
  - **Environment Record(환경 레코드)**
    - let이나 const로 선언된 변수의 할당, 블록 스코프, 클로저 등의 식별자 정보를 포함합니다.
    - 이곳에 저장된 식별자들은 코드가 실행되는 동안 참조하거나 변경될 수 있습니다.
  - **Outer Environment Reference(외부 환경 참조)**
    - 현재 컨텍스트에서 참조할 수 있는 외부의 LexicalEnvironment를 가리킵니다.
    - 이를 통해 스코프 체인이 형성되며, 이 체인을 따라 변수를 찾아나가는 것이 가능해집니다.

###  `this Binding`
this는 실행 컨텍스트가 생성될 때 결정되며, 그 값은 함수 호출 패턴에 따라 달라집니다.