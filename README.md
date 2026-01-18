# numerology
天賦優勢全息圖
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>天賦優勢心理學全息圖 - 專業工具版</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        :root {
            --bg-purple-light: #F2EBF7; 
            --text-purple-dark: #6a2c91;
            --main-green: #2d5a27;
        }
        body {
            font-family: "Microsoft JhengHei", sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #f4f4f9;
            margin: 0;
            padding: 20px;
        }
        .controls {
            background: white;
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
            margin-bottom: 20px;
            text-align: center;
            max-width: 450px;
            width: 100%;
        }
        .input-row { display: flex; justify-content: center; gap: 8px; margin-top: 15px; }
        input {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            text-align: center;
            font-size: 16px;
            outline: none;
        }
        input:focus { border-color: var(--text-purple-dark); }
        .btn-group { margin-top: 20px; display: flex; flex-wrap: wrap; gap: 10px; }
        button {
            padding: 12px;
            cursor: pointer;
            border: none;
            border-radius: 8px;
            background: var(--text-purple-dark);
            color: white;
            font-weight: bold;
            flex: 1;
            min-width: 120px;
            transition: 0.2s;
        }
        button:hover { opacity: 0.9; }
        .btn-clear { background-color: #f44336; }
        .btn-save { background-color: #333; }
        
        #captureArea {
            width: 450px;
            height: 600px;
            background: white;
            position: relative;
        }
        canvas { width: 100%; height: 100%; }
    </style>
</head>
<body>

<div class="controls">
    <input type="text" id="userName" placeholder="請輸入姓名" style="width: 90%;"><br>
    <div class="input-row">
        <input type="number" id="year" placeholder="年(YYYY)" min="1" max="9999">
        <input type="number" id="month" placeholder="月(MM)" min="1" max="12">
        <input type="number" id="day" placeholder="日(DD)" min="1" max="31">
    </div>
    <div class="btn-group">
        <button onclick="calculateAndDraw()">生成全息圖</button>
        <button class="btn-clear" onclick="clearFields()">清除資料</button>
        <button class="btn-save" onclick="saveImage()">一鍵截圖儲存</button>
    </div>
</div>

<div id="captureArea">
    <canvas id="holoCanvas" width="900" height="1200"></canvas>
</div>

<script>
    const canvas = document.getElementById('holoCanvas');
    const ctx = canvas.getContext('2d');

    // 驗證與自動換行邏輯
    const yearInput = document.getElementById('year');
    const monthInput = document.getElementById('month');
    const dayInput = document.getElementById('day');

    yearInput.addEventListener('input', function() {
        if(this.value.length >= 4) {
            this.value = this.value.slice(0, 4);
            monthInput.focus();
        }
    });

    monthInput.addEventListener('input', function() {
        if(this.value > 12) this.value = 12;
        if(this.value.length >= 2) {
            this.value = this.value.slice(0, 2);
            dayInput.focus();
        }
    });

    dayInput.addEventListener('input', function() {
        if(this.value > 31) this.value = 31;
        if(this.value.length >= 2) this.value = this.value.slice(0, 2);
    });

    // 離開欄位時自動補零
    [monthInput, dayInput].forEach(input => {
        input.addEventListener('blur', function() {
            if(this.value && this.value.length === 1) {
                this.value = '0' + this.value;
            }
        });
    });

    function getRoot(n1, n2) {
        let sum = parseInt(n1) + parseInt(n2);
        if (sum === 0) return 5;
        while (sum > 9) sum = Math.floor(sum / 10) + (sum % 10);
        return sum;
    }

    function calculateAndDraw() {
        const y = yearInput.value;
        const m = monthInput.value.padStart(2, '0');
        const d = dayInput.value.padStart(2, '0');
        const name = document.getElementById('userName').value;

        if(!y || !monthInput.value || !dayInput.value) {
            alert("請填寫完整的年、月、日");
            return;
        }

        // 公式定義：A, B = 日期 | C, D = 月份 | E, F = 年前二 | G, H = 年後二
        const A = d[0], B = d[1];
        const C = m[0], D = m[1];
        const E = y[0], F = y[1];
        const G = y[2], H = y[3];

        const I = getRoot(A, B);
        const J = getRoot(C, D);
        const K = getRoot(E, F);
        const L = getRoot(G, H);
        
        const M = getRoot(I, J);
        const N = getRoot(K, L);
        const O = getRoot(M, N);
        const Q = getRoot(N, O);
        const P = getRoot(M, O);
        const R = getRoot(Q, P);
        const X = getRoot(I, M);
        const W = getRoot(J, M);
        const S = getRoot(X, W);
        const V = getRoot(K, N);
        const U = getRoot(L, N);
        const T = getRoot(V, U);

        draw({name, A,B,C,D,E,F,G,H, I,J,K,L, M,N,O,P,Q,R,S,T,U,V,W,X});
    }

    function draw(data) {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.fillStyle = "white";
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        // 1. 背景「全」
        ctx.fillStyle = "#F2EBF7"; 
        ctx.textAlign = "center";
        ctx.font = "bold 800px 'Microsoft JhengHei'";
        ctx.fillText("全", 450, 900);

        // 2. 姓名 (左上)
        if(data.name) {
            ctx.textAlign = "left";
            ctx.fillStyle = "#333";
            ctx.font = "bold 32px 'Microsoft JhengHei'";
            ctx.fillText(`姓名：${data.name}`, 40, 60);
        }

        // 3. 圓圈座標
        const nodes = {
            R: [450, 100], Q: [360, 220], P: [540, 220],
            O: [450, 400], 
            M: [360, 680], N: [540, 680],
            I: [220, 950], J: [360, 950], K: [540, 950], L: [680, 950],
            X: [180, 360], W: [280, 360], S: [80, 360],
            V: [620, 360], U: [720, 360], T: [820, 360]
        };

        ctx.textAlign = "center";
        Object.keys(nodes).forEach(key => {
            const [x, y] = nodes[key];
            const isMain = (key === 'O');
            
            ctx.beginPath();
            ctx.arc(x, y, 40, 0, Math.PI * 2);
            ctx.strokeStyle = isMain ? "#4CAF50" : "#6a2c91";
            ctx.lineWidth = 4;
            ctx.stroke();
            ctx.fillStyle = "white";
            ctx.fill();

            ctx.font = "bold 42px Arial";
            ctx.fillStyle = isMain ? "#2d5a27" : "#6a2c91";
            ctx.fillText(data[key], x, y + 15);

            if(key === 'O') {
                ctx.fillStyle = "red"; ctx.fillText("★", x, y - 55);
            } else if(key === 'I') {
                ctx.fillStyle = "red"; ctx.fillText("★", x - 65, y + 10);
            } else if(key === 'L') {
                ctx.fillStyle = "red"; ctx.fillText("★", x + 65, y + 10);
            }
        });

        // 4. 等於符號
        ctx.font = "bold 35px Arial";
        ctx.fillStyle = "#6a2c91";
        ctx.fillText("=", 130, 370); 
        ctx.fillText("=", 770, 370); 

        // 5. 底部數字
        ctx.fillStyle = "#6a2c91";
        ctx.font = "bold 45px Arial";
        ctx.fillText(`${data.A}${data.B}`, 260, 1120); 
        ctx.fillText(`${data.C}${data.D}`, 410, 1120); 
        ctx.fillText(`${data.E}${data.F}`, 560, 1120); 
        ctx.fillText(`${data.G}${data.H}`, 690, 1120); 
        
        ctx.font = "24px 'Microsoft JhengHei'";
        ctx.fillStyle = "#888";
        ctx.fillText("Asia Training 生命密碼全息圖", 450, 1170);
    }

    function clearFields() {
        document.getElementById('userName').value = '';
        yearInput.value = '';
        monthInput.value = '';
        dayInput.value = '';
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        // 重新繪製空白「全」字
        ctx.fillStyle = "#F2EBF7"; 
        ctx.textAlign = "center";
        ctx.font = "bold 800px 'Microsoft JhengHei'";
        ctx.fillText("全", 450, 900);
    }

    function saveImage() {
        const name = document.getElementById('userName').value || '未命名';
        html2canvas(document.querySelector("#captureArea")).then(canvas => {
            const link = document.createElement('a');
            link.download = `生命密碼_${name}.png`;
            link.href = canvas.toDataURL();
            link.click();
        });
    }

    // 初始化顯示
    window.onload = () => {
        ctx.fillStyle = "#F2EBF7"; 
        ctx.textAlign = "center";
        ctx.font = "bold 800px 'Microsoft JhengHei'";
        ctx.fillText("全", 450, 900);
    };
</script>
</body>
</html>
