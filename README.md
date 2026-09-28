<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>漢方番号・漢字ドリル</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
    }
    body {
      background-color: #f4f6f9;
      color: #333;
      display: flex;
      justify-content: center;
      min-height: 100vh;
      padding: 16px;
    }
    .app-container {
      width: 100%;
      max-width: 480px;
      background: #ffffff;
      border-radius: 20px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.08);
      overflow: hidden;
      display: flex;
      flex-direction: column;
    }
    header {
      background: linear-gradient(135deg, #4f46e5, #7c3aed);
      color: white;
      padding: 24px 20px;
      text-align: center;
    }
    header h1 {
      font-size: 1.5rem;
      font-weight: 700;
      margin-bottom: 6px;
    }
    header p {
      font-size: 0.85rem;
      opacity: 0.9;
    }
    .content {
      padding: 20px;
      flex: 1;
      display: flex;
      flex-direction: column;
    }
    .card {
      background: #fff;
      border: 1px solid #e5e7eb;
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 16px;
    }
    .btn {
      width: 100%;
      padding: 14px;
      font-size: 1rem;
      font-weight: 600;
      border-radius: 12px;
      border: none;
      cursor: pointer;
      transition: all 0.2s ease;
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 12px;
      background: #f8fafc;
      border: 2px solid #e2e8f0;
      color: #1e293b;
    }
    .btn:active {
      transform: scale(0.98);
    }
    .btn-primary {
      background: #4f46e5;
      color: white;
      border: none;
      justify-content: center;
    }
    .btn-option {
      font-size: 1.1rem;
      text-align: left;
      padding: 16px;
      border-radius: 12px;
      background: #ffffff;
      border: 2px solid #e2e8f0;
      color: #1e293b;
      margin-bottom: 12px;
      width: 100%;
      cursor: pointer;
      font-weight: 600;
    }
    .btn-option.correct {
      background: #dcfce7 !important;
      border-color: #22c55e !important;
      color: #15803d !important;
    }
    .btn-option.incorrect {
      background: #fee2e2 !important;
      border-color: #ef4444 !important;
      color: #b91c1c !important;
    }
    .badge {
      background: #e0e7ff;
      color: #3730a3;
      padding: 4px 10px;
      border-radius: 20px;
      font-size: 0.8rem;
      font-weight: 600;
    }
    .number-display {
      text-align: center;
      margin: 20px 0;
    }
    .number-label {
      font-size: 0.9rem;
      color: #64748b;
      margin-bottom: 4px;
    }
    .number-val {
      font-size: 3.5rem;
      font-weight: 800;
      color: #1e293b;
      line-height: 1;
    }
    .progress-bar {
      height: 8px;
      background: #e2e8f0;
      border-radius: 4px;
      overflow: hidden;
      margin-bottom: 20px;
    }
    .progress-fill {
      height: 100%;
      background: #4f46e5;
      width: 0%;
      transition: width 0.3s ease;
    }
    .hidden {
      display: none !important;
    }
    .score-title {
      font-size: 1.2rem;
      font-weight: 700;
      text-align: center;
      margin-bottom: 8px;
    }
    .score-big {
      font-size: 3rem;
      font-weight: 800;
      color: #4f46e5;
      text-align: center;
      margin-bottom: 16px;
    }
  </style>
</head>
<body>

<div class="app-container">
  <header>
    <h1>漢方番号・漢字ドリル</h1>
    <p>番号に対応した漢方名をマスターしよう</p>
  </header>

  <div class="content">
    <!-- 範囲選択画面 -->
    <div id="screen-menu">
      <h2 style="font-size: 1.1rem; margin-bottom: 12px; color: #475569;">学習範囲を選択</h2>
      <button class="btn" onclick="startQuiz(0, 9999)">
        <span>全問 (125問)</span>
        <span class="badge">1〜3023</span>
      </button>
      <button class="btn" onclick="startQuiz(1, 40)">
        <span>1 〜 40 番</span>
        <span class="badge">40問</span>
      </button>
      <button class="btn" onclick="startQuiz(41, 80)">
        <span>41 〜 80 番</span>
        <span class="badge">39問</span>
      </button>
      <button class="btn" onclick="startQuiz(81, 120)">
        <span>81 〜 120 番</span>
        <span class="badge">38問</span>
      </button>
      <button class="btn" onclick="startQuiz(121, 9999)">
        <span>121 番以降</span>
        <span class="badge">18問</span>
      </button>
    </div>

    <!-- クイズ画面 -->
    <div id="screen-quiz" class="hidden">
      <div class="progress-bar">
        <div id="progress" class="progress-fill"></div>
      </div>
      <div style="display:flex; justify-content:space-between; font-size:0.85rem; color:#64748b; margin-bottom:12px;">
        <span id="quiz-count">第 1 問</span>
        <span id="score-count">正解: 0</span>
      </div>

      <div class="card number-display">
        <div class="number-label">漢方処方番号</div>
        <div id="kampo-num" class="number-val">1</div>
      </div>

      <div id="options-container"></div>

      <button id="next-btn" class="btn btn-primary hidden" onclick="nextQuestion()" style="margin-top: 12px;">
        次の問題へ ➔
      </button>
    </div>

    <!-- 結果画面 -->
    <div id="screen-result" class="hidden">
      <div class="card" style="text-align:center; padding: 24px;">
        <div class="score-title">学習終了！</div>
        <div id="final-score" class="score-big">0 / 0</div>
        <p id="accuracy-text" style="color:#64748b; margin-bottom: 20px;">正解率: 0%</p>
        <button class="btn btn-primary" onclick="backToMenu()">メニューに戻る</button>
      </div>
    </div>
  </div>
</div>

<script>
const kampoData = [
  {num: 1, name: "葛根湯"}, {num: 2, name: "葛根湯加川芎辛夷"}, {num: 3, name: "乙字湯"},
  {num: 5, name: "安中散"}, {num: 6, name: "十味敗毒湯"}, {num: 7, name: "八味地黄丸"},
  {num: 8, name: "大柴胡湯"}, {num: 9, name: "小柴胡湯"}, {num: 10, name: "柴胡桂枝湯"},
  {num: 11, name: "柴胡桂枝乾姜湯"}, {num: 12, name: "柴胡加竜骨牡蛎湯"}, {num: 14, name: "半夏瀉心湯"},
  {num: 15, name: "黄連解毒湯"}, {num: 16, name: "半夏厚朴湯"}, {num: 17, name: "五苓散"},
  {num: 18, name: "桂枝加朮附湯"}, {num: 19, name: "小青竜湯"}, {num: 20, name: "防已黄耆湯"},
  {num: 21, name: "小半夏加茯苓湯"}, {num: 22, name: "消風散"}, {num: 23, name: "当帰芍薬散"},
  {num: 24, name: "加味逍遙散"}, {num: 25, name: "桂枝茯苓丸"}, {num: 26, name: "桂枝加竜骨牡蛎湯"},
  {num: 27, name: "麻黄湯"}, {num: 28, name: "越婢加朮湯"}, {num: 29, name: "麦門冬湯"},
  {num: 30, name: "真武湯"}, {num: 31, name: "呉茱萸湯"}, {num: 32, name: "人参湯"},
  {num: 33, name: "大黄牡丹皮湯"}, {num: 34, name: "白虎加人参湯"}, {num: 35, name: "四逆散"},
  {num: 36, name: "木防已湯"}, {num: 37, name: "半夏白朮天麻湯"}, {num: 38, name: "当帰四逆加呉茱萸生姜湯"},
  {num: 39, name: "苓桂朮甘湯"}, {num: 40, name: "猪苓湯"}, {num: 41, name: "補中益気湯"},
  {num: 43, name: "六君子湯"}, {num: 45, name: "桂枝湯"}, {num: 46, name: "七物降下湯"},
  {num: 47, name: "釣藤散"}, {num: 48, name: "十全大補湯"}, {num: 50, name: "荊芥連翹湯"},
  {num: 51, name: "潤腸湯"}, {num: 52, name: "薏苡仁湯"}, {num: 53, name: "疎経活血湯"},
  {num: 54, name: "抑肝散"}, {num: 55, name: "麻杏甘石湯"}, {num: 56, name: "五淋散"},
  {num: 57, name: "温清飲"}, {num: 58, name: "清上防風湯"}, {num: 59, name: "治頭瘡一方"},
  {num: 60, name: "桂枝加芍薬湯"}, {num: 61, name: "桃核承気湯"}, {num: 62, name: "防風通聖散"},
  {num: 63, name: "五積散"}, {num: 64, name: "炙甘草湯"}, {num: 65, name: "帰脾湯"},
  {num: 66, name: "参蘇飲"}, {num: 67, name: "女神散"}, {num: 68, name: "芍薬甘草湯"},
  {num: 69, name: "茯苓飲"}, {num: 70, name: "香蘇散"}, {num: 71, name: "四物湯"},
  {num: 72, name: "甘麦大棗湯"}, {num: 73, name: "柴陥湯"}, {num: 74, name: "調胃承気湯"},
  {num: 75, name: "四君子湯"}, {num: 76, name: "竜胆瀉肝湯"}, {num: 77, name: "芎帰膠艾湯"},
  {num: 78, name: "麻杏薏甘湯"}, {num: 79, name: "平胃散"}, {num: 80, name: "柴胡清肝湯"},
  {num: 81, name: "二陳湯"}, {num: 82, name: "桂枝人参湯"}, {num: 83, name: "抑肝散加陳皮半夏"},
  {num: 84, name: "大黄甘草湯"}, {num: 85, name: "神秘湯"}, {num: 86, name: "当帰飲子"},
  {num: 87, name: "六味丸"}, {num: 88, name: "二朮湯"}, {num: 89, name: "治打撲一方"},
  {num: 90, name: "清肺湯"}, {num: 91, name: "竹筎温胆湯"}, {num: 92, name: "滋陰至宝湯"},
  {num: 93, name: "滋陰降火湯"}, {num: 95, name: "五虎湯"}, {num: 96, name: "柴朴湯"},
  {num: 97, name: "大防風湯"}, {num: 98, name: "黄耆建中湯"}, {num: 99, name: "小建中湯"},
  {num: 100, name: "大建中湯"}, {num: 101, name: "升麻葛根湯"}, {num: 102, name: "当帰湯"},
  {num: 103, name: "酸棗仁湯"}, {num: 104, name: "辛夷清肺湯"}, {num: 105, name: "通導散"},
  {num: 106, name: "温経湯"}, {num: 107, name: "牛車腎気丸"}, {num: 108, name: "人参養栄湯"},
  {num: 109, name: "小柴胡湯加桔梗石膏"}, {num: 110, name: "立効散"}, {num: 111, name: "清心蓮子飲"},
  {num: 112, name: "猪苓湯合四物湯"}, {num: 113, name: "三黄瀉心湯"}, {num: 114, name: "柴苓湯"},
  {num: 115, name: "胃苓湯"}, {num: 116, name: "茯苓飲合半夏厚朴湯"}, {num: 117, name: "茵蔯五苓散"},
  {num: 118, name: "苓姜朮甘湯"}, {num: 119, name: "苓甘姜味辛夏仁湯"}, {num: 120, name: "黄連湯"},
  {num: 121, name: "三物黄芩湯"}, {num: 122, name: "排膿散及湯"}, {num: 123, name: "当帰建中湯"},
  {num: 124, name: "川芎茶調散"}, {num: 125, name: "桂枝茯苓丸加薏苡仁"}, {num: 126, name: "麻子仁丸"},
  {num: 127, name: "麻黄附子細辛湯"}, {num: 128, name: "啓脾湯"}, {num: 133, name: "大承気湯"},
  {num: 134, name: "桂枝加芍薬大黄湯"}, {num: 135, name: "茵蔯蒿湯"}, {num: 136, name: "清暑益気湯"},
  {num: 137, name: "加味帰脾湯"}, {num: 138, name: "桔梗湯"}, {num: 501, name: "紫雲膏"},
  {num: 3020, name: "コウジン末"}, {num: 3023, name: "ブシ末"}
];

let currentQuestions = [];
let currentIndex = 0;
let score = 0;
let answered = false;

function startQuiz(min, max) {
  currentQuestions = kampoData.filter(q => q.num >= min && q.num <= max);
  currentIndex = 0;
  score = 0;
  
  document.getElementById('screen-menu').classList.add('hidden');
  document.getElementById('screen-quiz').classList.remove('hidden');
  document.getElementById('screen-result').classList.add('hidden');
  
  showQuestion();
}

function showQuestion() {
  answered = false;
  const q = currentQuestions[currentIndex];
  
  document.getElementById('kampo-num').textContent = q.num;
  document.getElementById('quiz-count').textContent = `第 ${currentIndex + 1} / ${currentQuestions.length} 問`;
  document.getElementById('score-count').textContent = `正解: ${score}`;
  document.getElementById('progress').style.width = `${((currentIndex) / currentQuestions.length) * 100}%`;
  document.getElementById('next-btn').classList.add('hidden');

  // 選択肢作成 (正解1つ + ダミー3つ)
  let choices = [q.name];
  while (choices.length < 4) {
    let randIndex = Math.floor(Math.random() * kampoData.length);
    let randName = kampoData[randIndex].name;
    if (!choices.includes(randName)) {
      choices.push(randName);
    }
  }
  
  // シャッフル
  choices.sort(() => Math.random() - 0.5);

  const container = document.getElementById('options-container');
  container.innerHTML = '';

  choices.forEach(choice => {
    const btn = document.createElement('button');
    btn.className = 'btn-option';
    btn.textContent = choice;
    btn.onclick = () => checkAnswer(btn, choice, q.name);
    container.appendChild(btn);
  });
}

function checkAnswer(selectedBtn, selectedText, correctText) {
  if (answered) return;
  answered = true;

  const buttons = document.querySelectorAll('.btn-option');
  buttons.forEach(btn => {
    btn.style.cursor = 'default';
    if (btn.textContent === correctText) {
      btn.classList.add('correct');
    }
  });

  if (selectedText === correctText) {
    score++;
    document.getElementById('score-count').textContent = `正解: ${score}`;
  } else {
    selectedBtn.classList.add('incorrect');
  }

  document.getElementById('next-btn').classList.remove('hidden');
}

function nextQuestion() {
  currentIndex++;
  if (currentIndex < currentQuestions.length) {
    showQuestion();
  } else {
    showResult();
  }
}

function showResult() {
  document.getElementById('screen-quiz').classList.add('hidden');
  document.getElementById('screen-result').classList.remove('hidden');
  
  document.getElementById('final-score').textContent = `${score} / ${currentQuestions.length}`;
  const pct = Math.round((score / currentQuestions.length) * 100);
  document.getElementById('accuracy-text').textContent = `正解率: ${pct}%`;
}

function backToMenu() {
  document.getElementById('screen-result').classList.add('hidden');
  document.getElementById('screen-menu').classList.remove('hidden');
}
</script>

</body>
</html>
