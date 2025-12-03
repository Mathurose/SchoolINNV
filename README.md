<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>LiteVibe — ระบบติดตามอารมณ์และพฤติกรรม</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#f6f9ff; --card:#ffffff; --primary:#6c63ff; --accent:#ff6fab; --muted:#6b7280;
    --danger:#ef4444; --warning:#f59e0b; --success:#16a34a; --info:#0ea5e9;
  }
  *{box-sizing:border-box}
  body{font-family:"Kanit",sans-serif;background:linear-gradient(180deg,#f3f7ff 0%,#ffffff 100%);margin:0;color:#0b1220}
  .app{max-width:1100px;margin:22px auto;padding:18px}
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
  .risk-item{display:flex;align-items:center;gap:10px;padding:8px;border-bottom:1px solid #f3f6fb}
  .risk-badge{padding:6px 8px;border-radius:8px;color:#fff}
  .risk-high{background:var(--danger)}
  .risk-medium{background:var(--warning)}
  .redeem-item{display:flex;align-items:center;gap:12px;padding:10px;border-radius:10px;border:1px solid #f3f6fb;background:#fbfbff}
  .req-pending{color:var(--info);font-weight:700}
  .req-approved{color:var(--success);font-weight:700}
  .req-rejected{color:#9ca3af;font-weight:700}
  /* appointment status badges (student view) */
  .appt-status{padding:6px 8px;border-radius:999px;color:#fff;font-weight:700;font-size:12px}
  .status-pending{background:var(--info)}
  .status-approved{background:var(--success)}
  .status-rejected{background:#6b7280}
  /* teacher icon statistic buttons */
  .icon-row{display:flex;gap:10px;align-items:center}
  .icon-btn{width:56px;height:56px;border-radius:12px;border:0;background:#fff;display:flex;flex-direction:column;align-items:center;justify-content:center;cursor:pointer;box-shadow:0 8px 20px rgba(16,24,45,0.05);transition:transform .12s}
  .icon-btn:hover{transform:translateY(-6px)}
  .icon-btn .ico{font-size:22px}
  .icon-btn.yellow{background:linear-gradient(135deg,#fff7e6,#fff1cc);border:1px solid rgba(245,158,11,0.08)}
  .icon-btn.pink{background:linear-gradient(135deg,#fff0f6,#ffedf7);border:1px solid rgba(255,122,152,0.08)}
  .icon-btn.blue{background:linear-gradient(135deg,#ecfeff,#e0f2fe);border:1px solid rgba(14,165,233,0.08)}
  /* small label under icon */
  .icon-label{font-size:11px;color:var(--muted);margin-top:4px}
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
        <div class="muted">ระบบติดตามอารมณ์และพฤติกรรม — Prototype</div>
      </div>
    </div>

    <div style="margin-left:auto;display:flex;gap:10px;align-items:center">
      <div id="currentUserBox" class="muted"></div>
      <button id="logoutBtn" class="btn-ghost" style="display:none">ออกจากระบบ</button>
    </div>
  </header>

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
          <input id="newName" placeholder="เช่น khonmek, thongfa" />
        </div>
        <div style="width:160px">
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
              <strong>ระบบแลกดาว</strong>
              <div style="margin-top:8px;display:flex;gap:8px;flex-wrap:wrap">
                <div class="redeem-item">
                  <div><strong>พัก 5 นาที</strong><div class="meta">ใช้ 10 ⭐</div></div>
                  <div style="margin-left:auto"><button class="requestRedeemBtn" data-name="พัก 5 นาที" data-cost="10">ขอแลก</button></div>
                </div>
                <div class="redeem-item">
                  <div><strong>คูปองเครื่องเขียน</strong><div class="meta">ใช้ 12 ⭐</div></div>
                  <div style="margin-left:auto"><button class="requestRedeemBtn" data-name="คูปองเครื่องเขียน" data-cost="12">ขอแลก</button></div>
                </div>
                <div class="redeem-item">
                  <div><strong>คูปองอาหาร/เครื่องดื่ม</strong><div class="meta">ใช้ 15 ⭐</div></div>
                  <div style="margin-left:auto"><button class="requestRedeemBtn" data-name="คูปองอาหาร" data-cost="15">ขอแลก</button></div>
                </div>
              </div>

              <h4 style="margin-top:12px">ประวัติการแลก</h4>
              <div id="studentRedeemHistory" class="list" style="margin-top:8px"></div>

              <h4 style="margin-top:12px">คำขอแลกที่ส่ง (สถานะ)</h4>
              <div id="studentRedeemRequests" class="list" style="margin-top:8px"></div>
            </div>

            <div class="card" style="margin-top:12px">
              <strong>นัดหมายปรึกษา</strong>
              <div style="margin-top:8px">
                <label>เลือกครูที่ต้องการนัด</label>
                <select id="apptTeacherSelect"></select>
                <label style="margin-top:8px">ข้อความสำหรับนัด</label>
                <input id="apptMsg" placeholder="สาเหตุ/หัวข้อที่ต้องการปรึกษา" />
                <div style="margin-top:8px"><button id="requestAppt">ส่งคำขอนัด</button></div>
                <div style="margin-top:10px" class="muted small">สถานะคำขอแสดงเป็นสีชัดเจน</div>
                <div id="apptHistory" class="list" style="margin-top:8px"></div>
              </div>
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
              <div class="muted">ดูสถิติ อนุมัติคำขอแลกดาว และนักเรียนเสี่ยง</div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <div style="display:flex;gap:12px;align-items:center;flex-wrap:wrap">
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

              <div style="display:flex;align-items:center;gap:8px">
                <!-- colorful icon buttons for stats -->
                <div class="icon-row">
                  <button class="icon-btn yellow" id="statIcon1" title="แสดงสถิติ"><div class="ico">📊</div><div class="icon-label">สรุป</div></button>
                  <button class="icon-btn pink" id="statIcon2" title="แสดงสถิติรายวัน"><div class="ico">📈</div><div class="icon-label">รายวัน</div></button>
                  <button class="icon-btn blue" id="statIcon3" title="แสดงเทรนด์"><div class="ico">📅</div><div class="icon-label">เทรนด์</div></button>
                </div>
              </div>
            </div>

            <div style="margin-top:12px" class="chart-wrap">
              <canvas id="teacherDetailChart" height="200"></canvas>
            </div>
            <div class="muted small" style="margin-top:8px">กดไอคอนสีสันด้านบนเพื่อแสดงกราฟโดยใช้โหมดและตัวเลือกปัจจุบัน</div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>คำขอแลกดาวจากนักเรียน</h4>
            <div id="teacherRedeemRequests" class="list"></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>นักเรียนที่มีความเสี่ยง (Emotion & Behavior)</h4>
            <div id="teacherRiskList" class="list"></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>กล่องคำขอนัดจากนักเรียน</h4>
            <div id="apptRequests" class="list"></div>
          </div>

        </div>

        <!-- ADMIN PANEL -->
        <div id="adminPanel" style="display:none">
          <div class="card">
            <div style="display:flex;align-items:center;gap:12px">
              <div><strong>แดชบอร์ดผู้บริหาร</strong></div>
              <div class="muted">สถิติภาพรวมโรงเรียน และนักเรียนที่มีความเสี่ยง</div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>สรุปอารมณ์นักเรียน (ภาพรวม)</h4>
            <div class="chart-wrap"><canvas id="adminMoodChart" height="140"></canvas></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>นักเรียนที่มีความเสี่ยง (ภาพรวมโรงเรียน)</h4>
            <div id="adminRiskList" class="list"></div>
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
/* LiteVibe v4.2 (updated)
   - student: appointment dropdown + colored status badges in appt history
   - teacher: colorful icon buttons to show statistics (use current mode/selection)
   - other functions preserved (redeem requests, approve/reject)
*/

const STORAGE_KEY = 'litevibe_data_v4';
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

/* ---------- storage & seed ---------- */
function defaultState(){ return { users: {}, activity: [], redeemRequests: [] }; }

function seedSampleData(s){
  s.users['khonmek'] = { name:'khonmek', display:'นักเรียนก้อนเมฆ', role:'student', classId:'M1A', grade:'ม.1', stars:5, avatar:'', moods:[
    {iso: isoDaysAgo(6), time: formatDate(isoDaysAgo(6)), key:'sad', emoji:'😢', label:'เศร้า', note:'เครียดเรื่องบ้าน'},
    {iso: isoDaysAgo(3), time: formatDate(isoDaysAgo(3)), key:'tired', emoji:'😴', label:'เหนื่อย', note:'นอนน้อย'}
  ], diaries:[{time:formatDate(isoDaysAgo(6)),text:'รู้สึกไม่ค่อยอยากไปโรงเรียน'}], appts:[], redeemHistory:[], quiz:[], reports:[{time:formatDate(isoDaysAgo(10)),text:'ครูทราบพฤติกรรมไม่ร่วมกิจกรรม'}] };

  s.users['thongfa'] = { name:'thongfa', display:'นักเรียนท้องฟ้า', role:'student', classId:'M1A', grade:'ม.1', stars:8, avatar:'', moods:[
    {iso: isoDaysAgo(2), time: formatDate(isoDaysAgo(2)), key:'happy', emoji:'🙂', label:'มีความสุข', note:'วันนี้สอบผ่าน'}
  ], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };

  s.users['medfon'] = { name:'medfon', display:'นักเรียนเม็ดฝน', role:'student', classId:'M2B', grade:'ม.2', stars:2, avatar:'', moods:[
    {iso: isoDaysAgo(5), time: formatDate(isoDaysAgo(5)), key:'angry', emoji:'😠', label:'โกรธ', note:'ทะเลาะกับเพื่อน'}
  ], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[{time:formatDate(isoDaysAgo(4)),text:'ถูกร้องเรียนเรื่องความประพฤติ'}] };

  s.users['ajarn_somchai'] = { name:'ajarn_somchai', display:'ครูสมชาย', role:'teacher', avatar:'', moods:[], diaries:[], inbox:[], reports:[] };
  s.users['ajarn_somsri'] = { name:'ajarn_somsri', display:'ครูสมศรี', role:'teacher', avatar:'', moods:[], diaries:[], inbox:[], reports:[] };

  s.users['principal'] = { name:'principal', display:'ผู้บริหาร', role:'admin', avatar:'', notes:[] };

  s.redeemRequests = [];
  s.activity.unshift({txt:'LiteVibe: ตัวอย่างข้อมูลถูกสร้างเพื่อทดลอง', time:new Date().toLocaleString()});
}

function loadState(){
  try{
    const raw = localStorage.getItem(STORAGE_KEY);
    if(!raw){ const s = defaultState(); seedSampleData(s); localStorage.setItem(STORAGE_KEY, JSON.stringify(s)); return s; }
    const parsed = JSON.parse(raw);
    if(!parsed.users || Object.keys(parsed.users).length===0){ seedSampleData(parsed); localStorage.setItem(STORAGE_KEY, JSON.stringify(parsed)); }
    if(!parsed.redeemRequests) parsed.redeemRequests = [];
    return parsed;
  }catch(e){ const s = defaultState(); seedSampleData(s); return s; }
}
function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

/* ---------- helpers ---------- */
function isoDaysAgo(days){ const d = new Date(); d.setDate(d.getDate()-days); return d.toISOString(); }
function formatDate(iso){ const d = new Date(iso); return d.toLocaleString(); }
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
  saveState();
  populateUserSelect();
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
  populateTeachersForAppt();
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
  if(u.avatar){
    const img = document.createElement('img'); img.src = u.avatar; avatarBox.appendChild(img);
  } else {
    avatarBox.innerText = (u.display||u.name).slice(0,2).toUpperCase();
  }
  nameEl.innerHTML = `<strong>${u.display || u.name}</strong>`;
  roleEl.innerText = u.role;

  // If user is teacher, hide the profile stars (requirement: remove teachers' stars in teacherdashboard)
  if(u.role === 'teacher'){
    starsWrap.style.display = 'none';
  } else {
    starsWrap.style.display = 'inline-block';
    starsEl.innerText = u.stars || 0;
  }

  let html = `<div class="meta">บันทึกล่าสุด:</div>`;
  if(u.moods && u.moods.length){
    const last = u.moods[u.moods.length-1];
    html += `<div style="margin-top:6px">${last.time} — ${last.emoji} ${last.label}</div><div class="muted" style="margin-top:6px">${last.note || '-'}</div>`;
  } else html += `<div class="muted" style="margin-top:6px">ยังไม่มีบันทึก</div>`;
  box.innerHTML = html;
}

/* ---------- Mood UI & Save ---------- */
/* We'll render separate mood button groups for student and teacher */
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
      // only select within this container
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
  if(note){
    u.diaries = u.diaries || [];
    u.diaries.push({time:entry.time, text:note});
  }
  saveState();
  logActivity(`${currentUser} (นักเรียน) บันทึกอารมณ์: ${meta.emoji} ${meta.label}`);
  document.getElementById('studentDiaryText').value = '';
  Array.from(document.querySelectorAll('#studentMoodButtons .emoji-btn')).forEach(b=>b.classList.remove('selected'));
  renderAll();
});

document.getElementById('saveTeacherMoodBtn').addEventListener('click', ()=>{
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
  if(note){
    u.diaries = u.diaries || [];
    u.diaries.push({time:entry.time, text:note});
  }
  saveState();
  logActivity(`${currentUser} (ครู) บันทึกอารมณ์: ${meta.emoji} ${meta.label}`);
  document.getElementById('teacherDiaryText').value = '';
  Array.from(document.querySelectorAll('#teacherMoodButtons .emoji-btn')).forEach(b=>b.classList.remove('selected'));
  renderAll();
});

/* ---------- Diary history render for student and teacher ---------- */
function renderDiaryHistoryForUser(user, containerId){
  const el = document.getElementById(containerId);
  if(!el) return;
  if(!user.diaries || !user.diaries.length){
    el.innerHTML = '<div class="meta">ยังไม่มีบันทึก My diary</div>';
    return;
  }
  el.innerHTML = user.diaries.slice().reverse().map(d=>`<div class="diary-item"><div class="meta">${d.time}</div><div style="margin-top:6px">${escapeHtml(d.text)}</div></div>`).join('');
}

/* ---------- Redeem / Appointments / Quiz (unchanged logic) ---------- */
document.getElementById('openRedeem').addEventListener('click', ()=>{
  const p = document.getElementById('redeemPanel');
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

/* appointments: populate teacher select */
function populateTeachersForAppt(){
  const sel = document.getElementById('apptTeacherSelect');
  if(!sel) return;
  sel.innerHTML = '<option value="">-- เลือกครู --</option>';
  Object.values(state.users).filter(u=>u.role==='teacher').forEach(t=>{
    const opt = document.createElement('option'); opt.value = t.name; opt.innerText = t.display || t.name; sel.appendChild(opt);
  });
}

/* send appt */
document.getElementById('requestAppt').addEventListener('click', ()=> {
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const t = document.getElementById('apptTeacherSelect').value;
  const msg = document.getElementById('apptMsg').value.trim();
  if(!t || !msg) return alert('กรุณาเลือกครูและกรอกข้อความ');
  const appt = { id: generateId(), teacher: t, student: currentUser, msg, status:'pending', time: new Date().toLocaleString(), teacherNote:'', iso:new Date().toISOString() };
  const u = state.users[currentUser]; u.appts = u.appts || []; u.appts.push(appt);
  if(!state.users[t]) state.users[t] = { name:t, display:t, role:'teacher', inbox:[], moods:[], diaries:[], reports:[], stars:0, avatar:'' };
  state.users[t].inbox = state.users[t].inbox || []; state.users[t].inbox.push(appt);
  saveState(); logActivity(`${currentUser} ขอเข้าปรึกษากับ ${t}`); renderAll();
  document.getElementById('apptMsg').value = '';
});

/* quiz selection + rendering (unchanged) */
const quizzes = {
  basic: [
    {q:'ในสัปดาห์ที่ผ่านมาคุณรู้สึกมีความสุขบ่อยแค่ไหน?', options:['ไม่เลย','บางครั้ง','บ่อย','ตลอดเวลา'], scores:[0,1,2,3]},
    {q:'คุณนอนหลับเพียงพอหรือไม่?', options:['ไม่เลย','บางครั้ง','พอประมาณ','เพียงพอ'], scores:[0,1,2,3]},
    {q:'คุณรู้สึกว่ามีคนคอยรับฟังเมื่อคุณต้องการหรือไม่?', options:['ไม่เลย','บางครั้ง','บ่อย','ตลอดเวลา'], scores:[0,1,2,3]}
  ],
  sleep: [
    {q:'โดยปกติคุณนอนกี่ชั่วโมงต่อคืน?', options:['น้อยกว่า 5 ชม','5-6 ชม','7-8 ชม','มากกว่า 8 ชม'], scores:[0,1,2,3]},
    {q:'คุณรู้สึกง่วงระหว่างเรียนบ่อยแค่ไหน?', options:['บ่อยมาก','บางครั้ง','นานๆ ครั้ง','ไม่เคย'], scores:[0,1,2,3]}
  ]
};

let currentQuizKey = '';

document.getElementById('startQuiz').addEventListener('click', ()=>{
  const sel = document.getElementById('quizSelect').value;
  if(!sel) return alert('กรุณาเลือกแบบทดสอบก่อนเริ่ม');
  currentQuizKey = sel;
  renderQuizQuestions(sel);
  document.getElementById('startQuiz').style.display='none';
  document.getElementById('submitQuiz').style.display='inline-block';
});
document.getElementById('submitQuiz').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  if(!currentQuizKey) return alert('ไม่มีแบบทดสอบ');
  const quiz = quizzes[currentQuizKey];
  let total = 0; let answered = true;
  quiz.forEach((qq,i)=>{
    const v = document.querySelector(`input[name="q${i}"]:checked`);
    if(!v) answered=false; else total += qq.scores[parseInt(v.value)];
  });
  if(!answered) return alert('กรุณาตอบทุกข้อ');
  state.users[currentUser].quiz = state.users[currentUser].quiz || [];
  state.users[currentUser].quiz.push({time:new Date().toLocaleString(), score:total, type:currentQuizKey});
  saveState(); logActivity(`${currentUser} ทำแบบทดสอบ ${currentQuizKey} (คะแนน ${total})`);
  document.getElementById('quizContainer').innerHTML = `<div class="meta">ผลคะแนน: <strong>${total}</strong></div>`;
  document.getElementById('submitQuiz').style.display='none'; document.getElementById('startQuiz').style.display='inline-block';
  currentQuizKey = '';
  document.getElementById('quizSelect').value = '';
});

function renderQuizQuestions(key){
  const quiz = quizzes[key];
  const qEl = document.getElementById('quizContainer');
  qEl.innerHTML = '';
  quiz.forEach((qq,i)=>{
    const div = document.createElement('div'); div.style.padding='8px';
    div.innerHTML = `<div><strong>Q${i+1}.</strong> ${qq.q}</div>`;
    qq.options.forEach((opt,j)=>{
      const id = `q${i}_o${j}`;
      div.innerHTML += `<div style="margin-left:8px"><input type="radio" name="q${i}" id="${id}" value="${j}"> <label for="${id}">${opt}</label></div>`;
    });
    qEl.appendChild(div);
  });
}

/* ---------- Teacher area: students list with avatars & star icon ---------- */
function renderStudentsList(){
  const q = document.getElementById('searchStudent').value.trim().toLowerCase();
  const container = document.getElementById('studentsList');
  const students = Object.values(state.users).filter(u=>u.role==='student' && (!q || (u.display||u.name).toLowerCase().includes(q)));
  if(!students.length){ container.innerHTML = '<div class="meta">ไม่มีนักเรียน</div>'; return; }
  container.innerHTML = students.map(s=>{
    const avatarHtml = s.avatar ? `<div class="student-avatar"><img src="${s.avatar}" alt=""></div>` : `<div class="student-avatar">${(s.display||s.name).slice(0,2).toUpperCase()}</div>`;
    return `<div style="padding:8px;border-bottom:1px solid #f3f6fb;display:flex;align-items:center;gap:10px">
      ${avatarHtml}
      <div><strong>${s.display||s.name}</strong><div class="meta"><span class="star-ico">⭐</span> ${s.stars||0}</div></div>
      <div style="margin-left:auto"><button class="addStar" data-name="${s.name}">ให้ +1</button><button class="removeStar" data-name="${s.name}">-1</button></div>
    </div>`;
  }).join('');
  document.querySelectorAll('.addStar').forEach(b=>b.addEventListener('click', e=>modifyStars(e.target.dataset.name,1)));
  document.querySelectorAll('.removeStar').forEach(b=>b.addEventListener('click', e=>modifyStars(e.target.dataset.name,-1)));
}
document.getElementById('searchStudent').addEventListener('input', renderStudentsList);
function modifyStars(name,delta){
  const s = state.users[name];
  if(!s) return alert('ไม่พบ');
  s.stars = Math.max(0,(s.stars||0)+delta);
  saveState(); logActivity(`${currentUser} ปรับดาวให้ ${name}: ${delta>0?'+':''}${delta}`); renderAll();
}

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

/* ---------- CHARTS (unchanged) ---------- */
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
  const aggregated = aggregateByPeriod(u.moods || [], period);
  const labels = aggregated.labels;
  const datasets = emojiChoices.map(e=>({ label:e.label, data: aggregated.data.map(d=>d[e.label]||0), backgroundColor: hexToRgba(e.color,0.95), stack:'s1' }));
  if(periodChart) periodChart.destroy();
  periodChart = new Chart(ctxPeriod, { type:'bar', data:{ labels, datasets }, options:{ responsive:true, plugins:{legend:{position:'bottom'}}, scales:{ x:{stacked:true}, y:{stacked:true, beginAtZero:true, ticks:{precision:0}} } } });
}

function aggregateByPeriod(moods, period){
  const now = new Date();
  if(period === 'week'){
    const days = [];
    for(let i=6;i>=0;i--){ const d = new Date(); d.setDate(now.getDate()-i); days.push(dateKey(d)); }
    const data = days.map(_=> ({}));
    moods.forEach(entry=>{
      if(!entry.iso) return;
      const d = new Date(entry.iso);
      const key = dateKey(d);
      const idx = days.indexOf(key);
      if(idx>=0) data[idx][entry.label] = (data[idx][entry.label]||0)+1;
    });
    return { labels: days.map(d=>formatDayLabel(d)), data };
  } else if(period === 'month'){
    const weeks = []; const weekRanges = [];
    for(let w=0; w<4; w++){
      const start = new Date(); start.setDate(now.getDate() - 30 + w*7);
      const end = new Date(); end.setDate(start.getDate() + 6);
      weekRanges.push({start, end});
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
  } else {
    const sem = getCurrentSemester(now);
    const months = []; const data=[];
    let m = new Date(sem.start);
    while(m <= now){
      months.push(formatMonthLabel(m)); data.push({}); m.setMonth(m.getMonth()+1);
    }
    moods.forEach(entry=>{
      if(!entry.iso) return;
      const d = new Date(entry.iso);
      if(d >= sem.start && d <= now){
        const idx = (d.getFullYear()*12 + d.getMonth()) - (sem.start.getFullYear()*12 + sem.start.getMonth());
        if(idx>=0 && idx<data.length) data[idx][entry.label] = (data[idx][entry.label]||0)+1;
      }
    });
    return { labels: months, data };
  }
}

/* teacher overall chart */
function renderTeacherChart(){
  const ctx = document.getElementById('moodChartAll');
  if(!ctx) return;
  const moodCounts = {}; emojiChoices.forEach(e=>moodCounts[e.label]=0);
  Object.values(state.users).filter(u=>u.role==='student').forEach(s=>{
    if(s.moods && s.moods.length){
      const last = s.moods[s.moods.length-1];
      moodCounts[last.label] = (moodCounts[last.label]||0)+1;
    }
  });
  const labels = Object.keys(moodCounts);
  const data = Object.values(moodCounts);
  if(allChart) allChart.destroy();
  allChart = new Chart(ctx, { type:'doughnut', data:{ labels, datasets:[{ data, backgroundColor: emojiChoices.map(e=>e.color) }] }, options:{responsive:true, plugins:{legend:{position:'bottom'}}} });
}

/* date helpers */
function dateKey(d){ const dt = new Date(d.getFullYear(), d.getMonth(), d.getDate()); return `${dt.getDate().toString().padStart(2,'0')} ${dt.toLocaleString('th-TH',{month:'short'})}`; }
function formatDayLabel(label){ return label; }
function stripTime(d){ return new Date(d.getFullYear(), d.getMonth(), d.getDate()); }
function endOfDay(d){ return new Date(d.getFullYear(), d.getMonth(), d.getDate(),23,59,59,999); }
function formatMonthLabel(d){ return d.toLocaleString('th-TH',{month:'short', year:'numeric'}); }
function getCurrentSemester(now){ const y = now.getFullYear(); if(now.getMonth() <= 5) return { start: new Date(y,0,1), end: new Date(y,5,30) }; else return { start: new Date(y,6,1), end: new Date(y,11,31) }; }
function hexToRgba(hex, a){ if(hex.startsWith('#')) hex = hex.slice(1); const bigint = parseInt(hex,16); const r = (bigint >> 16) & 255; const g = (bigint >> 8) & 255; const b = bigint & 255; return `rgba(${r},${g},${b},${a})`; }

/* redeem history render */
function renderRedeemHistory(u){
  const el = document.getElementById('redeemHistory');
  if(!el) return;
  if(!u.redeemHistory || !u.redeemHistory.length){ el.innerHTML = '<div class="meta">ยังไม่มีการแลก</div>'; return; }
  el.innerHTML = u.redeemHistory.slice().reverse().map(r=>`<div class="diary-item"><div><strong>${r.item}</strong> <div class="meta">(${r.cost} ⭐)</div></div><div class="meta">${r.time}</div></div>`).join('');
}

/* ---------- render panels and initial rendering ---------- */
function renderPanels(){
  if(!currentUser) return;
  const u = state.users[currentUser];
  document.getElementById('studentPanel').style.display = u.role === 'student' ? 'block' : 'none';
  document.getElementById('teacherPanel').style.display = u.role === 'teacher' ? 'block' : 'none';
  document.getElementById('adminPanel').style.display = u.role === 'admin' ? 'block' : 'none';

  // student-specific render
  if(u.role === 'student'){
    document.getElementById('myStars').innerText = u.stars || 0;
    renderApptHistory(u);
    renderRedeemHistory(u);
    renderDiaryHistoryForUser(u, 'studentDiaryHistory');
    document.getElementById('lastStudentMoodText').innerText = u.moods && u.moods.length ? `${u.moods[u.moods.length-1].emoji} ${u.moods[u.moods.length-1].label} — ${u.moods[u.moods.length-1].time}` : '-';
  } else {
    // teacher-specific render
    renderApptRequests();
    renderReportsList();
    buildReportStudentSelect();
    renderStudentsList();
    // teacher diary history
    renderDiaryHistoryForUser(u, 'teacherDiaryHistory');
    document.getElementById('lastTeacherMoodText').innerText = u.moods && u.moods.length ? `${u.moods[u.moods.length-1].emoji} ${u.moods[u.moods.length-1].label} — ${u.moods[u.moods.length-1].time}` : '-';
  }

  renderQuickPanel();
  populateTeachersForAppt();
}

/* appt history for student */
function renderApptHistory(u){
  const el = document.getElementById('apptHistory');
  if(!u.appts || !u.appts.length){ el.innerHTML = '<div class="meta">ยังไม่มีการขอนัด</div>'; return; }
  el.innerHTML = u.appts.slice().reverse().map(a=>`<div class="diary-item"><div class="meta">${a.time} → ถึง: ${a.teacher} [${a.status}]</div><div>${a.msg}</div><div class="meta">หมายเหตุครู: ${a.teacherNote || '-'}</div></div>`).join('');
}

/* activity and initial render */
renderActivity();
renderAll();
setInterval(()=>{ renderTeacherChart(); },5000);

/* helper: escapeHtml to prevent basic injection in diary display */
function escapeHtml(unsafe){
  return unsafe ? unsafe.replace(/[&<"'>]/g, function(m){ return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'}[m]; }) : '';
}

</script>
</body>
</html>
