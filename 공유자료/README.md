## 스터디 중 공유된 자료를 아카이브합니다.

### [1기]
- [Exploring PausableComposition internals in Jetpack Compose](https://blog.shreyaspatil.dev/exploring-pausablecomposition-internals-in-jetpack-compose) <br>
LazyColumn 같은 리스트에서 “앞으로 곧 보여질 아이템”을 미리, 그리고 쪼개서 compose 하고, 실제로 보여줄 때는 한 번에 apply 해서 스크롤을 부드럽게 만드는 내부 메커니즘(PausableComposition)의 구조와 소스 코드 레벨 동작을 분석한 글입니다.

- RecomposeScope에 관련된 글입니다. 우리가 흔히 사용하고 있는 Column, Box 모두 internal로 만들어져 있습니다. internal 또는 그 이외에 RecomposeScope가 만들어지지 않는 경우는 어떤것들이 있을까요? 이는 아주 중요한 내용을 다루고 있습니다. <br>
[Scoped recomposition in Jetpack Compose — what happens when state changes?](https://blog.zachklipp.com/scoped-recomposition-in-jetpack-compose-what-happens-when-state-changes/) <br>
그 외 추가로 같이 보면 좋은 글입니다. <br>
[Deep dive into annotations in Jetpack Compose](https://blog.shreyaspatil.dev/deep-dive-into-annotations-in-jetpack-compose) <br>
[https://speakerdeck.com/jisungbin/2025-keompojeu-mabeobsa](https://speakerdeck.com/jisungbin/2025-keompojeu-mabeobsa) <br>

- skydoves님 아티클 참고해서 AOSP에서 개발중인 retain API 관련해서 간략하게 지훈님이 작성하신 글입니다. <br>
[[Compose] retain API
](https://velog.io/@mraz3068/Compose-Retain-API)

- skydoves님이 작성하신 컴포즈 컴파일러 안정성에 관한 깃허브 입니다. <br>
[compose-stability-inference](https://github.com/skydoves/compose-stability-inference)

- skydoves님이 만드신 Compose Stability Analyzer 사용 후기를 지훈님이 적은 글입니다. 사용방법에 대해 자세히 나와있습니다. <br>
[Compose Stability Analyzer 사용 후기](https://velog.io/@mraz3068/compose-stability-analyzer-review)

- 2025 드로이드 나이츠에서 강다현님께서 발표하신 컴포즈 상태 스냅샷 발표입니다. 개인적으로 5장을 읽기 전에 먼저 이 영상을 보는 것을 강력 추천합니다. <br>
[[DroidKnights 2025] 강다현 - 컴포즈 스냅샷 내부원리 찍어먹기](https://www.youtube.com/watch?v=FfeOd_zzfYk)

- 컴포즈의 SlotTable은 GapBuffer를 사용합니다. Gap Buffer는 슬롯 배열 중간에 ‘빈 틈(gap)’을 하나 두고, 삽입·삭제가 일어날 위치로 그 틈만 움직여 나머지 슬롯 복사는 최소화하는 자료구조(배치 방식)입니다. <br>
[Understanding Gap Buffers in Jetpack Compose: The 60-Year-Old Algorithm Powering Modern Android UI](https://proandroiddev.com/understanding-gap-buffers-in-jetpack-compose-the-60-year-old-algorithm-powering-modern-android-ui-a07ca2013e64)

- LaunchedEffect는 내부 블럭에 작업에 상관없이 새로운 코루틴스코프를 만듭니다. 내부 블럭에서 suspend 함수를 호출할때만 launchedEffect 만들 수 있도록 하는 skydoves님의 라이브러리입니다. <br>
[compose-effects](https://github.com/skydoves/compose-effects)

- 최근 Compose 팀에서 현재 사용 중인 갭 버퍼(gap buffer) 대신 연결 리스트(linked list) 유사한 데이터 구조를 SlotTable에 적용하는 것을 목표로 한다는 것을 알고 계신가요? 이 변경의 목적은 SlotTable을 편집(edit) 할 때의 성능을 향상시키면서도, SlotTable을 생성(build) 할 때의 기존 성능을 유지하는 것이라 합니다. <br>
[https://android-review.googlesource.com/c/platform/frameworks/support/+/2644758](https://android-review.googlesource.com/c/platform/frameworks/support/+/2644758)


