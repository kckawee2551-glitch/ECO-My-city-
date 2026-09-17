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
            font-size: 18px;
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
        .log {
            background: #222;
            color: #00ff00;
            padding: 10px;
            border-radius: 5px;
            text-align: left;
            font-family: monospace;
            height: 100px;
            overflow-y: auto;
            font-size: 13px;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>🌱 EcoCity: พลังงานสร้างอนาคต</h1>
        <p>สร้างพลังงานสะอาดให้เมืองเติบโต และควบคุมมลพิษให้อยู่ในเกณฑ์ที่ปลอดภัย!</p>

        <div class="stats">
            <div>💰 งบประมาณ: <span id="money">1000</span> G</div>
            <div>⚡ ไฟฟ้า: <span id="energy">0</span> / <span id="demand">50</span> MW</div>
            <div>☁️ มลพิษ: <span id="pollution">0</span>%</div>
        </div>

        <div class="panel">
            <h3>🎛️ แผงควบคุมการสร้างโรงไฟฟ้า</h3>
            <button class="coal" onclick="buildPlant('coal')">🔥 โรงไฟฟ้าถ่านหิน (ราคา 150G)<br><small>+30 MW | +15% มลพิษ</small></button>
            <button onclick="buildPlant('solar')">☀️ ฟาร์มโซลาร์เซลล์ (ราคา 200G)<br><small>+20 MW | 0% มลพิษ</small></button>
            <button onclick="buildPlant('wind')">🌬️ กังหันลม (ราคา 180G)<br><small>+15 MW | 0% มลพิษ</small></button>
        </div>

        <div class="panel">
            <h3>📜 บันทึกเหตุการณ์เมือง</h3>
            <div id="log" class="log">ยินดีต้อนรับท่านนายกเทศมนตรี เริ่มต้นพัฒนาเมืองกันเถอะ!</div>
        </div>
    </div>

    <script>
        // ตัวแปรเกม
        let money = 1000;
        let energy = 0;
        let demand = 50;
        let pollution = 0;

        // อัปเดตการแสดงผลบนหน้าจอ
        function updateUI() {
            document.getElementById("money").innerText = money;
            document.getElementById("energy").innerText = energy;
            document.getElementById("demand").innerText = demand;
            document.getElementById("pollution").innerText = pollution;
        }

        // เพิ่มข้อความใน Log
        function logMessage(msg) {
            const logBox = document.getElementById("log");
            logBox.innerHTML += "<br>" + msg;
            logBox.scrollTop = logBox.scrollHeight;
        }

        // ฟังก์ชันสร้างโรงไฟฟ้า
        function buildPlant(type) {
            if (type === 'coal') {
                if (money >= 150) {
                    money -= 150;
                    energy += 30;
                    pollution += 15;
                    logMessage("❌ สร้างโรงไฟฟ้าถ่านหิน (+30 ไฟฟ้า, +15% มลพิษ)");
                } else {
                    logMessage("⚠️ งบประมาณไม่พอสร้างโรงไฟฟ้าถ่านหิน!");
                }
            } else if (type === 'solar') {
                if (money >= 200) {
                    money -= 200;
                    energy += 20;
                    logMessage("☀️ สร้างโซลาร์เซลล์สำเร็จ (+20 ไฟฟ้า, พลังงานสะอาด)");
                } else {
                    logMessage("⚠️ งบประมาณไม่พอสร้างโซลาร์เซลล์!");
                }
            } else if (type === 'wind') {
                if (money >= 180) {
                    money -= 180;
                    energy += 15;
                    logMessage("🌬️ สร้างกังหันลมสำเร็จ (+15 ไฟฟ้า, พลังงานสะอาด)");
                } else {
                    logMessage("⚠️ งบประมาณไม่พอสร้างกังหันลม!");
                }
            }
            updateUI();
        }

        // ระบบลูปเวลา (ทุกๆ 3 วินาที เมืองจะหักค่าใช้จ่ายและให้รายได้)
        setInterval(function() {
            // ได้รับรายได้ตามปริมาณไฟฟ้าที่ผลิตได้ (ถ้าผลิตพอดีหรือเกินความต้องการ)
            let income = Math.min(energy, demand) * 5;
            money += income;

            // เมืองเติบโต ความต้องการไฟฟ้าเพิ่มขึ้นเรื่อยๆ
            demand += 5;

            // ถ้ามลพิษสะสมสูงเกิน 80% จะโดนปรับเงิน
            if (pollution > 80) {
                money -= 100;
                logMessage("🚨 มลพิษล้นเมือง! รัฐบาลปรับเงิน 100G");
            }

            logMessage(`💰 สิ้นเดือน: ได้รับภาษี ${income}G | ความต้องการไฟฟ้าเพิ่มเป็น ${demand} MW`);
            updateUI();
        }, 3000);

        updateUI();
    </script>

</body>
</html>
