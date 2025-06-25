<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>App de Gestión de Dakumaker</title>
    <script src="https://cdn.jsdelivr.net/npm/react@18.2.0/umd/react.production.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/umd/react-dom.production.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/babel-standalone@7.22.9/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://apis.google.com/js/api.js"></script>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
        const { useState, useEffect } = React;

        // Machine and material data (simplified, normally fetched from Google Sheets)
        const machines = [
            "DKM-130SV", "DKM-180SV", "DKM-250SV", "DKM-350SV", "DKM-450SV",
            "DKM-550SV", "DKM-650SV", "DKM-850SV", "DKM-1150SV", "DKM-1350SV",
            "DKM-1650SV", "DKM-2250SV", "DKM-2800SV"
        ];

        const materials = [
            "ABS", "ABS/PC", "AS(SAN)", "CA", "HDPE", "LDPE", "PA6", "PA66",
            "PA6 GF", "PA66 GF", "PBT", "PC", "PC GF", "PET", "PET GF", "PMMA",
            "POM", "PP", "PP TALC", "PPE/PA", "PPS", "PS", "PS HI", "PSU",
            "PPUVC", "UPVC", "TPEU", "TPEV"
        ];

        // Mock machine specifications (replace with actual data from Google Sheets)
        const machineSpecs = {
            "DKM-180SV": {
                clampingForce: "1800 kN",
                injectionVolume: "271 cm³",
                maxPressure: "140 MPa"
            },
            // Add other machines similarly
        };

        // Mock material parameters
        const materialParams = {
            "ABS": {
                density: "1.04 g/cm³",
                meltTemp: "220-260°C",
                moldTemp: "50-80°C"
            },
            // Add other materials similarly
        };

        // Google Sheets API configuration
        const CLIENT_ID = 'YOUR_CLIENT_ID'; // Replace with your Google API Client ID
        const API_KEY = 'YOUR_API_KEY'; // Replace with your Google API Key
        const SPREADSHEET_ID = '1GTY9WtufW1ZuOV2j8y23rBa8fsCDHXic-7X5YaBnjr8';
        const DISCOVERY_DOCS = ["https://sheets.googleapis.com/$discovery/rest?version=v4"];
        const SCOPES = "https://www.googleapis.com/auth/spreadsheets";

        function App() {
            const [screen, setScreen] = useState('main');
            const [clientData, setClientData] = useState({
                name: '', company: '', phone: '', email: ''
            });
            const [experience, setExperience] = useState(null);
            const [selectedMachine, setSelectedMachine] = useState('');
            const [machineNotes, setMachineNotes] = useState('');
            const [selectedMaterial, setSelectedMaterial] = useState('');
            const [productionData, setProductionData] = useState({
                monthlyPieces: '', pieceWeight: ''
            });
            const [recommendation, setRecommendation] = useState('');

            // Initialize Google API
            useEffect(() => {
                window.gapi.load('client:auth2', () => {
                    window.gapi.client.init({
                        apiKey: API_KEY,
                        clientId: CLIENT_ID,
                        discoveryDocs: DISCOVERY_DOCS,
                        scope: SCOPES
                    }).then(() => {
                        // Handle authentication if needed
                    });
                });
            }, []);

            // Save data to Google Sheets
            const saveToGoogleSheets = async (data) => {
                try {
                    await window.gapi.client.sheets.spreadsheets.values.append({
                        spreadsheetId: SPREADSHEET_ID,
                        range: 'Sheet1!A:G',
                        valueInputOption: 'RAW',
                        resource: {
                            values: [[
                                data.name, data.company, data.phone, data.email,
                                data.machine || '', data.material || '',
                                JSON.stringify(data.production || {})
                            ]]
                        }
                    });
                } catch (error) {
                    console.error('Error saving to Google Sheets:', error);
                }
            };

            // Simulate Grok AI recommendation
            const getRecommendation = () => {
                // Mock logic based on production data
                const { monthlyPieces, pieceWeight } = productionData;
                if (monthlyPieces > 10000 && pieceWeight > 100) {
                    return "Based on high production volume and piece weight, recommend DKM-850SV from dakumar.com for optimal performance.";
                }
                return "For moderate production, consider DKM-350SV from dakumar.com.";
            };

            const handleClientSubmit = (e) => {
                e.preventDefault();
                setScreen('experience');
            };

            const handleExperienceSelect = (exp) => {
                setExperience(exp);
                setScreen(exp ? 'machine' : 'material');
            };

            const handleMachineSelect = (machine) => {
                setSelectedMachine(machine);
            };

            const handleMaterialSelect = (material) => {
                setSelectedMaterial(material);
                setScreen('production');
            };

            const handleProductionSubmit = (e) => {
                e.preventDefault();
                const rec = getRecommendation();
                setRecommendation(rec);
                saveToGoogleSheets({
                    ...clientData,
                    machine: selectedMachine,
                    material: selectedMaterial,
                    production: productionData
                });
                setScreen('recommendation');
            };

            return (
                <div className="container mx-auto p-4">
                    {screen === 'main' && (
                        <div className="text-center">
                            <h1 className="text-2xl font-bold mb-4">Gestión de Clientes</h1>
                            <button
                                className="bg-blue-500 text-white px-4 py-2 rounded"
                                onClick={() => setScreen('client')}
                            >
                                Nuevo Cliente
                            </button>
                        </div>
                    )}

                    {screen === 'client' && (
                        <form className="max-w-md mx-auto" onSubmit={handleClientSubmit}>
                            <h2 className="text-xl font-bold mb-4">Datos del Cliente</h2>
                            <input
                                type="text"
                                placeholder="Nombre"
                                className="w-full mb-2 p-2 border rounded"
                                value={clientData.name}
                                onChange={(e) => setClientData({ ...clientData, name: e.target.value })}
                                required
                            />
                            <input
                                type="text"
                                placeholder="Empresa"
                                className="w-full mb-2 p-2 border rounded"
                                value={clientData.company}
                                onChange={(e) => setClientData({ ...clientData, company: e.target.value })}
                                required
                            />
                            <input
                                type="tel"
                                placeholder="Teléfono"
                                className="w-full mb-2 p-2 border rounded"
                                value={clientData.phone}
                                onChange={(e) => setClientData({ ...clientData, phone: e.target.value })}
                                required
                            />
                            <input
                                type="email"
                                placeholder="Correo"
                                className="w-full mb-2 p-2 border rounded"
                                value={clientData.email}
                                onChange={(e) => setClientData({ ...clientData, email: e.target.value })}
                                required
                            />
                            <button
                                type="submit"
                                className="bg-green-500 text-white px-4 py-2 rounded w-full"
                            >
                                Siguiente
                            </button>
                        </form>
                    )}

                    {screen === 'experience' && (
                        <div className="text-center">
                            <h2 className="text-xl font-bold mb-4">¿El cliente tiene experiencia?</h2>
                            <button
                                className="bg-blue-500 text-white px-4 py-2 rounded mr-2"
                                onClick={() => handleExperienceSelect(true)}
                            >
                                Con Experiencia
                            </button>
                            <button
                                className="bg-blue-500 text-white px-4 py-2 rounded"
                                onClick={() => handleExperienceSelect(false)}
                            >
                                Sin Experiencia
                            </button>
                        </div>
                    )}

                    {screen === 'machine' && (
                        <div>
                            <h2 className="text-xl font-bold mb-4">Seleccione una Máquina</h2>
                            <ul className="grid grid-cols-2 gap-2">
                                {machines.map((machine) => (
                                    <li key={machine}>
                                        <button
                                            className={`w-full p-2 border rounded ${selectedMachine === machine ? 'bg-blue-500 text-white' : ''}`}
                                            onClick={() => handleMachineSelect(machine)}
                                        >
                                            {machine}
                                        </button>
                                    </li>
                                ))}
                            </ul>
                            {selectedMachine && (
                                <div className="mt-4">
                                    <h3 className="text-lg font-bold">Especificaciones de {selectedMachine}</h3>
                                    <pre>{JSON.stringify(machineSpecs[selectedMachine] || {}, null, 2)}</pre>
                                    <textarea
                                        placeholder="Notas adicionales (otra tecnología)"
                                        className="w-full p-2 border rounded mt-2"
                                        value={machineNotes}
                                        onChange={(e) => setMachineNotes(e.target.value)}
                                    />
                                </div>
                            )}
                            <button
                                className="bg-red-500 text-white px-4 py-2 rounded mt-4"
                                onClick={() => setScreen('main')}
                            >
                                Salir
                            </button>
                        </div>
                    )}

                    {screen === 'material' && (
                        <div>
                            <h2 className="text-xl font-bold mb-4">Seleccione un Material</h2>
                            <ul className="grid grid-cols-3 gap-2">
                                {materials.map((material) => (
                                    <li key={material}>
                                        <button
                                            className="w-full p-2 border rounded"
                                            onClick={() => handleMaterialSelect(material)}
                                        >
                                            {material}
                                        </button>
                                    </li>
                                ))}
                            </ul>
                        </div>
                    )}

                    {screen === 'production' && (
                        <form className="max-w-md mx-auto" onSubmit={handleProductionSubmit}>
                            <h2 className="text-xl font-bold mb-4">Datos de Producción</h2>
                            <p>Material Seleccionado: {selectedMaterial}</p>
                            <pre>{JSON.stringify(materialParams[selectedMaterial] || {}, null, 2)}</pre>
                            <input
                                type="number"
                                placeholder="Número de piezas al mes"
                                className="w-full mb-2 p-2 border rounded"
                                value={productionData.monthlyPieces}
                                onChange={(e) => setProductionData({ ...productionData, monthlyPieces: e.target.value })}
                                required
                            />
                            <input
                                type="number"
                                placeholder="Peso de la pieza (g)"
                                className="w-full mb-2 p-2 border rounded"
                                value={productionData.pieceWeight}
                                onChange={(e) => setProductionData({ ...productionData, pieceWeight: e.target.value })}
                                required
                            />
                            <button
                                type="submit"
                                className="bg-green-500 text-white px-4 py-2 rounded w-full"
                            >
                                Siguiente
                            </button>
                        </form>
                    )}

                    {screen === 'recommendation' && (
                        <div className="text-center">
                            <h2 className="text-xl font-bold mb-4">Recomendación</h2>
                            <p>{recommendation}</p>
                            <button
                                className="bg-red-500 text-white px-4 py-2 rounded mt-4"
                                onClick={() => setScreen('main')}
                            >
                                Volver al Inicio
                            </button>
                        </div>
                    )}
                </div>
            );
        }

        ReactDOM.render(<App />, document.getElementById('root'));
    </script>
</body>
</html>
