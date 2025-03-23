<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generador de Proyecto de Vida con IA</title>
    <style>
        /* Estilos base */
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            transition: background-color 0.3s, color 0.3s;
        }

        /* Barra superior fija */
        .top-bar {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            background-color: #2c2c2c;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            z-index: 1000;
        }

        .top-bar button {
            background: none;
            border: none;
            color: #bb86fc;
            font-size: 1.5em;
            cursor: pointer;
        }

        .top-bar button:hover {
            color: #ffffff;
        }

        /* Contenedor principal */
        .container {
            max-width: 800px;
            margin: 80px auto 20px;
            padding: 20px;
            border-radius: 8px;
            transition: background-color 0.3s, box-shadow 0.3s;
        }

        h1, h2 {
            transition: color 0.3s;
        }

        label {
            display: block;
            margin-bottom: 10px;
            font-weight: bold;
            transition: color 0.3s;
        }

        input, textarea, .multi-input, select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 1em;
            margin-bottom: 20px;
            transition: border-color 0.3s, background-color 0.3s, color 0.3s;
        }

        button {
            display: block;
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 4px;
            font-size: 1.1em;
            cursor: pointer;
            transition: background-color 0.3s, color 0.3s;
        }

        .add-button {
            margin-bottom: 20px;
        }

        .question {
            display: none;
        }

        .question.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .progress-bar {
            width: 100%;
            background-color: #e0e0e0;
            border-radius: 4px;
            margin-bottom: 20px;
            overflow: hidden;
        }

        .progress {
            height: 10px;
            background-color: #3498db;
            width: 0;
            transition: width 0.5s ease;
        }

        /* Modo oscuro (predeterminado) */
        body.dark-mode {
            background-color: #1e1e1e;
            color: #ffffff;
        }

        body.dark-mode .container {
            background-color: #2c2c2c;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }

        body.dark-mode h1, body.dark-mode h2 {
            color: #bb86fc; /* Morado */
        }

        body.dark-mode label {
            color: #bb86fc; /* Morado */
        }

        body.dark-mode input, body.dark-mode textarea, body.dark-mode .multi-input, body.dark-mode select {
            background-color: #333;
            border-color: #444;
            color: #ffffff;
        }

        body.dark-mode button {
            background-color: #bb86fc; /* Morado */
            color: #000000;
        }

        body.dark-mode .add-button {
            background-color: #3700b3; /* Morado oscuro */
            color: #ffffff;
        }

        body.dark-mode .add-button:hover {
            background-color: #6200ee; /* Morado más claro */
        }

        /* Modo claro */
        body.light-mode {
            background-color: #f4f4f9;
            color: #333;
        }

        body.light-mode .container {
            background-color: #ffffff;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        body.light-mode h1, body.light-mode h2 {
            color: #4caf50; /* Verde */
        }

        body.light-mode label {
            color: #4caf50; /* Verde */
        }

        body.light-mode input, body.light-mode textarea, body.light-mode .multi-input, body.light-mode select {
            background-color: #ffffff;
            border-color: #ddd;
            color: #333;
        }

        body.light-mode button {
            background-color: #4caf50; /* Verde */
            color: #ffffff;
        }

        body.light-mode .add-button {
            background-color: #388e3c; /* Verde oscuro */
            color: #ffffff;
        }

        body.light-mode .add-button:hover {
            background-color: #66bb6a; /* Verde más claro */
        }

        /* Modal de términos y condiciones */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            justify-content: center;
            align-items: center;
            z-index: 1001;
        }

        .modal-content {
            background-color: #2c2c2c;
            padding: 20px;
            border-radius: 8px;
            max-width: 600px;
            max-height: 80vh;
            overflow-y: auto;
        }

        .modal-content h2 {
            color: #bb86fc;
        }

        .modal-content p {
            color: #ffffff;
        }

        .modal-content button {
            background-color: #bb86fc;
            color: #000000;
            margin-top: 20px;
        }

        /* Asistente de IA */
        .assistant-chat {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 300px;
            background-color: #2c2c2c;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            display: none;
            flex-direction: column;
            padding: 10px;
            z-index: 1000;
        }

        .assistant-chat input {
            width: 100%;
            padding: 10px;
            border: 1px solid #444;
            border-radius: 4px;
            background-color: #333;
            color: #ffffff;
        }

        .assistant-chat button {
            background-color: #bb86fc;
            color: #000000;
            margin-top: 10px;
        }

        .assistant-messages {
            max-height: 200px;
            overflow-y: auto;
            margin-bottom: 10px;
        }

        .assistant-messages div {
            background-color: #444;
            padding: 10px;
            border-radius: 4px;
            margin-bottom: 5px;
            color: #ffffff;
        }
    </style>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
</head>
<body class="dark-mode">
    <!-- Barra superior -->
    <div class="top-bar">
        <button onclick="toggleMenu()">&#9776;</button>
        <button onclick="toggleAssistant()">🤖</button>
    </div>

    <!-- Menú de tres puntos -->
    <div class="menu" id="menu">
        <button onclick="toggleMode()">Cambiar modo oscuro/claro</button>
        <button onclick="openModal()">Términos y condiciones</button>
        <button onclick="deleteUser()">Borrar mi registro</button>
        <button onclick="logout()">Cerrar sesión</button>
    </div>

    <!-- Modal de términos y condiciones -->
    <div class="modal" id="modal">
        <div class="modal-content">
            <h2>Términos y Condiciones</h2>
            <p>
                <strong>A nombre de Diego Ortiz García</strong><br><br>
                1. El usuario acepta que los datos proporcionados serán almacenados localmente en su navegador.<br>
                2. El usuario puede borrar sus datos en cualquier momento desde la opción "Borrar mi registro".<br>
                3. Los datos no se compartirán con terceros y solo se usarán para generar el proyecto de vida.<br>
                4. El usuario es responsable de la veracidad de la información proporcionada.<br>
                5. Al usar esta aplicación, aceptas estos términos y condiciones.
            </p>
            <button onclick="closeModal()">Cerrar</button>
        </div>
    </div>

    <!-- Asistente de IA -->
    <div class="assistant-chat" id="assistant-chat">
        <div class="assistant-messages" id="assistant-messages"></div>
        <input type="text" id="assistant-input" placeholder="Escribe tu pregunta...">
        <button onclick="sendMessage()">Enviar</button>
    </div>

    <!-- Contenedor principal -->
    <div class="container">
        <h1>Generador de Proyecto de Vida con IA</h1>
        <div class="progress-bar">
            <div class="progress" id="progress"></div>
        </div>

        <!-- Preguntas -->
        <div class="question active" id="q0">
            <h2>Datos Personales</h2>
            <label for="nombre">Nombre:</label>
            <input type="text" id="nombre" placeholder="Ingresa tu nombre">
            <label for="apellido">Apellido:</label>
            <input type="text" id="apellido" placeholder="Ingresa tu apellido">
            <label for="cedula">Cédula:</label>
            <input type="text" id="cedula" placeholder="Ingresa tu cédula">
            <label for="seccion">Sección:</label>
            <input type="text" id="seccion" placeholder="Ingresa tu sección">
            <label for="fechaNacimiento">Fecha de Nacimiento:</label>
            <input type="date" id="fechaNacimiento">
            <button onclick="nextQuestion('q1')">Siguiente</button>
        </div>

        <!-- Resto de las preguntas (igual que antes) -->
        <!-- ... -->
    </div>

    <script>
        const { jsPDF } = window.jspdf;

        // Función para alternar el menú
        function toggleMenu() {
            const menu = document.getElementById('menu');
            menu.style.display = menu.style.display === 'flex' ? 'none' : 'flex';
        }

        // Función para cambiar entre modos
        function toggleMode() {
            const body = document.body;
            if (body.classList.contains('dark-mode')) {
                body.classList.remove('dark-mode');
                body.classList.add('light-mode');
            } else {
                body.classList.remove('light-mode');
                body.classList.add('dark-mode');
            }
            toggleMenu();
        }

        // Función para abrir el modal de términos y condiciones
        function openModal() {
            const modal = document.getElementById('modal');
            modal.style.display = 'flex';
        }

        // Función para cerrar el modal
        function closeModal() {
            const modal = document.getElementById('modal');
            modal.style.display = 'none';
        }

        // Función para borrar el registro del usuario
        function deleteUser() {
            localStorage.removeItem('userData');
            alert('Registro borrado correctamente.');
            toggleMenu();
        }

        // Función para cerrar sesión
        function logout() {
            localStorage.removeItem('userData');
            window.location.reload();
        }

        // Función para alternar el asistente de IA
        function toggleAssistant() {
            const chat = document.getElementById('assistant-chat');
            chat.style.display = chat.style.display === 'flex' ? 'none' : 'flex';
        }

        // Función para enviar mensajes al asistente de IA
        async function sendMessage() {
            const input = document.getElementById('assistant-input');
            const messages = document.getElementById('assistant-messages');
            const userMessage = input.value.trim();

            if (!userMessage) return;

            // Mostrar el mensaje del usuario
            messages.innerHTML += `<div><strong>Tú:</strong> ${userMessage}</div>`;
            input.value = '';

            // Llamar a la API de Gemini
            const apiKey = 'AIzaSyCYDcrIucZKYt3EETgqcWOylYYruaIkDN0'; // Reemplaza con tu clave de API
            const response = await fetch('https://api.gemini.com/v1/chat/completions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': `Bearer ${apiKey}`
                },
                body: JSON.stringify({
                    model: 'gemini-pro',
                    messages: [{ role: 'user', content: userMessage }]
                })
            });

            const data = await response.json();
            const aiMessage = data.choices[0].message.content;

            // Mostrar la respuesta de la IA
            messages.innerHTML += `<div><strong>IA:</strong> ${aiMessage}</div>`;
            messages.scrollTop = messages.scrollHeight;
        }

        // Resto del código (igual que antes)
        // ...
    </script>
</body>
</html>
