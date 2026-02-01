<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>سوق عزان الموحد 🛒</title>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@500;800;900&display=swap" rel="stylesheet">
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    
    <style>
        :root { --primary: #004d40; --orange: #ff6d00; --bg: #f0f4f7; --text: #2c3e50; }
        body { font-family: 'Tajawal', sans-serif; background: var(--bg); margin: 0; padding: 0; color: var(--text); overflow-x: hidden; }
        .container { max-width: 800px; margin: auto; min-height: 100vh; background: #fdfdfd; box-shadow: 0 0 20px rgba(0,0,0,0.05); }
        .header { background: linear-gradient(135deg, var(--primary), #00796b); color: white; padding: 40px 20px; text-align: center; border-radius: 0 0 40px 40px; }
        .header h1 { margin: 0; font-size: 1.8rem; font-weight: 900; }
        .main-content { padding: 0 20px; margin-top: -30px; }
        .section-card { background: white; border-radius: 25px; padding: 20px; box-shadow: 0 8px 25px rgba(0,0,0,0.06); margin-bottom: 20px; border: 1px solid #eee; }
        .grid-small { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 12px; }
        .offer-box { background: white; border-radius: 20px; padding: 15px; text-align: center; cursor: pointer; transition: 0.3s; border: 1px solid #eee; }
        .offer-box:active { transform: scale(0.95); }
        .offer-box b { font-weight: 800; display: block; margin-top: 8px; font-size: 0.9rem; }
        .status-tag { font-size: 0.7rem; font-weight: 800; padding: 3px 8px; border-radius: 8px; margin-top: 8px; display: inline-block; }
        .has { background: #e8f5e9; color: #2e7d32; }
        .none { background: #f5f5f5; color: #9e9e9e; }
        .m-btn { width: 100%; padding: 18px; border-radius: 15px; border: none; background: var(--primary); color: white; font-family: 'Tajawal'; font-weight: 900; font-size: 1.1rem; cursor: pointer; transition: 0.2s; }
        .m-btn:hover { opacity: 0.9; }
        .full-page { display: none; position: fixed; top: 0; left: 50%; transform: translateX(-50%); width: 100%; max-width: 800px; height: 100%; background: var(--bg); z-index: 9999; overflow-y: auto; }
        .top-nav { background: white; padding: 15px; display: flex; align-items: center; gap: 15px; position: sticky; top: 0; box-shadow: 0 2px 10px rgba(0,0,0,0.05); z-index: 10; }
        .back-btn { background: #eee; border: none; width: 40px; height: 40px; border-radius: 50%; cursor: pointer; font-size: 1.2rem; }
        .directory-list { display: flex; overflow-x: auto; gap: 10px; padding: 10px 0; scrollbar-width: none; }
        .dir-pill { background: var(--primary); color: white; padding: 10px 20px; border-radius: 25px; font-weight: 800; white-space: nowrap; cursor: pointer; }
        .product-card { background: white; margin: 10px 0; padding: 15px; border-radius: 20px; border-right: 6px solid #ddd; box-shadow: 0 3px 10px rgba(0,0,0,0.05); }
        input, select { width: 100%; padding: 15px; margin-bottom: 10px; border-radius: 12px; border: 1.5px solid #ddd; font-family: 'Tajawal'; font-weight: 700; box-sizing: border-box; outline: none; }
        input:focus { border-color: var(--primary); }
    </style>
</head>
<body>

    <audio id="waterSnd" preload="auto"><source src="https://www.soundjay.com/nature/sounds/water-droplet-1.mp3"></audio>

    <div class="container">
        <div class="header">
            <h1>سوق عزان الموحد 🛒</h1>
            <p>دليلك الذكي للتسوق والمسابقات</p>
        </div>

        <div class="main-content">
            <div id="winnerDiv" class="section-card" style="display:none; background: #fff9c4; border: 2px solid #f1c40f; text-align: center;">
                🏆 <b style="color: #d35400;">فائز اليوم:</b> <span id="wName" style="font-weight: 900;"></span>
            </div>

            <div id="ramadanSection" class="section-card" style="border: 3px solid var(--orange); display: none;">
                <div style="color: var(--orange); font-weight: 900; margin-bottom: 10px;">🌙 مسابقة رمضان</div>
                <p id="qDisplay" style="font-weight: 800;"></p>
                <button onclick="sendAns()" class="m-btn" style="background: #25D366;">إرسال الحل عبر واتساب ✅</button>
            </div>

            <div class="section-card">
                <div style="font-weight: 900; margin-bottom: 15px;">🔥 تخفيضات الأقسام</div>
                <div class="grid-small">
                    <div class="offer-box" onclick="openOffers('مطاعم وكفتيريا', '🍔')"><span>🍔</span><b>المطاعم</b><span id="st-مطاعم وكفتيريا" class="status-tag none">...</span></div>
                    <div class="offer-box" onclick="openOffers('مواد غذائية', '🍎')"><span>🍎</span><b>البقالات</b><span id="st-مواد غذائية" class="status-tag none">...</span></div>
                    <div class="offer-box" onclick="openOffers('مواد بناء وسباكة', '🏗️')"><span>🏗️</span><b>البناء</b><span id="st-مواد بناء وسباكة" class="status-tag none">...</span></div>
                    <div class="offer-box" onclick="openOffers('مواد كهرباء', '⚡')"><span>⚡</span><b>الكهرباء</b><span id="st-مواد كهرباء" class="status-tag none">...</span></div>
                    <div class="offer-box" onclick="openOffers('سلع متنوعة وأخرى', '🛒')"><span>🛒</span><b>أخرى</b><span id="st-سلع متنوعة وأخرى" class="status-tag none">...</span></div>
                </div>
            </div>

            <div class="section-card">
                <div style="font-weight: 900;">📂 دليل المحلات</div>
                <div class="directory-list">
                    <div class="dir-pill" onclick="openDir('مطاعم وكفتيريا')">🏘️ المطاعم</div>
                    <div class="dir-pill" onclick="openDir('مواد غذائية')">🍱 البقالات</div>
                    <div class="dir-pill" onclick="openDir('مواد بناء وسباكة')">🛠️ البناء</div>
                    <div class="dir-pill" onclick="openDir('مواد كهرباء')">💡 الكهرباء</div>
                    <div class="dir-pill" onclick="openDir('سلع متنوعة وأخرى')">🏷️ أخرى</div>
                </div>
            </div>

            <button onclick="openPage('merchantPage')" class="m-btn" style="background: #37474f; margin-bottom: 40px;">🔐 لوحة التاجر</button>
        </div>
    </div>

    <div id="offersPage" class="full-page">
        <div class="top-nav"><button class="back-btn" onclick="closePage('offersPage')">✕</button><h3 id="pgTitle" style="margin:0; font-weight:900;"></h3></div>
        <div id="offersList" style="padding:15px;"></div>
    </div>

    <div id="dirPage" class="full-page">
        <div class="top-nav"><button class="back-btn" onclick="closePage('dirPage')">✕</button><h3 id="dirTitle" style="margin:0; font-weight:900;"></h3></div>
        <div style="padding: 15px;">
            <input type="text" id="searchInp" placeholder="بحث عن اسم محل..." onkeyup="searchStores()">
            <div id="dirList"></div>
        </div>
    </div>

    <div id="merchantPage" class="full-page">
        <div class="top-nav"><button class="back-btn" onclick="closePage('merchantPage')">✕</button><h3 style="margin:0; font-weight:900;">إدارة المحل</h3></div>
        <div id="mAuth" style="padding:30px;">
            <input type="password" id="mCode" placeholder="كود المحل الخاص">
            <button class="m-btn" onclick="loginMerchant()">دخول</button>
        </div>
        <div id="mPanel" style="display:none; padding:20px;">
            <div id="mNameDisp" style="padding:15px; background:var(--primary); color:white; border-radius:15px; margin-bottom:15px; text-align:center; font-weight:900;"></div>
            <input type="text" id="pName" placeholder="اسم المنتج">
            <input type="text" id="pPrice" placeholder="السعر">
            <select id="pType"><option value="offer">🔥 تخفيض</option><option value="normal">📦 عادي</option></select>
            <button class="m-btn" onclick="addProduct()">نشر الآن ✅</button>
            <hr style="margin:20px 0; border:0; border-top:1px solid #ddd;">
            <div style="font-weight: 800; margin-bottom: 10px;">منتجاتك الحالية:</div>
            <div id="mProds"></div>
            <button onclick="logout()" style="width:100%; background:none; border:none; color:red; margin-top:30px; font-weight:900; cursor:pointer;">تسجيل الخروج</button>
        </div>
    </div>

    <script>
        // إعدادات Firebase
        const firebaseConfig = { databaseURL: "https://souq-tarim-default-rtdb.firebaseio.com/" };
        if (!firebase.apps.length) firebase.initializeApp(firebaseConfig);
        const db = firebase.database();

        // وظائف الواجهة
        function playWater() { const snd = document.getElementById('waterSnd'); snd.currentTime = 0; snd.play().catch(e=>{}); }
        function openPage(id) { playWater(); document.getElementById(id).style.display='block'; if(id==='merchantPage') checkAuth(); }
        function closePage(id) { playWater(); document.getElementById(id).style.display='none'; }

        // مزامنة المسابقة والجوائز
        db.ref('RamadanContest').on('value', snap => {
            const d = snap.val(); if(!d) return;
            document.getElementById('ramadanSection').style.display = d.status === 'on' ? 'block' : 'none';
            document.getElementById('qDisplay').innerText = d.question;
            if(d.winStatus === 'on' && d.winner) {
                document.getElementById('winnerDiv').style.display = 'block';
                document.getElementById('wName').innerText = d.winner;
            } else {
                document.getElementById('winnerDiv').style.display = 'none';
            }
        });

        // تحديث حالة وجود عروض في الصفحة الرئيسية
        function syncStatus() {
            const cats = ['مواد غذائية', 'مطاعم وكفتيريا', 'مواد بناء وسباكة', 'مواد كهرباء', 'سلع متنوعة وأخرى'];
            db.ref('Products').on('value', pSnap => {
                const prods = pSnap.val() || {};
                db.ref('Stores').once('value', sSnap => {
                    const stores = sSnap.val() || {};
                    cats.forEach(c => {
                        let hasOffer = false;
                        Object.keys(stores).forEach(sid => {
                            if(stores[sid].category === c && prods[sid]) {
                                Object.values(prods[sid]).forEach(p => { if(p.type==='offer') hasOffer = true; });
                            }
                        });
                        const el = document.getElementById('st-'+c);
                        if(el) {
                            el.innerText = hasOffer ? "✅ عروض" : "لا يوجد";
                            el.className = "status-tag " + (hasOffer ? "has" : "none");
                        }
                    });
                });
            });
        }
        syncStatus();

        // فتح العروض حسب القسم
        function openOffers(cat, icon) {
            playWater(); 
            document.getElementById('pgTitle').innerText = icon + " عروض " + cat;
            const list = document.getElementById('offersList'); 
            list.innerHTML = "<p style='text-align:center'>جاري البحث عن تخفيضات...</p>"; 
            openPage('offersPage');

            db.ref('Stores').orderByChild('category').equalTo(cat).once('value', sSnap => {
                const stores = sSnap.val() || {};
                db.ref('Products').once('value', pSnap => {
                    const prods = pSnap.val() || {}; 
                    list.innerHTML = "";
                    let count = 0;
                    Object.keys(stores).forEach(sid => {
                        if(prods[sid]) Object.values(prods[sid]).forEach(p => {
                            if(p.type==='offer') {
                                list.innerHTML += `<div class="product-card" style="border-right-color:var(--orange)">
                                    <b style="font-size:1.1rem">${p.name}</b> 
                                    <span style="float:left; color:var(--orange); font-weight:900;">${p.price}</span>
                                    <div style="margin-top:8px; font-size:0.8rem; color:#666">🏪 ${stores[sid].name}</div>
                                </div>`;
                                count++;
                            }
                        });
                    });
                    if(count === 0) list.innerHTML = "<p style='text-align:center; margin-top:40px; color:#999;'>نعتذر، لا توجد عروض حالياً في هذا القسم</p>";
                });
            });
        }

        // فتح دليل المحلات
        function openDir(cat) {
            playWater(); 
            document.getElementById('dirTitle').innerText = "دليل " + cat;
            const list = document.getElementById('dirList'); 
            list.innerHTML = "جاري التحميل..."; 
            openPage('dirPage');
            db.ref('Stores').orderByChild('category').equalTo(cat).once('value', snap => {
                list.innerHTML = "";
                if(!snap.exists()) { list.innerHTML = "لا توجد محلات مسجلة"; return; }
                snap.forEach(s => {
                    const data = s.val();
                    list.innerHTML += `<div class="product-card" onclick="window.open('tel:${data.phone}')">
                        <b>${data.name}</b><br>
                        <small>📞 اضغط للاتصال: ${data.phone}</small>
                    </div>`;
                });
            });
        }

        // وظيفة البحث
        function searchStores() {
            let filter = document.getElementById('searchInp').value.toLowerCase();
            let cards = document.querySelectorAll('#dirList .product-card');
            cards.forEach(card => {
                card.style.display = card.innerText.toLowerCase().includes(filter) ? "" : "none";
            });
        }

        // نظام إدارة التاجر
        function loginMerchant() {
            const code = document.getElementById('mCode').value;
            if(!code) return;
            db.ref('Stores/'+code).once('value', s => {
                if(s.exists()){ 
                    localStorage.setItem('storeKey', code); 
                    localStorage.setItem('storeName', s.val().name); 
                    checkAuth(); 
                    playWater(); 
                } else { alert("كود الدخول غير صحيح!"); }
            });
        }

        function checkAuth() {
            const key = localStorage.getItem('storeKey');
            if(key) { 
                document.getElementById('mAuth').style.display='none'; 
                document.getElementById('mPanel').style.display='block'; 
                document.getElementById('mNameDisp').innerText = "🏪 متجر: " + localStorage.getItem('storeName');
                loadMyProds(key);
            }
        }

        function addProduct() {
            const key = localStorage.getItem('storeKey');
            const name = document.getElementById('pName').value;
            const price = document.getElementById('pPrice').value;
            const type = document.getElementById('pType').value;
            if(!name || !price) { alert("أدخل البيانات"); return; }
            db.ref('Products/'+key).push({ name, price, type }).then(() => {
                document.getElementById('pName').value=""; 
                document.getElementById('pPrice').value=""; 
                playWater();
            });
        }

        function loadMyProds(key) {
            db.ref('Products/'+key).on('value', snap => {
                const list = document.getElementById('mProds'); list.innerHTML = "";
                snap.forEach(p => {
                    const item = p.val();
                    list.innerHTML += `<div style="padding:15px; border-bottom:1px solid #eee; display:flex; justify-content:space-between; align-items:center; background:white; margin-bottom:5px; border-radius:10px;">
                        <div><b>${item.name}</b> <br> <small style="color:var(--orange)">${item.price} (${item.type === 'offer' ? 'عروض' : 'عادي'})</small></div>
                        <button onclick="delP('${p.key}')" style="color:white; background:#e74c3c; border:none; padding:5px 10px; border-radius:8px; cursor:pointer;">حذف</button>
                    </div>`;
                });
            });
        }

        function delP(pid) { if(confirm("هل متأكد من حذف هذا المنتج؟")) db.ref('Products/'+localStorage.getItem('storeKey')+'/'+pid).remove(); }
        function logout() { localStorage.clear(); location.reload(); }
        function sendAns() { playWater(); window.open('https://wa.me/967776337604', '_blank'); }
    </script>
</body>
</html>
