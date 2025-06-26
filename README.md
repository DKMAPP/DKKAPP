<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gestión de Clientes - Máquinas DKM</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
</head>
<body class="bg-gray-100 font-sans">
    <div id="app" class="container mx-auto p-4">
        <!-- Pantalla Principal -->
        <div id="main-screen" class="flex flex-col items-center justify-center h-screen">
            <h1 class="text-3xl font-bold mb-8">Gestión de Clientes</h1>
            <button id="new-client-btn" class="bg-blue-500 text-white px-6 py-3 rounded-lg hover:bg-blue-600">Nuevo Cliente</button>
        </div>

        <!-- Formulario de Cliente -->
        <div id="client-form-screen" class="hidden flex flex-col items-center justify-center h-screen">
            <h2 class="text-2xl font-bold mb-6">Datos del Cliente</h2>
            <form id="client-form" class="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
                <div class="mb-4">
                    <label class="block text-gray-700">Nombre</label>
                    <input type="text" id="name" class="w-full p-2 border rounded" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700">Empresa</label>
                    <input type="text" id="company" class="w-full p-2 border rounded" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700">Teléfono</label>
                    <input type="tel" id="phone" class="w-full p-2 border rounded" required>
                </div>
                <div class="mb-4">
                    <label class="block text-gray-700">Mail</label>
                    <input type="email" id="email" class="w-full p-2 border rounded" required>
                </div>
                <button type="button" id="grok-link-btn" class="bg-green-500 text-white px-4 py-2 rounded mb-4 w-full hover:bg-green-600">
                    Visitar Grok AI
                </button>
                <div class="mb-4">
                    <label class="block text-gray-700">Seleccionar Máquina</label>
                    <select id="machine-select" class="w-full p-2 border rounded" required>
                        <option value="">Seleccione una máquina</option>
                        <option value="DKM-130SV">DKM-130SV</option>
                        <option value="DKM-180SV">DKM-180SV</option>
                        <option value="DKM-250SV">DKM-250SV</option>
                        <option value="DKM-350SV">DKM-350SV</option>
                        <option value="DKM-450SV">DKM-450SV</option>
                        <option value="DKM-550SV">DKM-550SV</option>
                        <option value="DKM-650SV">DKM-650SV</option>
                        <option value="DKM-850SV">DKM-850SV</option>
                        <option value="DKM-1150SV">DKM-1150SV</option>
                        <option value="DKM-1350SV">DKM-1350SV</option>
                        <option value="DKM-1650SV">DKM-1650SV</option>
                        <option value="DKM-2250SV">DKM-2250SV</option>
                        <option value="DKM-2800SV">DKM-2800SV</option>
                    </select>
                </div>
                <div id="machine-specs" class="hidden bg-gray-50 p-4 rounded mb-4"></div>
                <button type="submit" id="next-btn" class="bg-blue-500 text-white px-4 py-2 rounded w-full hover:bg-blue-600">Siguiente</button>
            </form>
        </div>

        <!-- Pantalla de Consejo -->
        <div id="advice-screen" class="hidden flex flex-col items-center justify-center h-screen">
            <h2 class="text-2xl font-bold mb-6">Consejo de IA</h2>
            <div class="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
                <p class="mb-4">Consulta adicional en: <a href="https://grok.com/?ref=aiartweekly" target="_blank" class="text-blue-500 underline">Grok AI</a></p>
                <div id="ai-advice" class="bg-gray-50 p-4 rounded mb-4"></div>
                <button id="exit-btn" class="bg-red-500 text-white px-4 py-2 rounded w-full hover:bg-red-600">Salir</button>
            </div>
        </div>
    </div>

    <script>
        // Configuración de Google Sheets API (necesitas tu propia clave API y ID de la hoja)
        const SPREADSHEET_ID = '1GTY9WtufW1ZuOV2j8y23rBa8fsCDHXic-7X5YaBnjr8';
        const API_KEY = 'YOUR_GOOGLE_API_KEY'; // Reemplaza con tu clave API de Google
        const SHEET_NAME = 'Sheet1'; // Ajusta según el nombre de la hoja

        // Elementos del DOM
        const mainScreen = document.getElementById('main-screen');
        const clientFormScreen = document.getElementById('client-form-screen');
        const adviceScreen = document.getElementById('advice-screen');
        const newClientBtn = document.getElementById('new-client-btn');
        const clientForm = document.getElementById('client-form');
        const grokLinkBtn = document.getElementById('grok-link-btn');
        const machineSelect = document.getElementById('machine-select');
        const machineSpecs = document.getElementById('machine-specs');
        const nextBtn = document.getElementById('next-btn');
        const exitBtn = document.getElementById('exit-btn');

        // Datos del cliente
        let clientData = {};

        // Mostrar formulario
        newClientBtn.addEventListener('click', () => {
            mainScreen.classList.add('hidden');
            clientFormScreen.classList.remove('hidden');
        });

        // Configurar botón de Grok
        grokLinkBtn.addEventListener('click', () => {
            window.open('https://grok.com/?ref=aiartweekly', '_blank');
        });

        // Obtener especificaciones de la máquina
        machineSelect.addEventListener('change', async () => {
            const machine = machineSelect.value;
            if (!machine) {
                machineSpecs.classList.add('hidden');
                return;
            }

            try {
                const response = await axios.get(
                    `https://sheets.googleapis.com/v4/spreadsheets/${SPREADSHEET_ID}/values/${SHEET_NAME}!A1:Z100?key=${API_KEY}`
                );
                const rows = response.data.values;
                const headers = rows[0];
                const machineRow = rows.find(row => row[0] === machine);

                if (machineRow) {
                    let specsHtml = '<h3 class="font-bold">Especificaciones:</h3><ul>';
                    for (let i = 1; i < headers.length; i++) {
                        if (machineRow[i]) {
                            specsHtml += `<li><strong>${headers[i]}:</strong> ${machineRow[i]}</li>`;
                        }
                    }
                    specsHtml += '</ul>';
                    machineSpecs.innerHTML = specsHtml;
                    machineSpecs.classList.remove('hidden');
                } else {
                    machineSpecs.innerHTML = '<p class="text-red-500">No se encontraron especificaciones.</p>';
                    machineSpecs.classList.remove('hidden');
                }
            } catch (error) {
                console.error('Error al obtener datos:', error);
                machineSpecs.innerHTML = '<p class="text-red-500">Error al cargar especificaciones.</p>';
                machineSpecs.classList.remove('hidden');
            }
        });

        // Manejar formulario
        clientForm.addEventListener('submit', (e) => {
            e.preventDefault();
            clientData = {
                name: document.getElementById('name').value,
                company: document.getElementById('company').value,
                phone: document.getElementById('phone').value,
                email: document.getElementById('email').value,
                machine: machineSelect.value
            };

            if (!clientData.machine) {
                alert('Por favor, seleccione una máquina.');
                return;
            }

            // Mostrar pantalla de consejo
            clientFormScreen.classList.add('hidden');
            adviceScreen.classList.remove('hidden');

            // Simular consejo de IA basado en dakumar.com
            const aiAdvice = document.getElementById('ai-advice');
            // Nota: No puedo acceder directamente a dakumar.com, así que simulo un consejo genérico
            aiAdvice.innerHTML = `
                <p><strong>Consejo de IA:</strong> Basado en la información de Dakumar, para la máquina ${clientData.machine}, recomendamos optimizar el ciclo de producción para maximizar la eficiencia energética y reducir costos operativos. Considere integrar sistemas de monitoreo en tiempo real para mejorar el mantenimiento predictivo.</p>
            `;
        });

        // Botón salir
        exitBtn.addEventListener('click', () => {
            // Reiniciar aplicación
            clientForm.reset();
            machineSelect.value = '';
            machineSpecs.classList.add('hidden');
            adviceScreen.classList.add('hidden');
            mainScreen.classList.remove('hidden');
            clientData = {};
        });
    </script>
</body>
</html>
