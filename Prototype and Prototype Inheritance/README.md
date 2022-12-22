# 프로토타입

> JavaScript는 클래스라는 개념이 없기 때문에 기존의 `객체를 복사`하여 새로운 객체를 생성하는 `prototype`기반의 언어다.<br/>
> 프로토타입 기반 언어는 객체 원형인 프로토타입을 이용하여 새로운 객체를 만들어내는데, 이 객체는 또 다른 객체의 원형이 될 수 있다.

### 함수와 객체 내부 구조

JavaScript에서 함수를 정의하면, 함수 멤버로 `prototype`이 있다. <br/>
이 `prototype`은 다른 곳에 생성된 함수의 프로토타입 객체를 참조하며, `prototype`객체 멤버인 constructor 속성이 함수를 참조하는 내부 구조를 가진다.
<div align="left">
<img src="https://user-images.githubusercontent.com/87024040/209089085-60dc329b-3be1-49a1-b3d7-b66a3c052492.png" width="600" height="300">
</div>
