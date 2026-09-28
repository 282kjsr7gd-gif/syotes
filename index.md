#   
  
<!DOCTYPE html>  
<html lang="ja">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>漢方番号・漢字ドリル</title>  
<!-- Tailwind CSS CDN for styling -->  
<script src="https://cdn.tailwindcss.com"></script>  
<style>  
/* Safari on iOS viewport height fix & subtle animations */  
body {  
min-height: 100vh;  
min-height: -webkit-fill-available;  
-webkit-tap-highlight-color: transparent;  
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;  
}  
.fade-in {  
animation: fadeIn 0.25s ease-out;  
}  
@keyframes fadeIn {  
from { opacity: 0; transform: translateY(4px); }  
to { opacity: 1; transform: translateY(0); }  
}  
</style>  
</head>  
<body class="bg-slate-100 text-slate-800 flex flex-col items-center justify-start min-h-screen p-4 sm:p-6">  
<div class="w-full max-w-xl bg-white rounded-2xl shadow-lg border border-slate-200 overflow-hidden my-auto fade-in">  
<!-- Header -->  
<header class="bg-indigo-600 text-white p-5 text-center shadow-md">  
<h1 class="text-2xl font-bold tracking-wide">漢方番号・漢字ドリル</h1>  
<p class="text-xs text-indigo-200 mt-1">番号に対応した漢方名をマスターしよう</p>  
</header>  
<!-- Main Content Area -->  
<main class="p-6">  
<!-- Range Selection Screen -->  
<div id="setup-screen" class="space-y-6">  
<div>  
<h2 class="text-lg font-semibold text-slate-700 mb-2">学習範囲を選択</h2>  
<p class="text-sm text-slate-500 mb-4">番号の若い順に出題されます。</p>  
<div class="grid grid-cols-1 sm:grid-cols-2 gap-3" id="range-buttons">  
<button onclick="startQuiz('all')" class="w-full py-3 px-4 bg-indigo-50 hover:bg-indigo-100 text-indigo-700 font-medium rounded-xl border border-indigo-200 transition text-left flex justify-between items-center">  
<span>全問 (125問)</span>  
<span class="text-xs bg-indigo-200 text-indigo-800 px-2 py-1 rounded-full">1～3023</span>  
</button>  
<button onclick="startQuiz('1-40')" class="w-full py-3 px-4 bg-slate-50 hover:bg-slate-100 text-slate-700 font-medium rounded-xl border border-slate-200 transition text-left flex justify-between items-center">  
<span>1 ～ 40 番</span>  
<span class="text-xs bg-slate-200 text-slate-600 px-2 py-1 rounded-full">40問</span>  
</button>  
<button onclick="startQuiz('41-80')" class="w-full py-3 px-4 bg-slate-50 hover:bg-slate-100 text-slate-700 font-medium rounded-xl border border-slate-200 transition text-left flex justify-between items-center">  
<span>41 ～ 80 番</span>  
<span class="text-xs bg-slate-200 text-slate-600 px-2 py-1 rounded-full">39問</span>  
</button>  
<button onclick="startQuiz('81-120')" class="w-full py-3 px-4 bg-slate-50 hover:bg-slate-100 text-slate-700 font-medium rounded-xl border border-slate-200 transition text-left flex justify-between items-center">  
<span>81 ～ 120 番</span>  
<span class="text-xs bg-slate-200 text-slate-600 px-2 py-1 rounded-full">38問</span>  
</button>  
<button onclick="startQuiz('[121-3023](tel:121-3023)')" class="w-full py-3 px-4 bg-slate-50 hover:bg-slate-100 text-slate-700 font-medium rounded-xl border border-slate-200 transition text-left flex justify-between items-center">  
<span>121 番以降</span>  
<span class="text-xs bg-slate-200 text-slate-600 px-2 py-1 rounded-full">18問</span>  
</button>  
</div>  
</div>  
</div>  
<!-- Quiz Screen -->  
<div id="quiz-screen" class="hidden space-y-6">  
<!-- Progress Header -->  
<div class="flex justify-between items-center text-sm font-medium text-slate-500">  
<span id="progress-text">問題 1 / 125</span>  
<span id="score-text" class="text-indigo-600">正解: 0</span>  
</div>  
<!-- Progress Bar -->  
<div class="w-full bg-slate-100 rounded-full h-2.5 overflow-hidden">  
<div id="progress-bar" class="bg-indigo-600 h-2.5 rounded-full transition-all duration-300" style="width: 0%"></div>  
</div>  
<!-- Card Display -->  
<div class="bg-gradient-to-br from-indigo-50 to-slate-50 rounded-2xl p-6 border border-indigo-100 text-center shadow-inner relative">  
<div class="text-xs font-bold uppercase tracking-wider text-indigo-500 mb-1">漢方製剤番号</div>  
<div id="question-number" class="text-5xl font-extrabold text-indigo-900 my-2">No. 1</div>  
<div id="result-feedback" class="h-6 text-sm font-bold transition-all duration-200"></div>  
</div>  
<!-- 4 Choices -->  
<div id="options-container" class="grid grid-cols-1 gap-3">  
<!-- Buttons inserted via JS -->  
</div>  
<!-- Next Button -->  
<button id="next-btn" onclick="nextQuestion()" class="hidden w-full py-3.5 bg-indigo-600 hover:bg-indigo-700 text-white font-bold rounded-xl shadow-md transition active:scale-98">  
次の問題へ  
</button>  
</div>  
<!-- Result Screen -->  
<div id="result-screen" class="hidden text-center space-y-6">  
<div class="p-6 bg-slate-50 rounded-2xl border border-slate-200">  
<h2 class="text-xl font-bold text-slate-800 mb-2">ドリル終了！</h2>  
<p class="text-sm text-slate-500 mb-4">お疲れ様でした。</p>  
<div class="text-4xl font-extrabold text-indigo-600 mb-2" id="final-score">0 / 0</div>  
<div class="text-sm font-medium text-slate-600" id="accuracy-rate">正解率: 0%</div>  
</div>  
<div class="space-y-3">  
<button id="review-btn" onclick="startReview()" class="hidden w-full py-3.5 bg-amber-500 hover:bg-amber-600 text-white font-bold rounded-xl shadow-md transition">  
間違えた問題だけ復習する  
</button>  
<button onclick="showSetup()" class="w-full py-3.5 bg-slate-200 hover:bg-slate-300 text-slate-700 font-bold rounded-xl transition">  
トップメニューに戻る  
</button>  
</div>  
</div>  
</main>  
</div>  
<!-- Application Logic -->  
<script>  
const KAMPO_DATA = [  
{ id: 1, name: "葛根湯" },  
{ id: 2, name: "葛根湯加川芎辛夷" },  
{ id: 3, name: "乙字湯" },  
{ id: 5, name: "安中散" },  
{ id: 6, name: "十味敗毒湯" },  
{ id: 7, name: "八味地黄丸" },  
{ id: 8, name: "大柴胡湯" },  
{ id: 9, name: "小柴胡湯" },  
{ id: 10, name: "柴胡桂枝湯" },  
{ id: 11, name: "柴胡桂枝乾姜湯" },  
{ id: 12, name: "柴胡加竜骨牡蛎湯" },  
{ id: 14, name: "半夏瀉心湯" },  
{ id: 15, name: "黄連解毒湯" },  
{ id: 16, name: "半夏厚朴湯" },  
{ id: 17, name: "五苓散" },  
{ id: 18, name: "桂枝加朮附湯" },  
{ id: 19, name: "小青竜湯" },  
{ id: 20, name: "防已黄耆湯" },  
{ id: 21, name: "小半夏加茯苓湯" },  
{ id: 22, name: "消風散" },  
{ id: 23, name: "当帰芍薬散" },  
{ id: 24, name: "加味逍遙散" },  
{ id: 25, name: "桂枝茯苓丸" },  
{ id: 26, name: "桂枝加竜骨牡蛎湯" },  
{ id: 27, name: "麻黄湯" },  
{ id: 28, name: "越婢加朮湯" },  
{ id: 29, name: "麦門冬湯" },  
{ id: 30, name: "真武湯" },  
{ id: 31, name: "呉茱萸湯" },  
{ id: 32, name: "人参湯" },  
{ id: 33, name: "大黄牡丹皮湯" },  
{ id: 34, name: "白虎加人参湯" },  
{ id: 35, name: "四逆散" },  
{ id: 36, name: "木防已湯" },  
{ id: 37, name: "半夏白朮天麻湯" },  
{ id: 38, name: "当帰四逆加呉茱萸生姜湯" },  
{ id: 39, name: "苓桂朮甘湯" },  
{ id: 40, name: "猪苓湯" },  
{ id: 41, name: "補中益気湯" },  
{ id: 43, name: "六君子湯" },  
{ id: 45, name: "桂枝湯" },  
{ id: 46, name: "七物降下湯" },  
{ id: 47, name: "釣藤散" },  
{ id: 48, name: "十全大補湯" },  
{ id: 50, name: "荊芥連翹湯" },  
{ id: 51, name: "潤腸湯" },  
{ id: 52, name: "薏苡仁湯" },  
{ id: 53, name: "疎経活血湯" },  
{ id: 54, name: "抑肝散" },  
{ id: 55, name: "麻杏甘石湯" },  
{ id: 56, name: "五淋散" },  
{ id: 57, name: "温清飲" },  
{ id: 58, name: "清上防風湯" },  
{ id: 59, name: "治頭瘡一方" },  
{ id: 60, name: "桂枝加芍薬湯" },  
{ id: 61, name: "桃核承気湯" },  
{ id: 62, name: "防風通聖散" },  
{ id: 63, name: "五積散" },  
{ id: 64, name: "炙甘草湯" },  
{ id: 65, name: "帰脾湯" },  
{ id: 66, name: "参蘇飲" },  
{ id: 67, name: "女神散" },  
{ id: 68, name: "芍薬甘草湯" },  
{ id: 69, name: "茯苓飲" },  
{ id: 70, name: "香蘇散" },  
{ id: 71, name: "四物湯" },  
{ id: 72, name: "甘麦大棗湯" },  
{ id: 73, name: "柴陥湯" },  
{ id: 74, name: "調胃承気湯" },  
{ id: 75, name: "四君子湯" },  
{ id: 76, name: "竜胆瀉肝湯" },  
{ id: 77, name: "芎帰膠艾湯" },  
{ id: 78, name: "麻杏薏甘湯" },  
{ id: 79, name: "平胃散" },  
{ id: 80, name: "柴胡清肝湯" },  
{ id: 81, name: "二陳湯" },  
{ id: 82, name: "桂枝人参湯" },  
{ id: 83, name: "抑肝散加陳皮半夏" },  
{ id: 84, name: "大黄甘草湯" },  
{ id: 85, name: "神秘湯" },  
{ id: 86, name: "当帰飲子" },  
{ id: 87, name: "六味丸" },  
{ id: 88, name: "二朮湯" },  
{ id: 89, name: "治打撲一方" },  
{ id: 90, name: "清肺湯" },  
{ id: 91, name: "竹筎温胆湯" },  
{ id: 92, name: "滋陰至宝湯" },  
{ id: 93, name: "滋陰降火湯" },  
{ id: 95, name: "五虎湯" },  
{ id: 96, name: "柴朴湯" },  
{ id: 97, name: "大防風湯" },  
{ id: 98, name: "黄耆建中湯" },  
{ id: 99, name: "小建中湯" },  
{ id: 100, name: "大建中湯" },  
{ id: 101, name: "升麻葛根湯" },  
{ id: 102, name: "当帰湯" },  
{ id: 103, name: "酸棗仁湯" },  
{ id: 104, name: "辛夷清肺湯" },  
{ id: 105, name: "通導散" },  
{ id: 106, name: "温経湯" },  
{ id: 107, name: "牛車腎気丸" },  
{ id: 108, name: "人参養栄湯" },  
{ id: 109, name: "小柴胡湯加桔梗石膏" },  
{ id: 110, name: "立効散" },  
{ id: 111, name: "清心蓮子飲" },  
{ id: 112, name: "猪苓湯合四物湯" },  
{ id: 113, name: "三黄瀉心湯" },  
{ id: 114, name: "柴苓湯" },  
{ id: 115, name: "胃苓湯" },  
{ id: 116, name: "茯苓飲合半夏厚朴湯" },  
{ id: 117, name: "茵蔯五苓散" },  
{ id: 118, name: "苓姜朮甘湯" },  
{ id: 119, name: "苓甘姜味辛夏仁湯" },  
{ id: 120, name: "黄連湯" },  
{ id: 121, name: "三物黄芩湯" },  
{ id: 122, name: "排膿散及湯" },  
{ id: 123, name: "当帰建中湯" },  
{ id: 124, name: "川芎茶調散" },  
{ id: 125, name: "桂枝茯苓丸加薏苡仁" },  
{ id: 126, name: "麻子仁丸" },  
{ id: 127, name: "麻黄附子細辛湯" },  
{ id: 128, name: "啓脾湯" },  
{ id: 133, name: "大承気湯" },  
{ id: 134, name: "桂枝加芍薬大黄湯" },  
{ id: 135, name: "茵蔯蒿湯" },  
{ id: 136, name: "清暑益気湯" },  
{ id: 137, name: "加味帰脾湯" },  
{ id: 138, name: "桔梗湯" },  
{ id: 501, name: "紫雲膏" },  
{ id: 3020, name: "コウジン末" },  
{ id: 3023, name: "ブシ末" }  
];  
let activeQuizList = [];  
let currentIndex = 0;  
let score = 0;  
let wrongList = [];  
let hasAnswered = false;  
function showSetup() {  
document.getElementById('setup-screen').classList.remove('hidden');  
document.getElementById('quiz-screen').classList.add('hidden');  
document.getElementById('result-screen').classList.add('hidden');  
}  
function startQuiz(range) {  
if (range === 'all') {  
activeQuizList = [...KAMPO_DATA];  
} else {  
const [min, max] = range.split('-').map(Number);  
activeQuizList = KAMPO_DATA.filter(item => item.id >= min && item.id <= max);  
}  
// Numbers are already sorted in KAMPO_DATA  
currentIndex = 0;  
score = 0;  
wrongList = [];  
document.getElementById('setup-screen').classList.add('hidden');  
document.getElementById('result-screen').classList.add('hidden');  
document.getElementById('quiz-screen').classList.remove('hidden');  
loadQuestion();  
}  
function startReview() {  
activeQuizList = [...wrongList];  
currentIndex = 0;  
score = 0;  
wrongList = [];  
document.getElementById('result-screen').classList.add('hidden');  
document.getElementById('quiz-screen').classList.remove('hidden');  
loadQuestion();  
}  
function loadQuestion() {  
hasAnswered = false;  
const currentItem = activeQuizList[currentIndex];  
// UI Header Update  
document.getElementById('progress-text').innerText = ⁠問題 ${currentIndex + 1} / ${activeQuizList.length}⁠;  
document.getElementById('score-text').innerText = ⁠正解: ${score}⁠;  
const progressPercent = ((currentIndex) / activeQuizList.length) * 100;  
document.getElementById('progress-bar').style.width = ⁠${progressPercent}%⁠;  
// Display Number  
document.getElementById('question-number').innerText = ⁠No. ${currentItem.id}⁠;  
document.getElementById('result-feedback').innerText = '';  
document.getElementById('next-btn').classList.add('hidden');  
// Generate 4 Multiple Choice Options  
const options = generateOptions(currentItem);  
const container = document.getElementById('options-container');  
container.innerHTML = '';  
options.forEach(opt => {  
const btn = document.createElement('button');  
btn.className = 'w-full py-3.5 px-4 bg-white hover:bg-slate-50 text-slate-800 font-semibold border-2 border-slate-200 rounded-xl transition text-center shadow-sm active:scale-98 text-base sm:text-lg';  
btn.innerText = opt.name;  
btn.onclick = () => checkAnswer(opt, currentItem, btn);  
container.appendChild(btn);  
});  
}  
function generateOptions(correctItem) {  
let dummyPool = KAMPO_DATA.filter(item => item.id !== correctItem.id);  
// Shuffle pool to pick 3 random dummies  
dummyPool.sort(() => Math.random() - 0.5);  
const selected = [correctItem, ...dummyPool.slice(0, 3)];  
// Shuffle the 4 options  
return selected.sort(() => Math.random() - 0.5);  
}  
function checkAnswer(selectedOpt, correctItem, btnElement) {  
if (hasAnswered) return;  
hasAnswered = true;  
const buttons = document.getElementById('options-container').querySelectorAll('button');  
const feedbackEl = document.getElementById('result-feedback');  
if (selectedOpt.id === correctItem.id) {  
score++;  
btnElement.classList.remove('border-slate-200', 'hover:bg-slate-50');  
btnElement.classList.add('bg-emerald-100', 'border-emerald-500', 'text-emerald-900');  
feedbackEl.innerText = '◯ 正解！';  
feedbackEl.className = 'h-6 text-sm font-bold text-emerald-600 fade-in';  
} else {  
wrongList.push(correctItem);  
btnElement.classList.remove('border-slate-200', 'hover:bg-slate-50');  
btnElement.classList.add('bg-rose-100', 'border-rose-500', 'text-rose-900');  
feedbackEl.innerText = ⁠✕ 不正解 (正解: ${correctItem.name})⁠;  
feedbackEl.className = 'h-6 text-sm font-bold text-rose-600 fade-in';  
// Highlight correct button  
buttons.forEach(b => {  
if (b.innerText === correctItem.name) {  
b.classList.remove('border-slate-200');  
b.classList.add('border-emerald-500', 'bg-emerald-50', 'text-emerald-800');  
}  
});  
}  
// Disable all option buttons  
buttons.forEach(b => b.disabled = true);  
// Show Next Button  
document.getElementById('next-btn').classList.remove('hidden');  
}  
function nextQuestion() {  
currentIndex++;  
if (currentIndex < activeQuizList.length) {  
loadQuestion();  
} else {  
showResults();  
}  
}  
function showResults() {  
document.getElementById('quiz-screen').classList.add('hidden');  
document.getElementById('result-screen').classList.remove('hidden');  
const total = activeQuizList.length;  
const rate = Math.round((score / total) * 100);  
document.getElementById('final-score').innerText = ⁠${score} / ${total}⁠;  
document.getElementById('accuracy-rate').innerText = ⁠正解率: ${rate}%⁠;  
const reviewBtn = document.getElementById('review-btn');  
if (wrongList.length > 0) {  
reviewBtn.classList.remove('hidden');  
reviewBtn.innerText = ⁠間違えた ${wrongList.length} 問を復習する⁠;  
} else {  
reviewBtn.classList.add('hidden');  
}  
}  
</script>  
</body>  
</html>  
