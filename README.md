<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Máy Tính Xác Suất Có Điều Kiện</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- FontAwesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                }
            }
        }
    </script>
    <style>
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(6px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .animate-fade-in {
            animation: fadeIn 0.3s ease-out forwards;
        }
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            cbd: #cbd5e1;
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 font-sans min-h-screen flex flex-col">

    <!-- Header -->
    <header class="bg-gradient-to-r from-blue-700 via-indigo-700 to-violet-800 text-white shadow-lg sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row justify-between items-center gap-4">
            <div class="flex items-center space-x-3">
                <div class="bg-white/10 p-2.5 rounded-xl backdrop-blur-md border border-white/20 shadow-inner">
                    <i class="fa-solid fa-calculator text-2xl text-amber-300"></i>
                </div>
                <div>
                    <h1 class="text-xl sm:text-2xl font-bold tracking-tight">MÁY TÍNH XÁC SUẤT CÓ ĐIỀU KIỆN</h1>
                    <p class="text-xs sm:text-sm text-blue-100 opacity-90">Công cụ học tập trực quan • Tự động suy luận & Giải thích từng bước</p>
                </div>
            </div>
            <div class="flex items-center gap-2 flex-wrap justify-center">
                <button onclick="loadPreset('basic')" class="px-3 py-1.5 bg-white/10 hover:bg-white/20 text-xs font-medium rounded-lg transition border border-white/20 flex items-center gap-1.5">
                    <i class="fa-solid fa-bolt text-amber-300"></i> Ví dụ cơ bản
                </button>
                <button onclick="loadPreset('medical')" class="px-3 py-1.5 bg-white/10 hover:bg-white/20 text-xs font-medium rounded-lg transition border border-white/20 flex items-center gap-1.5">
                    <i class="fa-solid fa-notes-medical text-emerald-300"></i> Xét nghiệm y tế
                </button>
                <button onclick="clearAllData()" class="px-3 py-1.5 bg-rose-600/80 hover:bg-rose-600 text-xs font-medium rounded-lg transition border border-rose-400/30 flex items-center gap-1.5 shadow-sm">
                    <i class="fa-solid fa-rotate-right"></i> Xóa dữ liệu
                </button>
            </div>
        </div>
    </header>

    <!-- Main Content Container -->
    <main class="max-w-7xl mx-auto px-4 py-6 sm:px-6 lg:px-8 flex-grow w-full grid grid-cols-1 lg:grid-cols-12 gap-6">

        <!-- Left Column: Input Panel & Warnings -->
        <div class="lg:col-span-4 space-y-6">
            
            <!-- Input Card -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-5 sm:p-6 transition hover:shadow-md">
                <div class="flex items-center justify-between pb-4 mb-4 border-b border-slate-100">
                    <h2 class="text-base font-semibold text-slate-900 flex items-center gap-2">
                        <i class="fa-solid fa-sliders text-blue-600"></i> Dữ Liệu Đầu Vào
                    </h2>
                    <span class="text-xs text-slate-500 bg-slate-100 px-2.5 py-1 rounded-full font-medium">Nhập số hoặc %</span>
                </div>
                <p class="text-xs text-slate-500 mb-4">Nhập bất kỳ dữ kiện nào bạn biết (vd: <code class="bg-slate-100 px-1 py-0.5 rounded text-blue-600 font-mono">0.6</code> hoặc <code class="bg-slate-100 px-1 py-0.5 rounded text-blue-600 font-mono">60%</code>). Hệ thống sẽ tự suy luận phần còn lại.</p>

                <div class="space-y-3.5">
                    <!-- Input items -->
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-medium text-slate-600 mb-1">P(A)</label>
                            <input type="text" id="in_pA" placeholder="vd: 0.6 hoặc 60%" class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:bg-white outline-none transition font-mono">
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-slate-600 mb-1">P(B)</label>
                            <input type="text" id="in_pB" placeholder="vd: 0.5 hoặc 50%" class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:bg-white outline-none transition font-mono">
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-medium text-slate-600 mb-1">P(A ∩ B)</label>
                            <input type="text" id="in_pA_and_B" placeholder="vd: 0.3" class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:bg-white outline-none transition font-mono">
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-slate-600 mb-1">P(A|B)</label>
                            <input type="text" id="in_pA_given_B" placeholder="vd: 0.6" class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:bg-white outline-none transition font-mono">
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-medium text-slate-600 mb-1">P(B|A)</label>
                            <input type="text" id="in_pB_given_A" placeholder="vd: 0.5" class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:bg-white outline-none transition font-mono">
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-slate-600 mb-1">P(A|B')</label>
                            <input type="text" id="in_pA_given_B_not" placeholder="vd: 0.6" class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:bg-white outline-none transition font-mono">
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs font-medium text-slate-600 mb-1">P(A'|B)</label>
                            <input type="text" id="in_pA_not_given_B" placeholder="vd: 0.4" class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:bg-white outline-none transition font-mono">
                        </div>
                        <div>
                            <label class="block text-xs font-medium text-slate-600 mb-1">P(A'|B')</label>
                            <input type="text" id="in_pA_not_given_B_not" placeholder="vd: 0.4" class="w-full px-3 py-2 text-sm bg-slate-50 border border-slate-200 rounded-xl focus:ring-2 focus:ring-blue-500 focus:bg-white outline-none transition font-mono">
                        </div>
                    </div>
                </div>

                <div class="mt-6 flex gap-3">
                    <button onclick="calculateAll()" class="flex-1 bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2.5 px-4 rounded-xl shadow-sm shadow-blue-500/20 transition flex items-center justify-center gap-2 text-sm">
                        <i class="fa-solid fa-wand-magic-sparkles"></i> Tính tất cả
                    </button>
                    <button onclick="clearAllData()" class="px-4 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 font-medium rounded-xl transition text-sm">
                        Xóa
                    </button>
                </div>
            </div>

            <!-- Validation & Error Box -->
            <div id="validationBox" class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-5 hidden">
                <h3 class="text-sm font-semibold text-slate-900 mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-triangle-exclamation text-amber-500"></i> Kiểm tra tính hợp lệ
                </h3>
                <div id="validationContent" class="space-y-2 text-xs">
                    <!-- Dynamic alerts -->
                </div>
            </div>

            <!-- Core Formulas Reference Box -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-5">
                <h3 class="text-sm font-semibold text-slate-900 mb-3 flex items-center gap-2">
                    <i class="fa-solid fa-book text-indigo-600"></i> Ký hiệu & Công thức cơ bản
                </h3>
                <div class="text-xs space-y-2 text-slate-600 leading-relaxed">
                    <p><code class="font-mono text-indigo-600 font-semibold">A'</code> : Biến cố đối của A <span class="text-slate-400">| P(A') = 1 - P(A)</span></p>
                    <p><code class="font-mono text-indigo-600 font-semibold">A ∩ B</code> : Giao của A và B</p>
                    <p><code class="font-mono text-indigo-600 font-semibold">A ∪ B</code> : Hợp của A và B <span class="text-slate-400">| P(A∪B) = P(A)+P(B)-P(A∩B)</span></p>
                    <p><code class="font-mono text-indigo-600 font-semibold">A|B</code> : A với điều kiện B <span class="text-slate-400">| P(A|B) = P(A∩B)/P(B)</span></p>
                    <p><code class="font-mono text-indigo-600 font-semibold">Bayes</code> : <span class="text-slate-400">P(A|B) = P(B|A)·P(A) / P(B)</span></p>
                </div>
            </div>

        </div>

        <!-- Right Column: Results, Matrix, Tree, Formula Cards, Step-by-Step -->
        <div class="lg:col-span-8 space-y-6">

            <!-- Tabs Navigation -->
            <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-2 flex overflow-x-auto gap-2 no-scrollbar">
                <button onclick="switchTab('overview')" id="tab-overview" class="tab-btn flex-1 min-w-[120px] py-2 px-3 rounded-xl text-xs font-semibold bg-blue-600 text-white transition shadow-sm flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-table-cells"></i> Tổng quan kết quả
                </button>
                <button onclick="switchTab('matrix')" id="tab-matrix" class="tab-btn flex-1 min-w-[120px] py-2 px-3 rounded-xl text-xs font-semibold text-slate-600 hover:bg-slate-100 transition flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-border-all"></i> Ma trận 2×2
                </button>
                <button onclick="switchTab('tree')" id="tab-tree" class="tab-btn flex-1 min-w-[120px] py-2 px-3 rounded-xl text-xs font-semibold text-slate-600 hover:bg-slate-100 transition flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-network-wired"></i> Sơ đồ cây
                </button>
                <button onclick="switchTab('formulas')" id="tab-formulas" class="tab-btn flex-1 min-w-[120px] py-2 px-3 rounded-xl text-xs font-semibold text-slate-600 hover:bg-slate-100 transition flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-square-root-variable"></i> Công thức & Thay số
                </button>
                <button onclick="switchTab('steps')" id="tab-steps" class="tab-btn flex-1 min-w-[120px] py-2 px-3 rounded-xl text-xs font-semibold text-slate-600 hover:bg-slate-100 transition flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-shoe-prints"></i> Giải thích từng bước
                </button>
            </div>

            <!-- Tab Content: Overview Results Table -->
            <div id="content-overview" class="tab-content space-y-6 animate-fade-in">
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 overflow-hidden">
                    <div class="px-6 py-4 border-b border-slate-100 flex justify-between items-center bg-slate-50/50">
                        <h3 class="text-sm font-semibold text-slate-900 flex items-center gap-2">
                            <i class="fa-solid fa-chart-pie text-blue-600"></i> Tổng hợp các Đại lượng Xác suất
                        </h3>
                        <span id="calcStatus" class="text-xs px-2.5 py-1 bg-amber-100 text-amber-800 rounded-full font-medium">Chưa có dữ liệu đầy đủ</span>
                    </div>
                    <div class="overflow-x-auto">
                        <table class="w-full text-left border-collapse">
                            <thead>
                                <tr class="bg-slate-50 text-slate-500 uppercase text-[11px] font-semibold tracking-wider border-b border-slate-200">
                                    <th class="py-3 px-6">Đại lượng</th>
                                    <th class="py-3 px-6">Ký hiệu / Ý nghĩa</th>
                                    <th class="py-3 px-6 text-right">Giá trị thập phân</th>
                                    <th class="py-3 px-6 text-right">Tỷ lệ phần trăm</th>
                                </tr>
                            </thead>
                            <tbody id="resultsTableBody" class="divide-y divide-slate-100 text-sm">
                                <!-- Populated by JS -->
                                <tr>
                                    <td colspan="4" class="py-8 text-center text-slate-400 italic">Vui lòng nhập dữ liệu và nhấn "Tính tất cả" ở cột bên trái.</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Tab Content: 2x2 Matrix -->
            <div id="content-matrix" class="tab-content hidden space-y-6 animate-fade-in">
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-6">
                    <div class="flex justify-between items-center mb-4">
                        <h3 class="text-sm font-semibold text-slate-900 flex items-center gap-2">
                            <i class="fa-solid fa-border-all text-blue-600"></i> Ma Trận Xác Suất 2 × 2 (Bảng Giao & Biên)
                        </h3>
                    </div>
                    <p class="text-xs text-slate-500 mb-6">Bảng thể hiện phân phối đồng thời và phân phối biên của hai biến cố A và B.</p>
                    
                    <div class="overflow-x-auto">
                        <table class="w-full text-center border-collapse font-mono text-sm">
                            <thead>
                                <tr class="bg-slate-100 text-slate-700 font-semibold text-xs border-b-2 border-slate-300">
                                    <th class="p-3 text-left">Biến cố</th>
                                    <th class="p-3">B</th>
                                    <th class="p-3">B'</th>
                                    <th class="p-3 bg-slate-200 text-slate-900">Tổng (Biên)</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-slate-200">
                                <tr class="hover:bg-slate-50">
                                    <td class="p-3 text-left font-sans font-semibold text-slate-700 bg-slate-50">A</td>
                                    <td id="mat_A_B" class="p-3 text-blue-600 font-semibold">-</td>
                                    <td id="mat_A_notB" class="p-3 text-blue-600 font-semibold">-</td>
                                    <td id="mat_A_total" class="p-3 font-bold bg-slate-100 text-slate-900">-</td>
                                </tr>
                                <tr class="hover:bg-slate-50">
                                    <td class="p-3 text-left font-sans font-semibold text-slate-700 bg-slate-50">A'</td>
                                    <td id="mat_notA_B" class="p-3 text-blue-600 font-semibold">-</td>
                                    <td id="mat_notA_notB" class="p-3 text-blue-600 font-semibold">-</td>
                                    <td id="mat_notA_total" class="p-3 font-bold bg-slate-100 text-slate-900">-</td>
                                </tr>
                            </tbody>
                            <tfoot>
                                <tr class="bg-slate-100 font-bold text-slate-900 border-t-2 border-slate-300">
                                    <td class="p-3 text-left font-sans">Tổng (Biên)</td>
                                    <td id="mat_total_B" class="p-3">-</td>
                                    <td id="mat_total_notB" class="p-3">-</td>
                                    <td id="mat_grand_total" class="p-3 bg-slate-200 text-indigo-700">1.00</td>
                                </tr>
                            </tfoot>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Tab Content: Probability Tree -->
            <div id="content-tree" class="tab-content hidden space-y-6 animate-fade-in">
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-6">
                    <h3 class="text-sm font-semibold text-slate-900 mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-network-wired text-blue-600"></i> Sơ Đồ Cây Xác Suất
                    </h3>
                    <p class="text-xs text-slate-500 mb-6">Biểu diễn xác suất theo nhánh điều kiện P(B) và P(B') kết hợp P(A|B), P(A'|B)...</p>

                    <div id="treeContainer" class="p-4 bg-slate-50 rounded-xl border border-slate-200/60 overflow-x-auto min-h-[300px] flex items-center justify-center">
                        <p class="text-xs text-slate-400 italic">Vui lòng nhập dữ liệu để hiển thị sơ đồ cây.</p>
                    </div>
                </div>
            </div>

            <!-- Tab Content: Formula Cards -->
            <div id="content-formulas" class="tab-content hidden space-y-6 animate-fade-in">
                <div class="flex justify-between items-center mb-2">
                    <h3 class="text-sm font-semibold text-slate-900 flex items-center gap-2">
                        <i class="fa-solid fa-square-root-variable text-blue-600"></i> Thẻ Công Thức & Thay Số Chi Tiết
                    </h3>
                    <span class="text-xs text-slate-500">Hiển thị công thức, thay số và kết quả từng đại lượng</span>
                </div>
                <div id="formulaCardsGrid" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <!-- Populated by JS -->
                    <div class="bg-white rounded-2xl p-5 border border-slate-200/80 text-center text-slate-400 italic col-span-2">
                        Chưa có dữ liệu tính toán.
                    </div>
                </div>
            </div>

            <!-- Tab Content: Step-by-Step Explanation -->
            <div id="content-steps" class="tab-content hidden space-y-6 animate-fade-in">
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200/80 p-6">
                    <h3 class="text-sm font-semibold text-slate-900 mb-2 flex items-center gap-2">
                        <i class="fa-solid fa-shoe-prints text-blue-600"></i> Quá Trình Suy Luận Từng Bước
                    </h3>
                    <p class="text-xs text-slate-500 mb-6">Theo dõi cách thuật toán từng bước khai thác các dữ kiện để tìm ra các ẩn số.</p>
                    
                    <div id="stepsContainer" class="space-y-4">
                        <div class="text-center text-slate-400 italic text-xs py-8">Chưa có quá trình suy luận nào được thực hiện.</div>
                    </div>
                </div>
            </div>

        </div>

    </main>

    <!-- Footer -->
    <footer class="bg-white border-t border-slate-200 py-4 mt-auto">
        <div class="max-w-7xl mx-auto px-4 text-center text-xs text-slate-500">
            Máy Tính Xác Suất Có Điều Kiện • Công cụ hỗ trợ học tập môn Toán Xác Suất Thống Kê
        </div>
    </footer>

    <!-- JavaScript Logic -->
    <script>
        // Tab switching
        function switchTab(tabName) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.querySelectorAll('.tab-btn').forEach(btn => {
                btn.classList.remove('bg-blue-600', 'text-white', 'shadow-sm');
                btn.classList.add('text-slate-600', 'hover:bg-slate-100');
            });
            
            document.getElementById('content-' + tabName).classList.remove('hidden');
            const activeBtn = document.getElementById('tab-' + tabName);
            activeBtn.classList.remove('text-slate-600', 'hover:bg-slate-100');
            activeBtn.classList.add('bg-blue-600', 'text-white', 'shadow-sm');
        }

        // Helper to parse input (supports decimal like 0.6 or percentage like 60% or 60)
        function parseVal(str) {
            if (str === null || str === undefined) return null;
            let s = String(str).trim();
            if (s === '') return null;
            if (s.endsWith('%')) {
                let num = parseFloat(s.slice(0, -1));
                return isNaN(num) ? null : num / 100;
            }
            let num = parseFloat(s);
            if (isNaN(num)) return null;
            // If user typed 60 instead of 0.6, but usually probabilities are <= 1. If > 1 and <= 100, treat as percentage? Or assume 0-1. Let's assume if > 1 it's percentage or invalid. Let's support >1 as percentage if reasonable, or strict 0-1. Actually, standard is 0 to 1, but if user enters 60, let's check if they meant percentage. Let's support explicit % or 0-1. If someone types 60 without %, let's treat as invalid probability unless <=1.
            return num;
        }

        function formatProb(val) {
            if (val === null || val === undefined || isNaN(val)) return '-';
            return val.toFixed(4);
        }

        function formatPercent(val) {
            if (val === null || val === undefined || isNaN(val)) return '-';
            return (val * 100).toFixed(2) + '%';
        }

        // Presets
        function loadPreset(type) {
            clearAllData();
            if (type === 'basic') {
                document.getElementById('in_pA').value = '0.6';
                document.getElementById('in_pB').value = '0.5';
                document.getElementById('in_pA_and_B').value = '0.3';
            } else if (type === 'medical') {
                // Disease B (0.01), Test positive A given B (0.95), Test positive A given B' (0.05)
                document.getElementById('in_pB').value = '0.01';
                document.getElementById('in_pA_given_B').value = '0.95';
                document.getElementById('in_pA_given_B_not').value = '0.05';
            }
            calculateAll();
        }

        function clearAllData() {
            const inputs = ['in_pA', 'in_pB', 'in_pA_and_B', 'in_pA_given_B', 'in_pB_given_A', 'in_pA_given_B_not', 'in_pA_not_given_B', 'in_pA_not_given_B_not'];
            inputs.forEach(id => document.getElementById(id).value = '');
            document.getElementById('resultsTableBody').innerHTML = `<tr><td colspan="4" class="py-8 text-center text-slate-400 italic">Vui lòng nhập dữ liệu và nhấn "Tính tất cả" ở cột bên trái.</td></tr>`;
            document.getElementById('validationBox').classList.add('hidden');
            document.getElementById('calcStatus').innerText = 'Chưa có dữ liệu đầy đủ';
            document.getElementById('calcStatus').className = 'text-xs px-2.5 py-1 bg-amber-100 text-amber-800 rounded-full font-medium';
            document.getElementById('formulaCardsGrid').innerHTML = `<div class="bg-white rounded-2xl p-5 border border-slate-200/80 text-center text-slate-400 italic col-span-2">Chưa có dữ liệu tính toán.</div>`;
            document.getElementById('stepsContainer').innerHTML = `<div class="text-center text-slate-400 italic text-xs py-8">Chưa có quá trình suy luận nào được thực hiện.</div>`;
            document.getElementById('treeContainer').innerHTML = `<p class="text-xs text-slate-400 italic">Vui lòng nhập dữ liệu để hiển thị sơ đồ cây.</p>`;
            
            // Clear matrix
            ['mat_A_B', 'mat_A_notB', 'mat_A_total', 'mat_notA_B', 'mat_notA_notB', 'mat_notA_total', 'mat_total_B', 'mat_total_notB'].forEach(id => {
                document.getElementById(id).innerText = '-';
            });
        }

        // Main Calculation & Multi-step Deduction Engine
        function calculateAll() {
            // Gather initial inputs
            let knowns = {};
            let stepsLog = [];

            const rawInputs = {
                pA: parseVal(document.getElementById('in_pA').value),
                pB: parseVal(document.getElementById('in_pB').value),
                pA_and_B: parseVal(document.getElementById('in_pA_and_B').value),
                pA_given_B: parseVal(document.getElementById('in_pA_given_B').value),
                pB_given_A: parseVal(document.getElementById('in_pB_given_A').value),
                pA_given_B_not: parseVal(document.getElementById('in_pA_given_B_not').value),
                pA_not_given_B: parseVal(document.getElementById('in_pA_not_given_B').value),
                pA_not_given_B_not: parseVal(document.getElementById('in_pA_not_given_B_not').value)
            };

            for (let k in rawInputs) {
                if (rawInputs[k] !== null && !isNaN(rawInputs[k])) {
                    knowns[k] = { val: rawInputs[k], formula: 'Dữ liệu nhập từ người dùng', subst: 'Nhập trực tiếp', stepNum: 0 };
                    stepsLog.push({ title: `Dữ liệu đầu vào: ${k}`, desc: `Người dùng cung cấp ${k} = ${rawInputs[k]}` });
                }
            }

            // The 4 fundamental joint probabilities that define the entire 2x2 space:
            // p11 = P(A ∩ B)
            // p12 = P(A ∩ B')
            // p21 = P(A' ∩ B)
            // p22 = P(A' ∩ B')
            // Let's also track marginals and conditionals.
            
            let changed = true;
            let iteration = 0;
            while (changed && iteration < 10) {
                changed = false;
                iteration++;

                // Helper to set known if not already known
                function setVal(key, val, formula, subst) {
                    if (knowns[key] === undefined && val !== null && !isNaN(val) && isFinite(val)) {
                        // Clamp between 0 and 1 for probability
                        val = Math.max(0, Math.min(1, val));
                        knowns[key] = { val: val, formula: formula, subst: subst, stepNum: iteration };
                        stepsLog.push({ title: `Bước suy luận (${key})`, desc: `Tính ${key} theo công thức: ${formula}`, subst: subst, result: val });
                        changed = true;
                    }
                }

                let pA = knowns.pA ? knowns.pA.val : null;
                let pB = knowns.pB ? knowns.pB.val : null;
                let pA_and_B = knowns.pA_and_B ? knowns.pA_and_B.val : null;
                let pA_given_B = knowns.pA_given_B ? knowns.pA_given_B.val : null;
                let pB_given_A = knowns.pB_given_A ? knowns.pB_given_A.val : null;
                let pA_given_B_not = knowns.pA_given_B_not ? knowns.pA_given_B_not.val : null;
                let pA_not_given_B = knowns.pA_not_given_B ? knowns.pA_not_given_B.val : null;
                let pA_not_given_B_not = knowns.pA_not_given_B_not ? knowns.pA_not_given_B_not.val : null;

                let pA_not = knowns.pA_not ? knowns.pA_not.val : (pA !== null ? 1 - pA : null);
                if (pA_not !== null && knowns.pA_not === undefined && pA !== null) {
                    setVal('pA_not', 1 - pA, 'P(A\') = 1 - P(A)', `1 - ${pA.toFixed(4)}`);
                }

                let pB_not = knowns.pB_not ? knowns.pB_not.val : (pB !== null ? 1 - pB : null);
                if (pB_not !== null && knowns.pB_not === undefined && pB !== null) {
                    setVal('pB_not', 1 - pB, 'P(B\') = 1 - P(B)', `1 - ${pB.toFixed(4)}`);
                }

                let pA_and_B_not = knowns.pA_and_B_not ? knowns.pA_and_B_not.val : null;
                let pA_not_and_B = knowns.pA_not_and_B ? knowns.pA_not_and_B.val : null;
                let pA_not_and_B_not = knowns.pA_not_and_B_not ? knowns.pA_not_and_B_not.val : null;

                // 1. From conditional & intersection
                if (pB !== null && pA_given_B !== null && pA_and_B === null) {
                    setVal('pA_and_B', pB * pA_given_B, 'P(A ∩ B) = P(B) × P(A|B)', `${pB.toFixed(4)} × ${pA_given_B.toFixed(4)}`);
                }
                if (pA !== null && pB_given_A !== null && pA_and_B === null) {
                    setVal('pA_and_B', pA * pB_given_A, 'P(A ∩ B) = P(A) × P(B|A)', `${pA.toFixed(4)} × ${pB_given_A.toFixed(4)}`);
                }
                if (pB !== null && pA_and_B !== null && pA_given_B === null && pB > 0) {
                    setVal('pA_given_B', pA_and_B / pB, 'P(A|B) = P(A ∩ B) / P(B)', `${pA_and_B.toFixed(4)} / ${pB.toFixed(4)}`);
                }
                if (pA !== null && pA_and_B !== null && pB_given_A === null && pA > 0) {
                    setVal('pB_given_A', pA_and_B / pA, 'P(B|A) = P(A ∩ B) / P(A)', `${pA_and_B.toFixed(4)} / ${pA.toFixed(4)}`);
                }

                // B' conditionals
                if (pB_not !== null && pA_given_B_not !== null && pA_and_B_not === null) {
                    setVal('pA_and_B_not', pB_not * pA_given_B_not, 'P(A ∩ B\') = P(B\') × P(A|B\')', `${pB_not.toFixed(4)} × ${pA_given_B_not.toFixed(4)}`);
                }
                if (pB_not !== null && pA_and_B_not !== null && pA_given_B_not === null && pB_not > 0) {
                    setVal('pA_given_B_not', pA_and_B_not / pB_not, 'P(A|B\') = P(A ∩ B\') / P(B\')', `${pA_and_B_not.toFixed(4)} / ${pB_not.toFixed(4)}`);
                }

                // A'|B conditionals
                if (pB !== null && pA_not_given_B !== null && pA_not_and_B === null) {
                    setVal('pA_not_and_B', pB * pA_not_given_B, 'P(A\' ∩ B) = P(B) × P(A\'|B)', `${pB.toFixed(4)} × ${pA_not_given_B.toFixed(4)}`);
                }
                if (pB !== null && pA_not_and_B !== null && pA_not_given_B === null && pB > 0) {
                    setVal('pA_not_given_B', pA_not_and_B / pB, 'P(A\'|B) = P(A\' ∩ B) / P(B)', `${pA_not_and_B.toFixed(4)} / ${pB.toFixed(4)}`);
                }

                // A'|B' conditionals
                if (pB_not !== null && pA_not_given_B_not !== null && pA_not_and_B_not === null) {
                    setVal('pA_not_and_B_not', pB_not * pA_not_given_B_not, 'P(A\' ∩ B\') = P(B\') × P(A\'|B\')', `${pB_not.toFixed(4)} × ${pA_not_given_B_not.toFixed(4)}`);
                }
                if (pB_not !== null && pA_not_and_B_not !== null && pA_not_given_B_not === null && pB_not > 0) {
                    setVal('pA_not_given_B_not', pA_not_and_B_not / pB_not, 'P(A\'|B\') = P(A\' ∩ B\') / P(B\')', `${pA_not_and_B_not.toFixed(4)} / ${pB_not.toFixed(4)}`);
                }

                // Marginals from joint relationships
                pA_and_B = knowns.pA_and_B ? knowns.pA_and_B.val : null;
                pA_and_B_not = knowns.pA_and_B_not ? knowns.pA_and_B_not.val : null;
                pA_not_and_B = knowns.pA_not_and_B ? knowns.pA_not_and_B.val : null;
                pA_not_and_B_not = knowns.pA_not_and_B_not ? knowns.pA_not_and_B_not.val : null;

                if (pA_and_B !== null && pA_and_B_not !== null && pA === null) {
                    setVal('pA', pA_and_B + pA_and_B_not, 'P(A) = P(A ∩ B) + P(A ∩ B\')', `${pA_and_B.toFixed(4)} + ${pA_and_B_not.toFixed(4)}`);
                }
                if (pA_not_and_B !== null && pA_not_and_B_not !== null && pA_not === null) {
                    setVal('pA_not', pA_not_and_B + pA_not_and_B_not, 'P(A\') = P(A\' ∩ B) + P(A\' ∩ B\')', `${pA_not_and_B.toFixed(4)} + ${pA_not_and_B_not.toFixed(4)}`);
                }
                if (pA_and_B !== null && pA_not_and_B !== null && pB === null) {
                    setVal('pB', pA_and_B + pA_not_and_B, 'P(B) = P(A ∩ B) + P(A\' ∩ B)', `${pA_and_B.toFixed(4)} + ${pA_not_and_B.toFixed(4)}`);
                }
                if (pA_and_B_not !== null && pA_not_and_B_not !== null && pB_not === null) {
                    setVal('pB_not', pA_and_B_not + pA_not_and_B_not, 'P(B\') = P(A ∩ B\') + P(A\' ∩ B\')', `${pA_and_B_not.toFixed(4)} + ${pA_not_and_B_not.toFixed(4)}`);
                }

                // Deduce joint from marginal and another joint
                pA = knowns.pA ? knowns.pA.val : null;
                pB = knowns.pB ? knowns.pB.val : null;
                pA_not = knowns.pA_not ? knowns.pA_not.val : null;
                pB_not = knowns.pB_not ? knowns.pB_not.val : null;

                if (pA !== null && pA_and_B !== null && pA_and_B_not === null) {
                    setVal('pA_and_B_not', pA - pA_and_B, 'P(A ∩ B\') = P(A) - P(A ∩ B)', `${pA.toFixed(4)} - ${pA_and_B.toFixed(4)}`);
                }
                if (pA !== null && pA_and_B_not !== null && pA_and_B === null) {
                    setVal('pA_and_B', pA - pA_and_B_not, 'P(A ∩ B) = P(A) - P(A ∩ B\')', `${pA.toFixed(4)} - ${pA_and_B_not.toFixed(4)}`);
                }
                if (pB !== null && pA_and_B !== null && pA_not_and_B === null) {
                    setVal('pA_not_and_B', pB - pA_and_B, 'P(A\' ∩ B) = P(B) - P(A ∩ B)', `${pB.toFixed(4)} - ${pA_and_B.toFixed(4)}`);
                }
                if (pB !== null && pA_not_and_B !== null && pA_and_B === null) {
                    setVal('pA_and_B', pB - pA_not_and_B, 'P(A ∩ B) = P(B) - P(A\' ∩ B)', `${pB.toFixed(4)} - ${pA_not_and_B.toFixed(4)}`);
                }
                if (pB_not !== null && pA_and_B_not !== null && pA_not_and_B_not === null) {
                    setVal('pA_not_and_B_not', pB_not - pA_and_B_not, 'P(A\' ∩ B\') = P(B\') - P(A ∩ B\')', `${pB_not.toFixed(4)} - ${pA_and_B_not.toFixed(4)}`);
                }
                if (pB_not !== null && pA_not_and_B_not !== null && pA_and_B_not === null) {
                    setVal('pA_and_B_not', pB_not - pA_not_and_B_not, 'P(A ∩ B\') = P(B\') - P(A\' ∩ B\')', `${pB_not.toFixed(4)} - ${pA_not_and_B_not.toFixed(4)}`);
                }
                if (pA_not !== null && pA_not_and_B !== null && pA_not_and_B_not === null) {
                    setVal('pA_not_and_B_not', pA_not - pA_not_and_B, 'P(A\' ∩ B\') = P(A\') - P(A\' ∩ B)', `${pA_not.toFixed(4)} - ${pA_not_and_B.toFixed(4)}`);
                }
                if (pA_not !== null && pA_not_and_B_not !== null && pA_not_and_B === null) {
                    setVal('pA_not_and_B', pA_not - pA_not_and_B_not, 'P(A\' ∩ B) = P(A\') - P(A\' ∩ B\')', `${pA_not.toFixed(4)} - ${pA_not_and_B_not.toFixed(4)}`);
                }

                // Total sum = 1 check and completion
                pA_and_B = knowns.pA_and_B ? knowns.pA_and_B.val : null;
                pA_and_B_not = knowns.pA_and_B_not ? knowns.pA_and_B_not.val : null;
                pA_not_and_B = knowns.pA_not_and_B ? knowns.pA_not_and_B.val : null;
                pA_not_and_B_not = knowns.pA_not_and_B_not ? knowns.pA_not_and_B_not.val : null;

                let jointsCount = [pA_and_B, pA_and_B_not, pA_not_and_B, pA_not_and_B_not].filter(x => x !== null).length;
                if (jointsCount === 3) {
                    if (pA_and_B === null) setVal('pA_and_B', 1 - (pA_and_B_not + pA_not_and_B + pA_not_and_B_not), 'Tổng 4 ô xác suất bằng 1', `1 - (${pA_and_B_not.toFixed(4)} + ${pA_not_and_B.toFixed(4)} + ${pA_not_and_B_not.toFixed(4)})`);
                    if (pA_and_B_not === null) setVal('pA_and_B_not', 1 - (pA_and_B + pA_not_and_B + pA_not_and_B_not), 'Tổng 4 ô xác suất bằng 1', `1 - (${pA_and_B.toFixed(4)} + ${pA_not_and_B.toFixed(4)} + ${pA_not_and_B_not.toFixed(4)})`);
                    if (pA_not_and_B === null) setVal('pA_not_and_B', 1 - (pA_and_B + pA_and_B_not + pA_not_and_B_not), 'Tổng 4 ô xác suất bằng 1', `1 - (${pA_and_B.toFixed(4)} + ${pA_and_B_not.toFixed(4)} + ${pA_not_and_B_not.toFixed(4)})`);
                    if (pA_not_and_B_not === null) setVal('pA_not_and_B_not', 1 - (pA_and_B + pA_and_B_not + pA_not_and_B), 'Tổng 4 ô xác suất bằng 1', `1 - (${pA_and_B.toFixed(4)} + ${pA_and_B_not.toFixed(4)} + ${pA_not_and_B.toFixed(4)})`);
                }

                // Law of total probability & Bayes remaining conditionals
                pA = knowns.pA ? knowns.pA.val : null;
                pB = knowns.pB ? knowns.pB.val : null;
                pB_not = knowns.pB_not ? knowns.pB_not.val : null;
                pA_not = knowns.pA_not ? knowns.pA_not.val : null;

                if (pB !== null && pB > 0 && pA_and_B !== null && knowns.pA_given_B === undefined) {
                    setVal('pA_given_B', pA_and_B / pB, 'P(A|B) = P(A ∩ B) / P(B)', `${pA_and_B.toFixed(4)} / ${pB.toFixed(4)}`);
                }
                if (pB !== null && pB > 0 && pA_not_and_B !== null && knowns.pA_not_given_B === undefined) {
                    setVal('pA_not_given_B', pA_not_and_B / pB, 'P(A\'|B) = P(A\' ∩ B) / P(B)', `${pA_not_and_B.toFixed(4)} / ${pB.toFixed(4)}`);
                }
                if (pB_not !== null && pB_not > 0 && pA_and_B_not !== null && knowns.pA_given_B_not === undefined) {
                    setVal('pA_given_B_not', pA_and_B_not / pB_not, 'P(A|B\') = P(A ∩ B\') / P(B\')', `${pA_and_B_not.toFixed(4)} / ${pB_not.toFixed(4)}`);
                }
                if (pB_not !== null && pB_not > 0 && pA_not_and_B_not !== null && knowns.pA_not_given_B_not === undefined) {
                    setVal('pA_not_given_B_not', pA_not_and_B_not / pB_not, 'P(A\'|B\') = P(A\' ∩ B\') / P(B\')', `${pA_not_and_B_not.toFixed(4)} / ${pB_not.toFixed(4)}`);
                }

                // Reverse conditionals: P(B|A), P(B|A'), etc.
                pA = knowns.pA ? knowns.pA.val : null;
                pA_not = knowns.pA_not ? knowns.pA_not.val : null;
                if (pA !== null && pA > 0 && pA_and_B !== null && knowns.pB_given_A === undefined) {
                    setVal('pB_given_A', pA_and_B / pA, 'P(B|A) = P(A ∩ B) / P(A)', `${pA_and_B.toFixed(4)} / ${pA.toFixed(4)}`);
                }
                if (pA !== null && pA > 0 && pA_and_B_not !== null && knowns.pB_not_given_A === undefined) {
                    setVal('pB_not_given_A', pA_and_B_not / pA, 'P(B\'|A) = P(A ∩ B\') / P(A)', `${pA_and_B_not.toFixed(4)} / ${pA.toFixed(4)}`);
                }
                if (pA_not !== null && pA_not > 0 && pA_not_and_B !== null && knowns.pB_given_A_not === undefined) {
                    setVal('pB_given_A_not', pA_not_and_B / pA_not, 'P(B|A\') = P(A\' ∩ B) / P(A\')', `${pA_not_and_B.toFixed(4)} / ${pA_not.toFixed(4)}`);
                }
                if (pA_not !== null && pA_not > 0 && pA_not_and_B_not !== null && knowns.pB_not_given_A_not === undefined) {
                    setVal('pB_not_given_A_not', pA_not_and_B_not / pA_not, 'P(B\'|A\') = P(A\' ∩ B\') / P(A\')', `${pA_not_and_B_not.toFixed(4)} / ${pA_not.toFixed(4)}`);
                }

                // Unions
                pA = knowns.pA ? knowns.pA.val : null;
                pB = knowns.pB ? knowns.pB.val : null;
                pB_not = knowns.pB_not ? knowns.pB_not.val : null;
                pA_not = knowns.pA_not ? knowns.pA_not.val : null;
                pA_and_B = knowns.pA_and_B ? knowns.pA_and_B.val : null;
                pA_and_B_not = knowns.pA_and_B_not ? knowns.pA_and_B_not.val : null;
                pA_not_and_B = knowns.pA_not_and_B ? knowns.pA_not_and_B.val : null;
                pA_not_and_B_not = knowns.pA_not_and_B_not ? knowns.pA_not_and_B_not.val : null;

                if (pA !== null && pB !== null && pA_and_B !== null && knowns.pA_or_B === undefined) {
                    setVal('pA_or_B', pA + pB - pA_and_B, 'P(A ∪ B) = P(A) + P(B) - P(A ∩ B)', `${pA.toFixed(4)} + ${pB.toFixed(4)} - ${pA_and_B.toFixed(4)}`);
                }
                if (pA !== null && pB_not !== null && pA_and_B_not !== null && knowns.pA_or_B_not === undefined) {
                    setVal('pA_or_B_not', pA + pB_not - pA_and_B_not, 'P(A ∪ B\') = P(A) + P(B\') - P(A ∩ B\')', `${pA.toFixed(4)} + ${pB_not.toFixed(4)} - ${pA_and_B_not.toFixed(4)}`);
                }
                if (pA_not !== null && pB !== null && pA_not_and_B !== null && knowns.pA_not_or_B === undefined) {
                    setVal('pA_not_or_B', pA_not + pB - pA_not_and_B, 'P(A\' ∪ B) = P(A\') + P(B) - P(A\' ∩ B)', `${pA_not.toFixed(4)} + ${pB.toFixed(4)} - ${pA_not_and_B.toFixed(4)}`);
                }
                if (pA_not !== null && pB_not !== null && pA_not_and_B_not !== null && knowns.pA_not_or_B_not === undefined) {
                    setVal('pA_not_or_B_not', pA_not + pB_not - pA_not_and_B_not, 'P(A\' ∪ B\') = P(A\') + P(B\') - P(A\' ∩ B\')', `${pA_not.toFixed(4)} + ${pB_not.toFixed(4)} - ${pA_not_and_B_not.toFixed(4)}`);
                }
            }

            // Validation checks
            let validations = [];
            let pA = knowns.pA ? knowns.pA.val : null;
            let pB = knowns.pB ? knowns.pB.val : null;
            let pA_and_B = knowns.pA_and_B ? knowns.pA_and_B.val : null;
            let pA_and_B_not = knowns.pA_and_B_not ? knowns.pA_and_B_not.val : null;
            let pA_not_and_B = knowns.pA_not_and_B ? knowns.pA_not_and_B.val : null;
            let pA_not_and_B_not = knowns.pA_not_and_B_not ? knowns.pA_not_and_B_not.val : null;

            if (pA !== null && (pA < 0 || pA > 1)) validations.push({ type: 'error', text: `P(A) = ${pA.toFixed(2)} nằm ngoài khoảng [0, 1].` });
            if (pB !== null && (pB < 0 || pB > 1)) validations.push({ type: 'error', text: `P(B) = ${pB.toFixed(2)} nằm ngoài khoảng [0, 1].` });
            if (pA_and_B !== null && pA !== null && pA_and_B > pA) validations.push({ type: 'error', text: `Mâu thuẫn: P(A ∩ B) (${pA_and_B.toFixed(2)}) lớn hơn P(A) (${pA.toFixed(2)}).` });
            if (pA_and_B !== null && pB !== null && pA_and_B > pB) validations.push({ type: 'error', text: `Mâu thuẫn: P(A ∩ B) (${pA_and_B.toFixed(2)}) lớn hơn P(B) (${pB.toFixed(2)}).` });
            if (pB === 0 && knowns.pA_given_B !== undefined) validations.push({ type: 'warning', text: `Không xác định P(A|B) vì P(B) = 0.` });

            let joints = [pA_and_B, pA_and_B_not, pA_not_and_B, pA_not_and_B_not].filter(x => x !== null);
            if (joints.length === 4) {
                let sumJoints = pA_and_B + pA_and_B_not + pA_not_and_B + pA_not_and_B_not;
                if (Math.abs(sumJoints - 1) > 0.001) {
                    validations.push({ type: 'warning', text: `Cảnh báo: Tổng 4 ô xác suất giao là ${sumJoints.toFixed(4)} (khác 1). Dữ liệu có thể chưa đồng nhất.` });
                }
            }

            // Render Validation Box
            let valBox = document.getElementById('validationBox');
            let valContent = document.getElementById('validationContent');
            if (validations.length > 0) {
                valBox.classList.remove('hidden');
                valContent.innerHTML = validations.map(v => `
                    <div class="p-2.5 rounded-xl flex items-start gap-2 ${v.type === 'error' ? 'bg-rose-50 text-rose-700 border border-rose-200' : 'bg-amber-50 text-amber-800 border border-amber-200'}">
                        <i class="fa-solid ${v.type === 'error' ? 'fa-circle-xmark text-rose-500' : 'fa-triangle-exclamation text-amber-500'} mt-0.5"></i>
                        <span>${v.text}</span>
                    </div>
                `).join('');
            } else {
                valBox.classList.add('hidden');
            }

            // Update Status Badge
            let countKnown = Object.keys(knowns).length;
            let statusBadge = document.getElementById('calcStatus');
            if (countKnown >= 8) {
                statusBadge.innerText = 'Đã suy luận đầy đủ';
                statusBadge.className = 'text-xs px-2.5 py-1 bg-emerald-100 text-emerald-800 rounded-full font-medium';
            } else if (countKnown > 0) {
                statusBadge.innerText = `Đã suy luận (${countKnown} đại lượng)`;
                statusBadge.className = 'text-xs px-2.5 py-1 bg-blue-100 text-blue-800 rounded-full font-medium';
            } else {
                statusBadge.innerText = 'Chưa có dữ liệu';
                statusBadge.className = 'text-xs px-2.5 py-1 bg-amber-100 text-amber-800 rounded-full font-medium';
            }

            // Render Results Table
            renderResultsTable(knowns);

            // Render Matrix 2x2
            renderMatrix(knowns);

            // Render Probability Tree
            renderTree(knowns);

            // Render Formula Cards
            renderFormulaCards(knowns);

            // Render Steps Explanation
            renderSteps(stepsLog);
        }

        function renderResultsTable(knowns) {
            const tableBody = document.getElementById('resultsTableBody');
            
            const defs = [
                { key: 'pA', name: 'P(A)', desc: 'Xác suất biến cố A' },
                { key: 'pA_not', name: "P(A')", desc: 'Xác suất biến cố đối A' },
                { key: 'pB', name: 'P(B)', desc: 'Xác suất biến cố B' },
                { key: 'pB_not', name: "P(B')", desc: 'Xác suất biến cố đối B' },
                { key: 'pA_and_B', name: 'P(A ∩ B)', desc: 'Xác suất giao A và B' },
                { key: 'pA_and_B_not', name: "P(A ∩ B')", desc: 'Xác suất A giao B đối' },
                { key: 'pA_not_and_B', name: "P(A' ∩ B)", desc: "Xác suất A đối giao B" },
                { key: 'pA_not_and_B_not', name: "P(A' ∩ B')", desc: "Xác suất cả hai biến cố đối" },
                { key: 'pA_or_B', name: 'P(A ∪ B)', desc: 'Xác suất hợp A và B' },
                { key: 'pA_given_B', name: 'P(A|B)', desc: 'Xác suất A với điều kiện B' },
                { key: 'pA_given_B_not', name: "P(A|B')", desc: "Xác suất A với điều kiện B đối" },
                { key: 'pA_not_given_B', name: "P(A'|B)", desc: "Xác suất A đối với điều kiện B" },
                { key: 'pA_not_given_B_not', name: "P(A'|B')", desc: "Xác suất cả hai đối với điều kiện B đối" },
                { key: 'pB_given_A', name: 'P(B|A)', desc: 'Xác suất B với điều kiện A' },
                { key: 'pB_not_given_A', name: "P(B'|A)", desc: "Xác suất B đối với điều kiện A" },
                { key: 'pB_given_A_not', name: "P(B|A')", desc: "Xác suất B với điều kiện A đối" },
                { key: 'pB_not_given_A_not', name: "P(B'|A')", desc: "Xác suất cả hai đối với điều kiện A đối" }
            ];

            let html = '';
            let hasAny = false;

            defs.forEach(item => {
                let k = knowns[item.key];
                if (k !== undefined) {
                    hasAny = true;
                    html += `
                        <tr class="hover:bg-slate-50/80 transition">
                            <td class="py-3 px-6 font-mono font-semibold text-blue-700">${item.name}</td>
                            <td class="py-3 px-6 text-slate-600 text-xs">${item.desc}</td>
                            <td class="py-3 px-6 text-right font-mono font-medium text-slate-800">${formatProb(k.val)}</td>
                            <td class="py-3 px-6 text-right font-mono font-semibold text-indigo-600">${formatPercent(k.val)}</td>
                        </tr>
                    `;
                }
            });

            if (!hasAny) {
                html = `<tr><td colspan="4" class="py-8 text-center text-slate-400 italic">Vui lòng nhập dữ liệu và nhấn "Tính tất cả" ở cột bên trái.</td></tr>`;
            }

            tableBody.innerHTML = html;
        }

        function renderMatrix(knowns) {
            const getVal = (key) => knowns[key] ? knowns[key].val : null;

            let pAB = getVal('pA_and_B');
            let pABnot = getVal('pA_and_B_not');
            let pNotAB = getVal('pA_not_and_B');
            let pNotABnot = getVal('pA_not_and_B_not');
            let pA = getVal('pA');
            let pNotA = getVal('pA_not');
            let pB = getVal('pB');
            let pNotB = getVal('pB_not');

            document.getElementById('mat_A_B').innerText = pAB !== null ? formatProb(pAB) + ` (${formatPercent(pAB)})` : '-';
            document.getElementById('mat_A_notB').innerText = pABnot !== null ? formatProb(pABnot) + ` (${formatPercent(pABnot)})` : '-';
            document.getElementById('mat_A_total').innerText = pA !== null ? formatProb(pA) + ` (${formatPercent(pA)})` : '-';

            document.getElementById('mat_notA_B').innerText = pNotAB !== null ? formatProb(pNotAB) + ` (${formatPercent(pNotAB)})` : '-';
            document.getElementById('mat_notA_notB').innerText = pNotABnot !== null ? formatProb(pNotABnot) + ` (${formatPercent(pNotABnot)})` : '-';
            document.getElementById('mat_notA_total').innerText = pNotA !== null ? formatProb(pNotA) + ` (${formatPercent(pNotA)})` : '-';

            document.getElementById('mat_total_B').innerText = pB !== null ? formatProb(pB) + ` (${formatPercent(pB)})` : '-';
            document.getElementById('mat_total_notB').innerText = pNotB !== null ? formatProb(pNotB) + ` (${formatPercent(pNotB)})` : '-';
        }

        function renderTree(knowns) {
            const getVal = (key) => knowns[key] ? knowns[key].val : null;
            let pB = getVal('pB');
            let pBnot = getVal('pB_not');
            let pAGivenB = getVal('pA_given_B');
            let pNotAGivenB = getVal('pA_not_given_B');
            let pAGivenBnot = getVal('pA_given_B_not');
            let pNotAGivenBnot = getVal('pA_not_given_B_not');

            let container = document.getElementById('treeContainer');
            if (pB === null && pBnot === null) {
                container.innerHTML = `<p class="text-xs text-slate-400 italic">Cần có thông tin về P(B) hoặc P(B') để vẽ sơ đồ cây.</p>`;
                return;
            }

            container.innerHTML = `
                <div class="flex flex-col md:flex-row items-center justify-around gap-8 w-full py-4 font-mono text-xs">
                    <!-- Branch B -->
                    <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm space-y-3 w-full md:w-1/2">
                        <div class="flex items-center justify-between pb-2 border-b border-slate-100 font-bold text-blue-700">
                            <span>Nhánh B</span>
                            <span>P(B) = ${pB !== null ? formatProb(pB) + ' (' + formatPercent(pB) + ')' : '?'}</span>
                        </div>
                        <div class="space-y-2 pl-4 border-l-2 border-blue-200">
                            <div class="flex justify-between items-center bg-slate-50 p-2 rounded-lg">
                                <span class="font-semibold text-slate-700">→ A | B</span>
                                <span class="text-indigo-600 font-semibold">${pAGivenB !== null ? formatProb(pAGivenB) + ' (' + formatPercent(pAGivenB) + ')' : '?'}</span>
                            </div>
                            <div class="flex justify-between items-center bg-slate-50 p-2 rounded-lg">
                                <span class="font-semibold text-slate-700">→ A' | B</span>
                                <span class="text-indigo-600 font-semibold">${pNotAGivenB !== null ? formatProb(pNotAGivenB) + ' (' + formatPercent(pNotAGivenB) + ')' : '?'}</span>
                            </div>
                        </div>
                    </div>

                    <!-- Branch B' -->
                    <div class="bg-white p-4 rounded-xl border border-slate-200 shadow-sm space-y-3 w-full md:w-1/2">
                        <div class="flex items-center justify-between pb-2 border-b border-slate-100 font-bold text-indigo-700">
                            <span>Nhánh B'</span>
                            <span>P(B') = ${pBnot !== null ? formatProb(pBnot) + ' (' + formatPercent(pBnot) + ')' : '?'}</span>
                        </div>
                        <div class="space-y-2 pl-4 border-l-2 border-indigo-200">
                            <div class="flex justify-between items-center bg-slate-50 p-2 rounded-lg">
                                <span class="font-semibold text-slate-700">→ A | B'</span>
                                <span class="text-indigo-600 font-semibold">${pAGivenBnot !== null ? formatProb(pAGivenBnot) + ' (' + formatPercent(pAGivenBnot) + ')' : '?'}</span>
                            </div>
                            <div class="flex justify-between items-center bg-slate-50 p-2 rounded-lg">
                                <span class="font-semibold text-slate-700">→ A' | B'</span>
                                <span class="text-indigo-600 font-semibold">${pNotAGivenBnot !== null ? formatProb(pNotAGivenBnot) + ' (' + formatPercent(pNotAGivenBnot) + ')' : '?'}</span>
                            </div>
                        </div>
                    </div>
                </div>
            `;
        }

        function renderFormulaCards(knowns) {
            const grid = document.getElementById('formulaCardsGrid');
            
            const cards = [
                { key: 'pA_not', name: "P(A')", formula: "P(A') = 1 - P(A)" },
                { key: 'pB_not', name: "P(B')", formula: "P(B') = 1 - P(B)" },
                { key: 'pA_given_B', name: "P(A|B)", formula: "P(A|B) = P(A ∩ B) / P(B)" },
                { key: 'pA_given_B_not', name: "P(A|B')", formula: "P(A|B') = P(A ∩ B') / P(B')" },
                { key: 'pA_not_given_B', name: "P(A'|B)", formula: "P(A'|B) = P(A' ∩ B) / P(B)" },
                { key: 'pA_not_given_B_not', name: "P(A'|B')", formula: "P(A'|B') = P(A' ∩ B') / P(B')" },
                { key: 'pA_and_B', name: "P(A ∩ B)", formula: "P(A ∩ B) = P(B) × P(A|B)" },
                { key: 'pA_and_B_not', name: "P(A ∩ B')", formula: "P(A ∩ B') = P(B') × P(A|B')" },
                { key: 'pA_not_and_B', name: "P(A' ∩ B)", formula: "P(A' ∩ B) = P(B) - P(A ∩ B)" },
                { key: 'pA_not_and_B_not', name: "P(A' ∩ B')", formula: "P(A' ∩ B') = 1 - P(A ∩ B) - P(A' ∩ B) - P(A ∩ B')" },
                { key: 'pA_or_B', name: "P(A ∪ B)", formula: "P(A ∪ B) = P(A) + P(B) - P(A ∩ B)" },
                { key: 'pB_given_A', name: "P(B|A)", formula: "P(B|A) = [P(A|B) × P(B)] / P(A) (Bayes)" }
            ];

            let html = '';
            cards.forEach(c => {
                let k = knowns[c.key];
                let valStr = k !== undefined ? `${formatProb(k.val)} = ${formatPercent(k.val)}` : 'Chưa đủ dữ liệu';
                let substStr = k !== undefined ? k.subst : '-';
                let formulaUsed = k !== undefined ? k.formula : c.formula;

                html += `
                    <div class="bg-white rounded-2xl p-5 border border-slate-200/80 shadow-sm space-y-3 flex flex-col justify-between">
                        <div>
                            <div class="flex justify-between items-center mb-2">
                                <span class="font-bold text-slate-900 font-mono text-base">${c.name}</span>
                                <span class="text-[10px] uppercase tracking-wider px-2 py-0.5 rounded-full font-semibold ${k !== undefined ? 'bg-emerald-100 text-emerald-800' : 'bg-slate-100 text-slate-500'}">
                                    ${k !== undefined ? 'Đã tính' : 'Chưa có'}
                                </span>
                            </div>
                            <div class="text-xs text-slate-500 font-mono bg-slate-50 p-2 rounded-xl border border-slate-100 mb-2">
                                <strong>Công thức:</strong> ${formulaUsed}
                            </div>
                            <div class="text-xs text-slate-600 font-mono">
                                <strong>Thay số:</strong> ${substStr}
                            </div>
                        </div>
                        <div class="pt-3 border-t border-slate-100 flex justify-between items-center">
                            <span class="text-xs font-medium text-slate-500">Kết quả:</span>
                            <span class="font-mono font-bold text-blue-700 text-sm">${valStr}</span>
                        </div>
                    </div>
                `;
            });

            grid.innerHTML = html;
        }

        function renderSteps(stepsLog) {
            const container = document.getElementById('stepsContainer');
            if (stepsLog.length === 0) {
                container.innerHTML = `<div class="text-center text-slate-400 italic text-xs py-8">Chưa có quá trình suy luận nào được thực hiện.</div>`;
                return;
            }

            container.innerHTML = stepsLog.map((step, idx) => `
                <div class="p-4 bg-slate-50 rounded-xl border border-slate-200/60 space-y-2">
                    <div class="flex items-center justify-between">
                        <span class="text-xs font-bold text-blue-700 uppercase tracking-wide">Bước ${idx + 1}: ${step.title}</span>
                        ${step.result !== undefined ? `<span class="text-xs font-mono font-bold text-indigo-600 bg-indigo-50 px-2 py-0.5 rounded">Kết quả: ${formatProb(step.result)} (${formatPercent(step.result)})</span>` : ''}
                    </div>
                    <p class="text-xs text-slate-700 font-medium">${step.desc}</p>
                    ${step.subst ? `<p class="text-xs font-mono text-slate-500 bg-white p-2 rounded border border-slate-200">Thay số: ${step.subst}</p>` : ''}
                </div>
            `).join('');
        }

        // Run initial calculation on load if preset wanted or empty
        window.onload = function() {
            loadPreset('basic');
        };
    </script>
</body>
</html>
