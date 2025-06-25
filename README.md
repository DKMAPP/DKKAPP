<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Client Management App</title>
  <script src="https://cdn.jsdelivr.net/npm/react@18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/react-dom@18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/babel-standalone@6.26.0/babel.min.js"></script>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
  <div id="root"></div>
  <script type="text/babel">
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

    // Mock machine specs and material parameters (replace with actual data from Google Sheets)
    const machineSpecs = {
      "DKM-180SV": {
        Clamping_Force: "1800 kN",
        Injection_Weight: "259 g",
        Tie_Bar_Distance: "460 x 460 mm",
        // Add more specs from your Google Sheet
      },
      // Add other machines
    };

    const materialParams = {
      "ABS": {
        Melt_Temperature: "220-260°C",
        Mold_Temperature: "50-80°C",
        Injection_Pressure: "80-120 MPa",
        // Add more params from your Google Sheet
      },
      // Add other materials
    };

    // Mock Grok AI recommendation function
    async function getGrokRecommendation(piecesPerMonth, pieceWeight, material) {
      // Placeholder for actual Grok API call
      return {
        recommendedMachine: "DKM-250SV",
        advice: `Based on ${piecesPerMonth} pieces/month and ${pieceWeight}g piece weight with ${material}, the DKM-250SV offers optimal efficiency and capacity. Consider this machine for cost-effective production.`
      };
    }

    // Mock Google Sheets API save function
    async function saveToGoogleSheets(data) {
      console.log("Saving to Google Sheets:", data);
      // Implement actual Google Sheets API call here
      // Requires Google Cloud project setup and service account credentials
      return { success: true };
    }

    function App() {
      const [step, setStep] = React.useState("home");
      const [clientData, setClientData] = React.useState({
        name: "", company: "", phone: "", email: ""
      });
      const [experience, setExperience] = React.useState(null);
      const [selectedMachine, setSelectedMachine] = React.useState("");
      const [machineNotes, setMachineNotes] = React.useState("");
      const [selectedMaterial, setSelectedMaterial] = React.useState("");
      const [productionData, setProductionData] = React.useState({
        piecesPerMonth: "", pieceWeight: ""
      });
      const [recommendation, setRecommendation] = React.useState(null);

      const handleClientSubmit = (e) => {
        e.preventDefault();
        setStep("experience");
      };

      const handleExperienceSelect = (exp) => {
        setExperience(exp);
        setStep(exp ? "machine" : "material");
      };

      const handleMachineSelect = (machine) => {
        setSelectedMachine(machine);
      };

      const handleMaterialSelect = (material) => {
        setSelectedMaterial(material);
        setStep("production");
      };

      const handleProductionSubmit = async (e) => {
        e.preventDefault();
        const rec = await getGrokRecommendation(
          productionData.piecesPerMonth,
          productionData.pieceWeight,
          selectedMaterial
        );
        setRecommendation(rec);
        setStep("recommendation");

        // Save all data to Google Sheets
        const dataToSave = {
          ...clientData,
          experience,
          machine: selectedMachine,
          machineNotes,
          material: selectedMaterial,
          ...productionData,
          recommendedMachine: rec.recommendedMachine,
          advice: rec.advice
        };
        await saveToGoogleSheets(dataToSave);
      };

      return (
        <div className="min-h-screen bg-gray-100 flex items-center justify-center p-4">
          {step === "home" && (
            <button
              className="bg-blue-500 text-white px-6 py-3 rounded-lg hover:bg-blue-600"
              onClick={() => setStep("client")}
            >
              Nuevo Cliente
            </button>
          )}

          {step === "client" && (
            <form onSubmit={handleClientSubmit} className="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
              <h2 className="text-2xl mb-4">Datos del Cliente</h2>
              <input
                type="text"
                placeholder="Nombre"
                value={clientData.name}
                onChange={(e) => setClientData({ ...clientData, name: e.target.value })}
                className="w-full mb-4 p-2 border rounded"
                required
              />
              <input
                type="text"
                placeholder="Empresa"
                value={clientData.company}
                onChange={(e) => setClientData({ ...clientData, company: e.target.value })}
                className="w-full mb-4 p-2 border rounded"
                required
              />
              <input
                type="tel"
                placeholder="Teléfono"
                value={clientData.phone}
                onChange={(e) => setClientData({ ...clientData, phone: e.target.value })}
                className="w-full mb-4 p-2 border rounded"
                required
              />
              <input
                type="email"
                placeholder="Correo"
                value={clientData.email}
                onChange={(e) => setClientData({ ...clientData, email: e.target.value })}
                className="w-full mb-4 p-2 border rounded"
                required
              />
              <button
                type="submit"
                className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
              >
                Siguiente
              </button>
            </form>
          )}

          {step === "experience" && (
            <div className="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
              <h2 className="text-2xl mb-4">¿Tiene experiencia?</h2>
              <div className="flex space-x-4">
                <button
                  className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
                  onClick={() => handleExperienceSelect(true)}
                >
                  Con Experiencia
                </button>
                <button
                  className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
                  onClick={() => handleExperienceSelect(false)}
                >
                  Sin Experiencia
                </button>
              </div>
            </div>
          )}

          {step === "machine" && (
            <div className="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
              <h2 className="text-2xl mb-4">Seleccione una Máquina</h2>
              <div className="grid grid-cols-2 gap-2 mb-4">
                {machines.map((machine) => (
                  <button
                    key={machine}
                    className={`p-2 border rounded ${selectedMachine === machine ? "bg-blue-500 text-white" : "bg-gray-100"}`}
                    onClick={() => handleMachineSelect(machine)}
                  >
                    {machine}
                  </button>
                ))}
              </div>
              {selectedMachine && (
                <div className="mb-4">
                  <h3 className="text-xl mb-2">Especificaciones de {selectedMachine}</h3>
                  {machineSpecs[selectedMachine] && (
                    <ul className="list-disc pl-5">
                      {Object.entries(machineSpecs[selectedMachine]).map(([key, value]) => (
                        <li key={key}>{key}: {value}</li>
                      ))}
                    </ul>
                  )}
                  <textarea
                    placeholder="Notas (otra tecnología requerida)"
                    value={machineNotes}
                    onChange={(e) => setMachineNotes(e.target.value)}
                    className="w-full mt-4 p-2 border rounded"
                  />
                </div>
              )}
              <button
                className="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600"
                onClick={() => setStep("home")}
              >
                Salir
              </button>
            </div>
          )}

          {step === "material" && (
            <div className="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
              <h2 className="text-2xl mb-4">Seleccione un Material</h2>
              <div className="grid grid-cols-3 gap-2 mb-4">
                {materials.map((material) => (
                  <button
                    key={material}
                    className={`p-2 border rounded ${selectedMaterial === material ? "bg-blue-500 text-white" : "bg-gray-100"}`}
                    onClick={() => handleMaterialSelect(material)}
                  >
                    {material}
                  </button>
                ))}
              </div>
            </div>
          )}

          {step === "production" && (
            <form onSubmit={handleProductionSubmit} className="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
              <h2 className="text-2xl mb-4">Datos de Producción</h2>
              {selectedMaterial && (
                <div className="mb-4">
                  <h3 className="text-xl mb-2">Parámetros de {selectedMaterial}</h3>
                  {materialParams[selectedMaterial] && (
                    <ul className="list-disc pl-5">
                      {Object.entries(materialParams[selectedMaterial]).map(([key, value]) => (
                        <li key={key}>{key}: {value}</li>
                      ))}
                    </ul>
                  )}
                </div>
              )}
              <input
                type="number"
                placeholder="Número de piezas al mes"
                value={productionData.piecesPerMonth}
                onChange={(e) => setProductionData({ ...productionData, piecesPerMonth: e.target.value })}
                className="w-full mb-4 p-2 border rounded"
                required
              />
              <input
                type="number"
                placeholder="Peso de la pieza (g)"
                value={productionData.pieceWeight}
                onChange={(e) => setProductionData({ ...productionData, pieceWeight: e.target.value })}
                className="w-full mb-4 p-2 border rounded"
                required
              />
              <button
                type="submit"
                className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
              >
                Siguiente
              </button>
            </form>
          )}

          {step === "recommendation" && recommendation && (
            <div className="bg-white p-6 rounded-lg shadow-md w-full max-w-md">
              <h2 className="text-2xl mb-4">Recomendación</h2>
              <p className="mb-4">
                <strong>Máquina Recomendada:</strong> {recommendation.recommendedMachine}
              </p>
              <p className="mb-4">{recommendation.advice}</p>
              <button
                className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
                onClick={() => setStep("home")}
              >
                Volver al Inicio
              </button>
            </div>
          )}
        </div>
      );
    }

    ReactDOM.render(<App />, document.getElementById("root"));
  </script>
</body>
</html>
