
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>똥 먹이기 시뮬레이터</title>
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

    .mode-buttons {
      display: flex;
      gap: 10px;
      margin-bottom: 18px;
    }

    .mode-btn {
      flex: 1;
      padding: 10px;
      border-radius: 14px;
      background: #fff;
      font-weight: bold;
      border: 2px solid #ffd08a;
    }

    .mode-btn.active {
      background: #ffb347;
      color: white;
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
      min-height: 60px;
      color: #5c3d1e;
      font-weight: bold;
      font-size: 1.1rem;
      background: rgba(255,255,255,0.7);
      border-radius: 18px;
      padding: 14px;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
    }

      10% {
        opacity: 1;
        transform: translateX(-50%) translateY(0px);
      }

      80% {
        opacity: 1;
      }

      100% {
        opacity: 0;
        transform: translateX(-50%) translateY(-20px);
      }
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

    <div class="mode-buttons">
      <button class="mode-btn active" id="randomBtn" onclick="setMode('random')">🎲 랜덤</button>
      <button class="mode-btn" id="loveBtn" onclick="setMode('love')">😍 좋아함</button>
      <button class="mode-btn" id="hateBtn" onclick="setMode('hate')">🤮 싫어함</button>
    </div>

    <button class="feed-btn" onclick="feedPoop()">
      💩 먹이기
    </button>

    <div class="message" id="message"></div>
  </div>

  <script>
    let count = 0;
    let characterName = "햄찌";

    let currentMode = 'random';

    const loveStarts = [
      "행복하게",
      "미친 듯이",
      "감동하며",
      "눈을 반짝이며",
      "울먹이며",
      "진지하게",
      "광기 어린 표정으로",
      "조용히",
      "흥분해서",
      "텐션 높게",
      "춤추면서",
      "철학자처럼",
      "배고픈 맹수처럼",
      "천천히",
      "폭주하듯",
      "감사한 표정으로",
      "의미심장하게",
      "기절 직전으로",
      "무아지경으로",
      "엄청난 집중력으로",
      "눈물을 흘리며",
      "행복에 겨워",
      "심장이 뛰는 듯",
      "입꼬리가 올라간 채",
      "온몸으로 기뻐하며",
      "몽롱한 표정으로",
      "세상을 다 가진 듯",
      "흥얼거리며",
      "황홀한 얼굴로",
      "반짝이는 눈으로",
      "숨을 헐떡이며",
      "즐거움에 취해",
      "들뜬 목소리로",
      "감탄하며",
      "웃음을 참지 못하며",
      "벅찬 감정으로",
      "심취한 채",
      "만족감에 빠져",
      "환희에 찬 얼굴로",
      "꿈꾸는 듯한 표정으로"
    ];

    const loveActions = [
      "먹어치웠다",
      "흡입했다",
      "행복해했다",
      "눈물을 흘렸다",
      "미소지었다",
      "만족했다",
      "춤을 췄다",
      "세상을 깨달았다",
      "진화했다",
      "박수를 쳤다",
      "감동받았다",
      "하늘을 바라봤다",
      "웃음을 터뜨렸다",
      "행복사 직전이 되었다",
      "브금을 틀고 싶어했다",
      "사랑에 빠졌다",
      "광기에 휩싸였다",
      "영혼까지 배불러했다",
      "인생 최고의 식사라고 느꼈다",
      "입안의 우주를 느꼈다",
      "세상을 용서했다",
      "갑자기 철이 들었다",
      "자신감을 얻었다",
      "행복의 극치를 맛봤다",
      "텐션이 폭발했다",
      "심장이 두근거렸다",
      "인생의 의미를 깨달았다",
      "자기 자신을 사랑하게 되었다",
      "웃다가 쓰러질 뻔했다",
      "갑자기 노래를 부르기 시작했다",
      "황홀경에 빠졌다",
      "감사를 느꼈다",
      "희망이 생겼다",
      "입안의 축제를 즐겼다",
      "마음을 열게 되었다",
      "신세계를 경험했다",
      "기쁨으로 떨었다",
      "행복한 비명을 질렀다",
      "온 세상이 아름다워 보였다",
      "우주와 하나가 된 기분을 느꼈다"
    ];

    const hateStarts = [
      "절망하며",
      "울먹이며",
      "도망치려 하며",
      "충격받은 채",
      "현타 온 얼굴로",
      "눈빛이 죽은 채",
      "억울한 표정으로",
      "배신감에 찬 얼굴로",
      "삶을 포기한 듯",
      "몸을 떨며",
      "무표정하게",
      "영혼이 빠져나간 채",
      "세상을 원망하며",
      "흐느끼며",
      "눈물을 참으며",
      "기억을 잃은 듯",
      "천천히 무너지며",
      "혼란스러워하며",
      "작게 한숨 쉬며",
      "현실을 부정하며",
      "정신이 나간 표정으로",
      "멍하니",
      "공허한 눈빛으로",
      "비틀거리며",
      "자아를 잃은 듯",
      "차갑게 식은 표정으로",
      "세상을 포기한 채",
      "숨을 떨며",
      "창백해진 얼굴로",
      "비명을 삼키며",
      "억지로 웃으며",
      "눈을 감은 채",
      "손을 떨며",
      "패배한 사람처럼",
      "울컥한 채",
      "현실도피하듯",
      "숨이 막힌 듯",
      "분노를 참으며",
      "불안에 떨며",
      "혼이 빠진 얼굴로"
    ];

    const hateActions = [
      "먹어버렸다",
      "후회했다",
      "울부짖었다",
      "벽을 바라봤다",
      "정신이 붕괴됐다",
      "기절할 것 같아졌다",
      "세상을 의심하기 시작했다",
      "인생을 회고했다",
      "현실을 받아들이지 못했다",
      "영혼이 흔들렸다",
      "한동안 말을 잃었다",
      "눈물을 흘렸다",
      "제작자를 원망했다",
      "구석으로 걸어갔다",
      "멍하니 서 있었다",
      "존재의 의미를 고민했다",
      "삶의 위기를 느꼈다",
      "충격에 빠졌다",
      "세상이 무너지는 기분을 느꼈다",
      "아무 생각도 하지 못했다",
      "자신을 잃어버렸다",
      "갑자기 철학자가 되었다",
      "혼자 중얼거리기 시작했다",
      "의식을 잃을 뻔했다",
      "자신의 선택을 후회했다",
      "허공만 바라봤다",
      "조용히 무너졌다",
      "감정을 잃어버렸다",
      "희망을 포기했다",
      "현실을 외면했다",
      "갑자기 늙어버린 것 같아졌다",
      "인생의 쓴맛을 느꼈다",
      "눈빛이 탁해졌다",
      "자기 자신과 싸우기 시작했다",
      "혼란에 빠졌다",
      "세상을 증오하게 되었다",
      "갑자기 아무 말도 하지 않았다",
      "눈앞이 흐려졌다",
      "모든 걸 내려놓고 싶어졌다",
      "기묘한 슬픔에 잠겼다"
    ];

    const extraEnds = [
      "💩",
      "...",
      "😭",
      "✨",
      "그리고 조용해졌다",
      "갑자기 철이 든 것 같다",
      "뭔가를 깨달은 표정이다",
      "눈빛이 심상치 않다",
      "오늘 하루를 잊지 못할 것 같다",
      "갑자기 하늘을 올려다본다",
      "잠깐 시간이 멈춘 듯하다",
      "세상이 느리게 움직이는 기분이다",
      "영혼이 탈주하려 한다",
      "캐릭터가 제작자를 노려본다",
      "눈동자가 흔들리고 있다",
      "갑자기 침묵했다",
      "공기가 무거워졌다",
      "브금이 슬퍼질 것 같다",
      "오늘 밤 잠 못 잘 것 같다",
      "어딘가 망가진 것 같다"
    ];

    const loveMessages = [];
    const hateMessages = [];

    for(let i = 0; i < loveStarts.length; i++) {
      for(let j = 0; j < loveActions.length; j++) {
        for(let k = 0; k < extraEnds.length; k++) {
          loveMessages.push(`${loveStarts[i]} ${loveActions[j]} ${extraEnds[k]}`);
        }
      }
    }

    for(let i = 0; i < hateStarts.length; i++) {
      for(let j = 0; j < hateActions.length; j++) {
        for(let k = 0; k < extraEnds.length; k++) {
          hateMessages.push(`${hateStarts[i]} ${hateActions[j]} ${extraEnds[k]}`);
        }
      }
    }

    const hateMessages = [
      "으악!! 싫어한다 😭",
      "도망가려 한다",
      "캐릭터가 충격받았다",
      "눈물을 글썽인다",
      "'이건 아니잖아...' 라는 표정이다",
      "입을 꾹 다물었다",
      "먹기 싫어서 버둥거린다",
      "한동안 말을 잃었다",
      "갑자기 세상이 미워진 듯하다",
      "캐릭터가 배신감을 느낀다",
      "조용히 절망하고 있다",
      "먹고 나서 벽을 바라본다",
      "표정이 썩어버렸다",
      "한숨을 깊게 쉰다",
      "먹자마자 후회했다",
      "삶의 의미를 고민하기 시작했다",
      "현실을 부정하는 중이다",
      "캐릭터가 울부짖는다",
      "갑자기 쓰러질 것 같다",
      "'왜 나한테 이런 짓을...'",
      "먹고 난 뒤 영혼이 빠져나갔다"
    ];

    const randomMessages = [...loveMessages, ...hateMessages];

    function setMode(mode) {
      currentMode = mode;

      document.querySelectorAll('.mode-btn').forEach(btn => {
        btn.classList.remove('active');
      });

      if(mode === 'random') {
        document.getElementById('randomBtn').classList.add('active');
        showBubble('🎲 랜덤 모드로 변경!');
      }

      if(mode === 'love') {
        document.getElementById('loveBtn').classList.add('active');
        showBubble('😍 좋아하는 반응만 출력!');
      }

      if(mode === 'hate') {
        document.getElementById('hateBtn').classList.add('active');
        showBubble('🤮 싫어하는 반응만 출력!');
      }
    }

    function setCharacterEmoji() {
      const emoji = document.getElementById('emojiInput').value.trim();

      if(emoji !== '') {
        document.getElementById('character').textContent = emoji;
        showBubble(`이모지가 ${emoji}(으)로 변경되었습니다!`);
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

        showBubble(`${characterName}(으)로 변경됨! 먹은 개수 초기화!`);
      }
    }

    function feedPoop() {
      count++;
      document.getElementById('count').textContent = count;

      let selectedArray = randomMessages;

      if(currentMode === 'love') {
        selectedArray = loveMessages;
      }

      if(currentMode === 'hate') {
        selectedArray = hateMessages;
      }

      const message = selectedArray[Math.floor(Math.random() * selectedArray.length)];
      showBubble(`${characterName}: ${message}`);

      const character = document.getElementById('character');
      character.classList.add('eating');

      setTimeout(() => {
        character.classList.remove('eating');
      }, 200);

      createPoopEffect();
    }

    function showBubble(text) {
      const area = document.getElementById('messageArea');

      const bubble = document.createElement('div');
      bubble.className = 'bubble';
      bubble.textContent = text;

      bubble.style.top = Math.random() * 60 + 'px';

      area.appendChild(bubble);

      setTimeout(() => {
        bubble.remove();
      }, 2000);
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
