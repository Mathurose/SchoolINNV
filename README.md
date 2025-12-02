<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>LiteVibe — ระบบติดตามอารมณ์และพฤติกรรม</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#f6f9ff; --card:#ffffff; --primary:#6c63ff; --accent:#ff6fab; --muted:#6b7280;
  }
  *{box-sizing:border-box}
  body{font-family:"Kanit",sans-serif;background:linear-gradient(180deg,#f3f7ff 0%,#ffffff 100%);margin:0;color:#0b1220}
  .app{max-width:1200px;margin:22px auto;padding:18px}
  header.app-header{display:flex;align-items:center;gap:12px;margin-bottom:18px}
  .logo{display:flex;align-items:center;gap:12px}
  .mark{width:54px;height:54px;border-radius:12px;background:linear-gradient(135deg,var(--primary),var(--accent));display:flex;align-items:center;justify-content:center;color:#fff;font-size:22px;box-shadow:0 12px 30px rgba(108,99,255,0.12)}
  h1{font-size:20px;margin:0;color:var(--primary)}
  .muted{color:var(--muted);font-size:13px}
  .card{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 8px 26px rgba(13,20,39,0.04);margin-bottom:14px}
  .grid{display:grid;grid-template-columns:1fr 360px;gap:14px}
  label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
  input[type=text],select,textarea{width:100%;padding:10px;border-radius:10px;border:1px solid #eef3ff;background:linear-gradient(#fff,#fbfdff);font-size:14px}
  button{background:var(--primary);color:#fff;border:0;padding:10px 12px;border-radius:10px;cursor:pointer;font-weight:600}
  .btn-ghost{background:transparent;border:1px solid rgba(108,99,255,0.12);color:var(--primary)}
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
  .student-avatar{width:48px;height:48px;border-radius:10px;background:linear-gradient(135deg,#fff,#f7fbff);display:inline-flex;align-items:center;justify-content:center;font-weight:700;color:var(--primary);border:1px solid rgba(0,0,0,0.04);overflow:hidden}
  .student-avatar img{width:100%;height:100%;object-fit:cover;display:block}
  .list{max-height:380px;overflow:auto}
  .meta{font-size:12px;color:var(--muted)}
  .diary-item{padding:8px;border-bottom:1px solid #f3f6fb}
  .small{font-size:13px}
  .segmented{display:flex;gap:8px}
  .segmented button{padding:8px 10px;border-radius:8px;border:1px solid #eef3ff;background:#fff}
  @media(max-width:980px){.grid{grid-template-columns:1fr} }
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

    <div style="margin-left:auto;display:flex;gap:10px;align-items:center">
      <div id="currentUserBox" class="muted"></div>
      <button id="logoutBtn" class="btn-ghost" style="display:none">ออกจากระบบ</button>
    </div>
  </header>

  <!-- AUTH -->
  <div id="authCard" class="card">
    <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
      <div style="flex:1;min-width:220px">
        <label>เลือกบัญชี</label>
        <select id="userSelect"></select>
      </div>
      <div style="min-width:160px">
        <label>บทบาท</label>
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
          <label>ชื่อผู้ใช้</label>
          <input id="newName" placeholder="เช่น kan, 630123" />
        </div>
        <div style="width:140px">
          <label>บทบาท</label>
          <select id="newRole"><option value="student">นักเรียน</option><option value="teacher">ครู</option><option value="admin">ผู้บริหาร</option></select>
        </div>
        <div style="display:flex;align-items:flex-end">
          <button id="createBtn" class="btn-ghost">สร้าง</button>
        </div>
      </div>
    </div>

    <div style="margin-top:10px" class="muted">เลือกบัญชีตัวอย่างหรือสร้างบัญชีใหม่ ข้อมูลเก็บในเครื่อง (localStorage)</div>
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
              <div id="studentMoodButtons" class="emoji-row"></div>
            </div>

            <div style="margin-top:12px">
              <label>ข้อความสั้น ๆ / My diary</label>
              <textarea id="studentDiaryText" rows="3" placeholder="เล่าเรื่องสั้น ๆ วันนี้เป็นอย่างไร..."></textarea>
            </div>

            <div style="display:flex;align-items:center;gap:10px;margin-top:10px">
              <button id="saveStudentMoodBtn">บันทึกอารมณ์</button>
              <div class="muted">บันทึกล่าสุด: <span id="lastStudentMoodText">-</span></div>
            </div>

            <div class="card" style="margin-top:12px">
              <div style="display:flex;align-items:center;gap:10px"><div><strong>สถิติอารมณ์</strong></div><div class="muted">ดูภาพรวม</div></div>
              <div class="periods" style="margin-top:10px">
                <button class="periodBtn active" data-period="week">สัปดาห์</button>
                <button class="periodBtn" data-period="month">เดือน</button>
                <button class="periodBtn" data-period="semester">ภาคการศึกษา</button>
              </div>
              <div class="chart-wrap"><canvas id="moodPeriodChart" height="170"></canvas></div>
              <div style="margin-top:10px" class="muted small">กราฟอัปเดตเมื่อบันทึกอารมณ์</div>
            </div>

            <div class="card" style="margin-top:12px">
              <strong>ประวัติ My diary</strong>
              <div id="studentDiaryHistory" class="list" style="margin-top:8px"></div>
            </div>

          </div>

        </div>

        <!-- TEACHER PANEL -->
        <div id="teacherPanel" style="display:none">
          <div class="card">
            <div style="display:flex;align-items:center;gap:12px">
              <div><strong>แผงครู</strong></div>
              <div class="muted">ดูสถิติอารมณ์รายบุคคล / รายห้อง / รายชั้นปี</div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <div style="display:flex;gap:12px;align-items:center">
              <div>
                <label>โหมดสถิติ</label>
                <div class="segmented" style="margin-top:6px">
                  <button class="teacherModeBtn active" data-mode="student">รายบุคคล</button>
                  <button class="teacherModeBtn" data-mode="class">รายห้องเรียน</button>
                  <button class="teacherModeBtn" data-mode="grade">รายชั้นปี</button>
                </div>
              </div>

              <div style="flex:1">
                <label id="teacherSelectLabel">เลือกนักเรียน</label>
                <select id="teacherSelect"></select>
              </div>

              <div style="min-width:140px">
                <label>ช่วงเวลา</label>
                <select id="teacherPeriod"><option value="week">สัปดาห์</option><option value="month">เดือน</option><option value="semester">ภาคการศึกษา</option></select>
              </div>

              <div style="display:flex;align-items:flex-end">
                <button id="teacherViewBtn" class="btn-ghost">แสดงกราฟ</button>
              </div>
            </div>

            <div style="margin-top:12px" class="chart-wrap">
              <canvas id="teacherDetailChart" height="200"></canvas>
            </div>
            <div class="muted small" style="margin-top:8px">สามารถเปลี่ยนโหมดเป็นรายบุคคล / ห้อง / ชั้นปี แล้วกด "แสดงกราฟ"</div>
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

        </div>

        <!-- ADMIN PANEL -->
        <div id="adminPanel" style="display:none">
          <div class="card">
            <div style="display:flex;align-items:center;gap:12px">
              <div><strong>แดชบอร์ดผู้บริหาร</strong></div>
              <div class="muted">สถิติภาพรวมโรงเรียน</div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>สรุปอารมณ์นักเรียน (ภาพรวม)</h4>
            <div class="chart-wrap"><canvas id="adminMoodChart" height="140"></canvas></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>สรุปพฤติกรรม / รายงาน</h4>
            <div style="display:flex;gap:12px;align-items:center">
              <div>
                <div class="small muted">จำนวนรายงานรวม</div>
                <div id="adminTotalReports" style="font-weight:700;font-size:18px;margin-top:6px">0</div>
              </div>
              <div>
                <div class="small muted">จำนวนการแลกของรางวัลรวม</div>
                <div id="adminTotalRedeems" style="font-weight:700;font-size:18px;margin-top:6px">0</div>
              </div>
              <div>
                <div class="small muted">จำนวนนักเรียนทั้งหมด</div>
                <div id="adminTotalStudents" style="font-weight:700;font-size:18px;margin-top:6px">0</div>
              </div>
            </div>
            <div style="margin-top:12px" class="chart-wrap">
              <canvas id="adminReportsByGrade" height="140"></canvas>
            </div>
          </div>
        </div>

      </div>

      <div>
        <div class="card">
          <div style="display:flex;align-items:center;gap:12px">
            <div class="student-avatar" id="profileAvatar"></div>
            <div>
              <div id="profileName"><strong>-</strong></div>
              <div id="profileRole" class="meta">-</div>
              <div id="profileClass" class="meta" style="margin-top:6px"></div>
            </div>
            <div style="margin-left:auto"><span class="badge" id="profileStarsWrap">⭐ <span id="profileStars">0</span></span></div>
          </div>

          <div id="profileBox" style="margin-top:12px"></div>

          <div style="margin-top:12px">
            <label>อัปโหลดรูปประจำตัว (จำลอง)</label>
            <input type="file" id="avatarInput" accept="image/*" />
            <div style="margin-top:8px"><button id="removeAvatar" class="btn-ghost">ลบรูปประจำตัว</button></div>
            <div class="meta" style="margin-top:8px">รูปจะถูกเก็บในเครื่อง (localStorage)</div>
          </div>
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
/* LiteVibe — extended for:
   - Teacher: show mood stats per student / per class / per grade
   - Admin: admindashboard with overall school stats
   Data stored in localStorage under key 'litevibe_data_v3'
*/

const STORAGE_KEY = 'litevibe_data_v3';
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
let teacherDetailChart = null;
let adminMoodChart = null;
let adminReportsChart = null;

/* ---------- storage & seed ---------- */
function defaultState(){ return { users: {}, activity: [] }; }

function seedSampleData(s){
  // Students with class and grade (class id and grade string)
  s.users['nam'] = { name:'nam', display:'น้ำ', role:'student', classId:'M1A', grade:'ม.1', stars:8, avatar:'', moods:[{iso:'2025-11-25T08:10:00.000Z', time:'2025-11-25 08:10', key:'happy', emoji:'🙂', label:'มีความสุข', note:'ตื่นสบาย' }], diaries:[{time:'2025-11-25 08:10',text:'วันนี้มีสอบวิทย์'}], appts:[], redeemHistory:[], quiz:[], reports:[] };
  s.users['pong'] = { name:'pong', display:'ป้อง', role:'student', classId:'M2B', grade:'ม.2', stars:12, avatar:'', moods:[{iso:'2025-11-29T07:50:00.000Z', time:'2025-11-29 07:50', key:'neutral', emoji:'😐', label:'เฉย ๆ', note:'เหนื่อย' }], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  s.users['siwarat'] = { name:'siwarat', display:'ศิวรัตน์', role:'student', classId:'M1A', grade:'ม.1', stars:3, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  s.users['kanya'] = { name:'kanya', display:'กัญญา', role:'student', classId:'M3C', grade:'ม.3', stars:6, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };

  // Teachers
  s.users['ajarn_nu'] = { name:'ajarn_nu', display:'ครูหนู', role:'teacher', avatar:'', moods:[], diaries:[], inbox:[], reports:[] };
  s.users['ajarn_korn'] = { name:'ajarn_korn', display:'ครูกร', role:'teacher', avatar:'', moods:[], diaries:[], inbox:[], reports:[] };

  // Admin
  s.users['principal'] = { name:'principal', display:'ผู้บริหาร', role:'admin', avatar:'', notes:[] };

  s.activity.unshift({txt:'LiteVibe: ตัวอย่างข้อมูลถูกสร้างเพื่อทดลอง', time:new Date().toLocaleString()});
}

function loadState(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(!raw){ const s = defaultState(); seedSampleData(s); localStorage.setItem(STORAGE_KEY, JSON.stringify(s)); return s; }
    const parsed = JSON.parse(raw);
    if(!parsed.users || Object.keys(parsed.users).length===0){ seedSampleData(parsed); localStorage.setItem(STORAGE_KEY, JSON.stringify(parsed)); }
    return parsed;
  }catch(e){ const s = defaultState(); seedSampleData(s); return s; }
}
function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

/* ---------- utilities ---------- */
function generateId(){ return 'id_' + Math.random().toString(36).slice(2,9); }
function logActivity(txt){ const time = new Date().toLocaleString(); state.activity.unshift({txt,time}); saveState(); renderActivity(); }

/* ---------- AUTH UI ---------- */
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
document.getElementById('showCreate').addEventListener('click', ()=> {
  document.getElementById('createRow').style.display = document.getElementById('createRow').style.display==='none' ? 'block' : 'none';
});
document.getElementById('createBtn').addEventListener('click', ()=>{
  const name = document.getElementById('newName').value.trim();
  const role = document.getElementById('newRole').value;
  if(!name) return alert('กรุณากรอกชื่อผู้ใช้');
  if(state.users[name]) return alert('ชื่อผู้ใช้นี้มีอยู่แล้ว');
  const u = { name, display:name, role, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  if(role==='student'){ u.classId='M1A'; u.grade='ม.1'; u.stars=0; }
  if(role==='teacher') u.inbox = [];
  state.users[name] = u;
  saveState(); populateUserSelect();
  alert('สร้างบัญชีเรียบร้อยแล้ว: ' + name);
  document.getElementById('newName').value='';
  document.getElementById('createRow').style.display='none';
});
document.getElementById('loginBtn').addEventListener('click', ()=>{
  const v = userSelect.value;
  if(!v) return alert('กรุณาเลือกบัญชี');
  if(v==='__create__'){ document.getElementById('createRow').style.display='block'; return; }
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

/* initial */
populateUserSelect();

/* ---------- Profile & Avatar ---------- */
const avatarInput = document.getElementById('avatarInput');
const removeAvatarBtn = document.getElementById('removeAvatar');
avatarInput.addEventListener('change', (e)=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบก่อนอัปโหลดรูป');
  const file = e.target.files[0];
  if(!file) return;
  const reader = new FileReader();
  reader.onload = function(ev){
    const dataUrl = ev.target.result;
    state.users[currentUser].avatar = dataUrl;
    saveState();
    logActivity(`${currentUser} อัปโหลดรูปประจำตัว`);
    renderAll();
  };
  reader.readAsDataURL(file);
});
removeAvatarBtn.addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  if(!confirm('ต้องการลบรูปประจำตัวหรือไม่?')) return;
  state.users[currentUser].avatar = '';
  saveState();
  renderAll();
});

/* ---------- Render helpers ---------- */
function renderAll(){
  renderProfile();
  renderPanels();
  renderActivity();
  renderTeacherControls();
  renderAdminDashboard();
  renderPeriodChart(currentPeriod);
}
function renderProfile(){
  const avatarBox = document.getElementById('profileAvatar');
  const nameEl = document.getElementById('profileName');
  const roleEl = document.getElementById('profileRole');
  const classEl = document.getElementById('profileClass');
  const starsWrap = document.getElementById('profileStarsWrap');
  const starsEl = document.getElementById('profileStars');
  const box = document.getElementById('profileBox');
  if(!currentUser){ avatarBox.innerHTML='LV'; nameEl.innerHTML='<strong>-</strong>'; roleEl.innerText='-'; classEl.innerText=''; starsWrap.style.display='inline-block'; starsEl.innerText='0'; box.innerHTML='เข้าสู่ระบบเพื่อดูโปรไฟล์'; return; }
  const u = state.users[currentUser];
  avatarBox.innerHTML = '';
  if(u.avatar){ const img = document.createElement('img'); img.src = u.avatar; avatarBox.appendChild(img); } else avatarBox.innerText = (u.display||u.name).slice(0,2).toUpperCase();
  nameEl.innerHTML = `<strong>${u.display || u.name}</strong>`;
  roleEl.innerText = u.role;
  classEl.innerText = (u.role === 'student' ? `ชั้นเรียน: ${u.classId || '-'} • ${u.grade || '-'}` : '');
  if(u.role === 'teacher' || u.role === 'admin') starsWrap.style.display = 'none'; else { starsWrap.style.display = 'inline-block'; starsEl.innerText = u.stars || 0; }
  let html = `<div class="meta">บันทึกล่าสุด:</div>`;
  if(u.moods && u.moods.length){ const last = u.moods[u.moods.length-1]; html += `<div style="margin-top:6px">${last.time} — ${last.emoji} ${last.label}</div><div class="muted" style="margin-top:6px">${last.note || '-'}</div>`; }
  else html += `<div class="muted" style="margin-top:6px">ยังไม่มีบันทึก</div>`;
  box.innerHTML = html;
}

/* ---------- Mood UI & Save (student & teacher keep same) ---------- */
function renderMoodButtons(containerId){
  const container = document.getElementById(containerId);
  if(!container) return;
  container.innerHTML = '';
  emojiChoices.forEach(e=>{
    const btn = document.createElement('button');
    btn.className = 'emoji-btn';
    btn.dataset.key = e.key;
    btn.innerHTML = `<div style="font-size:32px">${e.emoji}</div><div class="label">${e.label}</div>`;
    btn.style.background = `linear-gradient(180deg, rgba(255,255,255,1), ${hexToRgba(e.color,0.06)})`;
    btn.addEventListener('click', ()=>{
      Array.from(container.querySelectorAll('.emoji-btn')).forEach(b=>b.classList.remove('selected'));
      btn.classList.add('selected');
    });
    container.appendChild(btn);
  });
}
renderMoodButtons('studentMoodButtons');
renderMoodButtons('teacherMoodButtons');

document.getElementById('saveStudentMoodBtn').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const sel = document.querySelector('#studentMoodButtons .emoji-btn.selected');
  if(!sel) return alert('กรุณาเลือกรูปอารมณ์');
  const key = sel.dataset.key;
  const meta = emojiChoices.find(x=>x.key===key);
  const note = document.getElementById('studentDiaryText').value.trim();
  const now = new Date();
  const entry = { iso: now.toISOString(), time: now.toLocaleString(), key, emoji: meta.emoji, label: meta.label, note };
  const u = state.users[currentUser];
  u.moods = u.moods || []; u.moods.push(entry);
  if(note){ u.diaries = u.diaries || []; u.diaries.push({time:entry.time, text:note}); }
  saveState(); logActivity(`${currentUser} (นักเรียน) บันทึกอารมณ์: ${meta.emoji} ${meta.label}`);
  document.getElementById('studentDiaryText').value=''; Array.from(document.querySelectorAll('#studentMoodButtons .emoji-btn')).forEach(b=>b.classList.remove('selected'));
  renderAll();
});

document.getElementById('saveTeacherMoodBtn')?.addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const sel = document.querySelector('#teacherMoodButtons .emoji-btn.selected');
  if(!sel) return alert('กรุณาเลือกรูปอารมณ์สำหรับครู');
  const key = sel.dataset.key;
  const meta = emojiChoices.find(x=>x.key===key);
  const note = document.getElementById('teacherDiaryText').value.trim();
  const now = new Date();
  const entry = { iso: now.toISOString(), time: now.toLocaleString(), key, emoji: meta.emoji, label: meta.label, note };
  const u = state.users[currentUser];
  u.moods = u.moods || []; u.moods.push(entry);
  if(note){ u.diaries = u.diaries || []; u.diaries.push({time:entry.time, text:note}); }
  saveState(); logActivity(`${currentUser} (ครู) บันทึกอารมณ์: ${meta.emoji} ${meta.label}`);
  document.getElementById('teacherDiaryText').value=''; Array.from(document.querySelectorAll('#teacherMoodButtons .emoji-btn')).forEach(b=>b.classList.remove('selected'));
  renderAll();
});

/* ---------- Diary rendering (student & teacher) ---------- */
function renderDiaryHistoryForUser(user, containerId){
  const el = document.getElementById(containerId);
  if(!el) return;
  if(!user.diaries || !user.diaries.length){ el.innerHTML = '<div class="meta">ยังไม่มีบันทึก My diary</div>'; return; }
  el.innerHTML = user.diaries.slice().reverse().map(d=>`<div class="diary-item"><div class="meta">${d.time}</div><div style="margin-top:6px">${escapeHtml(d.text)}</div></div>`).join('');
}

/* ---------- Redeem/Appt/Quiz/Reports (kept) ---------- */
document.getElementById('openRedeem')?.addEventListener('click', ()=>{
  const p = document.getElementById('redeemPanel');
  if(!p) return;
  if(p.style.display==='block'){ p.style.display='none'; return; }
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
    u.stars -= cost; u.redeemHistory = u.redeemHistory || []; u.redeemHistory.push({item:name,cost,time:new Date().toLocaleString()});
    saveState(); logActivity(`${currentUser} แลก ${name} (-${cost} ⭐)`); renderAll();
  }
});

/* appointments: teacher select for students */
function populateTeachersForAppt(){
  const sel = document.getElementById('apptTeacherSelect');
  if(!sel) return;
  sel.innerHTML = '<option value="">-- เลือกครู --</option>';
  Object.values(state.users).filter(u=>u.role==='teacher').forEach(t=>{
    const opt = document.createElement('option'); opt.value = t.name; opt.innerText = t.display || t.name; sel.appendChild(opt);
  });
}

/* appointment send */
document.getElementById('requestAppt')?.addEventListener('click', ()=> {
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const t = document.getElementById('apptTeacherSelect')?.value;
  const msg = document.getElementById('apptMsg')?.value.trim();
  if(!t || !msg) return alert('กรุณาเลือกครูและกรอกข้อความ');
  const appt = { id: generateId(), teacher: t, student: currentUser, msg, status:'pending', time: new Date().toLocaleString(), teacherNote:'', iso:new Date().toISOString() };
  const u = state.users[currentUser]; u.appts = u.appts || []; u.appts.push(appt);
  if(!state.users[t]) state.users[t] = { name:t, display:t, role:'teacher', inbox:[], moods:[], diaries:[], reports:[], stars:0, avatar:'' };
  state.users[t].inbox = state.users[t].inbox || []; state.users[t].inbox.push(appt);
  saveState(); logActivity(`${currentUser} ขอเข้าปรึกษากับ ${t}`); renderAll();
  document.getElementById('apptMsg').value = '';
});

/* quiz (unchanged) */
const quizzes = {
  basic: [
    {q:'ในสัปดาห์ที่ผ่านมาคุณรู้สึกมีความสุขบ่อยแค่ไหน?', options:['ไม่เลย','บางครั้ง','บ่อย','ตลอดเวลา'], scores:[0,1,2,3]},
    {q:'คุณนอนหลับเพียงพอหรือไม่?', options:['ไม่เลย','บางครั้ง','พอประมาณ','เพียงพอ'], scores:[0,1,2,3]},
    {q:'คุณรู้สึกว่ามีคนคอยรับฟังเมื่อคุณต้องการหรือไม่?', options:['ไม่เลย','บางครั้ง','บ่อย','ตลอดเวลา'], scores:[0,1,2,3]}
  ]
};
let currentQuizKey = '';
document.getElementById('startQuiz')?.addEventListener('click', ()=>{
  const sel = document.getElementById('quizSelect')?.value;
  if(!sel) return alert('กรุณาเลือกแบบทดสอบก่อนเริ่ม');
  currentQuizKey = sel; renderQuizQuestions(sel);
  document.getElementById('startQuiz').style.display='none'; document.getElementById('submitQuiz').style.display='inline-block';
});
document.getElementById('submitQuiz')?.addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  if(!currentQuizKey) return alert('ไม่มีแบบทดสอบ');
  const quiz = quizzes[currentQuizKey];
  let total=0, answered=true;
  quiz.forEach((qq,i)=>{ const v = document.querySelector(`input[name="q${i}"]:checked`); if(!v) answered=false; else total += qq.scores[parseInt(v.value)]; });
  if(!answered) return alert('กรุณาตอบทุกข้อ');
  state.users[currentUser].quiz = state.users[currentUser].quiz || []; state.users[currentUser].quiz.push({time:new Date().toLocaleString(), score:total, type:currentQuizKey});
  saveState(); logActivity(`${currentUser} ทำแบบทดสอบ ${currentQuizKey} (คะแนน ${total})`);
  document.getElementById('quizContainer').innerHTML = `<div class="meta">ผลคะแนน: <strong>${total}</strong></div>`;
  document.getElementById('submitQuiz').style.display='none'; document.getElementById('startQuiz').style.display='inline-block';
  currentQuizKey=''; document.getElementById('quizSelect').value='';
});
function renderQuizQuestions(key){
  const quiz = quizzes[key];
  const qEl = document.getElementById('quizContainer');
  qEl.innerHTML = '';
  quiz.forEach((qq,i)=>{ const div = document.createElement('div'); div.style.padding='8px'; div.innerHTML = `<div><strong>Q${i+1}.</strong> ${qq.q}</div>`; qq.options.forEach((opt,j)=>{ const id = `q${i}_o${j}`; div.innerHTML += `<div style="margin-left:8px"><input type="radio" name="q${i}" id="${id}" value="${j}"> <label for="${id}">${opt}</label></div>`; }); qEl.appendChild(div); });
}

/* ---------- Teacher controls & charting ---------- */
function renderTeacherControls(){
  // Populate teacherSelect according to mode (default student)
  const modeBtns = document.querySelectorAll('.teacherModeBtn');
  modeBtns.forEach(b=>b.addEventListener('click', ()=>{
    modeBtns.forEach(x=>x.classList.remove('active')); b.classList.add('active');
    updateTeacherSelectLabel();
  }));
  updateTeacherSelectLabel();
  document.getElementById('teacherViewBtn')?.addEventListener('click', ()=> {
    const mode = document.querySelector('.teacherModeBtn.active')?.dataset.mode || 'student';
    const id = document.getElementById('teacherSelect')?.value;
    const period = document.getElementById('teacherPeriod')?.value || 'week';
    renderTeacherDetail(mode, id, period);
  });
  updateTeacherSelectLabel();
}

function updateTeacherSelectLabel(){
  const mode = document.querySelector('.teacherModeBtn.active')?.dataset.mode || 'student';
  const label = document.getElementById('teacherSelectLabel');
  const sel = document.getElementById('teacherSelect');
  if(!label || !sel) return;
  sel.innerHTML = '';
  if(mode === 'student'){
    label.innerText = 'เลือกนักเรียน';
    Object.values(state.users).filter(u=>u.role==='student').forEach(s=> {
      const opt = document.createElement('option'); opt.value = s.name; opt.innerText = `${s.display||s.name} • ${s.classId || ''} ${s.grade || ''}`; sel.appendChild(opt);
    });
  } else if(mode === 'class'){
    label.innerText = 'เลือกห้องเรียน';
    const classes = Array.from(new Set(Object.values(state.users).filter(u=>u.role==='student').map(s=>s.classId || '')));
    classes.forEach(c=>{ const opt = document.createElement('option'); opt.value = c; opt.innerText = c || '-'; sel.appendChild(opt); });
  } else {
    label.innerText = 'เลือกชั้นปี';
    const grades = Array.from(new Set(Object.values(state.users).filter(u=>u.role==='student').map(s=>s.grade || '')));
    grades.forEach(g=>{ const opt = document.createElement('option'); opt.value = g; opt.innerText = g || '-'; sel.appendChild(opt); });
  }
}

/* render teacher detail chart based on mode */
function renderTeacherDetail(mode, id, period){
  // gather list of students for this mode
  let students = [];
  if(mode === 'student'){
    if(!id) return alert('กรุณาเลือกนักเรียน');
    const s = state.users[id];
    if(!s) return alert('ไม่พบข้อมูลนักเรียน');
    students = [s];
  } else if(mode === 'class'){
    if(!id) return alert('กรุณาเลือกห้องเรียน');
    students = Object.values(state.users).filter(u=>u.role==='student' && (u.classId === id));
  } else {
    if(!id) return alert('กรุณาเลือกชั้นปี');
    students = Object.values(state.users).filter(u=>u.role==='student' && (u.grade === id));
  }

  // aggregate moods across students by the chosen period
  const labels = [];
  const moodLabels = emojiChoices.map(e=>e.label);
  const aggregatedData = []; // array of objects per label/time bucket
  // reuse aggregateByPeriod but for multiple students -> combine all moods of students
  const combinedMoods = [];
  students.forEach(s=> { if(s.moods) combinedMoods.push(...s.moods); });
  const agg = aggregateByPeriod(combinedMoods, period);
  const buckets = agg.labels;
  const datasets = emojiChoices.map(e=>{
    return { label: e.label, data: agg.data.map(d=>d[e.label]||0), backgroundColor: hexToRgba(e.color,0.95), stack:'s1' };
  });

  const ctx = document.getElementById('teacherDetailChart').getContext('2d');
  if(teacherDetailChart) teacherDetailChart.destroy();
  teacherDetailChart = new Chart(ctx, {
    type: 'bar',
    data: { labels: agg.labels, datasets },
    options: {
      responsive:true,
      plugins:{legend:{position:'bottom'}},
      scales:{ x:{ stacked:true }, y:{ stacked:true, beginAtZero:true, ticks:{precision:0} } }
    }
  });
}

/* ---------- Admin dashboard rendering ---------- */
function renderAdminDashboard(){
  if(!currentUser) return;
  const u = state.users[currentUser];
  // show admin panel only if role=admin
  document.getElementById('adminPanel').style.display = (u.role === 'admin') ? 'block' : 'none';
  if(u.role !== 'admin') return;

  // overall mood distribution across all students (latest mood per student)
  const moodCounts = {};
  emojiChoices.forEach(e=>moodCounts[e.label]=0);
  const students = Object.values(state.users).filter(x=>x.role==='student');
  students.forEach(s=>{
    if(s.moods && s.moods.length){
      const last = s.moods[s.moods.length-1];
      moodCounts[last.label] = (moodCounts[last.label]||0) + 1;
    }
  });
  const labels = Object.keys(moodCounts);
  const data = Object.values(moodCounts);
  const ctxMood = document.getElementById('adminMoodChart').getContext('2d');
  if(adminMoodChart) adminMoodChart.destroy();
  adminMoodChart = new Chart(ctxMood, { type:'doughnut', data:{ labels, datasets:[{ data, backgroundColor: emojiChoices.map(e=>e.color) }] }, options:{responsive:true, plugins:{legend:{position:'bottom'}}} });

  // behavior metrics
  const totalReports = Object.values(state.users).reduce((acc,u)=> acc + ((u.reports||[]).length), 0);
  const totalRedeems = Object.values(state.users).reduce((acc,u)=> acc + ((u.redeemHistory||[]).length), 0);
  document.getElementById('adminTotalReports').innerText = totalReports;
  document.getElementById('adminTotalRedeems').innerText = totalRedeems;
  document.getElementById('adminTotalStudents').innerText = students.length;

  // Reports by grade (bar chart)
  const grades = Array.from(new Set(students.map(s=>s.grade||'ไม่ระบุ')));
  const reportsByGrade = grades.map(g=> {
    const list = students.filter(s=> (s.grade||'ไม่ระบุ') === g);
    return list.reduce((acc,stu)=> acc + ((stu.reports||[]).length), 0);
  });
  const ctxRep = document.getElementById('adminReportsByGrade').getContext('2d');
  if(adminReportsChart) adminReportsChart.destroy();
  adminReportsChart = new Chart(ctxRep, {
    type:'bar',
    data: { labels: grades, datasets:[{ label:'จำนวนรายงาน', data:reportsByGrade, backgroundColor: '#6c63ff' }] },
    options:{ responsive:true, plugins:{legend:{display:false}} }
  });
}

/* ---------- Panels rendering ---------- */
function renderPanels(){
  if(!currentUser) return;
  const u = state.users[currentUser];
  // show/hide panels according to role
  document.getElementById('studentPanel').style.display = u.role === 'student' ? 'block' : 'none';
  document.getElementById('teacherPanel').style.display = u.role === 'teacher' ? 'block' : 'none';
  document.getElementById('adminPanel').style.display = u.role === 'admin' ? 'block' : 'none';

  // student view
  if(u.role === 'student'){
    document.getElementById('lastStudentMoodText').innerText = u.moods && u.moods.length ? `${u.moods[u.moods.length-1].emoji} ${u.moods[u.moods.length-1].label} — ${u.moods[u.moods.length-1].time}` : '-';
    renderDiaryHistoryForUser(u, 'studentDiaryHistory');
  }

  // teacher view: nothing immediate, teacher controls populate dynamically
  if(u.role === 'teacher'){
    renderApptRequests();
    renderReportsList();
    buildReportStudentSelect();
  }

  // admin view handled separately in renderAdminDashboard

  renderQuickPanel();
  populateTeachersForAppt();
}

/* ---------- Charts for student personal periods (reuse) ---------- */
const ctxPeriod = document.getElementById('moodPeriodChart')?.getContext('2d');
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
  const aggregated = aggregateByPeriod(u.moods || [], period);
  const datasets = emojiChoices.map(e=>({ label:e.label, data: aggregated.data.map(d=>d[e.label]||0), backgroundColor: hexToRgba(e.color,0.95), stack:'s1' }));
  if(!ctxPeriod) return;
  if(periodChart) periodChart.destroy();
  periodChart = new Chart(ctxPeriod, { type:'bar', data:{ labels: aggregated.labels, datasets }, options:{ responsive:true, plugins:{legend:{position:'bottom'}}, scales:{ x:{stacked:true}, y:{stacked:true, beginAtZero:true, ticks:{precision:0}} } } });
}

/* aggregate helper (used by teacher and personal) */
function aggregateByPeriod(moods, period){
  const now = new Date();
  if(period === 'week'){
    const days = []; for(let i=6;i>=0;i--){ const d = new Date(); d.setDate(now.getDate()-i); days.push(dateKey(d)); }
    const data = days.map(_=> ({}));
    moods.forEach(entry=>{ if(!entry.iso) return; const d = new Date(entry.iso); const key = dateKey(d); const idx = days.indexOf(key); if(idx>=0) data[idx][entry.label] = (data[idx][entry.label]||0)+1; });
    return { labels: days.map(d=>formatDayLabel(d)), data };
  } else if(period === 'month'){
    const weeks = []; const weekRanges = [];
    for(let w=0; w<4; w++){ const start = new Date(); start.setDate(now.getDate() - 30 + w*7); const end = new Date(); end.setDate(start.getDate() + 6); weekRanges.push({start, end}); weeks.push(`สัปดาห์ ${w+1}`); }
    const data = weeks.map(_=> ({}));
    moods.forEach(entry=>{ if(!entry.iso) return; const d = new Date(entry.iso); for(let i=0;i<weekRanges.length;i++){ if(d >= stripTime(weekRanges[i].start) && d <= endOfDay(weekRanges[i].end)){ data[i][entry.label] = (data[i][entry.label]||0)+1; break; } } });
    return { labels: weeks, data };
  } else {
    const sem = getCurrentSemester(now); const months = []; const data=[]; let m = new Date(sem.start);
    while(m <= now){ months.push(formatMonthLabel(m)); data.push({}); m.setMonth(m.getMonth()+1); }
    moods.forEach(entry=>{ if(!entry.iso) return; const d = new Date(entry.iso); if(d >= sem.start && d <= now){ const idx = (d.getFullYear()*12 + d.getMonth()) - (sem.start.getFullYear()*12 + sem.start.getMonth()); if(idx>=0 && idx<data.length) data[idx][entry.label] = (data[idx][entry.label]||0)+1; } });
    return { labels: months, data };
  }
}

/* ---------- Teacher/student lists & reports ---------- */
function renderStudentsList(){
  const q = document.getElementById('searchStudent')?.value.trim().toLowerCase();
  const container = document.getElementById('studentsList');
  if(!container) return;
  const students = Object.values(state.users).filter(u=>u.role==='student' && (!q || (u.display||u.name).toLowerCase().includes(q)));
  if(!students.length){ container.innerHTML = '<div class="meta">ไม่มีนักเรียน</div>'; return; }
  container.innerHTML = students.map(s=>{
    const avatarHtml = s.avatar ? `<div class="student-avatar"><img src="${s.avatar}" alt=""></div>` : `<div class="student-avatar">${(s.display||s.name).slice(0,2).toUpperCase()}</div>`;
    return `<div style="padding:8px;border-bottom:1px solid #f3f6fb;display:flex;align-items:center;gap:10px">
      ${avatarHtml}
      <div><strong>${s.display||s.name}</strong><div class="meta">${s.classId || '-'} • ${s.grade || '-'}</div></div>
    </div>`;
  }).join('');
}
document.getElementById('searchStudent')?.addEventListener('input', renderStudentsList);

/* appt inbox for teacher */
function renderApptRequests(){
  const el = document.getElementById('apptRequests');
  if(!currentUser) return;
  const inbox = (state.users[currentUser].inbox || []);
  if(!inbox.length){ el.innerHTML = '<div class="meta">ยังไม่มีคำขอนัด</div>'; return; }
  el.innerHTML = inbox.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time} — จาก: <strong>${a.student}</strong></div><div style="margin-top:6px">${a.msg}</div><div style="margin-top:8px">${a.status==='approved'?'<span class="badge">อนุมัติ</span>':`<button class="approveBtn" data-id="${a.id}">อนุมัติ</button><button class="rejectBtn" data-id="${a.id}">ปฏิเสธ</button>`} <button class="noteBtn" data-id="${a.id}">หมายเหตุ</button></div></div>`).join('');
  document.querySelectorAll('.approveBtn').forEach(b=>b.addEventListener('click', e=>handleApptAction(e.target.dataset.id,'approved')));
  document.querySelectorAll('.rejectBtn').forEach(b=>b.addEventListener('click', e=>handleApptAction(e.target.dataset.id,'rejected')));
  document.querySelectorAll('.noteBtn').forEach(b=>b.addEventListener('click', e=>{ const id = e.target.dataset.id; const note = prompt('หมายเหตุสำหรับการนัด:'); if(note !== null) handleApptNote(id,note); }));
}
function handleApptAction(id,status){
  const t = state.users[currentUser];
  const item = (t.inbox||[]).find(x=>x.id===id);
  if(!item) return;
  item.status = status;
  const stu = state.users[item.student];
  if(stu){ const ap = stu.appts.find(x=>x.id===id); if(ap) ap.status = status; }
  saveState(); logActivity(`${currentUser} ${status==='approved'?'อนุมัติ':'ปฏิเสธ'} นัดจาก ${item.student}`); renderAll();
}
function handleApptNote(id,note){
  const t = state.users[currentUser];
  const item = (t.inbox||[]).find(x=>x.id===id);
  if(!item) return;
  item.teacherNote = note;
  const stu = state.users[item.student];
  if(stu){ const ap = stu.appts.find(x=>x.id===id); if(ap) ap.teacherNote = note; }
  saveState(); logActivity(`${currentUser} บันทึกหมายเหตุนัด ${item.student}`); renderAll();
}

/* reports */
document.getElementById('sendReport')?.addEventListener('click', ()=>{
  const student = document.getElementById('reportStudent')?.value;
  const text = document.getElementById('reportText')?.value.trim();
  if(!student || !text) return alert('เลือกนักเรียนและกรอกข้อความ');
  const r = { id: generateId(), teacher: currentUser, student, text, time: new Date().toLocaleString(), iso: new Date().toISOString() };
  state.users[student].reports = state.users[student].reports || []; state.users[student].reports.push(r);
  saveState(); logActivity(`${currentUser} ส่งรายงาน: ${student}`); renderAll(); document.getElementById('reportText').value='';
});
function renderReportsList(){
  const all = [];
  Object.values(state.users).forEach(u=>{ if(u.reports) u.reports.forEach(r=>{ if(r.teacher === currentUser) all.push(r); })});
  const el = document.getElementById('reportsList');
  if(!el) return;
  if(!all.length){ el.innerHTML = '<div class="meta">ยังไม่มีรายงาน</div>'; return; }
  el.innerHTML = all.map(r=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${r.time} → ${r.student}</div><div>${r.text}</div></div>`).join('');
}
function buildReportStudentSelect(){
  const sel = document.getElementById('reportStudent'); if(!sel) return;
  sel.innerHTML = '<option value="">-- เลือกนักเรียน --</option>';
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
  } else if(u.role === 'teacher'){
    const pending = (u.inbox||[]).filter(i=>i.status==='pending').length;
    html += `<div style="margin-top:6px"><strong>คำขอนัดรออนุมัติ: ${pending}</strong></div>`;
  } else if(u.role === 'admin'){
    const students = Object.values(state.users).filter(x=>x.role==='student').length;
    html += `<div style="margin-top:6px"><strong>นักเรียนทั้งหมด: ${students}</strong></div>`;
  }
  el.innerHTML = html;
}

/* activity */
function renderActivity(){ const el = document.getElementById('activityLog'); el.innerHTML = state.activity.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time}</div><div>${a.txt}</div></div>`).join(''); }

/* ---------- Helpers ---------- */
function dateKey(d){ const dt = new Date(d.getFullYear(), d.getMonth(), d.getDate()); return `${dt.getDate().toString().padStart(2,'0')} ${dt.toLocaleString('th-TH',{month:'short'})}`; }
function formatDayLabel(label){ return label; }
function stripTime(d){ return new Date(d.getFullYear(), d.getMonth(), d.getDate()); }
function endOfDay(d){ return new Date(d.getFullYear(), d.getMonth(), d.getDate(),23,59,59,999); }
function formatMonthLabel(d){ return d.toLocaleString('th-TH',{month:'short', year:'numeric'}); }
function getCurrentSemester(now){ const y = now.getFullYear(); if(now.getMonth() <= 5) return { start: new Date(y,0,1), end: new Date(y,5,30) }; else return { start: new Date(y,6,1), end: new Date(y,11,31) }; }
function hexToRgba(hex, a){ if(hex.startsWith('#')) hex = hex.slice(1); const bigint = parseInt(hex,16); const r = (bigint >> 16) & 255; const g = (bigint >> 8) & 255; const b = bigint & 255; return `rgba(${r},${g},${b},${a})`; }
function escapeHtml(unsafe){ return unsafe ? unsafe.replace(/[&<"'>]/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'}[m])) : ''; }

/* initial renders and event wiring */
renderActivity();
renderAll();

/* keep admin and teacher charts updated periodically */
setInterval(()=>{ renderAdminDashboard(); },5000);

</script>
</body>
</html>
