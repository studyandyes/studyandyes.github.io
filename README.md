<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title> YES 모의고사 </title>
    <style>
        /* 기본 스타일 리셋 및 폰트 설정 */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Malgun Gothic', sans-serif;
        }

        body {
            background-color: #f5f7fa;
            color: #333;
            padding: 20px;
            display: flex;
            justify-content: center;
        }

        .container {
            max-width: 700px;
            width: 100%;
            background-color: #ffffff;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
        }

        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 20px;
        }

        /* 다운로드 섹션 스타일 */
        .download-box {
            background-color: #eef2f7;
            padding: 15px;
            border-radius: 8px;
            text-align: center;
            margin-bottom: 30px;
            border: 1px solid #d1d8e0;
        }

        .download-btn {
            display: inline-block;
            background-color: #3498db;
            color: white;
            padding: 10px 20px;
            text-decoration: none;
            border-radius: 5px;
            font-weight: bold;
            margin-top: 10px;
            transition: background 0.3s;
        }

        .download-btn:hover {
            background-color: #2980b9;
        }

        /* OMR 답안지 스타일 */
        .omr-title {
            font-size: 1.2rem;
            font-weight: bold;
            margin-bottom: 15px;
            border-bottom: 2px solid #2c3e50;
            padding-bottom: 5px;
        }

        .question-row {
            display: flex;
            align-items: center;
            padding: 12px 5px;
            border-bottom: 1px solid #edf2f7;
        }

        .q-number {
            width: 50px;
            font-weight: bold;
            color: #4a5568;
        }

        .options {
            display: flex;
            gap: 15px;
        }

        .options label {
            display: flex;
            align-items: center;
            gap: 5px;
            cursor: pointer;
        }

        /* 제출 버튼 */
        .submit-btn {
            display: block;
            width: 100%;
            background-color: #2ecc71;
            color: white;
            border: none;
            padding: 15px;
            font-size: 1.1rem;
            font-weight: bold;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 30px;
            transition: background 0.3s;
        }

        .submit-btn:hover {
            background-color: #27ae60;
        }

        /* 결과 창 스타일 */
        .result-box {
            display: none; /* 초기에는 숨김 */
            margin-top: 30px;
            padding: 20px;
            background-color: #fff;
            border: 2px solid #2cc71;
            border-radius: 8px;
        }

        .score-text {
            font-size: 1.5rem;
            font-weight: bold;
            text-align: center;
            color: #e74c3c;
            margin-bottom: 20px;
        }

        .review-list {
            margin-top: 15px;
        }

        .review-item {
            padding: 15px;
            margin-bottom: 10px;
            border-radius: 6px;
            border-left: 5px solid;
        }

        .correct-item {
            background-color: #f0fff4;
            border-left-color: #38a169;
        }

        .incorrect-item {
            background-color: #fff5f5;
            border-left-color: #e53e3e;
        }

        .explanation {
            margin-top: 5px;
            font-size: 0.9rem;
            color: #4a5568;
            background: #fff;
            padding: 8px;
            border-radius: 4px;
            border: 1px dashed #cbd5e0;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>📝 모의고사 채점 센터</h1>

    <div class="download-box">
        <p>먼저 아래 버튼을 눌러 모의고사 시험지(PDF)를 다운로드 받아 풀어보세요!</p>
        <a href="exam.pdf" download class="download-btn">📄 시험지 다운로드 (.pdf)</a>
    </div>

    <div class="omr-card">
        <div class="omr-title">OMR 답안 마킹</div>
        <form id="quizForm">
            <div id="omrContainer"></div>
            
            <button type="button" class="submit-btn" onclick="gradeExam()">📋 답안지 제출 및 채점하기</button>
        </form>
    </div>

    <div id="resultBox" class="result-box">
        <div class="score-text" id="scoreDisplay"></div>
        <div class="omr-title">🔎 문항별 정답 및 해설확인</div>
        <div class="review-list" id="reviewContainer"></div>
    </div>
</div>

<script>
    /**
     * ⚙️ [출제자 설정 구역] 
     * 문제 개수를 늘리거나 정답/해설을 바꾸려면 아래 배열 안의 데이터를 수정/추가해줘!
     * id: 문제번호, correct: 정답번호, score: 배점, comment: 해설내용
     */
    const examData = [
        { id: 1, correct: "1", score: 2},
        { id: 2, correct: "3", score: 2},
        { id: 3, correct: "4", score: 3},
        { id: 4, correct: "2", score: 20, comment: "4번 문항 해설: 보기 2번은 인과관계가 반대로 설명되어 틀린 보기입니다." },
        { id: 5, correct: "5", score: 20, comment: "5번 문항 해설: 종합적인 흐름상 결론으로 가장 적절한 것은 5번입니다." }
    ];

    // 페이지 로드 시 OMR 카드 자동 생성
    window.onload = function() {
        const container = document.getElementById('omrContainer');
        examData.forEach(q => {
            let row = document.createElement('div');
            row.className = 'question-row';
            
            let qNum = document.createElement('div');
            qNum.className = 'q-number';
            qNum.innerText = `${q.id}번.`;
            row.appendChild(qNum);

            let optionsDiv = document.createElement('div');
            optionsDiv.className = 'options';

            // 1번부터 5번 선택지 생성
            for (let i = 1; i <= 5; i++) {
                let label = document.createElement('label');
                label.innerHTML = `<input type="radio" name="q${q.id}" value="${i}"> ${i}`;
                optionsDiv.appendChild(label);
            }
            row.appendChild(optionsDiv);
            container.appendChild(row);
        });
    };

    // 채점 시스템 작동 함수
    function gradeExam() {
        let totalScore = 0;
        let maxScore = 0;
        const form = document.getElementById('quizForm');
        const reviewContainer = document.getElementById('reviewContainer');
        reviewContainer.innerHTML = ''; // 기존 결과 초기화

        examData.forEach(q => {
            maxScore += q.score;
            // 유저가 선택한 값 가져오기
            const selectedOpt = form.querySelector(`input[name="q${q.id}"]:checked`);
            const userAns = selectedOpt ? selectedOpt.value : "미표기";
            
            let isCorrect = (userAns === q.correct);
            if (isCorrect) {
                totalScore += q.score;
            }

            // 결과 상세 항목 HTML 생성
            let item = document.createElement('div');
            item.className = `review-item ${isCorrect ? 'correct-item' : 'incorrect-item'}`;
            
            item.innerHTML = `
                <strong>${q.id}번 문제</strong> — ${isCorrect ? '⭕ 정답입니다.' : '❌ 틀렸습니다.'}<br>
                <span style="font-size:0.95rem;">내 선택: <b style="color:${isCorrect?'#27ae60':'#e74c3c'}">${userAns}</b> | 실제정답: <b>${q.correct}</b></span>
                <div class="explanation">💡 ${q.comment}</div>
            `;
            reviewContainer.appendChild(item);
        });

        // 총점 표시 및 결과창 활성화
        document.getElementById('scoreDisplay').innerText = `총점: ${totalScore}점 / ${maxScore}점`;
        document.getElementById('resultBox').style.display = 'block';
        
        // 결과 화면으로 부드럽게 스크롤 이동
        document.getElementById('resultBox').scrollIntoView({ behavior: 'smooth' });
    }
</script>

</body>
</html>
