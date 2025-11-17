
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro Diario de Repartidor</title>
    <!-- Incluir Tailwind CSS para diseño moderno y responsivo -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Definición de la paleta Uber (aproximada) */
        :root {
            --color-uber-black: #1e1e1e;
            --color-uber-green: #00bf63; /* Verde Neón de Uber */
        }
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7f9fb; /* Fondo muy claro */
        }
        .app-container {
            max-width: 500px;
            margin: auto;
            padding: 1rem;
        }
        .currency-input {
            padding-left: 2rem !important;
        }
        
        /* Estilo para el botón de guardar */
        #save-button {
            background-color: var(--color-uber-green);
            color: var(--color-uber-black);
            font-weight: 700;
            transition: background-color 0.15s;
        }
        #save-button:hover:not(:disabled) {
            background-color: #00a855; 
        }
        #save-button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        .auth-button {
            background-color: var(--color-uber-black);
            color: white;
            transition: background-color 0.15s;
        }
        .auth-button:hover {
            background-color: #333;
        }
        .signup-button {
            background-color: #3b82f6; /* Blue for Sign Up */
        }
        .signup-button:hover {
            background-color: #2563eb;
        }
    </style>
</head>
<body>

    <div class="app-container">
        <header class="text-center py-6">
            <h1 class="text-3xl font-extrabold text-[var(--color-uber-black)]">💰 Control Diario de Entregas</h1>
            <p id="auth-status" class="text-xs text-gray-500 mt-1">Desconectado</p>
        </header>

        <!-- CONTENEDOR DE AUTENTICACIÓN (Visible cuando no hay usuario) -->
        <div id="auth-container" class="bg-white p-6 rounded-xl shadow-xl border border-gray-200">
            <h2 class="text-xl font-bold text-[var(--color-uber-black)] border-b pb-2 mb-4">Inicio de Sesión / Registro</h2>
            <div id="auth-message-box" class="mb-4 p-3 rounded-lg text-center font-semibold hidden"></div>
            
            <input type="email" id="auth-email" placeholder="Email" class="w-full p-3 border border-gray-300 rounded-lg mb-3" required>
            <input type="password" id="auth-password" placeholder="Contraseña (mínimo 6 caracteres)" class="w-full p-3 border border-gray-300 rounded-lg mb-4" required>
            
            <div class="grid grid-cols-2 gap-3">
                <button id="login-button" class="auth-button py-3 rounded-lg shadow-md transition duration-150">
                    Iniciar Sesión
                </button>
                <button id="signup-button" class="auth-button signup-button py-3 rounded-lg shadow-md transition duration-150">
                    Registrarse
                </button>
            </div>
        </div>

        <!-- CONTENIDO PRINCIPAL DE LA APP (Visible solo al iniciar sesión) -->
        <div id="app-content" class="hidden">
            <!-- Formulario de Entrada de Datos -->
            <div class="bg-white p-6 rounded-xl shadow-xl mb-8 border border-gray-200">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl font-bold text-[var(--color-uber-black)] border-b pb-2">Registro de Jornada</h2>
                    <button id="logout-button" class="text-sm text-red-500 hover:text-red-700 font-medium">Cerrar Sesión</button>
                </div>
                
                <form id="record-form">
                    
                    <!-- Sección KM y Fecha -->
                    <div class="grid grid-cols-2 gap-3">
                        <div class="col-span-2">
                            <label for="date" class="block text-sm font-medium text-gray-700">Fecha</label>
                            <input type="date" id="date" name="date" class="w-full p-2 border border-gray-300 rounded-lg" required>
                        </div>
                        <div>
                            <label for="kmInitial" class="block text-sm font-medium text-gray-700">Km Inicial</label>
                            <input type="number" id="kmInitial" name="kmInitial" class="w-full p-2 border border-gray-300 rounded-lg" required>
                        </div>
                        <div>
                            <label for="kmFinal" class="block text-sm font-medium text-gray-700">Km Final</label>
                            <input type="number" id="kmFinal" name="kmFinal" class="w-full p-2 border border-gray-300 rounded-lg" required>
                        </div>
                        <!-- KM Recorrido (Estilo Acento) -->
                        <div class="col-span-2 bg-gray-100 p-3 rounded-lg border-l-4 border-gray-900">
                            <span class="text-sm font-medium text-gray-700">Km Recorrido</span>
                            <p id="km-traveled-display" class="text-xl font-bold text-gray-900">0.00</p>
                        </div>
                    </div>

                    <!-- Sección Ingresos -->
                    <h3 class="text-lg font-semibold text-gray-800 mt-5 mb-3 border-t pt-4">Ganancias e Ingresos</h3>
                    <div class="grid grid-cols-2 gap-3">
                        <div class="relative">
                            <label for="totalEarnings" class="block text-sm font-medium text-gray-700">Ingreso Bruto Total (App)</label>
                            <span class="absolute left-3 top-8 text-gray-400 font-bold">$</span>
                            <input type="number" step="0.01" id="totalEarnings" name="totalEarnings" class="w-full p-2 border border-gray-300 rounded-lg currency-input" value="0" required>
                        </div>
                        <div class="relative">
                            <label for="cashReceived" class="block text-sm font-medium text-gray-700">Efectivo Cobrado</label>
                            <span class="absolute left-3 top-8 text-gray-400 font-bold">$</span>
                            <input type="number" step="0.01" id="cashReceived" name="cashReceived" class="w-full p-2 border border-gray-300 rounded-lg currency-input" value="0" required>
                        </div>
                        <!-- Neto a Depositar (Estilo Acento Verde) -->
                        <div class="col-span-2 bg-emerald-50 p-3 rounded-lg border-l-4 border-[var(--color-uber-green)]">
                            <span class="text-sm font-medium text-emerald-700">Neto a Depositar (App)</span>
                            <p id="net-to-deposit-display" class="text-xl font-bold text-emerald-900">$0.00</p>
                        </div>
                    </div>
                    
                    <!-- Sección Egresos -->
                    <h3 class="text-lg font-semibold text-gray-800 mt-5 mb-3 border-t pt-4">Egresos y Gastos Operativos</h3>
                    <div class="grid grid-cols-2 gap-3">
                        <div class="relative">
                            <label for="gasExpense" class="block text-sm font-medium text-gray-700">Gasolina</label>
                            <span class="absolute left-3 top-8 text-gray-400 font-bold">$</span>
                            <input type="number" step="0.01" id="gasExpense" name="gasExpense" class="w-full p-2 border border-gray-300 rounded-lg currency-input" value="0" required>
                        </div>
                        <div class="relative">
                            <label for="maintExpense" class="block text-sm font-medium text-gray-700">Mantenimiento/Aceite</label>
                            <span class="absolute left-3 top-8 text-gray-400 font-bold">$</span>
                            <input type="number" step="0.01" id="maintExpense" name="maintExpense" class="w-full p-2 border border-gray-300 rounded-lg currency-input" value="0" required>
                        </div>
                        <div class="relative">
                            <label for="foodExpense" class="block text-sm font-medium text-gray-700">Alimentación</label>
                            <span class="absolute left-3 top-8 text-gray-400 font-bold">$</span>
                            <input type="number" step="0.01" id="foodExpense" name="foodExpense" class="w-full p-2 border border-gray-300 rounded-lg currency-input" value="0" required>
                        </div>
                        <div class="relative">
                            <label for="otherExpense" class="block text-sm font-medium text-gray-700">Otros Gastos</label>
                            <span class="absolute left-3 top-8 text-gray-400 font-bold">$</span>
                            <input type="number" step="0.01" id="otherExpense" name="otherExpense" class="w-full p-2 border border-gray-300 rounded-lg currency-input" value="0">
                        </div>

                        <!-- Costo por Km (Estilo Advertencia, Rojo/Rosa) -->
                        <div class="col-span-2 bg-pink-50 p-3 rounded-lg border-l-4 border-pink-500">
                            <span class="text-sm font-medium text-pink-700">Costo Moto/Km (Análisis)</span>
                            <p id="cost-per-km-display" class="text-xl font-bold text-pink-800">$0.000</p>
                        </div>
                    </div>

                    <!-- Sección Salario y Utilidad -->
                    <h3 class="text-lg font-semibold text-gray-800 mt-5 mb-3 border-t pt-4">Utilidad y Salario</h3>
                    <div class="grid grid-cols-2 gap-3">
                        <div class="relative">
                            <label for="weeklySalaryGoal" class="block text-sm font-medium text-gray-700">Meta Salario SEMANAL</label>
                            <span class="absolute left-3 top-8 text-gray-400 font-bold">$</span>
                            <input type="number" step="0.01" id="weeklySalaryGoal" name="weeklySalaryGoal" class="w-full p-2 border border-gray-300 rounded-lg currency-input" value="100000" required>
                        </div>
                        <!-- Aporte Diario (Estilo Acento Verde Claro) -->
                        <div class="bg-emerald-100 p-3 rounded-lg border-l-4 border-emerald-500 flex flex-col justify-center">
                            <span class="text-sm font-medium text-emerald-700">Aporte Diario (Calculado)</span>
                            <p id="daily-salary-display" class="text-xl font-bold text-emerald-900">$0.00</p>
                        </div>
                        <!-- Ganancia Neta Diaria (Énfasis Principal - Verde Uber) -->
                        <div class="col-span-2 bg-[var(--color-uber-green)]/10 p-3 rounded-lg text-center border-2 border-[var(--color-uber-green)]">
                            <span class="text-lg font-medium text-[var(--color-uber-black)]">GANANCIA NETA DIARIA (Utilidad)</span>
                            <p id="net-daily-profit-display" class="text-3xl font-extrabold text-[var(--color-uber-black)]">$0.00</p>
                        </div>
                    </div>

                    <!-- Mensaje de estado -->
                    <div id="message-box" class="mt-4 p-3 rounded-lg text-center font-semibold hidden"></div>

                    <!-- Botón de Guardar (Estilo Uber Green) -->
                    <button type="submit" id="save-button" class="w-full mt-6 py-3 shadow-md transition duration-150 text-lg">
                        Guardar Registro Diario
                    </button>
                </form>
            </div>

            <!-- Historial de Registros -->
            <h2 class="text-2xl font-bold text-[var(--color-uber-black)] mt-8 mb-4 border-b pb-2">Historial de Registros</h2>
            <div id="records-list" class="space-y-3">
                <p class="text-gray-500 text-center py-4">Cargando registros...</p>
            </div>
        </div>


    </div> <!-- Fin app-container -->

    <!-- SCRIPTS DE FIREBASE Y LÓGICA DE LA APLICACIÓN -->
    <script type="module">
        // Importar módulos de Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { 
            getAuth, 
            createUserWithEmailAndPassword, 
            signInWithEmailAndPassword,
            signOut,
            onAuthStateChanged 
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // =========================================================================
        // === PASO CLAVE: PEGA AQUÍ TU CONFIGURACIÓN DE FIREBASE ===
        // Reemplaza los valores de '...' con tu configuración REAL de Firebase
         const firebaseConfig = {
  apiKey: "AIzaSyDi9N7bGww3hUIhUoKs3uq7vuvLvaCHWg4",
  authDomain: "micontrolrappiuber.firebaseapp.com",
  projectId: "micontrolrappiuber",
  storageBucket: "micontrolrappiuber.firebasestorage.app",
  messagingSenderId: "467432060578",
  appId: "1:467432060578:web:1c9e4f7304a82162c9c9fb"
};

        // ID de la aplicación. Debe coincidir con el usado en las Reglas de Seguridad.
        const appId = 'delivery-app-prod'; 
        // =========================================================================
        
        let app, db, auth, userId = null;

        // --- REFERENCIAS DEL DOM ---
        const form = document.getElementById('record-form');
        const recordsList = document.getElementById('records-list');
        const authStatus = document.getElementById('auth-status');
        const messageBox = document.getElementById('message-box'); // Para mensajes en la App principal
        const authMessageBox = document.getElementById('auth-message-box'); // Para mensajes de Auth
        const saveButton = document.getElementById('save-button');
        const appContent = document.getElementById('app-content');
        const authContainer = document.getElementById('auth-container');

        const authEmail = document.getElementById('auth-email');
        const authPassword = document.getElementById('auth-password');
        const loginButton = document.getElementById('login-button');
        const signupButton = document.getElementById('signup-button');
        const logoutButton = document.getElementById('logout-button');

        const inputFields = ['kmInitial', 'kmFinal', 'totalEarnings', 'cashReceived', 'gasExpense', 'maintExpense', 'foodExpense', 'otherExpense', 'weeklySalaryGoal'];

        // --- FUNCIONES DE UTILIDAD ---

        const showMessage = (box, message, type) => {
            box.textContent = message;
            box.classList.remove('hidden', 'bg-red-100', 'text-red-700', 'bg-green-100', 'text-green-700', 'bg-emerald-100', 'text-emerald-700');

            if (type === 'success') {
                box.classList.add('bg-emerald-100', 'text-emerald-700');
            } else {
                box.classList.add('bg-red-100', 'text-red-700');
            }

            setTimeout(() => {
                box.classList.add('hidden');
            }, 5000);
        };

        const toggleLoading = (button, isLoading, defaultText) => {
            button.disabled = isLoading;
            button.textContent = isLoading ? 'Cargando...' : defaultText;
        }

        // --- AUTENTICACIÓN ---

        const handleSignup = async () => {
            const email = authEmail.value;
            const password = authPassword.value;
            if (!email || password.length < 6) {
                showMessage(authMessageBox, 'El email y la contraseña (mínimo 6) son requeridos.', 'error');
                return;
            }
            toggleLoading(signupButton, true, 'Registrarse');
            try {
                await createUserWithEmailAndPassword(auth, email, password);
                showMessage(authMessageBox, '✅ Registro exitoso. Iniciando sesión...', 'success');
            } catch (error) {
                let msg = 'Error en el registro.';
                if (error.code === 'auth/email-already-in-use') {
                    msg = 'Ese email ya está registrado.';
                } else if (error.code === 'auth/weak-password') {
                    msg = 'La contraseña es muy débil (mínimo 6).';
                }
                showMessage(authMessageBox, `❌ ${msg} ${error.code}`, 'error');
            } finally {
                toggleLoading(signupButton, false, 'Registrarse');
            }
        };

        const handleLogin = async () => {
            const email = authEmail.value;
            const password = authPassword.value;
            if (!email || !password) {
                showMessage(authMessageBox, 'El email y la contraseña son requeridos.', 'error');
                return;
            }
            toggleLoading(loginButton, true, 'Iniciar Sesión');
            try {
                await signInWithEmailAndPassword(auth, email, password);
                showMessage(authMessageBox, '✅ Sesión iniciada.', 'success');
            } catch (error) {
                let msg = 'Credenciales incorrectas.';
                if (error.code === 'auth/user-not-found' || error.code === 'auth/wrong-password') {
                    msg = 'Email o contraseña inválidos.';
                }
                showMessage(authMessageBox, `❌ ${msg} ${error.code}`, 'error');
            } finally {
                toggleLoading(loginButton, false, 'Iniciar Sesión');
            }
        };

        const handleLogout = async () => {
            try {
                await signOut(auth);
                // El onAuthStateChanged se encargará de ocultar la app y mostrar el login
            } catch (error) {
                console.error("Error al cerrar sesión:", error);
                showMessage(messageBox, '❌ Error al cerrar sesión.', 'error');
            }
        };
        
        // --- FUNCIÓN DE INICIALIZACIÓN DE FIREBASE ---
        const setupFirebase = async () => {
            // Validación simple para verificar si el usuario pegó la clave
            if (firebaseConfig.apiKey.includes('PEGA_TU_API_KEY_AQUI')) {
                authStatus.textContent = '❌ Error: Configuración de Firebase incompleta.';
                console.error("Firebase no está configurado. Por favor, edita el archivo HTML y pega tu firebaseConfig.");
                return;
            }

            try {
                app = initializeApp(firebaseConfig);
                db = getFirestore(app);
                auth = getAuth(app);
            } catch (error) {
                authStatus.textContent = '❌ Error de inicialización de Firebase.';
                console.error("Error al inicializar Firebase:", error);
                return;
            }


            // 1. Manejo de Autenticación: Escucha cambios de estado de autenticación.
            onAuthStateChanged(auth, (user) => {
                if (user) {
                    // Usuario Autenticado (Email/Password)
                    userId = user.uid;
                    authStatus.textContent = `✅ Conectado como: ${user.email}`;
                    authContainer.classList.add('hidden');
                    appContent.classList.remove('hidden');
                    loadRecords(); // Cargar datos del nuevo usuario
                } else {
                    // No hay usuario autenticado
                    userId = null;
                    authStatus.textContent = 'Desconectado';
                    authContainer.classList.remove('hidden');
                    appContent.classList.add('hidden');
                    recordsList.innerHTML = '<p class="text-gray-500 text-center py-4">Inicia sesión para ver tus registros.</p>';
                }
            });
        };

        // --- CÁLCULOS Y LÓGICA DE LA APP (NO MODIFICADA) ---
        const calculateMetrics = () => {
            const getFloat = (id) => parseFloat(document.getElementById(id).value) || 0;

            const kmInitial = getFloat('kmInitial');
            const kmFinal = getFloat('kmFinal');
            const totalEarnings = getFloat('totalEarnings');
            const cashReceived = getFloat('cashReceived');
            const gasExpense = getFloat('gasExpense');
            const maintExpense = getFloat('maintExpense');
            const foodExpense = getFloat('foodExpense');
            const otherExpense = getFloat('otherExpense');
            const weeklySalaryGoal = getFloat('weeklySalaryGoal');
            
            // Cálculos Financieros
            const kmTraveled = Math.max(0, kmFinal - kmInitial); 
            const netToDeposit = totalEarnings - cashReceived; 
            const dailySalaryContribution = weeklySalaryGoal / 7; 
            const totalDailyExpense = gasExpense + maintExpense + foodExpense + otherExpense; 
            const motorcycleCost = gasExpense + maintExpense + otherExpense; 
            const costPerKm = kmTraveled > 0 ? (motorcycleCost / kmTraveled) : 0; 
            const netDailyProfit = totalEarnings - totalDailyExpense - dailySalaryContribution; 

            // Actualizar el DOM con los resultados
            document.getElementById('km-traveled-display').textContent = kmTraveled.toFixed(2);
            document.getElementById('net-to-deposit-display').textContent = `$${netToDeposit.toFixed(2)}`;
            document.getElementById('daily-salary-display').textContent = `$${dailySalaryContribution.toFixed(2)}`;
            document.getElementById('cost-per-km-display').textContent = `$${costPerKm.toFixed(3)}`;
            document.getElementById('net-daily-profit-display').textContent = `$${netDailyProfit.toFixed(2)}`;

            // Retornar métricas para el guardado
            return {
                kmTraveled,
                netToDeposit: parseFloat(netToDeposit.toFixed(2)),
                dailySalaryContribution: parseFloat(dailySalaryContribution.toFixed(2)),
                totalDailyExpense: parseFloat(totalDailyExpense.toFixed(2)),
                motorcycleCost: parseFloat(motorcycleCost.toFixed(2)),
                costPerKm: parseFloat(costPerKm.toFixed(3)),
                netDailyProfit: parseFloat(netDailyProfit.toFixed(2)),
            };
        };
        
        form.addEventListener('input', calculateMetrics);
        
        form.addEventListener('submit', async (e) => {
            e.preventDefault();
            if (!userId) {
                showMessage(messageBox, '❌ Error: Debes iniciar sesión para guardar.', 'error');
                return;
            }

            saveButton.disabled = true;
            saveButton.textContent = 'Guardando...';

            const metrics = calculateMetrics();
            
            const formData = inputFields.reduce((obj, field) => {
                obj[field] = parseFloat(document.getElementById(field).value) || 0;
                return obj;
            }, {});

            const recordData = {
                date: document.getElementById('date').value,
                ...formData,
                ...metrics,
                timestamp: serverTimestamp(),
                userEmail: auth.currentUser.email // Opcional: guardar email para referencia
            };

            try {
                // La clave de la separación de datos: la ruta incluye el userId
                const recordsCollectionRef = collection(db, `/artifacts/${appId}/users/${userId}/daily_records`);
                await addDoc(recordsCollectionRef, recordData);
                
                showMessage(messageBox, '✅ ¡Registro diario guardado con éxito!', 'success');
                
                const kmFinalValue = document.getElementById('kmFinal').value;
                form.reset();
                document.getElementById('date').valueAsDate = new Date(); 
                document.getElementById('kmInitial').value = kmFinalValue; 
                calculateMetrics(); 

            } catch (error) {
                console.error("Error al guardar el registro (¿Reglas de seguridad?):", error);
                showMessage(messageBox, '❌ Error al guardar. Revisa la consola y tus reglas de seguridad.', 'error');
            } finally {
                saveButton.disabled = false;
                saveButton.textContent = 'Guardar Registro Diario';
            }
        });

        // --- CARGA DE REGISTROS (NO MODIFICADA) ---
        const renderRecords = (records) => {
            if (records.length === 0) {
                recordsList.innerHTML = '<p class="text-gray-500 text-center py-4 bg-white rounded-xl shadow">No hay registros guardados aún. ¡Empieza a registrar!</p>';
                return;
            }

            records.sort((a, b) => new Date(b.date) - new Date(a.date));

            recordsList.innerHTML = records.map(record => {
                const date = new Date(record.date).toLocaleDateString('es-ES', { day: 'numeric', month: 'short', year: 'numeric' });
                const time = record.timestamp && record.timestamp.toDate ? record.timestamp.toDate().toLocaleTimeString('es-ES', { hour: '2-digit', minute: '2-digit' }) : 'N/A';
                
                const earnings = (record.totalEarnings || 0).toFixed(2);
                const netDeposit = (record.netToDeposit || 0).toFixed(2);
                const totalExpense = (record.totalDailyExpense || 0).toFixed(2);
                const km = (record.kmTraveled || 0).toFixed(2);
                const costPerKm = (record.costPerKm || 0).toFixed(3);
                const netProfit = (record.netDailyProfit || 0).toFixed(2);

                return `
                    <div class="bg-white p-4 rounded-xl shadow-md border border-gray-200 text-sm">
                        <div class="flex justify-between items-center mb-2 pb-2 border-b">
                            <h3 class="font-bold text-lg text-[var(--color-uber-black)]">${date} (${time})</h3>
                            ${record.userEmail ? `<span class="text-xs text-gray-500">${record.userEmail}</span>` : ''}
                        </div>
                        <div class="grid grid-cols-2 gap-2">
                            <p><span class="font-semibold text-gray-700">Total Ingreso:</span> $${earnings}</p>
                            <p><span class="font-semibold text-emerald-700">Neto a Depositar:</span> $${netDeposit}</p>
                            
                            <p class="col-span-2"><span class="font-semibold text-pink-600">Gastos Totales:</span> $${totalExpense}</p>
                            <p><span class="text-gray-600">Km Recorrido:</span> ${km} km</p>
                            <p><span class="text-gray-600">Costo/Km (Moto):</span> $${costPerKm}</p>
                            
                            <p class="col-span-2 text-xl mt-2 font-extrabold text-[var(--color-uber-green)] border-t pt-2">
                                Utilidad Neta: $${netProfit}
                            </p>
                        </div>
                    </div>
                `;
            }).join('');
        };

        const loadRecords = () => {
            if (!db || !userId) return;

            recordsList.innerHTML = '<p class="text-gray-500 text-center py-4">Conectando a la base de datos...</p>';

            const recordsCollectionRef = collection(db, `/artifacts/${appId}/users/${userId}/daily_records`);
            
            onSnapshot(recordsCollectionRef, (snapshot) => {
                const records = snapshot.docs.map(doc => ({
                    id: doc.id,
                    ...doc.data()
                }));
                renderRecords(records);
            }, (error) => {
                console.error("Error al obtener registros:", error);
                recordsList.innerHTML = '<p class="text-red-500 text-center py-4">Error al cargar datos. ¿Verificaste las reglas de seguridad?</p>';
            });
        };

        // --- INICIO Y EVENT LISTENERS ---
        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('date').valueAsDate = new Date();
            setupFirebase();
            calculateMetrics();
            
            // Eventos de Autenticación
            signupButton.addEventListener('click', handleSignup);
            loginButton.addEventListener('click', handleLogin);
            logoutButton.addEventListener('click', handleLogout);

            // Event listeners para recalcular métricas
            inputFields.forEach(id => {
                const element = document.getElementById(id);
                if (element) {
                    element.addEventListener('focus', calculateMetrics);
                    element.addEventListener('blur', calculateMetrics);
                }
            });
        });
        
    </script>
</body>
</html>
