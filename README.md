```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>شركة الباشا - نظام إدارة الموظفين والأجور</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts - Cairo -->
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700;800;900&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        navy: {
                            800: '#0f172a',
                            900: '#090d16',
                            950: '#030712',
                        },
                        gold: {
                            400: '#fbbf24',
                            500: '#f59e0b',
                            600: '#d97706',
                        }
                    },
                    fontFamily: {
                        cairo: ['Cairo', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #f8fafc;
        }
        @media print {
            .no-print {
                display: none !important;
            }
            .print-only {
                display: block !important;
            }
            .print-shadow-none {
                box-shadow: none !important;
                border: 1px solid #cbd5e1 !important;
            }
            body {
                background-color: white !important;
            }
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94a3b8;
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 min-h-screen flex flex-col justify-between">

    <!-- Toast Notification -->
    <div id="toast" class="fixed bottom-5 left-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 ease-in-out bg-slate-900 text-white px-5 py-3 rounded-xl shadow-2xl flex items-center gap-3 border border-slate-700">
        <i id="toastIcon" class="fas fa-check-circle text-emerald-400 text-xl"></i>
        <span id="toastMsg" class="font-semibold text-sm">تمت العملية بنجاح</span>
    </div>

    <!-- Main Container -->
    <div class="flex-grow flex flex-col">
        
        <!-- Header / Navigation Bar -->
        <header class="bg-gradient-to-r from-slate-950 via-slate-900 to-slate-950 text-white shadow-xl no-print border-b border-amber-500/20">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
                <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                    <!-- Logo and Title -->
                    <div class="flex items-center gap-3">
                        <div class="w-12 h-12 rounded-xl bg-gradient-to-tr from-amber-600 to-amber-400 flex items-center justify-center text-slate-950 shadow-lg shadow-amber-500/20 font-black text-2xl">
                            <i class="fas fa-crown"></i>
                        </div>
                        <div>
                            <h1 class="text-2xl font-black tracking-wide text-transparent bg-clip-text bg-gradient-to-r from-amber-200 via-amber-400 to-amber-500">
                                شركة الباشا
                            </h1>
                            <p class="text-xs text-slate-400 font-medium">نظام إدارة الحضور والسُلف والحسابات الشهري</p>
                        </div>
                    </div>

                    <!-- Navigation Tabs -->
                    <nav class="flex flex-wrap justify-center gap-1 sm:gap-2 bg-slate-800/80 p-1.5 rounded-xl border border-slate-700/60">
                        <button onclick="switchTab('employees')" id="tab-employees" class="nav-btn px-4 py-2 rounded-lg text-sm font-bold transition-all flex items-center gap-2">
                            <i class="fas fa-users"></i>
                            <span>الموظفين</span>
                        </button>
                        <button onclick="switchTab('attendance')" id="tab-attendance" class="nav-btn px-4 py-2 rounded-lg text-sm font-bold transition-all flex items-center gap-2">
                            <i class="fas fa-calendar-check"></i>
                            <span>الحضور اليومي</span>
                        </button>
                        <button onclick="switchTab('advances')" id="tab-advances" class="nav-btn px-4 py-2 rounded-lg text-sm font-bold transition-all flex items-center gap-2">
                            <i class="fas fa-hand-holding-usd"></i>
                            <span>السُلف المالية</span>
                        </button>
                        <button onclick="switchTab('report')" id="tab-report" class="nav-btn px-4 py-2 rounded-lg text-sm font-bold transition-all flex items-center gap-2">
                            <i class="fas fa-file-invoice-dollar"></i>
                            <span>التعداد الشهري</span>
                        </button>
                    </nav>
                </div>
            </div>
        </header>

        <!-- Printable Header (Only visible in Print) -->
        <div class="hidden print-only p-6 border-b-2 border-slate-900 mb-6">
            <div class="flex justify-between items-center">
                <div>
                    <h1 class="text-3xl font-black text-slate-900">شركة الباشا</h1>
                    <p class="text-sm text-slate-600 mt-1">تقرير الحسابات والتعداد الشهري المعتمد</p>
                </div>
                <div class="text-left text-sm text-slate-600">
                    <p id="print-report-date" class="font-bold"></p>
                    <p>تاريخ الطباعة: <span id="print-today-date"></span></p>
                </div>
            </div>
        </div>

        <!-- Main Content Body -->
        <main class="max-w-7xl mx-auto w-full px-4 sm:px-6 lg:px-8 py-8 flex-grow">

            <!-- SECTION 1: EMPLOYEES MANAGEMENT -->
            <section id="sec-employees" class="tab-content hidden space-y-6">
                <!-- Top Actions Bar -->
                <div class="flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 bg-white p-5 rounded-2xl shadow-sm border border-slate-200">
                    <div>
                        <h2 class="text-xl font-bold text-slate-900 flex items-center gap-2">
                            <i class="fas fa-users text-amber-500"></i>
                            قائمة الموظفين
                        </h2>
                        <p class="text-sm text-slate-500 mt-0.5">إدارة بيانات الموظفين وسعر اليومية الخاصة بكل موظف</p>
                    </div>
                    <button onclick="openEmployeeModal()" class="w-full sm:w-auto bg-slate-900 hover:bg-slate-800 text-amber-400 font-bold px-5 py-2.5 rounded-xl transition-all shadow-md flex items-center justify-center gap-2 border border-amber-500/30">
                        <i class="fas fa-plus-circle"></i>
                        <span>إضافة موظف جديد</span>
                    </button>
                </div>

                <!-- Employees Table -->
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                    <div class="overflow-x-auto">
                        <table class="w-full text-right border-collapse">
                            <thead>
                                <tr class="bg-slate-100/80 text-slate-700 text-xs font-bold uppercase border-b border-slate-200">
                                    <th class="py-4 px-6">#</th>
                                    <th class="py-4 px-6">اسم الموظف</th>
                                    <th class="py-4 px-6">سعر اليومية</th>
                                    <th class="py-4 px-6 text-center">الإجراءات</th>
                                </tr>
                            </thead>
                            <tbody id="employees-tbody" class="divide-y divide-slate-100 text-sm">
                                <!-- Dynamic rows -->
                            </tbody>
                        </table>
                    </div>
                    <div id="no-emp-msg" class="hidden p-8 text-center text-slate-400">
                        <i class="fas fa-user-slash text-4xl mb-3 block"></i>
                        لا يوجد موظفين مسجلين حالياً. قم بإضافة موظف جديد للبدء.
                    </div>
                </div>
            </section>

            <!-- SECTION 2: ATTENDANCE -->
            <section id="sec-attendance" class="tab-content hidden space-y-6">
                <!-- Date Filter & Actions Header -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-200 flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
                    <div class="flex flex-col sm:flex-row items-start sm:items-center gap-4 w-full md:w-auto">
                        <div class="w-full sm:w-auto">
                            <label class="block text-xs font-bold text-slate-500 mb-1">تاريخ الحضور</label>
                            <input type="date" id="attendance-date" onchange="loadAttendanceForDate()" class="w-full sm:w-auto bg-slate-50 border border-slate-300 rounded-xl px-4 py-2 font-bold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500">
                        </div>
                        <div class="flex items-center gap-2 mt-2 sm:mt-5 text-sm font-semibold text-slate-600 bg-slate-100 px-3 py-1.5 rounded-lg border border-slate-200">
                            <span>السجل لليوم:</span>
                            <span id="attendance-status-badge" class="px-2 py-0.5 rounded text-xs">جاري التحميل...</span>
                        </div>
                    </div>
                    
                    <div class="flex items-center gap-2 w-full md:w-auto justify-end">
                        <button onclick="setAllAttendance('full')" class="text-xs bg-emerald-50 hover:bg-emerald-100 text-emerald-700 font-bold border border-emerald-200 px-3 py-2 rounded-lg transition-all flex items-center gap-1">
                            <i class="fas fa-check-double"></i>
                            تحديد الجميع حاضر
                        </button>
                    </div>
                </div>

                <!-- Attendance Form List -->
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                    <div class="p-4 bg-slate-900 text-white flex justify-between items-center">
                        <span class="font-bold text-sm">تسجيل كشف اليوم</span>
                        <span class="text-xs text-amber-400 font-medium">* يتم الحفظ تلقائياً فور الاختيار</span>
                    </div>
                    <div id="attendance-list" class="divide-y divide-slate-100">
                        <!-- Dynamic list of employees for attendance -->
                    </div>
                </div>
            </section>

            <!-- SECTION 3: ADVANCES -->
            <section id="sec-advances" class="tab-content hidden space-y-6">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                    <!-- Add Advance Form -->
                    <div class="lg:col-span-1 bg-white p-6 rounded-2xl shadow-sm border border-slate-200 h-fit">
                        <h3 class="text-lg font-bold text-slate-900 mb-4 pb-3 border-b border-slate-100 flex items-center gap-2">
                            <i class="fas fa-plus-circle text-amber-500"></i>
                            تسجيل سُلفة جديدة
                        </h3>
                        <form id="advance-form" onsubmit="handleSaveAdvance(event)" class="space-y-4">
                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">الموظف</label>
                                <select id="adv-employee" required class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-sm font-semibold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500">
                                    <option value="">اختر الموظف...</option>
                                </select>
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">المبلغ (جنية)</label>
                                <input type="number" id="adv-amount" min="1" step="any" placeholder="0.00" required class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-sm font-semibold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500">
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">التاريخ</label>
                                <input type="date" id="adv-date" required class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-sm font-semibold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500">
                            </div>

                            <div>
                                <label class="block text-xs font-bold text-slate-700 mb-1">ملاحظة / سبب السلفة (اختياري)</label>
                                <textarea id="adv-note" rows="2" placeholder="أدخل أي تفاصيل إضافية..." class="w-full bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 text-sm font-semibold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500"></textarea>
                            </div>

                            <button type="submit" class="w-full bg-slate-900 hover:bg-slate-800 text-amber-400 font-bold py-2.5 rounded-xl transition-all shadow-md border border-amber-500/30 flex justify-center items-center gap-2">
                                <i class="fas fa-save"></i>
                                <span>حفظ السُلفة</span>
                            </button>
                        </form>
                    </div>

                    <!-- Advances Log Table -->
                    <div class="lg:col-span-2 bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
                        <div class="p-5 border-b border-slate-100 flex justify-between items-center bg-slate-50/50">
                            <div>
                                <h3 class="text-lg font-bold text-slate-900 flex items-center gap-2">
                                    <i class="fas fa-history text-amber-500"></i>
                                    سجل السُلف المسجلة
                                </h3>
                                <p class="text-xs text-slate-500 mt-0.5">عرض كافة المبالغ المسحوبة كـ سُلف</p>
                            </div>
                            <span id="advances-count" class="bg-amber-100 text-amber-800 text-xs font-bold px-2.5 py-1 rounded-full">0 سلفة</span>
                        </div>

                        <div class="overflow-x-auto">
                            <table class="w-full text-right border-collapse">
                                <thead>
                                    <tr class="bg-slate-100/80 text-slate-700 text-xs font-bold uppercase border-b border-slate-200">
                                        <th class="py-3.5 px-4">التاريخ</th>
                                        <th class="py-3.5 px-4">اسم الموظف</th>
                                        <th class="py-3.5 px-4">المبلغ</th>
                                        <th class="py-3.5 px-4">ملاحظات</th>
                                        <th class="py-3.5 px-4 text-center">حذف</th>
                                    </tr>
                                </thead>
                                <tbody id="advances-tbody" class="divide-y divide-slate-100 text-sm">
                                    <!-- Dynamic rows -->
                                </tbody>
                            </table>
                        </div>
                        <div id="no-adv-msg" class="hidden p-8 text-center text-slate-400">
                            <i class="fas fa-receipt text-4xl mb-3 block"></i>
                            لا توجد سُلف مسجلة حالياً.
                        </div>
                    </div>
                </div>
            </section>

            <!-- SECTION 4: MONTHLY REPORT -->
            <section id="sec-report" class="tab-content hidden space-y-6">
                <!-- Report Filters Bar -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-200 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4 no-print">
                    <div class="flex flex-wrap items-center gap-3 w-full sm:w-auto">
                        <div>
                            <label class="block text-xs font-bold text-slate-500 mb-1">اختر الشهر</label>
                            <select id="report-month" onchange="generateMonthlyReport()" class="bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 font-bold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm">
                                <option value="0">يناير (1)</option>
                                <option value="1">فبراير (2)</option>
                                <option value="2">مارس (3)</option>
                                <option value="3">أبريل (4)</option>
                                <option value="4">مايو (5)</option>
                                <option value="5">يونيو (6)</option>
                                <option value="6">يوليو (7)</option>
                                <option value="7">أغسطس (8)</option>
                                <option value="8">سبتمبر (9)</option>
                                <option value="9">أكتوبر (10)</option>
                                <option value="10">نوفمبر (11)</option>
                                <option value="11">ديسمبر (12)</option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-xs font-bold text-slate-500 mb-1">اختر السنة</label>
                            <select id="report-year" onchange="generateMonthlyReport()" class="bg-slate-50 border border-slate-300 rounded-xl px-3 py-2 font-bold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500 text-sm">
                                <!-- Dynamic years generated via JS -->
                            </select>
                        </div>
                    </div>

                    <div class="flex items-center gap-2">
                        <button onclick="window.print()" class="bg-slate-900 hover:bg-slate-800 text-white font-bold px-4 py-2.5 rounded-xl transition-all shadow-md border border-slate-700 flex items-center gap-2 text-sm">
                            <i class="fas fa-print text-amber-400"></i>
                            <span>طباعة التقرير</span>
                        </button>
                    </div>
                </div>

                <!-- Overall Key Statistics Cards -->
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 no-print">
                    <div class="bg-gradient-to-br from-slate-900 to-slate-800 text-white p-5 rounded-2xl border border-slate-700 shadow-md">
                        <p class="text-xs font-semibold text-slate-400">إجمالي الموظفين</p>
                        <h4 id="stat-total-emp" class="text-2xl font-black text-amber-400 mt-1">0</h4>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                        <p class="text-xs font-semibold text-slate-500">إجمالي الاستحقاق للأيام</p>
                        <h4 id="stat-total-earned" class="text-2xl font-black text-slate-900 mt-1">0.00 ج.م</h4>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                        <p class="text-xs font-semibold text-slate-500">إجمالي السُلف للشهر</p>
                        <h4 id="stat-total-advances" class="text-2xl font-black text-rose-600 mt-1">0.00 ج.م</h4>
                    </div>
                    <div class="bg-white p-5 rounded-2xl border border-amber-200 bg-amber-50/30 shadow-sm">
                        <p class="text-xs font-semibold text-amber-800">صافي المستحق النهائي</p>
                        <h4 id="stat-net-payout" class="text-2xl font-black text-emerald-700 mt-1">0.00 ج.م</h4>
                    </div>
                </div>

                <!-- Monthly Report Data Table -->
                <div class="bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden print-shadow-none">
                    <div class="p-4 bg-slate-900 text-white flex justify-between items-center no-print">
                        <span class="font-bold text-sm">جدول التعداد المستحق للشهر المحدد</span>
                        <span id="report-period-text" class="text-xs text-amber-400 font-bold"></span>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-right border-collapse">
                            <thead>
                                <tr class="bg-slate-100 text-slate-700 text-xs font-bold uppercase border-b border-slate-200">
                                    <th class="py-3.5 px-4">الموظف</th>
                                    <th class="py-3.5 px-4">اليومية</th>
                                    <th class="py-3.5 px-4 text-center">أيام الحضور</th>
                                    <th class="py-3.5 px-4">إجمالي المستحق</th>
                                    <th class="py-3.5 px-4 text-rose-600">إجمالي السُلف</th>
                                    <th class="py-3.5 px-4 text-emerald-700">المتبقي (الصافي)</th>
                                </tr>
                            </thead>
                            <tbody id="report-tbody" class="divide-y divide-slate-100 text-sm">
                                <!-- Dynamic rows -->
                            </tbody>
                            <tfoot>
                                <tr class="bg-slate-900 text-white font-bold text-sm border-t-2 border-amber-500">
                                    <td class="py-3.5 px-4" colspan="2">الإجمالي العام للشهر</td>
                                    <td id="foot-total-days" class="py-3.5 px-4 text-center text-amber-400">0</td>
                                    <td id="foot-total-earned" class="py-3.5 px-4 text-amber-400">0.00 ج.م</td>
                                    <td id="foot-total-advances" class="py-3.5 px-4 text-rose-400">0.00 ج.م</td>
                                    <td id="foot-net-payout" class="py-3.5 px-4 text-emerald-400">0.00 ج.م</td>
                                </tr>
                            </tfoot>
                        </table>
                    </div>
                </div>

                <!-- Print Footer / Signatures -->
                <div class="hidden print-only mt-12 pt-8 border-t border-slate-300">
                    <div class="grid grid-cols-3 text-center text-sm font-bold text-slate-800">
                        <div>
                            <p>المحاسب المسؤول</p>
                            <p class="mt-10">.......................</p>
                        </div>
                        <div>
                            <p>مدير الموارد البشرية</p>
                            <p class="mt-10">.......................</p>
                        </div>
                        <div>
                            <p>اعتماد مدير الشركة (الباشا)</p>
                            <p class="mt-10">.......................</p>
                        </div>
                    </div>
                </div>
            </section>

        </main>

        <!-- Footer -->
        <footer class="bg-slate-900 text-slate-400 py-4 text-center text-xs border-t border-slate-800 no-print">
            <p>جميع الحقوق محفوظة © شركة الباشا <span id="current-year"></span> - نظام الحسابات وإدارة الأجور</p>
        </footer>
    </div>

    <!-- MODAL: ADD / EDIT EMPLOYEE -->
    <div id="empModal" class="fixed inset-0 bg-slate-950/70 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white w-full max-w-md rounded-2xl shadow-2xl border border-slate-100 overflow-hidden transform transition-all">
            <div class="bg-slate-900 px-6 py-4 flex justify-between items-center border-b border-slate-800">
                <h3 id="empModalTitle" class="text-white font-bold text-lg flex items-center gap-2">
                    <i class="fas fa-user-plus text-amber-400"></i>
                    <span>إضافة موظف جديد</span>
                </h3>
                <button onclick="closeEmployeeModal()" class="text-slate-400 hover:text-white transition-colors">
                    <i class="fas fa-times text-xl"></i>
                </button>
            </div>
            
            <form id="empForm" onsubmit="handleSaveEmployee(event)" class="p-6 space-y-4">
                <input type="hidden" id="emp-id">

                <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">اسم الموظف الكامل</label>
                    <input type="text" id="emp-name" required placeholder="مثال: أحمد محمود علي" class="w-full bg-slate-50 border border-slate-300 rounded-xl px-4 py-2.5 text-sm font-semibold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>

                <div>
                    <label class="block text-xs font-bold text-slate-700 mb-1">سعر اليومية (بالجنية)</label>
                    <input type="number" id="emp-rate" min="0" step="any" required placeholder="0.00" class="w-full bg-slate-50 border border-slate-300 rounded-xl px-4 py-2.5 text-sm font-semibold text-slate-800 focus:outline-none focus:ring-2 focus:ring-amber-500">
                </div>

                <div class="flex justify-end gap-3 pt-4 border-t border-slate-100">
                    <button type="button" onclick="closeEmployeeModal()" class="px-4 py-2 rounded-xl text-slate-600 bg-slate-100 hover:bg-slate-200 text-sm font-bold transition-all">
                        إلغاء
                    </button>
                    <button type="submit" class="px-5 py-2 rounded-xl text-slate-950 bg-gradient-to-r from-amber-400 to-amber-500 hover:from-amber-500 hover:to-amber-600 text-sm font-bold transition-all shadow-md">
                        حفظ البيانات
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- JAVASCRIPT LOGIC -->
    <script>
        // ==========================================
        // STATE MANAGEMENT & LOCAL STORAGE
        // ==========================================
        const STORAGE_KEY_EMP = 'elpasha_employees';
        const STORAGE_KEY_ATT = 'elpasha_attendance';
        const STORAGE_KEY_ADV = 'elpasha_advances';

        let employees = [];
        let attendance = {}; // Format: { "YYYY-MM-DD": { empId: status } } -> status: 1 (full), 0.5 (half), 0 (absent)
        let advances = [];   // Array of { id, empId, amount, date, note }

        // Sample initial data if storage is empty
        const initialEmployees = [
            { id: '1', name: 'أحمد محمود العباسي', rate: 250 },
            { id: '2', name: 'محمد مصطفى كامل', rate: 300 },
            { id: '3', name: 'السيد عبد الرؤوف', rate: 200 },
            { id: '4', name: 'محمود حسن الشريف', rate: 220 }
        ];

        // Init App
        window.addEventListener('DOMContentLoaded', () => {
            loadDataFromStorage();
            initDateInputs();
            switchTab('attendance'); // Default active tab
            document.getElementById('current-year').textContent = new Date().getFullYear();
        });

        function loadDataFromStorage() {
            const rawEmp = localStorage.getItem(STORAGE_KEY_EMP);
            const rawAtt = localStorage.getItem(STORAGE_KEY_ATT);
            const rawAdv = localStorage.getItem(STORAGE_KEY_ADV);

            if (!rawEmp) {
                employees = [...initialEmployees];
                saveEmployees();
            } else {
                employees = JSON.parse(rawEmp);
            }

            attendance = rawAtt ? JSON.parse(rawAtt) : {};
            advances = rawAdv ? JSON.parse(rawAdv) : [];

            // Add sample advance if empty to illustrate functionality
            if (advances.length === 0 && employees.length > 0) {
                const today = new Date().toISOString().split('T')[0];
                advances = [
                    { id: 'adv-1', empId: employees[0].id, amount: 200, date: today, note: 'سلفة طارئة' }
                ];
                saveAdvances();
            }
        }

        function saveEmployees() {
            localStorage.setItem(STORAGE_KEY_EMP, JSON.stringify(employees));
        }

        function saveAttendance() {
            localStorage.setItem(STORAGE_KEY_ATT, JSON.stringify(attendance));
        }

        function saveAdvances() {
            localStorage.setItem(STORAGE_KEY_ADV, JSON.stringify(advances));
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const toastMsg = document.getElementById('toastMsg');
            const toastIcon = document.getElementById('toastIcon');

            toastMsg.textContent = message;
            if (type === 'success') {
                toastIcon.className = 'fas fa-check-circle text-emerald-400 text-xl';
            } else {
                toastIcon.className = 'fas fa-exclamation-circle text-rose-400 text-xl';
            }

            toast.classList.remove('translate-y-20', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');

            setTimeout(() => {
                toast.classList.remove('translate-y-0', 'opacity-100');
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }

        // Initialize Date Filters & Options
        function initDateInputs() {
            const today = new Date().toISOString().split('T')[0];
            document.getElementById('attendance-date').value = today;
            document.getElementById('adv-date').value = today;

            // Report Year Selector setup
            const yearSelect = document.getElementById('report-year');
            const currentYear = new Date().getFullYear();
            yearSelect.innerHTML = '';
            for (let y = currentYear - 2; y <= currentYear + 2; y++) {
                const opt = document.createElement('option');
                opt.value = y;
                opt.textContent = y;
                if (y === currentYear) opt.selected = true;
                yearSelect.appendChild(opt);
            }

            // Set current month in report filter
            document.getElementById('report-month').value = new Date().getMonth();
        }

        // Tab Switching Logic
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(sec => sec.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('bg-amber-400', 'text-slate-950', 'shadow-lg');
                btn.classList.add('text-slate-300', 'hover:bg-slate-800');
            });

            document.getElementById(`sec-${tabId}`).classList.remove('hidden');
            const activeBtn = document.getElementById(`tab-${tabId}`);
            activeBtn.classList.remove('text-slate-300', 'hover:bg-slate-800');
            activeBtn.classList.add('bg-amber-400', 'text-slate-950', 'shadow-lg');

            // Tab specific refreshes
            if (tabId === 'employees') renderEmployeesTable();
            if (tabId === 'attendance') loadAttendanceForDate();
            if (tabId === 'advances') renderAdvancesTab();
            if (tabId === 'report') generateMonthlyReport();
        }

        // ==========================================
        // SECTION 1: EMPLOYEES LOGIC
        // ==========================================
        function renderEmployeesTable() {
            const tbody = document.getElementById('employees-tbody');
            const noMsg = document.getElementById('no-emp-msg');
            tbody.innerHTML = '';

            if (employees.length === 0) {
                noMsg.classList.remove('hidden');
                return;
            }
            noMsg.classList.add('hidden');

            employees.forEach((emp, index) => {
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-slate-50/80 transition-colors';
                tr.innerHTML = `
                    <td class="py-4 px-6 text-slate-400 font-bold">${index + 1}</td>
                    <td class="py-4 px-6 font-bold text-slate-900">${escapeHtml(emp.name)}</td>
                    <td class="py-4 px-6 font-semibold text-slate-700">
                        <span class="bg-slate-100 text-slate-800 px-3 py-1 rounded-lg border border-slate-200">
                            ${Number(emp.rate).toFixed(2)} ج.م
                        </span>
                    </td>
                    <td class="py-4 px-6 text-center space-x-2 space-x-reverse">
                        <button onclick="editEmployee('${emp.id}')" class="text-slate-600 hover:text-amber-600 p-2 rounded-lg hover:bg-amber-50 transition-all">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button onclick="deleteEmployee('${emp.id}')" class="text-slate-600 hover:text-rose-600 p-2 rounded-lg hover:bg-rose-50 transition-all">
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function openEmployeeModal(empId = null) {
            const modal = document.getElementById('empModal');
            const title = document.getElementById('empModalTitle');
            const form = document.getElementById('empForm');

            form.reset();
            document.getElementById('emp-id').value = '';

            if (empId) {
                const emp = employees.find(e => e.id === empId);
                if (emp) {
                    title.innerHTML = `<i class="fas fa-user-edit text-amber-400"></i><span>تعديل بيانات الموظف</span>`;
                    document.getElementById('emp-id').value = emp.id;
                    document.getElementById('emp-name').value = emp.name;
                    document.getElementById('emp-rate').value = emp.rate;
                }
            } else {
                title.innerHTML = `<i class="fas fa-user-plus text-amber-400"></i><span>إضافة موظف جديد</span>`;
            }

            modal.classList.remove('hidden');
        }

        function closeEmployeeModal() {
            document.getElementById('empModal').classList.add('hidden');
        }

        function handleSaveEmployee(e) {
            e.preventDefault();
            const id = document.getElementById('emp-id').value;
            const name = document.getElementById('emp-name').value.trim();
            const rate = parseFloat(document.getElementById('emp-rate').value);

            if (!name || isNaN(rate)) return;

            if (id) {
                // Update
                const empIndex = employees.findIndex(e => e.id === id);
                if (empIndex !== -1) {
                    employees[empIndex].name = name;
                    employees[empIndex].rate = rate;
                    showToast('تم تعديل بيانات الموظف بنجاح');
                }
            } else {
                // Create
                const newEmp = {
                    id: Date.now().toString(),
                    name,
                    rate
                };
                employees.push(newEmp);
                showToast('تمت إضافة الموظف بنجاح');
            }

            saveEmployees();
            closeEmployeeModal();
            renderEmployeesTable();
        }

        function editEmployee(id) {
            openEmployeeModal(id);
        }

        function deleteEmployee(id) {
            if (confirm('هل أنت تأكد من حذف هذا الموظف؟ لن تتأثر الحسابات المكتملة سابقة.')) {
                employees = employees.filter(e => e.id !== id);
                saveEmployees();
                renderEmployeesTable();
                showToast('تم حذف الموظف');
            }
        }

        // ==========================================
        // SECTION 2: ATTENDANCE LOGIC
        // ==========================================
        function loadAttendanceForDate() {
            const date = document.getElementById('attendance-date').value;
            const listContainer = document.getElementById('attendance-list');
            const statusBadge = document.getElementById('attendance-status-badge');

            if (!date) return;

            listContainer.innerHTML = '';

            const dayRecord = attendance[date] || {};
            const isRecorded = attendance[date] !== undefined;

            if (isRecorded) {
                statusBadge.textContent = 'مسجل معتمد';
                statusBadge.className = 'px-2 py-0.5 rounded text-xs bg-emerald-100 text-emerald-800 font-bold';
            } else {
                statusBadge.textContent = 'غير مسجل بعد';
                statusBadge.className = 'px-2 py-0.5 rounded text-xs bg-amber-100 text-amber-800 font-bold';
            }

            if (employees.length === 0) {
                listContainer.innerHTML = `
                    <div class="p-8 text-center text-slate-400">
                        لا يوجد موظفين مسجلين في النظام. قم بفرز الموظفين من تبويب الموظفين أولاً.
                    </div>
                `;
                return;
            }

            employees.forEach(emp => {
                // Current status for this date: 1 = full, 0.5 = half, 0 = absent (default to absent if not explicitly set)
                const status = dayRecord[emp.id] !== undefined ? dayRecord[emp.id] : 0;

                const item = document.createElement('div');
                item.className = 'p-4 sm:p-5 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4 hover:bg-slate-50/50 transition-colors';
                
                item.innerHTML = `
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 rounded-full bg-slate-900 text-amber-400 font-bold flex items-center justify-center text-sm shadow-sm">
                            ${emp.name.charAt(0)}
                        </div>
                        <div>
                            <h4 class="font-bold text-slate-900">${escapeHtml(emp.name)}</h4>
                            <p class="text-xs text-slate-500">سعر اليومية: <span class="font-bold text-slate-700">${emp.rate} ج.م</span></p>
                        </div>
                    </div>

                    <div class="flex items-center gap-1.5 w-full sm:w-auto justify-stretch sm:justify-end">
                        <button onclick="setEmpAttendance('${emp.id}', 1)" class="flex-1 sm:flex-none px-3.5 py-2 rounded-xl text-xs font-bold border transition-all flex items-center justify-center gap-1 ${
                            status === 1 
                            ? 'bg-emerald-600 text-white border-emerald-600 shadow-md ring-2 ring-emerald-300' 
                            : 'bg-white text-slate-700 border-slate-300 hover:bg-slate-100'
                        }">
                            <i class="fas fa-check"></i>
                            <span>يوم كامل</span>
                        </button>

                        <button onclick="setEmpAttendance('${emp.id}', 0.5)" class="flex-1 sm:flex-none px-3.5 py-2 rounded-xl text-xs font-bold border transition-all flex items-center justify-center gap-1 ${
                            status === 0.5 
                            ? 'bg-amber-500 text-slate-950 border-amber-500 shadow-md ring-2 ring-amber-300' 
                            : 'bg-white text-slate-700 border-slate-300 hover:bg-slate-100'
                        }">
                            <i class="fas fa-adjust"></i>
                            <span>نصف يوم</span>
                        </button>

                        <button onclick="setEmpAttendance('${emp.id}', 0)" class="flex-1 sm:flex-none px-3.5 py-2 rounded-xl text-xs font-bold border transition-all flex items-center justify-center gap-1 ${
                            status === 0 
                            ? 'bg-rose-600 text-white border-rose-600 shadow-md ring-2 ring-rose-300' 
                            : 'bg-white text-slate-700 border-slate-300 hover:bg-slate-100'
                        }">
                            <i class="fas fa-times"></i>
                            <span>غائب</span>
                        </button>
                    </div>
                `;

                listContainer.appendChild(item);
            });
        }

        function setEmpAttendance(empId, status) {
            const date = document.getElementById('attendance-date').value;
            if (!date) return;

            if (!attendance[date]) {
                attendance[date] = {};
            }

            attendance[date][empId] = status;
            saveAttendance();
            loadAttendanceForDate();
            showToast('تم تحديث الحضور');
        }

        function setAllAttendance(type) {
            const date = document.getElementById('attendance-date').value;
            if (!date || employees.length === 0) return;

            if (!attendance[date]) {
                attendance[date] = {};
            }

            const val = type === 'full' ? 1 : 0;
            employees.forEach(e => {
                attendance[date][e.id] = val;
            });

            saveAttendance();
            loadAttendanceForDate();
            showToast(type === 'full' ? 'تم تسجيل الجميع حاضر (يوم كامل)' : 'تم إلغاء تسجيل الجميع');
        }

        // ==========================================
        // SECTION 3: ADVANCES LOGIC
        // ==========================================
        function renderAdvancesTab() {
            // Populate select box
            const select = document.getElementById('adv-employee');
            select.innerHTML = '<option value="">اختر الموظف...</option>';
            employees.forEach(emp => {
                const opt = document.createElement('option');
                opt.value = emp.id;
                opt.textContent = emp.name;
                select.appendChild(opt);
            });

            renderAdvancesTable();
        }

        function renderAdvancesTable() {
            const tbody = document.getElementById('advances-tbody');
            const noMsg = document.getElementById('no-adv-msg');
            const countBadge = document.getElementById('advances-count');

            tbody.innerHTML = '';
            countBadge.textContent = `${advances.length} سُلفة`;

            if (advances.length === 0) {
                noMsg.classList.remove('hidden');
                return;
            }
            noMsg.classList.add('hidden');

            // Sort advances newest first
            const sorted = [...advances].sort((a, b) => new Date(b.date) - new Date(a.date));

            sorted.forEach(adv => {
                const emp = employees.find(e => e.id === adv.empId);
                const empName = emp ? emp.name : 'موظف محذوف';

                const tr = document.createElement('tr');
                tr.className = 'hover:bg-slate-50/80 transition-colors';
                tr.innerHTML = `
                    <td class="py-3.5 px-4 font-semibold text-slate-600">${adv.date}</td>
                    <td class="py-3.5 px-4 font-bold text-slate-900">${escapeHtml(empName)}</td>
                    <td class="py-3.5 px-4 font-bold text-rose-600">${Number(adv.amount).toFixed(2)} ج.م</td>
                    <td class="py-3.5 px-4 text-xs text-slate-500 max-w-xs truncate">${adv.note ? escapeHtml(adv.note) : '-'}</td>
                    <td class="py-3.5 px-4 text-center">
                        <button onclick="deleteAdvance('${adv.id}')" class="text-slate-400 hover:text-rose-600 p-1.5 rounded-lg hover:bg-rose-50 transition-all">
                            <i class="fas fa-trash-alt"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
        }

        function handleSaveAdvance(e) {
            e.preventDefault();
            const empId = document.getElementById('adv-employee').value;
            const amount = parseFloat(document.getElementById('adv-amount').value);
            const date = document.getElementById('adv-date').value;
            const note = document.getElementById('adv-note').value.trim();

            if (!empId || isNaN(amount) || !date) return;

            const newAdv = {
                id: Date.now().toString(),
                empId,
                amount,
                date,
                note
            };

            advances.push(newAdv);
            saveAdvances();

            // Reset Form fields except date
            document.getElementById('adv-employee').value = '';
            document.getElementById('adv-amount').value = '';
            document.getElementById('adv-note').value = '';

            renderAdvancesTable();
            showToast('تم تسجيل السُلفة بنجاح');
        }

        function deleteAdvance(id) {
            if (confirm('هل أنت تأكد من حذف هذه السُلفة؟')) {
                advances = advances.filter(a => a.id !== id);
                saveAdvances();
                renderAdvancesTable();
                showToast('تم حذف السُلفة');
            }
        }

        // ==========================================
        // SECTION 4: MONTHLY REPORT LOGIC
        // ==========================================
        function generateMonthlyReport() {
            const selectedMonth = parseInt(document.getElementById('report-month').value);
            const selectedYear = parseInt(document.getElementById('report-year').value);

            const tbody = document.getElementById('report-tbody');
            tbody.innerHTML = '';

            const monthNames = [
                'يناير', 'فبراير', 'مارس', 'أبريل', 'مايو', 'يونيو',
                'يوليو', 'أغسطس', 'سبتمبر', 'أكتوبر', 'نوفمبر', 'ديسمبر'
            ];

            // Print Header Setup
            document.getElementById('report-period-text').textContent = `شهر: ${monthNames[selectedMonth]} ${selectedYear}`;
            document.getElementById('print-report-date').textContent = `عن شهر: ${monthNames[selectedMonth]} ${selectedYear}`;
            document.getElementById('print-today-date').textContent = new Date().toLocaleDateString('ar-EG');

            let totalEarnedAll = 0;
            let totalAdvancesAll = 0;
            let totalDaysAll = 0;

            employees.forEach(emp => {
                // 1. Calculate Days Worked in Selected Month
                let daysWorked = 0;
                for (const [dateStr, record] of Object.entries(attendance)) {
                    const dateObj = new Date(dateStr);
                    if (dateObj.getMonth() === selectedMonth && dateObj.getFullYear() === selectedYear) {
                        if (record[emp.id] !== undefined) {
                            daysWorked += Number(record[emp.id]);
                        }
                    }
                }

                // 2. Calculate Advances in Selected Month
                let empAdvances = 0;
                advances.forEach(adv => {
                    const advDate = new Date(adv.date);
                    if (adv.empId === emp.id && advDate.getMonth() === selectedMonth && advDate.getFullYear() === selectedYear) {
                        empAdvances += Number(adv.amount);
                    }
                });

                // 3. Financial calculations
                const totalEarned = daysWorked * emp.rate;
                const netPayout = totalEarned - empAdvances;

                totalEarnedAll += totalEarned;
                totalAdvancesAll += empAdvances;
                totalDaysAll += daysWorked;

                // 4. Render Row
                const tr = document.createElement('tr');
                tr.className = 'hover:bg-slate-50 transition-colors';
                tr.innerHTML = `
                    <td class="py-4 px-4 font-bold text-slate-900">${escapeHtml(emp.name)}</td>
                    <td class="py-4 px-4 font-semibold text-slate-700">${Number(emp.rate).toFixed(2)} ج.م</td>
                    <td class="py-4 px-4 text-center font-bold text-slate-900">
                        <span class="bg-slate-100 text-slate-800 px-3 py-1 rounded-lg border border-slate-200">
                            ${daysWorked} يوم
                        </span>
                    </td>
                    <td class="py-4 px-4 font-bold text-slate-900">${totalEarned.toFixed(2)} ج.م</td>
                    <td class="py-4 px-4 font-bold text-rose-600">${empAdvances.toFixed(2)} ج.م</td>
                    <td class="py-4 px-4 font-bold ${netPayout < 0 ? 'text-rose-600' : 'text-emerald-700'}">
                        ${netPayout.toFixed(2)} ج.م
                    </td>
                `;
                tbody.appendChild(tr);
            });

            const totalNetPayout = totalEarnedAll - totalAdvancesAll;

            // Update Header Stat Cards
            document.getElementById('stat-total-emp').textContent = employees.length;
            document.getElementById('stat-total-earned').textContent = `${totalEarnedAll.toFixed(2)} ج.م`;
            document.getElementById('stat-total-advances').textContent = `${totalAdvancesAll.toFixed(2)} ج.م`;
            document.getElementById('stat-net-payout').textContent = `${totalNetPayout.toFixed(2)} ج.م`;

            // Update Footer Totals
            document.getElementById('foot-total-days').textContent = `${totalDaysAll} يوم`;
            document.getElementById('foot-total-earned').textContent = `${totalEarnedAll.toFixed(2)} ج.م`;
            document.getElementById('foot-total-advances').textContent = `${totalAdvancesAll.toFixed(2)} ج.م`;
            document.getElementById('foot-net-payout').textContent = `${totalNetPayout.toFixed(2)} ج.م`;
        }

        // Helper Utility Functions
        function escapeHtml(str) {
            return String(str)
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
