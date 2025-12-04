<html lang="th">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>LiteVibe — ระบบติดตามอารมณ์และพฤติกรรม</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
  /* Purple - pink modern theme; important text areas use white background */
  :root{
    --bg-start: #faf5ff;
    --bg-end:   #fff0f6;
    --card: #ffffff;
    --primary: #7c3aed;
    --accent:  #fb7185;
    --muted: #6b7280;
    --danger:#ef4444; --warning:#f59e0b; --success:#16a34a; --info:#06b6d4;
  }
  *{box-sizing:border-box}
  body{font-family:"Kanit",sans-serif;background: linear-gradient(135deg,var(--bg-start) 0%, rgba(255,255,255,0.6) 40%, var(--bg-end) 100%);margin:0;color:#0b1220}
  .app{max-width:1100px;margin:22px auto;padding:18px}
  header.app-header{display:flex;align-items:center;gap:12px;margin-bottom:18px}
  .logo{display:flex;align-items:center;gap:12px}
  .mark{width:54px;height:54px;border-radius:12px;background:linear-gradient(135deg,var(--primary),var(--accent));display:flex;align-items:center;justify-content:center;color:#fff;font-size:22px;box-shadow:0 12px 30px rgba(124,58,237,0.12)}
  h1{font-size:20px;margin:0;color:var(--primary)}
  .muted{color:var(--muted);font-size:13px}
  .card{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 8px 22px rgba(12,10,20,0.04);margin-bottom:14px}
  .grid{display:grid;grid-template-columns:1fr 360px;gap:14px}
  label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
  input[type=text],select,textarea{width:100%;padding:10px;border-radius:10px;border:1px solid rgba(124,58,237,0.08);background:linear-gradient(#fff,#fff);font-size:14px}
  button{background:var(--primary);color:#fff;border:0;padding:10px 12px;border-radius:10px;cursor:pointer;font-weight:600}
  .btn-ghost{background:transparent;border:1px solid rgba(124,58,237,0.12);color:var(--primary);padding:9px 12px;border-radius:10px}
  .emoji-row{display:flex;gap:10px;flex-wrap:wrap}
  .emoji-btn{width:80px;height:80px;border-radius:16px;border:0;background:#fff;display:flex;flex-direction:column;align-items:center;justify-content:center;font-size:30px;cursor:pointer;transition:all .18s;box-shadow:0 8px 20px rgba(16,24,45,0.04)}
  .emoji-btn .label{font-size:12px;margin-top:6px;color:var(--muted)}
  .emoji-btn:hover{transform:translateY(-6px)}
  .emoji-btn.selected{outline:4px solid rgba(124,58,237,0.12);box-shadow:0 18px 36px rgba(219,39,119,0.06)}
  .periods{display:flex;gap:8px;margin-top:8px}
  .periods button{background:transparent;color:var(--muted);border:1px solid rgba(124,58,237,0.06);padding:8px 10px;border-radius:10px}
  .periods button.active{background:linear-gradient(90deg,var(--primary),var(--accent));color:#fff;border:0}
  .chart-wrap{margin-top:12px}
  .badge{background:var(--card);color:var(--primary);padding:6px 10px;border-radius:999px;border:1px solid rgba(124,58,237,0.08);font-weight:700}
  .student-avatar{width:48px;height:48px;border-radius:10px;background:linear-gradient(135deg,#fff,#faf5ff);display:inline-flex;align-items:center;justify-content:center;font-weight:700;color:var(--primary);border:1px solid rgba(0,0,0,0.04);overflow:hidden}
  .list{max-height:380px;overflow:auto}
  .meta{font-size:12px;color:var(--muted)}
  .diary-item{padding:8px;border-bottom:1px solid #f3f6fb;display:flex;gap:10px;align-items:flex-start}
  .diary-emoji{font-size:28px;line-height:1;min-width:42px;text-align:center}
  .diary-content{flex:1}
  .small{font-size:13px}
  .segmented{display:flex;gap:8px}
  .segmented button{padding:8px 10px;border-radius:8px;border:1px solid rgba(124,58,237,0.06);background:#fff}
  .risk-item{display:flex;align-items:center;gap:10px;padding:8px;border-bottom:1px solid #f3f6fb}
  .risk-badge{padding:6px 8px;border-radius:8px;color:#fff}
  .risk-high{background:var(--danger)}
  .risk-medium{background:var(--warning)}
  .redeem-item{display:flex;align-items:center;gap:12px;padding:10px;border-radius:10px;border:1px solid rgba(124,58,237,0.04);background:#fff}
  .flagged { color: var(--danger); font-weight:700; padding:4px 8px; border-radius:8px; background:#fff0f2; display:inline-block; }
  .card strong, .card h4, .badge { background: var(--card); padding: 6px 8px; border-radius: 8px; display: inline-block; }
  @media(max-width:980px){.grid{grid-template-columns:1fr} .teacher-mood-row{flex-direction:column} }
  .pastel-legend{display:flex;flex-wrap:wrap;gap:8px;margin-top:8px}
  .legend-item{display:flex;align-items:center;gap:8px}
  .legend-swatch{width:12px;height:12px;border-radius:3px}
</style>
</head>
<body>
<div class="app">
  <header class="app-header">
    <div class="logo">
      <div class="mark">LV</div>
      <div>
        <h1><span style="background:var(--card);padding:4px 8px;border-radius:8px">LiteVibe</span></h1>
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

            <div style="margin-top:12px" class="card">
              <div style="display:flex;align-items:center;justify-content:space-between">
                <div><strong>สถิติอารมณ์ประจำวัน</strong><div class="muted">แสดงสัดส่วนอารมณ์ของวันนี้</div></div>
                <div class="muted small">สีพาสเทล น่ารัก</div>
              </div>
              <div style="display:flex;gap:12px;align-items:center;margin-top:12px;flex-wrap:wrap">
                <div style="width:220px;height:220px">
                  <canvas id="studentDailyMoodPie"></canvas>
                </div>
                <div style="flex:1;min-width:200px">
                  <div id="studentDailyLegend" class="pastel-legend"></div>
                  <div class="muted small" style="margin-top:8px">กราฟจะแสดงข้อมูลจากบันทึกของวันนี้</div>
                </div>
              </div>
            </div>

            <!-- Today's mood display -->
            <div id="todayMoodDisplay" class="card" style="margin-top:12px;display:none">
              <div style="display:flex;align-items:center;gap:12px">
                <div id="todayMoodEmoji" style="font-size:36px"></div>
                <div style="flex:1">
                  <div id="todayMoodLabel" style="font-weight:700"></div>
                  <div id="todayMoodTime" class="meta" style="margin-top:6px"></div>
                  <div id="todayMoodNote" style="margin-top:8px"></div>
                </div>
              </div>
            </div>

            <div class="card" style="margin-top:12px">
              <strong>ขอนัดคำปรึกษา (เลือกครู)</strong>
              <div style="margin-top:8px">
                <label>เลือกครูที่ต้องการนัด (จำลอง)</label>
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
              <div class="muted">ดูสถิติ อนุมัติคำขอแลกดาว และจัดการดาวเด็กดี / ส่งรายงานให้ครูที่ปรึกษา</div>
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

          <!-- Teacher mood pies for teacher and selected student -->
          <div class="card" style="margin-top:12px">
            <h4>สถิติอารมณ์ประจำวัน (ครู และ นักเรียนที่เลือก)</h4>
            <div style="display:flex;gap:12px;align-items:flex-start" class="teacher-mood-row">
              <div style="width:260px">
                <strong>ครู (ของฉัน)</strong>
                <div style="width:220px;height:220px;margin-top:10px"><canvas id="teacherSelfMoodPie"></canvas></div>
                <div id="teacherSelfLegend" class="pastel-legend"></div>
              </div>
              <div style="width:260px">
                <strong>นักเรียน (ที่เลือก)</strong>
                <div style="width:220px;height:220px;margin-top:10px"><canvas id="teacherStudentMoodPie"></canvas></div>
                <div id="teacherStudentLegend" class="pastel-legend"></div>
              </div>
            </div>
            <div class="muted small" style="margin-top:8px">เลือกนักเรียนจากเมนูด้านบนเพื่ออัปเดตกราฟ</div>
          </div>

          <!-- Student list with add/remove star controls -->
          <div class="card" style="margin-top:12px">
            <div style="display:flex;align-items:center;justify-content:space-between">
              <div><strong>รายชื่อนักเรียน</strong></div>
              <div><input id="searchStudent" placeholder="ค้นหาชื่อนักเรียน" style="padding:8px;border-radius:8px;border:1px solid rgba(124,58,237,0.06)" /></div>
            </div>
            <div id="studentsList" class="list" style="margin-top:10px"></div>
          </div>

          <!-- Report to advisor card with star adjust -->
          <div class="card" style="margin-top:12px">
            <div style="display:flex;align-items:center;justify-content:space-between">
              <div><strong>ส่งรายงานพฤติกรรมนักเรียนให้ครูที่ปรึกษา</strong><div class="meta">สร้างรายงานและส่งตรงถึงครูที่ปรึกษา/หัวหน้าห้อง (พร้อมเพิ่ม/ลดดาว)</div></div>
            </div>
            <div style="margin-top:10px">
              <label>เลือกนักเรียน (จำลอง)</label>
              <select id="reportStudentSelect"></select>
              <label style="margin-top:8px">เลือกครูที่ปรึกษา (ผู้รับ)</label>
              <select id="reportAdvisorSelect"></select>
              <label style="margin-top:8px">เนื้อหารายงาน</label>
              <textarea id="reportTextToAdvisor" rows="3" placeholder="รายละเอียดพฤติกรรม/เหตุผล ที่ต้องการแจ้งครูที่ปรึกษา"></textarea>

              <div style="display:flex;gap:8px;margin-top:10px;align-items:center">
                <button id="sendReportToAdvisor">ส่งรายงาน</button>
                <button id="clearReportFields" class="btn-ghost">ล้างฟิลด์</button>
                <div style="margin-left:8px">
                  <label class="meta">ปรับดาวนักเรียน</label>
                  <div style="display:flex;gap:8px;margin-top:6px">
                    <button id="incStarBtn" class="btn-ghost">เพิ่มดาว +1</button>
                    <button id="decStarBtn" class="btn-ghost">ลดดาว −1</button>
                    <div id="starAdjustInfo" class="muted small" style="margin-left:6px"></div>
                  </div>
                </div>
              </div>

              <div id="reportSendResult" style="margin-top:8px" class="muted small"></div>
            </div>
          </div>

          <!-- Advisor inbox -->
          <div class="card" style="margin-top:12px">
            <h4>กล่องรายงานที่ได้รับ (สำหรับครูที่ปรึกษา)</h4>
            <div id="advisorInbox" class="list"></div>
          </div>

          <!-- Behavior risk chart for teacher (class-level or overall) -->
          <div class="card" style="margin-top:12px">
            <h4>สถิติเสี่ยงพฤติกรรม (Teacher view)</h4>
            <div class="chart-wrap"><canvas id="teacherBehaviorRiskChart" height="140"></canvas></div>
            <div class="muted small" style="margin-top:8px">แสดงภาพรวมความเสี่ยงจากรายงานพฤติกรรม และสัดส่วนอารมณ์</div>
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
              <div class="muted">สถิติภาพรวมโรงเรียน</div>
            </div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>สรุปอารมณ์นักเรียน (ภาพรวม)</h4>
            <div class="chart-wrap"><canvas id="adminMoodChart" height="140"></canvas></div>
          </div>

          <div class="card" style="margin-top:12px">
            <h4>สถิติเสี่ยงพฤติกรรม (ภาพรวม)</h4>
            <div class="chart-wrap"><canvas id="adminBehaviorRiskChart" height="140"></canvas></div>
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
/* v6 — Adds:
 1) Student dashboard: daily mood pie chart (pastel colors)
 2) Teacher dashboard: daily mood pie charts for teacher (self) and selected student
 3) Student appointment can choose predefined simulated teachers
 4) Teacher report UI allows choosing simulated students and adjust stars (+/-) while saving report
*/

/* ---------- config ---------- */
const STORAGE_KEY = 'litevibe_data_v6';
const emojiChoices = [
  {key:'very_happy', emoji:'😄', label:'มีความสุขมาก', color:'#FFD166'},
  {key:'happy', emoji:'🙂', label:'มีความสุข', color:'#7BE495'},
  {key:'neutral', emoji:'😐', label:'เฉย ๆ', color:'#A3A3FF'},
  {key:'sad', emoji:'😢', label:'เศร้า', color:'#90A3FF'},
  {key:'angry', emoji:'😠', label:'โกรธ', color:'#FF9AA2'},
  {key:'tired', emoji:'😴', label:'เหนื่อย', color:'#C6C6C6'}
];

let state = loadState();
let currentUser = null;
let studentDailyPie = null;
let teacherSelfPie = null;
let teacherStudentPie = null;
let adminMoodChart = null;
let adminBehaviorChart = null;

/* ---------- storage & seed ---------- */
function defaultState(){ return { users: {}, activity: [], redeemRequests: [] }; }
function seedSampleData(s){
  s.users['khonmek'] = { name:'khonmek', display:'ก้อนเมฆ', role:'student', classId:'M1A', grade:'ม.1', stars:5, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  s.users['thongfa'] = { name:'thongfa', display:'ท้องฟ้า', role:'student', classId:'M1A', grade:'ม.1', stars:8, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  s.users['medfon'] = { name:'medfon', display:'เม็ดฝน', role:'student', classId:'M2B', grade:'ม.2', stars:2, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };

  s.users['ajarn_somchai'] = { name:'ajarn_somchai', display:'ครูสมชาย', role:'teacher', avatar:'', moods:[], diaries:[], inbox:[], inboxReports:[], reports:[] };
  s.users['ajarn_somsri'] = { name:'ajarn_somsri', display:'ครูสมศรี', role:'teacher', avatar:'', moods:[], diaries:[], inbox:[], inboxReports:[], reports:[] };

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
    Object.values(parsed.users||{}).forEach(u=>{
      if(!u.reports) u.reports = [];
      if(u.role === 'teacher' && !u.inboxReports) u.inboxReports = [];
      if(!u.redeemHistory) u.redeemHistory = [];
      if(!u.diaries) u.diaries = [];
      if(!u.moods) u.moods = [];
    });
    if(!parsed.redeemRequests) parsed.redeemRequests = [];
    return parsed;
  }catch(e){ const s = defaultState(); seedSampleData(s); return s; }
}
function saveState(){ localStorage.setItem(STORAGE_KEY, JSON.stringify(state)); }

/* ---------- utilities ---------- */
function generateId(){ return 'id_' + Math.random().toString(36).slice(2,9); }
function logActivity(txt){ const time = new Date().toLocaleString(); state.activity.unshift({txt,time}); saveState(); renderActivity(); }
function hexToRgba(hex,a){ if(!hex) return `rgba(124,58,237,${a})`; if(hex.startsWith('#')) hex=hex.slice(1); const bigint=parseInt(hex,16); const r=(bigint>>16)&255; const g=(bigint>>8)&255; const b=bigint&255; return `rgba(${r},${g},${b},${a})`; }
function isSameDayIso(isoA, isoB){ try{ const a=new Date(isoA), b=new Date(isoB); return a.getFullYear()===b.getFullYear() && a.getMonth()===b.getMonth() && a.getDate()===b.getDate(); }catch(e){return false;} }
function escapeHtml(u){ return u ? u.replace(/[&<"'>]/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#039;'}[m])) : ''; }

/* ---------- AUTH UI ---------- */
const userSelect = document.getElementById('userSelect');
const selectedRole = document.getElementById('selectedRole');
function populateUserSelect(){
  userSelect.innerHTML = '';
  const users = Object.values(state.users);
  users.forEach(u => {
    const opt = document.createElement('option'); opt.value = u.name; opt.textContent = `${u.display || u.name} — ${u.role}`; userSelect.appendChild(opt);
  });
  const optNew = document.createElement('option'); optNew.value='__create__'; optNew.textContent='>> สร้างบัญชีใหม่ <<'; userSelect.appendChild(optNew);
  updateSelectedRole();
}
userSelect.addEventListener('change', updateSelectedRole);
function updateSelectedRole(){ const v = userSelect.value; if(v && v!=='__create__' && state.users[v]) selectedRole.innerText = state.users[v].role; else selectedRole.innerText = '-'; }
document.getElementById('showCreate').addEventListener('click', ()=> document.getElementById('createRow').style.display = document.getElementById('createRow').style.display==='none' ? 'block' : 'none');
document.getElementById('createBtn').addEventListener('click', ()=>{
  const name = document.getElementById('newName').value.trim();
  const role = document.getElementById('newRole').value;
  if(!name) return alert('กรุณากรอกชื่อผู้ใช้');
  if(state.users[name]) return alert('ชื่อผู้ใช้นี้มีอยู่แล้ว');
  const u = { name, display:name, role, avatar:'', moods:[], diaries:[], appts:[], redeemHistory:[], quiz:[], reports:[] };
  if(role==='student'){ u.classId='M1A'; u.grade='ม.1'; u.stars=0; }
  if(role==='teacher'){ u.inbox=[]; u.inboxReports=[]; u.reports=[]; }
  state.users[name]=u; saveState(); populateUserSelect(); alert('สร้างบัญชีเรียบร้อยแล้ว: '+name);
  document.getElementById('newName').value=''; document.getElementById('createRow').style.display='none';
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

/* ---------- Profile & Avatar ---------- */
const avatarInput = document.getElementById('avatarInput');
avatarInput.addEventListener('change', (e)=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบก่อนอัปโหลดรูป');
  const f = e.target.files[0]; if(!f) return;
  const r = new FileReader(); r.onload = ev => { state.users[currentUser].avatar = ev.target.result; saveState(); logActivity(`${currentUser} อัปโหลดรูป`); renderAll(); }; r.readAsDataURL(f);
});
document.getElementById('removeAvatar').addEventListener('click', ()=>{ if(!currentUser) return alert('กรุณาเข้าสู่ระบบ'); if(!confirm('ต้องการลบรูปประจำตัวหรือไม่?')) return; state.users[currentUser].avatar=''; saveState(); renderAll(); });

/* ---------- Mood buttons & save ---------- */
function renderMoodButtons(containerId){
  const container = document.getElementById(containerId);
  if(!container) return;
  container.innerHTML = '';
  emojiChoices.forEach(e=>{
    const btn = document.createElement('button'); btn.className='emoji-btn'; btn.dataset.key=e.key; btn.innerHTML=`<div style="font-size:32px">${e.emoji}</div><div class="label">${e.label}</div>`;
    btn.style.background = `linear-gradient(180deg, rgba(255,255,255,1), ${hexToRgba(e.color,0.06)})`;
    btn.addEventListener('click', ()=> { Array.from(container.querySelectorAll('.emoji-btn')).forEach(b=>b.classList.remove('selected')); btn.classList.add('selected'); });
    container.appendChild(btn);
  });
}
renderMoodButtons('studentMoodButtons');
renderMoodButtons('teacherMoodButtons');

document.getElementById('saveStudentMoodBtn').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const sel = document.querySelector('#studentMoodButtons .emoji-btn.selected'); if(!sel) return alert('กรุณาเลือกรูปอารมณ์');
  const key = sel.dataset.key; const meta = emojiChoices.find(x=>x.key===key);
  const note = document.getElementById('studentDiaryText').value.trim();
  const now = new Date(); const entry = { iso: now.toISOString(), time: now.toLocaleString(), key, emoji: meta.emoji, label: meta.label, note };
  const u = state.users[currentUser]; u.moods = u.moods||[]; u.moods.push(entry);
  u.diaries = u.diaries||[]; u.diaries.push({ time: entry.time, text: note, emoji: meta.emoji, label: meta.label, iso: entry.iso });
  saveState(); logActivity(`${currentUser} บันทึกอารมณ์: ${meta.emoji} ${meta.label}`); document.getElementById('studentDiaryText').value=''; Array.from(document.querySelectorAll('#studentMoodButtons .emoji-btn')).forEach(b=>b.classList.remove('selected')); renderAll();
});

/* ---------- Helper: get mood counts for a date ---------- */
function getMoodCountsForDate(user, dateIso){
  const result = {};
  emojiChoices.forEach(e=> result[e.label]=0);
  if(!user || !user.moods) return result;
  const d = new Date(dateIso);
  user.moods.forEach(m=>{
    try{
      if(isSameDayIso(m.iso, dateIso)){
        result[m.label] = (result[m.label]||0) + 1;
      }
    }catch(e){}
  });
  return result;
}

/* ---------- Pies rendering ---------- */
function renderStudentDailyPie(){
  const canvas = document.getElementById('studentDailyMoodPie');
  if(!canvas || !currentUser) return;
  const user = state.users[currentUser];
  const counts = getMoodCountsForDate(user, new Date().toISOString());
  const labels = []; const data=[]; const bg=[];
  emojiChoices.forEach(e=>{ const cnt = counts[e.label]||0; labels.push(e.label); data.push(cnt); bg.push(e.color); });
  if(studentDailyPie) studentDailyPie.destroy();
  studentDailyPie = new Chart(canvas.getContext('2d'), { type:'pie', data:{ labels, datasets:[{ data, backgroundColor:bg }] }, options:{ responsive:true, plugins:{legend:{position:'bottom'}} } });
  // build legend
  const legend = document.getElementById('studentDailyLegend'); legend.innerHTML = '';
  labels.forEach((l,i)=> legend.innerHTML += `<div class="legend-item"><div class="legend-swatch" style="background:${bg[i]}"></div><div>${l}: ${data[i]}</div></div>`);
}

function renderTeacherMoodPies(selectedStudentName){
  // teacher self
  const selfCanvas = document.getElementById('teacherSelfMoodPie');
  const studentCanvas = document.getElementById('teacherStudentMoodPie');
  if(currentUser && state.users[currentUser]){
    const selfCounts = getMoodCountsForDate(state.users[currentUser], new Date().toISOString());
    const labels=[]; const data=[]; const bg=[];
    emojiChoices.forEach(e=>{ labels.push(e.label); data.push(selfCounts[e.label]||0); bg.push(e.color); });
    if(teacherSelfPie) teacherSelfPie.destroy();
    teacherSelfPie = new Chart(selfCanvas.getContext('2d'), { type:'pie', data:{ labels, datasets:[{ data, backgroundColor:bg }] }, options:{ responsive:true, plugins:{legend:{display:false}} } });
    const legend = document.getElementById('teacherSelfLegend'); legend.innerHTML=''; labels.forEach((l,i)=> legend.innerHTML += `<div class="legend-item"><div class="legend-swatch" style="background:${bg[i]}"></div><div>${l}: ${data[i]}</div></div>`);
  }
  // selected student
  const s = state.users[selectedStudentName];
  const studentCounts = getMoodCountsForDate(s || {}, new Date().toISOString());
  const labels2=[]; const data2=[]; const bg2=[];
  emojiChoices.forEach(e=>{ labels2.push(e.label); data2.push(studentCounts[e.label]||0); bg2.push(e.color); });
  if(teacherStudentPie) teacherStudentPie.destroy();
  teacherStudentPie = new Chart(studentCanvas.getContext('2d'), { type:'pie', data:{ labels: labels2, datasets:[{ data:data2, backgroundColor:bg2 }] }, options:{ responsive:true, plugins:{legend:{display:false}} } });
  const legend2 = document.getElementById('teacherStudentLegend'); legend2.innerHTML=''; labels2.forEach((l,i)=> legend2.innerHTML += `<div class="legend-item"><div class="legend-swatch" style="background:${bg2[i]}"></div><div>${l}: ${data2[i]}</div></div>`);
}

/* ---------- populate teachers for appointment (student) ---------- */
function populateTeachersForAppt(){
  const sel = document.getElementById('apptTeacherSelect');
  if(!sel) return;
  sel.innerHTML = '<option value="">-- เลือกครู --</option>';
  Object.values(state.users).filter(u=>u.role==='teacher').forEach(t=>{ const opt=document.createElement('option'); opt.value=t.name; opt.innerText = t.display || t.name; sel.appendChild(opt); });
}

/* appointment send */
document.getElementById('requestAppt').addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const teacher = document.getElementById('apptTeacherSelect').value;
  const msg = document.getElementById('apptMsg').value.trim();
  if(!teacher || !msg) return alert('กรุณาเลือกครูและกรอกข้อความ');
  const appt = { id: generateId(), teacher, student: currentUser, msg, status:'pending', time: new Date().toLocaleString(), teacherNote:'', iso:new Date().toISOString() };
  state.users[currentUser].appts = state.users[currentUser].appts || []; state.users[currentUser].appts.push(appt);
  state.users[teacher].inbox = state.users[teacher].inbox || []; state.users[teacher].inbox.push(appt);
  saveState(); logActivity(`${currentUser} ขอเข้าปรึกษากับ ${teacher}`); alert('ส่งคำขอนัดเรียบร้อย'); document.getElementById('apptMsg').value=''; renderAll();
});

/* ---------- Students list + modify stars (teacher) ---------- */
function renderStudentsList(){
  const q = document.getElementById('searchStudent')?.value.trim().toLowerCase();
  const container = document.getElementById('studentsList'); if(!container) return;
  const students = Object.values(state.users).filter(u=>u.role==='student' && (!q || (u.display||u.name).toLowerCase().includes(q)));
  if(!students.length){ container.innerHTML = '<div class="meta">ไม่มีนักเรียน</div>'; return; }
  container.innerHTML = students.map(s=> {
    const avatar = s.avatar ? `<div class="student-avatar"><img src="${s.avatar}"></div>` : `<div class="student-avatar">${(s.display||s.name).slice(0,2).toUpperCase()}</div>`;
    return `<div style="padding:8px;border-bottom:1px solid #f3f6fb;display:flex;align-items:center;gap:10px">
      ${avatar}
      <div style="flex:1"><div><strong>${s.display||s.name}</strong></div><div class="meta">ชั้น: ${s.classId||'-'} • ${s.grade||'-'}</div></div>
      <div style="display:flex;align-items:center;gap:8px">
        <div style="font-weight:700;color:var(--primary)">⭐ <span id="star-count-${s.name}">${s.stars||0}</span></div>
        <div style="display:flex;flex-direction:column;gap:6px">
          <button class="addStar" data-name="${s.name}">+1</button>
          <button class="removeStar btn-ghost" data-name="${s.name}">−1</button>
        </div>
      </div>
    </div>`;
  }).join('');
  document.querySelectorAll('.addStar').forEach(b=>b.addEventListener('click', e=> modifyStars(e.target.dataset.name,1)));
  document.querySelectorAll('.removeStar').forEach(b=>b.addEventListener('click', e=> modifyStars(e.target.dataset.name,-1)));
}
document.getElementById('searchStudent')?.addEventListener('input', renderStudentsList);

function modifyStars(studentName, delta){
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const actor = state.users[currentUser];
  if(!actor || actor.role!=='teacher') return alert('ต้องเป็นครูจึงทำได้');
  const s = state.users[studentName];
  if(!s) return alert('ไม่พบนักเรียน');
  const old = s.stars||0; s.stars = Math.max(0, old + delta);
  saveState(); const el = document.getElementById(`star-count-${s.name}`); if(el) el.innerText = s.stars;
  logActivity(`${actor.display||actor.name} ${delta>0?'เพิ่ม':'ลด'} ดาวให้ ${s.display||s.name}: ${old} → ${s.stars}`);
  renderProfile(); renderQuickPanel();
}

/* ---------- Report to advisor with optional star adjust ---------- */
function populateReportSelectors(){
  const studentSel = document.getElementById('reportStudentSelect');
  const advSel = document.getElementById('reportAdvisorSelect');
  if(studentSel){ studentSel.innerHTML='<option value="">-- เลือกนักเรียน --</option>'; Object.values(state.users).filter(u=>u.role==='student').forEach(s=>{ const opt=document.createElement('option'); opt.value=s.name; opt.innerText=`${s.display||s.name} • ${s.classId||''}`; studentSel.appendChild(opt); }); }
  if(advSel){ advSel.innerHTML='<option value="">-- เลือกครูที่ปรึกษา --</option>'; Object.values(state.users).filter(u=>u.role==='teacher').forEach(t=>{ const opt=document.createElement('option'); opt.value=t.name; opt.innerText = t.display || t.name; advSel.appendChild(opt); }); }
}
document.getElementById('clearReportFields')?.addEventListener('click', ()=>{
  document.getElementById('reportStudentSelect').value='';
  document.getElementById('reportAdvisorSelect').value='';
  document.getElementById('reportTextToAdvisor').value='';
  document.getElementById('reportSendResult').innerText='';
  document.getElementById('starAdjustInfo').innerText='';
});
document.getElementById('incStarBtn')?.addEventListener('click', ()=>{
  const student = document.getElementById('reportStudentSelect').value;
  if(!student) return alert('เลือกลูกศิษย์ก่อนปรับดาว');
  modifyStars(student,1);
  document.getElementById('starAdjustInfo').innerText = `ปรับ +1 ให้ ${state.users[student].display||student}`;
});
document.getElementById('decStarBtn')?.addEventListener('click', ()=>{
  const student = document.getElementById('reportStudentSelect').value;
  if(!student) return alert('เลือกลูกศิษย์ก่อนปรับดาว');
  modifyStars(student,-1);
  document.getElementById('starAdjustInfo').innerText = `ปรับ -1 ให้ ${state.users[student].display||student}`;
});

document.getElementById('sendReportToAdvisor')?.addEventListener('click', ()=>{
  if(!currentUser) return alert('กรุณาเข้าสู่ระบบ');
  const sender = state.users[currentUser];
  if(!sender || sender.role!=='teacher') return alert('ต้องเป็นครูเพื่อส่งรายงาน');
  const studentName = document.getElementById('reportStudentSelect').value;
  const advisorName = document.getElementById('reportAdvisorSelect').value;
  const text = document.getElementById('reportTextToAdvisor').value.trim();
  if(!studentName || !advisorName || !text) return alert('กรุณาเลือกนักเรียน เลือกครูที่ปรึกษา และกรอกเนื้อหาหรือบันทึก');
  // simple local analysis (flagging) reused from prior versions (minimal)
  const lower = text.toLowerCase();
  const NEG = ['ตี','ทำร้าย','ทะเลาะ','โดนรังแก','กลั่นแกล้ง','หนีเรียน','ขาดเรียน','โดดเรียน','ซึมเศร้า','เครียด','คิดสั้น','ทำร้ายตัวเอง','ติดยา','เสพยา','เมา','รังแก','เกเร','พกของมีคม'];
  const matches = NEG.filter(k => lower.includes(k));
  const score = Math.min(5, matches.length);
  const flagged = score>0;
  const report = { id: generateId(), teacher: currentUser, teacherDisplay: sender.display||sender.name, student: studentName, text, time: new Date().toLocaleString(), viewed:false, flagged, score, matches };
  state.users[studentName].reports = state.users[studentName].reports || []; state.users[studentName].reports.push(report);
  state.users[advisorName].inboxReports = state.users[advisorName].inboxReports || []; state.users[advisorName].inboxReports.push(report);
  saveState(); logActivity(`${currentUser} ส่งรายงานของ ${studentName} ถึง ${advisorName} (flagged:${flagged}, score:${score})`);
  document.getElementById('reportSendResult').innerText = 'ส่งเรียบร้อยแล้ว';
  document.getElementById('reportTextToAdvisor').value='';
  renderAll();
});

/* ---------- Render advisor inbox ---------- */
function renderAdvisorInbox(){
  const el = document.getElementById('advisorInbox');
  if(!currentUser || !el) return;
  const u = state.users[currentUser];
  if(u.role!=='teacher'){ el.innerHTML = '<div class="meta">เฉพาะครูที่ปรึกษาเท่านั้นที่เห็นกล่องนี้</div>'; return; }
  const inbox = (u.inboxReports||[]).slice().reverse();
  if(!inbox.length){ el.innerHTML = '<div class="meta">ยังไม่มีรายงานที่ส่งมา</div>'; return; }
  el.innerHTML = inbox.map(r=>{
    const flaggedHtml = r.flagged ? `<div style="margin-top:6px"><span class="flagged">พบสัญญาณเชิงลบ (คะแนน ${r.score})</span></div>` : '';
    return `<div style="padding:8px;border-bottom:1px solid #f3f6fb">
      <div class="meta">${r.time} — รายงานจาก: <strong>${r.teacherDisplay||r.teacher}</strong></div>
      <div style="margin-top:6px"><strong>นักเรียน:</strong> ${state.users[r.student]?.display || r.student}</div>
      <div style="margin-top:6px">${escapeHtml(r.text)}</div>
      ${r.matches && r.matches.length ? `<div class="meta" style="margin-top:6px">คำที่ตรวจพบ: ${r.matches.join(' • ')}</div>` : ''}
      ${flaggedHtml}
    </div>`;
  }).join('');
}

/* ---------- Charts: admin mood (reused) ---------- */
function renderAdminMoodChart(){
  const el = document.getElementById('adminMoodChart'); if(!el) return;
  const moodCounts = {}; emojiChoices.forEach(e=> moodCounts[e.label]=0);
  const students = Object.values(state.users).filter(u=>u.role==='student');
  students.forEach(s=>{ if(s.moods && s.moods.length){ const last=s.moods[s.moods.length-1]; moodCounts[last.label] = (moodCounts[last.label]||0)+1; }});
  const labels = Object.keys(moodCounts), data = Object.values(moodCounts), bg = emojiChoices.map(e=>e.color);
  if(adminMoodChart) adminMoodChart.destroy();
  adminMoodChart = new Chart(el.getContext('2d'), { type:'doughnut', data:{ labels, datasets:[{ data, backgroundColor:bg }] }, options:{ responsive:true, plugins:{legend:{position:'bottom'}} } });
}

/* ---------- Render panels & other utilities ---------- */
function renderProfile(){
  const avatarBox = document.getElementById('profileAvatar');
  const nameEl = document.getElementById('profileName'); const roleEl = document.getElementById('profileRole'); const classEl = document.getElementById('profileClass');
  const starsWrap = document.getElementById('profileStarsWrap'); const starsEl = document.getElementById('profileStars');
  const box = document.getElementById('profileBox');
  if(!currentUser){ avatarBox.innerHTML='LV'; nameEl.innerHTML='<strong>-</strong>'; roleEl.innerText='-'; classEl.innerText=''; starsWrap.style.display='inline-block'; starsEl.innerText='0'; box.innerHTML='เข้าสู่ระบบเพื่อดูโปรไฟล์'; return; }
  const u = state.users[currentUser];
  avatarBox.innerHTML=''; if(u.avatar){ const img = document.createElement('img'); img.src=u.avatar; avatarBox.appendChild(img); } else avatarBox.innerText=(u.display||u.name).slice(0,2).toUpperCase();
  nameEl.innerHTML=`<strong>${u.display||u.name}</strong>`; roleEl.innerText = u.role;
  classEl.innerText = (u.role==='student'? `ชั้นเรียน: ${u.classId||'-'} • ${u.grade||'-'}` : '');
  if(u.role==='teacher' || u.role==='admin') starsWrap.style.display='none'; else { starsWrap.style.display='inline-block'; starsEl.innerText = u.stars||0; }
  let html = `<div class="meta">บันทึกล่าสุด:</div>`;
  if(u.moods && u.moods.length){ const last = u.moods[u.moods.length-1]; html += `<div style="margin-top:6px">${last.time} — ${last.emoji} ${last.label}</div><div class="muted" style="margin-top:6px">${last.note||'-'}</div>`; } else html += `<div class="muted" style="margin-top:6px">ยังไม่มีบันทึก</div>`;
  box.innerHTML = html;
}

function renderPanels(){
  if(!currentUser) return;
  const u = state.users[currentUser];
  document.getElementById('studentPanel').style.display = u.role==='student' ? 'block' : 'none';
  document.getElementById('teacherPanel').style.display = u.role==='teacher' ? 'block' : 'none';
  document.getElementById('adminPanel').style.display = u.role==='admin' ? 'block' : 'none';

  // Student-specific
  if(u.role==='student'){
    document.getElementById('lastStudentMoodText').innerText = u.moods && u.moods.length ? `${u.moods[u.moods.length-1].emoji} ${u.moods[u.moods.length-1].label} — ${u.moods[u.moods.length-1].time}` : '-';
    renderDiaryHistoryForUser(u, 'studentDiaryHistory');
    renderTodayMoodForStudent();
    renderStudentDailyPie();
    populateTeachersForAppt();
    renderApptHistoryForStudent();
  }

  // Teacher-specific
  if(u.role==='teacher'){
    renderApptRequests(); renderReportsList(); populateReportSelectors(); renderStudentsList(); renderAdvisorInbox(); renderTeacherBehaviorChart();
    // initialize teacher mood pies (selected student from teacherSelect if exists)
    const sel = document.getElementById('teacherSelect')?.value || document.getElementById('reportStudentSelect')?.value;
    renderTeacherMoodPies(sel);
  }

  // Admin
  if(u.role==='admin'){ renderAdminMoodChart(); renderBehaviorCharts(); }

  renderQuickPanel();
  renderActivity();
}

/* diary history display */
function renderDiaryHistoryForUser(user, containerId){
  const el = document.getElementById(containerId); if(!el) return;
  if(!user.diaries || !user.diaries.length){ el.innerHTML = '<div class="meta">ยังไม่มีบันทึก My diary</div>'; return; }
  el.innerHTML = user.diaries.slice().reverse().map(d=>{
    const emoji = d.emoji ? `<div class="diary-emoji">${d.emoji}</div>` : `<div class="diary-emoji">—</div>`;
    const noteHtml = d.text ? `<div>${escapeHtml(d.text)}</div>` : `<div class="muted">ไม่มีข้อความเพิ่มเติม</div>`;
    return `<div class="diary-item">${emoji}<div class="diary-content"><div class="meta">${d.time} ${d.label ? '— ' + d.label : ''}</div><div style="margin-top:6px">${noteHtml}</div></div></div>`;
  }).join('');
}

/* today's mood card for student */
function renderTodayMoodForStudent(){
  const el = document.getElementById('todayMoodDisplay'); if(!el) return;
  if(!currentUser) { el.style.display='none'; return; }
  const u = state.users[currentUser];
  if(!u.moods || !u.moods.length){ el.style.display='none'; return; }
  const last = u.moods[u.moods.length-1];
  if(!isSameDayIso(last.iso, new Date().toISOString())){ el.style.display='none'; return; }
  document.getElementById('todayMoodEmoji').innerText = last.emoji || '';
  document.getElementById('todayMoodLabel').innerText = `${last.label||''}`;
  document.getElementById('todayMoodTime').innerText = `${last.time||''}`;
  document.getElementById('todayMoodNote').innerText = last.note ? escapeHtml(last.note) : '-';
  el.style.display='block';
}

/* ---------- some minimal retained functions for appt/redeem/report/risks/charts ---------- */
/* To keep this file focused on requested features, we include essential helpers used above. */

function renderApptHistoryForStudent(){
  const el = document.getElementById('apptHistory'); if(!el || !currentUser) return;
  const u = state.users[currentUser];
  if(!u.appts || !u.appts.length){ el.innerHTML = '<div class="meta">ยังไม่มีการขอนัด</div>'; return; }
  el.innerHTML = u.appts.slice().reverse().map(a=>`<div class="diary-item"><div class="diary-emoji">📅</div><div class="diary-content"><div class="meta">${a.time} → ถึง: ${a.teacher} [${a.status}]</div><div style="margin-top:6px">${escapeHtml(a.msg)}</div></div></div>`).join('');
}

function renderApptRequests(){
  const el = document.getElementById('apptRequests'); if(!el || !currentUser) return;
  const inbox = (state.users[currentUser].inbox||[]);
  if(!inbox.length){ el.innerHTML = '<div class="meta">ยังไม่มีคำขอนัด</div>'; return; }
  el.innerHTML = inbox.map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time} — จาก: <strong>${a.student}</strong></div><div style="margin-top:6px">${escapeHtml(a.msg)}</div><div style="margin-top:8px">${a.status==='approved'?'<span class="badge">อนุมัติ</span>':`<button class="approveBtn" data-id="${a.id}">อนุมัติ</button><button class="rejectBtn" data-id="${a.id}">ปฏิเสธ</button>`} <button class="noteBtn" data-id="${a.id}">หมายเหตุ</button></div></div>`).join('');
  document.querySelectorAll('.approveBtn').forEach(b=>b.addEventListener('click', e=> handleApptAction(e.target.dataset.id,'approved')));
  document.querySelectorAll('.rejectBtn').forEach(b=>b.addEventListener('click', e=> handleApptAction(e.target.dataset.id,'rejected')));
  document.querySelectorAll('.noteBtn').forEach(b=>b.addEventListener('click', e=>{ const id=e.target.dataset.id; const note=prompt('หมายเหตุสำหรับการนัด:'); if(note!==null) handleApptNote(id,note); }));
}
function handleApptAction(id,status){ const t = state.users[currentUser]; const item = (t.inbox||[]).find(x=>x.id===id); if(!item) return; item.status = status; const stu = state.users[item.student]; if(stu){ const ap = stu.appts.find(x=>x.id===id); if(ap) ap.status = status; } saveState(); logActivity(`${currentUser} ${status==='approved'?'อนุมัติ':'ปฏิเสธ'} นัดจาก ${item.student}`); renderAll(); }
function handleApptNote(id,note){ const t = state.users[currentUser]; const item = (t.inbox||[]).find(x=>x.id===id); if(!item) return; item.teacherNote = note; const stu = state.users[item.student]; if(stu){ const ap = stu.appts.find(x=>x.id===id); if(ap) ap.teacherNote = note; } saveState(); logActivity(`${currentUser} บันทึกหมายเหตุนัด ${item.student}`); renderAll(); }

/* redeem requests minimal */
function renderTeacherRedeemRequests(){
  const el = document.getElementById('teacherRedeemRequests'); if(!el) return;
  const pending = (state.redeemRequests||[]).filter(r=>r.status==='pending');
  if(!pending.length){ el.innerHTML = '<div class="meta">ยังไม่มีคำขอแลกจากนักเรียน</div>'; return; }
  el.innerHTML = pending.slice().reverse().map(r=>{
    const s = state.users[r.student];
    const avatar = s && s.avatar ? `<div class="student-avatar"><img src="${s.avatar}"></div>` : `<div class="student-avatar">${(s? s.display : r.student).slice(0,2).toUpperCase()}</div>`;
    return `<div style="padding:8px;border-bottom:1px solid #f3f6fb;display:flex;gap:10px;align-items:center">${avatar}<div style="flex:1"><div><strong>${s? s.display : r.student}</strong> <span class="meta">${s? (s.classId || '') + ' • ' + (s.grade || '') : ''}</span></div><div class="meta" style="margin-top:6px">${r.item} — ${r.cost} ⭐</div><div class="meta" style="margin-top:6px">ส่ง: ${r.time}</div></div><div style="display:flex;flex-direction:column;gap:6px"><button class="approveRedeemBtn" data-id="${r.id}">อนุมัติ</button><button class="rejectRedeemBtn btn-ghost" data-id="${r.id}">ไม่อนุมัติ</button></div></div>`;
  }).join('');
  document.querySelectorAll('.approveRedeemBtn').forEach(b=>b.addEventListener('click', e=> handleApproveRedeem(e.target.dataset.id)));
  document.querySelectorAll('.rejectRedeemBtn').forEach(b=>b.addEventListener('click', e=> handleRejectRedeem(e.target.dataset.id)));
}
function handleApproveRedeem(id){ if(!currentUser) return alert('กรุณาเข้าสู่ระบบเป็นครูเพื่ออนุมัติ'); const req=(state.redeemRequests||[]).find(r=>r.id===id); if(!req) return alert('ไม่พบคำขอ'); const student=state.users[req.student]; if(!student) return alert('ไม่พบข้อมูลนักเรียน'); if((student.stars||0) < req.cost) if(!confirm(`นักเรียนมีดาวไม่พอ (${student.stars||0} ⭐) จะอนุมัติแล้วหักดาวลงไปหรือไม่?`)) return; student.stars = Math.max(0,(student.stars||0)-req.cost); student.redeemHistory = student.redeemHistory||[]; student.redeemHistory.push({ item:req.item, cost:req.cost, time:new Date().toLocaleString(), approvedBy: state.users[currentUser].display||currentUser }); req.status='approved'; req.approvedBy = state.users[currentUser].display||currentUser; saveState(); logActivity(`${currentUser} อนุมัติ: ${req.item} ของ ${student.name}`); renderAll(); }
function handleRejectRedeem(id){ if(!currentUser) return alert('กรุณาเข้าสู่ระบบเป็นครู'); const req=(state.redeemRequests||[]).find(r=>r.id===id); if(!req) return alert('ไม่พบคำขอ'); req.status='rejected'; req.approvedBy=state.users[currentUser].display||currentUser; saveState(); logActivity(`${currentUser} ปฏิเสธคำขอของ ${req.student}`); renderAll(); }

/* reports list for teacher (reports they created) */
function renderReportsList(){ const all=[]; Object.values(state.users).forEach(u=>{ if(u.reports) u.reports.forEach(r=>{ if(r.teacher===currentUser) all.push(r); })}); const el=document.getElementById('reportsList'); if(!el) return; if(!all.length){ el.innerHTML='<div class="meta">ยังไม่มีรายงาน</div>'; return; } el.innerHTML = all.map(r=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${r.time} → ${r.student}</div><div>${escapeHtml(r.text)}</div></div>`).join(''); }

/* risk detection & behavior charts (minimal, reused) */
function studentRiskInfo(student){ const reasons=[]; const now=new Date(); const moods=student.moods||[]; const negativeLabels=['เศร้า','โกรธ','เหนื่อย']; const sevenDaysAgo=new Date(); sevenDaysAgo.setDate(now.getDate()-7); const recent=moods.filter(m=>m.iso && new Date(m.iso)>=sevenDaysAgo); const negCount = recent.reduce((acc,m)=> acc + (negativeLabels.includes(m.label)?1:0),0); if(negCount>=2) reasons.push(`มี ${negCount} ครั้งของอารมณ์เชิงลบใน 7 วันล่าสุด`); const thirtyDaysAgo = new Date(); thirtyDaysAgo.setDate(now.getDate()-30); const reports = (student.reports||[]).filter(r=> r.time && new Date(r.time) >= thirtyDaysAgo); const flaggedReports = reports.filter(r=> r.flagged); if(flaggedReports.length>=1) reasons.push(`พบ ${flaggedReports.length} รายงานเชิงลบใน 30 วัน`); const rptCount=(student.reports||[]).length; if(rptCount>=3) reasons.push(`มี ${rptCount} รายงานพฤติกรรมทั้งหมด`); let level=null; if(reasons.length>=2) level='high'; else if(reasons.length===1) level='medium'; else level=null; return { level, reasons, flaggedReportsCount: flaggedReports.length }; }
function buildRiskLists(){ const students=Object.values(state.users).filter(u=>u.role==='student'); const risks=students.map(s=>({ student:s, info: studentRiskInfo(s) })).filter(x=>x.info.level); risks.sort((a,b)=>{ const score=l=> l==='high'?2:(l==='medium'?1:0); return score(b.info.level)-score(a.info.level); }); return risks; }

function renderTeacherBehaviorChart(){
  const mode = document.querySelector('.teacherModeBtn.active')?.dataset.mode || 'student';
  const labels=[]; const data=[];
  if(mode==='student'){
    const students = Object.values(state.users).filter(u=>u.role==='student');
    students.forEach(s=>{ labels.push(s.display||s.name); data.push(studentRiskInfo(s).flaggedReportsCount||0); });
  } else if(mode==='class'){
    const classes = Array.from(new Set(Object.values(state.users).filter(u=>u.role==='student').map(s=>s.classId||'ไม่ระบุ')));
    classes.forEach(c=>{ labels.push(c); const count = Object.values(state.users).filter(u=>u.role==='student'&&u.classId===c).reduce((acc,s)=> acc + (studentRiskInfo(s).flaggedReportsCount||0),0); data.push(count); });
  } else {
    const grades = Array.from(new Set(Object.values(state.users).filter(u=>u.role==='student').map(s=>s.grade||'ไม่ระบุ')));
    grades.forEach(g=>{ labels.push(g); const count = Object.values(state.users).filter(u=>u.role==='student'&&u.grade===g).reduce((acc,s)=> acc + (studentRiskInfo(s).flaggedReportsCount||0),0); data.push(count); });
  }
  const ctx = document.getElementById('teacherBehaviorRiskChart')?.getContext('2d'); if(!ctx) return;
  if(window.teacherBehaviorChart) window.teacherBehaviorChart.destroy();
  window.teacherBehaviorChart = new Chart(ctx, { type:'bar', data:{ labels, datasets:[{ label:'รายงานเชิงลบ (จำนวน)', data, backgroundColor: labels.map(()=>hexToRgba('#fb7185',0.9)) }] }, options:{ responsive:true, plugins:{legend:{display:false}}, scales:{ y:{beginAtZero:true,ticks:{precision:0}} } } });
}

function renderBehaviorCharts(){ renderAdminBehaviorChart(); renderTeacherBehaviorChart(); }
function renderAdminBehaviorChart(){
  const students = Object.values(state.users).filter(u=>u.role==='student'); const counts={high:0,medium:0,none:0};
  students.forEach(s=>{ const info=studentRiskInfo(s); if(info.level==='high') counts.high++; else if(info.level==='medium') counts.medium++; else counts.none++; });
  const ctx = document.getElementById('adminBehaviorRiskChart')?.getContext('2d'); if(!ctx) return;
  if(adminBehaviorChart) adminBehaviorChart.destroy();
  adminBehaviorChart = new Chart(ctx, { type:'doughnut', data:{ labels:['เสี่ยงสูง','เสี่ยงปานกลาง','ปกติ'], datasets:[{ data:[counts.high,counts.medium,counts.none], backgroundColor:['#ef4444','#f59e0b','#7c3aed'] }] }, options:{ responsive:true, plugins:{legend:{position:'bottom'}} } });
}

/* ---------- quick panel & activity ---------- */
function renderQuickPanel(){ const el=document.getElementById('quickPanel'); if(!currentUser){ el.innerHTML=''; return; } const u=state.users[currentUser]; let html=`<div class="meta">บทบาท: ${u.role}</div>`; if(u.role==='student'){ html+=`<div style="margin-top:6px"><strong>ดาว: ${u.stars||0}</strong></div>`; html+=`<div class="meta" style="margin-top:6px">บันทึก: ${(u.diaries||[]).length} ครั้ง</div>`; } else if(u.role==='teacher'){ const pending=(u.inbox||[]).filter(i=>i.status==='pending').length; html+=`<div style="margin-top:6px"><strong>คำขอนัดรออนุมัติ: ${pending}</strong></div>`; } else if(u.role==='admin'){ const students = Object.values(state.users).filter(x=>x.role==='student').length; html+=`<div style="margin-top:6px"><strong>นักเรียนทั้งหมด: ${students}</strong></div>`; } el.innerHTML=html; }
function renderActivity(){ const el=document.getElementById('activityLog'); if(!el) return; el.innerHTML = (state.activity||[]).map(a=>`<div style="padding:8px;border-bottom:1px solid #f3f6fb"><div class="meta">${a.time}</div><div>${a.txt}</div></div>`).join(''); }

/* ---------- initial render & events ---------- */
populateUserSelect();
renderActivity();
renderAll();

document.querySelectorAll('.teacherModeBtn').forEach(b=>b.addEventListener('click', ()=>{
  document.querySelectorAll('.teacherModeBtn').forEach(x=>x.classList.remove('active')); b.classList.add('active');
  updateTeacherSelectLabel(); renderTeacherBehaviorChart();
}));
document.getElementById('teacherSelect')?.addEventListener('change', ()=> { const sel = document.getElementById('teacherSelect'); renderTeacherMoodPies(sel.value); });
document.getElementById('teacherViewBtn')?.addEventListener('click', ()=> { const mode = document.querySelector('.teacherModeBtn.active')?.dataset.mode || 'student'; const id = document.getElementById('teacherSelect')?.value; const period = document.getElementById('teacherPeriod')?.value || 'week'; renderTeacherDetail(mode,id,period); });

function renderAll(){ renderProfile(); renderPanels(); renderActivity(); renderTeacherControls(); renderAdminMoodChart(); renderBehaviorCharts(); renderStudentDailyPie(); renderTeacherMoodPies(document.getElementById('teacherSelect')?.value); }
function renderTeacherControls(){ updateTeacherSelectLabel(); populateReportSelectors(); populateTeachersForAppt(); renderStudentsList(); renderAdvisorInbox(); renderTeacherBehaviorChart(); }
function updateTeacherSelectLabel(){
  const mode=document.querySelector('.teacherModeBtn.active')?.dataset.mode||'student'; const label=document.getElementById('teacherSelectLabel'); const sel=document.getElementById('teacherSelect'); if(!label||!sel) return; sel.innerHTML=''; if(mode==='student'){ label.innerText='เลือกนักเรียน'; Object.values(state.users).filter(u=>u.role==='student').forEach(s=>{ const opt=document.createElement('option'); opt.value=s.name; opt.innerText=`${s.display||s.name} • ${s.classId||''} ${s.grade||''}`; sel.appendChild(opt); }); } else if(mode==='class'){ label.innerText='เลือกห้องเรียน'; const classes=Array.from(new Set(Object.values(state.users).filter(u=>u.role==='student').map(s=>s.classId||'ไม่ระบุ'))); classes.forEach(c=>{ const opt=document.createElement('option'); opt.value=c; opt.innerText=c; sel.appendChild(opt); }); } else { label.innerText='เลือกชั้นปี'; const grades=Array.from(new Set(Object.values(state.users).filter(u=>u.role==='student').map(s=>s.grade||'ไม่ระบุ'))); grades.forEach(g=>{ const opt=document.createElement('option'); opt.value=g; opt.innerText=g; sel.appendChild(opt); }); } if(sel.options.length) sel.selectedIndex = 0; }

/* teacher detail chart (reuse earlier aggregate functions if available) */
function renderTeacherDetail(mode, id, period){
  // minimal implementation: reuse aggregateByPeriod from previous versions (not included in full here)
  // we'll show placeholder behavior if no data
  const ctx = document.getElementById('teacherDetailChart')?.getContext('2d'); if(!ctx) return;
  if(window.teacherDetailChart) window.teacherDetailChart.destroy();
  window.teacherDetailChart = new Chart(ctx, { type:'bar', data:{ labels:['อ.1','อ.2','อ.3'], datasets:[{ label:'ตัวอย่าง', data:[1,2,0], backgroundColor: ['#7c3aed','#fb7185','#7BE495'] }] }, options:{ responsive:true } });
}

/* end of script */
</script>
</body>
</html>
