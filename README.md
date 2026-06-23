<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Study Portal</title>
    <style>
        :root {
            --primary-color: #1e40af; 
            --border-color: #e5e7eb; 
            --bg-color: #fdfbf6; 
        }
        body {
            font-family: 'Malgun Gothic', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #fafaf8;
            color: #1f2937; 
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
            box-shadow: 0 1px 3px rgba(0,0,0,0.1);
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
            /* 부드러운 그림자 */
            box-shadow: 0 4px 6px rgba(0,0,0,0.05); 
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
            text-decoration: none;
            display: inline-block;
        }
        .download-btn:hover {
            background-color: #111827; 
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

        /*  출제진 화면 */
        .examiner-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        .examiner-card {
            background: white;
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            transition: transform 0.2s ease-in-out;
        }
        .examiner-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        .examiner-name {
            font-size: 18px;
            font-weight: bold;
            color: var(--primary-color);
            margin-bottom: 5px;
        }
        .examiner-role {
            font-size: 14px;
            color: #4b5563;
            font-weight: bold;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px dashed var(--border-color);
        }
        .examiner-desc {
            font-size: 13px;
            color: #6b7280;
            line-height: 1.6;
            text-align: left;
            word-break: keep-all;
        }
    </style>
</head>
<body>

    <nav>
        <div onclick="showSection('main')">홈</div>
        <div onclick="showSection('download')">문제 다운로드</div>
        <div onclick="showSection('answers')">정답확인</div>
        <div onclick="showSection('cutoffs')">등급컷확인</div>
        <div onclick="showSection('examiners')">출제진</div>
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
                <span>제1회 YES모의고사 수학 손풀이.pdf</span>
                <a href="YES.EXAM.F.A_01.zip" download="YES.EXAM.F.A_01.zip" class="download-btn">다운로드</a>
            </li>
           <li>
                <span>제1회 YES모의고사 수학 정답지.pdf</span>
                <a href="YES.EXAM.F.A.zip" download="YES.EXAM.F.A.zip" class="download-btn">다운로드</a>
            </li>
            <li>
                <span>제1회 YES모의고사 수학.pdf</span>
                <a href="YES.EXAM.F_01.zip" download="YES.EXAM.F_01.zip" class="download-btn">다운로드</a>
            </li>
        </ul>
    </div>

    <div id="answers" class="content-section">
        <h2>정답 입력</h2>
        <div class="subject-tabs">
            <button id="tab-korean" class="active-sub" onclick="showSubject('korean')">국어</button>
            <button id="tab-calc" onclick="showSubject('calc')">미적분</button>
            <button id="tab-stat" onclick="showSubject('stat')">확률과 통계</button>
            <button id="tab-geo" onclick="showSubject('geo')">기하</button>
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
                    <th>기하</th>
                </tr>
            </thead>
            <tbody>
                <tr><td>1등급</td><td></td><td>84</td><td>81</td><td>83</td></tr>
                <tr><td>2등급</td><td></td><td>78</td><td>74</td><td>75</td></tr>
                <tr><td>3등급</td><td></td><td>70</td><td>68</td><td>69</td></tr>
            </tbody>
        </table>
    </div>

    <div id="examiners" class="content-section">
        <h2>YES 모의고사 출제진</h2>
        <p style="color: #6b7280; font-size: 14px; margin-bottom: 20px;">
           양정의 출제진들이 모였다
        </p>
        <div class="examiner-grid">
            <div class="examiner-card">
                <div class="examiner-name">김호석</div>
                <div class="examiner-role">국어 영역 대표 출제 (독서)</div>
                <div class="examiner-desc">
                    - 현) 양정고등학교 학생<br>
                    - 현) YES 모의고사 국어출제위원
                </div>
            </div>

         <div class="examiner-card">
            <div class="examiner-name">이승헌</div>
                <div class="examiner-role">국어 영역 출제 (문학)</div>
                <div class="examiner-desc">
                    - 현) 양정고등학교 학생<br>
                    - 현) YES 모의고사 국어출제위원
                </div>
            </div>

          <div class="examiner-card">
            <div class="examiner-name">장환진</div>
                <div class="examiner-role">국어 영역 출제(언어와 매체)</div>
                <div class="examiner-desc">
                    - 현) 양정고등학교 학생<br>
                    - 현) YES 모의고사 국어출제위원
                </div>
            </div>
            
           <div class="examiner-card">
                <div class="examiner-name">김성엽</div>
                <div class="examiner-role">수학 영역 공동 출제 (공통/미적분/확통)</div>
                <div class="examiner-desc">
                    - 현) 양정고등학교 학생<br>
                    - 현) YES 모의고사 수학출제위원
                </div>
            </div>

            <div class="examiner-card">
                <div class="examiner-name">신재원</div>
                <div class="examiner-role">수학 영역 출제 (확통)</div>
                <div class="examiner-desc">
                    - 현) 양정고등학교 학생<br>
                    - 현) YES모의고사 수학출제위원
                </div>
            </div>
           <div class="examiner-card">
                <div class="examiner-name">윤예찬</div>
                <div class="examiner-role">수학 영역 공동 출제 (공통/미적분/기하)</div>
                <div class="examiner-desc">
                    - 현) 양정고등학교 학생<br>
                    - 현) YES 모의고사 수학출제위원
                </div>
            </div>
            <div class="examiner-card">
                <div class="examiner-name">이성준</div>
                <div class="examiner-role">수학 영역 대표 출제 (전체)</div>
                <div class="examiner-desc">
                    - 현) 양정고등학교 학생<br>
                    - 현) YES 모의고사 수학출제위원<br>
                    - 현) YES 모의고사 서버 관리자
                </div>
            </div>
        </div>
    </div>

    <script>
        const correctAnswers = {
            korean: { 1: "1", 2: "3", 3: "5", 4: "2" },
            calc: { 1: "2", 2: "3", 3: "5", 4: "5", 5: "5", 6: "5", 7: "1", 8: "3", 9: "2", 10: "1", 11: "4", 12: "2", 13: "2", 14: "1", 15: "4", 16: "11", 17: "6", 18: "7", 19: "3", 20: "29", 21: "20", 22: "480", 23: "4", 24: "1", 25: "2", 26: "5", 27: "1", 28: "2", 29: "402", 30: "125" },
            stat: { 1: "2", 2: "3", 3: "5", 4: "5", 5: "5", 6: "5", 7: "1", 8: "3", 9: "2", 10: "1", 11: "4", 12: "2", 13: "2", 14: "1", 15: "4", 16: "11", 17: "6", 18: "7", 19: "3", 20: "29", 21: "20", 22: "480", 23: "4", 24: "1", 25: "3", 26: "4", 27: "5", 28: "1", 29: "841", 30: "74" },
            geo: { 1: "2", 2: "3", 3: "5", 4: "5", 5: "5", 6: "5", 7: "1", 8: "3", 9: "2", 10: "1", 11: "4", 12: "2", 13: "2", 14: "1", 15: "4", 16: "11", 17: "6", 18: "7", 19: "3", 20: "29", 21: "20", 22: "480", 23: "2", 24: "5", 25: "1", 26: "2", 27: "5", 28: "3", 29: "2", 30: "4" }
        };

        let currentSubject = 'korean'; 

        function showSection(sectionId) {
            const sections = document.querySelectorAll('.content-section');
            sections.forEach(sec => sec.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            
            if(sectionId === 'answers') {
                showSubject('korean');
            }
        }

        function showSubject(subject) {
            currentSubject = subject; 

            document.querySelectorAll('.subject-tabs button').forEach(btn => btn.classList.remove('active-sub'));
            if(subject === 'korean') document.getElementById('tab-korean').classList.add('active-sub');
            if(subject === 'calc') document.getElementById('tab-calc').classList.add('active-sub');
            if(subject === 'stat') document.getElementById('tab-stat').classList.add('active-sub');
            if(subject === 'geo') document.getElementById('tab-geo').classList.add('active-sub');
             
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

        function gradeAnswers() {
            let qCount = currentSubject === 'korean' ? 45 : 30;
            let prefix = currentSubject === 'korean' ? 'kor' : currentSubject;
            
            let correctCount = 0;
            let answeredCount = 0;
            let totalScore = 0;
            let wrongQuestions = [];

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

                let realAnswer = correctAnswers[currentSubject][i];
                
                if (realAnswer && userAnswer === String(realAnswer)) {
                    correctCount++;
                    if (currentSubject !== 'korean') {
                        totalScore += getMathScore(i);
                    }
                } else {
                    wrongQuestions.push(i);
                }
            }

            let subjectName = currentSubject === 'korean' ? '국어' : currentSubject === 'calc' ? '미적분' : currentSubject === 'stat' ? '확률과 통계' : '기하';
            let wrongListStr = wrongQuestions.length > 0 ? wrongQuestions.join(', ') + '번' : '전원 정답!';

            if (currentSubject === 'korean') {
                alert(`[${subjectName} 채점 결과]\n- 푼 문제 수: ${answeredCount} / ${qCount}\n- 맞힌 문제 수: ${correctCount}개\n\n 틀린 문제:\n${wrongListStr}`);
            } else {
                alert(`[${subjectName} 채점 결과]\n- 푼 문제 수: ${answeredCount} / ${qCount}\n\n 틀린 문제:\n${wrongListStr}\n\n 최종 점수: ${totalScore}점 / 100점`);
            }
        }
    </script>
</body>
</html>
