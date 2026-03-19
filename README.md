# SmartBin[SMB.html](https://github.com/user-attachments/files/26125979/SMB.html)
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Smart Bin | Pro Fix v.9</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
  :root {
    --primary-green: #00b894; 
    --dark-grey: #2d3436;
    --light-bg: #f4f7f6;
    --white: #ffffff;
    --accent-gold: #f1c40f;
  }

  /* ป้องกันการค้างและช่วยให้สัมผัสแม่นยำ */
  * { 
    box-sizing: border-box; 
    font-family: 'Kanit', sans-serif;
    -webkit-tap-highlight-color: transparent;
    user-select: none; /* ป้องกันการเลือกข้อความขณะกด */
  }

  body { 
    background: #dfe6e9; 
    margin: 0; padding: 10px;
    display: flex; justify-content: center; align-items: center; 
    min-height: 100vh;
  }

  .kiosk-container {
    width: 100%; max-width: 900px; 
    height: 85vh; max-height: 750px; 
    background: var(--white); border-radius: 40px; 
    box-shadow: 0 30px 60px rgba(0,0,0,0.15);
    display: flex; flex-direction: column; overflow: hidden;
    position: relative; z-index: 1;
  }

  /* Header & Progress */
  .top-bar { height: 8px; background: #eee; width: 100%; }
  .progress { height: 100%; background: var(--primary-green); width: 25%; transition: 0.5s; }
  .nav-header { padding: 15px 30px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #eee; }
  .brand { font-size: 22px; font-weight: 800; color: var(--primary-green); }
  .user-badge { background: #e8f8f0; padding: 6px 15px; border-radius: 50px; font-size: 12px; display: none; }

  /* States */
  .state { 
    display: none; flex: 1; padding: 20px; 
    flex-direction: column; align-items: center; justify-content: center;
    animation: fadeIn 0.3s ease;
  }
  .state.active { display: flex; }
  @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }

  /* --- หน้าปก (State 1) ที่แก้ให้กดได้ 100% --- */
  .hero-icon { font-size: 80px; margin-bottom: 10px; pointer-events: none; }
  .hero-title { font-size: 36px; font-weight: 800; color: var(--dark-grey); text-align: center; margin: 0; line-height: 1.2; }
  .hero-title span { color: var(--primary-green); }
  .hero-subtitle { font-size: 18px; color: #636e72; text-align: center; margin: 15px 0 30px; }
  
  /* ปุ่มเริ่มต้นที่ขยายพื้นที่กด */
  .btn-hero {
    background: var(--primary-green); color: white; border: none; 
    padding: 25px 80px; border-radius: 100px; 
    font-size: 24px; font-weight: 600; 
    cursor: pointer; 
    box-shadow: 0 10px 20px rgba(0, 184, 148, 0.3);
    position: relative; z-index: 999; /* อยู่บนสุดเสมอ */
  }
  .btn-hero:active { transform: scale(0.95); opacity: 0.8; }

  /* Grid Items */
  .waste-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; width: 100%; }
  @media (min-width: 600px) { .waste-grid { grid-template-columns: repeat(4, 1fr); } }
  .waste-card { background: var(--light-bg); padding: 15px; border-radius: 20px; text-align: center; cursor: pointer; }
  .waste-card:active { background: #e8f8f0; }

  .rewards-list { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; width: 100%; }
  @media (min-width: 600px) { .rewards-list { grid-template-columns: repeat(3, 1fr); } }
  .reward-item { border: 1px solid #eee; border-radius: 20px; padding: 15px; text-align: center; position: relative; }
  .reward-item button { background: var(--dark-grey); color: white; border: none; padding: 8px 15px; border-radius: 10px; cursor: pointer; }

  /* Keypad */
  .keypad-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; width: 100%; max-width: 300px; }
  .num-key { height: 60px; background: #eee; border: none; border-radius: 15px; font-size: 22px; font-weight: 600; cursor: pointer; }
  .num-key:active { background: #ccc; }

  /* Overlay */
  .overlay { position: absolute; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.8); display: none; align-items: center; justify-content: center; z-index: 2000; }
  .modal { background: white; padding: 30px; border-radius: 30px; text-align: center; width: 85%; }
</style>
</head>
<body>

<div class="kiosk-container">
  <div class="top-bar"><div class="progress" id="pBar"></div></div>
  
  <div class="nav-header">
    <div class="brand">SMART<span>BIN.</span></div>
    <div class="user-badge" id="uBadge">👤 <span id="uPh">08x-xxx-xxxx</span> | <b id="uPts">0</b> pts</div>
  </div>

  <div id="s1" class="state active">
    <div class="hero-icon">♻️</div>
    <h1 class="hero-title">ทิ้งขยะวันนี้<br>รับแต้มแลก<span>ของรางวัลฟรี!</span></h1>
    <p class="hero-subtitle">เปลี่ยนขยะรีไซเคิลเป็นส่วนลดคาเฟ่<br>และของรางวัลสุดพรีเมียม</p>
    <button class="btn-hero" onclick="go(2, '40%')">เริ่มต้นใช้งาน</button>
  </div>

  <div id="s2" class="state">
    <h1 style="font-size:24px;">ระบุเบอร์โทรศัพท์</h1>
    <div id="phDisp" style="font-size: 45px; font-weight: bold; color: var(--primary-green); margin-bottom: 20px;">0__-___-____</div>
    <div class="keypad-grid">
        <button class="num-key" onclick="addPh('1')">1</button>
        <button class="num-key" onclick="addPh('2')">2</button>
        <button class="num-key" onclick="addPh('3')">3</button>
        <button class="num-key" onclick="addPh('4')">4</button>
        <button class="num-key" onclick="addPh('5')">5</button>
        <button class="num-key" onclick="addPh('6')">6</button>
        <button class="num-key" onclick="addPh('7')">7</button>
        <button class="num-key" onclick="addPh('8')">8</button>
        <button class="num-key" onclick="addPh('9')">9</button>
        <button class="num-key" style="background:#ffeaa7;" onclick="clrPh()">C</button>
        <button class="num-key" onclick="addPh('0')">0</button>
        <button class="num-key" style="background:var(--primary-green); color:white;" onclick="confirmLogin()">ตกลง</button>
    </div>
  </div>

  <div id="s3" class="state">
    <div style="display:flex; justify-content:space-between; align-items:center; width:100%; margin-bottom:15px;">
        <h2 style="margin:0; font-size:20px;">เลือกประเภทขยะ</h2>
        <div style="background:var(--dark-grey); color:white; padding:5px 15px; border-radius:10px;">สะสม: <b id="sessionPts" style="color:var(--primary-green)">0</b></div>
    </div>
    <div class="waste-grid">
        <div class="waste-card" onclick="addPts('PET', 15)"><div style="font-size:30px;">🍾</div><div>ขวดน้ำ</div><b style="color:var(--primary-green)">+15</b></div>
        <div class="waste-card" onclick="addPts('CAN', 20)"><div style="font-size:30px;">🥫</div><div>ป๋อง</div><b style="color:var(--primary-green)">+20</b></div>
        <div class="waste-card" onclick="addPts('GLASS', 25)"><div style="font-size:30px;">🍷</div><div>ขวดแก้ว</div><b style="color:var(--primary-green)">+25</b></div>
        <div class="waste-card" onclick="addPts('IT', 50)"><div style="font-size:30px;">🔋</div><div>ไอที</div><b style="color:var(--primary-green)">+50</b></div>
        <div class="waste-card" onclick="addPts('BOX', 10)"><div style="font-size:30px;">📦</div><div>กระดาษ</div><b style="color:var(--primary-green)">+10</b></div>
        <div class="waste-card" onclick="addPts('UHT', 12)"><div style="font-size:30px;">🧃</div><div>UHT</div><b style="color:var(--primary-green)">+12</b></div>
        <div class="waste-card" onclick="addPts('CUP', 8)"><div style="font-size:30px;">🥤</div><div>แก้วกแฟ</div><b style="color:var(--primary-green)">+8</b></div>
        <div class="waste-card" onclick="addPts('ST', 5)"><div style="font-size:30px;">🎋</div><div>หลอด</div><b style="color:var(--primary-green)">+5</b></div>
    </div>
    <button class="btn-hero" style="margin-top:20px; width:100%; font-size:18px; padding:15px;" onclick="go(4, '100%')">ยืนยัน</button>
  </div>

  <div id="s4" class="state">
    <h2 style="margin:0 0 15px 0; font-size:20px;">เลือกของรางวัล</h2>
    <div class="rewards-list">
        <div class="reward-item"><div style="font-size:24px;">☕</div><div>Amazon 20.-</div><button onclick="claim('AZ')">แลก</button></div>
        <div class="reward-item"><div style="font-size:24px;">🍔</div><div>McD 50.-</div><button onclick="claim('MC')">แลก</button></div>
        <div class="reward-item"><div style="font-size:24px;">🚇</div><div>MRT 50.-</div><button onclick="claim('MR')">แลก</button></div>
        <div class="reward-item"><div style="font-size:24px;">🚗</div><div>Grab 80.-</div><button onclick="claim('GB')">แลก</button></div>
        <div class="reward-item"><div style="font-size:24px;">🛍️</div><div>Eco Bag</div><button onclick="claim('BG')">แลก</button></div>
        <div class="reward-item"><div style="font-size:24px;">📱</div><div>เข้าแอป</div><button style="background:var(--primary-green);" onclick="claim('APP')">โอน</button></div>
    </div>
    <button class="btn-hero" style="margin-top:20px; width:100%; font-size:18px; padding:15px; background:var(--dark-grey);" onclick="reset()">กลับหน้าแรก</button>
  </div>

  <div class="overlay" id="qrOverlay">
    <div class="modal">
        <h2 id="qTitle" style="margin:0 0 10px 0;">เรียบร้อย!</h2>
        <div id="qrcode" style="display:flex; justify-content:center;"></div>
        <p style="margin:15px 0; color:#666;">รหัสส่งไปที่เบอร์คุณแล้ว</p>
        <button class="btn-hero" style="padding:10px 30px; font-size:16px;" onclick="document.getElementById('qrOverlay').style.display='none'">ปิด</button>
    </div>
  </div>
</div>

<script>
  let ph = ""; let sPts = 0;
  const qr = new QRCode(document.getElementById("qrcode"), { width: 160, height: 160 });

  function go(s, p) {
    document.querySelectorAll('.state').forEach(el => el.style.display = 'none');
    document.getElementById('s' + s).style.display = 'flex';
    document.getElementById('pBar').style.width = p;
  }

  function addPh(n) { if(ph.length < 10) ph += n; updatePh(); }
  function clrPh() { ph = ""; updatePh(); }
  function updatePh() { 
    let d = ph.padEnd(10, "_");
    document.getElementById('phDisp').innerText = d.replace(/(\d{3})(\d{3})(\d{4})/, "$1-$2-$3");
  }

  function confirmLogin() {
    if(ph.length === 10) {
        document.getElementById('uPh').innerText = ph.substring(0,3) + "-XXX-" + ph.substring(7);
        document.getElementById('uBadge').style.display = "block";
        go(3, '70%');
    } else { alert("กรุณากรอกเบอร์ให้ครบ"); }
  }

  function addPts(type, p) {
    sPts += p;
    document.getElementById('sessionPts').innerText = sPts;
    document.getElementById('uPts').innerText = sPts;
  }

  function claim(reward) {
    qr.clear();
    qr.makeCode("REWARD-" + reward + "-" + ph);
    document.getElementById('qTitle').innerText = reward === 'APP' ? "โอนแต้มสำเร็จ" : "แลกสิทธิ์สำเร็จ";
    document.getElementById('qrOverlay').style.display = "flex";
  }

  function reset() {
    ph = ""; sPts = 0; updatePh();
    document.getElementById('sessionPts').innerText = "0";
    document.getElementById('uPts').innerText = "0";
    document.getElementById('uBadge').style.display = "none";
    go(1, '25%');
  }
</script>

</body>
</html>
