<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مختبر الكسور: اكتشف السر</title>
    <style>
        * { box-sizing: border-box; }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f7f6;
            color: #333;
            text-align: center;
            margin: 0;
            padding: 0;
            overflow-x: hidden;
        }

        /* ------------------------------------- */
        /* تصميم المشهد السينمائي الافتتاحي */
        /* ------------------------------------- */
        #start-overlay {
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            min-height: 100vh; background: linear-gradient(135deg, #1565c0, #00897b);
        }
        .start-btn {
            background-color: #ff9800; color: white; font-size: 2rem; font-weight: bold;
            padding: 15px 50px; border-radius: 50px; border: none; cursor: pointer;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3); transition: transform 0.3s; animation: pulse 2s infinite;
        }
        .start-btn:hover { transform: scale(1.05); background-color: #f57c00; }
        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(255, 152, 0, 0.7); }
            70% { box-shadow: 0 0 0 20px rgba(255, 152, 0, 0); }
            100% { box-shadow: 0 0 0 0 rgba(255, 152, 0, 0); }
        }

        #intro-sequence {
            display: none; flex-direction: column; align-items: center; justify-content: center;
            min-height: 100vh; background: #2c3e50; color: white; padding: 20px;
        }
        .scene-intro {
            display: flex; justify-content: center; align-items: flex-end; gap: 40px;
            margin-bottom: 40px; height: 300px;
        }
        .kid-intro { font-size: 120px; line-height: 1; z-index: 2; animation: fadeIn 1s; }
        
        .door-frame {
            width: 160px; height: 260px; background: #1a252f;
            border: 12px solid #7f8c8d; border-bottom: none;
            position: relative; perspective: 1200px;
        }
        .door {
            width: 100%; height: 100%; background: #34495e;
            position: absolute; top: 0; left: 0; right: 0;
            transform-origin: right; transition: transform 1.5s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex; flex-direction: column; align-items: center; justify-content: center;
            border: 2px solid #2c3e50; box-shadow: inset 0 0 20px rgba(0,0,0,0.5);
        }
        .door.open { transform: rotateY(100deg); }
        .door-knob {
            width: 18px; height: 18px; background: #f1c40f; border-radius: 50%;
            position: absolute; left: 15px; top: 50%; box-shadow: 0 2px 5px rgba(0,0,0,0.5);
        }
        .door-sign {
            background: #e74c3c; color: white; padding: 8px 15px;
            border-radius: 5px; font-weight: bold; font-size: 18px;
            text-align: center; border: 2px solid #c0392b; box-shadow: 0 4px 6px rgba(0,0,0,0.3);
        }

        .dialog-box {
            background: rgba(255, 255, 255, 0.95); color: #2c3e50;
            padding: 25px; border-radius: 15px; border: 4px solid #1565c0;
            font-size: 24px; font-weight: bold; width: 80%; max-width: 800px;
            min-height: 120px; display: flex; align-items: center; justify-content: center;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5); margin-bottom: 20px; text-align: center;
            transition: all 0.3s;
        }
        .dialog-speaker {
            position: absolute; margin-top: -110px; background: #1565c0; color: white;
            padding: 5px 20px; border-radius: 10px; font-size: 18px;
        }
        .next-dialog-btn {
            background: #4caf50; color: white; font-size: 20px; font-weight: bold;
            padding: 12px 40px; border: none; border-radius: 10px; cursor: pointer;
            transition: all 0.3s; box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }
        .next-dialog-btn:hover { background: #388e3c; transform: translateY(-3px); }

        /* ------------------------------------- */
        /* تصميم المختبر الرئيسي */
        /* ------------------------------------- */
        #main-lab { display: none; padding: 20px 10px; animation: fadeIn 1.5s ease-in-out; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

        h1.lab-title { color: #1565c0; margin-bottom: 5px; }
        
        .story-board {
            background: white; padding: 20px; border-radius: 15px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.08); max-width: 1000px; margin: 0 auto;
            border-top: 5px solid #1565c0;
        }

        .controls { margin: 15px 0; display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; }
        .control-btn {
            background-color: #1976d2; color: white; border: none;
            padding: 12px 20px; border-radius: 8px; cursor: pointer;
            font-size: 16px; font-weight: bold; transition: all 0.3s;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }
        .control-btn:hover { background-color: #115293; transform: translateY(-2px); }
        .control-btn.random-btn { background-color: #ff9800; }
        .control-btn.random-btn:hover { background-color: #e65100; }
        .control-btn.math-btn { background-color: #4caf50; }
        .control-btn.math-btn:hover { background-color: #2e7d32; }

        .scene {
            display: flex; justify-content: space-around; align-items: flex-end;
            gap: 20px; margin: 40px 0; min-height: 250px; position: relative;
        }
        
        .kid-container { position: relative; display: flex; flex-direction: column; align-items: center; }
        .kid { font-size: 90px; line-height: 1; z-index: 2; position: relative; }
        .device-signal {
            width: 25px; height: 10px; background: #00e5ff; border-radius: 10px;
            position: absolute; top: 55px; right: 15px; z-index: 3; 
            box-shadow: 0 0 10px #00e5ff, 0 0 20px #00e5ff;
            opacity: 0; transition: transform 0.4s linear;
        }
        .kid-name { font-weight: bold; color: #555; margin-top: 5px; }

        .lab-screen-container { position: relative; display: flex; flex-direction: column; align-items: center; }
        .screen-stand { width: 60px; height: 30px; background: #7f8c8d; border-radius: 5px 5px 0 0; }
        .screen-base { width: 120px; height: 10px; background: #2c3e50; border-radius: 5px; }

        .math-windows {
            display: none; justify-content: center; align-items: center; gap: 15px;
            margin: 40px 0; min-height: 200px; flex-wrap: wrap;
            background: #f8f9fa; padding: 20px; border-radius: 15px; border: 2px dashed #bdc3c7; direction: ltr;
        }

        .math-symbol { font-size: 50px; font-weight: bold; color: #2c3e50; }
        .math-number {
            font-size: 50px; font-weight: bold; color: #1565c0;
            background: white; padding: 10px 25px; border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1); direction: ltr;
        }

        .window-frame {
            background: #2c3e50; padding: 12px; border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3); border: 3px solid #34495e; transition: all 0.5s;
        }
        
        .window { display: grid; grid-template-columns: repeat(4, 40px); grid-template-rows: repeat(2, 60px); gap: 0px; background: #e3f2fd; transition: all 0.4s ease-out; direction: ltr; }
        .window-unequal { display: grid; grid-template-columns: 60px 20px 50px 30px; grid-template-rows: 80px 40px; gap: 0px; background: #e3f2fd; transition: all 0.4s ease-out; direction: ltr; }

        .glass-pane {
            background: #bbdefb; border: 1px solid rgba(255,255,255,0.7); display: flex; align-items: center; justify-content: center;
            font-size: 13px; font-weight: bold; color: transparent; transition: all 0.4s; direction: ltr;
        }

        .divided { gap: 4px; background: transparent; }
        .divided .glass-pane { background: #90caf9; border: 1px solid #42a5f5; border-radius: 4px; color: #0d47a1; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .window-unequal.divided .glass-pane { transform: scale(0.95); background: #ffcc80; border-color: #ff9800; color: #e65100; }

        .highlight-add { background: #4caf50 !important; color: white !important; opacity: 1 !important; border-color: #2e7d32 !important; }
        .highlight-sub-have { background: #1976d2 !important; color: white !important; opacity: 1 !important; }
        .highlight-sub-remove { background: #e53935 !important; color: white !important; opacity: 0.8 !important; }
        .highlight-mul { background: #9c27b0 !important; color: white !important; opacity: 1 !important; }
        .highlight-div { background: #ff9800 !important; color: white !important; opacity: 1 !important; }
        .result-highlight { border: 3px solid #ffeb3b !important; transform: scale(1.1) !important; z-index: 2; }

        .equation-box {
            margin-top: 20px; padding: 20px; background: #fff; border-radius: 15px;
            box-shadow: 0 8px 20px rgba(0,0,0,0.08); text-align: right; border: 3px solid #1565c0; max-width: 800px; margin-left: auto; margin-right: auto;
        }
        .eq-main { font-size: 32px; font-weight: bold; text-align: center; color: #1565c0; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 2px dashed #bdc3c7; direction: ltr; display: inline-block; width: 100%; }
        .step-item { font-size: 18px; margin-bottom: 15px; padding: 12px; background: #f8f9fa; border-right: 5px solid #1565c0; border-radius: 5px; color: #34495e; display: flex; align-items: center; gap: 10px; }
        .step-item.warning { border-right-color: #e53935; background: #ffebee; }
        .step-num { background: #1565c0; color: white; width: 30px; height: 30px; display: flex; align-items: center; justify-content: center; border-radius: 50%; font-weight: bold; flex-shrink: 0; }
        .step-item.warning .step-num { background: #e53935; }
        .result-step { background: #e8f5e9; border-right-color: #4caf50; font-weight: bold; font-size: 24px; justify-content: center; text-align: center; direction: ltr; color: #2e7d32; }
        .math-text { direction: ltr; display: inline-block; margin: 0 5px; font-weight: bold; }
        #story-text { font-size: 19px; line-height: 1.6; color: #2c3e50; min-height: 60px; font-weight: bold; }
    </style>
</head>
<body>

    <div id="start-overlay">
        <button class="start-btn" onclick="startStory()">ابدأ القصة 🎬</button>
    </div>

    <div id="intro-sequence">
        <div class="scene-intro">
            <div class="kid-intro">👨🏽‍🔬</div>
            <div class="door-frame">
                <div class="door" id="lab-door">
                    <div class="door-sign">مختبر<br>الكسور ❌</div>
                    <div class="door-knob"></div>
                </div>
            </div>
        </div>
        
        <div class="dialog-box" id="dialog-text">
            <div class="dialog-speaker" id="dialog-speaker">النظام الصوتي</div>
            <span id="typing-text">🚨 تنبيه! لا يمكن فتح مختبر الكسور، إلا إذا اكتشف أحمد السر.</span>
        </div>
        <button class="next-dialog-btn" id="next-dialog-btn" onclick="nextDialog()">التالي ▶</button>
    </div>

    <div id="main-lab">
        <div class="story-board">
            <h1 class="lab-title">مختبر الكسور 🔬🧮</h1>
            <p id="story-text">أحمد الآن داخل المختبر، أمامه شاشة ذكية كاملة تمثل العدد الصحيح (1).</p>
            
            <div class="controls">
                <button class="control-btn" onclick="resetLab()">📺 الشاشة الكاملة (1)</button>
                <button class="control-btn random-btn" onclick="activateDevice(false)">🔀 الوضع العشوائي</button>
                <button class="control-btn math-btn" onclick="activateDevice(true)">📐 الوضع الرياضي</button>
                <button class="control-btn" onclick="showAddition()">➕ الجمع</button>
                <button class="control-btn" onclick="showSubtraction()">➖ الطرح</button>
                <button class="control-btn" onclick="showMultiplication()">✖️ الضرب</button>
                <button class="control-btn" onclick="showDivision()">➗ القسمة</button>
            </div>

            <div class="scene" id="main-scene">
                <div class="kid-container">
                    <div class="kid">👨🏽‍🔬</div>
                    <div class="device-signal" id="animated-signal"></div>
                    <div class="kid-name">أحمد مع جهاز التقسيم 🎛️</div>
                </div>
                <div class="lab-screen-container">
                    <div id="house-window-container"></div>
                    <div class="screen-stand"></div>
                    <div class="screen-base"></div>
                </div>
            </div>

            <div class="math-windows" id="math-scene"></div>

            <div class="equation-box" id="equation-display">
                <div class="eq-main">1 Screen = 1 Whole</div>
                <div id="steps-container">
                    <div class="step-item"><div class="step-num">💡</div><div>الشاشة الآن متصلة كقطعة واحدة، وتمثل العدد الصحيح 1.</div></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // --------------------------------------------------------
        // إصلاح وتطوير دالة القراءة الصوتية لضمان عملها
        // --------------------------------------------------------
        // خدعة برمجية لتحميل الأصوات مسبقاً في المتصفح لتجنب التأخير
        if ('speechSynthesis' in window) {
            window.speechSynthesis.onvoiceschanged = function() {
                window.speechSynthesis.getVoices();
            };
        }

        function readTextAloud(text, isRobot) {
            if ('speechSynthesis' in window) {
                // إيقاف أي صوت سابق لتجنب التداخل
                window.speechSynthesis.cancel();
                
                const utterance = new SpeechSynthesisUtterance(text);
                
                // جلب جميع الأصوات المثبتة في المتصفح
                const voices = window.speechSynthesis.getVoices();
                
                // البحث الذكي عن أي صوت يدعم اللغة العربية
                const arabicVoice = voices.find(voice => voice.lang.startsWith('ar'));
                
                if (arabicVoice) {
                    utterance.voice = arabicVoice;
                } else {
                    // كحل بديل إذا لم يجد الصوت، يجبره على قراءة العربية
                    utterance.lang = 'ar-SA';
                }
                
                // تعديل نبرة الصوت لتكون آمنة ومدعومة في كل المتصفحات
                if (isRobot) {
                    utterance.pitch = 0.7; // تم تغييرها من 0.5 إلى 0.7 لأن بعض المتصفحات ترفض النبرة المنخفضة جداً
                    utterance.rate = 0.9;
                } else {
                    utterance.pitch = 1.3; // تم تغييرها من 1.5 إلى 1.3 لتجنب الأخطاء
                    utterance.rate = 1.0;
                }
                
                window.speechSynthesis.speak(utterance);
            } else {
                console.log("المتصفح الخاص بك لا يدعم ميزة القراءة الصوتية.");
            }
        }

        function playAlertSound() {
            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'square';
                osc.frequency.setValueAtTime(600, audioCtx.currentTime);
                osc.frequency.setValueAtTime(800, audioCtx.currentTime + 0.1);
                gain.gain.setValueAtTime(0.1, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.4);
                osc.connect(gain); gain.connect(audioCtx.destination);
                osc.start(); osc.stop(audioCtx.currentTime + 0.4);
            } catch (e) {}
        }

        function playDoorSound() {
            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(150, audioCtx.currentTime);
                osc.frequency.linearRampToValueAtTime(50, audioCtx.currentTime + 1.2);
                gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
                gain.gain.linearRampToValueAtTime(0.01, audioCtx.currentTime + 1.2);
                osc.connect(gain); gain.connect(audioCtx.destination);
                osc.start(); osc.stop(audioCtx.currentTime + 1.2);
            } catch (e) {}
        }

        let dialogStep = 1;

        function startStory() {
            document.getElementById('start-overlay').style.display = 'none';
            document.getElementById('intro-sequence').style.display = 'flex';
            
            playAlertSound(); 
            // تشغيل القراءة الصوتية فور الدخول
            setTimeout(() => {
                readTextAloud("تنبيه! لا يمكن فتح مختبر الكسور، إلا إذا اكتشف أحمد السر.", true);
            }, 300);
        }

        function nextDialog() {
            if (dialogStep === 1) {
                document.getElementById('dialog-speaker').innerText = "أحمد";
                document.getElementById('dialog-speaker').style.background = "#ff9800";
                
                const ahmadText = "سمعت أن هناك سرًا يجعل بعض الأجزاء تُسمى كسورًا، وبعضها لا. هل تساعدني في اكتشافه؟";
                document.getElementById('typing-text').innerText = ahmadText;
                
                document.getElementById('next-dialog-btn').innerText = "نعم، دعنا نفتح الباب! 🚪";
                document.getElementById('next-dialog-btn').style.background = "#e65100";
                
                // تشغيل القراءة الصوتية بصوت أحمد
                readTextAloud(ahmadText, false);
                
                dialogStep = 2;
            } else if (dialogStep === 2) {
                if ('speechSynthesis' in window) window.speechSynthesis.cancel(); 
                
                document.getElementById('dialog-text').style.opacity = "0";
                document.getElementById('next-dialog-btn').style.display = "none";
                
                playDoorSound(); 
                document.getElementById('lab-door').classList.add('open');
                
                setTimeout(() => {
                    document.getElementById('intro-sequence').style.display = 'none';
                    document.getElementById('main-lab').style.display = 'block';
                    resetLab();
                }, 1600);
            }
        }

        // --------------------------------------------------------
        // أكواد المختبر الرئيسي
        // --------------------------------------------------------
        const mainScene = document.getElementById('main-scene');
        const mathScene = document.getElementById('math-scene');
        const houseWindowContainer = document.getElementById('house-window-container');
        const storyText = document.getElementById('story-text');
        const eqMain = document.querySelector('.eq-main');
        const stepsContainer = document.getElementById('steps-container');
        const signal = document.getElementById('animated-signal');

        function playLaserSound() {
            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                const osc = audioCtx.createOscillator();
                const gain = audioCtx.createGain();
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(800, audioCtx.currentTime);
                osc.frequency.exponentialRampToValueAtTime(100, audioCtx.currentTime + 0.3);
                gain.gain.setValueAtTime(0.3, audioCtx.currentTime);
                gain.gain.exponentialRampToValueAtTime(0.01, audioCtx.currentTime + 0.3);
                osc.connect(gain); gain.connect(audioCtx.destination);
                osc.start(); osc.stop(audioCtx.currentTime + 0.3);
            } catch (e) { }
        }

        function createWindow(isEqual = true) {
            const frame = document.createElement('div');
            frame.className = 'window-frame';
            const win = document.createElement('div');
            win.className = isEqual ? 'window' : 'window-unequal'; 
            for(let i=0; i<8; i++) {
                const pane = document.createElement('div');
                pane.className = 'glass-pane';
                pane.innerHTML = isEqual ? '<span>1/8</span>' : '؟';
                win.appendChild(pane);
            }
            frame.appendChild(win); return frame;
        }

        function createSymbol(char) { const el = document.createElement('div'); el.className = 'math-symbol'; el.innerText = char; return el; }
        function createNumber(num) { const el = document.createElement('div'); el.className = 'math-number'; el.innerText = num; return el; }

        function highlightPanes(winFrame, count, className, markX = false, isResult = false) {
            winFrame.querySelector('.window').classList.add('divided');
            const panes = winFrame.querySelector('.window').children;
            for(let i=0; i<8; i++) {
                if(i < count) {
                    panes[i].classList.add(className);
                    if(markX) panes[i].innerHTML = '❌';
                    if(isResult) panes[i].classList.add('result-highlight');
                } else {
                    panes[i].style.opacity = '0.15';
                }
            }
        }

        function resetLab() {
            mainScene.style.display = 'flex'; mathScene.style.display = 'none';
            signal.style.transform = 'translate(0, 0)'; signal.style.opacity = '0';
            houseWindowContainer.innerHTML = ''; houseWindowContainer.appendChild(createWindow(true)); 
            storyText.innerText = "أحمد الآن داخل المختبر، أمامه شاشة ذكية كاملة تمثل العدد الصحيح (1).";
            eqMain.innerHTML = "1 = 8/8";
            stepsContainer.innerHTML = `<div class="step-item"><div class="step-num">💡</div><div>الشاشة متصلة كقطعة واحدة غير مقسمة.</div></div>`;
        }

        function activateDevice(isEqual) {
            mainScene.style.display = 'flex'; mathScene.style.display = 'none';
            signal.style.transform = 'translate(0, 0)'; signal.style.opacity = '1';
            houseWindowContainer.innerHTML = ''; houseWindowContainer.appendChild(createWindow(isEqual));
            
            const winFrame = houseWindowContainer.querySelector('.window-frame');
            const win = winFrame.firstElementChild;
            const signalRect = signal.getBoundingClientRect(); const winRect = winFrame.getBoundingClientRect();
            const deltaX = winRect.left - signalRect.left + (winRect.width / 2);
            const deltaY = winRect.top - signalRect.top + (winRect.height / 2);

            storyText.innerText = "قام أحمد بتوجيه جهاز التقسيم نحو الشاشة... ⚡";
            signal.style.transform = `translate(${deltaX}px, ${deltaY}px)`;
            
            setTimeout(() => {
                signal.style.opacity = '0'; playLaserSound(); win.classList.add('divided'); 
                
                if(!isEqual) {
                    storyText.innerText = "تم تفعيل الوضع العشوائي! انقسمت الشاشة إلى أجزاء مختلفة الحجم.";
                    eqMain.innerHTML = "❌ هذا ليس كسراً رياضياً!";
                    stepsContainer.innerHTML = `
                        <div class="step-item warning"><div class="step-num">!</div><div>انقسمت الشاشة إلى 8 أجزاء، لكن <strong>لاحظ أن حجمها مختلف!</strong>.</div></div>
                        <div class="step-item warning"><div class="step-num">!</div><div><strong>ولذلك لا تمثل كسوراً رياضية:</strong> لا يمكننا تسمية هذه القطع بالكسر <span class="math-text">1/8</span> لأنها ليست متساوية.</div></div>
                        <div class="step-item"><div class="step-num">💡</div><div>جرب الآن الضغط على زر (الوضع الرياضي) لنرى كيف يكون الكسر الصحيح!</div></div>
                    `;
                } else {
                    storyText.innerText = "تم تفعيل الوضع الرياضي! انقسمت الشاشة بدقة إلى 8 أجزاء متساوية تماماً.";
                    eqMain.innerHTML = "1 &rarr; 8 &times; (1/8)";
                    stepsContainer.innerHTML = `
                        <div class="step-item"><div class="step-num">1</div><div>الشاشة انقسمت إلى 8 أجزاء <strong>متساوية تماماً</strong>.</div></div>
                        <div class="step-item"><div class="step-num">2</div><div>بما أنها متساوية، يمكننا الآن تسمية كل جزء بـ (ثُمن).</div></div>
                        <div class="step-item"><div class="step-num">3</div><div>يُكتب رياضياً: <span class="math-text">1/8</span>. الآن الشاشة جاهزة للعمليات الحسابية!</div></div>
                    `;
                }
            }, 400);
        }

        function setupMathMode() { mainScene.style.display = 'none'; mathScene.style.display = 'flex'; mathScene.innerHTML = ''; }

        function showAddition() {
            setupMathMode();
            const win1 = createWindow(); const win2 = createWindow(); const winResult = createWindow();
            mathScene.appendChild(win1); mathScene.appendChild(createSymbol('+'));
            mathScene.appendChild(win2); mathScene.appendChild(createSymbol('=')); mathScene.appendChild(winResult);

            highlightPanes(win1, 3, 'highlight-add'); highlightPanes(win2, 2, 'highlight-add');
            highlightPanes(winResult, 5, 'highlight-add', false, true);

            storyText.innerText = "عملية الجمع: دمج الأجزاء المحددة في الشاشتين.";
            eqMain.innerHTML = "3/8 + 2/8 = ?";
            stepsContainer.innerHTML = `
                <div class="step-item"><div class="step-num">1</div><div><strong>المقام ثابت (8):</strong> نظام تقسيم الشاشة لم يتغير (8 أجزاء).</div></div>
                <div class="step-item"><div class="step-num">2</div><div><strong>نجمع البسط:</strong> قمنا بتحديد 3 أجزاء + جزأين = 5 أجزاء.</div></div>
                <div class="step-item result-step">الناتج النهائي = 5/8</div>
            `;
        }

        function showSubtraction() {
            setupMathMode();
            const win1 = createWindow(); const win2 = createWindow(); const winResult = createWindow();
            mathScene.appendChild(win1); mathScene.appendChild(createSymbol('-'));
            mathScene.appendChild(win2); mathScene.appendChild(createSymbol('=')); mathScene.appendChild(winResult);

            highlightPanes(win1, 7, 'highlight-sub-have'); highlightPanes(win2, 3, 'highlight-sub-remove', true);
            highlightPanes(winResult, 4, 'highlight-sub-have', false, true);

            storyText.innerText = "عملية الطرح: إلغاء تفعيل أجزاء محددة.";
            eqMain.innerHTML = "7/8 - 3/8 = ?";
            stepsContainer.innerHTML = `
                <div class="step-item"><div class="step-num">1</div><div><strong>المقام ثابت (8):</strong> التقسيم الأساسي للشاشة ثابت.</div></div>
                <div class="step-item"><div class="step-num">2</div><div><strong>نطرح البسط:</strong> كان لدينا 7 أجزاء مفعلة، ألغينا منها 3 (علامة ❌)، يتبقى 4 أجزاء.</div></div>
                <div class="step-item result-step">الناتج النهائي = 4/8</div>
            `;
        }

        function showMultiplication() {
            setupMathMode();
            const win1 = createWindow(); const winResult = createWindow();
            mathScene.appendChild(win1); mathScene.appendChild(createSymbol('×'));
            mathScene.appendChild(createNumber('2')); mathScene.appendChild(createSymbol('='));
            mathScene.appendChild(winResult);

            highlightPanes(win1, 3, 'highlight-mul'); highlightPanes(winResult, 6, 'highlight-mul', false, true);

            storyText.innerText = "عملية الضرب: مضاعفة عدد الأجزاء المحددة.";
            eqMain.innerHTML = "3/8 &times; 2 = ?";
            stepsContainer.innerHTML = `
                <div class="step-item"><div class="step-num">1</div><div><strong>المعنى:</strong> العدد 2 يعني مضاعفة الـ (3 أجزاء) المحددة مرتين.</div></div>
                <div class="step-item"><div class="step-num">2</div><div><strong>نضرب البسط:</strong> 3 أجزاء × 2 = 6 أجزاء إجمالية.</div></div>
                <div class="step-item result-step">الناتج النهائي = 6/8</div>
            `;
        }

        function showDivision() {
            setupMathMode();
            const win1 = createWindow(); const winResult = createWindow();
            mathScene.appendChild(win1); mathScene.appendChild(createSymbol('÷'));
            mathScene.appendChild(createNumber('2')); mathScene.appendChild(createSymbol('='));
            mathScene.appendChild(winResult);

            highlightPanes(win1, 6, 'highlight-div'); highlightPanes(winResult, 3, 'highlight-div', false, true);

            storyText.innerText = "عملية القسمة: توزيع الأجزاء المحددة بالتساوي.";
            eqMain.innerHTML = "(6/8) &divide; 2 = ?";
            stepsContainer.innerHTML = `
                <div class="step-item"><div class="step-num">1</div><div><strong>المعنى:</strong> لدينا 6 أجزاء مفعلة نريد تقسيمها إلى مجموعتين متساويتين.</div></div>
                <div class="step-item"><div class="step-num">2</div><div><strong>نقسم البسط:</strong> 6 أجزاء ÷ 2 = 3 أجزاء لكل مجموعة.</div></div>
                <div class="step-item result-step">الناتج النهائي = 3/8</div>
            `;
        }
    </script>
</body>
</html>
