<head>
    <style>
        .container { display: flex; flex-direction: column; align-items: center; font-family: sans-serif; }
        #canvas { border: 5px solid #333; border-radius: 50%; margin: 20px; }
        .controls { margin-bottom: 20px; }
        button { padding: 10px 20px; font-size: 18px; cursor: pointer; background: #ff4757; color: white; border: none; border-radius: 5px; }
        textarea { width: 300px; height: 100px; margin-bottom: 10px; }
    </style>
</head>
<body>

<div class="container">
    <h2>MATIN</h2>
    <h2>កងវិលរើសអ្នកឈ្នះ</h2>
    <textarea id="names" placeholder="បញ្ចូលឈ្មោះ (ម្នាក់មួយបន្ទាត់)">អ្នកទី១&#10;អ្នកទី២&#10;អ្នកទី៣&#10;អ្នកទី៤</textarea>
    <div class="controls">
        <button onclick="spin()">វិលកង!</button>
    </div>
    <canvas id="canvas" width="400" height="400"></canvas>
    <h1 id="winner"></h1>
</div>

<script>
    const canvas = document.getElementById('canvas');
    const ctx = canvas.getContext('2d');
    let currentRotation = 0;

    function drawWheel(names) {
        const numSegments = names.length;
        const angleStep = (2 * Math.PI) / numSegments;
        const colors = ['#f1c40f', '#e67e22', '#e74c3c', '#9b59b6', '#3498db', '#2ecc71'];

        ctx.clearRect(0, 0, canvas.width, canvas.height);
        
        names.forEach((name, i) => {
            ctx.beginPath();
            ctx.fillStyle = colors[i % colors.length];
            ctx.moveTo(200, 200);
            ctx.arc(200, 200, 200, i * angleStep, (i + 1) * angleStep);
            ctx.fill();
            
            // ដាក់អក្សរឈ្មោះ
            ctx.save();
            ctx.translate(200, 200);
            ctx.rotate(i * angleStep + angleStep / 2);
            ctx.fillStyle = "white";
            ctx.font = "bold 16px Arial";
            ctx.fillText(name, 70, 5);
            ctx.restore();
        });
    }

    function spin() {
        const names = document.getElementById('names').value.split('\n').filter(n => n.trim() !== "");
        if (names.length < 2) return alert("សូមបញ្ចូលយ៉ាងតិច ២ ឈ្មោះ");

        const extraRotation = Math.random() * 360 + 1440; // វិលយ៉ាងតិច ៤ ជុំ
        currentRotation += extraRotation;
        
        canvas.style.transition = "transform 3s cubic-bezier(0.17, 0.67, 0.83, 0.67)";
        canvas.style.transform = `rotate(${currentRotation}deg)`;

        // បង្ហាញលទ្ធផលក្រោយវិលចប់
        setTimeout(() => {
            const actualDegree = currentRotation % 360;
            const index = names.length - 1 - Math.floor((actualDegree / 360) * names.length);
            document.getElementById('winner').innerText = "អ្នកឈ្នះគឺ៖ " + names[index];
        }, 3000);
        drawWheel(names);
    }

    // គូរលើកដំបូង
    drawWheel(document.getElementById('names').value.split('\n'));
</script>
