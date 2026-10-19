<!DOCTYPE html>
<html lang="vi" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hộp Quà May Mắn - Lucky Gift Box</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Montserrat:wght@400;600;700;800;900&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Montserrat', 'sans-serif'],
                        cursive: ['Dancing Script', 'cursive'],
                    },
                    colors: {
                        gold: {
                            300: '#fde047',
                            400: '#facc15',
                            500: '#eab308',
                            600: '#ca8a04',
                        }
                    },
                    animation: {
                        'float': 'float 4s ease-in-out infinite',
                        'pulse-glow': 'pulseGlow 2s infinite',
                        'bounce-gentle': 'bounceGentle 2s infinite',
                        'spin-slow': 'spin 14s linear infinite',
                        'shimmer': 'shimmer 2.5s infinite',
                    },
                    keyframes: {
                        float: {
                            '0%, 100%': { transform: 'translateY(0px)' },
                            '50%': { transform: 'translateY(-8px)' },
                        },
                        pulseGlow: {
                            '0%, 100%': { boxShadow: '0 0 15px rgba(234, 179, 8, 0.3)' },
                            '50%': { boxShadow: '0 0 30px rgba(234, 179, 8, 0.7), 0 0 45px rgba(244, 114, 182, 0.5)' },
                        },
                        bounceGentle: {
                            '0%, 100%': { transform: 'translateY(0) scale(1)' },
                            '50%': { transform: 'translateY(-6px) scale(1.05)' },
                        },
                        shimmer: {
                            '0%': { transform: 'translateX(-100%)' },
                            '100%': { transform: 'translateX(200%)' }
                        }
                    }
                }
            }
        }
    </script>

    <style>
        /* Modern Glassmorphism & Custom CSS */
        body {
            background: radial-gradient(circle at 50% 15%, #2d0b50 0%, #150030 55%, #070014 100%);
            min-height: 100vh;
            overflow-x: hidden;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        .glass-panel {
            background: rgba(255, 255, 255, 0.08);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.16);
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.5);
        }

        /* 3D Flip Card Container */
        .card-perspective {
            perspective: 1000px;
        }

        .card-inner {
            position: relative;
            width: 100%;
            height: 100%;
            transition: transform 0.6s cubic-bezier(0.4, 0, 0.2, 1);
            transform-style: preserve-3d;
        }

        .card-inner.is-flipped {
            transform: rotateY(180deg);
        }

        .card-front, .card-back {
            position: absolute;
            inset: 0;
            -webkit-backface-visibility: hidden;
            backface-visibility: hidden;
            border-radius: 1rem;
        }

        .card-front {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.14) 0%, rgba(255, 255, 255, 0.03) 100%);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
        }

        .card-front:hover {
            border-color: rgba(250, 204, 21, 0.6);
            box-shadow: 0 12px 30px rgba(234, 179, 8, 0.3), 0 0 20px rgba(244, 114, 182, 0.3);
        }

        .card-back {
            transform: rotateY(180deg);
            background: linear-gradient(135deg, rgba(45, 15, 80, 0.9) 0%, rgba(20, 5, 40, 0.95) 100%);
            backdrop-filter: blur(12px);
            border: 1.5px solid rgba(250, 204, 21, 0.5);
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
        }

        /* Gold Gradient Text */
        .text-gold-gradient {
            background: linear-gradient(135deg, #fff3a1 0%, #facc15 50%, #eab308 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        /* Ribbon Styling */
        .ribbon-v {
            position: absolute;
            top: 0;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
            width: 20%;
            background: linear-gradient(90deg, #f43f5e, #fb7185, #f43f5e);
            box-shadow: 0 0 8px rgba(244, 63, 94, 0.5);
        }

        .ribbon-h {
            position: absolute;
            left: 0;
            right: 0;
            top: 50%;
            transform: translateY(-50%);
            height: 20%;
            background: linear-gradient(180deg, #f43f5e, #fb7185, #f43f5e);
            box-shadow: 0 0 8px rgba(244, 63, 94, 0.5);
        }

        /* Slide Transition Container */
        .slides-wrapper {
            display: flex;
            transition: transform 0.5s cubic-bezier(0.25, 1, 0.5, 1);
            width: 300%;
        }

        .slide-page {
            width: 33.333333%;
            flex-shrink: 0;
        }

        /* Custom Scrollbar */
        .custom-scroll::-webkit-scrollbar {
            width: 6px;
        }
        .custom-scroll::-webkit-scrollbar-track {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 10px;
        }
        .custom-scroll::-webkit-scrollbar-thumb {
            background: rgba(250, 204, 21, 0.4);
            border-radius: 10px;
        }

        /* Modal Pop Animation */
        @keyframes modalPop {
            0% { transform: scale(0.7) translateY(20px); opacity: 0; }
            100% { transform: scale(1) translateY(0); opacity: 1; }
        }
        .animate-modal {
            animation: modalPop 0.35s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
        }

        /* Sunrays Effect for Grand Prize */
        .sunrays {
            background: conic-gradient(
                from 0deg,
                transparent 0deg 15deg,
                rgba(250, 204, 21, 0.2) 15deg 30deg,
                transparent 30deg 45deg,
                rgba(250, 204, 21, 0.2) 45deg 60deg,
                transparent 60deg 75deg,
                rgba(250, 204, 21, 0.2) 75deg 90deg,
                transparent 90deg 105deg,
                rgba(250, 204, 21, 0.2) 105deg 120deg,
                transparent 120deg 135deg,
                rgba(250, 204, 21, 0.2) 135deg 150deg,
                transparent 150deg 165deg,
                rgba(250, 204, 21, 0.2) 165deg 180deg,
                transparent 180deg 195deg,
                rgba(250, 204, 21, 0.2) 195deg 210deg,
                transparent 210deg 225deg,
                rgba(250, 204, 21, 0.2) 225deg 240deg,
                transparent 240deg 255deg,
                rgba(250, 204, 21, 0.2) 255deg 270deg,
                transparent 270deg 285deg,
                rgba(250, 204, 21, 0.2) 285deg 300deg,
                transparent 300deg 315deg,
                rgba(250, 204, 21, 0.2) 315deg 330deg,
                transparent 330deg 345deg,
                rgba(250, 204, 21, 0.2) 345deg 360deg
            );
        }

        /* Flying Hearts Container */
        #heartsContainer {
            position: fixed;
            inset: 0;
            pointer-events: none;
            z-index: 60;
            overflow: hidden;
        }

        .flying-heart {
            position: absolute;
            bottom: -50px;
            font-size: 1.8rem;
            animation: flyUp 2.8s cubic-bezier(0.25, 0.46, 0.45, 0.94) forwards;
            opacity: 0.9;
            filter: drop-shadow(0 0 6px rgba(255, 105, 180, 0.8));
        }

        @keyframes flyUp {
            0% {
                transform: translateY(0) scale(0.5) rotate(0deg);
                opacity: 0.9;
            }
            50% {
                opacity: 1;
            }
            100% {
                transform: translateY(-110vh) scale(1.4) rotate(360deg);
                opacity: 0;
            }
        }
    </style>
</head>
<body class="text-white font-sans antialiased flex flex-col justify-between min-h-screen relative selection:bg-pink-500 selection:text-white">

    <!-- Starfield & Particle Background Canvas -->
    <canvas id="starCanvas" class="fixed inset-0 pointer-events-none z-0"></canvas>

    <!-- Flying Hearts Effect Layer -->
    <div id="heartsContainer"></div>

    <!-- Header Section -->
    <header class="relative z-10 pt-4 pb-2 px-3 md:px-8 max-w-7xl mx-auto w-full">
        <div class="glass-panel rounded-2xl p-3.5 md:p-4 flex flex-col md:flex-row items-center justify-between gap-3 md:gap-4">
            
            <!-- Logo & Title -->
            <div class="flex items-center gap-3">
                <div class="w-11 h-11 md:w-12 md:h-12 rounded-xl bg-gradient-to-tr from-rose-500 via-purple-500 to-amber-400 p-[2px] shadow-lg animate-pulse-glow flex-shrink-0">
                    <div class="w-full h-full bg-slate-950 rounded-[10px] flex items-center justify-center">
                        <i class="fa-solid fa-gift text-xl md:text-2xl text-amber-300"></i>
                    </div>
                </div>
                <div>
                    <h1 class="text-xl md:text-2xl font-black text-gold-gradient tracking-wider uppercase">Hộp Quà May Mắn</h1>
                    <p class="text-[11px] md:text-xs text-purple-200 font-medium">3 Trang • 30 Ô Quà Ngẫu Nhiên ✨</p>
                </div>
            </div>

            <!-- Progress Tracker Bar & Actions -->
            <div class="flex items-center gap-2 md:gap-3 flex-wrap justify-center w-full md:w-auto">
                
                <!-- Progress Bar & Counter -->
                <div class="bg-purple-950/70 border border-purple-500/30 px-3 py-1.5 rounded-xl flex items-center gap-2 text-xs md:text-sm flex-grow md:flex-grow-0 min-w-[170px]">
                    <i class="fa-solid fa-box-open text-amber-400"></i>
                    <div class="flex-grow">
                        <div class="flex justify-between text-[11px] mb-1 font-semibold">
                            <span>Đã mở</span>
                            <span id="openedCounter" class="text-amber-300">0/30</span>
                        </div>
                        <div class="w-full h-2 bg-slate-800 rounded-full overflow-hidden border border-white/10">
                            <div id="progressBar" class="h-full bg-gradient-to-r from-amber-400 to-pink-500 w-0 transition-all duration-500 rounded-full"></div>
                        </div>
                    </div>
                </div>

                <!-- Bag / Inventory Button -->
                <button id="bagBtn" onclick="openInventory()" class="bg-gradient-to-r from-pink-600 to-purple-600 hover:from-pink-500 hover:to-purple-500 px-3 py-2 rounded-xl font-semibold text-xs md:text-sm transition-all shadow-lg hover:shadow-pink-500/25 active:scale-95 flex items-center gap-2">
                    <i class="fa-solid fa-bag-shopping"></i>
                    <span class="hidden sm:inline">Túi Quà</span>
                    <span id="bagBadge" class="bg-amber-400 text-slate-950 text-[10px] px-1.5 py-0.5 rounded-full font-black">0</span>
                </button>

                <!-- Sound Toggle -->
                <button id="soundBtn" onclick="toggleSound()" class="bg-slate-800/80 hover:bg-slate-700 border border-white/10 p-2 md:p-2.5 rounded-xl transition-all text-amber-300 hover:text-amber-200" title="Bật/Tắt Âm Thanh">
                    <i id="soundIcon" class="fa-solid fa-volume-high text-base md:text-lg"></i>
                </button>

                <!-- Reshuffle / Reset Button -->
                <button onclick="resetGamePrompt()" class="bg-slate-800/80 hover:bg-slate-700 border border-white/10 p-2 md:p-2.5 rounded-xl transition-all text-rose-300 hover:text-rose-200" title="Xáo Trộn & Chơi Lại">
                    <i class="fa-solid fa-rotate text-base md:text-lg"></i>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Workspace -->
    <main class="relative z-10 flex-grow flex flex-col justify-center max-w-7xl mx-auto w-full px-3 md:px-6 py-2">
        
        <!-- Slide Viewport Container with Touch Support -->
        <div id="slideViewport" class="overflow-hidden w-full rounded-3xl py-2 touch-pan-y">
            <div id="slidesWrapper" class="slides-wrapper">
                
                <!-- Slide 1 (10 Boxes) -->
                <div class="slide-page px-1.5">
                    <div id="gridSlide0" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-3 md:gap-4">
                        <!-- Box Cards 1-10 -->
                    </div>
                </div>

                <!-- Slide 2 (10 Boxes) -->
                <div class="slide-page px-1.5">
                    <div id="gridSlide1" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-3 md:gap-4">
                        <!-- Box Cards 11-20 -->
                    </div>
                </div>

                <!-- Slide 3 (10 Boxes) -->
                <div class="slide-page px-1.5">
                    <div id="gridSlide2" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-5 gap-3 md:gap-4">
                        <!-- Box Cards 21-30 -->
                    </div>
                </div>

            </div>
        </div>

        <!-- Navigation Controls & Page Dots -->
        <div class="mt-4 flex flex-col sm:flex-row items-center justify-between gap-3 glass-panel p-2.5 md:p-3 rounded-2xl max-w-xl mx-auto w-full">
            <button id="prevBtn" onclick="changeSlide(-1)" class="w-full sm:w-auto px-4 py-2 rounded-xl bg-purple-900/60 hover:bg-purple-800 border border-purple-500/30 text-amber-300 font-semibold text-xs md:text-sm transition-all disabled:opacity-30 disabled:cursor-not-allowed flex items-center justify-center gap-2">
                <i class="fa-solid fa-chevron-left"></i> Trang Trước
            </button>

            <!-- Page Indicators -->
            <div class="flex items-center gap-2.5 my-1 sm:my-0">
                <button onclick="goToSlide(0)" id="dot0" class="w-8 h-8 md:w-9 md:h-9 rounded-xl font-bold text-xs md:text-sm transition-all flex items-center justify-center border border-white/20">1</button>
                <button onclick="goToSlide(1)" id="dot1" class="w-8 h-8 md:w-9 md:h-9 rounded-xl font-bold text-xs md:text-sm transition-all flex items-center justify-center border border-white/20">2</button>
                <button onclick="goToSlide(2)" id="dot2" class="w-8 h-8 md:w-9 md:h-9 rounded-xl font-bold text-xs md:text-sm transition-all flex items-center justify-center border border-white/20">3</button>
            </div>

            <button id="nextBtn" onclick="changeSlide(1)" class="w-full sm:w-auto px-4 py-2 rounded-xl bg-purple-900/60 hover:bg-purple-800 border border-purple-500/30 text-amber-300 font-semibold text-xs md:text-sm transition-all disabled:opacity-30 disabled:cursor-not-allowed flex items-center justify-center gap-2">
                Trang Sau <i class="fa-solid fa-chevron-right"></i>
            </button>
        </div>

    </main>

    <!-- Footer -->
    <footer class="relative z-10 py-2.5 text-center text-[11px] md:text-xs text-purple-300/70">
        <p>💡 Dùng phím mũi tên <kbd class="px-1.5 py-0.5 bg-white/10 rounded text-amber-300">←</kbd> <kbd class="px-1.5 py-0.5 bg-white/10 rounded text-amber-300">→</kbd> hoặc vuốt màn hình để chuyển trang quà!</p>
    </footer>

    <!-- GIFT REVEAL MODAL -->
    <div id="giftModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-md hidden transition-opacity">
        <div id="modalCard" class="relative max-w-md w-full glass-panel rounded-3xl p-6 md:p-8 text-center border-2 border-amber-400/40 shadow-2xl overflow-hidden animate-modal">
            
            <!-- Sunrays Background for Special Gift -->
            <div id="sunraysBg" class="absolute inset-0 sunrays animate-spin-slow opacity-0 pointer-events-none transition-opacity duration-500"></div>

            <!-- Close Button -->
            <button onclick="closeGiftModal()" class="absolute top-4 right-4 z-20 text-purple-300 hover:text-white bg-white/10 w-9 h-9 rounded-full flex items-center justify-center transition-colors">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>

            <!-- Gift Category Badge -->
            <div class="relative z-10 mb-3">
                <span id="giftCategoryBadge" class="inline-block px-3.5 py-1 rounded-full text-xs font-black uppercase tracking-widest bg-pink-500/20 text-pink-300 border border-pink-500/30 mb-2">
                    Món Quà Ngọt Ngào
                </span>
                <h2 id="giftTitle" class="text-2xl md:text-3xl font-black text-gold-gradient">
                    Tên Món Quà
                </h2>
            </div>

            <!-- Gift Visual Icon Display -->
            <div class="relative z-10 my-5 py-2 flex items-center justify-center">
                <div id="giftIconContainer" class="w-32 h-32 md:w-36 md:h-36 rounded-3xl bg-gradient-to-tr from-purple-900/90 to-pink-600/80 border-2 border-amber-300/60 flex items-center justify-center text-6xl md:text-7xl shadow-2xl animate-bounce-gentle relative">
                    <span id="giftEmoji">🍭</span>
                    <div id="specialSparkle" class="absolute -top-3 -right-3 text-amber-300 text-3xl hidden animate-pulse">✨</div>
                </div>
            </div>

            <!-- Gift Description -->
            <div class="relative z-10 mb-6">
                <p id="giftDesc" class="text-xs md:text-sm text-purple-100 font-medium leading-relaxed bg-black/30 p-3.5 rounded-2xl border border-white/10">
                    Mô tả phần quà sẽ hiển thị ở đây.
                </p>
            </div>

            <!-- Action Button -->
            <div class="relative z-10">
                <button onclick="closeGiftModal()" class="w-full py-3 px-4 rounded-xl bg-gradient-to-r from-amber-400 via-yellow-400 to-amber-500 hover:from-amber-300 hover:to-yellow-300 text-slate-950 font-black text-sm shadow-lg shadow-amber-500/20 transition-all active:scale-95 flex items-center justify-center gap-2">
                    <span>Nhận Món Quà Này</span>
                    <i class="fa-solid fa-heart"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- INVENTORY / BAG MODAL -->
    <div id="inventoryModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-md hidden transition-opacity">
        <div class="relative max-w-lg w-full glass-panel rounded-3xl p-6 text-center border border-purple-500/30 shadow-2xl animate-modal max-h-[85vh] flex flex-col">
            
            <!-- Close Button -->
            <button onclick="closeInventoryModal()" class="absolute top-4 right-4 z-20 text-purple-300 hover:text-white bg-white/10 w-8 h-8 rounded-full flex items-center justify-center">
                <i class="fa-solid fa-xmark text-base"></i>
            </button>

            <!-- Header -->
            <div class="mb-4">
                <h2 class="text-2xl font-black text-gold-gradient flex items-center justify-center gap-2">
                    <i class="fa-solid fa-box-open text-amber-400"></i> Túi Quà Đã Thu Hoạch
                </h2>
                <p class="text-xs text-purple-200 mt-1">Danh sách các món quà bạn đã tìm thấy</p>
            </div>

            <!-- Inventory Items List -->
            <div id="inventoryList" class="flex-grow overflow-y-auto custom-scroll pr-1 my-2 space-y-2.5">
                <!-- Dynamically populated -->
            </div>

            <!-- Modal Footer -->
            <div class="mt-4 pt-3 border-t border-white/10 flex justify-end">
                <button onclick="closeInventoryModal()" class="w-full py-2.5 rounded-xl bg-purple-900/60 hover:bg-purple-800 text-purple-100 font-semibold text-xs md:text-sm border border-purple-500/30 transition-all">
                    Đóng Túi Quà
                </button>
            </div>
        </div>
    </div>

    <!-- GRAND FINISH / COMPLETION MODAL -->
    <div id="finishModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/85 backdrop-blur-md hidden transition-opacity">
        <div class="relative max-w-md w-full glass-panel rounded-3xl p-6 md:p-8 text-center border-2 border-amber-300/60 shadow-2xl animate-modal overflow-hidden">
            <div class="absolute inset-0 sunrays animate-spin-slow opacity-20 pointer-events-none"></div>

            <div class="relative z-10 w-20 h-20 bg-gradient-to-tr from-amber-400 to-yellow-300 rounded-3xl flex items-center justify-center mx-auto mb-4 text-4xl text-slate-950 shadow-xl shadow-amber-400/30 animate-bounce-gentle">
                👑
            </div>

            <h2 class="relative z-10 text-2xl md:text-3xl font-black text-gold-gradient mb-2">
                XUẤT SẮC! HOÀN THÀNH 30/30 QUÀ
            </h2>
            <p class="relative z-10 text-xs md:text-sm text-purple-100 mb-6 leading-relaxed">
                Bạn đã khám phá trọn vẹn tất cả 30 hộp quà may mắn! Chúc bạn luôn tràn ngập niềm vui, hạnh phúc và gặp thật nhiều may mắn mỗi ngày! ✨
            </p>

            <div class="relative z-10 flex flex-col gap-2.5">
                <button onclick="openInventory(); closeFinishModal();" class="w-full py-3 rounded-xl bg-purple-800/80 hover:bg-purple-700 text-amber-300 border border-purple-400/40 text-xs md:text-sm font-bold transition-all">
                    <i class="fa-solid fa-eye mr-1.5"></i> Xem Lại Tất Cả Phần Quà
                </button>
                <button onclick="closeFinishModal(); confirmResetGame();" class="w-full py-3 rounded-xl bg-gradient-to-r from-amber-400 via-yellow-400 to-amber-500 hover:from-amber-300 hover:to-yellow-300 text-slate-950 text-xs md:text-sm font-black shadow-lg shadow-amber-500/20 transition-all">
                    <i class="fa-solid fa-arrows-rotate mr-1.5"></i> Xáo Trộn & Chơi Lại Lần Nữa
                </button>
            </div>
        </div>
    </div>

    <!-- RESET CONFIRMATION MODAL -->
    <div id="resetModal" class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-slate-950/80 backdrop-blur-md hidden transition-opacity">
        <div class="relative max-w-sm w-full glass-panel rounded-3xl p-6 text-center border border-rose-500/40 shadow-2xl animate-modal">
            <div class="w-14 h-14 bg-rose-500/20 border border-rose-500/40 rounded-full flex items-center justify-center mx-auto mb-3 text-rose-400 text-2xl">
                <i class="fa-solid fa-arrows-rotate"></i>
            </div>
            <h3 class="text-lg font-bold text-white mb-1.5">Xáo trộn lại từ đầu?</h3>
            <p class="text-xs text-purple-200 mb-5">Tất cả 30 hộp quà sẽ được đóng lại và đặt vào các vị trí ngẫu nhiên mới.</p>

            <div class="flex gap-3">
                <button onclick="closeResetModal()" class="flex-1 py-2.5 rounded-xl bg-slate-800 hover:bg-slate-700 text-purple-200 text-xs font-semibold">
                    Hủy bỏ
                </button>
                <button onclick="confirmResetGame()" class="flex-1 py-2.5 rounded-xl bg-gradient-to-r from-rose-600 to-pink-600 hover:from-rose-500 hover:to-pink-500 text-white text-xs font-bold shadow-lg shadow-rose-600/30">
                    Xác nhận
                </button>
            </div>
        </div>
    </div>

    <!-- JAVASCRIPT LOGIC -->
    <script>
        /* ==========================================================================
           1. EXACT GIFT DATA DEFINITIONS (Sum = 30 Items)
           ========================================================================== */
        const GIFT_TYPES = {
            LOLLIPOP: {
                id: 'lollipop',
                name: 'Cây Kẹo Mút Vui Vẻ',
                emoji: '🍭',
                badge: 'Phổ biến',
                color: 'from-pink-500 to-rose-400',
                desc: 'Ngọt ngào, ngọt lịm mang lại niềm vui tươi mới và năng lượng tích cực cả ngày!',
                isSpecial: false,
                isLove: false,
                countTotal: 10
            },
            RING: {
                id: 'ring',
                name: 'Chiếc Nhẫn Ngọt Ngào',
                emoji: '💍',
                badge: 'Sang xịn',
                color: 'from-amber-400 to-yellow-300',
                desc: 'Thiết kế lấp lánh tinh tế, tượng trưng cho những tình cảm chân thành và bền chặt nhất!',
                isSpecial: false,
                isLove: true,
                countTotal: 5
            },
            CHOCOLATE: {
                id: 'chocolate',
                name: 'Bánh Socola Độc Lạ',
                emoji: '🍫',
                badge: 'Độc đáo',
                color: 'from-amber-700 to-yellow-600',
                desc: 'Hương vị đắng nhẹ hòa quyện ngọt ngào đầy cá tính, món quà không thể chối từ!',
                isSpecial: false,
                isLove: false,
                countTotal: 6
            },
            HAIRCLIP: {
                id: 'hairclip',
                name: 'Kẹp Tóc Hạnh Phúc',
                emoji: '🎀',
                badge: 'Dễ thương',
                color: 'from-fuchsia-500 to-pink-400',
                desc: 'Điểm xuyết vẻ đẹp thanh lịch, dịu dàng và mang lại sự tự tin rạng rỡ cho bạn!',
                isSpecial: false,
                isLove: true,
                countTotal: 4
            },
            CARD: {
                id: 'card',
                name: 'Thiệp Yêu Thương',
                emoji: '💌',
                badge: 'Ấm áp',
                color: 'from-indigo-500 to-purple-400',
                desc: 'Gửi gắm lời chúc an lành, vạn sự như ý, luôn mỉm cười và tràn ngập niềm vui!',
                isSpecial: false,
                isLove: true,
                countTotal: 4
            },
            SPECIAL: {
                id: 'special',
                name: 'PHẦN QUÀ ĐẶC BIỆT THƯỢNG HẠNG',
                emoji: '🎁✨',
                badge: '🏆 GIẢI ĐỘC ĐẮC 🏆',
                color: 'from-amber-300 via-yellow-400 to-amber-500',
                desc: '🎉 CHÚC MỪNG BẠN! Bạn đã trúng Món Quà Đặc Biệt Duy Nhất trong 30 hộp quà! Chúc bạn may mắn nhân đôi, đại cát đại lợi!',
                isSpecial: true,
                isLove: false,
                countTotal: 1
            }
        };

        /* ==========================================================================
           2. APP STATE MANAGEMENT
           ========================================================================== */
        let state = {
            boxes: [],           // Array of 30 box objects
            currentSlide: 0,     // 0, 1, 2
            soundEnabled: true,
            audioCtx: null
        };

        /* ==========================================================================
           3. WEB AUDIO SYNTHESIZER ENGINE (Clean & Error-free Context Unlock)
           ========================================================================== */
        function ensureAudioUnlocked() {
            if (!state.audioCtx) {
                const AudioCtx = window.AudioContext || window.webkitAudioContext;
                if (AudioCtx) {
                    state.audioCtx = new AudioCtx();
                }
            }
            if (state.audioCtx && state.audioCtx.state === 'suspended') {
                state.audioCtx.resume();
            }
        }

        function playSound(type) {
            if (!state.soundEnabled) return;
            ensureAudioUnlocked();
            if (!state.audioCtx) return;

            try {
                const ctx = state.audioCtx;
                const now = ctx.currentTime;

                if (type === 'click') {
                    const osc = ctx.createOscillator();
                    const gain = ctx.createGain();
                    osc.type = 'sine';
                    osc.frequency.setValueAtTime(450, now);
                    osc.frequency.exponentialRampToValueAtTime(850, now + 0.08);
                    gain.gain.setValueAtTime(0.12, now);
                    gain.gain.exponentialRampToValueAtTime(0.01, now + 0.08);
                    osc.connect(gain);
                    gain.connect(ctx.destination);
                    osc.start(now);
                    osc.stop(now + 0.08);
                } else if (type === 'slide') {
                    const osc = ctx.createOscillator();
                    const gain = ctx.createGain();
                    osc.type = 'triangle';
                    osc.frequency.setValueAtTime(320, now);
                    osc.frequency.exponentialRampToValueAtTime(550, now + 0.12);
                    gain.gain.setValueAtTime(0.08, now);
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.12);
                    osc.connect(gain);
                    gain.connect(ctx.destination);
                    osc.start(now);
                    osc.stop(now + 0.12);
                } else if (type === 'openNormal') {
                    // Bright Bell Chime
                    const notes = [523.25, 659.25, 783.99, 1046.50]; // C5, E5, G5, C6
                    notes.forEach((freq, idx) => {
                        const osc = ctx.createOscillator();
                        const gain = ctx.createGain();
                        osc.type = 'sine';
                        osc.frequency.setValueAtTime(freq, now + idx * 0.07);
                        gain.gain.setValueAtTime(0.18, now + idx * 0.07);
                        gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.07 + 0.35);
                        osc.connect(gain);
                        gain.connect(ctx.destination);
                        osc.start(now + idx * 0.07);
                        osc.stop(now + idx * 0.07 + 0.35);
                    });
                } else if (type === 'openHeart') {
                    // Sweet Loving Harmonies
                    const notes = [587.33, 739.99, 880.00, 1174.66]; // D5, F#5, A5, D6
                    notes.forEach((freq, idx) => {
                        const osc = ctx.createOscillator();
                        const gain = ctx.createGain();
                        osc.type = 'sine';
                        osc.frequency.setValueAtTime(freq, now + idx * 0.08);
                        gain.gain.setValueAtTime(0.2, now + idx * 0.08);
                        gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.08 + 0.45);
                        osc.connect(gain);
                        gain.connect(ctx.destination);
                        osc.start(now + idx * 0.08);
                        osc.stop(now + idx * 0.08 + 0.45);
                    });
                } else if (type === 'openSpecial') {
                    // Grand Fanfare
                    const notes = [523.25, 659.25, 783.99, 1046.50, 1318.51, 1567.98];
                    notes.forEach((freq, idx) => {
                        const osc = ctx.createOscillator();
                        const gain = ctx.createGain();
                        osc.type = 'triangle';
                        osc.frequency.setValueAtTime(freq, now + idx * 0.09);
                        gain.gain.setValueAtTime(0.25, now + idx * 0.09);
                        gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.09 + 0.7);
                        osc.connect(gain);
                        gain.connect(ctx.destination);
                        osc.start(now + idx * 0.09);
                        osc.stop(now + idx * 0.09 + 0.7);
                    });
                } else if (type === 'finish') {
                    // Victory Celebration Anthem
                    const notes = [523.25, 659.25, 783.99, 1046.50, 1318.51, 1567.98, 2093.00];
                    notes.forEach((freq, idx) => {
                        const osc = ctx.createOscillator();
                        const gain = ctx.createGain();
                        osc.type = 'sine';
                        osc.frequency.setValueAtTime(freq, now + idx * 0.1);
                        gain.gain.setValueAtTime(0.2, now + idx * 0.1);
                        gain.gain.exponentialRampToValueAtTime(0.001, now + idx * 0.1 + 0.8);
                        osc.connect(gain);
                        gain.connect(ctx.destination);
                        osc.start(now + idx * 0.1);
                        osc.stop(now + idx * 0.1 + 0.8);
                    });
                }
            } catch (e) {
                console.warn('Audio playback handled safely:', e);
            }
        }

        function toggleSound() {
            state.soundEnabled = !state.soundEnabled;
            const soundIcon = document.getElementById('soundIcon');
            if (state.soundEnabled) {
                soundIcon.className = 'fa-solid fa-volume-high text-base md:text-lg text-amber-300';
                playSound('click');
            } else {
                soundIcon.className = 'fa-solid fa-volume-xmark text-base md:text-lg text-slate-400';
            }
        }

        /* ==========================================================================
           4. GAME SETUP & SHUFFLE
           ========================================================================== */
        function createInitialGiftsList() {
            const list = [];
            Object.values(GIFT_TYPES).forEach(gift => {
                for (let i = 0; i < gift.countTotal; i++) {
                    list.push(gift.id);
                }
            });
            return list;
        }

        // Fisher-Yates Shuffle Algorithm
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        function initGame(shuffle = true) {
            const giftList = createInitialGiftsList();
            if (shuffle) {
                shuffleArray(giftList);
            }

            state.boxes = giftList.map((giftId, index) => ({
                id: index,
                giftId: giftId,
                isOpened: false
            }));

            renderAllSlides();
            updateHeaderStats();
            goToSlide(0, false);
        }

        /* ==========================================================================
           5. RENDER FUNCTIONS
           ========================================================================== */
        function renderAllSlides() {
            for (let slideIdx = 0; slideIdx < 3; slideIdx++) {
                const gridContainer = document.getElementById(`gridSlide${slideIdx}`);
                gridContainer.innerHTML = '';

                // Each slide renders exactly 10 boxes
                const startIdx = slideIdx * 10;
                const endIdx = startIdx + 10;
                const slideBoxes = state.boxes.slice(startIdx, endIdx);

                slideBoxes.forEach(box => {
                    const giftInfo = GIFT_TYPES[box.giftId.toUpperCase()];
                    const boxElement = document.createElement('div');
                    boxElement.className = 'card-perspective h-36 sm:h-40 md:h-44 w-full cursor-pointer group';
                    boxElement.onclick = () => handleBoxClick(box.id);

                    const isFlippedClass = box.isOpened ? 'is-flipped' : '';

                    boxElement.innerHTML = `
                        <div class="card-inner ${isFlippedClass}">
                            <!-- FRONT OF CARD (Unopened Gift Box) -->
                            <div class="card-front p-3 flex flex-col items-center justify-center text-center overflow-hidden">
                                <div class="ribbon-v opacity-90"></div>
                                <div class="ribbon-h opacity-90"></div>

                                <!-- Center Ribbon Bow Node -->
                                <div class="relative z-10 w-11 h-11 md:w-12 md:h-12 rounded-full bg-gradient-to-tr from-rose-600 via-pink-500 to-amber-300 p-0.5 shadow-xl flex items-center justify-center group-hover:scale-110 group-hover:rotate-12 transition-all duration-300">
                                    <div class="w-full h-full bg-slate-950 rounded-full flex items-center justify-center border border-white/20">
                                        <i class="fa-solid fa-gift text-lg md:text-xl text-amber-300"></i>
                                    </div>
                                </div>

                                <!-- Box Number Tag -->
                                <div class="relative z-10 mt-2.5 px-2.5 py-0.5 rounded-full bg-slate-950/80 border border-amber-400/40 text-[10px] md:text-[11px] font-black text-amber-300 shadow">
                                    Hộp #${box.id + 1}
                                </div>
                            </div>

                            <!-- BACK OF CARD (Opened Reward Item) -->
                            <div class="card-back p-3 flex flex-col items-center justify-center text-center overflow-hidden">
                                <span class="text-[9px] md:text-[10px] px-2 py-0.5 rounded-full bg-emerald-500/20 text-emerald-300 font-bold mb-1 border border-emerald-500/30">
                                    <i class="fa-solid fa-check mr-0.5"></i> Đã mở
                                </span>
                                <div class="text-3xl md:text-4xl my-1 animate-bounce-gentle">${giftInfo.emoji}</div>
                                <h4 class="text-[11px] md:text-xs font-bold text-amber-200 line-clamp-1">${giftInfo.name}</h4>
                                <span class="text-[9px] text-purple-300/80 mt-0.5">#${box.id + 1}</span>
                            </div>
                        </div>
                    `;

                    gridContainer.appendChild(boxElement);
                });
            }
        }

        function updateHeaderStats() {
            const openedCount = state.boxes.filter(b => b.isOpened).length;
            document.getElementById('openedCounter').innerText = `${openedCount}/30`;
            document.getElementById('bagBadge').innerText = openedCount;

            const progressPct = (openedCount / 30) * 100;
            document.getElementById('progressBar').style.width = `${progressPct}%`;
        }

        /* ==========================================================================
           6. SLIDE NAVIGATION & INTERACTORS
           ========================================================================== */
        function goToSlide(slideIdx, playSnd = true) {
            if (slideIdx < 0) slideIdx = 0;
            if (slideIdx > 2) slideIdx = 2;

            if (playSnd && state.currentSlide !== slideIdx) {
                playSound('slide');
            }

            state.currentSlide = slideIdx;

            // Slide Wrapper Offset
            const wrapper = document.getElementById('slidesWrapper');
            wrapper.style.transform = `translateX(-${slideIdx * 33.333333}%)`;

            // Nav Buttons
            document.getElementById('prevBtn').disabled = (slideIdx === 0);
            document.getElementById('nextBtn').disabled = (slideIdx === 2);

            // Pagination Dots
            [0, 1, 2].forEach(i => {
                const dot = document.getElementById(`dot${i}`);
                if (i === slideIdx) {
                    dot.className = 'w-8 h-8 md:w-9 md:h-9 rounded-xl font-bold text-xs md:text-sm transition-all flex items-center justify-center bg-gradient-to-r from-amber-400 to-yellow-500 text-slate-950 shadow-lg shadow-amber-400/30 scale-110';
                } else {
                    dot.className = 'w-8 h-8 md:w-9 md:h-9 rounded-xl font-bold text-xs md:text-sm transition-all flex items-center justify-center bg-purple-950/60 hover:bg-purple-800 text-purple-200 border border-purple-500/30';
                }
            });
        }

        function changeSlide(direction) {
            goToSlide(state.currentSlide + direction);
        }

        // Keyboard Navigation (Arrow Keys)
        window.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowLeft') {
                changeSlide(-1);
            } else if (e.key === 'ArrowRight') {
                changeSlide(1);
            }
        });

        // Touch Swipe Navigation
        let touchStartX = 0;
        let touchEndX = 0;
        const slideViewport = document.getElementById('slideViewport');

        slideViewport.addEventListener('touchstart', (e) => {
            touchStartX = e.changedTouches[0].screenX;
        }, { passive: true });

        slideViewport.addEventListener('touchend', (e) => {
            touchEndX = e.changedTouches[0].screenX;
            handleSwipe();
        }, { passive: true });

        function handleSwipe() {
            const threshold = 40; // minimum distance
            if (touchEndX < touchStartX - threshold) {
                changeSlide(1); // Swipe Left -> Next
            } else if (touchEndX > touchStartX + threshold) {
                changeSlide(-1); // Swipe Right -> Prev
            }
        }

        /* ==========================================================================
           7. GIFT REVEAL & SPECIAL EFFECTS LOGIC
           ========================================================================== */
        function handleBoxClick(boxId) {
            ensureAudioUnlocked();
            const box = state.boxes.find(b => b.id === boxId);
            if (!box) return;

            const giftInfo = GIFT_TYPES[box.giftId.toUpperCase()];

            const isFirstTimeOpen = !box.isOpened;
            if (isFirstTimeOpen) {
                box.isOpened = true;
                renderAllSlides();
                updateHeaderStats();
            }

            // Particle Effects & Audio Trigger
            if (giftInfo.isSpecial) {
                playSound('openSpecial');
                triggerSpecialFireworks();
            } else if (giftInfo.isLove) {
                playSound('openHeart');
                triggerFlyingHearts();
                triggerNormalConfetti();
            } else {
                playSound('openNormal');
                triggerNormalConfetti();
            }

            // Show Reveal Modal
            showGiftModal(giftInfo);

            // Check if user completed all 30 gifts
            if (isFirstTimeOpen) {
                const totalOpened = state.boxes.filter(b => b.isOpened).length;
                if (totalOpened === 30) {
                    setTimeout(() => {
                        playSound('finish');
                        showFinishModal();
                    }, 1200);
                }
            }
        }

        /* ==========================================================================
           8. ANIMATED PARTICLES & FIREWORKS ENGINE
           ========================================================================== */
        function triggerNormalConfetti() {
            if (typeof confetti !== 'function') return;

            confetti({
                particleCount: 45,
                spread: 60,
                origin: { y: 0.6 },
                colors: ['#facc15', '#f43f5e', '#a855f7', '#38bdf8']
            });
        }

        function triggerFlyingHearts() {
            const container = document.getElementById('heartsContainer');
            const heartEmojis = ['💖', '💗', '💕', '❤️', '🎀', '✨'];

            for (let i = 0; i < 16; i++) {
                setTimeout(() => {
                    const heart = document.createElement('div');
                    heart.className = 'flying-heart';
                    heart.innerText = heartEmojis[Math.floor(Math.random() * heartEmojis.length)];
                    heart.style.left = `${Math.random() * 90 + 5}%`;
                    heart.style.animationDuration = `${2.2 + Math.random() * 1.2}s`;

                    container.appendChild(heart);

                    setTimeout(() => {
                        heart.remove();
                    }, 3500);
                }, i * 90);
            }
        }

        function triggerSpecialFireworks() {
            if (typeof confetti !== 'function') return;

            const duration = 3.5 * 1000;
            const animationEnd = Date.now() + duration;

            const interval = setInterval(function() {
                const timeLeft = animationEnd - Date.now();

                if (timeLeft <= 0) {
                    return clearInterval(interval);
                }

                const particleCount = 60 * (timeLeft / duration);

                // Gold & Diamond Burst
                confetti({
                    particleCount,
                    startVelocity: 35,
                    spread: 360,
                    ticks: 70,
                    origin: { x: Math.random(), y: Math.random() - 0.2 },
                    colors: ['#facc15', '#eab308', '#ffffff', '#ff4d6d', '#ec4899', '#a855f7']
                });
            }, 200);
        }

        /* ==========================================================================
           9. MODALS CONTROL
           ========================================================================== */
        function showGiftModal(gift) {
            const modal = document.getElementById('giftModal');
            const sunrays = document.getElementById('sunraysBg');
            const sparkle = document.getElementById('specialSparkle');
            const giftBadge = document.getElementById('giftCategoryBadge');

            document.getElementById('giftTitle').innerText = gift.name;
            document.getElementById('giftEmoji').innerText = gift.emoji;
            document.getElementById('giftDesc').innerText = gift.desc;
            giftBadge.innerText = gift.badge;

            if (gift.isSpecial) {
                sunrays.classList.remove('opacity-0');
                sparkle.classList.remove('hidden');
                giftBadge.className = 'inline-block px-3.5 py-1 rounded-full text-xs font-black uppercase tracking-widest bg-amber-400 text-slate-950 shadow-lg animate-pulse mb-2';
            } else {
                sunrays.classList.add('opacity-0');
                sparkle.classList.add('hidden');
                giftBadge.className = 'inline-block px-3.5 py-1 rounded-full text-xs font-black uppercase tracking-widest bg-pink-500/20 text-pink-300 border border-pink-500/30 mb-2';
            }

            modal.classList.remove('hidden');
        }

        function closeGiftModal() {
            playSound('click');
            document.getElementById('giftModal').classList.add('hidden');
        }

        function openInventory() {
            playSound('click');
            const modal = document.getElementById('inventoryModal');
            const listContainer = document.getElementById('inventoryList');
            listContainer.innerHTML = '';

            const openedBoxes = state.boxes.filter(b => b.isOpened);

            if (openedBoxes.length === 0) {
                listContainer.innerHTML = `
                    <div class="py-10 text-center text-purple-300/60">
                        <i class="fa-solid fa-box-open text-4xl mb-3 block opacity-50"></i>
                        <p class="text-xs md:text-sm">Bạn chưa mở hộp quà nào.</p>
                        <p class="text-[11px] mt-1 text-purple-400">Hãy chạm vào các hộp quà trên màn hình để khám phá ngay nhé!</p>
                    </div>
                `;
            } else {
                // Group by gift ID
                const counts = {};
                openedBoxes.forEach(b => {
                    counts[b.giftId] = (counts[b.giftId] || 0) + 1;
                });

                Object.keys(counts).forEach(giftId => {
                    const gift = GIFT_TYPES[giftId.toUpperCase()];
                    const qty = counts[giftId];

                    const itemEl = document.createElement('div');
                    itemEl.className = 'glass-panel p-3 rounded-2xl flex items-center justify-between gap-3 text-left border border-white/10';
                    itemEl.innerHTML = `
                        <div class="flex items-center gap-3">
                            <div class="w-11 h-11 rounded-xl bg-purple-900/70 border border-purple-400/30 flex items-center justify-center text-2xl shadow flex-shrink-0">
                                ${gift.emoji}
                            </div>
                            <div>
                                <h4 class="text-xs md:text-sm font-bold ${gift.isSpecial ? 'text-amber-300' : 'text-white'}">${gift.name}</h4>
                                <span class="text-[10px] text-purple-300">${gift.badge}</span>
                            </div>
                        </div>
                        <div class="bg-amber-400/20 text-amber-300 border border-amber-400/40 px-2.5 py-0.5 rounded-full text-xs font-black">
                            x${qty}
                        </div>
                    `;
                    listContainer.appendChild(itemEl);
                });
            }

            modal.classList.remove('hidden');
        }

        function closeInventoryModal() {
            playSound('click');
            document.getElementById('inventoryModal').classList.add('hidden');
        }

        function showFinishModal() {
            document.getElementById('finishModal').classList.remove('hidden');
        }

        function closeFinishModal() {
            playSound('click');
            document.getElementById('finishModal').classList.add('hidden');
        }

        function resetGamePrompt() {
            playSound('click');
            document.getElementById('resetModal').classList.remove('hidden');
        }

        function closeResetModal() {
            playSound('click');
            document.getElementById('resetModal').classList.add('hidden');
        }

        function confirmResetGame() {
            closeResetModal();
            playSound('click');
            initGame(true);
        }

        /* ==========================================================================
           10. BACKGROUND CANVAS STARFIELD
           ========================================================================== */
        function initStarfield() {
            const canvas = document.getElementById('starCanvas');
            const ctx = canvas.getContext('2d');
            let stars = [];

            function resize() {
                canvas.width = window.innerWidth;
                canvas.height = window.innerHeight;
                createStars();
            }

            function createStars() {
                stars = [];
                const count = Math.floor((canvas.width * canvas.height) / 7500);
                for (let i = 0; i < count; i++) {
                    stars.push({
                        x: Math.random() * canvas.width,
                        y: Math.random() * canvas.height,
                        radius: Math.random() * 1.5 + 0.5,
                        alpha: Math.random(),
                        speed: Math.random() * 0.015 + 0.005
                    });
                }
            }

            function animate() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                stars.forEach(s => {
                    s.alpha += s.speed;
                    if (s.alpha > 1 || s.alpha < 0) s.speed = -s.speed;

                    ctx.beginPath();
                    ctx.arc(s.x, s.y, s.radius, 0, Math.PI * 2);
                    ctx.fillStyle = `rgba(255, 235, 180, ${Math.abs(s.alpha)})`;
                    ctx.fill();
                });
                requestAnimationFrame(animate);
            }

            window.addEventListener('resize', resize);
            resize();
            animate();
        }

        /* ==========================================================================
           11. APP ENTRY POINT
           ========================================================================== */
        window.onload = function() {
            initStarfield();
            initGame(true);

            // Audio Unlock Guard on First User Touch/Click/Keypress
            const unlockEvents = ['click', 'touchstart', 'keydown'];
            const unlockHandler = function() {
                ensureAudioUnlocked();
                unlockEvents.forEach(evt => document.body.removeEventListener(evt, unlockHandler));
            };
            unlockEvents.forEach(evt => document.body.addEventListener(evt, unlockHandler, { once: true }));
        };
    </script>
</body>
</html>
