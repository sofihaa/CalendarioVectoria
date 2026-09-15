<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vectoria — Panel de Gestión Editorial</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        body { 
            font-family: 'Space Grotesk', sans-serif; 
            background-color: #f3f0ea; 
            color: #111111;
        }
        .transition-all-custom { transition: all 0.2s ease-in-out; }
        .editorial-shadow { box-shadow: 0 4px 20px rgba(0, 0, 0, 0.04); }
    </style>
</head>
<body class="min-h-screen flex flex-col bg-[#f3f0ea]">

    <!-- VIEW 0: HOME / LANDING WELCOME SCREEN (Matches Figma #6 style) -->
    <div id="viewHome" class="fixed inset-0 bg-[#f3f0ea] z-50 flex items-center justify-center p-6">
        <div class="max-w-md w-full flex flex-col items-center animate-in fade-in duration-300">
            <!-- Purple Card from Reference -->
            <div class="bg-gradient-to-b from-violet-600 to-indigo-600 rounded-[32px] p-12 w-full text-center shadow-2xl relative flex flex-col items-center justify-center min-h-[260px]">
                <h1 class="text-4xl md:text-5xl font-black text-white tracking-tight">Vectoria</h1>
            </div>
            <!-- Bottom Floating Dock for Landing -->
            <div class="flex items-center justify-center gap-4 -mt-6 z-10 w-full">
                <button onclick="switchView('calendar')" class="bg-violet-600 hover:bg-violet-700 text-white w-14 h-12 rounded-2xl shadow-lg flex items-center justify-center transition hover:scale-105" title="Calendario">
                    <i class="fa-solid fa-calendar-days text-sm"></i>
                </button>
                <button onclick="toggleActionMenu()" class="bg-violet-600 hover:bg-violet-700 text-white w-14 h-12 rounded-2xl shadow-lg flex items-center justify-center transition hover:scale-105" title="Nuevo...">
                    <i class="fa-solid fa-plus text-sm"></i>
                </button>
                <button onclick="switchView('tasks')" class="bg-violet-600 hover:bg-violet-700 text-white w-14 h-12 rounded-2xl shadow-lg flex items-center justify-center transition hover:scale-105" title="Pendientes">
                    <i class="fa-solid fa-list-check text-sm"></i>
                </button>
            </div>
            <div class="mt-8">
                <button onclick="switchView('clients')" class="text-xs font-bold uppercase tracking-widest text-neutral-600 hover:text-black underline transition">
                    Ver CRM y Clientes
                </button>
            </div>
        </div>
    </div>

    <!-- MAIN SHELL WITH SIDEBAR -->
    <div class="flex-1 flex min-h-screen">
        
        <!-- SIDEBAR (Dark Rounded Container matching Figma Design) -->
        <aside class="w-64 bg-[#111111] text-white p-6 flex flex-col justify-between hidden md:flex rounded-r-[32px] my-4 ml-4 shadow-xl">
            <div class="space-y-8">
                <!-- Brand / Logo -->
                <div>
                    <h1 onclick="switchView('home')" class="text-3xl font-bold tracking-tight text-white cursor-pointer hover:opacity-90">Vectoria</h1>
                </div>

                <!-- Navigation Links / Filters -->
                <nav class="space-y-3">
                    <button onclick="switchView('clients')" id="nav-clients" class="w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-neutral-400 hover:text-white hover:bg-neutral-800 transition">
                        Clientes
                    </button>
                    <button onclick="switchView('calendar')" id="nav-calendar" class="w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-white bg-neutral-800 transition">
                        Calendario
                    </button>
                    <button onclick="switchView('tasks')" id="nav-tasks" class="w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-neutral-400 hover:text-white hover:bg-neutral-800 transition">
                        Pendientes / Tareas
                    </button>
                </nav>

                <!-- Category Filter Pills (Matches Figma Sidebar) -->
                <div class="space-y-2 pt-4 border-t border-neutral-800">
                    <span class="text-[10px] uppercase font-bold text-neutral-500 tracking-widest px-4 block">Filtros de Contenido</span>
                    <button onclick="filterContentType('Reels')" id="filter-Reels" class="w-full py-2.5 px-4 rounded-full text-xs font-bold text-neutral-900 bg-[#b2bc8e] hover:opacity-95 transition text-left flex items-center justify-between">
                        <span>Reels</span>
                        <span id="count-Reels" class="bg-white/40 px-2 py-0.5 rounded-full text-[10px]">0</span>
                    </button>
                    <button onclick="filterContentType('Posteos')" id="filter-Posteos" class="w-full py-2.5 px-4 rounded-full text-xs font-bold text-white bg-[#8299cc] hover:opacity-95 transition text-left flex items-center justify-between">
                        <span>Posteos</span>
                        <span id="count-Posteos" class="bg-black/20 px-2 py-0.5 rounded-full text-[10px]">0</span>
                    </button>
                    <button onclick="filterContentType('Grabaciones')" id="filter-Grabaciones" class="w-full py-2.5 px-4 rounded-full text-xs font-bold text-neutral-900 bg-[#f4b6d1] hover:opacity-95 transition text-left flex items-center justify-between">
                        <span>Grabaciones</span>
                        <span id="count-Grabaciones" class="bg-white/40 px-2 py-0.5 rounded-full text-[10px]">0</span>
                    </button>
                    <button onclick="filterContentType('Generales')" id="filter-Generales" class="w-full py-2.5 px-4 rounded-full text-xs font-bold text-neutral-900 bg-[#f6d76b] hover:opacity-95 transition text-left flex items-center justify-between">
                        <span>Generales</span>
                        <span id="count-Generales" class="bg-white/40 px-2 py-0.5 rounded-full text-[10px]">0</span>
                    </button>
                    <button onclick="filterContentType('All')" id="filter-All" class="w-full py-2 px-4 rounded-xl text-xs font-semibold text-neutral-400 hover:text-white transition text-left">
                        Mostrar Todos
                    </button>
                </div>
            </div>

            <!-- Footer links -->
            <div class="space-y-2 pt-4 border-t border-neutral-800 text-xs">
                <a href="#" onclick="switchView('clients'); return false;" class="block py-2 px-4 text-neutral-400 hover:text-white font-semibold">CALENDARIO CLIENTE</a>
                <a href="#" onclick="switchView('tasks'); return false;" class="block py-2 px-4 text-neutral-400 hover:text-white font-semibold">CALENDARIO GENERAL</a>
            </div>
        </aside>

        <!-- CONTENT AREA -->
        <div class="flex-1 flex flex-col min-w-0">
            
            <!-- HEADER -->
            <header class="p-6 md:p-8 flex items-center justify-between">
                <div>
                    <h2 class="text-3xl md:text-4xl font-black tracking-tighter text-black uppercase" id="viewTitle">Calendario</h2>
                    <p class="text-xs text-neutral-600 uppercase tracking-widest font-semibold mt-1" id="clientSubtitle">Óptica Lux — Paraná, Entre Ríos</p>
                </div>
                <div class="flex items-center space-x-3">
                    <a id="driveFolderBtn" href="https://drive.google.com/drive/folders/1placeholder" target="_blank" class="bg-white hover:bg-neutral-100 border border-neutral-300 text-black px-4 py-2 rounded-xl text-xs font-semibold shadow-sm flex items-center space-x-2 transition">
                        <i class="fa-brands fa-google-drive text-emerald-600"></i>
                        <span class="hidden sm:inline">Carpeta Drive</span>
                    </a>
                </div>
            </header>

            <!-- MAIN VIEW CONTAINER -->
            <main class="flex-1 px-4 md:px-8 pb-28 max-w-7xl w-full mx-auto">
                
                <!-- VIEW 1: CLIENTS CRM -->
                <div id="viewClients" class="hidden space-y-6">
                    <!-- Category Filter Chips -->
                    <div class="flex items-center gap-2 flex-wrap pb-2">
                        <button onclick="filterClients('All')" id="client-filter-All" class="px-4 py-2 rounded-full text-xs font-bold bg-black text-white transition">All</button>
                        <button onclick="filterClients('Óptica')" id="client-filter-Óptica" class="px-4 py-2 rounded-full text-xs font-semibold bg-white hover:bg-neutral-100 border border-neutral-300 text-neutral-800 transition">Óptica</button>
                        <button onclick="filterClients('Comercio')" id="client-filter-Comercio" class="px-4 py-2 rounded-full text-xs font-semibold bg-white hover:bg-neutral-100 border border-neutral-300 text-neutral-800 transition">Comercio</button>
                        <button onclick="filterClients('Comida')" id="client-filter-Comida" class="px-4 py-2 rounded-full text-xs font-semibold bg-white hover:bg-neutral-100 border border-neutral-300 text-neutral-800 transition">Comida</button>
                        <button onclick="filterClients('Inmobiliario')" id="client-filter-Inmobiliario" class="px-4 py-2 rounded-full text-xs font-semibold bg-white hover:bg-neutral-100 border border-neutral-300 text-neutral-800 transition">Inmobiliario</button>
                        <button onclick="filterClients('Interesados')" id="client-filter-Interesados" class="px-4 py-2 rounded-full text-xs font-semibold bg-white hover:bg-neutral-100 border border-neutral-300 text-neutral-800 transition">Interesados</button>
                    </div>

                    <!-- Client Cards Grid -->
                    <div id="clientCardsGrid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                        <!-- Populated by JS -->
                    </div>
                </div>

                <!-- VIEW 2: CALENDAR (Septiembre 2026) -->
                <div id="viewCalendar" class="space-y-6">
                    <!-- Top Bar with Month Switcher Pill -->
                    <div class="flex items-center justify-between flex-wrap gap-4">
                        <div class="bg-white rounded-2xl border border-[#e2ddd5] p-3 shadow-sm flex items-center space-x-3">
                            <span class="text-sm font-bold text-black pl-2" id="pickerMonthYearLabel">Septiembre 2026</span>
                            <div class="flex items-center space-x-1">
                                <button onclick="changeMonth(-1)" class="w-8 h-8 rounded-xl hover:bg-neutral-100 flex items-center justify-center text-neutral-600 transition">
                                    <i class="fa-solid fa-chevron-left text-xs"></i>
                                </button>
                                <button onclick="changeMonth(1)" class="w-8 h-8 rounded-xl hover:bg-neutral-100 flex items-center justify-center text-neutral-600 transition">
                                    <i class="fa-solid fa-chevron-right text-xs"></i>
                                </button>
                            </div>
                        </div>
                        <button onclick="goToSeptember2026()" class="px-4 py-2 bg-neutral-200 hover:bg-neutral-300 text-black text-xs font-bold rounded-xl transition">
                            Ir a Septiembre 2026
                        </button>
                    </div>

                    <!-- Calendar Table Grid -->
                    <div class="bg-white rounded-3xl border border-[#e2ddd5] editorial-shadow overflow-hidden flex flex-col">
                        <div class="grid grid-cols-7 bg-black text-white text-center text-xs font-bold tracking-wider py-3.5">
                            <div>LUN</div><div>MAR</div><div>MIÉ</div><div>JUE</div><div>VIE</div><div>SÁB</div><div>DOM</div>
                        </div>
                        <div id="calendarGrid" class="grid grid-cols-7 flex-1 auto-rows-fr bg-[#e2ddd5] gap-px">
                            <!-- Populated dynamically -->
                        </div>
                    </div>
                </div>

                <!-- VIEW 3: TASKS / PENDIENTES -->
                <div id="viewTasks" class="hidden space-y-6">
                    <div class="bg-white rounded-3xl border border-[#e2ddd5] p-6 editorial-shadow flex flex-col gap-4">
                        <div class="flex items-center justify-between border-b border-neutral-100 pb-4">
                            <div>
                                <h3 class="text-base font-bold text-black">Tareas Pendientes y Finalizadas (Vectoria)</h3>
                                <p class="text-xs text-neutral-500">Gestión interna de la agencia y entregables</p>
                            </div>
                            <button onclick="openModal('event')" class="bg-black text-white px-4 py-2 rounded-xl text-xs font-bold">
                                <i class="fa-solid fa-plus mr-1"></i> Nueva Tarea
                            </button>
                        </div>
                        <div id="fullTasksList" class="space-y-3">
                            <!-- Populated dynamically -->
                        </div>
                    </div>
                </div>

            </main>
        </div>
    </div>

    <!-- FLOATING DOCK (Matches Figma Bottom Menu with '+' popup trigger) -->
    <div class="fixed bottom-6 left-1/2 transform -translate-x-1/2 z-40">
        <div class="bg-[#111111] text-white px-6 py-3 rounded-full shadow-2xl flex items-center space-x-8 border border-neutral-800">
            <button onclick="switchView('calendar')" class="text-neutral-400 hover:text-white transition text-lg" title="Calendario">
                <i class="fa-solid fa-calendar-days"></i>
            </button>
            <button onclick="toggleActionMenu()" class="w-12 h-12 bg-[#f4b6d1] hover:opacity-90 text-black rounded-full flex items-center justify-center text-xl font-bold transition shadow-lg transform hover:scale-105" title="Nuevo...">
                <i class="fa-solid fa-plus" id="dockPlusIcon"></i>
            </button>
            <button onclick="switchView('tasks')" class="text-neutral-400 hover:text-white transition text-lg" title="Pendientes">
                <i class="fa-solid fa-list-check"></i>
            </button>
        </div>
    </div>

    <!-- POPUP ACTION MENU (Matches Figma #4 screen) -->
    <div id="actionMenuModal" class="fixed inset-0 bg-black/40 backdrop-blur-xs z-50 hidden flex items-center justify-center p-4">
        <div class="bg-[#9ba87d] rounded-3xl p-6 max-w-sm w-full shadow-2xl flex flex-col gap-3 relative animate-in fade-in zoom-in duration-150 border border-[#869466]">
            <button onclick="toggleActionMenu()" class="absolute -top-3 -right-3 w-8 h-8 bg-white text-black rounded-full shadow-md flex items-center justify-center font-bold hover:bg-neutral-100">
                <i class="fa-solid fa-xmark text-sm"></i>
            </button>
            <button onclick="openModal('client'); toggleActionMenu();" class="w-full py-3.5 px-5 bg-[#f3f0ea] hover:bg-white text-black font-bold rounded-full text-xs shadow-sm transition text-left flex items-center justify-between">
                <span>Nuevo cliente</span>
                <span class="w-3 h-3 rounded-full bg-white border border-neutral-400"></span>
            </button>
            <button onclick="openModal('event'); toggleActionMenu();" class="w-full py-3.5 px-5 bg-[#e4ecce] hover:bg-[#d8e3be] text-black font-bold rounded-full text-xs shadow-sm transition text-left flex items-center justify-between">
                <span>Nuevo evento</span>
                <span class="w-3 h-3 rounded-full bg-[#9ba87d]"></span>
            </button>
            <button onclick="openModal('video'); toggleActionMenu();" class="w-full py-3.5 px-5 bg-[#d2e0b5] hover:bg-[#c6d7a2] text-black font-bold rounded-full text-xs shadow-sm transition text-left flex items-center justify-between">
                <span>Nuevo video</span>
                <span class="w-3 h-3 rounded-full bg-[#8299cc]"></span>
            </button>
        </div>
    </div>

    <!-- UNIVERSAL MODAL FOR CREATING/EDITING -->
    <div id="universalModal" class="fixed inset-0 bg-black/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl shadow-2xl max-w-lg w-full overflow-hidden border border-[#e2ddd5] max-h-[90vh] flex flex-col">
            <div class="px-6 py-4 border-b border-neutral-100 flex items-center justify-between bg-[#f3f0ea]">
                <h3 id="modalHeaderTitle" class="font-bold text-black text-base">Crear Elemento</h3>
                <button onclick="closeModal()" class="text-neutral-500 hover:text-black p-1 rounded-lg">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>
            <div id="modalBodyContent" class="p-6 space-y-4 overflow-y-auto flex-1">
                <!-- Dynamically populated -->
            </div>
        </div>
    </div>

    <!-- TOAST -->
    <div id="toast" class="fixed bottom-20 right-5 bg-black text-white px-4 py-3 rounded-xl shadow-lg transform translate-y-20 opacity-0 transition-all duration-300 z-50 flex items-center space-x-2 text-sm">
        <i class="fa-solid fa-circle-check text-emerald-400"></i>
        <span id="toastMessage">Operación exitosa</span>
    </div>

    <script>
        let currentView = 'home';
        let currentClient = { name: 'Óptica Lux', type: 'Óptica', location: 'Paraná', driveUrl: 'https://drive.google.com/drive/folders/mock-optica-lux' };
        let currentDate = new Date(2026, 8, 15); // Septiembre 2026
        let activeContentTypeFilter = 'All';
        let activeClientCategoryFilter = 'All';

        let clientsList = [
            { id: '1', name: 'Óptica Lux', type: 'Óptica', location: 'Paraná', objective: 'Fortalecer presencia en redes sociales para atraer nuevos clientes y fidelizar a los actuales.', target: 'Personas desde 25 a 45 años, con interés en salud visual.', firstMonth: 'Septiembre 2026', driveUrl: 'https://drive.google.com/drive/folders/mock-optica-lux' }
        ];

        let eventsList = [
            { id: '1', day: 1, type: 'Reels', category: 'Generales', title: 'Tomas generales del local, mostrando ubicación y productos', client: 'Óptica Lux', status: 'Pendiente', link: 'https://www.instagram.com/reel/DFZzvjdsqf8/' },
            { id: '2', day: 5, type: 'Generales', category: 'Generales', title: 'Revisar pauta publicitaria en Meta Ads', client: 'Óptica Lux', status: 'Pendiente', link: '' },
            { id: '3', day: 11, type: 'Posteos', category: 'Generales', title: 'Foto de producto con datos generales y horarios', client: 'Óptica Lux', status: 'Pendiente', link: '' },
            { id: '4', day: 15, type: 'Generales', category: 'Generales', title: 'Reunión mensual de revisión de métricas y engagement', client: 'Óptica Lux', status: 'Pendiente', link: '' },
            { id: '5', day: 21, type: 'Reels', category: 'Generales', title: 'Video de manos mostrando armazones destacados', client: 'Óptica Lux', status: 'Pendiente', link: 'https://vt.tiktok.com/ZSqCy8cHE/' }
        ];

        const monthsNames = ["Enero", "Febrero", "Marzo", "Abril", "Mayo", "Junio", "Julio", "Agosto", "Septiembre", "Octubre", "Noviembre", "Diciembre"];

        window.onload = function() {
            const savedClients = localStorage.getItem('vectoria_clients');
            if(savedClients) { try { clientsList = JSON.parse(savedClients); } catch(e){} }
            const savedEvents = localStorage.getItem('vectoria_events');
            if(savedEvents) { try { eventsList = JSON.parse(savedEvents); } catch(e){} }

            applyClientSettings();
            renderCalendar();
            renderClients();
            renderTasks();
            updateSidebarCounts();
            switchView('home');
        };

        function saveToStorage() {
            localStorage.setItem('vectoria_clients', JSON.stringify(clientsList));
            localStorage.setItem('vectoria_events', JSON.stringify(eventsList));
            updateSidebarCounts();
        }

        function applyClientSettings() {
            document.getElementById('clientSubtitle').innerText = `${currentClient.name} — ${currentClient.location}`;
            document.getElementById('driveFolderBtn').href = currentClient.driveUrl;
        }

        function switchView(view) {
            currentView = view;
            document.getElementById('viewHome').classList.add('hidden');
            document.getElementById('viewClients').classList.add('hidden');
            document.getElementById('viewCalendar').classList.add('hidden');
            document.getElementById('viewTasks').classList.add('hidden');

            if(view === 'home') {
                document.getElementById('viewHome').classList.remove('hidden');
                return;
            }

            document.getElementById('nav-clients').className = "w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-neutral-400 hover:text-white hover:bg-neutral-800 transition";
            document.getElementById('nav-calendar').className = "w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-neutral-400 hover:text-white hover:bg-neutral-800 transition";
            document.getElementById('nav-tasks').className = "w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-neutral-400 hover:text-white hover:bg-neutral-800 transition";

            if(view === 'clients') {
                document.getElementById('viewClients').classList.remove('hidden');
                document.getElementById('nav-clients').className = "w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-white bg-neutral-800 transition";
                document.getElementById('viewTitle').innerText = "Clientes";
            } else if(view === 'calendar') {
                document.getElementById('viewCalendar').classList.remove('hidden');
                document.getElementById('nav-calendar').className = "w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-white bg-neutral-800 transition";
                document.getElementById('viewTitle').innerText = "Calendario";
            } else if(view === 'tasks') {
                document.getElementById('viewTasks').classList.remove('hidden');
                document.getElementById('nav-tasks').className = "w-full text-left py-2.5 px-4 rounded-xl text-xs font-bold uppercase tracking-widest text-white bg-neutral-800 transition";
                document.getElementById('viewTitle').innerText = "Pendientes";
                renderTasks();
            }
        }

        function toggleActionMenu() {
            const m = document.getElementById('actionMenuModal');
            m.classList.toggle('hidden');
        }

        function updateSidebarCounts() {
            document.getElementById('count-Reels').innerText = eventsList.filter(e => e.type === 'Reels').length;
            document.getElementById('count-Posteos').innerText = eventsList.filter(e => e.type === 'Posteos').length;
            document.getElementById('count-Grabaciones').innerText = eventsList.filter(e => e.type === 'Grabaciones').length;
            document.getElementById('count-Generales').innerText = eventsList.filter(e => e.type === 'Generales').length;
        }

        function filterContentType(type) {
            activeContentTypeFilter = type;
            renderCalendar();
            showToast(`Filtro: ${type}`);
        }

        function filterClients(cat) {
            activeClientCategoryFilter = cat;
            ['All', 'Óptica', 'Comercio', 'Comida', 'Inmobiliario', 'Interesados'].forEach(c => {
                const btn = document.getElementById(`client-filter-${c}`);
                if(btn) {
                    if(c === cat) btn.className = "px-4 py-2 rounded-full text-xs font-bold bg-black text-white transition";
                    else btn.className = "px-4 py-2 rounded-full text-xs font-semibold bg-white hover:bg-neutral-100 border border-neutral-300 text-neutral-800 transition";
                }
            });
            renderClients();
        }

        function renderClients() {
            const grid = document.getElementById('clientCardsGrid');
            grid.innerHTML = '';
            let list = [...clientsList];
            if(activeClientCategoryFilter !== 'All') {
                list = list.filter(c => c.type === activeClientCategoryFilter);
            }

            if(list.length === 0) {
                grid.innerHTML = `<div class="col-span-full text-center py-12 text-neutral-400 text-xs">No hay clientes en esta categoría</div>`;
                return;
            }

            list.forEach((c, idx) => {
                let card = document.createElement('div');
                let bgColors = ['bg-[#f6d76b]', 'bg-[#bcd2ed]', 'bg-[#f4b6d1]', 'bg-[#b2bc8e]'];
                let bgBg = bgColors[idx % bgColors.length];
                card.className = `${bgBg} rounded-3xl p-6 editorial-shadow flex flex-col justify-between gap-4 cursor-pointer hover:scale-[1.01] transition relative group`;
                
                card.onclick = () => {
                    currentClient = c;
                    applyClientSettings();
                    switchView('calendar');
                    showToast(`Cliente seleccionado: ${c.name}`);
                };

                card.innerHTML = `
                    <div class="flex items-center justify-between">
                        <div class="flex items-center space-x-3">
                            <div class="w-12 h-12 rounded-full bg-black/10 flex items-center justify-center font-bold text-base">
                                ${c.name.substring(0,2).toUpperCase()}
                            </div>
                            <div>
                                <div class="text-[10px] uppercase font-bold tracking-widest text-neutral-700">Cliente</div>
                                <h3 class="text-xl font-black text-black">${c.name}</h3>
                            </div>
                        </div>
                        <button onclick="deleteClient(event, '${c.id}')" class="w-8 h-8 rounded-full bg-white/60 hover:bg-rose-600 hover:text-white text-neutral-700 flex items-center justify-center transition shadow-xs" title="Eliminar cliente">
                            <i class="fa-solid fa-trash-can text-xs"></i>
                        </button>
                    </div>
                    <div class="space-y-2 text-xs text-neutral-800 border-t border-black/10 pt-4">
                        <div class="flex justify-between"><strong>Tipo:</strong> <span>${c.type}</span></div>
                        <div class="flex justify-between"><strong>Ubicación:</strong> <span>${c.location}</span></div>
                        <div><strong>Objetivo General:</strong> <p class="text-neutral-700 mt-1 line-clamp-2">${c.objective}</p></div>
                        <div class="flex justify-between pt-1"><strong>Primer mes:</strong> <span>${c.firstMonth}</span></div>
                    </div>
                `;
                grid.appendChild(card);
            });
        }

        function deleteClient(e, id) {
            e.stopPropagation();
            if(confirm('¿Estás segura de que deseas eliminar este cliente?')) {
                clientsList = clientsList.filter(c => c.id !== id);
                saveToStorage();
                renderClients();
                showToast('Cliente eliminado por completo');
            }
        }

        function changeMonth(dir) {
            currentDate.setMonth(currentDate.getMonth() + dir);
            renderCalendar();
        }

        function goToSeptember2026() {
            currentDate = new Date(2026, 8, 15);
            renderCalendar();
        }

        function renderCalendar() {
            const grid = document.getElementById('calendarGrid');
            const pickerLabel = document.getElementById('pickerMonthYearLabel');
            grid.innerHTML = '';

            const year = currentDate.getFullYear();
            const month = currentDate.getMonth();
            pickerLabel.innerText = `${monthsNames[month]} ${year}`;

            const jsFirstDay = new Date(year, month, 1).getDay();
            let startingSpaces = jsFirstDay === 0 ? 6 : jsFirstDay - 1;
            const totalDays = new Date(year, month + 1, 0).getDate();
            const prevTotalDays = new Date(year, month, 0).getDate();

            for (let i = startingSpaces - 1; i >= 0; i--) {
                let dayNum = prevTotalDays - i;
                let cell = createDayCell(dayNum, true, null);
                grid.appendChild(cell);
            }

            for (let day = 1; day <= totalDays; day++) {
                let cell = createDayCell(day, false, day);
                grid.appendChild(cell);
            }

            let totalCellsSoFar = startingSpaces + totalDays;
            let remainingCells = totalCellsSoFar % 7 === 0 ? 0 : 7 - (totalCellsSoFar % 7);
            for (let day = 1; day <= remainingCells; day++) {
                let cell = createDayCell(day, true, null);
                grid.appendChild(cell);
            }
        }

        function createDayCell(dayNumber, isInactive, dateDay = null) {
            let div = document.createElement('div');
            div.className = `bg-white p-2.5 flex flex-col transition min-h-[120px] ${isInactive ? 'bg-[#f7f5ef] text-neutral-400' : 'hover:bg-neutral-50 cursor-pointer'}`;
            
            if(!isInactive && dateDay !== null) {
                div.onclick = () => openModal('event', null, dateDay);
            }

            let headerDiv = document.createElement('div');
            headerDiv.className = "flex items-center justify-between mb-1.5";

            let span = document.createElement('span');
            span.className = `text-xs font-bold ${isInactive ? 'text-neutral-400' : 'text-neutral-900'}`;
            span.innerText = dayNumber;
            headerDiv.appendChild(span);
            div.appendChild(headerDiv);

            let eventsContainer = document.createElement('div');
            eventsContainer.className = "flex-1 flex flex-col gap-1 overflow-y-auto";

            if (!isInactive && dateDay !== null) {
                let dayEvents = eventsList.filter(ev => Number(ev.day) === Number(dateDay));
                if(activeContentTypeFilter !== 'All') {
                    dayEvents = dayEvents.filter(ev => ev.type === activeContentTypeFilter);
                }

                dayEvents.forEach(ev => {
                    let badge = document.createElement('div');
                    let bgCol = 'bg-black text-white';
                    if(ev.type === 'Reels') bgCol = 'bg-[#b2bc8e] text-black font-bold';
                    else if(ev.type === 'Posteos') bgCol = 'bg-[#8299cc] text-white font-bold';
                    else if(ev.type === 'Grabaciones') bgCol = 'bg-[#f4b6d1] text-black font-bold';
                    else if(ev.type === 'Generales') bgCol = 'bg-[#f6d76b] text-black font-bold';

                    badge.className = `text-[10px] px-2 py-1 rounded-full truncate shadow-xs flex items-center justify-between ${bgCol}`;
                    badge.innerHTML = `<span class="truncate">[${ev.type}] ${ev.title}</span>`;
                    badge.onclick = (e) => {
                        e.stopPropagation();
                        openModal('event', ev.id);
                    };
                    eventsContainer.appendChild(badge);
                });
            }

            div.appendChild(eventsContainer);
            return div;
        }

        function renderTasks() {
            const container = document.getElementById('fullTasksList');
            container.innerHTML = '';
            if(eventsList.length === 0) {
                container.innerHTML = `<div class="text-center py-8 text-neutral-400 text-xs">No hay tareas o pendientes</div>`;
                return;
            }

            eventsList.forEach(t => {
                let card = document.createElement('div');
                card.className = "p-4 bg-[#f9f8f6] hover:bg-neutral-100 rounded-2xl border border-[#e8e4dc] flex items-center justify-between transition";
                card.innerHTML = `
                    <div class="flex items-center space-x-3">
                        <div class="w-8 h-8 rounded-xl bg-black text-white flex items-center justify-center font-bold text-xs">
                            <i class="fa-solid fa-list-check"></i>
                        </div>
                        <div>
                            <div class="text-xs font-bold text-neutral-900">${t.title}</div>
                            <div class="text-[11px] text-neutral-500 mt-0.5">Día ${t.day} • Tipo: ${t.type} • Estado: ${t.status}</div>
                        </div>
                    </div>
                    <div class="flex items-center space-x-2">
                        <button onclick="openModal('event', '${t.id}')" class="px-3 py-1.5 bg-white border border-neutral-300 rounded-xl text-xs font-semibold hover:bg-neutral-50">Editar</button>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function openModal(mode, itemId = null, defaultDay = 1) {
            const modal = document.getElementById('universalModal');
            const title = document.getElementById('modalHeaderTitle');
            const body = document.getElementById('modalBodyContent');
            body.innerHTML = '';

            if(mode === 'client') {
                title.innerText = 'Nuevo Cliente';
                body.innerHTML = `
                    <form onsubmit="saveNewClient(event)" class="space-y-4">
                        <div>
                            <label class="block text-xs font-bold uppercase mb-1">Nombre del Cliente</label>
                            <input type="text" id="cliName" required class="w-full p-3 bg-neutral-50 border rounded-xl text-sm" placeholder="Ej. Óptica Central">
                        </div>
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-bold uppercase mb-1">Categoría</label>
                                <select id="cliType" class="w-full p-3 bg-neutral-50 border rounded-xl text-sm">
                                    <option value="Óptica">Óptica</option>
                                    <option value="Comercio">Comercio</option>
                                    <option value="Comida">Comida</option>
                                    <option value="Inmobiliario">Inmobiliario</option>
                                    <option value="Interesados">Interesados</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-bold uppercase mb-1">Ubicación</label>
                                <input type="text" id="cliLoc" required class="w-full p-3 bg-neutral-50 border rounded-xl text-sm" placeholder="Paraná">
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase mb-1">Objetivo General</label>
                            <textarea id="cliObj" rows="2" class="w-full p-3 bg-neutral-50 border rounded-xl text-sm" placeholder="Fortalecer presencia..."></textarea>
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase mb-1">Link Carpeta Google Drive</label>
                            <input type="url" id="cliDrive" class="w-full p-3 bg-neutral-50 border rounded-xl text-sm" placeholder="https://drive.google.com/...">
                        </div>
                        <div class="flex justify-end gap-2 pt-2">
                            <button type="button" onclick="closeModal()" class="px-4 py-2 border rounded-xl text-sm font-semibold">Cancelar</button>
                            <button type="submit" class="px-4 py-2 bg-black text-white rounded-xl text-sm font-semibold">Guardar Cliente</button>
                        </div>
                    </form>
                `;
            } else {
                title.innerText = itemId ? 'Editar Evento / Video' : 'Nuevo Evento / Video';
                let ev = itemId ? eventsList.find(e => e.id === itemId) : { day: defaultDay, type: 'Reels', category: 'Generales', title: '', status: 'Pendiente', link: '' };
                
                body.innerHTML = `
                    <form onsubmit="saveEventItem(event, '${itemId || ''}')" class="space-y-4">
                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-bold uppercase mb-1">Día del Mes</label>
                                <input type="number" id="evDay" min="1" max="31" value="${ev.day}" required class="w-full p-3 bg-neutral-50 border rounded-xl text-sm">
                            </div>
                            <div>
                                <label class="block text-xs font-bold uppercase mb-1">Tipo</label>
                                <select id="evType" class="w-full p-3 bg-neutral-50 border rounded-xl text-sm">
                                    <option value="Reels" ${ev.type==='Reels'?'selected':''}>Reels</option>
                                    <option value="Posteos" ${ev.type==='Posteos'?'selected':''}>Posteos</option>
                                    <option value="Grabaciones" ${ev.type==='Grabaciones'?'selected':''}>Grabaciones</option>
                                    <option value="Generales" ${ev.type==='Generales'?'selected':''}>Generales</option>
                                </select>
                            </div>
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase mb-1">Título / Tema</label>
                            <input type="text" id="evTitle" required value="${ev.title}" class="w-full p-3 bg-neutral-50 border rounded-xl text-sm" placeholder="Descripción del contenido...">
                        </div>
                        <div>
                            <label class="block text-xs font-bold uppercase mb-1">Link (Drive/TikTok/Canva)</label>
                            <input type="url" id="evLink" value="${ev.link || ''}" class="w-full p-3 bg-neutral-50 border rounded-xl text-sm" placeholder="https://...">
                        </div>
                        <div class="flex items-center justify-between pt-2">
                            ${itemId ? `<button type="button" onclick="deleteEventItem('${itemId}')" class="text-rose-600 font-bold text-sm">Eliminar</button>` : '<div></div>'}
                            <div class="flex gap-2">
                                <button type="button" onclick="closeModal()" class="px-4 py-2 border rounded-xl text-sm font-semibold">Cancelar</button>
                                <button type="submit" class="px-4 py-2 bg-black text-white rounded-xl text-sm font-semibold">Guardar</button>
                            </div>
                        </div>
                    </form>
                `;
            }
            modal.classList.remove('hidden');
        }

        function closeModal() { document.getElementById('universalModal').classList.add('hidden'); }

        function saveNewClient(e) {
            e.preventDefault();
            let newC = {
                id: Date.now().toString(),
                name: document.getElementById('cliName').value,
                type: document.getElementById('cliType').value,
                location: document.getElementById('cliLoc').value,
                objective: document.getElementById('cliObj').value,
                target: 'Público general',
                firstMonth: 'Septiembre 2026',
                driveUrl: document.getElementById('cliDrive').value || 'https://drive.google.com'
            };
            clientsList.push(newC);
            currentClient = newC;
            applyClientSettings();
            saveToStorage();
            closeModal();
            renderClients();
            showToast('Cliente creado');
        }

        function saveEventItem(e, id) {
            e.preventDefault();
            let day = parseInt(document.getElementById('evDay').value);
            let type = document.getElementById('evType').value;
            let title = document.getElementById('evTitle').value;
            let link = document.getElementById('evLink').value;

            if(id) {
                let ev = eventsList.find(x => x.id === id);
                if(ev) { ev.day = day; ev.type = type; ev.title = title; ev.link = link; }
                showToast('Actualizado');
            } else {
                eventsList.push({ id: Date.now().toString(), day, type, category: 'Generales', title, status: 'Pendiente', link });
                showToast('Creado');
            }
            saveToStorage();
            closeModal();
            renderCalendar();
            renderTasks();
        }

        function deleteEventItem(id) {
            eventsList = eventsList.filter(x => x.id !== id);
            saveToStorage();
            closeModal();
            renderCalendar();
            renderTasks();
            showToast('Eliminado');
        }

        function showToast(message) {
            const toast = document.getElementById('toast');
            document.getElementById('toastMessage').innerText = message;
            toast.classList.remove('translate-y-20', 'opacity-0');
            setTimeout(() => toast.classList.add('translate-y-20', 'opacity-0'), 3000);
        }
    </script>
</body>
</html>
