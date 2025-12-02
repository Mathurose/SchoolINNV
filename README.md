<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>LiteVibe — ระบบติดตามอารมณ์และพฤติกรรม</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;700&display=swap" rel="stylesheet">
<link rel="icon" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 viewBox=%220 0 100 100%22><text y=%22.9em%22 font-size=%2290%22>🌈</text></svg>">
<style>
  :root{
    --bg:#f6f9ff; --card:#ffffff; --primary:#6c63ff; --accent:#ff6fab; --muted:#6b7280;
    --success:#22c55e; --danger:#ef4444;
  }
  *{box-sizing:border-box}
  body{font-family:"Kanit",sans-serif;background:linear-gradient(180deg,#f3f7ff 0%,#ffffff 100%);margin:0;color:#0b1220}
  .app{max-width:1200px;margin:22px auto;padding:18px}
  header.app-header{display:flex;align-items:center;gap:12px;margin-bottom:18px}
  .logo{display:flex;align-items:center;gap:12px}
  .mark{width:58px;height:58px;border-radius:14px;background:linear-gradient(135deg,var(--primary),var(--accent));display:flex;align-items:center;justify-content:center;color:#fff;font-size:24px;box-shadow:0 12px 30px rgba(108,99,255,0.12)}
  h1{font-size:20px;margin:0;color:var(--primary)}
  .muted{color:var(--muted);font-size:13px}
  .card{background:var(--card);border-radius:14px;padding:14px;box-shadow:0 8px 26px rgba(13,20,39,0.04);margin-bottom:14px}
  .grid{display:grid;grid-template-columns:1fr 380px;gap:14px}
  label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
  input[type=text],select,textarea{width:100%;padding:10px;border-radius:10px;border:1px solid #eef3ff;background:linear-gradient(#fff,#fbfdff);font-size:14px}
  button{background:var(--primary);color:#fff;border:0;padding:10px 12px;border-radius:10px;cursor:pointer;font-weight:600}
  .btn-ghost{background:transparent;border:1px solid rgba(108,99,255,0.12);color:var(--primary)}
  .row{display:flex;gap:10px;align-items:center}
  .emoji-row{display:flex;gap:10px;flex-wrap:wrap}
  .emoji-btn{width:80px;height:80px;border-radius:16px;border:0;background:#fff;display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:30px;cursor:pointer;transition:all .18s;box-shadow:0 8px 20px rgba(16,24,45,0.06)}
  .emoji-btn .label{font-size:12px;margin-top:6px;color:var(--muted)}
  .emoji-btn:hover{transform:translateY(-6px)}
  .emoji-btn.selected{outline:4px solid rgba(108,99,255,0.12);box-shadow:0 20px 40px rgba(108,99,255,0.08)}
  .periods{display:flex;gap:8px;margin-top:8px}
  .periods button{background:transparent;color:var(--muted);border:1px solid #f1f5ff;padding:8px 10px;border-radius:10px}
  .periods button.active{background:linear-gradient(90deg,var(--primary),var(--accent));color:#fff;border:0}
  .chart-wrap{margin-top:12px}
  .badge{background:linear-gradient(90deg,#fff,#fff);color:var(--primary);padding:6px 10px;border-radius:999px;border:1px solid rgba(108,99,255,0.08);font-weight:700}
  .student-avatar{width:48px;height:48px;border-radius:12px;background:linear-gradient(135deg,#fff,#f7fbff);display:inline-flex;align-items:center;justify-content:center;font-weight:700;color:var(--primary);border:1px solid rgba(0,0,0,0.04)}
  .list{max-height:380px;overflow:auto}
  .meta{font-size:12px;color:var(--muted)}
  @media(max-width:980px){.grid{grid-template-columns:1fr} .header-right{display:none}}
</style>
</head>
<body>
<div class="app">
  <header class="app-header">
    <div class="logo">
      <div class="mark">LV</div>
      <div>
        <h1>LiteVibe</h1>
        <div class="muted">ติดตามอารมณ์และพฤติกรรม — สำหรับมัธยมศึกษา</div>
      </div>
    </div>

    <div class="header-right" style="margin-left:auto;display:flex;gap:10px;align-items:center">
      <div id="currentUserBox" class="muted"></div>
      <button id="logoutBtn" class="btn-ghost" style="display:none">ออกจากระบบ</button>
    </div>
  </header>

  <!-- AUTH: Dropdown selection for names -->
  <div id="authCard" class="card">
    <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
      <div style="flex:1;min-width:220px">
        <label>เลือกบัญชี</label>
        <select id="userSelect"></select>
      </div>

      <div style="min-width:160px">
        <label>บทบาท (อัตโนมัติจากบัญชี)</label>
        <div id="selectedRole" class="muted">-</div>
      </div>

      <div style="display:flex;gap:8px;align-items:flex-end">
        <button id="loginBtn">เข้าสู่ระบบ</button>
        <button id="showCreate" class="btn-ghost">สร้างบัญชีใหม่</button>
      </div>
    </div>

    <div id="createRow" style="display:none;margin-top:12px">
      <div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap">
        <div style="flex:1;min-width:220px">
          <label>ชื่อผู้ใช้ (อังกฤษ/เลข)</label>
          <input id="newName" placeholder="เช่น kan, 630123" />
        </div>
        <div style="width:140px">
          <label>บทบาท</label>
          <select id="newRole"><option value="student">นักเรียน</option><option value="teacher">ครู</option></select>
        </div>
        <div style="display:flex;align-items:flex-end">
          <button id="createBtn" class="btn-ghost">สร้าง</button>
        </div>
      </div>
    </div>

    <div style="margin-top:10px" class="muted">เลือกบัญชีจากรายการตัวอย่างหรือสร้างบัญชีใหม่ ข้อมูลจะถูกเก็บในเครื่อง (localStorage)</div>
  </div>

  <div id="mainArea" style="display:none">
    <div class="grid">
      <div>
        <!-- STUDENT PANEL -->
        <div id="studentPanel" style="display:none">
          <div class="card">
            <div style="display:flex;align-items:center;gap:12px">
              <div><strong>บันทึกอารมณ์ประจำวัน</strong></div>
              <div class="muted">เลือกอิโมจิน่ารักแล้วเขียนบันทึกสั้น ๆ</div>
              <div class="right"><span class="badge">LiteVibe</span></div>
            </div>

            <div style="margin-top:12px">
              <label>อารมณ์วันนี้</label>
              <div id="moodButtons" class="emoji-row"></div>
            </div>

            <div style="margin-top:12px">
              <label>ข้อความสั้น ๆ / My diary</label>
              <textarea id="diaryText" rows="3" placeholder="เล่าเรื่องสั้น ๆ วันนี้เป็นอย่างไร..."></textarea>
            </div>

            <div style="display:flex;align-items:center;gap:10px;margin-top:10px">
              <button id="saveMoodBtn">บันทึกอารมณ์</button>
              <div class="muted">บันทึกล่าสุด: <span id="lastMoodText">-</span></div>
            </div>

            <div class="card" style="margin-top:12px">
              <div style="display:flex;align-items:center;gap:10px">
                <div><strong>สถิติอารมณ์</strong></div>
                <div class="muted">ดูภาพรวมสำหรับช่วงต่าง ๆ</div>
              </div>

              <div class="periods" style="margin-top:10px">
                <button class="periodBtn active" data-period="week">สัปดาห์</button>
                <button class="periodBtn" data-period="month">เดือน</button>
                <button class="periodBtn" data-period="semester">ภาคการศึกษา</button>
              </div>

              <div class="chart-wrap">
                <canvas id="moodPeriodChart" height="170"></canvas>
              </div>

              <div style="margin-top:10px" class="muted small">หมายเหตุ: กราฟจะอัปเดตทันทีเมื่อบันทึกอารมณ์</div>
            </div>

          </div>

          <!-- other student features (redeem / appt / quiz) remain accessible below -->
          <div class="card" style="margin-top:12px">
            <strong>ดาวเด็กดี</strong>
            <div style="display:flex;align-items:center;gap:12px;margin-top:8px">
              <div class="muted">ดาวของคุณ: <span class="badge">⭐ <span id="myStars">0</span></span></div>
              <button id="openRedeem" class="btn-ghost">แลกของรางวัล</button>
            </div>
            <div id="redeemPanel" style="display:none;margin-top:10px"></div>
          </div>

          <div class="card" style="margin-top:12px">
            <strong>นัดหมายปรึกษา</strong>
            <div style="margin-top:8px">
              <label>ครูที่ต้องการนัด</label>
              <input id="apptTeacher" placeholder="เช่น ajarn_nu" />
              <label style="margin-top:8px">ข้อความ</label>
              <input id="apptMsg" />
              <div style="margin-top:8px"><button id="requestAppt">ส่งคำขอ</button></div>
              <div id="apptHistory" class="list" style="margin-top:8px"></div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <strong>แบบทดสอบจิตวิทยาเบื้องต้น</strong>
            <div id="quizPanel" style="margin-top:8px">
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
              <div class="muted">จัดการนักเรียน ดูคำขอนัด และสถิติ</div>
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
            <label style="margin-top:8px">ข้อความ</label>
            <textarea id="reportText" rows="3"></textarea>
            <div style="margin-top:8px"><button id="sendReport">บันทึกและส่ง</button></div>
            <div id="reportsList" style="margin-top:8px" class="list"></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>สถิติภาพรวมอารมณ์ (นักเรียนทั้งหมด)</h4>
            <canvas id="moodChartAll" height="140"></canvas>
          </div>
        </div>

      </div>

      <div>
        <div class="card">
          <div style="display:flex;align-items:center;gap:12px">
            <div class="student-avatar" id="profileAvatar">LV</div>
            <div>
              <div id="profileName"><strong>-</strong></div>
              <div id="profileRole" class="meta">-</div>
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

    <footer style="margin-top:16px" class="muted">LiteVibe — Prototype (เก็บข้อมูลในเครื่อง)</footer>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
/* LiteVibe (updated)
 - Login: dropdown selection of existing sample users (plus create new)
 - Mood entries stored with ISO timestamp for accurate aggregation
 - Mood UI: colorful emoji buttons
 - After mood save: update period chart (week / month / semester)
 - Charts: stacked bar per period (moods per day/week/month) and overall teacher doughnut
*/

const STORAGE_KEY = 'litevibe_data_v2';
const emojiChoices = [
  {key:'very_happy', emoji:'😄', label:'มีความสุขมาก', color:'#FFD166'},
  {key:'happy', emoji:'🙂', label:'มีความสุข', color:'#7BE495'},
  {key:'neutral', emoji:'😐', label:'เฉย ๆ', color:'#A3A3FF'},
  {key:'sad', emoji:'😢', label:'เศร้า', color:'#90A7FF'},
  {key:'angry', emoji:'😠', label:'โกรธ', color:'#FF9AA2'},
  {key:'tired', emoji:'😴', label:'เหนื่อย', color:'#C6C6C6'}
];

let state = loadState();
let currentUser = null;
let periodChart = null;
let allChart = null;

/* ---------- storage and seed ---------- */
function defaultState(){ return { users: {}, activity: [] }; }

function seedSampleData(s){
  s.users['nam'] = { name:'nam', display:'น้ำ', role:'student', stars:8, moods:[
    {iso:'2025-11-25T08:10:00.000Z', time:'2025-11-25 08:10', key:'happy', emoji:'🙂', label:'มีความสุข', note:'ตื่นสบาย' }
  ], diaries:[{time:'2025-11-25 08:10',text:'วันนี้มีสอบวิทย์'}], appts:[], redeemHistory:[], quiz:[{time:'2025-11-24',score:5}], reports:[] };
  s.users['pong'] = { name:'pong', display:'ป้อง', role:'student', stars:12, moods:[
    {iso:'2025-11-29T07:50:00.000Z', time:'2025-11-29 07:50', key:'neutral', emoji:'😐', label:'เฉย ๆ', note:'เหนื่อยเล็กน้อย' }
  ], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  s.users['siwarat'] = { name:'siwarat', display:'ศิวรัตน์', role:'student', stars:3, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };

  s.users['ajarn_nu'] = { name:'ajarn_nu', display:'ครูหนู', role:'teacher', moods:[], diaries:[], inbox:[], reports:[] };
  s.users['ajarn_korn'] = { name:'ajarn_korn', display:'ครูกร', role:'teacher', moods:[], diaries:[], inbox:[], reports:[] };

  s.activity.unshift({txt:'LiteVibe: สร้างตัวอย่างนักเรียนและครูสำหรับทดลอง', time:new Date().toLocaleString()});
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
    if(!parsed.users || Object.keys(parsed.users).length===0){ seedSampleData(parsed); localStorage.setItem(STORAGE_KEY, JSON.stringify(parsed)); }
    return parsed;
  }catch(e){ const s = defaultState(); seedSampleData(s); return s; }
}
function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

/* ---------- utilities ---------- */
function generateId(){ return 'id_' + Math.random().toString(36).slice(2,9); }
function logActivity(txt){ const time = new Date().toLocaleString(); state.activity.unshift({txt,time}); saveState(); renderActivity(); }

/* ---------- auth UI ---------- */
const userSelect = document.getElementById('userSelect');
const selectedRole = document.getElementById('selectedRole');
function populateUserSelect(){
  userSelect.innerHTML = '';
  const users = Object.values(state.users);
  users.forEach(u=>{
    const opt = document.createElement('option');
    opt.value = u.name;
    opt.textContent = `${u.display || u.name} — ${u.role}`;
    userSelect.appendChild(opt);
  });
  // add create option at end
  const optNew = document.createElement('option');
  optNew.value = '__create__';
  optNew.textContent = '>> สร้างบัญชีใหม่ <<';
  userSelect.appendChild(optNew);
  updateSelectedRole();
}
userSelect.addEventListener('change', updateSelectedRole);
function updateSelectedRole(){
  const v = userSelect.value;
  if(v && v!=='__create__' && state.users[v]) selectedRole.innerText = state.users[v].role;
  else selectedRole.innerText = '-';
}

/* create account UI */
document.getElementById('showCreate').addEventListener('click', ()=> {
  document.getElementById('createRow').style.display = document.getElementById('createRow').style.display==='none' ? 'block' : 'none';
});
document.getElementById('createBtn').addEventListener('click', ()=>{
  const name = document.getElementById('newName').value.trim();
  const role = document.getElementById('newRole').value;
  if(!name) return alert('กรุณากรอกชื่อผู้ใช้');
  if(state.users[name]) return alert('ชื่อผู้ใช้นี้มีอยู่แล้ว');
  const u = { name, display:name, role, stars:0, moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  if(role==='teacher') u.inbox = [];
  state.users[name] = u;
  saveState();
  populateUserSelect();
  alert('สร้างบัญชีเรียบร้อยแล้ว: ' + name);
  document.getElementById('newName').value='';
  document.getElementById('createRow').style.display='none';
});

/* login */
document.getElementById('loginBtn').addEventListener('click', ()=>{
  const v = userSelect.value;
  if(!v) return alert('กรุณาเลือกบัญชี');
  if(v==='__create__') { document.getElementById('createRow').style.display='block'; return; }
  loginAs(v);
});
function loginAs(name){
  currentUser = name;
  const u = state.users[name];
  document.getElementById('authCard').style.display='none';
  document.getElementById('mainArea').style.display='block';
  document.getElementById('logoutBtn').style.display='inline-block';
  document.getElementById('currentUserBox').innerText = `${u.display || u.name} (${u.role})`;
  renderAll();
}
document.getElementById('logoutBtn').addEventListener('click', ()=> {
  currentUser = null;
  document.getElementById('authCard').style.display='';
  document.getElementById('mainArea').style.display='none';
  document.getElementById('logoutBtn').style.display='none';
  document.getElementById('currentUserBox').innerText = '';
});

/* ---------- initial population ---------- */
populateUserSelect();

/* ---------- render helpers ---------- */
function renderAll(){
  renderProfile();
  renderPanels();
  renderActivity();
  renderTeacherChart();
  // update period chart
  renderPeriodChart(currentPeriod);
}

/* Profile */
function renderProfile(){
  const avatar = document.getElementById('profileAvatar');
  const nameEl = document.getElementById('profileName');
  const roleEl = document.getElementById('profileRole');
  const starsEl = document.getElementById('profileStars');
  const box = document.getElementById('profileBox');
  if(!currentUser){ avatar.innerText='LV'; nameEl.innerHTML='<strong>-</strong>'; roleEl.innerText='-'; starsEl.innerText='0'; box.innerHTML='เข้าสู่ระบบเพื่อดูโปรไฟล์'; return; }
  const u = state.users[currentUser];
  avatar.innerText = (u.display||u.name).slice(0,2).toUpperCase();
  nameEl.innerHTML = `<strong>${u.display || u.name}</strong>`;
  roleEl.innerText = u.role;
  starsEl.innerText = u.stars || 0;
  let html = `<div class="meta">บันทึกล่าสุด:</div>`;
  if(u.moods && u.moods.length){
    const last = u.moods[u.moods.length-1];
    html += `<div style="margin-top:6px">${last.time} — ${last.emoji} ${last.label}</div><div class="muted" style="margin-top:6px">${last.note || '-'}</div>`;
  } else html += `<div class="muted" style="margin-top:6px">ยังไม่มีบันทึก</div>`;
  box.innerHTML = html;
}

/* ---------- Mood UI and save ---------- */
const moodButtonsContainer = document.getElementById('moodButtons');
function renderMoodButtons(){
  moodButtonsContainer.innerHTML = '';
  emojiChoices.forEach(e=>{
    const btn = document.createElement('button');
    btn.className = 'emoji-btn';
    btn.dataset.key = e.key;
    btn.innerHTML = `<div style="font-size:32px">${e.emoji}</div><div class="label">${e.label}</div>`;
    // add small color accent background
    btn.style.background = `linear-gradient(180deg, rgba(255,255,255,1), ${hexToRgba(e.color,0.06)})`;
    btn.addEventListener('click', ()=>{
      document.querySelectorAll('.emoji-btn').forEach(b=>b.classList.remove('selected'));
      btn.classList.add('selected');
    });
    moodButtonsContainer.appendChild(btn);
  });
}
renderMoodButtons();

document.getElementById('saveMoodBtn').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const sel = document.querySelector('.emoji-btn.selected');
  if(!sel) return alert('กรุณาเลือกรูปอารมณ์');
  const key = sel.dataset.key;
  const meta = emojiChoices.find(x=>x.key===key);
  const note = document.getElementById('diaryText').value.trim();
  const now = new Date();
  const entry = { iso: now.toISOString(), time: now.toLocaleString(), key, emoji: meta.emoji, label: meta.label, note };
  const u = state.users[currentUser];
  u.moods = u.moods || [];
  u.moods.push(entry);
  if(note) u.diaries = u.diaries || [], u.diaries.push({time:entry.time, text:note});
  saveState();
  logActivity(`${currentUser} บันทึกอารมณ์: ${meta.emoji} ${meta.label}`);
  document.getElementById('diaryText').value = '';
  document.querySelectorAll('.emoji-btn').forEach(b=>b.classList.remove('selected'));
  renderAll();
});

/* ---------- Redeem / Appointments / Quiz / Teacher functions (kept from prior) ---------- */
document.getElementById('openRedeem').addEventListener('click', ()=>{
  const p = document.getElementById('redeemPanel');
  if(p.style.display==='block'){ p.style.display='none'; return; }
  // build redeem items
  p.style.display='block';
  p.innerHTML = `
    <div style="display:flex;gap:10px;flex-wrap:wrap">
      <div style="padding:10px;border-radius:10px;background:#fbfbff"><div><strong>พัก 5 นาที</strong></div><div class="meta">10 ⭐</div><div style="margin-top:6px"><button class="redeemBtn" data-name="พัก 5 นาที" data-cost="10">แลก</button></div></div>
      <div style="padding:10px;border-radius:10px;background:#fbfbff"><div><strong>คูปองเครื่องเขียน</strong></div><div class="meta">12 ⭐</div><div style="margin-top:6px"><button class="redeemBtn" data-name="คูปองเครื่องเขียน" data-cost="12">แลก</button></div></div>
      <div style="padding:10px;border-radius:10px;background:#fbfbff"><div><strong>คูปองอาหาร/เครื่องดื่ม</strong></div><div class="meta">15 ⭐</div><div style="margin-top:6px"><button class="redeemBtn" data-name="คูปองอาหาร" data-cost="15">แลก</button></div></div>
    </div>
    <h4 style="margin-top:10px">ประวัติการแลก</h4>
    <div id="redeemHistory" class="list"></div>
  `;
  renderRedeemHistory(state.users[currentUser]);
});
document.addEventListener('click', (e)=> {
  if(e.target && e.target.matches('.redeemBtn')){
    const name = e.target.dataset.name; const cost = parseInt(e.target.dataset.cost);
    if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
    const u = state.users[currentUser];
    if((u.stars||0) < cost) return alert('ดาวไม่พอ');
    if(!confirm(`ต้องการแลก ${name} ใช้ ${cost} ⭐?`)) return;
    u.stars -= cost;
    u.redeemHistory = u.redeemHistory || [];
    u.redeemHistory.push({item:name,cost,time:new Date().toLocaleString()});
    saveState();
    logActivity(`${currentUser} แลก ${name} (-${cost} ⭐)`);
    renderAll();
  }
});

/* appointments */
document.getElementById('requestAppt').addEventListener('click', ()=> {
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const t = document.getElementById('apptTeacher').value.trim();
  const msg = document.getElementById('apptMsg').value.trim();
  if(!t || !msg) return alert('กรุณากรอกชื่อครูและข้อความ');
  const appt = { id: generateId(), teacher: t, student: currentUser, msg, status:'pending', time: new Date().toLocaleString(), teacherNote:'', iso:new Date().toISOString() };
  const u = state.users[currentUser];
  u.appts = u.appts || []; u.appts.push(appt);
  if(!state.users[t]) state.users[t] = { name:t, display:t, role:'teacher', inbox:[], moods:[], diaries:[], reports:[], stars:0 };
  state.users[t].inbox = state.users[t].inbox || []; state.users[t].inbox.push(appt);
  saveState();
  logActivity(`${currentUser} ขอเข้าปรึกษากับ ${t}`);
  renderAll();
});

/* Quiz (kept simple) */
const sampleQuiz = [
  {q:'ในสัปดาห์ที่ผ่านมาคุณรู้สึกมีความสุขบ่อยแค่ไหน?', options:['ไม่เลย','บางครั้ง','บ่อย','ตลอดเวลา'], scores:[0,1,2,3]},
  {q:'คุณนอนหลับเพียงพอหรือไม่?', options:['ไม่เลย','บางครั้ง','พอประมาณ','เพียงพอ'], scores:[0,1,2,3]},
  {q:'คุณรู้สึกว่ามีคนคอยรับฟังเมื่อคุณต้องการหรือไม่?', options:['ไม่เลย','บางครั้ง','บ่อย','ตลอดเวลา'], scores:[0,1,2,3]}
];
function renderQuizUI(){
  const qEl = document.getElementById('quizQuestions');
  qEl.innerHTML = '';
  sampleQuiz.forEach((qq,i)=>{
    const div = document.createElement('div'); div.style.padding='8px';
    div.innerHTML = `<div><strong>Q${i+1}.</strong> ${qq.q}</div>`;
    qq.options.forEach((opt,j)=>{
      const id = `q${i}_o${j}`;
      div.innerHTML += `<div style="margin-left:8px"><input type="radio" name="q${i}" id="${id}" value="${j}"> <label for="${id}">${opt}</label></div>`;
    });
    qEl.appendChild(div);
  });
  document.getElementById('startQuiz').style.display='inline-block';
  document.getElementById('submitQuiz').style.display='none';
}
document.getElementById('startQuiz').addEventListener('click', ()=> { document.getElementById('startQuiz').style.display='none'; document.getElementById('submitQuiz').style.display='inline-block'; });
document.getElementById('submitQuiz').addEventListener('click', ()=> {
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  let total=0, answered=true;
  sampleQuiz.forEach((qq,i)=>{ const v = document.querySelector(`input[name="q${i}"]:checked`); if(!v) answered=false; else total += qq.scores[parseInt(v.value)]; });
  if(!answered) return alert('กรุณาตอบทุกข้อ');
  const u = state.users[currentUser]; u.quiz = u.quiz || []; u.quiz.push({time:new Date().toLocaleString(), score:total});
  saveState();
  logActivity(`${currentUser} ทำแบบทดสอบ คะแนน ${total}`);
  document.getElementById('quizResult').innerHTML = `<div class="meta">ผลคะแนน: <strong>${total}</strong></div>`;
  document.getElementById('submitQuiz').style.display='none'; document.getElementById('startQuiz').style.display='inline-block';
});
renderQuizUI();

/* ---------- Teacher area: students list, inbox, reports ---------- */
function renderPanels(){
  if(!currentUser) return;
  const u = state.users[currentUser];
  document.getElementById('studentPanel').style.display = u.role === 'student' ? 'block' : 'none';
  document.getElementById('teacherPanel').style.display = u.role === 'teacher' ? 'block' : 'none';
  if(u.role === 'student'){
    document.getElementById('myStars').innerText = u.stars || 0;
    renderApptHistory(u);
    renderRedeemHistory(u);
  } else {
    renderStudentsList();
    renderApptRequests();
    renderReportsList();
    buildReportStudentSelect();
  }
  renderQuickPanel();
}

/* students list */
function renderStudentsList(){
  const q = document.getElementById('searchStudent').value.trim().toLowerCase();
  const container = document.getElementById('studentsList');
  const students = Object.values(state.users).filter(x=>x.role==='student' && (!q || (x.display||x.name).toLowerCase().includes(q)));
  if(!students.length){ container.innerHTML = '<div class="meta">ไม่มีนักเรียน</div>'; return; }
  container.innerHTML = students.map(s=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb;display:flex;align-items:center;gap:10px"><div class="student-avatar">${(s.display||s.name).slice(0,2).toUpperCase()}</div><div><strong>${s.display||s.name}</strong><div class="meta">ดาว: ${s.stars||0}</div></div><div style="margin-left:auto"><button class="addStar" data-name="${s.name}">ให้ +1</button><button class="removeStar" data-name="${s.name}">-1</button></div></div>`).join('');
  document.querySelectorAll('.addStar').forEach(b=>b.addEventListener('click', e=>modifyStars(e.target.dataset.name,1)));
  document.querySelectorAll('.removeStar').forEach(b=>b.addEventListener('click', e=>modifyStars(e.target.dataset.name,-1)));
}
document.getElementById('searchStudent').addEventListener('input', renderStudentsList);

function modifyStars(name,delta){
  const s = state.users[name];
  if(!s) return alert('ไม่พบ');
  s.stars = Math.max(0,(s.stars||0)+delta);
  saveState();
  logActivity(`${currentUser} ปรับดาวให้ ${name}: ${delta>0?'+':''}${delta}`);
  renderAll();
}

/* appt */
function renderApptHistory(u){
  const el = document.getElementById('apptHistory');
  if(!u.appts || !u.appts.length){ el.innerHTML='<div class="meta">ยังไม่มีการขอนัด</div>'; return; }
  el.innerHTML = u.appts.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time} → ถึง: ${a.teacher} [${a.status}]</div><div>${a.msg}</div><div class="meta">หมายเหตุครู: ${a.teacherNote || '-'}</div></div>`).join('');
}

/* teacher inbox */
function renderApptRequests(){
  const el = document.getElementById('apptRequests');
  if(!currentUser) return;
  const inbox = (state.users[currentUser].inbox || []);
  if(!inbox.length){ el.innerHTML = '<div class="meta">ยังไม่มีคำขอนัด</div>'; return; }
  el.innerHTML = inbox.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time} — จาก: <strong>${a.student}</strong></div><div style="margin-top:6px">${a.msg}</div><div style="margin-top:8px">${a.status==='approved'?'<span class="badge">อนุมัติ</span>':`<button class="approveBtn" data-id="${a.id}">อนุมัติ</button><button class="rejectBtn" data-id="${a.id}">ปฏิเสธ</button>`} <button class="noteBtn" data-id="${a.id}">หมายเหตุ</button></div></div>`).join('');
  document.querySelectorAll('.approveBtn').forEach(b=>b.addEventListener('click', e=>handleApptAction(e.target.dataset.id,'approved')));
  document.querySelectorAll('.rejectBtn').forEach(b=>b.addEventListener('click', e=>handleApptAction(e.target.dataset.id,'rejected')));
  document.querySelectorAll('.noteBtn').forEach(b=>b.addEventListener('click', e=>{
    const id = e.target.dataset.id;
    const note = prompt('หมายเหตุสำหรับการนัด:');
    if(note !== null) handleApptNote(id,note);
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
  saveState(); logActivity(`${currentUser} ${status==='approved'?'อนุมัติ':'ปฏิเสธ'} นัดจาก ${item.student}`); renderAll();
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
  saveState(); logActivity(`${currentUser} บันทึกหมายเหตุนัด ${item.student}`); renderAll();
}

/* reports */
document.getElementById('sendReport').addEventListener('click', ()=>{
  const student = document.getElementById('reportStudent').value;
  const text = document.getElementById('reportText').value.trim();
  if(!student || !text) return alert('เลือกนักเรียนและกรอกข้อความ');
  const r = { id: generateId(), teacher: currentUser, student, text, time: new Date().toLocaleString(), iso: new Date().toISOString() };
  state.users[student].reports = state.users[student].reports || []; state.users[student].reports.push(r);
  saveState(); logActivity(`${currentUser} ส่งรายงาน: ${student}`); renderAll(); document.getElementById('reportText').value='';
});
function renderReportsList(){
  const all = [];
  Object.values(state.users).forEach(u=>{ if(u.reports) u.reports.forEach(r=>{ if(r.teacher === currentUser) all.push(r); })});
  const el = document.getElementById('reportsList');
  if(!all.length){ el.innerHTML = '<div class="meta">ยังไม่มีรายงาน</div>'; return; }
  el.innerHTML = all.map(r=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${r.time} → ${r.student}</div><div>${r.text}</div></div>`).join('');
}
function buildReportStudentSelect(){
  const sel = document.getElementById('reportStudent'); sel.innerHTML = '<option value="">-- เลือกนักเรียน --</option>';
  Object.values(state.users).filter(u=>u.role==='student').forEach(s=>{ const opt = document.createElement('option'); opt.value = s.name; opt.innerText = s.display || s.name; sel.appendChild(opt); });
}

/* quick panel */
function renderQuickPanel(){
  const el = document.getElementById('quickPanel');
  if(!currentUser){ el.innerHTML=''; return; }
  const u = state.users[currentUser];
  let html = `<div class="meta">บทบาท: ${u.role}</div>`;
  if(u.role==='student'){
    html += `<div style="margin-top:6px"><strong>ดาว: ${u.stars || 0}</strong></div>`;
    html += `<div class="meta" style="margin-top:6px">บันทึก: ${(u.diaries||[]).length} ครั้ง</div>`;
  } else {
    const pending = (u.inbox||[]).filter(i=>i.status==='pending').length;
    html += `<div style="margin-top:6px"><strong>คำขอนัดรออนุมัติ: ${pending}</strong></div>`;
    const reports = Object.values(state.users).reduce((acc,usr)=> acc + ((usr.reports||[]).filter(r=>r.teacher===currentUser).length),0);
    html += `<div class="meta" style="margin-top:6px">รายงานที่ส่ง: ${reports}</div>`;
  }
  el.innerHTML = html;
}

/* activity */
function renderActivity(){ const el = document.getElementById('activityLog'); el.innerHTML = state.activity.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time}</div><div>${a.txt}</div></div>`).join(''); }

/* ---------- CHART: Period aggregation (week / month / semester) ---------- */
const ctxPeriod = document.getElementById('moodPeriodChart').getContext('2d');
let currentPeriod = 'week';
document.querySelectorAll('.periodBtn').forEach(b=>{
  b.addEventListener('click', ()=>{
    document.querySelectorAll('.periodBtn').forEach(x=>x.classList.remove('active'));
    b.classList.add('active');
    currentPeriod = b.dataset.period;
    renderPeriodChart(currentPeriod);
  });
});

function renderPeriodChart(period){
  if(!currentUser) return;
  const u = state.users[currentUser];
  const moods = emojiChoices.map(m=>m.label);
  const colorMap = {}; emojiChoices.forEach(e=>colorMap[e.label]=e.color);

  const aggregated = aggregateByPeriod(u.moods || [], period);
  const labels = aggregated.labels;
  const datasets = emojiChoices.map((e, idx)=>{
    return {
      label: e.label,
      data: aggregated.data.map(d=>d[e.label]||0),
      backgroundColor: hexToRgba(e.color, 0.92),
      stack: 'stack1'
    };
  });

  if(periodChart) periodChart.destroy();
  periodChart = new Chart(ctxPeriod, {
    type: 'bar',
    data: { labels, datasets },
    options: {
      responsive: true,
      plugins: { legend:{position:'bottom'} },
      scales: {
        x: { stacked: true },
        y: { stacked: true, beginAtZero:true, ticks:{precision:0} }
      }
    }
  });
}

/* aggregate moods into bins depending on period:
   - week: last 7 days -> labels: day names (e.g., 25 Nov)
   - month: last 30 days grouped by week -> labels: 'สัปดาห์ 1',..'สัปดาห์ 4'
   - semester: detect current semester (Jan-Jun / Jul-Dec), labels per month in semester so far
*/
function aggregateByPeriod(moods, period){
  const now = new Date();
  if(period === 'week'){
    const days = [];
    for(let i=6;i>=0;i--){ const d = new Date(); d.setDate(now.getDate()-i); days.push(dateKey(d)); }
    // prepare counts per day
    const data = days.map(_=> ({}));
    moods.forEach(entry=>{
      if(!entry.iso) return;
      const d = new Date(entry.iso);
      const key = dateKey(d);
      const idx = days.indexOf(key);
      if(idx >= 0){
        data[idx][entry.label] = (data[idx][entry.label]||0) + 1;
      }
    });
    return { labels: days.map(d=>formatDayLabel(d)), data };
  } else if(period === 'month'){
    // group last 30 days into 4 weeks: week 1 (days -30 to -23), week2 (-22 to -15) etc
    const weeks = [];
    const weekRanges = [];
    for(let w=0; w<4; w++){
      const start = new Date(); start.setDate(now.getDate() - 30 + w*7);
      const end = new Date(); end.setDate(start.getDate() + 6);
      weekRanges.push({start:start, end:end});
      weeks.push(`สัปดาห์ ${w+1}`);
    }
    const data = weeks.map(_=> ({}));
    moods.forEach(entry=>{
      if(!entry.iso) return;
      const d = new Date(entry.iso);
      for(let i=0;i<weekRanges.length;i++){
        if(d >= stripTime(weekRanges[i].start) && d <= endOfDay(weekRanges[i].end)){
          data[i][entry.label] = (data[i][entry.label]||0)+1; break;
        }
      }
    });
    return { labels: weeks, data };
  } else { // semester
    const sem = getCurrentSemester(now);
    // generate month labels within semester up to current month
    const months = [];
    const data = [];
    let m = new Date(sem.start);
    while(m <= now){
      const key = `${m.getFullYear()}-${(m.getMonth()+1).toString().padStart(2,'0')}`;
      months.push(formatMonthLabel(m));
      data.push({});
      m.setMonth(m.getMonth()+1);
    }
    moods.forEach(entry=>{
      if(!entry.iso) return;
      const d = new Date(entry.iso);
      if(d >= sem.start && d <= now){
        // find month index
        const idx = (d.getFullYear()*12 + d.getMonth()) - (sem.start.getFullYear()*12 + sem.start.getMonth());
        if(idx >=0 && idx < data.length){
          data[idx][entry.label] = (data[idx][entry.label]||0)+1;
        }
      }
    });
    return { labels: months, data };
  }
}

/* ---------- teacher overall chart (doughnut) ---------- */
function renderTeacherChart(){
  const ctx = document.getElementById('moodChartAll');
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
  if(allChart) allChart.destroy();
  allChart = new Chart(ctx, {
    type: 'doughnut',
    data: { labels, datasets:[{ data, backgroundColor: emojiChoices.map(e=>e.color) }] },
    options: { responsive:true, plugins:{legend:{position:'bottom'}} }
  });
}

/* ---------- helper date functions ---------- */
function dateKey(d){ const dt = new Date(d.getFullYear(), d.getMonth(), d.getDate()); return `${dt.getDate().toString().padStart(2,'0')} ${dt.toLocaleString('th-TH',{month:'short'})}`; }
function formatDayLabel(label){ return label; }
function stripTime(d){ return new Date(d.getFullYear(), d.getMonth(), d.getDate()); }
function endOfDay(d){ return new Date(d.getFullYear(), d.getMonth(), d.getDate(),23,59,59,999); }
function formatMonthLabel(d){ return d.toLocaleString('th-TH',{month:'short', year:'numeric'}); }
function getCurrentSemester(now){
  // semester 1: Jan-Jun, semester 2: Jul-Dec
  const y = now.getFullYear();
  if(now.getMonth() <= 5){ // Jan-Jun
    return { start: new Date(y,0,1), end: new Date(y,5,30) };
  } else {
    return { start: new Date(y,6,1), end: new Date(y,11,31) };
  }
}
function hexToRgba(hex, a){
  if(hex.startsWith('#')) hex = hex.slice(1);
  const bigint = parseInt(hex,16);
  const r = (bigint >> 16) & 255;
  const g = (bigint >> 8) & 255;
  const b = bigint & 255;
  return `rgba(${r},${g},${b},${a})`;
}

/* ---------- Redeem history render ---------- */
function renderRedeemHistory(u){
  const el = document.getElementById('redeemHistory');
  if(!el) return;
  if(!u.redeemHistory || !u.redeemHistory.length){ el.innerHTML = '<div class="meta">ยังไม่มีการแลก</div>'; return; }
  el.innerHTML = u.redeemHistory.map(r=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div><strong>${r.item}</strong> <div class="meta">(${r.cost} ⭐)</div></div><div class="meta">${r.time}</div></div>`).join('');
}

/* ---------- activity and initial render ---------- */
renderActivity();
renderAll();

/* keep teacher chart updated occasionally */
setInterval(()=>{ renderTeacherChart(); },5000);

/* ---------- simple helpers to keep UI consistent ---------- */
function renderApptRequests(){ /* simply reuse earlier function to avoid duplication */ 
  if(!currentUser) return;
  document.getElementById('apptRequests').innerHTML = (state.users[currentUser].inbox || []).length ? '' : '<div class="meta">ยังไม่มีคำขอนัด</div>';
  // attach handlers via renderPanels call
}

/* Render students list initially for teacher input */
renderStudentsList();

</script>
</body>
</html>
