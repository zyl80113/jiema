<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>接码平台</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f4f4f4;
        }
        .container {
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            font-size: 24px;
            text-align: center;
        }
        .btn {
            display: inline-block;
            background-color: #28a745;
            color: white;
            padding: 10px 20px;
            text-decoration: none;
            border-radius: 5px;
            margin-top: 10px;
            cursor: pointer;
        }
        .btn:disabled {
            background-color: #ccc;
            cursor: not-allowed;
        }
        .result {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>接码平台</h1>
        
        <button id="getPhoneBtn" class="btn">获取手机号</button>

        <div class="result">
            <p id="phone">手机号: </p>
            <p id="sms">验证码: </p>
        </div>

        <button id="getCodeBtn" class="btn" style="display:none;">获取验证码</button>
    </div>

    <script>
        // 你的 API 地址和令牌
        const API_URL = 'https://api.haozhuyun.cn/sms/';
        const TOKEN = 'your_token';  // 替换成从登录获取的令牌
        const SID = 'your_sid';      // 替换成你的项目ID

        // 获取手机号
        document.getElementById('getPhoneBtn').addEventListener('click', function() {
            fetch(API_URL + `?api=getPhone&token=${TOKEN}&sid=${SID}`)
                .then(response => response.json())
                .then(data => {
                    if (data.code === "0") {
                        document.getElementById('phone').innerText = '手机号: ' + data.phone;
                        document.getElementById('getCodeBtn').style.display = 'inline-block';
                    } else {
                        alert('获取手机号失败: ' + data.msg);
                    }
                })
                .catch(err => {
                    alert('请求失败: ' + err.message);
                });
        });

        // 获取验证码
        document.getElementById('getCodeBtn').addEventListener('click', function() {
            const phone = document.getElementById('phone').innerText.split(': ')[1];

            fetch(API_URL + `?api=getMessage&token=${TOKEN}&sid=${SID}&phone=${phone}`)
                .then(response => response.json())
                .then(data => {
                    if (data.code === "0") {
                        document.getElementById('sms').innerText = '验证码: ' + data.yzm;
                    } else {
                        alert('获取验证码失败: ' + data.msg);
                    }
                })
                .catch(err => {
                    alert('请求失败: ' + err.message);
                });
        });
    </script>
</body>
</html>
