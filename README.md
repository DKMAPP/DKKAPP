<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cotizador Dakumar</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Cotizador de Máquinas de Inyección</h1>
        <div class="logo">
            <img src="https://www.dakumar.com/images/logo.png" alt="Logo Dakumar">
        </div>
        <button id="nuevoCliente" class="btn-primario">Nuevo Cliente</button>
    </div>
    <script src="js/script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Datos del Cliente</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Información del Cliente</h1>
        <form id="formCliente">
            <div class="form-group">
                <label for="nombre">Nombre completo:</label>
                <input type="text" id="nombre" required>
            </div>
            <div class="form-group">
                <label for="empresa">Empresa:</label>
                <input type="text" id="empresa" required>
            </div>
            <div class="form-group">
                <label for="email">Email:</label>
                <input type="email" id="email" required>
            </div>
            <div class="form-group">
                <label for="telefono">Teléfono:</label>
                <input type="tel" id="telefono" required>
            </div>
            <div class="form-group">
                <label for="pais">País:</label>
                <input type="text" id="pais" required>
            </div>
            <div class="form-group">
                <label for="ciudad">Ciudad:</label>
                <input type="text" id="ciudad" required>
            </div>
            <div class="form-group">
                <label for="industria">Industria:</label>
                <input type="text" id="industria">
            </div>
            <button type="submit" class="btn-primario">Enviar</button>
        </form>
    </div>
    <script src="js/script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Selección de Máquina</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Seleccione una máquina</h1>
        <div class="maquina-info">
            <select id="selectMaquina" class="form-control">
                <option value="">-- Seleccione una máquina --</option>
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
            
            <div id="especificaciones" class="especificaciones-container">
                <!-- Aquí se mostrarán las especificaciones -->
            </div>
            
            <button id="btnSiguiente" class="btn-primario" disabled>Siguiente</button>
        </div>
    </div>
    <script src="js/data.js"></script>
    <script src="js/script.js"></script>
</body>
</html>
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cotización</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <div class="container">
        <h1>Resumen de Cotización</h1>
        <div class="cliente-info">
            <h2>Datos del Cliente</h2>
            <div id="datosCliente"></div>
        </div>
        
        <div class="cotizacion-container">
            <h2>Detalle de la Cotización</h2>
            <table id="tablaCotizacion">
                <thead>
                    <tr>
                        <th>Producto</th>
                        <th>Precio (USD)</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Aquí se llenará con JavaScript -->
                </tbody>
                <tfoot>
                    <tr>
                        <th>Total</th>
                        <th id="totalCotizacion">$0.00</th>
                    </tr>
                </tfoot>
            </table>
        </div>
        
        <button id="btnSalir" class="btn-primario">Salir</button>
    </div>
    <script src="js/data.js"></script>
    <script src="js/script.js"></script>
</body>
</html>
// Datos de las máquinas y sus especificaciones
const maquinas = {
    "DKM-130SV": {
        precio: 23718,
        especificaciones: {
            "Screw diameter B": "30mm",
            "Tie bar distance": "300x300mm",
            "Injection weight": "133g"
        },
        equipos: [
            { nombre: "Autocargador STL-300GE", precio: 496.80 },
            { nombre: "Secador XHD-25KG", precio: 408.00 },
            { nombre: "Molino XFS-180", precio: 862.80 },
            { nombre: "Torre de enfriamiento 10T", precio: 952.80 },
            { nombre: "Chiller de agua SC-05WCI", precio: 2208.00 },
            { nombre: "Chiller de aire SC-10ACI", precio: 4416.00 }
        ]
    },
    "DKM-180SV": {
        precio: 28580,
        especificaciones: {
            "Screw diameter B": "35mm",
            "Tie bar distance": "350x350mm",
            "Injection weight": "180g"
        },
        equipos: [
            { nombre: "Autocargador STL-300GN", precio: 549.60 },
            { nombre: "Secador XHD-50KG", precio: 502.80 },
            { nombre: "Molino XFS-230", precio: 1290.00 },
            { nombre: "Torre de enfriamiento 10T", precio: 952.80 },
            { nombre: "Chiller de agua SC-08WCI", precio: 3588.00 },
            { nombre: "Chiller de aire SC-10ACI", precio: 4416.00 }
        ]
    },
    // ... (agregar el resto de las máquinas con el mismo formato)
    "DKM-2800SV": {
        precio: 578856,
        especificaciones: {
            "Screw diameter B": "110mm",
            "Tie bar distance": "1200x1200mm",
            "Injection weight": "2800g"
        },
        equipos: [
            { nombre: "Autocargador STL-7.5HP", precio: 1774.80 },
            { nombre: "Secador XHD-600KG", precio: 2440.80 },
            { nombre: "Molino XFS-1000", precio: 12379.20 },
            { nombre: "Torre de enfriamiento 150T", precio: 6795.60 },
            { nombre: "Chiller de agua SC-50WCI", precio: 16560.00 },
            { nombre: "Chiller de aire SC-40ACI", precio: 16560.00 }
        ]
    }
};

// Almacenamiento local de datos del cliente
let clienteData = {};

// Función para guardar datos del cliente
function guardarCliente(data) {
    clienteData = data;
    localStorage.setItem('clienteData', JSON.stringify(data));
}

// Función para obtener datos del cliente
function obtenerCliente() {
    const data = localStorage.getItem('clienteData');
    return data ? JSON.parse(data) : {};
}
document.addEventListener('DOMContentLoaded', function() {
    // Navegación entre páginas
    if (document.getElementById('nuevoCliente')) {
        document.getElementById('nuevoCliente').addEventListener('click', function() {
            window.location.href = 'formulario.html';
        });
    }

    // Manejo del formulario de cliente
    if (document.getElementById('formCliente')) {
        document.getElementById('formCliente').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const cliente = {
                nombre: document.getElementById('nombre').value,
                empresa: document.getElementById('empresa').value,
                email: document.getElementById('email').value,
                telefono: document.getElementById('telefono').value,
                pais: document.getElementById('pais').value,
                ciudad: document.getElementById('ciudad').value,
                industria: document.getElementById('industria').value
            };
            
            guardarCliente(cliente);
            window.location.href = 'seleccion-maquina.html';
        });
    }

    // Selección de máquina
    if (document.getElementById('selectMaquina')) {
        const selectMaquina = document.getElementById('selectMaquina');
        const btnSiguiente = document.getElementById('btnSiguiente');
        const especificacionesDiv = document.getElementById('especificaciones');
        
        selectMaquina.addEventListener('change', function() {
            const modelo = this.value;
            if (modelo) {
                const maquina = maquinas[modelo];
                
                // Mostrar especificaciones
                let html = `<h3>${modelo}</h3>`;
                html += `<p><strong>Precio:</strong> $${maquina.precio.toLocaleString()}</p>`;
                html += '<h4>Especificaciones técnicas:</h4><ul>';
                
                for (const [key, value] of Object.entries(maquina.especificaciones)) {
                    html += `<li><strong>${key}:</strong> ${value}</li>`;
                }
                
                html += '</ul>';
                especificacionesDiv.innerHTML = html;
                
                // Habilitar botón siguiente
                btnSiguiente.disabled = false;
                btnSiguiente.onclick = function() {
                    localStorage.setItem('maquinaSeleccionada', modelo);
                    window.location.href = 'cotizacion.html';
                };
            } else {
                especificacionesDiv.innerHTML = '';
                btnSiguiente.disabled = true;
            }
        });
    }

    // Generación de cotización
    if (document.getElementById('tablaCotizacion')) {
        const modelo = localStorage.getItem('maquinaSeleccionada');
        const cliente = obtenerCliente();
        const maquina = maquinas[modelo];
        
        // Mostrar datos del cliente
        let clienteHtml = `
            <p><strong>Nombre:</strong> ${cliente.nombre}</p>
            <p><strong>Empresa:</strong> ${cliente.empresa}</p>
            <p><strong>Contacto:</strong> ${cliente.email} | ${cliente.telefono}</p>
            <p><strong>Ubicación:</strong> ${cliente.ciudad}, ${cliente.pais}</p>
        `;
        document.getElementById('datosCliente').innerHTML = clienteHtml;
        
        // Llenar tabla de cotización
        const tbody = document.querySelector('#tablaCotizacion tbody');
        let total = maquina.precio;
        
        // Agregar máquina principal
        let row = document.createElement('tr');
        row.innerHTML = `
            <td>${modelo}</td>
            <td>$${maquina.precio.toLocaleString()}</td>
        `;
        tbody.appendChild(row);
        
        // Agregar equipos auxiliares
        maquina.equipos.forEach(equipo => {
            row = document.createElement('tr');
            row.innerHTML = `
                <td>${equipo.nombre}</td>
                <td>$${equipo.precio.toLocaleString()}</td>
            `;
            tbody.appendChild(row);
            total += equipo.precio;
        });
        
        // Mostrar total
        document.getElementById('totalCotizacion').textContent = `$${total.toLocaleString()}`;
        
        // Botón salir
        document.getElementById('btnSalir').addEventListener('click', function() {
            window.location.href = 'index.html';
        });
    }
});
/* Estilos generales */
body {
    font-family: 'Arial', sans-serif;
    line-height: 1.6;
    margin: 0;
    padding: 0;
    color: #333;
    background-color: #f5f5f5;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

h1, h2, h3, h4 {
    color: #2c3e50;
}

/* Botones */
.btn-primario {
    background-color: #3498db;
    color: white;
    border: none;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    border-radius: 5px;
    display: block;
    margin: 20px auto;
    width: 200px;
    text-align: center;
}

.btn-primario:hover {
    background-color: #2980b9;
}

/* Formularios */
.form-group {
    margin-bottom: 15px;
}

.form-group label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
}

.form-group input, .form-control {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    box-sizing: border-box;
}

/* Tablas */
table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
}

th, td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

th {
    background-color: #3498db;
    color: white;
}

tr:hover {
    background-color: #f5f5f5;
}

/* Especificaciones */
.especificaciones-container {
    margin: 20px 0;
    padding: 15px;
    background-color: white;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0,0,0,0.1);
}

/* Logo */
.logo {
    text-align: center;
    margin: 20px 0;
}

.logo img {
    max-width: 200px;
    height: auto;
}

/* Responsivo */
@media (max-width: 768px) {
    .container {
        padding: 10px;
    }
    
    .btn-primario {
        width: 100%;
    }
}
