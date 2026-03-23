<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HKDSE ICT 溫習筆記</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <nav>
        <div class="logo">DSE ICT Master</div>
        <ul>
            <li><a href="#notes">筆記</a></li>
            <li><a href="#quiz">MC 練習</a></li>
            <li><a href="#glossary">術語</a></li>
        </ul>
    </nav>

    <header>
        <h1>HKDSE ICT 溫習網站</h1>
        <p>為你的 DSE 作最後衝刺</p>
    </header>

    <main>
        <section id="notes">
            <h2>核心單元摘要</h2>
            <div class="card-grid">
                <div class="card">
                    <h3>系統單位 (Units)</h3>
                    <p>1 TB = 1024 GB, 1 GB = 1024 MB...</p>
                </div>
                <div class="card">
                    <h3>網絡 (Networking)</h3>
                    <p>OSI 七層模型、TCP/IP、HTTP vs HTTPS</p>
                </div>
            </div>
        </section>

        <section id="quiz">
            <h2>模擬練習 (MC)</h2>
            <div id="quiz-container">
                <p id="question">問題載入中...</p>
                <div id="options"></div>
                <button onclick="checkAnswer()">提交答案</button>
                <p id="feedback"></p>
            </div>
        </section>
    </main>

    <script src="script.js"></script>
</body>
</html>
const quizData = [
    {
        question: "在電腦系統中，下列哪項是 1 GB 的準確數值？",
        options: ["1000 MB", "1024 MB", "1024 KB", "1000 KB"],
        correct: 1
    },
    {
        question: "哪種協議用於安全網頁瀏覽？",
        options: ["HTTP", "FTP", "HTTPS", "SMTP"],
        correct: 2
    }
];

let currentIdx = 0;

function loadQuiz() {
    const q = quizData[currentIdx];
    document.getElementById('question').innerText = q.question;
    let optionsHtml = '';
    q.options.forEach((opt, index) => {
        optionsHtml += `<input type="radio" name="answer" value="${index}"> ${opt}<br>`;
    });
    document.getElementById('options').innerHTML = optionsHtml;
}

function checkAnswer() {
    const selected = document.querySelector('input[name="answer"]:checked');
    if (!selected) return alert("請選擇一個答案");

    const feedback = document.getElementById('feedback');
    if (parseInt(selected.value) === quizData[currentIdx].correct) {
        feedback.innerText = "正確！";
        feedback.style.color = "green";
    } else {
        feedback.innerText = "錯誤，再試一次。";
        feedback.style.color = "red";
    }
}

loadQuiz();
