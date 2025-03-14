<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>隨機數生成器</title>
    <style>
        body {
            font-size: 20px;
        }
        button {
            font-size: 20px;
            padding: 10px 20px;
            cursor: pointer;
            margin: 10px;
        }
        #result {
            font-size: 24px;
            margin-top: 20px;
        }
        #history {
            margin-top: 20px;
        }
        ul {
            padding-left: 20px;
        }
        li {
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <button id="battleBtn">戰法</button>
    <button id="betBtn">下注</button>

    <p id="result"></p>

    <div id="history">
        <h2>歷史結果：</h2>
        <ul id="historyList"></ul>
    </div>

    <script>
        let lastBattleResult = localStorage.getItem("lastBattleResult"); // 從localStorage中獲取最後的戰法結果
        let history = JSON.parse(localStorage.getItem("history")) || []; // 從localStorage中獲取歷史結果

        // 顯示歷史結果
        const historyList = document.getElementById("historyList");
        history.forEach(item => {
            const listItem = document.createElement("li");
            listItem.textContent = item;
            historyList.appendChild(listItem);
        });

        // 如果有之前的戰法結果，顯示出來
        if (lastBattleResult !== null) {
            document.getElementById("battleBtn").textContent = `戰法: ${lastBattleResult}`;
            document.getElementById("result").textContent = `戰法結果: ${lastBattleResult}`;
        }

        // 當「戰法」按鈕被按下時
        document.getElementById("battleBtn").addEventListener("click", function() {
            // 生成第一個隨機數 1-4
            const firstRandom = Math.floor(Math.random() * 4) + 1;
            lastBattleResult = firstRandom; // 儲存戰法結果到變數

            // 更新戰法按鈕的文字顯示為隨機數
            document.getElementById("battleBtn").textContent = `戰法: ${firstRandom}`;

            document.getElementById("result").textContent = `戰法結果: ${firstRandom}`;

            // 儲存到localStorage中
            localStorage.setItem("lastBattleResult", firstRandom);

            // 記錄歷史結果
            history.push(`戰法結果: ${firstRandom}`);
            localStorage.setItem("history", JSON.stringify(history));

            // 更新歷史結果顯示
            const listItem = document.createElement("li");
            listItem.textContent = `戰法結果: ${firstRandom}`;
            historyList.appendChild(listItem);
        });

        // 當「下注」按鈕被按下時
        document.getElementById("betBtn").addEventListener("click", function() {
            if (lastBattleResult === null) {
                document.getElementById("result").textContent = "請先按下戰法按鈕生成結果！";
                return; // 如果沒有先點擊戰法按鈕，則提示
            }

            // 生成第二個隨機數 3-18
            const secondRandom = Math.floor(Math.random() * 16) + 3;
            let resultText = "";

            // 根據第一個隨機數的結果來決定顯示的結論
            if (lastBattleResult === 1) {
                resultText = (secondRandom % 2 === 0) ? "下閒" : "下莊";
            } else if (lastBattleResult === 2) {
                resultText = (secondRandom % 2 === 1) ? "下閒" : "下莊";
            } else if (lastBattleResult === 3) {
                resultText = (secondRandom > 10) ? "下閒" : "下莊";
            } else if (lastBattleResult === 4) {
                resultText = (secondRandom <= 10) ? "下閒" : "下莊";
            }

            // 顯示結果
            const resultDisplay = `第二個隨機數: ${secondRandom} => 結果: ${resultText}`;
            document.getElementById("result").textContent = resultDisplay;

            // 保留歷史結果
            const historyList = document.getElementById("historyList");
            const listItem = document.createElement("li");
            listItem.textContent = resultDisplay; // 添加到歷史列表
            historyList.appendChild(listItem);
        });
    </script>
</body>
</html>
