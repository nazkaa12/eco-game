# eco-game
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Теңізді құтқар PRO</title>

<style>
body{
    margin:0;
    text-align:center;
    font-family:Arial;
    color:white;
    overflow:hidden;
    background: linear-gradient(to bottom,#87CEFA 0%,#00BFFF 40%,#0077be 100%);
}

/* Толқын анимация */
.wave{
    position:absolute;
    bottom:0;
    width:200%;
    height:150px;
    background:rgba(255,255,255,0.3);
    border-radius:100%;
    animation:waveMove 8s linear infinite;
}

.wave2{
    bottom:20px;
    opacity:0.4;
    animation:waveMove 12s linear infinite reverse;
}

@keyframes waveMove{
    from{ transform:translateX(0); }
    to{ transform:translateX(-50%); }
}

h1{margin-top:15px;}

#score,#timer{
    font-size:20px;
    margin:5px;
}

.trash{
    position:absolute;
    font-size:40px;
    cursor:pointer;
    transition: transform 0.3s;
}

.trash:hover{
    transform: scale(1.2);
}

.bin{
    position:absolute;
    bottom:20px;
    width:120px;
    height:100px;
    line-height:100px;
    border-radius:20px;
    font-weight:bold;
    color:white;
    box-shadow:0 8px 20px rgba(0,0,0,0.3);
}

.plastic{background:gold; left:20%;}
.glass{background:blue; left:45%;}
.paper{background:green; left:70%;}

#result{
    font-size:28px;
    margin-top:10px;
}

button{
    margin-top:10px;
    padding:10px 20px;
    border:none;
    border-radius:15px;
    font-size:16px;
}
</style>
</head>

<body>

<h1>🌊 ТЕҢІЗДІ ҚҰТҚАР PRO</h1>
<div id="score">Ұпай: 0</div>
<div id="timer">Уақыт: 30</div>
<div id="result"></div>

<div id="game"></div>

<div class="bin plastic" data-type="plastic">ПЛАСТИК</div>
<div class="bin glass" data-type="glass">ШЫНЫ</div>
<div class="bin paper" data-type="paper">ҚАҒАЗ</div>

<button onclick="restart()">Қайта бастау</button>

<!-- Толқындар -->
<div class="wave"></div>
<div class="wave wave2"></div>

<script>
let score=0;
let time=30;
let game=document.getElementById("game");
let timerDisplay=document.getElementById("timer");
let scoreDisplay=document.getElementById("score");
let result=document.getElementById("result");

let trashTypes=[
    {emoji:"🥤",type:"plastic"},
    {emoji:"🍾",type:"glass"},
    {emoji:"📄",type:"paper"},
    {emoji:"🧴",type:"plastic"},
    {emoji:"📦",type:"paper"}
];

function spawnTrash(){
    let item=trashTypes[Math.floor(Math.random()*trashTypes.length)];
    let div=document.createElement("div");
    div.className="trash";
    div.innerHTML=item.emoji;
    div.dataset.type=item.type;
    div.style.left=Math.random()*90+"%";
    div.style.top="0px";
    game.appendChild(div);

    let fall=setInterval(()=>{
        div.style.top=(div.offsetTop+3)+"px";
        if(div.offsetTop>window.innerHeight-150){
            div.remove();
            clearInterval(fall);
        }
    },30);

    div.onclick=function(){
        selected=div;
    }
}

let selected=null;

document.querySelectorAll(".bin").forEach(bin=>{
    bin.onclick=function(){
        if(!selected) return;
        if(selected.dataset.type===bin.dataset.type){
            score++;
            scoreDisplay.innerHTML="Ұпай: "+score;
            result.innerHTML="🌊 ТЕҢІЗДІ ҚҰТҚАРДЫҢ!";
            selected.remove();
        }else{
            result.innerHTML="❌ ҚАТЕ!";
        }
        selected=null;
    }
});

let gameInterval=setInterval(spawnTrash,1000);

let timerInterval=setInterval(()=>{
    time--;
    timerDisplay.innerHTML="Уақыт: "+time;
    if(time<=0){
        clearInterval(timerInterval);
        clearInterval(gameInterval);
        result.innerHTML="👏 КЕРЕМЕТ! СЕН ЭКО-БАТЫРСЫҢ! Ұпай: "+score;
    }
},1000);

function restart(){
    location.reload();
}
</script>

</body>
</html>
