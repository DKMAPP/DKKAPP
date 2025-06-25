<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Client and Machine Selection App</title>
    <script src="https://cdn.jsdelivr.net/npm/react@18/umd/react.development.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/babel-standalone@6/babel.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body>
    <div id="root"></div>
    <script type="text/babel">
        const machines = [
            'DKM-130SV', 'DKM-180SV', 'DKM-250SV', 'DKM-350SV', 'DKM-450SV',
            'DKM-550SV', 'DKM-650SV', 'DKM-850SV', 'DKM-1150SV', 'DKM-1350SV',
            'DKM-1650SV', 'DKM-2250SV', 'DKM-2800SV'
        ];

        // Simulated machine specs (replace with actual data from dakumar.com)
        const machineSpecs = {
            'DKM-130SV': { screwDiameterB: '32 mm', tieBarDistance: '360 x 360 mm', injectionWeight: '100 g' },
            'DKM-180SV': { screwDiameterB: '36 mm', tieBarDistance: '410 x 410 mm', injectionWeight: '150 g' },
            'DKM-250SV': { screwDiameterB: '40 mm', tieBarDistance: '460 x 460 mm', injectionWeight: '200 g' },
            'DKM-350SV': { screwDiameterB: '45 mm', tieBarDistance: '510 x 510 mm', injectionWeight: '300 g' },
            'DKM-450SV': { screwDiameterB: '50 mm', tieBarDistance: '560 x 560 mm', injectionWeight: '400 g' },
            'DKM-550SV': { screwDiameterB: '55 mm', tieBarDistance: '610 x 610 mm', injectionWeight: '500 g' },
            'DKM-650SV': { screwDiameterB: '60 mm', tieBarDistance: '660 x 660 mm', injectionWeight: '600 g' },
            'DKM-850SV': { screwDiameterB: '65 mm', tieBarDistance: '710 x 710 mm', injectionWeight: '800 g' },
            'DKM-1150SV': { screwDiameterB: '70 mm', tieBarDistance: '760 x 760 mm', injectionWeight: '1000 g' },
            'DKM-1350SV': { screwDiameterB: '75 mm', tieBarDistance: '810 x 810 mm', injectionWeight: '1200 g' },
            'DKM-1650SV': { screwDiameterB: '80 mm', tieBarDistance: '860 x 860 mm', injectionWeight: '1500 g' },
            'DKM-2250SV': { screwDiameterB: '85 mm', tieBarDistance: '910 x 910 mm', injectionWeight: '2000 g' },
            'DKM-2800SV': { screwDiameterB: '90 mm', tieBarDistance: '960 x 960 mm', injectionWeight: '2500 g' }
        };

        // Simulated pricing data (replace with actual data from Google Sheet)
        const machinePrices = {
            'DKM-130SV': '$50,000', 'DKM-180SV': '$60,000', 'DKM-250SV': '$70,000',
            'DKM-350SV': '$80,000', 'DKM-450SV': '$90,000', 'DKM-550SV': '$100,000',
            'DKM-650SV': '$110,000', 'DKM-850SV': '$120,000', 'DKM-1150SV': '$130,000',
            'DKM-1350SV': '$140,000', 'DKM-1650SV': '$150,000', 'DKM-2250SV': '$160,000',
            'DKM-2800SV': '$170,000'
        };

        function App() {
            const [screen, setScreen] = React.useState('main');
            const [formData, setFormData] = React.useState({
                name: '', email: '', company: '', phone: '', address: ''
            });
            const [selectedMachine, setSelectedMachine] = React.useState('');

            const handleFormChange = (e) => {
                setFormData({ ...formData, [e.target.name]: e.target.value });
            };

            const handleMachineSelect = (e) => {
                setSelectedMachine(e.target.value);
            };

            const renderMainScreen = () => (
                <div className="flex flex-col items-center justify-center h-screen bg-gray-100">
                    <h1 className="text-3xl font-bold mb-8">Client Management</h1>
                    <button
                        onClick={() => setScreen('form')}
                        className="bg-blue-500 text-white px-6 py-3 rounded-lg hover:bg-blue-600"
                    >
                        New Client
                    </button>
                </div>
            );

            const renderFormScreen = () => (
                <div className="flex flex-col items-center p-6 bg-gray-100 min-h-screen">
                    <h2 className="text-2xl font-bold mb-6">New Client Form</h2>
                    <div className="w-full max-w-md bg-white p-6 rounded-lg shadow-md">
                        <div className="mb-4">
                            <label className="block text-gray-700">Name</label>
                            <input
                                type="text"
                                name="name"
                                value={formData.name}
                                onChange={handleFormChange}
                                className="w-full px-3 py-2 border rounded-lg"
                                required
                            />
                        </div>
                        <div className="mb-4">
                            <label className="block text-gray-700">Email</label>
                            <input
                                type="email"
                                name="email"
                                value={formData.email}
                                onChange={handleFormChange}
                                className="w-full px-3 py-2 border rounded-lg"
                                required
                            />
                        </div>
                        <div className="mb-4">
                            <label className="block text-gray-700">Company</label>
                            <input
                                type="text"
                                name="company"
                                value={formData.company}
                                onChange={handleFormChange}
                                className="w-full px-3 py-2 border rounded-lg"
                            />
                        </div>
                        <div className="mb-4">
                            <label className="block text-gray-700">Phone</label>
                            <input
                                type="tel"
                                name="phone"
                                value={formData.phone}
                                onChange={handleFormChange}
                                className="w-full px-3 py-2 border rounded-lg"
                            />
                        </div>
                        <div className="mb-4">
                            <label className="block text-gray-700">Address</label>
                            <textarea
                                name="address"
                                value={formData.address}
                                onChange={handleFormChange}
                                className="w-full px-3 py-2 border rounded-lg"
                            ></textarea>
                        </div>
                        <button
                            onClick={() => setScreen('machine')}
                            className="bg-blue-500 text-white px-6 py-3 rounded-lg hover:bg-blue-600 w-full"
                            disabled={!formData.name || !formData.email}
                        >
                            Next
                        </button>
                    </div>
                </div>
            );

            const renderMachineScreen = () => (
                <div className="flex flex-col items-center p-6 bg-gray-100 min-h-screen">
                    <h2 className="text-2xl font-bold mb-6">Select Machine</h2>
                    <div className="w-full max-w-md bg-white p-6 rounded-lg shadow-md">
                        <div className="mb-4">
                            <label className="block text-gray-700">Machine Model</label>
                            <select
                                value={selectedMachine}
                                onChange={handleMachineSelect}
                                className="w-full px-3 py-2 border rounded-lg"
                            >
                                <option value="">Select a machine</option>
                                {machines.map((machine) => (
                                    <option key={machine} value={machine}>
                                        {machine}
                                    </option>
                                ))}
                            </select>
                        </div>
                        {selectedMachine && machineSpecs[selectedMachine] && (
                            <div className="mb-4">
                                <h3 className="text-lg font-semibold">Specifications</h3>
                                <p>Screw Diameter B: {machineSpecs[selectedMachine].screwDiameterB}</p>
                                <p>Tie Bar Distance: {machineSpecs[selectedMachine].tieBarDistance}</p>
                                <p>Injection Weight: {machineSpecs[selectedMachine].injectionWeight}</p>
                            </div>
                        )}
                        <button
                            onClick={() => setScreen('price')}
                            className="bg-blue-500 text-white px-6 py-3 rounded-lg hover:bg-blue-600 w-full"
                            disabled={!selectedMachine}
                        >
                            Next
                        </button>
                    </div>
                </div>
            );

            const renderPriceScreen = () => (
                <div className="flex flex-col items-center p-6 bg-gray-100 min-h-screen">
                    <h2 className="text-2xl font-bold mb-6">Machine Price</h2>
                    <div className="w-full max-w-md bg-white p-6 rounded-lg shadow-md">
                        <p className="text-lg">Machine: {selectedMachine}</p>
                        <p className="text-lg font-semibold">Price: {machinePrices[selectedMachine]}</p>
                        <button
                            onClick={() => setScreen('machine')}
                            className="bg-gray-500 text-white px-6 py-3 rounded-lg hover:bg-gray-600 w-full mt-4"
                        >
                            Back
                        </button>
                    </div>
                </div>
            );

            return (
                <div>
                    {screen === 'main' && renderMainScreen()}
                    {screen === 'form' && renderFormScreen()}
                    {screen === 'machine' && renderMachineScreen()}
                    {screen === 'price' && renderPriceScreen()}
                </div>
            );
        }

        ReactDOM.render(<App />, document.getElementById('root'));
    </script>
</body>
</html>
