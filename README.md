<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Registro Diario de Repartidor</title>
    
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black">
    <meta name="theme-color" content="#1e1e1e"> 
    <link rel="manifest" href="/manifest.json">
    
    <script src="https://cdn.tailwindcss.com"></script>
    
    <style>
        /* Estilos CSS */
        :root {
            --color-uber-black: #1e1e1e;
            --color-uber-green: #00bf63;
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f7f9fb; 
        }
        .app-container {
            max-width: 500px;
            margin: auto;
            padding: 1rem;
        }
        .currency-input {
            padding-left: 2rem !important;
        }
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
            background-color: #3b82f6; 
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
            <p id="auth-status" class="text-xs text-gray-500 mt-1">Cargando...</p>
        </header>

        <div id="auth-container" class="bg-white p-6 rounded-xl shadow-xl border border-gray-200">
            <h2 class="text-xl font-bold text-[var(--color-uber-black)] border-b pb-2 mb-4">Inicio de Sesión / Registro</h2>
            <div id="auth-message-box" class="mb-4 p-3 rounded-lg text-center font-semibold hidden"></div>
            
            <input type="email" id="auth-email" placeholder="Email" class="w-full p-3 border border-gray-300 rounded-lg mb-3" required>
            <input type="password" id="auth-password" placeholder="Contraseña (mínimo 6 caracteres)" class="w-full p-3 border border-gray-300 rounded-lg mb-4" required>
            
            <div class="grid grid-cols-3 gap-3">
                <button id="login-button" class="auth-button py-3 rounded-lg shadow-md transition duration-150">
                    Login
                </button>
                <button id="signup-button" class="auth-button signup-button py-3 rounded-lg shadow-md transition duration-150">
                    Registro
                </button>
                <button id="demo-button" class="auth-button bg-gray-500 hover:bg-gray-600 py-3 rounded-lg shadow-md transition duration-150">
                    Modo Demo
                </button>
            </div>
        </div>

        <div id="app-content" class="hidden">
            <div class="bg-white p-6 rounded-xl shadow-xl mb-8 border border-gray-200">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl font-bold text-[var(--color-uber-black)] border-b pb-2">Registro de Jornada</h2>
                    <button id="logout-button" class="text-sm text-red-500 hover:text-red-700 font-medium">Cerrar Sesión</button>
                </div>
                
                <form id="record-form">
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
                        <div class="col-span-2 bg-gray-100 p-3 rounded-lg border-l-4 border-gray-900">
                            <span class="text-sm font-medium text-gray-700">Km Recorrido</span>
                            <p id="km-traveled-display" class="text-xl font-bold text-gray-900">0.00</p>
                        </div>
                    </div>
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
                        <div class="col-span-2 bg-emerald-50 p-3 rounded-lg border-l-4 border-[var(--color-uber-green)]">
                            <span class="text-sm font-medium text-emerald-700">Neto a Depositar (App)</span>
                            <p id="net-to-deposit-display" class="text-xl font-bold text-emerald-900">$0.00</p>
                        </div>
                    </div>
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
                        <div class="col-span-2 bg-pink-50 p-3 rounded-lg border-l-4 border-pink-500">
                            <span class="text-sm font-medium text-pink-700">Costo Moto/Km (Análisis)</span>
                            <p id="cost-per-km-display" class="text-xl font-bold text-pink-800">$0.000</p>
                        </div>
                    </div>
                    <h3 class="text-lg font-semibold text-gray-800 mt-5 mb-3 border-t pt-4">Utilidad y Salario</h3>
                    <div class="grid grid-cols-2 gap-3">
                        <div class="relative">
                            <label for="weeklySalaryGoal" class="block text-sm font-medium text-gray-700">Meta Salario SEMANAL</label>
                            <span class="absolute left-3 top-8 text-gray-400 font-bold">$</span>
                            <input type="number" step="0.01" id="weeklySalaryGoal" name="weeklySalaryGoal" class="w-full p-2 border border-gray-300 rounded-lg currency-input" value="200" required>
                        </div>
                        <div class="bg-emerald-100 p-3 rounded-lg border-l-4 border-emerald-500 flex flex-col justify-center">
                            <span class="text-sm font-medium text-emerald-700">Aporte Diario (Calculado)</span>
                            <p id="daily-salary-display" class="text-xl font-bold text-emerald-900">$0.00</p>
                        </div>
                        <div class="col-span-2 bg-[var(--color-uber-green)]/10 p-3 rounded-lg text-center border-2 border-[var(--color-uber-green)]">
                            <span class="text-lg font-medium text-[var(--color-uber-black)]">GANANCIA NETA DIARIA (Utilidad)</span>
                            <p id="net-daily-profit-display" class="text-3xl font-extrabold text-[var(--color-uber-black)]">$0.00</p>
                        </div>
                    </div>
                    <div id="message-box" class="mt-4 p-3 rounded-lg text-center font-semibold hidden"></div>
                    <button type="submit" id="save-button" class="w-full mt-6 py-3 shadow-md transition duration-150 text-lg">
                        Guardar Registro Diario
                    </button>
                </form>
            </div>

            <h2 class="text-2xl font-bold text-[var(--color-uber-black)] mt-8 mb-4 border-b pb-2">Historial de Registros</h2>
            <div id="records-list" class="space-y-3">
                <p class="text-gray-500 text-center py-4">Cargando registros...</p>
            </div>
        </div>


    </div> <script type="module">
        // Importar módulos de Firebase
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { 
            getAuth, 
            createUserWithEmailAndPassword, 
            signInWithEmailAndPassword,     
            signOut,                        
            onAuthStateChanged,
            signInAnonymously              // <--- Importación para Modo Demo
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { 
            getFirestore, 
            collection, 
            addDoc, 
            onSnapshot, 
            serverTimestamp, 
            deleteDoc, 
            doc                             // <--- Importación para Eliminar Registro
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // =========================================================================
        // === PASO CLAVE: PEGA AQUÍ TU CONFIGURACIÓN DE FIREBASE ===
      const firebaseConfig = {
  apiKey: "AIzaSyDi9N7bGww3hUIhUoKs3uq7vuvLvaCHWg4",
  authDomain: "micontrolrappiuber.firebaseapp.com",
  projectId: "micontrolrappiuber",
  storageBucket: "micontrolrappiuber.firebasestorage.app",
  messagingSenderId: "467432060578",
  appId: "1:467432060578:web:1c9e4f7304a82162c9c9fb"
};

        const appId = 'delivery-app-prod'; 
        // =========================================================================
        
        let app, db, auth, userId = null;

        // --- REFERENCIAS DEL DOM ---
        const form = document.getElementById('record-form');
        const recordsList = document.getElementById('records-list');
        const authStatus = document.getElementById('auth-status');
        const messageBox = document.getElementById('message-box'); 
        const authMessageBox = document.getElementById('auth-message-box'); 
        const saveButton = document.getElementById('save-button');
        const appContent = document.getElementById('app-content'); 
        const authContainer = document.getElementById('auth-container'); 

        const authEmail = document.getElementById('auth-email'); 
        const authPassword = document.getElementById('auth-password'); 
        const loginButton = document.getElementById('login-button'); 
        const signupButton = document.getElementById('signup-button'); 
        const logoutButton = document.getElementById('logout-button'); 
        const demoButton = document.getElementById('demo-button'); // <--- Nuevo botón

        const inputFields = ['kmInitial', 'kmFinal', 'totalEarnings', 'cashReceived', 'gasExpense', 'maintExpense', 'foodExpense', 'otherExpense', 'weeklySalaryGoal'];

        // --- Funciones de utilidad ---
        const showMessage = (box, message, type) => {
            box.textContent = message;
            box.classList.remove('hidden', 'bg-red-100', 'text-red-700', 'bg-green-100', 'text-green-700', 'bg-emerald-100', 'text-emerald-700');

            if (type === 'success') {
                box.classList.add('bg-emerald-100', 'text-emerald-700');
            } else {
                box.classList.add('bg-red-100', 'text-red-700');
            }

            box.classList.remove('hidden');

            setTimeout(() => {
                box.classList.add('hidden');
            }, 5000);
        };
        const toggleLoading = (button, isLoading, defaultText) => {
            button.disabled = isLoading;
            button.textContent = isLoading ? 'Cargando...' : defaultText;
        }

        // --- Lógica de Autenticación ---
        const handleSignup = async () => {
             toggleLoading(signupButton, true, 'Registrarse');
             try {
                await createUserWithEmailAndPassword(auth, authEmail.value, authPassword.value);
                showMessage(authMessageBox, 'Registro exitoso. Iniciando sesión...', 'success');
             } catch (error) {
                showMessage(authMessageBox, `Error al registrar: ${error.message}`, 'error');
             }
             toggleLoading(signupButton, false, 'Registrarse');
        };
        const handleLogin = async () => {
             toggleLoading(loginButton, true, 'Iniciar Sesión');
             try {
                await signInWithEmailAndPassword(auth, authEmail.value, authPassword.value);
                showMessage(authMessageBox, 'Sesión iniciada con éxito.', 'success');
             } catch (error) {
                showMessage(authMessageBox, `Error al iniciar sesión: ${error.message}`, 'error');
             }
             toggleLoading(loginButton, false, 'Iniciar Sesión');
        };
        const handleLogout = async () => { 
             try {
                await signOut(auth);
                showMessage(authStatus, 'Sesión cerrada.', 'success');
             } catch (error) {
                console.error("Error al cerrar sesión: ", error);
             }
        };

        // --- NUEVA FUNCIÓN: INICIO DE SESIÓN ANÓNIMO (MODO DEMO) ---
        const handleDemoLogin = async () => {
            toggleLoading(demoButton, true, 'Modo Demo');
            try {
                await signInAnonymously(auth);
                showMessage(authMessageBox, 'Demo iniciada. ¡Bienvenido!', 'success');
            } catch (error) {
                showMessage(authMessageBox, `Error al iniciar Demo: ${error.message}`, 'error');
            }
            toggleLoading(demoButton, false, 'Modo Demo');
        };

        
        // --- FUNCIÓN DE INICIALIZACIÓN DE FIREBASE Y GESTOR DE ESTADO ---
        const setupFirebase = () => { 
            app = initializeApp(firebaseConfig, appId);
            auth = getAuth(app);
            db = getFirestore(app);

            onAuthStateChanged(auth, (user) => {
                if (user) {
                    // Usuario logueado
                    userId = user.uid;
                    const authTypeText = user.isAnonymous ? ' (Modo Demo)' : '';
                    authStatus.textContent = `Sesión iniciada: ${user.email || 'Anónimo'}${authTypeText}`;
                    authContainer.classList.add('hidden');
                    appContent.classList.remove('hidden');
                    loadRecords(); // Cargar los registros del usuario
                } else {
                    // Usuario deslogueado
                    userId = null;
                    authStatus.textContent = 'Cerrada. Inicia sesión.';
                    authContainer.classList.remove('hidden');
                    appContent.classList.add('hidden');
                    recordsList.innerHTML = '<p class="text-gray-500 text-center py-4">Inicia sesión para ver tus registros.</p>';
                }
            });
        };
        
        // --- FUNCIÓN DE CÁLCULO DE MÉTRICAS ---
        const calculateMetrics = () => { 
            const kmInitial = parseFloat(document.getElementById('kmInitial').value) || 0;
            const kmFinal = parseFloat(document.getElementById('kmFinal').value) || 0;
            const totalEarnings = parseFloat(document.getElementById('totalEarnings').value) || 0;
            const cashReceived = parseFloat(document.getElementById('cashReceived').value) || 0;
            const gasExpense = parseFloat(document.getElementById('gasExpense').value) || 0;
            const maintExpense = parseFloat(document.getElementById('maintExpense').value) || 0;
            const foodExpense = parseFloat(document.getElementById('foodExpense').value) || 0;
            const otherExpense = parseFloat(document.getElementById('otherExpense').value) || 0;
            const weeklySalaryGoal = parseFloat(document.getElementById('weeklySalaryGoal').value) || 0;

            const kmTraveled = kmFinal - kmInitial;
            const netToDeposit = totalEarnings - cashReceived;
            const totalExpenses = gasExpense + maintExpense + foodExpense + otherExpense;
            
            const costPerKm = kmTraveled > 0 ? totalExpenses / kmTraveled : 0;
            const dailySalary = weeklySalaryGoal / 7;
            
            const netDailyProfit = totalEarnings - totalExpenses; 

            document.getElementById('km-traveled-display').textContent = kmTraveled.toFixed(2);
            document.getElementById('net-to-deposit-display').textContent = `$${netToDeposit.toFixed(2)}`;
            document.getElementById('cost-per-km-display').textContent = `$${costPerKm.toFixed(3)}`;
            document.getElementById('daily-salary-display').textContent = `$${dailySalary.toFixed(2)}`;
            document.getElementById('net-daily-profit-display').textContent = `$${netDailyProfit.toFixed(2)}`;
        };

        // --- MANEJO DE EVENTOS (submit) ---
        form.addEventListener('input', calculateMetrics);
        form.addEventListener('submit', async (e) => { 
            e.preventDefault();
            if (!userId) {
                showMessage(messageBox, 'Debes iniciar sesión para guardar un registro.', 'error');
                return;
            }
            toggleLoading(saveButton, true, 'Guardar Registro Diario');

            const recordData = {
                date: document.getElementById('date').value, 
                kmInitial: parseFloat(document.getElementById('kmInitial').value) || 0,
                kmFinal: parseFloat(document.getElementById('kmFinal').value) || 0,
                totalEarnings: parseFloat(document.getElementById('totalEarnings').value) || 0,
                cashReceived: parseFloat(document.getElementById('cashReceived').value) || 0,
                gasExpense: parseFloat(document.getElementById('gasExpense').value) || 0,
                maintExpense: parseFloat(document.getElementById('maintExpense').value) || 0,
                foodExpense: parseFloat(document.getElementById('foodExpense').value) || 0,
                otherExpense: parseFloat(document.getElementById('otherExpense').value) || 0,
                weeklySalaryGoal: parseFloat(document.getElementById('weeklySalaryGoal').value) || 0,
                createdAt: serverTimestamp() 
            };
            
            try {
                await addDoc(collection(db, `users/${userId}/records`), recordData);
                showMessage(messageBox, '¡Registro guardado con éxito!', 'success');
            } catch (error) {
                console.error("Error al guardar el documento: ", error);
                showMessage(messageBox, `Error al guardar: ${error.message}`, 'error');
            } finally {
                toggleLoading(saveButton, false, 'Guardar Registro Diario');
            }
        });

        // --- FUNCIÓN: ELIMINAR REGISTRO ---
        window.eliminarRegistro = async (registroID) => {
            if (!userId) {
                alert("Error: Usuario no autenticado.");
                return;
            }

            const confirmar = confirm("¿Estás seguro de que quieres eliminar este registro de forma permanente?");
            
            if (confirmar) {
                try {
                    const documentoRef = doc(db, `users/${userId}/records`, registroID);
                    await deleteDoc(documentoRef);
                    showMessage(messageBox, `Registro (${registroID.substring(0, 5)}...) eliminado con éxito.`, 'success');
                } catch (error) {
                    console.error("Error al intentar eliminar el registro: ", error);
                    showMessage(messageBox, "Hubo un error al eliminar el registro.", 'error');
                }
            }
        }

        // --- FUNCIÓN RENDERRECORDS MODIFICADA PARA INCLUIR EL BOTÓN ---
        const renderRecords = (records) => {
            recordsList.innerHTML = '';
            
            if (records.length === 0) {
                recordsList.innerHTML = '<p class="text-gray-500 text-center py-4">Aún no hay registros guardados.</p>';
                return;
            }

            records.forEach(record => {
                const dateObj = record.date instanceof Date ? record.date : (record.date?.toDate ? record.date.toDate() : new Date(record.date));
                const formattedDate = dateObj.toLocaleDateString('es-ES', { year: 'numeric', month: 'long', day: 'numeric' });
                
                const totalExpenses = record.gasExpense + record.maintExpense + record.foodExpense + record.otherExpense;
                const netDailyProfit = record.totalEarnings - totalExpenses;
                const profitColor = netDailyProfit >= 0 ? 'text-emerald-600' : 'text-red-600';

                const recordItem = document.createElement('div');
                recordItem.className = 'bg-white p-4 rounded-lg shadow-md border border-gray-100 flex justify-between items-start';
                
                recordItem.innerHTML = `
                    <div class="flex-grow">
                        <div class="flex justify-between items-start mb-2">
                            <h3 class="text-lg font-bold text-gray-900">${formattedDate}</h3>
                            
                            <button 
                                onclick="eliminarRegistro('${record.id}')" 
                                class="text-sm text-red-500 hover:text-red-700 bg-red-50 hover:bg-red-100 rounded-full px-3 py-1 font-medium transition duration-150"
                                title="Eliminar registro permanentemente"
                            >
                                🗑️ Eliminar
                            </button>
                            </div>
                        <div class="text-sm text-gray-600 space-y-1">
                            <p><strong>Km Recorridos:</strong> ${(record.kmFinal - record.kmInitial).toFixed(2)} km</p>
                            <p><strong>Ingreso Bruto:</strong> $${record.totalEarnings.toFixed(2)}</p>
                            <p><strong>Total Gastos:</strong> $${totalExpenses.toFixed(2)}</p>
                        </div>
                        <p class="mt-2 text-xl font-extrabold ${profitColor}">
                            Utilidad Neta: $${netDailyProfit.toFixed(2)}
                        </p>
                    </div>
                `;

                recordsList.appendChild(recordItem);
            });
        };

        // --- FUNCIÓN PARA CARGAR REGISTROS ---
        const loadRecords = () => { 
            if (!userId) return;

            const q = collection(db, `users/${userId}/records`);
            onSnapshot(q, (snapshot) => {
                const records = [];
                snapshot.forEach(doc => {
                    records.push({ id: doc.id, ...doc.data() }); 
                });
                records.sort((a, b) => b.createdAt.toDate() - a.createdAt.toDate());
                renderRecords(records);
            }, (error) => {
                console.error("Error al cargar registros: ", error);
                recordsList.innerHTML = '<p class="text-red-500 text-center py-4">Error al cargar registros.</p>';
            });
        };

        // [PWA] - Lógica del Service Worker
        const registerServiceWorker = () => {
            const swCode = `
                const CACHE_NAME = 'delivery-app-cache-v1';
                const urlsToCache = [
                    '/', 
                ];
                self.addEventListener('install', (event) => {
                    event.waitUntil(
                        caches.open(CACHE_NAME)
                            .then((cache) => {
                                console.log('Service Worker instalado. Archivos en caché.');
                                return cache.addAll(urlsToCache);
                            })
                    );
                });
                self.addEventListener('fetch', (event) => {
                    if (event.request.method !== 'GET') return;
                    event.respondWith(
                        caches.match(event.request)
                            .then((response) => {
                                if (response) {
                                    return response;
                                }
                                return fetch(event.request);
                            })
                    );
                });
                self.addEventListener('activate', (event) => {
                    const cacheWhitelist = [CACHE_NAME];
                    event.waitUntil(
                        caches.keys().then((cacheNames) => {
                            return Promise.all(
                                cacheNames.map((cacheName) => {
                                    if (cacheWhitelist.indexOf(cacheName) === -1) {
                                        return caches.delete(cacheName);
                                    }
                                })
                            );
                        })
                    );
                });
            `;

            if ('serviceWorker' in navigator) {
                const blob = new Blob([swCode], { type: 'application/javascript' });
                const swUrl = URL.createObjectURL(blob);

                navigator.serviceWorker.register(swUrl, { scope: '/' })
                    .then((registration) => {
                        console.log('Service Worker registrado con éxito:', registration);
                    })
                    .catch((error) => {
                        console.error('Fallo el registro del Service Worker:', error);
                    });
            }
        };

        document.addEventListener('DOMContentLoaded', () => {
            document.getElementById('date').valueAsDate = new Date();
            setupFirebase();
            calculateMetrics();

            // Eventos de Autenticación
            signupButton.addEventListener('click', handleSignup);
            loginButton.addEventListener('click', handleLogin);
            logoutButton.addEventListener('click', handleLogout);
            demoButton.addEventListener('click', handleDemoLogin); // <--- Evento Modo Demo
            
            // Event listeners para recalcular métricas
            inputFields.forEach(id => {
                const element = document.getElementById(id);
                if (element) {
                    element.addEventListener('focus', calculateMetrics);
                    element.addEventListener('blur', calculateMetrics);
                }
            });

            // [PWA] - Registrar el Service Worker al cargar la página
            registerServiceWorker();
        });
        
    </script>
</body>
</html>
