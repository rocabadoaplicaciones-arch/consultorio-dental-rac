<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RAC - Consultorio Dental Multisucursal</title>
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body class="bg-slate-900 font-sans text-slate-100 antialiased min-h-screen flex flex-col">

    <!-- PANTALLA DE ACCESO / LOGIN -->
    <div id="loginScreen" class="fixed inset-0 bg-slate-950/95 z-50 flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-800 rounded-2xl shadow-2xl max-w-md w-full p-6 sm:p-8 text-center relative overflow-hidden">
            <div class="absolute top-0 left-0 right-0 h-1.5 bg-blue-600"></div>
            <div class="w-16 h-16 bg-blue-900/50 text-blue-400 border border-blue-700/50 rounded-full flex items-center justify-center mx-auto mb-4 text-2xl shadow-inner">
                <i class="fa-solid fa-tooth"></i>
            </div>
            <h1 class="text-2xl font-bold text-white tracking-wide">Consultorio Dental RAC</h1>
            <p class="text-xs text-blue-400 mt-1 uppercase tracking-wider font-semibold">Rocabado Auditores Consultores</p>
            
            <div class="mt-6 space-y-3">
                <button onclick="loginAs('admin')" class="w-full bg-blue-600 hover:bg-blue-500 text-white font-medium py-3 px-4 rounded-xl shadow-lg transition flex items-center justify-center gap-2 cursor-pointer">
                    <i class="fa-solid fa-user-shield"></i> Administrador / Dueño (Master)
                </button>
                <button onclick="loginAs('operador')" class="w-full bg-slate-800 hover:bg-slate-700 text-slate-200 border border-slate-700 font-medium py-3 px-4 rounded-xl transition flex items-center justify-center gap-2 cursor-pointer">
                    <i class="fa-solid fa-user-tie"></i> Operador de Sucursal
                </button>
            </div>
            <div class="mt-6 pt-4 border-t border-slate-800 text-[11px] text-slate-400 flex justify-between items-center">
                <span>Soporte: Mickel Rocabado</span>
                <a href="https://wa.me/" target="_blank" class="text-emerald-400 hover:underline flex items-center gap-1 font-medium"><i class="fa-brands fa-whatsapp"></i> WhatsApp</a>
            </div>
        </div>
    </div>

    <!-- APLICACIÓN PRINCIPAL -->
    <div id="appContainer" class="flex-1 flex flex-col hidden">
        <!-- Header -->
        <header class="bg-slate-900 border-b border-slate-800 sticky top-0 z-40 shadow-md">
            <div class="max-w-7xl mx-auto px-4 py-3 flex justify-between items-center">
                <div class="flex items-center gap-3">
                    <div class="bg-blue-600 p-2 rounded-xl text-white shadow-md">
                        <i class="fa-solid fa-tooth text-lg"></i>
                    </div>
                    <div>
                        <h2 class="font-bold text-sm sm:text-base text-white leading-tight">Consultorio Dental RAC</h2>
                        <p class="text-xs text-blue-400" id="headerBranch">Sucursal Activa: La Pampa</p>
                    </div>
                </div>
                <div class="flex items-center gap-2">
                    <select id="branchSelector" onchange="switchBranch(this.value)" class="bg-slate-800 text-slate-200 text-xs rounded-xl px-3 py-2 border border-slate-700 outline-none font-medium cursor-pointer">
                        <option value="pampa">Sucursal La Pampa</option>
                        <option value="villa">Sucursal La Villa</option>
                        <option value="consolidado">Consolidado Total</option>
                    </select>
                    <button onclick="logout()" class="bg-slate-800 hover:bg-rose-900/40 text-slate-300 hover:text-rose-400 p-2 rounded-xl text-xs transition border border-slate-700 cursor-pointer" title="Cerrar Sesión">
                        <i class="fa-solid fa-right-from-bracket"></i>
                    </button>
                </div>
            </div>
        </header>

        <!-- Navegación por Pestañas -->
        <nav class="bg-slate-900/80 backdrop-blur border-b border-slate-800 px-4">
            <div class="max-w-7xl mx-auto flex gap-2 overflow-x-auto py-2 no-scrollbar">
                <button onclick="switchTab('dashboard')" id="tab-dashboard" class="px-4 py-2 rounded-xl text-xs font-semibold bg-blue-600 text-white transition flex items-center gap-2 shrink-0 cursor-pointer">
                    <i class="fa-solid fa-chart-pie"></i> Resumen Gerencial
                </button>
                <button onclick="switchTab('pos')" id="tab-pos" class="px-4 py-2 rounded-xl text-xs font-semibold bg-slate-800 text-slate-300 hover:bg-slate-700 transition flex items-center gap-2 shrink-0 cursor-pointer">
                    <i class="fa-solid fa-cash-register"></i> POS & Ingresos
                </button>
                <button onclick="switchTab('agenda')" id="tab-agenda" class="px-4 py-2 rounded-xl text-xs font-semibold bg-slate-800 text-slate-300 hover:bg-slate-700 transition flex items-center gap-2 shrink-0 cursor-pointer">
                    <i class="fa-solid fa-calendar-days"></i> Agenda & Odontograma
                </button>
                <button onclick="switchTab('inventario')" id="tab-inventario" class="px-4 py-2 rounded-xl text-xs font-semibold bg-slate-800 text-slate-300 hover:bg-slate-700 transition flex items-center gap-2 shrink-0 cursor-pointer">
                    <i class="fa-solid fa-boxes-stacked"></i> Inventario & Stock
                </button>
                <button onclick="switchTab('gastos')" id="tab-gastos" class="px-4 py-2 rounded-xl text-xs font-semibold bg-slate-800 text-slate-300 hover:bg-slate-700 transition flex items-center gap-2 shrink-0 cursor-pointer">
                    <i class="fa-solid fa-file-invoice-dollar"></i> Egresos & Caja
                </button>
            </div>
        </nav>

        <!-- Contenido de las Pestañas -->
        <main class="flex-1 max-w-7xl w-full mx-auto p-4 sm:p-6 mb-16">
            
            <!-- PESTAÑA 1: DASHBOARD -->
            <div id="content-dashboard" class="space-y-6">
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                    <div class="bg-slate-900 border border-slate-800 p-4 rounded-2xl shadow-sm">
                        <p class="text-xs text-slate-400 font-medium">Ingresos en Efectivo</p>
                        <h3 class="text-2xl font-bold text-emerald-400 mt-1" id="dashEfectivo">Bs. 0.00</h3>
                        <span class="text-[10px] text-slate-500 mt-1 block">Arqueo físico de caja</span>
                    </div>
                    <div class="bg-slate-900 border border-slate-800 p-4 rounded-2xl shadow-sm">
                        <p class="text-xs text-slate-400 font-medium">Ingresos por QR / Transf.</p>
                        <h3 class="text-2xl font-bold text-indigo-400 mt-1" id="dashQR">Bs. 0.00</h3>
                        <span class="text-[10px] text-slate-500 mt-1 block">Abonos digitales directos</span>
                    </div>
                    <div class="bg-slate-900 border border-slate-800 p-4 rounded-2xl shadow-sm">
                        <p class="text-xs text-slate-400 font-medium">Servicios Realizados</p>
                        <h3 class="text-2xl font-bold text-blue-400 mt-1" id="dashServicios">0</h3>
                        <span class="text-[10px] text-slate-500 mt-1 block">Pacientes atendidos hoy</span>
                    </div>
                    <div class="bg-slate-900 border border-slate-800 p-4 rounded-2xl shadow-sm">
                        <p class="text-xs text-slate-400 font-medium">Gastos Operativos</p>
                        <h3 class="text-2xl font-bold text-rose-400 mt-1" id="dashGastos">Bs. 0.00</h3>
                        <span class="text-[10px] text-slate-500 mt-1 block">Insumos y sueldos del día</span>
                    </div>
                </div>

                <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-sm flex flex-col sm:flex-row justify-between items-center gap-4">
                    <div>
                        <h3 class="font-bold text-base text-white">Arqueo Diario y Cierre de Caja</h3>
                        <p class="text-xs text-slate-400 mt-0.5">Consolida los ingresos de efectivo y QR por sucursal listo para enviar vía WhatsApp.</p>
                    </div>
                    <button onclick="generarCierreWhatsApp()" class="bg-emerald-600 hover:bg-emerald-500 text-white font-medium py-2.5 px-5 rounded-xl text-xs transition flex items-center gap-2 shadow-md cursor-pointer">
                        <i class="fa-brands fa-whatsapp text-base"></i> Enviar Cierre por WhatsApp
                    </button>
                </div>
            </div>

            <!-- PESTAÑA 2: POS & INGRESOS -->
            <div id="content-pos" class="hidden space-y-6">
                <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-sm max-w-2xl mx-auto">
                    <h3 class="font-bold text-lg text-white mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-cash-register text-emerald-400"></i> Registrar Cobro de Servicio Dental
                    </h3>
                    <form onsubmit="registrarVenta(event)" class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium text-slate-300 mb-1">Nombre del Paciente</label>
                            <input type="text" id="posPaciente" required placeholder="Ej. Juan Pérez" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white outline-none focus:border-blue-500">
                        </div>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-medium text-slate-300 mb-1">Tratamiento / Servicio</label>
                                <select id="posServicio" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white outline-none focus:border-blue-500">
                                    <option value="Resina estética|150">Resina estética (Bs. 150)</option>
                                    <option value="Limpieza ultrasonido|120">Limpieza con ultrasonido (Bs. 120)</option>
                                    <option value="Endodoncia|450">Endodoncia (Bs. 450)</option>
                                    <option value="Extracción simple|100">Extracción simple (Bs. 100)</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium text-slate-300 mb-1">Odontólogo Responsable</label>
                                <select id="posDoctor" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white outline-none focus:border-blue-500">
                                    <option value="Dr. Mickel Rocabado">Dr. Mickel Rocabado</option>
                                    <option value="Dra. Andrea Suárez">Dra. Andrea Suárez</option>
                                </select>
                            </div>
                        </div>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-medium text-slate-300 mb-1">Forma de Pago</label>
                                <select id="posMetodo" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white outline-none focus:border-blue-500">
                                    <option value="Efectivo">Efectivo</option>
                                    <option value="QR">QR / Transferencia</option>
                                    <option value="Credito">Crédito (Por Cobrar)</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium text-slate-300 mb-1">Monto en Bolivianos (Bs.)</label>
                                <input type="number" id="posMonto" required placeholder="150" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white outline-none focus:border-blue-500">
                            </div>
                        </div>
                        <button type="submit" class="w-full bg-blue-600 hover:bg-blue-500 text-white font-medium py-3 rounded-xl shadow-lg transition text-sm cursor-pointer">
                            Cobrar y Emitir Comprobante
                        </button>
                    </form>
                </div>
            </div>

            <!-- PESTAÑA 3: AGENDA Y ODONTOGRAMA -->
            <div id="content-agenda" class="hidden space-y-6">
                <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-sm">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="font-bold text-lg text-white flex items-center gap-2">
                            <i class="fa-solid fa-tooth text-blue-400"></i> Odontograma Ejecutivo Interactivo
                        </h3>
                        <span class="text-xs bg-blue-900/50 border border-blue-700/50 text-blue-300 px-3 py-1 rounded-full">Selección de Pieza</span>
                    </div>
                    <p class="text-xs text-slate-400 mb-6">Selecciona una pieza dental para registrar el diagnóstico o tratamiento correspondiente en la sucursal activa.</p>
                    <div class="grid grid-cols-8 sm:grid-cols-16 gap-2 text-center">
                        <!-- Simulación piezas dentales superiores e inferiores -->
                        <script>
                            for(let i=11; i<=18; i++) document.write(`<div onclick="seleccionarPieza(${i})" class="bg-slate-800 hover:bg-blue-600 border border-slate-700 rounded-xl p-3 text-xs font-bold cursor-pointer transition">${i}</div>`);
                            for(let i=21; i<=28; i++) document.write(`<div onclick="seleccionarPieza(${i})" class="bg-slate-800 hover:bg-blue-600 border border-slate-700 rounded-xl p-3 text-xs font-bold cursor-pointer transition">${i}</div>`);
                            for(let i=41; i<=48; i++) document.write(`<div onclick="seleccionarPieza(${i})" class="bg-slate-800 hover:bg-blue-600 border border-slate-700 rounded-xl p-3 text-xs font-bold cursor-pointer transition">${i}</div>`);
                            for(let i=31; i<=38; i++) document.write(`<div onclick="seleccionarPieza(${i})" class="bg-slate-800 hover:bg-blue-600 border border-slate-700 rounded-xl p-3 text-xs font-bold cursor-pointer transition">${i}</div>`);
                        </script>
                    </div>
                    <div class="mt-6 p-4 bg-slate-800/50 border border-slate-700/50 rounded-xl flex justify-between items-center text-xs">
                        <span id="piezaSeleccionadaInfo" class="text-slate-300">Ninguna pieza seleccionada. Haz clic en un número superior o inferior.</span>
                        <button onclick="alert('Recordatorio de control programado por WhatsApp')" class="bg-slate-700 hover:bg-slate-600 text-white px-3 py-1.5 rounded-lg transition cursor-pointer">Programar Recordatorio</button>
                    </div>
                </div>
            </div>

            <!-- PESTAÑA 4: INVENTARIO -->
            <div id="content-inventario" class="hidden space-y-6">
                <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-sm">
                    <h3 class="font-bold text-lg text-white mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-boxes-stacked text-purple-400"></i> Control de Stock e Insumos Compartidos
                    </h3>
                    <p class="text-xs text-slate-400 mb-4">El stock se descuenta automáticamente cada vez que registras un procedimiento en La Pampa o La Villa.</p>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs text-slate-300">
                            <thead class="bg-slate-800 text-slate-400 uppercase text-[10px]">
                                <tr>
                                    <th class="p-3 rounded-l-xl">Insumo / Material</th>
                                    <th class="p-3">Stock La Pampa</th>
                                    <th class="p-3">Stock La Villa</th>
                                    <th class="p-3 rounded-r-xl">Estado</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-800">
                                <tr>
                                    <td class="p-3 font-medium text-white">Anestesia Lidocaína (Caja)</td>
                                    <td class="p-3">12 cajas</td>
                                    <td class="p-3">8 cajas</td>
                                    <td class="p-3"><span class="bg-emerald-900/50 text-emerald-400 border border-emerald-700/50 px-2 py-0.5 rounded-full text-[10px]">Óptimo</span></td>
                                </tr>
                                <tr>
                                    <td class="p-3 font-medium text-white">Resina Filtek Z250</td>
                                    <td class="p-3">5 jeringas</td>
                                    <td class="p-3">3 jeringas</td>
                                    <td class="p-3"><span class="bg-emerald-900/50 text-emerald-400 border border-emerald-700/50 px-2 py-0.5 rounded-full text-[10px]">Óptimo</span></td>
                                </tr>
                                <tr>
                                    <td class="p-3 font-medium text-white">Guantes de Látex (Caja 100u)</td>
                                    <td class="p-3 text-amber-400">2 cajas (Bajo)</td>
                                    <td class="p-3">4 cajas</td>
                                    <td class="p-3"><span class="bg-amber-900/50 text-amber-400 border border-amber-700/50 px-2 py-0.5 rounded-full text-[10px]">Atención</span></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- PESTAÑA 5: GASTOS -->
            <div id="content-gastos" class="hidden space-y-6">
                <div class="bg-slate-900 border border-slate-800 rounded-2xl p-6 shadow-sm max-w-2xl mx-auto">
                    <h3 class="font-bold text-lg text-white mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-file-invoice-dollar text-rose-400"></i> Registro de Egresos y Gastos Operativos
                    </h3>
                    <form onsubmit="registrarGasto(event)" class="space-y-4">
                        <div>
                            <label class="block text-xs font-medium text-slate-300 mb-1">Concepto o Proveedor</label>
                            <input type="text" id="gastoConcepto" required placeholder="Ej. Compra de materiales de laboratorio" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white outline-none focus:border-blue-500">
                        </div>
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-xs font-medium text-slate-300 mb-1">Categoría</label>
                                <select id="gastoCategoria" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white outline-none focus:border-blue-500">
                                    <option value="Insumos Dentales">Insumos Dentales</option>
                                    <option value="Alquiler">Alquiler de Sucursal</option>
                                    <option value="Servicios Básicos">Servicios Básicos (Luz/Agua)</option>
                                    <option value="Sueldos">Sueldos y Personal</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-medium text-slate-300 mb-1">Monto (Bs.)</label>
                                <input type="number" id="gastoMonto" required placeholder="300" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-sm text-white outline-none focus:border-blue-500">
                            </div>
                        </div>
                        <button type="submit" class="w-full bg-rose-600 hover:bg-rose-500 text-white font-medium py-3 rounded-xl shadow-lg transition text-sm cursor-pointer">
                            Registrar Gasto
                        </button>
                    </form>
                </div>
            </div>

        </main>

        <!-- Footer -->
        <footer class="bg-slate-900 border-t border-slate-800 py-4 text-center text-xs text-slate-400 mt-auto">
            <p>RAC - Rocabado Auditores Consultores · Consultorio Dental Integrado</p>
        </footer>
    </div>

    <!-- Script Lógico -->
    <script>
        let db = {
            sucursal: 'pampa',
            ingresosEfectivo: 0,
            ingresosQR: 0,
            serviciosCount: 0,
            gastosTotal: 0
        };

        function loginAs(role) {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('appContainer').classList.remove('hidden');
        }

        function logout() {
            document.getElementById('appContainer').classList.add('hidden');
            document.getElementById('loginScreen').classList.remove('hidden');
        }

        function switchTab(tabId) {
            ['dashboard', 'pos', 'agenda', 'inventario', 'gastos'].forEach(t => {
                document.getElementById('content-' + t).classList.add('hidden');
                document.getElementById('tab-' + t).className = "px-4 py-2 rounded-xl text-xs font-semibold bg-slate-800 text-slate-300 hover:bg-slate-700 transition flex items-center gap-2 shrink-0 cursor-pointer";
            });
            document.getElementById('content-' + tabId).classList.remove('hidden');
            document.getElementById('tab-' + tabId).className = "px-4 py-2 rounded-xl text-xs font-semibold bg-blue-600 text-white transition flex items-center gap-2 shrink-0 cursor-pointer";
        }

        function switchBranch(branch) {
            db.sucursal = branch;
            let name = branch === 'pampa' ? 'Sucursal La Pampa' : branch === 'villa' ? 'Sucursal La Villa' : 'Consolidado General (Pampa + Villa)';
            document.getElementById('headerBranch').innerText = "Sucursal Activa: " + name;
        }

        function registrarVenta(e) {
            e.preventDefault();
            let monto = parseFloat(document.getElementById('posMonto').value);
            let metodo = document.getElementById('posMetodo').value;
            let paciente = document.getElementById('posPaciente').value;

            if(metodo === 'Efectivo') db.ingresosEfectivo += monto;
            else if(metodo === 'QR') db.ingresosQR += monto;
            db.serviciosCount += 1;

            actualizarDashboard();
            alert("¡Servicio registrado con éxito para " + paciente + "!");
            e.target.reset();
            switchTab('dashboard');
        }

        function registrarGasto(e) {
            e.preventDefault();
            let monto = parseFloat(document.getElementById('gastoMonto').value);
            db.gastosTotal += monto;
            actualizarDashboard();
            alert("¡Gasto registrado correctamente!");
            e.target.reset();
            switchTab('dashboard');
        }

        function actualizarDashboard() {
            document.getElementById('dashEfectivo').innerText = "Bs. " + db.ingresosEfectivo.toFixed(2);
            document.getElementById('dashQR').innerText = "Bs. " + db.ingresosQR.toFixed(2);
            document.getElementById('dashServicios').innerText = db.serviciosCount;
            document.getElementById('dashGastos').innerText = "Bs. " + db.gastosTotal.toFixed(2);
        }

        function seleccionarPieza(num) {
            document.getElementById('piezaSeleccionadaInfo').innerText = "Pieza Dental Seleccionada: #" + num + " (Lista para diagnóstico o tratamiento)";
        }

        function generarCierreWhatsApp() {
            let totalVentas = db.ingresosEfectivo + db.ingresosQR;
            let msg = `*CIERRE DE CAJA - CONSULTORIO DENTAL RAC*%0A` +
                      `📍 Sucursal: ${db.sucursal.toUpperCase()}%0A` +
                      `💵 Efectivo: Bs. ${db.ingresosEfectivo.toFixed(2)}%0A` +
                      `📱 QR / Transf: Bs. ${db.ingresosQR.toFixed(2)}%0A` +
                      `🦷 Servicios: ${db.serviciosCount}%0A` +
                      `📉 Gastos: Bs. ${db.gastosTotal.toFixed(2)}%0A` +
                      `💰 *Total Ingresos: Bs. ${totalVentas.toFixed(2)}*`;
            window.open(`https://wa.me/?text=${msg}`, '_blank');
        }
    </script>
</body>
</html>
