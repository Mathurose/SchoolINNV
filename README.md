<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>LiteVibe — ระบบติดตามอารมณ์และพฤติกรรม</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;700&display=swap" rel="stylesheet">
<link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🌈</text></svg>">
<style>
  :root{
    --bg:#f3f7ff; --card:#ffffff; --primary:#6c63ff; --accent:#ff7aa2; --muted:#6b7280;
    --glass: rgba(255,255,255,0.6);
  }
  *{box-sizing:border-box}
  body{font-family:"Kanit",sans-serif;background: linear-gradient(180deg,#eef5ff 0%,#fbfbff 100%);margin:0;color:#0f1724}
  .app{max-width:1200px;margin:24px auto;padding:20px;}
  header.app-header{display:flex;align-items:center;gap:16px;margin-bottom:18px}
  .logo{display:flex;align-items:center;gap:12px}
  .logo .mark{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--primary),var(--accent));display:flex;align-items:center;justify-content:center;color:#fff;font-size:28px;box-shadow:0 8px 30px rgba(108,99,255,0.18)}
  h1{font-size:20px;margin:0;color:var(--primary)}
  .header-right{margin-left:auto;display:flex;gap:12px;align-items:center}
  .card{background:var(--card);border-radius:14px;padding:14px;box-shadow:0 6px 20px rgba(15,23,36,0.04);margin-bottom:16px}
  .grid{display:grid;grid-template-columns: 1fr 360px;gap:16px}
  .row{display:flex;gap:12px}
  label{font-size:13px;color:var(--muted);display:block;margin-bottom:6px}
  input[type=text],select,textarea{width:100%;padding:10px;border-radius:10px;border:1px solid #eef2ff;background:linear-gradient(#fff,#fbfdff);font-size:14px}
  button{background:var(--primary);color:#fff;border:0;padding:10px 12px;border-radius:10px;cursor:pointer;font-weight:600}
  .btn-ghost{background:transparent;border:1px solid #e8eefe;color:var(--primary)}
  .muted{color:var(--muted);font-size:13px}
  .small{font-size:13px}
  .emoji-row{display:flex;gap:10px;flex-wrap:wrap}
  .emoji-btn{width:70px;height:70px;border-radius:12px;border:1px solid transparent;background:linear-gradient(180deg,#fff,#ffffff);display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:28px;cursor:pointer;box-shadow:0 6px 18px rgba(15,23,36,0.03)}
  .emoji-btn span.label{font-size:11px;color:var(--muted);margin-top:4px}
  .emoji-btn.selected{outline:3px solid rgba(108,99,255,0.12);box-shadow:0 12px 30px rgba(108,99,255,0.08);transform:translateY(-4px)}
  .badge{background:linear-gradient(90deg,#fff7f9,#fff);color:var(--primary);padding:6px 10px;border-radius:999px;border:1px solid rgba(108,99,255,0.08);font-weight:700}
  .flex{display:flex;align-items:center;gap:8px}
  .right{margin-left:auto}
  .list{max-height:320px;overflow:auto}
  .student-avatar{width:44px;height:44px;border-radius:12px;background:linear-gradient(135deg,#fff,#fbfbff);display:inline-flex;align-items:center;justify-content:center;font-weight:700;color:var(--primary);border:1px solid rgba(0,0,0,0.04)}
  .meta{font-size:12px;color:var(--muted)}
  @media(max-width:1000px){.grid{grid-template-columns:1fr} .header-right{display:none}}
</style>
</head>
<body>
<div class="app">
  <header class="app-header">
    <div class="logo">
      <div class="mark">LV</div>
      <div>
        <h1>LiteVibe</h1>
        <div class="muted small">ติดตามอารมณ์และพฤติกรรม — เพื่อสภาพแวดล้อมการเรียนที่อบอุ่น</div>
      </div>
    </div>

    <div class="header-right">
      <div id="currentUserBox" class="muted small"></div>
      <div style="display:flex;gap:8px;align-items:center">
        <button id="logoutBtn" class="btn-ghost" style="display:none">ออกจากระบบ</button>
      </div>
    </div>
  </header>

  <div id="authCard" class="card" style="margin-bottom:20px">
    <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
      <div style="min-width:220px;flex:1">
        <label>ชื่อผู้ใช้</label>
        <input id="username" type="text" placeholder="เช่น nam, pong, ajarn_bo" />
      </div>
      <div style="width:160px">
        <label>บทบาท</label>
        <select id="role">
          <option value="student">นักเรียน</option>
          <option value="teacher">ครู</option>
        </select>
      </div>
      <div style="display:flex;gap:8px;align-items:flex-end">
        <button id="loginBtn">เข้าสู่ระบบ / สร้างบัญชี</button>
        <button id="demoBtn" class="btn-ghost">ใช้บัญชีตัวอย่าง</button>
      </div>
    </div>
    <div style="margin-top:10px" class="muted small">ข้อมูลตัวอย่างและการใช้งานจะเก็บในเครื่อง (localStorage) — เหมาะสำหรับทดลอง</div>
  </div>

  <div id="mainArea" style="display:none">
    <div class="grid">
      <div>
        <!-- STUDENT PANEL -->
        <div id="studentPanel" style="display:none">
          <div class="card">
            <div style="display:flex;align-items:center;gap:12px">
              <div style="font-size:18px"><strong>บันทึกอารมณ์ประจำวัน</strong></div>
              <div class="muted small">บันทึกสั้น ๆ และเก็บเป็น My diary</div>
              <div class="right"><span class="badge">✨ LiteVibe</span></div>
            </div>

            <div style="margin-top:12px">
              <label>เลือกรูปอารมณ์</label>
              <div id="moodButtons" class="emoji-row"></div>
            </div>

            <div style="margin-top:12px">
              <label>ข้อความสั้น ๆ / My diary</label>
              <textarea id="diaryText" rows="3" placeholder="เล่าเรื่องสั้น ๆ วันนี้เป็นอย่างไร..."></textarea>
            </div>

            <div style="display:flex;align-items:center;gap:10px;margin-top:10px">
              <button id="saveMoodBtn">บันทึกอารมณ์</button>
              <div class="muted small">บันทึกล่าสุด: <span id="lastMoodText">-</span></div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <div style="display:flex;align-items:center;gap:12px">
              <div><strong>ดาวเด็กดี</strong></div>
              <div class="muted small">สะสมแล้วนำไปแลกของรางวัล</div>
              <div class="right"><span class="badge">⭐ <span id="myStars">0</span></span></div>
            </div>

            <div style="margin-top:12px;display:flex;gap:8px;align-items:center">
              <button id="openRedeem" class="btn-ghost">แลกของรางวัล</button>
              <div class="muted small">ตัวอย่าง: พัก 5 นาที = 10⭐, คูปองเครื่องเขียน = 12⭐</div>
            </div>

            <div id="redeemPanel" style="display:none;margin-top:12px">
              <div style="display:flex;gap:10px;flex-wrap:wrap">
                <div class="card" style="padding:10px;border-radius:10px">
                  <div><strong>เวลาพัก 5 นาที</strong></div>
                  <div class="meta">ใช้ 10 ดาว</div>
                  <button class="redeemBtn" data-name="เวลาพัก 5 นาที" data-cost="10" style="margin-top:8px">แลก</button>
                </div>
                <div class="card" style="padding:10px;border-radius:10px">
                  <div><strong>คูปองเครื่องเขียน</strong></div>
                  <div class="meta">ใช้ 12 ดาว</div>
                  <button class="redeemBtn" data-name="คูปองเครื่องเขียน" data-cost="12" style="margin-top:8px">แลก</button>
                </div>
                <div class="card" style="padding:10px;border-radius:10px">
                  <div><strong>คูปองเครื่องดื่ม/อาหาร</strong></div>
                  <div class="meta">ใช้ 15 ดาว</div>
                  <button class="redeemBtn" data-name="คูปองอาหาร" data-cost="15" style="margin-top:8px">แลก</button>
                </div>
              </div>

              <h4 style="margin-top:12px">ประวัติการแลก</h4>
              <div id="redeemHistory" class="list"></div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <div style="display:flex;align-items:center;gap:12px">
              <div><strong>นัดหมายปรึกษา</strong></div>
              <div class="muted small">ขอนัดกับครูที่ปรึกษาหรือครูแนะแนว</div>
            </div>
            <div style="margin-top:10px">
              <label>ครูที่ต้องการนัด</label>
              <input id="apptTeacher" placeholder="เช่น ajarn_nu" />
              <label style="margin-top:8px">ข้อความสำหรับนัด</label>
              <input id="apptMsg" placeholder="สาเหตุ/หัวข้อที่ต้องการปรึกษา" />
              <div style="display:flex;gap:8px;margin-top:10px">
                <button id="requestAppt">ส่งคำขอนัด</button>
                <div class="muted small">ประวัติการขอนัด</div>
              </div>
              <div id="apptHistory" class="list" style="margin-top:8px"></div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <div style="display:flex;align-items:center;gap:12px">
              <div><strong>แบบทดสอบจิตวิทยาเบื้องต้น</strong></div>
              <div class="muted small">คำถามสั้น ๆ เพื่อประเมินอารมณ์</div>
            </div>
            <div id="quizPanel" style="margin-top:10px">
              <div id="quizQuestions"></div>
              <div style="display:flex;gap:8px;margin-top:8px">
                <button id="startQuiz">เริ่มแบบทดสอบ</button>
                <button id="submitQuiz" style="display:none">ส่งคำตอบ</button>
              </div>
              <div id="quizResult" style="margin-top:8px"></div>
            </div>
          </div>
        </div>

        <!-- TEACHER PANEL -->
        <div id="teacherPanel" style="display:none">
          <div class="card">
            <div style="display:flex;align-items:center;gap:12px">
              <div><strong>แผงครู</strong></div>
              <div class="muted small">จัดการนักเรียน ดูคำขอนัด และสถิติ</div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <label>ค้นหานักเรียน</label>
            <input id="searchStudent" placeholder="ค้นหา..." />
            <div id="studentsList" class="list" style="margin-top:8px"></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>กล่องคำขอนัดจากนักเรียน</h4>
            <div id="apptRequests" class="list"></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>บันทึกรายงานพฤติกรรม</h4>
            <label>เลือกนักเรียน</label>
            <select id="reportStudent"></select>
            <label style="margin-top:8px">ข้อความรายงาน</label>
            <textarea id="reportText" rows="3"></textarea>
            <div style="margin-top:8px"><button id="sendReport">บันทึกและส่ง</button></div>
            <div id="reportsList" style="margin-top:8px" class="list"></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>สถิติภาพรวมอารมณ์</h4>
            <canvas id="moodChart" height="140"></canvas>
          </div>
        </div>
      </div>

      <div>
        <div class="card">
          <div style="display:flex;align-items:center;gap:12px">
            <div class="student-avatar" id="profileAvatar">LV</div>
            <div>
              <div id="profileName"><strong>-</strong></div>
              <div class="meta" id="profileRole">-</div>
            </div>
            <div class="right"><span class="badge">⭐ <span id="profileStars">0</span></span></div>
          </div>

          <div id="profileBox" style="margin-top:12px"></div>
        </div>

        <div class="card" style="margin-top:12px">
          <h4>แดชบอร์ดด่วน</h4>
          <div id="quickPanel"></div>
        </div>

        <div class="card" style="margin-top:12px">
          <h4>กิจกรรมล่าสุด (Log)</h4>
          <div id="activityLog" class="list"></div>
        </div>
      </div>
    </div>

    <footer style="margin-top:16px" class="muted small">LiteVibe — Prototype (เก็บข้อมูลในเครื่อง).</footer>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
/* LiteVibe prototype by assistant
   - data in localStorage key 'litevibe_data_v1'
   - seeded sample users when empty
*/

const STORAGE_KEY = 'litevibe_data_v1';
const emojiChoices = [
  {key:'very_happy', emoji:'😄', label:'มีความสุขมาก'},
  {key:'happy', emoji:'🙂', label:'สุขสบาย'},
  {key:'neutral', emoji:'😐', label:'เฉย ๆ'},
  {key:'sad', emoji:'😢', label:'เศร้า'},
  {key:'angry', emoji:'😠', label:'โกรธ'},
  {key:'tired', emoji:'😴', label:'เหนื่อย'}
];

let state = loadState();
let currentUser = null;
let moodChart = null;

function defaultState(){ return { users: {}, activity: [] }; }

function seedSampleData(s){
  // students
  s.users['nam'] = { name:'nam', display:'น้ำ', role:'student', stars:8, moods:[
    {time:'2025-11-30 08:10', key:'happy', emoji:'🙂', label:'สุขสบาย', note:'ตื่นมาสบายใจ' }
  ], diaries:[{time:'2025-11-30 08:10',text:'วันนี้มีสอบวิทย์'}], appts:[], redeemHistory:[], quiz:[{time:'2025-11-29',score:5}], reports:[] };
  s.users['pong'] = { name:'pong', display:'ป้อง', role:'student', stars:12, moods:[
    {time:'2025-11-30 07:50', key:'neutral', emoji:'😐', label:'เฉย ๆ', note:'ปวดเมื่อย' }
  ], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  s.users['siwarat'] = { name:'siwarat', display:'ศิวรัตน์', role:'student', stars:3, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };

  // teachers
  s.users['ajarn_nu'] = { name:'ajarn_nu', display:'ครูหนู', role:'teacher', stars:0, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[], inbox:[] };
  s.users['ajarn_korn'] = { name:'ajarn_korn', display:'ครูกร', role:'teacher', stars:0, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[], inbox:[] };

  s.activity.unshift({txt:'ระบบ LiteVibe สร้างตัวอย่างนักเรียนและครูสำหรับทดลอง', time:new Date().toLocaleString()});
}

function loadState(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(!raw){
      const s = defaultState();
      seedSampleData(s);
      localStorage.setItem(STORAGE_KEY, JSON.stringify(s));
      return s;
    }
    const parsed = JSON.parse(raw);
    // seed if empty
    if(!parsed.users || Object.keys(parsed.users).length===0){ seedSampleData(parsed); localStorage.setItem(STORAGE_KEY, JSON.stringify(parsed)); }
    return parsed;
  }catch(e){ const s = defaultState(); seedSampleData(s); return s; }
}
function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

function logActivity(txt){
  const time = new Date().toLocaleString();
  state.activity.unshift({txt,time});
  saveState();
  renderActivity();
}

/* AUTH */
document.getElementById('loginBtn').addEventListener('click', ()=>{
  const name = document.getElementById('username').value.trim();
  const role = document.getElementById('role').value;
  if(!name) return alert('กรุณากรอกชื่อผู้ใช้');
  let user = state.users[name];
  if(!user){
    user = { name, display:name, role, stars:0, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
    if(role==='teacher') user.inbox = [];
    state.users[name] = user;
    saveState();
    logActivity(`สร้างบัญชีใหม่: ${name} (${role})`);
  }
  loginAs(name);
});
document.getElementById('demoBtn').addEventListener('click', ()=>{
  // show a simple selector of sample accounts
  const opts = Object.values(state.users).slice(0,6).map(u=>`${u.name} (${u.role})`).join('\\n');
  const pick = prompt('เลือกบัญชีตัวอย่างโดยพิมพ์ชื่อผู้ใช้:\\n' + opts);
  if(pick && state.users[pick.trim()]) loginAs(pick.trim());
});

function loginAs(name){
  currentUser = name;
  const user = state.users[currentUser];
  document.getElementById('authCard').style.display='none';
  document.getElementById('mainArea').style.display='block';
  document.getElementById('logoutBtn').style.display='inline-block';
  document.getElementById('currentUserBox').innerText = `${user.display || user.name} (${user.role})`;
  renderAll();
}
document.getElementById('logoutBtn').addEventListener('click', ()=>{
  currentUser = null;
  document.getElementById('authCard').style.display='';
  document.getElementById('mainArea').style.display='none';
  document.getElementById('logoutBtn').style.display='none';
  document.getElementById('currentUserBox').innerText = '';
});

/* RENDER */
function renderAll(){ renderProfile(); renderPanels(); renderActivity(); renderChart(); }

function renderProfile(){
  const box = document.getElementById('profileBox');
  const avatar = document.getElementById('profileAvatar');
  const nameEl = document.getElementById('profileName');
  const roleEl = document.getElementById('profileRole');
  const starsEl = document.getElementById('profileStars');
  if(!currentUser){ box.innerHTML='เข้าสู่ระบบเพื่อดูโปรไฟล์'; avatar.innerText='LV'; nameEl.innerHTML='<strong>-</strong>'; roleEl.innerText='-'; starsEl.innerText='0'; return; }
  const user = state.users[currentUser];
  avatar.innerText = (user.display||user.name).slice(0,2).toUpperCase();
  nameEl.innerHTML = `<strong>${user.display || user.name}</strong>`;
  roleEl.innerText = `${user.role}`;
  starsEl.innerText = user.stars || 0;
  let html = `<div style="margin-top:8px" class="meta">บันทึกล่าสุด:</div>`;
  if(user.moods && user.moods.length){
    const last = user.moods[user.moods.length-1];
    html += `<div style="margin-top:6px">${last.time} — ${last.emoji} ${last.label}</div><div class="muted small" style="margin-top:6px">${last.note || '-'}</div>`;
  } else html += `<div class="muted small" style="margin-top:6px">ยังไม่มีบันทึก</div>`;
  box.innerHTML = html;
}

/* PANELS */
function renderPanels(){
  if(!currentUser) return;
  const user = state.users[currentUser];
  document.getElementById('studentPanel').style.display = user.role==='student' ? 'block' : 'none';
  document.getElementById('teacherPanel').style.display = user.role==='teacher' ? 'block' : 'none';
  if(user.role==='student'){
    renderMoodUI(user);
    document.getElementById('myStars').innerText = user.stars || 0;
    renderRedeemHistory(user);
    renderApptHistory(user);
    renderQuizUI(user);
  } else {
    renderStudentsList();
    renderApptRequests();
    renderReportsList();
    buildReportStudentSelect();
  }
  renderQuickPanel();
}

function renderActivity(){
  const el = document.getElementById('activityLog');
  el.innerHTML = state.activity.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time}</div><div>${a.txt}</div></div>`).join('');
}

/* Mood UI */
function renderMoodUI(user){
  const container = document.getElementById('moodButtons');
  container.innerHTML = '';
  emojiChoices.forEach(e=>{
    const btn = document.createElement('button');
    btn.className='emoji-btn';
    btn.dataset.key = e.key;
    btn.innerHTML = `<div>${e.emoji}</div><div class="label">${e.label}</div>`;
    btn.addEventListener('click', ()=> {
      document.querySelectorAll('.emoji-btn').forEach(b=>b.classList.remove('selected'));
      btn.classList.add('selected');
      btn.dataset.selected = '1';
    });
    container.appendChild(btn);
  });
  const last = user.moods.length ? user.moods[user.moods.length-1] : null;
  document.getElementById('lastMoodText').innerText = last ? `${last.emoji} ${last.label} — ${last.time}` : '-';
}

document.getElementById('saveMoodBtn').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const user = state.users[currentUser];
  const sel = document.querySelector('.emoji-btn.selected');
  if(!sel) return alert('กรุณาเลือกรูปอารมณ์');
  const key = sel.dataset.key;
  const meta = emojiChoices.find(e=>e.key===key);
  const note = document.getElementById('diaryText').value.trim();
  const entry = { time: new Date().toLocaleString(), key, emoji: meta.emoji, label: meta.label, note };
  user.moods.push(entry);
  if(note) user.diaries.push({time:entry.time, text:note});
  saveState();
  logActivity(`${currentUser} บันทึกอารมณ์: ${meta.emoji} ${meta.label}`);
  renderAll();
  document.getElementById('diaryText').value='';
  document.querySelectorAll('.emoji-btn').forEach(b=>b.classList.remove('selected'));
});

/* Redeem */
document.getElementById('openRedeem').addEventListener('click', ()=>{
  const panel = document.getElementById('redeemPanel');
  panel.style.display = panel.style.display==='none' ? 'block' : 'none';
});
function renderRedeemHistory(user){
  const el = document.getElementById('redeemHistory');
  if(!user.redeemHistory || !user.redeemHistory.length) { el.innerHTML = '<div class="muted small">ยังไม่มีการแลก</div>'; return; }
  el.innerHTML = user.redeemHistory.map(r=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div><strong>${r.item}</strong> <span class="meta">(${r.cost} ⭐)</span></div><div class="meta">${r.time}</div></div>`).join('');
}
document.addEventListener('click', (e)=>{
  if(e.target && e.target.matches('.redeemBtn')){
    const name = e.target.dataset.name;
    const cost = parseInt(e.target.dataset.cost);
    if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
    const user = state.users[currentUser];
    if(user.stars < cost) return alert('ดาวไม่พอสำหรับการแลก');
    if(!confirm(`ต้องการใช้ ${cost} ดาว เพื่อแลก "${name}" หรือไม่?`)) return;
    user.stars -= cost;
    user.redeemHistory.push({item:name,cost,time:new Date().toLocaleString()});
    saveState();
    logActivity(`${currentUser} แลก: ${name} (-${cost} ⭐)`);
    renderAll();
  }
});

/* Appointments */
document.getElementById('requestAppt').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const teacher = document.getElementById('apptTeacher').value.trim();
  const msg = document.getElementById('apptMsg').value.trim();
  if(!teacher || !msg) return alert('กรุณากรอกชื่อครูและข้อความนัด');
  const user = state.users[currentUser];
  const appt = {id:generateId(), teacher, msg, time:new Date().toLocaleString(), status:'pending', student:currentUser, teacherNote:''};
  user.appts.push(appt);
  if(state.users[teacher] && state.users[teacher].role==='teacher'){
    state.users[teacher].inbox = state.users[teacher].inbox || [];
    state.users[teacher].inbox.push(appt);
  } else {
    if(!state.users[teacher]) state.users[teacher] = { name:teacher, display:teacher, role:'teacher', stars:0, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[], inbox:[appt] };
    else { state.users[teacher].inbox = state.users[teacher].inbox || []; state.users[teacher].inbox.push(appt); }
  }
  saveState();
  logActivity(`${currentUser} ขอเข้าปรึกษากับ ${teacher}`);
  renderAll();
  document.getElementById('apptTeacher').value=''; document.getElementById('apptMsg').value='';
});
function renderApptHistory(user){
  const el = document.getElementById('apptHistory');
  if(!user.appts || !user.appts.length) { el.innerHTML='<div class="muted small">ยังไม่มีการขอนัด</div>'; return; }
  el.innerHTML = user.appts.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div><strong>ถึง: ${a.teacher}</strong> <span class="meta">[${a.status}]</span></div><div class="meta">${a.time}</div><div>${a.msg}</div><div class="meta">หมายเหตุครู: ${a.teacherNote||'-'}</div></div>`).join('');
}

/* Teacher inbox */
function renderApptRequests(){
  const el = document.getElementById('apptRequests');
  if(!currentUser) return;
  const user = state.users[currentUser];
  const inbox = user.inbox || [];
  if(!inbox.length) { el.innerHTML='<div class="muted small">ยังไม่มีคำขอนัด</div>'; return; }
  el.innerHTML = inbox.map(a=>{
    return `<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time} — จาก: <strong>${a.student}</strong></div><div style="margin-top:6px">${a.msg}</div><div style="margin-top:8px">${a.status==='approved' ? '<span class="badge">อนุมัติ</span>' : `<button class="approveBtn" data-id="${a.id}">อนุมัติ</button><button class="rejectBtn" data-id="${a.id}">ปฏิเสธ</button>`} <button class="noteBtn" data-id="${a.id}">บันทึกหมายเหตุ</button></div></div>`;
  }).join('');
  document.querySelectorAll('.approveBtn').forEach(b=>b.addEventListener('click', (e)=> handleApptAction(e.target.dataset.id,'approved')));
  document.querySelectorAll('.rejectBtn').forEach(b=>b.addEventListener('click', (e)=> handleApptAction(e.target.dataset.id,'rejected')));
  document.querySelectorAll('.noteBtn').forEach(b=>b.addEventListener('click', (e)=> {
    const id = e.target.dataset.id;
    const note = prompt('บันทึกหมายเหตุสำหรับการนัด (จะไปลงในข้อมูลนัดของนักเรียน):');
    if(note!==null) handleApptNote(id,note);
  }));
}
function handleApptAction(id,status){
  const t = state.users[currentUser];
  const item = (t.inbox||[]).find(x=>x.id===id);
  if(!item) return;
  item.status = status;
  const stu = state.users[item.student];
  if(stu){
    const ap = stu.appts.find(x=>x.id===id);
    if(ap) ap.status = status;
  }
  saveState();
  logActivity(`${currentUser} ${status==='approved'?'อนุมัติ':'ปฏิเสธ'} นัดจาก ${item.student}`);
  renderAll();
}
function handleApptNote(id,note){
  const t = state.users[currentUser];
  const item = (t.inbox||[]).find(x=>x.id===id);
  if(!item) return;
  item.teacherNote = note;
  const stu = state.users[item.student];
  if(stu){
    const ap = stu.appts.find(x=>x.id===id);
    if(ap) ap.teacherNote = note;
  }
  saveState();
  logActivity(`${currentUser} บันทึกหมายเหตุนัด (${item.student})`);
  renderAll();
}

/* Students list & star management */
function renderStudentsList(){
  const q = document.getElementById('searchStudent').value.trim().toLowerCase();
  const container = document.getElementById('studentsList');
  const students = Object.values(state.users).filter(u=>u.role==='student' && (!q || (u.display||u.name).toLowerCase().includes(q)));
  if(!students.length) { container.innerHTML='<div class="muted small">ไม่มีนักเรียน</div>'; return; }
  container.innerHTML = students.map(s=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb;display:flex;align-items:center;gap:10px"><div class="student-avatar">${(s.display||s.name).slice(0,2).toUpperCase()}</div><div><strong>${s.display||s.name}</strong><div class="meta">ดาว: ${s.stars||0}</div></div><div class="right"><button class="addStar" data-name="${s.name}">ให้ +1</button><button class="removeStar" data-name="${s.name}">-1</button><button class="viewProfile" data-name="${s.name}">ดู</button></div></div>`).join('');
  document.querySelectorAll('.addStar').forEach(b=>b.addEventListener('click', e=>modifyStars(e.target.dataset.name,1)));
  document.querySelectorAll('.removeStar').forEach(b=>b.addEventListener('click', e=>modifyStars(e.target.dataset.name,-1)));
  document.querySelectorAll('.viewProfile').forEach(b=>b.addEventListener('click', e=>viewStudentProfile(e.target.dataset.name)));
}
document.getElementById('searchStudent').addEventListener('input', renderStudentsList);

function modifyStars(studentName,delta){
  const s = state.users[studentName];
  if(!s) return alert('ไม่พบชื่อ');
  s.stars = Math.max(0,(s.stars||0)+delta);
  saveState();
  logActivity(`${currentUser} ปรับดาวให้ ${studentName}: ${delta>0?'+':''}${delta}`);
  renderAll();
}
function viewStudentProfile(name){
  const s = state.users[name];
  if(!s) return;
  let txt = `โปรไฟล์: ${s.display||s.name}\nบทบาท: ${s.role}\nดาว: ${s.stars}\n\nบันทึกล่าสุด:\n`;
  if(s.moods.length) txt += `${s.moods[s.moods.length-1].time} ${s.moods[s.moods.length-1].emoji} ${s.moods[s.moods.length-1].label}\n\n`;
  txt += 'ประวัติการแลก:\n';
  (s.redeemHistory||[]).forEach(r=> txt += `${r.time} - ${r.item} (-${r.cost})\n`);
  alert(txt);
}

/* Reports */
document.getElementById('sendReport').addEventListener('click', ()=>{
  const student = document.getElementById('reportStudent').value;
  const text = document.getElementById('reportText').value.trim();
  if(!student || !text) return alert('เลือกนักเรียนและกรอกข้อความ');
  const r = {id:generateId(), teacher:currentUser, student, text, time:new Date().toLocaleString()};
  state.users[student].reports = state.users[student].reports || [];
  state.users[student].reports.push(r);
  saveState();
  logActivity(`${currentUser} ส่งรายงานพฤติกรรม: ${student}`);
  renderAll();
  document.getElementById('reportText').value='';
});
function renderReportsList(){
  const all = [];
  Object.values(state.users).forEach(u=>{
    if(u.reports) u.reports.forEach(r=>{
      if(r.teacher===currentUser) all.push(r);
    });
  });
  const el = document.getElementById('reportsList');
  if(!all.length) { el.innerHTML='<div class="muted small">ยังไม่มีรายงาน</div>'; return; }
  el.innerHTML = all.map(r=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${r.time} → ${r.student}</div><div>${r.text}</div></div>`).join('');
}
function buildReportStudentSelect(){
  const sel = document.getElementById('reportStudent');
  sel.innerHTML = '<option value="">-- เลือกนักเรียน --</option>';
  Object.values(state.users).filter(u=>u.role==='student').forEach(s=>{
    const opt = document.createElement('option'); opt.value=s.name; opt.innerText = s.display || s.name; sel.appendChild(opt);
  });
}

/* Quiz */
const sampleQuiz = [
  {q:'ในสัปดาห์ที่ผ่านมาคุณรู้สึกมีความสุขบ่อยแค่ไหน?', options:['ไม่เลย','บางครั้ง','บ่อย','ตลอดเวลา'], scores:[0,1,2,3]},
  {q:'คุณนอนหลับเพียงพอหรือไม่?', options:['ไม่เลย','บางครั้ง','พอประมาณ','เพียงพอ'], scores:[0,1,2,3]},
  {q:'คุณรู้สึกว่ามีคนคอยรับฟังเมื่อคุณต้องการหรือไม่?', options:['ไม่เลย','บางครั้ง','บ่อย','ตลอดเวลา'], scores:[0,1,2,3]}
];
function renderQuizUI(user){
  const qEl = document.getElementById('quizQuestions');
  qEl.innerHTML = '';
  sampleQuiz.forEach((qq,i)=>{
    const div = document.createElement('div');
    div.style.padding='8px';
    div.innerHTML = `<div><strong>Q${i+1}.</strong> ${qq.q}</div>`;
    qq.options.forEach((opt,j)=>{
      const id = `q${i}_o${j}`;
      div.innerHTML += `<div style="margin-left:8px"><input type="radio" name="q${i}" id="${id}" value="${j}"> <label for="${id}">${opt}</label></div>`;
    });
    qEl.appendChild(div);
  });
  document.getElementById('startQuiz').style.display='inline-block';
  document.getElementById('submitQuiz').style.display='none';
  document.getElementById('quizResult').innerHTML='';
}
document.getElementById('startQuiz').addEventListener('click', ()=>{
  document.getElementById('startQuiz').style.display='none';
  document.getElementById('submitQuiz').style.display='inline-block';
});
document.getElementById('submitQuiz').addEventListener('click', ()=>{
  if(!currentUser) return;
  let total=0; let answered=true;
  sampleQuiz.forEach((qq,i)=>{
    const v = document.querySelector(`input[name="q${i}"]:checked`);
    if(!v) answered=false;
    else total += qq.scores[parseInt(v.value)];
  });
  if(!answered) return alert('กรุณาตอบทุกข้อ');
  state.users[currentUser].quiz.push({time:new Date().toLocaleString(), score:total});
  saveState();
  logActivity(`${currentUser} ทำแบบทดสอบ (คะแนน ${total})`);
  document.getElementById('quizResult').innerHTML = `<div class="small">ผลคะแนน: <strong>${total}</strong></div><div class="meta">คำแนะนำ: ${total<=2?'ควรสนใจและสังเกตอารมณ์เพิ่มเติม':'สภาพทั่วไปปกติ'}</div>`;
  document.getElementById('submitQuiz').style.display='none';
  document.getElementById('startQuiz').style.display='inline-block';
});

/* Chart */
function renderChart(){
  const ctx = document.getElementById('moodChart');
  if(!ctx) return;
  const moodCounts = {};
  emojiChoices.forEach(e=>moodCounts[e.label]=0);
  Object.values(state.users).filter(u=>u.role==='student').forEach(s=>{
    if(s.moods && s.moods.length){
      const last = s.moods[s.moods.length-1];
      moodCounts[last.label] = (moodCounts[last.label]||0)+1;
    }
  });
  const labels = Object.keys(moodCounts);
  const data = Object.values(moodCounts);
  if(moodChart) moodChart.destroy();
  moodChart = new Chart(ctx, {
    type:'doughnut',
    data: {
      labels, datasets:[{data, backgroundColor:['#6c63ff','#7dd3fc','#c4b5fd','#fda4af','#ffb020','#94a3b8']}]
    },
    options:{responsive:true,plugins:{legend:{position:'bottom'}}}
  });
}

/* Quick panel */
function renderQuickPanel(){
  const el = document.getElementById('quickPanel');
  if(!currentUser){ el.innerHTML=''; return; }
  const u = state.users[currentUser];
  let html = `<div class="meta">บทบาท: ${u.role}</div>`;
  if(u.role==='student'){
    html += `<div style="margin-top:6px"><strong>ดาว: ${u.stars}</strong></div>`;
    html += `<div class="meta" style="margin-top:6px">บันทึก: ${u.diaries.length || 0} ครั้ง</div>`;
  } else {
    const pending = (u.inbox||[]).filter(i=>i.status==='pending').length;
    html += `<div style="margin-top:6px"><strong>คำขอนัดรออนุมัติ: ${pending}</strong></div>`;
    const reports = Object.values(state.users).reduce((acc,usr)=> acc + ((usr.reports||[]).filter(r=>r.teacher===currentUser).length),0);
    html += `<div class="meta" style="margin-top:6px">รายงานที่ส่ง: ${reports}</div>`;
  }
  el.innerHTML = html;
}

/* UTIL */
function generateId(){ return 'id_' + Math.random().toString(36).slice(2,9); }

/* initial render */
renderActivity();
renderAll();

/* keep chart updated occasionally */
setInterval(()=>{ renderChart(); },5000);

/* make sure student list available */
renderStudentsList();

</script>
</body>
</html>
