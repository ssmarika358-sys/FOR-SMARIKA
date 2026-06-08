<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Heart - Smarika</title>
    <style>
        body {
            margin: 0;
            height: 100vh;
            background-color: #0b0b0b;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        #hc {
            position: relative;
            width: 560px;
            height: 560px;
            background: #0b0b0b;
            border-radius: 12px;
            overflow: hidden;
        }

        .hl {
            position: absolute;
            color: #ff3366;
            font-weight: bold;
            font-size: 11px;
            user-select: none;
            font-family: Arial, sans-serif;
        }

        #cm {
            position: absolute;
            top: 45%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0);
            color: #fff;
            font-size: 40px;
            font-weight: 900;
            white-space: nowrap;
            opacity: 0;
            z-index: 10;
            font-family: Arial, sans-serif;
            transition: all 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.5);
            text-shadow: 0 0 15px #ff3366, 0 0 30px #ff3366;
        }

        #cm.pop {
            opacity: 1;
            transform: translate(-50%, -50%) scale(1);
            animation: pulse 1.5s infinite alternate ease-in-out;
        }

        @keyframes pulse {
            0% { transform: translate(-50%, -50%) scale(1); }
            100% { transform: translate(-50%, -50%) scale(1.08); }
        }
    </style>
</head>
<body>

    <div id="hc">
        <div id="cm">SMARIKA</div>
    </div>

    <script>
        const container = document.getElementById('hc');
        const centerMessage = document.getElementById('cm');
        const phrase = "ILOVEYOU";
        let phraseIndex = 0;
        const centerX = 280, centerY = 260;
        const lettersArray = [];

        for (let y = 1.5; y >= -1.5; y -= 0.09) {
            for (let x = -1.5; x <= 1.5; x += 0.045) {
                if (Math.pow(x*x + y*y - 1, 3) - x*x * y*y*y <= 0) {
                    const targetX = centerX + x * 150;
                    const targetY = centerY - y * 150;
                    const el = document.createElement('div');
                    el.className = 'hl';
                    el.innerText = phrase[phraseIndex++ % phrase.length];
                    el.style.left = targetX + 'px';
                    el.style.top = targetY + 'px';
                    el.style.opacity = '0';
                    container.appendChild(el);
                    lettersArray.push({ element: el, targetX, targetY });
                }
            }
        }

        lettersArray.sort(() => Math.random() - 0.5);

        function runCycle() {
            lettersArray.forEach((item, i) => {
                const el = item.element;
                const startX = Math.random() < 0.5 ? -Math.random()*300 : 860 + Math.random()*300;
                const startY = Math.random() < 0.5 ? -Math.random()*300 : 860 + Math.random()*300;
                el.style.transition = 'none';
                el.style.opacity = '0';
                el.style.transform = `translate(${startX - item.targetX}px, ${startY - item.targetY}px)`;

                setTimeout(() => {
                    el.style.transition = 'transform 2.5s cubic-bezier(0.175, 0.885, 0.32, 1.15), opacity 1.5s ease';
                    el.style.opacity = '1';
                    el.style.transform = 'translate(0, 0)';
                }, 50 + i * 2);
            });

            const cycleTime = 50 + lettersArray.length * 2 + 2800;

            setTimeout(() => {
                centerMessage.classList.add('pop');
            }, cycleTime - 1200);

            setTimeout(() => {
                centerMessage.classList.remove('pop');
                centerMessage.style.transition = 'none';
                centerMessage.style.opacity = '0';
                centerMessage.style.transform = 'translate(-50%,-50%) scale(0)';
                setTimeout(() => {
                    centerMessage.style.transition = 'all 0.8s cubic-bezier(0.175, 0.885, 0.32, 1.5)';
                }, 50);

                lettersArray.forEach(item => {
                    item.element.style.transition = 'opacity 0.8s ease';
                    item.element.style.opacity = '0';
                });

                setTimeout(runCycle, 1200);
            }, cycleTime + 1500);
        }

        setTimeout(runCycle, 400);
    </script>

</body>
</html>
