
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Kardashev Tipo II - Civilização Estelar & Esfera de Dyson</title>
    <style>
        :root {
            --bg-color: #02040a;
            --panel-bg: rgba(10, 15, 30, 0.90);
            --border-color: rgba(245, 158, 11, 0.35);
            --accent-gold: #f59e0b;
            --accent-cyan: #38bdf8;
            --accent-green: #10b981;
            --accent-purple: #a855f7;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
            user-select: none;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-main);
            height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
        }

        header {
            height: 60px;
            background: linear-gradient(180deg, rgba(15, 23, 42, 0.95) 0%, rgba(2, 4, 10, 0.8) 100%);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 25px;
            z-index: 10;
        }

        header h1 {
            font-size: 1.05rem;
            letter-spacing: 2px;
            color: var(--accent-gold);
            text-transform: uppercase;
        }

        .status-badge {
            font-size: 0.75rem;
            padding: 4px 12px;
            border-radius: 12px;
            background: rgba(245, 158, 11, 0.15);
            border: 1px solid var(--accent-gold);
            color: var(--accent-gold);
            letter-spacing: 1px;
        }

        .main-container {
            display: grid;
            grid-template-columns: 380px 1fr;
            height: calc(100vh - 60px);
            position: relative;
        }

        /* PAINEL LATERAL DE CONTROLE */
        .control-panel {
            background: var(--panel-bg);
            border-right: 1px solid var(--border-color);
            backdrop-filter: blur(12px);
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 16px;
            overflow-y: auto;
            z-index: 5;
        }

        .kardashev-box {
            background: radial-gradient(circle, rgba(245, 158, 11, 0.2) 0%, rgba(2, 4, 10, 0.8) 100%);
            border: 1px solid var(--accent-gold);
            border-radius: 10px;
            padding: 16px;
            text-align: center;
            box-shadow: 0 0 25px rgba(245, 158, 11, 0.2);
        }

        .kardashev-score {
            font-size: 2.4rem;
            font-family: monospace;
            font-weight: bold;
            color: #fff;
            text-shadow: 0 0 15px var(--accent-gold);
            margin: 4px 0;
        }

        .section-title {
            font-size: 0.78rem;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            color: var(--accent-gold);
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 6px;
        }

        .metric-group {
            display: flex;
            flex-direction: column;
            gap: 6px;
        }

        .metric-header {
            display: flex;
            justify-content: space-between;
            font-size: 0.8rem;
        }

        .metric-value {
            font-family: monospace;
            font-weight: bold;
            color: var(--accent-gold);
        }

        input[type="range"] {
            width: 100%;
            height: 6px;
            border-radius: 3px;
            background: rgba(255, 255, 255, 0.1);
            outline: none;
            accent-color: var(--accent-gold);
            cursor: pointer;
        }

        .toggle-box {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background: rgba(255, 255, 255, 0.03);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 6px;
            padding: 8px 12px;
            font-size: 0.78rem;
        }

        .switch {
            position: relative;
            display: inline-block;
            width: 36px;
            height: 18px;
        }

        .switch input { opacity: 0; width: 0; height: 0; }

        .slider {
            position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0;
            background-color: rgba(255,255,255,0.2); transition: .3s; border-radius: 18px;
        }

        .slider:before {
            position: absolute; content: ""; height: 12px; width: 12px; left: 3px; bottom: 3px;
            background-color: white; transition: .3s; border-radius: 50%;
        }

        input:checked + .slider { background-color: var(--accent-gold); }
        input:checked + .slider:before { transform: translateX(18px); }

        .info-card {
            background: rgba(0, 0, 0, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.08);
            border-radius: 8px;
            padding: 12px;
            font-size: 0.8rem;
            line-height: 1.45;
            color: var(--text-muted);
        }

        .info-card strong {
            color: var(--text-main);
        }

        /* VIEWPORT CANVAS SIMULAÇÃO */
        .viewport {
            position: relative;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, #0a0e1a 0%, #02040a 100%);
            overflow: hidden;
        }

        canvas {
            width: 100%;
            height: 100%;
            display: block;
        }

        .hud-overlay {
            position: absolute;
            bottom: 20px;
            right: 20px;
            background: var(--panel-bg);
            border: 1px solid var(--border-color);
            border-radius: 8px;
            padding: 15px;
            font-family: monospace;
            font-size: 0.75rem;
            pointer-events: none;
            display: flex;
            flex-direction: column;
            gap: 6px;
            backdrop-filter: blur(10px);
        }

        .hud-line {
            display: flex;
            justify-content: space-between;
            gap: 20px;
        }

        ::-webkit-scrollbar { width: 5px; }
        ::-webkit-scrollbar-track { background: transparent; }
        ::-webkit-scrollbar-thumb { background: var(--border-color); border-radius: 3px; }
    </style>
</head>
<body>

    <header>
        <h1>CIVILIZAÇÃO TIPO II: CAPTURA ESTELAR TOTAL (ESFERA DE DYSON)</h1>
        <div class="status-badge">SISTEMA SOLAR INTERNO: ESTÁVEL</div>
    </header>

    <div class="main-container">
        <!-- PAINEL LATERAL -->
        <aside class="control-panel">
            <div class="kardashev-box">
                <div style="font-size: 0.72rem; color: var(--text-muted); text-transform: uppercase;">Métrica de Kardashev</div>
                <div class="kardashev-score" id="k-score">K 2.000</div>
                <div style="font-size: 0.78rem; color: var(--accent-gold);" id="power-display">1.00 × 10²⁶ W</div>
            </div>

            <div class="section-title">1. Enxame e Captura Solar</div>
            
            <div class="metric-group">
                <div class="metric-header">
                    <span>Fluxo Energético Capturado (10²⁶ W)</span>
                    <span class="metric-value" id="val-power">1.0 × 10²⁶ W</span>
                </div>
                <input type="range" id="input-power" min="1.0" max="10.0" step="0.1" value="1.0">
            </div>

            <div class="metric-group">
                <div class="metric-header">
                    <span>Densidade de Satélites do Enxame</span>
                    <span class="metric-value" id="val-density">100%</span>
                </div>
                <input type="range" id="input-density" min="20" max="100" value="100">
            </div>

            <div class="section-title">2. Infraestrutura e Megasestruturas</div>

            <div class="toggle-box">
                <span>Feixes de Transmissão por Laser/Micro-ondas</span>
                <label class="switch"><input type="checkbox" id="sw-beams" checked><span class="slider"></span></label>
            </div>

            <div class="toggle-box">
                <span>Escudo Planetário Interceptador de Meteoros</span>
                <label class="switch"><input type="checkbox" id="sw-defense" checked><span class="slider"></span></label>
            </div>

            <div class="toggle-box">
                <span>Cérebro Matrioshka (Supercomputador)</span>
                <label class="switch"><input type="checkbox" id="sw-matrioshka" checked><span class="slider"></span></label>
            </div>

            <div class="section-title">3. Diagnóstico de Capacidades</div>
            <div class="info-card">
                <strong>Status de Capacidade Tecnológica:</strong><br>
                • <strong>Imunidade Extintiva:</strong> Defesas ativas contra cometas, eras glaciais e erupções solares.<br>
                • <strong>Poder Computacional:</strong> ~<span id="comp-flops">1.0 × 10⁴⁵</span> FLOPS (Processamento Computronium).<br>
                • <strong>Colonização Planetária:</strong> Terraformação ativa concluída em Marte e na Lua.
            </div>
        </aside>

        <!-- VIEWPORT CANVAS SIMULAÇÃO -->
        <main class="viewport">
            <canvas id="simCanvas"></canvas>

            <div class="hud-overlay">
                <div class="hud-line"><span>RADIAÇÃO SOLAR RETIDA:</span><span id="hud-capture" style="color: var(--accent-gold)">100%</span></div>
                <div class="hud-line"><span>AMEAÇAS EXTERNAS:</span><span id="hud-defense" style="color: var(--accent-green)">NEUTRALIZADAS (100%)</span></div>
                <div class="hud-line"><span>PROCESSAMENTO VIRTUAL:</span><span id="hud-comp" style="color: var(--accent-purple)">ILIMITADO</span></div>
                <div class="hud-line"><span>HORIZONTE TEMPORAL:</span><span style="color: var(--accent-cyan)">ANO ~3500+</span></div>
            </div>
        </main>
    </div>

    <script>
        const canvas = document.getElementById('simCanvas');
        const ctx = canvas.getContext('2d');

        // Estado da Simulação
        const state = {
            powerWatts: 1e26,
            densityPercent: 100,
            kardashevScale: 2.0,
            showBeams: true,
            defenseActive: true,
            matrioshkaActive: true,
            rotationAngle: 0,
            pulse: 0
        };

        function resizeCanvas() {
            canvas.width = canvas.parentElement.clientWidth;
            canvas.height = canvas.parentElement.clientHeight;
        }
        window.addEventListener('resize', resizeCanvas);
        resizeCanvas();

        // Elementos de Entrada
        const inputPower = document.getElementById('input-power');
        const inputDensity = document.getElementById('input-density');
        const swBeams = document.getElementById('sw-beams');
        const swDefense = document.getElementById('sw-defense');
        const swMatrioshka = document.getElementById('sw-matrioshka');

        // Geração de Coletores da Esfera / Enxame de Dyson
        const dysonSwarm = [];
        const MAX_SWARM = 280;
        for (let i = 0; i < MAX_SWARM; i++) {
            dysonSwarm.push({
                radius: 80 + Math.random() * 65,
                angle: Math.random() * Math.PI * 2,
                speed: (0.002 + Math.random() * 0.004) * (Math.random() > 0.5 ? 1 : -1),
                size: 2 + Math.random() * 2.5,
                inclination: (Math.random() - 0.5) * 0.6
            });
        }

        // Asteroides de Teste para o Sistema de Defesa
        const asteroids = [];
        for (let i = 0; i < 4; i++) {
            asteroids.push({
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height,
                vx: (Math.random() - 0.5) * 1.2,
                vy: (Math.random() - 0.5) * 1.2,
                destroyed: false,
                laserTimer: 0
            });
        }

        // Atualização dos Cálculos Físicos e Matemáticos
        function updatePhysics() {
            const mult = parseFloat(inputPower.value); // 1.0 a 10.0
            state.powerWatts = mult * 1e26; // 10^26 a 10^27 W

            state.densityPercent = parseInt(inputDensity.value);

            // Fórmula de Kardashev: K = (log10(P) - 6) / 10
            state.kardashevScale = (Math.log10(state.powerWatts) - 6) / 10;

            state.showBeams = swBeams.checked;
            state.defenseActive = swDefense.checked;
            state.matrioshkaActive = swMatrioshka.checked;

            // Interface
            document.getElementById('k-score').innerText = `K ${state.kardashevScale.toFixed(3)}`;
            
            const displayVal = (state.powerWatts / 1e26).toFixed(2);
            document.getElementById('power-display').innerText = `${displayVal} × 10²⁶ W`;
            document.getElementById('val-power').innerText = `${(mult * 1.0).toFixed(1)} × 10²⁶ W`;
            document.getElementById('val-density').innerText = `${state.densityPercent}%`;

            // FLOPS
            const flopsVal = (mult * 1.0).toFixed(1);
            document.getElementById('comp-flops').innerText = `${flopsVal} × 10⁴⁵`;

            // HUD
            document.getElementById('hud-capture').innerText = `${state.densityPercent}%`;
            
            const defHUD = document.getElementById('hud-defense');
            defHUD.innerText = state.defenseActive ? 'NEUTRALIZADAS (100%)' : 'DESATIVADO (VULNERÁVEL)';
            defHUD.style.color = state.defenseActive ? 'var(--accent-green)' : '#ef4444';

            const compHUD = document.getElementById('hud-comp');
            compHUD.innerText = state.matrioshkaActive ? 'ILIMITADO (CÉREBRO MATRIOSHKA)' : 'CONVENCIONAL';
            compHUD.style.color = state.matrioshkaActive ? 'var(--accent-purple)' : 'var(--text-muted)';
        }

        // Eventos
        inputPower.addEventListener('input', updatePhysics);
        inputDensity.addEventListener('input', updatePhysics);
        swBeams.addEventListener('change', updatePhysics);
        swDefense.addEventListener('change', updatePhysics);
        swMatrioshka.addEventListener('change', updatePhysics);

        // Loop de Renderização no Canvas
        function render() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            const cx = canvas.width / 2;
            const cy = canvas.height / 2;
            const sunRadius = 45;

            state.rotationAngle += 0.002;
            state.pulse += 0.03;

            // 1. Grade de Fundo Sci-Fi
            ctx.strokeStyle = 'rgba(245, 158, 11, 0.03)';
            ctx.lineWidth = 1;
            const gridSize = 50;
            for (let x = 0; x < canvas.width; x += gridSize) {
                ctx.beginPath(); ctx.moveTo(x, 0); ctx.lineTo(x, canvas.height); ctx.stroke();
            }
            for (let y = 0; y < canvas.height; y += gridSize) {
                ctx.beginPath(); ctx.moveTo(0, y); ctx.lineTo(canvas.width, y); ctx.stroke();
            }

            // 2. Órbitas Planetárias
            const orbitEarthRadius = 240;
            const orbitMarsRadius = 310;

            ctx.strokeStyle = 'rgba(255, 255, 255, 0.06)';
            ctx.lineWidth = 1;
            ctx.beginPath(); ctx.arc(cx, cy, orbitEarthRadius, 0, Math.PI * 2); ctx.stroke();
            ctx.beginPath(); ctx.arc(cx, cy, orbitMarsRadius, 0, Math.PI * 2); ctx.stroke();

            // 3. Estrela Hospedeira (Sol)
            const sunGrad = ctx.createRadialGradient(cx, cy, 10, cx, cy, sunRadius + 15);
            sunGrad.addColorStop(0, '#ffffff');
            sunGrad.addColorStop(0.3, '#fef08a');
            sunGrad.addColorStop(0.7, '#f59e0b');
            sunGrad.addColorStop(1, 'rgba(245, 158, 11, 0)');

            ctx.beginPath();
            ctx.arc(cx, cy, sunRadius + 15, 0, Math.PI * 2);
            ctx.fillStyle = sunGrad;
            ctx.fill();

            // 4. Camadas do Cérebro Matrioshka (Se Ativo)
            if (state.matrioshkaActive) {
                for (let r = 0; r < 3; r++) {
                    const mRadius = sunRadius + 22 + r * 12;
                    ctx.beginPath();
                    ctx.arc(cx, cy, mRadius, 0, Math.PI * 2);
                    ctx.strokeStyle = `rgba(168, 85, 247, ${0.25 - r * 0.06 + Math.sin(state.pulse + r) * 0.05})`;
                    ctx.lineWidth = 1.5;
                    ctx.setLineDash([4, 6]);
                    ctx.stroke();
                    ctx.setLineDash([]);
                }
            }

            // 5. Enxame de Dyson (Satélites Coletores Orbitais)
            const activeSwarmCount = Math.floor((state.densityPercent / 100) * MAX_SWARM);
            for (let i = 0; i < activeSwarmCount; i++) {
                const sat = dysonSwarm[i];
                sat.angle += sat.speed;

                const x = cx + Math.cos(sat.angle) * sat.radius;
                const y = cy + Math.sin(sat.angle) * (sat.radius * (1 + sat.inclination * 0.3));

                ctx.beginPath();
                ctx.arc(x, y, sat.size, 0, Math.PI * 2);
                ctx.fillStyle = '#fbbf24';
                ctx.shadowColor = '#f59e0b';
                ctx.shadowBlur = 6;
                ctx.fill();
                ctx.shadowBlur = 0;

                // Conexões Energéticas do Enxame
                if (i % 8 === 0 && state.showBeams) {
                    ctx.beginPath();
                    ctx.moveTo(cx, cy);
                    ctx.lineTo(x, y);
                    ctx.strokeStyle = `rgba(245, 158, 11, ${0.12 + Math.sin(state.pulse + i) * 0.05})`;
                    ctx.lineWidth = 0.8;
                    ctx.stroke();
                }
            }

            // 6. Planetas Colonizados
            // Terra
            const earthAngle = state.rotationAngle * 0.8;
            const ex = cx + Math.cos(earthAngle) * orbitEarthRadius;
            const ey = cy + Math.sin(earthAngle) * orbitEarthRadius;

            // Escudo Planetário da Terra
            if (state.defenseActive) {
                ctx.beginPath();
                ctx.arc(ex, ey, 14, 0, Math.PI * 2);
                ctx.strokeStyle = `rgba(56, 189, 248, ${0.5 + Math.sin(state.pulse) * 0.2})`;
                ctx.lineWidth = 1.5;
                ctx.stroke();
            }

            ctx.beginPath();
            ctx.arc(ex, ey, 9, 0, Math.PI * 2);
            ctx.fillStyle = '#38bdf8';
            ctx.shadowColor = '#38bdf8';
            ctx.shadowBlur = 10;
            ctx.fill();
            ctx.shadowBlur = 0;

            // Marte (Terraformado)
            const marsAngle = state.rotationAngle * 0.5 + 2;
            const mx = cx + Math.cos(marsAngle) * orbitMarsRadius;
            const my = cy + Math.sin(marsAngle) * orbitMarsRadius;

            ctx.beginPath();
            ctx.arc(mx, my, 7, 0, Math.PI * 2);
            ctx.fillStyle = '#10b981'; // Verde indicando terraformação
            ctx.shadowColor = '#10b981';
            ctx.shadowBlur = 8;
            ctx.fill();
            ctx.shadowBlur = 0;

            // 7. Feixes Principais de Transmissão Estelar para os Planetas
            if (state.showBeams) {
                // Feixe para a Terra
                ctx.beginPath();
                ctx.moveTo(cx, cy);
                ctx.lineTo(ex, ey);
                ctx.strokeStyle = `rgba(56, 189, 248, ${0.35 + Math.sin(state.pulse * 1.5) * 0.15})`;
                ctx.lineWidth = 2;
                ctx.stroke();

                // Feixe para Marte
                ctx.beginPath();
                ctx.moveTo(cx, cy);
                ctx.lineTo(mx, my);
                ctx.strokeStyle = `rgba(16, 185, 129, ${0.35 + Math.sin(state.pulse * 1.5) * 0.15})`;
                ctx.lineWidth = 1.5;
                ctx.stroke();
            }

            // 8. Simulação de Defesa Ativa Contra Asteroides
            asteroids.forEach(ast => {
                ast.x += ast.vx;
                ast.y += ast.vy;

                // Loop nas bordas
                if (ast.x < 0) ast.x = canvas.width;
                if (ast.x > canvas.width) ast.x = 0;
                if (ast.y < 0) ast.y = canvas.height;
                if (ast.y > canvas.height) ast.y = 0;

                // Desenhar Asteroide
                ctx.beginPath();
                ctx.arc(ast.x, ast.y, 4, 0, Math.PI * 2);
                ctx.fillStyle = '#94a3b8';
                ctx.fill();

                // Interceptação por Laser da Esfera de Dyson se estiver próximo e a defesa ativa
                const distToEarth = Math.hypot(ast.x - ex, ast.y - ey);
                if (state.defenseActive && distToEarth < 180) {
                    ctx.beginPath();
                    ctx.moveTo(ex, ey);
                    ctx.lineTo(ast.x, ast.y);
                    ctx.strokeStyle = '#ef4444';
                    ctx.lineWidth = 1.5;
                    ctx.stroke();

                    // Explosão / Neutralização
                    ctx.beginPath();
                    ctx.arc(ast.x, ast.y, 8, 0, Math.PI * 2);
                    ctx.fillStyle = 'rgba(239, 68, 68, 0.5)';
                    ctx.fill();
                }
            });

            requestAnimationFrame(render);
        }

        // Inicialização
        updatePhysics();
        render();
    </script>
</body>
</html>
