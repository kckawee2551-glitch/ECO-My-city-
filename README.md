# ECOing
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EcoCity: พลังงานสร้างอนาคต</title>
    <!-- ฟอนต์ Prompt จาก Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Prompt:wght@300;400;500;600&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Prompt', sans-serif;
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            position: relative;
        }
        .container {
            width: 100%;
            max-width: 750px;
            background: #ffffff;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.12);
            transition: all 0.3s ease;
        }

        h1 { 
            color: #16a085; 
            margin-bottom: 5px; 
            font-weight: 600;
            font-size: 28px;
        }
        .subtitle {
            color: #7f8c8d;
            font-size: 15px;
            margin-bottom: 25px;
        }
        .stats {
            display: flex;
            justify-content: space-between;
            background: #f8f9fa;
            padding: 20px;
            border-radius: 12px;
            margin-bottom: 25px;
            border: 1px solid #e9ecef;
        }
        .stat-box {
            text-align: center;
            flex: 1;
        }
        .stat-box span {
            display: block;
            font-size: 22px;
            font-weight: 600;
            color: #2c3e50;
            margin-top: 5px;
        }
        .panel {
            margin-bottom: 25px;
            padding: 20px;
            border: 1px solid #e9ecef;
            border-radius: 12px;
            background: #fafbfc;
        }
        .panel h3 {
            margin-top: 0;
            font-size: 18px;
            color: #34495e;
        }
        .button-group {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-bottom: 12px;
        }
        .button-full {
            display: grid;
            grid-template-columns: 1fr;
        }
        button {
            font-family: 'Prompt', sans-serif;
            background-color: #27ae60;
            color: white;
            border: none;
            padding: 12px 10px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            font-weight: 500;
            transition: all 0.2s ease;
            box-shadow: 0 4px 6px rgba(0,0,0,0.05);
        }
        button:hover { background-color: #219653; transform: translateY(-2px); }
        button:active { transform: translateY(0); }
        button.coal { background-color: #e74c3c; }
        button.coal:hover { background-color: #c0392b; }
        button.filter-btn { background-color: #2980b9; }
        button.filter-btn:hover { background-color: #2471a3; }
        button:disabled { background-color: #bdc3c7; cursor: not-allowed; transform: none; box-shadow: none; }
        
        .log {
            background: #1e1e1e;
            color: #4af626;
            padding: 15px;
            border-radius: 8px;
            text-align: left;
            font-family: 'Courier New', Courier, monospace;
            height: 140px;
            overflow-y: auto;
            font-size: 13px;
            line-height: 1.5;
        }

        /* กล่องแสดงอุปสรรคตรงกลางจอชั่วคราว */
        #obstacle-banner {
            display: none;
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: rgba(0, 0, 0, 0.85);
            color: white;
            padding: 30px 50px;
            border-radius: 15px;
            text-align: center;
            z-index: 1000;
            box-shadow: 0 10px 25px rgba(0,0,0,0.5);
            animation: fadeInOut 3s ease forwards;
        }
        #obstacle-icon {
            font-size: 50px;
            margin-bottom: 10px;
        }
        #obstacle-text {
            font-size: 20px;
            font-weight: 500;
        }
        @keyframes fadeInOut {
            0% { opacity: 0; transform: translate(-50%, -40%); }
            15% { opacity: 1; transform: translate(-50%, -50%); }
            85% { opacity: 1; transform: translate(-50%, -50%); }
            100% { opacity: 0; transform: translate(-50%, -60%); }
        }

        /* หน้าต่างแสดงผลจบเกม (GameOver / Win Screen) */
        #game-over-screen {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.7);
            justify-content: center;
            align-items: center;
            z-index: 2000;
        }
        .modal-content {
            background: white;
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            max-width: 400px;
            width: 90%;
            box-shadow: 0 15px 35px rgba(0,0,0,0.3);
            animation: popUp 0.3s ease;
        }
        @keyframes popUp {
            0% { transform: scale(0.8); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
        .modal-content h2 { font-size: 28px; margin-top: 0; }
        .modal-content p { color: #666; font-size: 16px; margin-bottom: 25px; }
        .modal-content button {
            width: 100%;
            padding: 12px;
            font-size: 16px;
            background-color: #2c3e50;
        }
        .modal-content button:hover { background-color: #34495e; }
    </style>
</head>
<body>

    <!-- แบนเนอร์แสดงอุปสรรคตรงกลางจอ (ค้าง 3 วินาที) -->
    <div id="obstacle-banner">
        <div id="obstacle-icon">⚠️</div>
        <div id="obstacle-text">เกิดอุปสรรคขึ้นในเมือง!</div>
    </div>

    <!-- หน้าต่างจบเกม (ซ่อนไว้จนกว่าจะแพ้หรือชนะ) -->
    <div id="game-over-screen">
        <div class="modal-content">
            <h2 id="modal-title">คุณแพ้แล้ว</h2>
            <p id="modal-desc">เมืองของคุณไม่สามารถไปต่อได้...</p>
            <button onclick="restartGame()">🔄 เล่นใหม่อีกครั้ง</button>
        </div>
    </div>

    <div class="container" id="game-container">
        <h1>🌱 EcoCity: พลังงานสร้างอนาคต</h1>
        <div class="subtitle">ภารกิจ: พัฒนาพลังงานสะอาดให้ถึง 300 MW โดยรักษามลพิษไม่ให้เกิน 10%</div>

        <div class="stats">
            <div class="stat-box">
                💰 งบประมาณ
                <span id="money" style="color: #27ae60;">1000 G</span>
            </div>
            <div class="stat-box">
                ⚡ พลังงานไฟฟ้า
                <span><span id="energy">0</span> / <span id="demand">50</span> MW</span>
            </div>
            <div class="stat-box">
                ☁️ มลพิษ
                <span id="pollution" style="color: #e67e22;">0%</span>
            </div>
        </div>

        <div class="panel">
            <h3>🎛️ แผงควบคุมการสร้างโรงไฟฟ้าและระบบ</h3>
            <div class="button-group">
                <button id="btn-coal" class="coal" onclick="buildPlant('coal')">🔥 ถ่านหิน (150G)<br><small>+30MW | +15% มลพิษ</small></button>
                <button id="btn-solar" onclick="buildPlant('solar')">☀️ โซลาร์ (200G)<br><small>+20MW | 0% มลพิษ</small></button>
                <button id="btn-wind" onclick="buildPlant('wind')">🌬️ กังหันลม (180G)<br><small>+15MW | 0% มลพิษ</small></button>
            </div>
            <div class="button-full">
                <button id="btn-filter" class="filter-btn" onclick="buildPlant('filter')">🌿 ระบบกรองคาร์บอน (400G)<br><small>ลดมลพิษ 25% | ใช้พลังงานขับเคลื่อน -10MW</small></button>
            </div>
        </div>

        <div class="panel">
            <h3>📜 บันทึกเหตุการณ์เมือง</h3>
            <div id="log" class="log">[ระบบ] ยินดีต้อนรับท่านนายกเทศมนตรี เริ่มต้นพัฒนาเมืองพลังงานสะอาดกันเถอะ!</div>
        </div>
    </div>

    <script>
        let money = 1000;
        let energy = 0;
        let demand = 50;
        let pollution = 0;
        let gameActive = true;
        let afkTimer = 0; // ตัวจับเวลาไม่ได้เล่น (วินาที)
        const AFK_LIMIT = 1200; // 20 นาที = 1200 วินาที

        function updateUI() {
            document.getElementById("money").innerText = money + " G";
            document.getElementById("energy").innerText = energy;
            document.getElementById("demand").innerText = demand;
            document.getElementById("pollution").innerText = pollution + "%";
        }

        // แสดงรูปและข้อความอุปสรรคกลางจอ 3 วินาที
        function showObstaclePopup(icon, message) {
            const banner = document.getElementById("obstacle-banner");
            document.getElementById("obstacle-icon").innerText = icon;
            document.getElementById("obstacle-text").innerText = message;
            
            // รีเซ็ตอนิเมชันโดยการซ่อนแล้วแสดงใหม่
            banner.style.display = "none";
            setTimeout(() => {
                banner.style.display = "block";
            }, 10);
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

        function resetAfkTimer() {
            afkTimer = 0; // รีเซ็ตเวลา AFK ทุกครั้งที่มีการกดปุ่มกระทำใดๆ ในเกม
        }

        function buildPlant(type) {
            if (!gameActive) return;
            resetAfkTimer();

            if (type === 'coal') {
                if (money >= 150) {
                    money -= 150;
                    energy += 30;
                    pollution += 15;
                    logMessage("❌ สร้างโรงไฟฟ้าถ่านหินสำเร็จ (+30 MW, +15% มลพิษ)");
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
            } else if (type === 'filter') {
                if (money >= 400) {
                    money -= 400;
                    pollution = Math.max(0, pollution - 25);
                    energy = Math.max(0, energy - 10);
                    logMessage("🌿 เปิดใช้งานระบบกรองคาร์บอนสำเร็จ! (มลพิษลดลง 25%, เสียพลังงาน 10 MW)", "success");
                } else {
                    logMessage("⚠️ งบประมาณไม่พอสร้างระบบกรองคาร์บอน (ต้องการ 400G)!", "warning");
                }
            }
            updateUI();
            checkGameStatus();
        }

        function triggerGameOver(reasonText) {
            if (!gameActive) return;
            gameActive = false;
            document.getElementById("modal-title").innerText = "❌ คุณแพ้แล้ว";
            document.getElementById("modal-title").style.color = "#e74c3c";
            document.getElementById("modal-desc").innerText = reasonText;
            document.getElementById("game-over-screen").style.display = "flex";
        }

        function triggerVictory(reasonText) {
            if (!gameActive) return;
            gameActive = false;
            document.getElementById("modal-title").innerText = "🎉 คุณชนะแล้ว!";
            document.getElementById("modal-title").style.color = "#27ae60";
            document.getElementById("modal-desc").innerText = reasonText;
            document.getElementById("game-over-screen").style.display = "flex";
        }

        function restartGame() {
            location.reload();
        }

        function checkGameStatus() {
            if (!gameActive) return;

            if (pollution >= 80) {
                triggerGameOver("มลพิษล้นเมืองเกิน 80% ประชาชนอพยพหนีหมดแล้ว!");
            } else if (money < 0) {
                triggerGameOver("งบประมาณเมืองติดลบ รัฐบาลล้มละลาย!");
            } else if (afkTimer >= AFK_LIMIT) {
                triggerGameOver("คุณปล่อยทิ้งไว้ไม่เล่นเกิน 20 นาที เมืองถูกทอดทิ้ง!");
            }

            if (energy >= 300 && pollution <= 10) {
                triggerVictory("คุณพัฒนาเมืองกลายเป็นมหานครพลังงานสะอาดระดับประเทศได้สำเร็จ!");
            }
        }

        // ลูปเวลาหลัก (ทุกๆ 1 วินาที สำหรับเช็ค AFK และทุกๆ รอบ 3.5 วินาที สำหรับเหตุการณ์เมือง)
        let gameLoopCounter = 0;
        setInterval(function() {
            if (!gameActive) return;

            afkTimer++; // นับเวลา AFK ทุก 1 วินาที
            checkGameStatus();

            gameLoopCounter++;
            if (gameLoopCounter >= 3.5) { // จำลองรอบเวลาเมืองทุกๆ 3.5 วินาที
                gameLoopCounter = 0;

                let income = Math.min(energy, demand) * 5;
                money += income;
                demand += 5;

                logMessage(`💰 สิ้นเดือน: ได้รับภาษี ${income}G | ความต้องการไฟฟ้าเพิ่มเป็น ${demand} MW`);

                let eventChance = Math.random();
                if (eventChance < 0.3) {
                    money -= 50;
                    logMessage("🌪️ เกิดพายุฤดูร้อน! เสียค่าซ่อมบำรุงระบบโครงข่ายไฟฟ้า 50G", "warning");
                    showObstaclePopup("🌪️", "พายุฤดูร้อนถล่มเมือง! เสียค่าซ่อม 50G");
                } else if (eventChance < 0.5) {
                    if (pollution > 30) {
                        money -= 100;
                        logMessage("🪧 ประชาชนรวมตัวประท้วงเรื่องมลพิษ! รัฐบาลเสียค่าชดเชย 100G", "danger");
                        showObstaclePopup("🪧", "ประชาชนประท้วงเรื่องมลพิษ! เสียค่าชดเชย 100G");
                    } else {
                        logMessage("✨ ประชาชนชื่นชมเมืองที่อากาศบริสุทธิ์ ได้รับโบนัสภาษี +50G", "success");
                        money += 50;
                    }
                }

                updateUI();
                checkGameStatus();
            }
        }, 1000);

        updateUI();
    </script>

</body>
</html>
