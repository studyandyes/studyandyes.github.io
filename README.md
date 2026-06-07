<!DOCTYPE html>
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
            <img src="IMG_6273.jpg" alt="메인 페이지 이미지">
        </div>
    </div>

    <div id="download" class="content-section">
        <h2>자료실</h2>
        <ul class="file-list">
            <li>
                <span>2026학년도 모의고사 국어 문제지.pdf</span>
                <button class="download-btn">다운로드</button>
            </li>
            <li>
                <span>2026학년도 모의고사 수학 문제지.pdf</span>
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
                <tr><td>1등급</td><td>92</td><td>88</td><td>90</td></tr>
                <tr><td>2등급</td><td>85</td><td>80</td><td>81</td></tr>
                <tr><td>3등급</td><td>77</td><td>71</td><td>73</td></tr>
            </tbody>
        </table>
    </div>

    <script>
        // 탭 전환 기능
        function showSection(sectionId) {
            const sections = document.querySelectorAll('.content-section');
            sections.forEach(sec => sec.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');
            
            // 정답확인 탭을 누르면 기본으로 국어 화면 표시
            if(sectionId === 'answers') {
                showSubject('korean');
            }
        }

        // 과목별 답안지 생성 기능
        function showSubject(subject) {
            // 서브 탭 색상 변경
            document.querySelectorAll('.subject-tabs button').forEach(btn => btn.classList.remove('active-sub'));
            if(subject === 'korean') document.getElementById('tab-korean').classList.add('active-sub');
            if(subject === 'calc') document.getElementById('tab-calc').classList.add('active-sub');
            if(subject === 'stat') document.getElementById('tab-stat').classList.add('active-sub');

            const container = document.getElementById('subject-content');
            let html = '';

            if (subject === 'korean') {
                // 국어 45문제 (전부 객관식)
                for (let i = 1; i <= 45; i++) {
                    html += createObjectiveQuestion(i, 'kor');
                }
            } else {
                // 미적분, 확률과 통계 30문제 (16~22, 29, 30 주관식)
                const subjectiveNums = [16, 17, 18, 19, 20, 21, 22, 29, 30];
                for (let i = 1; i <= 30; i++) {
                    if (subjectiveNums.includes(i)) {
                        html += createSubjectiveQuestion(i, subject);
                    } else {
                        html += createObjectiveQuestion(i, subject);
                    }
                }
            }
            container.innerHTML = html;
        }

        // 객관식 HTML 생성
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

        // 주관식 HTML 생성
        function createSubjectiveQuestion(num, prefix) {
            return `
                <div class="question-item">
                    <strong>${num}번 (주관식)</strong><br>
                    <input type="text" placeholder="정답 입력" name="${prefix}_q${num}">
                </div>
            `;
        }
    </script>
</body>
</html>
