
<script>
import { onMount } from 'svelte';
var time;
var question;
let stopwatch = "";
let watch;
var answer;
var status = "Solve me !!";
var ans;
const operators = {
    "x": "*",
    "+": "+",
    "-": "-",
    "÷": "/"
}
function focus(el) { el.focus(); }
$: if(ans) ans.onchange = (e) => {
    if(e.currentTarget.value == answer) {
        stopCount(); setTimeout(() => { startCount(); status = "Solve me !" }, 1500);
        status = "Correct !"
    }else {
        status = "Wrong"
        setTimeout(() => { status = "Solve me !" }, 1500);
    }
}

const generate = () => {
    const rand = (e) => Math.floor(Math.random() * e);
    var result;
    result = [];
    for(let i=0; i<(rand(2)+1); i++) {
    const opt = Object.keys(operators)[rand(Object.keys(operators).length - 1)];
    const operator = operators[opt];
    result.push({ opt, operator })
    }
    const com = result.map(e => `${rand(9)+1}${e.operator}${rand(9)+1}`).join("");
    question = com;
    answer = eval(com);
}

let startCount = (e) => {
if(e) e.currentTarget.style.display = "none";
time = 0;
generate()
watch =  setInterval(() => {
var minutes = Math.floor((time % (1000 * 60 * 60)) / (1000 * 60));
var seconds = Math.floor((time % (1000 * 60)) / 1000);
time+=1000;
stopwatch = `${minutes ? `${minutes}min` : ""} ${seconds}sec`
}, 1000);
if(ans) ans?.focus();
}

let stopCount = (e) => {
    if(e && e.currentTarget.textContent != "Stop") {
        e.currentTarget.textContent = "Stop";
        startCount()
    }else {
        if(e) e.currentTarget.textContent = "Another Round";
        clearInterval(watch);
    }
}
</script>
<body>
<div class="container">
<button on:click={startCount}>Start</button>
{#if time}
<p>{stopwatch}</p>
<h1>{question}</h1>
<p>{status}</p>
<input bind:this={ans} type="number" use:focus/>
<button on:click={stopCount}>Stop</button>
{/if}
</div>
<div class="footer">
    <p>Math Game &copy; Lee Chee Yong 2023</p>
    <p>This project is available as open source under the terms of the <a style="color: #6b8772; text-decoration: none" href="https://github.com/joeleeofficial/Joe-Portfolio/LICENSE">MIT License</a></p>
</div>
</body>
