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
