
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Study Portal</title>
    <style>
        :root {
            --primary-color: #007bff;
            --border-color: #ddd;
            --bg-color: #f8f9fa;
        }
        body {
            font-family: 'Malgun Gothic', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #fff;
        }
        
        /* 상단 네비게이션 바 */
        nav {
            background-color: var(--primary-color);
            color: white;
            display: flex;
            justify-content: space-around;
            padding: 15px 0;
            position: sticky;
            top: 0;
            z-index: 100;
        }
        nav div {
            cursor: pointer;
            font-weight: bold;
            font-size: 16px;
        }
        nav div:hover {
            text-decoration: underline;
        }
        
        /* 메인 컨텐츠 영역 */
        .content-section {
            display: none;
            padding: 20px;
            max-width: 800px;
            margin: 0 auto;
        }
        .active {
            display: block;
        }

        /* 메인 페이지 이미지 */
        .main-image-container {
            text-align: center;
        }
        .main-image-container img {
            max-width: 100%;
            height: auto;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
        }

        /* 다운로드 화면 */
        .file-list {
            list-style: none;
            padding: 0;
            margin-top: 20px;
        }
        .file-list li {
            padding: 15px;
            background: var(--bg-color);
            margin-bottom: 10px;
            border: 1px solid var(--border-color);
            border-radius: 5px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .download-btn {
            background-color: var(--primary-color);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 3px;
            cursor: pointer;
            font-weight: bold;
        }

        /* 정답 확인 화면 */
        .subject-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        .subject-tabs button {
            flex: 1;
            padding: 10px;
            border: 1px solid var(--primary-color);
            background-color: white;
            color: var(--primary-color);
            cursor: pointer;
            border-radius: 5px;
            font-weight: bold;
        }
        .subject-tabs button.active-sub {
            background-color: var(--primary-color);
            color: white;
        }
        
        .answer-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
        }
        .question-item {
            background: var(--bg-color);
            padding: 10px;
            border: 1px solid var(--border-color);
            border-radius: 5px;
            font-size: 14px;
        }
        .question-item input[type="text"] {
            width: 80%;
            padding: 5px;
            margin-top: 5px;
            border: 1px solid #ccc;
            border-radius: 3px;
        }
        .options {
            display: flex;
            gap: 5px;
            margin-top: 5px;
        }
        .options label {
            cursor: pointer;
        }

        .submit-btn-container {
            text-align: center;
            margin-top: 30px;
        }
        .submit-btn {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
        }

        /* 등급컷 화면 */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid var(--border-color);
            padding: 12px;
            text-align: center;
        }
        th {
            background-color: var(--primary-color);
            color: white;
        }
    </style>
</head>
<body>

    <nav>
        <div onclick="showSection('main')">홈</div>
        <div onclick="showSection('download')">문제 다운로드</div>
        <div onclick="showSection('answers')">정답확인</div>
        <div onclick="showSection('cutoffs')">등급컷확인</div>
    </nav>

    <div id="main" class="content-section active">
        <div class="main-image-container">
            <img src="IMG_6272.png" alt="메인 페이지 이미지">
        </div>
    </div>

    <div id="download" class="content-section">
        <h2>자료실</h2>
        <ul class="file-list">
            <li>
                <span>제1회 YES모의고사.pdf</span>
                <button class="download-btn">다운로드</button>
            </li>
        </ul>
    </div>

    <div id="answers" class="content-section">
        <h2>정답 입력</h2>
        <div class="subject-tabs">
            <button id="tab-korean" class="active-sub" onclick="showSubject('korean')">국어</button>
            <button id="tab-calc" onclick="showSubject('calc')">미적분</button>
            <button id="tab-stat" onclick="showSubject('stat')">확률과 통계</button>
        </div>

        <div id="subject-content" class="answer-grid">
            </div>

        <div class="submit-btn-container">
            <button class="submit-btn" onclick="gradeAnswers()">채점하기</button>
        </div>
    </div>

    <div id="cutoffs" class="content-section">
        <h2>예상 등급컷</h2>
        <table>
            <thead>
                <tr>
                    <th>등급</th>
                    <th>국어</th>
                    <th>미적분</th>
                    <th>확률과 통계</th>
                </tr>
            </thead>
            <tbody>
                <tr><td>1등급</td><td></td><td></td><td></td></tr>
                <tr><td>2등급</td><td></td><td></td><td></td></tr>
                <tr><td>3등급</td><td></td><td></td><td></td></tr>
            </tbody>
        </table>
    </div>

    <script>
        // =======================================================
        // [정답 설정 공간]
        // 채점을 위해 각 과목의 실제 정답을 아래에 쉼표(,)로 구분하여 입력하세요.
        // 현재 테스트를 위해 1번부터 몇 문제만 임의로 적혀있습니다. 
        // 실제 사용할 때는 모든 문제의 번호와 정답을 기입해야 정상적으로 100점 만점이 계산됩니다.
        // =======================================================
        const correctAnswers = {
            korean: {
                1: "1", 2: "3", 3: "5", 4: "2" 
            },
            calc: {
                1: "2", 2: "4", 3: "1", 16: "15", 30: "250"
            },
            stat: {
                1: "3", 2: "1", 3: "5", 16: "12", 30: "120"
            }
        };

        let currentSubject = 'korean'; 

        // 탭 전환 기능
        function showSection(sectionId) {
            const sections = document.querySelectorAll('.content-section');
            sections.forEach(sec => sec.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            
            if(sectionId === 'answers') {
                showSubject('korean');
            }
        }

        // 과목별 답안지 생성
        function showSubject(subject) {
            currentSubject = subject; 

            document.querySelectorAll('.subject-tabs button').forEach(btn => btn.classList.remove('active-sub'));
            if(subject === 'korean') document.getElementById('tab-korean').classList.add('active-sub');
            if(subject === 'calc') document.getElementById('tab-calc').classList.add('active-sub');
            if(subject === 'stat') document.getElementById('tab-stat').classList.add('active-sub');

            const container = document.getElementById('subject-content');
            let html = '';
            let prefix = subject === 'korean' ? 'kor' : subject;

            if (subject === 'korean') {
                for (let i = 1; i <= 45; i++) {
                    html += createObjectiveQuestion(i, prefix);
                }
            } else {
                const subjectiveNums = [16, 17, 18, 19, 20, 21, 22, 29, 30];
                for (let i = 1; i <= 30; i++) {
                    if (subjectiveNums.includes(i)) {
                        html += createSubjectiveQuestion(i, prefix);
                    } else {
                        html += createObjectiveQuestion(i, prefix);
                    }
                }
            }
            container.innerHTML = html;
        }

        function createObjectiveQuestion(num, prefix) {
            let options = '';
            for(let opt=1; opt<=5; opt++) {
                options += `<label><input type="radio" name="${prefix}_q${num}" value="${opt}"> ${opt}</label>`;
            }
            return `
                <div class="question-item">
                    <strong>${num}번</strong>
                    <div class="options">${options}</div>
                </div>
            `;
        }

        function createSubjectiveQuestion(num, prefix) {
            return `
                <div class="question-item">
                    <strong>${num}번 (주관식)</strong><br>
                    <input type="text" placeholder="정답 입력" name="${prefix}_q${num}">
                </div>
            `;
        }

        // 수학 과목 배점 계산 함수
        function getMathScore(qNum) {
            if (qNum <= 2) return 2;
            if (qNum <= 8) return 3;
            if (qNum <= 15) return 4;
            if (qNum <= 19) return 3;
            if (qNum <= 22) return 4;
            if (qNum === 23) return 2;
            if (qNum <= 27) return 3;
            if (qNum <= 30) return 4;
            return 0;
        }

        // 채점 및 결과 출력 기능
        function gradeAnswers() {
            let qCount = currentSubject === 'korean' ? 45 : 30;
            let prefix = currentSubject === 'korean' ? 'kor' : currentSubject;
            
            let correctCount = 0;
            let answeredCount = 0;
            let totalScore = 0;
            let wrongQuestions = []; // 틀린 문제 번호를 저장할 배열

            for (let i = 1; i <= qCount; i++) {
                let userAnswer = "";
                let isSubjective = (currentSubject !== 'korean') && [16, 17, 18, 19, 20, 21, 22, 29, 30].includes(i);
                
                if (isSubjective) {
                    let input = document.querySelector(`input[name="${prefix}_q${i}"]`);
                    if (input && input.value.trim() !== "") {
                        userAnswer = input.value.trim();
                        answeredCount++;
                    }
                } else {
                    let checked = document.querySelector(`input[name="${prefix}_q${i}"]:checked`);
                    if (checked) {
                        userAnswer = checked.value;
                        answeredCount++;
                    }
                }

                // 등록된 정답 가져오기 (정답이 등록되지 않은 문항도 오답으로 처리됨)
                let realAnswer = correctAnswers[currentSubject][i];
                
                if (realAnswer && userAnswer === String(realAnswer)) {
                    correctCount++;
                    if (currentSubject !== 'korean') {
                        totalScore += getMathScore(i); // 수학일 경우 점수 합산
                    }
                } else {
                    wrongQuestions.push(i); // 틀리거나 풀지 않은 문제는 오답 배열에 추가
                }
            }

            // 결과 안내창 띄우기
            let subjectName = currentSubject === 'korean' ? '국어' : (currentSubject === 'calc' ? '미적분' : '확률과 통계');
            let wrongListStr = wrongQuestions.length > 0 ? wrongQuestions.join(', ') + '번' : '전원 정답!';

            if (currentSubject === 'korean') {
                // 국어 결과
                alert(`[${subjectName} 채점 결과]\n- 푼 문제 수: ${answeredCount} / ${qCount}\n- 맞힌 문제 수: ${correctCount}개\n\n 틀린 문제:\n${wrongListStr}`);
            } else {
                // 수학(미적분, 확통) 결과
                alert(`[${subjectName} 채점 결과]\n- 푼 문제 수: ${answeredCount} / ${qCount}\n\n 틀린 문제:\n${wrongListStr}\n\n 최종 점수: ${totalScore}점 / 100점`);
            }
        }
    </script>
</body>
</html>
