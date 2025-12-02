<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cuestionario: La Conexión con Dios</title>
    <!-- Carga de Tailwind CSS para estilos modernos y responsive -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6;
        }
        /* Estilo personalizado para las tarjetas de pregunta */
        .question-card {
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
            transition: transform 0.2s;
        }
        .option-button {
            transition: all 0.15s ease-in-out;
            cursor: pointer;
        }
        .option-button:hover:not(.disabled) {
            background-color: #e5e7eb;
            transform: translateY(-2px);
        }
        .correct {
            background-color: #d1fae5; /* Verde claro */
            border-color: #10b981; /* Verde fuerte */
            animation: pulse-correct 0.5s;
        }
        .incorrect {
            background-color: #fee2e2; /* Rojo claro */
            border-color: #ef4444; /* Rojo fuerte */
            animation: shake-incorrect 0.4s;
        }
        .disabled {
            pointer-events: none;
        }

        /* Animaciones */
        @keyframes pulse-correct {
            0% { transform: scale(1); }
            50% { transform: scale(1.02); }
            100% { transform: scale(1); }
        }
        @keyframes shake-incorrect {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-5px); }
            40%, 80% { transform: translateX(5px); }
        }
        .rationale-text {
            border-left: 4px solid #3b82f6;
        }
    </style>
</head>
<body class="min-h-screen flex items-center justify-center p-4 sm:p-6">

    <div id="quiz-container" class="w-full max-w-2xl bg-white rounded-xl question-card p-6 sm:p-8">
        <h1 class="text-3xl font-bold text-gray-800 mb-6 text-center border-b pb-3 border-blue-200">
            Cuestionario: La Conexión con Dios
        </h1>
        <div id="quiz-content">
            <!-- Las preguntas se cargarán aquí -->
            <div class="text-center p-8">
                <p class="text-gray-500">Cargando cuestionario...</p>
            </div>
        </div>
        
        <div id="results" class="hidden mt-8 p-6 bg-blue-50 border-t-4 border-blue-400 rounded-lg">
            <h2 class="text-2xl font-bold text-blue-700 mb-3">Resultados Finales</h2>
            <p id="score-display" class="text-xl text-gray-800 font-semibold">Puntuación: 0/10</p>
            <button onclick="resetQuiz()" class="mt-4 w-full sm:w-auto px-6 py-3 bg-blue-600 text-white font-semibold rounded-lg hover:bg-blue-700 transition duration-150">
                Repetir Cuestionario
            </button>
        </div>
    </div>

    <script>
        // Los datos del cuestionario en formato JSON
        const quizData = [
            {
                question: "¿Cuál fue el motivo principal por el que maquinaron contra Daniel?",
                options: [
                    { text: "Abría ventanas", isCorrect: false, rationale: "Este era un hábito de Daniel, pero el complot se originó por su posición de favor." },
                    { text: "Temor a secretos", isCorrect: false, rationale: "La conspiración se centró en su creciente influencia y estatus." },
                    { text: "Mala gestión", isCorrect: false, rationale: "No pudieron encontrar falta ni error en su gestión, lo que aumentó la envidia." },
                    { text: "Envidia por su favor con el rey", isCorrect: true, rationale: "Daniel sobresalía, y el rey planeaba ponerlo sobre todo el reino, lo que desató los celos." }
                ],
                answered: false
            },
            {
                question: "¿Cómo podían lograr la caída de Daniel según sus enemigos?",
                options: [
                    { text: "Callar su sabiduría", isCorrect: false, rationale: "Su sabiduría era evidente, no podían simplemente silenciarla." },
                    { text: "Exponer errores", isCorrect: false, rationale: "Buscaron errores sin éxito porque Daniel era fiel." },
                    { text: "Cortar su relación con Dios", isCorrect: true, rationale: "Los enemigos sabían que la única manera de atacarlo era a través de su inquebrantable obediencia a la ley de su Dios." },
                    { text: "Convencer al rey", isCorrect: false, rationale: "El rey amaba a Daniel, por lo que tuvieron que engañarlo con una ley inmutable." }
                ],
                answered: false
            },
            {
                question: "Si se corta la relación con Dios, ¿cuál es la consecuencia principal sugerida?",
                options: [
                    { text: "Todos se irán", isCorrect: false, rationale: "Aunque puede haber abandono, la consecuencia más profunda es la desconexión de la fuente de vida." },
                    { text: "Le irá mal con el tiempo (cosecha futura)", isCorrect: true, rationale: "El corte de la conexión espiritual (la 'raíz') lleva inevitablemente al deterioro de la 'cosecha' o resultados de vida a largo plazo." },
                    { text: "Le irá mal al día siguiente", isCorrect: false, rationale: "Las consecuencias espirituales a menudo son progresivas, no siempre inmediatas." },
                    { text: "Pierde el cargo inmediatamente", isCorrect: false, rationale: "El problema central es espiritual y afecta el futuro de su vida, no solo su puesto." }
                ],
                answered: false
            },
            {
                question: "En el contexto de Daniel, ¿qué significa espiritual y prácticamente 'abrir las ventanas'?",
                options: [
                    { text: "Testimonio público de fe", isCorrect: true, rationale: "Orar con las ventanas abiertas es un acto que proclama públicamente a quién sirve y en quién confía, sirviendo como testimonio." },
                    { text: "Presencia circulando", isCorrect: false, rationale: "Se relaciona más con la declaración pública de fe que con un concepto abstracto." },
                    { text: "Autoridad política", isCorrect: false, rationale: "El acto era de devoción, no de manifestación de su cargo." },
                    { text: "Transparencia administrativa", isCorrect: false, rationale: "Aunque fue transparente, el acto es principalmente un testimonio de fe." }
                ],
                answered: false
            },
            {
                question: "¿Cuál es el rol principal del 'Daniel' de la casa?",
                options: [
                    { text: "Conectar con el cielo (intercesor)", isCorrect: true, rationale: "El rol es ser el puente o intercesor que mantiene la conexión divina activa para el hogar." },
                    { text: "Orar solo en secreto", isCorrect: false, rationale: "El rol abarca ser el canal de conexión y autoridad espiritual para la familia." },
                    { text: "Ganar dinero para la familia", isCorrect: false, rationale: "La provisión material es una responsabilidad, pero el rol de 'Daniel' es fundamentalmente espiritual." },
                    { text: "Convencer a todos con palabras", isCorrect: false, rationale: "Su poder radica en la conexión con Dios, no en la persuasión humana." }
                ],
                answered: false
            },
            {
                question: "Según la enseñanza, ¿contra qué pelea realmente la familia (aunque parezca otra cosa)?",
                options: [
                    { text: "El edicto del rey", isCorrect: false, rationale: "El edicto era solo la manifestación visible del ataque." },
                    { text: "La luz que usted porta", isCorrect: true, rationale: "La batalla espiritual se dirige contra la luz que usted representa y que expone las tinieblas." },
                    { text: "Teología", isCorrect: false, rationale: "La pelea no es de doctrina, sino de poder y autoridad espiritual." },
                    { text: "Hipocresía", isCorrect: false, rationale: "La lucha se centra en la oposición al testimonio visible de la persona." }
                ],
                answered: false
            },
            {
                question: "¿Cuál fue la condición práctica para la victoria diaria de Daniel?",
                options: [
                    { text: "Un año de fidelidad", isCorrect: false, rationale: "La fidelidad se mantuvo a lo largo de su vida, pero el acto específico era una disciplina diaria." },
                    { text: "Conversión total", isCorrect: false, rationale: "Daniel ya era un hombre de fe, su victoria se basaba en el sostenimiento de sus hábitos espirituales." },
                    { text: "Tres años de estudio", isCorrect: false, rationale: "Ese fue su período de entrenamiento, pero la victoria del día a día se cimentaba en su rutina." },
                    { text: "Orar 3 veces al día (hábito constante)", isCorrect: true, rationale: "Su disciplina innegociable de orar tres veces al día fue el ancla y la fuente de su fortaleza." }
                ],
                answered: false
            },
            {
                question: "¿Para qué revela Dios lo escondido o los secretos de la casa?",
                options: [
                    { text: "Para el chisme", isCorrect: false, rationale: "La revelación de Dios nunca tiene un propósito destructivo o de murmuración." },
                    { text: "Para confrontar con ira", isCorrect: false, rationale: "La revelación es para la intercesión y la solución, no para la condenación." },
                    { text: "Para demostrar superioridad", isCorrect: false, rationale: "El propósito es ministerial y de servicio." },
                    { text: "Para orar y liberar", isCorrect: true, rationale: "Dios revela los secretos o problemas para que el creyente sepa cómo interceder y actuar para traer liberación y solución." }
                ],
                answered: false
            },
            {
                question: "¿Cuál es el mandato descrito en Hechos 13:47 mencionado en el estudio?",
                options: [
                    { text: "Orar sin cesar", isCorrect: false, rationale: "Es un mandato, pero no el que se cita específicamente en este versículo sobre la misión." },
                    { text: "Alumbrar luz", isCorrect: false, rationale: "Es el concepto central, pero el versículo es más específico sobre el grupo al que está dirigida esa luz." },
                    { text: "Recibir sabiduría", isCorrect: false, rationale: "La sabiduría es una bendición, no el mandato primario de este versículo." },
                    { text: "Ser luz a los gentiles (naciones)", isCorrect: true, rationale: "El mandato es 'Te he puesto para luz de los gentiles, a fin de que seas para salvación hasta lo último de la tierra,' enfocándose en la misión." }
                ],
                answered: false
            },
            {
                question: "¿Qué enseña Mateo 5:16 sobre el propósito de nuestras buenas obras?",
                options: [
                    { text: "Obtener reconocimiento", isCorrect: false, rationale: "El propósito no es el reconocimiento personal." },
                    { text: "Obtener la victoria personal", isCorrect: false, rationale: "La victoria es un resultado, pero el propósito es glorificar al Padre." },
                    { text: "Conseguir seguidores", isCorrect: false, rationale: "Las buenas obras no buscan aumentar el número de seguidores del creyente." },
                    { text: "Glorificar al Padre que está en los cielos", isCorrect: true, rationale: "El versículo indica que el propósito es que los hombres vean las obras y glorifiquen a Dios." }
                ],
                answered: false
            }
        ];

        const quizContent = document.getElementById('quiz-content');
        const resultsDiv = document.getElementById('results');
        const scoreDisplay = document.getElementById('score-display');
        let score = 0;

        // Función para renderizar el cuestionario
        function renderQuiz() {
            quizContent.innerHTML = '';
            quizData.forEach((q, qIndex) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'mb-8 p-4 bg-white rounded-xl question-card shadow-md';
                questionDiv.innerHTML = `
                    <h3 class="text-lg font-semibold text-gray-700 mb-4">
                        ${qIndex + 1}. ${q.question}
                    </h3>
                    <div id="options-${qIndex}" class="space-y-3">
                        ${q.options.map((option, oIndex) => `
                            <button
                                id="q${qIndex}-o${oIndex}"
                                class="option-button w-full text-left p-3 border-2 border-gray-300 rounded-lg hover:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-500 text-gray-800"
                                data-q-index="${qIndex}"
                                data-o-index="${oIndex}"
                                onclick="checkAnswer(event)"
                            >
                                ${option.text}
                            </button>
                        `).join('')}
                    </div>
                    <div id="rationale-${qIndex}" class="rationale-text hidden mt-3 p-3 bg-gray-100 rounded-lg text-sm text-gray-600">
                        <!-- Rationale will be inserted here -->
                    </div>
                `;
                quizContent.appendChild(questionDiv);
            });

            // Si todas las preguntas están respondidas, mostrar resultados
            if (quizData.every(q => q.answered)) {
                showResults();
            } else {
                resultsDiv.classList.add('hidden');
            }
        }

        // Función para verificar la respuesta
        function checkAnswer(event) {
            const button = event.currentTarget;
            const qIndex = parseInt(button.getAttribute('data-q-index'));
            const oIndex = parseInt(button.getAttribute('data-o-index'));
            const question = quizData[qIndex];
            
            if (question.answered) return; // Evitar doble clic

            question.answered = true;
            
            const optionsContainer = document.getElementById(`options-${qIndex}`);
            const rationaleDiv = document.getElementById(`rationale-${qIndex}`);
            
            // Deshabilitar todos los botones para esta pregunta
            optionsContainer.querySelectorAll('button').forEach(btn => {
                btn.classList.add('disabled', 'bg-gray-100');
                btn.classList.remove('hover:border-blue-500', 'hover:bg-gray-200');
            });
            
            // Mostrar retroalimentación
            if (question.options[oIndex].isCorrect) {
                button.classList.add('correct', 'border-4');
                score++;
                rationaleDiv.innerHTML = `<span class="font-bold text-green-700">¡Respuesta Correcta!</span> ${question.options[oIndex].rationale}`;
            } else {
                button.classList.add('incorrect', 'border-4');
                rationaleDiv.innerHTML = `<span class="font-bold text-red-700">Respuesta Incorrecta.</span> ${question.options[oIndex].rationale}`;
                
                // Resaltar la respuesta correcta
                const correctOptionIndex = question.options.findIndex(opt => opt.isCorrect);
                const correctButton = document.getElementById(`q${qIndex}-o${correctOptionIndex}`);
                if (correctButton) {
                    correctButton.classList.add('correct', 'border-4', 'disabled');
                }
            }
            
            rationaleDiv.classList.remove('hidden');

            // Verificar si todas las preguntas han sido respondidas
            if (quizData.every(q => q.answered)) {
                showResults();
            }
        }

        // Función para mostrar los resultados
        function showResults() {
            scoreDisplay.textContent = `Puntuación: ${score}/${quizData.length}`;
            resultsDiv.classList.remove('hidden');
            
            // Ocultar las preguntas después de mostrar los resultados (opcional)
            // quizContent.classList.add('hidden'); 
        }

        // Función para reiniciar el cuestionario
        function resetQuiz() {
            score = 0;
            quizData.forEach(q => {
                q.answered = false;
            });
            renderQuiz();
        }

        // Iniciar el cuestionario al cargar la página
        window.onload = renderQuiz;
    </script>

</body>
</html>
