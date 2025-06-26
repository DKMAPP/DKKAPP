<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DKM Machine Selector</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
</head>
<body class="bg-gray-100 font-sans">
    <div id="app" class="container mx-auto p-4">
        <!-- First Screen: Machine Selection -->
        <div id="machine-selection" class="bg-white rounded-lg shadow-lg p-6 mb-6">
            <h1 class="text-2xl font-bold mb-4">Select a DKM Machine</h1>
            <select id="machine-select" class="w-full p-2 border rounded mb-4">
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
            <div id="machine-specs" class="mt-4 hidden">
                <h2 class="text-xl font-semibold">Machine Specifications</h2>
                <p id="screw-diameter" class="mb-2"></p>
                <p id="tie-bar-distance" class="mb-2"></p>
                <p id="injection-weight" class="mb-2"></p>
                <button id="next-btn" class="mt-4 bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">Next</button>
            </div>
        </div>

        <!-- Second Screen: Pricing Information -->
        <div id="pricing-info" class="bg-white rounded-lg shadow-lg p-6 hidden">
            <h1 class="text-2xl font-bold mb-4">Pricing Information</h1>
            <table class="w-full border-collapse mb-4">
                <thead>
                    <tr class="bg-gray-200">
                        <th class="border p-2">Item</th>
                        <th class="border p-2">Price</th>
                    </tr>
                </thead>
                <tbody id="pricing-table"></tbody>
                <tfoot>
                    <tr class="font-bold">
                        <td class="border p-2">Total</td>
                        <td id="total-price" class="border p-2"></td>
                    </tr>
                </tfoot>
            </table>
            <div class="flex space-x-4">
                <button id="exit-btn" class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600">Exit</button>
                <a href="https://docs.google.com/forms/d/e/1FAIpQLSd3rHnJlEC94iOAyeMJvPprfWpdtXDUNoMAzXXyPMwsKp9wJQ/viewform?usp=dialog" target="_blank" class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600">Customer Data</a>
            </div>
        </div>
    </div>

    <script>
        // Machine data with specifications and pricing
        const machineData = {
            "DKM-130SV": {
                specs: { screwDiameter: "36 mm", tieBarDistance: "410 x 410 mm", injectionWeight: "183 g" },
                price: 23718,
                auxiliaries: [
                    { name: "Autocargador STL-300GE", price: 496.80 },
                    { name: "Secador XHD-25KG", price: 408.00 },
                    { name: "Molino XFS-180", price: 862.80 },
                    { name: "Torre de enfriamiento 10T", price: 952.80 },
                    { name: "Chiller de agua SC-05WCI", price: 2208.00 },
                    { name: "Chiller de aire SC-10ACI", price: 4416.00 }
                ]
            },
            "DKM-180SV": {
                specs: { screwDiameter: "40 mm", tieBarDistance: "460 x 460 mm", injectionWeight: "286 g" },
                price: 28580,
                auxiliaries: [
                    { name: "Autocargador STL-300GN", price: 549.60 },
                    { name: "Secador XHD-50KG", price: 502.80 },
                    { name: "Molino XFS-230", price: 1290.00 },
                    { name: "Torre de enfriamiento 10T", price: 952.80 },
                    { name: "Chiller de agua SC-08WCI", price: 3588.00 },
                    { name: "Chiller de aire SC-10ACI", price: 4416.00 }
                ]
            },
            "DKM-250SV": {
                specs: { screwDiameter: "45 mm", tieBarDistance: "530 x 530 mm", injectionWeight: "405 g" },
                price: 36770,
                auxiliaries: [
                    { name: "Autocargador STL-450GE", price: 697.20 },
                    { name: "Secador XHD-50KG", price: 502.80 },
                    { name: "Molino XFS-400", price: 2151.60 },
                    { name: "Torre de enfriamiento 15T", price: 1083.60 },
                    { name: "Chiller de agua SC-10WCI", price: 3864.00 },
                    { name: "Chiller de aire SC-12ACI", price: 4968.00 }
                ]
            },
            "DKM-350SV": {
                specs: { screwDiameter: "50 mm", tieBarDistance: "610 x 610 mm", injectionWeight: "623 g" },
                price: 51388,
                auxiliaries: [
                    { name: "Autocargador STL-450GE", price: 697.20 },
                    { name: "Secador XHD-75KG", price: 594.00 },
                    { name: "Molino XFS-500", price: 2979.60 },
                    { name: "Torre de enfriamiento 20T", price: 1299.60 },
                    { name: "Chiller de agua SC-12WCI", price: 4416.00 },
                    { name: "Chiller de aire SC-15ACI", price: 6348.00 }
                ]
            },
            "DKM-450SV": {
                specs: { screwDiameter: "55 mm", tieBarDistance: "680 x 680 mm", injectionWeight: "893 g" },
                price: 67210,
                auxiliaries: [
                    { name: "Autocargador STL-600GN", price: 818.40 },
                    { name: "Secador XHD-100KG", price: 788.40 },
                    { name: "Molino XFS-600", price: 4864.80 },
                    { name: "Torre de enfriamiento 30T", price: 2335.20 },
                    { name: "Chiller de agua SC-15WCI", price: 4968.00 },
                    { name: "Chiller de aire SC-15ACI", price: 6348.00 }
                ]
            },
            "DKM-550SV": {
                specs: { screwDiameter: "60 mm", tieBarDistance: "730 x 730 mm", injectionWeight: "1134 g" },
                price: 80114,
                auxiliaries: [
                    { name: "Autocargador STL-600GN", price: 818.40 },
                    { name: "Secador XHD-150KG", price: 948.00 },
                    { name: "Molino XFS-600", price: 4864.80 },
                    { name: "Torre de enfriamiento 30T", price: 2335.20 },
                    { name: "Chiller de agua SC-20WCI", price: 4968.00 },
                    { name: "Chiller de aire SC-20ACI", price: 8004.00 }
                ]
            },
            "DKM-650SV": {
                specs: { screwDiameter: "65 mm", tieBarDistance: "810 x 810 mm", injectionWeight: "1466 g" },
                price: 107884,
                auxiliaries: [
                    { name: "Autocargador STL-800GN", price: 1035.60 },
                    { name: "Secador XHD-200KG", price: 1267.20 },
                    { name: "Molino XFS-800", price: 8766.00 },
                    { name: "Torre de enfriamiento 40T", price: 2463.60 },
                    { name: "Chiller de agua SC-25WCI", price: 8004.00 },
                    { name: "Chiller de aire SC-20ACI", price: 8004.00 }
                ]
            },
            "DKM-850SV": {
                specs: { screwDiameter: "70 mm", tieBarDistance: "910 x 910 mm", injectionWeight: "1882 g" },
                price: 128760,
                auxiliaries: [
                    { name: "Autocargador STL-800GN", price: 1035.60 },
                    { name: "Secador XHD-250KG", price: 1479.60 },
                    { name: "Molino XFS-800", price: 8766.00 },
                    { name: "Torre de enfriamiento 60T", price: 3110.40 },
                    { name: "Chiller de agua SC-25WCI", price: 8004.00 },
                    { name: "Chiller de aire SC-25ACI", price: 10350.00 }
                ]
            },
            "DKM-1150SV": {
                specs: { screwDiameter: "80 mm", tieBarDistance: "1010 x 1010 mm", injectionWeight: "2605 g" },
                price: 178824,
                auxiliaries: [
                    { name: "Autocargador STL-3.5HP", price: 1345.20 },
                    { name: "Secador XHD-250KG", price: 1479.60 },
                    { name: "Molino XFS-1000", price: 12379.20 },
                    { name: "Torre de enfriamiento 80T", price: 3814.80 },
                    { name: "Chiller de agua SC-30WCI", price: 9660.00 },
                    { name: "Chiller de aire SC-30ACI", price: 12420.00 }
                ]
            },
            "DKM-1350SV": {
                specs: { screwDiameter: "85 mm", tieBarDistance: "1080 x 1080 mm", injectionWeight: "3056 g" },
                price: 219500,
                auxiliaries: [
                    { name: "Autocargador STL-5HP", price: 1502.40 },
                    { name: "Secador XHD-250KG", price: 1479.60 },
                    { name: "Molino XFS-1000", price: 12379.20 },
                    { name: "Torre de enfriamiento 100T", price: 4508.40 },
                    { name: "Chiller de agua SC-30WCI", price: 9660.00 },
                    { name: "Chiller de aire SC-30ACI", price: 12420.00 }
                ]
            },
            "DKM-1650SV": {
                specs: { screwDiameter: "90 mm", tieBarDistance: "1160 x 1160 mm", injectionWeight: "3665 g" },
                price: 283300,
                auxiliaries: [
                    { name: "Autocargador STL-5HP", price: 1502.40 },
                    { name: "Secador XHD-300KG", price: 1666.80 },
                    { name: "Molino XFS-1000", price: 12379.20 },
                    { name: "Torre de enfriamiento 125T", price: 5319.60 },
                    { name: "Chiller de agua SC-30WCI", price: 9660.00 },
                    { name: "Chiller de aire SC-30ACI", price: 12420.00 }
                ]
            },
            "DKM-2250SV": {
                specs: { screwDiameter: "100 mm", tieBarDistance: "1260 x 1260 mm", injectionWeight: "4717 g" },
                price: 417860,
                auxiliaries: [
                    { name: "Autocargador STL-7.5HP", price: 1774.80 },
                    { name: "Secador XHD-400KG", price: 1944.00 },
                    { name: "Molino XFS-1000", price: 12379.20 },
                    { name: "Torre de enfriamiento 150T", price: 6795.60 },
                    { name: "Chiller de agua SC-40WCI", price: 13524.00 },
                    { name: "Chiller de aire SC-40ACI", price: 16560.00 }
                ]
            },
            "DKM-2800SV": {
                specs: { screwDiameter: "110 mm", tieBarDistance: "1360 x 1360 mm", injectionWeight: "5896 g" },
                price: 578856,
                auxiliaries: [
                    { name: "Autocargador STL-7.5HP", price: 1774.80 },
                    { name: "Secador XHD-600KG", price: 2440.80 },
                    { name: "Molino XFS-1000", price: 12379.20 },
                    { name: "Torre de enfriamiento 150T", price: 6795.60 },
                    { name: "Chiller de agua SC-50WCI", price: 16560.00 },
                    { name: "Chiller de aire SC-40ACI", price: 16560.00 }
                ]
            }
        };

        // DOM elements
        const machineSelect = document.getElementById('machine-select');
        const machineSpecsDiv = document.getElementById('machine-specs');
        const screwDiameter = document.getElementById('screw-diameter');
        const tieBarDistance = document.getElementById('tie-bar-distance');
        const injectionWeight = document.getElementById('injection-weight');
        const nextBtn = document.getElementById('next-btn');
        const pricingInfoDiv = document.getElementById('pricing-info');
        const pricingTable = document.getElementById('pricing-table');
        const totalPrice = document.getElementById('total-price');
        const exitBtn = document.getElementById('exit-btn');
        const machineSelectionDiv = document.getElementById('machine-selection');

        // Event listener for machine selection
        machineSelect.addEventListener('change', (e) => {
            const selectedMachine = e.target.value;
            if (selectedMachine && machineData[selectedMachine]) {
                const specs = machineData[selectedMachine].specs;
                screwDiameter.textContent = `Screw Diameter B: ${specs.screwDiameter}`;
                tieBarDistance.textContent = `Tie Bar Distance: ${specs.tieBarDistance}`;
                injectionWeight.textContent = `Injection Weight: ${specs.injectionWeight}`;
                machineSpecsDiv.classList.remove('hidden');
            } else {
                machineSpecsDiv.classList.add('hidden');
            }
        });

        // Event listener for Next button
        nextBtn.addEventListener('click', () => {
            const selectedMachine = machineSelect.value;
            if (selectedMachine && machineData[selectedMachine]) {
                machineSelectionDiv.classList.add('hidden');
                pricingInfoDiv.classList.remove('hidden');

                // Populate pricing table
                const data = machineData[selectedMachine];
                let total = data.price;
                pricingTable.innerHTML = '';

                // Add machine row
                const machineRow = document.createElement('tr');
                machineRow.innerHTML = `
                    <td class="border p-2">${selectedMachine}</td>
                    <td class="border p-2">$${data.price.toFixed(2)}</td>
                `;
                pricingTable.appendChild(machineRow);

                // Add auxiliary equipment rows (sorted descending by name)
                const sortedAuxiliaries = data.auxiliaries.sort((a, b) => b.name.localeCompare(a.name));
                sortedAuxiliaries.forEach(aux => {
                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td class="border p-2">${aux.name}</td>
                        <td class="border p-2">$${aux.price.toFixed(2)}</td>
                    `;
                    pricingTable.appendChild(row);
                    total += aux.price;
                });

                // Update total price
                totalPrice.textContent = `$${total.toFixed(2)}`;
            }
        });

        // Event listener for Exit button
        exitBtn.addEventListener('click', () => {
            window.location.reload();
        });
    </script>
</body>
</html>
