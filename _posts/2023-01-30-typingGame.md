---
layout: single
title: "자바스크립트 타자게임 클론코딩 후기"
category: JS
tag: [JS, Git]
# 테이블 목차
toc: true
# author_profile 이 false 이면 게시글에 들어갔을 때 왼쪽 사이드바가 접힘
author_profile: true
sidebar:
  nav: "docs"
---

## 참고 사이트

혼자서 프로젝트를 해보려니, 프로그래밍을 짜는 로직과 그걸 짜는 코드에 대한 감각이 떨어진다는 것을 느끼게 되었다.

그래서 유튜브에서 제공하는 무료 클론코딩 강의를 몇 개를 수강하면서 바닐라 자바스크립트에 대한 이해를 높이고

토이 프로젝트에 대한 경험을 쌓으려고 한다.

강의는 데브리님의 [Vanilla 자바스크립트 타자게임 만들기! 실전코스](https://www.youtube.com/watch?v=_CsGSE5gwTA&list=PLpJDjPqxGWGrSGPUBqWlsJlcLF_grNClK) 를 바탕으로 진행했다.

## 타자게임 로직

> [내가 만든 타자게임](https://man-ze.github.io/TypingGame/)

1. 페이지가 렌더링 되었을 때 (init())

   1. 페이지가 렌더링 되면 버튼 상태가 _"게임로딩중"_ 이 되었다가, *"게임시작"*으로 변경

2. 게임중일때

   1. 남은시간이 최초 할당된 시간에서 점점 가감됨

   2. 제시된 단어와 user 가 input 창에 입력한 단어의 일치여부 확인 후, 정답인 경우 획득점수 가산

   3. user 가 정답을 입력할 경우 남은 시간은 초기화 / 시간 내 정답을 입력하지 못할 경우 게임이 끝남

   4. 게임이 끝날 경우 버튼은 _"다시시작"_ 상태로 변경

   5. 제시된 단어는 랜덤 API 사이트에서 추출

      (_배열로 개발자가 직접 단어를 할당할 수 있으나 번거롭고 단어가 정해져있다는 한계_)

## 소스코드

```javascript
const GAME_TIME = 9; // game 시간을 상수로 설정하여 하드코딩 방지
let score = 0;
let time = GAME_TIME;
let isPlaying = false;
let timeInterval;
let checkInterval;
let words = [];

const inputWord = document.querySelector(".input");
const changeWord = document.querySelector(".changeWord");
const scoreShow = document.querySelector(".score");
const timeDisplay = document.querySelector(".time");
const button = document.querySelector(".loading");

init();

// 화면이 렌더링 되었을 때 바로 실행되는 초기값
function init() {
  buttonChange("게임로딩중");
  getWords();
  // eventListener 콜백함수로 checkMatch 함수 사용
  inputWord.addEventListener("input", checkMatch);
}

// 게임 실행 여부 확인
function checkStatus() {
  if (!isPlaying && time === 0) {
    buttonChange("다시시작"); // if 문을 만족할 경우 다시시작
    clearInterval(checkInterval);
  }
}

// 버튼 클릭 시 interval 실행 (게임실행)
function run() {
  if (isPlaying) {
    return; // 게임중인 경우 버튼을 눌러도 중복이 안되게
  }
  isPlaying = true;
  time = GAME_TIME;
  inputWord.focus(); // 마우스 포커스 이동
  scoreShow.innerText = 0; // 획득점수 초기화
  timeInterval = setInterval(countDown, 1000); // 1초마다 한번씩 countDown() 실행
  checkInterval = setInterval(checkStatus, 50); // 0.05 초마다 한번씩 checkStatus() 실행
  buttonChange("게임중");
}

// 단어장
function getWords() {
  axios
    .get("https://random-word-api.herokuapp.com/word?number=100")
    .then(function (response) {
      // 성공 핸들링

      response.data.forEach((newWord) => {
        if (newWord.length < 10) {
          words.push(newWord);
        }
      });
      console.log(words);

      buttonChange("게임시작");

      // words = response.data;
    })
    .catch(function (error) {
      // 에러 핸들링
      console.log(error);
    });
}

// 단어 일치 체크하고 점수 상향하기
function checkMatch() {
  if (inputWord.value.toLowerCase() === changeWord.innerText.toLowerCase()) {
    score++;
    if (!isPlaying) {
      return; // !isPlaying 를 만족하면 게임중단 (함수중단)
    }
    scoreShow.innerText = score;
    inputWord.value = ""; // 초기화
    time = GAME_TIME;

    // API 의 단어를 랜덤한 인덱스로 선별
    const randomIndex = Math.floor(Math.random() * words.length); // Math.floor 로 소수점 자르기
    changeWord.innerText = words[randomIndex];
  }
}

// 남은시간
function countDown() {
  time > 0 ? time-- : (isPlaying = false);
  if (!isPlaying) {
    clearInterval(timeInterval);
  }
  timeDisplay.innerHTML = time;
}

// 버튼 상태 변경
function buttonChange(text) {
  button.innerText = text;
  text === "게임시작"
    ? button.classList.remove("loading")
    : button.classList.add("loading");
}
```

> API 단어장 생성시 필요한 사이트

[랜덤 단어장 호출 사이트](https://random-word-api.herokuapp.com/home) : 주소 뒤에 _word?number=호출하고 싶은 단어숫자_ 를 입력하면 랜덤 인덱스로 단어장이 생성된다.

[axios CDN](https://axios-http.com/kr/docs/intro)

## 새로 알게 된 개념

> setInterval()

함수를 주기적으로 실행하게 하는 내장 메서드.

```javascript
// countDown 함수를 1초에 한번씩 실행
// 0.1초 = 100
// 1초 = 1,000
timeInterval = setInterval(countDown, 1000);
```

✨대부분의 브라우저에서 alert / confirm / prompt 등의 명령어는 창이 떠있는 중에도

내부 타이머(setInterval())가 진행이 된다.

> clearInterval()

setInterval() 로 호출한 함수를 중단하게 만든다.

> return 의 활용도

생각보다 해당 함수를 탈출하기 위한 용도로 return 문을 많이 사용함 (_↔ break 는 반복문을 탈출하기 위한 용도_)

> InnerHTML vs InnerText

InnerHTML 은 HTML 내부의 태그 요소까지 적용이 된 상태로 DOM 을 추출한다.

```html
<p id="test1"></p>
<p id="test2"></p>

<script>
  const inHTML = document.getElementById("test1");
  const inText = document.getElementById("test2");
  // innerHTML <strong> 태그까지 적용되어 브라우저에 텍스트 굵기가 강조되어 렌더링된다.
  inHTML.innerHTML =
    "<strong>innerHTML은 html 태그 요소를 모두 인식합니다.</strong>";
  // innerText 는 <strong> 태그를 텍스트로 인식하여 브라우저에 렌더링된다.
  inText.innerText =
    "<strong>innerText는 html 태그를 텍스트로 인식합니다.</strong>";
</script>
```

반면 InnerText 는 HTML 내부의 태그 요소까지 텍스트로 인식하여 브라우저에 렌더링한다.
