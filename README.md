<!DOCTYPE html>
<html>
<head>
    <title>IT-keywords知识小问答</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Arial', sans-serif;
        }

        body {
            background-color: #e8f5e9;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .quiz-container {
            max-width: 600px;
            width: 100%;
            background-color: white;
            border-radius: 15px;
            padding: 25px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .title {
            text-align: center;
            color: #2e7d32;
            margin-bottom: 20px;
            font-size: 24px;
            font-weight: bold;
        }

        .header {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            background-color: #c8e6c9;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            color: #2e7d32;
        }

        .question-container {
            background-color: #f1f8e9;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
        }

        #question {
            color: #1b5e20;
            margin-bottom: 15px;
            font-size: 18px;
        }

        .options button {
            display: block;
            width: 100%;
            padding: 12px;
            margin: 10px 0;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            background-color: #81c784;
            color: white;
            font-size: 16px;
            transition: all 0.3s ease;
        }

        .options button:hover {
            background-color: #66bb6a;
            transform: translateY(-2px);
        }

        .options button:disabled {
            background-color: #c8c8c8;
            cursor: not-allowed;
            transform: none;
        }

        .result {
            text-align: center;
            margin-top: 20px;
            font-weight: bold;
            padding: 15px;
            border-radius: 8px;
            transition: all 0.3s ease;
        }

        .progress-bar {
            width: 100%;
            height: 10px;
            background-color: #e0e0e0;
            border-radius: 5px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background-color: #4caf50;
            width: 0%;
            transition: width 0.3s ease;
        }

        @media (max-width: 768px) {
            .quiz-container {
                padding: 15px;
            }
            
            .options button {
                font-size: 16px;
                padding: 15px;
            }
        }

        .end-message {
            text-align: center;
            color: #2e7d32;
        }

        .end-message h2 {
            margin-bottom: 15px;
        }

        .end-message p {
            margin: 10px 0;
            font-size: 18px;
        }

        .gift-message {
            color: #1b5e20;
            font-weight: bold;
            font-size: 20px;
            margin-top: 20px;
            padding: 15px;
            background-color: #c8e6c9;
            border-radius: 8px;
        }
    </style>
</head>
<body>
    <div class="quiz-container">
        <div class="title">IT-keywords知识小问答</div>
        <div class="progress-bar">
            <div class="progress" id="progress"></div>
        </div>
        <div class="header">
            <div class="score">得分：<span id="score">0</span>/5</div>
            <div class="timer">剩余时间：<span id="time">300</span>秒</div>
        </div>
        <div class="question-container">
            <h2 id="question"></h2>
            <div class="options" id="options"></div>
        </div>
        <div id="result" class="result"></div>
    </div>

    <script>
        const questions = [
            {
                question: "什么是API？",
                options: [
                    "应用程序接口（Application Programming Interface）",
                    "自动程序识别（Automatic Program Identification）",
                    "高级程序集成（Advanced Program Integration）",
                    "应用程序标识（Application Program Identity）"
                ],
                correct: 0
            },
            {
                question: "什么是Docker？",
                options: [
                    "一种编程语言",
                    "容器化平台",
                    "数据库管理系统",
                    "网络协议"
                ],
                correct: 1
            },
            {
                question: "什么是Git？",
                options: [
                    "图形处理软件",
                    "操作系统",
                    "分布式版本控制系统",
                    "网络浏览器"
                ],
                correct: 2
            },
            {
                question: "什么是REST API？",
                options: [
                    "表述性状态传递接口",
                    "远程数据存储",
                    "实时同步技术",
                    "程序测试工具"
                ],
                correct: 0
            },
            {
                question: "什么是DevOps？",
                options: [
                    "开发工具集",
                    "开发与运维的协作文化",
                    "部署系统",
                    "代码管理软件"
                ],
                correct: 1
            }
        ];

        let currentQuestion = 0;
        let score = 0;
        let timeLeft = 300;
        let answeredQuestions = new Set();

        function startQuiz() {
            showQuestion();
            startTimer();
            updateProgress();
        }

        function showQuestion() {
            const question = questions[currentQuestion];
            document.getElementById('question').textContent = `问题 ${currentQuestion + 1}/5: ${question.question}`;
            
            const optionsContainer = document.getElementById('options');
            optionsContainer.innerHTML = '';
            
            question.options.forEach((option, index) => {
                const button = document.createElement('button');
                button.textContent = option;
                button.onclick = () => checkAnswer(index);
                if (answeredQuestions.has(currentQuestion)) {
                    button.disabled = true;
                }
                optionsContainer.appendChild(button);
            });
        }

        function updateProgress() {
            const progress = (currentQuestion / questions.length) * 100;
            document.getElementById('progress').style.width = `${progress}%`;
        }

        function checkAnswer(selectedOption) {
            if (answeredQuestions.has(currentQuestion)) {
                return;
            }

            const question = questions[currentQuestion];
            const buttons = document.querySelectorAll('.options button');
            buttons.forEach(button => button.disabled = true);
            answeredQuestions.add(currentQuestion);

            if (selectedOption === question.correct) {
                score++;
                document.getElementById('score').textContent = score;
                showResult(true);
            } else {
                showResult(false);
            }
            
            setTimeout(() => {
                currentQuestion++;
                if (currentQuestion < questions.length) {
                    showQuestion();
                    updateProgress();
                } else {
                    endQuiz();
                }
            }, 1000);
        }

        function showResult(isCorrect) {
            const resultDiv = document.getElementById('result');
            resultDiv.textContent = isCorrect ? "✅ 回答正确！" : "❌ 回答错误！";
            resultDiv.style.color = isCorrect ? "#2e7d32" : "#c62828";
            resultDiv.style.backgroundColor = isCorrect ? "#c8e6c9" : "#ffcdd2";
        }

        function startTimer() {
            const timer = setInterval(() => {
                timeLeft--;
                document.getElementById('time').textContent = timeLeft;
                
                if (timeLeft <= 0) {
                    clearInterval(timer);
                    endQuiz();
                }
            }, 1000);
        }

        function endQuiz() {
            const container = document.querySelector('.quiz-container');
            container.innerHTML = `
                <div class="end-message">
                    <h2>测试结束！</h2>
                    <p>您的得分：${score}/5</p>
                    <div class="gift-message">
                        ${score === 5 ? 
                        '🎉 恭喜您获得礼品！请到前台领取。' : 
                        '💪 继续加油！很遗憾未能获得礼品。'}
                    </div>
                </div>
            `;
        }

        window.onload = startQuiz;
    </script>
</body>
</html>
