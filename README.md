<html lang="th" class="h-full">
 <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ระบบตรวจสอบคะแนนนักเรียน</title>
  <script src="/_sdk/element_sdk.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
        body {
            box-sizing: border-box;
        }
        
        @import url('https://fonts.googleapis.com/css2?family=Sarabun:wght@400;500;600;700&display=swap');
        
        * {
            font-family: 'Sarabun', sans-serif;
        }
        
        .math-pattern {
            background-image: 
                linear-gradient(45deg, rgba(236, 72, 153, 0.05) 25%, transparent 25%),
                linear-gradient(-45deg, rgba(244, 114, 182, 0.05) 25%, transparent 25%),
                linear-gradient(45deg, transparent 75%, rgba(236, 72, 153, 0.05) 75%),
                linear-gradient(-45deg, transparent 75%, rgba(244, 114, 182, 0.05) 75%);
            background-size: 60px 60px;
            background-position: 0 0, 0 30px, 30px -30px, -30px 0px;
        }
        
        .score-card {
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }
        
        .score-card:hover {
            transform: translateY(-4px) rotate(1deg);
            box-shadow: 0 12px 24px rgba(0, 0, 0, 0.15);
        }
        
        .grade-excellent {
            background: linear-gradient(135deg, #10b981 0%, #059669 100%);
        }
        
        .grade-good {
            background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
        }
        
        .grade-fair {
            background: linear-gradient(135deg, #f59e0b 0%, #d97706 100%);
        }
        
        .grade-poor {
            background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
        }
        
        .input-field {
            transition: all 0.3s ease;
        }
        
        .input-field:focus {
            outline: none;
            box-shadow: 0 0 0 4px rgba(236, 72, 153, 0.2);
            transform: scale(1.02);
        }
        
        .search-button {
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .search-button:before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.3);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }
        
        .search-button:hover:before {
            width: 300px;
            height: 300px;
        }
        
        .search-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(236, 72, 153, 0.4);
        }
        
        .search-button:active {
            transform: translateY(0);
        }
        
        .math-icon {
            display: inline-block;
            animation: float 3s ease-in-out infinite;
        }
        
        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }
        
        .result-enter {
            animation: slideIn 0.5s ease-out;
        }
        
        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }
    </style>
  <style>@view-transition { navigation: auto; }</style>
  <script src="/_sdk/data_sdk.js" type="text/javascript"></script>
 </head>
 <body class="h-full">
  <div id="app" class="w-full h-full"></div>
  <script>
        const defaultConfig = {
            background_color: '#fce7f3',
            surface_color: '#ffffff',
            text_color: '#831843',
            primary_action_color: '#ec4899',
            secondary_action_color: '#f472b6',
            font_family: 'Sarabun',
            font_size: 16,
            app_title: 'ระบบตรวจสอบคะแนนวิชาคณิตศาสตร์',
            subtitle: 'ชั้นประถมศึกษาปีที่ 2/5 (MEP)',
            teacher_info: 'ครูผู้สอน นางวิรัลพัชษ์ สว่างเดือน',
            input_label_1: 'เลขประจำตัวนักเรียน (5 หลัก)',
            input_label_2: 'รหัสห้องเรียน',
            button_text: 'ดูคะแนนของฉัน ✨'
        };

        // ข้อมูลนักเรียน 30 คน
        const studentData = {
            '22321_205MEP68': {
                studentId: '22321',
                classCode: '205MEP68',
                name: 'เด็กชายภัทรพล แก่นกางหวาย',
                grade: 'ป.2/5',
                mathScore: 16,
                fullScore: 20
            },
            '22330_205MEP68': {
                studentId: '22330',
                classCode: '205MEP68',
                name: 'เด็กชายเอกศิษฎ์ ทองสกุล',
                grade: 'ป.2/5',
                mathScore: 15,
                fullScore: 20
            },
            '22335_205MEP68': {
                studentId: '22335',
                classCode: '205MEP68',
                name: 'เด็กชายภิญญาพัชญ์ เปรมสุข',
                grade: 'ป.2/5',
                mathScore: 2,
                fullScore: 20
            },
            '22337_205MEP68': {
                studentId: '22337',
                classCode: '205MEP68',
                name: 'เด็กชายเมธาสิทธิ์ คงนุมัติ',
                grade: 'ป.2/5',
                mathScore: 7,
                fullScore: 20
            },
            '22345_205MEP68': {
                studentId: '22345',
                classCode: '205MEP68',
                name: 'เด็กชายณัฎฐ์ตฤณ สุขประเสริฐ',
                grade: 'ป.2/5',
                mathScore: 17,
                fullScore: 20
            },
            '22364_205MEP68': {
                studentId: '22364',
                classCode: '205MEP68',
                name: 'เด็กชายณภัทร แช่มวงศ์',
                grade: 'ป.2/5',
                mathScore: 9,
                fullScore: 20
            },
            '22386_205MEP68': {
                studentId: '22386',
                classCode: '205MEP68',
                name: 'เด็กชายมีคุณ รัตนานนท์',
                grade: 'ป.2/5',
                mathScore: 16,
                fullScore: 20
            },
            '22795_205MEP68': {
                studentId: '22795',
                classCode: '205MEP68',
                name: 'เด็กชายวรปรัชญ์ กิจเปรมถาวร',
                grade: 'ป.2/5',
                mathScore: 11,
                fullScore: 20
            },
            '22799_205MEP68': {
                studentId: '22799',
                classCode: '205MEP68',
                name: 'เด็กชายอาชวิน เอื้อทยา',
                grade: 'ป.2/5',
                mathScore: 0,
                fullScore: 20
            },
            '23364_205MEP68': {
                studentId: '23364',
                classCode: '205MEP68',
                name: 'เด็กชายเป็นไทย แก้วแดง',
                grade: 'ป.2/5',
                mathScore: 16,
                fullScore: 20
            },
            '23365_205MEP68': {
                studentId: '23365',
                classCode: '205MEP68',
                name: 'เด็กชายรัตนพล เอมนฤมล',
                grade: 'ป.2/5',
                mathScore: 10,
                fullScore: 20
            },
            '23366_205MEP68': {
                studentId: '23366',
                classCode: '205MEP68',
                name: 'เด็กชายสุกฤษฎิ์ ขอเสริมกลาง',
                grade: 'ป.2/5',
                mathScore: 12,
                fullScore: 20
            },
            '23367_205MEP68': {
                studentId: '23367',
                classCode: '205MEP68',
                name: 'เด็กชายธนวัฒน์ พรไตรรัตน์',
                grade: 'ป.2/5',
                mathScore: 9,
                fullScore: 20
            },
            '23368_205MEP68': {
                studentId: '23368',
                classCode: '205MEP68',
                name: 'เด็กชายณัฐพนธ์ กันชะนะ',
                grade: 'ป.2/5',
                mathScore: 12,
                fullScore: 20
            },
            '23369_205MEP68': {
                studentId: '23369',
                classCode: '205MEP68',
                name: 'เด็กชายกมลภพ ขยายฤทธิ์',
                grade: 'ป.2/5',
                mathScore: 9,
                fullScore: 20
            },
            '22324_205MEP68': {
                studentId: '22324',
                classCode: '205MEP68',
                name: 'เด็กหญิงณัฐชนก พงษ์พิทักษ์วิเศษ',
                grade: 'ป.2/5',
                mathScore: 8,
                fullScore: 20
            },
            '22327_205MEP68': {
                studentId: '22327',
                classCode: '205MEP68',
                name: 'เด็กหญิงปวริศา กงฉิน',
                grade: 'ป.2/5',
                mathScore: 9,
                fullScore: 20
            },
            '22351_205MEP68': {
                studentId: '22351',
                classCode: '205MEP68',
                name: 'เด็กหญิงรดา อินเที่ยง',
                grade: 'ป.2/5',
                mathScore: 11,
                fullScore: 20
            },
            '22366_205MEP68': {
                studentId: '22366',
                classCode: '205MEP68',
                name: 'เด็กหญิงออมสิน พบสุข',
                grade: 'ป.2/5',
                mathScore: 4,
                fullScore: 20
            },
            '22370_205MEP68': {
                studentId: '22370',
                classCode: '205MEP68',
                name: 'เด็กหญิงปทิตตา ชัยพฤกษ์',
                grade: 'ป.2/5',
                mathScore: 12,
                fullScore: 20
            },
            '22798_205MEP68': {
                studentId: '22798',
                classCode: '205MEP68',
                name: 'เด็กหญิงรวินท์นิภา โตแทน',
                grade: 'ป.2/5',
                mathScore: 15,
                fullScore: 20
            },
            '23088_205MEP68': {
                studentId: '23088',
                classCode: '205MEP68',
                name: 'เด็กหญิงปวริศา กายจริต',
                grade: 'ป.2/5',
                mathScore: 4,
                fullScore: 20
            },
            '23370_205MEP68': {
                studentId: '23370',
                classCode: '205MEP68',
                name: 'เด็กหญิงธนันรดา ผาสุขถิน',
                grade: 'ป.2/5',
                mathScore: 11,
                fullScore: 20
            },
            '23371_205MEP68': {
                studentId: '23371',
                classCode: '205MEP68',
                name: 'เด็กหญิงปุญณดา คงสมจิตร์',
                grade: 'ป.2/5',
                mathScore: 17,
                fullScore: 20
            },
            '23372_205MEP68': {
                studentId: '23372',
                classCode: '205MEP68',
                name: 'เด็กหญิงอริยดา ฉ่ำชื่น',
                grade: 'ป.2/5',
                mathScore: 7,
                fullScore: 20
            },
            '23373_205MEP68': {
                studentId: '23373',
                classCode: '205MEP68',
                name: 'เด็กหญิงทักษิยนันท์ สุจริตพงษ์',
                grade: 'ป.2/5',
                mathScore: 14,
                fullScore: 20
            },
            '23374_205MEP68': {
                studentId: '23374',
                classCode: '205MEP68',
                name: 'เด็กหญิงวนัฏษญา แสนกือ',
                grade: 'ป.2/5',
                mathScore: 6,
                fullScore: 20
            },
            '23375_205MEP68': {
                studentId: '23375',
                classCode: '205MEP68',
                name: 'เด็กหญิงสุพิชญา วังหอม',
                grade: 'ป.2/5',
                mathScore: 16,
                fullScore: 20
            },
            '23376_205MEP68': {
                studentId: '23376',
                classCode: '205MEP68',
                name: 'เด็กหญิงปุญฐิตา เฉลิมโภชน์',
                grade: 'ป.2/5',
                mathScore: 7,
                fullScore: 20
            },
            '23377_205MEP68': {
                studentId: '23377',
                classCode: '205MEP68',
                name: 'เด็กหญิงณภัทร พุ่มพวง',
                grade: 'ป.2/5',
                mathScore: 2,
                fullScore: 20
            }
        };

        function getGradeClass(percentage) {
            if (percentage >= 80) return 'grade-excellent';
            if (percentage >= 70) return 'grade-good';
            if (percentage >= 60) return 'grade-fair';
            return 'grade-poor';
        }

        function calculatePercentage(score, fullScore) {
            return ((score / fullScore) * 100).toFixed(2);
        }

        function getEncouragementMessage(percentage) {
            const percent = parseFloat(percentage);
            if (percent >= 70) return '🌟 เก่งมาก! ทำได้ดีเลยนะคะ 🌟';
            if (percent >= 60) return '👏 ทำได้ดีมาก ใกล้เป้าหมายแล้วนะคะ 👏';
            if (percent >= 50) return '💪 เริ่มต้นได้ดี อย่าท้อนะคะ ความเก่งกำลังมา 💪';
            if (percent >= 30) return '📚 ค่อยๆ ฝึกนะคะ เดี๋ยวจะขึ้นค่ะ 📚';
            if (percent >= 15) return '✏️ เริ่มใหม่ได้เสมอ ฝึกทำโจทย์เยอะๆ นะคะ ✏️';
            return '💖 ฝึกทำโจทย์เยอะๆ นะคะ และตั้งใจให้มากขึ้นนะคะ 💖';
        }

        async function onConfigChange(config) {
            const baseSize = config.font_size || defaultConfig.font_size;
            const customFont = config.font_family || defaultConfig.font_family;
            const fontStack = `${customFont}, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif`;
            
            const backgroundColor = config.background_color || defaultConfig.background_color;
            const surfaceColor = config.surface_color || defaultConfig.surface_color;
            const textColor = config.text_color || defaultConfig.text_color;
            const primaryColor = config.primary_action_color || defaultConfig.primary_action_color;
            const secondaryColor = config.secondary_action_color || defaultConfig.secondary_action_color;
            
            const appTitle = config.app_title || defaultConfig.app_title;
            const subtitle = config.subtitle || defaultConfig.subtitle;
            const teacherInfo = config.teacher_info || defaultConfig.teacher_info;
            const inputLabel1 = config.input_label_1 || defaultConfig.input_label_1;
            const inputLabel2 = config.input_label_2 || defaultConfig.input_label_2;
            const buttonText = config.button_text || defaultConfig.button_text;

            const app = document.getElementById('app');
            app.style.backgroundColor = backgroundColor;
            app.style.fontFamily = fontStack;
            app.style.color = textColor;
            
            const titleElement = document.getElementById('main-title');
            if (titleElement) {
                titleElement.textContent = appTitle;
                titleElement.style.fontSize = `${baseSize * 1.875}px`;
                titleElement.style.fontFamily = fontStack;
                titleElement.style.color = textColor;
            }
            
            const subtitleElement = document.getElementById('main-subtitle');
            if (subtitleElement) {
                subtitleElement.textContent = subtitle;
                subtitleElement.style.fontSize = `${baseSize * 1.25}px`;
                subtitleElement.style.fontFamily = fontStack;
                subtitleElement.style.color = textColor;
            }
            
            const teacherElement = document.getElementById('teacher-info');
            if (teacherElement) {
                teacherElement.textContent = teacherInfo;
                teacherElement.style.fontSize = `${baseSize}px`;
                teacherElement.style.fontFamily = fontStack;
                teacherElement.style.color = textColor;
            }
            
            const label1Element = document.getElementById('input-label-1');
            if (label1Element) {
                label1Element.textContent = inputLabel1;
                label1Element.style.fontSize = `${baseSize * 0.875}px`;
                label1Element.style.fontFamily = fontStack;
                label1Element.style.color = textColor;
            }
            
            const label2Element = document.getElementById('input-label-2');
            if (label2Element) {
                label2Element.textContent = inputLabel2;
                label2Element.style.fontSize = `${baseSize * 0.875}px`;
                label2Element.style.fontFamily = fontStack;
                label2Element.style.color = textColor;
            }
            
            const input1Element = document.getElementById('student-id');
            if (input1Element) {
                input1Element.style.fontSize = `${baseSize}px`;
                input1Element.style.fontFamily = fontStack;
                input1Element.style.color = textColor;
                input1Element.style.backgroundColor = surfaceColor;
                input1Element.style.borderColor = secondaryColor;
            }
            
            const input2Element = document.getElementById('class-code');
            if (input2Element) {
                input2Element.style.fontSize = `${baseSize}px`;
                input2Element.style.fontFamily = fontStack;
                input2Element.style.color = textColor;
                input2Element.style.backgroundColor = surfaceColor;
                input2Element.style.borderColor = secondaryColor;
            }
            
            const buttonElement = document.getElementById('search-button');
            if (buttonElement) {
                buttonElement.textContent = buttonText;
                buttonElement.style.backgroundColor = primaryColor;
                buttonElement.style.fontSize = `${baseSize * 1.125}px`;
                buttonElement.style.fontFamily = fontStack;
            }
            
            const resultCard = document.getElementById('result-card');
            if (resultCard) {
                resultCard.style.backgroundColor = surfaceColor;
            }
            
            const studentName = document.getElementById('student-name');
            if (studentName) {
                studentName.style.fontSize = `${baseSize * 1.75}px`;
                studentName.style.fontFamily = fontStack;
                studentName.style.color = textColor;
            }
            
            const studentGrade = document.getElementById('student-grade');
            if (studentGrade) {
                studentGrade.style.fontSize = `${baseSize * 1.125}px`;
                studentGrade.style.fontFamily = fontStack;
                studentGrade.style.color = secondaryColor;
            }
            
            const scoresContainer = document.getElementById('scores-container');
            if (scoresContainer) {
                const subjectLabels = scoresContainer.querySelectorAll('.subject-name');
                subjectLabels.forEach(label => {
                    label.style.fontSize = `${baseSize * 1.25}px`;
                    label.style.fontFamily = fontStack;
                });
                
                const scoreValues = scoresContainer.querySelectorAll('.score-value');
                scoreValues.forEach(value => {
                    value.style.fontSize = `${baseSize * 3}px`;
                    value.style.fontFamily = fontStack;
                });
                
                const fullScores = scoresContainer.querySelectorAll('.full-score');
                fullScores.forEach(score => {
                    score.style.fontSize = `${baseSize * 1.5}px`;
                    score.style.fontFamily = fontStack;
                });
                
                const percentages = scoresContainer.querySelectorAll('.percentage-text');
                percentages.forEach(perc => {
                    perc.style.fontSize = `${baseSize * 1.125}px`;
                    perc.style.fontFamily = fontStack;
                });
            }
        }

        function searchStudent() {
            const studentId = document.getElementById('student-id').value.trim();
            const classCode = document.getElementById('class-code').value.trim();
            const resultDiv = document.getElementById('result');
            const errorDiv = document.getElementById('error-message');
            
            errorDiv.classList.add('hidden');
            resultDiv.classList.add('hidden');
            
            if (!studentId) {
                showError('กรุณากรอกเลขประจำตัวนักเรียน 🙏');
                return;
            }
            
            if (studentId.length !== 5) {
                showError('เลขประจำตัวนักเรียนต้องเป็น 5 หลักเท่านั้น 🔢');
                return;
            }
            
            if (!classCode) {
                showError('กรุณากรอกรหัสห้องเรียน 📝');
                return;
            }
            
            const searchKey = `${studentId}_${classCode}`;
            const student = studentData[searchKey];
            
            if (!student) {
                showError('❌ ไม่พบข้อมูล กรุณาตรวจสอบเลขประจำตัวและรหัสห้องเรียนอีกครั้ง');
                return;
            }
            
            displayResults(student);
        }
        
        function showError(message) {
            const errorDiv = document.getElementById('error-message');
            errorDiv.textContent = message;
            errorDiv.classList.remove('hidden');
        }
        
        function displayResults(student) {
            const percentage = calculatePercentage(student.mathScore, student.fullScore);
            
            document.getElementById('student-name').textContent = student.name;
            document.getElementById('student-grade').textContent = `ชั้น ${student.grade} 🎓`;
            
            const scoresContainer = document.getElementById('scores-container');
            scoresContainer.innerHTML = '';
            
            const scoreCard = document.createElement('div');
            scoreCard.className = `score-card rounded-3xl p-8 text-white ${getGradeClass(parseFloat(percentage))}`;
            scoreCard.innerHTML = `
                <div class="text-center">
                    <div class="subject-name font-semibold mb-6">📐 คณิตศาสตร์<br>บทที่ 7 เรื่อง เวลา ⏰</div>
                    <div class="flex items-baseline justify-center gap-3 mb-4">
                        <div class="score-value font-bold">${student.mathScore}</div>
                        <div class="full-score opacity-90">/ ${student.fullScore}</div>
                    </div>
                    <div class="percentage-text font-semibold bg-white bg-opacity-20 rounded-full px-6 py-2 inline-block">
                        คิดเป็น ${percentage}%
                    </div>
                </div>
            `;
            scoresContainer.appendChild(scoreCard);
            
            // อัพเดทข้อความกำลังใจ
            const encouragementElement = document.getElementById('encouragement-message');
            if (encouragementElement) {
                encouragementElement.textContent = getEncouragementMessage(percentage);
            }
            
            const resultDiv = document.getElementById('result');
            resultDiv.classList.remove('hidden');
            resultDiv.classList.add('result-enter');
            
            onConfigChange(window.elementSdk ? window.elementSdk.config : defaultConfig);
        }

        function init() {
            const app = document.getElementById('app');
            
            app.innerHTML = `
                <main class="w-full h-full overflow-auto math-pattern">
                    <div class="min-h-full flex items-center justify-center p-6">
                        <div class="w-full max-w-4xl">
                            <header class="text-center mb-10">
                                <div class="math-icon text-6xl mb-4">🎯📐✏️</div>
                                <h1 id="main-title" class="font-bold mb-2">ระบบตรวจสอบคะแนนวิชาคณิตศาสตร์</h1>
                                <p id="main-subtitle" class="font-semibold mb-1">ชั้นประถมศึกษาปีที่ 2/5 (MEP)</p>
                                <p id="teacher-info" class="opacity-80">ครูผู้สอน นางวิรัลพัชษ์ สว่างเดือน</p>
                            </header>
                            
                            <div class="bg-white rounded-3xl shadow-2xl p-8 mb-8 border-4 border-pink-300">
                                <form id="search-form" class="max-w-lg mx-auto">
                                    <div class="mb-6">
                                        <label id="input-label-1" for="student-id" class="block font-semibold mb-2">
                                            เลขประจำตัวนักเรียน (5 หลัก)
                                        </label>
                                        <input 
                                            type="text" 
                                            id="student-id" 
                                            class="input-field w-full px-5 py-4 border-3 border-pink-300 rounded-xl"
                                            placeholder="เช่น 22321"
                                            maxlength="5"
                                        >
                                    </div>
                                    
                                    <div class="mb-6">
                                        <label id="input-label-2" for="class-code" class="block font-semibold mb-2">
                                            รหัสห้องเรียน
                                        </label>
                                        <input 
                                            type="text" 
                                            id="class-code" 
                                            class="input-field w-full px-5 py-4 border-3 border-pink-300 rounded-xl"
                                            placeholder="เช่น 205MEP68"
                                        >
                                    </div>
                                    
                                    <button 
                                        type="submit" 
                                        id="search-button"
                                        class="search-button w-full px-8 py-4 text-white font-bold rounded-xl relative"
                                    >
                                        ดูคะแนนของฉัน ✨
                                    </button>
                                </form>
                                
                                <div id="error-message" class="hidden mt-6 p-5 bg-red-50 border-3 border-red-300 text-red-700 rounded-xl text-center font-medium"></div>
                            </div>
                            
                            <div id="result" class="hidden">
                                <div id="result-card" class="rounded-3xl shadow-2xl p-8 mb-6 border-4 border-pink-300">
                                    <div class="text-center mb-8">
                                        <h2 id="student-name" class="font-bold mb-2">-</h2>
                                        <p id="student-grade" class="font-medium">-</p>
                                    </div>
                                    
                                    <div id="scores-container" class="grid grid-cols-1 gap-6 max-w-xl mx-auto">
                                    </div>
                                </div>
                                
                                <div class="text-center bg-white rounded-2xl p-6 border-3 border-pink-200">
                                    <p id="encouragement-message" class="text-lg font-medium">🌟 เก่งมาก! ทำได้ดีเลยนะคะ 🌟</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </main>
            `;
            
            document.getElementById('search-form').addEventListener('submit', (e) => {
                e.preventDefault();
                searchStudent();
            });
            
            onConfigChange(window.elementSdk ? window.elementSdk.config : defaultConfig);
        }

        if (window.elementSdk) {
            window.elementSdk.init({
                defaultConfig,
                onConfigChange,
                mapToCapabilities: (config) => ({
                    recolorables: [
                        {
                            get: () => config.background_color || defaultConfig.background_color,
                            set: (value) => {
                                config.background_color = value;
                                window.elementSdk.setConfig({ background_color: value });
                            }
                        },
                        {
                            get: () => config.surface_color || defaultConfig.surface_color,
                            set: (value) => {
                                config.surface_color = value;
                                window.elementSdk.setConfig({ surface_color: value });
                            }
                        },
                        {
                            get: () => config.text_color || defaultConfig.text_color,
                            set: (value) => {
                                config.text_color = value;
                                window.elementSdk.setConfig({ text_color: value });
                            }
                        },
                        {
                            get: () => config.primary_action_color || defaultConfig.primary_action_color,
                            set: (value) => {
                                config.primary_action_color = value;
                                window.elementSdk.setConfig({ primary_action_color: value });
                            }
                        },
                        {
                            get: () => config.secondary_action_color || defaultConfig.secondary_action_color,
                            set: (value) => {
                                config.secondary_action_color = value;
                                window.elementSdk.setConfig({ secondary_action_color: value });
                            }
                        }
                    ],
                    borderables: [],
                    fontEditable: {
                        get: () => config.font_family || defaultConfig.font_family,
                        set: (value) => {
                            config.font_family = value;
                            window.elementSdk.setConfig({ font_family: value });
                        }
                    },
                    fontSizeable: {
                        get: () => config.font_size || defaultConfig.font_size,
                        set: (value) => {
                            config.font_size = value;
                            window.elementSdk.setConfig({ font_size: value });
                        }
                    }
                }),
                mapToEditPanelValues: (config) => new Map([
                    ['app_title', config.app_title || defaultConfig.app_title],
                    ['subtitle', config.subtitle || defaultConfig.subtitle],
                    ['teacher_info', config.teacher_info || defaultConfig.teacher_info],
                    ['input_label_1', config.input_label_1 || defaultConfig.input_label_1],
                    ['input_label_2', config.input_label_2 || defaultConfig.input_label_2],
                    ['button_text', config.button_text || defaultConfig.button_text]
                ])
            });
        }

        if (document.readyState === 'loading') {
            document.addEventListener('DOMContentLoaded', init);
        } else {
            init();
        }
    </script>
 <script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9b3dc9e397a18945',t:'MTc2NjcyMjg1Ny4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
