# ECOing
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EcoCity: พลังงานสร้างอนาคต</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #eef2f3;
            color: #333;
            text-align: center;
            margin: 0;
            padding: 20px;
        }
        .container {
            max-width: 600px;
            margin: 0 auto;
            background: white;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        h1 { color: #2c3e50; }
        .stats {
            display: flex;
            justify-content: space-around;
            background: #f8f9fa;
            padding: 15px;
            border-radius: 8px;
            margin: 15px 0;
            font-size: 16px;
            font-weight: bold;
        }
        .panel {
            margin: 20px 0;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
        }
        button {
            background-color: #27ae60;
            color: white;
            border: none;
            padding: 10px 15px;
            margin: 5px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
            transition: 0.2s;
        }
        button:hover { background-color: #219653; }
        button.coal { background-color: #e74c3c; }
        button.coal:hover { background-color: #c0392b; }
        button:disabled { background-color: #bdc3c7; cursor: not-allowed; }
        .log {
            background: #222;
            color: #00ff00;
            padding: 10px;
            border-radius: 5px;
            text-align: left;
            font-family: monospace;
            height: 120px;
            overflow-y: auto;
            font-size: 13px;
        }
        .danger { color: #c0392b; }
        .success { color: #27ae60; }
    </style>
</head>
<body>

    <div class="container">
        <h1>🌱 EcoCity: พลังงานสร้างอนาคต</h1>
        <p>พัฒนาพลังงานสะอาดให้ถึง 300 MW โดยคุมมลพิษไม่ให้เกิน 10% และระวังอุปสรรคในเมือง!</p>

        <div class="stats">
            <div>💰 งบ: <span id="money">1000</span> G</div>
            <div>⚡ ไฟฟ้า: <span id="energy">0</span> / <span id="demand">50</span> MW</div>
            <div>☁️ มลพิษ: <span id="pollution">0</span>%</div>
        </div>

        <div class="panel">
            <h3>🎛️ แผงควบคุมการสร้างโรงไฟฟ้า</h3>
            <button id="btn-coal" class="coal" onclick="buildPlant('coal')">🔥 โรงไฟฟ้าถ่านหิน (150G)<br><small>+30 MW | +15% มลพิษ</small></button>
            <button id="btn-solar" onclick="buildPlant('solar')">☀️ โซลาร์เซลล์ (200G)<br><small>+20 MW | 0% มลพิษ</small></button>
            <button id="btn-wind" onclick="buildPlant('wind')">🌬️ กังหันลม (180G)<br><small>+15 MW | 0% มลพิษ</small></button>
        </div>

        <div class="panel">
            <h3>📜 บันทึกเหตุการณ์เมือง</h3>
            <div id="log" class="log">ยินดีต้อนรับท่านนายกฯ เริ่มต้นพัฒนาเมืองกันเถอะ!</div>
        </div>
    </div>

    <script>
        let money = 1000;
        let energy = 0;
        let demand = 50;
        let pollution = 0;
        let gameActive = true;

        function updateUI() {
            document.getElementById("money").innerText = money;
            document.getElementById("energy").innerText = energy;
            document.getElementById("demand").innerText = demand;
            document.getElementById("pollution").innerText = pollution;
        }

        function logMessage(msg, type = "") {
            const logBox = document.getElementById("log");
            let colorStyle = "";
            if (type === 'danger') colorStyle = "color: #ff6b6b;";
            if (type === 'success') colorStyle = "color: #51cf66;";
            if (type === 'warning') colorStyle = "color: #fcc419;";
            
            logBox.innerHTML += `<br><span style="${colorStyle}">${msg}</span>`;
            logBox.scrollTop = logBox.scrollHeight;
        }

        function buildPlant(type) {
            if (!gameActive) return;

            if (type === 'coal') {
                if (money >= 150) {
                    money -= 150;
                    energy += 30;
                    pollution += 15;
                    logMessage("❌ สร้างโรงไฟฟ้าถ่านหิน (+30 MW, +15% มลพิษ)");
                } else {
                    logMessage("⚠️ งบประมาณไม่พอสร้างโรงไฟฟ้าถ่านหิน!", "warning");
                }
            } else if (type === 'solar') {
                if (money >= 200) {
                    money -= 200;
                    energy += 20;
                    logMessage("☀️ สร้างโซลาร์เซลล์สำเร็จ (+20 MW, พลังงานสะอาด)");
                } else {
                    logMessage("⚠️ งบประมาณไม่พอสร้างโซลาร์เซลล์!", "warning");
                }
            } else if (type === 'wind') {
                if (money >= 180) {
                    money -= 180;
                    energy += 15;
                    logMessage("🌬️ สร้างกังหันลมสำเร็จ (+15 MW, พลังงานสะอาด)");
                } else {
                    logMessage("⚠️ งบประมาณไม่พอสร้างกังหันลม!", "warning");
                }
            }
            updateUI();
            checkGameStatus();
        }

        // ระบบตรวจสอบเงื่อนไข แพ้ / ชนะ
        function checkGameStatus() {
            if (!gameActive) return;

            // เงื่อนไขแพ้: มลพิษเกิน 80% หรือเงินติดลบ
            if (pollution >= 80) {
                gameActive = false;
                logMessage("💀 เกมโอเวอร์! เมืองล่มสลายเพราะวิกฤตมลพิษเกิน 80%!", "danger");
                alert("💀 เกมโอเวอร์! มลพิษล้นเมืองเกิน 80% ประชาชนอพยพหนีหมดแล้ว!");
                disableButtons();
            } else if (money < 0) {
                gameActive = false;
                logMessage("💸 เกมโอเวอร์! งบประมาณของเมืองติดลบ ล้มละลาย!", "danger");
                alert("💸 เกมโอเวอร์! งบประมาณเมืองเป็นติดลบ รัฐบาลล้มละลาย!");
                disableButtons();
            }

            // เงื่อนไขชนะ: ผลิตไฟฟ้าถึง 300 MW และคุมมลพิษไม่ให้เกิน 10%
            if (energy >= 300 && pollution <= 10) {
                gameActive = false;
                logMessage("🎉 ยินดีด้วย! คุณสร้าง 'มหานครพลังงานสะอาด' สำเร็จ!", "success");
                alert("🎉 ชนะแล้ว! คุณพัฒนาเมืองกลายเป็นมหานครพลังงานสะอาดระดับประเทศได้สำเร็จ!");
                disableButtons();
            }
        }

        function disableButtons() {
            document.getElementById("btn-coal").disabled = true;
            document.getElementById("btn-solar").disabled = true;
            document.getElementById("btn-wind").disabled = true;
        }

        // ระบบลูปเวลาและอุปสรรคสุ่ม (ทุกๆ 3.5 วินาที)
        setInterval(function() {
            if (!gameActive) return;

            // 1. คำนวณรายได้จากไฟฟ้า
            let income = Math.min(energy, demand) * 5;
            money += income;
            demand += 5; // ความต้องการไฟฟ้าเพิ่มขึ้นเรื่อยๆ

            logMessage(`💰 สิ้นเดือน: ได้รับภาษี ${income}G | ความต้องการไฟฟ้าเพิ่มเป็น ${demand} MW`);

            // 2. ระบบสุ่มอุปสรรค (Event Random) ทุกๆ รอบ
            let eventChance = Math.random();
            if (eventChance < 0.3) {
                // อุปสรรคที่ 1: ฟ้าปิด/ลมสงบ (โซลาร์หรือลมผลิตไฟลดลงชั่วคราว หรือเสียเงินซ่อมบำรุง)
                money -= 50;
                logMessage("🌪️ เกิดพายุฤดูร้อน! เสียค่าซ่อมบำรุงระบบโครงข่ายไฟฟ้า 50G", "warning");
            } else if (eventChance < 0.5) {
                // อุปสรรคที่ 2: ประชาชนประท้วงเรื่องมลพิษ (ถ้ามลพิษมากกว่า 30%)
                if (pollution > 30) {
                    money -= 100;
                    logMessage("🪧 ประชาชนรวมตัวประท้วงเรื่องมลพิษ! รัฐบาลเสียค่าชดเชย 100G", "danger");
                } else {
                    logMessage("✨ ประชาชนชื่นชมเมืองที่อากาศบริสุทธิ์ ได้รับโบนัสภาษี +50G", "success");
                    money += 50;
                }
            }

            updateUI();
            checkGameStatus();
        }, 3500);

        updateUI();
    </script>

</body>
</html>
