<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Client and Machine Selector</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 flex items-center justify-center h-screen">
    <div id="mainScreen" class="bg-white p-8 rounded-lg shadow-lg text-center">
        <h1 class="text-2xl font-bold mb-4">Bienvenido</h1>
        <button id="newClientBtn" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">Nuevo Cliente</button>
    </div>

    <div id="clientFormScreen" class="hidden bg-white p-8 rounded-lg shadow-lg w-full max-w-md">
        <h2 class="text-xl font-bold mb-4">Datos del Cliente</h2>
        <form id="clientForm">
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
                <label class="block text-gray-700">Correo</label>
                <input type="email" id="email" class="w-full p-2 border rounded" required>
            </div>
            <div class="mb-4">
                <label class="block text-gray-700">Link Grok</label>
                <input type="url" id="grokLink" class="w-full p-2 border rounded" value="https://grok.com/?ref=aiartweekly" readonly>
            </div>
            <div class="mb-4">
                <label class="block text-gray-700">Máquina</label>
                <select id="machineSelect" class="w-full p-2 border rounded" required>
                    <option value="" disabled selected>Selecciona una máquina</option>
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
            <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">Siguiente</button>
        </form>
    </div>

    <div id="machineDetailsScreen" class="hidden bg-white p-8 rounded-lg shadow-lg w-full max-w-md">
        <h2 class="text-xl font-bold mb-4">Especificaciones de la Máquina</h2>
        <div id="machineDetails" class="mb-4"></div>
        <div class="mb-4">
            <label class="block text-gray-700">Link Grok para Consejo</label>
            <input type="url" id="grokAdviceLink" class="w-full p-2 border rounded" value="https://grok.com/?ref=aiartweekly" readonly>
        </div>
        <div id="advice" class="mb-4 text-gray-700"></div>
        <button id="exitBtn" class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600">Salir</button>
    </div>

    <script>
        // Mock machine specifications (since Google Sheets API access is restricted)
        const machineSpecs = {
            "DKM-130SV": { clampForce: "130 tons", injectionWeight: "100g", maxPressure: "150 MPa" },
            "DKM-180SV": { clampForce: "180 tons", injectionWeight: "150g", maxPressure: "160 MPa" },
            "DKM-250SV": { clampForce: "250 tons", injectionWeight: "200g", maxPressure: "170 MPa" },
            "DKM-350SV": { clampForce: "350 tons", injectionWeight: "300g", maxPressure: "180 MPa" },
            "DKM-450SV": { clampForce: "450 tons", injectionWeight: "400g", maxPressure: "190 MPa" },
            "DKM-550SV": { clampForce: "550 tons", injectionWeight: "500g", maxPressure: "200 MPa" },
            "DKM-650SV": { clampForce: "650 tons", injectionWeight: "600g", maxPressure: "210 MPa" },
            "DKM-850SV": { clampForce: "850 tons", injectionWeight: "800g", maxPressure: "220 MPa" },
            "DKM-1150SV": { clampForce: "1150 tons", injectionWeight: "1100g", maxPressure: "230 MPa" },
            "DKM-1350SV": { clampForce: "1350 tons", injectionWeight: "1300g", maxPressure: "240 MPa" },
            "DKM-1650SV": { clampForce: "1650 tons", injectionWeight: "1600g", maxPressure: "250 MPa" },
            "DKM-2250SV": { clampForce: "2250 tons", injectionWeight: "2200g", maxPressure: "260 MPa" },
            "DKM-2800SV": { clampForce: "2800 tons", injectionWeight: "2700g", maxPressure: "270 MPa" }
        };

        // Mock advice from dakumar.com
        const advice = "Based on the machine selected, ensure proper maintenance schedules and check compatibility with your production needs. Visit dakumar.com for detailed machine manuals and support.";

        // Screen navigation
        const mainScreen = document.getElementById("mainScreen");
        const clientFormScreen = document.getElementById("clientFormScreen");
        const machineDetailsScreen = document.getElementById("machineDetailsScreen");

        document.getElementById("newClientBtn").addEventListener("click", () => {
            mainScreen.classList.add("hidden");
            clientFormScreen.classList.remove("hidden");
        });

        document.getElementById("clientForm").addEventListener("submit", (e) => {
            e.preventDefault();
            const machine = document.getElementById("machineSelect").value;
            const specs = machineSpecs[machine] || { error: "No specifications available" };
            const detailsDiv = document.getElementById("machineDetails");
            detailsDiv.innerHTML = `
                <p><strong>Modelo:</strong> ${machine}</p>
                <p><strong>Fuerza de Cierre:</strong> ${specs.clampForce || "N/A"}</p>
                <p><strong>Peso de Inyección:</strong> ${specs.injectionWeight || "N/A"}</p>
                <p><strong>Presión Máxima:</strong> ${specs.maxPressure || "N/A"}</p>
            `;
            document.getElementById("advice").textContent = advice;
            clientFormScreen.classList.add("hidden");
            machineDetailsScreen.classList.remove("hidden");
        });

        document.getElementById("exitBtn").addEventListener("click", () => {
            machineDetailsScreen.classList.add("hidden");
            mainScreen.classList.remove("hidden");
            document.getElementById("clientForm").reset();
        });
    </script>
</body>
</html>
