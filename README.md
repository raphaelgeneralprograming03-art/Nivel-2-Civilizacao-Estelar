
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulador Kardashev: Nível 2.0 - Civilização Estelar</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        :root {
            --bg-dark: #050811;
            --card-bg: #0d1322;
            --border-color: #1e293b;
            --accent-gold: #f59e0b;
            --accent-cyan: #06b6d4;
            --accent-purple: #8b5cf6;
        }

        body {
            background-color: var(--bg-dark);
            color: #f1f5f9;
            font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        .card {
            background-color: var(--card-bg);
            border: 1px solid var(--border-color);
        }

        .custom-slider-gold {
            accent-color: var(--accent-gold);
        }

        .glow-gold {
            box-shadow: 0 0 20px rgba(245, 158, 11, 0.2);
        }

        .solar-glow {
            animation: pulse-solar 3s infinite ease-in-out;
        }

        @keyframes pulse-solar {
            0%, 100% { box-shadow: 0 0 15px rgba(245, 158, 11, 0.3); }
            50% { box-shadow: 0 0 30px rgba(245, 158, 11, 0.6); }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col p-4 md:p-6">

    <!-- Header -->
    <header class="max-w-7xl mx-auto w-full mb-6 flex flex-col md:flex-row justify-between items-start md:items-center border-b border-gray-800 pb-4 gap-4">
        <div>
            <span class="text-xs font-mono uppercase tracking-widest text-amber-400">Simulador de Evolução de Civilização</span>
            <h1 class="text-2xl md:text-3xl font-bold flex items-center gap-3">
                Escala Kardashev: <span id="kardashevDisplay" class="text-amber-400 font-mono">2.000</span>
                <span class="text-xs px-2.5 py-1 rounded-full bg-amber-500/20 text-amber-300 border border-amber-500/40 font-semibold">Nível 2.0 - Civilização Estelar</span>
            </h1>
        </div>
        <div class="flex items-center gap-3">
            <button id="btnAdvance" onclick="advanceYear()" class="bg-amber-500 hover:bg-amber-400 text-gray-950 font-bold px-5 py-2.5 rounded-lg shadow-lg shadow-amber-500/20 transition cursor-pointer flex items-center gap-2">
                <span>Avançar 10 Anos</span>
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 5l7 7-7 7M5 5l7 7-7 7"></path></svg>
            </button>
            <button id="btnReset" onclick="resetSimulation()" class="bg-gray-800 hover:bg-gray-700 text-gray-300 px-3 py-2.5 rounded-lg text-sm transition cursor-pointer border border-gray-700">
                Reiniciar
            </button>
        </div>
    </header>

    <!-- Main Grid -->
    <main class="max-w-7xl mx-auto w-full grid grid-cols-1 lg:grid-cols-3 gap-6 flex-1">
        
        <!-- Left Column: Controls & Construction Engine -->
        <section class="space-y-6">
            <!-- Energy Metric -->
            <div class="card p-5 rounded-xl glow-gold solar-glow border-amber-500/30">
                <div class="flex justify-between items-center mb-2">
                    <h2 class="text-xs font-bold text-amber-400 uppercase tracking-wider">⚡ Captação Estelar</h2>
                    <span id="dysonCoverage" class="text-xs font-mono text-amber-300">12.5% do Sol</span>
                </div>
                <div class="text-2xl md:text-3xl font-mono font-bold text-amber-300 mb-1" id="wattsDisplay">
                    4.82 × 10²⁵ W
                </div>
                <p class="text-xs text-gray-400">Meta do Nível 2.0: Captura total de ~3.86 × 10²⁶ Watts.</p>
            </div>

            <!-- Controls -->
            <div class="card p-5 rounded-xl space-y-5">
                <h2 class="text-md font-bold text-amber-400 flex items-center gap-2">
                    <span>🛠️ Engenharia de Megaescala</span>
                </h2>
                
                <!-- Mercury Mining Slider -->
                <div>
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium text-gray-200">Mineração de Mercúrio (Von Neumann)</span>
                        <span id="mercuryVal" class="font-mono text-amber-400">25%</span>
                    </div>
                    <input type="range" id="mercurySlider" min="0" max="100" value="25" class="w-full custom-slider-gold" oninput="updateSimulation()">
                    <p class="text-xs text-gray-400 mt-1">Desmantelamento automatizado de Mercúrio para construção de satélites do Enxame.</p>
                </div>

                <!-- Star Lifting Slider -->
                <div>
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium text-gray-200">Levantamento Estelar (Star Lifting)</span>
                        <span id="starLiftingVal" class="font-mono text-amber-400">15%</span>
                    </div>
                    <input type="range" id="starLiftingSlider" min="0" max="100" value="15" class="w-full custom-slider-gold" oninput="updateSimulation()">
                    <p class="text-xs text-gray-400 mt-1">Extração magnética direta de hidrogênio/hélio da atmosfera solar.</p>
                </div>

                <!-- Matrioshka Brain Slider -->
                <div>
                    <div class="flex justify-between items-center text-sm mb-1">
                        <span class="font-medium text-gray-200">Cérebro de Matrioshka</span>
                        <span id="matrioshkaVal" class="font-mono text-purple-400">10%</span>
                    </div>
                    <input type="range" id="matrioshkaSlider" min="0" max="100" value="10" class="w-full custom-slider-gold" oninput="updateSimulation()">
                    <p class="text-xs text-gray-400 mt-1">Alocação de energia do Enxame para simulações ecossistêmicas e mentes digitais.</p>
                </div>
            </div>

            <!-- Stellar Status Indicators -->
            <div class="card p-5 rounded-xl space-y-3">
                <h3 class="text-xs font-bold text-gray-400 uppercase tracking-wider">Status das Megastruturas</h3>
                
                <div class="flex justify-between items-center text-xs bg-gray-900/60 p-2.5 rounded border border-gray-800">
                    <span>Massa de Mercúrio Restante:</span>
                    <span id="mercuryMassRemaining" class="font-mono text-amber-400 font-bold">75.0%</span>
                </div>

                <div class="flex justify-between items-center text-xs bg-gray-900/60 p-2.5 rounded border border-gray-800">
                    <span>Brilho Infravermelho Residual:</span>
                    <span id="irSignature" class="font-mono text-cyan-400 font-bold">Moderado (Alta emissão de calor)</span>
                </div>

                <div class="flex justify-between items-center text-xs bg-gray-900/60 p-2.5 rounded border border-gray-800">
                    <span>Capacidade de Processamento Quântico:</span>
                    <span id="quantumPetaflops" class="font-mono text-purple-400 font-bold">1.2 × 10⁴⁰ FLOPS</span>
                </div>
            </div>
        </section>

        <!-- Center Column: Solar System Domain Map -->
        <section class="space-y-6">
            <div class="card p-5 rounded-xl">
                <h2 class="text-lg font-bold text-amber-400 mb-4">🪐 Estado dos Corpos Celestes</h2>
                
                <div class="space-y-3 text-sm">
                    <!-- Sun -->
                    <div class="p-3 bg-gray-900/50 rounded-lg border border-gray-800">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-amber-300">☀️ O Sol (Enxame de Dyson)</span>
                            <span id="sunBadge" class="text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30">Enxame Parcial</span>
                        </div>
                        <p id="sunDesc" class="text-xs text-gray-300">Bilhões de coletores ópticos de grafeno transmitindo energia via lasers infravermelhos.</p>
                    </div>

                    <!-- Earth -->
                    <div class="p-3 bg-gray-900/50 rounded-lg border border-gray-800">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-emerald-300">🌍 Terra</span>
                            <span class="text-xs px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30">Reserva Histórica</span>
                        </div>
                        <p class="text-xs text-gray-300">Zero indústrias pesadas. Preservação ecológica total e centro administrativo solar.</p>
                    </div>

                    <!-- Mars & Venus -->
                    <div class="p-3 bg-gray-900/50 rounded-lg border border-gray-800">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-cyan-300">♂️♀️ Marte & Vênus</span>
                            <span id="terraformBadge" class="text-xs px-2 py-0.5 rounded bg-cyan-500/20 text-cyan-300 border border-cyan-500/30">Terraformação em Andamento</span>
                        </div>
                        <p id="terraformDesc" class="text-xs text-gray-300">Campo magnético artificial em Marte ativo; resfriamento e neutralização de gases em Vênus.</p>
                    </div>

                    <!-- Gas Giants -->
                    <div class="p-3 bg-gray-900/50 rounded-lg border border-gray-800">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-orange-300">🪐 Júpiter & Saturno</span>
                            <span class="text-xs px-2 py-0.5 rounded bg-orange-500/20 text-orange-300 border border-orange-500/30">Refinaria de Combustível</span>
                        </div>
                        <p class="text-xs text-gray-300">Sondas mineradoras extraindo hidrogênio e hélio-3 para frotas de fusão/antimatéria.</p>
                    </div>

                    <!-- O'Neill Cylinders -->
                    <div class="p-3 bg-gray-900/50 rounded-lg border border-gray-800">
                        <div class="flex justify-between items-center mb-1">
                            <span class="font-bold text-purple-300">🏗️ Cilindros de O'Neill & Habitats</span>
                            <span id="habitatBadge" class="text-xs px-2 py-0.5 rounded bg-purple-500/20 text-purple-300 border border-purple-500/30">8.2 Bilhões de Habitantes</span>
                        </div>
                        <p class="text-xs text-gray-300">A maior parte da população humana reside em megastruturas rotatórias no espaço.</p>
                    </div>
                </div>
            </div>

            <!-- Interstellar Missions Log -->
            <div class="card p-5 rounded-xl">
                <div class="flex justify-between items-center mb-2">
                    <h3 class="text-md font-bold text-gray-200">🚀 Frotas Interstelares (Alpha Centauri)</h3>
                    <button onclick="launchInterstellarMission()" class="text-xs bg-cyan-600 hover:bg-cyan-500 px-2.5 py-1 rounded text-white font-semibold transition">
                        Lançar Sonda de Vóton (0.15c)
                    </button>
                </div>
                <div id="simLog" class="h-36 overflow-y-auto space-y-2 text-xs font-mono bg-gray-950/80 p-3 rounded border border-gray-800">
                    <p class="text-amber-400">[Ano 2200] Era Estelar Iniciada. Enxame de Dyson em expansão contínua em Mercúrio.</p>
                </div>
            </div>
        </section>

        <!-- Right Column: Graphs & Future Trajectory -->
        <section class="space-y-6">
            <div class="card p-5 rounded-xl flex flex-col justify-between">
                <div>
                    <h2 class="text-lg font-bold text-amber-400 mb-1">📈 Curva de Captura Energética</h2>
                    <p class="text-xs text-gray-400 mb-4">Evolução do Enxame de Dyson e consumo computacional.</p>
                </div>
                <div class="h-64">
                    <canvas id="energyChart"></canvas>
                </div>
            </div>

            <div id="outcomeBox" class="card p-5 rounded-xl border-amber-500/40">
                <h3 id="outcomeTitle" class="font-bold text-md text-amber-400 mb-1">Domínio do Sistema Solar</h3>
                <p id="outcomeText" class="text-xs text-gray-300 leading-relaxed">
                    O Enxame de Dyson está capturando energia suficiente para alimentar a terraformação planetária e sustentar a rede quântica do Cérebro de Matrioshka.
                </p>
            </div>
        </section>

    </main>

    <script>
        let currentYear = 2200;
        let kardashevLevel = 2.000;
        let mercuryMassPct = 75.0;
        let interstellarMissionsLaunched = 0;

        let ctx = document.getElementById('energyChart').getContext('2d');
        let chart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: [2200],
                datasets: [
                    {
                        label: 'Kardashev Level',
                        data: [2.000],
                        borderColor: '#f59e0b',
                        backgroundColor: 'rgba(245, 158, 11, 0.15)',
                        yAxisID: 'y',
                        tension: 0.2,
                        fill: true
                    },
                    {
                        label: 'Massa Restante Mercúrio (%)',
                        data: [75.0],
                        borderColor: '#06b6d4',
                        borderDash: [4, 4],
                        yAxisID: 'y1',
                        tension: 0.2
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    x: {
                        ticks: { color: '#9ca3af' },
                        grid: { color: '#1e293b' }
                    },
                    y: {
                        type: 'linear',
                        display: true,
                        position: 'left',
                        min: 1.95,
                        max: 2.30,
                        ticks: { color: '#f59e0b' },
                        grid: { color: '#1e293b' }
                    },
                    y1: {
                        type: 'linear',
                        display: true,
                        position: 'right',
                        min: 0,
                        max: 100,
                        ticks: { color: '#06b6d4' },
                        grid: { drawOnChartArea: false }
                    }
                },
                plugins: {
                    legend: { labels: { color: '#f3f4f6', boxWidth: 10 } }
                }
            }
        });

        function updateSimulation() {
            let mercuryMining = parseInt(document.getElementById('mercurySlider').value);
            let starLifting = parseInt(document.getElementById('starLiftingSlider').value);
            let matrioshka = parseInt(document.getElementById('matrioshkaSlider').value);

            document.getElementById('mercuryVal').innerText = mercuryMining + '%';
            document.getElementById('starLiftingVal').innerText = starLifting + '%';
            document.getElementById('matrioshkaVal').innerText = matrioshka + '%';

            // Calculations
            let coveragePct = Math.min(100, (mercuryMining * 0.75) + (starLifting * 0.25));
            let totalWattsExponent = 25.0 + (coveragePct / 100);
            let totalWattsCoeff = (3.86 * (coveragePct / 100) + 0.1).toFixed(2);

            document.getElementById('dysonCoverage').innerText = coveragePct.toFixed(1) + '% do Sol';
            document.getElementById('wattsDisplay').innerText = `${totalWattsCoeff} × 10²⁶ W`;

            // Quantum flops calculation
            let flopsExp = 38 + Math.floor(matrioshka / 10);
            document.getElementById('quantumPetaflops').innerText = `1.2 × 10⁴${flopsExp % 10} FLOPS`;

            // State updates
            updateBadges(coveragePct, mercuryMining, starLifting);
        }

        function updateBadges(coverage, mercury, lifting) {
            let sunBadge = document.getElementById('sunBadge');
            let sunDesc = document.getElementById('sunDesc');
            if (coverage > 80) {
                sunBadge.innerText = "Enxame Quase Completo";
                sunBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/30 text-amber-200 border border-amber-400/50";
                sunDesc.innerText = "O Sol brilha prioritariamente no espectro infravermelho para observadores externos.";
            } else if (coverage > 40) {
                sunBadge.innerText = "Enxame Avançado";
                sunBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30";
                sunDesc.innerText = "Densa nuvem de coletores leves redirecionando lasers para todos os planetas e luas.";
            } else {
                sunBadge.innerText = "Enxame Parcial";
                sunBadge.className = "text-xs px-2 py-0.5 rounded bg-amber-500/20 text-amber-300 border border-amber-500/30";
                sunDesc.innerText = "Bilhões de coletores ópticos de grafeno transmitindo energia via lasers infravermelhos.";
            }

            let terraformBadge = document.getElementById('terraformBadge');
            let terraformDesc = document.getElementById('terraformDesc');
            if (coverage > 50) {
                terraformBadge.innerText = "Terraformação Concluída";
                terraformBadge.className = "text-xs px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-300 border border-emerald-500/30";
                terraformDesc.innerText = "Marte e Vênus possuem oceanos estáveis, campos magnéticos e atmosferas respiráveis.";
            } else {
                terraformBadge.innerText = "Terraformação em Andamento";
                terraformBadge.className = "text-xs px-2 py-0.5 rounded bg-cyan-500/20 text-cyan-300 border border-cyan-500/30";
                terraformDesc.innerText = "Campo magnético artificial em Marte ativo; resfriamento e neutralização de gases em Vênus.";
            }
        }

        function advanceYear() {
            currentYear += 10;

            let mercuryMining = parseInt(document.getElementById('mercurySlider').value);
            let starLifting = parseInt(document.getElementById('starLiftingSlider').value);

            // Deplete Mercury mass slowly
            mercuryMassPct = Math.max(0, Math.round((mercuryMassPct - (mercuryMining * 0.15)) * 10) / 10);
            document.getElementById('mercuryMassRemaining').innerText = mercuryMassPct.toFixed(1) + '%';

            // Increase Kardashev Level
            let deltaK = (mercuryMining * 0.001) + (starLifting * 0.0008);
            kardashevLevel = Math.min(2.10, Math.round((kardashevLevel + deltaK) * 1000) / 1000);
            document.getElementById('kardashevDisplay').innerText = kardashevLevel.toFixed(3);

            // Log
            let log = document.getElementById('simLog');
            let newLog = document.createElement('p');
            newLog.className = "text-amber-300";
            newLog.innerText = `[Ano ${currentYear}] Kardashev: ${kardashevLevel.toFixed(3)} | Massa Mercúrio: ${mercuryMassPct}% | Enxame Ativo.`;
            log.prepend(newLog);

            // Update Chart
            chart.data.labels.push(currentYear);
            chart.data.datasets[0].data.push(kardashevLevel);
            chart.data.datasets[1].data.push(mercuryMassPct);
            chart.update();

            checkOutcome();
        }

        function launchInterstellarMission() {
            interstellarMissionsLaunched++;
            let log = document.getElementById('simLog');
            let newLog = document.createElement('p');
            newLog.className = "text-cyan-400 font-bold";
            newLog.innerText = `[Ano ${currentYear}] Frota #${interstellarMissionsLaunched} com propulsão por Velas de Fóton enviada rumo a Alpha Centauri (0.18c).`;
            log.prepend(newLog);
        }

        function checkOutcome() {
            let title = document.getElementById('outcomeTitle');
            let text = document.getElementById('outcomeText');
            let box = document.getElementById('outcomeBox');

            if (kardashevLevel >= 2.05) {
                title.innerText = "✨ Civilização Estelar Plena (Tipo II)";
                title.className = "font-bold text-md text-amber-300 mb-1";
                box.className = "card p-5 rounded-xl border-amber-500/60 bg-amber-950/20";
                text.innerText = "O Sol foi completamente dominado. A humanidade expandiu seus horizontes e as primeiras colônias em Alpha Centauri estão estabelecidas.";
            } else if (mercuryMassPct === 0) {
                title.innerText = "💥 Mercúrio Totalmente Consumido";
                title.className = "font-bold text-md text-cyan-300 mb-1";
                text.innerText = "Mercúrio foi completamente convertido nos satélites do Enxame de Dyson e nos supercomputadores do Cérebro de Matrioshka.";
            }
        }

        function resetSimulation() {
            currentYear = 2200;
            kardashevLevel = 2.000;
            mercuryMassPct = 75.0;
            interstellarMissionsLaunched = 0;

            document.getElementById('mercurySlider').value = 25;
            document.getElementById('starLiftingSlider').value = 15;
            document.getElementById('matrioshkaSlider').value = 10;

            document.getElementById('simLog').innerHTML = '<p class="text-amber-400">[Ano 2200] Simulação reiniciada no Nível Kardashev 2.000.</p>';

            chart.data.labels = [2200];
            chart.data.datasets[0].data = [2.000];
            chart.data.datasets[1].data = [75.0];
            chart.update();

            updateSimulation();
            document.getElementById('kardashevDisplay').innerText = '2.000';
            document.getElementById('mercuryMassRemaining').innerText = '75.0%';
        }

        // Initial setup
        updateSimulation();
    </script>
</body>
</html>
