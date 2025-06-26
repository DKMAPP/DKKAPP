<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DKM Machine Selector</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
        }
        .container {
            max-width: 100%;
            padding: 1rem;
        }
        @media (min-width: 640px) {
            .container {
                max-width: 640px;
                margin: auto;
            }
        }
    </style>
</head>
<body class="bg-gray-100">
    <div class="container mx-auto">
        <!-- Screen 1: Machine Selection -->
        <div id="screen1" class="min-h-screen flex flex-col justify-center">
            <h1 class="text-2xl font-bold text-center mb-6">Select a DKM Machine</h1>
            <select id="machineSelect" class="w-full p-3 border rounded-lg mb-4 focus:outline-none focus:ring-2 focus:ring-blue-500">
                <option value="">Select a machine</option>
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
            <div id="machineSpecs" class="mt-4 p-4 bg-white rounded-lg shadow"></div>
            <button id="nextButton" class="w-full mt-6 bg-blue-500 text-white p-3 rounded-lg hover:bg-blue-600 disabled:bg-gray-400" disabled>Next</button>
        </div>

        <!-- Screen 2: Pricing Information -->
        <div id="screen2" class="min-h-screen flex flex-col justify-center hidden">
            <h1 class="text-2xl font-bold text-center mb-6">Pricing Details</h1>
            <div id="pricingInfo" class="p-4 bg-white rounded-lg shadow mb-6"></div>
            <div class="flex flex-col space-y-4">
                <button id="backButton" class="w-full bg-gray-500 text-white p-3 rounded-lg hover:bg-gray-600">Exit</button>
                <a href="https://docs.google.com/forms/d/e/1FAIpQLSd3rHnJlEC94iOAyeMJvPprfWpdtXDUNoMAzXXyPMwsKp9wJQ/viewform?usp=dialog" target="_blank" class="w-full bg-green-500 text-white p-3 rounded-lg hover:bg-green-600 text-center">Customer Data</a>
            </div>
        </div>
    </div>

    <script>
        // Machine specifications (mock data, as real-time scraping is not feasible)
        const machineData = {
            'DKM-130SV': { screwDiameterB: '40 mm', tieBarDistance: '410 x 410 mm', injectionWeight: '215 g' },
            'DKM-180SV': { screwDiameterB: '45 mm', tieBarDistance: '460 x 460 mm', injectionWeight: '315 g' },
            'DKM-250SV': { screwDiameterB: '50 mm', tieBarDistance: '510 x 510 mm', injectionWeight: '430 g' },
            'DKM-350SV': { screwDiameterB: '55 mm', tieBarDistance: '610 x 610 mm', injectionWeight: '645 g' },
            'DKM-450SV': { screwDiameterB: '60 mm', tieBarDistance: '710 x 710 mm', injectionWeight: '930 g' },
            'DKM-550SV': { screwDiameterB: '65 mm', tieBarDistance: '760 x 760 mm', injectionWeight: '1280 g' },
            'DKM-650SV': { screwDiameterB: '70 mm', tieBarDistance: '810 x 810 mm', injectionWeight: '1700 g' },
            'DKM-850SV': { screwDiameterB: '80 mm', tieBarDistance: '910 x 910 mm', injectionWeight: '2600 g' },
            'DKM-1150SV': { screwDiameterB: '90 mm', tieBarDistance: '1010 x 1010 mm', injectionWeight: '3800 g' },
            'DKM-1350SV': { screwDiameterB: '95 mm', tieBarDistance: '1080 x 1080 mm', injectionWeight: '4800 g' },
            'DKM-1650SV': { screwDiameterB: '100 mm', tieBarDistance: '1160 x 1160 mm', injectionWeight: '6000 g' },
            'DKM-2250SV': { screwDiameterB: '110 mm', tieBarDistance: '1250 x 1250 mm', injectionWeight: '8000 g' },
            'DKM-2800SV': { screwDiameterB: '120 mm', tieBarDistance: '1350 x 1350 mm', injectionWeight: '10000 g' }
        };

        // Pricing data
        const pricingData = {
            'DKM-130SV': { machinePrice: 23718, equipment: [
                { name: 'Autocargador STL-300GE', price: 496.80 },
                { name: 'Secador XHD-25KG', price: 408.00 },
                { name: 'Molino XFS-180', price: 862.80 },
                { name: 'Torre de enfriamiento 10T', price: 952.80 },
                { name: 'Chiller de agua SC-05WCI', price: 2208.00 },
                { name: 'Chiller de aire SC-10ACI', price: 4416.00 }
            ]},
            'DKM-180SV': { machinePrice: 28580, equipment: [
                { name: 'Autocargador STL-300GN', price: 549.60 },
                { name: 'Secador XHD-50KG', price: 502.80 },
                { name: 'Molino XFS-230', price: 1290.00 },
                { name: 'Torre de enfriamiento 10T', price: 952.80 },
                { name: 'Chiller de agua SC-08WCI', price: 3588.00 },
                { name: 'Chiller de aire SC-10ACI', price: 4416.00 }
            ]},
            'DKM-250SV': { machinePrice: 36770, equipment: [
                { name: 'Autocargador STL-450GE', price: 697.20 },
                { name: 'Secador XHD-50KG', price: 502.80 },
                { name: 'Molino XFS-400', price: 2151.60 },
                { name: 'Torre de enfriamiento 15T', price: 1083.60 },
                { name: 'Chiller de agua SC-10WCI', price: 3864.00 },
                { name: 'Chiller de aire SC-12ACI', price: 4968.00 }
            ]},
            'DKM-350SV': { machinePrice: 51388, equipment: [
                { name: 'Autocargador STL-450GE', price: 697.20 },
                { name: 'Secador XHD-75KG', price: 594.00 },
                { name: 'Molino XFS-500', price: 2979.60 },
                { name: 'Torre de enfriamiento 20T', price: 1299.60 },
                { name: 'Chiller de agua SC-12WCI', price: 4416.00 },
                { name: 'Chiller de aire SC-15ACI', price: 6348.00 }
            ]},
            'DKM-450SV': { machinePrice: 67210, equipment: [
                { name: 'Autocargador STL-600GN', price: 818.40 },
                { name: 'Secador XHD-100KG', price: 788.40 },
                { name: 'Molino XFS-600', price: 4864.80 },
                { name: 'Torre de enfriamiento 30T', price: 2335.20 },
                { name: 'Chiller de agua SC-15WCI', price: 4968.00 },
                { name: 'Chiller de aire SC-15ACI', price: 6348.00 }
            ]},
            'DKM-550SV': { machinePrice: 80114, equipment: [
                { name: 'Autocargador STL-600GN', price: 818.40 },
                { name: 'Secador XHD-150KG', price: 948.00 },
                { name: 'Molino XFS-600', price: 4864.80 },
                { name: 'Torre de enfriamiento 30T', price: 2335.20 },
                { name: 'Chiller de agua SC-20WCI', price: 4968.00 },
                { name: 'Chiller de aire SC-20ACI', price: 8004.00 }
            ]},
            'DKM-650SV': { machinePrice: 107884, equipment: [
                { name: 'Autocargador STL-800GN', price: 1035.60 },
                { name: 'Secador XHD-200KG', price: 1267.20 },
                { name: 'Molino XFS-800', price: 8766.00 },
                { name: 'Torre de enfriamiento 40T', price: 2463.60 },
                { name: 'Chiller de agua SC-25WCI', price: 8004.00 },
                { name: 'Chiller de aire SC-20ACI', price: 8004.00 }
            ]},
            'DKM-850SV': { machinePrice: 128760, equipment: [
                { name: 'Autocargador STL-800GN', price: 1035.60 },
                { name: 'Secador XHD-250KG', price: 1479.60 },
                { name: 'Molino XFS-800', price: 8766.00 },
                { name: 'Torre de enfriamiento 60T', price: 3110.40 },
                { name: 'Chiller de agua SC-25WCI', price: 8004.00 },
                { name: 'Chiller de aire SC-25ACI', price: 10350.00 }
            ]},
            'DKM-1150SV': { machinePrice: 178824, equipment: [
                { name: 'Autocargador STL-3.5HP', price: 1345.20 },
                { name: 'Secador XHD-250KG', price: 1479.60 },
                { name: 'Molino XFS-1000', price: 12379.20 },
                { name: 'Torre de enfriamiento 80T', price: 3814.80 },
                { name: 'Chiller de agua SC-30WCI', price: 9660.00 },
                { name: 'Chiller de aire SC-30ACI', price: 12420.00 }
            ]},
            'DKM-1350SV': { machinePrice: 219500, equipment: [
                { name: 'Autocargador STL-5HP', price: 1502.40 },
                { name: 'Secador XHD-250KG', price: 1479.60 },
                { name: 'Molino XFS-1000', price: 12379.20 },
                { name: 'Torre de enfriamiento 100T', price: 4508.40 },
                { name: 'Chiller de agua SC-30WCI', price: 9660.00 },
                { name: 'Chiller de aire SC-30ACI', price: 12420.00 }
            ]},
            'DKM-1650SV': { machinePrice: 283300, equipment: [
                { name: 'Autocargador STL-5HP', price: 1502.40 },
                { name: 'Secador XHD-300KG', price: 1666.80 },
                { name: 'Molino XFS-1000', price: 12379.20 },
                { name: 'Torre de enfriamiento 125T', price: 5319.60 },
                { name: 'Chiller de agua SC-30WCI', price: 9660.00 },
                { name: 'Chiller de aire SC-30ACI', price: 12420.00 }
            ]},
            'DKM-2250SV': { machinePrice: 417860, equipment: [
                { name: 'Autocargador STL-7.5HP', price: 1774.80 },
                { name: 'Secador XHD-400KG', price: 1944.00 },
                { name: 'Molino XFS-1000', price: 12379.20 },
                { name: 'Torre de enfriamiento 150T', price: 6795.60 },
                { name: 'Chiller de agua SC-40WCI', price: 13524.00 },
                { name: 'Chiller de aire SC-40ACI', price: 16560.00 }
            ]},
            'DKM-2800SV': { machinePrice: 578856, equipment: [
                { name: 'Autocargador STL-7.5HP', price: 1774.80 },
                { name: 'Secador XHD-600KG', price: 2440.80 },
                { name: 'Molino XFS-1000', price: 12379.20 },
                { name: 'Torre de enfriamiento 150T', price: 6795.60 },
                { name: 'Chiller de agua SC-50WCI', price: 16560.00 },
                { name: 'Chiller de aire SC-40ACI', price: 16560.00 }
            ]}
        };

        const machineSelect = document.getElementById('machineSelect');
        const machineSpecs = document.getElementById('machineSpecs');
        const nextButton = document.getElementById('nextButton');
        const screen1 = document.getElementById('screen1');
        const screen2 = document.getElementById('screen2');
        const pricingInfo = document.getElementById('pricingInfo');
        const backButton = document.getElementById('backButton');

        machineSelect.addEventListener('change', () => {
            const selectedMachine = machineSelect.value;
            if (selectedMachine && machineData[selectedMachine]) {
                const specs = machineData[selectedMachine];
                machineSpecs.innerHTML = `
                    <h2 class="text-xl font-semibold mb-2">${selectedMachine}</h2>
                    <p><strong>Screw Diameter B:</strong> ${specs.screwDiameterB}</p>
                    <p><strong>Tie Bar Distance:</strong> ${specs.tieBarDistance}</p>
                    <p><strong>Injection Weight:</strong> ${specs.injectionWeight}</p>
                `;
                nextButton.disabled = false;
            } else {
                machineSpecs.innerHTML = '';
                nextButton.disabled = true;
            }
        });

        nextButton.addEventListener('click', () => {
            const selectedMachine = machineSelect.value;
            if (selectedMachine && pricingData[selectedMachine]) {
                const data = pricingData[selectedMachine];
                const items = [
                    { name: selectedMachine, price: data.machinePrice },
                    ...data.equipment
                ].sort((a, b) => b.price - a.price); // Sort by price descending

                let total = items.reduce((sum, item) => sum + item.price, 0);
                pricingInfo.innerHTML = `
                    <h2 class="text-xl font-semibold mb-4">Pricing Breakdown</h2>
                    ${items.map(item => `
                        <p><strong>${item.name}:</strong> $${item.price.toFixed(2)}</p>
                    `).join('')}
                    <p class="mt-4 font-bold"><strong>Total:</strong> $${total.toFixed(2)}</p>
                `;
                screen1.classList.add('hidden');
                screen2.classList.remove('hidden');
            }
        });

        backButton.addEventListener('click', () => {
            window.location.reload(); // Exit by reloading the page
        });
    </script>
</body>
</html>
