[Smart Bin.html](https://github.com/user-attachments/files/26126138/Smart.Bin.html)
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart Bin | Mega Rewards v.8</title>
<link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<style>
  :root {
    --primary-green: #00b894; 
    --dark-grey: #2d3436;
    --light-bg: #f4f7f6;
    --white: #ffffff;
    --accent-gold: #f1c40f;
    --mint: #e8f8f0;
  }

  * { box-sizing: border-box; font-family: 'Kanit', sans-serif; transition: 0.3s cubic-bezier(0.4, 0, 0.2, 1); }
  body { background: #dfe6e9; display: flex; justify-content: center; align-items: center; min-height: 100vh; margin: 0; }

  .kiosk-container {
    width: 950px; height: 800px; background: var(--white); 
    border-radius: 40px; box-shadow: 0 30px 60px rgba(0,0,0,0.12);
    display: flex; flex-direction: column; overflow: hidden; position: relative;
    border: 1px solid #eee;
  }

  .top-bar { height: 10px; background: #eee; display: flex; }
  .progress { height: 100%; background: var(--primary-green); width: 25%; }

  .nav-header { padding: 25px 45px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #f1f1f1; }
  .brand { font-size: 28px; font-weight: 800; color: var(--primary-green); letter-spacing: -1.5px; }
  .brand span { color: var(--dark-grey); }
  .user-badge { background: var(--mint); padding: 10px 20px; border-radius: 50px; font-size: 14px; color: var(--dark-grey); display: none; border: 1px solid var(--primary-green); }

  .state { display: none; flex: 1; padding: 20px 45px; flex-direction: column; animation: fadeIn 0.5s ease; }
  .state.active { display: flex; }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }

  /* ----------------------------------------------------------- */
  /* --- แก้ไขเฉพาะหน้าปก (STATE 1) เพื่อความสวยงาม --- */
  /* ----------------------------------------------------------- */
  .hero-section {
    flex: 1; display: flex; flex-direction: column; align-items: center; justify-content: center;
    text-align: center; position: relative; padding: 40px;
  }
  .hero-icon-3d { font-size: 130px; margin-bottom: 20px; animation: float 3s ease-in-out infinite; }
  
  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-15px); }
  }

  .hero-title {
    font-size: 54px; font-weight: 800; color: var(--dark-grey); 
    line-height: 1.1; margin: 0 0 15px 0; letter-spacing: -1px;
  }
  .hero-title span { color: var(--primary-green); }

  .hero-subtitle {
    font-size: 22px; color: #636e72; font-weight: 300; max-width: 600px; margin: 0 0 40px 0;
  }

  .promo-banner {
    background: var(--mint); color: var(--primary-green); padding: 15px 30px;
    border-radius: 50px; font-weight: 600; font-size: 16px; margin-bottom: 40px;
    border: 1px solid rgba(0, 184, 148, 0.3); box-shadow: 0 5px 15px rgba(0, 184, 148, 0.1);
  }

  .btn-hero {
    background: var(--primary-green); color: white; border: none; padding: 22px 70px;
    border-radius: 100px; font-size: 26px; font-weight: 600; cursor: pointer;
    box-shadow: 0 10px 25px rgba(0, 184, 148, 0.4); 
    text-transform: uppercase; letter-spacing: 1px;
  }
  .btn-hero:hover { background: #00a383; transform: scale(1.03) translateY(-2px); }
  .btn-hero:active { transform: scale(0.98); box-shadow: 0 5px 10px rgba(0, 184, 148, 0.2); }
  
  .eco-elements {
    position: absolute; font-size: 50px; opacity: 0.1;
  }

  /* --- จบส่วนแก้ไขหน้าปก --- */
  /* ----------------------------------------------------------- */

  /* Waste Grid */
  .waste-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-top: 10px; }
  .waste-card {
    background: var(--light-bg); padding: 15px; border-radius: 20px; text-align: center;
    cursor: pointer; border: 2px solid transparent;
  }
  .waste-card:hover { border-color: var(--primary-green); background: white; transform: translateY(-3px); }
  .waste-card .icon { font-size: 35px; margin-bottom: 5px; }
  .waste-card h4 { margin: 5px 0; font-size: 14px; color: var(--dark-grey); }
  .waste-card span { color: var(--primary-green); font-weight: bold; font-size: 15px; }

  /* Mega Rewards Market */
  .rewards-list { display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin-top: 10px; overflow-y: auto; padding-right: 5px; }
  .reward-item {
    border: 1px solid #eee; border-radius: 20px; padding: 15px; text-align: center;
    position: relative; background: white;
  }
  .reward-item:hover { box-shadow: 0 5px 15px rgba(0,0,0,0.05); }
  .tag { position: absolute; top: 8px; right: 8px; background: var(--accent-gold); color: #000; padding: 2px 8px; border-radius: 8px; font-size: 11px; font-weight: bold; }

  /* Controls */
  .btn-action { background: var(--dark-grey); color: white; border: none; padding: 15px 35px; border-radius: 15px; font-size: 18px; font-weight: 600; cursor: pointer; }
  .btn-green { background: var(--primary-green); box-shadow: 0 8px 15px rgba(0,184,148,0.2); }

  /* Modal */
  .overlay { position: absolute; top:0; left:0; width:100%; height:100%; background: rgba(0,0,0,0.85); display: none; align-items: center; justify-content: center; z-index: 100; }
  .modal { background: white; padding: 35px; border-radius: 30px; text-align: center; max-width: 420px; }

</style>
</head>
<body>

<div class="kiosk-container">
  <div class="top-bar"><div class="progress" id="pBar"></div></div>
  
  <div class="nav-header">
    <div class="brand">SMART<span>BIN.</span></div>
    <div class="user-badge" id="uBadge">👤 <span id="uPh">08x-xxx-xxxx</span> | <b id="uPts">540</b> pts</div>
  </div>

  <div id="s1" class="state active">
    <div class="hero-section">
        <div class="hero-icon-3d">♻️</div>
        
        <h1 class="hero-title">ทิ้งขยะวันนี้<br>รับแต้มแลก<span>ของรางวัลฟรี!</span></h1>
        
        <p class="hero-subtitle">ยกระดับการแยกขยะของคุณ ให้เป็นส่วนลดคาเฟ่,<br>คูปองร้านอาหาร และสินค้า Upcycling สุดพรีเมียม</p>
        
        <div class="promo-banner">
            🔥 <b>โปรโมชั่นพิเศษ:</b> ทิ้งขยะไอที (E-Waste) วันนี้ รับแต้ม x2 ทันที!
        </div>
        
        <button class="btn-hero" onclick="go(2, '40%')">เริ่มต้นใช้งาน</button>
        
        <div class="eco-elements" style="top:50px; left:50px;">🌿</div>
        <div class="eco-elements" style="bottom:50px; right:50px;">🌍</div>
        <div class="eco-elements" style="top:150px; right:100px; font-size:30px; opacity:0.05">🍾</div>
        <div class="eco-elements" style="bottom:150px; left:100px; font-size:30px; opacity:0.05">🥫</div>
    </div>
  </div>

  <div id="s2" class="state" style="align-items:center; justify-content:center;">
    <h1>ระบุเบอร์โทรศัพท์</h1>
    <div id="phDisp" style="font-size: 50px; font-weight: bold; color: var(--primary-green); margin: 15px 0; letter-spacing: 5px;">0__-___-____</div>
    <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px;">
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('1')">1</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('2')">2</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('3')">3</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('4')">4</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('5')">5</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('6')">6</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('7')">7</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('8')">8</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('9')">9</button>
        <button class="btn-action" style="background:#ffeaa7; color:#d35400;" onclick="clrPh()">C</button>
        <button class="btn-action" style="background:#f1f1f1; color:#333;" onclick="addPh('0')">0</button>
        <button class="btn-action btn-green" onclick="confirmLogin()">ตกลง</button>
    </div>
  </div>

  <div id="s3" class="state">
    <div style="display:flex; justify-content:space-between; align-items:center;">
        <h2>ทิ้งขยะลงช่องรับ</h2>
        <div style="background:var(--dark-grey); color:white; padding:8px 20px; border-radius:15px;">แต้มรอบนี้: <b id="sessionPts" style="color:var(--primary-green); font-size: 20px;">0</b></div>
    </div>
    <div class="waste-grid">
        <div class="waste-card" onclick="addPts('ขวดพลาสติก', 15)">
            <div class="icon">🍾</div><h4>ขวด PET</h4><span>+15 pts</span>
        </div>
        <div class="waste-card" onclick="addPts('กระป๋องอลูมิเนียม', 20)">
            <div class="icon">🥫</div><h4>กระป๋อง</h4><span>+20 pts</span>
        </div>
        <div class="waste-card" onclick="addPts('ขวดแก้ว', 25)">
            <div class="icon">🍷</div><h4>ขวดแก้ว</h4><span>+25 pts</span>
        </div>
        <div class="waste-card" onclick="addPts('E-Waste', 50)">
            <div class="icon">🔋</div><h4>ถ่าน/ไอที</h4><span>+50 pts</span>
        </div>
        <div class="waste-card" onclick="addPts('กระดาษ/ลัง', 10)">
            <div class="icon">📦</div><h4>กระดาษ/ลัง</h4><span>+10 pts</span>
        </div>
        <div class="waste-card" onclick="addPts('กล่อง UHT', 12)">
            <div class="icon">🧃</div><h4>กล่อง UHT</h4><span>+12 pts</span>
        </div>
        <div class="waste-card" onclick="addPts('แก้วพลาสติก', 8)">
            <div class="icon">🥤</div><h4>แก้วกาแฟ</h4><span>+8 pts</span>
        </div>
        <div class="waste-card" onclick="addPts('หลอด', 5)">
            <div class="icon">🎋</div><h4>หลอด</h4><span>+5 pts</span>
        </div>
    </div>
    <button class="btn-action btn-green" style="margin-top:20px; width:100%;" onclick="go(4, '100%')">ยืนยันและดูของรางวัล</button>
  </div>

  <div id="s4" class="state">
    <h2>แลกของรางวัลพิเศษ</h2>
    <div class="rewards-list">
        <div class="reward-item">
            <span class="tag">150 pts</span>
            <div style="font-size:30px;">☕</div>
            <h4 style="margin:5px 0;">Amazon Discount 20.-</h4>
            <button class="btn-action" style="padding:5px 12px; font-size:13px;" onclick="claim('Amazon 20THB')">แลกรับสิทธิ์</button>
        </div>
        <div class="reward-item">
            <span class="tag">300 pts</span>
            <div style="font-size:30px;">🍔</div>
            <h4 style="margin:5px 0;">คูปอง McDonald's 50.-</h4>
            <button class="btn-action" style="padding:5px 12px; font-size:13px;" onclick="claim('McD 50THB')">แลกรับสิทธิ์</button>
        </div>
        <div class="reward-item">
            <span class="tag">500 pts</span>
            <div style="font-size:30px;">🚇</div>
            <h4 style="margin:5px 0;">เติมเงิน MRT/BTS 50.-</h4>
            <button class="btn-action" style="padding:5px 12px; font-size:13px;" onclick="claim('Metro 50THB')">แลกรับสิทธิ์</button>
        </div>
        <div class="reward-item">
            <span class="tag">600 pts</span>
            <div style="font-size:30px;">🚗</div>
            <h4 style="margin:5px 0;">Grab Discount 80.-</h4>
            <button class="btn-action" style="padding:5px 12px; font-size:13px;" onclick="claim('Grab 80THB')">แลกรับสิทธิ์</button>
        </div>
        <div class="reward-item">
            <span class="tag">1000 pts</span>
            <div style="font-size:30px;">🛍️</div>
            <h4 style="margin:5px 0;">กระเป๋าผ้า Upcycling</h4>
            <button class="btn-action" style="padding:5px 12px; font-size:13px;" onclick="claim('Eco Bag')">แลกรับสิทธิ์</button>
        </div>
        <div class="reward-item">
            <span class="tag">สะสม</span>
            <div style="font-size:30px;">📱</div>
            <h4 style="margin:5px 0;">โอนแต้มเข้าแอป (QR)</h4>
            <button class="btn-action btn-green" style="padding:5px 12px; font-size:13px;" onclick="claim('Transfer All')">โอนทันที</button>
        </div>
    </div>
    <div style="margin-top:20px; display:flex; gap:10px; justify-content:center;">
        <button class="btn-action" style="padding: 10px 30px;" onclick="reset()">จบงาน</button>
    </div>
  </div>

  <div class="overlay" id="qrOverlay">
    <div class="modal">
        <h2 id="qTitle">สำเร็จ!</h2>
        <div id="qrcode"></div>
        <p style="margin:15px 0; color:#666;">รหัสถูกส่งไปที่เบอร์คุณแล้ว<br>สามารถใช้งานได้ทันที</p>
        <button class="btn-action btn-green" onclick="document.getElementById('qrOverlay').style.display='none'">ปิด</button>
    </div>
  </div>

</div>

<script>
  let ph = ""; let sPts = 0; let totalPts = 540;
  const qr = new QRCode(document.getElementById("qrcode"), { width: 180, height: 180 });

  function go(s, p) {
    document.querySelectorAll('.state').forEach(el => el.classList.remove('active'));
    document.getElementById('s' + s).classList.add('active');
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
        document.getElementById('uPh').innerText = ph.replace(/(\d{3})(\d{3})(\d{4})/, "$1-XXX-XXXX");
        document.getElementById('uBadge').style.display = "block";
        go(3, '70%');
    } else { alert("กรุณากรอกเบอร์ให้ครบ"); }
  }

  function addPts(type, p) {
    sPts += p;
    document.getElementById('sessionPts').innerText = sPts;
    const card = event.currentTarget;
    card.style.background = "#e8f8f0";
    setTimeout(() => card.style.background = "var(--light-bg)", 250);
  }

  function claim(reward) {
    qr.clear();
    qr.makeCode(`USER:${ph}|REWARD:${reward}|PTS_EARNED:${sPts}`);
    document.getElementById('qTitle').innerText = reward === 'Transfer All' ? "โอนแต้มเรียบร้อย" : "แลกสิทธิ์สำเร็จ";
    document.getElementById('qrOverlay').style.display = "flex";
  }

  function reset() {
    ph = ""; sPts = 0; updatePh();
    document.getElementById('sessionPts').innerText = "0";
    document.getElementById('uBadge').style.display = "none";
    go(1, '25%');
  }
</script>

</body>
</html>
