---
date: 2025 년 03 월 19 일 17 시 03 분
tags:
  - Dev
  - Thymeleaf
  - attribute
  - 속성
author: Joung Dong Hee
share: true
---
## 속성 - Attribute

Thymeleaf에서는 HTML 속성에 동적으로 값을 설정할 수 있습니다. 이를 통해 HTML 속성을 유연하게 조작하고 다양한 조건에 맞춰 속성 값을 설정할 수 있습니다.

### 1. 속성 값 설정

HTML 속성 값을 설정하려면 `th:attr`을 사용하면 됩니다. `th:attr`은 HTML에서 사용되는 속성의 값을 설정할 때 유용합니다. 사용 가능한 속성은 [타임리프 공식 홈페이지](https://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#setting-the-value-of-any-attribute)에서 확인할 수 있습니다.

예를 들어, 다음과 같이 `th:attr="class='large'"`를 사용하면, 렌더링 시 기존 `class="text"` 속성은 사라지고 `class="large"`로 대체됩니다.

```html
<input type="text" class="text" th:attr="class='large'"/>
```

렌더링 후:

```html
<input type="text" class="large">
```
### 2. 속성 명 지정 (간편한 방법)

속성 값을 설정할 때, `th:attr` 대신에 속성명을 직접 지정하는 방법이 더 간단하고 직관적입니다. 예를 들어 클래스를 변경하고자 할 경우 `th:class`를 사용하여 `class` 속성을 변경할 수 있습니다.

```html
<input type="text" class="text" th:class="'large'" />
```

렌더링 후:

```html
<input type="text" class="large">
```
### 3. 속성 값 추가

기존에 선언된 속성 값에 추가하려면 `th:attrappend`를 사용합니다. 예를 들어, `class="text"`에 `large` 값을 뒤에 추가하고자 할 경우 아래와 같이 사용합니다.

```html
<input type="text" class="text" th:attrappend="class='large'" /><br/>
```

이렇게 하면 `class="textlarge"`가 렌더링됩니다. 만약 띄어쓰기를 넣지 않고 붙여서 추가하려면, `th:attrappend`에 띄어쓰기를 명시적으로 추가해야 합니다.

```html
<input type="text" class="text" th:attrappend="class=' ' + 'large'" />
```

또한, `th:classappend`와 `th:styleappend`를 사용하면 `class`와 `style` 속성에 값을 추가하는 간편한 방법을 제공합니다. 이와 같은 방법으로 다중 속성을 설정할 수도 있습니다. 예를 들어, 여러 속성 값을 한 번에 설정하고 싶다면, `th:attr`을 사용하여 `class`와 `style` 속성을 동시에 설정할 수 있습니다:

```html
<input type="text" th:attr="class='large' style='color: red;'" />
```


이렇게 하면 `class="large"`와 `style="color: red;"`가 동시에 적용되어 렌더링됩니다.

### 4. 고정 boolean 속성

일부 HTML5/XHTML 속성은 고정된 값이 있으며, 값에 상관없이 해당 속성이 존재하면 적용됩니다. 예를 들어, `checked` 속성은 `true`나 `false`와 상관없이 속성이 존재하면 체크박스가 체크된 상태로 렌더링됩니다.

```html
<input type="checkbox" name="active" checked="false">
```

위 코드에서 `checked="false"`를 명시했지만, 실제로는 다음 이미지 와 같이 체크박스가 체크된 상태로 렌더링됩니다.

![image.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20250319181225.png)

렌더링 후:

```html
<input type="checkbox" name="active" checked="checked">

```


이러한 속성에 대해 Thymeleaf에서는 값을 `false`로 설정하면 해당 속성이 렌더링되지 않습니다. 예를 들어, `th:checked="true"`와 `th:checked="false"`로 설정하면, `false`인 경우 `checked` 속성이 아예 생기지 않습니다.

```html
<input type="checkbox" name="active" th:checked="true" /><br/>  
<input type="checkbox" name="active" th:checked="false" /><br/>
```


렌더링 후:

```html
<input type="checkbox" name="active" checked="checked">
<input type="checkbox" name="active">
```

`checked` 외에도 `readonly`, `multiple` 등의 고정 boolean 속성이 있으며, 이들 속성에 대한 자세한 정보는 [타임리프 공식 홈페이지](https://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#fixed-value-boolean-attributes)에서 참고할 수 있습니다.
