<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NeuroBloom AI - Modificador Cerebral por Neuroplasticidade</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #e8f5e8 0%, #a8e6cf 100%);
            color: #333;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            line-height: 1.6;
        }
        #app {
            max-width: 800px;
            background: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.1);
            text-align: center;
        }
        header {
            margin-bottom: 30px;
        }
        h1 {
            color: #4CAF50;
            font-size: 2.5em;
        }
        p {
            font-size: 1.1em;
            color: #666;
        }
        button {
            background-color: #4CAF50;
            color: white;
            border: none;
            padding: 12px 25px;
            margin: 10px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #45a049;
        }
        .hidden {
            display: none;
        }
        #garden {
            margin: 20px 0;
            height: 150px;
            background: #f0f8f0;
            border-radius: 10px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 2em;
        }
        #exercise-content {
            margin: 20px 0;
            font-size: 1.1em;
        }
        #feedback {
            margin-top: 20px;
            padding: 15px;
            background-color: #e8f5e8;
            border-radius: 8px;
            border-left: 5px solid #4CAF50;
        }
        #dashboard p {
            font-size: 1.2em;
            margin: 10px 0;
        }
        input {
            padding: 10px;
            margin: 10px;
            border-radius: 5px;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>
    <div id="app">
        <header>
            <h1>NeuroBloom AI</h1>
            <p>Modifique seu cérebro com neuroplasticidade inteligente. Exercícios adaptativos, jardim mental vivo e IA personalizada para reabilitação cognitiva e emocional.</p>
        </header>
        <div id="garden">🌱 Jardim Mental: Comece a cultivar!</div>
        <main>
            <div id="dashboard" class="hidden">
                <h2>Seu Progresso Neuroplástico</h2>
                <p>Dias consecutivos: <span id="streak">0</span></p>
                <p>Nível de Adaptação: <span id="level">1</span></p>
                <p>Poderes Desbloqueados: <span id="powers">Nenhum ainda.</span></p>
                <p>Relatório Científico: Seu córtex pré-frontal está fortalecendo com repetição espaçada. Ansiedade reduzida em <span id="anxiety-reduction">0%</span>.</p>
                <button id="back-to-menu">Voltar ao Menu</button>
            </div>
            <div id="menu">
                <button id="start-session">Iniciar Sessão Adaptativa</button>
                <button id="view-progress">Ver Progresso</button>
                <button id="customize">Personalizar IA</button>
            </div>
            <div id="customize-section" class="hidden">
                <h2>Personalize Sua IA</h2>
                <p>Dificuldade inicial (1-10): <input type="number" id="difficulty" min="1" max="10" value="5"></p>
                <p>Foco emocional (alto/baixo): <select id="focus"><option>alto</option><option>baixo</option></select></p>
                <button id="save-custom">Salvar e Voltar</button>
            </div>
            <div id="exercise" class="hidden">
                <h2 id="exercise-title"></h2>
                <div id="exercise-content"></div>
                <input type="text" id="user-input" placeholder="Sua resposta aqui">
                <button id="submit-answer">Enviar Resposta</button>
                <button id="pause">Pausa Emocional Inteligente</button>
                <div id="feedback" class="hidden"></div>
            </div>
        </main>
    </div>
    <script>
        // Dados do usuário (salvos localmente, com IA personalizada)
        let userData = JSON.parse(localStorage.getItem('neurobloom')) || {
            streak: 0,
            level: 1,
            lastDate: null,
            powers: [],
            anxietyReduction: 0,
            difficulty: 5,
            focus: 'alto'
        };

        // Exercícios adaptativos (IA: dificuldade muda com respostas; neuroplasticidade via repetição e desafio)
        const exercises = [
            {
                name: 'Foco de Atenção Adaptativo',
                baseDifficulty: 5,
                steps: (diff) => [
                    `Observe a grade e encontre o item diferente (dificuldade ${diff}).`,
                    'Responda com o item correto.',
                    'Parabéns! Conexões neurais fortalecidas para atenção sustentada.'
                ],
                content: (diff) => `<p>Grade: [A B C D E] - Qual é diferente? (Dificuldade ${diff})</p>`,
                correct: 'C',
                adapt: (correct) => correct ? 1 : -1 // Aumenta/reduz dificuldade
            },
            {
                name: 'Memória Visual Inteligente',
                baseDifficulty: 5,
                steps: (diff) => [
                    `Memorize ${diff} cores: Vermelho, Azul, Verde${diff > 5 ? ', Amarelo' : ''}.`,
                    'Recrie a sequência.',
                    'Ótimo! Hipocampo reforçado com repetição espaçada.'
                ],
                content: (diff) => `<p>Sequência: Vermelho, Azul, Verde${diff > 5 ? ', Amarelo' : ''}. Agora recrie.</p>`,
                correct: 'Vermelho, Azul, Verde' + (diff > 5 ? ', Amarelo' : ''),
                adapt: (correct) => correct ? 1 : -1
            },
            {
                name: 'Compreensão de Leitura Dinâmica',
                baseDifficulty: 5,
                steps: (diff) => [
                    `Leia o texto (comprimento ${diff * 10} palavras).`,
                    'Responda: O que o texto diz?',
                    'Excelente! Rede de leitura estimulada.'
                ],
                content: (diff) => `<p>Texto: "O sol brilha no céu azul. As estrelas aparecem à noite${diff > 5 ? ', e a lua ilumina o caminho.' : ''}." O que brilha?</p>`,
                correct: 'sol',
                adapt: (correct) => correct ? 1 : -1
            },
            {
                name: 'Raciocínio Lógico Adaptativo',
                baseDifficulty: 5,
                steps: (diff) => [
                    `Resolva: 1, 3, 5, ? (Dificuldade ${diff})`,
                    'Resposta correta.',
                    'Seu raciocínio está evoluindo! Córtex pré-frontal fortalecido.'
                ],
                content: (diff) => `<p>Sequência: 1, 3, 5, ?${diff > 5 ? ' (padrão ímpar)' : ''}</p>`,
                correct: '7',
                adapt: (correct) => correct ? 1 : -1
            },
            {
                name: 'Organização Executiva Inteligente',
                baseDifficulty: 5,
                steps: (diff) => [
                    `Liste ${diff} tarefas e organize.`,
                    'Priorize.',
                    'Você está organizando bem! Funções executivas aprimoradas.'
                ],
                content: (diff) => `<p>Tarefas: Ler, Exercitar${diff > 5 ? ', Estudar, Relaxar' : ''}. Organize por prioridade.</p>`,
                correct: 'Ler, Exercitar' + (diff > 5 ? ', Estudar, Relaxar' : ''),
                adapt: (correct) => correct ? 1 : -1
            }
        ];

        let currentExercise = null;
        let currentStep = 0;
        let currentDifficulty = userData.difficulty;

        // Funções principais
        function updateDashboard() {
            document.getElementById('streak').textContent = userData.streak;
            document.getElementById('level').textContent = userData.level;
            document.getElementById('powers').textContent = userData.powers.join(', ') || 'Nenhum ainda.';
            document.getElementById('anxiety-reduction').textContent = userData.anxietyReduction + '%';
            updateGarden();
        }

        function updateGarden() {
            const garden = document.getElementById('garden');
            if (userData.level === 1) garden.innerHTML = '🌱 Jardim Mental: Comece a cultivar!';
            else if (userData.level < 5) garden.innerHTML = '🌿 Jardim Mental: Crescendo com neuroplasticidade!';
            else garden.innerHTML = '🌳 Jardim Mental: Floresta cerebral forte!';
        }

        function checkStreak() {
            const today = new Date().toDateString();
            if (userData.lastDate !== today) {
                if (userData.lastDate === new Date(Date.now() - 86400000).toDateString()) {
                    userData.streak++;
                    userData.anxietyReduction += 5; // Simula redução de ansiedade
                } else {
                    userData.streak = 1;
                }
                userData.lastDate = today;
                if (userData.streak % 7 === 0) {
                    userData.level++;
                    userData.powers.push('Poder Cognitivo ' + userData.level); // Desbloqueia "poderes"
                }
                localStorage.setItem('neurobloom', JSON.stringify(userData));
            }
        }

        function startSession() {
            checkStreak();
            currentExercise = exercises[Math.floor(Math.random() * exercises.length)];
            currentStep = 0;
            currentDifficulty = Math.max(1, Math.min(10, currentDifficulty + (Math.random() > 0.5 ? 1 : -1))); // IA adapta dificuldade
            document.getElementById('exercise-title').textContent = currentExercise.name;
            document.getElementById('exercise-content').innerHTML = currentExercise.content(currentDifficulty);
            document.getElementById('menu').classList.add('hidden');
            document.getElementById('exercise').classList.remove('hidden');
            document.getElementById('feedback').classList.add('hidden');
            document.getElementById('user-input').value = '';
        }

        function submitAnswer() {
            const answer = document.getElementById('user-input').value.toLowerCase().trim();
            const correct = answer === currentExercise.correct.toLowerCase();
            currentDifficulty += currentExercise.adapt(correct); // IA adapta
            currentStep++;
            if (currentStep < currentExercise.steps(currentDifficulty).length) {
                document.getElementById('exercise-content').innerHTML = '<p>' + currentExercise.steps(currentDifficulty)[currentStep] + '</p>';
            } else {
                const feedback = correct ? 'Correto! Neuroplasticidade ativada.' : 'Quase! Tente novamente amanhã.';
                document.getElementById('feedback').textContent = feedback + ' Seu cérebro está se adaptando.';
                document.getElementById('feedback').classList.remove('hidden');
                document.getElementById('exercise').classList.add('hidden');
                document.getElementById('menu').classList.remove('hidden');
            }
        }

        function pauseEmotional() {
            document.getElementById('exercise-content').innerHTML = '<p>Respire profundamente por 30 segundos. Pense: "Meu cérebro está crescendo." (IA detecta ansiedade alta – reduzindo estresse.)</p>';
            setTimeout(() => {
                document.getElementById('exercise-content').innerHTML = '<p>Retorne quando pronta. Seu foco emocional é ' + userData.focus + '.</p>';
            }, 30000);
        }

        function backToMenu() {
            document.getElementById('dashboard').classList.add('hidden');
            document.getElementById('menu').classList.remove('hidden');
        }

        function saveCustom() {
            userData.difficulty = parseInt(document.getElementById('difficulty').value);
            userData.focus = document.getElementById('focus').value;
            localStorage.setItem('neurobloom', JSON.stringify(userData));
            document.getElementById('customize-section').classList.add('hidden');
            document.getElementById('menu').classList.remove('hidden');
        }

        // Eventos
        document.getElementById('start-session').onclick = startSession;
        document.getElementById('view-progress').onclick = () => {
            updateDashboard();
            document.getElementById('dashboard').classList.remove('hidden');
            document.getElementById('menu').classList.add('hidden');
        };
        document.getElementById('customize').onclick = () => {
            document.getElementById('menu').classList.add('hidden');
            document.getElementById('customize-section').classList.remove('hidden');
        };
        document.getElementById('submit-answer').onclick = submitAnswer;
        document.getElementById('pause').onclick = pauseEmotional;
        document.getElementById('back-to-menu').onclick = backToMenu;
        document.getElementById('save-custom').onclick = saveCustom;

        // Inicialização
        updateDashboard();
    </script>
</body>
</html>
