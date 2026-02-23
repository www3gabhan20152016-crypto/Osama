
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    >
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.1.3/firebase-database-compat.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root { 
            --bg: #0b0f19; --card-bg: #161d2f; --primary: #fbbf24; 
            --text-main: #f8fafc; --text-dim: #94a3b8; --success: #10b981; --danger: #ef4444; --info: #3b82f6; --warning: #f59e0b;
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.3); --radius: 20px;
            --ym-red: #ed1c24; /* لون يمن موبايل */
            --notification-bg: #1e293b;
        }
        
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
        }
        
        body { 
            font-family: 'Cairo', sans-serif; 
            background-color: var(--bg); 
            color: var(--text-main); 
            margin: 0;
            padding-bottom: 90px;
            overflow-x: hidden;
        }
        
        /* شاشات الدخول والتسجيل */
        .auth-container { 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            justify-content: center; 
            min-height: 100vh; 
            padding: 20px; 
            background: linear-gradient(135deg, #0b0f19 0%, #1a1f33 100%);
        }
        
        .auth-card { 
            background: var(--card-bg); 
            padding: 30px 25px; 
            border-radius: var(--radius); 
            width: 100%; 
            max-width: 400px; 
            box-shadow: var(--shadow); 
            border: 1px solid rgba(251, 191, 36, 0.1);
            animation: fadeIn 0.5s ease-out;
        }
        
        .auth-card h2 { 
            text-align: center; 
            color: var(--primary); 
            margin-bottom: 25px;
            font-size: 1.5rem;
        }
        
        /* الهيدر والمحفظة */
        .royal-header { 
            background: linear-gradient(135deg, #1e293b 0%, #0f172a 100%); 
            padding: 30px 20px 70px; 
            border-radius: 0 0 50px 50px;
            position: relative;
            z-index: 10;
        }
        
        .wallet-gold-card { 
            background: linear-gradient(135deg, #fbbf24 0%, #d97706 100%); 
            margin: -50px 20px 20px; 
            padding: 25px; 
            border-radius: var(--radius); 
            color: #000; 
            display: flex; 
            justify-content: space-between; 
            align-items: center;
            box-shadow: var(--shadow);
            position: relative;
            z-index: 20;
            animation: slideUp 0.5s ease-out;
        }
        
        /* الحقول والأزرار */
        input, textarea, select { 
            background: #1e293b; 
            border: 1px solid rgba(255,255,255,0.1); 
            color: white; 
            padding: 16px; 
            border-radius: 15px; 
            width: 100%; 
            margin-top: 10px; 
            font-family: 'Cairo'; 
            outline: none;
            font-size: 1rem;
            transition: border-color 0.3s;
        }
        
        input:focus, textarea:focus, select:focus { 
            border-color: var(--primary); 
            box-shadow: 0 0 0 2px rgba(251, 191, 36, 0.2);
        }
        
        .btn-royal { 
            background: linear-gradient(90deg, #fbbf24, #f59e0b); 
            color: #000; 
            border: none; 
            padding: 18px; 
            border-radius: 15px; 
            width: 100%; 
            font-weight: 900; 
            cursor: pointer; 
            margin-top: 15px; 
            transition: all 0.3s;
            font-size: 1.1rem;
            box-shadow: 0 5px 15px rgba(251, 191, 36, 0.3);
            position: relative;
            overflow: hidden;
        }
        
        .btn-royal::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, rgba(255,255,255,0.2), rgba(255,255,255,0));
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .btn-royal:hover::after, .btn-royal:active::after {
            opacity: 1;
        }
        
        .btn-royal:disabled { 
            background: #334155; 
            color: #64748b; 
            cursor: not-allowed;
            box-shadow: none;
        }
        
        /* أزرار يمن موبايل */
        .btn-mobile {
            background: linear-gradient(135deg, var(--ym-red), #ff6b6b);
            color: white;
            box-shadow: 0 5px 15px rgba(237, 28, 36, 0.3);
        }
        
        /* الباقات والدردشة */
        .pkg-grid { 
            display: grid; 
            grid-template-columns: repeat(auto-fill, minmax(150px, 1fr)); 
            gap: 12px; 
            padding: 15px; 
        }
        
        .pkg-card { 
            background: var(--card-bg); 
            padding: 20px 15px; 
            border-radius: var(--radius); 
            text-align: center; 
            border: 1px solid rgba(255,255,255,0.05); 
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100px;
        }
        
        .pkg-card:hover {
            transform: translateY(-5px);
            box-shadow: var(--shadow);
        }
        
        .pkg-card.selected { 
            border: 2px solid var(--primary); 
            background: rgba(251, 191, 36, 0.05); 
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(251, 191, 36, 0.1);
        }
        
        /* كروت باقات يمن موبايل */
        .mobile-package-card {
            background: var(--card-bg); 
            padding: 15px; 
            border-radius: 15px; 
            text-align: center; 
            border: 1px solid rgba(255,255,255,0.05); 
            cursor: pointer;
            transition: all 0.3s;
            min-height: 90px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }
        
        .mobile-package-card:hover {
            transform: translateY(-3px);
            box-shadow: var(--shadow);
        }
        
        .mobile-package-card.selected { 
            border: 2px solid var(--ym-red); 
            background: rgba(237, 28, 36, 0.05); 
            transform: translateY(-3px);
        }
        
        .mobile-packages-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin: 10px 0;
        }
        
        .mobile-price-tag {
            display: inline-block;
            background: linear-gradient(135deg, var(--ym-red), #ff6b6b);
            color: white;
            padding: 3px 8px;
            border-radius: 10px;
            font-weight: bold;
            font-size: 0.8rem;
            margin-top: 5px;
        }
        
        .chat-area { 
            height: 50vh; 
            overflow-y: auto; 
            padding: 15px; 
            display: flex; 
            flex-direction: column; 
            gap: 12px; 
            background: rgba(0,0,0,0.2); 
            border-radius: var(--radius); 
            margin: 10px 0;
            scroll-behavior: smooth;
        }
        
        .msg { 
            padding: 12px 16px; 
            border-radius: 18px; 
            max-width: 85%; 
            font-size: 0.95rem;
            word-wrap: break-word;
            animation: fadeIn 0.3s ease-out;
            line-height: 1.4;
        }
        
        .msg.admin { 
            background: #334155; 
            align-self: flex-start;
            border-bottom-right-radius: 5px;
        }
        
        .msg.user { 
            background: var(--primary); 
            color: #000; 
            align-self: flex-end;
            border-bottom-left-radius: 5px;
            font-weight: bold;
        }
        
        .msg-time {
            font-size: 0.7rem;
            margin-top: 5px;
            opacity: 0.7;
            text-align: left;
        }
        
        /* التنقل السفلي */
        .nav-royal { 
            position: fixed; 
            bottom: 0; 
            left: 0; 
            right: 0; 
            background: rgba(22, 29, 47, 0.95); 
            backdrop-filter: blur(10px); 
            border-radius: 25px 25px 0 0; 
            display: flex; 
            justify-content: space-around; 
            padding: 12px 0;
            border-top: 1px solid rgba(255,255,255,0.1);
            z-index: 1000;
            box-shadow: 0 -5px 20px rgba(0, 0, 0, 0.3);
        }
        
        .nav-item { 
            color: var(--text-dim); 
            text-align: center; 
            font-size: 10px; 
            cursor: pointer;
            padding: 8px 12px;
            border-radius: 12px;
            transition: all 0.3s;
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }
        
        .nav-item:hover {
            background: rgba(255, 255, 255, 0.05);
        }
        
        .nav-item.active { 
            color: var(--primary);
            background: rgba(251, 191, 36, 0.1);
        }
        
        .nav-item b { 
            font-size: 22px; 
            display: block;
            margin-bottom: 4px;
        }
        
        .nav-badge {
            position: absolute;
            top: -5px;
            right: 5px;
            background: var(--danger);
            color: white;
            font-size: 10px;
            min-width: 18px;
            height: 18px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }
        
        /* تبويبات المحتوى */
        .tab-content {
            animation: fadeIn 0.3s ease-out;
        }
        
        /* حالات التحميل */
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255,255,255,.3);
            border-radius: 50%;
            border-top-color: var(--primary);
            animation: spin 1s ease-in-out infinite;
            margin-right: 10px;
        }
        
        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(11, 15, 25, 0.9);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 2000;
            color: white;
        }
        
        /* ========== الإشعارات الجديدة ========== */
        .notification-container {
            position: fixed;
            top: 20px;
            right: 20px;
            left: 20px;
            z-index: 3000;
            pointer-events: none;
        }
        
        .notification {
            background: var(--notification-bg);
            border-radius: var(--radius);
            padding: 15px;
            margin-bottom: 10px;
            box-shadow: var(--shadow);
            border-right: 4px solid var(--primary);
            animation: slideDown 0.5s ease-out;
            transform: translateY(-100px);
            opacity: 0;
            pointer-events: auto;
            display: flex;
            align-items: flex-start;
            gap: 12px;
        }
        
        .notification.show {
            transform: translateY(0);
            opacity: 1;
        }
        
        .notification.success {
            border-right-color: var(--success);
        }
        
        .notification.error {
            border-right-color: var(--danger);
        }
        
        .notification.info {
            border-right-color: var(--info);
        }
        
        .notification.warning {
            border-right-color: var(--warning);
        }
        
        .notification.mobile {
            border-right-color: var(--ym-red);
        }
        
        .notification-icon {
            font-size: 1.3rem;
            flex-shrink: 0;
        }
        
        .notification.success .notification-icon {
            color: var(--success);
        }
        
        .notification.error .notification-icon {
            color: var(--danger);
        }
        
        .notification.info .notification-icon {
            color: var(--info);
        }
        
        .notification.warning .notification-icon {
            color: var(--warning);
        }
        
        .notification.mobile .notification-icon {
            color: var(--ym-red);
        }
        
        .notification-content {
            flex: 1;
        }
        
        .notification-title {
            font-weight: 700;
            margin-bottom: 5px;
            font-size: 0.95rem;
        }
        
        .notification-message {
            color: var(--text-dim);
            font-size: 0.85rem;
            line-height: 1.4;
        }
        
        .notification-close {
            background: none;
            border: none;
            color: var(--text-dim);
            font-size: 1rem;
            cursor: pointer;
            padding: 0;
            flex-shrink: 0;
        }
        
        /* ========== تبويب الإشعارات ========== */
        .notifications-list {
            padding: 15px;
            max-height: 60vh;
            overflow-y: auto;
        }
        
        .notification-item {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 10px;
            border-right: 4px solid var(--primary);
            cursor: pointer;
            transition: all 0.3s;
            position: relative;
        }
        
        .notification-item:hover {
            transform: translateX(-5px);
            background: rgba(255, 255, 255, 0.05);
        }
        
        .notification-item.unread {
            border-right-color: var(--info);
            background: rgba(59, 130, 246, 0.05);
        }
        
        .notification-item.success {
            border-right-color: var(--success);
        }
        
        .notification-item.error {
            border-right-color: var(--danger);
        }
        
        .notification-item.warning {
            border-right-color: var(--warning);
        }
        
        .notification-item.mobile {
            border-right-color: var(--ym-red);
        }
        
        .notification-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 8px;
        }
        
        .notification-type {
            font-size: 0.8rem;
            padding: 3px 10px;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.1);
        }
        
        .notification-time {
            font-size: 0.7rem;
            color: var(--text-dim);
        }
        
        .notification-body {
            font-size: 0.9rem;
            margin-bottom: 5px;
        }
        
        .notification-details {
            font-size: 0.8rem;
            color: var(--text-dim);
        }
        
        .mark-read-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: var(--text-dim);
            padding: 5px 10px;
            border-radius: 10px;
            font-size: 0.7rem;
            cursor: pointer;
            margin-top: 10px;
        }
        
        /* كروت الطلبات */
        .order-card {
            background: var(--card-bg);
            padding: 18px;
            border-radius: var(--radius);
            margin-bottom: 12px;
            border-right: 4px solid var(--primary);
            animation: fadeIn 0.5s ease-out;
            transition: transform 0.3s;
        }
        
        .order-card:hover {
            transform: translateX(-5px);
        }
        
        .order-status {
            display: inline-block;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: bold;
            margin-top: 8px;
        }
        
        .status-waiting { background: rgba(255, 193, 7, 0.2); color: #ffc107; }
        .status-done { background: rgba(16, 185, 129, 0.2); color: var(--success); }
        .status-canceled { background: rgba(239, 68, 68, 0.2); color: var(--danger); }
        
        /* البحث */
        .search-box {
            position: relative;
            margin-bottom: 15px;
        }
        
        .search-box i {
            position: absolute;
            left: 15px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-dim);
        }
        
        .search-box input {
            padding-right: 15px;
            padding-left: 45px;
        }
        
        /* عناوين الأقسام */
        .section-title {
            color: var(--primary);
            border-right: 4px solid var(--primary);
            padding-right: 10px;
            margin: 20px 0 10px;
            font-size: 1rem;
        }
        
        .mobile-section-title {
            color: var(--ym-red);
            border-right: 4px solid var(--ym-red);
            padding-right: 10px;
            margin: 20px 0 10px;
            font-size: 1rem;
        }
        
        /* الرسوم المتحركة */
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes slideUp {
            from { transform: translateY(20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        @keyframes slideDown {
            from { transform: translateY(-100px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        /* تحسينات للجوال */
        @media (max-width: 480px) {
            .pkg-grid, .mobile-packages-grid {
                grid-template-columns: 1fr 1fr;
            }
            
            .auth-card {
                padding: 25px 20px;
            }
            
            .nav-item {
                font-size: 9px;
                padding: 6px 8px;
            }
            
            .nav-item b {
                font-size: 20px;
            }
            
            .wallet-gold-card {
                padding: 20px;
                margin: -50px 15px 15px;
            }
        }
        
        @media (max-width: 360px) {
            .pkg-grid, .mobile-packages-grid {
                grid-template-columns: 1fr;
            }
        }
        
        /* فائدة */
        .hidden { display: none !important; }
        .text-center { text-align: center; }
        .mb-10 { margin-bottom: 10px; }
        .mt-10 { margin-top: 10px; }
        .mb-20 { margin-bottom: 20px; }
        .mt-20 { margin-top: 20px; }
        .d-flex { display: flex; }
        .align-center { align-items: center; }
        .justify-between { justify-content: space-between; }
        .w-100 { width: 100%; }
        
        /* شريط التمرير */
        ::-webkit-scrollbar {
            width: 6px;
        }
        
        ::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.1);
            border-radius: 10px;
        }
        
        ::-webkit-scrollbar-thumb {
            background: rgba(251, 191, 36, 0.5);
            border-radius: 10px;
        }
        
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(251, 191, 36, 0.7);
        }
    </style>
</head>
<body>

<div id="auth-section" class="auth-container">
    <div id="login-box" class="auth-card">
        <h2><i class="fas fa-sign-in-alt"></i> تسجيل الدخول</h2>
        <input type="tel" id="l-phone" placeholder="رقم الهاتف">
        <input type="password" id="l-pass" placeholder="كلمة المرور">
        <button class="btn-royal" onclick="handleLogin()" id="login-btn">
            <span id="login-text">دخول</span>
            <span id="login-loading" class="hidden"><span class="loading"></span> جاري الدخول...</span>
        </button>
        <p class="text-center mt-20" onclick="toggleAuth('reg')">ليس لديك حساب؟ <b style="color:var(--primary)">افتح حساب جديد</b></p>
    </div>

    <div id="reg-box" class="auth-card hidden">
        <h2><i class="fas fa-user-plus"></i> حساب جديد</h2>
        <input type="text" id="r-name" placeholder="الاسم الكامل">
        <input type="tel" id="r-phone" placeholder="رقم الهاتف">
        <input type="password" id="r-pass" placeholder="كلمة المرور">
        <button class="btn-royal" onclick="handleRegister()" id="register-btn">
            <span id="register-text">تأكيد التسجيل</span>
            <span id="register-loading" class="hidden"><span class="loading"></span> جاري التسجيل...</span>
        </button>
        <p class="text-center mt-20" onclick="toggleAuth('login')">لديك حساب؟ <b style="color:var(--primary)">سجل دخولك</b></p>
    </div>
</div>

<div id="main-app" class="hidden">
    <!-- شاشة التحميل -->
    <div id="loading-screen" class="loading-overlay hidden">
        <span class="loading" style="width: 40px; height: 40px;"></span>
        <p class="mt-20">جاري تحميل البيانات...</p>
    </div>
    
    <!-- منطقة الإشعارات العائمة -->
    <div id="notification-container" class="notification-container"></div>
    
    <!-- الهيدر -->
    <div class="royal-header">
        <div class="d-flex justify-between align-center">
            <div>
                <span style="color:var(--text-dim); font-size:12px;">أهلاً بك</span><br>
                <b id="u-name-display">...</b>
                <div style="font-size: 0.8rem; color: var(--text-dim); margin-top: 2px;" id="last-login"></div>
            </div>
            <div style="display: flex; gap: 10px;">
                <button onclick="showTab('notifications')" style="background: transparent; border: none; color: white; position: relative;">
                    <i class="fas fa-bell" style="font-size: 1.3rem;"></i>
                    <span id="notification-badge" class="nav-badge hidden">0</span>
                </button>
                <button onclick="logout()" style="background:var(--danger); border:none; color:white; padding:8px 15px; border-radius:10px; font-family:'Cairo'; font-weight:bold;">
                    <i class="fas fa-sign-out-alt"></i> خروج
                </button>
            </div>
        </div>
    </div>

    <!-- محفظة المستخدم -->
    <div class="wallet-gold-card">
        <div>
            <span>رصيدك الملكي</span><br>
            <b><span id="u-balance">0</span> ريال</b>
            <div style="font-size: 0.8rem; margin-top: 5px;">آخر تحديث: <span id="balance-time">الآن</span></div>
        </div>
        <button onclick="showTab('deposit')" style="background:#000; color:#fbbf24; border:none; padding:12px 18px; border-radius:12px; font-weight:900; display:flex; align-items:center;">
            <i class="fas fa-plus-circle" style="margin-left: 5px;"></i> إيداع +
        </button>
    </div>

    <!-- تبويب الرئيسية -->
    <div id="tab-home" class="tab-content">
        <div style="padding: 0 20px;">
            <!-- اختيار نوع الخدمة -->
            <div style="margin-bottom: 20px;">
                <div class="section-title">اختر نوع الخدمة</div>
                <div style="display: flex; gap: 10px; margin-top: 10px;">
                    <button id="service-pubg" class="btn-royal" style="flex: 1; padding: 12px; font-size: 1rem;" onclick="selectService('pubg')">
                        <i class="fas fa-gamepad"></i> ببجي
                    </button>
                    <button id="service-mobile" class="btn-royal btn-mobile" style="flex: 1; padding: 12px; font-size: 1rem;" onclick="selectService('mobile')">
                        <i class="fas fa-mobile-alt"></i> يمن موبايل
                    </button>
                </div>
            </div>
            
            <!-- قسم ببجي -->
            <div id="pubg-section">
                <h3 style="color:var(--primary); margin: 20px 0 10px;"><i class="fas fa-gamepad"></i> اختيار باقة ببجي</h3>
                <div class="pkg-grid" id="pkgs-list">
                    <div class="pkg-card" style="background: rgba(0,0,0,0.1);">
                        <span class="loading"></span>
                        <div style="margin-top: 10px; color: var(--text-dim);">جاري تحميل الباقات...</div>
                    </div>
                </div>
                
                <h3 style="color:var(--primary); margin: 20px 0 10px;"><i class="fas fa-shopping-cart"></i> طلب شحن ببجي</h3>
                <input type="number" id="p-id" placeholder="أدخل آيدي اللاعب (ID)">
                <textarea id="p-note" rows="2" placeholder="أي ملاحظات إضافية؟"></textarea>
                <button class="btn-royal" id="btn-buy-pubg" onclick="processPubgOrder()" disabled>
                    <span id="buy-pubg-text">اختر باقة</span>
                    <span id="buy-pubg-loading" class="hidden"><span class="loading"></span> جاري المعالجة...</span>
                </button>
            </div>
            
            <!-- قسم يمن موبايل -->
            <div id="mobile-section" class="hidden">
                <h3 style="color:var(--ym-red); margin: 20px 0 10px;"><i class="fas fa-mobile-alt"></i> شحن رصيد يمن موبايل</h3>
                
                <div class="input-group" style="margin-bottom: 20px;">
                    <label><i class="fas fa-phone"></i> رقم الجوال</label>
                    <input type="number" id="mobile-phone" placeholder="77XXXXXXX" style="text-align: center;">
                </div>
                
                <div class="mobile-section-title">باقات 4G الفورية</div>
                <div class="mobile-packages-grid" id="mobile-4g-packages"></div>
                
                <div class="mobile-section-title">باقات هدايا (مكالمات + نت)</div>
                <div class="mobile-packages-grid" id="mobile-gift-packages"></div>
                
                <div class="mobile-section-title">باقات مزايا الجديدة</div>
                <div class="mobile-packages-grid" id="mobile-mazaia-packages"></div>
                
                <button class="btn-royal btn-mobile" id="btn-buy-mobile" onclick="processMobileOrder()" style="margin-top: 20px;" disabled>
                    <span id="buy-mobile-text">اختر باقة</span>
                    <span id="buy-mobile-loading" class="hidden"><span class="loading"></span> جاري المعالجة...</span>
                </button>
            </div>
            
            <!-- ملخص الطلبات الأخيرة -->
            <div id="recent-orders" class="mt-20" style="display: none;">
                <h4 style="color:var(--primary); margin-bottom: 10px;"><i class="fas fa-history"></i> آخر الطلبات</h4>
                <div id="recent-orders-list"></div>
                <div class="text-center mt-10">
                    <button onclick="showTab('orders')" style="background: transparent; border: 1px solid var(--primary); color: var(--primary); padding: 10px 20px; border-radius: 10px; font-weight: bold; width: auto;">
                        عرض الكل <i class="fas fa-arrow-left"></i>
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- تبويب الإشعارات -->
    <div id="tab-notifications" class="tab-content hidden" style="padding:20px;">
        <div class="pane-header" style="margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px solid var(--border-color);">
            <h4 style="color:var(--primary); margin:0;"><i class="fas fa-bell"></i> الإشعارات</h4>
            <div class="d-flex align-center" style="gap: 10px;">
                <button onclick="markAllAsRead()" class="btn-royal" style="width: auto; padding: 8px 15px; font-size: 0.8rem;">
                    <i class="fas fa-check-double"></i> تعيين الكل كمقروء
                </button>
                <button onclick="clearAllNotifications()" class="btn-royal" style="width: auto; padding: 8px 15px; font-size: 0.8rem; background: var(--danger);">
                    <i class="fas fa-trash"></i> حذف الكل
                </button>
            </div>
        </div>
        
        <div id="notifications-list" class="notifications-list">
            <div class="loading">
                <i class="fas fa-spinner fa-spin"></i>
                <p>جاري تحميل الإشعارات...</p>
            </div>
        </div>
        
        <div id="no-notifications" class="hidden" style="text-align: center; padding: 40px 20px; color: var(--text-dim);">
            <i class="fas fa-bell-slash" style="font-size: 3rem; margin-bottom: 15px;"></i>
            <h4>لا توجد إشعارات</h4>
            <p>سيظهر هنا جميع الإشعارات الخاصة بك</p>
        </div>
    </div>

    <!-- تبويب الدردشة -->
    <div id="tab-chat" class="tab-content hidden">
        <div style="padding:15px;">
            <h4 style="color:var(--primary); margin:0 0 10px;"><i class="fas fa-comments"></i> الدردشة مع الإدارة</h4>
            <div id="chat-box" class="chat-area">
                <div class="msg admin">
                    مرحباً! كيف يمكنني مساعدتك؟
                    <div class="msg-time">الآن</div>
                </div>
            </div>
            <div class="d-flex" style="gap:10px; margin-top: 10px;">
                <input type="text" id="chat-input" placeholder="اكتب رسالتك..." style="margin:0;" onkeypress="if(event.key === 'Enter') sendMsg()">
                <button class="btn-royal" style="width:auto; margin:0; padding: 12px 20px;" onclick="sendMsg()">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
            <p style="color: var(--text-dim); font-size: 0.8rem; text-align: center; margin-top: 10px;">
                متوسط وقت الرد: ٥-١٠ دقائق
            </p>
        </div>
    </div>

    <!-- تبويب الطلبات -->
    <div id="tab-orders" class="tab-content hidden" style="padding:20px;">
        <h4 style="color:var(--primary); margin-bottom: 15px;"><i class="fas fa-clipboard-list"></i> سجل العمليات</h4>
        
        <div class="search-box">
            <i class="fas fa-search"></i>
            <input type="text" id="search-orders" placeholder="ابحث في طلباتك..." onkeyup="filterOrders()">
        </div>
        
        <div style="margin-bottom: 10px;" class="d-flex justify-between align-center">
            <div>
                <button id="filter-all" class="filter-btn active" onclick="filterOrdersByStatus('all')" style="background: var(--primary); color: #000; border: none; padding: 5px 12px; border-radius: 15px; font-size: 0.8rem; margin-left: 5px;">
                    الكل
                </button>
                <button id="filter-waiting" class="filter-btn" onclick="filterOrdersByStatus('انتظار')" style="background: transparent; color: var(--text-dim); border: 1px solid var(--text-dim); padding: 5px 12px; border-radius: 15px; font-size: 0.8rem; margin-left: 5px;">
                    قيد الانتظار
                </button>
                <button id="filter-done" class="filter-btn" onclick="filterOrdersByStatus('مكتمل')" style="background: transparent; color: var(--text-dim); border: 1px solid var(--text-dim); padding: 5px 12px; border-radius: 15px; font-size: 0.8rem;">
                    مكتملة
                </button>
            </div>
            <button onclick="refreshOrders()" style="background: transparent; color: var(--primary); border: none; font-size: 1.2rem;">
                <i class="fas fa-sync-alt"></i>
            </button>
        </div>
        
        <div id="orders-list">
            <div class="order-card">
                <div class="text-center" style="color: var(--text-dim);">
                    <span class="loading"></span>
                    <div style="margin-top: 10px;">جاري تحميل الطلبات...</div>
                </div>
            </div>
        </div>
        
        <div id="no-orders" class="hidden" style="text-align: center; padding: 40px 20px; color: var(--text-dim);">
            <i class="fas fa-inbox" style="font-size: 3rem; margin-bottom: 15px;"></i>
            <p>لا توجد طلبات لعرضها</p>
        </div>
    </div>

    <!-- تبويب الإيداع -->
    <div id="tab-deposit" class="tab-content hidden" style="padding:20px;">
        <div class="auth-card" style="max-width:none; border: 1px solid rgba(251, 191, 36, 0.2);">
            <h3 style="color:var(--primary); margin-top:0;"><i class="fas fa-wallet"></i> إيداع رصيد</h3>
            <p style="color: var(--text-dim); margin-bottom: 15px;">قم بإيداع الرصيد عبر الحوالة البنكية ثم تأكيدها هنا</p>
            
            <div style="background: rgba(251, 191, 36, 0.1); padding: 15px; border-radius: 15px; margin-bottom: 20px;">
                <h4 style="color: var(--primary); margin-bottom: 10px;"><i class="fas fa-university"></i> معلومات الحساب البنكي</h4>
                <div style="background: rgba(0,0,0,0.2); padding: 12px; border-radius: 10px; margin-bottom: 8px;">
                    <div style="color: var(--text-dim); font-size: 0.9rem;">اسم البنك</div>
                    <div style="font-weight: bold;">بنك الراجحي</div>
                </div>
                <div style="background: rgba(0,0,0,0.2); padding: 12px; border-radius: 10px; margin-bottom: 8px;">
                    <div style="color: var(--text-dim); font-size: 0.9rem;">رقم الحساب</div>
                    <div style="font-weight: bold;">SA1234567890123456789</div>
                </div>
                <div style="background: rgba(0,0,0,0.2); padding: 12px; border-radius: 10px;">
                    <div style="color: var(--text-dim); font-size: 0.9rem;">اسم المستفيد</div>
                    <div style="font-weight: bold;">أبو أيمن برو</div>
                </div>
            </div>
            
            <input type="number" id="d-amount" placeholder="المبلغ المودع (ريال سعودي)">
            <input type="text" id="d-note" placeholder="رقم الحوالة أو المرجع">
            <select id="d-method" style="margin-top: 10px;">
                <option value="">طريقة الدفع</option>
                <option value="تحويل بنكي">تحويل بنكي</option>
                <option value="STC Pay">STC Pay</option>
                <option value="بطاقة ائتمان">بطاقة ائتمان</option>
            </select>
            <button class="btn-royal mt-10" onclick="submitDeposit()" id="deposit-btn">
                <span id="deposit-text">تأكيد الحوالة</span>
                <span id="deposit-loading" class="hidden"><span class="loading"></span> جاري الإرسال...</span>
            </button>
            
            <div style="margin-top: 20px; padding: 15px; background: rgba(0,0,0,0.2); border-radius: 15px;">
                <h4 style="color: var(--primary); margin-bottom: 10px;"><i class="fas fa-info-circle"></i> ملاحظات مهمة</h4>
                <ul style="padding-right: 20px; color: var(--text-dim); font-size: 0.9rem;">
                    <li>سيتم تفعيل الرصيد خلال ١-٤ ساعات عمل</li>
                    <li>احتفظ برقم الحوالة للمراجعة</li>
                    <li>للإستفسار اتصل بالدعم الفني</li>
                </ul>
            </div>
        </div>
    </div>

    <!-- التنقل السفلي -->
    <div class="nav-royal">
        <div class="nav-item active" onclick="showTab('home', this)">
            <b>🏠</b>الرئيسية
        </div>
        <div class="nav-item" onclick="showTab('notifications', this)" style="position: relative;">
            <b>🔔</b>الإشعارات
            <span id="nav-notification-badge" class="nav-badge hidden">0</span>
        </div>
        <div class="nav-item" onclick="showTab('chat', this)">
            <b>💬</b>الدردشة
        </div>
        <div class="nav-item" onclick="showTab('orders', this)">
            <b>📋</b>طلباتي
        </div>
        <div class="nav-item" onclick="showTab('deposit', this)">
            <b>💰</b>الإيداع
        </div>
    </div>
</div>

<script>
    // تهيئة Firebase
    const firebaseConfig = { 
        databaseURL: "https://bobjy-3f6f0-default-rtdb.firebaseio.com/" 
    };
    
    try {
        firebase.initializeApp(firebaseConfig);
    } catch (error) {
        console.log("Firebase already initialized");
    }
    
    const db = firebase.database();
    let user = null;
    let selectedPkg = null;
    let selectedMobilePkg = null;
    let allOrders = [];
    let currentService = 'pubg';
    let notifications = [];
    let unreadCount = 0;
    
    // --- إدارة الجلسة والدخول ---
    function toggleAuth(mode) {
        document.getElementById('login-box').classList.toggle('hidden', mode === 'reg');
        document.getElementById('reg-box').classList.toggle('hidden', mode === 'login');
        
        if (mode === 'login') {
            document.getElementById('l-phone').focus();
        } else {
            document.getElementById('r-name').focus();
        }
    }
    
    function showLoading(buttonId, show) {
        const btn = document.getElementById(buttonId);
        if (!btn) return;
        
        const textSpan = btn.querySelector('[id$="-text"]');
        const loadingSpan = btn.querySelector('[id$="-loading"]');
        
        if (textSpan && loadingSpan) {
            textSpan.classList.toggle('hidden', show);
            loadingSpan.classList.toggle('hidden', !show);
            btn.disabled = show;
        }
    }
    
    async function handleRegister() {
        const name = document.getElementById('r-name').value.trim();
        const phone = document.getElementById('r-phone').value.trim();
        const pass = document.getElementById('r-pass').value;
        
        if (!name || !phone || !pass) {
            showNotification('خطأ', 'يرجى ملء جميع الحقول', 'error');
            return;
        }
        
        if (pass.length < 6) {
            showNotification('خطأ', 'كلمة المرور يجب أن تكون 6 أحرف على الأقل', 'error');
            return;
        }
        
        showLoading('register-btn', true);
        
        try {
            const snapshot = await db.ref('Users/' + phone).once('value');
            
            if (snapshot.exists()) {
                showNotification('خطأ', 'رقم الهاتف مسجل بالفعل', 'error');
                showLoading('register-btn', false);
                return;
            }
            
            await db.ref('Users/' + phone).set({
                name: name,
                phone: phone,
                pass: pass,
                balance: 0,
                joined: new Date().toLocaleString('ar-SA'),
                lastLogin: new Date().toLocaleString('ar-SA'),
                notifications: 0
            });
            
            // إضافة إشعار ترحيبي
            await db.ref('Notifications/' + phone).push().set({
                type: 'info',
                title: 'مرحباً بك!',
                message: 'شكراً لتسجيلك في منصة أبو أيمن برو',
                read: false,
                createdAt: Date.now(),
                timestamp: new Date().toLocaleString('ar-SA')
            });
            
            showNotification('نجاح', 'تم التسجيل بنجاح! سجل دخولك الآن', 'success');
            setTimeout(() => {
                toggleAuth('login');
                showLoading('register-btn', false);
                document.getElementById('l-phone').value = phone;
                document.getElementById('l-pass').value = pass;
            }, 1500);
            
        } catch (error) {
            showNotification('خطأ', 'حدث خطأ أثناء التسجيل', 'error');
            console.error(error);
            showLoading('register-btn', false);
        }
    }
    
    async function handleLogin() {
        const phone = document.getElementById('l-phone').value.trim();
        const pass = document.getElementById('l-pass').value;
        
        if (!phone || !pass) {
            showNotification('خطأ', 'يرجى إدخال رقم الهاتف وكلمة المرور', 'error');
            return;
        }
        
        showLoading('login-btn', true);
        
        try {
            const snapshot = await db.ref('Users/' + phone).once('value');
            
            if (!snapshot.exists()) {
                showNotification('خطأ', 'رقم الهاتف غير مسجل', 'error');
                showLoading('login-btn', false);
                return;
            }
            
            const userData = snapshot.val();
            
            if (userData.pass !== pass) {
                showNotification('خطأ', 'كلمة المرور غير صحيحة', 'error');
                showLoading('login-btn', false);
                return;
            }
            
            await db.ref('Users/' + phone).update({
                lastLogin: new Date().toLocaleString('ar-SA')
            });
            
            user = userData;
            localStorage.setItem('user_session', JSON.stringify({
                phone: phone,
                loginTime: Date.now()
            }));
            
            showNotification('مرحباً', `أهلاً بك ${user.name}`, 'success');
            setTimeout(() => {
                launchApp();
                showLoading('login-btn', false);
            }, 1000);
            
        } catch (error) {
            showNotification('خطأ', 'حدث خطأ أثناء الدخول', 'error');
            console.error(error);
            showLoading('login-btn', false);
        }
    }
    
    // --- نظام الإشعارات المحسن ---
    function showNotification(title, message, type = 'info', duration = 5000) {
        const container = document.getElementById('notification-container');
        const notificationId = 'notif-' + Date.now();
        
        const notification = document.createElement('div');
        notification.id = notificationId;
        notification.className = `notification ${type}`;
        notification.innerHTML = `
            <div class="notification-icon">
                ${type === 'success' ? '<i class="fas fa-check-circle"></i>' :
                  type === 'error' ? '<i class="fas fa-exclamation-circle"></i>' :
                  type === 'warning' ? '<i class="fas fa-exclamation-triangle"></i>' :
                  type === 'mobile' ? '<i class="fas fa-mobile-alt"></i>' :
                  '<i class="fas fa-info-circle"></i>'}
            </div>
            <div class="notification-content">
                <div class="notification-title">${title}</div>
                <div class="notification-message">${message}</div>
            </div>
            <button class="notification-close" onclick="document.getElementById('${notificationId}').remove()">
                <i class="fas fa-times"></i>
            </button>
        `;
        
        container.appendChild(notification);
        
        // إظهار الإشعار
        setTimeout(() => {
            notification.classList.add('show');
        }, 10);
        
        // إخفاء الإشعار بعد المدة المحددة
        setTimeout(() => {
            if (document.getElementById(notificationId)) {
                notification.classList.remove('show');
                setTimeout(() => {
                    if (document.getElementById(notificationId)) {
                        notification.remove();
                    }
                }, 500);
            }
        }, duration);
    }
    
    async function saveUserNotification(title, message, type = 'info') {
        if (!user || !user.phone) return;
        
        const notificationData = {
            type: type,
            title: title,
            message: message,
            read: false,
            createdAt: Date.now(),
            timestamp: new Date().toLocaleString('ar-SA')
        };
        
        try {
            await db.ref('Notifications/' + user.phone).push().set(notificationData);
            await db.ref('Users/' + user.phone + '/notifications').transaction(n => (n || 0) + 1);
        } catch (error) {
            console.error('Error saving notification:', error);
        }
    }
    
    function loadNotifications() {
        if (!user || !user.phone) return;
        
        const notificationsList = document.getElementById('notifications-list');
        notificationsList.innerHTML = `
            <div class="loading">
                <i class="fas fa-spinner fa-spin"></i>
                <p>جاري تحميل الإشعارات...</p>
            </div>
        `;
        
        db.ref('Notifications/' + user.phone).orderByChild('createdAt').on('value', snap => {
            notificationsList.innerHTML = '';
            notifications = [];
            unreadCount = 0;
            
            if (!snap.exists() || snap.numChildren() === 0) {
                notificationsList.innerHTML = `
                    <div class="empty-state">
                        <i class="fas fa-bell-slash"></i>
                        <h4>لا توجد إشعارات</h4>
                        <p>سيظهر هنا جميع الإشعارات الخاصة بك</p>
                    </div>
                `;
                updateNotificationBadge(0);
                return;
            }
            
            const notificationItems = [];
            
            snap.forEach(child => {
                const notif = child.val();
                notif.id = child.key;
                notificationItems.push(notif);
                
                if (!notif.read) {
                    unreadCount++;
                }
            });
            
            // ترتيب تنازلي حسب التاريخ
            notificationItems.sort((a, b) => b.createdAt - a.createdAt);
            
            notificationItems.forEach(notif => {
                const notificationItem = document.createElement('div');
                notificationItem.className = `notification-item ${notif.type} ${notif.read ? '' : 'unread'}`;
                notificationItem.onclick = () => markAsRead(notif.id);
                
                const typeText = notif.type === 'success' ? 'نجاح' :
                               notif.type === 'error' ? 'خطأ' :
                               notif.type === 'warning' ? 'تحذير' :
                               notif.type === 'mobile' ? 'يمن موبايل' : 'معلومات';
                
                notificationItem.innerHTML = `
                    <div class="notification-header">
                        <div class="notification-type">${typeText}</div>
                        <div class="notification-time">${notif.timestamp || 'قبل قليل'}</div>
                    </div>
                    <div class="notification-body">
                        <strong>${notif.title}</strong>
                        <p style="margin-top: 5px; color: var(--text-dim); font-size: 0.85rem;">${notif.message}</p>
                    </div>
                    ${!notif.read ? `
                        <button class="mark-read-btn" onclick="event.stopPropagation(); markAsRead('${notif.id}')">
                            تعيين كمقروء
                        </button>
                    ` : ''}
                `;
                
                notificationsList.appendChild(notificationItem);
            });
            
            notifications = notificationItems;
            updateNotificationBadge(unreadCount);
        });
    }
    
    function markAsRead(notificationId) {
        if (!user || !user.phone) return;
        
        db.ref('Notifications/' + user.phone + '/' + notificationId).update({
            read: true,
            readAt: Date.now()
        })
            .then(() => {
                unreadCount = Math.max(0, unreadCount - 1);
                updateNotificationBadge(unreadCount);
            })
            .catch(error => {
                console.error('Error marking notification as read:', error);
            });
    }
    
    async function markAllAsRead() {
        if (!user || !user.phone || notifications.length === 0) return;
        
        const updates = {};
        notifications.forEach(notif => {
            if (!notif.read) {
                updates[notif.id + '/read'] = true;
                updates[notif.id + '/readAt'] = Date.now();
            }
        });
        
        if (Object.keys(updates).length > 0) {
            try {
                await db.ref('Notifications/' + user.phone).update(updates);
                unreadCount = 0;
                updateNotificationBadge(0);
                showNotification('تم', 'تم تعيين جميع الإشعارات كمقروءة', 'success');
            } catch (error) {
                showNotification('خطأ', 'حدث خطأ أثناء تحديث الإشعارات', 'error');
            }
        }
    }
    
    async function clearAllNotifications() {
        if (!user || !user.phone) return;
        
        if (confirm('هل أنت متأكد من حذف جميع الإشعارات؟')) {
            try {
                await db.ref('Notifications/' + user.phone).remove();
                await db.ref('Users/' + user.phone).update({ notifications: 0 });
                notifications = [];
                unreadCount = 0;
                updateNotificationBadge(0);
                showNotification('تم', 'تم حذف جميع الإشعارات', 'success');
            } catch (error) {
                showNotification('خطأ', 'حدث خطأ أثناء حذف الإشعارات', 'error');
            }
        }
    }
    
    function updateNotificationBadge(count) {
        const badge = document.getElementById('notification-badge');
        const navBadge = document.getElementById('nav-notification-badge');
        
        if (count > 0) {
            badge.textContent = count > 9 ? '9+' : count;
            navBadge.textContent = count > 9 ? '9+' : count;
            badge.classList.remove('hidden');
            navBadge.classList.remove('hidden');
        } else {
            badge.classList.add('hidden');
            navBadge.classList.add('hidden');
        }
        
        document.getElementById('nav-notification-badge').textContent = count > 9 ? '9+' : count;
    }
    
    // --- بدء التطبيق ---
    window.onload = async () => {
        try {
            const saved = localStorage.getItem('user_session');
            
            if (saved) {
                const session = JSON.parse(saved);
                const snapshot = await db.ref('Users/' + session.phone).once('value');
                
                if (snapshot.exists()) {
                    user = snapshot.val();
                    
                    const sessionAge = Date.now() - session.loginTime;
                    const sevenDays = 7 * 24 * 60 * 60 * 1000;
                    
                    if (sessionAge < sevenDays) {
                        launchApp();
                    } else {
                        localStorage.removeItem('user_session');
                        showNotification('انتهت الجلسة', 'يرجى تسجيل الدخول مرة أخرى', 'warning');
                    }
                } else {
                    localStorage.removeItem('user_session');
                }
            }
        } catch (error) {
            console.error('Error restoring session:', error);
        }
        
        const inputs = document.querySelectorAll('input, textarea, select');
        inputs.forEach(input => {
            input.addEventListener('focus', function() {
                this.parentElement.style.transform = 'translateY(-2px)';
            });
            
            input.addEventListener('blur', function() {
                this.parentElement.style.transform = 'translateY(0)';
            });
        });
    };
    
    function launchApp() {
        document.getElementById('auth-section').classList.add('hidden');
        document.getElementById('main-app').classList.remove('hidden');
        document.getElementById('loading-screen').classList.remove('hidden');
        
        document.getElementById('u-name-display').textContent = user.name;
        
        const lastLogin = user.lastLogin || 'أول دخول';
        document.getElementById('last-login').textContent = `آخر دخول: ${lastLogin}`;
        
        loadUserData();
        loadPkgs();
        loadMobilePackages();
        loadOrders();
        loadChat();
        loadNotifications();
        
        setTimeout(() => {
            document.getElementById('loading-screen').classList.add('hidden');
            showNotification('تم التحميل', 'مرحباً بك في تطبيق أبو أيمن برو', 'success');
        }, 2000);
    }
    
    function loadUserData() {
        db.ref('Users/' + user.phone + '/balance').on('value', snap => {
            const newBalance = snap.val() || 0;
            
            if (user.balance !== undefined && newBalance !== user.balance) {
                const diff = newBalance - user.balance;
                if (diff > 0) {
                    const message = `تم إضافة ${diff} ريال إلى رصيدك`;
                    showNotification('تم زيادة الرصيد', message, 'success');
                    saveUserNotification('زيادة رصيد', message, 'success');
                }
            }
            
            user.balance = newBalance;
            document.getElementById('u-balance').textContent = user.balance;
            document.getElementById('balance-time').textContent = new Date().toLocaleTimeString('ar-SA', {hour: '2-digit', minute:'2-digit'});
            
            updateBuyButtons();
        });
    }
    
    function logout() {
        if (confirm('هل تريد تسجيل الخروج؟')) {
            localStorage.removeItem('user_session');
            user = null;
            document.getElementById('main-app').classList.add('hidden');
            document.getElementById('auth-section').classList.remove('hidden');
            showNotification('تم الخروج', 'نراك قريباً!', 'info');
        }
    }
    
    // --- اختيار نوع الخدمة ---
    function selectService(service) {
        currentService = service;
        
        const pubgSection = document.getElementById('pubg-section');
        const mobileSection = document.getElementById('mobile-section');
        const pubgBtn = document.getElementById('service-pubg');
        const mobileBtn = document.getElementById('service-mobile');
        
        if (service === 'pubg') {
            pubgSection.classList.remove('hidden');
            mobileSection.classList.add('hidden');
            pubgBtn.style.background = 'linear-gradient(90deg, #fbbf24, #f59e0b)';
            mobileBtn.style.background = 'linear-gradient(135deg, var(--ym-red), #ff6b6b)';
        } else {
            pubgSection.classList.add('hidden');
            mobileSection.classList.remove('hidden');
            pubgBtn.style.background = 'linear-gradient(135deg, #fbbf24, #f59e0b)';
            mobileBtn.style.background = 'linear-gradient(135deg, #ed1c24, #ff4444)';
            mobileBtn.style.boxShadow = '0 5px 15px rgba(237, 28, 36, 0.3)';
        }
        
        updateBuyButtons();
    }
    
    function updateBuyButtons() {
        if (currentService === 'pubg' && selectedPkg) {
            const btn = document.getElementById('btn-buy-pubg');
            const buyText = btn.querySelector('#buy-pubg-text');
            
            if (user.balance < selectedPkg.price) {
                btn.disabled = true;
                buyText.textContent = "الرصيد لا يكفي";
                buyText.style.color = "#ff4d4d";
            } else {
                btn.disabled = false;
                buyText.textContent = `شحن ${selectedPkg.name} ✅`;
                buyText.style.color = "";
            }
        } else if (currentService === 'mobile' && selectedMobilePkg) {
            const btn = document.getElementById('btn-buy-mobile');
            const buyText = btn.querySelector('#buy-mobile-text');
            
            if (user.balance < selectedMobilePkg.price) {
                btn.disabled = true;
                buyText.textContent = "الرصيد لا يكفي";
                buyText.style.color = "#ff4d4d";
            } else {
                btn.disabled = false;
                buyText.textContent = `شحن ${selectedMobilePkg.name} ✅`;
                buyText.style.color = "";
            }
        }
    }
    
    // --- إدارة باقات ببجي ---
    function loadPkgs() {
        db.ref('Settings/Prices').on('value', snap => {
            const list = document.getElementById('pkgs-list');
            
            if (!snap.exists() || snap.numChildren() === 0) {
                list.innerHTML = `
                    <div class="pkg-card" style="grid-column: 1 / -1; background: rgba(255,77,77,0.1); border-color: var(--danger);">
                        <i class="fas fa-exclamation-triangle" style="color: var(--danger); font-size: 2rem; margin-bottom: 10px;"></i>
                        <div>لا توجد باقات متاحة حالياً</div>
                        <div style="color: var(--text-dim); font-size: 0.8rem; margin-top: 5px;">يرجى المحاولة لاحقاً</div>
                    </div>
                `;
                return;
            }
            
            list.innerHTML = "";
            let firstPkg = null;
            
            snap.forEach(c => {
                const pkgData = c.val();
                if (pkgData.type === 'pubg') {
                    const div = document.createElement('div');
                    div.className = 'pkg-card';
                    div.innerHTML = `
                        <b style="font-size: 1.1rem;">${c.key}</b>
                        <div style="margin-top: 8px;">
                            <span style="color:var(--primary); font-weight: bold; font-size: 1.2rem;">${pkgData.price} ريال</span>
                        </div>
                        <div style="font-size: 0.8rem; color: var(--text-dim); margin-top: 5px;">انقر للاختيار</div>
                    `;
                    
                    const pkgInfo = { name: c.key, price: pkgData.price };
                    
                    div.onclick = () => {
                        document.querySelectorAll('.pkg-card').forEach(i => i.classList.remove('selected'));
                        div.classList.add('selected');
                        selectedPkg = pkgInfo;
                        updateBuyButtons();
                    };
                    
                    list.appendChild(div);
                    
                    if (!firstPkg) {
                        firstPkg = div;
                        firstPkg.click();
                    }
                }
            });
            
            if (!firstPkg) {
                list.innerHTML = `
                    <div class="pkg-card" style="grid-column: 1 / -1; background: rgba(255,77,77,0.1); border-color: var(--danger);">
                        <i class="fas fa-exclamation-triangle" style="color: var(--danger); font-size: 2rem; margin-bottom: 10px;"></i>
                        <div>لا توجد باقات ببجي متاحة حالياً</div>
                        <div style="color: var(--text-dim); font-size: 0.8rem; margin-top: 5px;">يرجى المحاولة لاحقاً</div>
                    </div>
                `;
            }
        });
    }
    
    // --- إدارة باقات يمن موبايل ---
    function loadMobilePackages() {
        db.ref('MobilePackages').orderByChild('status').equalTo('active').on('value', snap => {
            const grid4g = document.getElementById('mobile-4g-packages');
            const gridGift = document.getElementById('mobile-gift-packages');
            const gridMazaia = document.getElementById('mobile-mazaia-packages');
            
            grid4g.innerHTML = '';
            gridGift.innerHTML = '';
            gridMazaia.innerHTML = '';
            
            let firstPkg = null;
            
            if (!snap.exists() || snap.numChildren() === 0) {
                grid4g.innerHTML = `
                    <div style="grid-column: 1 / -1; text-align: center; padding: 20px; color: var(--text-dim);">
                        <i class="fas fa-mobile-alt" style="font-size: 2rem; margin-bottom: 10px;"></i>
                        <div>لا توجد باقات متاحة حالياً</div>
                    </div>
                `;
                return;
            }
            
            snap.forEach(child => {
                const pkg = child.val();
                const div = document.createElement('div');
                div.className = 'mobile-package-card';
                div.innerHTML = `
                    <h3 style="margin: 0; font-size: 0.9rem; color: var(--text-main);">${pkg.name}</h3>
                    <p style="margin: 5px 0 0; font-size: 0.8rem; color: var(--text-dim);">${pkg.description || ''}</p>
                    <span class="mobile-price-tag">${pkg.price} ريال</span>
                `;
                
                const pkgInfo = { 
                    name: pkg.name, 
                    price: pkg.price, 
                    desc: pkg.description,
                    id: child.key
                };
                
                div.onclick = () => {
                    document.querySelectorAll('.mobile-package-card').forEach(c => c.classList.remove('selected'));
                    div.classList.add('selected');
                    selectedMobilePkg = pkgInfo;
                    updateBuyButtons();
                };
                
                switch(pkg.category) {
                    case '4g':
                        grid4g.appendChild(div);
                        break;
                    case 'gift':
                        gridGift.appendChild(div);
                        break;
                    case 'mazaia':
                        gridMazaia.appendChild(div);
                        break;
                    default:
                        grid4g.appendChild(div);
                }
                
                if (!firstPkg) {
                    firstPkg = div;
                }
            });
            
            if (firstPkg) {
                firstPkg.click();
            }
        });
    }
    
    // --- معالجة طلبات ببجي ---
    async function processPubgOrder() {
        const playerId = document.getElementById('p-id').value.trim();
        const note = document.getElementById('p-note').value.trim();
        
        if (!playerId) {
            showNotification('خطأ', 'يرجى إدخال آيدي اللاعب', 'error');
            document.getElementById('p-id').focus();
            return;
        }
        
        if (!selectedPkg) {
            showNotification('خطأ', 'يرجى اختيار باقة', 'error');
            return;
        }
        
        if (user.balance < selectedPkg.price) {
            showNotification('خطأ', 'رصيدك غير كافي لهذه الباقة', 'error');
            showTab('deposit');
            return;
        }
        
        showLoading('btn-buy-pubg', true);
        
        try {
            await db.ref('Users/' + user.phone).update({
                balance: user.balance - selectedPkg.price
            });
            
            const orderRef = db.ref('Orders').push();
            const orderId = orderRef.key;
            const orderTime = new Date().toLocaleString('ar-SA');
            
            await orderRef.set({
                id: orderId,
                clientName: user.name,
                clientPhone: user.phone,
                package: selectedPkg.name,
                price: selectedPkg.price,
                pubgId: playerId,
                note: note,
                status: 'انتظار',
                type: 'شحن ببجي',
                time: orderTime,
                timestamp: Date.now()
            });
            
            // إضافة إشعار للمستخدم
            await saveUserNotification(
                'طلب جديد', 
                `تم إنشاء طلب شحن ${selectedPkg.name} بقيمة ${selectedPkg.price} ريال`,
                'info'
            );
            
            // إشعار عاجل
            showNotification('نجاح الطلب', `تم طلب شحن ${selectedPkg.name} بنجاح`, 'success');
            
            document.getElementById('p-id').value = '';
            document.getElementById('p-note').value = '';
            
            showTab('orders');
            
        } catch (error) {
            showNotification('خطأ', 'حدث خطأ أثناء معالجة الطلب', 'error');
            console.error(error);
        } finally {
            showLoading('btn-buy-pubg', false);
        }
    }
    
    // --- معالجة طلبات يمن موبايل ---
    async function processMobileOrder() {
        const mobilePhone = document.getElementById('mobile-phone').value.trim();
        
        if (!mobilePhone || mobilePhone.length < 9) {
            showNotification('خطأ', 'يرجى إدخال رقم جوال يمن موبايل صحيح', 'error');
            document.getElementById('mobile-phone').focus();
            return;
        }
        
        if (!selectedMobilePkg) {
            showNotification('خطأ', 'يرجى اختيار باقة', 'error');
            return;
        }
        
        if (user.balance < selectedMobilePkg.price) {
            showNotification('خطأ', 'رصيدك غير كافي لهذه الباقة', 'error');
            showTab('deposit');
            return;
        }
        
        showLoading('btn-buy-mobile', true);
        
        try {
            await db.ref('Users/' + user.phone).update({
                balance: user.balance - selectedMobilePkg.price
            });
            
            const orderRef = db.ref('Orders').push();
            const orderId = orderRef.key;
            const orderTime = new Date().toLocaleString('ar-SA');
            
            await orderRef.set({
                id: orderId,
                clientName: user.name,
                clientPhone: user.phone,
                package: selectedMobilePkg.name,
                price: selectedMobilePkg.price,
                mobileNumber: mobilePhone,
                description: selectedMobilePkg.desc,
                status: 'انتظار',
                type: 'شحن يمن موبايل',
                time: orderTime,
                timestamp: Date.now()
            });
            
            // إضافة إشعار للمستخدم
            await saveUserNotification(
                'طلب يمن موبايل', 
                `تم إنشاء طلب شحن ${selectedMobilePkg.name} للرقم ${mobilePhone} بقيمة ${selectedMobilePkg.price} ريال`,
                'mobile'
            );
            
            // إشعار عاجل
            showNotification('نجاح الطلب', `تم طلب شحن ${selectedMobilePkg.name} للرقم ${mobilePhone}`, 'mobile');
            
            showTab('orders');
            
        } catch (error) {
            showNotification('خطأ', 'حدث خطأ أثناء معالجة الطلب', 'error');
            console.error(error);
        } finally {
            showLoading('btn-buy-mobile', false);
        }
    }
    
    async function submitDeposit() {
        const amount = document.getElementById('d-amount').value.trim();
        const note = document.getElementById('d-note').value.trim();
        const method = document.getElementById('d-method').value;
        
        if (!amount || amount <= 0) {
            showNotification('خطأ', 'يرجى إدخال مبلغ صحيح', 'error');
            document.getElementById('d-amount').focus();
            return;
        }
        
        if (!note) {
            showNotification('خطأ', 'يرجى إدخال رقم الحوالة', 'error');
            document.getElementById('d-note').focus();
            return;
        }
        
        if (!method) {
            showNotification('خطأ', 'يرجى اختيار طريقة الدفع', 'error');
            return;
        }
        
        showLoading('deposit-btn', true);
        
        try {
            const depositRef = db.ref('Orders').push();
            const depositId = depositRef.key;
            const depositTime = new Date().toLocaleString('ar-SA');
            
            await depositRef.set({
                id: depositId,
                clientName: user.name,
                clientPhone: user.phone,
                amount: amount,
                note: note,
                method: method,
                status: 'انتظار',
                type: 'إيداع',
                time: depositTime,
                timestamp: Date.now()
            });
            
            // إضافة إشعار للمستخدم
            await saveUserNotification(
                'طلب إيداع', 
                `تم إرسال طلب إيداع بقيمة ${amount} ريال`,
                'info'
            );
            
            showNotification('تم الإرسال', 'تم إرسال طلب الإيداع بنجاح', 'success');
            
            document.getElementById('d-amount').value = '';
            document.getElementById('d-note').value = '';
            document.getElementById('d-method').value = '';
            
            showTab('home');
            
        } catch (error) {
            showNotification('خطأ', 'حدث خطأ أثناء إرسال طلب الإيداع', 'error');
            console.error(error);
        } finally {
            showLoading('deposit-btn', false);
        }
    }
    
    // --- إدارة الدردشة ---
    function loadChat() {
        if (!user || !user.phone) return;
        
        db.ref('Chats/' + user.phone).on('value', snap => {
            const box = document.getElementById('chat-box');
            
            if (!snap.exists() || snap.numChildren() === 0) {
                box.innerHTML = `
                    <div class="msg admin">
                        مرحباً! كيف يمكنني مساعدتك؟
                        <div class="msg-time">الآن</div>
                    </div>
                `;
                return;
            }
            
            box.innerHTML = "";
            const messages = [];
            
            snap.forEach(c => {
                messages.push({
                    id: c.key,
                    ...c.val()
                });
            });
            
            messages.sort((a, b) => a.time - b.time);
            
            messages.forEach(m => {
                const msgDiv = document.createElement('div');
                msgDiv.className = `msg ${m.sender === 'admin' ? 'admin' : 'user'}`;
                
                const timeStr = m.time ? new Date(m.time).toLocaleTimeString('ar-SA', {hour: '2-digit', minute:'2-digit'}) : 'الآن';
                
                msgDiv.innerHTML = `
                    ${m.text}
                    <div class="msg-time">${timeStr}</div>
                `;
                
                box.appendChild(msgDiv);
            });
            
            box.scrollTop = box.scrollHeight;
        });
    }
    
    async function sendMsg() {
        if (!user || !user.phone) return;
        
        const input = document.getElementById('chat-input');
        const txt = input.value.trim();
        
        if (!txt) return;
        
        try {
            await db.ref('Chats/' + user.phone).push({
                sender: 'user',
                text: txt,
                time: Date.now()
            });
            
            input.value = "";
            input.focus();
            
        } catch (error) {
            showNotification('خطأ', 'حدث خطأ أثناء إرسال الرسالة', 'error');
            console.error(error);
        }
    }
    
    // --- إدارة الطلبات ---
    function loadOrders() {
        if (!user || !user.phone) return;
        
        db.ref('Orders').orderByChild('timestamp').on('value', snap => {
            allOrders = [];
            const list = document.getElementById('orders-list');
            const recentList = document.getElementById('recent-orders-list');
            const noOrders = document.getElementById('no-orders');
            
            if (!snap.exists() || snap.numChildren() === 0) {
                list.innerHTML = '';
                noOrders.classList.remove('hidden');
                document.getElementById('recent-orders').style.display = 'none';
                return;
            }
            
            noOrders.classList.add('hidden');
            
            snap.forEach(c => {
                const order = c.val();
                if (order.clientPhone === user.phone) {
                    allOrders.push(order);
                }
            });
            
            allOrders.sort((a, b) => (b.timestamp || 0) - (a.timestamp || 0));
            
            displayOrders(allOrders);
            
            if (allOrders.length > 0) {
                document.getElementById('recent-orders').style.display = 'block';
                recentList.innerHTML = '';
                
                const recentOrders = allOrders.slice(0, 3);
                recentOrders.forEach(o => {
                    const orderCard = createOrderCard(o);
                    recentList.appendChild(orderCard);
                });
            } else {
                document.getElementById('recent-orders').style.display = 'none';
            }
        });
    }
    
    function createOrderCard(order) {
        const div = document.createElement('div');
        div.className = 'order-card';
        
        let statusClass = 'status-waiting';
        if (order.status === 'مكتمل') statusClass = 'status-done';
        if (order.status === 'ملغي') statusClass = 'status-canceled';
        
        let details = '';
        if (order.type === 'شحن ببجي') {
            details = `
                <div style="color: var(--text-dim); font-size: 0.9rem; margin-top: 5px;">
                    <div>الباقة: <b style="color: var(--primary);">${order.package}</b></div>
                    <div>آيدي اللاعب: <b>${order.pubgId || 'غير محدد'}</b></div>
                    ${order.note ? `<div>ملاحظات: ${order.note}</div>` : ''}
                </div>
            `;
        } else if (order.type === 'شحن يمن موبايل') {
            details = `
                <div style="color: var(--text-dim); font-size: 0.9rem; margin-top: 5px;">
                    <div>الباقة: <b style="color: var(--ym-red);">${order.package}</b></div>
                    <div>رقم الجوال: <b>${order.mobileNumber || 'غير محدد'}</b></div>
                    <div>الوصف: <b>${order.description || 'غير محدد'}</b></div>
                </div>
            `;
        } else if (order.type === 'إيداع') {
            details = `
                <div style="color: var(--text-dim); font-size: 0.9rem; margin-top: 5px;">
                    <div>المبلغ: <b style="color: var(--primary);">${order.amount} ريال</b></div>
                    <div>طريقة الدفع: <b>${order.method || 'غير محدد'}</b></div>
                    <div>رقم الحوالة: <b>${order.note || 'غير محدد'}</b></div>
                </div>
            `;
        }
        
        div.innerHTML = `
            <div class="d-flex justify-between align-center">
                <div>
                    <b>${order.package || order.type}</b>
                    <div style="font-size: 0.85rem; color: var(--text-dim);">${order.time}</div>
                </div>
                <div class="${statusClass} order-status">${order.status}</div>
            </div>
            ${details}
            <div style="margin-top: 10px; font-size: 0.9rem;">
                <span style="color: var(--text-dim);">المبلغ:</span>
                <b style="color: var(--primary); margin-right: 5px;">${order.price || order.amount} ريال</b>
            </div>
        `;
        
        return div;
    }
    
    function displayOrders(orders) {
        const list = document.getElementById('orders-list');
        const noOrders = document.getElementById('no-orders');
        
        if (orders.length === 0) {
            list.innerHTML = '';
            noOrders.classList.remove('hidden');
            return;
        }
        
        noOrders.classList.add('hidden');
        list.innerHTML = '';
        
        orders.forEach(order => {
            const orderCard = createOrderCard(order);
            list.appendChild(orderCard);
        });
    }
    
    function filterOrders() {
        const searchText = document.getElementById('search-orders').value.toLowerCase();
        
        if (!searchText) {
            displayOrders(allOrders);
            return;
        }
        
        const filtered = allOrders.filter(order => {
            return (
                (order.package && order.package.toLowerCase().includes(searchText)) ||
                (order.type && order.type.toLowerCase().includes(searchText)) ||
                (order.pubgId && order.pubgId.includes(searchText)) ||
                (order.mobileNumber && order.mobileNumber.includes(searchText)) ||
                (order.note && order.note.toLowerCase().includes(searchText)) ||
                (order.status && order.status.toLowerCase().includes(searchText)) ||
                (order.time && order.time.includes(searchText))
            );
        });
        
        displayOrders(filtered);
    }
    
    function filterOrdersByStatus(status) {
        document.querySelectorAll('.filter-btn').forEach(btn => {
            btn.classList.remove('active');
            btn.style.background = 'transparent';
            btn.style.color = 'var(--text-dim)';
            btn.style.border = '1px solid var(--text-dim)';
        });
        
        const activeBtn = document.getElementById(`filter-${status === 'all' ? 'all' : status === 'انتظار' ? 'waiting' : 'done'}`);
        activeBtn.classList.add('active');
        activeBtn.style.background = 'var(--primary)';
        activeBtn.style.color = '#000';
        activeBtn.style.border = 'none';
        
        if (status === 'all') {
            displayOrders(allOrders);
            return;
        }
        
        const filtered = allOrders.filter(order => order.status === status);
        displayOrders(filtered);
    }
    
    function refreshOrders() {
        const refreshBtn = document.querySelector('#tab-orders button[onclick="refreshOrders()"]');
        refreshBtn.style.transform = 'rotate(360deg)';
        refreshBtn.style.transition = 'transform 0.5s';
        
        setTimeout(() => {
            refreshBtn.style.transform = 'rotate(0deg)';
        }, 500);
        
        showNotification('تحديث', 'جاري تحديث قائمة الطلبات', 'info');
    }
    
    // --- إدارة التبويبات ---
    function showTab(tabName, element = null) {
        document.querySelectorAll('.tab-content').forEach(tab => {
            tab.classList.add('hidden');
        });
        
        document.getElementById('tab-' + tabName).classList.remove('hidden');
        
        if (element) {
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('active');
            });
            element.classList.add('active');
        } else {
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('active');
                if (item.textContent.includes(getTabTitle(tabName))) {
                    item.classList.add('active');
                }
            });
        }
        
        if (tabName === 'home') {
            document.getElementById('p-id').focus();
        } else if (tabName === 'chat') {
            document.getElementById('chat-input').focus();
        } else if (tabName === 'deposit') {
            document.getElementById('d-amount').focus();
        } else if (tabName === 'notifications') {
            loadNotifications();
        }
    }
    
    function getTabTitle(tabName) {
        const titles = {
            'home': 'الرئيسية',
            'notifications': 'الإشعارات',
            'chat': 'الدردشة',
            'orders': 'طلباتي',
            'deposit': 'الإيداع'
        };
        return titles[tabName] || tabName;
    }
    
    document.addEventListener('DOMContentLoaded', function() {
        const chatInput = document.getElementById('chat-input');
        if (chatInput) {
            chatInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    sendMsg();
                }
            });
        }
    });
    
    // --- مراقبة تحديثات الطلبات للإشعارات ---
    function monitorOrderUpdates() {
        if (!user || !user.phone) return;
        
        db.ref('Orders').orderByChild('clientPhone').equalTo(user.phone).on('child_changed', (snap) => {
            const order = snap.val();
            const previousOrder = allOrders.find(o => o.id === order.id);
            
            if (previousOrder && previousOrder.status !== order.status) {
                let notificationTitle = '';
                let notificationMessage = '';
                
                switch(order.status) {
                    case 'مكتمل':
                        notificationTitle = 'طلب مكتمل';
                        notificationMessage = `تم إكمال طلب ${order.package || order.type} بنجاح`;
                        showNotification('🎉 طلب مكتمل', `تم إكمال طلبك ${order.package || order.type}`, 'success');
                        break;
                    case 'ملغي':
                        notificationTitle = 'طلب ملغي';
                        notificationMessage = `تم إلغاء طلب ${order.package || order.type}`;
                        showNotification('⚠️ طلب ملغي', `تم إلغاء طلبك ${order.package || order.type}`, 'warning');
                        break;
                }
                
                if (notificationTitle) {
                    saveUserNotification(notificationTitle, notificationMessage, 
                        order.status === 'مكتمل' ? 'success' : 'warning');
                }
            }
        });
    }
</script>
</body>
</html>
