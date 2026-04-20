<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hệ Thống Học Tiếng Anh Chuyên Sâu</title>
    <link href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;800&display=swap" rel="stylesheet">
    <style>
        :root {
            --primary: #4361ee;
            --secondary: #3f37c9;
            --success: #4cc9f0;
            --correct: #2b9348;
            --wrong: #d90429;
            --bg: #f8f9fa;
            --card-bg: #ffffff;
            --text-main: #2b2d42;
            --text-muted: #8d99ae;
        }

        body {
            font-family: 'Nunito', sans-serif;
            background-color: var(--bg);
            color: var(--text-main);
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            line-height: 1.6;
        }

        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            width: 100%;
            padding: 30px 0;
            text-align: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        header h1 { margin: 0; font-size: 2.5rem; letter-spacing: 1px; }
        header p { margin: 10px 0 0 0; font-size: 1.2rem; opacity: 0.9; }

        .container {
            width: 90%;
            max-width: 900px;
            margin-top: 30px;
        }

        .level-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            overflow-x: auto;
            padding-bottom: 10px;
        }

        .tab {
            flex: 1;
            text-align: center;
            padding: 15px 20px;
            background-color: var(--card-bg);
            border-radius: 12px;
            cursor: pointer;
            font-weight: 600;
            font-size: 1.1rem;
            color: var(--text-muted);
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            transition: all 0.3s ease;
            white-space: nowrap;
        }

        .tab:hover { transform: translateY(-3px); box-shadow: 0 5px 15px rgba(0,0,0,0.1); }
        .tab.active { background-color: var(--primary); color: white; }

        .card {
            background: var(--card-bg);
            padding: 40px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.08);
        }

        .question-title { font-size: 1.5rem; font-weight: 800; margin-bottom: 25px; color: var(--text-main); }

        .options-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }

        .option-btn {
            background-color: var(--bg);
            border: 2px solid #e9ecef;
            padding: 15px;
            border-radius: 10px;
            font-size: 1.1rem;
            font-family: 'Nunito', sans-serif;
            font-weight: 600;
            color: var(--text-main);
            cursor: pointer;
            transition: all 0.2s;
            text-align: left;
        }

        .option-btn:hover { border-color: var(--primary); background-color: #f0f4ff; }
        .option-btn:disabled { cursor: not-allowed; opacity: 0.8; }
        
        .option-btn.correct-ans { background-color: #d8f3dc; border-color: var(--correct); color: var(--correct); }
        .option-btn.wrong-ans { background-color: #ffe5d9; border-color: var(--wrong); color: var(--wrong); }

        .explanation-box {
            margin-top: 25px;
            padding: 20px;
            border-radius: 10px;
            display: none;
            animation: fadeIn 0.5s;
        }

        .explanation-box.success { background-color: #d8f3dc; border-left: 5px solid var(--correct); }
        .explanation-box.error { background-color: #ffe5d9; border-left: 5px solid var(--wrong); }
        .explanation-title { font-weight: 800; font-size: 1.2rem; margin-bottom: 10px; }

        .next-btn {
            margin-top: 20px;
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 12px 25px;
            font-size: 1.1rem;
            font-weight: 600;
            border-radius: 8px;
            cursor: pointer;
            display: none;
            font-family: 'Nunito', sans-serif;
        }
        .next-btn:hover { background-color: var(--secondary); }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(-10px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Responsive cho điện thoại */
        @media (max-width: 600px) {
            .options-grid { grid-template-columns: 1fr; }
            .card { padding: 20px; }
        }
    </style>
</head>
<body>

    <header>
        <h1>Master English ✨</h1>
        <p>Hệ thống bài tập kèm lời giải chi tiết</p>
    </header>

    <div class="container">
        <div class="level-tabs">
            <div class="tab active" onclick="loadLevel('A1')">A1 (Mất gốc)</div>
            <div class="tab" onclick="loadLevel('B1')">B1 (Trung cấp)</div>
            <div class="tab" onclick="loadLevel('C1')">C1 (IELTS 8.0)</div>
        </div>

        <div class="card" id="quiz-area">
            <div class="question-title" id="question-text">Đang tải câu hỏi...</div>
            <div class="options-grid" id="options-area">
                </div>

            <div class="explanation-box" id="explanation-area">
                <div class="explanation-title" id="exp-title">Kết quả</div>
                <div id="exp-content">Chi tiết lời giải...</div>
            </div>

            <button class="next-btn" id="next-btn" onclick="nextQuestion()">Câu tiếp theo ➔</button>
        </div>
    </div>

    <script>
        // CẤU TRÚC DỮ LIỆU CÓ THỂ MỞ RỘNG LÊN 1000+ BÀI
        // Bạn chỉ cần copy-paste và thêm câu hỏi vào các mảng (Array) bên dưới
        const database = {
            'A1': [
                { 
                    q: "Chọn từ đúng: 'She ___ my best friend.'", 
                    options: ["am", "is", "are", "be"], 
                    answer: "is",
                    explanation: "Với chủ ngữ ngôi thứ 3 số ít (He, She, It), động từ 'to be' ở hiện tại đơn luôn được chia là 'is'."
                },
                { 
                    q: "Từ vựng nào chỉ 'Quả dưa hấu' trong tiếng Anh?", 
                    options: ["Melon", "Watermelon", "Pineapple", "Coconut"], 
                    answer: "Watermelon",
                    explanation: "Watermelon /'wɔ:tə,melən/ (Danh từ): Quả dưa hấu. <br>Các từ khác: Melon (dưa lưới), Pineapple (quả dứa/thơm), Coconut (quả dừa)."
                }
            ],
            'B1': [
                { 
                    q: "Hoàn thành câu: 'If it rains tomorrow, we ___ at home.'", 
                    options: ["will stay", "would stay", "stayed", "stay"], 
                    answer: "will stay",
                    explanation: "Đây là câu điều kiện loại 1 (sự việc có thể xảy ra ở hiện tại hoặc tương lai). <br><b>Cấu trúc:</b> If + S + V(hiện tại đơn), S + will + V(nguyên mẫu)."
                }
            ],
            'C1': [
                { 
                    q: "Chọn từ đồng nghĩa (Synonym) cao cấp cho từ 'Important':", 
                    options: ["Good", "Crucial", "Nice", "Okay"], 
                    answer: "Crucial",
                    explanation: "<b>Crucial</b> /'kru:ʃl/ mang ý nghĩa 'vô cùng quan trọng, mang tính quyết định' (extremely important). Trong IELTS Writing/Speaking, dùng 'crucial', 'vital' hoặc 'paramount' sẽ giúp bạn đạt điểm Từ vựng (Lexical Resource) cao hơn rất nhiều so với 'important'."
                }
            ]
        };

        let currentLevel = 'A1';
        let currentIndex = 0;

        function loadLevel(level) {
            // Đổi giao diện Tab
            document.querySelectorAll('.tab').forEach(el => el.classList.remove('active'));
            event.currentTarget.classList.add('active');

            // Cập nhật dữ liệu
            currentLevel = level;
            currentIndex = 0;
            renderQuestion();
        }

        function renderQuestion() {
            const dataList = database[currentLevel];
            
            // Xử lý khi hết câu hỏi trong level
            if(currentIndex >= dataList.length) {
                document.getElementById('question-text').innerHTML = "🎉 Tuyệt vời! Bạn đã hoàn thành cấp độ này.";
                document.getElementById('options-area').innerHTML = "";
                document.getElementById('explanation-area').style.display = "none";
                document.getElementById('next-btn').style.display = "none";
                return;
            }

            const currentQ = dataList[currentIndex];
            document.getElementById('question-text').innerText = `Câu ${currentIndex + 1}: ${currentQ.q}`;
            
            // Render nút đáp án
            const optionsArea = document.getElementById('options-area');
            optionsArea.innerHTML = ""; 
            
            currentQ.options.forEach(opt => {
                const btn = document.createElement('button');
                btn.className = "option-btn";
                btn.innerText = opt;
                btn.onclick = (e) => checkAnswer(opt, e.target, currentQ);
                optionsArea.appendChild(btn);
            });

            // Ẩn lời giải và nút Next
            document.getElementById('explanation-area').style.display = "none";
            document.getElementById('next-btn').style.display = "none";
        }

        function checkAnswer(selectedOpt, btnElement, questionData) {
            // Khóa tất cả các nút sau khi chọn
            document.querySelectorAll('.option-btn').forEach(btn => btn.disabled = true);
            
            const expArea = document.getElementById('explanation-area');
            const expTitle = document.getElementById('exp-title');
            const expContent = document.getElementById('exp-content');

            expArea.style.display = "block";
            expContent.innerHTML = questionData.explanation; // Hỗ trợ in đậm/xuống dòng HTML trong lời giải

            if (selectedOpt === questionData.answer) {
                btnElement.classList.add('correct-ans');
                expArea.className = "explanation-box success";
                expTitle.innerText = "✅ Chính xác!";
                expTitle.style.color = "var(--correct)";
            } else {
                btnElement.classList.add('wrong-ans');
                // Đánh dấu nút đúng cho người dùng thấy
                document.querySelectorAll('.option-btn').forEach(btn => {
                    if(btn.innerText === questionData.answer) btn.classList.add('correct-ans');
                });

                expArea.className = "explanation-box error";
                expTitle.innerText = "❌ Chưa đúng rồi!";
                expTitle.style.color = "var(--wrong)";
            }

            document.getElementById('next-btn').style.display = "block";
        }

        function nextQuestion() {
            currentIndex++;
            renderQuestion();
        }

        // Tải câu hỏi đầu tiên khi mở web
        window.onload = renderQuestion;
    </script>
</body>
</html>
