# NTPU-Food-Wheel
不要再問我吃啥了 很躁
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>食物輪盤</title>
    <style>
        body {
            text-align: center;
            background-color: #1E3A8A;
            color: white;
            font-family: Arial, sans-serif;
        }
        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 50px;
        }
        canvas {
            border: 5px solid white;
            border-radius: 50%;
            background: #3B82F6;
        }
        button, input {
            margin-top: 10px;
            padding: 10px;
            font-size: 16px;
            border: none;
            background: #60A5FA;
            color: white;
            cursor: pointer;
            border-radius: 5px;
        }
        button:hover {
            background: #2563EB;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>今天吃什麼？</h1>
        <canvas id="wheelCanvas" width="400" height="400"></canvas>
        <button onclick="spinWheel()">轉動輪盤</button>
        <h2 id="result"></h2>
        <input type="text" id="newOption" placeholder="什麼好吃的加進來吧">
        <button onclick="addOption()">新增</button>
    </div>

    <script>
        let foodOptions = [
            "茗食茶", "吉購吉", "川邊烤肉", "帝一美食", "八方", "夏冰冬鍋", "安可美式餐廳", "聚", "堤諾義大利比薩", "林記宜蘭麵食館", 
            "一燔歐姆蛋咖哩", "原桌kimolo", "Yukimasa", "私嚐の吃飯", "滾吧", "茱莉媽媽", "YCHY", "隨園", "蔥抓餅", "狂一鍋", "泰廚房", 
            "貝里義大利薄餅", "好嘉飯館", "三峽第一臭豆腐", "大義了美食", "大口吃飯糰", "找光頭", "泰鍋古意", "大雄", "鮮味青食堂", 
            "俠客蛋餅", "麵G", "宏丞", "牛車水鐵板燒", "古都燒肉飯", "聯華港式", "旭壽司", "舊池屋", "夯牛屋", "湘柏苑", "啵霸", 
            "牛好吃", "八鍋", "五花馬水餃", "蕉ㄚ", "赤田屋咖哩", "秋香鵝肉", "黑霸", "小巷子", "黃燜雞", "Ti Jo Kitchen", 
            "龍鳳廣式燒臘", "麥當勞", "漫步波奇", "伊垛", "甘泉魚麵", "員林肉圓", "早到晚到"
        ];
        
        const canvas = document.getElementById("wheelCanvas");
        const ctx = canvas.getContext("2d");
        let angle = 0;
        let spinning = false;
        
        function drawWheel() {
            const numOptions = foodOptions.length;
            const sliceAngle = (2 * Math.PI) / numOptions;
            const colors = ["#93C5FD", "#60A5FA", "#2563EB"];
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            for (let i = 0; i < numOptions; i++) {
                ctx.beginPath();
                ctx.moveTo(200, 200);
                ctx.arc(200, 200, 200, angle + i * sliceAngle, angle + (i + 1) * sliceAngle);
                ctx.fillStyle = colors[i % 3];
                ctx.fill();
                ctx.stroke();
                
                ctx.save();
                ctx.translate(200, 200);
                ctx.rotate(angle + (i + 0.5) * sliceAngle);
                ctx.textAlign = "right";
                ctx.fillStyle = "white";
                ctx.font = "14px Arial";
                ctx.fillText(foodOptions[i], 170, 5);
                ctx.restore();
            }
        }
        
        function spinWheel() {
            if (spinning) return;
            spinning = true;
            let spinTime = Math.random() * 3000 + 2000;
            let startAngle = angle;
            let finalAngle = startAngle + Math.random() * (10 * Math.PI) + (5 * Math.PI);
            let startTime = null;
            
            function animateWheel(time) {
                if (!startTime) startTime = time;
                let progress = (time - startTime) / spinTime;
                if (progress > 1) progress = 1;
                angle = startAngle + (finalAngle - startAngle) * easeOut(progress);
                drawWheel();
                if (progress < 1) {
                    requestAnimationFrame(animateWheel);
                } else {
                    spinning = false;
                    let selectedIndex = Math.floor(((angle % (2 * Math.PI)) / (2 * Math.PI)) * foodOptions.length);
                    document.getElementById("result").innerText = "今天吃：" + foodOptions[selectedIndex];
                }
            }
            requestAnimationFrame(animateWheel);
        }
        
        function addOption() {
            let newOption = document.getElementById("newOption").value.trim();
            if (newOption && !foodOptions.includes(newOption)) {
                foodOptions.push(newOption);
                drawWheel();
                document.getElementById("newOption").value = "";
            }
        }
        
        function easeOut(t) {
            return 1 - Math.pow(1 - t, 3);
        }
        
        drawWheel();
    </script>
</body>
</html>
