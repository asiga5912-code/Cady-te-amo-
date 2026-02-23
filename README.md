<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Para Candy - Carta de Amor</title>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@400;700&family=Playfair+Display:ital,wght@0,400;0,700;1,400&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Playfair Display', serif;
            overflow: hidden;
            background: #0f0f0f;
        }

        .book-container {
            width: 100vw;
            height: 100vh;
            position: relative;
            perspective: 1500px;
        }

        .page {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            opacity: 0;
            transform: translateX(100%) rotateY(-90deg);
            transition: all 1.2s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
        }

        .page.active {
            opacity: 1;
            transform: translateX(0) rotateY(0);
            z-index: 10;
        }

        .page.prev {
            opacity: 0;
            transform: translateX(-100%) rotateY(90deg);
            z-index: 5;
        }

        .page-content {
            background: rgba(255, 255, 255, 0.95);
            padding: 50px;
            max-width: 600px;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            text-align: center;
            position: relative;
            overflow: hidden;
            animation: float 6s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }

        .page-content::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(255,182,193,0.1), transparent);
            transform: rotate(45deg);
            animation: shimmer 3s infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%) translateY(-100%) rotate(45deg); }
            100% { transform: translateX(100%) translateY(100%) rotate(45deg); }
        }

        h1 {
            font-family: 'Dancing Script', cursive;
            font-size: 3.5em;
            color: #d63384;
            margin-bottom: 20px;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }

        .subtitle {
            font-size: 1.2em;
            color: #6c757d;
            font-style: italic;
            margin-bottom: 30px;
        }

        .letter-text {
            font-size: 1.3em;
            line-height: 1.8;
            color: #333;
            text-align: left;
            margin: 20px 0;
            font-style: italic;
        }

        .signature {
            font-family: 'Dancing Script', cursive;
            font-size: 2em;
            color: #d63384;
            margin-top: 30px;
            text-align: right;
        }

        .heart {
            color: #e74c3c;
            font-size: 1.5em;
            animation: heartbeat 1.5s ease-in-out infinite;
            display: inline-block;
        }

        @keyframes heartbeat {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.2); }
        }

        .controls {
            position: fixed;
            bottom: 40px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 20px;
            z-index: 100;
        }

        .btn {
            background: linear-gradient(135deg, #ff6b6b, #ee5a6f);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 50px;
            font-size: 1.1em;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(238, 90, 111, 0.4);
            font-family: 'Playfair Display', serif;
        }

        .btn:hover {
            transform: translateY(-3px) scale(1.05);
            box-shadow: 0 6px 20px rgba(238, 90, 111, 0.6);
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .page-indicator {
            position: fixed;
            top: 30px;
            right: 30px;
            background: rgba(255,255,255,0.9);
            padding: 10px 20px;
            border-radius: 20px;
            font-size: 0.9em;
            color: #d63384;
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .floating-hearts {
            position: fixed;
            width: 100%;
            height: 100%;
            pointer-events: none;
            overflow: hidden;
            z-index: 1;
        }

        .floating-heart {
            position: absolute;
            color: rgba(255, 182, 193, 0.6);
            font-size: 20px;
            animation: floatUp 10s linear infinite;
            bottom: -100px;
        }

        @keyframes floatUp {
            0% {
                transform: translateY(0) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(-100vh) rotate(720deg);
                opacity: 0;
            }
        }

        .special-message {
            background: linear-gradient(135deg, #ffeef8, #ffe4e1);
            padding: 20px;
            border-radius: 15px;
            margin: 20px 0;
            border-left: 5px solid #d63384;
        }

        .photo-frame {
            width: 200px;
            height: 200px;
            border-radius: 50%;
            border: 5px solid white;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            margin: 20px auto;
            overflow: hidden;
            position: relative;
        }

        .photo-frame img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .quote {
            font-family: 'Dancing Script', cursive;
            font-size: 1.8em;
            color: #8b4513;
            margin: 20px 0;
            line-height: 1.4;
        }

        /* Efecto de partículas de amor */
        .particles {
            position: absolute;
            width: 100%;
            height: 100%;
            overflow: hidden;
            pointer-events: none;
        }

        .particle {
            position: absolute;
            width: 6px;
            height: 6px;
            background: #ff69b4;
            border-radius: 50%;
            animation: particleFloat 15s infinite;
        }

        @keyframes particleFloat {
            0%, 100% {
                transform: translateY(100vh) scale(0);
                opacity: 0;
            }
            10% {
                opacity: 1;
            }
            90% {
                opacity: 1;
            }
            100% {
                transform: translateY(-100vh) scale(1);
                opacity: 0;
            }
        }
    </style>
</head>
<body>

    <div class="floating-hearts" id="floatingHearts"></div>

    <div class="page-indicator">
        Página <span id="currentPage">1</span> de <span id="totalPages">5</span>
    </div>

    <div class="book-container">

        <!-- Página 1: Portada -->
        <div class="page active" style="background-image: url('https://kimi-web-img.moonshot.cn/img/images.stockcake.com/0940b03cf9e3e13d135678fe9570461d1e8b75c9.jpg');">
            <div class="particles" id="particles1"></div>
            <div class="page-content">
                <h1>Para Candy</h1>
                <div class="subtitle">Una carta escrita desde lo más profundo de mi corazón</div>
                <div style="font-size: 4em; margin: 20px 0;">💌</div>
                <p style="font-style: italic; color: #666;">"El amor no se mira, se siente, y aún más cuando estás tú"</p>
                <p style="margin-top: 30px; font-size: 0.9em; color: #999;">Desliza para comenzar esta historia de amor...</p>
            </div>
        </div>

        <!-- Página 2: El Comienzo -->
        <div class="page" style="background-image: url('https://kimi-web-img.moonshot.cn/img/images.stockcake.com/9fbabc4aeff6c4886ca2c7db249d35f4ca0729b2.jpg');">
            <div class="particles" id="particles2"></div>
            <div class="page-content">
                <h1>El Momento Que Todo Cambió</h1>
                <div class="letter-text">
                    <p>Querida Candy,</p>
                    <p>Desde el primer instante en que te vi, supe que mi vida nunca volvería a ser la misma. No fue solo tu sonrisa, ni tu mirada, fue esa sensación de que el universo finalmente había alineado todas las piezas.</p>
                    <p>Contigo descubrí que el amor no es solo un sentimiento, es un hogar donde siempre quiero estar.</p>
                </div>
                <div class="special-message">
                    <span class="heart">❤️</span> Eres mi persona favorita en todo el mundo <span class="heart">❤️</span>
                </div>
                <div class="signature">Con todo mi amor,<br>Tu persona enamorada</div>
            </div>
        </div>

        <!-- Página 3: Lo Que Siento -->
        <div class="page" style="background-image: url('https://kimi-web-img.moonshot.cn/img/images.stockcake.com/ef944252e74c6bc42905b2515cf9d3a1667bd9b9.jpg');">
            <div class="particles" id="particles3"></div>
            <div class="page-content">
                <h1>Palabras Que No Alcanzan</h1>
                <div class="letter-text">
                    <p>Candy, mi amor,</p>
                    <p>Intentar describir lo que siento por ti es como intentar contar las estrellas: sé que hay millones, pero cada una es única y especial.</p>
                    <p>Amo la forma en que iluminas cada habitación con tu presencia. Amo cómo tu risa es mi melodía favorita. Amo cómo, incluso en los días grises, tú eres mi arcoíris.</p>
                </div>
                <div class="quote">
                    "Eres el sueño que nunca quiero despertar,<br>
                    la realidad más hermosa que jamás imaginé."
                </div>
                <div style="margin-top: 20px; font-size: 2em;">✨ 💕 ✨</div>
            </div>
        </div>

        <!-- Página 4: Nuestro Futuro -->
        <div class="page" style="background-image: url('https://kimi-web-img.moonshot.cn/img/images.stockcake.com/f859b53d7d7d97a1864ca69668526182157c96b0.jpg');">
            <div class="particles" id="particles4"></div>
            <div class="page-content">
                <h1>Para Siempre Contigo</h1>
                <div class="letter-text">
                    <p>Mi dulce Candy,</p>
                    <p>No sé qué nos depara el futuro, pero sé una cosa con absoluta certeza: quiero vivirlo contigo. Quiero las aventuras y los días tranquilos, los desafíos y las victorias.</p>
                    <p>Quiero seguir eligiéndote cada mañana, cada día, cada momento de mi vida.</p>
                </div>
                <div class="special-message" style="background: linear-gradient(135deg, #e3f2fd, #f3e5f5);">
                    <strong>Promesa:</strong> Amarte en tu mejor versión, cuidarte en tu peor momento, y celebrar cada paso del camino juntos.
                </div>
                <div style="margin-top: 20px; font-size: 1.5em;">
                    🔮 💍 🏡 👶 🌅
                </div>
            </div>
        </div>

        <!-- Página 5: Cierre -->
        <div class="page" style="background-image: url('https://kimi-web-img.moonshot.cn/img/media.istockphoto.com/05534474bf4e86246e4e6d74ac52d328a8a94fba.jpg');">
            <div class="particles" id="particles5"></div>
            <div class="page-content" style="background: linear-gradient(135deg, rgba(255,255,255,0.98), rgba(255,240,245,0.98));">
                <h1>Eres Mi Todo</h1>
                <div style="font-size: 5em; margin: 20px 0; animation: heartbeat 1s infinite;">❤️</div>
                <div class="letter-text" style="text-align: center;">
                    <p>Candy,</p>
                    <p>Gracias por existir.<br>
                    Gracias por ser tú.<br>
                    Gracias por permitirme amarte.</p>
                    <p style="margin-top: 30px; font-size: 1.4em; color: #d63384; font-weight: bold;">
                        Te amo más de lo que las palabras pueden expresar,<br>
                        hoy y siempre.
                    </p>
                </div>
                <div class="signature" style="text-align: center; margin-top: 40px;">
                    Eternamente tuyo/a,<br>
                    <span style="font-size: 1.5em;">💋</span>
                </div>
                <div style="margin-top: 30px; font-size: 0.9em; color: #999; font-style: italic;">
                    "Y vivieron felices para siempre... <br>Empezando hoy."
                </div>
            </div>
        </div>

    </div>

    <div class="controls">
        <button class="btn" id="prevBtn" onclick="changePage(-1)" disabled>← Anterior</button>
        <button class="btn" id="nextBtn" onclick="changePage(1)">Siguiente →</button>
    </div>

    <script>
        let currentPageIndex = 0;
        const pages = document.querySelectorAll('.page');
        const totalPages = pages.length;

        document.getElementById('totalPages').textContent = totalPages;

        // Crear corazones flotantes
        function createFloatingHearts() {
            const container = document.getElementById('floatingHearts');
            setInterval(() => {
                const heart = document.createElement('div');
                heart.className = 'floating-heart';
                heart.innerHTML = '❤️';
                heart.style.left = Math.random() * 100 + '%';
                heart.style.animationDuration = (Math.random() * 5 + 5) + 's';
                heart.style.fontSize = (Math.random() * 20 + 10) + 'px';
                container.appendChild(heart);
                
                setTimeout(() => heart.remove(), 10000);
            }, 500);
        }

        // Crear partículas
        function createParticles(containerId) {
            const container = document.getElementById(containerId);
            for (let i = 0; i < 30; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.animationDelay = Math.random() * 15 + 's';
                particle.style.animationDuration = (Math.random() * 10 + 10) + 's';
                container.appendChild(particle);
            }
        }

        // Inicializar partículas en todas las páginas
        for (let i = 1; i <= 5; i++) {
            createParticles('particles' + i);
        }

        function updatePages() {
            pages.forEach((page, index) => {
                page.classList.remove('active', 'prev');
                if (index === currentPageIndex) {
                    page.classList.add('active');
                } else if (index < currentPageIndex) {
                    page.classList.add('prev');
                }
            });

            document.getElementById('currentPage').textContent = currentPageIndex + 1;
            document.getElementById('prevBtn').disabled = currentPageIndex === 0;
            document.getElementById('nextBtn').disabled = currentPageIndex === totalPages - 1;
            
            if (currentPageIndex === totalPages - 1) {
                document.getElementById('nextBtn').textContent = '💕 Fin 💕';
            } else {
                document.getElementById('nextBtn').textContent = 'Siguiente →';
            }
        }

        function changePage(direction) {
            const newIndex = currentPageIndex + direction;
            if (newIndex >= 0 && newIndex < totalPages) {
                currentPageIndex = newIndex;
                updatePages();
                
                // Efecto de sonido visual (confeti en la última página)
                if (currentPageIndex === totalPages - 1) {
                    createConfetti();
                }
            }
        }

        function createConfetti() {
            for (let i = 0; i < 50; i++) {
                setTimeout(() => {
                    const confetti = document.createElement('div');
                    confetti.style.position = 'fixed';
                    confetti.style.width = '10px';
                    confetti.style.height = '10px';
                    confetti.style.background = ['#ff6b6b', '#feca57', '#48dbfb', '#ff9ff3', '#54a0ff'][Math.floor(Math.random() * 5)];
                    confetti.style.left = Math.random() * 100 + '%';
                    confetti.style.top = '-10px';
                    confetti.style.borderRadius = '50%';
                    confetti.style.zIndex = '1000';
                    confetti.style.pointerEvents = 'none';
                    confetti.style.animation = `floatUp ${Math.random() * 3 + 2}s linear forwards`;
                    document.body.appendChild(confetti);
                    setTimeout(() => confetti.remove(), 5000);
                }, i * 50);
            }
        }

        // Navegación con teclado
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowRight') changePage(1);
            if (e.key === 'ArrowLeft') changePage(-1);
        });

        // Navegación táctil
        let touchStartX = 0;
        let touchEndX = 0;

        document.addEventListener('touchstart', (e) => {
            touchStartX = e.changedTouches[0].screenX;
        });

        document.addEventListener('touchend', (e) => {
            touchEndX = e.changedTouches[0].screenX;
            if (touchStartX - touchEndX > 50) changePage(1);
            if (touchEndX - touchStartX > 50) changePage(-1);
        });

        // Inicializar
        createFloatingHearts();
        updatePages();

        // Efecto de máquina de escribir en la primera página
        function typeWriter(element, text, speed = 50) {
            let i = 0;
            element.textContent = '';
            function type() {
                if (i < text.length) {
                    element.textContent += text.charAt(i);
                    i++;
                    setTimeout(type, speed);
                }
            }
            type();
        }
    </script>
</body>
</html>
