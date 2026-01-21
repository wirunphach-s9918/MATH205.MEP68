<html lang="th" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ระบบประกาศคะแนนสอบ</title>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Mitr:wght@300;400;500;600;700&amp;display=swap" rel="stylesheet">
  <style>
    body {
      box-sizing: border-box;
      font-family: 'Mitr', sans-serif;
    }
    
    .gradient-bg {
      background: linear-gradient(180deg, #60a5fa 0%, #93c5fd 50%, #dbeafe 100%);
      position: relative;
      overflow: hidden;
    }
    
    .cloud {
      position: absolute;
      background: white;
      border-radius: 100px;
      opacity: 0.9;
      animation: float 20s infinite ease-in-out;
    }
    
    .cloud::before,
    .cloud::after {
      content: '';
      position: absolute;
      background: white;
      border-radius: 100px;
    }
    
    .cloud1 {
      width: 120px;
      height: 50px;
      top: 10%;
      left: 10%;
      animation-delay: 0s;
    }
    
    .cloud1::before {
      width: 60px;
      height: 60px;
      top: -30px;
      left: 20px;
    }
    
    .cloud1::after {
      width: 70px;
      height: 55px;
      top: -25px;
      right: 20px;
    }
    
    .cloud2 {
      width: 100px;
      height: 40px;
      top: 20%;
      right: 15%;
      animation-delay: -5s;
    }
    
    .cloud2::before {
      width: 50px;
      height: 50px;
      top: -25px;
      left: 15px;
    }
    
    .cloud2::after {
      width: 60px;
      height: 45px;
      top: -20px;
      right: 15px;
    }
    
    .cloud3 {
      width: 90px;
      height: 35px;
      top: 60%;
      left: 20%;
      animation-delay: -10s;
    }
    
    .cloud3::before {
      width: 45px;
      height: 45px;
      top: -20px;
      left: 12px;
    }
    
    .cloud3::after {
      width: 50px;
      height: 40px;
      top: -18px;
      right: 12px;
    }
    
    .cloud4 {
      width: 110px;
      height: 45px;
      top: 70%;
      right: 10%;
      animation-delay: -15s;
    }
    
    .cloud4::before {
      width: 55px;
      height: 55px;
      top: -28px;
      left: 18px;
    }
    
    .cloud4::after {
      width: 65px;
      height: 50px;
      top: -23px;
      right: 18px;
    }
    
    @keyframes float {
      0%, 100% {
        transform: translateY(0px) translateX(0px);
      }
      25% {
        transform: translateY(-15px) translateX(10px);
      }
      50% {
        transform: translateY(-5px) translateX(-10px);
      }
      75% {
        transform: translateY(-20px) translateX(5px);
      }
    }
    
    .card-shadow {
      box-shadow: 0 10px 40px rgba(168, 85, 247, 0.15), 0 4px 12px rgba(0, 0, 0, 0.05);
    }
    
    .pulse-animation {
      animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
    }
    
    @keyframes pulse {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.7; }
    }
    
    .slide-up {
      animation: slideUp 0.5s ease-out forwards;
    }
    
    @keyframes slideUp {
      from {
        opacity: 0;
        transform: translateY(20px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }
    
    .bounce-in {
      animation: bounceIn 0.6s ease-out forwards;
    }
    
    @keyframes bounceIn {
      0% {
        opacity: 0;
        transform: scale(0.3);
      }
      50% {
        transform: scale(1.05);
      }
      70% {
        transform: scale(0.9);
      }
      100% {
        opacity: 1;
        transform: scale(1);
      }
    }
    
    .input-focus {
      transition: all 0.3s ease;
    }
    
    .input-focus:focus {
      transform: scale(1.02);
    }
    
    .btn-hover {
      transition: all 0.3s ease;
    }
    
    .btn-hover:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(168, 85, 247, 0.4);
    }
    
    .btn-hover:active {
      transform: translateY(0);
    }
  </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full w-full overflow-auto">
  <div id="app" class="h-full w-full gradient-bg">
   <div class="cloud cloud1"></div>
   <div class="cloud cloud2"></div>
   <div class="cloud cloud3"></div>
   <div class="cloud cloud4"></div>
  </div>
  <script>
    const MAX_SCORE = 20;
    
    const defaultConfig = {
      school_name: "โรงเรียนประตูชัย",
      class_name: "ชั้นประถมศึกษาปีที่ 2/5",
      subject_name: "สาย MEP (Mini English Program)",
      teacher_name: "ครูผู้สอน นางวิรัลพัชษ์ สว่างเดือน",
      lesson1_name: "บทที่ 7 เรื่อง เวลา",
      lesson2_name: "บทที่ 8 เรื่อง การวัดปริมาตร",
      lesson3_name: "บทที่ 9 เรื่อง รูปเรขาคณิต",
      background_color: "#60a5fa",
      card_color: "#ffffff",
      primary_color: "#fb923c",
      text_color: "#1e3a8a",
      accent_color: "#f472b6",
      font_family: "Mitr",
      font_size: 16
    };

    const studentsData = {
      "22321": { name: "เด็กชายภัทรพ��� แกนยางหวาย", lesson1: 16, lesson2: 13, lesson3: 16 },
      "22330": { name: "เด็กชายเอกศิษ���์ ทองสกุล", lesson1: 15, lesson2: 11, lesson3: 19 },
      "22335": { name: "เด็กชายภิญญาพัชญ์ เปรมสุข", lesson1: 2, lesson2: 7, lesson3: 10 },
      "22337": { name: "เด็กชายเมธาสิทธิ์ คงนุมัต��", lesson1: 7, lesson2: 6, lesson3: 14 },
      "22345": { name: "เด็กชายณัฎฐ์ตฤณ สุขประเสริฐ", lesson1: 17, lesson2: 11, lesson3: 19 },
      "22364": { name: "เด็กชายณภัทร แช่มวงศ์", lesson1: 9, lesson2: 8, lesson3: 12 },
      "22386": { name: "เด็กชายมีคุณ รัตนานนท์", lesson1: 16, lesson2: 16, lesson3: 20 },
      "22795": { name: "เด็กชายวรปรัชญ์ กิจเปรมถาวร", lesson1: 11, lesson2: 11, lesson3: 16 },
      "22799": { name: "เด็กชายอาชวิน เอื้อ��ยา", lesson1: 0, lesson2: 2, lesson3: 0 },
      "23364": { name: "เด็กชายเป็นไทย แก้วแดง", lesson1: 16, lesson2: 15, lesson3: 17 },
      "23365": { name: "เด็กชายรัตนพล เอมนฤมล", lesson1: 10, lesson2: 10, lesson3: 14 },
      "23366": { name: "เด็ก���ายสุกฤษฎิ์ ขอเสริมกลาง", lesson1: 12, lesson2: 11, lesson3: 20 },
      "23367": { name: "เด็กชายธ��วัฒน์ พรไตรรัตน์", lesson1: 9, lesson2: 3, lesson3: 13 },
      "23368": { name: "เด็กชายณัฐพนธ์ กันชะนะ", lesson1: 12, lesson2: 12, lesson3: 19 },
      "23369": { name: "เด็กชายกมลภพ ขยายฤทธิ์", lesson1: 9, lesson2: 3, lesson3: 15 },
      "22324": { name: "เด็กหญิงณัฐชนก พงษ์พิทักษ์วิเศษ", lesson1: 8, lesson2: 9, lesson3: 17 },
      "22327": { name: "เด็กหญิงปวริศา กงฉิน", lesson1: 9, lesson2: 8, lesson3: "absent" },
      "22351": { name: "เด็กหญิงรดา อินเที่ยง", lesson1: 11, lesson2: 10, lesson3: 19 },
      "22366": { name: "เด็กหญิงออมสิน พบสุข", lesson1: 4, lesson2: "absent", lesson3: 10 },
      "22370": { name: "เด็กหญิงปทิตตา ชัยพฤกษ์", lesson1: 12, lesson2: 14, lesson3: 17 },
      "22798": { name: "เด็กหญิงรวินท์นิภา โตแทน", lesson1: 15, lesson2: 17, lesson3: 20 },
      "23088": { name: "เด็กหญิงปวริศา กายจริต", lesson1: 4, lesson2: 4, lesson3: 7 },
      "23370": { name: "เด็กหญิงธนันรดา ผาสุขถิน", lesson1: 11, lesson2: 9, lesson3: 14 },
      "23371": { name: "เด็กหญิงปุญณดา คงสมจิตร์", lesson1: 17, lesson2: 12, lesson3: 17 },
      "23372": { name: "เด็กหญิงอริยดา ฉ่ำชื่น", lesson1: 7, lesson2: 5, lesson3: 17 },
      "23373": { name: "เด็กหญิงทักษิยนันท์ สุจริตพงษ์", lesson1: 14, lesson2: 12, lesson3: 15 },
      "23374": { name: "เด็กหญิงวนัฏษญา แสนกือ", lesson1: 6, lesson2: 4, lesson3: 10 },
      "23375": { name: "เด็กหญิงสุพิชญา วังหอม", lesson1: 16, lesson2: 11, lesson3: 18 },
      "23376": { name: "เด็กหญิงปุญฐิตา เฉลิมโภชน์", lesson1: 7, lesson2: 10, lesson3: 10 },
      "23377": { name: "เด็กหญิงณภัทร พุ่มพวง", lesson1: 2, lesson2: 6, lesson3: 11 }
    };

    let currentStudentId = null;
    let currentView = 'login';
    let config = { ...defaultConfig };

    function getScoreColor(score, max) {
      if (score === null || score === "absent") return '#9ca3af';
      const percent = (score / max) * 100;
      if (percent >= 80) return '#22c55e';
      if (percent >= 60) return '#84cc16';
      if (percent >= 50) return '#eab308';
      if (percent >= 40) return '#f97316';
      return '#ef4444';
    }

    function getScoreEmoji(score, max) {
      if (score === null || score === "absent") return '���';
      const percent = (score / max) * 100;
      if (percent >= 90) return '🌟';
      if (percent >= 80) return '⭐';
      if (percent >= 70) return '😊';
      if (percent >= 60) return '🙂';
      if (percent >= 50) return '💪';
      return '📚';
    }

    function getScoreMessage(score, max) {
      if (score === "absent") return 'ขาดสอบ';
      if (score === null) return 'ไม่มีข้อมูล';
      const percent = (score / max) * 100;
      if (percent >= 90) return 'ยอดเยี่ยมมาก!';
      if (percent >= 80) return 'ดีเยี่ยม!';
      if (percent >= 70) return 'ดีมาก!';
      if (percent >= 60) return 'ดี!';
      if (percent >= 50) return 'ผ่านเกณฑ์!';
      return 'พยายามต่อไปนะ!';
    }

    function calculateClassStats() {
      const students = Object.values(studentsData);
      
      const lesson1Scores = students.map(s => s.lesson1).filter(s => s !== null && s !== "absent" && typeof s === 'number');
      const lesson2Scores = students.map(s => s.lesson2).filter(s => s !== null && s !== "absent" && typeof s === 'number');
      const lesson3Scores = students.map(s => s.lesson3).filter(s => s !== null && s !== "absent" && typeof s === 'number');
      
      const calculateStats = (scores) => {
        if (scores.length === 0) return { avg: 0, max: 0, min: 0 };
        const sum = scores.reduce((a, b) => a + b, 0);
        return {
          avg: (sum / scores.length).toFixed(2),
          max: Math.max(...scores),
          min: Math.min(...scores)
        };
      };
      
      return {
        lesson1: calculateStats(lesson1Scores),
        lesson2: calculateStats(lesson2Scores),
        lesson3: calculateStats(lesson3Scores)
      };
    }

    function renderLoginView() {
      const app = document.getElementById('app');
      const fontFamily = config.font_family || defaultConfig.font_family;
      const fontSize = config.font_size || defaultConfig.font_size;
      const bgColor = config.background_color || defaultConfig.background_color;
      const cardColor = config.card_color || defaultConfig.card_color;
      const primaryColor = config.primary_color || defaultConfig.primary_color;
      const textColor = config.text_color || defaultConfig.text_color;
      const accentColor = config.accent_color || defaultConfig.accent_color;
      
      app.style.fontFamily = `${fontFamily}, sans-serif`;
      app.style.fontSize = `${fontSize}px`;
      app.style.background = `linear-gradient(180deg, ${bgColor} 0%, #93c5fd 50%, #dbeafe 100%)`;
      
      app.innerHTML = `
        <div class="cloud cloud1"></div>
        <div class="cloud cloud2"></div>
        <div class="cloud cloud3"></div>
        <div class="cloud cloud4"></div>

        <div class="min-h-full w-full flex flex-col items-center justify-center p-4">
          <div class="w-full max-w-md slide-up">
            <!-- School Header -->
            <div class="text-center mb-6">
              <div class="inline-flex items-center justify-center w-20 h-20 rounded-full mb-4" style="background: linear-gradient(135deg, ${primaryColor}, ${accentColor});">
                <span class="text-4xl">📚</span>
              </div>
              <h1 class="text-2xl font-bold mb-1" style="color: ${textColor}; font-size: ${fontSize * 1.5}px;" id="school-name-display">${config.school_name || defaultConfig.school_name}</h1>
              <p class="text-sm opacity-80" style="color: ${textColor}; font-size: ${fontSize * 0.875}px;" id="class-name-display">${config.class_name || defaultConfig.class_name}</p>
              <p class="text-xs opacity-60" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;" id="subject-name-display">${config.subject_name || defaultConfig.subject_name}</p>
            </div>
            
            <!-- Login Card -->
            <div class="rounded-3xl p-8 card-shadow" style="background: ${cardColor};">
              <div class="text-center mb-6">
                <h2 class="text-xl font-semibold mb-2" style="color: ${textColor}; font-size: ${fontSize * 1.25}px;">🎯 ตรวจสอบคะแนนสอบ</h2>
                <p class="text-sm opacity-70" style="color: ${textColor}; font-size: ${fontSize * 0.875}px;">กรุณากรอกรหัสนักเรียน 5 หลัก</p>
              </div>
              
              <div class="space-y-4">
                <div>
                  <label class="block text-sm font-medium mb-2" style="color: ${textColor}; font-size: ${fontSize * 0.875}px;">รหัสนักเรียน</label>
                  <input 
                    type="text" 
                    id="student-id-input"
                    maxlength="5"
                    placeholder="XXXXX"
                    class="w-full px-4 py-3 rounded-xl border-2 text-center font-semibold tracking-widest input-focus outline-none"
                    style="border-color: ${primaryColor}30; color: ${textColor}; font-size: ${fontSize * 1.25}px;"
                    inputmode="numeric"
                    pattern="[0-9]*"
                  >
                </div>
                
                <div id="error-message" class="hidden text-center py-2 px-4 rounded-lg bg-red-50 text-red-600" style="font-size: ${fontSize * 0.875}px;">
                  ⚠️ ไม่พบรหัสนักเรียนนี้ในระบบ
                </div>
                
                <button 
                  id="check-score-btn"
                  class="w-full py-4 rounded-xl text-white font-semibold btn-hover"
                  style="background: linear-gradient(135deg, ${primaryColor}, ${accentColor}); font-size: ${fontSize * 1.125}px;"
                >
                  🔍 ตรวจสอบคะแนน
                </button>
              </div>
              
              <div class="mt-6 pt-4 border-t border-gray-100 text-center">
                <p class="text-xs opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;" id="teacher-name-display">${config.teacher_name || defaultConfig.teacher_name}</p>
              </div>
            </div>
            
            <!-- Footer -->
            <div class="mt-6 text-center">
              <p class="text-xs opacity-40" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;">🔒 ข้อมูลคะแนนเป็นความลับส่วนบุคคล</p>
            </div>
          </div>
        </div>
      `;
      
      const input = document.getElementById('student-id-input');
      const btn = document.getElementById('check-score-btn');
      const errorMsg = document.getElementById('error-message');
      
      input.addEventListener('input', (e) => {
        e.target.value = e.target.value.replace(/\D/g, '').slice(0, 5);
        errorMsg.classList.add('hidden');
      });
      
      input.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
          checkStudent();
        }
      });
      
      btn.addEventListener('click', checkStudent);
      
      function checkStudent() {
        const studentId = input.value.trim();
        if (studentId.length !== 5) {
          errorMsg.textContent = '⚠️ กรุณากรอกรหัสนักเรียน 5 หลัก';
          errorMsg.classList.remove('hidden');
          input.focus();
          return;
        }
        
        if (!studentsData[studentId]) {
          errorMsg.textContent = '⚠️ ไม่พบรหัสนักเรียนนี้ในระบบ';
          errorMsg.classList.remove('hidden');
          input.value = '';
          input.focus();
          return;
        }
        
        currentStudentId = studentId;
        currentView = 'result';
        renderResultView();
      }
    }

    function renderResultView() {
      const app = document.getElementById('app');
      const student = studentsData[currentStudentId];
      const fontFamily = config.font_family || defaultConfig.font_family;
      const fontSize = config.font_size || defaultConfig.font_size;
      const bgColor = config.background_color || defaultConfig.background_color;
      const cardColor = config.card_color || defaultConfig.card_color;
      const primaryColor = config.primary_color || defaultConfig.primary_color;
      const textColor = config.text_color || defaultConfig.text_color;
      const accentColor = config.accent_color || defaultConfig.accent_color;
      
      app.style.fontFamily = `${fontFamily}, sans-serif`;
      app.style.fontSize = `${fontSize}px`;
      
      const lesson1Color = getScoreColor(student.lesson1, MAX_SCORE);
      const lesson2Color = getScoreColor(student.lesson2, MAX_SCORE);
      const lesson3Color = getScoreColor(student.lesson3, MAX_SCORE);
      const lesson1Percent = (student.lesson1 !== null && student.lesson1 !== "absent") ? (student.lesson1 / MAX_SCORE) * 100 : 0;
      const lesson2Percent = (student.lesson2 !== null && student.lesson2 !== "absent") ? (student.lesson2 / MAX_SCORE) * 100 : 0;
      const lesson3Percent = (student.lesson3 !== null && student.lesson3 !== "absent") ? (student.lesson3 / MAX_SCORE) * 100 : 0;
      
      const totalScore = (typeof student.lesson1 === 'number' ? student.lesson1 : 0) + 
                         (typeof student.lesson2 === 'number' ? student.lesson2 : 0) + 
                         (typeof student.lesson3 === 'number' ? student.lesson3 : 0);
      const maxTotal = MAX_SCORE * 3;
      const totalPercent = (totalScore / maxTotal) * 100;
      const totalColor = getScoreColor(totalScore, maxTotal);
      
      const classStats = calculateClassStats();
      
      app.innerHTML = `
        <div class="min-h-full w-full flex flex-col items-center justify-start p-4 py-8 overflow-auto">
          <div class="w-full max-w-2xl bounce-in">
            <!-- Back Button -->
            <button id="back-btn" class="mb-4 flex items-center gap-2 px-4 py-2 rounded-full font-medium transition-all hover:scale-105" style="color: ${primaryColor}; background: ${primaryColor}15; font-size: ${fontSize * 0.875}px;">
              ← กลับหน้าหลัก
            </button>
            
            <!-- Student Info Card -->
            <div class="rounded-3xl p-6 card-shadow mb-4" style="background: ${cardColor};">
              <div class="flex items-center gap-4 mb-4">
                <div class="w-16 h-16 rounded-full flex items-center justify-center text-3xl" style="background: linear-gradient(135deg, ${primaryColor}20, ${accentColor}20);">
                  ${student.name.includes('ชาย') ? '👦' : '👧'}
                </div>
                <div>
                  <h2 class="font-bold" style="color: ${textColor}; font-size: ${fontSize * 1.125}px;">${student.name}</h2>
                  <p class="opacity-60" style="color: ${textColor}; font-size: ${fontSize * 0.875}px;">รหัส: ${currentStudentId}</p>
                  <p class="opacity-40" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;" id="class-display-result">${config.class_name || defaultConfig.class_name}</p>
                </div>
              </div>
            </div>
            
            <!-- Scores Grid -->
            <div class="grid grid-cols-3 gap-3 mb-4">
              <!-- Lesson 1 -->
              <div class="rounded-2xl p-4 card-shadow" style="background: ${cardColor};">
                <div class="text-center">
                  <p class="font-medium opacity-60 mb-2" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;">บทที่ 7</p>
                  <div class="relative w-20 h-20 mx-auto mb-2">
                    <svg class="w-full h-full transform -rotate-90">
                      <circle cx="40" cy="40" r="35" fill="none" stroke="#e9d5ff" stroke-width="6"/>
                      <circle cx="40" cy="40" r="35" fill="none" stroke="${lesson1Color}" stroke-width="6" 
                        stroke-dasharray="${lesson1Percent * 2.2} 220" stroke-linecap="round"/>
                    </svg>
                    <div class="absolute inset-0 flex flex-col items-center justify-center">
                      <span class="font-bold" style="color: ${lesson1Color}; font-size: ${fontSize * 1.125}px;">${student.lesson1 === "absent" ? '❌' : (student.lesson1 !== null ? student.lesson1 : '-')}</span>
                      ${student.lesson1 !== "absent" ? `<span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;">/${MAX_SCORE}</span>` : ''}
                    </div>
                  </div>
                  <div style="font-size: ${fontSize * 1.25}px;" class="mb-1">${getScoreEmoji(student.lesson1, MAX_SCORE)}</div>
                  <p class="font-medium" style="color: ${lesson1Color}; font-size: ${fontSize * 0.75}px;">${getScoreMessage(student.lesson1, MAX_SCORE)}</p>
                  <p class="opacity-40 mt-1" style="color: ${textColor}; font-size: ${fontSize * 0.7}px;">เวลา</p>
                  
                  <!-- Class Stats -->
                  <div class="mt-3 pt-3 border-t border-gray-100 space-y-1">
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">เฉลี่ย:</span>
                      <span class="font-semibold" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">${classStats.lesson1.avg}</span>
                    </div>
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">สูงสุด:</span>
                      <span class="font-semibold text-green-600" style="font-size: ${fontSize * 0.625}px;">${classStats.lesson1.max}</span>
                    </div>
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">ต่ำสุด:</span>
                      <span class="font-semibold text-orange-600" style="font-size: ${fontSize * 0.625}px;">${classStats.lesson1.min}</span>
                    </div>
                  </div>
                </div>
              </div>
              
              <!-- Lesson 2 -->
              <div class="rounded-2xl p-4 card-shadow" style="background: ${cardColor};">
                <div class="text-center">
                  <p class="font-medium opacity-60 mb-2" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;">บทที่ 8</p>
                  <div class="relative w-20 h-20 mx-auto mb-2">
                    <svg class="w-full h-full transform -rotate-90">
                      <circle cx="40" cy="40" r="35" fill="none" stroke="#e9d5ff" stroke-width="6"/>
                      <circle cx="40" cy="40" r="35" fill="none" stroke="${lesson2Color}" stroke-width="6" 
                        stroke-dasharray="${lesson2Percent * 2.2} 220" stroke-linecap="round"/>
                    </svg>
                    <div class="absolute inset-0 flex flex-col items-center justify-center">
                      <span class="font-bold" style="color: ${lesson2Color}; font-size: ${fontSize * 1.125}px;">${student.lesson2 === "absent" ? '❌' : (student.lesson2 !== null ? student.lesson2 : '-')}</span>
                      ${student.lesson2 !== "absent" ? `<span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;">/${MAX_SCORE}</span>` : ''}
                    </div>
                  </div>
                  <div style="font-size: ${fontSize * 1.25}px;" class="mb-1">${getScoreEmoji(student.lesson2, MAX_SCORE)}</div>
                  <p class="font-medium" style="color: ${lesson2Color}; font-size: ${fontSize * 0.75}px;">${getScoreMessage(student.lesson2, MAX_SCORE)}</p>
                  <p class="opacity-40 mt-1" style="color: ${textColor}; font-size: ${fontSize * 0.7}px;">วัดปริมาตร</p>
                  
                  <!-- Class Stats -->
                  <div class="mt-3 pt-3 border-t border-gray-100 space-y-1">
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">เฉลี่ย:</span>
                      <span class="font-semibold" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">${classStats.lesson2.avg}</span>
                    </div>
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">สูงสุด:</span>
                      <span class="font-semibold text-green-600" style="font-size: ${fontSize * 0.625}px;">${classStats.lesson2.max}</span>
                    </div>
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">ต่ำสุด:</span>
                      <span class="font-semibold text-orange-600" style="font-size: ${fontSize * 0.625}px;">${classStats.lesson2.min}</span>
                    </div>
                  </div>
                </div>
              </div>
              
              <!-- Lesson 3 -->
              <div class="rounded-2xl p-4 card-shadow" style="background: ${cardColor};">
                <div class="text-center">
                  <p class="font-medium opacity-60 mb-2" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;">บทที่ 9</p>
                  <div class="relative w-20 h-20 mx-auto mb-2">
                    <svg class="w-full h-full transform -rotate-90">
                      <circle cx="40" cy="40" r="35" fill="none" stroke="#e9d5ff" stroke-width="6"/>
                      <circle cx="40" cy="40" r="35" fill="none" stroke="${lesson3Color}" stroke-width="6" 
                        stroke-dasharray="${lesson3Percent * 2.2} 220" stroke-linecap="round"/>
                    </svg>
                    <div class="absolute inset-0 flex flex-col items-center justify-center">
                      <span class="font-bold" style="color: ${lesson3Color}; font-size: ${fontSize * 1.125}px;">${student.lesson3 === "absent" ? '❌' : (student.lesson3 !== null ? student.lesson3 : '-')}</span>
                      ${student.lesson3 !== "absent" ? `<span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;">/${MAX_SCORE}</span>` : ''}
                    </div>
                  </div>
                  <div style="font-size: ${fontSize * 1.25}px;" class="mb-1">${getScoreEmoji(student.lesson3, MAX_SCORE)}</div>
                  <p class="font-medium" style="color: ${lesson3Color}; font-size: ${fontSize * 0.75}px;">${getScoreMessage(student.lesson3, MAX_SCORE)}</p>
                  <p class="opacity-40 mt-1" style="color: ${textColor}; font-size: ${fontSize * 0.7}px;">เรขาคณิต</p>
                  
                  <!-- Class Stats -->
                  <div class="mt-3 pt-3 border-t border-gray-100 space-y-1">
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">เฉลี่ย:</span>
                      <span class="font-semibold" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">${classStats.lesson3.avg}</span>
                    </div>
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">สูงสุด:</span>
                      <span class="font-semibold text-green-600" style="font-size: ${fontSize * 0.625}px;">${classStats.lesson3.max}</span>
                    </div>
                    <div class="flex justify-between items-center">
                      <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.625}px;">ต่ำสุด:</span>
                      <span class="font-semibold text-orange-600" style="font-size: ${fontSize * 0.625}px;">${classStats.lesson3.min}</span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            
            <!-- Total Score -->
            <div class="rounded-2xl p-6 card-shadow" style="background: linear-gradient(135deg, ${primaryColor}10, ${accentColor}10);">
              <div class="flex items-center justify-between">
                <div>
                  <p class="font-medium opacity-60 mb-1" style="color: ${textColor}; font-size: ${fontSize * 0.875}px;">คะแนนรวม</p>
                  <div class="flex items-baseline gap-2">
                    <span class="font-bold" style="color: ${totalColor}; font-size: ${fontSize * 2.5}px;">${totalScore}</span>
                    <span class="opacity-50" style="color: ${textColor}; font-size: ${fontSize * 1.125}px;">/ ${maxTotal}</span>
                  </div>
                  <p class="mt-1" style="color: ${totalColor}; font-size: ${fontSize * 0.875}px;">${getScoreMessage(totalScore, maxTotal)}</p>
                </div>
                <div style="font-size: ${fontSize * 3.75}px;">${getScoreEmoji(totalScore, maxTotal)}</div>
              </div>
              
              <!-- Progress Bar -->
              <div class="mt-4 h-3 rounded-full bg-white overflow-hidden">
                <div class="h-full rounded-full transition-all duration-1000" style="width: ${totalPercent}%; background: linear-gradient(90deg, ${primaryColor}, ${accentColor});"></div>
              </div>
              <p class="text-center mt-2 opacity-50" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;">${totalPercent.toFixed(1)}% ของคะแนนเต็ม</p>
            </div>
            
            <!-- Motivational Message -->
            <div class="mt-4 p-4 rounded-2xl text-center" style="background: ${cardColor};">
              <p style="color: ${textColor}; font-size: ${fontSize * 0.875}px;">
                ${totalPercent >= 70 ? '🎉 เก่งมากเลย! รักษาความดีต่อไปนะ' : totalPercent >= 50 ? '💪 พยายามดีแล้ว! สู้ต่อไปนะ' : '📖 ตั้งใจเรียนเพิ่มขึ้นอีกนิดนะ'}
              </p>
            </div>
            
            <!-- Footer -->
            <div class="mt-6 text-center">
              <p class="opacity-40" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;" id="school-display-result">${config.school_name || defaultConfig.school_name}</p>
              <p class="opacity-30 mt-1" style="color: ${textColor}; font-size: ${fontSize * 0.75}px;" id="teacher-display-result">${config.teacher_name || defaultConfig.teacher_name}</p>
            </div>
          </div>
        </div>
      `;
      
      document.getElementById('back-btn').addEventListener('click', () => {
        currentView = 'login';
        currentStudentId = null;
        renderLoginView();
      });
    }

    function render() {
      if (currentView === 'login') {
        renderLoginView();
      } else if (currentView === 'result') {
        renderResultView();
      }
    }

    async function initApp() {
      if (window.elementSdk) {
        window.elementSdk.init({
          defaultConfig,
          onConfigChange: async (newConfig) => {
            config = { ...newConfig };
            render();
          },
          mapToCapabilities: (cfg) => ({
            recolorables: [
              {
                get: () => cfg.background_color || defaultConfig.background_color,
                set: (value) => { cfg.background_color = value; window.elementSdk.setConfig({ background_color: value }); }
              },
              {
                get: () => cfg.card_color || defaultConfig.card_color,
                set: (value) => { cfg.card_color = value; window.elementSdk.setConfig({ card_color: value }); }
              },
              {
                get: () => cfg.text_color || defaultConfig.text_color,
                set: (value) => { cfg.text_color = value; window.elementSdk.setConfig({ text_color: value }); }
              },
              {
                get: () => cfg.primary_color || defaultConfig.primary_color,
                set: (value) => { cfg.primary_color = value; window.elementSdk.setConfig({ primary_color: value }); }
              },
              {
                get: () => cfg.accent_color || defaultConfig.accent_color,
                set: (value) => { cfg.accent_color = value; window.elementSdk.setConfig({ accent_color: value }); }
              }
            ],
            borderables: [],
            fontEditable: {
              get: () => cfg.font_family || defaultConfig.font_family,
              set: (value) => { cfg.font_family = value; window.elementSdk.setConfig({ font_family: value }); }
            },
            fontSizeable: {
              get: () => cfg.font_size || defaultConfig.font_size,
              set: (value) => { cfg.font_size = value; window.elementSdk.setConfig({ font_size: value }); }
            }
          }),
          mapToEditPanelValues: (cfg) => new Map([
            ["school_name", cfg.school_name || defaultConfig.school_name],
            ["class_name", cfg.class_name || defaultConfig.class_name],
            ["subject_name", cfg.subject_name || defaultConfig.subject_name],
            ["teacher_name", cfg.teacher_name || defaultConfig.teacher_name],
            ["lesson1_name", cfg.lesson1_name || defaultConfig.lesson1_name],
            ["lesson2_name", cfg.lesson2_name || defaultConfig.lesson2_name],
            ["lesson3_name", cfg.lesson3_name || defaultConfig.lesson3_name]
          ])
        });
      }
      render();
    }

    initApp();
  </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9c16552a31c374f2',t:'MTc2ODk5MzQ4Ni4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
