---
title: "JavaScript - addEventListener"
excerpt: "JavaScript : addEventListener의 이해 "

toc: true
toc_sticky: true

categories:
  - JavaScript	
tags: 
  - JavaScript
  - addEventListener

last_modified_at: 2021-08-23T12:15:00

---

# EventTarget.addEventListener()

**EventTarget**의 **addEventListener()** 메서드는 지정한 이벤트가 대상에 전달될 때마다 호출할 함수를 설정한다. 일반적인 대상은 Element, Document, Window지만, **XMLHttpRequest**와 같이 이벤트를 지원하는 모든 객체를 대상으로 지정할 수 있다.

**addEventListener()**는 **EventTarget**의 주어진 이벤트 유형에, **EventListener**를 구현한 함수 또는 객체를 이벤트 처리기 목록에 추가해 작동한다.

## 구문

```js
target.addEventListener(type, listener[, options]);
target.addEventListener(type, listener[, useCapture]);
target.addEventListener(type, listener[, useCapture, wantsUntrusted]); // Gecko/Mozilla
```

### 매개변수

**`type`**

​	반응할 [이벤트](https://ingyeomnote.github.io/javascript/js-event/) 유형을 나타내는 대소문자 구분 문자열.

**`listener`**

​	지정된 타입의 이벤트가 발생했을 때, 알림(**Event**인터페이스를 구현하는 객체)을 받는 객체이다. **EventListener** 인터페이스 또는 **JavaScript** **function**를 구현하는 객쳐여야만 한다. 

**(EventListener** : EventListener 인터페이스는 EventTarget 객체로부터 발생한 이벤트를 처리하기 위한 오브젝트를 말한다.)

**`options` `Optional`**

​	이벤트 리스너에 대한 특성을 지정하는 옵셕 객체이다. 사용 가능한 옵션은 다음과 같다.

- **capture**: **DOM 트리**의 하단에 있는 **EventTarget** 으로 전송하기 전에, 등록된 **listener**로 이 타입의 이벤트의 전송 여부를 나타내는 **Boolean** 이다. 
- **once: **리스너를 추가한 후 **한 번만 호출**되어야 함을 나타내는 **Boolean**이다. **true**이면 호출 할 때 listener가 자동으로 **삭제**된다.
- **passive:** **true**일 경우, listener에서 지정한 함수가 **preventDefault()**를 호출하지 않음을 나타내는 **Boolean**이다. **passive listener**가 **preventDefault()**를 호출하면 **user agent**는 콘솔 경고를 생성하는 것 외의 작업은 수행하지 않는다.

**`useCapture` `Optional`**

​	DOM트리의 하단에 있는 EventTarget으로 전송하기 전에, 등록된 listener로 이 타입의 이벤트의 전송여부를 나타내는 Boolean이다. 트리에서 위쪽으로 버블링되는 이벤트는 캡처를 사용하도록, 지정된 listener를 트리거하지 않는다. 이벤트 버블링과 캡쳐는 두 요소(엘리먼트)가 해당 이벤트에 대한 핸들(함수)를 등록한 경우, 다른 요소 내에 중첩된 요소에서 발생하는 이벤트를 전파하는 두 가지 방법이다.

