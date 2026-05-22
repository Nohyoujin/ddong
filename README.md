# 💩 캐릭터 똥 먹이기 사이트 (HTML)

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>똥 먹이기 </title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: "Pretendard", sans-serif;
    }

    body {
      background: linear-gradient(180deg, #fff7e8, #ffd7a3);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
    }

    .container {
      width: 420px;
      background: rgba(255,255,255,0.8);
      backdrop-filter: blur(10px);
      border-radius: 30px;
      padding: 30px;
      text-align: center;
      box-shadow: 0 10px 40px rgba(0,0,0,0.15);
    }

    h1 {
      font-size: 2rem;
      margin-bottom: 20px;
    }

    .name-box {
      display: flex;
      gap: 10px;
      margin-bottom: 25px;
    }

    input {
      flex: 1;
      padding: 12px;
      border-radius: 14px;
      border: none;
      background: #fff;
      font-size: 1rem;
    }

    button {
      border: none;
      cursor: pointer;
      transition: 0.2s;
    }

    .set-btn {
      padding: 12px 16px;
      border-radius: 14px;
      background: #ff9f43;
      color: white;
      font-weight: bold;
    }

    .set-btn:hover {
      transform: scale(1.05);
    }

    .character {
      font-size: 8rem;
      margin: 10px 0;
      transition: transform 0.2s;
      user-select: none;
    }

    .character.eating {
      transform: scale(1.15) rotate(-5deg);
    }

    .character-name {
      font-size: 1.5rem;
      font-weight: bold;
      margin-bottom: 10px;
    }

    .count {
      font-size: 1.2rem;
      margin-bottom: 25px;
      color: #5c3d1e;
    }

    .feed-btn {
      width: 100%;
      padding: 18px;
      border-radius: 20px;
      background: #6b3e12;
      color: white;
      font-size: 1.2rem;
      font-weight: bold;
      box-shadow: 0 8px 0 #4d2c09;
    }

    .feed-btn:active {
      transform: translateY(5px);
      box-shadow: 0 3px 0 #4d2c09;
    }

    .message {
      margin-top: 20px;
      min-height: 24px;
      color: #7a4a1b;
      font-weight: bold;
    }

    .poop {
      position: absolute;
      font-size: 2rem;
      animation: floatUp 1.2s forwards;
      pointer-events: none;
    }

    @keyframes floatUp {
      0% {
        opacity: 1;
        transform: translateY(0) scale(1);
      }
      100% {
        opacity: 0;
        transform: translateY(-120px) scale(1.8);
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>💩 먹이기 시뮬레이터</h1>

    <div class="name-box">
      <input type="text" id="nameInput" placeholder="캐릭터 이름 입력" />
      <button class="set-btn" onclick="setCharacterName()">설정</button>
    </div>

    <div class="name-box">
      <input type="text" id="emojiInput" placeholder="캐릭터 이모지 입력 (예: 🐱)" />
      <button class="set-btn" onclick="setCharacterEmoji()">이모지 변경</button>
    </div>

    <div class="character" id="character">🐹</div>
    <div class="character-name" id="characterName">햄찌</div>
    <div class="count">먹은 개수: <span id="count">0</span></div>

    <button class="feed-btn" onclick="feedPoop()">
      💩 먹이기
    </button>

    <div class="message" id="message"></div>
  </div>

  <script>
    let count = 0;
    let characterName = "햄찌";

    const messages = [
      "야르~",
      "오늘의 먹방은 바로바로~ 똥! 잘먹겠습니다~",
      "뚕 없냐 뚕",
      "저에게 오늘도 일용할 양식을 주셔서 감사드립니다",
      "똥이야 말로 진정한 미슐랭 아닐까?",
      "맛있게 먹었습니다...",
      "우걱우걱 💩",
      "행복해 보입니다",
      "더 달라는 눈빛이다",
      "오늘도 건강한(?) 식사",
      "만족해합니다",
      "엄청난 속도로 먹어치웠다",
      "눈을 반짝이며 먹었다 ✨",
      "살짝 울컥한 표정이다",
      "먹고 나서 춤을 춘다",
      "배가 빵빵해졌다",
      "어째서인지 행복해보인다",
      "한입 더 달라고 조른다",
      "말없이 흡입했다",
      "굉장한 집중력으로 먹었다",
      "먹는 속도가 심상치 않다",
      "오늘의 미식가가 되었다",
      "기묘한 만족감을 느끼는 중",
      "고개를 끄덕였다",
      "뭔가 깨달음을 얻은 표정이다",
      "먹고 난 뒤 허공을 바라본다",
      "눈물이 한 방울 흐른다",
      "이게 진정한 식사인가...",
      "먹고 나서 갑자기 강해졌다",
      "입가에 미소가 번졌다",
      "한층 진화한 느낌이다",
      "행복 수치가 상승했다",
      "갑자기 텐션이 올라갔다",
      "만족스럽게 트림했다",
      "먹자마자 기절하듯 행복해했다",
      "으악!! 싫어한다 😭",
      "도망가려 한다",
      "충격받았다",
      "눈물을 글썽인다",
      "'이건 아니잖아...' 라는 표정이다",
      "입을 꾹 다물었다",
      "먹기 싫어서 버둥거린다",
      "한동안 말을 잃었다",
      "갑자기 세상이 미워진 듯하다",
      "배신감을 느낀다",
      "조용히 절망하고 있다",
      "먹고 나서 벽을 바라본다",
      "표정이 썩어버렸다",
      "한숨을 깊게 쉰다",
      "먹자마자 후회했다",
      "삶의 의미를 고민하기 시작했다",
      "현실을 부정하는 중이다",
      "울부짖는다",
      "갑자기 쓰러질 것 같다",
      "'왜 나한테 이런 짓을...'",
      "먹고 난 뒤 영혼이 빠져나갔다",
      "맛의 신세계를 봤다",
      "공중제비를 돌았다",
      "갑자기 자신감이 넘친다",
      "입에서 무지개가 나올 것 같다",
      "'한 번 더 가능?'",
      "어쩐지 눈빛이 광기다",
      "방금 행복의 극치를 느꼈다",
      "박수를 친다",
      "오늘 하루 최고의 식사였다",
      "브금을 틀고 싶어진다",
      "배가 따뜻해졌다",
      "'나쁘지 않은데?'",
      "입안에서 우주가 펼쳐졌다",
      "기분이 몽글몽글해졌다",
      "눈에 별이 생겼다 ✨",
      "행복사 직전이다",
      "갑자기 춤 배틀을 신청했다",
      "'또 먹고 싶어...'",
      "먹자마자 사랑에 빠졌다",
      "뇌가 녹아내리는 기분이다",
      "만족의 미소를 짓는다",
      "'이것이 인생...'",
      "갑자기 자신을 돌아본다",
      "잠시 현실을 잊었다",
      "감동받았다",
      "눈가가 촉촉해졌다",
      "말없이 엄지를 올린다",
      "먹고 나서 하늘을 올려다본다",
      "갑자기 철이 든 것 같다",
      "영혼까지 배부르다",
      "뭔가 위험한 맛이었다",
      "흔들리고 있다",
      "속이 울렁거리기 시작했다",
      "방금 생명의 위협을 느꼈다",
      "살려달라고 한다",
      "갑자기 의심이 많아졌다",
      "눈빛에서 빛이 사라졌다",
      "한동안 멍하니 있었다",
      "갑자기 기억을 잃은 듯하다",
      "'엄마...'",
      "먹자마자 현실을 깨달았다",
      "작게 흐느낀다",
      "눈동자가 흔들린다",
      "세상이 천천히 무너지는 중이다",
      "갑자기 인생 회고를 시작했다",
      "먹고 난 뒤 조용해졌다",
      "구석으로 걸어간다",
      "갑자기 현타가 왔다",
      "'이건 꿈일 거야...'",
      "잠깐 영혼이 분리됐다",
      "제작자를 원망한다",
      "먹고 나서 3초간 정지했다",
      "몸이 떨리고 있다",
      "억울해 보인다",
      "눈빛이 죽어있다",
      "갑자기 아무 생각도 안 난다"
    ];

    function setCharacterEmoji() {
      const emoji = document.getElementById('emojiInput').value.trim();

      if(emoji !== '') {
        document.getElementById('character').textContent = emoji;
        document.getElementById('message').textContent =
          `이모지가 ${emoji}(으)로 변경되었습니다!`;
      }
    }

    function setCharacterName() {
      const input = document.getElementById('nameInput');
      const newName = input.value.trim();

      if(newName !== '') {
        characterName = newName;
        document.getElementById('characterName').textContent = characterName;

        count = 0;
        document.getElementById('count').textContent = count;

        document.getElementById('message').textContent =
          `${characterName}(으)로 변경됨! 먹은 개수 초기화!`;
      }
    }

    function feedPoop() {
      count++;
      document.getElementById('count').textContent = count;

      const message = messages[Math.floor(Math.random() * messages.length)];
      document.getElementById('message').textContent =
        `${message}`;

      const character = document.getElementById('character');
      character.classList.add('eating');

      setTimeout(() => {
        character.classList.remove('eating');
      }, 200);

      createPoopEffect();
    }

    function createPoopEffect() {
      const poop = document.createElement('div');
      poop.className = 'poop';
      poop.textContent = '💩';

      poop.style.left = Math.random() * window.innerWidth + 'px';
      poop.style.top = (window.innerHeight - 100) + 'px';

      document.body.appendChild(poop);

      setTimeout(() => {
        poop.remove();
      }, 1200);
    }
  </script>
</body>
</html>
```

이걸 `index.html`로 저장해서 실행하면 돼!
