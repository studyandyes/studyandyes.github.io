<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>YES 모의고사</title>

<style>

body{
    margin:0;
    padding:20px;
    background:#f3f4f6;
    font-family:Arial,sans-serif;
}

.container{
    max-width:900px;
    margin:auto;
    background:white;
    border-radius:20px;
    padding:30px;
}

.logo{
    text-align:center;
    font-size:50px;
    font-weight:bold;
    color:#2f64d6;
    margin-bottom:20px;
}

.title{
    text-align:center;
    font-size:42px;
    font-weight:bold;
    color:#2d3748;
}

.download-box{
    background:#eef2f7;
    padding:25px;
    border-radius:15px;
    margin:30px 0;
    text-align:center;
}

.download-btn{
    display:inline-block;
    background:#5b9be6;
    color:white;
    text-decoration:none;
    padding:15px 40px;
    border-radius:10px;
    font-size:20px;
    margin-top:15px;
}

.subject-select{
    text-align:center;
    font-size:22px;
    margin-bottom:30px;
}

.section-title{
    font-size:32px;
    font-weight:bold;
    margin-bottom:20px;
}

.question{
    display:flex;
    align-items:center;
    gap:15px;
    padding:15px 0;
    border-bottom:1px solid #ddd;
}

.question-number{
    width:60px;
    font-weight:bold;
    font-size:20px;
}

.subjective{
    width:120px;
    padding:8px;
    font-size:18px;
}

.submit-btn{
    width:100%;
    margin-top:30px;
    padding:18px;
    background:#57c26a;
    border:none;
    color:white;
    font-size:24px;
    border-radius:12px;
    cursor:pointer;
}

.result{
    text-align:center;
    font-size:30px;
    font-weight:bold;
    margin-top:20px;
}

</style>
</head>

<body>

<div class="container">

<div class="logo">Study And Yes</div>

<div class="title">YES 모의고사</div>

<div class="download-box">
    <p>YES 모의고사 다운로드 버튼</p>

    <a href="exam.pdf" class="download-btn">
        📄 시험지 다운로드(.pdf)
    </a>
</div>

<div class="subject-select">

<label>
<input type="radio" name="subject" value="calc" checked>
미적분
</label>

&nbsp;&nbsp;&nbsp;

<label>
<input type="radio" name="subject" value="stat">
확률과 통계
</label>

</div>

<div class="section-title">
OMR 답안 마킹
</div>

<div id="questions"></div>

<button class="submit-btn" onclick="gradeExam()">
📋 답안지 제출 및 채점하기
</button>

<div id="result" class="result"></div>

</div>

<script>

/* ==========================
   미적분 정답
========================== */

const calcAnswers = [
1,2,3,4,5,
1,2,3,4,5,
1,2,3,4,5,
120,35,8,14,9,17,21,
1,2,3,4,5,1,
27,128
];

/* ==========================
   확통 정답
========================== */

const statAnswers = [
5,4,3,2,1,
5,4,3,2,1,
5,4,3,2,1,
15,24,12,7,9,18,20,
5,4,3,2,1,5,
32,256
];

/* ==========================
   문항 생성
========================== */

const questions =
document.getElementById("questions");

for(let i=1;i<=30;i++){

    let html="";

    if(
        (i>=16 && i<=22) ||
        i===29 ||
        i===30
    ){

        html=`
        <div class="question">

            <div class="question-number">
            ${i}번.
            </div>

            <input
            type="number"
            id="q${i}"
            class="subjective"
            placeholder="정답 입력">

        </div>
        `;
    }

    else{

        html=`
        <div class="question">

            <div class="question-number">
            ${i}번.
            </div>

            <label><input type="radio" name="q${i}" value="1"> 1</label>

            <label><input type="radio" name="q${i}" value="2"> 2</label>

            <label><input type="radio" name="q${i}" value="3"> 3</label>

            <label><input type="radio" name="q${i}" value="4"> 4</label>

            <label><input type="radio" name="q${i}" value="5"> 5</label>

        </div>
        `;
    }

    questions.innerHTML += html;
}

/* ==========================
   채점
========================== */

function gradeExam(){

    const subject =
    document.querySelector(
    'input[name="subject"]:checked'
    ).value;

    const answers =
    subject==="calc"
    ? calcAnswers
    : statAnswers;

    let score = 0;

    for(let i=1;i<=30;i++){

        if(
            (i>=16 && i<=22) ||
            i===29 ||
            i===30
        ){

            const input =
            document.getElementById(`q${i}`);

            if(
                input.value !== "" &&
                Number(input.value)
                === answers[i-1]
            ){
                score++;
            }

        }else{

            const checked =
            document.querySelector(
            `input[name="q${i}"]:checked`
            );

            if(!checked) continue;

            if(
                Number(checked.value)
                === answers[i-1]
            ){
                score++;
            }
        }
    }

    document.getElementById("result")
    .innerHTML =
    `점수 : ${score} / 30`;
}

</script>

</body>
</html>
