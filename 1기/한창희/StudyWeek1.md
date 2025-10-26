### [ Ch1 - Composable 함수들 ]

<< Composable 함수의 의미 >>

* Composable 함수는 Jetpack Compose 의 가장 기본이 되는 요소이며, Composable 트리 구조를 작성하는데 사용
* 트리라는 표현?
    * Compose Runtime이 Composable 함수를 메모리에서 큰 트리의 일원인 하나의 노드(node)로서 이해하고 나타낼 수 있다

* @Composable 어노테이션을 추가하는 것만으로 어떠한 표준 코틀린 함수를 Composable 함수로 만들 수 있다
    * @Composable 어노테이션 >> 컴파일러에게 이 함수가 본질적으로 데이터를 하나의 노드(node)로 변환하여 Composable Tree에 기재하겠다는 의도
    * 트리에 데이터를 삽입하는 동작
        * 이 동작을 함수 실행의 부수 효과(side effect)로 발생한다고 말할 수 있다
        * 일반적으로 Compose 관용어로 ‘방출(emitting)’ 이라는 표현을 사용
            * Composable 함수는 실행될 때 함수의 구현 정보를 방출, 이는 Composition 처리 과정 중에 발생
    * [Q]
        * 여기서 데이터?
            * UI
            * LaunchedEffect와 같은 요소들
        * 부수 효과?
            * Compose Tree 구조에 변화를 주는 관점?


* Composable 함수를 실행하는 유일한 목적은 트리의 인메모리 표현(in-memory representation)을 만들거나 업데이트 하는 것
    * 인메모리 표현?
    * Composable 함수는 읽은 데이터가 변경될 때마다 다시 실행되므로 > 나타내는 메모리 구조를 항상 최신 상태로 유지
        * 트리의 최신 상태 유지를 위해, 새로운 노드 삽입, 제거, 교체, 위치 변경
        * Composable 함수는 트리로부터 state를 읽거나 쓸 수도 있다


* Composition
    * Composable 함수를 실행하고, UI를 나타내는 트리 구조를 출력하는 행위



<< Composable 함수의 속성 >>

@Composable 어노테이션은 해당 어노테이션이 적용된 함수나 표현식의 타입을 효과적으로 변경, 다른 타입과 마찬가지로 해당 타입에 일부 제약 사항이나 특성을 부여
이러한 특성들은 Jetpack Compose와 고도로 밀접한 관련이 있으며, Compose의 라이브러리들이 제 기능을 하도록 하는 데 아주 중요한 역할을 수행
    * 제약 사항 및 특성 ?
* 
* 위와 같이 특별한 타입?이 된 Composable 함수는 새로운 능력과 함께 꼭 지켜야할 제약 사항들을 가지게 된다
* Compose Runtime은 Composable 함수가 사전에 정의된 특성을 준수하도록 가정
    * Compose Runtime은 위 가정을 바탕으로 앱의 속도를 높이는 여러 ‘최적화’ 기법을 포함하고 있다
        * 병렬적인 Composition
        * 우선순위에 따른 임의의 Composition 정렬
        * 스마트 Recomposition
        * 위치 기억법 (positional memoization)
        * etc…
* 
* p12 회색 문단
    * Compose Runtime이 UI를 빠르게 최적화(병렬, 순서 무시, 생략)할 수 있는 이유는, 개발자가 Composable 함수를 '순수하게'(부수 효과 없이, 독립적으로) 작성할 것이라는 '확실성'을 전제로 하기 때문?

* Recomposition
    * Composition을 다시 실행하여 레이아웃을 업데이트하는 일련의 과정



<< 호출 컨텍스트 >>

* Composable 함수의 속성은 대부분 Compose Compiler에 의해 활성화된다
    * Compose Compiler는 Kotlin 컴파일러 플러그인
    * Kotlin 컴파일러가 액세스할 수 있는 모든 정보에 액세스할 수 있고, Composable 함수의 중간 표현인 IR을 가로채고 변환하여 원본 소스의 모든 Composable 함수에 추가적인 정보를 부여할 수 있다
    * 각 Composable 함수에 추가된 요소 중 하나는 함수의 매개변수 목록의 끝에 새롭게 추가된 Composer이다.
        * 이 매개변수는 암묵적이며 개발자는 이에 대해 알아야 할 필요가 없다. Composer 매개변수의 인스턴스는 런타임에 주입되며, 모든 하위 Composable 호출로 전달되므로 트리의 모든 수준에서 접근할 수 있다
    * 
    * 우리가 코드를 빌드(컴파일)할 때, Compose Compiler가 작성한 Composable 함수를 변형시킨다

* IR
    * Intermediate Representation
    * Kotlin 컴파일러가 소스파일을 해석하는 하나의 과정으로 이해하기

<img width="646" height="181" alt="Pasted Graphic 6" src="https://github.com/user-attachments/assets/9af9c275-a3b5-484d-b047-fbbe3755573b" />

￼<img width="916" height="687" alt="Pasted Graphic 5" src="https://github.com/user-attachments/assets/5d3a6459-4540-4e6a-856c-29c0eaa38242" />

￼
* Compose Compiler는 위와 같이 코드를 변환한다
* Composer는 트리 내에서 모든 Composable 호출로 전달된다
* Compose Compiler는 Composable 함수는 오로지 다른 Composable 함수에서만 호출되도록 제한한다

* Composer는 개발자가 작성하는 Composable 코드와 Compose Runtime 간의 중재자 역할을 한다
* Composer 매개변수의 인스턴스는 런타임에 주입, 트리를 따라 모든 자식 Composable 함수에게 전달된다


<< 멱등성 (Idempotent) >>

* 같은 데이터를 넣으면 몇 번을 실행해도 항상 같은 UI가 나와야 한다
    * 항상 동일한 트리가 생성되어야 한다
* Jetpack Compose Runtime은 recomposition과 같은 작업을 위해 이러한 멱등성이 성립해야 한다
* Compose 최적화에서 핵심이라 생각되는 ‘Skipping(생략)’ 은 멱등성을 가정
    * Recomposition 수행 시 멱등성이 있으니 입력값이 그대로면 결과도 이전과 같다 생각하고 skip 
    * 이미 메모리에 저장된 UI 노드를 재사용 > 변경된 부분만 다시 그리는 과정을 통해 최적화 수행

<< 통제되지 않은 사이드 이펙트 방지 >>

* 사이드 이펙트(side effect)는 호출된 함수의 제어를 벗어나서 발생할 수 있는 예상치 못한 모든 동작을 의미
    * 로컬 캐시에서 데이터 읽기, 네트워크 요청 작업, 전역 변수 설정과 같은 동작들이 사이드 이펙트로 간주
* 사이드 이펙트 방지
    * 함수 실행이 함수 외부에 여향을 주거나, 외부로부터 영향을 받으면 안된다는 규칙
    * 입력값이 같아도 외부 요인(ex> 네트워크 상태) 에 따라 결과가 달라질 수 있다
* 사이드 이펙트 문제점
    * 멱등성을 깨뜨림

￼
    * 제어를 하지 못하고 여러 번 호출
        * Composable 함수는 Compose Runtime에 의해 짧은 시간 애에 여러 번 다시
    * 실행 순서 보장 X
        * Compose Runtime은 최적화를 위해 자신의 권한으로 Composable 함수의 실행 전략을 선택한다
            * 하드웨어의 멀티 코어의 이점을 활용하기 위해 recomposition을 다른 스레드로 이전
                * 서로 다른 스레드에서 병렬로 실행 가능
            * 필요성이나 우선순위에 따라 임의의 순서로 실행
                * 예시로 화면에 보이지 않는 Composable은 낮은 우선순위로 할당될 수 있다

* 앱에서 네트워크 요청 혹은 DB 저장은 대부분 필수적이므로 Compose 는 위 문제를 아래 방식으로 해결
    * Stateless 지향
        * 필요한 데이터를 매개변수로 전달 받고, 해당 입력값만을 사용해 UI를 그리기
    * 이펙트 핸들러 사용
        * 네트워크 요청과 같이 필수적인 사이드 이펙트는 Composable 함수 본체에 직접 작성 X
        * LaunchedEffect와 같은 이펙트 핸들러 내부에서 실행
        * Composable 라이프사이클을 인식
            * 화면이 사라질 때 이펙트 핸들러에 작성한 동작 취소
            * 입력값이 그대로면 recomposition 되어도 이펙트 핸들러가 한번만 호출되게 할 수 있다


<< 재시작 가능(Restartable) >>

* 일반 함수
    * 콜 스택의 일부로서 프로그램이 실행될 때 단 한번만 호출
* Composable 함수
    * 여러 번 다시 시작 가능
        * Composable 함수는 자신이 관찰하는 State가 변경되면 재실행되도록 설계

* Compose Compiler
    * 우리가 작성한 코드를 분석해서, 특정 State를 읽고 있는 Composable 함수가 무엇인지 찾아낸다
    * 그리고 Compose Runtime에게 위 함수는 해당 State 변경 시 재시작 되어야 한다고 알려주는 데 필요한 코드를 생성한다
    * State를 읽지 않는 Composable은 재시작 필요가 없으므로, 컴파일러도 관련 코드를 만들지 않는다


<< 빠른 실행 (Fast execution) >>

* Composable 함수들은 UI를 구축하거나 반환하지 않는다
    * Composable은 단순히 인메모리 구조를 구축 및 업데이트 하기 위한 데이터를 방출하는 것.
        * 이 메커니즘이 Composable을 더욱 빠르게 만들며 런타임이 아무 문제없이 해당 함수를 여러 번 실행할 수 있도록 한다
    * View 객체 처럼 무거운 UI를 직접 구축 X
* 함수가 자주 실행될 수 있으므로 비용이 큰 계산을 함수 본체에 직접 두면 안되며, 이런 작업들은 코루틴으로 분리해서 처리해야 한다
    * Composable 생명주기를 아는 이펙트 핸들러 내에서 처리해야 한다


<< 위치 기억법 (Positional memoization) >>

* Memoization ?
    * 함수의 결과를 캐싱하는 기법
    * 함수 재호출 시 입력값이 같다면, 다시 계산하지 않고 캐시된 결과를 바로 반환 (순수 함수 케이스에 가능)
* 위치 기억법 ?
    * Compose는 일반적인 memoization과 다르게, ‘입력값’만 보는게 아니라, 소스 코드 내의 호출 위치까지 조합해서 캐시 키(key)를 만든다
    * MyComposable 함수 안에서 Text("Hello")를 세 번 호출하면, Compose는 이들을 "같은 함수"로 보지 않고, "1번 위치의 Text", "2번 위치의 Text", "3번 위치의 Text"처럼 고유한 정체성(ID)을 가진 3개의 다른 노드로 인메모리 트리에 저장한다
    * 이 "위치 기반 정체성" 덕분에, MyComposable이 재구성(재시작)될 때 "1번 Text의 입력값은 그대로네?
        * 그럼 생략(Skip)이 가능해진다

<img width="686" height="349" alt="Pasted Graphic 8" src="https://github.com/user-attachments/assets/4706b59c-fd6f-4e71-9301-49afb6c7bab1" />


"위치" 기억법의 한계와 해결책

* 문제점 (e.g., for 루프): 
    * for 루프로 리스트를 표시할 때, 리스트의 '중간이나 맨 앞'에 새 아이템을 추가하면 어떻게 될까?
        * 기존 아이템들의 '위치'가 전부 한 칸씩 밀려버린다
    * Compose는 “? 2번 위치에 있던 애가 아니네?"라고 착각하고, 데이터가 변경되지 않았음에도 불구하고 그 지점 아래의 모든 Composable을 맹목적으로 재구성하게 된다 > 이는 매우 비효율적 
* 해결책 ( key() Composable ): 개발자가 Compose에게 명시적인 키를 알려주면 된다
    * key(talk.id) { ... } 처럼 talk.id 같은 고유한(Unique) 키를 제공하면, Compose는 '위치'가 아니라 이 '키'를 보고 정체성을 판단
    * talk.id가 123인 애는 아까 2번 위치에 있었는데 지금 3번 위치로 이동 > 데이터는 그대로니 재구성은 생략(Skip)


* remember: 개발자를 위한 위치 기억법
    * remember는 이 "위치 기억법" 메커니즘을 개발자가 직접 사용할 수 있게 노출시킨 API
    * 비용이 큰 계산 결과를 캐싱할 때 사용
    * 
    * 작동 방식
        * remember는 Composable의 호출 위치와 함수의 입력값 조합하여 고유한 키를 만든다
        * 계산된 값(e.g., filters)을 인메모리 구조(슬롯 테이블)의 해당 Composable 슬롯 범위 내에 저장한다
        * FilteredImage 함수가 리컴포지션될 때 path 값이 그대로라면, computeFilters를 다시 실행하지 않고 슬롯에서 캐시된 filters 값을 바로 꺼내 반환한다
￼


<< Suspend 함수와의 유사성 >>
* Kotlin suspend 함수는 다른 suspend 함수에서만 호출될 수 있으므로, suspend 함수 또한 호출 컨텍스트를 필요로 한다
* Composable 함수도 동일하게 Composable 내에서만 호출
* 호출 컨텍스트
    * suspend 함수 > Continuation 이라는 매개변수가 암시적으로 추가
    * Composable > Composer 라는 매개변수가 암시적으로 추가

* Continuation ?
    * 코루틴 런타임이 사용하는 도구, 일종의 콜백과 같다
    * 프로그램에게 어떻게 실행을 계속 이어서 할지 알려주는 역할
    * 함수를 일시중단하고 이후 재개하는 데 필요한 모든 정보를 담고 있다

* suspend 대신 @Composable을 만든 이유?
    * suspend 목표
        * 실행을 중단, 재개하는 것에 초점
        * 실행 흐름 제어가 목적
    * Compose
        * 많은 함수 호출을 바탕으로 인메모리 표현을 생성하는 것이 목표
            * 트리를 생성하는 것 ??
        * 런타임은 인메모리 표현을 최적화
* suspend의 Continuation 인터페이스는 Compose 트리 생성 및 최적화라는 Compose의 목표와 맞지 않았던 것?






[ TODO.. ]

<< Composable 함수의 색깔 (The color of Composable functions) >>

* ‘함수 컬러링'이란? Bob Nystrom이라는 엔지니어가 만든 비유로, "서로 다른 목적을 가진 함수는 서로 다른 '색깔'을 갖는다"는 개념
* 예시: sync 함수(동기, 파란색)와 async 함수(비동기, 빨간색)가 있다
    * '색깔'의 규칙: "파란색" 함수 안에서는 "빨간색" 함수를 직접 호출할 수 없다







[ Question ]
* Composer?
    * 우리가 직접 알 필요는 없다 > 숨겨진 매니저 느낌
    * Compose Compiler가 모든 컴포저블 함수에 추가하는 매개변수
    * UI 트리 최상위에서 주입된 후, 하위 트리로 전달된다
    * ㅡㅡㅡㅡㅡ
    * 이곳에 Text를 배치해, 이 Column을 업데이트해. 와 같이 실질적인 UI 구축과 업데이트 담당 (by Gemini)
* Compose Runtime VS Composer?
    * Compose Runtime은 "상태가 변하면 UI가 업데이트되어야 한다"는 Compose의 마법을 가능하게 하는 '운영체제(OS)' 그 자체이고, Composer는 그 OS가 UI 트리를 구축하기 위해 사용하는'핵심 드라이버(Driver)'라고 생각. (by Gemini)
* p24
    * path 는 state 형태?
* 



* Slot Table, Composition Tree ?


***


### [ Ch2 - Compose compiler ]


Jetpack Compose는 여러 라이브러리로 구성되지만, 이 책은 Compiler(컴파일러), Runtime(런타임), UI 세 가지에 중점을 둔다.
이 중 Compose Compiler와 Compose Runtime이 Jetpack Compose의 핵심 요소다.
Compose UI 라이브러리는 엄밀히 말해 이 핵심 아키텍처의 일부가 아니다. Runtime과 Compiler는 어떤 클라이언트 라이브러리에서도 사용될 수 있도록 포괄적으로 설계되었다. Compose UI는 이 핵심(Runtime + Compiler)을 활용하는 여러 클라이언트 중 하나일 뿐이다. (JetBrains의 데스크톱 및 웹용 라이브러리도 다른 클라이언트다.)
그럼에도 Compose UI를 살펴보는 것은, Compose의 런타임 인메모리 표현(우리가 1장에서 배운)이 어떻게 실제 UI 요소로 구체화되는지 이해하는 데 도움이 된다.



<< Compose 컴파일러란? >>

Compose는 코드 생성에 다소 의존한다. 하지만 kapt 같은 일반적인 어노테이션 프로세서를 사용하지 않는다.
Compose Compiler는 실제 Kotlin 컴파일러 플러그인이다.



<< 컴파일러 플러그인의 장점 >>

kapt는 컴파일 이전에 실행되지만, 컴파일러 플러그인은 컴파일 과정에 직접 내장된다. 이 덕분에 코드에 대한 더 관련성 있는 정보에 접근할 수 있고 프로세스도 빠르다.
컴파일러 플러그인의 가장 큰 장점은, 어노테이션 프로세서처럼 새 코드를 '추가'만 하는 것이 아니라 , 개발자가 작성한 기존 소스 코드를 마음대로 '수정'할 수 있다는 것이다.
이 수정 작업은 컴파일러의 IR (Intermediate Representation) 단계에서 일어난다. 이 강력한 기능을 통해 Compose Compiler는 Composable 함수들을 Compose Runtime이 요구하는 대로 마음대로 변형시킬 수 있다. (1장에서 본 $composer 매개변수를 주입하는 것이 바로 이 작업이다)



현재 많은 어노테이션 프로세서들이 컴파일러 플러그인이나 KSP(Kotlin Symbol Processing)로 전환되는 추세다. KSP는 Google이 kapt의 대체재로 제안하는 "경량" 컴파일러 플러그인이다.
Compose는 IR 변환에 크게 의존하는데 , 만약 모든 라이브러리가 KSP 대신 무거운 IR 변환을 남용한다면, 언어 자체를 불안정하게 만들 위험이 있다. 따라서 전반적으로는 KSP가 더 나은 선택일 수 있다



<< Compose 어노테이션들 >>

@Composable

* 1장에서 다룬 가장 중요한 어노테이션이다.
* 컴파일러 플러그인이 이 어노테이션이 붙은 대상의 타입을 변경하고 코드를 변형시킨다.
* 이 변형을 통해 함수는 "메모리"를 갖게 되어 remember를 호출할 수 있고, 라이프사이클을 가지며, 트리에 노드를 방출(emit)할 수 있게 된다.
* 요약하면, 데이터를 트리의 노드(UI 노드 등)로 매핑하는 역할을 한다


@ComposableCompilerApi

이 어노테이션은 Compose에서 컴파일러에 의해서만 사용된다는 의도를 나타내기 위해 쓰인다.
이는 잠재적 사용자들에게 해당 사실을 알리고, 주의해서 사용해야 함을 알리기 위한 목적을 지니고 있다


@ComposableCompilerApi

Compose에서 일부 외부로 노출된 API 표면이 더 이상 변경되지 않고, stable 버전에 포함 되었음에도 불구하고 내부적으로는 변화가 발생할 수 있음으로 internal(내부)로 지정된다
이 어노테이션은 Kotlin이 지원하지 않는 개념인 모듈 전체에서의 사용을 허용하므로 Kotlin의 internal 키워드보다 더 넓은 범위를 갖는다



@DisallowComposableCalls

* "Composable 함수를 호출하는 것을 금지"하는 어노테이션이다.
* 주로 인라인 람다 매개변수에 사용된다.
* 핵심 예시: remember
    * remember { ... }의 {...} 람다 블록은 최초 컴포지션에서만 실행된다.
    * 만약 이 블록 안에서 Text() 같은 Composable을 호출하는 것을 허용하면, 이 Text 노드는 슬롯 테이블에 생성되었다가, 다음 재구성(recomposition)부터는 이 람다가 호출되지 않아 즉시 삭제되어야 한다. 이는 비효율적이다.
    * @DisallowComposableCalls는 remember의 람다처럼 조건부로(딱 한 번만) 호출되는 인라인 람다에서 이런 비효율을 막기 위해 사용된다



@ReadOnlyComposable

이 어노테이션이 붙은 함수는 "읽기 전용"이며, 컴포지션에 절대 "쓰지" 않는다는 것을 컴파일러에 알린다. (여기서 "쓴다"는 것은 슬롯 테이블에 정보를 저장/수정할 '그룹'을 생성한다는 의미다.)
최적화: 컴파일러는 이 함수가 '읽기 전용'임을 알고 재구성(recomposition)이나 이동(movable)에 필요한 '그룹' 생성을 생략하여 코드를 최적화한다.
주요 용도: MaterialTheme.colors, isSystemInDarkTheme(), LocalContext처럼 한 번 설정되고 절대 변하지 않는 값(CompositionLocal)을 읽어오는 유틸리티 함수들에 사용된다



@NonRestartableComposable

* 이 어노테이션이 붙은 함수는 '재시작(Restartable)'이 불가능해진다.
* 최적화: 컴파일러는 이 함수가 재시작되지 않을 것을 알기 때문에, 재구성(recomposition)이나 생략(skipping)에 필요한 보일러플레이트 코드를 생성하지 않는다.
* 주요 용도: 매우 드물게 사용된다. 로직이 거의 없고, 이 함수를 부르는 부모가 재구성될 때만 함께 업데이트되어야 하는 특수한 경우에 성능 최적화를 위해 사용될 수 있다


@StableMarker

* @Immutable과 @Stable에 붙는 메타 어노테이션이다.
* "안정적"이라고 표시된 타입은 다음 3가지 요구 사항을 만족한다는 약속이다.
    1. equals 결과가 항상 동일해야 한다.
    2. public 프로퍼티가 변경되면 컴포지션에 알려야 한다(notify).
    3. 모든 public 프로퍼티 또한 안정적(stable)이어야 한다.
* 이 '약속'은 컴파일 시점에 검증되지 않으므로, 개발자가 보장해야 한다.
* 대부분 컴파일러가 타입을 추론하지만, 인터페이스나 내부적으로 가변적이지만 겉보기엔 안정적인 타입에는 개발자가 직접 명시해줘야 한다.   

@Immutable (불변)

* 가장 강력한 약속이다. "인스턴스 생성 후 모든 public 프로퍼티가 절대 변경되지 않는다"는 의미다.
* Kotlin의 val보다 강력하다. val은 재할당만 막지만, @Immutable은 val list: MutableList처럼 참조된 객체 내부가 변하는 것조차 허용하지 않음을 의미한다.
* 컴파일러는 이 약속을 믿고, 이 객체를 읽는 값은 절대 변하지 않는다고 가정하여 재구성 생략(skipping)을 최적화한다.
* 모든 프로퍼티는 val이어야 하며, 원시 타입이거나 똑같이 @Immutable로 표시된 타입이어야 한다.   

@Stable (안정적)

* @Immutable보다 가벼운 약속이다.
* 타입에 적용 시: "타입 자체는 가변적(mutable)일 수 있지만, @StableMarker의 3가지 요구 사항(변경 시 알림 등)을 준수한다"는 의미다.
    * (예: MutableState를 private으로 소유하고, 변경 시 컴포지션에 알리는 ViewModel 상태 객체) 
* 함수에 적용 시: "이 함수는 동일한 입력에 대해 항상 동일한 결과를 반환한다"는 의미다.
* 핵심: Composable 함수에 전달되는 모든 파라미터 타입이 @Stable 또는 @Immutable이면, 런타임은 파라미터 값이 이전과 동일한지 비교하고, 모두 같다면 재구성(recomposition)을 생략한다.
* 현재 상태
    * 지금 당장은 컴파일러가 @Immutable과 @Stable을 동일하게 취급하여 재구성 생략 최적화에 사용한다. 하지만 두 어노테이션이 분리된 이유는 미래에 더 세분화된 최적화를 위함이다.




<< 컴파일러 확장 등록 (Registering Compiler extensions) >>

Compose 컴파일러 플러그인은 Kotlin 컴파일 파이프라인에 자신을 등록하기 위해 ComponentRegistrar라는 메커니즘을 사용한다. 이 과정에서 런타임에 필요한 코드를 생성하고 라이브러리 사용을 돕는 여러 컴파일러 익스텐션(extension)을 등록한다. 또한 '라이브 리터럴' 활성화나 디버깅을 위한 소스 정보 포함 같이, 개발자가 활성화한 컴파일러 플래그에 따라 특정 익스텐션을 등록하기도 한다.



<< Kotlin 컴파일러 버전 (Kotlin Compiler version) >>

Compose 컴파일러는 매우 구체적인 Kotlin 버전을 요구하며, 버전이 일치하는지 가장 먼저 확인한다. 버전 불일치는 큰 방해 요소가 될 수 있다. suppressKotlinVersionCompatibilityCheck라는 매개변수를 사용해 이 버전 검사를 우회할 수도 있지만, 이는 개발자가 위험을 감수해야 하며 컴파일 오류로 쉽게 이어질 수 있다.


<< 정적 분석 (Static analysis) >>

컴파일러 플러그인의 표준 동작은 린팅(linting), 즉 정적 분석으로 시작한다. 정적 분석은 소스 코드를 스캔하여 라이브러리 어노테이션을 찾고, 런타임이 기대하는 방식대로 올바르게 사용되었는지 검사한다. 이 모든 검증은 컴파일러의 프론트엔드(frontend) 단계에서 이루어지며, IDE 플러그인과 통합되어 개발자에게 가능한 빠른 피드백을 제공한다.


<< 정적 검사기 (Static Checkers) >>

등록된 익스텐션 중 일부는 개발자가 코딩하는 동안 가이드를 제공하는 정적 검사기 형태로 제공된다. 이 검사기들은 함수 호출, 타입, 선언 등을 검사하여 1장에서 배운 Composable 함수의 요구 사항을 위반할 경우 즉각 IDE에 보고한다. 이 검사들은 개발자가 타이핑하는 동안 실행되므로, 사용자 경험이 버벅거리지 않도록 반드시 가볍고(lightweight) 빠르게 처리되어야 한다.



<< 호출 검사 (Call checks) >>

호출 검사기는 Composable 함수가 다양한 컨텍스트 내에서 올바르게 호출되는지 검증한다. 예를 들어 @DisallowComposableCalls나 @ReadOnlyComposable 어노테이션 범위 내에서의 호출을 검사한다. 이 검사기는 방문자 패턴(visitor pattern)을 사용해 소스 코드의 모든 호출(PSI 트리)을 반복적으로 방문한다. 때로는 단일 노드 분석만으로는 충분하지 않아 더 넓은 문맥이 필요한데 , 이때 컴파일러는 '컨텍스트 추적(context trace)'에 정보를 기록해 두었다가 더 복잡한 검증을 수행하는 데 사용한다.


주요 검사 항목은 다음과 같다:
* try/catch 블록이나 @DisallowComposableCalls 람다처럼 Composable 호출이 허용되지 않는 위치에서 호출이 발생했는지 확인한다.
* 인라인(inline) 함수가 Composable 컨텍스트에서 호출될 때, 해당 인라인 람다 안에서 Composable 함수를 호출할 수 있도록 올바르게 처리한다.
* 때로는 누락된 @Composable 어노테이션을 추가하도록 개발자에게 제안하기도 한다.
* @ReadOnlyComposable 함수가 읽기 전용이 아닌 다른 Composable 함수를 호출하여 계약을 위반하는지 검사한다.
* Jetpack Compose에서 지원하지 않는 Composable 함수의 참조(reference) 사용을 허용하지 않는다.  

<< 타입 검사 (Type checks) >>

컴파일러는 함수 호출뿐만 아니라 타입에 대해서도 검사를 수행한다. @Composable 어노테이션이 달린 타입을 예상했지만 실제로는 어노테이션이 없는 타입이 발견되면 오류를 보고한다


<< 선언 검사 (Declaration checks) >>

호출 위치나 타입 검사만으로는 충분하지 않으며, 프로퍼티, 접근자, 함수 선언, 매개변수 등 엘리먼트의 '선언 위치'도 분석해야 한다.

* 재정의(Override) 검사: Composable로 어노테이션된 프로퍼티나 함수가 재정의될 때, 재정의하는 쪽에서도 Composable 어노테이션이 붙어있는지 확인하여 일관성을 유지한다.
* suspend 금지: Composable 함수가 suspend 함수로 선언되었는지 확인한다. 이 둘은 서로 다른 목적을 위해 설계되었으며 함께 지원되지 않는다.
* 기타 금지: main 함수를 Composable로 만들거나, Composable 프로퍼티의 backing 필드를 선언하는 것도 금지된다.  


<< 진단 제지기 (Diagnostic suppression) >>

컴파일러 플러그인은 특정 상황에 대한 컴파일러의 기본 진단(오류 알림 등)을 '음소거'할 수 있는 익스텐션을 등록할 수 있다. 이는 Compose가 Kotlin이 일반적으로 허용하지 않는 특수한 코드를 생성하거나 지원하기 위해 필요하다.

* 인라인 람다의 non-source 어노테이션: Kotlin은 인라인 람다에 BINARY나 RUNTIME 보존 타입을 가진 어노테이션을 붙이는 것을 금지한다 (람다가 인라인되면 어노테이션이 붙을 대상이 사라지므로). 하지만 Compose의 진단 제지기는 이 어노테이션이 @Composable일 경우에만 이 검사를 특별히 우회한다. 이 덕분에 개발자는 함수 선언 시점이 아니라 호출하는 위치에서 람다에 @Composable 어노테이션을 붙이는 유연한 코드 작성이 가능하다. 
* 함수 유형의 명명된 매개변수: Kotlin은 함수 유형(람다)의 인수에 이름을 붙이는 것을 허용하지 않는다. 하지만 함수가 @Composable로 어노테이션 처리된 경우, Compose의 제지기가 이 검사를 우회하여 명명된 매개변수 사용을 허용한다. 
* 멀티플랫폼 지원: expect 클래스의 멤버와 같은 Kotlin 멀티플랫폼 관련 제약 사항도 동일하게 제지되어, Compose가 멀티플랫폼을 원활하게 지원하도록 한다.  


<< 런타임 버전 검사 (Runtime version check) >>

코드 생성 직전에, 컴파일러는 사용 중인 Compose Runtime의 버전을 확인한다. 각 Compose Compiler는 지원하는 최소 런타임 버전이 정해져 있으므로, 런타임이 누락되었거나 너무 오래된 버전이 아닌지 감지하는 과정이 필요하다. 이는 Kotlin 컴파일러 버전 검사 이후 두 번째로 수행되는 버전 검사다.



<< 코드 생성 (Code generation) >>

컴파일러 플러그인의 마지막 단계는 코드 생성이다. 어노테이션 프로세서와 달리 컴파일러 플러그인은 새로운 코드를 생성할 뿐만 아니라 기존 소스 코드를 수정할 수 있다.



<< 코틀린 IR (The Kotlin IR) >>

코드 생성 및 수정은 Kotlin 컴파일러의 백엔드 단계, 즉 IR(Intermediate Representation)에 접근하여 이루어진다. IR은 목표 플랫폼(JVM, JS 등)에 관계없는 중간 표현이다. Compose Compiler는 IrGenerationExtension을 구현하여 IR을 생성하고 수정한다.

* 핵심 작업: 이 IR 변형을 통해 개발자 몰래 Composable 함수에 암시적인 Composer 매개변수를 주입하고, 이 매개변수를 모든 하위 Composable 호출에 전달한다.
* 멀티플랫폼: IR을 생성함으로써 Compose가 생성하는 코드가 멀티플랫폼 호환성을 가질 수 있게 된다. 


<< 낮추기 (Lowering) >>

"낮추기(lowering)"는 고급 프로그래밍 개념(e.g., Composable 함수)을 컴파일러가 더 낮은 수준의 원자적인 개념(e.g., 런타임이 이해하는 그룹)으로 변환하는 작업을 의미한다. 이는 IR 코드 생성 단계에서 발생하며, IR 트리의 모든 엘리먼트를 방문하여 런타임의 요구 사항에 맞게 코드를 조정(변형)한다.


낮추기 단계에서 발생하는 주요 작업은 다음과 같다:
* 클래스 안정성을 추론하고 관련 메타데이터를 추가한다.
* 라이브 리터럴 표현식을 가변적인 상태(state) 인스턴스에 접근하도록 변환한다.
* Composable 함수에 암시적인 Composer 매개변수를 삽입하고 모든 하위 호출에 전달한다.
* Composable 함수의 본문을 래핑(wrapping)하여 다음과 같은 고급 기능을 구현한다:
    * 컨트롤 플로우(e.g., if문)를 위한 다양한 유형의 '그룹(group)'을 생성한다.
    * Kotlin의 기본 기능을 쓰지 않고, Compose가 자체적으로 디폴트 매개변수를 지원하도록 구현한다.
    * 함수가 스스로 재구성(recomposition)을 생략하는 법을 학습시킨다 (스마트 Recomposition).
    * 상태 변경 정보를 트리 아래로 전파하여 자동 재구성을 가능하게 한다.



<< 클래스 안정성 추론 (Inferring class stability) >>

'스마트 Recomposition'은 Composable의 입력값이 안정적(stable)일 때 재구성(recomposition)을 생략(skip)하는 것을 의미한다. 안정성은 런타임이 재구성 생략 여부를 판단하기 위해 입력값을 안전하게 비교할 수 있음을 의미하며, 런타임을 돕는 것이 궁극적인 목적이다.


<< 안정적인 타입의 3가지 특성 >>

Compose가 타입을 '안정적'으로 간주하기 위해 만족해야 하는 3가지 특성이 있다.
1. 일관된 equals: 두 인스턴스에 대한 equals 결과는 항상 동일해야 한다.
2. 변경 알림: public 프로퍼티가 변경되면, 컴포지션에 항상 그 변경 사항을 알려야 한다. 만약 알림 없이 값이 변하면, 컴포지션은 최신 상태를 알 수 없어 불일치가 발생하므로 , 이 경우 스마트 Recomposition은 불가능하다.
3. 안정적인 프로퍼티: 모든 public 프로퍼티 역시 원시 타입이거나 안정적인 타입이어야 한다.
원시 타입(String 포함)과 함수형 타입은 기본적으로 불변(immutable)이므로 안정적인 것으로 간주된다 . MutableState처럼 값이 변하더라도 변경 사항을 컴포지션에 알리기 때문에 안정적인 타입도 있다.


<< 컴파일러의 자동 추론 >>

개발자가 매번 @Immutable 또는 @Stable 어노테이션을 붙이는 것은 위험하고 유지보수가 어렵다. 따라서 Compose 컴파일러가 스스로 클래스의 안정성을 추론하는 것이 바람직하다.
컴파일러는 안정성을 추론할 자격이 있는 클래스들(일반 public 클래스나 데이터 클래스 등 )을 방문하여 @StabilityInferred라는 어노테이션과 $stable이라는 합성값(합성된 int 필드)을 클래스에 추가한다 . 런타임은 이 정보를 바탕으로 해당 클래스를 사용하는 Composable의 재구성 여부를 결정한다.



<< 안정성 추론 기준 >>

* 필드: 클래스의 모든 필드(JVM 바이트코드 기준)가 읽기 전용(val)이고 안정적인 타입이면, 해당 클래스는 안정적으로 추론된다 . var 필드가 있으면 불안정하게 추론된다.
* 제네릭: class Foo<T>(val value: T)처럼 제네릭 타입 T가 필드에 사용되면, Foo의 안정성은 런타임에 T에 전달될 타입의 안정성에 의존하게 된다. 컴파일러는 이 의존성을 비트마스크로 계산하여 @StabilityInferred 어노테이션에 기록한다.
* 중첩: Foo(val bar: Bar)처럼 다른 클래스로 구성된 경우, 안정성은 모든 매개변수 클래스의 안정성을 재귀적으로 조합하여 추론된다.
* 내부 상태: private var count처럼 클래스 내부에서만 사용하는 가변 상태(state)가 있어도, 이 값은 시간이 지남에 따라 변하므로 컴파일러는 이 클래스를 신뢰할 수 없고 불안정하다고 간주한다 .
* 인터페이스: 컴파일러는 List<T> 같은 인터페이스는 불안정하다고 가정한다. List가 불변 구현체(listOf)가 아닌 가변 구현체(ArrayList)로 전달될 수 있기 때문이다 .
* 추론 불가: 컴파일러가 public 프로퍼티의 불변성을 추론할 수 없는 경우, 기본적으로 불안정하다고 간주한다.

컴파일러가 모델을 불안정하다고 추론하더라도, 개발자가 해당 타입이 안정성의 계약을 지킨다고 확신하는 경우, 명시적으로 @Stable 어노테이션을 표기하여 컴파일러의 추론을 덮어쓰고 재구성 생략(skipping)을 유도할 수 있다


<< 라이브 리터럴 활성화 (Enabling live literals) >>

이것은 구현 세부 사항이며 자주 변경될 수 있다.
* 목적: 컴파일러 플래그(liveLiteralsEnabled)를 켜면, IDE의 Compose 미리 보기(Preview)가 리컴파일(recompilation) 없이 코드 변경 사항을 실시간으로 반영할 수 있게 된다.
* 작동 원리: 컴파일러가 코드에 하드코딩된 상수 표현식을 MutableState에서 값을 읽어오는 코드로 변환한다.
* 반환 예시 추가 학습하기 

<< Compose 람다식 기억법 (Compose lambda memoization) >>

컴파일러가 Composable 함수에 전달된 람다식을 최적화하여, 재구성이 발생할 때 람다 인스턴스를 불필요하게 다시 생성하지 않고 재사용하도록 만드는 IR 변환 작업이다.
두 가지 유형의 람다에 대해 수행된다.

1. Composable이 아닌 람다식 (Non-composable lambdas)

* Kotlin은 외부 변수를 캡처(capture)하지 않는 람다만 싱글톤으로 최적화한다. 만약 람다가 외부 변수를 캡처하면, 이 람다는 재구성될 때마다 새로운 인스턴스로 생성된다.
* Compose의 최적화: 컴파일러는 이처럼 '캡처하는 람다'를 자동으로 remember 호출로 감싸는 IR 코드를 생성한다.
* 작동 원리:
    1. 컴파일러는 remember 호출을 삽입한다.
    2. 람다가 캡처한 모든 값을 remember의 key로 자동 추가한다
    3. 캡처된 값(key)이 안정적(stable)이고 변경되지 않았다면 , remember는 새 람다 인스턴스를 만들지 않고 이전에 기억해 둔 람다 인스턴스를 재사용한다. 
이 최적화는 캡처하는 람다에만 적합하며 , 캡처된 값이 remember의 key로 사용되어 값이 달라질 때만 람다를 새로 만들도록 보장한다.


2. Composable 람다식 (Composable lambdas)
Composable이 아닌 람다와 마찬가지로 Composable 람다식도 '기억'(memoization)될 수 있으며, 최종 목표는 람다식을 슬롯 테이블에 저장하고 재사용하는 것이다.
컴파일러는 Composable 람다식을 composableLambda(...)라는 팩토리 함수 호출로 변형한다. 이 함수에는 다음과 같은 매개변수들이 전달된다

1. $composer: 현재 컨텍스트의 Composer.
2. $key: 해시코드와 소스 코드 위치를 조합한 고유 키. 위치 기억법(positional memoization)을 위해 사용된다 .
3. $shouldBeTracked: 람다가 외부 변수를 캡처하는지 여부. 만약 캡처하는 값이 없어 변경될 일이 없다면(false), Kotlin의 기본 람다 최적화처럼 싱글톤 인스턴스로 처리되어 추적할 필요가 없다 .
4. expression: 람다식 본문.  
이 팩토리 함수는 '교체 가능한 그룹(replaceable group)'을 컴포지션에 추가하여 런타임이 람다를 저장하고 검색하게 한다.
특히 Composable 람다식은 State<FunctionType>와 유사하게 구현되어 최적화된다. lambda(a, b) 호출은 lambda.value.invoke(a, b)와 동등하게 처리된다.

이는 "도넛 홀 생략하기(donut-hole skipping)"라는 지능적인 재구성(recomposition)을 가능하게 한다. 람다식이 트리 상위에서 업데이트되더라도, Compose는 중간 계층을 모두 건너뛰고 이 람다가 실제로 읽히는(호출되는) 트리의 가장 낮은 위치에서만 재구성을 수행할 수 있다



<< Composer 주입하기 (Injecting the Composer) >>

컴파일러 변환의 핵심 단계다. 컴파일러는 모든 Composable 함수에 $composer라는 합성 매개변수를 추가하여 기존 함수를 대체한다

이 $composer는 코드의 모든 Composable 함수 호출(람다 포함)에 전달되어, 트리 어느 지점에서나 항상 사용할 수 있게 된다. 이 작업으로 함수의 타입 시그니처가 변경되므로, 컴파일러는 타입 리매핑(remapping) 작업도 수행한다

변환된 함수 내부에서는 $composer.start(...)와 $composer.end() 같은 호출이 추가되어 그룹을 관리하며, 하위 Composable(e.g., Column, Text)을 호출할 때 $composer가 그대로 전달된다

(단, Composable이 아닌 inline 람다나 expect 함수는 이 변환에서 제외된다)



<< 비교 전파 (Comparison propagation) >>

컴파일러는 $composer 외에 $changed라는 또 다른 Int 타입 메타데이터 매개변수를 주입한다
이 매개변수는 이전 컴포지션 이후 입력 매개변수가 변경되었는지에 대한 힌트를 비트마스크(bitmask) 형태로 담고 있으며, 런타임이 재구성을 생략(skip)하는 데 사용된다


이 힌트(비트마스크 값)는 런타임에게 3가지 시나리오를 알려준다:
1. 정적(Static): 입력값이 리터럴 상수처럼 컴파일 타임에 이미 알려진 값이면, 런타임은 "이 값은 절대 변하지 않는다"는 것을 알고 equals() 비교 자체를 생략한다
2. 확실함(Certain): 상위 Composable에서 이미 비교가 끝났고 "항상 변경되지 않았다"고 보장되면, 런타임은 equals() 비교를 생략한다
3. 불확실(Uncertain): 힌트가 없으면(기본값 0), 런타임은 숏컷을 사용하지 않고 모든 매개변수를 슬롯 테이블의 이전 값과 equals()로 비교한다
컴파일러는 함수 본문에 이 $changed 값을 사용하는 로직을 주입한다. $dirty라는 로컬 변수가 이 비트마스크 값을 받고, "불확실"한 매개변수에 대해서만 $composer.changed(text)를 호출해 실제 변경 여부를 검사한다

최종적으로 $dirty 값이 (변경 없음)이고 컴포저가 'skipping' 모드라면, $composer.skipToGroupEnd()를 호출하여 함수 본문 실행 전체를 건너뛴다. 그렇지 않으면 본문이 실행(재구성)된다

이 정보는 하위 Composable 함수를 호출할 때도 그 함수의 $changed 매개변수를 통해 계속 전달되어야 하며, 이를 "비교 전파”라고 부른다 . 또한 $changed 비트마스크는 매개변수의 안정성(stable/unstable) 정보도 인코딩하여 최적화에 활용한다


<< 디폴트 매개변수 (Default parameters) >>

Kotlin의 기본 디폴트 매개변수 기능은 Composable 함수에서 그대로 사용할 수 없다. Composable 함수는 매개변수의 기본 표현식을 함수 본문의 범위(생성된 그룹) 내에서 실행해야 하기 때문이다
대신 Compose는 컴파일러를 통해 이 문제를 해결한다. 컴파일러는 Composable 함수에 $default라는 또 다른 Int 타입의 메타데이터 매개변수를 추가한다
이 $default 매개변수는 $changed처럼 비트마스크 기법을 사용한다. 이 비트마스크는 호출자가 어떤 매개변수의 값을 제공했는지, 아니면 생략했는지를 나타낸다


컴파일러는 함수 본문 내에 이 비트마스크를 확인하는 로직을 삽입한다. 예를 들어 fun A(x: Int = 0)는 fun A(x: Int, $changed: Int, $default: Int)로 변환된다 그리고 함수 본문에는 val $x = if ($default and 0b1 != 0) 0 else x와 같은 코드가 생성되어, 호출자가 값을 제공하지 않았다면($default 비트가 켜져 있다면) x에 기본값 0을 할당한다




<< 컨트롤 플로우 그룹 생성 (Control flow group generation) >>

컴파일러는 각 Composable 함수의 본문에 '그룹(group)'을 삽입한다. 이 그룹은 Composable 함수의 현 상태에 대한 모든 정보를 감싸고 보존한다
런타임은 이 그룹 정보를 바탕으로 컨트롤 플로우(e.g., if문)를 어떻게 다룰지 결정한다. 그룹은 또한 소스 코드의 호출 위치 정보를 기반으로 생성된 '키(key)'를 가지며, 이는 1장에서 배운 위치 기억법(positional memoization)을 가능하게 한다
컨트롤 플로우 구조에 따라 3가지 유형의 그룹이 생성된다

1. 교체 가능한 그룹 (Replaceable groups)

조건부 논리(e.g., if (condition) { Text("A") } else { Text("B") })에서 사용된다
if문의 Text("A")와 else문의 Text("B")는 각각 '교체 가능한 그룹'으로 감싸진다. condition이 true에서 false로 바뀌면, 런타임은 Text("A")에 해당하는 그룹을 Text("B") 그룹으로 '교체'한다
@NonRestartableComposable로 표시된 함수나 Composable 람다식(이전에 composableLambda 팩토리 함수로 변환된) 역시 이 교체 가능한 그룹으로 감싸진다


2. 이동 가능한 그룹 (Movable groups)

이 그룹은 정체성을 잃지 않고 재정렬이 가능하다
현재 이 그룹은 key 함수를 사용할 때만 활용된다. for 루프 내에서 key(talk.id) { Talk(talk) } 코드를 사용하면, Talk Composable은 '이동 가능한 그룹'으로 감싸진다
이 덕분에 talks 리스트의 순서가 변경되더라도, 런타임은 talk.id라는 고유한 정체성을 인식하여 노드를 파괴하고 새로 만드는 대신, 기존 노드의 '이동'으로 처리하여 효율을 높인다


3. 재시작 가능한 그룹 (Restartable groups)

아마도 가장 흥미로운 그룹일 것이다. 이 그룹은 '재시작 가능한' Composable 함수에만 삽입된다 (즉, 상태를 읽는 대부분의 일반적인 Composable 함수)
컴파일러는 함수 본문을 composer.startRestartGroup()과 composer.endRestartGroup()으로 감싼다
endRestartGroup() 함수는 특별히 nullable한 값을 반환한다

* null 반환: 만약 함수가 어떠한 상태(state)도 읽지 않았다면, 재구성이 필요 없으므로 null을 반환한다. 런타임은 이 함수를 재시작하는 방법을 알 필요가 없다
* null이 아닌 값 반환: 만약 함수가 상태를 읽었다면, endRestartGroup()은 람다를 생성하여 반환한다. 이 람다는 런타임에게 이 Composable을 "어떻게 재시작(다시 실행)해야 하는지" 알려주는 역할을 한다
A(x) 예제에서 endRestartGroup()는 updateScope { next -> A(x, next, $changed or 0b1) }를 호출하는 람다를 반환한다. 상태 변경 시 이 updateScope가 호출되어 A 함수가 재실행(재시작)된다
￼
<img width="697" height="435" alt="Pasted Graphic 10" src="https://github.com/user-attachments/assets/6ac260bb-5c04-4d79-8efb-fc30d51b964a" />





그룹 생성 요약

컴파일러는 다음과 같은 논리에 따라 그룹을 생성한다

* 블록이 항상 정확히 1번만 실행되면 그룹이 필요 없다
* if문처럼 여러 블록 중 하나만 실행되면, 각 블록 주위에 '교체 가능한 그룹'을 삽입한다
* key 함수 본문에는 '이동 가능한 그룹'을 사용한다
    * 그 외 상태를 읽는 재시작 가능한 함수에는 '재시작 가능한 그룹'을 사용한다 ??



Klib과 미끼 생성 (Klib and decoy generation)

* Todo - 추가 학습 필요

이것은 Kotlin/JS 및 .klib (멀티플랫폼)를 위한 컴파일러의 특별한 지원 기능이다

Kotlin/JS는 종속 라이브러리의 코드를 IR(중간 표현) 형태로 불러와서 사용(역직렬화)한다. 그런데 Compose 컴파일러는 1장에서 배운 것처럼 Composable 함수에 $composer, $changed 같은 합성 매개변수를 추가하며 함수의 시그니처(타입)를 변경한다. 이로 인해, 원본 코드의 함수 선언과 컴파일러가 변환한 IR의 함수 시그니처가 일치하지 않는 문제가 발생한다

해결책
Compose 컴파일러는 JVM에서처럼 기존 함수의 IR을 그냥 교체하는 대신, Kotlin/JS 환경에서는 '복사본'을 만드는 방식을 사용한다



[ Question ]

* Immutable VS Stable



