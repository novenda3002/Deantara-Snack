# Deantara-Snack
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Deantara Snacks - Keuangan & Stok Risol</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chart.js CDN -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#fffbe3',
                            100: '#fef3c7',
                            500: '#f59e0b',
                            600: '#d97706',
                            700: '#b45309',
                            800: '#92400e',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        @media print {
            .no-print { display: none !important; }
            .print-only { display: block !important; }
            body { background: white; color: black; }
            .card-shadow { box-shadow: none !important; border: 1px solid #e5e7eb !important; }
        }
        .print-only { display: none; }
        .tab-btn.active {
            border-bottom-color: #d97706;
            color: #d97706;
            font-weight: 600;
        }
    </style>
</head>
<body class="bg-amber-50/30 text-slate-800 font-sans min-h-screen flex flex-col antialiased">

<!-- Header / Navbar -->
<header class="bg-white border-b border-amber-100 sticky top-0 z-30 no-print shadow-sm">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between">
        <div class="flex items-center gap-3">
            <div class="bg-gradient-to-tr from-amber-600 to-amber-500 text-white p-2.5 rounded-xl flex items-center justify-center shadow-md shadow-amber-500/20">
                <i class="fa-solid fa-cookie-bite text-xl"></i>
            </div>
            <div>
                <h1 class="font-bold text-lg text-slate-900 leading-tight">Deantara Snacks <span class="text-xs bg-amber-100 text-amber-800 font-semibold px-2 py-0.5 rounded-full border border-amber-200">Spesialis Risol</span></h1>
                <p class="text-xs text-slate-500">Pembukuan Keuangan & Stok Opname Risol</p>
            </div>
        </div>
        
        <div class="flex items-center gap-2">
            <button onclick="loadSampleRisolData()" class="text-xs bg-amber-50 hover:bg-amber-100 text-amber-700 font-medium px-3 py-2 rounded-lg transition-colors border border-amber-200 flex items-center gap-1.5">
                <i class="fa-solid fa-wand-magic-sparkles text-amber-600"></i>
                <span class="hidden sm:inline">Muat Data</span> Demo Risol
            </button>
            <button onclick="window.print()" class="bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium px-3 py-2 rounded-lg text-xs transition-colors border border-slate-300 flex items-center gap-1.5">
                <i class="fa-solid fa-print"></i>
                <span class="hidden sm:inline">Cetak</span> Laporan
            </button>
        </div>
    </div>

    <!-- Navigation Tabs -->
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex space-x-6 border-t border-slate-100 text-sm font-medium text-slate-500 overflow-x-auto">
        <button onclick="switchTab('dashboard')" id="tab-btn-dashboard" class="tab-btn active py-3 border-b-2 border-transparent hover:text-amber-600 flex items-center gap-2 whitespace-nowrap">
            <i class="fa-solid fa-chart-pie"></i> Dashboard Usaha
        </button>
        <button onclick="switchTab('transactions')" id="tab-btn-transactions" class="tab-btn py-3 border-b-2 border-transparent hover:text-amber-600 flex items-center gap-2 whitespace-nowrap">
            <i class="fa-solid fa-receipt"></i> Transaksi Kas
        </button>
        <button onclick="switchTab('stock')" id="tab-btn-stock" class="tab-btn py-3 border-b-2 border-transparent hover:text-amber-600 flex items-center gap-2 whitespace-nowrap">
            <i class="fa-solid fa-boxes-stacked"></i> Stok Risol & Opname
        </button>
        <button onclick="switchTab('monthly')" id="tab-btn-monthly" class="tab-btn py-3 border-b-2 border-transparent hover:text-amber-600 flex items-center gap-2 whitespace-nowrap">
            <i class="fa-solid fa-calendar-days"></i> Laporan Bulanan
        </button>
        <button onclick="switchTab('reports')" id="tab-btn-reports" class="tab-btn py-3 border-b-2 border-transparent hover:text-amber-600 flex items-center gap-2 whitespace-nowrap">
            <i class="fa-solid fa-file-invoice-dollar"></i> Laporan Ringkas & Export
        </button>
    </div>
</header>

<!-- Header Saat Dicetak -->
<div class="print-only p-6 border-b border-slate-300 mb-6">
    <h1 class="text-2xl font-bold text-slate-900">Laporan Keuangan & Stok Usaha Risol - Deantara Snacks</h1>
    <p class="text-sm text-slate-600" id="print-date">Dicetak pada: -</p>
</div>

<!-- Main Content -->
<main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 space-y-6">

    <!-- TAB 1: DASHBOARD -->
    <div id="tab-dashboard" class="tab-content space-y-6">
        <!-- Ringkasan Statistik Utama (Terpisah Uang Modal & Omset Penjualan) -->
        <section class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-5 gap-4">
            <!-- Kas Bersih -->
            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm relative overflow-hidden">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-semibold uppercase tracking-wider text-slate-400">Kas Bersih (Tersedia)</p>
                        <h3 class="text-xl font-bold text-slate-900 mt-1" id="stat-saldo">Rp 0</h3>
                    </div>
                    <div class="p-2.5 rounded-xl bg-amber-50 text-amber-600">
                        <i class="fa-solid fa-wallet text-lg"></i>
                    </div>
                </div>
                <p class="mt-3 text-[11px] text-slate-500">(Omset + Modal) - Pengeluaran</p>
            </div>

            <!-- Omset Penjualan -->
            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm relative overflow-hidden">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-semibold uppercase tracking-wider text-slate-400">Omset Penjualan</p>
                        <h3 class="text-xl font-bold text-emerald-600 mt-1" id="stat-omset">Rp 0</h3>
                    </div>
                    <div class="p-2.5 rounded-xl bg-emerald-50 text-emerald-600">
                        <i class="fa-solid fa-cart-shopping text-lg"></i>
                    </div>
                </div>
                <p class="mt-3 text-[11px] text-emerald-600 font-medium">Murni dari penjualan produk</p>
            </div>

            <!-- Suntikan Modal -->
            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm relative overflow-hidden">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-semibold uppercase tracking-wider text-slate-400">Modal Disetor & Sisa</p>
                        <h3 class="text-xl font-bold text-blue-600 mt-1" id="stat-modal">Rp 0</h3>
                    </div>
                    <div class="p-2.5 rounded-xl bg-blue-50 text-blue-600">
                        <i class="fa-solid fa-piggy-bank text-lg"></i>
                    </div>
                </div>
                <p class="mt-3 text-[11px] text-slate-500 flex items-center gap-1" id="stat-modal-status">
                    Sisa modal setelah operasional
                </p>
            </div>

            <!-- Pengeluaran Operasional -->
            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm relative overflow-hidden">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-semibold uppercase tracking-wider text-slate-400">Pengeluaran Operasional</p>
                        <h3 class="text-xl font-bold text-rose-600 mt-1" id="stat-pengeluaran">Rp 0</h3>
                    </div>
                    <div class="p-2.5 rounded-xl bg-rose-50 text-rose-600">
                        <i class="fa-solid fa-arrow-up-right text-lg"></i>
                    </div>
                </div>
                <p class="mt-3 text-[11px] text-rose-600 font-medium">Bahan baku & operasional</p>
            </div>

            <!-- Total Stok Risol -->
            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm relative overflow-hidden">
                <div class="flex justify-between items-start">
                    <div>
                        <p class="text-xs font-semibold uppercase tracking-wider text-slate-400">Total Stok Risol</p>
                        <h3 class="text-xl font-bold text-amber-600 mt-1" id="stat-total-stok">0 Pcs</h3>
                    </div>
                    <div class="p-2.5 rounded-xl bg-amber-50 text-amber-600">
                        <i class="fa-solid fa-cookie text-lg"></i>
                    </div>
                </div>
                <p class="mt-3 text-[11px] text-slate-500 flex items-center gap-1" id="stat-stok-warning">
                    <i class="fa-solid fa-circle-check text-emerald-500"></i> Semua varian stok aman
                </p>
            </div>
        </section>

        <!-- Monitoring Stok Risol Cepat -->
        <section class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm">
            <div class="flex items-center justify-between mb-4">
                <h3 class="font-bold text-slate-900 flex items-center gap-2 text-sm sm:text-base">
                    <i class="fa-solid fa-boxes-stacked text-amber-600"></i>
                    Ringkasan Stok Varian Risol
                </h3>
                <button onclick="switchTab('stock')" class="text-xs text-amber-600 font-semibold hover:underline">Kelola & Restock &rarr;</button>
            </div>
            <div class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-3" id="quick-stock-cards">
                <!-- Cards Stok diisi via JS -->
            </div>
        </section>

        <!-- Visualisasi Charts -->
        <section class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            <div class="lg:col-span-7 bg-white p-5 rounded-2xl border border-amber-100 shadow-sm">
                <h3 class="font-bold text-slate-900 mb-4 text-sm sm:text-base">Tren Omset Penjualan vs Pengeluaran</h3>
                <div class="relative h-64">
                    <canvas id="trendChart"></canvas>
                </div>
            </div>
            <div class="lg:col-span-5 bg-white p-5 rounded-2xl border border-amber-100 shadow-sm">
                <h3 class="font-bold text-slate-900 mb-4 text-sm sm:text-base">Komposisi Pengeluaran Kas</h3>
                <div class="relative h-64">
                    <canvas id="expenseCategoryChart"></canvas>
                </div>
            </div>
        </section>
    </div>

    <!-- TAB 2: TRANSAKSI KAS -->
    <div id="tab-transactions" class="tab-content hidden space-y-6">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            <!-- Form Pencatatan Transaksi -->
            <section class="lg:col-span-5 bg-white p-6 rounded-2xl border border-amber-100 shadow-sm no-print">
                <div class="flex items-center justify-between pb-4 mb-4 border-b border-slate-100">
                    <h2 class="text-base font-bold text-slate-900 flex items-center gap-2">
                        <i class="fa-solid fa-square-plus text-amber-600"></i>
                        <span id="form-title">Tambah Transaksi Kas</span>
                    </h2>
                </div>

                <form id="transaction-form" onsubmit="handleFormSubmit(event)" class="space-y-4">
                    <input type="hidden" id="edit-id" value="">

                    <!-- Radio Jenis Transaksi -->
                    <div>
                        <label class="block text-xs font-medium text-slate-700 mb-1.5">Jenis Transaksi</label>
                        <div class="grid grid-cols-2 gap-3">
                            <label class="cursor-pointer border border-slate-200 rounded-xl p-2.5 flex items-center justify-center gap-2 text-sm font-medium transition-all has-[:checked]:bg-emerald-50 has-[:checked]:border-emerald-500 has-[:checked]:text-emerald-700">
                                <input type="radio" name="type" value="pemasukan" checked onchange="onTypeOrCategoryChange()" class="hidden">
                                <i class="fa-solid fa-circle-arrow-down text-emerald-600"></i> Pemasukan
                            </label>
                            <label class="cursor-pointer border border-slate-200 rounded-xl p-2.5 flex items-center justify-center gap-2 text-sm font-medium transition-all has-[:checked]:bg-rose-50 has-[:checked]:border-rose-500 has-[:checked]:text-rose-700">
                                <input type="radio" name="type" value="pengeluaran" onchange="onTypeOrCategoryChange()" class="hidden">
                                <i class="fa-solid fa-circle-arrow-up text-rose-600"></i> Pengeluaran
                            </label>
                        </div>
                    </div>

                    <!-- Tanggal & Kategori -->
                    <div class="grid grid-cols-1 gap-3">
                        <div>
                            <label for="date" class="block text-xs font-medium text-slate-700 mb-1">Tanggal Transaksi</label>
                            <input type="date" id="date" required class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-amber-500">
                        </div>

                        <!-- Pemilihan Kategori Interaktif -->
                        <div>
                            <div class="flex items-center justify-between mb-1.5">
                                <label class="block text-xs font-medium text-slate-700">Pilih Kategori</label>
                                <span class="text-[11px] text-amber-700 font-semibold" id="selected-category-label">Penjualan Risol Satuan</span>
                            </div>
                            <input type="hidden" id="category" required value="Penjualan Risol Satuan">
                            
                            <div id="category-chips-container" class="grid grid-cols-2 gap-2">
                                <!-- Diisi via JS -->
                            </div>
                        </div>
                    </div>

                    <!-- PANEL DEDIKASI PENJUALAN RISOL -->
                    <div id="risol-sales-panel" class="p-3.5 bg-amber-50/80 border border-amber-200 rounded-xl space-y-3">
                        <div class="flex items-center justify-between border-b border-amber-200/60 pb-2">
                            <span class="text-xs font-bold text-amber-900 flex items-center gap-1.5">
                                <i class="fa-solid fa-bolt text-amber-600"></i> Opsi Penjualan Risol
                            </span>
                            <div class="flex bg-amber-100 p-0.5 rounded-lg text-xs font-medium">
                                <button type="button" id="sale-type-satuan" onclick="setRisolSaleType('satuan')" class="px-2.5 py-1 rounded-md transition-all bg-white text-amber-900 font-bold shadow-xs">Satuan</button>
                                <button type="button" id="sale-type-paket" onclick="setRisolSaleType('paket')" class="px-2.5 py-1 rounded-md transition-all text-amber-700 hover:text-amber-900">Paket Box (3 pcs)</button>
                            </div>
                        </div>

                        <!-- OPSI 1: PENJUALAN SATUAN -->
                        <div id="risol-satuan-container" class="space-y-3">
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                                <div>
                                    <label for="risol-variant-satuan" class="block text-xs font-medium text-amber-800 mb-1">Pilih Varian Risol</label>
                                    <select id="risol-variant-satuan" onchange="calculateRisolTotal()" class="w-full px-3 py-2 bg-white border border-amber-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-amber-500">
                                        <!-- Varian terisi otomatis -->
                                    </select>
                                </div>
                                <div>
                                    <label for="risol-qty-satuan" class="block text-xs font-medium text-amber-800 mb-1">Jumlah Terjual (Pcs)</label>
                                    <input type="number" id="risol-qty-satuan" min="1" value="1" oninput="calculateRisolTotal()" class="w-full px-3 py-2 bg-white border border-amber-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-amber-500">
                                </div>
                            </div>
                        </div>

                        <!-- OPSI 2: PENJUALAN PAKET BOX -->
                        <div id="risol-paket-container" class="space-y-3 hidden">
                            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                                <div>
                                    <label for="risol-qty-paket" class="block text-xs font-medium text-amber-800 mb-1">Jumlah Paket Box</label>
                                    <input type="number" id="risol-qty-paket" min="1" value="1" oninput="calculateRisolTotal()" class="w-full px-3 py-2 bg-white border border-amber-300 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-amber-500">
                                </div>
                                <div class="flex items-end">
                                    <button type="button" onclick="openVariantPickerModal()" class="w-full bg-amber-600 hover:bg-amber-700 text-white font-semibold py-2 px-3 rounded-xl text-xs flex items-center justify-center gap-1.5 shadow-sm transition-colors">
                                        <i class="fa-solid fa-list-check"></i>
                                        <span>Pilih Isi Custom (3 Pcs)</span>
                                    </button>
                                </div>
                            </div>
                            <div id="paket-summary-badge" class="p-2 bg-amber-100/70 border border-amber-200 rounded-lg text-xs text-amber-800 font-medium">
                                Belum memilih isi kombinasi varian.
                            </div>
                        </div>

                        <p class="text-xs text-amber-700 font-medium" id="risol-calc-preview">Harga Satuan: Rp 4.000 | Total: Rp 4.000</p>
                    </div>

                    <!-- Jumlah Nominal Uang -->
                    <div>
                        <label for="amount" class="block text-xs font-medium text-slate-700 mb-1">Total Nominal Uang (Rp)</label>
                        <input type="number" id="amount" min="1" placeholder="Contoh: 50000" required class="w-full px-3.5 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm font-semibold focus:outline-none focus:ring-2 focus:ring-amber-500">
                    </div>

                    <!-- Keterangan -->
                    <div>
                        <label for="description" class="block text-xs font-medium text-slate-700 mb-1">Keterangan Catatan</label>
                        <textarea id="description" rows="2" placeholder="Catatan transaksi (misal: Pembelian Tepung Breadcrumb 5kg)" class="w-full px-3.5 py-2 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-amber-500 resize-none"></textarea>
                    </div>

                    <button type="submit" id="submit-btn" class="w-full bg-amber-600 hover:bg-amber-700 text-white font-semibold py-3 px-4 rounded-xl shadow-md shadow-amber-600/20 transition-all flex items-center justify-center gap-2">
                        <i class="fa-solid fa-save"></i>
                        <span>Simpan Transaksi</span>
                    </button>
                </form>
            </section>

            <!-- Tabel Riwayat Transaksi Kas -->
            <section class="lg:col-span-7 bg-white rounded-2xl border border-amber-100 shadow-sm overflow-hidden flex flex-col justify-between">
                <div>
                    <div class="p-5 border-b border-slate-100 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-3">
                        <h2 class="font-bold text-slate-900 flex items-center gap-2">
                            <i class="fa-solid fa-list-check text-amber-600"></i> Riwayat Kas Usaha
                        </h2>
                        <div class="flex items-center gap-2 w-full sm:w-auto">
                            <input type="text" id="search-input" oninput="renderApp()" placeholder="Cari..." class="px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-xl text-xs w-full sm:w-40 focus:outline-none focus:ring-2 focus:ring-amber-500">
                            <select id="filter-type" onchange="renderApp()" class="px-3 py-1.5 bg-slate-50 border border-slate-200 rounded-xl text-xs focus:outline-none focus:ring-2 focus:ring-amber-500">
                                <option value="all">Semua Jenis</option>
                                <option value="pemasukan">Pemasukan</option>
                                <option value="pengeluaran">Pengeluaran</option>
                            </select>
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-slate-50 text-slate-500 text-xs uppercase font-semibold border-b border-slate-200">
                                    <th class="p-3">Tanggal</th>
                                    <th class="p-3">Jenis/Kategori</th>
                                    <th class="p-3">Keterangan</th>
                                    <th class="p-3 text-right">Jumlah</th>
                                    <th class="p-3 text-center no-print">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="transaction-list" class="divide-y divide-slate-100 text-xs">
                                <!-- Baris Tabel JavaScript -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </section>
        </div>
    </div>

    <!-- TAB 3: STOK RISOL & STOCK OPNAME -->
    <div id="tab-stock" class="tab-content hidden space-y-6">
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">

            <!-- Kelola Varian & Restock/Opname -->
            <section class="lg:col-span-8 bg-white p-6 rounded-2xl border border-amber-100 shadow-sm space-y-6">
                <div class="flex items-center justify-between border-b border-slate-100 pb-3">
                    <div>
                        <h2 class="font-bold text-slate-900 flex items-center gap-2 text-base">
                            <i class="fa-solid fa-cookie-bite text-amber-600"></i> Daftar Varian Risol & Stok
                        </h2>
                        <p class="text-xs text-slate-500 mt-0.5">Kelola stok, restock produksi harian, dan lakukan stock opname langsung di tiap varian.</p>
                    </div>
                    <button onclick="toggleVariantForm()" class="bg-amber-600 hover:bg-amber-700 text-white text-xs px-3 py-2 rounded-xl font-medium transition-colors shadow-sm flex items-center gap-1.5">
                        <i class="fa-solid fa-plus"></i> Varian Baru
                    </button>
                </div>

                <!-- Form Tambah / Edit Varian -->
                <form id="variant-form" onsubmit="handleVariantSubmit(event)" class="hidden p-4 bg-amber-50/50 border border-amber-200 rounded-xl space-y-3">
                    <input type="hidden" id="variant-edit-id" value="">
                    <h4 class="text-xs font-bold text-amber-900" id="variant-form-title">Tambah Varian Risol Baru</h4>
                    <div>
                        <label class="block text-xs text-slate-600 mb-1">Nama Varian Risol</label>
                        <input type="text" id="variant-name" placeholder="misal: Risol Rogout Ayam" required class="w-full px-3 py-1.5 bg-white border border-slate-200 rounded-lg text-xs">
                    </div>
                    <div class="grid grid-cols-3 gap-2">
                        <div>
                            <label class="block text-xs text-slate-600 mb-1">Harga Jual (Rp)</label>
                            <input type="number" id="variant-price" min="0" placeholder="4000" required class="w-full px-3 py-1.5 bg-white border border-slate-200 rounded-lg text-xs">
                        </div>
                        <div>
                            <label class="block text-xs text-slate-600 mb-1">Stok Awal (Pcs)</label>
                            <input type="number" id="variant-stock" min="0" placeholder="45" required class="w-full px-3 py-1.5 bg-white border border-slate-200 rounded-lg text-xs">
                        </div>
                        <div>
                            <label class="block text-xs text-slate-600 mb-1">Stok Min (Pcs)</label>
                            <input type="number" id="variant-min" min="0" placeholder="12" required class="w-full px-3 py-1.5 bg-white border border-slate-200 rounded-lg text-xs">
                        </div>
                    </div>
                    <div class="flex justify-end gap-2 pt-1">
                        <button type="button" onclick="toggleVariantForm()" class="px-3 py-1 bg-slate-200 text-slate-700 text-xs rounded-lg">Batal</button>
                        <button type="submit" class="px-3 py-1 bg-amber-600 text-white text-xs rounded-lg font-semibold">Simpan Varian</button>
                    </div>
                </form>

                <!-- Tabel Varian Risol -->
                <div class="overflow-x-auto">
                    <table class="w-full text-left text-xs border-collapse">
                        <thead>
                            <tr class="bg-slate-50 text-slate-500 font-semibold border-b border-slate-200">
                                <th class="p-3">Varian Risol</th>
                                <th class="p-3">Harga</th>
                                <th class="p-3">Stok Sistem</th>
                                <th class="p-3 text-center">Aksi Restock & Opname</th>
                                <th class="p-3 text-center">Edit / Hapus</th>
                            </tr>
                        </thead>
                        <tbody id="variant-list-body" class="divide-y divide-slate-100">
                            <!-- Generated JS -->
                        </tbody>
                    </table>
                </div>
            </section>

            <!-- Riwayat Stock Opname -->
            <section class="lg:col-span-4 bg-white p-6 rounded-2xl border border-amber-100 shadow-sm space-y-4">
                <div class="border-b border-slate-100 pb-3">
                    <h2 class="font-bold text-slate-900 flex items-center gap-2 text-sm">
                        <i class="fa-solid fa-clock-rotate-left text-amber-600"></i> Riwayat Penyesuaian / Opname
                    </h2>
                    <p class="text-[11px] text-slate-500 mt-0.5">Catatan fisik, selisih hasil opname, & alasan penyesuaian.</p>
                </div>

                <div class="max-h-[380px] overflow-y-auto space-y-2 pr-1" id="opname-logs-container">
                    <!-- Log diisi JS -->
                </div>
            </section>

        </div>
    </div>

    <!-- TAB 4: LAPORAN BULANAN (OMSET, KEUNTUNGAN & RUGI-LABA) -->
    <div id="tab-monthly" class="tab-content hidden space-y-6">
        <!-- Header Filter Bulan -->
        <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
            <div>
                <h2 class="font-bold text-slate-900 text-base flex items-center gap-2">
                    <i class="fa-solid fa-calendar-days text-amber-600"></i> Laporan Rekapitulasi Keuangan Bulanan
                </h2>
                <p class="text-xs text-slate-500 mt-0.5">Pantau omset murni, biaya operasional, pengurangan modal, dan laba/rugi bersih secara aktual.</p>
            </div>
            <div class="flex items-center gap-2 w-full sm:w-auto">
                <label for="monthly-select" class="text-xs text-slate-600 font-semibold whitespace-nowrap">Pilih Periode:</label>
                <select id="monthly-select" onchange="renderMonthlyReport()" class="px-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs font-bold focus:outline-none focus:ring-2 focus:ring-amber-500">
                    <!-- Filled dynamically -->
                </select>
            </div>
        </div>

        <!-- Kartu Omset & Profit Bulanan -->
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm">
                <p class="text-xs font-semibold uppercase text-slate-400">Total Omset Penjualan</p>
                <h3 class="text-2xl font-bold text-emerald-600 mt-1" id="monthly-omset">Rp 0</h3>
                <p class="text-[11px] text-slate-400 mt-2">Pemasukan murni dari penjualan</p>
            </div>

            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm">
                <p class="text-xs font-semibold uppercase text-slate-400">Modal Disetor & Sisa Modal</p>
                <h3 class="text-2xl font-bold text-blue-600 mt-1" id="monthly-modal">Rp 0</h3>
                <p class="text-[11px] font-semibold mt-2" id="monthly-modal-subtext">Disetor: Rp 0</p>
            </div>

            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm">
                <p class="text-xs font-semibold uppercase text-slate-400">Total Biaya Operasional</p>
                <h3 class="text-2xl font-bold text-rose-600 mt-1" id="monthly-expense">Rp 0</h3>
                <p class="text-[11px] text-slate-400 mt-2">Total pengeluaran bulan ini</p>
            </div>

            <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm">
                <p class="text-xs font-semibold uppercase text-slate-400">Laba / Rugi Bersih</p>
                <h3 class="text-2xl font-bold text-slate-900 mt-1" id="monthly-net-profit">Rp 0</h3>
                <p class="text-[11px] font-semibold mt-2" id="monthly-profit-status">Status: -</p>
            </div>
        </div>

        <!-- Bar Chart Komparasi Omset vs Laba Bulanan -->
        <div class="bg-white p-5 rounded-2xl border border-amber-100 shadow-sm space-y-4">
            <h3 class="font-bold text-slate-900 text-sm sm:text-base">Grafik Komparasi Omset & Keuntungan Bulanan</h3>
            <div class="relative h-72">
                <canvas id="monthlyBarChart"></canvas>
            </div>
        </div>

        <!-- Tabel Histori Performa Per Bulan -->
        <div class="bg-white rounded-2xl border border-amber-100 shadow-sm overflow-hidden space-y-3">
            <div class="p-4 border-b border-slate-100">
                <h3 class="font-bold text-slate-900 text-sm">Tabel Perbandingan Bulan ke Bulan</h3>
            </div>
            <div class="overflow-x-auto">
                <table class="w-full text-left border-collapse text-xs">
                    <thead>
                        <tr class="bg-slate-50 text-slate-500 font-semibold border-b border-slate-200">
                            <th class="p-3">Periode Bulan</th>
                            <th class="p-3 text-right">Omset Penjualan</th>
                            <th class="p-3 text-right">Modal Disetor</th>
                            <th class="p-3 text-right">Beban Operasional</th>
                            <th class="p-3 text-right">Laba / Rugi Bersih</th>
                            <th class="p-3 text-center">Margin Keuntungan</th>
                        </tr>
                    </thead>
                    <tbody id="monthly-history-table" class="divide-y divide-slate-100">
                        <!-- Diisi via JS -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- TAB 5: LAPORAN RINGKAS & EXPORT -->
    <div id="tab-reports" class="tab-content hidden space-y-6">
        <div class="bg-white p-6 rounded-2xl border border-amber-100 shadow-sm space-y-4">
            <h2 class="font-bold text-slate-900 flex items-center gap-2">
                <i class="fa-solid fa-file-invoice-dollar text-amber-600"></i> Laporan Ringkasan Total & Ekspor Data
            </h2>
            <div class="grid grid-cols-1 sm:grid-cols-4 gap-4 pt-2">
                <div class="p-4 bg-emerald-50 rounded-xl border border-emerald-100">
                    <p class="text-xs text-emerald-700 font-medium">Total Omset Penjualan</p>
                    <h4 class="text-xl font-bold text-emerald-800 mt-1" id="report-income">Rp 0</h4>
                </div>
                <div class="p-4 bg-blue-50 rounded-xl border border-blue-100">
                    <p class="text-xs text-blue-700 font-medium">Total Suntikan Modal</p>
                    <h4 class="text-xl font-bold text-blue-800 mt-1" id="report-modal">Rp 0</h4>
                </div>
                <div class="p-4 bg-rose-50 rounded-xl border border-rose-100">
                    <p class="text-xs text-rose-700 font-medium">Total Pengeluaran Kas</p>
                    <h4 class="text-xl font-bold text-rose-800 mt-1" id="report-expense">Rp 0</h4>
                </div>
                <div class="p-4 bg-amber-50 rounded-xl border border-amber-100">
                    <p class="text-xs text-amber-700 font-medium">Laba/Rugi Murni Usaha</p>
                    <h4 class="text-xl font-bold text-amber-800 mt-1" id="report-profit">Rp 0</h4>
                </div>
            </div>
            <div class="flex flex-wrap gap-3 pt-4 border-t border-slate-100">
                <button onclick="exportToCSV()" class="bg-emerald-600 hover:bg-emerald-700 text-white font-medium px-4 py-2.5 rounded-xl text-xs flex items-center gap-2 transition-all shadow-sm">
                    <i class="fa-solid fa-file-excel"></i> Export CSV Kas
                </button>
                <button onclick="exportStockCSV()" class="bg-amber-600 hover:bg-amber-700 text-white font-medium px-4 py-2.5 rounded-xl text-xs flex items-center gap-2 transition-all shadow-sm">
                    <i class="fa-solid fa-boxes-stacked"></i> Export CSV Stok Risol
                </button>
                <button onclick="window.print()" class="bg-slate-800 hover:bg-slate-900 text-white font-medium px-4 py-2.5 rounded-xl text-xs flex items-center gap-2 transition-all shadow-sm">
                    <i class="fa-solid fa-print"></i> Cetak Laporan Lengkap
                </button>
            </div>
        </div>
    </div>

</main>

<!-- Modal Restock & Stock Opname Terintegrasi -->
<div id="restock-opname-modal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center hidden no-print p-4">
    <div class="bg-white rounded-2xl max-w-md w-full p-6 shadow-xl space-y-4">
        <div class="flex justify-between items-center border-b border-slate-100 pb-3">
            <h3 class="font-bold text-slate-900 text-base flex items-center gap-2" id="modal-title">
                <i class="fa-solid fa-boxes-packing text-amber-600"></i> Restock / Stock Opname
            </h3>
            <button onclick="closeRestockModal()" class="text-slate-400 hover:text-slate-600">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>
        </div>
        
        <form id="modal-action-form" onsubmit="handleModalSubmit(event)" class="space-y-4">
            <input type="hidden" id="modal-variant-id" value="">
            
            <div>
                <p class="text-xs text-slate-500">Varian Risol:</p>
                <p class="text-sm font-bold text-amber-800" id="modal-variant-name">-</p>
            </div>

            <!-- Mode Selection -->
            <div class="grid grid-cols-2 gap-2 bg-slate-100 p-1 rounded-xl text-xs font-semibold">
                <button type="button" id="mode-restock-btn" onclick="setModalMode('restock')" class="py-2 rounded-lg bg-white text-amber-800 shadow-xs">
                    + Tambah Stok (Produksi)
                </button>
                <button type="button" id="mode-opname-btn" onclick="setModalMode('opname')" class="py-2 rounded-lg text-slate-600">
                    <i class="fa-solid fa-clipboard-check"></i> Stock Opname Physical
                </button>
            </div>

            <!-- Fields for Restock -->
            <div id="restock-fields" class="space-y-3">
                <div>
                    <label class="block text-xs font-medium text-slate-700 mb-1">Jumlah Tambah Stok (Pcs)</label>
                    <input type="number" id="modal-add-qty" min="1" value="20" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-sm font-bold">
                </div>
            </div>

            <!-- Fields for Opname -->
            <div id="opname-fields" class="space-y-3 hidden">
                <div>
                    <label class="block text-xs font-medium text-slate-700 mb-1">Stok Fisik Sebenarnya (Pcs)</label>
                    <input type="number" id="modal-actual-qty" min="0" value="0" oninput="updateOpnameDiff()" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-sm font-bold">
                </div>
                <div class="p-2.5 bg-slate-50 rounded-xl border border-slate-200 text-xs flex justify-between items-center">
                    <span class="text-slate-500">Stok Sistem Saat Ini: <strong id="modal-system-qty">0</strong> pcs</span>
                    <span id="modal-diff-badge" class="font-bold px-2 py-0.5 rounded text-slate-700">Selisih: 0</span>
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-700 mb-1">Alasan Penyesuaian Opname</label>
                    <select id="modal-opname-reason" class="w-full px-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs">
                        <option value="Hasil Hitung Rutin">Hasil Hitung Rutin</option>
                        <option value="Risol Rusak / Pecah Kulit">Risol Rusak / Pecah Kulit</option>
                        <option value="Basi / Kadaluarsa">Basi / Kadaluarsa</option>
                        <option value="Bonus / Sampling Gratis">Bonus / Sampling Gratis</option>
                        <option value="Selisih Pembukuan">Selisih Pembukuan</option>
                    </select>
                </div>
            </div>

            <button type="submit" class="w-full bg-amber-600 hover:bg-amber-700 text-white font-semibold py-2.5 rounded-xl text-xs transition-colors shadow-sm">
                Simpan Perubahan Stok
            </button>
        </form>
    </div>
</div>

<!-- Modal Custom Paket Box Picker -->
<div id="custom-paket-modal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center hidden no-print p-4">
    <div class="bg-white rounded-2xl max-w-md w-full p-6 shadow-xl space-y-4">
        <div class="flex justify-between items-center border-b border-slate-100 pb-3">
            <h3 class="font-bold text-slate-900 text-base flex items-center gap-2">
                <i class="fa-solid fa-box text-amber-600"></i> Pilih Varian Paket Box (Total 3 Pcs)
            </h3>
            <button onclick="closeCustomPaketModal()" class="text-slate-400 hover:text-slate-600">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>
        </div>

        <!-- Item selector container -->
        <div id="paket-items-selector" class="space-y-2 max-h-60 overflow-y-auto">
            <!-- Varian & pengatur jumlah akan diisi secara dinamis via JS -->
        </div>

        <div class="flex items-center justify-between border-t border-slate-100 pt-3">
            <span class="text-xs text-slate-500">Total Dipilih: <strong id="paket-total-count" class="text-amber-700">0 / 3 Pcs</strong></span>
            <div class="flex gap-2">
                <button type="button" onclick="closeCustomPaketModal()" class="px-3 py-1.5 bg-slate-100 text-slate-700 hover:bg-slate-200 rounded-xl text-xs font-semibold">
                    Batal
                </button>
                <button type="button" onclick="saveCustomPaketSelection()" class="px-3 py-1.5 bg-amber-600 text-white hover:bg-amber-700 rounded-xl text-xs font-semibold">
                    Simpan Kombinasi
                </button>
            </div>
        </div>
    </div>
</div>

<!-- Modal Konfirmasi Hapus Transaksi -->
<div id="delete-confirm-modal" class="fixed inset-0 bg-slate-900/50 backdrop-blur-sm z-50 flex items-center justify-center hidden no-print p-4">
    <div class="bg-white rounded-2xl max-w-sm w-full p-6 shadow-xl space-y-4 text-center">
        <div class="w-12 h-12 bg-rose-100 text-rose-600 rounded-full flex items-center justify-center mx-auto text-xl">
            <i class="fa-solid fa-trash-can"></i>
        </div>
        <div>
            <h3 class="font-bold text-slate-900 text-base">Hapus Transaksi Kas?</h3>
            <p class="text-xs text-slate-500 mt-1">Apakah Anda yakin ingin menghapus transaksi ini? Data kas dan stok yang terkait akan disesuaikan kembali.</p>
        </div>
        <div class="p-3 bg-slate-50 rounded-xl border border-slate-200 text-left text-xs space-y-1">
            <p id="delete-info-category" class="font-bold text-slate-800">-</p>
            <p id="delete-info-desc" class="text-slate-500 truncate">-</p>
            <p id="delete-info-amount" class="font-semibold text-rose-600">-</p>
        </div>
        <div class="flex gap-2 pt-2">
            <button type="button" onclick="closeDeleteModal()" class="flex-1 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold rounded-xl text-xs transition-colors">
                Batal
            </button>
            <button type="button" id="confirm-delete-btn" class="flex-1 py-2.5 bg-rose-600 hover:bg-rose-700 text-white font-semibold rounded-xl text-xs transition-colors shadow-sm">
                Ya, Hapus
            </button>
        </div>
    </div>
</div>

<!-- Message Alert Toast -->
<div id="toast-msg" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 pointer-events-none bg-slate-900 text-white px-4 py-3 rounded-xl shadow-lg text-xs flex items-center gap-2 border border-slate-700">
    <i class="fa-solid fa-circle-info text-amber-400"></i>
    <span id="toast-text">Notifikasi</span>
</div>

<!-- JAVASCRIPT APPLICATION LOGIC -->
<script>
    // DATA STATE & CATEGORIES
    const CATEGORIES = {
        pemasukan: ['Penjualan Risol Satuan', 'Penjualan Paket Box', 'Suntikan Modal', 'Lain-lain'],
        pengeluaran: ['Bahan Baku (Tepung, Telur, dll)', 'Gas & Listrik', 'Kemasan & Plastik', 'Gaji & Operasional', 'Lain-lain']
    };

    let variants = JSON.parse(localStorage.getItem('deantara_variants')) || [
        { id: 'v1', name: 'Risol Mayo Smoke Beef', price: 4000, stock: 50, minStock: 10 },
        { id: 'v2', name: 'Risol Ragout Ayam', price: 4000, stock: 40, minStock: 10 },
        { id: 'v3', name: 'Risol Melted Cheese Chicken', price: 4000, stock: 35, minStock: 8 },
        { id: 'v4', name: 'Risol Sayur Telur', price: 3500, stock: 25, minStock: 10 }
    ];

    let transactions = JSON.parse(localStorage.getItem('deantara_transactions')) || [];
    let opnameLogs = JSON.parse(localStorage.getItem('deantara_opname_logs')) || [];

    // State Temp untuk Penjualan Risol & Modals
    let currentRisolSaleType = 'satuan';
    let customPaketSelection = {};
    let currentModalMode = 'restock';
    let transactionToDeleteId = null;

    // Charts
    let trendChartObj = null;
    let expenseChartObj = null;
    let monthlyBarChartObj = null;

    // INIT ON LOAD
    window.onload = function() {
        const dateInput = document.getElementById('date');
        if (dateInput) dateInput.value = new Date().toISOString().split('T')[0];
        
        const printDateElem = document.getElementById('print-date');
        if (printDateElem) printDateElem.innerText = 'Dicetak pada: ' + new Date().toLocaleString('id-ID');
        
        renderCategoryChips('pemasukan');
        populateRisolVariantDropdown();
        renderApp();
        initCharts();
    };

    // NAVIGATION TABS
    function switchTab(tabName) {
        document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
        document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));

        const targetTab = document.getElementById('tab-' + tabName);
        const targetBtn = document.getElementById('tab-btn-' + tabName);
        if (targetTab) targetTab.classList.remove('hidden');
        if (targetBtn) targetBtn.classList.add('active');

        if (tabName === 'dashboard') {
            updateCharts();
        } else if (tabName === 'monthly') {
            renderMonthlyReport();
        }
    }

    // SAVE STATE TO LOCAL STORAGE
    function saveData() {
        localStorage.setItem('deantara_variants', JSON.stringify(variants));
        localStorage.setItem('deantara_transactions', JSON.stringify(transactions));
        localStorage.setItem('deantara_opname_logs', JSON.stringify(opnameLogs));
        renderApp();
    }

    // TOAST MESSAGE
    function showToast(msg) {
        const toast = document.getElementById('toast-msg');
        if (!toast) return;
        document.getElementById('toast-text').innerText = msg;
        toast.classList.remove('translate-y-20', 'opacity-0');
        setTimeout(() => {
            toast.classList.add('translate-y-20', 'opacity-0');
        }, 3000);
    }

    // RENDER CATEGORY CHIPS
    function renderCategoryChips(type) {
        const container = document.getElementById('category-chips-container');
        if (!container) return;
        const list = CATEGORIES[type] || [];
        container.innerHTML = '';

        list.forEach((cat, idx) => {
            const isFirst = idx === 0;
            const btn = document.createElement('button');
            btn.type = 'button';
            btn.className = `cat-chip px-2.5 py-2 text-xs rounded-xl border text-left transition-all font-medium flex items-center justify-between ${
                isFirst ? 'bg-amber-100 border-amber-500 text-amber-900 font-bold' : 'bg-slate-50 border-slate-200 text-slate-600 hover:bg-slate-100'
            }`;
            btn.innerHTML = `<span>${cat}</span>`;
            btn.onclick = () => selectCategory(cat, btn);
            container.appendChild(btn);

            if (isFirst) {
                document.getElementById('category').value = cat;
                document.getElementById('selected-category-label').innerText = cat;
                toggleRisolPanelVisibility(cat);
            }
        });
    }

    function selectCategory(cat, btnElement) {
        document.querySelectorAll('.cat-chip').forEach(b => {
            b.className = 'cat-chip px-2.5 py-2 text-xs rounded-xl border text-left transition-all font-medium flex items-center justify-between bg-slate-50 border-slate-200 text-slate-600 hover:bg-slate-100';
        });
        btnElement.className = 'cat-chip px-2.5 py-2 text-xs rounded-xl border text-left transition-all font-medium flex items-center justify-between bg-amber-100 border-amber-500 text-amber-900 font-bold';
        
        document.getElementById('category').value = cat;
        document.getElementById('selected-category-label').innerText = cat;
        toggleRisolPanelVisibility(cat);
    }

    function onTypeOrCategoryChange() {
        const checkedRadio = document.querySelector('input[name="type"]:checked');
        if (checkedRadio) {
            renderCategoryChips(checkedRadio.value);
        }
    }

    function toggleRisolPanelVisibility(categoryName) {
        const panel = document.getElementById('risol-sales-panel');
        if (!panel) return;
        if (categoryName.includes('Risol') || categoryName.includes('Box')) {
            panel.classList.remove('hidden');
            calculateRisolTotal();
        } else {
            panel.classList.add('hidden');
        }
    }

    // RISOL SALES SPECIFIC LOGIC
    function setRisolSaleType(type) {
        currentRisolSaleType = type;
        const btnSatuan = document.getElementById('sale-type-satuan');
        const btnPaket = document.getElementById('sale-type-paket');
        const containerSatuan = document.getElementById('risol-satuan-container');
        const containerPaket = document.getElementById('risol-paket-container');

        if (type === 'satuan') {
            if (btnSatuan) btnSatuan.className = 'px-2.5 py-1 rounded-md transition-all bg-white text-amber-900 font-bold shadow-xs';
            if (btnPaket) btnPaket.className = 'px-2.5 py-1 rounded-md transition-all text-amber-700 hover:text-amber-900';
            if (containerSatuan) containerSatuan.classList.remove('hidden');
            if (containerPaket) containerPaket.classList.add('hidden');
        } else {
            if (btnPaket) btnPaket.className = 'px-2.5 py-1 rounded-md transition-all bg-white text-amber-900 font-bold shadow-xs';
            if (btnSatuan) btnSatuan.className = 'px-2.5 py-1 rounded-md transition-all text-amber-700 hover:text-amber-900';
            if (containerPaket) containerPaket.classList.remove('hidden');
            if (containerSatuan) containerSatuan.classList.add('hidden');
        }
        calculateRisolTotal();
    }

    function populateRisolVariantDropdown() {
        const select = document.getElementById('risol-variant-satuan');
        if (!select) return;
        select.innerHTML = '';
        variants.forEach(v => {
            const opt = document.createElement('option');
            opt.value = v.id;
            opt.innerText = `${v.name} - Rp ${v.price.toLocaleString('id-ID')} (Stok: ${v.stock})`;
            select.appendChild(opt);
        });
    }

    function calculateRisolTotal() {
        const catElem = document.getElementById('category');
        if (!catElem) return;
        const category = catElem.value;
        if (!category.includes('Risol') && !category.includes('Box')) return;

        const amountInput = document.getElementById('amount');
        const preview = document.getElementById('risol-calc-preview');

        if (currentRisolSaleType === 'satuan') {
            const variantId = document.getElementById('risol-variant-satuan').value;
            const qty = parseInt(document.getElementById('risol-qty-satuan').value) || 0;
            const variant = variants.find(v => v.id === variantId);

            if (variant) {
                const total = variant.price * qty;
                if (amountInput) amountInput.value = total;
                if (preview) preview.innerText = `Harga ${variant.name}: Rp ${variant.price.toLocaleString('id-ID')} x ${qty} pcs = Rp ${total.toLocaleString('id-ID')}`;
            }
        } else {
            const qtyBox = parseInt(document.getElementById('risol-qty-paket').value) || 0;
            const total = qtyBox * 12000;
            if (amountInput) amountInput.value = total;
            if (preview) preview.innerText = `Paket Box (3 Pcs): Rp 12.000 x ${qtyBox} Box = Rp ${total.toLocaleString('id-ID')}`;
        }
    }

    // CUSTOM PAKET MODAL PICKER
    function openVariantPickerModal() {
        const container = document.getElementById('paket-items-selector');
        if (!container) return;
        container.innerHTML = '';

        variants.forEach(v => {
            const currentQty = customPaketSelection[v.id] || 0;
            const div = document.createElement('div');
            div.className = 'flex items-center justify-between p-2.5 bg-slate-50 border border-slate-200 rounded-xl';
            div.innerHTML = `
                <div>
                    <p class="text-xs font-bold text-slate-800">${v.name}</p>
                    <p class="text-[10px] text-slate-500">Stok: ${v.stock} pcs</p>
                </div>
                <div class="flex items-center gap-2">
                    <button type="button" onclick="adjustPaketItemQty('${v.id}', -1)" class="w-7 h-7 bg-white border border-slate-300 rounded-lg font-bold text-xs">-</button>
                    <span id="paket-qty-${v.id}" class="text-xs font-bold w-4 text-center">${currentQty}</span>
                    <button type="button" onclick="adjustPaketItemQty('${v.id}', 1)" class="w-7 h-7 bg-white border border-slate-300 rounded-lg font-bold text-xs">+</button>
                </div>
            `;
            container.appendChild(div);
        });

        updatePaketTotalCount();
        document.getElementById('custom-paket-modal').classList.remove('hidden');
    }

    function adjustPaketItemQty(variantId, change) {
        let current = customPaketSelection[variantId] || 0;
        current = Math.max(0, current + change);

        let sum = 0;
        for (let id in customPaketSelection) {
            if (id !== variantId) sum += customPaketSelection[id];
        }

        if (sum + current > 3) {
            showToast('Maksimal 3 pcs risol per 1 Paket Box!');
            return;
        }

        customPaketSelection[variantId] = current;
        const badge = document.getElementById(`paket-qty-${variantId}`);
        if (badge) badge.innerText = current;

        updatePaketTotalCount();
    }

    function updatePaketTotalCount() {
        let total = 0;
        for (let id in customPaketSelection) {
            total += customPaketSelection[id];
        }
        const cnt = document.getElementById('paket-total-count');
        if (cnt) cnt.innerText = `${total} / 3 Pcs`;
    }

    function closeCustomPaketModal() {
        document.getElementById('custom-paket-modal').classList.add('hidden');
    }

    function saveCustomPaketSelection() {
        let total = 0;
        let summaryText = [];

        for (let id in customPaketSelection) {
            const q = customPaketSelection[id];
            if (q > 0) {
                total += q;
                const v = variants.find(x => x.id === id);
                if (v) summaryText.push(`${q} ${v.name}`);
            }
        }

        if (total !== 3) {
            showToast('Harap pilih tepat 3 pcs risol untuk 1 paket box!');
            return;
        }

        document.getElementById('paket-summary-badge').innerText = 'Kombinasi per Box: ' + summaryText.join(', ');
        closeCustomPaketModal();
        showToast('Kombinasi paket berhasil disimpan!');
    }

    // TRANSACTION FORM SUBMISSION
    function handleFormSubmit(e) {
        e.preventDefault();
        const type = document.querySelector('input[name="type"]:checked').value;
        const date = document.getElementById('date').value;
        const category = document.getElementById('category').value;
        const amount = parseFloat(document.getElementById('amount').value) || 0;
        let description = document.getElementById('description').value;

        if (amount <= 0) {
            showToast('Nominal harus lebih besar dari Rp 0');
            return;
        }

        // DEDICATED STOCK DEDUCTION LOGIC FOR RISOL SALES
        if (type === 'pemasukan' && (category.includes('Risol') || category.includes('Box'))) {
            if (currentRisolSaleType === 'satuan') {
                const variantId = document.getElementById('risol-variant-satuan').value;
                const qty = parseInt(document.getElementById('risol-qty-satuan').value) || 0;
                const variant = variants.find(v => v.id === variantId);

                if (variant) {
                    if (variant.stock < qty) {
                        showToast(`Stok ${variant.name} tidak mencukupi! (Sisa: ${variant.stock})`);
                        return;
                    }
                    variant.stock -= qty;
                    description = `[Penjualan ${qty} Pcs ${variant.name}] ` + description;
                }
            } else if (currentRisolSaleType === 'paket') {
                const boxQty = parseInt(document.getElementById('risol-qty-paket').value) || 0;
                let totalPcs = boxQty * 3;
                let descList = [];

                let hasCustom = false;
                for (let id in customPaketSelection) {
                    if (customPaketSelection[id] > 0) hasCustom = true;
                }

                if (hasCustom) {
                    for (let id in customPaketSelection) {
                        const pcsPerBox = customPaketSelection[id];
                        if (pcsPerBox > 0) {
                            const totalToDeduct = pcsPerBox * boxQty;
                            const v = variants.find(x => x.id === id);
                            if (v) {
                                if (v.stock < totalToDeduct) {
                                    showToast(`Stok ${v.name} tidak cukup untuk ${boxQty} Paket Box!`);
                                    return;
                                }
                                v.stock -= totalToDeduct;
                                descList.push(`${totalToDeduct} Pcs ${v.name}`);
                            }
                        }
                    }
                } else {
                    const firstVar = variants[0];
                    if (firstVar) {
                        if (firstVar.stock < totalPcs) {
                            showToast(`Stok ${firstVar.name} tidak cukup!`);
                            return;
                        }
                        firstVar.stock -= totalPcs;
                        descList.push(`${totalPcs} Pcs ${firstVar.name}`);
                    }
                }

                description = `[Penjualan ${boxQty} Paket Box (${descList.join(', ')})] ` + description;
            }
        }

        const newTx = {
            id: 'tx_' + Date.now(),
            type,
            date,
            category,
            amount,
            description: description || category
        };
        transactions.unshift(newTx);
        showToast('Transaksi berhasil disimpan!');

        resetForm();
        saveData();
    }

    function resetForm() {
        document.getElementById('transaction-form').reset();
        document.getElementById('edit-id').value = '';
        document.getElementById('form-title').innerText = 'Tambah Transaksi Kas';
        document.getElementById('submit-btn').innerHTML = '<i class="fa-solid fa-save"></i><span>Simpan Transaksi</span>';
        document.getElementById('date').value = new Date().toISOString().split('T')[0];
        
        const defaultRadio = document.querySelector('input[name="type"][value="pemasukan"]');
        if (defaultRadio) defaultRadio.checked = true;
        renderCategoryChips('pemasukan');
        populateRisolVariantDropdown();
        customPaketSelection = {};
    }

    // DELETE TRANSACTION WITH STOCK RESTORATION
    function deleteTransaction(id) {
        const tx = transactions.find(t => t.id === id);
        if (!tx) return;

        transactionToDeleteId = id;
        document.getElementById('delete-info-category').innerText = `[${tx.type.toUpperCase()}] ${tx.category}`;
        document.getElementById('delete-info-desc').innerText = tx.description || '-';
        document.getElementById('delete-info-amount').innerText = `Rp ${tx.amount.toLocaleString('id-ID')}`;

        document.getElementById('confirm-delete-btn').onclick = () => executeDeleteTransaction(id);
        document.getElementById('delete-confirm-modal').classList.remove('hidden');
    }

    function closeDeleteModal() {
        document.getElementById('delete-confirm-modal').classList.add('hidden');
        transactionToDeleteId = null;
    }

    function executeDeleteTransaction(id) {
        const txIndex = transactions.findIndex(t => t.id === id);
        if (txIndex === -1) {
            closeDeleteModal();
            return;
        }

        const tx = transactions[txIndex];

        // RESTORE STOCK IF THIS TRANSACTION WAS A RISOL SALE
        if (tx.type === 'pemasukan' && (tx.category.includes('Risol') || tx.category.includes('Box'))) {
            variants.forEach(v => {
                const regex = new RegExp(`(\\d+)\\s*Pcs\\s*${v.name}`, 'i');
                const match = tx.description.match(regex);
                if (match && match[1]) {
                    const qtyToRestore = parseInt(match[1]);
                    v.stock += qtyToRestore;
                    
                    opnameLogs.unshift({
                        id: 'log_' + Date.now(),
                        date: new Date().toLocaleDateString('id-ID'),
                        variantName: v.name,
                        type: 'Restorasi Hapus Transaksi',
                        diff: `+${qtyToRestore}`,
                        reason: `Pengembalian stok dari transaksi kas yang dihapus`,
                        finalStock: v.stock
                    });
                }
            });
        }

        transactions.splice(txIndex, 1);
        closeDeleteModal();
        saveData();
        showToast('Transaksi berhasil dihapus & data diperbarui.');
    }

    // RENDER MAIN APP & STATS
    function renderApp() {
        let totalOmset = 0;
        let totalModalDisetor = 0;
        let totalExpense = 0;

        transactions.forEach(t => {
            if (t.type === 'pemasukan') {
                if (t.category === 'Suntikan Modal') totalModalDisetor += t.amount;
                else totalOmset += t.amount;
            } else {
                totalExpense += t.amount;
            }
        });

        // LOGIC PERSIS: Omset digunakan dulu untuk menutupi Pengeluaran
        // Jika Pengeluaran > Omset, selisih defisit akan mengurangi Uang Modal Disetor
        const deficit = Math.max(0, totalExpense - totalOmset);
        const sisaModalAwal = Math.max(0, totalModalDisetor - deficit);

        const saldo = (totalOmset + totalModalDisetor) - totalExpense;
        const netProfit = totalOmset - totalExpense;

        // Stats Cards
        document.getElementById('stat-saldo').innerText = `Rp ${saldo.toLocaleString('id-ID')}`;
        document.getElementById('stat-omset').innerText = `Rp ${totalOmset.toLocaleString('id-ID')}`;
        
        // Tampilan Modal Disetor vs Sisa Modal Tergerus
        const modalStatEl = document.getElementById('stat-modal');
        const modalStatusEl = document.getElementById('stat-modal-status');
        if (modalStatEl) {
            modalStatEl.innerText = `Rp ${sisaModalAwal.toLocaleString('id-ID')}`;
        }
        if (modalStatusEl) {
            if (deficit > 0) {
                modalStatusEl.innerHTML = `<i class="fa-solid fa-triangle-exclamation text-rose-500"></i> Modal tergerus defisit Rp ${deficit.toLocaleString('id-ID')}`;
                modalStatusEl.className = 'mt-3 text-[11px] text-rose-600 font-semibold flex items-center gap-1';
            } else {
                modalStatusEl.innerHTML = `<i class="fa-solid fa-circle-check text-emerald-500"></i> Disetor: Rp ${totalModalDisetor.toLocaleString('id-ID')}`;
                modalStatusEl.className = 'mt-3 text-[11px] text-slate-500 flex items-center gap-1';
            }
        }

        document.getElementById('stat-pengeluaran').innerText = `Rp ${totalExpense.toLocaleString('id-ID')}`;

        // Total Risol Stock
        let totalStock = variants.reduce((sum, v) => sum + v.stock, 0);
        document.getElementById('stat-total-stok').innerText = `${totalStock} Pcs`;

        // Low Stock Warning
        const lowStockVars = variants.filter(v => v.stock <= v.minStock);
        const warningEl = document.getElementById('stat-stok-warning');
        if (warningEl) {
            if (lowStockVars.length > 0) {
                warningEl.innerHTML = `<i class="fa-solid fa-triangle-exclamation text-amber-500"></i> ${lowStockVars.length} varian butuh restock!`;
                warningEl.className = 'mt-3 text-[11px] text-amber-600 font-semibold flex items-center gap-1';
            } else {
                warningEl.innerHTML = `<i class="fa-solid fa-circle-check text-emerald-500"></i> Semua varian stok aman`;
                warningEl.className = 'mt-3 text-[11px] text-slate-500 flex items-center gap-1';
            }
        }

        // Quick Stock Cards
        const quickCardsContainer = document.getElementById('quick-stock-cards');
        if (quickCardsContainer) {
            quickCardsContainer.innerHTML = '';
            variants.forEach(v => {
                const isLow = v.stock <= v.minStock;
                const card = document.createElement('div');
                card.className = `p-3 rounded-xl border transition-all ${
                    isLow ? 'bg-rose-50/50 border-rose-200' : 'bg-slate-50/80 border-slate-200'
                }`;
                card.innerHTML = `
                    <p class="text-[11px] font-medium text-slate-500 truncate">${v.name}</p>
                    <p class="text-base font-bold ${isLow ? 'text-rose-600' : 'text-slate-800'} mt-0.5">${v.stock} <span class="text-xs font-normal text-slate-500">pcs</span></p>
                    <div class="mt-1 flex items-center justify-between text-[10px]">
                        <span class="text-slate-400">Min: ${v.minStock}</span>
                        ${isLow ? '<span class="bg-rose-100 text-rose-700 font-bold px-1.5 py-0.5 rounded">Low</span>' : '<span class="text-emerald-600 font-medium">Aman</span>'}
                    </div>
                `;
                quickCardsContainer.appendChild(card);
            });
        }

        // Transactions Table Render
        const search = document.getElementById('search-input').value.toLowerCase();
        const filterType = document.getElementById('filter-type').value;

        const filteredTxs = transactions.filter(t => {
            const matchType = filterType === 'all' || t.type === filterType;
            const matchSearch = t.category.toLowerCase().includes(search) || t.description.toLowerCase().includes(search);
            return matchType && matchSearch;
        });

        const tbody = document.getElementById('transaction-list');
        if (tbody) {
            tbody.innerHTML = '';
            if (filteredTxs.length === 0) {
                tbody.innerHTML = `<tr><td colspan="5" class="p-6 text-center text-slate-400">Belum ada catatan transaksi kas.</td></tr>`;
            } else {
                filteredTxs.forEach(t => {
                    const tr = document.createElement('tr');
                    tr.className = 'hover:bg-amber-50/30 transition-colors';
                    tr.innerHTML = `
                        <td class="p-3 text-slate-600 whitespace-nowrap">${t.date}</td>
                        <td class="p-3">
                            <span class="inline-block px-2 py-0.5 rounded-md text-[11px] font-semibold ${
                                t.type === 'pemasukan' ? (t.category === 'Suntikan Modal' ? 'bg-blue-100 text-blue-800' : 'bg-emerald-100 text-emerald-800') : 'bg-rose-100 text-rose-800'
                            }">${t.category}</span>
                        </td>
                        <td class="p-3 text-slate-700 max-w-xs truncate">${t.description}</td>
                        <td class="p-3 text-right font-bold whitespace-nowrap ${t.type === 'pemasukan' ? 'text-emerald-600' : 'text-rose-600'}">
                            ${t.type === 'pemasukan' ? '+' : '-'} Rp ${t.amount.toLocaleString('id-ID')}
                        </td>
                        <td class="p-3 text-center no-print whitespace-nowrap">
                            <button onclick="deleteTransaction('${t.id}')" title="Hapus Transaksi" class="text-slate-400 hover:text-rose-600 p-1.5 rounded-lg hover:bg-rose-50 transition-colors"><i class="fa-solid fa-trash-can"></i></button>
                        </td>
                    `;
                    tbody.appendChild(tr);
                });
            }
        }

        renderVariantTable();
        renderOpnameLogs();

        // Reports Tab Stats
        document.getElementById('report-income').innerText = `Rp ${totalOmset.toLocaleString('id-ID')}`;
        document.getElementById('report-modal').innerText = `Rp ${totalModalDisetor.toLocaleString('id-ID')}`;
        document.getElementById('report-expense').innerText = `Rp ${totalExpense.toLocaleString('id-ID')}`;
        document.getElementById('report-profit').innerText = `Rp ${netProfit.toLocaleString('id-ID')}`;

        populateRisolVariantDropdown();
        populateMonthlyPeriodDropdown();
    }

    // RENDER VARIANT TABLE
    function renderVariantTable() {
        const tbody = document.getElementById('variant-list-body');
        if (!tbody) return;
        tbody.innerHTML = '';

        variants.forEach(v => {
            const isLow = v.stock <= v.minStock;
            const tr = document.createElement('tr');
            tr.className = 'hover:bg-slate-50 transition-colors';
            tr.innerHTML = `
                <td class="p-3 font-semibold text-slate-900">${v.name}</td>
                <td class="p-3 text-slate-600">Rp ${v.price.toLocaleString('id-ID')}</td>
                <td class="p-3">
                    <span class="font-bold text-sm ${isLow ? 'text-rose-600' : 'text-slate-800'}">${v.stock} pcs</span>
                    <span class="text-[10px] text-slate-400 block">Min: ${v.minStock} pcs</span>
                </td>
                <td class="p-3 text-center">
                    <button onclick="openRestockModal('${v.id}')" class="bg-amber-100 hover:bg-amber-200 text-amber-800 font-medium px-2.5 py-1 rounded-lg text-[11px] transition-colors">
                        <i class="fa-solid fa-arrows-rotate"></i> Restock / Opname
                    </button>
                </td>
                <td class="p-3 text-center space-x-1">
                    <button onclick="editVariant('${v.id}')" class="text-slate-400 hover:text-amber-600 p-1"><i class="fa-solid fa-pen"></i></button>
                    <button onclick="deleteVariant('${v.id}')" class="text-slate-400 hover:text-rose-600 p-1"><i class="fa-solid fa-trash-can"></i></button>
                </td>
            `;
            tbody.appendChild(tr);
        });
    }

    // VARIANT MANAGEMENT
    function toggleVariantForm() {
        const form = document.getElementById('variant-form');
        if (!form) return;
        form.classList.toggle('hidden');
        if (!form.classList.contains('hidden')) {
            document.getElementById('variant-edit-id').value = '';
            document.getElementById('variant-form-title').innerText = 'Tambah Varian Risol Baru';
            form.reset();
        }
    }

    function handleVariantSubmit(e) {
        e.preventDefault();
        const editId = document.getElementById('variant-edit-id').value;
        const name = document.getElementById('variant-name').value;
        const price = parseFloat(document.getElementById('variant-price').value) || 0;
        const stock = parseInt(document.getElementById('variant-stock').value) || 0;
        const minStock = parseInt(document.getElementById('variant-min').value) || 0;

        if (editId) {
            const v = variants.find(x => x.id === editId);
            if (v) {
                v.name = name;
                v.price = price;
                v.stock = stock;
                v.minStock = minStock;
                showToast('Varian berhasil diperbarui!');
            }
        } else {
            variants.push({ id: 'v_' + Date.now(), name, price, stock, minStock });
            showToast('Varian risol baru berhasil ditambahkan!');
        }

        toggleVariantForm();
        saveData();
    }

    function editVariant(id) {
        const v = variants.find(x => x.id === id);
        if (!v) return;

        document.getElementById('variant-edit-id').value = v.id;
        document.getElementById('variant-name').value = v.name;
        document.getElementById('variant-price').value = v.price;
        document.getElementById('variant-stock').value = v.stock;
        document.getElementById('variant-min').value = v.minStock;

        document.getElementById('variant-form-title').innerText = 'Edit Varian Risol';
        document.getElementById('variant-form').classList.remove('hidden');
    }

    function deleteVariant(id) {
        if (variants.length <= 1) {
            showToast('Harus menyisakan minimal 1 varian risol!');
            return;
        }
        if (confirm('Hapus varian risol ini dari sistem?')) {
            variants = variants.filter(v => v.id !== id);
            saveData();
            showToast('Varian berhasil dihapus!');
        }
    }

    // RESTOCK & STOCK OPNAME MODAL
    function openRestockModal(variantId) {
        const v = variants.find(x => x.id === variantId);
        if (!v) return;

        document.getElementById('modal-variant-id').value = v.id;
        document.getElementById('modal-variant-name').innerText = v.name;
        document.getElementById('modal-system-qty').innerText = v.stock;
        document.getElementById('modal-actual-qty').value = v.stock;

        setModalMode('restock');
        document.getElementById('restock-opname-modal').classList.remove('hidden');
    }

    function closeRestockModal() {
        document.getElementById('restock-opname-modal').classList.add('hidden');
    }

    function setModalMode(mode) {
        currentModalMode = mode;
        const btnRestock = document.getElementById('mode-restock-btn');
        const btnOpname = document.getElementById('mode-opname-btn');
        const restockFields = document.getElementById('restock-fields');
        const opnameFields = document.getElementById('opname-fields');

        if (mode === 'restock') {
            btnRestock.className = 'py-2 rounded-lg bg-white text-amber-800 shadow-xs font-bold';
            btnOpname.className = 'py-2 rounded-lg text-slate-600 font-medium';
            restockFields.classList.remove('hidden');
            opnameFields.classList.add('hidden');
        } else {
            btnOpname.className = 'py-2 rounded-lg bg-white text-amber-800 shadow-xs font-bold';
            btnRestock.className = 'py-2 rounded-lg text-slate-600 font-medium';
            opnameFields.classList.remove('hidden');
            restockFields.classList.add('hidden');
            updateOpnameDiff();
        }
    }

    function updateOpnameDiff() {
        const variantId = document.getElementById('modal-variant-id').value;
        const v = variants.find(x => x.id === variantId);
        if (!v) return;

        const actual = parseInt(document.getElementById('modal-actual-qty').value) || 0;
        const diff = actual - v.stock;
        const badge = document.getElementById('modal-diff-badge');

        if (diff === 0) {
            badge.className = 'font-bold px-2 py-0.5 rounded bg-slate-200 text-slate-700';
            badge.innerText = 'Sesuai (0)';
        } else if (diff > 0) {
            badge.className = 'font-bold px-2 py-0.5 rounded bg-emerald-100 text-emerald-800';
            badge.innerText = `Surplus (+${diff})`;
        } else {
            badge.className = 'font-bold px-2 py-0.5 rounded bg-rose-100 text-rose-800';
            badge.innerText = `Selisih/Minus (${diff})`;
        }
    }

    function handleModalSubmit(e) {
        e.preventDefault();
        const variantId = document.getElementById('modal-variant-id').value;
        const v = variants.find(x => x.id === variantId);
        if (!v) return;

        if (currentModalMode === 'restock') {
            const addQty = parseInt(document.getElementById('modal-add-qty').value) || 0;
            v.stock += addQty;
            
            opnameLogs.unshift({
                id: 'log_' + Date.now(),
                date: new Date().toLocaleDateString('id-ID'),
                variantName: v.name,
                type: 'Restock Produksi',
                diff: `+${addQty}`,
                reason: 'Hasil Tambah Stok Produksi Harian',
                finalStock: v.stock
            });
            showToast(`Stok ${v.name} berhasil ditambah +${addQty} pcs.`);
        } else {
            const actualQty = parseInt(document.getElementById('modal-actual-qty').value) || 0;
            const diff = actualQty - v.stock;
            const reason = document.getElementById('modal-opname-reason').value;

            v.stock = actualQty;

            opnameLogs.unshift({
                id: 'log_' + Date.now(),
                date: new Date().toLocaleDateString('id-ID'),
                variantName: v.name,
                type: 'Stock Opname Physical',
                diff: diff > 0 ? `+${diff}` : `${diff}`,
                reason: reason,
                finalStock: v.stock
            });
            showToast(`Stock Opname ${v.name} diperbarui ke ${actualQty} pcs.`);
        }

        closeRestockModal();
        saveData();
    }

    function renderOpnameLogs() {
        const container = document.getElementById('opname-logs-container');
        if (!container) return;
        container.innerHTML = '';

        if (opnameLogs.length === 0) {
            container.innerHTML = `<p class="text-center text-xs text-slate-400 py-6">Belum ada riwayat stock opname.</p>`;
            return;
        }

        opnameLogs.slice(0, 15).forEach(log => {
            const item = document.createElement('div');
            item.className = 'p-3 bg-slate-50 border border-slate-200 rounded-xl space-y-1';
            item.innerHTML = `
                <div class="flex justify-between items-center">
                    <span class="text-xs font-bold text-slate-800">${log.variantName}</span>
                    <span class="text-[10px] font-semibold px-2 py-0.5 rounded ${
                        log.diff.startsWith('+') ? 'bg-emerald-100 text-emerald-800' : 'bg-rose-100 text-rose-800'
                    }">${log.diff} Pcs</span>
                </div>
                <div class="flex justify-between text-[11px] text-slate-500">
                    <span>${log.reason}</span>
                    <span>Stok Akhir: <strong>${log.finalStock}</strong></span>
                </div>
                <p class="text-[9px] text-slate-400">${log.date} - ${log.type}</p>
            `;
            container.appendChild(item);
        });
    }

    // MONTHLY REPORT LOGIC
    function populateMonthlyPeriodDropdown() {
        const select = document.getElementById('monthly-select');
        if (!select) return;

        const periods = new Set();
        transactions.forEach(t => {
            if (t.date) {
                const ym = t.date.substring(0, 7); // "YYYY-MM"
                periods.add(ym);
            }
        });

        // Always add current month if empty
        const currentYM = new Date().toISOString().substring(0, 7);
        periods.add(currentYM);

        const sortedPeriods = Array.from(periods).sort().reverse();
        const selectedVal = select.value && Array.from(periods).includes(select.value) ? select.value : currentYM;

        select.innerHTML = '';
        sortedPeriods.forEach(ym => {
            const [year, month] = ym.split('-');
            const monthName = new Date(parseInt(year), parseInt(month) - 1, 1).toLocaleString('id-ID', { month: 'long', year: 'numeric' });
            const opt = document.createElement('option');
            opt.value = ym;
            opt.innerText = monthName;
            if (ym === selectedVal) opt.selected = true;
            select.appendChild(opt);
        });
    }

    function renderMonthlyReport() {
        populateMonthlyPeriodDropdown();
        const select = document.getElementById('monthly-select');
        if (!select) return;
        const selectedYM = select.value;

        let mOmset = 0;
        let mModal = 0;
        let mExpense = 0;

        const monthlyMap = {};

        if (selectedYM) {
            monthlyMap[selectedYM] = { omset: 0, modal: 0, expense: 0 };
        }

        transactions.forEach(t => {
            if (!t.date) return;
            const ym = t.date.substring(0, 7);
            if (!monthlyMap[ym]) monthlyMap[ym] = { omset: 0, modal: 0, expense: 0 };

            const amount = parseFloat(t.amount) || 0;

            if (t.type === 'pemasukan') {
                if (t.category === 'Suntikan Modal') {
                    monthlyMap[ym].modal += amount;
                    if (ym === selectedYM) mModal += amount;
                } else {
                    monthlyMap[ym].omset += amount;
                    if (ym === selectedYM) mOmset += amount;
                }
            } else if (t.type === 'pengeluaran') {
                monthlyMap[ym].expense += amount;
                if (ym === selectedYM) mExpense += amount;
            }
        });

        const netProfit = mOmset - mExpense;
        const mDeficit = Math.max(0, mExpense - mOmset);
        const mSisaModal = Math.max(0, mModal - mDeficit);

        document.getElementById('monthly-omset').innerText = `Rp ${mOmset.toLocaleString('id-ID')}`;
        
        const mModalEl = document.getElementById('monthly-modal');
        const mModalSubEl = document.getElementById('monthly-modal-subtext');
        if (mModalEl) mModalEl.innerText = `Rp ${mSisaModal.toLocaleString('id-ID')}`;
        if (mModalSubEl) {
            if (mDeficit > 0) {
                mModalSubEl.innerHTML = `<span class="text-rose-600 font-bold">Tergerus Defisit Rp ${mDeficit.toLocaleString('id-ID')}</span> (Disetor: Rp ${mModal.toLocaleString('id-ID')})`;
            } else {
                mModalSubEl.innerText = `Disetor Bulan Ini: Rp ${mModal.toLocaleString('id-ID')}`;
                mModalSubEl.className = 'text-[11px] font-semibold text-slate-500 mt-2';
            }
        }

        document.getElementById('monthly-expense').innerText = `Rp ${mExpense.toLocaleString('id-ID')}`;
        
        const profitEl = document.getElementById('monthly-net-profit');
        const statusEl = document.getElementById('monthly-profit-status');

        if (profitEl) {
            profitEl.innerText = `Rp ${netProfit.toLocaleString('id-ID')}`;

            if (netProfit > 0) {
                profitEl.className = 'text-2xl font-bold text-emerald-600 mt-1';
                statusEl.innerHTML = `<span class="text-emerald-700 bg-emerald-100 px-2 py-0.5 rounded-full">Untung (Profit)</span>`;
            } else if (netProfit < 0) {
                profitEl.className = 'text-2xl font-bold text-rose-600 mt-1';
                statusEl.innerHTML = `<span class="text-rose-700 bg-rose-100 px-2 py-0.5 rounded-full">Defisit / Rugi (Modal Berkurang)</span>`;
            } else {
                profitEl.className = 'text-2xl font-bold text-slate-800 mt-1';
                statusEl.innerHTML = `<span class="text-slate-700 bg-slate-200 px-2 py-0.5 rounded-full">Impas (Break Even)</span>`;
            }
        }

        // Monthly History Table Render
        const historyBody = document.getElementById('monthly-history-table');
        if (historyBody) {
            historyBody.innerHTML = '';
            const sortedKeys = Object.keys(monthlyMap).sort().reverse();

            if (sortedKeys.length === 0) {
                historyBody.innerHTML = `<tr><td colspan="6" class="p-4 text-center text-slate-400">Belum ada riwayat transaksi bulanan.</td></tr>`;
            } else {
                sortedKeys.forEach(ym => {
                    const [y, m] = ym.split('-');
                    const monthName = new Date(parseInt(y), parseInt(m) - 1, 1).toLocaleString('id-ID', { month: 'long', year: 'numeric' });
                    const data = monthlyMap[ym];
                    const profit = data.omset - data.expense;
                    const itemDeficit = Math.max(0, data.expense - data.omset);
                    const sisaModal = Math.max(0, data.modal - itemDeficit);
                    const margin = data.omset > 0 ? ((profit / data.omset) * 100).toFixed(1) : '0.0';

                    const tr = document.createElement('tr');
                    tr.className = ym === selectedYM ? 'bg-amber-50/50 font-semibold' : 'hover:bg-slate-50';
                    tr.innerHTML = `
                        <td class="p-3 text-slate-800 font-medium">${monthName}</td>
                        <td class="p-3 text-right text-emerald-600">Rp ${data.omset.toLocaleString('id-ID')}</td>
                        <td class="p-3 text-right text-blue-600">
                            <div>Rp ${sisaModal.toLocaleString('id-ID')}</div>
                            <div class="text-[9px] text-slate-400">(Disetor: Rp ${data.modal.toLocaleString('id-ID')})</div>
                        </td>
                        <td class="p-3 text-right text-rose-600">Rp ${data.expense.toLocaleString('id-ID')}</td>
                        <td class="p-3 text-right font-bold ${profit >= 0 ? 'text-emerald-600' : 'text-rose-600'}">Rp ${profit.toLocaleString('id-ID')}</td>
                        <td class="p-3 text-center">
                            <span class="px-2 py-0.5 rounded text-[11px] font-bold ${profit >= 0 ? 'bg-emerald-100 text-emerald-800' : 'bg-rose-100 text-rose-800'}">${margin}%</span>
                        </td>
                    `;
                    historyBody.appendChild(tr);
                });
            }
        }

        updateMonthlyBarChart(monthlyMap);
    }

    function updateMonthlyBarChart(monthlyMap) {
        const canvas = document.getElementById('monthlyBarChart');
        if (!canvas) return;

        if (monthlyBarChartObj) {
            monthlyBarChartObj.destroy();
        }

        const sortedKeys = Object.keys(monthlyMap).sort();
        const labels = sortedKeys.map(ym => {
            const [y, m] = ym.split('-');
            return new Date(parseInt(y), parseInt(m) - 1, 1).toLocaleString('id-ID', { month: 'short', year: '2-digit' });
        });

        const omsetArr = sortedKeys.map(ym => monthlyMap[ym].omset);
        const expenseArr = sortedKeys.map(ym => monthlyMap[ym].expense);
        const profitArr = sortedKeys.map(ym => monthlyMap[ym].omset - monthlyMap[ym].expense);

        const ctx = canvas.getContext('2d');
        monthlyBarChartObj = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: labels,
                datasets: [
                    { label: 'Omset Penjualan', data: omsetArr, backgroundColor: '#10b981', borderRadius: 6 },
                    { label: 'Biaya Operasional', data: expenseArr, backgroundColor: '#f43f5e', borderRadius: 6 },
                    { label: 'Laba / Rugi Bersih', data: profitArr, backgroundColor: '#f59e0b', borderRadius: 6 }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: { legend: { position: 'top' } }
            }
        });
    }

    // CHARTS LOGIC DASHBOARD
    function initCharts() {
        const trendCtx = document.getElementById('trendChart')?.getContext('2d');
        const expenseCtx = document.getElementById('expenseCategoryChart')?.getContext('2d');

        if (trendCtx) {
            trendChartObj = new Chart(trendCtx, {
                type: 'line',
                data: {
                    labels: [],
                    datasets: [
                        { label: 'Omset Penjualan (Rp)', data: [], borderColor: '#10b981', backgroundColor: 'rgba(16, 185, 129, 0.1)', fill: true, tension: 0.3 },
                        { label: 'Pengeluaran (Rp)', data: [], borderColor: '#f43f5e', backgroundColor: 'rgba(244, 63, 94, 0.1)', fill: true, tension: 0.3 }
                    ]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        if (expenseCtx) {
            expenseChartObj = new Chart(expenseCtx, {
                type: 'doughnut',
                data: {
                    labels: [],
                    datasets: [{ data: [], backgroundColor: ['#f59e0b', '#ef4444', '#3b82f6', '#8b5cf6', '#64748b'] }]
                },
                options: { responsive: true, maintainAspectRatio: false }
            });
        }

        updateCharts();
    }

    function updateCharts() {
        if (!trendChartObj || !expenseChartObj) return;

        const datesMap = {};
        transactions.forEach(t => {
            if (!datesMap[t.date]) datesMap[t.date] = { omset: 0, exp: 0 };
            if (t.type === 'pemasukan' && t.category !== 'Suntikan Modal') {
                datesMap[t.date].omset += t.amount;
            } else if (t.type === 'pengeluaran') {
                datesMap[t.date].exp += t.amount;
            }
        });

        const sortedDates = Object.keys(datesMap).sort();
        trendChartObj.data.labels = sortedDates;
        trendChartObj.data.datasets[0].data = sortedDates.map(d => datesMap[d].omset);
        trendChartObj.data.datasets[1].data = sortedDates.map(d => datesMap[d].exp);
        trendChartObj.update();

        const catMap = {};
        transactions.filter(t => t.type === 'pengeluaran').forEach(t => {
            catMap[t.category] = (catMap[t.category] || 0) + t.amount;
        });

        expenseChartObj.data.labels = Object.keys(catMap);
        expenseChartObj.data.datasets[0].data = Object.values(catMap);
        expenseChartObj.update();
    }

    // DEMO SAMPLE DATA LOAD
    function loadSampleRisolData() {
        variants = [
            { id: 'v1', name: 'Risol Mayo Smoke Beef', price: 4000, stock: 45, minStock: 10 },
            { id: 'v2', name: 'Risol Ragout Ayam', price: 4000, stock: 8, minStock: 10 },
            { id: 'v3', name: 'Risol Melted Cheese Chicken', price: 4000, stock: 30, minStock: 8 },
            { id: 'v4', name: 'Risol Sayur Telur', price: 3500, stock: 15, minStock: 10 }
        ];

        const today = new Date().toISOString().split('T')[0];
        transactions = [
            { id: 'tx_1', type: 'pemasukan', date: today, category: 'Suntikan Modal', amount: 500000, description: 'Modal awal disetor kas usaha' },
            { id: 'tx_2', type: 'pengeluaran', date: today, category: 'Bahan Baku (Tepung, Telur, dll)', amount: 180000, description: 'Pembelian tepung terigu, mayo, & smoke beef' },
            { id: 'tx_3', type: 'pemasukan', date: today, category: 'Penjualan Risol Satuan', amount: 40000, description: '[Penjualan 10 Pcs Risol Mayo Smoke Beef] Pelanggan umum' },
            { id: 'tx_4', type: 'pemasukan', date: today, category: 'Penjualan Paket Box', amount: 36000, description: '[Penjualan 3 Paket Box (9 Pcs Risol Melted Cheese Chicken)] Pesanan box' }
        ];

        opnameLogs = [
            { id: 'l1', date: new Date().toLocaleDateString('id-ID'), variantName: 'Risol Ragout Ayam', type: 'Stock Opname Physical', diff: '-2', reason: 'Risol Pecah Kulit saat Penggorengan', finalStock: 8 }
        ];

        saveData();
        showToast('Data Demo Risol Deantara Snacks berhasil diisi!');
    }

    // EXPORT FUNCTIONALITIES
    function exportToCSV() {
        if (transactions.length === 0) {
            showToast('Tidak ada data transaksi untuk diekspor!');
            return;
        }

        let csvContent = "data:text/csv;charset=utf-8,ID,Tanggal,Jenis,Kategori,Nominal,Keterangan\n";
        transactions.forEach(t => {
            csvContent += `"${t.id}","${t.date}","${t.type}","${t.category}","${t.amount}","${t.description.replace(/"/g, '""')}"\n`;
        });

        const encodedUri = encodeURI(csvContent);
        const link = document.createElement('a');
        link.setAttribute('href', encodedUri);
        link.setAttribute('download', `Kas_Deantara_Snacks_${new Date().toISOString().split('T')[0]}.csv`);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
    }

    function exportStockCSV() {
        let csvContent = "data:text/csv;charset=utf-8,ID Varian,Nama Varian,Harga Jual,Stok Saat Ini,Stok Minimal\n";
        variants.forEach(v => {
            csvContent += `"${v.id}","${v.name}","${v.price}","${v.stock}","${v.minStock}"\n`;
        });

        const encodedUri = encodeURI(csvContent);
        const link = document.createElement('a');
        link.setAttribute('href', encodedUri);
        link.setAttribute('download', `Stok_Risol_Deantara_${new Date().toISOString().split('T')[0]}.csv`);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
    }
</script>
</body>
</html>
