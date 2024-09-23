## beforeCreate()
* 앱이 완전히 초기화 되기 이전에 호출
* 화면에 아직 아무런 랜더링이 시작안됨
* 아래의 이미지 처럼 아직 화면에 랜더링이 시작되기 전에 break 에 걸리는 것을 확인할수 있다.

![[Pasted image 20240908150629.png]]

## created()
* 앱이 완전히 초기화 된 이후 호출
* 화면에 아직 아무런 랜더링이 시작안됨
* created 또한 beforeCreate 훅 과 동일 하게 아직 화면에 랜더링이 시작되기 전에 호출 됨

![[Pasted image 20240908150844.png]]

## Compile Template
* 모든 동적 플레이스 홀더와 **{{ }}** 와 같은 문법을 html 로 변환하는 단계

## beforeMount()
* 화면에 무언가를 표시하기 직전의 단계 

## mounted()
* 화면에 무언가를 표시하는 단계 
* 모든 콘텐츠가 있는 html 요소를 추가하는 단계
* 다음 이미지 처럼 beforeMount 이후 실행 하는  mounted 에는 화면에 무엇 인가 표시되기 시작함 Compile Template 을 끝마친 상태이기 때문에 화면에 표시되는 출력값이 생김

![[Pasted image 20240908151132.png]]

## Mounted Vue Instance

* 마운트 된 새로운 Vue 인스턴스가 완성됨
### instance Unmounted

* 종종 인스턴스 vue 앱 을 Unmounted 경우가 있다.
* Unmounted 될 경우 화면의 모든 콘텐츠가 삭제 되며 앱의 사용이 불가능하다.
* 아래의 두 라이프 사이클(beforeUnmounted,Unmounted) 는 코드 실행을 정리할때 사용할수 있다.
* 서버에서 http 요청을 보내서 마운트 해제되는 앱을 추적하거나 할때 사용할수 있다.
* Unmounted 를 할 경우 기존에 정의한 mounted id 값 `#app` 의 정의 된 부분에서 화면이 전부 제거가 된다.

![[Pasted image 20240908152235.png]]
#### beforeUnmounted

* 콘텐츠 삭제 직전 실행되는 단계이다.

#### Unmounted 
* 콘텐츠 삭제후 실행되는 단계

## Data Changed
* 데이터의 변경이 이뤄질 경우 새로운 생명 주기를 실행함
* 아래의 두개의 생명주기는 화면의 업데이트가 필요할 때 만 실행되는 것 으로 화면의 업데이트 사항이 없을 경우에는 실행을 안함
* ![[화면 캡처 2024-09-08 151652.png]]

## beforeUpdate() 

* 앱 내에서 업데이트를 완전히 처리하지 않았을때에 대한 단계
* 아래의 이미지 처럼 사용자가 입력한 내용이 아직 화면에 반영이 안된 단계
* 
![[Pasted image 20240908151512.png]]

## updated 

* **beforeUpdate** 처리가 완료되었을때 에 대한 단계 
* 사용자가 입력한 내용이 화면에 반영되는 단계
![[Pasted image 20240908151602.png]]

**만약 beforeUpdate ,  updated 단계를 거쳐 화면에 업데이트가 완전히 이루어 질 경우 다시 mounted 를 할 필요가 없다**
