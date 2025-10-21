<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ScanManager Pro - Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        /* Estilos para el login */
        .login-container {
            background: linear-gradient(135deg, #111827 0%, #1f2937 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .login-card {
            background-color: #1f2937;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
            padding: 2rem;
            width: 100%;
            max-width: 400px;
            border: 1px solid #374151;
        }
        
        .login-title {
            font-family: 'Arial', sans-serif;
            font-weight: 1000;
            color: white;
            text-shadow: 
                0 1px 0 #4a5568,
                0 2px 0 #4a5568,
                0 3px 0 #4a5568,
                0 4px 0 #4a5568,
                0 5px 0 #4a5568,
                0 6px 1px rgba(0,0,0,.1),
                0 0 5px rgba(0,0,0,.1),
                0 1px 3px rgba(0,0,0,.3),
                0 3px 5px rgba(0,0,0,.2),
                0 5px 10px rgba(0,0,0,.25),
                0 10px 10px rgba(0,0,0,.2),
                0 20px 20px rgba(0,0,0,.15);
            letter-spacing: 1px;
            transform: perspective(500px) rotateX(10deg);
            text-align: center;
            margin-bottom: 1.5rem;
        }
        
        .login-input {
            background-color: #374151;
            color: white;
            border: 1px solid #4b5563;
            border-radius: 6px;
            padding: 0.75rem;
            width: 100%;
            margin-bottom: 1rem;
            transition: all 0.3s;
        }
        
        .login-input:focus {
            outline: none;
            border-color: #6b7280;
            box-shadow: 0 0 0 3px rgba(107, 114, 128, 0.3);
        }
        
        .login-button {
            background-color: #111827;
            color: white;
            border: none;
            border-radius: 6px;
            padding: 0.75rem;
            width: 100%;
            font-weight: 600;
            cursor: pointer;
            transition: background-color 0.3s;
            margin-top: 0.5rem;
        }
        
        .login-button:hover {
            background-color: #1f2937;
        }
        
        .login-error {
            color: #ef4444;
            font-size: 0.875rem;
            margin-top: 0.5rem;
            text-align: center;
            display: none;
        }
        
        /* Estilos CSS personalizados para la aplicación principal */
        .modal {
            transition: opacity 0.3s ease-in-out;
        }

        .modal-content {
            transition: transform 0.3s ease-in-out;
            max-height: 80vh;
            overflow-y: auto;
        }

        .modal.hidden {
            opacity: 0;
            pointer-events: none;
        }

        .modal.hidden .modal-content {
            transform: translateY(-20px);
        }

        .size-row {
            border: 1px solid #4b5563;
            border-radius: 8px;
            margin-bottom: 12px;
        }

        .size-header {
            background-color: #374151;
            padding: 12px;
            border-bottom: 1px solid #4b5563;
            font-weight: 600;
            color: #f9fafb;
        }

        .size-inputs {
            padding: 16px;
            display: flex;
            gap: 16px;
            flex-wrap: wrap;
        }

        .box-type-section {
            border: 1px solid #4b5563;
            border-radius: 6px;
            padding: 12px;
            flex: 1;
            min-width: 250px;
            background-color: #4b5563;
        }

        .box-type-header {
            font-weight: 500;
            color: #d1d5db;
            margin-bottom: 8px;
            text-align: center;
        }

        .input-group {
            display: flex;
            gap: 8px;
            align-items: center;
        }

        .input-group label {
            font-size: 12px;
            color: #9ca3af;
            min-width: 80px;
        }

        .input-group input {
            flex: 1;
            padding: 6px 8px;
            border: 1px solid #6b7280;
            border-radius: 4px;
            font-size: 14px;
            background-color: #374151;
            color: white;
        }

        .input-group input:focus {
            outline: none;
            border-color: #6b7280;
            box-shadow: 0 0 0 2px rgba(107, 114, 128, 0.2);
        }

        .scanner-input:focus {
            box-shadow: 0 0 0 3px rgba(107, 114, 128, 0.5);
            border-color: #6b7280;
        }

        .disabled-checkbox {
            cursor: not-allowed;
            opacity: 0.5;
        }

        /* Estilos 3D para el título */
        .title-3d {
            font-family: 'Arial white', Arial, sans-serif;
            font-weight: 1000;
            color: white;
            text-shadow: 
                0 1px 0 #4a5568,
                0 2px 0 #4a5568,
                0 3px 0 #4a5568,
                0 4px 0 #4a5568,
                0 5px 0 #4a5568,
                0 6px 1px rgba(0,0,0,.1),
                0 0 5px rgba(0,0,0,.1),
                0 1px 3px rgba(0,0,0,.3),
                0 3px 5px rgba(0,0,0,.2),
                0 5px 10px rgba(0,0,0,.25),
                0 10px 10px rgba(0,0,0,.2),
                0 20px 20px rgba(0,0,0,.15);
            letter-spacing: 1px;
            transform: perspective(500px) rotateX(10deg);
        }

        /* Colores negro y gris corporativo */
        .corporate-bg {
            background-color: #111827;
        }

        .corporate-card {
            background-color: #1f2937;
            color: white;
        }

        .corporate-header {
            background-color: #111827;
        }

        .corporate-button {
            background-color: #374151;
            color: white;
        }

        .corporate-button:hover {
            background-color: #4b5563;
        }

        .corporate-primary {
            background-color: #111827;
            color: white;
        }

        .corporate-primary:hover {
            background-color: #1f2937;
        }

        .corporate-secondary {
            background-color: #374151;
            color: white;
        }

        .corporate-secondary:hover {
            background-color: #4b5563;
        }

        /* Efectos de sombra y transición */
        .card-shadow {
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.3), 0 2px 4px -1px rgba(0, 0, 0, 0.2);
        }

        .hover-lift {
            transition: transform 0.2s ease, box-shadow 0.2s ease;
        }

        .hover-lift:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.3), 0 4px 6px -2px rgba(0, 0, 0, 0.2);
        }

        /* Gradientes sutiles */
        .gradient-bg {
            background: linear-gradient(135deg, #111827 0%, #1f2937 100%);
        }

        .gradient-header {
            background: linear-gradient(135deg, #111827 0%, #374151 100%);
        }

        /* Animaciones */
        @keyframes pulse-glow {
            0% {
                box-shadow: 0 0 5px rgba(107, 114, 128, 0.5);
            }
            50% {
                box-shadow: 0 0 20px rgba(107, 114, 128, 0.8);
            }
            100% {
                box-shadow: 0 0 5px rgba(107, 114, 128, 0.5);
            }
        }

        .pulse-glow {
            animation: pulse-glow 2s infinite;
        }

        /* Efecto de brillo en botones */
        .glow-button {
            position: relative;
            overflow: hidden;
        }

        .glow-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
            transition: left 0.5s;
        }

        .glow-button:hover::before {
            left: 100%;
        }

        /* Spinner de carga */
        .spinner {
            border: 4px solid rgba(0, 0, 0, 0.1);
            border-left-color: #6b7280;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            to {
                transform: rotate(360deg);
            }
        }

        /* Mensajes de error */
        .error-message {
            color: #ef4444;
            font-size: 0.875rem;
            margin-top: 0.25rem;
        }

        .input-error {
            border-color: #ef4444;
            box-shadow: 0 0 0 2px rgba(239, 68, 68, 0.2);
        }

        /* Nuevos estilos para el sistema de escaneo */
        .duplicate-box {
            background-color: #7f1d1d !important;
            border-left: 4px solid #ef4444;
        }

        .scanned-box {
            background-color: #14532d !important;
            border-left: 4px solid #22c55e;
        }

        .box-table-container {
            max-height: 400px;
            overflow-y: auto;
        }

        .box-table {
            width: 100%;
            border-collapse: collapse;
        }

        .box-table th, .box-table td {
            padding: 8px 12px;
            text-align: left;
            border-bottom: 1px solid #4b5563;
            color: white;
        }

        .box-table th {
            background-color: #374151;
            font-weight: 600;
            position: sticky;
            top: 0;
        }

        .box-table tr:hover {
            background-color: #374151;
        }

        .scan-counter {
            font-size: 1.5rem;
            font-weight: bold;
            text-align: center;
            margin: 10px 0;
            color: white;
        }

        .scan-feedback {
            padding: 8px;
            border-radius: 4px;
            margin: 8px 0;
            text-align: center;
            font-weight: 500;
        }

        .scan-success {
            background-color: #14532d;
            color: #22c55e;
        }

        .scan-error {
            background-color: #7f1d1d;
            color: #ef4444;
        }

        .scan-warning {
            background-color: #78350f;
            color: #f59e0b;
        }

        /* Estilos para inputs y selects */
        input, select, textarea {
            background-color: #374151;
            color: white;
            border: 1px solid #4b5563;
        }

        input:focus, select:focus, textarea:focus {
            outline: none;
            border-color: #6b7280;
            box-shadow: 0 0 0 2px rgba(107, 114, 128, 0.2);
        }

        /* Placeholder color */
        ::placeholder {
            color: #9ca3af;
        }

        /* Scrollbar personalizado */
        ::-webkit-scrollbar {
            width: 8px;
        }

        ::-webkit-scrollbar-track {
            background: #374151;
        }

        ::-webkit-scrollbar-thumb {
            background: #6b7280;
            border-radius: 4px;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: #9ca3af;
        }
        
        /* Estilo para campos opcionales */
        .optional-field {
            border-left: 3px solid #6b7280;
        }
        
        .optional-field-label {
            color: #9ca3af;
            font-style: italic;
        }
        
        /* Ocultar la aplicación principal inicialmente */
        #app-container {
            display: none;
        }

        /* Estilos para el Dashboard */
        .dashboard-card {
            background-color: #1f2937;
            border-radius: 12px;
            padding: 1.5rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
            border: 1px solid #374151;
        }

        .dashboard-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.4);
        }

        .metric-card {
            text-align: center;
            padding: 1.5rem;
        }

        .metric-value {
            font-size: 2.5rem;
            font-weight: 800;
            margin-bottom: 0.5rem;
        }

        .metric-label {
            font-size: 0.875rem;
            color: #9ca3af;
            text-transform: uppercase;
            letter-spacing: 0.05em;
        }

        .progress-bar {
            height: 8px;
            background-color: #374151;
            border-radius: 4px;
            overflow: hidden;
            margin-top: 0.5rem;
        }

        .progress-fill {
            height: 100%;
            border-radius: 4px;
            transition: width 0.5s ease;
        }

        .chart-container {
            position: relative;
            height: 300px;
            width: 100%;
        }

        .status-badge {
            display: inline-block;
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
        }

        .status-completed {
            background-color: #14532d;
            color: #22c55e;
        }

        .status-in-progress {
            background-color: #78350f;
            color: #f59e0b;
        }

        .status-not-started {
            background-color: #374151;
            color: #9ca3af;
        }

        .recent-orders-table {
            width: 100%;
            border-collapse: collapse;
        }

        .recent-orders-table th,
        .recent-orders-table td {
            padding: 0.75rem;
            text-align: left;
            border-bottom: 1px solid #374151;
        }

        .recent-orders-table th {
            background-color: #374151;
            font-weight: 600;
            color: #d1d5db;
        }

        .recent-orders-table tr:hover {
            background-color: #374151;
        }

        .dashboard-section-title {
            font-size: 1.25rem;
            font-weight: 600;
            margin-bottom: 1rem;
            color: white;
            border-bottom: 1px solid #374151;
            padding-bottom: 0.5rem;
        }

        .tab-button {
            padding: 0.75rem 1.5rem;
            font-weight: 500;
            border-bottom: 2px solid transparent;
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .tab-button.active {
            color: white;
            border-bottom-color: #3b82f6;
        }

        .tab-button:not(.active) {
            color: #9ca3af;
        }

        .tab-button:hover:not(.active) {
            color: #d1d5db;
        }
    </style>
</head>
<body class="corporate-bg">
    <!-- Pantalla de Login -->
    <div id="login-container" class="login-container">
        <div class="login-card card-shadow">
            <h1 class="login-title">ScanManager Pro</h1>
            <form id="login-form">
                <div class="mb-4">
                    <input type="text" id="username" class="login-input" placeholder="Usuario" required>
                </div>
                <div class="mb-4">
                    <input type="password" id="password" class="login-input" placeholder="Contraseña" required>
                </div>
                <button type="submit" class="login-button glow-button">Iniciar Sesión</button>
                <div id="login-error" class="login-error">Usuario o contraseña incorrectos</div>
            </form>
        </div>
    </div>

    <!-- Aplicación Principal (oculta inicialmente) -->
    <div id="app-container">
        <header class="gradient-header text-white p-4 shadow-lg">
            <div class="container mx-auto flex justify-between items-center">
                <h1 class="text-2xl title-3d">ScanManager Pro</h1>
                <div>
                    <button onclick="app.showNewOrderModal()" class="corporate-primary px-4 py-2 rounded-lg font-semibold transition-colors mr-2 glow-button">
                        + Nueva Orden
                    </button>
                    <button onclick="app.showMaintenanceModal()" class="corporate-secondary px-4 py-2 rounded-lg font-semibold transition-colors mr-2 glow-button">
                        Mantenimiento
                    </button>
                    <button onclick="app.showSkuModal()" class="corporate-secondary px-4 py-2 rounded-lg font-semibold transition-colors glow-button">
                        Gestión de SKUs
                    </button>
                    <button onclick="app.logout()" class="corporate-secondary px-4 py-2 rounded-lg font-semibold transition-colors ml-2 glow-button">
                        Cerrar Sesión
                    </button>
                </div>
            </div>
        </header>

        <!-- Contenido principal con navegación por pestañas -->
        <div class="container mx-auto p-4">
            <!-- Navegación por pestañas -->
            <div class="flex border-b border-gray-700 mb-6">
                <button id="dashboard-tab" class="tab-button active">Dashboard</button>
                <button id="orders-tab" class="tab-button">Órdenes</button>
            </div>

            <!-- Contenido del Dashboard -->
            <div id="dashboard-content">
                <!-- Métricas principales -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="dashboard-card metric-card">
                        <div class="metric-value" id="total-orders">0</div>
                        <div class="metric-label">Total Órdenes</div>
                    </div>
                    <div class="dashboard-card metric-card">
                        <div class="metric-value" id="completed-orders">0</div>
                        <div class="metric-label">Órdenes Completadas</div>
                    </div>
                    <div class="dashboard-card metric-card">
                        <div class="metric-value" id="in-progress-orders">0</div>
                        <div class="metric-label">Órdenes en Progreso</div>
                    </div>
                    <div class="dashboard-card metric-card">
                        <div class="metric-value" id="total-pieces">0</div>
                        <div class="metric-label">Total Piezas</div>
                    </div>
                </div>

                <!-- Gráficos y estadísticas -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-8">
                    <!-- Gráfico de estado de órdenes -->
                    <div class="dashboard-card">
                        <h3 class="dashboard-section-title">Estado de Órdenes</h3>
                        <div class="chart-container">
                            <canvas id="orders-status-chart"></canvas>
                        </div>
                    </div>

                    <!-- Gráfico de progreso de producción -->
                    <div class="dashboard-card">
                        <h3 class="dashboard-section-title">Progreso de Producción</h3>
                        <div class="chart-container">
                            <canvas id="production-progress-chart"></canvas>
                        </div>
                    </div>
                </div>

                <!-- Órdenes recientes y estadísticas por estilo -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-8">
                    <!-- Órdenes recientes -->
                    <div class="dashboard-card">
                        <h3 class="dashboard-section-title">Órdenes Recientes</h3>
                        <div class="overflow-x-auto">
                            <table class="recent-orders-table">
                                <thead>
                                    <tr>
                                        <th>Orden</th>
                                        <th>Cliente</th>
                                        <th>Estilo</th>
                                        <th>Progreso</th>
                                        <th>Estado</th>
                                    </tr>
                                </thead>
                                <tbody id="recent-orders-table-body">
                                    <!-- Las órdenes recientes se cargarán aquí -->
                                </tbody>
                            </table>
                        </div>
                    </div>

                    <!-- Estadísticas por estilo -->
                    <div class="dashboard-card">
                        <h3 class="dashboard-section-title">Órdenes por Estilo</h3>
                        <div class="chart-container">
                            <canvas id="orders-by-style-chart"></canvas>
                        </div>
                    </div>
                </div>

                <!-- Actividad reciente -->
                <div class="dashboard-card">
                    <h3 class="dashboard-section-title">Actividad Reciente</h3>
                    <div id="recent-activity" class="space-y-4">
                        <!-- La actividad reciente se cargará aquí -->
                    </div>
                </div>
            </div>

            <!-- Contenido de Órdenes (oculto inicialmente) -->
            <div id="orders-content" class="hidden">
                <!-- Búsqueda avanzada -->
                <div class="corporate-card rounded-lg shadow-md mb-4 p-4 card-shadow hover-lift">
                    <div class="flex flex-col md:flex-row gap-4">
                        <div class="flex-1">
                            <label for="searchInput" class="block text-sm font-medium text-gray-300">Buscar Órdenes:</label>
                            <input type="text" id="searchInput" placeholder="Buscar por número, estilo, color..." 
                                   class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-600 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md">
                        </div>
                        <div class="flex-1">
                            <label for="filterStatus" class="block text-sm font-medium text-gray-300">Filtrar por Estado:</label>
                            <select id="filterStatus" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-600 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md">
                                <option value="all">Todas las órdenes</option>
                                <option value="completed">Completadas</option>
                                <option value="in-progress">En progreso</option>
                                <option value="not-started">No iniciadas</option>
                            </select>
                        </div>
                    </div>
                </div>

                <div class="corporate-card rounded-lg shadow-md mb-4 p-4 card-shadow hover-lift">
                    <label for="orderSelect" class="block text-sm font-medium text-gray-300">Seleccionar Orden:</label>
                    <select id="orderSelect" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-600 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md" onchange="app.showOrder(this.value)">
                        <option value="">Selecciona una orden</option>
                    </select>
                </div>

                <div id="orderContent" class="corporate-card rounded-lg shadow-md p-6 card-shadow">
                    <div class="text-center text-gray-400 py-8">
                        <p class="text-lg">Crea una nueva orden de producción para comenzar</p>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal Nueva Orden -->
        <div id="newOrderModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 hidden z-50">
            <div class="corporate-card rounded-lg max-w-4xl w-full modal-content card-shadow">
                <div class="flex justify-between items-center p-6 border-b border-gray-700">
                    <h2 class="text-xl font-bold text-white">Nueva Orden de Producción</h2>
                    <button onclick="app.closeNewOrderModal()" class="text-gray-400 hover:text-white">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
                <div class="p-6">
                    <form id="newOrderForm">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-300 mb-2">Número de Orden</label>
                                <input type="text" id="orderNumber" required
                                       class="w-full p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500 validation-input"
                                       data-validation="orderNumber">
                                <div class="error-message hidden" id="orderNumberError"></div>
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-300 mb-2">Nombre del Cliente</label>
                                <input type="text" id="customerName" required
                                       class="w-full p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500 validation-input"
                                       data-validation="required">
                                <div class="error-message hidden" id="customerNameError"></div>
                            </div>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                            <div>
                                <label for="orderStyle" class="block text-sm font-medium text-gray-300 mb-2">Estilo</label>
                                <select id="orderStyle" required class="w-full p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500 validation-input" 
                                        onchange="app.generateSizeInputs()" data-validation="required">
                                    <option value="">Selecciona un estilo</option>
                                </select>
                                <div class="error-message hidden" id="orderStyleError"></div>
                            </div>
                            <div>
                                <label for="orderColor" class="block text-sm font-medium text-gray-300 mb-2">Color</label>
                                <select id="orderColor" required class="w-full p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500 validation-input" 
                                        onchange="app.generateSizeInputs()" data-validation="required">
                                    <option value="">Selecciona un color</option>
                                </select>
                                <div class="error-message hidden" id="orderColorError"></div>
                            </div>
                        </div>
                        <div class="mb-6">
                            <h3 class="text-lg font-semibold text-white mb-4">Configuración por Talla</h3>
                            <p class="text-sm text-gray-400 mb-4">Para cada talla, especifica la cantidad de cajas grandes y pequeñas, junto con las piezas por caja. **El SKU se asignará automáticamente.**</p>
                            <div id="sizesContainer"></div>
                            <div class="error-message hidden" id="sizesError"></div>
                        </div>
                        <div class="flex justify-end space-x-3 mt-6 pt-4 border-t border-gray-700">
                            <button type="button" onclick="app.closeNewOrderModal()" class="px-6 py-2 text-gray-400 hover:text-white transition-colors">
                                Cancelar
                            </button>
                            <button type="submit" class="corporate-primary px-6 py-2 rounded-md transition-colors glow-button flex items-center justify-center min-w-32">
                                <span id="createOrderText">Crear Orden</span>
                                <div id="createOrderSpinner" class="spinner hidden ml-2" style="width: 20px; height: 20px;"></div>
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>

        <!-- Modal de Escaneo de Cajas -->
        <div id="boxScanModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 hidden z-50">
            <div class="corporate-card rounded-lg max-w-4xl w-full modal-content card-shadow">
                <div class="flex justify-between items-center p-6 border-b border-gray-700">
                    <h2 class="text-xl font-bold text-white">Sistema de Escaneo de Cajas</h2>
                    <button onclick="app.closeBoxScanModal()" class="text-gray-400 hover:text-white">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
                <div class="p-6">
                    <div class="mb-6">
                        <div class="flex justify-between items-center">
                            <h3 class="text-lg font-semibold text-white">Escaneo en Tiempo Real</h3>
                            <div class="scan-counter">
                                <span id="boxScanCount">0</span> cajas escaneadas
                            </div>
                        </div>
                        <div id="scanFeedback" class="scan-feedback hidden"></div>
                    </div>
                    
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-6">
                        <div>
                            <label for="boxScannerInput" class="block text-sm font-medium text-gray-300 mb-2">Escanear Código de Caja</label>
                            <input type="text" id="boxScannerInput" 
                                   class="w-full p-4 border border-gray-600 rounded-md text-center text-lg font-semibold scanner-input"
                                   placeholder="Use el escáner para leer el código">
                            <div class="error-message hidden" id="boxScannerError"></div>
                            
                            <div class="mt-4">
                                <label for="boxSizeSelect" class="block text-sm font-medium text-gray-300 mb-2">Seleccionar Talla</label>
                                <select id="boxSizeSelect" class="w-full p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500">
                                    <option value="">Selecciona una talla</option>
                                </select>
                            </div>
                            
                            <p class="text-xs text-gray-400 mt-2">Presione Enter después de escanear o haga clic en "Agregar Caja"</p>
                        </div>
                        
                        <div class="bg-gray-800 p-4 rounded-lg">
                            <h4 class="font-semibold text-white mb-2">Información de la Orden</h4>
                            <p class="text-gray-300"><strong>Orden:</strong> <span id="scanOrderNumber">-</span></p>
                            <p class="text-gray-300"><strong>Cliente:</strong> <span id="scanCustomerName">-</span></p>
                            <p class="text-gray-300"><strong>Estilo:</strong> <span id="scanOrderStyle">-</span></p>
                            <p class="text-gray-300"><strong>Color:</strong> <span id="scanOrderColor">-</span></p>
                            <p class="text-gray-300"><strong>Talla Seleccionada:</strong> <span id="scanCurrentSize">-</span></p>
                            <p class="text-gray-300"><strong>SKU:</strong> <span id="scanCurrentSku">-</span></p>
                        </div>
                    </div>
                    
                    <div class="mb-4">
                        <button onclick="app.addScannedBox()" class="w-full corporate-primary py-3 rounded-md font-semibold transition-colors glow-button">
                            Agregar Caja Escaneada
                        </button>
                    </div>
                    
                    <div class="box-table-container">
                        <h4 class="font-semibold text-white mb-2">Cajas Escaneadas</h4>
                        <table class="box-table">
                            <thead>
                                <tr>
                                    <th>Caja#</th>
                                    <th>Caja ID</th>
                                    <th>Cajas Repetidas</th>
                                    <th>Estilo</th>
                                    <th>Color</th>
                                    <th>Tamaño</th>
                                    <th>Talla</th>
                                    <th>SKU/UPC</th>
                                    <th>Cantidad</th>
                                </tr>
                            </thead>
                            <tbody id="scannedBoxesTable">
                                <!-- Las cajas escaneadas se mostrarán aquí -->
                            </tbody>
                        </table>
                    </div>
                    
                    <div class="flex justify-between mt-6 pt-4 border-t border-gray-700">
                        <button onclick="app.generateExcelReport()" class="corporate-primary px-6 py-2 rounded-md transition-colors glow-button">
                            Generar Reporte Excel
                        </button>
                        <button onclick="app.clearScannedBoxes()" class="corporate-secondary px-6 py-2 rounded-md transition-colors glow-button">
                            Limpiar Cajas
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Modal de Escaneo -->
        <div id="scanModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 hidden z-50">
            <div class="corporate-card rounded-lg max-w-sm w-full p-6 modal-content card-shadow">
                <div class="flex justify-end">
                    <button onclick="app.closeScanModal()" class="text-gray-400 hover:text-white">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
                <div class="text-center mb-4">
                    <div class="mx-auto w-16 h-16 bg-gray-800 rounded-full flex items-center justify-center mb-3 pulse-glow">
                        <svg class="w-8 h-8 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v1m6 11h2m-6 0h-2v4m0-11v3m0 0h.01M12 12h4.01M16 20h4M4 12h4m12 0h.01M5 8h2a1 1 0 001-1V6a1 1 0 00-1-1H5a1 1 0 00-1 1v1a1 1 0 001 1z"></path>
                        </svg>
                    </div>
                    <h3 class="text-lg font-semibold text-white">Escanear Piezas</h3>
                    <p class="text-sm text-gray-400 mt-1">Talla: <span id="scanSize"></span></p>
                </div>
                <div class="space-y-4">
                    <div id="boxStatus" class="text-center font-bold text-white">
                        <span id="scannedInBox">0</span> / <span id="piecesPerBox">0</span> piezas en caja
                    </div>
                    <div class="flex items-center justify-center space-x-2">
                        <input type="checkbox" id="smallBoxCheck" onchange="app.updateBoxSize()" class="h-4 w-4 text-gray-400 focus:ring-gray-500 border-gray-600 rounded">
                        <label for="smallBoxCheck" class="text-sm font-medium text-gray-300">Escanear caja pequeña</label>
                    </div>
                    <div>
                        <label id="scannerLabel" class="block text-sm font-medium text-gray-300 mb-1">Escanear Código (SKU/EPC)</label>
                        <input type="text" id="scannerInput" class="scanner-input w-full p-3 border border-gray-600 rounded-md text-center text-lg font-semibold validation-input" data-validation="sku">
                        <div class="error-message hidden" id="scannerError"></div>
                    </div>
                    <div id="scanMessage" class="text-center text-sm font-semibold h-4"></div>
                </div>
            </div>
        </div>

        <!-- Notificación -->
        <div id="notification" class="fixed top-4 right-4 text-white py-2 px-4 rounded-md shadow-lg transition-transform transform translate-x-full hidden z-50">
            <div class="flex items-center">
                <span id="notificationMessage"></span>
            </div>
        </div>

        <!-- Modal de Mantenimiento -->
        <div id="maintenanceModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 hidden z-50">
            <div class="corporate-card rounded-lg max-w-4xl w-full modal-content card-shadow">
                <div class="flex justify-between items-center p-6 border-b border-gray-700">
                    <h2 class="text-xl font-bold text-white">Mantenimiento de Órdenes</h2>
                    <button onclick="app.closeMaintenanceModal()" class="text-gray-400 hover:text-white">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
                <div class="p-6 overflow-y-auto">
                    <label for="maintenanceOrderSelect" class="block text-sm font-medium text-gray-300">Seleccionar Orden:</label>
                    <select id="maintenanceOrderSelect" class="mt-1 block w-full pl-3 pr-10 py-2 text-base border-gray-600 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md" onchange="app.loadOrderForMaintenance(this.value)">
                        <option value="">Selecciona una orden</option>
                    </select>
                    <div id="maintenanceContent" class="mt-4"></div>
                </div>
                <div class="flex justify-between p-6 border-t border-gray-700">
                    <div class="flex space-x-2">
                        <button onclick="app.downloadOrderData('json')" class="corporate-secondary px-4 py-2 rounded-md transition-colors glow-button flex items-center">
                            <span>JSON</span>
                        </button>
                        <button onclick="app.downloadOrderData('csv')" class="corporate-secondary px-4 py-2 rounded-md transition-colors glow-button flex items-center">
                            <span>Excel (CSV)</span>
                        </button>
                    </div>
                    <div class="flex space-x-2">
                        <button onclick="app.saveMaintenanceChanges()" class="corporate-primary px-6 py-2 rounded-md transition-colors glow-button flex items-center justify-center min-w-32">
                            <span id="saveChangesText">Guardar Cambios</span>
                            <div id="saveChangesSpinner" class="spinner hidden ml-2" style="width: 20px; height: 20px;"></div>
                        </button>
                        <button onclick="app.showConfirmationModal()" class="corporate-secondary px-6 py-2 rounded-md transition-colors glow-button">
                            Eliminar Orden
                        </button>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Modal de Confirmación -->
        <div id="confirmationModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 hidden z-50">
            <div class="corporate-card rounded-lg p-6 max-w-sm w-full text-center modal-content card-shadow">
                <div class="mx-auto w-12 h-12 bg-red-900 rounded-full flex items-center justify-center mb-4">
                    <svg class="w-6 h-6 text-red-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z"></path>
                    </svg>
                </div>
                <h3 class="text-lg font-semibold text-white mb-2">¿Estás seguro?</h3>
                <p class="text-sm text-gray-400">Esta acción no se puede deshacer. Todos los datos de esta orden se perderán permanentemente.</p>
                <div class="flex justify-center space-x-4 mt-6">
                    <button onclick="app.closeConfirmationModal()" class="px-4 py-2 text-gray-400 hover:text-white transition-colors rounded-md">
                        Cancelar
                    </button>
                    <button onclick="app.deleteOrder()" class="corporate-secondary px-4 py-2 rounded-md transition-colors glow-button">
                        Eliminar
                    </button>
                </div>
            </div>
        </div>

        <!-- Modal de Gestión de SKUs -->
        <div id="skuModal" class="modal fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center p-4 hidden z-50">
            <div class="corporate-card rounded-lg max-w-4xl w-full modal-content card-shadow">
                <div class="flex justify-between items-center p-6 border-b border-gray-700">
                    <h2 class="text-xl font-bold text-white">Gestión de SKUs y EPCs</h2>
                    <button onclick="app.closeSkuModal()" class="text-gray-400 hover:text-white">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
                <div class="p-6">
                    <form id="skuForm">
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                            <div>
                                <label for="skuStyle" class="block text-sm font-medium text-gray-300 mb-2">Estilo</label>
                                <div class="flex space-x-2">
                                    <select id="skuStyleSelect" class="w-2/3 p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500" 
                                            onchange="app.handleStyleChange(this.value, 'select')">
                                        <option value="">Selecciona o añade un estilo</option>
                                    </select>
                                    <input type="text" id="skuStyleInput" placeholder="Añadir nuevo estilo" 
                                           class="w-1/3 p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500" 
                                           oninput="app.handleStyleChange(this.value, 'input')">
                                    <div class="error-message hidden" id="skuStyleError"></div>
                                </div>
                            </div>
                            <div>
                                <label for="skuColor" class="block text-sm font-medium text-gray-300 mb-2">Color</label>
                                <div class="flex space-x-2">
                                    <select id="skuColorSelect" class="w-2/3 p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500" 
                                            onchange="app.handleColorChange(this.value, 'select')">
                                        <option value="">Selecciona o añade un color</option>
                                    </select>
                                    <input type="text" id="skuColorInput" placeholder="Añadir nuevo color" 
                                           class="w-1/3 p-3 border border-gray-600 rounded-md focus:ring-gray-500 focus:border-gray-500" 
                                           oninput="app.handleColorChange(this.value, 'input')">
                                    <div class="error-message hidden" id="skuColorError"></div>
                                </div>
                            </div>
                        </div>
                        <div id="skuInputsContainer" class="mt-4"></div>
                        <div class="flex justify-end space-x-3 mt-6 pt-4 border-t border-gray-700">
                            <button type="button" onclick="app.closeSkuModal()" class="px-6 py-2 text-gray-400 hover:text-white transition-colors">
                                Cancelar
                            </button>
                            <button type="button" onclick="app.saveSkuData()" class="corporate-primary px-6 py-2 rounded-md transition-colors glow-button flex items-center justify-center min-w-32">
                                <span id="saveSkuText">Guardar SKUs</span>
                                <div id="saveSkuSpinner" class="spinner hidden ml-2" style="width: 20px; height: 20px;"></div>
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
    
    <script>
        // Sistema de autenticación
        const auth = {
            // Credenciales válidas
            validCredentials: {
                username: 'admin',
                password: 'cgb96'
            },
            
            // Verificar credenciales
            checkCredentials(username, password) {
                return username === this.validCredentials.username && password === this.validCredentials.password;
            },
            
            // Iniciar sesión
            login(username, password) {
                if (this.checkCredentials(username, password)) {
                    localStorage.setItem('isLoggedIn', 'true');
                    return true;
                }
                return false;
            },
            
            // Cerrar sesión
            logout() {
                localStorage.removeItem('isLoggedIn');
                window.location.reload();
            },
            
            // Verificar si el usuario está autenticado
            isAuthenticated() {
                return localStorage.getItem('isLoggedIn') === 'true';
            }
        };

        // Aplicación principal
        const app = {
            // Referencias del DOM
            DOM: {
                loginContainer: document.getElementById('login-container'),
                appContainer: document.getElementById('app-container'),
                loginForm: document.getElementById('login-form'),
                loginError: document.getElementById('login-error'),
                orderSelect: document.getElementById('orderSelect'),
                orderContent: document.getElementById('orderContent'),
                newOrderModal: document.getElementById('newOrderModal'),
                orderColorSelect: document.getElementById('orderColor'),
                orderStyleSelect: document.getElementById('orderStyle'),
                newOrderForm: document.getElementById('newOrderForm'),
                orderNumberInput: document.getElementById('orderNumber'),
                customerNameInput: document.getElementById('customerName'),
                sizesContainer: document.getElementById('sizesContainer'),
                scanModal: document.getElementById('scanModal'),
                scanSizeSpan: document.getElementById('scanSize'),
                scannerInput: document.getElementById('scannerInput'),
                scannedInBoxSpan: document.getElementById('scannedInBox'),
                piecesPerBoxSpan: document.getElementById('piecesPerBox'),
                smallBoxCheck: document.getElementById('smallBoxCheck'),
                scanMessageDiv: document.getElementById('scanMessage'),
                notification: document.getElementById('notification'),
                maintenanceModal: document.getElementById('maintenanceModal'),
                maintenanceOrderSelect: document.getElementById('maintenanceOrderSelect'),
                maintenanceContent: document.getElementById('maintenanceContent'),
                confirmationModal: document.getElementById('confirmationModal'),
                skuModal: document.getElementById('skuModal'),
                skuColorSelect: document.getElementById('skuColorSelect'),
                skuColorInput: document.getElementById('skuColorInput'),
                skuStyleSelect: document.getElementById('skuStyleSelect'),
                skuStyleInput: document.getElementById('skuStyleInput'),
                skuInputsContainer: document.getElementById('skuInputsContainer'),
                searchInput: document.getElementById('searchInput'),
                filterStatus: document.getElementById('filterStatus'),
                // Nuevas referencias para el sistema de escaneo de cajas
                boxScanModal: document.getElementById('boxScanModal'),
                boxScannerInput: document.getElementById('boxScannerInput'),
                boxScanCount: document.getElementById('boxScanCount'),
                scannedBoxesTable: document.getElementById('scannedBoxesTable'),
                scanFeedback: document.getElementById('scanFeedback'),
                scanOrderNumber: document.getElementById('scanOrderNumber'),
                scanCustomerName: document.getElementById('scanCustomerName'),
                scanOrderStyle: document.getElementById('scanOrderStyle'),
                scanOrderColor: document.getElementById('scanOrderColor'),
                scanCurrentSize: document.getElementById('scanCurrentSize'),
                scanCurrentSku: document.getElementById('scanCurrentSku'),
                boxSizeSelect: document.getElementById('boxSizeSelect'),
                // Referencias del dashboard
                dashboardContent: document.getElementById('dashboard-content'),
                ordersContent: document.getElementById('orders-content'),
                dashboardTab: document.getElementById('dashboard-tab'),
                ordersTab: document.getElementById('orders-tab'),
                totalOrders: document.getElementById('total-orders'),
                completedOrders: document.getElementById('completed-orders'),
                inProgressOrders: document.getElementById('in-progress-orders'),
                totalPieces: document.getElementById('total-pieces'),
                recentOrdersTableBody: document.getElementById('recent-orders-table-body'),
                recentActivity: document.getElementById('recent-activity')
            },

            // Estado de la aplicación
            state: {
                orders: {},
                productData: {},
                styles: [],
                colors: [],
                currentOrderNumber: null,
                currentSize: null,
                scannedInBox: 0,
                isSmallBox: false,
                piecesPerBox: 0,
                allSizes: ['XS', 'S', 'M', 'L', 'XL', '2XL', '3XL', '4XL', '5XL', 'L TALL', 'XL TALL', '2XL TALL', '3XL TALL', '4XL TALL', '5XL TALL'],
                // Nuevo estado para el sistema de escaneo de cajas
                scannedBoxes: [],
                currentScannedBoxId: 1,
                boxScanSession: {
                    orderNumber: null,
                    customerName: null,
                    style: null,
                    color: null,
                    currentSize: null,
                    currentSku: null
                },
                // Estado para el dashboard
                dashboardCharts: {}
            },

            // Funciones de Inicialización y Almacenamiento
            init() {
                // Verificar autenticación
                if (!auth.isAuthenticated()) {
                    this.showLogin();
                    return;
                }
                
                // Si está autenticado, mostrar la aplicación
                this.showApp();
                
                // Inicializar la aplicación
                this.loadOrders();
                this.loadProductData(); 
                this.loadStyles();
                this.loadColors();
                this.loadScannedBoxes();
                this.updateOrderSelects();
                this.setupEventListeners();
                this.setupSearchFunctionality();
                this.setupBoxScanning();
                this.setupDashboardNavigation();
                this.showDashboard();
            },
            
            // Mostrar pantalla de login
            showLogin() {
                this.DOM.loginContainer.style.display = 'flex';
                this.DOM.appContainer.style.display = 'none';
                
                // Configurar el formulario de login
                this.DOM.loginForm.addEventListener('submit', (e) => {
                    e.preventDefault();
                    const username = document.getElementById('username').value;
                    const password = document.getElementById('password').value;
                    
                    if (auth.login(username, password)) {
                        this.init(); // Reiniciar para mostrar la aplicación
                    } else {
                        this.DOM.loginError.style.display = 'block';
                    }
                });
            },
            
            // Mostrar aplicación principal
            showApp() {
                this.DOM.loginContainer.style.display = 'none';
                this.DOM.appContainer.style.display = 'block';
            },
            
            // Cerrar sesión
            logout() {
                auth.logout();
            },

            // Configurar navegación del dashboard
            setupDashboardNavigation() {
                this.DOM.dashboardTab.addEventListener('click', () => {
                    this.showDashboard();
                });

                this.DOM.ordersTab.addEventListener('click', () => {
                    this.showOrders();
                });
            },

            // Mostrar dashboard
            showDashboard() {
                this.DOM.dashboardContent.classList.remove('hidden');
                this.DOM.ordersContent.classList.add('hidden');
                
                // Actualizar clases de pestañas activas
                this.DOM.dashboardTab.classList.add('active');
                this.DOM.ordersTab.classList.remove('active');
                
                // Actualizar datos del dashboard
                this.updateDashboard();
            },

            // Mostrar órdenes
            showOrders() {
                this.DOM.dashboardContent.classList.add('hidden');
                this.DOM.ordersContent.classList.remove('hidden');
                
                // Actualizar clases de pestañas activas
                this.DOM.ordersTab.classList.add('active');
                this.DOM.dashboardTab.classList.remove('active');
            },

            // Actualizar dashboard
            updateDashboard() {
                this.calculateDashboardMetrics();
                this.renderRecentOrders();
                this.renderRecentActivity();
                this.renderCharts();
            },

            // Calcular métricas del dashboard
            calculateDashboardMetrics() {
                const orders = this.state.orders;
                let totalOrders = 0;
                let completedOrders = 0;
                let inProgressOrders = 0;
                let totalPieces = 0;

                for (const orderNumber in orders) {
                    totalOrders++;
                    const order = orders[orderNumber];
                    
                    // Calcular piezas totales
                    totalPieces += order.totalPieces;
                    
                    // Calcular estado de la orden
                    let totalScanned = 0;
                    let totalExpected = 0;
                    
                    for (const size in order.sizes) {
                        totalScanned += order.sizes[size].scannedPieces;
                        totalExpected += order.sizes[size].expectedPieces;
                    }
                    
                    if (totalScanned === 0) {
                        // No iniciada
                    } else if (totalScanned >= totalExpected) {
                        completedOrders++;
                    } else {
                        inProgressOrders++;
                    }
                }

                // Actualizar métricas en la interfaz
                this.DOM.totalOrders.textContent = totalOrders;
                this.DOM.completedOrders.textContent = completedOrders;
                this.DOM.inProgressOrders.textContent = inProgressOrders;
                this.DOM.totalPieces.textContent = totalPieces.toLocaleString();
            },

            // Renderizar órdenes recientes
            renderRecentOrders() {
                const orders = this.state.orders;
                const tableBody = this.DOM.recentOrdersTableBody;
                tableBody.innerHTML = '';

                // Obtener las 5 órdenes más recientes
                const recentOrders = Object.values(orders)
                    .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
                    .slice(0, 5);

                if (recentOrders.length === 0) {
                    tableBody.innerHTML = `
                        <tr>
                            <td colspan="5" class="text-center py-4 text-gray-400">
                                No hay órdenes recientes
                            </td>
                        </tr>
                    `;
                    return;
                }

                recentOrders.forEach(order => {
                    // Calcular progreso
                    let totalScanned = 0;
                    let totalExpected = 0;
                    
                    for (const size in order.sizes) {
                        totalScanned += order.sizes[size].scannedPieces;
                        totalExpected += order.sizes[size].expectedPieces;
                    }
                    
                    const progress = Math.min(100, (totalScanned / totalExpected) * 100) || 0;
                    
                    // Determinar estado
                    let status = 'not-started';
                    let statusText = 'No Iniciada';
                    
                    if (totalScanned === 0) {
                        status = 'not-started';
                        statusText = 'No Iniciada';
                    } else if (totalScanned >= totalExpected) {
                        status = 'completed';
                        statusText = 'Completada';
                    } else {
                        status = 'in-progress';
                        statusText = 'En Progreso';
                    }

                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td class="font-medium">${order.orderNumber}</td>
                        <td>${order.customerName}</td>
                        <td>${order.style}</td>
                        <td>
                            <div class="flex items-center">
                                <div class="w-full bg-gray-700 rounded-full h-2 mr-2">
                                    <div class="bg-gray-400 h-2 rounded-full" style="width: ${progress}%"></div>
                                </div>
                                <span class="text-xs">${progress.toFixed(0)}%</span>
                            </div>
                        </td>
                        <td>
                            <span class="status-badge status-${status}">${statusText}</span>
                        </td>
                    `;
                    tableBody.appendChild(row);
                });
            },

            // Renderizar actividad reciente
            renderRecentActivity() {
                const activityContainer = this.DOM.recentActivity;
                activityContainer.innerHTML = '';

                // Obtener las 10 actividades más recientes (escaneos de cajas)
                const recentScannedBoxes = this.state.scannedBoxes
                    .sort((a, b) => new Date(b.scannedAt) - new Date(a.scannedAt))
                    .slice(0, 10);

                if (recentScannedBoxes.length === 0) {
                    activityContainer.innerHTML = `
                        <div class="text-center text-gray-400 py-4">
                            No hay actividad reciente
                        </div>
                    `;
                    return;
                }

                recentScannedBoxes.forEach(box => {
                    const activityItem = document.createElement('div');
                    activityItem.className = 'flex items-start';
                    
                    const icon = document.createElement('div');
                    icon.className = 'flex-shrink-0 mr-3';
                    icon.innerHTML = `
                        <div class="w-8 h-8 bg-gray-700 rounded-full flex items-center justify-center">
                            <svg class="w-4 h-4 text-gray-400" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path>
                            </svg>
                        </div>
                    `;
                    
                    const content = document.createElement('div');
                    content.className = 'flex-1';
                    
                    const time = new Date(box.scannedAt).toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
                    
                    content.innerHTML = `
                        <p class="text-sm text-white">
                            Caja <span class="font-semibold">${box.boxId}</span> escaneada para la orden <span class="font-semibold">${box.orderNumber}</span>
                        </p>
                        <p class="text-xs text-gray-400 mt-1">
                            ${box.style} - ${box.color} - ${box.size} • ${time}
                        </p>
                    `;
                    
                    activityItem.appendChild(icon);
                    activityItem.appendChild(content);
                    activityContainer.appendChild(activityItem);
                });
            },

            // Renderizar gráficos
            renderCharts() {
                this.renderOrdersStatusChart();
                this.renderProductionProgressChart();
                this.renderOrdersByStyleChart();
            },

            // Gráfico de estado de órdenes
            renderOrdersStatusChart() {
                const ctx = document.getElementById('orders-status-chart').getContext('2d');
                
                // Destruir gráfico anterior si existe
                if (this.state.dashboardCharts.ordersStatus) {
                    this.state.dashboardCharts.ordersStatus.destroy();
                }
                
                const orders = this.state.orders;
                let completed = 0;
                let inProgress = 0;
                let notStarted = 0;

                for (const orderNumber in orders) {
                    const order = orders[orderNumber];
                    
                    let totalScanned = 0;
                    let totalExpected = 0;
                    
                    for (const size in order.sizes) {
                        totalScanned += order.sizes[size].scannedPieces;
                        totalExpected += order.sizes[size].expectedPieces;
                    }
                    
                    if (totalScanned === 0) {
                        notStarted++;
                    } else if (totalScanned >= totalExpected) {
                        completed++;
                    } else {
                        inProgress++;
                    }
                }

                this.state.dashboardCharts.ordersStatus = new Chart(ctx, {
                    type: 'doughnut',
                    data: {
                        labels: ['Completadas', 'En Progreso', 'No Iniciadas'],
                        datasets: [{
                            data: [completed, inProgress, notStarted],
                            backgroundColor: [
                                '#22c55e', // Verde para completadas
                                '#f59e0b', // Amarillo para en progreso
                                '#6b7280'  // Gris para no iniciadas
                            ],
                            borderWidth: 0
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                position: 'bottom',
                                labels: {
                                    color: '#d1d5db',
                                    font: {
                                        size: 12
                                    }
                                }
                            }
                        }
                    }
                });
            },

            // Gráfico de progreso de producción
            renderProductionProgressChart() {
                const ctx = document.getElementById('production-progress-chart').getContext('2d');
                
                // Destruir gráfico anterior si existe
                if (this.state.dashboardCharts.productionProgress) {
                    this.state.dashboardCharts.productionProgress.destroy();
                }
                
                const orders = this.state.orders;
                const labels = [];
                const expectedData = [];
                const scannedData = [];

                // Obtener las 5 órdenes más recientes
                const recentOrders = Object.values(orders)
                    .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt))
                    .slice(0, 5);

                recentOrders.forEach(order => {
                    labels.push(`Orden ${order.orderNumber}`);
                    
                    let totalScanned = 0;
                    let totalExpected = 0;
                    
                    for (const size in order.sizes) {
                        totalScanned += order.sizes[size].scannedPieces;
                        totalExpected += order.sizes[size].expectedPieces;
                    }
                    
                    expectedData.push(totalExpected);
                    scannedData.push(totalScanned);
                });

                this.state.dashboardCharts.productionProgress = new Chart(ctx, {
                    type: 'bar',
                    data: {
                        labels: labels,
                        datasets: [
                            {
                                label: 'Piezas Esperadas',
                                data: expectedData,
                                backgroundColor: '#374151',
                                borderColor: '#4b5563',
                                borderWidth: 1
                            },
                            {
                                label: 'Piezas Escaneadas',
                                data: scannedData,
                                backgroundColor: '#22c55e',
                                borderColor: '#16a34a',
                                borderWidth: 1
                            }
                        ]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        scales: {
                            x: {
                                grid: {
                                    color: '#374151'
                                },
                                ticks: {
                                    color: '#d1d5db'
                                }
                            },
                            y: {
                                beginAtZero: true,
                                grid: {
                                    color: '#374151'
                                },
                                ticks: {
                                    color: '#d1d5db'
                                }
                            }
                        },
                        plugins: {
                            legend: {
                                labels: {
                                    color: '#d1d5db'
                                }
                            }
                        }
                    }
                });
            },

            // Gráfico de órdenes por estilo
            renderOrdersByStyleChart() {
                const ctx = document.getElementById('orders-by-style-chart').getContext('2d');
                
                // Destruir gráfico anterior si existe
                if (this.state.dashboardCharts.ordersByStyle) {
                    this.state.dashboardCharts.ordersByStyle.destroy();
                }
                
                const orders = this.state.orders;
                const styleCounts = {};

                // Contar órdenes por estilo
                for (const orderNumber in orders) {
                    const order = orders[orderNumber];
                    const style = order.style;
                    
                    if (!styleCounts[style]) {
                        styleCounts[style] = 0;
                    }
                    
                    styleCounts[style]++;
                }

                const labels = Object.keys(styleCounts);
                const data = Object.values(styleCounts);

                this.state.dashboardCharts.ordersByStyle = new Chart(ctx, {
                    type: 'pie',
                    data: {
                        labels: labels,
                        datasets: [{
                            data: data,
                            backgroundColor: [
                                '#3b82f6', '#ef4444', '#10b981', '#f59e0b', 
                                '#8b5cf6', '#06b6d4', '#f97316', '#84cc16'
                            ],
                            borderWidth: 0
                        }]
                    },
                    options: {
                        responsive: true,
                        maintainAspectRatio: false,
                        plugins: {
                            legend: {
                                position: 'bottom',
                                labels: {
                                    color: '#d1d5db',
                                    font: {
                                        size: 12
                                    }
                                }
                            }
                        }
                    }
                });
            },

            // Resto de las funciones de la aplicación (sin cambios)
            setupEventListeners() {
                this.DOM.newOrderForm.addEventListener('submit', this.createNewOrder.bind(this));
                this.DOM.scannerInput.addEventListener('keydown', (e) => {
                    if (e.key === 'Enter') {
                        e.preventDefault();
                        this.processScannedCode();
                    }
                });

                // Validación en tiempo real solo para otros formularios, no para SKU
                document.querySelectorAll('.validation-input').forEach(input => {
                    if (!input.closest('#skuModal')) {
                        input.addEventListener('blur', (e) => {
                            this.validateField(e.target);
                        });
                        input.addEventListener('input', (e) => {
                            this.clearFieldError(e.target);
                        });
                    }
                });
            },

            setupBoxScanning() {
                // Configurar el escáner de cajas
                this.DOM.boxScannerInput.addEventListener('keydown', (e) => {
                    if (e.key === 'Enter') {
                        e.preventDefault();
                        this.addScannedBox();
                    }
                });

                // Configurar cambio de talla
                this.DOM.boxSizeSelect.addEventListener('change', (e) => {
                    this.updateBoxScanSession();
                });
            },

            setupSearchFunctionality() {
                this.DOM.searchInput.addEventListener('input', (e) => {
                    this.filterOrders(e.target.value, this.DOM.filterStatus.value);
                });

                this.DOM.filterStatus.addEventListener('change', (e) => {
                    this.filterOrders(this.DOM.searchInput.value, e.target.value);
                });
            },

            filterOrders(searchTerm, statusFilter) {
                const options = this.DOM.orderSelect.options;
                searchTerm = searchTerm.toLowerCase();
                
                for (let i = 1; i < options.length; i++) {
                    const orderNumber = options[i].value;
                    const order = this.state.orders[orderNumber];
                    const orderText = `Orden ${orderNumber} ${order.style} ${order.color}`.toLowerCase();
                    
                    // Calcular estado de la orden
                    let orderStatus = 'not-started';
                    let totalScanned = 0;
                    let totalExpected = 0;
                    
                    for (const size in order.sizes) {
                        totalScanned += order.sizes[size].scannedPieces;
                        totalExpected += order.sizes[size].expectedPieces;
                    }
                    
                    if (totalScanned === 0) {
                        orderStatus = 'not-started';
                    } else if (totalScanned >= totalExpected) {
                        orderStatus = 'completed';
                    } else {
                        orderStatus = 'in-progress';
                    }

                    const matchesSearch = searchTerm === '' || orderText.includes(searchTerm);
                    const matchesStatus = statusFilter === 'all' || orderStatus === statusFilter;
                    
                    options[i].style.display = (matchesSearch && matchesStatus) ? '' : 'none';
                }
            },

            loadOrders() {
                const storedOrders = localStorage.getItem('orders');
                if (storedOrders) {
                    this.state.orders = JSON.parse(storedOrders);
                }
            },

            loadProductData() {
                const storedData = localStorage.getItem('productData');
                if (storedData) {
                    this.state.productData = JSON.parse(storedData);
                }
            },

            loadStyles() {
                const storedStyles = localStorage.getItem('styles');
                if (storedStyles) {
                    this.state.styles = JSON.parse(storedStyles);
                }
            },

            loadColors() {
                const storedColors = localStorage.getItem('colors');
                if (storedColors) {
                    this.state.colors = JSON.parse(storedColors);
                }
            },

            loadScannedBoxes() {
                const storedBoxes = localStorage.getItem('scannedBoxes');
                if (storedBoxes) {
                    this.state.scannedBoxes = JSON.parse(storedBoxes);
                }
            },

            saveOrders() {
                localStorage.setItem('orders', JSON.stringify(this.state.orders));
                // Actualizar dashboard cuando se guardan órdenes
                this.updateDashboard();
            },

            saveProductData() {
                localStorage.setItem('productData', JSON.stringify(this.state.productData));
            },

            saveStyles() {
                localStorage.setItem('styles', JSON.stringify(this.state.styles));
            },

            saveColors() {
                localStorage.setItem('colors', JSON.stringify(this.state.colors));
            },

            saveScannedBoxes() {
                localStorage.setItem('scannedBoxes', JSON.stringify(this.state.scannedBoxes));
                // Actualizar dashboard cuando se guardan cajas escaneadas
                this.updateDashboard();
            },

            // Funciones de Validación
            validateField(field) {
                const validationType = field.getAttribute('data-validation');
                const value = field.value.trim();
                let isValid = true;
                let errorMessage = '';

                switch (validationType) {
                    case 'orderNumber':
                        if (!value) {
                            isValid = false;
                            errorMessage = 'El número de orden es obligatorio';
                        } else if (this.state.orders[value]) {
                            isValid = false;
                            errorMessage = 'Ya existe una orden con este número';
                        }
                        break;
                    
                    case 'required':
                        if (!value) {
                            isValid = false;
                            errorMessage = 'Este campo es obligatorio';
                        }
                        break;
                    
                    case 'sku':
                        // SKU ya no es obligatorio, solo validar formato si se ingresa
                        if (value && !/^[A-Z0-9\s-]+$/.test(value)) {
                            isValid = false;
                            errorMessage = 'El SKU/EPC solo puede contener letras mayúsculas, números, espacios y guiones';
                        }
                        break;
                    
                    case 'style':
                        if (value && !/^[A-Z0-9\s-]+$/.test(value)) {
                            isValid = false;
                            errorMessage = 'El estilo solo puede contener letras mayúsculas, números, espacios y guiones';
                        }
                        break;
                    
                    case 'color':
                        if (value && !/^[a-zA-Z\s-]+$/.test(value)) {
                            isValid = false;
                            errorMessage = 'El color solo puede contener letras, espacios y guiones';
                        }
                        break;

                    case 'positiveNumber':
                        if (value && (isNaN(value) || parseInt(value) < 0)) {
                            isValid = false;
                            errorMessage = 'Debe ser un número positivo';
                        }
                        break;
                }

                this.showFieldError(field, isValid, errorMessage);
                return isValid;
            },

            showFieldError(field, isValid, message) {
                const errorElement = document.getElementById(field.id + 'Error') || field.nextElementSibling;
                
                if (!isValid) {
                    field.classList.add('input-error');
                    if (errorElement && errorElement.classList.contains('error-message')) {
                        errorElement.textContent = message;
                        errorElement.classList.remove('hidden');
                    }
                } else {
                    this.clearFieldError(field);
                }
            },

            clearFieldError(field) {
                field.classList.remove('input-error');
                const errorElement = document.getElementById(field.id + 'Error') || field.nextElementSibling;
                if (errorElement && errorElement.classList.contains('error-message')) {
                    errorElement.classList.add('hidden');
                }
            },

            validateForm(form) {
                let isValid = true;
                const fields = form.querySelectorAll('.validation-input');
                
                fields.forEach(field => {
                    if (!this.validateField(field)) {
                        isValid = false;
                    }
                });

                return isValid;
            },

            // Funciones de los Modales
            showNewOrderModal() {
                this.DOM.newOrderModal.classList.remove('hidden');
                this.DOM.newOrderForm.reset();
                this.DOM.sizesContainer.innerHTML = '';
                this.populateStyleSelect(this.DOM.orderStyleSelect);
                this.populateColorSelect(this.DOM.orderColorSelect);
                
                // Limpiar errores
                document.querySelectorAll('.error-message').forEach(el => {
                    el.classList.add('hidden');
                });
                document.querySelectorAll('.input-error').forEach(el => {
                    el.classList.remove('input-error');
                });
            },

            closeNewOrderModal() {
                this.DOM.newOrderModal.classList.add('hidden');
                this.DOM.newOrderForm.reset();
            },

            showMaintenanceModal() {
                this.DOM.maintenanceModal.classList.remove('hidden');
                this.updateOrderSelects();
                this.DOM.maintenanceContent.innerHTML = '';
            },

            closeMaintenanceModal() {
                this.DOM.maintenanceModal.classList.add('hidden');
                this.DOM.maintenanceOrderSelect.value = '';
                this.DOM.maintenanceContent.innerHTML = '';
            },
            
            showSkuModal() {
                this.DOM.skuModal.classList.remove('hidden');
                this.DOM.skuStyleInput.value = '';
                this.DOM.skuColorInput.value = '';
                this.updateStyleAndColorSelects();
                this.DOM.skuInputsContainer.innerHTML = '';
                
                // Limpiar errores
                document.querySelectorAll('.error-message').forEach(el => {
                    el.classList.add('hidden');
                });
                document.querySelectorAll('.input-error').forEach(el => {
                    el.classList.remove('input-error');
                });
            },

            closeSkuModal() {
                this.DOM.skuModal.classList.add('hidden');
            },

            showConfirmationModal() {
                const orderNumber = this.DOM.maintenanceOrderSelect.value;
                if (!orderNumber) {
                    this.showNotification('Por favor, selecciona una orden para eliminar.', 'error');
                    return;
                }
                this.DOM.confirmationModal.classList.remove('hidden');
            },

            closeConfirmationModal() {
                this.DOM.confirmationModal.classList.add('hidden');
            },

            openScanModal() {
                this.DOM.scanSizeSpan.textContent = this.state.currentSize;
                this.DOM.scannerInput.value = '';
                this.DOM.scanMessageDiv.textContent = '';
                this.DOM.scannerInput.classList.remove('border-green-500', 'border-red-500');
                this.DOM.scanModal.classList.remove('hidden');
                setTimeout(() => this.DOM.scannerInput.focus(), 100);
            },

            closeScanModal() {
                this.DOM.scanModal.classList.add('hidden');
                this.state.scannedInBox = 0;
                this.saveOrders();
                this.showOrder(this.state.currentOrderNumber);
            },

            // Nuevas funciones para el sistema de escaneo de cajas
            openBoxScanModal(orderNumber, size) {
                if (!orderNumber || !this.state.orders[orderNumber]) {
                    this.showNotification('Por favor, selecciona una orden válida primero.', 'error');
                    return;
                }
                
                const order = this.state.orders[orderNumber];
                
                // Configurar la sesión de escaneo
                this.state.boxScanSession = {
                    orderNumber: orderNumber,
                    customerName: order.customerName,
                    style: order.style,
                    color: order.color,
                    currentSize: size || '',
                    currentSku: size ? order.sizes[size]?.sku : ''
                };
                
                // Actualizar la interfaz
                this.DOM.scanOrderNumber.textContent = orderNumber;
                this.DOM.scanCustomerName.textContent = order.customerName || '-';
                this.DOM.scanOrderStyle.textContent = order.style;
                this.DOM.scanOrderColor.textContent = order.color;
                this.DOM.scanCurrentSize.textContent = size || '-';
                this.DOM.scanCurrentSku.textContent = size ? order.sizes[size]?.sku : '-';
                
                // Llenar el selector de tallas
                this.populateSizeSelect(order);
                
                // Actualizar contador
                this.updateBoxScanCount();
                
                // Mostrar la tabla de cajas escaneadas
                this.renderScannedBoxesTable();
                
                // Mostrar el modal
                this.DOM.boxScanModal.classList.remove('hidden');
                setTimeout(() => this.DOM.boxScannerInput.focus(), 100);
            },

            populateSizeSelect(order) {
                this.DOM.boxSizeSelect.innerHTML = '<option value="">Selecciona una talla</option>';
                
                for (const size in order.sizes) {
                    const option = document.createElement('option');
                    option.value = size;
                    option.textContent = size;
                    this.DOM.boxSizeSelect.appendChild(option);
                }
                
                // Si se proporcionó una talla inicial, seleccionarla
                if (this.state.boxScanSession.currentSize) {
                    this.DOM.boxSizeSelect.value = this.state.boxScanSession.currentSize;
                }
            },

            updateBoxScanSession() {
                const selectedSize = this.DOM.boxSizeSelect.value;
                const order = this.state.orders[this.state.boxScanSession.orderNumber];
                
                if (selectedSize && order.sizes[selectedSize]) {
                    this.state.boxScanSession.currentSize = selectedSize;
                    this.state.boxScanSession.currentSku = order.sizes[selectedSize].sku;
                    
                    // Actualizar la interfaz
                    this.DOM.scanCurrentSize.textContent = selectedSize;
                    this.DOM.scanCurrentSku.textContent = order.sizes[selectedSize].sku;
                    
                    // Actualizar contador y tabla
                    this.updateBoxScanCount();
                    this.renderScannedBoxesTable();
                } else {
                    this.state.boxScanSession.currentSize = null;
                    this.state.boxScanSession.currentSku = null;
                    this.DOM.scanCurrentSize.textContent = '-';
                    this.DOM.scanCurrentSku.textContent = '-';
                }
            },

            closeBoxScanModal() {
                this.DOM.boxScanModal.classList.add('hidden');
                this.DOM.boxScannerInput.value = '';
                this.DOM.boxSizeSelect.value = '';
                this.hideScanFeedback();
                this.saveScannedBoxes();
            },

            addScannedBox() {
                const boxId = this.DOM.boxScannerInput.value.trim();
                const selectedSize = this.DOM.boxSizeSelect.value;
                
                if (!boxId) {
                    this.showScanFeedback('❌ El ID de la caja no puede estar vacío', 'error');
                    return;
                }
                
                if (!selectedSize) {
                    this.showScanFeedback('❌ Debe seleccionar una talla', 'error');
                    return;
                }
                
                // Verificar si la caja ya fue escaneada (duplicado)
                const isDuplicate = this.state.scannedBoxes.some(box => 
                    box.boxId === boxId && 
                    box.orderNumber === this.state.boxScanSession.orderNumber
                );
                
                if (isDuplicate) {
                    this.showScanFeedback('⚠️ Caja duplicada detectada', 'warning');
                    this.playDuplicateSound(); // Reproducir sonido de duplicado
                    
                    // Marcar como duplicada en la tabla
                    const duplicateBox = this.state.scannedBoxes.find(box => 
                        box.boxId === boxId && 
                        box.orderNumber === this.state.boxScanSession.orderNumber
                    );
                    
                    if (duplicateBox) {
                        duplicateBox.isDuplicate = true;
                        duplicateBox.duplicateCount = (duplicateBox.duplicateCount || 1) + 1;
                    }
                } else {
                    // Agregar nueva caja escaneada
                    const newBox = {
                        boxNumber: this.state.currentScannedBoxId++,
                        boxId: boxId,
                        isDuplicate: false,
                        duplicateCount: 0,
                        orderNumber: this.state.boxScanSession.orderNumber,
                        customerName: this.state.boxScanSession.customerName,
                        style: this.state.boxScanSession.style,
                        color: this.state.boxScanSession.color,
                        size: selectedSize,
                        sku: this.state.boxScanSession.currentSku,
                        quantity: 1,
                        scannedAt: new Date().toISOString()
                    };
                    
                    this.state.scannedBoxes.push(newBox);
                    this.showScanFeedback('✅ Caja agregada correctamente', 'success');
                }
                
                // Actualizar la interfaz
                this.updateBoxScanCount();
                this.renderScannedBoxesTable();
                this.DOM.boxScannerInput.value = '';
                this.DOM.boxScannerInput.focus();
                
                // Guardar en localStorage
                this.saveScannedBoxes();
            },
playDuplicateSound() {
    const audioContext = new (window.AudioContext || window.webkitAudioContext)();
    const oscillator = audioContext.createOscillator();
    const gainNode = audioContext.createGain();
    
    // SONIDO "SIGNAL" - Tik digital rápido
    oscillator.type = 'square'; // Sonido digital seco
    
    // Tono único: 1000Hz (entre 900-1200Hz)
    oscillator.frequency.setValueAtTime(1000, audioContext.currentTime);
    
    // Envolvente muy rápida: ataque y decay instantáneos
    gainNode.gain.setValueAtTime(0, audioContext.currentTime);     // Inicio en 0
    gainNode.gain.linearRampToValueAtTime(0.4, audioContext.currentTime + 0.01);  // Ataque súper rápido
    gainNode.gain.setValueAtTime(0.4, audioContext.currentTime + 0.04);           // Sostenido muy breve
    gainNode.gain.linearRampToValueAtTime(0, audioContext.currentTime + 0.08);    // Decay rápido
    
    oscillator.connect(gainNode);
    gainNode.connect(audioContext.destination);
    
    oscillator.start();
    oscillator.stop(audioContext.currentTime + 0.08); // Duración total: 0.08s
},
            updateBoxScanCount() {
                const count = this.state.scannedBoxes.filter(box => 
                    box.orderNumber === this.state.boxScanSession.orderNumber &&
                    box.size === this.state.boxScanSession.currentSize
                ).length;
                
                this.DOM.boxScanCount.textContent = count;
            },

            renderScannedBoxesTable() {
                const filteredBoxes = this.state.scannedBoxes.filter(box => 
                    box.orderNumber === this.state.boxScanSession.orderNumber
                );
                
                let tableHTML = '';
                
                if (filteredBoxes.length === 0) {
                    tableHTML = '<tr><td colspan="9" class="text-center py-4 text-gray-400">No hay cajas escaneadas aún</td></tr>';
                } else {
                    filteredBoxes.forEach(box => {
                        const duplicateInfo = box.isDuplicate ? 
                            `Sí (${box.duplicateCount} veces)` : 'No';
                        
                        const rowClass = box.isDuplicate ? 'duplicate-box' : 'scanned-box';
                        
                        tableHTML += `
                            <tr class="${rowClass}">
                                <td>${box.boxNumber}</td>
                                <td>${box.boxId}</td>
                                <td>${duplicateInfo}</td>
                                <td>${box.style}</td>
                                <td>${box.color}</td>
                                <td>${this.getBoxSizeType(box)}</td>
                                <td>${box.size}</td>
                                <td>${box.sku}</td>
                                <td>${box.quantity}</td>
                            </tr>
                        `;
                    });
                }
                
                this.DOM.scannedBoxesTable.innerHTML = tableHTML;
            },

            getBoxSizeType(box) {
                // Determinar si es caja REGULAR o TALL basado en la talla
                return box.size.includes('TALL') ? 'TLL' : 'REG';
            },

            showScanFeedback(message, type) {
                this.DOM.scanFeedback.textContent = message;
                this.DOM.scanFeedback.className = `scan-feedback scan-${type}`;
                this.DOM.scanFeedback.classList.remove('hidden');
                
                // Ocultar después de 3 segundos
                setTimeout(() => {
                    this.hideScanFeedback();
                }, 3000);
            },

            hideScanFeedback() {
                this.DOM.scanFeedback.classList.add('hidden');
            },

            generateExcelReport() {
                const filteredBoxes = this.state.scannedBoxes.filter(box => 
                    box.orderNumber === this.state.boxScanSession.orderNumber
                );
                
                if (filteredBoxes.length === 0) {
                    this.showNotification('No hay cajas escaneadas para generar el reporte.', 'warning');
                    return;
                }
                
                // Preparar datos para Excel
                const excelData = [
                    // Encabezados
                    ['Reporte de Cajas Escaneadas'],
                    ['Fecha:', new Date().toLocaleDateString()],
                    ['Hora:', new Date().toLocaleTimeString()],
                    ['Orden:', this.state.boxScanSession.orderNumber],
                    ['Cliente:', this.state.boxScanSession.customerName],
                    ['Estilo:', this.state.boxScanSession.style],
                    ['Color:', this.state.boxScanSession.color],
                    [],
                    // Encabezados de la tabla
                    ['Caja#', 'Caja ID', 'Cajas Repetidas', 'Estilo', 'Color', 'Tamaño', 'Talla', 'SKU/UPC', 'Cantidad']
                ];
                
                // Datos de las cajas
                filteredBoxes.forEach(box => {
                    const duplicateInfo = box.isDuplicate ? `Sí (${box.duplicateCount})` : 'No';
                    const boxSizeType = this.getBoxSizeType(box);
                    
                    excelData.push([
                        box.boxNumber,
                        box.boxId,
                        duplicateInfo,
                        box.style,
                        box.color,
                        boxSizeType,
                        box.size,
                        box.sku,
                        box.quantity
                    ]);
                });
                
                // Totales
                const totalBoxes = filteredBoxes.length;
                const totalPieces = filteredBoxes.reduce((sum, box) => sum + box.quantity, 0);
                const duplicateBoxes = filteredBoxes.filter(box => box.isDuplicate).length;
                
                excelData.push([]);
                excelData.push(['TOTALES:']);
                excelData.push(['Cajas Escaneadas:', totalBoxes]);
                excelData.push(['Piezas Totales:', totalPieces]);
                excelData.push(['Cajas Duplicadas:', duplicateBoxes]);
                excelData.push([]);
                excelData.push(['FIN DE REPORTE']);
                
                // Crear libro de trabajo
                const wb = XLSX.utils.book_new();
                const ws = XLSX.utils.aoa_to_sheet(excelData);
                
                // Ajustar anchos de columna
                const colWidths = [
                    {wch: 8},  // Caja#
                    {wch: 15}, // Caja ID
                    {wch: 15}, // Cajas Repetidas
                    {wch: 12}, // Estilo
                    {wch: 10}, // Color
                    {wch: 10}, // Tamaño
                    {wch: 10}, // Talla
                    {wch: 15}, // SKU/UPC
                    {wch: 10}  // Cantidad
                ];
                ws['!cols'] = colWidths;
                
                // Agregar hoja al libro
                XLSX.utils.book_append_sheet(wb, ws, 'Reporte Cajas');
                
                // Generar y descargar archivo
                XLSX.writeFile(wb, `reporte_cajas_${this.state.boxScanSession.orderNumber}_${new Date().toISOString().split('T')[0]}.xlsx`);
                
                this.showNotification('Reporte Excel generado y descargado correctamente', 'success');
            },

            clearScannedBoxes() {
                if (confirm('¿Estás seguro de que quieres eliminar todas las cajas escaneadas para esta orden?')) {
                    this.state.scannedBoxes = this.state.scannedBoxes.filter(box => 
                        box.orderNumber !== this.state.boxScanSession.orderNumber
                    );
                    
                    this.updateBoxScanCount();
                    this.renderScannedBoxesTable();
                    this.saveScannedBoxes();
                    this.showNotification('Cajas escaneadas eliminadas', 'success');
                }
            },

            // Funciones de Visualización y Datos
            populateStyleSelect(selectElement) {
                selectElement.innerHTML = '<option value="">Selecciona un estilo</option>';
                this.state.styles.forEach(style => {
                    const option = document.createElement('option');
                    option.value = style;
                    option.textContent = style;
                    selectElement.appendChild(option);
                });
            },

            populateColorSelect(selectElement) {
                selectElement.innerHTML = '<option value="">Selecciona un color</option>';
                this.state.colors.forEach(color => {
                    const option = document.createElement('option');
                    option.value = color;
                    option.textContent = color;
                    selectElement.appendChild(option);
                });
            },

            // Funciones del Modal de Nueva Orden
            generateSizeInputs() {
                const color = this.DOM.orderColorSelect.value.trim().toLowerCase();
                const style = this.DOM.orderStyleSelect.value.trim().toUpperCase();

                let html = '';

                if (color && style) {
                    const productSkus = this.state.productData[style]?.[color] || {};
                    this.state.allSizes.forEach(size => {
                        const sku = productSkus[size] || 'No asignado';
                        html += `
                            <div class="size-row">
                                <div class="size-header">${size} <span class="text-xs font-normal text-gray-400"> - SKU: ${sku}</span></div>
                                <div class="size-inputs">
                                    <div class="box-type-section">
                                        <div class="box-type-header">Cajas Grandes</div>
                                        <div class="input-group">
                                            <label>Cantidad:</label>
                                            <input type="number" id="${size}_large_count" min="0" value="0" class="validation-input" data-validation="positiveNumber">
                                        </div>
                                        <div class="input-group">
                                            <label>Piezas/Caja:</label>
                                            <input type="number" id="${size}_large_pieces" min="0" value="0" class="validation-input" data-validation="positiveNumber">
                                        </div>
                                    </div>
                                    <div class="box-type-section">
                                        <div class="box-type-header">Cajas Pequeñas</div>
                                        <div class="input-group">
                                            <label>Cantidad:</label>
                                            <input type="number" id="${size}_small_count" min="0" value="0" class="validation-input" data-validation="positiveNumber">
                                        </div>
                                        <div class="input-group">
                                            <label>Piezas/Caja:</label>
                                            <input type="number" id="${size}_small_pieces" min="0" value="0" class="validation-input" data-validation="positiveNumber">
                                        </div>
                                    </div>
                                </div>
                            </div>
                        `;
                    });
                }
                this.DOM.sizesContainer.innerHTML = html;
                
                // Reconfigurar event listeners para los nuevos campos
                this.DOM.sizesContainer.querySelectorAll('.validation-input').forEach(input => {
                    input.addEventListener('blur', (e) => {
                        this.validateField(e.target);
                    });
                    input.addEventListener('input', (e) => {
                        this.clearFieldError(e.target);
                    });
                });
            },

            async createNewOrder(e) {
                e.preventDefault();
                
                if (!this.validateForm(this.DOM.newOrderForm)) {
                    this.showNotification('Por favor, corrige los errores en el formulario.', 'error');
                    return;
                }

                // Mostrar indicador de carga
                this.showLoading('createOrder');

                try {
                    // Simular procesamiento asíncrono
                    await new Promise(resolve => setTimeout(resolve, 1500));

                    const orderNumber = this.DOM.orderNumberInput.value.trim();
                    const customerName = this.DOM.customerNameInput.value.trim();
                    const color = this.DOM.orderColorSelect.value.trim().toLowerCase();
                    const style = this.DOM.orderStyleSelect.value.trim().toUpperCase();
                    
                    const productSkus = this.state.productData[style]?.[color];
                    if (!productSkus) {
                        this.showNotification('No se encontraron SKUs para esta combinación de estilo y color. Por favor, asegúrate de haberlos creado en la Gestión de SKUs.', 'error');
                        this.hideLoading('createOrder');
                        return;
                    }
                    
                    const sizesData = {};
                    let totalPieces = 0;
                    let hasValidSizes = false;

                    this.state.allSizes.forEach(size => {
                        const largeCount = parseInt(document.getElementById(`${size}_large_count`).value) || 0;
                        const largePieces = parseInt(document.getElementById(`${size}_large_pieces`).value) || 0;
                        const smallCount = parseInt(document.getElementById(`${size}_small_count`).value) || 0;
                        const smallPieces = parseInt(document.getElementById(`${size}_small_pieces`).value) || 0;
                        const expectedPieces = (largeCount * largePieces) + (smallCount * smallPieces);

                        if (expectedPieces > 0) {
                            if (!productSkus[size]) {
                                this.showNotification(`No hay un SKU definido para la talla ${size} en esta combinación de producto. Por favor, defínelo en Gestión de SKUs antes de crear la orden.`, 'error');
                                this.hideLoading('createOrder');
                                return;
                            }

                            totalPieces += expectedPieces;
                            hasValidSizes = true;
                            sizesData[size] = {
                                sku: productSkus[size],
                                large_count: largeCount,
                                large_pieces: largePieces,
                                small_count: smallCount,
                                small_pieces: smallPieces,
                                expectedPieces: expectedPieces,
                                scannedPieces: 0,
                                scannedLargeBoxes: 0,
                                scannedSmallBoxes: 0
                            };
                        }
                    });

                    if (!hasValidSizes) {
                        this.showNotification('La orden debe tener al menos una talla con piezas.', 'error');
                        this.hideLoading('createOrder');
                        return;
                    }

                    this.state.orders[orderNumber] = {
                        orderNumber: orderNumber,
                        customerName: customerName,
                        color: color,
                        style: style,
                        sizes: sizesData,
                        totalPieces: totalPieces,
                        createdAt: new Date().toISOString()
                    };

                    this.saveOrders();
                    this.updateOrderSelects();
                    this.showOrder(orderNumber);
                    this.closeNewOrderModal();
                    this.showNotification('Orden creada exitosamente', 'success');
                    
                } catch (error) {
                    this.showNotification('Error al crear la orden: ' + error.message, 'error');
                } finally {
                    this.hideLoading('createOrder');
                }
            },

            // Funciones de Visualización de Orden
            updateOrderSelects() {
                this.updateSelectElement(this.DOM.orderSelect);
                this.updateSelectElement(this.DOM.maintenanceOrderSelect);
            },

            updateSelectElement(selectElement) {
                const currentOrder = selectElement.value;
                selectElement.innerHTML = '<option value="">Selecciona una orden</option>';
                for (const orderNumber in this.state.orders) {
                    const option = document.createElement('option');
                    option.value = orderNumber;
                    const order = this.state.orders[orderNumber];
                    
                    // Calcular estado
                    let totalScanned = 0;
                    let totalExpected = 0;
                    for (const size in order.sizes) {
                        totalScanned += order.sizes[size].scannedPieces;
                        totalExpected += order.sizes[size].expectedPieces;
                    }
                    
                    let statusText = '';
                    if (totalScanned === 0) {
                        statusText = ' (No iniciada)';
                    } else if (totalScanned >= totalExpected) {
                        statusText = ' (Completada)';
                    } else {
                        statusText = ' (En progreso)';
                    }
                    
                    option.textContent = `Orden ${orderNumber} - ${order.customerName} - ${order.style}${statusText}`;
                    selectElement.appendChild(option);
                }
                if (currentOrder && this.state.orders[currentOrder]) {
                    selectElement.value = currentOrder;
                }
            },

            showOrder(orderNumber) {
                if (!orderNumber) {
                    this.DOM.orderContent.innerHTML = `<div class="text-center text-gray-400 py-8"><p class="text-lg">Selecciona una orden para ver los detalles.</p></div>`;
                    return;
                }
                this.state.currentOrderNumber = orderNumber;
                const order = this.state.orders[orderNumber];
                
                // Calcular progreso general
                let totalScanned = 0;
                let totalExpected = 0;
                for (const size in order.sizes) {
                    totalScanned += order.sizes[size].scannedPieces;
                    totalExpected += order.sizes[size].expectedPieces;
                }
                const overallProgress = Math.min(100, (totalScanned / totalExpected) * 100) || 0;
                
                let orderDetails = `
                    <div class="mb-6">
                        <p class="font-semibold text-white mb-2">Detalles de la orden ${orderNumber}:</p>
                        <p class="text-sm text-gray-400">Cliente: <span class="font-semibold">${order.customerName}</span></p>
                        <p class="text-sm text-gray-400">Color: <span class="font-semibold">${order.color}</span> | Estilo: <span class="font-semibold">${order.style}</span></p>
                        <p class="text-sm text-gray-400">Creada: <span class="font-semibold">${new Date(order.createdAt).toLocaleDateString()}</span></p>
                    </div>
                    <div class="mb-6 bg-gray-800 p-4 rounded-lg">
                        <div class="flex justify-between items-center mb-2">
                            <p class="text-xl font-bold text-white">Progreso General</p>
                            <p class="text-lg font-semibold text-gray-300">${totalScanned} / ${totalExpected} piezas</p>
                        </div>
                        <div class="w-full bg-gray-700 rounded-full h-4">
                            <div class="bg-gray-400 h-4 rounded-full transition-all duration-300" style="width: ${overallProgress}%;"></div>
                        </div>
                    </div>
                `;
                
                let sizeListHTML = `<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">`;
                for (const size in order.sizes) {
                    const sizeData = order.sizes[size];
                    const totalBoxes = sizeData.scannedLargeBoxes + sizeData.scannedSmallBoxes;
                    const progress = Math.min(100, (sizeData.scannedPieces / sizeData.expectedPieces) * 100) || 0;
                    sizeListHTML += `
                        <div class="bg-gray-800 rounded-lg p-4 shadow-sm border border-gray-700">
                            <h4 class="font-bold text-white">${size}</h4>
                            <p class="text-xs text-gray-400">SKU: <span class="font-semibold">${sizeData.sku || 'N/A'}</span></p>
                            <p class="text-sm text-gray-400">Esperado: <span class="font-semibold">${sizeData.expectedPieces}</span> piezas</p>
                            <p class="text-sm text-gray-400">Escaneado: <span id="${orderNumber}_${size}_scanned" class="font-semibold text-gray-300">${sizeData.scannedPieces}</span> piezas</p>
                            <p class="text-sm text-gray-400">Cajas completadas: <span id="${orderNumber}_${size}_boxes" class="font-semibold text-gray-300">${totalBoxes}</span></p>
                            <div class="w-full bg-gray-700 rounded-full h-2.5 mt-2">
                                <div id="${orderNumber}_${size}_progress" class="bg-gray-400 h-2.5 rounded-full transition-all duration-300" style="width: ${progress}%;"></div>
                            </div>
                            <div class="flex flex-col space-y-2 mt-4">
                                <button onclick="app.startScanning('${orderNumber}', '${size}')" class="corporate-secondary font-semibold py-2 px-4 rounded transition-colors">
                                    Escanear ${size}
                                </button>
                                <button onclick="app.openBoxScanModal('${orderNumber}', '${size}')" class="corporate-primary font-semibold py-2 px-4 rounded transition-colors">
                                    Escanear Cajas ${size}
                                </button>
                            </div>
                        </div>
                    `;
                }
                sizeListHTML += `</div>`;
                this.DOM.orderContent.innerHTML = orderDetails + sizeListHTML;
            },

            // Funciones del Modal de Escáner
            startScanning(orderNumber, size) {
                this.state.currentOrderNumber = orderNumber;
                this.state.currentSize = size;
                const order = this.state.orders[orderNumber];
                const sizeData = order.sizes[size];
                this.state.scannedInBox = 0;
                this.state.isSmallBox = false;
                const smallBoxCheck = this.DOM.smallBoxCheck;
                if (sizeData.small_count > 0 && sizeData.scannedSmallBoxes < sizeData.small_count) {
                    smallBoxCheck.disabled = false;
                    smallBoxCheck.classList.remove('disabled-checkbox');
                } else {
                    smallBoxCheck.disabled = true;
                    smallBoxCheck.classList.add('disabled-checkbox');
                }
                smallBoxCheck.checked = false;
                this.updateBoxSize();
                this.openScanModal();
            },

            updateBoxSize() {
                const order = this.state.orders[this.state.currentOrderNumber];
                const sizeData = order.sizes[this.state.currentSize];
                this.state.isSmallBox = this.DOM.smallBoxCheck.checked;
                this.state.piecesPerBox = this.state.isSmallBox ? sizeData.small_pieces : sizeData.large_pieces;
                this.state.scannedInBox = 0;
                this.updateBoxStatus();
                this.DOM.scannerInput.focus();
            },

            updateBoxStatus() {
                this.DOM.scannedInBoxSpan.textContent = this.state.scannedInBox;
                this.DOM.piecesPerBoxSpan.textContent = this.state.piecesPerBox;
            },

            processScannedCode() {
                const code = this.DOM.scannerInput.value.trim();
                const order = this.state.orders[this.state.currentOrderNumber];
                const sizeData = order.sizes[this.state.currentSize];
                
                // Validar código
                if (!code) {
                    this.showScanMessage('❌ El código no puede estar vacío', 'red');
                    return;
                }
                
                if (sizeData.sku && code !== sizeData.sku) {
                    this.showScanMessage('❌ SKU incorrecto. Escanee el código correcto para esta talla.', 'red');
                    return;
                }
                
                if (this.state.scannedInBox >= this.state.piecesPerBox) {
                    this.showScanMessage('❌ Caja ya completa. Cierre el modal para continuar.', 'red');
                    return;
                }
                
                if (sizeData.scannedPieces >= sizeData.expectedPieces) {
                    this.showScanMessage('❌ Orden de producción para esta talla completada.', 'red');
                    return;
                }
                
                this.state.scannedInBox++;
                sizeData.scannedPieces++;
                if (this.state.scannedInBox === this.state.piecesPerBox) {
                    if (this.state.isSmallBox) {
                        sizeData.scannedSmallBoxes++;
                    } else {
                        sizeData.scannedLargeBoxes++;
                    }
                    this.showNotification('Caja completada ✅', 'success');
                    this.closeScanModal();
                } else {
                    this.showScanMessage('✅ Pieza registrada. Faltan ' + (this.state.piecesPerBox - this.state.scannedInBox) + ' piezas.', 'green');
                    this.updateBoxStatus();
                }
                this.saveOrders();
            },

            showScanMessage(message, type) {
                this.DOM.scanMessageDiv.textContent = message;
                this.DOM.scanMessageDiv.classList.remove('text-green-400', 'text-red-400');
                this.DOM.scanMessageDiv.classList.add(`text-${type}-400`);
                this.DOM.scannerInput.classList.add(`border-${type}-400`);
                setTimeout(() => {
                    this.DOM.scannerInput.value = '';
                    this.DOM.scannerInput.classList.remove(`border-${type}-400`);
                    this.DOM.scanMessageDiv.textContent = '';
                }, 500);
            },
            
            showNotification(message, type = 'success') {
                const notification = this.DOM.notification;
                const messageElement = document.getElementById('notificationMessage');
                
                messageElement.textContent = message;
                
                // Configurar colores según el tipo
                if (type === 'success') {
                    notification.className = 'fixed top-4 right-4 bg-green-900 text-green-100 py-2 px-4 rounded-md shadow-lg transition-transform transform translate-x-full hidden z-50';
                } else if (type === 'error') {
                    notification.className = 'fixed top-4 right-4 bg-red-900 text-red-100 py-2 px-4 rounded-md shadow-lg transition-transform transform translate-x-full hidden z-50';
                } else if (type === 'warning') {
                    notification.className = 'fixed top-4 right-4 bg-yellow-900 text-yellow-100 py-2 px-4 rounded-md shadow-lg transition-transform transform translate-x-full hidden z-50';
                }
                
                notification.classList.remove('hidden', 'translate-x-full');
                notification.classList.add('translate-x-0');
                
                setTimeout(() => {
                    notification.classList.remove('translate-x-0');
                    notification.classList.add('translate-x-full');
                    setTimeout(() => notification.classList.add('hidden'), 500);
                }, 3000);
            },

            // Funciones de carga
            showLoading(buttonType) {
                const spinner = document.getElementById(`${buttonType}Spinner`);
                const text = document.getElementById(`${buttonType}Text`);
                
                if (spinner) spinner.classList.remove('hidden');
                if (text) text.classList.add('opacity-50');
            },

            hideLoading(buttonType) {
                const spinner = document.getElementById(`${buttonType}Spinner`);
                const text = document.getElementById(`${buttonType}Text`);
                
                if (spinner) spinner.classList.add('hidden');
                if (text) text.classList.remove('opacity-50');
            },

            // Funciones del Modal de Mantenimiento
            loadOrderForMaintenance(orderNumber) {
                if (!orderNumber) {
                    this.DOM.maintenanceContent.innerHTML = '';
                    return;
                }
                this.state.currentOrderNumber = orderNumber;
                const order = this.state.orders[orderNumber];
                let maintenanceHTML = `
                    <h3 class="text-lg font-semibold text-white mb-4">Mantenimiento de Orden ${orderNumber}</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                        <div>
                            <label class="block text-sm font-medium text-gray-300">Cliente:</label>
                            <input type="text" value="${order.customerName || 'N/A'}" readonly class="w-full p-2 bg-gray-700 border border-gray-600 rounded-md text-sm">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-300">Color:</label>
                            <input type="text" value="${order.color || 'N/A'}" readonly class="w-full p-2 bg-gray-700 border border-gray-600 rounded-md text-sm">
                        </div>
                        <div>
                            <label class="block text-sm font-medium text-gray-300">Estilo:</label>
                            <input type="text" value="${order.style || 'N/A'}" readonly class="w-full p-2 bg-gray-700 border border-gray-600 rounded-md text-sm">
                        </div>
                    </div>
                    <p class="text-sm text-gray-400 mb-4">Edita las cantidades, el SKU y elimina tallas si es necesario.</p>
                    <div id="sizeMaintenanceContainer" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                `;
                const existingSizes = Object.keys(order.sizes);
                existingSizes.forEach(size => {
                    const sizeData = order.sizes[size];
                    maintenanceHTML += `
                        <div class="bg-gray-800 rounded-lg p-4 shadow-sm border border-gray-700" id="maint_${size}_card">
                            <div class="flex justify-between items-center mb-2">
                                <h4 class="font-bold text-white">${size}</h4>
                                <div class="flex space-x-2">
                                    <button onclick="app.resetScannedData('${orderNumber}', '${size}')" class="text-red-400 hover:text-red-300 text-xs font-semibold">Eliminar Datos Escaneados</button>
                                    <button onclick="app.removeSizeFromMaintenance('${orderNumber}', '${size}')" class="corporate-secondary text-xs px-2 py-1 rounded">Eliminar Talla</button>
                                </div>
                            </div>
                            <div class="space-y-3">
                                <div>
                                    <label class="block text-xs font-medium text-gray-400">SKU:</label>
                                    <input type="text" id="maint_${size}_sku" value="${sizeData.sku || ''}" class="w-full p-2 border border-gray-600 rounded-md text-sm validation-input" data-validation="sku">
                                </div>
                                <div>
                                    <label class="block text-xs font-medium text-gray-400">Cantidad Cajas Grandes:</label>
                                    <input type="number" id="maint_${size}_large_count" value="${sizeData.large_count}" min="0" class="w-full p-2 border border-gray-600 rounded-md text-sm validation-input" data-validation="positiveNumber">
                                </div>
                                <div>
                                    <label class="block text-xs font-medium text-gray-400">Piezas/Caja Grande:</label>
                                    <input type="number" id="maint_${size}_large_pieces" value="${sizeData.large_pieces}" min="0" class="w-full p-2 border border-gray-600 rounded-md text-sm validation-input" data-validation="positiveNumber">
                                </div>
                                <hr class="my-2 border-gray-600">
                                <div>
                                    <label class="block text-xs font-medium text-gray-400">Cantidad Cajas Pequeñas:</label>
                                    <input type="number" id="maint_${size}_small_count" value="${sizeData.small_count}" min="0" class="w-full p-2 border border-gray-600 rounded-md text-sm validation-input" data-validation="positiveNumber">
                                </div>
                                <div>
                                    <label class="block text-xs font-medium text-gray-400">Piezas/Caja Pequeña:</label>
                                    <input type="number" id="maint_${size}_small_pieces" value="${sizeData.small_pieces}" min="0" class="w-full p-2 border border-gray-600 rounded-md text-sm validation-input" data-validation="positiveNumber">
                                </div>
                            </div>
                        </div>
                    `;
                });
                maintenanceHTML += `</div><hr class="my-6 border-gray-600">`;
                maintenanceHTML += this.renderNewSizeForm(order);
                this.DOM.maintenanceContent.innerHTML = maintenanceHTML;
                
                // Reconfigurar event listeners
                this.DOM.maintenanceContent.querySelectorAll('.validation-input').forEach(input => {
                    input.addEventListener('blur', (e) => {
                        this.validateField(e.target);
                    });
                    input.addEventListener('input', (e) => {
                        this.clearFieldError(e.target);
                    });
                });
            },

            renderNewSizeForm(order) {
                const existingSizes = Object.keys(order.sizes);
                const availableSizes = this.state.allSizes.filter(size => !existingSizes.includes(size));

                if (availableSizes.length === 0) {
                    return `<p class="text-sm text-gray-400 text-center">No hay más tallas disponibles para agregar a esta orden.</p>`;
                }

                let sizeOptions = availableSizes.map(size => `<option value="${size}">${size}</option>`).join('');

                return `
                    <div class="bg-gray-800 p-4 rounded-lg">
                        <h3 class="font-bold text-white mb-4">Añadir una Talla</h3>
                        <div class="mb-4">
                            <label for="add_size_select" class="block text-sm font-medium text-gray-300">Seleccionar Talla</label>
                            <select id="add_size_select" class="w-full p-2 border border-gray-600 rounded-md text-sm">
                                <option value="">Selecciona una talla...</option>
                                ${sizeOptions}
                            </select>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                            <div class="box-type-section bg-gray-700 p-4">
                                <div class="box-type-header">Cajas Grandes</div>
                                <div class="input-group">
                                    <label>Cantidad:</label>
                                    <input type="number" id="add_size_large_count" min="0" value="0" class="validation-input" data-validation="positiveNumber">
                                </div>
                                <div class="input-group mt-2">
                                    <label>Piezas/Caja:</label>
                                    <input type="number" id="add_size_large_pieces" min="0" value="0" class="validation-input" data-validation="positiveNumber">
                                </div>
                            </div>
                            <div class="box-type-section bg-gray-700 p-4">
                                <div class="box-type-header">Cajas Pequeñas</div>
                                <div class="input-group">
                                    <label>Cantidad:</label>
                                    <input type="number" id="add_size_small_count" min="0" value="0" class="validation-input" data-validation="positiveNumber">
                                </div>
                                <div class="input-group mt-2">
                                    <label>Piezas/Caja:</label>
                                    <input type="number" id="add_size_small_pieces" min="0" value="0" class="validation-input" data-validation="positiveNumber">
                                </div>
                            </div>
                        </div>
                        <button type="button" onclick="app.addSizeToOrder()" class="w-full corporate-primary px-6 py-2 rounded-md transition-colors mt-2">
                            Añadir Talla a la Orden
                        </button>
                    </div>
                `;
            },

            addSizeToOrder() {
                const orderNumber = this.state.currentOrderNumber;
                if (!orderNumber) return;

                const order = this.state.orders[orderNumber];
                const newSize = document.getElementById('add_size_select').value;
                const largeCount = parseInt(document.getElementById('add_size_large_count').value) || 0;
                const largePieces = parseInt(document.getElementById('add_size_large_pieces').value) || 0;
                const smallCount = parseInt(document.getElementById('add_size_small_count').value) || 0;
                const smallPieces = parseInt(document.getElementById('add_size_small_pieces').value) || 0;

                if (!newSize) {
                    this.showNotification('Por favor, selecciona una talla para añadir.', 'error');
                    return;
                }
                
                const expectedPieces = (largeCount * largePieces) + (smallCount * smallPieces);
                if (expectedPieces <= 0) {
                    this.showNotification('La nueva talla debe tener al menos una pieza.', 'error');
                    return;
                }

                const productSkus = this.state.productData[order.style]?.[order.color];
                if (!productSkus || !productSkus[newSize]) {
                    this.showNotification(`No hay un SKU definido para la talla ${newSize} en esta combinación de producto. Por favor, defínelo en Gestión de SKUs antes de añadirlo.`, 'error');
                    return;
                }

                order.sizes[newSize] = {
                    sku: productSkus[newSize],
                    large_count: largeCount,
                    large_pieces: largePieces,
                    small_count: smallCount,
                    small_pieces: smallPieces,
                    expectedPieces: expectedPieces,
                    scannedPieces: 0,
                    scannedLargeBoxes: 0,
                    scannedSmallBoxes: 0
                };
                
                order.totalPieces += expectedPieces;
                this.saveOrders();
                this.loadOrderForMaintenance(orderNumber);
                this.showOrder(orderNumber);
                this.showNotification(`Talla ${newSize} añadida exitosamente.`, 'success');
            },

            async saveMaintenanceChanges() {
                const orderNumber = this.DOM.maintenanceOrderSelect.value;
                if (!orderNumber) {
                    this.showNotification('Por favor, selecciona una orden para guardar.', 'error');
                    return;
                }

                // Mostrar indicador de carga
                this.showLoading('saveChanges');

                try {
                    // Simular procesamiento asíncrono
                    await new Promise(resolve => setTimeout(resolve, 1000));

                    const order = this.state.orders[orderNumber];
                    let newTotalPieces = 0;
                    const currentSizes = Object.keys(order.sizes);
                    
                    currentSizes.forEach(size => {
                        const sizeData = order.sizes[size];
                        const largeCount = parseInt(document.getElementById(`maint_${size}_large_count`).value) || 0;
                        const largePieces = parseInt(document.getElementById(`maint_${size}_large_pieces`).value) || 0;
                        const smallCount = parseInt(document.getElementById(`maint_${size}_small_count`).value) || 0;
                        const smallPieces = parseInt(document.getElementById(`maint_${size}_small_pieces`).value) || 0;
                        const sku = document.getElementById(`maint_${size}_sku`).value.trim();
                        const expectedPieces = (largeCount * largePieces) + (smallCount * smallPieces);
                        
                        if (expectedPieces > 0) {
                            sizeData.large_count = largeCount;
                            sizeData.large_pieces = largePieces;
                            sizeData.small_count = smallCount;
                            sizeData.small_pieces = smallPieces;
                            sizeData.sku = sku;
                            sizeData.expectedPieces = expectedPieces;
                            newTotalPieces += expectedPieces;
                        } else {
                            delete order.sizes[size];
                        }
                    });
                    order.totalPieces = newTotalPieces;
                    this.saveOrders();
                    this.updateOrderSelects();
                    this.showOrder(orderNumber);
                    this.closeMaintenanceModal();
                    this.showNotification('Cambios guardados exitosamente.', 'success');
                    
                } catch (error) {
                    this.showNotification('Error al guardar los cambios: ' + error.message, 'error');
                } finally {
                    this.hideLoading('saveChanges');
                }
            },

            removeSizeFromMaintenance(orderNumber, size) {
                const order = this.state.orders[orderNumber];
                const sizeCard = document.getElementById(`maint_${size}_card`);
                if (confirm(`¿Estás seguro de que quieres eliminar la talla ${size} de esta orden?`)) {
                    order.totalPieces -= order.sizes[size].expectedPieces;
                    delete order.sizes[size];
                    this.saveOrders();
                    sizeCard.remove();
                    this.loadOrderForMaintenance(orderNumber);
                    this.showOrder(orderNumber);
                    this.showNotification('Talla eliminada exitosamente. Recuerda Guardar Cambios para que la modificación sea permanente.', 'warning');
                }
            },

            resetScannedData(orderNumber, size) {
                const order = this.state.orders[orderNumber];
                const sizeData = order.sizes[size];
                if (confirm(`¿Estás seguro de que quieres restablecer los datos escaneados para la talla ${size}?`)) {
                    sizeData.scannedPieces = 0;
                    sizeData.scannedLargeBoxes = 0;
                    sizeData.scannedSmallBoxes = 0;
                    this.saveOrders();
                    this.loadOrderForMaintenance(orderNumber);
                    this.showOrder(orderNumber);
                    this.showNotification('Datos escaneados restablecidos. Recuerda Guardar Cambios para que la modificación sea permanente.', 'warning');
                }
            },

            downloadOrderData(format) {
                const orderNumber = this.DOM.maintenanceOrderSelect.value;
                if (!orderNumber) {
                    this.showNotification('Por favor, selecciona una orden para descargar.', 'error');
                    return;
                }
                const order = this.state.orders[orderNumber];
                
                if (format === 'json') {
                    const data = JSON.stringify(order, null, 2);
                    const blob = new Blob([data], { type: 'application/json' });
                    const url = URL.createObjectURL(blob);
                    const a = document.createElement('a');
                    a.href = url;
                    a.download = `orden_${orderNumber}.json`;
                    document.body.appendChild(a);
                    a.click();
                    document.body.removeChild(a);
                    URL.revokeObjectURL(url);
                } else if (format === 'csv') {
                    // Cabeceras mejoradas para Excel
                    let csvContent = 'Número de Orden,Cliente,Estilo,Color,Talla,SKU,Piezas Esperadas,Piezas Escaneadas,Porcentaje Completado,Cajas Grandes Programadas,Piezas por Caja Grande,Cajas Pequeñas Programadas,Piezas por Caja Pequeña,Cajas Grandes Escaneadas,Cajas Pequeñas Escaneadas,Fecha de Creación,Estado\n';
                    
                    for (const size in order.sizes) {
                        const sizeData = order.sizes[size];
                        const completion = ((sizeData.scannedPieces / sizeData.expectedPieces) * 100).toFixed(2);
                        
                        // Determinar estado
                        let estado = '';
                        if (sizeData.scannedPieces === 0) {
                            estado = 'No Iniciado';
                        } else if (sizeData.scannedPieces >= sizeData.expectedPieces) {
                            estado = 'Completado';
                        } else {
                            estado = 'En Progreso';
                        }
                        
                        csvContent += `"${orderNumber}","${order.customerName}","${order.style}","${order.color}","${size}","${sizeData.sku || 'N/A'}","${sizeData.expectedPieces}","${sizeData.scannedPieces}","${completion}%","${sizeData.large_count}","${sizeData.large_pieces}","${sizeData.small_count}","${sizeData.small_pieces}","${sizeData.scannedLargeBoxes}","${sizeData.scannedSmallBoxes}","${new Date(order.createdAt).toLocaleDateString()}","${estado}"\n`;
                    }
                    
                    // Añadir resumen general al final
                    let totalScanned = 0;
                    let totalExpected = 0;
                    let totalLargeBoxesScanned = 0;
                    let totalSmallBoxesScanned = 0;
                    
                    for (const size in order.sizes) {
                        const sizeData = order.sizes[size];
                        totalScanned += sizeData.scannedPieces;
                        totalExpected += sizeData.expectedPieces;
                        totalLargeBoxesScanned += sizeData.scannedLargeBoxes;
                        totalSmallBoxesScanned += sizeData.scannedSmallBoxes;
                    }
                    
                    const overallCompletion = ((totalScanned / totalExpected) * 100).toFixed(2);
                    
                    csvContent += `\nRESUMEN GENERAL\n`;
                    csvContent += `"Total Piezas Esperadas","${totalExpected}"\n`;
                    csvContent += `"Total Piezas Escaneadas","${totalScanned}"\n`;
                    csvContent += `"Porcentaje General","${overallCompletion}%"\n`;
                    csvContent += `"Total Cajas Grandes Escaneadas","${totalLargeBoxesScanned}"\n`;
                    csvContent += `"Total Cajas Pequeñas Escaneadas","${totalSmallBoxesScanned}"\n`;
                    csvContent += `"Total Cajas Completadas","${totalLargeBoxesScanned + totalSmallBoxesScanned}"\n`;
                    
                    const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
                    const url = URL.createObjectURL(blob);
                    const link = document.createElement('a');
                    link.href = url;
                    link.download = `Orden_${orderNumber}_${order.customerName}_${order.style}_${order.color}_${new Date().toISOString().split('T')[0]}.csv`;
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                    URL.revokeObjectURL(url);
                }
                
                this.showNotification(`Datos exportados en formato ${format.toUpperCase()}`, 'success');
            },

            deleteOrder() {
                const orderNumber = this.DOM.maintenanceOrderSelect.value;
                delete this.state.orders[orderNumber];
                this.saveOrders();
                this.updateOrderSelects();
                this.DOM.orderSelect.value = '';
                this.showOrder(null);
                this.closeConfirmationModal();
                this.closeMaintenanceModal();
                this.showNotification('Orden eliminada exitosamente.', 'success');
            },

            // Funciones del Modal de Gestión de SKUs
            handleStyleChange(value, type) {
                if (type === 'select') {
                    this.DOM.skuStyleInput.value = '';
                } else if (type === 'input') {
                    this.DOM.skuStyleSelect.value = '';
                }
                this.renderSkuInputs();
            },

            handleColorChange(value, type) {
                if (type === 'select') {
                    this.DOM.skuColorInput.value = '';
                } else if (type === 'input') {
                    this.DOM.skuColorSelect.value = '';
                }
                this.renderSkuInputs();
            },

            updateStyleAndColorSelects() {
                this.populateStyleSelect(this.DOM.skuStyleSelect);
                this.populateColorSelect(this.DOM.skuColorSelect);
                this.renderSkuInputs();
            },

            renderSkuInputs() {
                const selectedStyle = this.DOM.skuStyleSelect.value;
                const newStyle = this.DOM.skuStyleInput.value.trim().toUpperCase();
                const selectedColor = this.DOM.skuColorSelect.value;
                const newColor = this.DOM.skuColorInput.value.trim().toLowerCase();

                const style = selectedStyle || newStyle;
                const color = selectedColor || newColor;
                
                let html = '';

                if (style && color) {
                    const existingSkus = this.state.productData[style]?.[color] || {};
                    html = `<h3 class="font-semibold text-white mb-2">Asignar SKUs para Estilo ${style} - Color ${color}</h3>
                            <p class="text-sm text-gray-400 mb-4">Los campos de SKU son opcionales. Puedes guardar sin completar todos los campos.</p>
                            <div class="space-y-3">`;
                    this.state.allSizes.forEach(size => {
                        const skuValue = existingSkus[size] || '';
                        html += `
                            <div class="optional-field">
                                <label for="sku_${size}" class="block text-xs font-medium text-gray-400 optional-field-label">${size} SKU/EPC (Opcional):</label>
                                <input type="text" id="sku_${size}" value="${skuValue}" class="w-full p-2 border border-gray-600 rounded-md text-sm">
                            </div>
                        `;
                    });
                    html += `</div>`;
                } else {
                    html = `<p class="text-gray-400 text-center">Selecciona un estilo y color o agrega uno nuevo para asignar los SKUs.</p>`;
                }

                this.DOM.skuInputsContainer.innerHTML = html;
            },

            async saveSkuData() {
                // QUITAMOS LA VALIDACIÓN DEL FORMULARIO - EL BOTÓN SIEMPRE FUNCIONA
                
                // Mostrar indicador de carga
                this.showLoading('saveSku');

                try {
                    // Simular procesamiento asíncrono
                    await new Promise(resolve => setTimeout(resolve, 1000));

                    const selectedStyle = this.DOM.skuStyleSelect.value;
                    const newStyle = this.DOM.skuStyleInput.value.trim().toUpperCase();
                    const selectedColor = this.DOM.skuColorSelect.value;
                    const newColor = this.DOM.skuColorInput.value.trim().toLowerCase();

                    const style = selectedStyle || newStyle;
                    const color = selectedColor || newColor;

                    // Si no hay estilo ni color, mostrar advertencia pero permitir guardar
                    if (!style && !color) {
                        this.showNotification('No se ha seleccionado ningún estilo o color. Se guardará una entrada vacía.', 'warning');
                    }

                    // Guardar estilos y colores en las listas
                    if (newStyle && !this.state.styles.includes(newStyle)) {
                        this.state.styles.push(newStyle);
                        this.state.styles.sort();
                        this.saveStyles();
                    }
                    if (newColor && !this.state.colors.includes(newColor)) {
                        this.state.colors.push(newColor);
                        this.state.colors.sort();
                        this.saveColors();
                    }

                    // Guardar estilos y colores seleccionados también
                    if (selectedStyle && !this.state.styles.includes(selectedStyle)) {
                        this.state.styles.push(selectedStyle);
                        this.state.styles.sort();
                        this.saveStyles();
                    }
                    if (selectedColor && !this.state.colors.includes(selectedColor)) {
                        this.state.colors.push(selectedColor);
                        this.state.colors.sort();
                        this.saveColors();
                    }

                    // Inicializar la estructura de datos si no existe
                    if (!this.state.productData[style]) {
                        this.state.productData[style] = {};
                    }
                    if (!this.state.productData[style][color]) {
                        this.state.productData[style][color] = {};
                    }
                    
                    // Guardar los SKUs
                    this.state.allSizes.forEach(size => {
                        const sku = document.getElementById(`sku_${size}`)?.value.trim() || '';
                        if (sku) {
                            this.state.productData[style][color][size] = sku;
                        } else {
                            // Si no hay SKU, eliminar la entrada para esa talla
                            delete this.state.productData[style][color][size];
                        }
                    });

                    this.saveProductData();
                    this.showNotification('SKUs guardados exitosamente.', 'success');
                    
                    // Actualizar los selects en otros modales
                    this.populateStyleSelect(this.DOM.orderStyleSelect);
                    this.populateColorSelect(this.DOM.orderColorSelect);
                    
                    this.closeSkuModal();
                    
                } catch (error) {
                    this.showNotification('Error al guardar los SKUs: ' + error.message, 'error');
                } finally {
                    this.hideLoading('saveSku');
                }
            }
        };

        // Inicializar la aplicación
        document.addEventListener('DOMContentLoaded', () => {
            app.init();
        });
    </script>
</body>
</html>
