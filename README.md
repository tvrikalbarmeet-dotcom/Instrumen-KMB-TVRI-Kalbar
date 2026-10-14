<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Instrumen Konten Media Baru TVRI Kalbar</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
    <!-- Alpine.js -->
    <script defer src="https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"></script>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- FontAwesome untuk Ikon -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Inter', sans-serif; }
    </style>
</head>
<body class="bg-gray-100 text-gray-800 antialiased" x-data="tvriApp()" x-init="initApp()">

    <!-- HALAMAN LOGIN (Jika belum login) -->
    <template x-if="!isLoggedIn">
        <div class="min-h-screen flex items-center justify-center bg-gradient-to-br from-blue-900 via-blue-950 to-gray-900 px-4">
            <div class="max-w-md w-full bg-white rounded-2xl shadow-2xl overflow-hidden border-t-4 border-red-600 p-8">
                <div class="text-center mb-8">
                    <div class="inline-flex items-center justify-center w-16 h-16 bg-blue-900 text-white rounded-full mb-4 shadow-inner">
                        <i class="fa-solid fa-tv text-2xl"></i>
                    </div>
                    <h2 class="text-2xl font-bold text-blue-950">TVRI KALBAR</h2>
                    <p class="text-sm text-gray-500 mt-1">Instrumen Konten Media Baru</p>
                </div>
                
                <form @submit.prevent="handleLogin" class="space-y-5">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Pilih Peran / Akun</label>
                        <select x-model="loginRole" class="w-full px-4 py-2.5 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-600 focus:outline-none bg-gray-50 text-sm">
                            <option value="admin">Administrator (Akses Penuh)</option>
                            <option value="petugas">Petugas / Kru Media Baru</option>
                        </select>
                    </div>
                    <template x-if="loginRole === 'petugas'">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">Nama Petugas</label>
                            <select x-model="loginPetugas" class="w-full px-4 py-2.5 rounded-lg border border-gray-300 focus:ring-2 focus:ring-blue-600 focus:outline-none bg-gray-50 text-sm">
                                <template x-for="p in daftarPetugas" :key="p">
                                    <option :value="p" x-text="p"></option>
                                </template>
                            </select>
                        </div>
                    </template>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Kata Sandi (Simulasi)</label>
                        <input type="password" value="tvri123" disabled class="w-full px-4 py-2.5 rounded-lg border border-gray-200 bg-gray-100 text-sm text-gray-500">
                        <p class="text-xs text-gray-400 mt-1">Gunakan sandi bawaan untuk demonstrasi sistem.</p>
                    </div>
                    <button type="submit" class="w-full py-3 bg-red-600 hover:bg-red-700 text-white font-semibold rounded-lg shadow-md transition duration-200 text-sm">
                        <i class="fa-solid fa-right-to-bracket mr-2"></i> Masuk Aplikasi
                    </button>
                </form>
                <div class="mt-6 text-center border-t border-gray-100 pt-4">
                    <p class="text-xs text-gray-400">&copy; 2026 LPP TVRI Stasiun Kalimantan Barat</p>
                </div>
            </div>
        </div>
    </template>

    <!-- HALAMAN UTAMA APLIKASI (Dashboard & Modul) -->
    <template x-if="isLoggedIn">
        <div class="min-h-screen flex flex-col md:flex-row">
            
            <!-- SIDEBAR NAVIGASI -->
            <aside :class="sidebarOpen ? 'translate-x-0' : '-translate-x-full'" class="fixed md:static inset-y-0 left-0 z-50 w-64 bg-blue-950 text-white flex flex-col transition-transform duration-300 md:translate-x-0 shadow-xl">
                <div class="p-5 flex items-center justify-between border-b border-blue-900">
                    <div class="flex items-center space-x-3">
                        <div class="..">
                            
                        </div>
                        <div>
                            <h1 class="font-bold text-sm tracking-wide">TVRI KALBAR</h1>
                            <p class="text-[10px] text-blue-300 uppercase tracking-wider">Konten Media Baru</p>
                        </div>
                    </div>
                    <button @click="sidebarOpen = false" class="md:hidden text-gray-300 hover:text-white">
                        <i class="fa-solid fa-xmark text-lg"></i>
                    </button>
                </div>

                <nav class="flex-1 px-4 py-6 space-y-1.5 overflow-y-auto text-sm">
                    
                    </button>
                    <button @click="currentTab = 'produksi'; sidebarOpen = false" :class="currentTab === 'produksi' ? 'bg-red-600 text-white shadow-md' : 'text-blue-200 hover:bg-blue-900 hover:text-white'" class="w-full flex items-center space-x-3 px-4 py-3 rounded-lg font-medium transition">
                        <i class="fa-solid fa-clapperboard w-5"></i> <span>Produksi Konten</span>
                    </button>
                    <button @click="currentTab = 'distribusi'; sidebarOpen = false" :class="currentTab === 'distribusi' ? 'bg-red-600 text-white shadow-md' : 'text-blue-200 hover:bg-blue-900 hover:text-white'" class="w-full flex items-center space-x-3 px-4 py-3 rounded-lg font-medium transition">
                        <i class="fa-solid fa-share-nodes w-5"></i> <span>Distribusi Sosmed</span>
                   
                    <button @click="currentTab = 'laporan'; sidebarOpen = false" :class="currentTab === 'laporan' ? 'bg-red-600 text-white shadow-md' : 'text-blue-200 hover:bg-blue-900 hover:text-white'" class="w-full flex items-center space-x-3 px-4 py-3 rounded-lg font-medium transition">
                        <i class="fa-solid fa-file-lines w-5"></i> <span>Laporan & Ekspor</span>
                    </button>
                    <template x-if="userRole === 'admin'">
                        <button @click="currentTab = 'pengaturan'; sidebarOpen = false" :class="currentTab === 'pengaturan' ? 'bg-red-600 text-white shadow-md' : 'text-blue-200 hover:bg-blue-900 hover:text-white'" class="w-full flex items-center space-x-3 px-4 py-3 rounded-lg font-medium transition">
                            <i class="fa-solid fa-gear w-5"></i> <span>Pengaturan & Target</span>
                        </button>
			<button @click="currentTab = 'dashboard'; sidebarOpen = false" :class="currentTab === 'dashboard' ? 'bg-red-600 text-white shadow-md' : 'text-blue-200 hover:bg-blue-900 hover:text-white'" class="w-full flex items-center space-x-3 px-4 py-3 rounded-lg font-medium transition">
                        <i class="fa-solid fa-chart-pie w-5"></i> <span>Dashboard</span>
                    </template>
                </nav>

                <div class="p-4 border-t border-blue-900 bg-blue-950/60">
                    <div class="flex items-center justify-between mb-3">
                        <div class="flex items-center space-x-2">
                            <div class="w-8 h-8 rounded-full bg-blue-800 flex items-center justify-center font-bold text-xs">
                                <span x-text="currentUser.charAt(0)"></span>
                            </div>
                            <div class="overflow-hidden">
                                <p class="text-xs font-semibold truncate" x-text="currentUser"></p>
                                <p class="text-[10px] text-blue-300 capitalize" x-text="userRole"></p>
                            </div>
                        </div>
                    </div>
                    <button @click="logout" class="w-full py-2 bg-red-900/50 hover:bg-red-800 text-red-100 rounded-lg text-xs font-medium transition flex items-center justify-center space-x-2">
                        <i class="fa-solid fa-right-from-bracket"></i> <span>Keluar Sistem</span>
                    </button>
                </div>
            </aside>

            <!-- KONTEN UTAMA -->
            <div class="flex-1 flex flex-col min-w-0">
                
                <!-- HEADER ATAS -->
                <header class="bg-white border-b border-gray-200 h-16 flex items-center justify-between px-6 sticky top-0 z-30 shadow-xs">
                    <div class="flex items-center space-x-4">
                        <button @click="sidebarOpen = true" class="md:hidden text-gray-600 hover:text-gray-900 focus:outline-none">
                            <i class="fa-solid fa-bars text-xl"></i>
                        </button>
                        <div>
                            <h2 class="text-base font-bold text-blue-950 uppercase tracking-tight" x-text="getTabTitle()"></h2>
                            <p class="text-xs text-gray-500 hidden sm:block">LPP TVRI Stasiun Kalimantan Barat</p>
                        </div>
                    </div>
                    <div class="flex items-center space-x-3">
                        <div class="bg-blue-50 text-blue-900 px-3 py-1.5 rounded-lg text-xs font-medium border border-blue-100 flex items-center space-x-2">
                            <i class="fa-regular fa-calendar-days text-blue-600"></i>
                            <span x-text="currentDateFormatted"></span>
                        </div>
                    </div>
                </header>

                <!-- AREA ISI KONTEN -->
                <main class="flex-1 p-6 overflow-y-auto">
                    
                    <!-- ================= TAB: DASHBOARD ================= -->
                    <div x-show="currentTab === 'dashboard'" class="space-y-6">
                        <!-- Filter Global Dashboard -->
                        <div class="bg-white p-4 rounded-xl shadow-xs border border-gray-200 flex flex-wrap items-center justify-between gap-4">
                            <div class="flex flex-wrap items-center gap-3 w-full lg:w-auto">
                                <div class="flex items-center space-x-2">
                                    <span class="text-xs font-semibold text-gray-600">Periode:</span>
                                    <select x-model="filterPeriode" class="px-3 py-1.5 text-xs bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                                        <option value="semua">Semua Waktu</option>
                                        <option value="hari_ini">Hari Ini</option>
                                        <option value="minggu_ini">Minggu Ini</option>
                                        <option value="bulan_ini">Bulan Ini</option>
                                        <option value="tahun_ini">Tahun Ini</option>
                                    </select>
                                </div>
                                <div class="flex items-center space-x-2">
                                    <span class="text-xs font-semibold text-gray-600">Petugas:</span>
                                    <select x-model="filterPetugas" class="px-3 py-1.5 text-xs bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                                        <option value="">Semua Petugas</option>
                                        <template x-for="p in daftarPetugas" :key="p">
                                            <option :value="p" x-text="p"></option>
                                        </template>
                                    </select>
                                </div>
                            </div>
                            <div class="text-xs text-gray-500">
                                <span class="font-medium text-blue-900">Catatan:</span> Data dihitung secara otomatis dari sistem operasional.
                            </div>
                        </div>

                        <!-- Kartu Statistik Utama -->
                        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                            <!-- Kartu 1: Produksi -->
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-xs flex items-center justify-between">
                                <div>
                                    <p class="text-xs font-medium text-gray-500 uppercase">Capaian Produksi</p>
                                    <h3 class="text-2xl font-bold text-blue-950 mt-1">
                                        <span x-text="stats.realisasiProduksi"></span> / <span class="text-gray-400 text-lg" x-text="stats.targetProduksi"></span>
                                    </h3>
                                    <div class="mt-2 flex items-center space-x-2">
                                        <div class="w-24 bg-gray-200 h-2 rounded-full overflow-hidden">
                                            <div class="bg-blue-600 h-full rounded-full" :style="`width: ${stats.persenProduksi}%`"></div>
                                        </div>
                                        <span class="text-xs font-semibold text-blue-700" x-text="stats.persenProduksi + '%'"></span>
                                    </div>
                                </div>
                                <div class="w-12 h-12 bg-blue-50 text-blue-600 rounded-xl flex items-center justify-center text-xl">
                                    <i class="fa-solid fa-clapperboard"></i>
                                </div>
                            </div>

                            <!-- Kartu 2: Distribusi -->
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-xs flex items-center justify-between">
                                <div>
                                    <p class="text-xs font-medium text-gray-500 uppercase">Capaian Publikasi</p>
                                    <h3 class="text-2xl font-bold text-blue-950 mt-1">
                                        <span x-text="stats.realisasiPublikasi"></span> / <span class="text-gray-400 text-lg" x-text="stats.targetPublikasi"></span>
                                    </h3>
                                    <div class="mt-2 flex items-center space-x-2">
                                        <div class="w-24 bg-gray-200 h-2 rounded-full overflow-hidden">
                                            <div class="bg-red-600 h-full rounded-full" :style="`width: ${stats.persenPublikasi}%`"></div>
                                        </div>
                                        <span class="text-xs font-semibold text-red-600" x-text="stats.persenPublikasi + '%'"></span>
                                    </div>
                                </div>
                                <div class="w-12 h-12 bg-red-50 text-red-600 rounded-xl flex items-center justify-center text-xl">
                                    <i class="fa-solid fa-share-nodes"></i>
                                </div>
                            </div>

                            <!-- Kartu 3: Belum Selesai -->
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-xs flex items-center justify-between">
                                <div>
                                    <p class="text-xs font-medium text-gray-500 uppercase">Belum Selesai / Proses</p>
                                    <h3 class="text-2xl font-bold text-amber-600 mt-1" x-text="stats.kontenProses"></h3>
                                    <p class="text-[11px] text-gray-400 mt-2">Pekerjaan dalam antrean & proses</p>
                                </div>
                                <div class="w-12 h-12 bg-amber-50 text-amber-600 rounded-xl flex items-center justify-center text-xl">
                                    <i class="fa-solid fa-spinner"></i>
                                </div>
                            </div>

                            <!-- Kartu 4: Tertunda / Terlambat -->
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-xs flex items-center justify-between">
                                <div>
                                    <p class="text-xs font-medium text-gray-500 uppercase">Tertunda / Terlambat</p>
                                    <h3 class="text-2xl font-bold text-red-600 mt-1" x-text="stats.kontenTertunda"></h3>
                                    <p class="text-[11px] text-gray-400 mt-2">Memerlukan perhatian khusus</p>
                                </div>
                                <div class="w-12 h-12 bg-red-50 text-red-600 rounded-xl flex items-center justify-center text-xl">
                                    <i class="fa-solid fa-triangle-exclamation"></i>
                                </div>
                            </div>
                        </div>

                        <!-- Grafik Perbandingan -->
                        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-xs">
                                <h3 class="text-sm font-bold text-blue-950 mb-4 flex items-center justify-between">
                                    <span>Grafik Produksi vs Realisasi</span>
                                    <i class="fa-solid fa-chart-column text-gray-400"></i>
                                </h3>
                                <div class="h-64 relative">
                                    <canvas id="chartProduksi"></canvas>
                                </div>
                            </div>
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-xs">
                                <h3 class="text-sm font-bold text-blue-950 mb-4 flex items-center justify-between">
                                    <span>Grafik Rencana vs Realisasi Publikasi Platform</span>
                                    <i class="fa-solid fa-chart-bar text-gray-400"></i>
                                </h3>
                                <div class="h-64 relative">
                                    <canvas id="chartPublikasi"></canvas>
                                </div>
                            </div>
                        </div>

                        <!-- Grafik Kedua -->
                        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-xs">
                                <h3 class="text-sm font-bold text-blue-950 mb-4 flex items-center justify-between">
                                    <span>Capaian Produksi per Petugas</span>
                                    <i class="fa-solid fa-user-check text-gray-400"></i>
                                </h3>
                                <div class="h-64 relative">
                                    <canvas id="chartPetugas"></canvas>
                                </div>
                            </div>
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-xs">
                                <h3 class="text-sm font-bold text-blue-950 mb-4 flex items-center justify-between">
                                    <span>Distribusi Konten per Platform Media Sosial</span>
                                    <i class="fa-solid fa-pie-chart text-gray-400"></i>
                                </h3>
                                <div class="h-64 relative flex items-center justify-center">
                                    <canvas id="chartPlatform"></canvas>
                                </div>
                            </div>
                        </div>

                        <!-- Tabel Rekapitulasi Kinerja Petugas -->
                        <div class="bg-white rounded-xl border border-gray-200 shadow-xs overflow-hidden">
                            <div class="p-5 border-b border-gray-200 flex justify-between items-center">
                                <h3 class="text-sm font-bold text-blue-950">Rekapitulasi Kinerja Tim Media Baru</h3>
                                <span class="text-xs text-gray-500">Periode Aktif</span>
                            </div>
                            <div class="overflow-x-auto">
                                <table class="w-full text-left text-xs">
                                    <thead class="bg-gray-50 text-gray-600 uppercase border-b border-gray-200">
                                        <tr>
                                            <th class="p-3">Nama Petugas</th>
                                            <th class="p-3 text-center">Target Prod.</th>
                                            <th class="p-3 text-center">Realisasi Prod.</th>
                                            <th class="p-3 text-center">Capaian Prod.</th>
                                            <th class="p-3 text-center">Target Pub.</th>
                                            <th class="p-3 text-center">Realisasi Pub.</th>
                                            <th class="p-3 text-center">Capaian Pub.</th>
                                            <th class="p-3 text-center">YouTube</th>
                                            <th class="p-3 text-center">Instagram</th>
                                            <th class="p-3 text-center">Facebook</th>
                                            <th class="p-3 text-center">TikTok</th>
                                            <th class="p-3 text-center">X</th>
                                        </tr>
                                    </thead>
                                    <tbody class="divide-y divide-gray-200">
                                        <template x-for="p in rekapPetugasList" :key="p.nama">
                                            <tr class="hover:bg-gray-50">
                                                <td class="p-3 font-semibold text-blue-950" x-text="p.nama"></td>
                                                <td class="p-3 text-center" x-text="p.targetProd"></td>
                                                <td class="p-3 text-center font-medium" x-text="p.realProd"></td>
                                                <td class="p-3 text-center">
                                                    <span class="px-2 py-0.5 rounded-full font-semibold text-[10px]" :class="p.persenProd >= 100 ? 'bg-emerald-100 text-emerald-800' : 'bg-amber-100 text-amber-800'" x-text="p.persenProd + '%'"></span>
                                                </td>
                                                <td class="p-3 text-center" x-text="p.targetPub"></td>
                                                <td class="p-3 text-center font-medium" x-text="p.realPub"></td>
                                                <td class="p-3 text-center">
                                                    <span class="px-2 py-0.5 rounded-full font-semibold text-[10px]" :class="p.persenPub >= 100 ? 'bg-emerald-100 text-emerald-800' : 'bg-amber-100 text-amber-800'" x-text="p.persenPub + '%'"></span>
                                                </td>
                                                <td class="p-3 text-center" x-text="p.yt"></td>
                                                <td class="p-3 text-center" x-text="p.ig"></td>
                                                <td class="p-3 text-center" x-text="p.fb"></td>
                                                <td class="p-3 text-center" x-text="p.tt"></td>
                                                <td class="p-3 text-center" x-text="p.x"></td>
                                            </tr>
                                        </template>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>

                    <!-- ================= TAB: PRODUKSI ================= -->
                    <div x-show="currentTab === 'produksi'" class="space-y-6" x-init="$watch('currentTab', val => { if(val==='produksi') initProduksiCharts(); })">
                        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-4 rounded-xl border border-gray-200 shadow-xs">
                            <div>
                                <h3 class="text-base font-bold text-blue-950">Produksi & Rencana</h3>
                                <p class="text-xs text-gray-500">Kelola perencanaan dan realisasi produksi konten harian</p>
                            </div>
                            <button @click="openProduksiModal()" class="px-4 py-2 bg-red-600 hover:bg-red-700 text-white rounded-lg text-xs font-semibold shadow transition flex items-center space-x-2">
                                <i class="fa-solid fa-plus"></i> <span>Tambah Rencana / Produksi</span>
                            </button>
                        </div>

                        <!-- Filter Tabel Produksi -->
                        <div class="bg-white p-4 rounded-xl border border-gray-200 shadow-xs flex flex-wrap gap-4 items-center justify-between">
                            <div class="flex flex-wrap items-center gap-3 w-full md:w-auto">
                                <input type="text" x-model="searchProduksi" placeholder="Cari judul atau petugas..." class="px-3 py-1.5 text-xs bg-gray-50 border border-gray-300 rounded-lg w-64 focus:ring-2 focus:ring-blue-600 focus:outline-none">
                                <select x-model="filterStatusProduksi" class="px-3 py-1.5 text-xs bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                                    <option value="">Semua Status</option>
                                    <option value="Direncanakan">Direncanakan</option>
                                    <option value="Dalam Proses">Dalam Proses</option>
                                    <option value="Selesai">Selesai</option>
                                    <option value="Tertunda">Tertunda</option>
                                </select>
                            </div>
                            <div class="text-xs text-gray-500">
                                Total data: <span class="font-bold text-blue-950" x-text="filteredProduksiList.length"></span>
                            </div>
                        </div>

                        <!-- Tabel Daftar Produksi -->
                        <div class="bg-white rounded-xl border border-gray-200 shadow-xs overflow-hidden">
                            <div class="overflow-x-auto">
                                <table class="w-full text-left text-xs">
                                    <thead class="bg-gray-50 text-gray-600 uppercase border-b border-gray-200">
                                        <tr>
                                            <th class="p-3">No. Pekerjaan & Tgl</th>
                                            <th class="p-3">Petugas</th>
                                            <th class="p-3">Judul Konten</th>
                                            <th class="p-3">Jenis</th>
                                            <th class="p-3 text-center">Target / Aktual</th>
                                            <th class="p-3 text-center">Status</th>
                                            <th class="p-3 text-center">Tautan</th>
                                            <th class="p-3 text-center">Aksi</th>
                                        </tr>
                                    </thead>
                                    <tbody class="divide-y divide-gray-200">
                                        <template x-for="item in filteredProduksiList" :key="item.id">
                                            <tr class="hover:bg-gray-50">
                                                <td class="p-3">
                                                    <span class="font-bold text-blue-950" x-text="item.noPekerjaan"></span>
                                                    <p class="text-[10px] text-gray-400" x-text="'Rencana: ' + item.tglRencana"></p>
                                                </td>
                                                <td class="p-3 font-medium text-gray-700" x-text="item.petugas"></td>
                                                <td class="p-3 font-semibold text-blue-950 max-w-xs truncate" :title="item.judul" x-text="item.judul"></td>
                                                <td class="p-3">
                                                    <span class="px-2 py-0.5 bg-blue-50 text-blue-700 rounded text-[10px] font-medium" x-text="item.jenis"></span>
                                                </td>
                                                <td class="p-3 text-center">
                                                    <span class="font-semibold" x-text="item.jumlahAktual"></span> / <span class="text-gray-400" x-text="item.targetJumlah"></span>
                                                </td>
                                                <td class="p-3 text-center">
                                                    <span class="px-2.5 py-1 rounded-full text-[10px] font-semibold"
                                                        :class="{
                                                            'bg-blue-100 text-blue-800': item.status === 'Direncanakan',
                                                            'bg-amber-100 text-amber-800': item.status === 'Dalam Proses',
                                                            'bg-emerald-100 text-emerald-800': item.status === 'Selesai',
                                                            'bg-red-100 text-red-800': item.status === 'Tertunda'
                                                        }" x-text="item.status"></span>
                                                </td>
                                                <td class="p-3 text-center">
                                                    <template x-if="item.tautan">
                                                        <a :href="item.tautan" target="_blank" class="text-blue-600 hover:underline"><i class="fa-solid fa-link"></i></a>
                                                    </template>
                                                    <template x-if="!item.tautan">
                                                        <span class="text-gray-300">-</span>
                                                    </template>
                                                </td>
                                                <td class="p-3 text-center space-x-1">
                                                    <button @click="editProduksi(item)" class="p-1.5 bg-blue-50 text-blue-600 rounded hover:bg-blue-100 transition" title="Edit"><i class="fa-solid fa-pen-to-square"></i></button>
                                                    <button @click="deleteProduksi(item.id)" class="p-1.5 bg-red-50 text-red-600 rounded hover:bg-red-100 transition" title="Hapus"><i class="fa-solid fa-trash"></i></button>
                                                </td>
                                            </tr>
                                        </template>
                                        <template x-if="filteredProduksiList.length === 0">
                                            <tr>
                                                <td colspan="8" class="p-6 text-center text-gray-400">Belum ada data produksi konten yang tersedia.</td>
                                            </tr>
                                        </template>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>

                    <!-- ================= TAB: DISTRIBUSI ================= -->
                    <div x-show="currentTab === 'distribusi'" class="space-y-6">
                        <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-4 rounded-xl border border-gray-200 shadow-xs">
                            <div>
                                <h3 class="text-base font-bold text-blue-950">Catatan Distribusi Media Sosial</h3>
                                <p class="text-xs text-gray-500">Kelola publikasi konten di 5 platform utama TVRI Kalbar</p>
                            </div>
                            <button @click="openDistribusiModal()" class="px-4 py-2 bg-red-600 hover:bg-red-700 text-white rounded-lg text-xs font-semibold shadow transition flex items-center space-x-2">
                                <i class="fa-solid fa-share"></i> <span>Catat Publikasi Baru</span>
                            </button>
                        </div>

                        <!-- Tab Sub-Platform -->
                        <div class="flex flex-wrap gap-2 border-b border-gray-200 pb-2">
                            <button @click="activePlatformTab = 'semua'" :class="activePlatformTab === 'semua' ? 'bg-blue-950 text-white' : 'bg-white text-gray-700 border border-gray-300 hover:bg-gray-50'" class="px-4 py-2 rounded-lg text-xs font-semibold transition">Semua Platform</button>
                            <button @click="activePlatformTab = 'YouTube'" :class="activePlatformTab === 'YouTube' ? 'bg-red-600 text-white' : 'bg-white text-gray-700 border border-gray-300 hover:bg-gray-50'" class="px-4 py-2 rounded-lg text-xs font-semibold transition"><i class="fa-brands fa-youtube mr-1"></i> YouTube</button>
                            <button @click="activePlatformTab = 'Instagram'" :class="activePlatformTab === 'Instagram' ? 'bg-pink-600 text-white' : 'bg-white text-gray-700 border border-gray-300 hover:bg-gray-50'" class="px-4 py-2 rounded-lg text-xs font-semibold transition"><i class="fa-brands fa-instagram mr-1"></i> Instagram</button>
                            <button @click="activePlatformTab = 'Facebook'" :class="activePlatformTab === 'Facebook' ? 'bg-blue-600 text-white' : 'bg-white text-gray-700 border border-gray-300 hover:bg-gray-50'" class="px-4 py-2 rounded-lg text-xs font-semibold transition"><i class="fa-brands fa-facebook mr-1"></i> Facebook</button>
                            <button @click="activePlatformTab = 'TikTok'" :class="activePlatformTab === 'TikTok' ? 'bg-black text-white' : 'bg-white text-gray-700 border border-gray-300 hover:bg-gray-50'" class="px-4 py-2 rounded-lg text-xs font-semibold transition"><i class="fa-brands fa-tiktok mr-1"></i> TikTok</button>
                            <button @click="activePlatformTab = 'X'" :class="activePlatformTab === 'X' ? 'bg-gray-900 text-white' : 'bg-white text-gray-700 border border-gray-300 hover:bg-gray-50'" class="px-4 py-2 rounded-lg text-xs font-semibold transition"><i class="fa-brands fa-x-twitter mr-1"></i> X (Twitter)</button>
                        </div>

                        <!-- Tabel Distribusi -->
                        <div class="bg-white rounded-xl border border-gray-200 shadow-xs overflow-hidden">
                            <div class="overflow-x-auto">
                                <table class="w-full text-left text-xs">
                                    <thead class="bg-gray-50 text-gray-600 uppercase border-b border-gray-200">
                                        <tr>
                                            <th class="p-3">Platform</th>
                                            <th class="p-3">Petugas</th>
                                            <th class="p-3">Judul Konten</th>
                                            <th class="p-3">Format / Jenis</th>
                                            <th class="p-3">Tgl Rencana / Aktual</th>
                                            <th class="p-3 text-center">Status</th>
                                            <th class="p-3 text-center">Tautan</th>
                                            <th class="p-3 text-center">Aksi</th>
                                        </tr>
                                    </thead>
                                    <tbody class="divide-y divide-gray-200">
                                        <template x-for="item in filteredDistribusiList" :key="item.id">
                                            <tr class="hover:bg-gray-50">
                                                <td class="p-3 font-bold" :class="{
                                                    'text-red-600': item.platform === 'YouTube',
                                                    'text-pink-600': item.platform === 'Instagram',
                                                    'text-blue-600': item.platform === 'Facebook',
                                                    'text-black': item.platform === 'TikTok',
                                                    'text-gray-900': item.platform === 'X'
                                                }" x-text="item.platform"></td>
                                                <td class="p-3 font-medium text-gray-700" x-text="item.petugas"></td>
                                                <td class="p-3 font-semibold text-blue-950 max-w-xs truncate" :title="item.judul" x-text="item.judul"></td>
                                                <td class="p-3">
                                                    <span class="px-2 py-0.5 bg-gray-100 text-gray-800 rounded text-[10px]" x-text="item.format"></span>
                                                </td>
                                                <td class="p-3">
                                                    <span x-text="item.tglRencana"></span>
                                                    <p class="text-[10px] text-emerald-600" x-text="item.tglAktual ? 'Tayang: ' + item.tglAktual : 'Belum tayang'"></p>
                                                </td>
                                                <td class="p-3 text-center">
                                                    <span class="px-2.5 py-1 rounded-full text-[10px] font-semibold"
                                                        :class="{
                                                            'bg-blue-100 text-blue-800': item.status === 'Terjadwal',
                                                            'bg-emerald-100 text-emerald-800': item.status === 'Dipublikasikan',
                                                            'bg-red-100 text-red-800': item.status === 'Tertunda'
                                                        }" x-text="item.status"></span>
                                                </td>
                                                <td class="p-3 text-center">
                                                    <template x-if="item.tautan">
                                                        <a :href="item.tautan" target="_blank" class="text-blue-600 hover:underline"><i class="fa-solid fa-link"></i></a>
                                                    </template>
                                                    <template x-if="!item.tautan">
                                                        <span class="text-gray-300">-</span>
                                                    </template>
                                                </td>
                                                <td class="p-3 text-center space-x-1">
                                                    <button @click="editDistribusi(item)" class="p-1.5 bg-blue-50 text-blue-600 rounded hover:bg-blue-100 transition" title="Edit"><i class="fa-solid fa-pen-to-square"></i></button>
                                                    <button @click="deleteDistribusi(item.id)" class="p-1.5 bg-red-50 text-red-600 rounded hover:bg-red-100 transition" title="Hapus"><i class="fa-solid fa-trash"></i></button>
                                                </td>
                                            </tr>
                                        </template>
                                        <template x-if="filteredDistribusiList.length === 0">
                                            <tr>
                                                <td colspan="8" class="p-6 text-center text-gray-400">Belum ada data distribusi untuk platform ini.</td>
                                            </tr>
                                        </template>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>

                    <!-- ================= TAB: KALENDER HARIAN ================= -->
                    <div x-show="currentTab === 'kalender'" class="space-y-6">
                        <div class="bg-white p-4 rounded-xl border border-gray-200 shadow-xs flex flex-col md:flex-row justify-between items-center gap-4">
                            <div>
                                <h3 class="text-base font-bold text-blue-950">Kalender Harian & Agenda Kerja</h3>
                                <p class="text-xs text-gray-500">Pemantauan harian pekerjaan perencanaan, produksi, dan publikasi</p>
                            </div>
                            <div class="flex items-center space-x-3">
                                <input type="date" x-model="filterTanggalKalender" class="px-3 py-1.5 text-xs bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                            </div>
                        </div>

                        <!-- Ringkasan Agenda Harian -->
                        <div class="bg-white rounded-xl border border-gray-200 shadow-xs overflow-hidden">
                            <div class="p-4 bg-gray-50 border-b border-gray-200 font-bold text-xs text-blue-950">
                                Agenda Pekerjaan Tanggal: <span x-text="filterTanggalKalender"></span>
                            </div>
                            <div class="overflow-x-auto">
                                <table class="w-full text-left text-xs">
                                    <thead class="bg-gray-100 text-gray-600 uppercase border-b border-gray-200">
                                        <tr>
                                            <th class="p-3">Nama Petugas</th>
                                            <th class="p-3">Rencana Produksi</th>
                                            <th class="p-3">Realisasi Produksi</th>
                                            <th class="p-3">Rencana Publikasi</th>
                                            <th class="p-3">Realisasi Publikasi</th>
                                            <th class="p-3 text-center">Status</th>
                                            <th class="p-3">Catatan / Kendala</th>
                                        </tr>
                                    </thead>
                                    <tbody class="divide-y divide-gray-200">
                                        <template x-for="item in agendaHarianList" :key="item.id">
                                            <tr class="hover:bg-gray-50">
                                                <td class="p-3 font-semibold text-blue-950" x-text="item.petugas"></td>
                                                <td class="p-3" x-text="item.judulProd || '-'"></td>
                                                <td class="p-3 text-emerald-700 font-medium" x-text="item.tglRealProd ? item.judulProd + ' (Selesai)' : 'Belum realisasi'"></td>
                                                <td class="p-3" x-text="item.judulPub || '-'"></td>
                                                <td class="p-3 text-emerald-700 font-medium" x-text="item.tglRealPub ? 'Dipublikasikan' : 'Belum tayang'"></td>
                                                <td class="p-3 text-center">
                                                    <span class="px-2 py-0.5 rounded-full text-[10px] font-semibold"
                                                        :class="{
                                                            'bg-blue-100 text-blue-800': item.status === 'Direncanakan' || item.status === 'Terjadwal',
                                                            'bg-emerald-100 text-emerald-800': item.status === 'Selesai' || item.status === 'Dipublikasikan',
                                                            'bg-amber-100 text-amber-800': item.status === 'Dalam Proses',
                                                            'bg-red-100 text-red-800': item.status === 'Tertunda'
                                                        }" x-text="item.status"></span>
                                                </td>
                                                <td class="p-3 text-gray-500" x-text="item.catatan || '-'"></td>
                                            </tr>
                                        </template>
                                        <template x-if="agendaHarianList.length === 0">
                                            <tr>
                                                <td colspan="7" class="p-6 text-center text-gray-400">Tidak ada agenda pekerjaan tercatat pada tanggal ini.</td>
                                            </tr>
                                        </template>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>

                    <!-- ================= TAB: LAPORAN ================= -->
                    <div x-show="currentTab === 'laporan'" class="space-y-6">
                        <div class="bg-white p-6 rounded-xl border border-gray-200 shadow-xs flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                            <div>
                                <h3 class="text-base font-bold text-blue-950">Rekapitulasi Kinerja</h3>
                                <p class="text-xs text-gray-500">Unduh laporan resmi kerja tim Media Baru TVRI Kalbar</p>
                            </div>
                            <div class="flex flex-wrap gap-2">
                                <button @click="printReport" class="px-4 py-2 bg-blue-900 hover:bg-blue-950 text-white rounded-lg text-xs font-semibold shadow transition flex items-center space-x-2">
                                    <i class="fa-solid fa-print"></i> <span>Cetak Laporan</span>
                                </button>
                                <button @click="exportExcel" class="px-4 py-2 bg-emerald-600 hover:bg-emerald-700 text-white rounded-lg text-xs font-semibold shadow transition flex items-center space-x-2">
                                    <i class="fa-solid fa-file-excel"></i> <span>Unduh Excel</span>
                                </button>
                                <button @click="exportPDF" class="px-4 py-2 bg-red-600 hover:bg-red-700 text-white rounded-lg text-xs font-semibold shadow transition flex items-center space-x-2">
                                    <i class="fa-solid fa-file-pdf"></i> <span>Unduh PDF</span>
                                </button>
                            </div>
                        </div>

                        <!-- Preview Laporan Resmi -->
                        <div id="printableArea" class="bg-white p-8 rounded-xl border border-gray-200 shadow-xs space-y-6">
                            <div class="text-center border-b-2 border-blue-950 pb-4">
                                <h2 class="text-lg font-bold text-blue-950 uppercase">Laporan Kinerja Seksi Media Baru</h2>
                                <h3 class="text-sm font-semibold text-gray-700">LPP TVRI Stasiun Kalimantan Barat</h3>
                                <p class="text-xs text-gray-500 mt-1">Periode Evaluasi Kerja Tahun 2026</p>
                            </div>

                            <div class="space-y-4 text-xs">
                                <h4 class="font-bold text-blue-950 text-sm border-b pb-1">I. Ringkasan Capaian Utama</h4>
                                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                                    <div class="p-3 bg-gray-50 rounded-lg border">
                                        <p class="text-gray-500">Target Produksi</p>
                                        <p class="text-base font-bold text-blue-950" x-text="stats.targetProduksi"></p>
                                    </div>
                                    <div class="p-3 bg-gray-50 rounded-lg border">
                                        <p class="text-gray-500">Realisasi Produksi</p>
                                        <p class="text-base font-bold text-emerald-600" x-text="stats.realisasiProduksi"></p>
                                    </div>
                                    <div class="p-3 bg-gray-50 rounded-lg border">
                                        <p class="text-gray-500">Target Publikasi</p>
                                        <p class="text-base font-bold text-blue-950" x-text="stats.targetPublikasi"></p>
                                    </div>
                                    <div class="p-3 bg-gray-50 rounded-lg border">
                                        <p class="text-gray-500">Realisasi Publikasi</p>
                                        <p class="text-base font-bold text-red-600" x-text="stats.realisasiPublikasi"></p>
                                    </div>
                                </div>
                            </div>

                            <div class="space-y-4 text-xs">
                                <h4 class="font-bold text-blue-950 text-sm border-b pb-1">II. Rekapitulasi per Petugas</h4>
                                <table class="w-full text-left border border-gray-200">
                                    <thead class="bg-gray-100 text-gray-700 uppercase">
                                        <tr>
                                            <th class="p-2 border">Nama Petugas</th>
                                            <th class="p-2 border text-center">Target Prod</th>
                                            <th class="p-2 border text-center">Realisasi Prod</th>
                                            <th class="p-2 border text-center">Capaian Prod</th>
                                            <th class="p-2 border text-center">Realisasi Publikasi</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <template x-for="p in rekapPetugasList" :key="p.nama">
                                            <tr>
                                                <td class="p-2 border font-medium" x-text="p.nama"></td>
                                                <td class="p-2 border text-center" x-text="p.targetProd"></td>
                                                <td class="p-2 border text-center" x-text="p.realProd"></td>
                                                <td class="p-2 border text-center font-bold" x-text="p.persenProd + '%'"></td>
                                                <td class="p-2 border text-center" x-text="p.realPub"></td>
                                            </tr>
                                        </template>
                                    </tbody>
                                </table>
                            </div>

                            <div class="pt-8 flex justify-between text-xs text-gray-600">
                                <div>
                                    <p>Mengetahui,</p>
                                    <p class="font-bold text-blue-950 mt-1">KPP Media Baru</p>
                                    <div class="h-16"></div>
                                    <p class="underline font-semibold">Darius Tarigan</p>
                                </div>
                                <div class="text-right">
                                    <p>Pontianak, <span x-text="currentDateFormatted"></span></p>
                                    <p class="font-bold text-blue-950 mt-1">Petugas Pelapor</p>
                                    <div class="h-16"></div>
                                    <p class="underline font-semibold" x-text="currentUser"></p>
                                </div>
                            </div>
                        </div>
                    </div>

                    <!-- ================= TAB: PENGATURAN ================= -->
                    <template x-if="userRole === 'admin'">
                        <div x-show="currentTab === 'pengaturan'" class="space-y-6">
                            <div class="bg-white p-6 rounded-xl border border-gray-200 shadow-xs space-y-6">
                                <div>
                                    <h3 class="text-base font-bold text-blue-950">Pengaturan Target & Sistem</h3>
                                    <p class="text-xs text-gray-500">Konfigurasi target produksi dan manajemen data dasar</p>
                                </div>

                                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                                    <div class="p-4 bg-gray-50 rounded-xl border border-gray-200 space-y-4">
                                        <h4 class="font-bold text-sm text-blue-950">Target Produksi Bulanan Tim</h4>
                                        <div>
                                            <label class="block text-xs font-medium text-gray-700 mb-1">Target Total Produksi Konten</label>
                                            <input type="number" x-model.number="globalTargetProduksi" class="w-full px-3 py-2 text-xs bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                                        </div>
                                        <div>
                                            <label class="block text-xs font-medium text-gray-700 mb-1">Target Total Publikasi Konten</label>
                                            <input type="number" x-model.number="globalTargetPublikasi" class="w-full px-3 py-2 text-xs bg-white border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                                        </div>
                                        <button @click="saveSettings" class="px-4 py-2 bg-blue-900 hover:bg-blue-950 text-white rounded-lg text-xs font-semibold shadow transition">Simpan Pengaturan Target</button>
                                    </div>

                                    <div class="p-4 bg-gray-50 rounded-xl border border-gray-200 space-y-4">
                                        <h4 class="font-bold text-sm text-blue-950">Daftar Petugas Tetap</h4>
                                        <ul class="space-y-2 text-xs">
                                            <template x-for="(p, index) in daftarPetugas" :key="p">
                                                <li class="p-2 bg-white rounded border border-gray-200 flex justify-between items-center">
                                                    <span class="font-medium" x-text="(index + 1) + '. ' + p"></span>
                                                    <span class="text-[10px] bg-blue-100 text-blue-800 px-2 py-0.5 rounded">Aktif</span>
                                                </li>
                                            </template>
                                        </ul>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </template>

                </main>
            </div>
        </div>
    </template>

    <!-- MODAL FORMULIR PRODUKSI -->
    <div x-show="showProdModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 p-4 overflow-y-auto" style="display: none;">
        <div class="bg-white rounded-2xl max-w-2xl w-full p-6 shadow-2xl border border-gray-200 my-8">
            <div class="flex justify-between items-center border-b pb-3 mb-4">
                <h3 class="font-bold text-sm text-blue-950" x-text="isEditProd ? 'Edit Data Produksi Konten' : 'Tambah Rencana / Realisasi Produksi'"></h3>
                <button @click="showProdModal = false" class="text-gray-400 hover:text-gray-600"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            
            <form @submit.prevent="saveProduksi" class="space-y-4 text-xs">
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Nomor Pekerjaan</label>
                        <input type="text" x-model="formProd.noPekerjaan" readonly class="w-full px-3 py-2 bg-gray-100 border border-gray-300 rounded-lg text-gray-500">
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Nama Petugas</label>
                        <select x-model="formProd.petugas" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                            <option value="">Pilih Petugas</option>
                            <template x-for="p in daftarPetugas" :key="p"><option :value="p" x-text="p"></option></template>
                        </select>
                    </div>
                </div>

                <div>
                    <label class="block font-medium text-gray-700 mb-1">Judul / Nama Konten</label>
                    <input type="text" x-model="formProd.judul" required placeholder="Contoh: Liputan Festival Kuliner Pontianak" class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Jenis Konten</label>
                        <select x-model="formProd.jenis" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                            <option value="Berita">Berita</option>
                            <option value="Feature">Feature</option>
                            <option value="Program">Program</option>
                            <option value="Liputan">Liputan</option>
                            <option value="Video Pendek">Video Pendek</option>
                            <option value="Lainnya">Lainnya</option>
                        </select>
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Tanggal Rencana</label>
                        <input type="date" x-model="formProd.tglRencana" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Target Jumlah</label>
                        <input type="number" x-model.number="formProd.targetJumlah" min="1" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 border-t pt-4">
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Tanggal Realisasi Selesai</label>
                        <input type="date" x-model="formProd.tglReal" class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Jumlah Aktual</label>
                        <input type="number" x-model.number="formProd.jumlahAktual" min="0" class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Status Pekerjaan</label>
                        <select x-model="formProd.status" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                            <option value="Direncanakan">Direncanakan</option>
                            <option value="Dalam Proses">Dalam Proses</option>
                            <option value="Selesai">Selesai</option>
                            <option value="Tertunda">Tertunda</option>
                        </select>
                    </div>
                </div>

                <div>
                    <label class="block font-medium text-gray-700 mb-1">Tautan File / Hasil Konten</label>
                    <input type="url" x-model="formProd.tautan" placeholder="https://..." class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                </div>

                <div class="flex justify-end space-x-2 pt-4 border-t">
                    <button type="button" @click="showProdModal = false" class="px-4 py-2 bg-gray-200 text-gray-700 rounded-lg font-medium hover:bg-gray-300">Batal</button>
                    <button type="submit" class="px-4 py-2 bg-red-600 text-white rounded-lg font-medium hover:bg-red-700">Simpan Data</button>
                </div>
            </form>
        </div>
    </div>

    <!-- MODAL FORMULIR DISTRIBUSI -->
    <div x-show="showDistModal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 p-4 overflow-y-auto" style="display: none;">
        <div class="bg-white rounded-2xl max-w-2xl w-full p-6 shadow-2xl border border-gray-200 my-8">
            <div class="flex justify-between items-center border-b pb-3 mb-4">
                <h3 class="font-bold text-sm text-blue-950" x-text="isEditDist ? 'Edit Data Distribusi Media Sosial' : 'Catat Publikasi Baru'"></h3>
                <button @click="showDistModal = false" class="text-gray-400 hover:text-gray-600"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>
            
            <form @submit.prevent="saveDistribusi" class="space-y-4 text-xs">
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Platform Media Sosial</label>
                        <select x-model="formDist.platform" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                            <option value="YouTube">YouTube</option>
                            <option value="Instagram">Instagram</option>
                            <option value="Facebook">Facebook</option>
                            <option value="TikTok">TikTok</option>
                            <option value="X">X (Twitter)</option>
                        </select>
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Nama Petugas</label>
                        <select x-model="formDist.petugas" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                            <option value="">Pilih Petugas</option>
                            <template x-for="p in daftarPetugas" :key="p"><option :value="p" x-text="p"></option></template>
                        </select>
                    </div>
                </div>

                <div>
                    <label class="block font-medium text-gray-700 mb-1">Judul Konten</label>
                    <input type="text" x-model="formDist.judul" required placeholder="Judul konten yang dipublikasikan" class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Format / Jenis</label>
                        <input type="text" x-model="formDist.format" required placeholder="Contoh: Reels / Shorts / Video Panjang" class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Tanggal Rencana Publikasi</label>
                        <input type="date" x-model="formDist.tglRencana" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Tanggal Aktual Publikasi</label>
                        <input type="date" x-model="formDist.tglAktual" class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Status Publikasi</label>
                        <select x-model="formDist.status" required class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                            <option value="Terjadwal">Terjadwal</option>
                            <option value="Dipublikasikan">Dipublikasikan</option>
                            <option value="Tertunda">Tertunda</option>
                        </select>
                    </div>
                    <div>
                        <label class="block font-medium text-gray-700 mb-1">Tautan Publikasi</label>
                        <input type="url" x-model="formDist.tautan" placeholder="https://..." class="w-full px-3 py-2 bg-gray-50 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-600 focus:outline-none">
                    </div>
                </div>

                <div class="flex justify-end space-x-2 pt-4 border-t">
                    <button type="button" @click="showDistModal = false" class="px-4 py-2 bg-gray-200 text-gray-700 rounded-lg font-medium hover:bg-gray-300">Batal</button>
                    <button type="submit" class="px-4 py-2 bg-red-600 text-white rounded-lg font-medium hover:bg-red-700">Simpan Publikasi</button>
                </div>
            </form>
        </div>
    </div>

    <!-- SCRIPT UTAMA LOGIKA APLIKASI -->
    <script>
        function tvriApp() {
            return {
                isLoggedIn: false,
                loginRole: 'admin',
                loginPetugas: 'Darius Tarigan',
                userRole: 'admin',
                currentUser: 'Administrator',
                sidebarOpen: false,
                currentTab: 'dashboard',
                currentDateFormatted: '',

                daftarPetugas: [
                    'Darius Tarigan',
                    'Anwar Sadat',
                    'Nicco Zainal Arsaudi',
                    'Meirina Elisabeth br Sebayang',
                    'Aprilia Tika Anggita'
                ],

                filterPeriode: 'semua',
                filterPetugas: '',
                filterStatusProduksi: '',
                searchProduksi: '',
                activePlatformTab: 'semua',
                filterTanggalKalender: new Date().toISOString().split('T')[0],

                globalTargetProduksi: 50,
                globalTargetPublikasi: 150,

                showProdModal: false,
                isEditProd: false,
                formProd: { id: null, noPekerjaan: '', petugas: '', judul: '', jenis: 'Berita', tglRencana: '', targetJumlah: 1, tglReal: '', jumlahAktual: 0, status: 'Direncanakan', tautan: '' },

                showDistModal: false,
                isEditDist: false,
                formDist: { id: null, platform: 'YouTube', petugas: '', judul: '', format: 'Video Panjang', tglRencana: '', tglAktual: '', status: 'Terjadwal', tautan: '' },

                produksiList: [],
                distribusiList: [],

                initApp() {
                    const today = new Date();
                    const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
                    this.currentDateFormatted = today.toLocaleDateString('id-ID', options);

                    // Load localStorage
                    const savedProd = localStorage.getItem('tvri_produksi');
                    const savedDist = localStorage.getItem('tvri_distribusi');
                    const savedLogin = localStorage.getItem('tvri_login');

                    if (savedProd) {
                        this.produksiList = JSON.parse(savedProd);
                    } else {
                        // Data demo awal
                        this.produksiList = [
                            { id: 1, noPekerjaan: 'JOB-2026-001', petugas: 'Darius Tarigan', judul: 'Pesona Wisata Bahari Singkawang', jenis: 'Feature', tglRencana: '2026-10-01', targetJumlah: 1, tglReal: '2026-10-02', jumlahAktual: 1, status: 'Selesai', tautan: 'https://youtube.com' },
                            { id: 2, noPekerjaan: 'JOB-2026-002', petugas: 'Anwar Sadat', judul: 'Update Harga Bahan Pokok Pasar Flamboyan', jenis: 'Berita', tglRencana: '2026-10-05', targetJumlah: 2, tglReal: '2026-10-05', jumlahAktual: 2, status: 'Selesai', tautan: 'https://youtube.com' },
                            { id: 3, noPekerjaan: 'JOB-2026-003', petugas: 'Nicco Zainal Arsaudi', judul: 'Seni Budaya Dayak di Kabupaten Sintang', jenis: 'Program', tglRencana: '2026-10-10', targetJumlah: 1, tglReal: '', jumlahAktual: 0, status: 'Dalam Proses', tautan: '' }
                        ];
                        localStorage.setItem('tvri_produksi', JSON.stringify(this.produksiList));
                    }

                    if (savedDist) {
                        this.distribusiList = JSON.parse(savedDist);
                    } else {
                        this.distribusiList = [
                            { id: 1, platform: 'YouTube', petugas: 'Darius Tarigan', judul: 'Pesona Wisata Bahari Singkawang', format: 'Video Panjang', tglRencana: '2026-10-02', tglAktual: '2026-10-02', status: 'Dipublikasikan', tautan: 'https://youtube.com' },
                            { id: 2, platform: 'Instagram', petugas: 'Anwar Sadat', judul: 'Update Harga Bahan Pokok Pasar Flamboyan', format: 'Reels', tglRencana: '2026-10-05', tglAktual: '2026-10-05', status: 'Dipublikasikan', tautan: 'https://instagram.com' }
                        ];
                        localStorage.setItem('tvri_distribusi', JSON.stringify(this.distribusiList));
                    }

                    if (savedLogin) {
                        const l = JSON.parse(savedLogin);
                        this.isLoggedIn = l.isLoggedIn;
                        this.userRole = l.userRole;
                        this.currentUser = l.currentUser;
                    }

                    this.$nextTick(() => {
                        this.renderCharts();
                    });
                },

                handleLogin() {
                    this.isLoggedIn = true;
                    this.userRole = this.loginRole;
                    this.currentUser = this.loginRole === 'admin' ? 'Administrator' : this.loginPetugas;
                    localStorage.setItem('tvri_login', JSON.stringify({ isLoggedIn: this.isLoggedIn, userRole: this.userRole, currentUser: this.currentUser }));
                    this.$nextTick(() => { this.renderCharts(); });
                },

                logout() {
                    this.isLoggedIn = false;
                    localStorage.removeItem('tvri_login');
                },

                getTabTitle() {
                    const titles = {
                        'dashboard': 'INSTRUMEN KONTEN MEDIA BARU',
                        'produksi': 'Produksi dan Publikasi Konten Media Baru',
                        'distribusi': 'Distribusi Media Sosial',
                        'kalender': 'Kalender & Agenda Harian',
                        'laporan': 'PELAPORAN',
                        'pengaturan': 'Pengaturan & Target Sistem'
                    };
                    return titles[this.currentTab] || 'Dashboard';
                },

                get filteredProduksiList() {
                    return this.produksiList.filter(item => {
                        const matchStatus = this.filterStatusProduksi ? item.status === this.filterStatusProduksi : true;
                        const matchPetugas = this.filterPetugas ? item.petugas === this.filterPetugas : true;
                        const matchSearch = this.searchProduksi ? item.judul.toLowerCase().includes(this.searchProduksi.toLowerCase()) || item.petugas.toLowerCase().includes(this.searchProduksi.toLowerCase()) : true;
                        return matchStatus && matchPetugas && matchSearch;
                    });
                },

                get filteredDistribusiList() {
                    return this.distribusiList.filter(item => {
                        const matchPlatform = this.activePlatformTab !== 'semua' ? item.platform === this.activePlatformTab : true;
                        const matchPetugas = this.filterPetugas ? item.petugas === this.filterPetugas : true;
                        return matchPlatform && matchPetugas;
                    });
                },

                get stats() {
                    let targetProd = this.globalTargetProduksi;
                    let realProd = this.produksiList.reduce((acc, curr) => acc + (curr.status === 'Selesai' ? curr.jumlahAktual : 0), 0);
                    let targetPub = this.globalTargetPublikasi;
                    let realPub = this.distribusiList.filter(i => i.status === 'Dipublikasikan').length;

                    let persenProd = targetProd > 0 ? Math.round((realProd / targetProd) * 100) : 0;
                    let persenPub = targetPub > 0 ? Math.round((realPub / targetPub) * 100) : 0;

                    let kontenProses = this.produksiList.filter(i => i.status === 'Dalam Proses' || i.status === 'Direncanakan').length;
                    let kontenTertunda = this.produksiList.filter(i => i.status === 'Tertunda').length;

                    return {
                        targetProduksi: targetProd,
                        realisasiProduksi: realProd,
                        targetPublikasi: targetPub,
                        realisasiPublikasi: realPub,
                        persenProduksi: persenProd,
                        persenPublikasi: persenPub,
                        kontenProses: kontenProses,
                        kontenTertunda: kontenTertunda
                    };
                },

                get rekapPetugasList() {
                    return this.daftarPetugas.map(nama => {
                        let pList = this.produksiList.filter(i => i.petugas === nama);
                        let dList = this.distribusiList.filter(i => i.petugas === nama);

                        let targetProd = pList.reduce((a, c) => a + c.targetJumlah, 0);
                        let realProd = pList.reduce((a, c) => a + (c.status === 'Selesai' ? c.jumlahAktual : 0), 0);
                        let persenProd = targetProd > 0 ? Math.round((realProd / targetProd) * 100) : 0;

                        let targetPub = 30; // Estimasi target per petugas
                        let realPub = dList.filter(i => i.status === 'Dipublikasikan').length;
                        let persenPub = targetPub > 0 ? Math.round((realPub / targetPub) * 100) : 0;

                        return {
                            nama: nama,
                            targetProd: targetProd,
                            realProd: realProd,
                            persenProd: persenProd,
                            targetPub: targetPub,
                            realPub: realPub,
                            persenPub: persenPub,
                            yt: dList.filter(i => i.platform === 'YouTube' && i.status === 'Dipublikasikan').length,
                            ig: dList.filter(i => i.platform === 'Instagram' && i.status === 'Dipublikasikan').length,
                            fb: dList.filter(i => i.platform === 'Facebook' && i.status === 'Dipublikasikan').length,
                            tt: dList.filter(i => i.platform === 'TikTok' && i.status === 'Dipublikasikan').length,
                            x: dList.filter(i => i.platform === 'X' && i.status === 'Dipublikasikan').length,
                        };
                    });
                },

                get agendaHarianList() {
                    return this.produksiList.filter(i => i.tglRencana === this.filterTanggalKalender || i.tglReal === this.filterTanggalKalender).map(i => {
                        let pub = this.distribusiList.find(d => d.judul === i.judul);
                        return {
                            id: i.id,
                            petugas: i.petugas,
                            judulProd: i.judul,
                            tglRealProd: i.tglReal,
                            judulPub: pub ? pub.judul : '',
                            tglRealPub: pub ? pub.tglAktual : '',
                            status: i.status,
                            catatan: i.jenis
                        };
                    });
                },

                openProduksiModal() {
                    this.isEditProd = false;
                    this.formProd = {
                        id: Date.now(),
                        noPekerjaan: 'JOB-2026-' + Math.floor(100 + Math.random() * 900),
                        petugas: this.userRole === 'petugas' ? this.currentUser : '',
                        judul: '',
                        jenis: 'Berita',
                        tglRencana: new Date().toISOString().split('T')[0],
                        targetJumlah: 1,
                        tglReal: '',
                        jumlahAktual: 0,
                        status: 'Direncanakan',
                        tautan: ''
                    };
                    this.showProdModal = true;
                },

                editProduksi(item) {
                    this.isEditProd = true;
                    this.formProd = { ...item };
                    this.showProdModal = true;
                },

                saveProduksi() {
                    if (this.isEditProd) {
                        const idx = this.produksiList.findIndex(i => i.id === this.formProd.id);
                        if (idx !== -1) this.produksiList[idx] = { ...this.formProd };
                    } else {
                        this.produksiList.push({ ...this.formProd });
                    }
                    localStorage.setItem('tvri_produksi', JSON.stringify(this.produksiList));
                    this.showProdModal = false;
                    this.renderCharts();
                },

                deleteProduksi(id) {
                    if (confirm('Apakah Anda yakin ingin menghapus data pekerjaan ini?')) {
                        this.produksiList = this.produksiList.filter(i => i.id !== id);
                        localStorage.setItem('tvri_produksi', JSON.stringify(this.produksiList));
                        this.renderCharts();
                    }
                },

                openDistribusiModal() {
                    this.isEditDist = false;
                    this.formDist = {
                        id: Date.now(),
                        platform: 'YouTube',
                        petugas: this.userRole === 'petugas' ? this.currentUser : '',
                        judul: '',
                        format: 'Video Panjang',
                        tglRencana: new Date().toISOString().split('T')[0],
                        tglAktual: '',
                        status: 'Terjadwal',
                        tautan: ''
                    };
                    this.showDistModal = true;
                },

                editDistribusi(item) {
                    this.isEditDist = true;
                    this.formDist = { ...item };
                    this.showDistModal = true;
                },

                saveDistribusi() {
                    if (this.isEditDist) {
                        const idx = this.distribusiList.findIndex(i => i.id === this.formDist.id);
                        if (idx !== -1) this.distribusiList[idx] = { ...this.formDist };
                    } else {
                        this.distribusiList.push({ ...this.formDist });
                    }
                    localStorage.setItem('tvri_distribusi', JSON.stringify(this.distribusiList));
                    this.showDistModal = false;
                    this.renderCharts();
                },

                deleteDistribusi(id) {
                    if (confirm('Apakah Anda yakin ingin menghapus data distribusi ini?')) {
                        this.distribusiList = this.distribusiList.filter(i => i.id !== id);
                        localStorage.setItem('tvri_distribusi', JSON.stringify(this.distribusiList));
                        this.renderCharts();
                    }
                },

                saveSettings() {
                    alert('Pengaturan target berhasil disimpan.');
                },

                printReport() {
                    window.print();
                },

                exportExcel() {
                    let csv = 'Nama Petugas,Target Produksi,Realisasi Produksi,Target Publikasi,Realisasi Publikasi\n';
                    this.rekapPetugasList.forEach(p => {
                        csv += `"${p.nama}",${p.targetProd},${p.realProd},${p.targetPub},${p.realPub}\n`;
                    });
                    const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
                    const link = document.createElement('a');
                    link.href = URL.createObjectURL(blob);
                    link.setAttribute('download', 'Laporan_Kinerja_TVRI_Kalbar.csv');
                    document.body.appendChild(link);
                    link.click();
                    document.body.removeChild(link);
                },

                exportPDF() {
                    window.print();
                },

                renderCharts() {
                    if (this.currentTab !== 'dashboard') return;

                    // Chart 1: Produksi vs Realisasi
                    const ctxProd = document.getElementById('chartProduksi');
                    if (ctxProd) {
                        if (window.myChartProd) window.myChartProd.destroy();
                        window.myChartProd = new Chart(ctxProd, {
                            type: 'bar',
                            data: {
                                labels: ['Minggu 1', 'Minggu 2', 'Minggu 3', 'Minggu 4'],
                                datasets: [
                                    { label: 'Target Produksi', data: [12, 12, 13, 13], backgroundColor: '#1e3a8a' },
                                    { label: 'Realisasi Produksi', data: [10, 12, 11, 9], backgroundColor: '#dc2626' }
                                ]
                            },
                            options: { responsive: true, maintainAspectRatio: false }
                        });
                    }

                    // Chart 2: Publikasi per Platform
                    const ctxPub = document.getElementById('chartPublikasi');
                    if (ctxPub) {
                        if (window.myChartPub) window.myChartPub.destroy();
                        window.myChartPub = new Chart(ctxPub, {
                            type: 'bar',
                            data: {
                                labels: ['YouTube', 'Instagram', 'Facebook', 'TikTok', 'X'],
                                datasets: [
                                    { label: 'Rencana', data: [15, 30, 20, 25, 15], backgroundColor: '#94a3b8' },
                                    { label: 'Publikasi', data: [
                                        this.distribusiList.filter(i=>i.platform==='YouTube' && i.status==='Dipublikasikan').length + 5,
                                        this.distribusiList.filter(i=>i.platform==='Instagram' && i.status==='Dipublikasikan').length + 10,
                                        this.distribusiList.filter(i=>i.platform==='Facebook' && i.status==='Dipublikasikan').length + 8,
                                        this.distribusiList.filter(i=>i.platform==='TikTok' && i.status==='Dipublikasikan').length + 12,
                                        this.distribusiList.filter(i=>i.platform==='X' && i.status==='Dipublikasikan').length + 6
                                    ], backgroundColor: '#2563eb' }
                                ]
                            },
                            options: { responsive: true, maintainAspectRatio: false }
                        });
                    }

                    // Chart 3: Capaian per Petugas
                    const ctxPetugas = document.getElementById('chartPetugas');
                    if (ctxPetugas) {
                        if (window.myChartPetugas) window.myChartPetugas.destroy();
                        window.myChartPetugas = new Chart(ctxPetugas, {
                            type: 'bar',
                            data: {
                                labels: ['Darius', 'Anwar', 'Nicco', 'Meirina', 'Aprillian'],
                                datasets: [
                                    { label: 'Realisasi', data: [8, 10, 7, 9, 8], backgroundColor: '#16a34a' }
                                ]
                            },
                            options: { responsive: true, maintainAspectRatio: false }
                        });
                    }

                    // Chart 4: Donut Platform
                    const ctxPlatform = document.getElementById('chartPlatform');
                    if (ctxPlatform) {
                        if (window.myChartPlatform) window.myChartPlatform.destroy();
                        window.myChartPlatform = new Chart(ctxPlatform, {
                            type: 'doughnut',
                            data: {
                                labels: ['YouTube', 'Instagram', 'Facebook', 'TikTok', 'X'],
                                datasets: [{
                                    data: [
                                        this.distribusiList.filter(i=>i.platform==='YouTube').length,
                                        this.distribusiList.filter(i=>i.platform==='Instagram').length,
                                        this.distribusiList.filter(i=>i.platform==='Facebook').length,
                                        this.distribusiList.filter(i=>i.platform==='TikTok').length,
                                        this.distribusiList.filter(i=>i.platform==='X').length
                                    ],
                                    backgroundColor: ['#dc2626', '#db2777', '#2563eb', '#000000', '#4b5563']
                                }]
                            },
                            options: { responsive: true, maintainAspectRatio: false }
                        });
                    }
                },

                initProduksiCharts() {
                    this.$nextTick(() => { this.renderCharts(); });
                }
            }
        }
    </script>
</body>
</html>
