<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>LiteVibe — ระบบติดตามอารมณ์และพฤติกรรม (Teacher Minimal)</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg-start:#faf5ff; --bg-end:#fff0f6; --card:#fff;
    --primary:#7c3aed; --accent:#fb7185; --muted:#6b7280;
    --danger:#ef4444; --success:#16a34a; --info:#06b6d4;
  }
  *{box-sizing:border-box}
  body{font-family:"Kanit",sans-serif;background:linear-gradient(135deg,var(--bg-start) 0%, rgba(255,255,255,0.6) 40%, var(--bg-end) 100%);margin:0;color:#071026}
  .app{max-width:1100px;margin:22px auto;padding:18px}
  header{display:flex;align-items:center;gap:12px;margin-bottom:18px}
  .mark{width:54px;height:54px;border-radius:12px;background:linear-gradient(135deg,var(--primary),var(--accent));display:flex;align-items:center;justify-content:center;color:#fff;font-weight:700}
  h1{font-size:18px;margin:0;color:var(--primary)}
  .muted{color:var(--muted);font-size:13px}
  .card{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 8px 22px rgba(12,10,20,0.04);margin-bottom:14px}
  .grid{display:grid;grid-template-columns:1fr 360px;gap:14px}
  label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
  input[type=text],select,textarea{width:100%;padding:10px;border-radius:10px;border:1px solid rgba(124,58,237,0.08);background:#fff;font-size:14px}
  button{background:var(--primary);color:#fff;border:0;padding:8px 12px;border-radius:10px;cursor:pointer;font-weight:700}
  .btn-ghost{background:transparent;border:1px solid rgba(124,58,237,0.12);color:var(--primary);padding:8px 10px;border-radius:10px}
  .emoji-row{display:flex;gap:10px;flex-wrap:wrap}
  .emoji-btn{width:70px;height:70px;border-radius:12px;border:0;background:#fff;display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:28px;cursor:pointer;transition:all .14s;box-shadow:0 8px 20px rgba(16,24,45,0.04)}
  .emoji-btn .label{font-size:12px;margin-top:6px;color:var(--muted)}
  .emoji-btn:hover{transform:translateY(-6px)}
  .emoji-btn.selected{outline:4px solid rgba(124,58,237,0.12)}
  .list{max-height:360px;overflow:auto}
  .student-avatar{width:42px;height:42px;border-radius:8px;background:linear-gradient(135deg,#fff,#faf5ff);display:inline-flex;align-items:center;justify-content:center;font-weight:700;color:var(--primary);border:1px solid rgba(0,0,0,0.04);overflow:hidden}
  .diary-item{padding:8px;border-bottom:1px solid #f3f6fb;display:flex;gap:10px;align-items:flex-start}
  .diary-emoji{font-size:26px;min-width:36px;text-align:center}
  .meta{font-size:12px;color:var(--muted)}
  .danger-badge{background:var(--danger);color:#fff;padding:6px 8px;border-radius:8px;font-weight:700}
  .ok-badge{background:var(--success);color:#fff;padding:6px 8px;border-radius:8px;font-weight:700}
  .controls{display:flex;gap:8px;flex-wrap:wrap}
  .small{font-size:13px;color:var(--muted)}
  @media(max-width:980px){.grid{grid-template-columns:1fr} }
  .pastel-legend{display:flex;flex-direction:column;gap:6px;margin-top:8px}
  .legend-row{display:flex;gap:8px;align-items:center}
  .swatch{width:12px;height:12px;border-radius:3px}
  .muted-note{font-size:12px;color:var(--muted);margin-top:8px}
</style>
</head>
<body>
<div class="app">
  <header>
    <div class="mark">LV</div>
    <div>
      <h1>LiteVibe — Teacher</h1>
      <div class="muted">หน้าใช้งานครู (เวอร์ชันย่อ)</div>
    </div>

    <div style="margin-left:auto;display:flex;align-items:center;gap:10px">
      <div id="currentUserBox" class="muted"></div>
      <button id="logoutBtn" class="btn-ghost" style="display:none">ออกจากระบบ</button>
    </div>
  </header>

  <!-- simple auth / select -->
  <div class="card" id="authCard">
    <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
      <div style="flex:1;min-width:200px">
        <label>เลือกบัญชี</label>
        <select id="userSelect"></select>
      </div>
      <div style="min-width:120px">
        <label>บทบาท</label>
        <div id="selectedRole" class="muted">-</div>
      </div>
      <div style="display:flex;gap:8px;align-items:flex-end">
        <button id="loginBtn">เข้าสู่ระบบ</button>
        <button id="showCreate" class="btn-ghost">สร้างบัญชี</button>
      </div>
    </div>

    <div id="createRow" style="display:none;margin-top:12px">
      <div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap">
        <div style="flex:1;min-width:180px">
          <label>ชื่อผู้ใช้</label>
          <input id="newName" placeholder="เช่น ajarn_somchai" />
        </div>
        <div style="width:140px">
          <label>บทบาท</label>
          <select id="newRole"><option value="teacher">ครู</option><option value="student">นักเรียน</option></select>
        </div>
        <div>
          <button id="createBtn" class="btn-ghost">สร้าง</button>
        </div>
      </div>
    </div>

    <div class="muted-note">เลือกบัญชีครูที่จำลองไว้เพื่อทดสอบฟังก์ชัน</div>
  </div>

  <div id="mainArea" style="display:none">
    <div class="grid">
      <div>
        <!-- TEACHER PANEL (minimal per request) -->
        <div id="teacherPanel" class="card" style="display:none; padding:16px 16px 6px 16px">

          <!-- 1) daily mood + diary -->
          <div style="margin-bottom:12px">
            <div style="display:flex;align-items:center;justify-content:space-between">
              <div><strong>บันทึกอารมณ์ประจำวัน (ครู)</strong><div class="muted small">เลือกอิโมจิและบันทึกข้อความสั้น ๆ</div></div>
              <div id="teacherStarsWrap" class="muted small"></div>
            </div>
            <div style="margin-top:10px" id="teacherMoodArea">
              <label>อารมณ์วันนี้</label>
              <div id="teacherMoodButtons" class="emoji-row"></div>

              <label style="margin-top:8px">บันทึกสั้น ๆ</label>
              <textarea id="teacherDiaryText" rows="3" placeholder="บันทึกสำหรับวันนี้..."></textarea>

              <div style="display:flex;gap:10px;margin-top:8px">
                <button id="saveTeacherMoodBtn">บันทึกอารมณ์</button>
                <div class="muted small" id="teacherLastMood">บันทึกล่าสุด: -</div>
              </div>
            </div>
          </div>

          <!-- 2) teacher daily pie -->
          <div style="margin-bottom:12px;">
            <strong>สถิติอารมณ์วันนี้ — ครู (ของฉัน)</strong>
            <div style="display:flex;gap:12px;align-items:flex-start;margin-top:10px">
              <div style="width:220px;height:220px"><canvas id="teacherSelfMoodPie"></canvas></div>
              <div style="flex:1">
                <div id="teacherSelfLegend" class="pastel-legend"></div>
                <div class="muted-note">กราฟแสดงสัดส่วนอารมณ์ที่บันทึกในวันนี้</div>
              </div>
            </div>
          </div>

          <!-- 3) students at-risk list -->
          <div style="margin-bottom:12px;">
            <strong>รายการนักเรียนที่มีความเสี่ยง (อารมณ์/พฤติกรรม)</strong>
            <div id="teacherRiskList" class="list" style="margin-top:8px"></div>
          </div>

          <!-- 4) simulated behavior reports for advisor -->
          <div style="margin-bottom:12px;">
            <strong>รายงานพฤติกรรม (กล่องที่ปรึกษา)</strong>
            <div id="advisorInbox" class="list" style="margin-top:8px"></div>
          </div>

          <!-- 5) student list with add/remove stars -->
          <div style="margin-bottom:12px;">
            <strong>จัดการดาวนักเรียน (เพิ่ม/ลด เด็กดี)</strong>
            <div style="margin-top:8px">
              <input id="searchStudent" placeholder="ค้นหาชื่อนักเรียน" style="padding:8px;border-radius:8px;border:1px solid rgba(124,58,237,0.06);width:100%" />
            </div>
            <div id="studentsList" class="list" style="margin-top:8px"></div>
          </div>

          <!-- 6) appointment requests from students (approve) -->
          <div style="margin-bottom:12px;">
            <strong>คำขอนัดจากนักเรียน</strong>
            <div id="apptRequests" class="list" style="margin-top:8px"></div>
          </div>

          <!-- 7) redeem (request approve) -->
          <div style="margin-bottom:12px;">
            <strong>คำขอแลกดาวจากนักเรียน</strong>
            <div id="teacherRedeemRequests" class="list" style="margin-top:8px"></div>
          </div>

        </div>
      </div>

      <div>
        <!-- Right column: profile / quick / log -->
        <div class="card">
          <div style="display:flex;align-items:center;gap:12px">
            <div class="student-avatar" id="profileAvatar">T</div>
            <div>
              <div id="profileName"><strong>-</strong></div>
              <div id="profileRole" class="meta">-</div>
              <div id="profileClass" class="meta" style="margin-top:6px"></div>
            </div>
          </div>

          <div style="margin-top:12px" id="profileBox"></div>
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
/* Teacher-minimal page (v7 -> teacher-only features)
   - storage: localStorage 'litevibe_teacher_min_v1'
   - seed with some teachers/students
*/

/* ---------- config ---------- */
const STORAGE_KEY = 'litevibe_teacher_min_v1';
const emojiChoices = [
  {key:'very_happy', emoji:'😄', label:'มีความสุขมาก', color:'#FFD7A6'},
  {key:'happy', emoji:'🙂', label:'มีความสุข', color:'#B9F6CA'},
  {key:'neutral', emoji:'😐', label:'เฉย ๆ', color:'#C9C8FF'},
  {key:'sad', emoji:'😢', label:'เศร้า', color:'#AED6FF'},
  {key:'angry', emoji:'😠', label:'โกรธ', color:'#FFB3C7'},
  {key:'tired', emoji:'😴', label:'เหนื่อย', color:'#E8E8E8'}
];

let state = loadState();
let currentUser = null;
let teacherSelfPie = null;
let studentDailyPie = null;

/* ---------- storage & seed ---------- */
function defaultState(){ return { users: {}, activity: [], redeemRequests: [] }; }

function seedSampleData(s){
  // students
  s.users['nam'] = { name:'nam', display:'น้ำ', role:'student', classId:'M1A', grade:'ม.1', stars:8, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], reports:[] };
  s.users['pong'] = { name:'pong', display:'ป้อง', role:'student', classId:'M2B', grade:'ม.2', stars:3, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], reports:[] };
  s.users['siwarat'] = { name:'siwarat', display:'ศิวรัตน์', role:'student', classId:'M1A', grade:'ม.1', stars:5, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], reports:[] };

  // teachers
  s.users['ajarn_nu'] = { name:'ajarn_nu', display:'ครูหนู', role:'teacher', avatar:'', moods:[], diaries:[], inbox:[], inboxReports:[], reports:[] };
  s.users['ajarn_korn'] = { name:'ajarn_korn', display:'ครูกร', role:'teacher', avatar:'', moods:[], diaries:[], inbox:[], inboxReports:[], reports:[] };

  // sample: a report already in an advisor's inbox (simulated)
  const sampleReport = { id: 'r1', teacher: 'ajarn_korn', teacherDisplay: 'ครูกร', student: 'nam', text: 'มีการถูกรังแกในห้องพัก', time: new Date().toLocaleString(), viewed:false, flagged:true, score:2, matches:['รังแก'] };
  s.users['ajarn_nu'].inboxReports.push(sampleReport);
  s.users['nam'].reports.push(sampleReport);

  // sample appointment request
  const ap = { id:'a1', teacher:'ajarn_nu', student:'pong', msg:'ขอนัดปรึกษาเรื่องการเรียน', status:'pending', time: new Date().toLocaleString(), iso:new Date().toISOString() };
  s.users['pong'].appts.push(ap);
  s.users['ajarn_nu'].inbox.push(ap);

  // sample redeem request
  const req = { id:'re1', student:'siwarat', item:'คูปองเครื่องเขียน', cost:12, time:new Date().toLocaleString(), status:'pending' };
  s.redeemRequests.push(req);

  s.activity.unshift({ txt:'ตัวอย่างข้อมูลถูกสร้างไว้สำหรับการทดสอบ', time:new Date().toLocaleString() });
}

function loadState(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(!raw){ const s=defaultState(); seedSampleData(s); localStorage.setItem(STORAGE_KEY, JSON.stringify(s)); return s; }
    const parsed = JSON.parse(raw);
    if(!parsed.users || Object.keys(parsed.users).length===0){ seedSampleData(parsed); localStorage.setItem(STORAGE_KEY, JSON.stringify(parsed)); }
    Object.values(parsed.users||{}).forEach(u=>{
      if(!u.moods) u.moods=[];
      if(!u.diaries) u.diaries=[];
      if(!u.reports) u.reports=[];
      if(u.role==='teacher' && !u.inboxReports) u.inboxReports=[];
      if(u.role==='teacher' && !u.inbox) u.inbox=[];
    });
    if(!parsed.redeemRequests) parsed.redeemRequests=[];
    return parsed;
  }catch(e){
    const s=defaultState(); seedSampleData(s); return s;
  }
}
function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

/* ---------- utilities ---------- */
function generateId(){ return 'id_' + Math.random().toString(36).slice(2,9); }
function logActivity(txt){ const time = new Date().toLocaleString(); state.activity.unshift({txt,time}); saveState(); renderActivity(); }
function hexToRgba(hex, a){ if(hex.startsWith('#')) hex = hex.slice(1); const bigint = parseInt(hex,16); const r=(bigint>>16)&255; const g=(bigint>>8)&255; const b=bigint&255; return `rgba(${r},${g},${b},${a})`; }
function isSameDayIso(a,b){ try{ const da=new Date(a), db=new Date(b); return da.getFullYear()===db.getFullYear() && da.getMonth()===db.getMonth() && da.getDate()===db.getDate(); }catch(e){return false;} }
function escapeHtml(s){ return s ? s.replace(/[&<"'>]/g,m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'}[m])) : ''; }

/* ---------- Auth UI ---------- */
const userSelect = document.getElementById('userSelect');
const selectedRole = document.getElementById('selectedRole');
function populateUserSelect(){
  userSelect.innerHTML = '';
  Object.values(state.users).forEach(u=>{
    const opt = document.createElement('option'); opt.value=u.name; opt.textContent = `${u.display||u.name} — ${u.role}`; userSelect.appendChild(opt);
  });
  updateSelectedRole();
}
userSelect.addEventListener('change', ()=> {
  const v = userSelect.value;
  if(v && state.users[v]) selectedRole.innerText = state.users[v].role;
  else selectedRole.innerText = '-';
});
document.getElementById('showCreate').addEventListener('click', ()=> {
  document.getElementById('createRow').style.display = document.getElementById('createRow').style.display === 'none' ? 'block' : 'none';
});
document.getElementById('createBtn').addEventListener('click', ()=>{
  const name = document.getElementById('newName').value.trim();
  const role = document.getElementById('newRole').value;
  if(!name) return alert('กรุณากรอกชื่อผู้ใช้');
  if(state.users[name]) return alert('ชื่อผู้ใช้นี้มีอยู่แล้ว');
  const u = { name, display:name, role, avatar:'', moods:[], diaries:[], appts:[], reports:[] };
  if(role==='student'){ u.classId='M1A'; u.grade='ม.1'; u.stars=0; u.redeemHistory=[]; }
  if(role==='teacher'){ u.inbox=[]; u.inboxReports=[]; u.reports=[]; }
  state.users[name] = u; saveState(); populateUserSelect(); alert('สร้างบัญชีเรียบร้อย'); document.getElementById('newName').value=''; document.getElementById('createRow').style.display='none';
});
document.getElementById('loginBtn').addEventListener('click', ()=>{
  const v = userSelect.value;
  if(!v) return alert('กรุณาเลือกบัญชี');
  loginAs(v);
});
function loginAs(name){
  currentUser = name;
  const u = state.users[name];
  document.getElementById('authCard').style.display = 'none';
  document.getElementById('mainArea').style.display = 'block';
  document.getElementById('logoutBtn').style.display = 'inline-block';
  document.getElementById('currentUserBox').innerText = `${u.display||u.name} (${u.role})`;
  renderAll();
}
document.getElementById('logoutBtn').addEventListener('click', ()=> {
  currentUser = null;
  document.getElementById('authCard').style.display = '';
  document.getElementById('mainArea').style.display = 'none';
  document.getElementById('logoutBtn').style.display = 'none';
  document.getElementById('currentUserBox').innerText = '';
});

/* ---------- Render mood buttons & save (teacher) ---------- */
function renderMoodButtons(containerId){
  const container = document.getElementById(containerId);
  if(!container) return;
  container.innerHTML = '';
  emojiChoices.forEach(e=>{
    const btn = document.createElement('button'); btn.className='emoji-btn'; btn.dataset.key=e.key; btn.innerHTML = `<div>${e.emoji}</div><div class="label">${e.label}</div>`;
    btn.style.background = `linear-gradient(180deg,#fff, ${hexToRgba(e.color,0.08)})`;
    btn.addEventListener('click', ()=> { Array.from(container.querySelectorAll('.emoji-btn')).forEach(b=>b.classList.remove('selected')); btn.classList.add('selected'); });
    container.appendChild(btn);
  });
}
renderMoodButtons('teacherMoodButtons');

document.getElementById('saveTeacherMoodBtn').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบเป็นครู');
  const u = state.users[currentUser];
  if(!u || u.role!=='teacher') return alert('ต้องเป็นครูเท่านั้น');
  const sel = document.querySelector('#teacherMoodButtons .emoji-btn.selected');
  if(!sel) return alert('กรุณาเลือกรูปอารมณ์');
  const key = sel.dataset.key;
  const meta = emojiChoices.find(x=>x.key===key);
  const note = document.getElementById('teacherDiaryText').value.trim();
  const now = new Date();
  const entry = { iso: now.toISOString(), time: now.toLocaleString(), key, emoji: meta.emoji, label: meta.label, note };
  u.moods = u.moods || []; u.moods.push(entry);
  u.diaries = u.diaries || []; u.diaries.push({ time: entry.time, text: note, emoji: meta.emoji, label: meta.label, iso: entry.iso });
  saveState(); logActivity(`${currentUser} บันทึกอารมณ์: ${meta.emoji} ${meta.label}`);
  document.getElementById('teacherDiaryText').value = '';
  Array.from(document.querySelectorAll('#teacherMoodButtons .emoji-btn')).forEach(b=>b.classList.remove('selected'));
  renderAll();
});

/* ---------- teacher pie (self) ---------- */
function getMoodCountsForDate(user, dateIso){
  const res = {}; emojiChoices.forEach(e=> res[e.label]=0);
  if(!user || !user.moods) return res;
  user.moods.forEach(m=> { if(isSameDayIso(m.iso, dateIso)) res[m.label] = (res[m.label]||0) + 1; });
  return res;
}
function renderTeacherSelfPie(){
  const canvas = document.getElementById('teacherSelfMoodPie');
  if(!canvas || !currentUser) return;
  const u = state.users[currentUser];
  const counts = getMoodCountsForDate(u, new Date().toISOString());
  const labels = [], data = [], bg = [];
  emojiChoices.forEach(e=> { labels.push(e.label); data.push(counts[e.label]||0); bg.push(e.color); });
  if(teacherSelfPie) teacherSelfPie.destroy();
  teacherSelfPie = new Chart(canvas.getContext('2d'), { type:'pie', data:{ labels, datasets:[{ data, backgroundColor:bg }] }, options:{ responsive:true, plugins:{legend:{display:false}} } });
  const legend = document.getElementById('teacherSelfLegend'); legend.innerHTML = '';
  labels.forEach((l,i)=> legend.innerHTML += `<div class="legend-row"><div class="swatch" style="background:${bg[i]}"></div><div>${l}: ${data[i]}</div></div>`);
}

/* ---------- risk list ---------- */
function studentRiskInfo(student){
  const reasons = [];
  const now = new Date();
  const moods = student.moods || [];
  const negativeLabels = ['เศร้า','โกรธ','เหนื่อย'];
  const sevenDaysAgo = new Date(); sevenDaysAgo.setDate(now.getDate()-7);
  const recent = moods.filter(m => m.iso && new Date(m.iso) >= sevenDaysAgo);
  const negCount = recent.reduce((acc,m)=> acc + (negativeLabels.includes(m.label) ? 1 : 0), 0);
  if(negCount >= 2) reasons.push(`มี ${negCount} ครั้งของอารมณ์เชิงลบใน 7 วัน`);
  // flagged reports in 30 days
  const thirty = new Date(); thirty.setDate(now.getDate()-30);
  const reports = (student.reports || []).filter(r => r.time && new Date(r.time) >= thirty);
  const flaggedReports = reports.filter(r => r.flagged);
  if(flaggedReports.length) reasons.push(`มี ${flaggedReports.length} รายงานเชิงลบใน 30 วัน`);
  if((student.reports||[]).length >= 3) reasons.push(`มี ${student.reports.length} รายงานทั้งหมด`);
  let level = null;
  if(reasons.length >= 2) level = 'high';
  else if(reasons.length === 1) level = 'medium';
  return { level, reasons, flaggedReportsCount: flaggedReports.length };
}

function renderRiskList(){
  const el = document.getElementById('teacherRiskList');
  if(!el) return;
  const students = Object.values(state.users).filter(u => u.role === 'student');
  const risks = students.map(s => ({ s, info: studentRiskInfo(s) })).filter(x => x.info.level);
  if(!risks.length){ el.innerHTML = '<div class="meta">ยังไม่มีนักเรียนที่อยู่ในเกณฑ์เสี่ยง</div>'; return; }
  el.innerHTML = risks.map(r=>{
    const s = r.s;
    const avatar = s.avatar ? `<div class="student-avatar"><img src="${s.avatar}" style="width:100%;height:100%;object-fit:cover"></div>` : `<div class="student-avatar">${(s.display||s.name).slice(0,2).toUpperCase()}</div>`;
    const levelClass = r.info.level === 'high' ? 'danger-badge' : 'ok-badge';
    return `<div style="padding:10px;border-bottom:1px solid #f3f6fb;display:flex;gap:12px;align-items:flex-start">
      ${avatar}
      <div style="flex:1">
        <div><strong>${s.display||s.name}</strong> <span class="meta"> • ${s.classId || '-'} ${s.grade||''}</span></div>
        <div class="meta" style="margin-top:6px">${r.info.reasons.join(' • ')}</div>
      </div>
      <div><div class="${levelClass}">${r.info.level === 'high' ? 'เสี่ยงสูง' : 'เสี่ยงปานกลาง'}</div></div>
    </div>`;
  }).join('');
}

/* ---------- advisor inbox (behavior reports) ---------- */
function renderAdvisorInbox(){
  const el = document.getElementById('advisorInbox');
  if(!el || !currentUser) return;
  const u = state.users[currentUser];
  if(u.role !== 'teacher'){ el.innerHTML = '<div class="meta">เฉพาะครูที่ปรึกษาเท่านั้น</div>'; return; }
  const inbox = (u.inboxReports || []).slice().reverse();
  if(!inbox.length){ el.innerHTML = '<div class="meta">ยังไม่มีรายงาน</div>'; return; }
  el.innerHTML = inbox.map(r=>{
    return `<div style="padding:10px;border-bottom:1px solid #f3f6fb">
      <div class="meta">${r.time} — จาก: <strong>${r.teacherDisplay||r.teacher}</strong></div>
      <div style="margin-top:6px"><strong>นักเรียน:</strong> ${state.users[r.student]?.display || r.student}</div>
      <div style="margin-top:6px">${escapeHtml(r.text)}</div>
      ${r.flagged ? `<div style="margin-top:8px"><span class="danger-badge">สัญญาณเชิงลบ (คะแนน ${r.score})</span></div>` : ''}
    </div>`;
  }).join('');
}

/* ---------- students list + modify stars ---------- */
function renderStudentsList(){
  const q = document.getElementById('searchStudent')?.value.trim().toLowerCase();
  const container = document.getElementById('studentsList');
  if(!container) return;
  const students = Object.values(state.users).filter(u => u.role === 'student' && (!q || (u.display||u.name).toLowerCase().includes(q)));
  if(!students.length){ container.innerHTML = '<div class="meta">ไม่มีนักเรียน</div>'; return; }
  container.innerHTML = students.map(s=>{
    const avatar = s.avatar ? `<div class="student-avatar"><img src="${s.avatar}" style="width:100%;height:100%;object-fit:cover"></div>` : `<div class="student-avatar">${(s.display||s.name).slice(0,2).toUpperCase()}</div>`;
    return `<div style="padding:8px;border-bottom:1px solid #f3f6fb;display:flex;align-items:center;gap:10px">
      ${avatar}
      <div style="flex:1">
        <div><strong>${s.display||s.name}</strong></div>
        <div class="meta">ชั้น: ${s.classId||'-'} • ${s.grade||'-'}</div>
      </div>
      <div style="display:flex;align-items:center;gap:8px">
        <div style="font-weight:700;color:var(--primary)">⭐ <span id="star-count-${s.name}">${s.stars||0}</span></div>
        <div style="display:flex;flex-direction:column;gap:6px">
          <button class="addStar" data-name="${s.name}">+1</button>
          <button class="removeStar btn-ghost" data-name="${s.name}">−1</button>
        </div>
      </div>
    </div>`;
  }).join('');
  document.querySelectorAll('.addStar').forEach(b=>b.addEventListener('click', e=> modifyStars(e.target.dataset.name, 1)));
  document.querySelectorAll('.removeStar').forEach(b=>b.addEventListener('click', e=> modifyStars(e.target.dataset.name, -1)));
}
document.getElementById('searchStudent')?.addEventListener('input', renderStudentsList);

function modifyStars(name, delta){
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบเป็นครู');
  const actor = state.users[currentUser];
  if(!actor || actor.role !== 'teacher') return alert('ต้องเป็นครูจึงทำได้');
  const s = state.users[name];
  if(!s) return alert('ไม่พบนักเรียน');
  const old = s.stars||0; s.stars = Math.max(0, old + delta);
  saveState();
  const el = document.getElementById(`star-count-${s.name}`); if(el) el.innerText = s.stars;
  logActivity(`${actor.display||actor.name} ${delta>0?'เพิ่ม':'ลด'} ดาวให้ ${s.display||s.name}: ${old} → ${s.stars}`);
  renderQuickPanel();
}

/* ---------- appointment requests (approve) ---------- */
function renderApptRequests(){
  const el = document.getElementById('apptRequests');
  if(!el || !currentUser) return;
  const inbox = (state.users[currentUser].inbox || []);
  if(!inbox.length){ el.innerHTML = '<div class="meta">ยังไม่มีคำขอนัด</div>'; return; }
  el.innerHTML = inbox.slice().reverse().map(a=>{
    return `<div style="padding:10px;border-bottom:1px solid #f3f6fb">
      <div class="meta">${a.time} — จาก: <strong>${state.users[a.student]?.display || a.student}</strong></div>
      <div style="margin-top:6px">${escapeHtml(a.msg)}</div>
      <div style="margin-top:8px"><button class="approveApptBtn" data-id="${a.id}">อนุมัติ</button> <span class="meta" style="margin-left:8px">${a.status}</span></div>
    </div>`;
  }).join('');
  document.querySelectorAll('.approveApptBtn').forEach(b=>b.addEventListener('click', e=> {
    const id = e.target.dataset.id;
    handleApproveAppt(id);
  }));
}

function handleApproveAppt(id){
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบเป็นครู');
  const t = state.users[currentUser];
  const item = (t.inbox || []).find(x => x.id === id);
  if(!item) return alert('ไม่พบคำขอ');
  item.status = 'approved';
  // update student appt status
  const stu = state.users[item.student];
  if(stu && stu.appts){
    const ap = stu.appts.find(x => x.id === id);
    if(ap) ap.status = 'approved';
  }
  saveState(); logActivity(`${currentUser} อนุมัติคำขอเข้าปรึกษาจาก ${item.student}`); renderAll();
}

/* ---------- redeem requests (approve/reject) ---------- */
function renderTeacherRedeemRequests(){
  const el = document.getElementById('teacherRedeemRequests');
  if(!el) return;
  const pending = (state.redeemRequests || []).filter(r => r.status === 'pending');
  if(!pending.length){ el.innerHTML = '<div class="meta">ยังไม่มีคำขอแลก</div>'; return; }
  el.innerHTML = pending.slice().reverse().map(r=>{
    const s = state.users[r.student];
    return `<div style="padding:10px;border-bottom:1px solid #f3f6fb">
      <div><strong>${s? s.display : r.student}</strong> <div class="meta">${s? (s.classId||'') : ''}</div></div>
      <div class="meta" style="margin-top:6px">${r.item} — ${r.cost} ⭐</div>
      <div style="margin-top:8px"><button class="approveRedeemBtn" data-id="${r.id}">อนุมัติ</button> <button class="rejectRedeemBtn btn-ghost" data-id="${r.id}">ปฏิเสธ</button></div>
    </div>`;
  }).join('');
  document.querySelectorAll('.approveRedeemBtn').forEach(b=>b.addEventListener('click', e=> handleApproveRedeem(e.target.dataset.id)));
  document.querySelectorAll('.rejectRedeemBtn').forEach(b=>b.addEventListener('click', e=> handleRejectRedeem(e.target.dataset.id)));
}

function handleApproveRedeem(id){
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบเป็นครู');
  const req = (state.redeemRequests||[]).find(r=>r.id===id);
  if(!req) return alert('ไม่พบคำขอ');
  const student = state.users[req.student];
  if(!student) return alert('ไม่พบนักเรียน');
  // deduct stars if available
  student.stars = Math.max(0, (student.stars||0) - req.cost);
  student.redeemHistory = student.redeemHistory || [];
  student.redeemHistory.push({ item: req.item, cost: req.cost, time: new Date().toLocaleString(), approvedBy: state.users[currentUser].display || currentUser });
  req.status = 'approved';
  saveState(); logActivity(`${currentUser} อนุมัติการแลกของ ${student.name}: ${req.item}`); renderAll();
}
function handleRejectRedeem(id){
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบเป็นครู');
  const req = (state.redeemRequests||[]).find(r=>r.id===id);
  if(!req) return alert('ไม่พบคำขอ');
  req.status = 'rejected';
  saveState(); logActivity(`${currentUser} ปฏิเสธคำขอแลกของ ${req.student}`); renderAll();
}

/* ---------- quick panel & activity ---------- */
function renderQuickPanel(){
  const el = document.getElementById('quickPanel');
  if(!currentUser){ el.innerHTML=''; return; }
  const u = state.users[currentUser];
  let html = `<div class="meta">บทบาท: ${u.role}</div>`;
  if(u.role === 'teacher'){
    const pendingAppts = (u.inbox||[]).filter(i=>i.status==='pending').length;
    html += `<div style="margin-top:6px"><strong>คำขอนัดรออนุมัติ: ${pendingAppts}</strong></div>`;
  }
  el.innerHTML = html;
}
function renderActivity(){ const el = document.getElementById('activityLog'); if(!el) return; el.innerHTML = (state.activity||[]).map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time}</div><div>${a.txt}</div></div>`).join(''); }

/* ---------- render profile / diary ---------- */
function renderProfile(){
  const avatar = document.getElementById('profileAvatar');
  const name = document.getElementById('profileName');
  const role = document.getElementById('profileRole');
  const pbox = document.getElementById('profileBox');
  if(!currentUser){ avatar.textContent='T'; name.innerHTML='<strong>-</strong>'; role.innerText='-'; pbox.innerText='เข้าสู่ระบบเพื่อดูโปรไฟล์'; return; }
  const u = state.users[currentUser];
  avatar.innerText = (u.display||u.name).slice(0,2).toUpperCase();
  name.innerHTML = `<strong>${u.display||u.name}</strong>`;
  role.innerText = u.role;
  pbox.innerHTML = `<div class="meta">บันทึกล่าสุด:</div>`;
  if(u.moods && u.moods.length){
    const last = u.moods[u.moods.length-1];
    pbox.innerHTML += `<div style="margin-top:6px">${last.time} — ${last.emoji} ${last.label}</div><div class="muted-note" style="margin-top:6px">${last.note||'-'}</div>`;
  } else pbox.innerHTML += `<div class="muted-note" style="margin-top:6px">ยังไม่มีบันทึก</div>`;
}

/* ---------- main render ---------- */
function renderAll(){
  populateUserSelect();
  renderActivity();
  if(!currentUser) return;
  const u = state.users[currentUser];
  // only show teacherPanel if teacher
  document.getElementById('teacherPanel').style.display = (u.role === 'teacher') ? 'block' : 'none';
  document.getElementById('mainArea').style.display = 'block';
  renderProfile();
  renderQuickPanel();
  if(u.role === 'teacher'){
    document.getElementById('teacherLastMood').innerText = u.moods && u.moods.length ? `บันทึกล่าสุด: ${u.moods[u.moods.length-1].emoji} ${u.moods[u.moods.length-1].label}` : 'บันทึกล่าสุด: -';
    renderTeacherSelfPie();
    renderRiskList();
    renderAdvisorInbox();
    renderStudentsList();
    renderApptRequests();
    renderTeacherRedeemRequests();
  }
}

/* ---------- initial ---------- */
populateUserSelect();
renderActivity();

/* show auth area initially */
document.getElementById('authCard').style.display = '';
document.getElementById('mainArea').style.display = 'none';

/* If you want, auto-login a sample teacher for quicker testing */
// loginAs('ajarn_nu');

</script>
</body>
</html>
