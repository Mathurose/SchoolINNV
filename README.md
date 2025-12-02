<!doctype html>
<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>ระบบติดตามอารมณ์และพฤติกรรมนักเรียน (Prototype)</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#f6f9ff; --card:#ffffff; --accent:#4a90e2; --muted:#6b7280;
  }
  body{font-family:"Kanit",sans-serif;background:linear-gradient(180deg,#eef6ff 0%,#f9fbff 100%);margin:0;padding:20px;color:#111;}
  .container{max-width:1100px;margin:0 auto;}
  header{display:flex;align-items:center;justify-content:space-between;margin-bottom:18px;}
  h1{font-size:20px;margin:0;color:var(--accent)}
  .card{background:var(--card);box-shadow:0 6px 18px rgba(40,40,60,0.06);border-radius:12px;padding:16px;margin-bottom:16px;}
  .two-cols{display:flex;gap:16px;}
  .col{flex:1;}
  label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px;}
  input[type=text],textarea,select{width:100%;padding:10px;border-radius:8px;border:1px solid #e6e9ef;font-size:14px;}
  button{background:var(--accent);color:#fff;border:0;padding:10px 12px;border-radius:8px;cursor:pointer;}
  .btn-ghost{background:transparent;color:var(--accent);border:1px solid #dbe8ff;}
  .emoji-btn{font-size:28px;padding:8px 12px;border-radius:10px;border:1px solid transparent;cursor:pointer;background:transparent;}
  .emoji-btn.selected{background:linear-gradient(90deg,#fff6e6,#fff);box-shadow:0 6px 14px rgba(74,144,226,0.08);border-color:#ffd27a;}
  .small{font-size:13px;color:var(--muted);}
  .flex{display:flex;gap:8px;align-items:center;}
  .right{margin-left:auto;}
  .list{max-height:300px;overflow:auto;}
  table{width:100%;border-collapse:collapse;}
  th,td{padding:8px;border-bottom:1px solid #f0f3f8;text-align:left;font-size:14px;}
  .badge{background:#eef7ff;color:var(--accent);padding:6px 8px;border-radius:8px;font-weight:600;}
  .actions button{margin-right:8px;}
  .muted{color:var(--muted)}
  @media(max-width:900px){.two-cols{flex-direction:column}}
</style>
</head>
<body>
<div class="container">
  <header>
    <h1>ระบบติดตามอารมณ์และพฤติกรรม</h1>
    <div id="currentUserBox" class="small"></div>
  </header>

  <div id="authCard" class="card">
    <h3>เข้าสู่ระบบ / สร้างบัญชี</h3>
    <div style="display:flex;gap:8px;flex-wrap:wrap;align-items:center">
      <div style="flex:1;min-width:220px">
        <label>ชื่อผู้ใช้</label>
        <input id="username" type="text" placeholder="เช่น pornpim, ajarn_nu" />
      </div>
      <div style="width:160px">
        <label>บทบาท</label>
        <select id="role">
          <option value="student">นักเรียน</option>
          <option value="teacher">ครู</option>
        </select>
      </div>
      <div style="display:flex;align-items:flex-end;gap:8px">
        <button id="loginBtn">เข้าสู่ระบบ / สร้างบัญชี</button>
        <button id="logoutBtn" class="btn-ghost" style="display:none">ออกจากระบบ</button>
      </div>
    </div>
    <p class="small muted" style="margin-top:10px">หมายเหตุ: ข้อมูลจะถูกเก็บในเครื่อง (localStorage) สำหรับต้นแบบนี้</p>
  </div>

  <div id="mainArea" style="display:none">
    <div class="two-cols">
      <div class="col">
        <!-- Student view -->
        <div id="studentPanel" class="card" style="display:none">
          <h3>แผงนักเรียน</h3>

          <div class="card">
            <label>บันทึกอารมณ์ประจำวัน (เลือกรูปอิโมจิ)</label>
            <div id="moodButtons" class="flex" style="flex-wrap:wrap">
              <!-- emojis inserted by JS -->
            </div>
            <label style="margin-top:8px">ข้อความสั้น ๆ / My diary</label>
            <textarea id="diaryText" rows="3" placeholder="เขียนบันทึกวันนี้..."></textarea>
            <div style="display:flex;align-items:center;margin-top:8px">
              <button id="saveMoodBtn">บันทึกอารมณ์และบันทึก</button>
              <div class="muted small" style="margin-left:12px">บันทึกล่าสุด: <span id="lastMoodText">-</span></div>
            </div>
          </div>

          <div class="card">
            <h4>ดาวเด็กดี</h4>
            <div style="display:flex;align-items:center;gap:12px">
              <div><span class="badge">⭐ <span id="myStars">0</span></span></div>
              <div class="muted small">สะสมและนำไปแลกของรางวัลได้</div>
              <button id="openRedeem" class="right btn-ghost">แลกของรางวัล</button>
            </div>
            <div id="redeemPanel" style="display:none;margin-top:12px">
              <div style="display:flex;gap:8px;flex-wrap:wrap">
                <div class="card" style="padding:10px">
                  <div><strong>เวลาพัก 5 นาที</strong></div>
                  <div class="small muted">ใช้ 10 ดาว</div>
                  <button class="redeemBtn" data-name="เวลาพัก 5 นาที" data-cost="10">แลก</button>
                </div>
                <div class="card" style="padding:10px">
                  <div><strong>คูปองเครื่องเขียน</strong></div>
                  <div class="small muted">ใช้ 12 ดาว</div>
                  <button class="redeemBtn" data-name="คูปองเครื่องเขียน" data-cost="12">แลก</button>
                </div>
                <div class="card" style="padding:10px">
                  <div><strong>คูปองเครื่องดื่ม/อาหาร</strong></div>
                  <div class="small muted">ใช้ 15 ดาว</div>
                  <button class="redeemBtn" data-name="คูปองอาหาร" data-cost="15">แลก</button>
                </div>
              </div>
              <h5 style="margin-top:12px">ประวัติการแลก</h5>
              <div id="redeemHistory" class="list"></div>
            </div>
          </div>

          <div class="card">
            <h4>นัดหมายปรึกษา</h4>
            <label>เลือกครูที่จะนัด (ใส่ชื่อครู)</label>
            <input id="apptTeacher" placeholder="ชื่อครูที่ปรึกษา/ครูแนะแนว" />
            <label style="margin-top:8px">ข้อความสำหรับนัด</label>
            <input id="apptMsg" placeholder="สาเหตุ/หัวข้อที่ต้องการปรึกษา" />
            <div style="display:flex;gap:8px;margin-top:8px">
              <button id="requestAppt">ส่งคำขอนัด</button>
              <div class="muted small">ประวัติการขอนัด</div>
            </div>
            <div id="apptHistory" class="list" style="margin-top:8px"></div>
          </div>

          <div class="card">
            <h4>แบบทดสอบจิตวิทยาเบื้องต้น</h4>
            <p class="small muted">คำถามสั้น ๆ เพื่อประเมินอารมณ์</p>
            <div id="quizPanel">
              <div id="quizQuestions"></div>
              <div style="display:flex;gap:8px;margin-top:8px">
                <button id="startQuiz">เริ่มแบบทดสอบ</button>
                <button id="submitQuiz" style="display:none">ส่งคำตอบ</button>
              </div>
              <div id="quizResult" style="margin-top:8px"></div>
            </div>
          </div>

        </div>

        <!-- Teacher view -->
        <div id="teacherPanel" class="card" style="display:none">
          <h3>แผงครู</h3>

          <div class="card">
            <h4>ค้นหานักเรียน / รายการนักเรียน</h4>
            <label>ค้นหาชื่อ</label>
            <input id="searchStudent" placeholder="ค้นหา..." />
            <div id="studentsList" class="list" style="margin-top:8px"></div>
          </div>

          <div class="card">
            <h4>กล่องขอนัดหมายจากนักเรียน</h4>
            <div id="apptRequests" class="list"></div>
          </div>

          <div class="card">
            <h4>บันทึกรายงานพฤติกรรม</h4>
            <label>เลือกนักเรียน</label>
            <select id="reportStudent"></select>
            <label style="margin-top:8px">ข้อความรายงาน</label>
            <textarea id="reportText" rows="3"></textarea>
            <div style="margin-top:8px"><button id="sendReport">บันทึกและส่ง</button></div>
            <div id="reportsList" style="margin-top:8px" class="list"></div>
          </div>

          <div class="card">
            <h4>สถิติภาพรวมอารมณ์</h4>
            <canvas id="moodChart" height="140"></canvas>
          </div>
        </div>

      </div>

      <div class="col">
        <div class="card">
          <h3>ข้อมูลโปรไฟล์</h3>
          <div id="profileBox">
            <!-- dynamic -->
          </div>
        </div>

        <div class="card">
          <h3>แดชบอร์ดด่วน</h3>
          <div id="quickPanel"></div>
        </div>

        <div class="card">
          <h3>กิจกรรมล่าสุด (Log)</h3>
          <div id="activityLog" class="list"></div>
        </div>

      </div>
    </div>
  </div>

  <footer style="margin-top:18px" class="small muted">ต้นแบบ — เก็บข้อมูลในเครื่อง (localStorage). หากต้องการเชื่อมเซิร์ฟเวอร์ บอกผมได้เลย</footer>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
/*
  Prototype system:
  - Data stored in localStorage under key "mood_system_data"
  - Structure:
    { users: { username: {name, role, stars, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] } }, activity:[] }
*/

const STORAGE_KEY = 'mood_system_data_v1';
const emojiChoices = [
  {key:'very_happy', emoji:'😄', label:'มีความสุขมาก'},
  {key:'happy', emoji:'🙂', label:'ปกติ/สบายใจ'},
  {key:'neutral', emoji:'😐', label:'เฉย ๆ'},
  {key:'sad', emoji:'😢', label:'เศร้า'},
  {key:'angry', emoji:'😠', label:'โกรธ'},
  {key:'tired', emoji:'😴', label:'เหนื่อย/หมดแรง'}
];

let state = loadState();
let currentUser = null;
let moodChart = null;

function defaultState(){
  return { users: {}, activity: [] };
}
function loadState(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(!raw) return defaultState();
    return JSON.parse(raw);
  }catch(e){ return defaultState(); }
}
function saveState(){
  localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
}

function logActivity(txt){
  const time = new Date().toLocaleString();
  state.activity.unshift({txt,time});
  saveState();
  renderActivity();
}

/* Auth */
document.getElementById('loginBtn').addEventListener('click', ()=>{
  const name = document.getElementById('username').value.trim();
  const role = document.getElementById('role').value;
  if(!name){ alert('กรุณากรอกชื่อผู้ใช้'); return; }
  let user = state.users[name];
  if(!user){
    // create
    user = { name, role, stars:0, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
    state.users[name] = user;
    saveState();
    logActivity(`สร้างบัญชีใหม่: ${name} (${role})`);
  }
  currentUser = name;
  document.getElementById('authCard').style.display='none';
  document.getElementById('mainArea').style.display='block';
  document.getElementById('logoutBtn').style.display='inline-block';
  document.getElementById('currentUserBox').innerText = `เข้าสู่ระบบ: ${name} (${user.role})`;
  renderAll();
});
document.getElementById('logoutBtn').addEventListener('click', ()=>{
  currentUser = null;
  document.getElementById('authCard').style.display='';
  document.getElementById('mainArea').style.display='none';
  document.getElementById('logoutBtn').style.display='none';
  document.getElementById('currentUserBox').innerText = '';
});

/* Render */
function renderAll(){
  renderProfile();
  renderPanels();
  renderActivity();
  renderChart();
}

function renderProfile(){
  const box = document.getElementById('profileBox');
  if(!currentUser){ box.innerHTML=''; return; }
  const user = state.users[currentUser];
  let html = `<div><strong>${user.name}</strong> <span class="small muted">(${user.role})</span></div>`;
  html += `<div class="small muted" style="margin-top:6px">ดาวสะสม: <span class="badge">⭐ <span id="profileStars">${user.stars}</span></span></div>`;
  html += `<div style="margin-top:8px"><strong>บันทึกล่าสุด</strong></div>`;
  if(user.moods.length>0){
    const last = user.moods[user.moods.length-1];
    html += `<div class="muted small">${last.time} — ${last.emoji} ${last.label} </div>`;
    html += `<div class="small" style="margin-top:6px;color:#333">${last.note||''}</div>`;
  } else { html += `<div class="muted small">ยังไม่มีบันทึก</div>`; }
  box.innerHTML = html;
}

function renderPanels(){
  if(!currentUser) return;
  const user = state.users[currentUser];
  document.getElementById('studentPanel').style.display = user.role==='student' ? 'block' : 'none';
  document.getElementById('teacherPanel').style.display = user.role==='teacher' ? 'block' : 'none';
  // student specifics
  if(user.role==='student'){
    renderMoodUI(user);
    document.getElementById('myStars').innerText = user.stars;
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
  el.innerHTML = state.activity.map(a=>`<div style="padding:6px;border-bottom:1px solid #f3f6fb"><div class="small muted">${a.time}</div><div>${a.txt}</div></div>`).join('');
}

/* Mood UI */
function renderMoodUI(user){
  const container = document.getElementById('moodButtons');
  container.innerHTML = '';
  emojiChoices.forEach(e=>{
    const btn = document.createElement('button');
    btn.className='emoji-btn';
    btn.dataset.key = e.key;
    btn.innerHTML = `${e.emoji}<div class="small muted" style="font-size:11px">${e.label}</div>`;
    btn.addEventListener('click', ()=> {
      // toggle selection
      document.querySelectorAll('.emoji-btn').forEach(b=>b.classList.remove('selected'));
      btn.classList.add('selected');
      btn.dataset.selected = '1';
    });
    container.appendChild(btn);
  });
  // last mood text
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

/* Stars / Redeem */
document.getElementById('openRedeem').addEventListener('click', ()=>{
  const panel = document.getElementById('redeemPanel');
  panel.style.display = panel.style.display==='none' ? 'block' : 'none';
});
function renderRedeemHistory(user){
  const el = document.getElementById('redeemHistory');
  if(!user.redeemHistory.length) { el.innerHTML = '<div class="muted small">ยังไม่มีการแลก</div>'; return; }
  el.innerHTML = user.redeemHistory.map(r=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div><strong>${r.item}</strong> <span class="small muted">(${r.cost} ⭐)</span></div><div class="small muted">${r.time}</div></div>`).join('');
}
document.querySelectorAll('.redeemBtn').forEach(btn=>{
  btn && btn.addEventListener('click', (e)=>{
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
  });
});

/* Appointments */
document.getElementById('requestAppt').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const teacher = document.getElementById('apptTeacher').value.trim();
  const msg = document.getElementById('apptMsg').value.trim();
  if(!teacher || !msg) return alert('กรุณากรอกชื่อครูและข้อความนัด');
  const user = state.users[currentUser];
  // add to student's appts as status 'pending'
  const appt = {id:generateId(), teacher, msg, time:new Date().toLocaleString(), status:'pending', student:currentUser, teacherNote:''};
  user.appts.push(appt);
  // also store globally in a simple way by adding to teacher's inbox if teacher exists
  if(state.users[teacher] && state.users[teacher].role==='teacher'){
    state.users[teacher].inbox = state.users[teacher].inbox || [];
    state.users[teacher].inbox.push(appt);
  } else {
    // teacher not registered yet, create a placeholder teacher account
    if(!state.users[teacher]) {
      state.users[teacher] = { name:teacher, role:'teacher', stars:0, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[], inbox:[appt] };
    } else {
      state.users[teacher].inbox = state.users[teacher].inbox || [];
      state.users[teacher].inbox.push(appt);
    }
  }
  saveState();
  logActivity(`${currentUser} ขอเข้าปรึกษากับ ${teacher}`);
  renderAll();
  document.getElementById('apptTeacher').value=''; document.getElementById('apptMsg').value='';
});
function renderApptHistory(user){
  const el = document.getElementById('apptHistory');
  if(!user.appts.length) { el.innerHTML='<div class="muted small">ยังไม่มีการขอนัด</div>'; return; }
  el.innerHTML = user.appts.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div><strong>ถึง: ${a.teacher}</strong> <span class="small muted">[${a.status}]</span></div><div class="small muted">${a.time}</div><div>${a.msg}</div><div class="small muted">หมายเหตุครู: ${a.teacherNote||'-'}</div></div>`).join('');
}

/* Teacher: view appt requests */
function renderApptRequests(){
  const el = document.getElementById('apptRequests');
  if(!currentUser) return;
  const user = state.users[currentUser];
  const inbox = user.inbox || [];
  if(!inbox.length) { el.innerHTML='<div class="muted small">ยังไม่มีคำขอนัด</div>'; return; }
  el.innerHTML = inbox.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div><strong>จาก: ${a.student}</strong> <span class="small muted">${a.time}</span></div><div>${a.msg}</div><div style="margin-top:6px">${a.status==='approved' ? '<span class="badge">อนุมัติ</span>' : `<button class="approveBtn" data-id="${a.id}">อนุมัติ</button><button class="rejectBtn" data-id="${a.id}">ปฏิเสธ</button>`} <button class="noteBtn" data-id="${a.id}">บันทึกหมายเหตุ</button></div></div>`).join('');
  // attach events
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
  // reflect to student's appt record
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
  // reflect to student's appt
  const stu = state.users[item.student];
  if(stu){
    const ap = stu.appts.find(x=>x.id===id);
    if(ap) ap.teacherNote = note;
  }
  saveState();
  logActivity(`${currentUser} บันทึกหมายเหตุนัด (${item.student})`);
  renderAll();
}

/* Teacher: students list & star management */
function renderStudentsList(){
  const q = document.getElementById('searchStudent').value.trim().toLowerCase();
  const container = document.getElementById('studentsList');
  const students = Object.values(state.users).filter(u=>u.role==='student' && (!q || u.name.toLowerCase().includes(q)));
  if(!students.length) { container.innerHTML='<div class="muted small">ไม่มีนักเรียน</div>'; return; }
  container.innerHTML = students.map(s=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div style="display:flex;align-items:center"><div><strong>${s.name}</strong><div class="small muted">ดาว: ${s.stars}</div></div><div class="right actions"><button class="addStar" data-name="${s.name}">ให้ดาว +1</button><button class="removeStar" data-name="${s.name}">ลดดาว -1</button><button class="viewProfile" data-name="${s.name}">ดู</button></div></div></div>`).join('');
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
  // show a simple modal-like prompt
  let txt = `โปรไฟล์: ${s.name}\nบทบาท: ${s.role}\nดาว: ${s.stars}\n\nบันทึกล่าสุด:\n`;
  if(s.moods.length) txt += `${s.moods[s.moods.length-1].time} ${s.moods[s.moods.length-1].emoji} ${s.moods[s.moods.length-1].label}\n\n`;
  txt += 'ประวัติการแลก:\n';
  s.redeemHistory.forEach(r=> txt += `${r.time} - ${r.item} (-${r.cost})\n`);
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
  // show all reports teacher created
  const all = [];
  Object.values(state.users).forEach(u=>{
    if(u.reports) u.reports.forEach(r=>{
      if(r.teacher===currentUser) all.push(r);
    });
  });
  const el = document.getElementById('reportsList');
  if(!all.length) { el.innerHTML='<div class="muted small">ยังไม่มีรายงาน</div>'; return; }
  el.innerHTML = all.map(r=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="small muted">${r.time} → ${r.student}</div><div>${r.text}</div></div>`).join('');
}
function buildReportStudentSelect(){
  const sel = document.getElementById('reportStudent');
  sel.innerHTML = '<option value="">-- เลือกนักเรียน --</option>';
  Object.values(state.users).filter(u=>u.role==='student').forEach(s=>{
    const opt = document.createElement('option'); opt.value=s.name; opt.innerText = s.name; sel.appendChild(opt);
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
  let total=0;
  let answered=true;
  sampleQuiz.forEach((qq,i)=>{
    const v = document.querySelector(`input[name="q${i}"]:checked`);
    if(!v) answered=false;
    else total += qq.scores[parseInt(v.value)];
  });
  if(!answered) return alert('กรุณาตอบทุกข้อ');
  // store
  state.users[currentUser].quiz.push({time:new Date().toLocaleString(), score:total});
  saveState();
  logActivity(`${currentUser} ทำแบบทดสอบ (คะแนน ${total})`);
  document.getElementById('quizResult').innerHTML = `<div class="small">ผลคะแนน: <strong>${total}</strong></div><div class="muted small">คำแนะนำ: ${total<=2?'ควรสนใจและสังเกตอารมณ์เพิ่มเติม':'สภาพทั่วไปปกติ'}</div>`;
  document.getElementById('submitQuiz').style.display='none';
  document.getElementById('startQuiz').style.display='inline-block';
});

/* Chart: mood distribution across all students */
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
    type:'bar',
    data: {
      labels, datasets:[{label:'จำนวนนักเรียน (บันทึกล่าสุด)', data, backgroundColor:labels.map(l=>randomColor())}]
    },
    options:{responsive:true,plugins:{legend:{display:false}}}
  });
}

/* Quick panel */
function renderQuickPanel(){
  const el = document.getElementById('quickPanel');
  if(!currentUser){ el.innerHTML=''; return; }
  const u = state.users[currentUser];
  let html = `<div class="small muted">บทบาท: ${u.role}</div>`;
  if(u.role==='student'){
    html += `<div style="margin-top:6px"><strong>ดาว: ${u.stars}</strong></div>`;
    html += `<div class="small" style="margin-top:6px">บันทึกชีวิต: ${u.diaries.length} ครั้ง</div>`;
  } else {
    // teacher
    const pending = (u.inbox||[]).filter(i=>i.status==='pending').length;
    html += `<div style="margin-top:6px"><strong>คำขอนัดรออนุมัติ: ${pending}</strong></div>`;
    const reports = Object.values(state.users).reduce((acc,usr)=> acc + ((usr.reports||[]).filter(r=>r.teacher===currentUser).length),0);
    html += `<div class="small muted" style="margin-top:6px">รายงานที่ส่ง: ${reports}</div>`;
  }
  el.innerHTML = html;
}

/* Utilities */
function generateId(){ return 'id_' + Math.random().toString(36).slice(2,9); }
function randomColor(){ return `hsl(${Math.floor(Math.random()*360)} 70% 60%)`; }

/* Initial Render */
renderActivity();
renderAll();

/* Expose simple admin function for testing (give stars) */
window.__giveStars = (username, n) => { if(state.users[username]){ state.users[username].stars += n; saveState(); renderAll(); } }

/* When teacher page loads, ensure students list updates */
setInterval(()=>{ /* keep charts up-to-date for demo */ renderChart(); },5000);

/* Render students list initially */
renderStudentsList();

</script>
</body>
</html>
