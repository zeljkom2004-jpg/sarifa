<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Website Chat Demo</title>
<style>
:root{
--bg:#0b1220;
--panel:#0f1a2e;
--header:#1c5fe6;
--headerText:#ffffff;
--bubbleBot:#17284a;
--bubbleUser:#2a6fe8;
--text:#e6ecfa;
--muted:#9fb0d0;
--stroke:rgba(255,255,255,.06);
--shadow:0 20px 50px rgba(0,0,0,.5);
--radius:18px;
}

*{box-sizing:border-box}

body{
margin:0;
background:#0a0f1a;
color:var(--text);
font:14px/1.35 system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial;
display:grid;
place-items:center;
min-height:100vh;
padding:20px;
}

/* iPhone-like frame */
.phone{
width:min(420px, 96vw);
background:linear-gradient(180deg,#1c1f27,#0f1218);
border-radius:34px;
padding:18px;
box-shadow:
0 20px 50px rgba(0,0,0,.6),
inset 0 0 0 1px rgba(255,255,255,.08);
position:relative;
}


.screen{
height:min(760px, 84vh);
border-radius:26px;
overflow:hidden;
background:var(--bg);
border:1px solid rgba(0,0,0,.1);
position:relative;
}

/* iPhone notch */
.notch{
position:absolute;
left:50%;
top:10px;
transform:translateX(-50%);
width:150px;
height:26px;
background:#cfcfcf;
border-radius:0 0 16px 16px;
opacity:.45;
z-index:5;
pointer-events:none;
}

/* Header */
.header{
height:74px;
background:linear-gradient(180deg, var(--header), #1a56d8);
display:flex;
align-items:flex-end;
padding:14px 14px 12px;
gap:12px;
position:relative;
z-index:3;
-webkit-font-smoothing:antialiased;
-moz-osx-font-smoothing:grayscale;
backdrop-filter:blur(8px);
}

.hamburger{
width:28px;
height:28px;
border:1px solid rgba(255,255,255,.2);
border-radius:10px;
display:grid;
place-items:center;
background:rgba(0,0,0,.08);
flex:0 0 auto;
}

.hamburger span{
width:14px;
height:2px;
background:#fff;
display:block;
box-shadow:0 5px 0 #fff, 0 -5px 0 #fff;
border-radius:2px;
}

.titleWrap{
min-width:0;
flex:1;
}

.title{
font-weight:700;
font-size:20px;
letter-spacing:.2px;
white-space:nowrap;
overflow:hidden;
text-overflow:ellipsis;
color:var(--headerText);
}

.subtitle{
margin-top:2px;
font-size:12px;
color:rgba(255,255,255,.85);
display:flex;
align-items:center;
gap:8px;
}

.dot{
width:8px;
height:8px;
border-radius:50%;
background:#37d67a;
box-shadow:0 0 0 3px rgba(55,214,122,.22);
}

.pill{
margin-left:auto;
font-size:12px;
color:#fff;
padding:6px 10px;
border-radius:999px;
border:1px solid rgba(255,255,255,.18);
background:rgba(0,0,0,.08);
cursor:pointer;
user-select:none;
}

/* Chat */
.chat{
height:calc(100% - 74px - 78px);
padding:14px 12px 10px;
overflow:auto;
scroll-behavior:smooth;
}

.row{
display:flex;
gap:10px;
margin:10px 0;
align-items:flex-end;
}

.row.user{justify-content:flex-end}

.avatar{
width:30px;
height:30px;
border-radius:50%;
background:linear-gradient(180deg, #2d3d68, #121a2c);
border:1px solid rgba(255,255,255,.1);
display:grid;
place-items:center;
flex:0 0 auto;
color:#cfe0ff;
font-weight:700;
font-size:12px;
}

.bubble{
max-width:78%;
padding:10px 12px;
border-radius:16px;
border:1px solid rgba(255,255,255,.05);
box-shadow:0 8px 18px rgba(0,0,0,.22);
word-wrap:break-word;
white-space:pre-wrap;
}

.bot .bubble{
background:var(--bubbleBot);
border-top-left-radius:10px;
}

.user .bubble{
background:var(--bubbleUser);
border-top-right-radius:10px;
}

.meta{
margin-top:4px;
font-size:11px;
color:var(--muted);
opacity:.9;
display:flex;
gap:10px;
align-items:center;
}

.meta .chip{
padding:3px 8px;
border-radius:999px;
border:1px solid rgba(255,255,255,.08);
background:rgba(255,255,255,.04);
}

/* Typing indicator */
.typing{
display:inline-flex;
gap:6px;
align-items:center;
}

.typing i{
width:6px;
height:6px;
border-radius:50%;
background:rgba(232,238,252,.7);
display:block;
animation:bounce 1.2s infinite ease-in-out;
}

.typing i:nth-child(2){animation-delay:.15s}
.typing i:nth-child(3){animation-delay:.3s}

@keyframes bounce{
0%,80%,100%{transform:translateY(0);opacity:.55}
40%{transform:translateY(-4px);opacity:1}
}

/* Quick replies */
.quick{
display:flex;
flex-wrap:wrap;
gap:8px;
margin-top:8px;
}

.qr{
font-size:12px;
color:#eaf1ff;
padding:7px 10px;
border-radius:999px;
border:1px solid rgba(255,255,255,.14);
background:rgba(255,255,255,.06);
cursor:pointer;
user-select:none;
transition:transform .05s ease;
}

.qr:hover{border-color:rgba(255,255,255,.26)}
.qr:active{transform:scale(.98)}

/* Composer */
.composer{
height:78px;
background:linear-gradient(180deg, rgba(15,26,46,.25), rgba(15,26,46,.75));
padding:12px;
display:flex;
gap:10px;
align-items:center;
position:relative;
z-index:3;
}

.plus{
width:40px;
height:40px;
border-radius:50%;
border:1px solid rgba(255,255,255,.14);
background:rgba(255,255,255,.05);
display:grid;
place-items:center;
color:#dbe7ff;
font-size:22px;
user-select:none;
}

.input{
flex:1;
height:42px;
border-radius:999px;
border:1px solid rgba(255,255,255,.14);
background:rgba(255,255,255,.06);
padding:0 14px;
color:var(--text);
outline:none;
}

.send{
width:44px;
height:44px;
border-radius:50%;
border:none;
background:var(--bubbleUser);
color:#fff;
font-weight:700;
cursor:pointer;
box-shadow:0 10px 20px rgba(42,111,232,.25);
}

.send:disabled{
opacity:.55;
cursor:not-allowed;
box-shadow:none;
}

/* Small helper badge */
.badge{
position:absolute;
right:12px;
top:88px;
font-size:11px;
color:rgba(232,238,252,.85);
background:rgba(0,0,0,.2);
border:1px solid rgba(255,255,255,.12);
padding:6px 10px;
border-radius:999px;
z-index:2;
user-select:none;
}

a.ctaLink{
color:#ffffff;
text-decoration:none;
font-weight:700;
border-bottom:1px solid rgba(255,255,255,.35);
}
.notch { display: none !important; }

</style>
</head>

<body>
<div class="phone">
<div class="screen" id="screen">
<div class="notch" aria-hidden="true"></div>

<div class="header">
<div class="hamburger" title="Menu"><span></span></div>

<div class="titleWrap">
<div class="title" id="companyTitle">Your Company</div>
<div class="subtitle">
<span class="dot"></span>
<span id="statusText">Online • replies instantly</span>
</div>
</div>

<div class="pill" id="resetBtn" title="Reset demo">Reset</div>
</div>

<div class="badge">Demo chat • website widget</div>

<div class="chat" id="chat" aria-live="polite" aria-label="Chat messages"></div>

<div class="composer">
<div class="plus" title="Attachments">+</div>
<input class="input" id="msg" type="text" placeholder="Type a message…" autocomplete="off" />
<button class="send" id="sendBtn" disabled>➤</button>
</div>
</div>
</div>

<script>
// =======================
// 1) EDIT THESE 3 VALUES
// =======================
const COMPANY_NAME = "SARIFA"; // <-- Put client's company name here
const CTA_LINK = "https://example.com/quote"; // <-- Quote/booking link (can be placeholder for demo)
const BRAND_TONE = "professional"; // "professional" | "friendly" (demo only)

// ===================================
// 2) QUESTION BANK (edit any time)
// ===================================
// These are the "random questions" the assistant can ask (insurance + lead qualification).
const QUESTION_BANK = [
"Are you looking for a new quote or help with an existing policy?",
"What state are you located in?",
"What type of insurance are you interested in (auto, home, renters, commercial, life, other)?",
"When do you need coverage to start?",
"What’s the best way to reach you (phone or email)?",
"Are you shopping for the best price, the best coverage, or a balance of both?",
"Do you have a current insurer? If yes, who is it (optional)?",
"Is this for personal or business coverage?",
"Do you prefer a quick call or should we keep it chat/email for now?",
"Any urgent deadline (today/this week/no rush)?"
];

// Optional: quick reply chips for faster demo
const QUICK_REPLIES = {
intent: ["New quote", "Existing policy", "Just a question"],
contact: ["Phone", "Email"],
urgency: ["Today", "This week", "No rush"]
};

// ===================================
// 3) SIMPLE DEMO FLOW (state machine)
// ===================================
const flow = [
{ key: "intent", prompt: "Hi! Quick question — are you looking for a new quote or help with an existing policy?", quick: QUICK_REPLIES.intent },
{ key: "state", prompt: "Great — what state are you in?", quick: [] },
{ key: "type", prompt: "What type of coverage do you need (auto, home, renters, commercial, life, etc.)?", quick: [] },
{ key: "start", prompt: "When do you need coverage to start?", quick: QUICK_REPLIES.urgency },
{ key: "contact", prompt: "Last step — what’s the best way to reach you (phone or email)?", quick: QUICK_REPLIES.contact }
];

// ===================================
// 4) IMPLEMENTATION
// ===================================
const chatEl = document.getElementById("chat");
const msgEl = document.getElementById("msg");
const sendBtn = document.getElementById("sendBtn");
const resetBtn = document.getElementById("resetBtn");
const companyTitle = document.getElementById("companyTitle");

companyTitle.textContent = COMPANY_NAME;

const state = {
step: 0,
answers: {},
busy: false
};

function nowTime(){
const d = new Date();
return d.toLocaleTimeString([], {hour:"2-digit", minute:"2-digit"});
}

function scrollToBottom(){
chatEl.scrollTop = chatEl.scrollHeight;
}

function el(tag, attrs = {}, children = []){
const node = document.createElement(tag);
Object.entries(attrs).forEach(([k,v]) => {
if(k === "class") node.className = v;
else if(k === "html") node.innerHTML = v;
else node.setAttribute(k, v);
});
children.forEach(c => node.appendChild(c));
return node;
}

function addMessage({from, text, meta = true, quick = null, isTyping = false}){
const row = el("div", {class: "row " + (from === "user" ? "user" : "bot")});

if(from !== "user"){
row.appendChild(el("div", {class:"avatar", title:COMPANY_NAME + " assistant"}, [document.createTextNode("AI")]));
}

const bubble = el("div", {class:"bubble"});
if(isTyping){
bubble.appendChild(el("span", {class:"typing", "aria-label":"Typing"}, [
el("i"), el("i"), el("i")
]));
}else{
bubble.textContent = text;
}

const wrap = el("div");
wrap.appendChild(bubble);

if(meta){
const metaRow = el("div", {class:"meta"});
metaRow.appendChild(el("span", {class:"chip"}, [document.createTextNode("Now")]));
metaRow.appendChild(el("span", {}, [document.createTextNode(nowTime())]));
wrap.appendChild(metaRow);
}

// Quick replies
if(quick && Array.isArray(quick) && quick.length){
const qWrap = el("div", {class:"quick"});
quick.slice(0,6).forEach(label => {
const btn = el("div", {class:"qr", role:"button", tabindex:"0"}, [document.createTextNode(label)]);
btn.addEventListener("click", () => {
if(state.busy) return;
userSend(label);
});
btn.addEventListener("keydown", (e) => {
if(e.key === "Enter" || e.key === " ") btn.click();
});
qWrap.appendChild(btn);
});
wrap.appendChild(qWrap);
}

row.appendChild(wrap);

if(from === "user"){
// user row has no avatar; aligns right
}

chatEl.appendChild(row);
scrollToBottom();
}

function addBotTyping(){
addMessage({from:"bot", text:"", meta:false, isTyping:true});
scrollToBottom();
}

function removeLastTyping(){
// Remove last bot message if it is a typing bubble
const rows = Array.from(chatEl.querySelectorAll(".row.bot"));
const last = rows[rows.length - 1];
if(!last) return;
const bubble = last.querySelector(".bubble");
if(bubble && bubble.querySelector(".typing")){
last.remove();
}
}

function pickRandomQuestion(exclude = []){
const pool = QUESTION_BANK.filter(q => !exclude.includes(q));
return pool[Math.floor(Math.random() * pool.length)] || "What can I help you with today?";
}

function botSay(text, quick = null){
state.busy = true;
addBotTyping();
const delay = 600 + Math.floor(Math.random()*500);
setTimeout(() => {
removeLastTyping();
addMessage({from:"bot", text, quick});
state.busy = false;
}, delay);
}

function nextStep(){
if(state.step < flow.length){
const stepObj = flow[state.step];
botSay(stepObj.prompt, stepObj.quick);
return;
}

// After collecting basics, show a strong “handoff” + CTA
const summary = [
"Perfect — thanks. Here’s what I captured:",
`• Need: ${state.answers.intent || "—"}`,
`• State: ${state.answers.state || "—"}`,
`• Coverage type: ${state.answers.type || "—"}`,
`• Start date: ${state.answers.start || "—"}`,
`• Best contact: ${state.answers.contact || "—"}`,
"",
"Next step: I can route you directly to the right quote/support flow.",
`Use this link: ${CTA_LINK}`
].join("\n");

botSay(summary);

// Add one “random” helpful question for polish
setTimeout(() => {
const extra = pickRandomQuestion();
botSay(extra);
}, 900);
}

function normalizeText(t){
return (t || "").trim();
}

function userSend(text){
const clean = normalizeText(text);
if(!clean || state.busy) return;

addMessage({from:"user", text:clean});
msgEl.value = "";
sendBtn.disabled = true;

// Save answer if we're in the flow
if(state.step < flow.length){
const stepObj = flow[state.step];
state.answers[stepObj.key] = clean;
state.step += 1;

// Smart little acknowledgement (conservative/professional)
const ack = [
"Got it.",
"Thanks — noted.",
"Perfect, thank you.",
"Understood."
][Math.floor(Math.random()*4)];

botSay(ack);

// Ask next question
setTimeout(() => nextStep(), 650);
return;
}

// If user keeps chatting after flow, answer conservatively
const fallback =
"Thanks — I can help with that. For the fastest response, can you share whether this is a new quote or an existing policy, and which state you’re in?";
botSay(fallback, QUICK_REPLIES.intent);
}

function boot(){
// First bot message: short + strong value (no tech talk)
botSay(
`Hi! I’m the ${COMPANY_NAME} website assistant. I can help with quotes, coverage questions, and routing you to the right next step.`
);
setTimeout(() => nextStep(), 700);
}

function reset(){
chatEl.innerHTML = "";
state.step = 0;
state.answers = {};
state.busy = false;
boot();
}

// Input handling
msgEl.addEventListener("input", () => {
sendBtn.disabled = !normalizeText(msgEl.value) || state.busy;
});

msgEl.addEventListener("keydown", (e) => {
if(e.key === "Enter"){
e.preventDefault();
if(!sendBtn.disabled) userSend(msgEl.value);
}
});

sendBtn.addEventListener("click", () => userSend(msgEl.value));
resetBtn.addEventListener("click", reset);

// Safety: never show romantic/roleplay content (professional only)
// Start demo
reset();

// Optional: expose CTA link visually if you want (kept conservative).
// You can uncomment the snippet below to show CTA as clickable link inside bot message.
// NOTE: If you do, the CTA in the summary will still appear as plain text too.
//
// function linkifyLastCTA(){
// const rows = Array.from(chatEl.querySelectorAll(".row.bot .bubble"));
// const last = rows[rows.length - 1];
// if(!last) return;
// last.innerHTML = last.textContent.replace(CTA_LINK, `<a class="ctaLink" href="${CTA_LINK}" target="_blank" rel="noreferrer">${CTA_LINK}</a>`);
// }
</script>
</body>
</html>
