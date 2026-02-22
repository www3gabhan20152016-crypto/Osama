<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>المسبحة الإلكترونية</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f0f4f8;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .tasbih-container {
            background-color: #ffffff;
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            text-align: center;
            width: 300px;
        }

        h2 { color: #2c3e50; margin-bottom: 20px; }

        #counter {
            font-size: 60px;
            font-weight: bold;
            color: #27ae60;
            margin: 20px 0;
            font-family: 'Courier New', Courier, monospace;
        }

        .target-text {
            color: #7f8c8d;
            margin-bottom: 20px;
            font-size: 18px;
        }

        select {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border-radius: 10px;
            border: 1px solid #ddd;
            font-size: 16px;
            outline: none;
        }

        .btn-main {
            background-color: #27ae60;
            color: white;
            border: none;
            width: 150px;
            height: 150px;
            border-radius: 50%;
            font-size: 24px;
            cursor: pointer;
            box-shadow: 0 6px #1e8449;
            transition: all 0.1s;
            margin-bottom: 20px;
            outline: none;
        }

        .btn-main:active {
            box-shadow: 0 2px #1e8449;
            transform: translateY(4px);
        }

        .btn-reset {
            background-color: #e74c3c;
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 14px;
        }

        .btn-reset:hover { background-color: #c0392b; }
    </style>
</head>
<body>

<div class="tasbih-container">
    <h2>المسبحة الإلكترونية</h2>
    
    <select id="dikr-select">
        <option value="سبحان الله">سبحان الله</option>
        <option value="الحمد لله">الحمد لله</option>
        <option value="لا إله إلا الله">لا إله إلا الله</option>
        <option value="الله أكبر">الله أكبر</option>
        <option value="أستغفر الله">أستغفر الله</option>
    </select>

    <div id="counter">0</div>
    
    <button class="btn-main" onclick="increment()">اضغط</button>
    
    <br>
    
    <button class="btn-reset" onclick="resetCounter()">إعادة ضبط</button>
</div>

<script>
    let count = 0;
    const counterDisplay = document.getElementById('counter');

    function increment() {
        count++;
        counterDisplay.innerText = count;
        
        // تأثير بسيط عند الضغط (اختياري)
        if ('vibrate' in navigator) {
            navigator.vibrate(50); // اهتزاز بسيط للهواتف
        }
    }

    function resetCounter() {
        if(confirm("هل تريد تصفير العداد؟")) {
            count = 0;
            counterDisplay.innerText = count;
        }
    }
</script>

</body>
</html>
