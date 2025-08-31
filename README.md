<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Rough Plots · Daily Work Update</title>
  <style>
    :root{
      --primary: #2563eb;
      --primary-600: #1d4ed8;
      --bg: #f8fafc;
      --card: #ffffff;
      --muted: #6b7280;
      --danger: #ef4444;
      --radius: 12px;
      --shadow: 0 8px 24px rgba(2,6,23,0.08);
    }
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, Arial; background:linear-gradient(180deg,#f8fbff, #ffffff); color:#0f172a; padding:24px;}

    /* LOGIN centered */
    #loginScreen{min-height:calc(100vh - 48px); display:grid; place-items:center;}
    .login-card{width:100%;max-width:420px;background:var(--card);border-radius:var(--radius);padding:28px;box-shadow:var(--shadow);border:1px solid #e6eefb}
    .login-title{font-size:20px;font-weight:700;margin-bottom:6px;color:var(--primary)}
    .login-sub{font-size:13px;color:var(--muted);margin-bottom:16px}
    .row{display:flex;align-items:center;gap:10px;margin:10px 0}
    .row label{width:110px;font-weight:600}
    input[type="text"],input[type="password"],select,input[type="date"],input[type="number"]{flex:1;padding:10px;border-radius:8px;border:1px solid #e6eefb;outline:none}
    .btn{background:var(--primary);color:#fff;padding:10px 14px;border-radius:10px;border:none;cursor:pointer;font-weight:700}
    .btn:hover{background:var(--primary-600)}

    /* Dashboard */
    #dashboard{display:none;max-width:1200px;margin:0 auto}
    .dash{display:flex;align-items:center;justify-content:space-between;gap:10px;margin-bottom:18px}
    .dash h2{margin:0;font-size:20px}
    .dash-buttons{display:flex;gap:12px}
    .card-btn{padding:14px 18px;border-radius:12px;background:var(--card);box-shadow:var(--shadow);border:1px solid #eef3ff;cursor:pointer;font-weight:700}
    .card-btn:hover{background:#eef7ff}

    /* Form layout */
    #roughPlots{display:none;max-width:1200px;margin:0 auto}
    .form-wrap{display:flex;gap:20px;background:var(--card);padding:18px;border-radius:12px;border:1px solid #eef3ff;box-shadow:var(--shadow);flex-wrap:wrap}
    .col{flex:1 1 480px;min-width:320px}
    .field{display:flex;align-items:center;gap:12px;margin:10px 0}
    .field label{width:200px;font-weight:700;white-space:nowrap}
    .required::after{content:" *";color:var(--danger);margin-left:6px}
    .unit-input{position:relative;flex:1}
    .unit-input input{width:100%;padding:10px 48px 10px 12px;border-radius:8px;border:1px solid #e6eefb}
    .unit-badge{position:absolute;right:8px;top:50%;transform:translateY(-50%);background:#eef2ff;padding:4px 8px;border-radius:999px;font-size:12px;color:#0f172a;border:1px solid #e6eefb;pointer-events:none}

    .grid-two{display:grid;grid-template-columns:1fr 1fr;gap:10px}
    .helpers{display:flex;gap:16px;margin-top:16px;flex-wrap:wrap}
    .box{flex:1;min-height:160px;border-radius:10px;border:2px dashed #e6eefb;display:flex;align-items:center;justify-content:center;background:#fcfeff;padding:10px;text-align:center}

    .upload-controls{display:flex;gap:8px;margin-top:10px;justify-content:center}
    table{width:100%;border-collapse:collapse;margin-top:22px;background:var(--card);border-radius:10px;overflow:hidden;box-shadow:var(--shadow)}
    th,td{border:1px solid #eef3ff;padding:10px;font-size:13px;text-align:left}
    th{background:#f1f8ff}
    .edit{cursor:pointer;color:var(--primary);font-weight:700}
    .actions{display:flex;gap:10px;justify-content:flex-end;margin-top:12px}

    /* responsive */
    @media (max-width:900px){ .field label{width:140px} .unit-input input{padding-right:36px}}
  </style>
</head>
<body>

  <!-- LOGIN -->
  <section id="loginScreen">
    <div class="login-card">
      <div class="login-title">Rough Plots · Daily Work Update</div>
      <div class="login-sub">Sign in to manage Rough Plots</div>
      <div class="row"><label>Login ID</label><input id="loginId" type="text" placeholder="Enter Login ID" /></div>
      <div class="row"><label>Password</label><input id="password" type="password" placeholder="Enter Password" /></div>
      <div style="display:flex;justify-content:flex-end;margin-top:10px"><button class="btn" onclick="login()">Login</button></div>
      <div style="font-size:12px;color:var(--muted);margin-top:10px">Demo mode: any non-empty login shows the dashboard.</div>
    </div>
  </section>

  <!-- DASHBOARD -->
  <section id="dashboard">
    <div class="dash">
      <h2>Dashboard</h2>
      <div class="dash-buttons">
        <div class="card-btn" onclick="openPage('roughPlots')">📐 Rough Plots</div>
        <div class="card-btn" onclick="alert('Sitting Final - not implemented yet')">🪑 Sitting Final</div>
        <div class="card-btn" onclick="alert('Sitting Inform - not implemented yet')">📝 Sitting Inform</div>
      </div>
    </div>
  </section>

  <!-- ROUGH PLOTS FORM -->
  <section id="roughPlots">
    <h2 style="margin-top:0">Rough Plots Entry</h2>

    <div class="form-wrap">
      <!-- LEFT COLUMN -->
      <div class="col">
        <div class="field"><label class="required">Location</label>
          <select id="location"><option value="">-- Select --</option><option>GST</option><option>ECR</option><option>OMR</option></select></div>

        <div class="field"><label class="required">Layout Name</label><input id="layoutName" type="text" placeholder="Alpha-numeric" /></div>

        <div class="field"><label class="required">Plot No</label><input id="plotNo" type="text" placeholder="Alpha-numeric" /></div>

        <div class="field"><label>Road Feet / Facing</label><input id="roadFeet" type="text" placeholder="Alpha-numeric" /></div>

        <div class="field"><label class="required">Mediator Name</label><input id="mediatorName" type="text" placeholder="Alpha-numeric" /></div>

        <div class="field"><label class="required">Mediator Contact</label><input id="mediatorContact" type="text" maxlength="10" placeholder="10-digit number" /></div>

        <div class="field"><label class="required">Property Seller Name</label><input id="sellerName" type="text" placeholder="Alpha-numeric" /></div>

        <div class="field"><label class="required">Property Seller Contact</label><input id="sellerContact" type="text" maxlength="10" placeholder="10-digit number" /></div>
      </div>

      <!-- RIGHT COLUMN -->
      <div class="col">
        <div class="field"><label class="required">Village Name</label><input id="villageName" type="text" placeholder="Alpha-numeric" /></div>

        <div class="field">
          <label class="required">Size</label>
          <div style="flex:1">
            <div class="grid-two">
              <div class="unit-input"><input id="sizeSqft" type="number" step="any" placeholder="Sq.ft" /><span class="unit-badge">Sq.ft</span></div>
              <div class="unit-input"><input id="sizeAcre" type="number" step="any" placeholder="Acre" /><span class="unit-badge">Acre</span></div>
              <div class="unit-input"><input id="sizeCent" type="number" step="any" placeholder="Cents" /><span class="unit-badge">Cents</span></div>
              <div class="unit-input"><input id="sizeGround" type="number" step="any" placeholder="Ground" /><span class="unit-badge">Ground</span></div>
            </div>
          </div>
        </div>

        <div class="field">
          <label class="required">Rate</label>
          <div style="flex:1;display:flex;gap:10px">
            <input id="rateValue" type="number" step="any" placeholder="Enter rate"/>
            <select id="rateUnit"><option value="sqft">Per Sq.ft</option><option value="acre">Per Acre</option><option value="cent">Per Cent</option><option value="ground">Per Ground</option></select>
          </div>
        </div>

        <div class="field"><label class="required">Overall Budget</label>
          <div class="unit-input"><input id="overallBudget" type="text" readonly /><span class="unit-badge">₹</span></div>
        </div>

        <div class="field"><label class="required">Property Sourcing</label>
          <select id="sourcing">
            <option value="">-- Select --</option>
            <option>Mr.AGS Sir</option><option>Mr.Anand Sir</option><option>Mr.Srikanth Sir</option>
            <option>Management</option><option>Mrs.Poonguzhali Narendiran Mam</option><option>Mr.Idhayadulla Sir</option>
            <option>Mr.Parthiban Sir</option><option>Mr.Subash Sir</option><option>Mr.Arun Sir</option>
            <option>Mr.Sarath Sir</option><option>Mr.Anbuselvan Sir</option><option>Mr.Dhanasekar Sir</option>
          </select>
        </div>

        <div class="field"><label class="required">Date</label><input id="date" type="date" /></div>
      </div>
    </div>

    <!-- Map + Upload -->
    <div class="helpers">
      <div class="box" id="mapBox">Map View (embed map here)</div>
      <div class="box">
        <div style="width:100%">
          <div style="font-weight:700;margin-bottom:8px">Upload Files</div>
          <input id="fileUpload" type="file" multiple />
          <div class="upload-controls">
            <button class="btn" onclick="downloadFiles()">Download</button>
            <button class="btn" onclick="copyLink()">Copy Link</button>
          </div>
        </div>
      </div>
    </div>

    <div class="actions">
      <button class="btn" onclick="saveData()">Save Property</button>
      <button class="btn" onclick="backToDashboard()">Back to Dashboard</button>
    </div>

    <!-- Table -->
    <table id="propertyTable">
      <thead>
        <tr>
          <th>Location</th><th>Village</th><th>Layout</th><th>Plot No</th><th>Size</th><th>Rate</th><th>Mediator</th><th>Sourcing</th><th>Date</th><th>Edit</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </section>

<script>
  // Navigation
  function login(){
    const id=document.getElementById('loginId').value.trim();
    const pw=document.getElementById('password').value.trim();
    if(!id || !pw){ alert('Enter Login ID and Password'); return; }
    document.getElementById('loginScreen').style.display='none';
    document.getElementById('dashboard').style.display='block';
  }
  function openPage(pageId){
    document.getElementById('dashboard').style.display='none';
    if(pageId==='roughPlots'){
      document.getElementById('roughPlots').style.display='block';
      window.scrollTo({top:0,behavior:'smooth'});
    }
  }
  function backToDashboard(){
    document.getElementById('roughPlots').style.display='none';
    document.getElementById('dashboard').style.display='block';
  }

  // Conversion constants
  const ACRE_TO_SQFT=43560, CENT_TO_SQFT=435.6, GROUND_TO_SQFT=2400;

  // Elements
  const els={
    sqft: document.getElementById('sizeSqft'),
    acre: document.getElementById('sizeAcre'),
    cent: document.getElementById('sizeCent'),
    ground: document.getElementById('sizeGround'),
    rateValue: document.getElementById('rateValue'),
    rateUnit: document.getElementById('rateUnit'),
    budget: document.getElementById('overallBudget'),
    date: document.getElementById('date')
  };

  // Set default date to today
  (function(){
    const t=new Date();
    const y=t.getFullYear(), m=String(t.getMonth()+1).padStart(2,'0'), d=String(t.getDate()).padStart(2,'0');
    els.date.value=`${y}-${m}-${d}`;
  })();

  // Size conversion helpers
  let busy=false;
  function round(v){ return isFinite(v)? Math.round((v+Number.EPSILON)*100)/100 : ''; }
  function toSqftFrom(which,val){
    val = parseFloat(val) || 0;
    if(!val) return 0;
    if(which==='sqft') return val;
    if(which==='acre') return val*ACRE_TO_SQFT;
    if(which==='cent') return val*CENT_TO_SQFT;
    if(which==='ground') return val*GROUND_TO_SQFT;
    return 0;
  }
  function fromSqft(sqft){
    if(!sqft){ els.acre.value=''; els.cent.value=''; els.ground.value=''; return; }
    els.acre.value = round(sqft/ACRE_TO_SQFT);
    els.cent.value = round(sqft/CENT_TO_SQFT);
    els.ground.value = round(sqft/GROUND_TO_SQFT);
  }
  function handleSizeInput(which){
    if(busy) return; busy=true;
    const v = document.getElementById({sqft:'sizeSqft',acre:'sizeAcre',cent:'sizeCent',ground:'sizeGround'}[which]).value;
    const sqft = toSqftFrom(which, v);
    if(sqft>0){
      if(which!=='sqft') els.sqft.value = round(sqft);
      fromSqft(sqft);
    } else {
      // clear others if input cleared
      if(which==='sqft'){ els.acre.value=els.cent.value=els.ground.value=''; }
      if(which==='acre'){ els.sqft.value=els.cent.value=els.ground.value=''; }
      if(which==='cent'){ els.sqft.value=els.acre.value=els.ground.value=''; }
      if(which==='ground'){ els.sqft.value=els.acre.value=els.cent.value=''; }
    }
    computeBudget();
    busy=false;
  }

  ['sqft','acre','cent','ground'].forEach(k=>{
    document.getElementById('size'+ (k==='sqft'?'Sqft': k.charAt(0).toUpperCase()+k.slice(1))).addEventListener('input', ()=>handleSizeInput(k));
  });

  // Rate conversion to per sqft
  function ratePerSqft(){
    const rate = parseFloat(els.rateValue.value) || 0;
    const unit = els.rateUnit.value;
    if(!rate) return 0;
    if(unit==='sqft') return rate;
    if(unit==='acre') return rate / ACRE_TO_SQFT;
    if(unit==='cent') return rate / CENT_TO_SQFT;
    if(unit==='ground') return rate / GROUND_TO_SQFT;
    return 0;
  }
  function computeBudget(){
    const sqft = parseFloat(els.sqft.value) || 0;
    const rps = ratePerSqft();
    const total = sqft * rps;
    els.budget.value = total ? Number(total.toFixed(2)).toLocaleString('en-IN') : '';
  }
  els.rateValue.addEventListener('input', computeBudget);
  els.rateUnit.addEventListener('change', computeBudget);

  // Upload helpers (temporary object URLs)
  let lastObjectURL = '';
  function downloadFiles(){
    const inp = document.getElementById('fileUpload');
    if(!inp.files || !inp.files.length){ alert('Choose a file first'); return; }
    const file = inp.files[0];
    const url = URL.createObjectURL(file);
    const a = document.createElement('a');
    a.href = url; a.download = file.name; document.body.appendChild(a); a.click(); a.remove();
    lastObjectURL = url;
  }
  function copyLink(){
    const inp = document.getElementById('fileUpload');
    if(!inp.files || !inp.files.length){ alert('Choose a file first'); return; }
    if(!lastObjectURL) lastObjectURL = URL.createObjectURL(inp.files[0]);
    navigator.clipboard.writeText(lastObjectURL).then(()=>alert('Temporary link copied (valid while tab open).'));
  }

  // Validation helpers
  function isTenDigits(s){ return /^\d{10}$/.test(s); }

  // Save to table
  function fmtDate(iso){ if(!iso) return ''; const [y,m,d]=iso.split('-'); return `${d}/${m}/${y}`; }

  function saveData(){
    // required fields check
    const required = [
      ['location','Select Location'],
      ['layoutName','Enter Layout Name'],
      ['plotNo','Enter Plot No'],
      ['mediatorName','Enter Mediator Name'],
      ['mediatorContact','Enter Mediator Contact (10 digits)'],
      ['sellerName','Enter Seller Name'],
      ['sellerContact','Enter Seller Contact (10 digits)'],
      ['villageName','Enter Village Name'],
      ['rateValue','Enter Rate Value'],
      ['sourcing','Select Property Sourcing'],
      ['date','Choose Date']
    ];
    for(const [id,msg] of required){
      const el = document.getElementById(id);
      if(!el.value){ alert(msg); el.focus(); return; }
    }
    if(!isTenDigits(document.getElementById('mediatorContact').value)){ alert('Mediator Contact must be 10 digits'); return; }
    if(!isTenDigits(document.getElementById('sellerContact').value)){ alert('Seller Contact must be 10 digits'); return; }
    if(!els.sqft.value){ alert('Enter size in any unit'); return; }

    const tbody = document.querySelector('#propertyTable tbody');
    const tr = document.createElement('tr');

    const rateDisp = `${Number(document.getElementById('rateValue').value).toLocaleString('en-IN')} / ${document.getElementById('rateUnit').selectedOptions[0].textContent.replace('Per ','')}`;
    const cells = [
      document.getElementById('location').value,
      document.getElementById('villageName').value,
      document.getElementById('layoutName').value,
      document.getElementById('plotNo').value,
      `${Number(els.sqft.value).toLocaleString('en-IN')} Sq.ft`,
      rateDisp,
      document.getElementById('mediatorName').value,
      document.getElementById('sourcing').value,
      fmtDate(document.getElementById('date').value)
    ];

    cells.forEach((c,i)=>{ const td=document.createElement('td'); td.textContent=c; tr.appendChild(td); });
    const tdEdit=document.createElement('td'); tdEdit.className='edit'; tdEdit.textContent='✏️'; tdEdit.title='Edit'; tdEdit.addEventListener('click', ()=>editRow(tr)); tr.appendChild(tdEdit);
    tbody.appendChild(tr);
    alert('Property saved to list.');
  }

  // Simple edit row (populate form with row values for editing)
  function editRow(tr){
    const tds = tr.querySelectorAll('td');
    // populate fields
    document.getElementById('location').value = tds[0].textContent;
    document.getElementById('villageName').value = tds[1].textContent;
    document.getElementById('layoutName').value = tds[2].textContent;
    document.getElementById('plotNo').value = tds[3].textContent;

    // size cell contains e.g. "17,000 Sq.ft" -> extract numeric
    const sizeText = tds[4].textContent.replace(/,| Sq\.ft/g,'');
    document.getElementById('sizeSqft').value = Number(sizeText) || '';
    handleSizeInput('sqft');

    // rate cell e.g. "1,00,000 / Acre" -> split
    const rateParts = tds[5].textContent.split('/').map(s=>s.trim());
    if(rateParts.length>=2){
      const value = rateParts[0].replace(/,/g,'');
      const unitText = rateParts[1];
      document.getElementById('rateValue').value = value;
      // map unit text to select value
      const map = {'Sq.ft':'sqft','Sq.ft':'sqft','Acre':'acre','Cent':'cent','Ground':'ground'};
      // determine select by matching
      const sel = Array.from(document.getElementById('rateUnit').options).find(o=>o.text.includes(unitText));
      if(sel) document.getElementById('rateUnit').value = sel.value;
    }

    document.getElementById('mediatorName').value = tds[6].textContent;
    document.getElementById('sourcing').value = tds[7].textContent;
    // date dd/mm/yyyy -> convert back to yyyy-mm-dd if possible
    const dateParts = tds[8].textContent.split('/');
    if(dateParts.length===3){ document.getElementById('date').value = `${dateParts[2]}-${dateParts[1].padStart(2,'0')}-${dateParts[0].padStart(2,'0')}`; }

    // remove the row being edited
    tr.remove();
    window.scrollTo({top:0,behavior:'smooth'});
  }
</script>
</body>
</html>
"""
