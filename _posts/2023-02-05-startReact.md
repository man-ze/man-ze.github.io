---
layout: single
title: "리액트 시작"
category: React
tag: [React]
# 테이블 목차
toc: true
# author_profile 이 false 이면 게시글에 들어갔을 때 왼쪽 사이드바가 접힘
author_profile: true
published: true
sidebar:
  nav: "docs"
---

## 프레임워크 vs 라이브러리

프로그램 <span style="color:red">제어흐름 권한</span>에 따라 프레임워크(Framework)와 라이브러리(Library)를 구분한다.<br/>
프레임워크는 제어권한이 프로그램에 있는 반면, 라이브러리는 개발자에게 있다. <br/>
<span style="background-color:#fff5b1">라이브러리는 특정 기능이 담긴 코드나 함수의 집합체</span>이기 때문에,
사용자가 필요에 의해 해당 기능을 호출해서 사용할 수 있다. <br/>
리액트의 대표적 라이브러리로는
useState(컴포넌트 상태 관리), reactRouter(리액트 라우팅 관리) 등을 들 수 있다.

## 리액트의 장점과 단점

> 장점

1. 빠른 업데이트와 렌더링 속도 (Virtual DOM 사용)
2. 컴포넌트 기반 구조
3. 높은 재사용성 (유지보수가 용이함)
4. 앱을 개발하고 싶을 경우 React Native 로 개발가능

> 단점

1. 높은 상태관리 복잡도

🙄 [Virtual DOM 이란?](https://velopert.com/3236) <br/>
DOM이 변경되었을 경우, 일반 브라우저는 변경이 된 모든 DOM을 찾는 과정을 거치기 때문에 많은 비용과 시간이 소모된다. <br/>반면 리액트에서 사용하는 virtual DOM(가상객체)은 DOM이 변경된 경우, <br />변경된 부분만을 찾아 virtual DOM에 저장한 후 기존 DOM에 던져준다.
<br/>
극단적으로 말하자면 일반 브라우저는 30개의 DOM이 변경된 경우 30번의 계산과 리렌더링 과정이 필요한 반면, <br/>리액트의 virtual DOM은 1번이면 충분하다는 것이다.

🙄 [SPA란?](https://velog.io/@gwanuuoo/SPA%EB%8A%94-%EA%B8%B0%EC%A1%B4-%EC%9B%B9%EC%82%AC%EC%9D%B4%ED%8A%B8%EC%99%80-%EC%B0%A8%EC%9D%B4)
최신 웹 트랜드는 SPA (Single Page Application) 로 정의된다. <br/>
SPA 는 최초 실행 시 브라우저에 전체 페이지를 로드하고, 페이지간 이동시에는
필요한 데이터만 전달받아 사용하는 것을 의미한다.

## 프로젝트 생성

✔리액트를 시작하려면 기본적으로 <span style="background-color:#fff5b1">node.js 설치가 선행되어야 한다.</span>

1. 프로젝트 폴더 만들기

리액트 작업 폴더에서 cmd 또는 powershell 실행 후 명령어 입력 <br/>
<span style="background-color:#dcffe4">npx create-react-app "프로젝트명"</span>

2. 리액트 실행 화면 미리보기(go live) <br/>

<span style="background-color:#dcffe4">npm start</span>

## JSX 에 대해

### JSX 정의

<span style="color:#8e44ad">JSX = Javascript + XML / HTML</span>

JSX 란 자바스크립트와 HTML 코드가 결합된 자바스크립트의 확장 문법이다. <br/>
JSX 는 XML, HTML 코드를 자바스크립트 코드로 변환해준다. <br/>
JSX 를 사용하면 코드가 간결해지기 때문에 가독성이 향상된다.

### JSX 문법

> class 를 선언할 때는 class 대신 className 로 선언하기

```
   // 기존 html 에서 class 선언 시
   <div class="container"></div>

   // JSX 에서 class 선언 시
   <div className="container"></div>
```

> 반복문(if, for) 사용 불가 (☞<span style="background-color:#fff5b1">삼항연산자 또는 map</span>등 주변 요소 이용할 것)

> 변수 데이터 바인딩 시 중괄호{} 문법 적용 가능

```
 const a = 'JSX 중괄호 문법';
 <div className="container">
  <h3>{ a }</h3> // JSX 중괄호 문법
 </div>
```

```
 const a = 'JSX 중괄호 문법';
 <div className={a}> // className 에 'JSX 중괄호 문법' 이 바인딩 됨
 </div>
```

> inline style 넣을 때 객체 형태로 넣어주되, 속성은 카멜케이스로 선언해야한다.
