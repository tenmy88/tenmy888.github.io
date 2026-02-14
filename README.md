<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>迎新红包大冒险 - 幸运大转盘</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
  
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            min-height: 100vh;
            background: linear-gradient(135deg, #f97316 0%, #db2777 35%, #7c3aed 100%);
            padding: 1rem;
            color: white;
        }
  
        .container {
            max-width: 1200px;
            margin: 0 auto;
        }
  
        .header {
            text-align: center;
            margin-bottom: 2rem;
        }
  
        .header h1 {
            font-size: clamp(1.5rem, 5vw, 2.5rem);
            font-weight: 800;
            color: #fff;
            margin: 0.5rem 0;
            text-shadow: 0 4px 6px rgba(0,0,0,0.2);
        }
  
        .header p {
            color: rgba(255, 255, 255, 0.9);
            font-size: 1.1rem;
            font-weight: 500;
        }
 
        .header .emoji-bounce {
            display: inline-block;
            animation: bounce 2s infinite;
        }
  
        .main-grid {
            display: grid;
            grid-template-columns: 320px 1fr;
            gap: 2rem;
            align-items: start;
        }
 
        /* 移动端适配*/
        @media (max-width: 900px) {
            .main-grid {
                grid-template-columns: 1fr;
            }
            .list-panel { order: 2; }
            .wheel-panel { order: 1; }
        }
  
        .panel {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(20px);
            border-radius: 1.5rem;
            padding: 1.5rem;
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }
  
        .panel-header {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            margin-bottom: 1.5rem;
            padding-bottom: 1rem;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
  
        .panel-header h2 {
            font-size: 1.5rem;
            font-weight: bold;
            color: #fff;
        }
  
        .icon {
            color: #fcd34d;
            font-size: 1.8rem;
        }
  
        .input-group {
            margin-bottom: 1rem;
        }
  
        .input-label {
            color: rgba(255, 255, 255, 0.9);
            margin-bottom: 0.5rem;
            display: block;
            font-weight: 500;
        }
  
        .input-row {
            display: flex;
            gap: 0.5rem;
        }
  
        input, textarea {
            background: rgba(255, 255, 255, 0.15);
            border: 1px solid rgba(255, 255, 255, 0.3);
            border-radius: 0.5rem;
            padding: 0.75rem;
            color: white;
            outline: none;
            transition: all 0.3s;
            width: 100%;
        }
  
        input::placeholder, textarea::placeholder {
            color: rgba(255, 255, 255, 0.5);
        }
  
        input:focus, textarea:focus {
            border-color: #fcd34d;
            background: rgba(255, 255, 255, 0.25);
            box-shadow: 0 0 0 3px rgba(252, 211, 77, 0.2);
        }
  
        .input-row input {
            flex: 1;
        }
  
        .btn {
            padding: 0.75rem 1.25rem;
            border: none;
            border-radius: 0.5rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 0.5rem;
            text-decoration: none;
            user-select: none;
        }
  
        .btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none !important;
        }
  
        .btn-primary {
            background: #fff;
            color: #db2777;
            white-space: nowrap;
        }
  
        .btn-primary:hover:not(:disabled) {
            background: #f3f4f6;
            transform: translateY(-1px);
        }
  
        .btn-danger {
            background: linear-gradient(135deg, #ef4444 0%, #b91c1c 100%);
            color: white;
            box-shadow: 0 4px 12px rgba(239, 68, 68, 0.3);
        }
 
        .btn-danger:hover:not(:disabled) {
            background: linear-gradient(135deg, #dc2626 0%, #991b1b 100%);
            transform: translateY(-1px);
        }
  
        .btn-secondary {
            background: rgba(255, 255, 255, 0.2);
            color: white;
            border: 1px solid rgba(255, 255, 255, 0.3);
        }
  
        .btn-secondary:hover {
            background: rgba(255, 255, 255, 0.3);
        }
  
        .punishment-list {
            max-height: 500px;
            overflow-y: auto;
            padding-right: 5px;
        }
         
        .punishment-list::-webkit-scrollbar {
            width: 6px;
        }
        .punishment-list::-webkit-scrollbar-thumb {
            background: rgba(255,255,255,0.3);
            border-radius: 3px;
        }
  
        .punishment-item {
            background: rgba(255, 255, 255, 0.1);
            padding: 0.75rem;
            border-radius: 0.75rem;
            margin-bottom: 0.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            transition: all 0.3s;
            color: white;
            border-left: 4px solid transparent;
        }
  
        .punishment-item:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateX(4px);
        }
  
        .punishment-item.active {
            background: rgba(252, 211, 77, 0.2);
            border-left-color: #fcd34d;
            font-weight: bold;
        }
  
        .punishment-item .remove-btn {
            background: rgba(0, 0, 0, 0.2);
            border: none;
            color: #fca5a5;
            cursor: pointer;
            font-size: 1.2rem;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s;
        }
  
        .punishment-item .remove-btn:hover {
            background: #ef4444;
            color: white;
        }
  
        .wheel-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100%;
        }
  
        .wheel-wrapper {
            position: relative;
            margin-bottom: 2rem;
            filter: drop-shadow(0 20px 40px rgba(0, 0, 0, 0.3));
            width: 100%;
            max-width: 500px;
            aspect-ratio: 1/1;
        }
  
        .wheel-pointer {
            position: absolute;
            top: -10px;
            left: 50%;
            transform: translateX(-50%);
            z-index: 10;
            width: 40px;
            height: 40px;
            background: #ef4444;
            border: 4px solid #fff;
            border-radius: 50%;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }
        .wheel-pointer::after {
            content: '';
            position: absolute;
            top: 100%;
            left: 50%;
            transform: translateX(-50%);
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-top: 20px solid #ef4444;
        }
  
        .wheel-svg {
            width: 100%;
            height: 100%;
            transition-property: transform;
            transition-timing-function: cubic-bezier(0.15, 0, 0.20, 1);
        }
  
        .control-buttons {
            display: flex;
            gap: 1.5rem;
            margin-bottom: 2rem;
            flex-wrap: wrap;
            justify-content: center;
        }
  
        .result-display {
            background: rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(10px);
            border: 2px solid #fcd34d;
            color: #fcd34d;
            padding: 1.5rem 3rem;
            border-radius: 1rem;
            text-align: center;
            font-weight: bold;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
            min-width: 300px;
            transform: scale(0.9);
            opacity: 0;
            transition: all 0.5s;
        }
 
        .result-display.show {
            transform: scale(1);
            opacity: 1;
        }
  
        .result-display h3 {
            font-size: 1.5rem;
            margin-bottom: 0.5rem;
            color: white;
        }
         
        .result-display .punishment-text {
            font-size: 1.75rem;
            font-weight: 800;
            word-break: break-word;
        }
  
        .celebration-modal {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.85);
            backdrop-filter: blur(8px);
            z-index: 50;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 1rem;
        }
  
        .celebration-content {
            background: linear-gradient(135deg, #ef4444 0%, #b91c1c 100%);
            border-radius: 2rem;
            padding: 3rem;
            max-width: 32rem;
            width: 100%;
            text-align: center;
            position: relative;
            overflow: hidden;
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.5);
            border: 4px solid rgba(255,255,255,0.2);
        }
  
        .celebration-bg {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: radial-gradient(circle, rgba(255,255,255,0.1) 0%, transparent 70%);
            animation: pulseBg 3s infinite;
        }
  
        .celebration-close {
            position: absolute;
            top: 1.5rem;
            right: 1.5rem;
            width: 2.5rem;
            height: 2.5rem;
            background: rgba(0, 0, 0, 0.3);
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            color: white;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
            transition: background 0.3s;
        }
  
        .celebration-close:hover {
            background: rgba(255, 255, 255, 0.2);
        }
  
        .celebration-inner {
            position: relative;
            z-index: 10;
        }
  
        .celebration-emoji {
            font-size: 6rem;
            margin-bottom: 1rem;
            display: block;
            filter: drop-shadow(0 4px 8px rgba(0,0,0,0.3));
        }
  
        .celebration-title {
            font-size: 2.5rem;
            font-weight: 900;
            color: #fff;
            margin-bottom: 1.5rem;
            text-transform: uppercase;
            letter-spacing: 2px;
        }
  
        .celebration-result-box {
            background: rgba(0, 0, 0, 0.3);
            backdrop-filter: blur(10px);
            border-radius: 1.5rem;
            padding: 2rem;
            margin-bottom: 2rem;
            border: 2px dashed rgba(255,255,255,0.3);
        }
  
        .celebration-result-label {
            font-size: 1.2rem;
            color: rgba(255,255,255,0.8);
            margin-bottom: 0.5rem;
        }
 
        .celebration-result-text {
            font-size: 2rem;
            font-weight: bold;
            color: #fcd34d;
            line-height: 1.3;
        }
  
        .celebration-btn {
            background: white;
            color: #b91c1c;
            border: none;
            padding: 1rem 3rem;
            border-radius: 3rem;
            font-weight: 900;
            font-size: 1.25rem;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }
  
        .celebration-btn:hover {
            background: #f3f4f6;
            transform: translateY(-2px) scale(1.05);
        }
  
        .empty-state {
            padding: 2rem;
            border-radius: 1rem;
            background: rgba(255, 255, 255, 0.05);
            color: rgba(255, 255, 255, 0.5);
            text-align: center;
            font-style: italic;
        }
  
        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }
 
        @keyframes pulseBg {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }
  
        @keyframes bounce {
            0%, 20%, 53%, 80%, 100% { transform: translateY(0); }
            40%, 43% { transform: translateY(-15px); }
            70% { transform: translateY(-7px); }
            90% { transform: translateY(-3px); }
        }
 
        @keyframes modalPop {
            0% { transform: scale(0.8) translateY(50px); opacity: 0; }
            70% { transform: scale(1.05); }
            100% { transform: scale(1) translateY(0); opacity: 1; }
        }
  
        .hidden {
            display: none;
        }
 
        .modal-enter {
            animation: modalPop 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 头部标题 -->
        <div class="header">
            <div style="display: flex; align-items: center; justify-content: center; gap: 1rem;">
                <span class="emoji-bounce">&#129313;</span>
                <h1>迎新红包大冒险 - 幸运大转盘</h1>
                <span class="emoji-bounce">&#127919;</span>
            </div>
            <p>恭喜发财，刺激好玩！准备好了吗？</p>
        </div>
  
        <div class="main-grid">
            <!-- 什么红包呢 -->
            <div class="panel list-panel">
                <div class="panel-header">
                    <span class="icon">&#128221;</span>
                    <h2>红包多大</h2>
                </div>
                  
                <div class="input-group">
                    <label class="input-label">添加红包：</label>
                    <div class="input-row">
                        <input type="text" id="punishmentInput" placeholder="例如：发红包10元" autocomplete="off">
                        <button class="btn btn-primary" id="addPunishmentBtn">添加</button>
                    </div>
                </div>
  
                <div class="punishment-list" id="punishmentList">
                </div>
            </div>
  
            <!-- 抽奖转盘 -->
            <div class="panel wheel-panel" style="display: flex; flex-direction: column; align-items: center; justify-content: center;">
                <div class="wheel-container">
                    <div class="wheel-wrapper">
                        <div class="wheel-pointer"></div>
                        <svg id="wheelSvg" class="wheel-svg" viewBox="-250 -250 500 500">
                            <defs>
                                <radialGradient id="centerGradient">
                                    <stop offset="0%" stop-color="#fcd34d" />
                                    <stop offset="100%" stop-color="#f59e0b" />
                                </radialGradient>
                                <filter id="shadow" x="-20%" y="-20%" width="140%" height="140%">
                                    <feDropShadow dx="0" dy="0" stdDeviation="5" flood-color="rgba(0,0,0,0.3)"/>
                                </filter>
                            </defs>
                            <g id="wheelSections"></g>
                            <circle cx="0" cy="0" r="40" fill="url(#centerGradient)" stroke="#fff" stroke-width="5" filter="url(#shadow)"/>
                            <circle cx="0" cy="0" r="30" fill="none" stroke="#fff" stroke-width="2" stroke-dasharray="4 4"/>
                        </svg>
                    </div>
  
                    <div class="control-buttons">
                        <button class="btn btn-danger" id="startBtn" style="min-width: 160px; font-size: 1.2rem; padding: 1rem;">
                            <span>&#9654;</span>
                            <span id="startBtnText">开始抽奖</span>
                        </button>
                        <button class="btn btn-secondary" id="resetBtn" style="padding: 1rem;">
                            <span>&#8635;</span>
                            重置
                        </button>
                    </div>
  
                    <div id="resultDisplay" class="result-display">
                        <h3>抽奖结果</h3>
                        <div class="punishment-text" id="winnerPunishment">...</div>
                    </div>
                </div>
            </div>
        </div>
    </div>
 
    <div id="celebrationModal" class="celebration-modal hidden">
        <div class="celebration-content">
            <div class="celebration-bg"></div>
            <button class="celebration-close" id="closeCelebrationBtn">×</button>
            <div class="celebration-inner">
                <span class="celebration-emoji">&#128520;</span>
                <h2 class="celebration-title">恭喜发财!</h2>
                  
                <div class="celebration-result-box">
                    <div class="celebration-result-label">接受挑战吧：</div>
                    <div class="celebration-result-text" id="celebrationPunishment"></div>
                </div>
  
                <button class="celebration-btn" id="confirmCelebrationBtn">恭喜发财！</button>
            </div>
        </div>
    </div>
  
    <script>
        // 全局变量
        const defaultPunishments = [
            '10元',
            '20元',
            '1分元',
            '1角元',
            '1元',
            '2元',
            '5元',
            '5角',
            '50元',
            '100元'
        ];
 
let punishments = JSON.parse(localStorage.getItem('wheelPunishments')) || defaultPunishments;
         
        let isSpinning = false;
        let currentRotation = 0;
        let currentPunishment = '';
  
        const colors = [
            '#FF6B6B', '#4ECDC4', '#45B7D1', '#FFA07A',
            '#98D8C8', '#F7DC6F', '#BB8FCE', '#F1948A',
            '#82E0AA', '#85C1E9', '#F0B27A', '#D7BDE2'
        ];
 
        function savePunishments() {
            localStorage.setItem('wheelPunishments', JSON.stringify(punishments));
        }
  
        document.addEventListener('DOMContentLoaded', function() {
            updatePunishmentList();
            updateWheel();
              
            const input = document.getElementById('punishmentInput');
            input.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    addPunishment();
                }
            });
 
            document.getElementById('addPunishmentBtn').addEventListener('click', addPunishment);
            document.getElementById('startBtn').addEventListener('click', startGame);
            document.getElementById('resetBtn').addEventListener('click', resetWheel);
            document.getElementById('closeCelebrationBtn').addEventListener('click', closeCelebration);
            document.getElementById('confirmCelebrationBtn').addEventListener('click', closeCelebration);
        });
  
        function addPunishment() {
            const input = document.getElementById('punishmentInput');
            const name = input.value.trim();
              
            if (name && !punishments.includes(name)) {
                punishments.push(name);
                input.value = '';
                input.focus();
                updatePunishmentList();
                updateWheel();
                savePunishments();
            }
        }
  
        function removePunishment(index) {
            if (isSpinning) return;
            punishments.splice(index, 1);
            updatePunishmentList();
            updateWheel();
            savePunishments();
        }
  
        function updatePunishmentList() {
            const list = document.getElementById('punishmentList');
              
            if (punishments.length === 0) {
                list.innerHTML = '<div class="empty-state">惩罚项目库是空的，请先添加项目！</div>';
                return;
            }
              
            list.innerHTML = punishments.map((punishment, index) => `
                <div class="punishment-item ${currentPunishment === punishment ? 'active' : ''}">
                    <span>${punishment}</span>
                    <button class="remove-btn">×</button>
                </div>
            `).join('');
        }
  
        function updateWheel() {
            const sectionsGroup = document.getElementById('wheelSections');
            sectionsGroup.innerHTML = '';
              
            if (punishments.length === 0) {
                const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
                path.setAttribute('d', 'M 0 0 L 240 0 A 240 240 0 1 1 -240 0 Z');
                path.setAttribute('fill', 'rgba(255,255,255,0.2)');
                path.setAttribute('stroke', '#fff');
                path.setAttribute('stroke-width', '4');
                  
                const text = document.createElementNS('http://www.w3.org/2000/svg', 'text');
                text.setAttribute('x', '0');
                text.setAttribute('y', '0');
                text.setAttribute('fill', '#fff');
                text.setAttribute('font-size', '32');
                text.setAttribute('font-weight', 'bold');
                text.setAttribute('text-anchor', 'middle');
                text.setAttribute('dominant-baseline', 'middle');
                text.textContent = '请添加项目';
                  
                sectionsGroup.appendChild(path);
                sectionsGroup.appendChild(text);
                return;
            }
              
            const sectionAngle = 360 / punishments.length;
            const radius = 240;
  
            punishments.forEach((punishment, index) => {
                const startAngle = index * sectionAngle;
                const endAngle = (index + 1) * sectionAngle;
                  
                const startAngleRad = (startAngle * Math.PI) / 180;
                const endAngleRad = (endAngle * Math.PI) / 180;
                  
                const largeArcFlag = sectionAngle > 180 ? 1 : 0;
                  
                const x1 = Math.cos(startAngleRad) * radius;
                const y1 = Math.sin(startAngleRad) * radius;
                const x2 = Math.cos(endAngleRad) * radius;
                const y2 = Math.sin(endAngleRad) * radius;
                  
                const pathData = `M 0 0 L ${x1} ${y1} A ${radius} ${radius} 0 ${largeArcFlag} 1 ${x2} ${y2} Z`;
                  
                const path = document.createElementNS('http://www.w3.org/2000/svg', 'path');
                path.setAttribute('d', pathData);
                path.setAttribute('fill', colors[index % colors.length]);
                path.setAttribute('stroke', '#fff');
                path.setAttribute('stroke-width', '4');
                path.setAttribute('filter', 'url(#shadow)');
                  
                const midAngle = (startAngle + endAngle) / 2;
                const textRadius = radius * 0.65;
                const textX = Math.cos((midAngle * Math.PI) / 180) * textRadius;
                const textY = Math.sin((midAngle * Math.PI) / 180) * textRadius;
                  
                let fontSize = 24;
                if (punishments.length > 12) fontSize = 14;
                else if (punishments.length > 8) fontSize = 18;
                  
                const text = document.createElementNS('http://www.w3.org/2000/svg', 'text');
                text.setAttribute('x', textX);
                text.setAttribute('y', textY);
                text.setAttribute('fill', '#fff');
                text.setAttribute('font-size', fontSize);
                text.setAttribute('font-weight', 'bold');
                text.setAttribute('text-anchor', 'middle');
                text.setAttribute('dominant-baseline', 'middle');
                text.setAttribute('transform', `rotate(${midAngle}, ${textX}, ${textY})`);
                 
                const maxChars = 8;
                let displayText = punishment;
                if (punishment.length > maxChars) {
                    displayText = punishment.substring(0, maxChars) + '..';
                }
                text.textContent = displayText;
                  
                sectionsGroup.appendChild(path);
                sectionsGroup.appendChild(text);
            });
        }
  
        function startGame() {
            if (punishments.length === 0) {
                alert('请先添加惩罚项目！');
                return;
            }
  
            if (isSpinning) return;
  
            isSpinning = true;
            currentPunishment = '';
              
            const startBtn = document.getElementById('startBtn');
            const startBtnText = document.getElementById('startBtnText');
            startBtn.disabled = true;
            startBtnText.textContent = '抽取中...';
              
            const resultDisplay = document.getElementById('resultDisplay');
            resultDisplay.classList.remove('show');
  
            const randomDuration = 4000 + Math.random() * 4000;
 
 
            const randomDegreeOffset = Math.floor(Math.random() * 360);
            const randomSpins = 5 + Math.floor(Math.random() * 5);
            const finalRotation = currentRotation + (randomSpins * 360) + randomDegreeOffset;
 
            currentRotation = finalRotation;
 
            const wheelSvg = document.getElementById('wheelSvg');
            wheelSvg.style.transitionDuration = `${randomDuration}ms`;
            wheelSvg.style.transform = `rotate(${currentRotation}deg)`;
 
            setTimeout(() => {
                isSpinning = false;
 
                const POINTER_ANGLE = 270;
 
                const actualRotation = currentRotation % 360;
 
                let winningAngle = (POINTER_ANGLE - actualRotation);
 
                winningAngle = winningAngle % 360;
                if (winningAngle < 0) {
                    winningAngle += 360;
                }
 
                const sectionAngle = 360 / punishments.length;
                const winningIndex = Math.floor(winningAngle / sectionAngle);
 
                currentPunishment = punishments[winningIndex];
 
                startBtn.disabled = false;
                startBtnText.textContent = '再次抽取';
                  
                document.getElementById('winnerPunishment').textContent = currentPunishment;
                resultDisplay.classList.add('show');
                  
                updatePunishmentList();
                showCelebration();
            }, randomDuration);
        }
  
        function resetWheel() {
            if (isSpinning) return;
              
            currentRotation = 0;
            currentPunishment = '';
              
            const wheelSvg = document.getElementById('wheelSvg');
            wheelSvg.style.transitionDuration = '0ms';
            wheelSvg.style.transform = 'rotate(0deg)';
              
            const resultDisplay = document.getElementById('resultDisplay');
            resultDisplay.classList.remove('show');
            updatePunishmentList();
            closeCelebration();
        }
  
        function showCelebration() {
            const modal = document.getElementById('celebrationModal');
            document.getElementById('celebrationPunishment').textContent = currentPunishment;
             
            modal.classList.remove('hidden');
            const content = modal.querySelector('.celebration-content');
            content.classList.remove('modal-enter');
            void content.offsetWidth;
            content.classList.add('modal-enter');
        }
  
        function closeCelebration() {
            document.getElementById('celebrationModal').classList.add('hidden');
        }
    </script>
</body>
</html>
