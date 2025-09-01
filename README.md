<!doctype html>
<html lang="en"><head>
<meta charset="utf-8"><meta name="viewport" content="width=device-width, initial-scale=1">
<title>SiestaHub — Microsoft Nap Pods (Single File)</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
<style>
*{box-sizing:border-box}
:root{--bg:#0b1220;--card:#0f172a;--fg:#e5e7eb;--muted:#94a3b8;--brand:#60a5fa;--acc:#22c55e;--warn:#f59e0b;--line:#1f2a44}
@media (prefers-color-scheme: light){:root{--bg:#f8fafc;--card:#ffffff;--fg:#0f172a;--muted:#475569;--brand:#2563eb;--acc:#16a34a;--warn:#b45309;--line:#e5e7eb}}
html,body{margin:0;padding:0;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,Helvetica,Arial,sans-serif;background:var(--bg);color:var(--fg)}
.shell{max-width:1100px;margin:0 auto;padding:20px}
header{padding-top:28px;text-align:center}
.brand{display:flex;gap:10px;align-items:center;justify-content:center}
.brand .dot{width:14px;height:14px;border-radius:50%;background:var(--brand);box-shadow:0 0 0 6px rgba(96,165,250,.15)}
h1{margin:0;font-size:34px;font-weight:800;letter-spacing:.2px}
.tag{margin:6px 0 0;color:var(--muted)}
.grid2{display:grid;grid-template-columns:2fr 1.1fr;gap:18px}
.card{background:var(--card);border:1px solid var(--line);border-radius:16px;padding:18px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px}
label{display:flex;flex-direction:column;gap:6px;font-weight:600}
input,select{padding:10px;border-radius:12px;border:1px solid var(--line);background:transparent;color:var(--fg)}
.range input{width:100%}
.muted{color:var(--muted)} .s{font-size:12px}
.suggest{margin:10px 0 0;display:flex;flex-wrap:wrap;gap:8px;align-items:center;color:var(--muted)}
.chip{padding:6px 10px;border-radius:999px;border:1px solid var(--line);background:transparent;color:var(--fg);cursor:pointer}
.chip:hover{border-color:var(--brand)}
.actions{display:flex;gap:10px;margin-top:14px;flex-wrap:wrap}
button{background:var(--brand);color:#fff;border:none;border-radius:12px;padding:10px 16px;font-weight:700;cursor:pointer}
button.ghost{background:transparent;border:1px solid var(--line);color:var(--fg)}
button:hover{filter:brightness(.95)}
.alert{background:rgba(245,158,11,.1);border:1px solid var(--warn);color:var(--warn);padding:10px;border-radius:10px;margin-top:10px}
.ok{background:rgba(34,197,94,.1);border:1px solid var(--acc);color:var(--acc);padding:10px;border-radius:10px;margin-top:10px}
.hidden{display:none} .confirm{margin-top:8px}
.agenda .list{margin-top:10px;border:1px dashed var(--line);border-radius:12px;padding:10px;max-height:240px;overflow:auto}
.item{display:flex;justify-content:space-between;border-bottom:1px dashed var(--line);padding:6px 0}
.item:last-child{border-bottom:none}
.line{height:1px;background:var(--line);margin:16px 0}
.foot{display:flex;justify-content:space-between;align-items:center;gap:10px}
.qrwrap{display:flex;flex-direction:column;align-items:center;gap:6px}
.qr{width:120px;height:120px;border-radius:10px;border:1px solid var(--line);background:#fff}
@media (max-width:900px){.grid2{grid-template-columns:1fr}}
</style>
</head>
<body>
<header class="shell">
  <div class="brand">
    <div class="dot"></div>
    <h1>SiestaHub</h1>
  </div>
  <p class="tag">Recharge your mind. Reimagine what's possible.</p>
</header>

<main class="shell grid2">
  <section class="card">
    <h2>Book a nap</h2>
    <p class="muted">Reserve a 10–25 minute siesta in your office nap pods. This demo stores bookings in your browser and creates a calendar invite.</p>

    <form id="form">
      <div class="grid">
        <label>Office
          <select id="office" required>
            <option value="">Choose office</option>
            <option>Redmond</option>
            <option>Dubai Internet City</option>
            <option>Hyderabad</option>
            <option>Singapore</option>
            <option>Dublin</option>
          </select>
        </label>
        <label>Room / Pod
          <select id="room" required>
            <option value="">Choose pod</option>
            <option>Pod A</option>
            <option>Pod B</option>
            <option>Pod C</option>
            <option>Pod D</option>
          </select>
        </label>
        <label>Date
          <input type="date" id="date" required>
        </label>
        <label>Start time
          <input type="time" id="time" required>
        </label>
        <label class="range">Duration: <span id="durLabel">20</span> min
          <input type="range" id="duration" min="10" max="25" value="20" step="5">
        </label>
        <label>Your name
          <input type="text" id="name" placeholder="Alex Chen" required>
        </label>
        <label>Email (optional)
          <input type="email" id="email" placeholder="alex@contoso.com">
        </label>
      </div>
      <div class="suggest">
        Quick picks:
        <button class="chip" data-time="13:00" type="button">13:00</button>
        <button class="chip" data-time="13:30" type="button">13:30</button>
        <button class="chip" data-time="14:00" type="button">14:00</button>
        <button class="chip" data-time="14:30" type="button">14:30</button>
        <button class="chip" data-time="15:00" type="button">15:00</button>
      </div>
      <div class="actions">
        <button type="submit">Book siesta</button>
        <button id="reset" type="button" class="ghost">Reset</button>
      </div>
    </form>

    <div id="conflict" class="alert hidden"></div>
    <div id="ok" class="ok hidden"></div>

    <div id="confirm" class="hidden confirm">
      <h3>You're booked ✅</h3>
      <p id="confirmText"></p>
      <div class="actions">
        <button id="downloadIcs">Download .ics</button>
        <button id="new">New booking</button>
      </div>
    </div>
  </section>

  <aside class="card">
    <h3>Today's agenda</h3>
    <p class="muted">See pod reservations (this device).</p>
    <div class="agenda">
      <label>Choose date
        <input type="date" id="agendaDate">
      </label>
      <div id="agendaList" class="list"></div>
    </div>
    <div class="line"></div>
    <h3>Guidelines</h3>
    <ul>
      <li>One siesta per employee per day.</li>
      <li>10–25 minutes to avoid grogginess.</li>
      <li>Arrive on time — pods release after 5 minutes.</li>
      <li>Phones on silent. No food or drinks inside.</li>
    </ul>
    <h3>Why naps?</h3>
    <p>Short naps can improve focus, mood, and learning. Early afternoon aligns with the natural circadian dip.</p>
  </aside>
</main>

<footer class="shell foot">
  <div class="qrwrap">
    <div class="muted s">Once this page is live, make a QR to its URL.</div>
  </div>
  <div class="muted s">© Microsoft — concept demo for coursework</div>
</footer>

<script>
const form = document.getElementById('form');
const conflict = document.getElementById('conflict');
const ok = document.getElementById('ok');
const confirmBox = document.getElementById('confirm');
const confirmText = document.getElementById('confirmText');
const downloadBtn = document.getElementById('downloadIcs');
const newBtn = document.getElementById('new');
const dur = document.getElementById('duration');
const durLabel = document.getElementById('durLabel');
const agendaDate = document.getElementById('agendaDate');
const agendaList = document.getElementById('agendaList');

dur.addEventListener('input', ()=> durLabel.textContent = dur.value);
document.querySelectorAll('.chip').forEach(btn => {
  btn.addEventListener('click', ()=>{ document.getElementById('time').value = btn.dataset.time; });
});
document.getElementById('reset').addEventListener('click', ()=>{
  form.reset(); dur.value=20; durLabel.textContent='20'; conflict.classList.add('hidden'); ok.classList.add('hidden');
});

function pad(n){ return String(n).padStart(2,'0'); }
function toLocal(date, time){
  const [y,m,d] = date.split('-').map(Number);
  const [hh,mm] = time.split(':').map(Number);
  return new Date(y, m-1, d, hh, mm, 0);
}
function toICSDate(dt){
  const u = new Date(dt.getTime() - dt.getTimezoneOffset()*60000);
  return `${u.getUTCFullYear()}${pad(u.getUTCMonth()+1)}${pad(u.getUTCDate())}T${pad(u.getUTCHours())}${pad(u.getUTCMinutes())}00Z`;
}
function icsBlob({summary, description, location, start, end}){
  const uid = (crypto.randomUUID && crypto.randomUUID()) || Math.random().toString(36).slice(2);
  const lines = [
    'BEGIN:VCALENDAR','VERSION:2.0','PRODID:-//SiestaHub//EN','CALSCALE:GREGORIAN','METHOD:PUBLISH',
    'BEGIN:VEVENT',`UID:${uid}`,`DTSTAMP:${toICSDate(new Date())}`,
    `DTSTART:${toICSDate(start)}`,`DTEND:${toICSDate(end)}`,
    `SUMMARY:${summary}`,`DESCRIPTION:${description}`,`LOCATION:${location}`,
    'END:VEVENT','END:VCALENDAR'
  ].join('\\r\\n');
  return new Blob([lines], {type:'text/calendar'});
}
function key(b){ return `${b.office}__${b.room}`; }
function load(){ return JSON.parse(localStorage.getItem('siesta_single') || '{}'); }
function save(data){ localStorage.setItem('siesta_single', JSON.stringify(data)); }
function overlaps(aStart, aEnd, bStart, bEnd){ return !(aEnd <= bStart || aStart >= bEnd); }
function addBooking(b){
  const data = load();
  const k = key(b);
  data[k] = data[k] || [];
  for (const x of data[k]){
    if (overlaps(new Date(x.start), new Date(x.end), b.start, b.end)){
      return {ok:false, conflict:x};
    }
  }
  data[k].push({start:b.start.toISOString(), end:b.end.toISOString(), name:b.name, email:b.email});
  save(data);
  return {ok:true};
}
function listForDate(dateStr){
  const data = load(); const items=[];
  for (const [k, arr] of Object.entries(data)){
    for (const x of arr){
      const s = new Date(x.start);
      if (s.toISOString().slice(0,10) === dateStr){
        const [office, room] = k.split('__');
        items.push({office, room, start:new Date(x.start), end:new Date(x.end), name:x.name});
      }
    }
  }
  items.sort((a,b)=> a.start - b.start);
  return items;
}
function renderAgenda(dateStr){
  agendaList.innerHTML='';
  const items = listForDate(dateStr);
  if (!items.length){ agendaList.innerHTML = '<div class="muted">No bookings for this date (on this device).</div>'; return; }
  for (const it of items){
    const div = document.createElement('div');
    div.className='item';
    const t = `${it.start.toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'})}–${it.end.toLocaleTimeString([], {hour:'2-digit', minute:'2-digit'})}`;
    div.innerHTML = `<span>${t}</span><span>${it.office} • ${it.room}</span>`;
    agendaList.appendChild(div);
  }
}
(function init(){
  const today = new Date();
  const ds = today.toISOString().slice(0,10);
  document.getElementById('date').value = ds;
  agendaDate.value = ds;
  renderAgenda(ds);
  agendaDate.addEventListener('change', ()=> renderAgenda(agendaDate.value));
})();
form.addEventListener('submit', (e)=>{
  e.preventDefault();
  const office = document.getElementById('office').value;
  const room = document.getElementById('room').value;
  const date = document.getElementById('date').value;
  const time = document.getElementById('time').value;
  const duration = parseInt(document.getElementById('duration').value,10);
  const name = document.getElementById('name').value;
  const email = document.getElementById('email').value;
  const start = toLocal(date, time);
  const end = new Date(start.getTime() + duration*60000);
  const result = addBooking({office, room, start, end, name, email});
  if (!result.ok){
    conflict.textContent = `That slot overlaps with an existing booking in ${room}. Please choose another time.`;
    conflict.classList.remove('hidden'); ok.classList.add('hidden');
    return;
  }
  conflict.classList.add('hidden');
  ok.textContent = 'Slot reserved locally — generate your calendar invite below.';
  ok.classList.remove('hidden');
  const blob = icsBlob({
    summary:`Siesta — ${room}`,
    description:`Siesta for ${name} at ${office}.`,
    location:`${office} — ${room}`,
    start, end
  });
  const url = URL.createObjectURL(blob);
  downloadBtn.onclick = ()=>{ const a=document.createElement('a'); a.href=url; a.download='siesta.ics'; a.click(); };
  confirmText.textContent = `${name}, you're booked in ${room} at ${office} on ${start.toLocaleDateString()} from ${start.toLocaleTimeString([], {hour:'2-digit',minute:'2-digit'})} for ${duration} minutes.`;
  confirmBox.classList.remove('hidden');
  renderAgenda(date);
});
newBtn.addEventListener('click', ()=>{
  form.reset(); dur.value=20; durLabel.textContent='20'; confirmBox.classList.add('hidden'); ok.classList.add('hidden');
});
</script>
</body></html>
