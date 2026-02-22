## 스터디 중 공유된 자료를 아카이브합니다.

### [1기]
- [Exploring PausableComposition internals in Jetpack Compose](https://blog.shreyaspatil.dev/exploring-pausablecomposition-internals-in-jetpack-compose) <br>
LazyColumn 같은 리스트에서 “앞으로 곧 보여질 아이템”을 미리, 그리고 쪼개서 compose 하고, 실제로 보여줄 때는 한 번에 apply 해서 스크롤을 부드럽게 만드는 내부 메커니즘(PausableComposition)의 구조와 소스 코드 레벨 동작을 분석한 글입니다.

- [Scoped recomposition in Jetpack Compose — what happens when state changes?](https://blog.zachklipp.com/scoped-recomposition-in-jetpack-compose-what-happens-when-state-changes/) <br>
  RecomposeScope에 관련된 글입니다. 우리가 흔히 사용하고 있는 Column, Box 모두 inline으로 만들어져 있습니다. inline 또는 그 이외에 RecomposeScope가 만들어지지 않는 경우는 어떤것들이 있을까요? 이는 아주 중요한 내용을 다루고 있습니다. <br>
그 외 추가로 같이 보면 좋은 글입니다. <br>
[Deep dive into annotations in Jetpack Compose](https://blog.shreyaspatil.dev/deep-dive-into-annotations-in-jetpack-compose) <br>
[https://speakerdeck.com/jisungbin/2025-keompojeu-mabeobsa](https://speakerdeck.com/jisungbin/2025-keompojeu-mabeobsa)

- [[Compose] retain API](https://velog.io/@mraz3068/Compose-Retain-API) <br>
  skydoves님 아티클 참고해서 AOSP에서 개발중인 retain API 관련해서 간략하게 지훈님이 작성하신 글입니다.

- [compose-stability-inference](https://github.com/skydoves/compose-stability-inference) <br>
  skydoves님이 작성하신 컴포즈 컴파일러 안정성에 관한 깃허브 입니다.

- [Compose Stability Analyzer 사용 후기](https://velog.io/@mraz3068/compose-stability-analyzer-review) <br>
  skydoves님이 만드신 Compose Stability Analyzer 사용 후기를 지훈님이 적은 글입니다. 사용방법에 대해 자세히 나와있습니다.

- [[DroidKnights 2025] 강다현 - 컴포즈 스냅샷 내부원리 찍어먹기](https://www.youtube.com/watch?v=FfeOd_zzfYk) <br>
  2025 드로이드 나이츠에서 강다현님께서 발표하신 컴포즈 상태 스냅샷 발표입니다. 개인적으로 5장을 읽기 전에 먼저 이 영상을 보는 것을 강력 추천합니다. 

- [Implementing snapshot-aware data structures](https://blog.zachklipp.com/implementing-snapshot-aware-data-structures/) <br>
Jetpack Compose의 스냅샷(MVCC) 상태 시스템이 내부적으로 어떻게 동작하는지와, StateObject/StateRecord를 이용해 스냅샷을 인지하는 커스텀 자료구조를 구현하는 방법을 설명하는 글입니다. <br>

- [Understanding Gap Buffers in Jetpack Compose: The 60-Year-Old Algorithm Powering Modern Android UI](https://proandroiddev.com/understanding-gap-buffers-in-jetpack-compose-the-60-year-old-algorithm-powering-modern-android-ui-a07ca2013e64) <br>
컴포즈의 SlotTable은 GapBuffer를 사용합니다. Gap Buffer는 슬롯 배열 중간에 ‘빈 틈(gap)’을 하나 두고, 삽입·삭제가 일어날 위치로 그 틈만 움직여 나머지 슬롯 복사는 최소화하는 자료구조(배치 방식)입니다. <br>

- [compose-effects](https://github.com/skydoves/compose-effects) <br>
LaunchedEffect는 내부 블럭에 작업에 상관없이 새로운 코루틴스코프를 만듭니다. 내부 블럭에서 suspend 함수를 호출할때만 launchedEffect 만들 수 있도록 하는 skydoves님의 라이브러리입니다.

- [https://android-review.googlesource.com/c/platform/frameworks/support/+/2644758](https://android-review.googlesource.com/c/platform/frameworks/support/+/2644758) <br>
최근 Compose 팀에서 현재 사용 중인 갭 버퍼(gap buffer) 대신 연결 리스트(linked list) 유사한 데이터 구조를 SlotTable에 적용하는 것을 목표로 한다는 것을 알고 계신가요? 이 변경의 목적은 SlotTable을 편집(edit) 할 때의 성능을 향상시키면서도, SlotTable을 생성(build) 할 때의 기존 성능을 유지하는 것이라 합니다.

- [Jetpack Compose: LazyColumn/LazyRow 내부 코드 분석 ~ 1부 LazyColumn/LazyRow](https://pluu.github.io/blog/android/androidx/2025/01/10/LazyList-1/) <br>
LazyList에 대한 시리즈 분석글입니다. 처음 LazyColum/Row의 구조에 대한 흐름도를 보여줘서 이해하기 쉬운 것 같습니다.

### [2기]
- [Conscious Compose optimization](https://proandroiddev.com/conscious-compose-optimization-e16144b80eef#039b) <br>
  [Conscious Compose optimization 2: Tackling composition](https://proandroiddev.com/conscious-compose-optimization-2-tackling-composition-f3e42ce3069d) <br>
  위 글은 Compose의 성능 최적화 방법을 다양하게 소개하고 있습니다. 처음 최적화에 대해 알게 됐거나 이미 알고 있어도 다시 보면 좋은 내용입니다.

- [VS Code가 1GB 파일을 여는 방법: Rope에서 Piece Table까지](https://www.pwnz.kr/posts/text-editor-data-structures) <br>
  대용량 텍스트를 빠르게 편집하기 위해 일반 문자열의 한계를 출발점으로 Gap Buffer·Rope·Piece Table을 비교하고, VS Code가 Piece Table을 확장한 ‘Piece Tree’로 1GB급 파일도 효율적으로 여는 원리를 설명한 글입니다.
  컴포즈에서는 Gap Buffer를 사용하고 있고 흔히 텍스트 편집기에서 사용되는데요. Gap Buffer에 대한 짧은 내용이 존재하고 이외 다른 자료구조의 특징들을 실행 가능한 시각화된 자료로 파악할 수 있어 좋습니다.

- [Animations with Lookahead in Jetpack Compose](https://proandroiddev.com/animations-with-lookahead-in-jetpack-compose-60423fe0d1a7) <br>
  [Container Transform Animation with Lookahead in Jetpack Compose](https://medium.com/@pushpalroy2007/container-transform-animation-with-lookahead-in-jetpack-compose-1c0db2a9b0a9) <br>
  [Using LookaheadScope and intermediateLayout for bottom bar inside Compose navigation](https://medium.com/@hanas.marc/using-lookaheadscope-and-intermediatelayout-for-bottom-bar-inside-compose-navigation-e5abd0cb3cdc) <br>
  책에는 LookaheadLayout으로 나오지만 LookaheadScope로 변경이 되었습니다. 읽어볼만한 글 3개를 공유합니다.

- [ModifierOrderGuesser](https://marcinmoskala.com/ModifierOrderGuesser/) <br>
  Compose 개발을 하면 Modifier를 반드시 사용할 수 밖에 없는데요. Modifier의 순서는 중요합니다. 게임을 통해 수련해봅시다. <br>
  (추가로 https://kt.academy/ 이곳의 상단 'Free resources'에 가시면 다양한 게임을 해볼 수 있습니다. 강력추천!)

- [Compose Modifiers deep dive](https://www.youtube.com/watch?v=BjGX2RftXsU) <br>
  책의 주석에 있는 Modifier에 관한 영상입니다. 왜 초기에 컴포즈의 Modifier가 성능이 안 좋았고 왜 각각의 Node로 만들게 됐는지 그 역사를 알려줍니다. 영어라 불편하다면 노트북LM으로 만들어서 보는 것을 추천드립니다.

- [[Compose] Snapshot System을 분석해보자](https://haeti.palms.blog/compose-snapshot-system) <br>
  스냅샷 시스템에 대해 이해하기 쉽게 잘 설명되어 있는 글입니다. derivedStateOf()가 내부적으로 Snapshot을 어떻게 사용하는지 알려줍니다.

- [CMP Scrollbar <br>](https://kotlinlang.org/docs/multiplatform/compose-desktop-scrollbars.html)
   구글 공식문서를 보면 [Scrollbar](https://developer.android.com/jetpack/androidx/compose-roadmap?hl=ko)가 아직 지원예정입니다. 그런데 JetBrains에서 스크롤바를 먼저 만들었는데요.
   저는 실제 실무에 해당 Scrollbar를 적용했습니다. 다만, CMP 용으로 만들어져서 안드 프로젝트에는 직접 적용할 수 없고, 해당 라이브러리를 안드로이드에서만 사용할 수 있도록 만든 어떤 개발자분의 Github 레포를 적용했습니다.
   Scrollbar에 대해 공부하고 싶다면 CMP의 Scrollbar를 파헤쳐 봅시다!

- [Enhance Compose Recomposition Performance with produceState and rememberUpdatedState #826 <br>](https://github.com/skydoves/landscapist/pull/826)
   2기 스터디원으로 참여하신 가은님이 Landscapist 라이브러리의 컨트리뷰터가 되셨습니다! 적절한 Side Effect Handler를 사용하여 Recomposition을 개선한 내용을 확인해보시죠! <br>
   추가로 해당 coil과 Landscapist의 성능을 비교한 [글](https://velog.io/@geun5744/Compose-%EC%9D%B4%EB%AF%B8%EC%A7%80-%EB%A1%9C%EB%94%A9-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC-%EC%84%B1%EB%8A%A5-%ED%83%90%EA%B5%AC-AsyncImage-CoilImage) 까지 올려주셨습니다!

### [3기]
- [🚀 Jetpack Compose Internals: How Slot Table + Gap Buffer Make UI Fast](https://blog.stackademic.com/jetpack-compose-internals-how-slot-table-gap-buffer-make-ui-fast-83fe7ad12494) <br>
  [Inside Jetpack Compose](https://medium.com/%40takahirom/inside-jetpack-compose-2e971675e55e) <br>
  갭 버퍼에 관한 짧은 첫번째 글과 컴포즈 내부 구조와 함께 갭버퍼에 대한 내용이 소개된 두 번째 글을 공유합니다.

