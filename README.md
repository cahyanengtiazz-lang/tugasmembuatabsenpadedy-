```html
<!DOCTYPE html>
<html lang="id" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Absensi Kelas Digital</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Lucide Icons CDN -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#f0f7ff',
                            100: '#e0effe',
                            500: '#0284c7',
                            600: '#0369a1',
                            700: '#075985',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body { font-family: 'Inter', sans-serif; }
        /* Custom scrollbar */
        ::-webkit-scrollbar { width: 6px; height: 6px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
    </style>
</head>
<body class="h-full flex flex-col text-slate-800 antialiased selection:bg-brand-500 selection:text-white">

    <!-- Top Navigation Header -->
    <header class="bg-white border-b border-slate-200 sticky top-0 z-30">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-16">
                <!-- Logo & Title -->
                <div class="flex items-center space-x-3">
                    <div class="bg-brand-600 text-white p-2.5 rounded-xl shadow-md shadow-brand-500/20">
                        <i data-lucide="clipboard-check" class="w-6 h-6"></i>
                    </div>
                    <div>
                        <h1 class="text-xl font-bold text-slate-900 tracking-tight leading-tight">AbsensiKelas<span class="text-brand-600">Pro</span></h1>
                        <p class="text-xs text-slate-500 font-medium">Sistem Pencatatan Kehadiran Siswa</p>
                    </div>
                </div>

                <!-- Navigation Tabs -->
                <nav class="hidden md:flex space-x-1 bg-slate-100 p-1 rounded-xl">
                    <button onclick="switchTab('absensi')" id="tab-btn-absensi" class="tab-btn active inline-flex items-center px-4 py-2 text-sm font-medium rounded-lg transition-all duration-150 bg-white text-slate-900 shadow-sm">
                        <i data-lucide="calendar-check" class="w-4 h-4 mr-2 text-brand-600"></i>
                        Absensi Hari Ini
                    </button>
                    <button onclick="switchTab('siswa')" id="tab-btn-siswa" class="tab-btn inline-flex items-center px-4 py-2 text-sm font-medium rounded-lg transition-all duration-150 text-slate-600 hover:text-slate-900 hover:bg-slate-200/60">
                        <i data-lucide="users" class="w-4 h-4 mr-2"></i>
                        Daftar Siswa
                    </button>
                    <button onclick="switchTab('rekap')" id="tab-btn-rekap" class="tab-btn inline-flex items-center px-4 py-2 text-sm font-medium rounded-lg transition-all duration-150 text-slate-600 hover:text-slate-900 hover:bg-slate-200/60">
                        <i data-lucide="bar-chart-3" class="w-4 h-4 mr-2"></i>
                        Rekap & Laporan
                    </button>
                </nav>

                <!-- Quick Actions -->
                <div class="flex items-center space-x-2">
                    <button onclick="exportDataToCSV()" class="inline-flex items-center px-3 py-2 border border-slate-300 text-xs sm:text-sm font-medium rounded-lg text-slate-700 bg-white hover:bg-slate-50 hover:border-slate-400 transition-colors shadow-sm">
                        <i data-lucide="download" class="w-4 h-4 sm:mr-1.5 text-slate-500"></i>
                        <span class="hidden sm:inline">Export CSV</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Navigation Bar -->
        <div class="md:hidden border-t border-slate-200 bg-slate-50 px-2 py-2 flex justify-around">
            <button onclick="switchTab('absensi')" id="mobile-tab-absensi" class="mobile-tab active flex flex-col items-center px-3 py-1 rounded-lg text-brand-600 font-medium text-xs">
                <i data-lucide="calendar-check" class="w-5 h-5 mb-1"></i>
                Absensi
            </button>
            <button onclick="switchTab('siswa')" id="mobile-tab-siswa" class="mobile-tab flex flex-col items-center px-3 py-1 rounded-lg text-slate-600 font-medium text-xs">
                <i data-lucide="users" class="w-5 h-5 mb-1"></i>
                Siswa
            </button>
            <button onclick="switchTab('rekap')" id="mobile-tab-rekap" class="mobile-tab flex flex-col items-center px-3 py-1 rounded-lg text-slate-600 font-medium text-xs">
                <i data-lucide="bar-chart-3" class="w-5 h-5 mb-1"></i>
                Rekap
            </button>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">

        <!-- Controls Header / Context Bar -->
        <div class="bg-white p-4 sm:p-5 rounded-2xl border border-slate-200/80 shadow-sm flex flex-col sm:flex-row gap-4 justify-between items-start sm:items-center">
            <div class="flex flex-wrap items-center gap-3 w-full sm:w-auto">
                <div>
                    <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">Tanggal Absensi</label>
                    <input type="date" id="attendance-date" onchange="onDateOrMetaChange()" class="bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-lg focus:ring-brand-500 focus:border-brand-500 block p-2.5 font-medium shadow-sm">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">Kelas</label>
                    <select id="class-select" onchange="onDateOrMetaChange()" class="bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-lg focus:ring-brand-500 focus:border-brand-500 block p-2.5 font-medium shadow-sm min-w-[130px]">
                        <option value="XII IPA 1">XII IPA 1</option>
                        <option value="XII IPA 2">XII IPA 2</option>
                        <option value="XII IPS 1">XII IPS 1</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-500 uppercase tracking-wider mb-1">Mata Pelajaran</label>
                    <input type="text" id="subject-input" value="Matematika" onchange="onDateOrMetaChange()" placeholder="Nama Mapel" class="bg-slate-50 border border-slate-300 text-slate-800 text-sm rounded-lg focus:ring-brand-500 focus:border-brand-500 block p-2.5 font-medium shadow-sm w-36 sm:w-44">
                </div>
            </div>

            <!-- Global Action Info -->
            <div class="flex items-center justify-between sm:justify-end w-full sm:w-auto gap-3 pt-2 sm:pt-0 border-t sm:border-t-0 border-slate-100">
                <span id="save-status-badge" class="inline-flex items-center px-2.5 py-1 rounded-full text-xs font-medium bg-emerald-100 text-emerald-800 border border-emerald-200">
                    <i data-lucide="check-circle-2" class="w-3.5 h-3.5 mr-1 text-emerald-600"></i> Terseimpan Otomatis
                </span>
                <button onclick="resetTodayAttendance()" class="text-xs text-rose-600 hover:text-rose-700 font-medium inline-flex items-center px-2.5 py-1.5 rounded-lg hover:bg-rose-50 transition-colors">
                    <i data-lucide="rotate-ccw" class="w-3.5 h-3.5 mr-1"></i> Reset Hari Ini
                </button>
            </div>
        </div>

        <!-- Statistics Summary Cards -->
        <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-5 gap-3 sm:gap-4">
            <!-- Total Siswa -->
            <div class="bg-white p-4 rounded-xl border border-slate-200/80 shadow-sm">
                <div class="flex items-center justify-between">
                    <span class="text-xs font-medium text-slate-500 uppercase">Total Siswa</span>
                    <div class="p-2 bg-slate-100 rounded-lg text-slate-600">
                        <i data-lucide="users" class="w-4 h-4"></i>
                    </div>
                </div>
                <p id="stat-total" class="text-2xl font-bold text-slate-800 mt-2">0</p>
                <p class="text-xs text-slate-400 mt-0.5">Siswa terdaftar</p>
            </div>

            <!-- Hadir -->
            <div class="bg-white p-4 rounded-xl border border-emerald-200/80 shadow-sm bg-gradient-to-br from-emerald-50/30 to-white">
                <div class="flex items-center justify-between">
                    <span class="text-xs font-semibold text-emerald-700 uppercase">Hadir</span>
                    <div class="p-2 bg-emerald-100 rounded-lg text-emerald-700">
                        <i data-lucide="user-check" class="w-4 h-4"></i>
                    </div>
                </div>
                <div class="flex items-baseline justify-between mt-2">
                    <p id="stat-hadir" class="text-2xl font-bold text-emerald-700">0</p>
                    <span id="stat-hadir-percent" class="text-xs font-semibold text-emerald-600">0%</span>
                </div>
                <p class="text-xs text-emerald-600/80 mt-0.5">Mengikuti kelas</p>
            </div>

            <!-- Izin -->
            <div class="bg-white p-4 rounded-xl border border-amber-200/80 shadow-sm bg-gradient-to-br from-amber-50/30 to-white">
                <div class="flex items-center justify-between">
                    <span class="text-xs font-semibold text-amber-700 uppercase">Izin</span>
                    <div class="p-2 bg-amber-100 rounded-lg text-amber-700">
                        <i data-lucide="file-text" class="w-4 h-4"></i>
                    </div>
                </div>
                <div class="flex items-baseline justify-between mt-2">
                    <p id="stat-izin" class="text-2xl font-bold text-amber-700">0</p>
                    <span id="stat-izin-percent" class="text-xs font-semibold text-amber-600">0%</span>
                </div>
                <p class="text-xs text-amber-600/80 mt-0.5">Dengan keterangan</p>
            </div>

            <!-- Sakit -->
            <div class="bg-white p-4 rounded-xl border border-blue-200/80 shadow-sm bg-gradient-to-br from-blue-50/30 to-white">
                <div class="flex items-center justify-between">
                    <span class="text-xs font-semibold text-blue-700 uppercase">Sakit</span>
                    <div class="p-2 bg-blue-100 rounded-lg text-blue-700">
                        <i data-lucide="stethoscope" class="w-4 h-4"></i>
                    </div>
                </div>
                <div class="flex items-baseline justify-between mt-2">
                    <p id="stat-sakit" class="text-2xl font-bold text-blue-700">0</p>
                    <span id="stat-sakit-percent" class="text-xs font-semibold text-blue-600">0%</span>
                </div>
                <p class="text-xs text-blue-600/80 mt-0.5">Kondisi kesehatan</p>
            </div>

            <!-- Alpa -->
            <div class="bg-white p-4 rounded-xl border border-rose-200/80 shadow-sm bg-gradient-to-br from-rose-50/30 to-white col-span-2 sm:col-span-1">
                <div class="flex items-center justify-between">
                    <span class="text-xs font-semibold text-rose-700 uppercase">Alpa</span>
                    <div class="p-2 bg-rose-100 rounded-lg text-rose-700">
                        <i data-lucide="user-x" class="w-4 h-4"></i>
                    </div>
                </div>
                <div class="flex items-baseline justify-between mt-2">
                    <p id="stat-alpa" class="text-2xl font-bold text-rose-700">0</p>
                    <span id="stat-alpa-percent" class="text-xs font-semibold text-rose-600">0%</span>
                </div>
                <p class="text-xs text-rose-600/80 mt-0.5">Tanpa keterangan</p>
            </div>
        </div>

        <!-- TAB 1: ABSENSI HARI INI -->
        <section id="tab-content-absensi" class="space-y-4">
            <!-- Table Toolbar -->
            <div class="flex flex-col sm:flex-row justify-between items-stretch sm:items-center gap-3 bg-white p-3.5 rounded-xl border border-slate-200/80 shadow-sm">
                <!-- Search input -->
                <div class="relative flex-1 max-w-md">
                    <i data-lucide="search" class="w-4 h-4 absolute left-3 top-1/2 -translate-y-1/2 text-slate-400"></i>
                    <input type="text" id="attendance-search" onkeyup="renderAttendanceList()" placeholder="Cari nama atau NIS siswa..." class="w-full pl-9 pr-4 py-2 bg-slate-50 border border-slate-200 rounded-lg text-sm text-slate-800 focus:bg-white focus:ring-2 focus:ring-brand-500 focus:border-brand-500 transition-all outline-none">
                </div>

                <!-- Batch Actions -->
                <div class="flex items-center justify-between sm:justify-end gap-2">
                    <span class="text-xs text-slate-500 font-medium hidden lg:inline">Aksi Cepat:</span>
                    <button onclick="markAllStatus('Hadir')" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-emerald-100 text-emerald-800 hover:bg-emerald-200 transition-colors flex items-center">
                        <i data-lucide="check" class="w-3.5 h-3.5 mr-1"></i> Semua Hadir
                    </button>
                    <button onclick="markAllStatus('Alpa')" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-rose-100 text-rose-800 hover:bg-rose-200 transition-colors flex items-center">
                        <i data-lucide="x" class="w-3.5 h-3.5 mr-1"></i> Semua Alpa
                    </button>
                </div>
            </div>

            <!-- Attendance Table Container -->
            <div class="bg-white rounded-2xl border border-slate-200/80 shadow-sm overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50/80 border-b border-slate-200 text-xs font-semibold text-slate-500 uppercase tracking-wider">
                                <th class="py-3.5 px-4 w-12 text-center">No</th>
                                <th class="py-3.5 px-4">Siswa</th>
                                <th class="py-3.5 px-4 text-center">Status Kehadiran</th>
                                <th class="py-3.5 px-4 hidden sm:table-cell">Catatan / Keterangan</th>
                            </tr>
                        </thead>
                        <tbody id="attendance-tbody" class="divide-y divide-slate-100 text-sm">
                            <!-- Dynamically populated rows -->
                        </tbody>
                    </table>
                </div>

                <!-- Empty State -->
                <div id="attendance-empty" class="hidden py-12 text-center">
                    <i data-lucide="user-minus" class="w-12 h-12 text-slate-300 mx-auto mb-3"></i>
                    <p class="text-slate-600 font-medium">Tidak ada siswa ditemukan.</p>
                    <p class="text-xs text-slate-400 mt-1">Coba sesuaikan kata kunci pencarian atau tambah siswa baru.</p>
                </div>
            </div>
        </section>

        <!-- TAB 2: DAFTAR SISWA MANAGEMENT -->
        <section id="tab-content-siswa" class="space-y-4 hidden">
            <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-4 rounded-2xl border border-slate-200/80 shadow-sm">
                <div>
                    <h2 class="text-lg font-bold text-slate-900">Manajemen Roster Siswa</h2>
                    <p class="text-xs text-slate-500">Kelola daftar nama dan NIS siswa untuk kelas terpilih.</p>
                </div>
                <div class="flex items-center gap-2 w-full sm:w-auto">
                    <button onclick="openAddStudentModal()" class="w-full sm:w-auto px-4 py-2 bg-brand-600 hover:bg-brand-700 text-white font-medium text-sm rounded-xl transition-all shadow-md shadow-brand-500/20 inline-flex items-center justify-center">
                        <i data-lucide="user-plus" class="w-4 h-4 mr-2"></i> Tambah Siswa Baru
                    </button>
                </div>
            </div>

            <!-- Student List Table -->
            <div class="bg-white rounded-2xl border border-slate-200/80 shadow-sm overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50/80 border-b border-slate-200 text-xs font-semibold text-slate-500 uppercase tracking-wider">
                                <th class="py-3.5 px-4 w-12 text-center">No</th>
                                <th class="py-3.5 px-4">NIS</th>
                                <th class="py-3.5 px-4">Nama Lengkap</th>
                                <th class="py-3.5 px-4">Jenis Kelamin</th>
                                <th class="py-3.5 px-4">Kelas</th>
                                <th class="py-3.5 px-4 text-right">Aksi</th>
                            </tr>
                        </thead>
                        <tbody id="student-tbody" class="divide-y divide-slate-100 text-sm">
                            <!-- Populated via JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

        <!-- TAB 3: REKAP & LAPORAN -->
        <section id="tab-content-rekap" class="space-y-4 hidden">
            <div class="bg-white p-5 rounded-2xl border border-slate-200/80 shadow-sm">
                <div class="flex flex-col md:flex-row justify-between items-start md:items-center gap-4 mb-6">
                    <div>
                        <h2 class="text-lg font-bold text-slate-900">Rekapitulasi Kehadiran Kelas</h2>
                        <p class="text-xs text-slate-500">Ringkasan akumulasi kehadiran per siswa dari seluruh tanggal yang tersimpan.</p>
                    </div>
                    <button onclick="exportDataToCSV()" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white text-sm font-medium rounded-xl transition-all shadow-md shadow-emerald-600/20 inline-flex items-center">
                        <i data-lucide="file-spreadsheet" class="w-4 h-4 mr-2"></i> Download Laporan Excel (CSV)
                    </button>
                </div>

                <!-- Recap Table -->
                <div class="overflow-x-auto rounded-xl border border-slate-200">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-slate-50 text-xs font-semibold text-slate-600 uppercase border-b border-slate-200">
                                <th class="py-3 px-4 w-12 text-center">No</th>
                                <th class="py-3 px-4">Nama Siswa</th>
                                <th class="py-3 px-4 text-center text-emerald-700 bg-emerald-50/50">Hadir</th>
                                <th class="py-3 px-4 text-center text-amber-700 bg-amber-50/50">Izin</th>
                                <th class="py-3 px-4 text-center text-blue-700 bg-blue-50/50">Sakit</th>
                                <th class="py-3 px-4 text-center text-rose-700 bg-rose-50/50">Alpa</th>
                                <th class="py-3 px-4 text-center">Persentase</th>
                            </tr>
                        </thead>
                        <tbody id="recap-tbody" class="divide-y divide-slate-100 text-sm">
                            <!-- Populated via JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </section>

    </main>

    <!-- MODAL: Tambah/Edit Siswa -->
    <div id="student-modal" class="fixed inset-0 z-50 bg-slate-900/40 backdrop-blur-sm flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-2xl max-w-md w-full p-6 shadow-2xl border border-slate-100 transform transition-all">
            <div class="flex justify-between items-center mb-4 border-b border-slate-100 pb-3">
                <h3 id="modal-title" class="text-lg font-bold text-slate-900">Tambah Siswa Baru</h3>
                <button onclick="closeStudentModal()" class="text-slate-400 hover:text-slate-600 p-1 rounded-lg">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            
            <form id="student-form" onsubmit="handleSaveStudent(event)" class="space-y-4">
                <input type="hidden" id="student-id">
                <div>
                    <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Nomor Induk Siswa (NIS)</label>
                    <input type="text" id="modal-nis" required placeholder="Contoh: 2026001" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-brand-500 focus:bg-white outline-none">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Nama Lengkap Siswa</label>
                    <input type="text" id="modal-name" required placeholder="Contoh: Ahmad Subagja" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-brand-500 focus:bg-white outline-none">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Jenis Kelamin</label>
                    <select id="modal-gender" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-brand-500 focus:bg-white outline-none">
                        <option value="L">Laki-laki (L)</option>
                        <option value="P">Perempuan (P)</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-600 uppercase mb-1">Kelas Target</label>
                    <select id="modal-class" class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-300 rounded-xl text-sm focus:ring-2 focus:ring-brand-500 focus:bg-white outline-none">
                        <option value="XII IPA 1">XII IPA 1</option>
                        <option value="XII IPA 2">XII IPA 2</option>
                        <option value="XII IPS 1">XII IPS 1</option>
                    </select>
                </div>

                <div class="flex justify-end gap-2 pt-4 border-t border-slate-100">
                    <button type="button" onclick="closeStudentModal()" class="px-4 py-2 border border-slate-300 text-slate-700 text-sm font-medium rounded-xl hover:bg-slate-50">
                        Batal
                    </button>
                    <button type="submit" class="px-4 py-2 bg-brand-600 hover:bg-brand-700 text-white text-sm font-medium rounded-xl shadow-md shadow-brand-500/20">
                        Simpan Data
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL CONFIRMATION (Custom, NO alert/confirm used) -->
    <div id="confirm-modal" class="fixed inset-0 z-50 bg-slate-900/40 backdrop-blur-sm flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-2xl max-w-sm w-full p-6 shadow-2xl border border-slate-100 text-center">
            <div id="confirm-icon-container" class="w-12 h-12 bg-rose-100 text-rose-600 rounded-full flex items-center justify-center mx-auto mb-3">
                <i data-lucide="alert-triangle" class="w-6 h-6"></i>
            </div>
            <h3 id="confirm-title" class="text-base font-bold text-slate-900">Konfirmasi Aksi</h3>
            <p id="confirm-message" class="text-xs text-slate-500 mt-1 mb-5">Apakah Anda yakin ingin melanjutkan?</p>
            
            <div class="flex justify-center gap-3">
                <button id="confirm-cancel-btn" class="w-full py-2 border border-slate-300 text-slate-700 text-sm font-medium rounded-xl hover:bg-slate-50">
                    Batal
                </button>
                <button id="confirm-ok-btn" class="w-full py-2 bg-rose-600 hover:bg-rose-700 text-white text-sm font-medium rounded-xl shadow-md shadow-rose-600/20">
                    Ya, Lanjutkan
                </button>
            </div>
        </div>
    </div>

    <!-- TOAST NOTIFICATION -->
    <div id="toast" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 ease-out pointer-events-none">
        <div class="bg-slate-900 text-white px-4 py-3 rounded-xl shadow-xl flex items-center space-x-3 border border-slate-800">
            <div id="toast-icon" class="text-emerald-400">
                <i data-lucide="check-circle" class="w-5 h-5"></i>
            </div>
            <span id="toast-message" class="text-sm font-medium">Pesan notifikasi</span>
        </div>
    </div>

    <script>
        // Default Sample Initial Data
        const DEFAULT_STUDENTS = [
            { id: "1001", nis: "202601", name: "Aditya Pratama", gender: "L", class: "XII IPA 1" },
            { id: "1002", nis: "202602", name: "Bunga Citra Lestari", gender: "P", class: "XII IPA 1" },
            { id: "1003", nis: "202603", name: "Deni Kurniawan", gender: "L", class: "XII IPA 1" },
            { id: "1004", nis: "202604", name: "Eka Putri Melati", gender: "P", class: "XII IPA 1" },
            { id: "1005", nis: "202605", name: "Faris Al-Fatih", gender: "L", class: "XII IPA 1" },
            { id: "1006", nis: "202606", name: "Gita Gutawa", gender: "P", class: "XII IPA 1" },
            { id: "1007", nis: "202607", name: "Hendra Wijaya", gender: "L", class: "XII IPA 1" },
            { id: "1008", nis: "202608", name: "Indah Permatasari", gender: "P", class: "XII IPA 1" }
        ];

        // State variables
        let students = JSON.parse(localStorage.getItem('absensi_students')) || DEFAULT_STUDENTS;
        let attendanceDB = JSON.parse(localStorage.getItem('absensi_records')) || {};

        // On Page Load Initialization
        window.addEventListener('DOMContentLoaded', () => {
            // Set current date input default to today
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('attendance-date').value = today;

            // Load saved roster or default
            saveStudentsToStorage();
            
            // Render Initial Views
            renderAttendanceList();
            renderStudentTable();
            renderRecapTable();

            // Initialize Lucide icons
            lucide.createIcons();
        });

        function saveStudentsToStorage() {
            localStorage.setItem('absensi_students', JSON.stringify(students));
        }

        function saveAttendanceDBToStorage() {
            localStorage.setItem('absensi_records', JSON.stringify(attendanceDB));
            showToast("Perubahan absensi disimpan", "success");
        }

        function getCurrentKey() {
            const date = document.getElementById('attendance-date').value;
            const cls = document.getElementById('class-select').value;
            return `${cls}__${date}`;
        }

        function onDateOrMetaChange() {
            renderAttendanceList();
            renderRecapTable();
        }

        function renderAttendanceList() {
            const key = getCurrentKey();
            const cls = document.getElementById('class-select').value;
            const searchQuery = document.getElementById('attendance-search').value.toLowerCase();
            
            // Filter students by selected class
            let filteredStudents = students.filter(s => s.class === cls);

            // Apply search
            if (searchQuery) {
                filteredStudents = filteredStudents.filter(s => 
                    s.name.toLowerCase().includes(searchQuery) || 
                    s.nis.toLowerCase().includes(searchQuery)
                );
            }

            const tbody = document.getElementById('attendance-tbody');
            const emptyEl = document.getElementById('attendance-empty');
            tbody.innerHTML = '';

            if (filteredStudents.length === 0) {
                emptyEl.classList.remove('hidden');
                updateStats(0, 0, 0, 0, 0);
                return;
            } else {
                emptyEl.classList.add('hidden');
            }

            // Ensure attendance record exists for this key
            if (!attendanceDB[key]) {
                attendanceDB[key] = {};
            }

            let countHadir = 0, countIzin = 0, countSakit = 0, countAlpa = 0;

            filteredStudents.forEach((student, index) => {
                // Default status is 'Hadir' if not set
                if (!attendanceDB[key][student.id]) {
                    attendanceDB[key][student.id] = { status: 'Hadir', note: '' };
                }

                const rec = attendanceDB[key][student.id];
                const status = rec.status || 'Hadir';
                const note = rec.note || '';

                // Increment stats counters
                if (status === 'Hadir') countHadir++;
                else if (status === 'Izin') countIzin++;
                else if (status === 'Sakit') countSakit++;
                else if (status === 'Alpa') countAlpa++;

                const row = document.createElement('tr');
                row.className = "hover:bg-slate-50/70 transition-colors";

                row.innerHTML = `
                    <td class="py-3.5 px-4 text-center text-xs font-semibold text-slate-400">${index + 1}</td>
                    <td class="py-3.5 px-4">
                        <div class="font-bold text-slate-900">${escapeHtml(student.name)}</div>
                        <div class="text-xs text-slate-400 font-mono">NIS: ${escapeHtml(student.nis)} • ${student.gender === 'L' ? 'Laki-laki' : 'Perempuan'}</div>
                    </td>
                    <td class="py-3.5 px-4 text-center">
                        <div class="inline-flex rounded-xl bg-slate-100 p-1 gap-1 border border-slate-200">
                            <button onclick="setStatus('${student.id}', 'Hadir')" class="px-2.5 py-1 text-xs font-semibold rounded-lg transition-all ${status === 'Hadir' ? 'bg-emerald-600 text-white shadow-sm' : 'text-slate-600 hover:text-slate-900 hover:bg-slate-200/50'}">
                                Hadir
                            </button>
                            <button onclick="setStatus('${student.id}', 'Izin')" class="px-2.5 py-1 text-xs font-semibold rounded-lg transition-all ${status === 'Izin' ? 'bg-amber-500 text-white shadow-sm' : 'text-slate-600 hover:text-slate-900 hover:bg-slate-200/50'}">
                                Izin
                            </button>
                            <button onclick="setStatus('${student.id}', 'Sakit')" class="px-2.5 py-1 text-xs font-semibold rounded-lg transition-all ${status === 'Sakit' ? 'bg-blue-600 text-white shadow-sm' : 'text-slate-600 hover:text-slate-900 hover:bg-slate-200/50'}">
                                Sakit
                            </button>
                            <button onclick="setStatus('${student.id}', 'Alpa')" class="px-2.5 py-1 text-xs font-semibold rounded-lg transition-all ${status === 'Alpa' ? 'bg-rose-600 text-white shadow-sm' : 'text-slate-600 hover:text-slate-900 hover:bg-slate-200/50'}">
                                Alpa
                            </button>
                        </div>
                    </td>
                    <td class="py-3.5 px-4 hidden sm:table-cell">
                        <input type="text" value="${escapeHtml(note)}" onchange="setNote('${student.id}', this.value)" placeholder="Keterangan (opsional)..." class="w-full bg-slate-50 hover:bg-white focus:bg-white border border-slate-200 focus:border-brand-500 rounded-lg px-2.5 py-1 text-xs text-slate-700 transition-all outline-none">
                    </td>
                `;
                tbody.appendChild(row);
            });

            updateStats(filteredStudents.length, countHadir, countIzin, countSakit, countAlpa);
            lucide.createIcons();
        }

        function updateStats(total, hadir, izin, sakit, alpa) {
            document.getElementById('stat-total').innerText = total;
            document.getElementById('stat-hadir').innerText = hadir;
            document.getElementById('stat-izin').innerText = izin;
            document.getElementById('stat-sakit').innerText = sakit;
            document.getElementById('stat-alpa').innerText = alpa;

            const calcPct = (val) => total > 0 ? Math.round((val / total) * 100) + '%' : '0%';
            document.getElementById('stat-hadir-percent').innerText = calcPct(hadir);
            document.getElementById('stat-izin-percent').innerText = calcPct(izin);
            document.getElementById('stat-sakit-percent').innerText = calcPct(sakit);
            document.getElementById('stat-alpa-percent').innerText = calcPct(alpa);
        }

        function setStatus(studentId, newStatus) {
            const key = getCurrentKey();
            if (!attendanceDB[key]) attendanceDB[key] = {};
            if (!attendanceDB[key][studentId]) attendanceDB[key][studentId] = { status: 'Hadir', note: '' };

            attendanceDB[key][studentId].status = newStatus;
            saveAttendanceDBToStorage();
            renderAttendanceList();
            renderRecapTable();
        }

        function setNote(studentId, noteText) {
            const key = getCurrentKey();
            if (!attendanceDB[key]) attendanceDB[key] = {};
            if (!attendanceDB[key][studentId]) attendanceDB[key][studentId] = { status: 'Hadir', note: '' };

            attendanceDB[key][studentId].note = noteText;
            saveAttendanceDBToStorage();
        }

        function markAllStatus(status) {
            const key = getCurrentKey();
            const cls = document.getElementById('class-select').value;
            const classStudents = students.filter(s => s.class === cls);

            if (!attendanceDB[key]) attendanceDB[key] = {};
            
            classStudents.forEach(s => {
                if (!attendanceDB[key][s.id]) {
                    attendanceDB[key][s.id] = { status: status, note: '' };
                } else {
                    attendanceDB[key][s.id].status = status;
                }
            });

            saveAttendanceDBToStorage();
            renderAttendanceList();
            renderRecapTable();
            showToast(`Semua siswa ditandai ${status}`, "info");
        }

        function resetTodayAttendance() {
            showConfirmModal(
                "Reset Absensi Hari Ini",
                "Semua data kehadiran untuk kelas dan tanggal ini akan dikembalikan ke status awal.",
                () => {
                    const key = getCurrentKey();
                    delete attendanceDB[key];
                    saveAttendanceDBToStorage();
                    renderAttendanceList();
                    renderRecapTable();
                    showToast("Absensi hari ini berhasil direset", "info");
                }
            );
        }

        function renderStudentTable() {
            const tbody = document.getElementById('student-tbody');
            tbody.innerHTML = '';

            students.forEach((s, idx) => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50/70 transition-colors";
                tr.innerHTML = `
                    <td class="py-3 px-4 text-center text-xs text-slate-400 font-semibold">${idx + 1}</td>
                    <td class="py-3 px-4 font-mono text-xs text-slate-600">${escapeHtml(s.nis)}</td>
                    <td class="py-3 px-4 font-bold text-slate-900">${escapeHtml(s.name)}</td>
                    <td class="py-3 px-4">
                        <span class="inline-flex items-center px-2 py-0.5 rounded text-xs font-medium ${s.gender === 'L' ? 'bg-blue-100 text-blue-800' : 'bg-pink-100 text-pink-800'}">
                            ${s.gender === 'L' ? 'Laki-laki' : 'Perempuan'}
                        </span>
                    </td>
                    <td class="py-3 px-4 text-xs font-medium text-slate-600">${escapeHtml(s.class)}</td>
                    <td class="py-3 px-4 text-right">
                        <div class="flex items-center justify-end space-x-1">
                            <button onclick="editStudent('${s.id}')" class="p-1.5 text-slate-500 hover:text-brand-600 hover:bg-slate-100 rounded-lg transition-colors">
                                <i data-lucide="edit-3" class="w-4 h-4"></i>
                            </button>
                            <button onclick="deleteStudent('${s.id}')" class="p-1.5 text-slate-500 hover:text-rose-600 hover:bg-rose-50 rounded-lg transition-colors">
                                <i data-lucide="trash-2" class="w-4 h-4"></i>
                            </button>
                        </div>
                    </td>
                `;
                tbody.appendChild(tr);
            });

            lucide.createIcons();
        }

        function renderRecapTable() {
            const cls = document.getElementById('class-select').value;
            const classStudents = students.filter(s => s.class === cls);
            const tbody = document.getElementById('recap-tbody');
            tbody.innerHTML = '';

            classStudents.forEach((s, idx) => {
                let h = 0, i = 0, sk = 0, a = 0;

                // Loop over all saved dates for this class
                Object.keys(attendanceDB).forEach(key => {
                    if (key.startsWith(cls + '__')) {
                        const rec = attendanceDB[key][s.id];
                        if (rec) {
                            if (rec.status === 'Hadir') h++;
                            else if (rec.status === 'Izin') i++;
                            else if (rec.status === 'Sakit') sk++;
                            else if (rec.status === 'Alpa') a++;
                        }
                    }
                });

                const totalSessions = h + i + sk + a;
                const percentage = totalSessions > 0 ? Math.round((h / totalSessions) * 100) : 100;

                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50/70 transition-colors";
                tr.innerHTML = `
                    <td class="py-3 px-4 text-center text-xs text-slate-400 font-semibold">${idx + 1}</td>
                    <td class="py-3 px-4 font-bold text-slate-900">${escapeHtml(s.name)}</td>
                    <td class="py-3 px-4 text-center font-bold text-emerald-700 bg-emerald-50/30">${h}</td>
                    <td class="py-3 px-4 text-center font-bold text-amber-700 bg-amber-50/30">${i}</td>
                    <td class="py-3 px-4 text-center font-bold text-blue-700 bg-blue-50/30">${sk}</td>
                    <td class="py-3 px-4 text-center font-bold text-rose-700 bg-rose-50/30">${a}</td>
                    <td class="py-3 px-4 text-center">
                        <div class="flex items-center justify-center gap-2">
                            <div class="w-16 bg-slate-100 rounded-full h-2 overflow-hidden hidden sm:block">
                                <div class="bg-brand-600 h-2 rounded-full" style="width: ${percentage}%"></div>
                            </div>
                            <span class="text-xs font-bold text-slate-700">${percentage}%</span>
                        </div>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function openAddStudentModal() {
            document.getElementById('modal-title').innerText = "Tambah Siswa Baru";
            document.getElementById('student-id').value = "";
            document.getElementById('modal-nis').value = "";
            document.getElementById('modal-name').value = "";
            document.getElementById('modal-gender').value = "L";
            document.getElementById('modal-class').value = document.getElementById('class-select').value;
            
            document.getElementById('student-modal').classList.remove('hidden');
        }

        function editStudent(id) {
            const student = students.find(s => s.id === id);
            if (!student) return;

            document.getElementById('modal-title').innerText = "Edit Data Siswa";
            document.getElementById('student-id').value = student.id;
            document.getElementById('modal-nis').value = student.nis;
            document.getElementById('modal-name').value = student.name;
            document.getElementById('modal-gender').value = student.gender;
            document.getElementById('modal-class').value = student.class;

            document.getElementById('student-modal').classList.remove('hidden');
        }

        function closeStudentModal() {
            document.getElementById('student-modal').classList.add('hidden');
        }

        function handleSaveStudent(event) {
            event.preventDefault();
            const id = document.getElementById('student-id').value;
            const nis = document.getElementById('modal-nis').value.trim();
            const name = document.getElementById('modal-name').value.trim();
            const gender = document.getElementById('modal-gender').value;
            const cls = document.getElementById('modal-class').value;

            if (id) {
                // Update
                const index = students.findIndex(s => s.id === id);
                if (index !== -1) {
                    students[index] = { id, nis, name, gender, class: cls };
                    showToast("Data siswa berhasil diperbarui", "success");
                }
            } else {
                // Add New
                const newId = Date.now().toString();
                students.push({ id: newId, nis, name, gender, class: cls });
                showToast("Siswa baru berhasil ditambahkan", "success");
            }

            saveStudentsToStorage();
            renderStudentTable();
            renderAttendanceList();
            renderRecapTable();
            closeStudentModal();
        }

        function deleteStudent(id) {
            const student = students.find(s => s.id === id);
            if (!student) return;

            showConfirmModal(
                "Hapus Data Siswa",
                `Apakah Anda yakin ingin menghapus ${student.name}? Data absensi terkait juga akan disembunyikan.`,
                () => {
                    students = students.filter(s => s.id !== id);
                    saveStudentsToStorage();
                    renderStudentTable();
                    renderAttendanceList();
                    renderRecapTable();
                    showToast("Siswa telah dihapus", "info");
                }
            );
        }

        function exportDataToCSV() {
            const cls = document.getElementById('class-select').value;
            const classStudents = students.filter(s => s.class === cls);

            let csvContent = "data:text/csv;charset=utf-8,";
            csvContent += "No,NIS,Nama Siswa,Jenis Kelamin,Kelas,Hadir,Izin,Sakit,Alpa,Persentase\n";

            classStudents.forEach((s, idx) => {
                let h = 0, i = 0, sk = 0, a = 0;
                Object.keys(attendanceDB).forEach(key => {
                    if (key.startsWith(cls + '__')) {
                        const rec = attendanceDB[key][s.id];
                        if (rec) {
                            if (rec.status === 'Hadir') h++;
                            else if (rec.status === 'Izin') i++;
                            else if (rec.status === 'Sakit') sk++;
                            else if (rec.status === 'Alpa') a++;
                        }
                    }
                });

                const total = h + i + sk + a;
                const pct = total > 0 ? Math.round((h / total) * 100) : 100;

                const row = [
                    idx + 1,
                    `"${s.nis}"`,
                    `"${s.name}"`,
                    s.gender,
                    `"${s.class}"`,
                    h, i, sk, a,
                    `"${pct}%"`
                ].join(",");

                csvContent += row + "\n";
            });

            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `Rekap_Absensi_${cls.replace(/\s+/g, '_')}.csv`);
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);

            showToast("Laporan CSV berhasil diunduh", "success");
        }

        function switchTab(tabName) {
            // Update desktop navigation UI
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('active', 'bg-white', 'text-slate-900', 'shadow-sm');
                btn.classList.add('text-slate-600');
            });
            const activeBtn = document.getElementById(`tab-btn-${tabName}`);
            if (activeBtn) {
                activeBtn.classList.add('active', 'bg-white', 'text-slate-900', 'shadow-sm');
                activeBtn.classList.remove('text-slate-600');
            }

            // Update mobile navigation UI
            document.querySelectorAll('.mobile-tab').forEach(btn => {
                btn.classList.remove('active', 'text-brand-600');
                btn.classList.add('text-slate-600');
            });
            const activeMobileBtn = document.getElementById(`mobile-tab-${tabName}`);
            if (activeMobileBtn) {
                activeMobileBtn.classList.add('active', 'text-brand-600');
                activeMobileBtn.classList.remove('text-slate-600');
            }

            // Toggle sections
            document.getElementById('tab-content-absensi').classList.add('hidden');
            document.getElementById('tab-content-siswa').classList.add('hidden');
            document.getElementById('tab-content-rekap').classList.add('hidden');

            document.getElementById(`tab-content-${tabName}`).classList.remove('hidden');
        }

        let confirmActionCallback = null;

        function showConfirmModal(title, message, callback) {
            document.getElementById('confirm-title').innerText = title;
            document.getElementById('confirm-message').innerText = message;
            confirmActionCallback = callback;

            const modal = document.getElementById('confirm-modal');
            modal.classList.remove('hidden');

            document.getElementById('confirm-cancel-btn').onclick = () => {
                modal.classList.add('hidden');
            };

            document.getElementById('confirm-ok-btn').onclick = () => {
                if (confirmActionCallback) confirmActionCallback();
                modal.classList.add('hidden');
            };
        }

        function showToast(message, type = "info") {
            const toast = document.getElementById('toast');
            const toastMessage = document.getElementById('toast-message');
            toastMessage.innerText = message;

            toast.classList.remove('translate-y-20', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');

            setTimeout(() => {
                toast.classList.remove('translate-y-0', 'opacity-100');
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 2500);
        }

        function escapeHtml(text) {
            if (!text) return '';
            return text
                .replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
        }
    </script>
</body>
</html>
```
