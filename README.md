<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>랜덤 플레이 푸드 - Random Play Food</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Canvas Confetti -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts (Gaegu & Noto Sans KR) -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Gaegu:wght@400;700&family=Noto+Sans+KR:wght@400;500;700;900&display=swap" rel="stylesheet">

    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        'diner-bg': '#FFFBF0',
                        'diner-orange': '#FF7B54',
                        'diner-yellow': '#FFB26B',
                        'diner-cream': '#FFF4E0',
                        'diner-mint': '#4E9F3D',
                        'diner-brown': '#5C3D2E',
                        'diner-wood': '#8D5B4C',
                        'naver-green': '#03C75A',
                        'kakao-yellow': '#FEE500'
                    },
                    fontFamily: {
                        'cute': ['Gaegu', 'cursive'],
                        'sans': ['Noto Sans KR', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #FFFBF0;
            background-image: radial-gradient(#FFB26B 0.75px, transparent 0.75px), radial-gradient(#FFB26B 0.75px, #FFFBF0 0.75px);
            background-size: 30px 30px;
            background-position: 0 0, 15px 15px;
            font-family: 'Noto Sans KR', sans-serif;
            color: #5C3D2E;
        }

        .step-card {
            box-shadow: 0 10px 25px -5px rgba(141, 91, 76, 0.1), 0 8px 10px -6px rgba(141, 91, 76, 0.1);
            border: 3px solid #FFB26B;
            transition: all 0.3s ease;
        }

        .tag-btn {
            transition: all 0.2s ease;
            user-select: none;
        }

        .tag-btn.selected {
            background-color: #FF7B54 !important;
            color: white !important;
            border-color: #FF7B54 !important;
            transform: scale(1.03);
            box-shadow: 0 4px 12px rgba(255, 123, 84, 0.3);
        }

        /* custom scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #FFF4E0;
        }
        ::-webkit-scrollbar-thumb {
            background: #FFB26B;
            border-radius: 4px;
        }
    </style>
</head>
<body class="min-h-screen flex flex-col justify-between pb-10">

    <header class="pt-6 pb-4 text-center px-4">
        <div class="inline-block bg-white px-6 py-1.5 rounded-full border-4 border-diner-orange shadow-md transform -rotate-1 mb-2">
            <span class="text-diner-orange font-bold text-xs tracking-widest uppercase"><i class="fa-solid fa-utensils mr-1"></i> Random Play Food</span>
        </div>
        <h1 class="text-4xl md:text-5xl font-cute font-bold text-diner-brown drop-shadow-sm">
            🎲 랜덤 플레이 푸드 🍕
        </h1>
        <p class="text-sm text-amber-800 mt-1 font-medium">내 주변 맛집 추천까지 한번에! 포근한 취향 추천 식당</p>
    </header>

    <main class="max-w-3xl mx-auto px-4 w-full flex-grow">

        <div class="mb-6 bg-white/90 backdrop-blur rounded-2xl p-4 border-2 border-diner-yellow/50 shadow-sm">
            <div class="flex justify-between items-center relative">
                <div class="absolute top-1/2 left-6 right-6 h-1 bg-amber-100 -translate-y-1/2 -z-0"></div>
                <div id="progress-line" class="absolute top-1/2 left-6 h-1 bg-diner-orange -translate-y-1/2 -z-0 transition-all duration-300" style="width: 0%;"></div>

                <div id="step-ind-1" class="step-indicator relative z-10 flex flex-col items-center">
                    <div class="w-10 h-10 rounded-full bg-diner-orange text-white flex items-center justify-center font-bold shadow-md transition-all">1</div>
                    <span class="text-xs font-bold mt-1 text-diner-brown">맛 선택</span>
                </div>

                <div id="step-ind-2" class="step-indicator relative z-10 flex flex-col items-center">
                    <div class="w-10 h-10 rounded-full bg-amber-200 text-amber-800 flex items-center justify-center font-bold shadow-sm transition-all">2</div>
                    <span class="text-xs font-medium mt-1 text-amber-700">주메뉴</span>
                </div>

                <div id="step-ind-3" class="step-indicator relative z-10 flex flex-col items-center">
                    <div class="w-10 h-10 rounded-full bg-amber-200 text-amber-800 flex items-center justify-center font-bold shadow-sm transition-all">3</div>
                    <span class="text-xs font-medium mt-1 text-amber-700">음식 출처</span>
                </div>

                <div id="step-ind-4" class="step-indicator relative z-10 flex flex-col items-center">
                    <div class="w-10 h-10 rounded-full bg-amber-200 text-amber-800 flex items-center justify-center font-bold shadow-sm transition-all">4</div>
                    <span class="text-xs font-medium mt-1 text-amber-700">룰렛 추천</span>
                </div>
            </div>
        </div>

        <div class="bg-white rounded-3xl p-6 md:p-8 step-card relative overflow-hidden">
            
            <section id="step-1" class="step-content">
                <div class="text-center mb-6">
                    <span class="bg-amber-100 text-diner-orange text-xs font-bold px-3 py-1 rounded-full uppercase">Step 1</span>
                    <h2 class="text-2xl font-bold mt-2 text-diner-brown">지금 어떤 맛이 끌리시나요?</h2>
                    <p class="text-sm text-gray-500 mt-1">원하는 맛을 선택하거나 '상관없음'을 눌러주세요! (복수 선택 가능)</p>
                </div>

                <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 my-6">
                    <button type="button" onclick="toggleOption('flavor', '매운맛', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🌶️</span>
                        매운맛
                    </button>
                    <button type="button" onclick="toggleOption('flavor', '짠맛', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🧂</span>
                        짠맛 / 단짠
                    </button>
                    <button type="button" onclick="toggleOption('flavor', '단맛', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🍯</span>
                        단맛
                    </button>
                    <button type="button" onclick="toggleOption('flavor', '담백한 맛', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🥗</span>
                        담백한 맛
                    </button>
                    <button type="button" onclick="toggleOption('flavor', '기름진 맛', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🥓</span>
                        기름진 / 고소한 맛
                    </button>
                    <button type="button" onclick="toggleOption('flavor', '상큼한 맛', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🍋</span>
                        상큼 / 새콤한 맛
                    </button>
                </div>

                <div class="flex justify-center mb-6">
                    <button type="button" onclick="selectAny('flavor', this)" class="tag-btn w-full sm:w-2/3 border-2 border-dashed border-diner-orange bg-orange-50 rounded-2xl p-3 text-center font-bold text-diner-orange hover:bg-orange-100">
                        ✨ 상관없음 (아무 맛이나 좋아!)
                    </button>
                </div>

                <div class="flex justify-end items-center mt-8 pt-4 border-t border-amber-100">
                    <button onclick="nextStep(2)" class="bg-diner-orange hover:bg-orange-600 text-white font-bold px-6 py-3 rounded-2xl shadow-md transition-transform active:scale-95 flex items-center gap-2">
                        다음 단계 <i class="fa-solid fa-arrow-right"></i>
                    </button>
                </div>
            </section>

            <section id="step-2" class="step-content hidden">
                <div class="text-center mb-6">
                    <span class="bg-amber-100 text-diner-orange text-xs font-bold px-3 py-1 rounded-full uppercase">Step 2</span>
                    <h2 class="text-2xl font-bold mt-2 text-diner-brown">주메뉴 종류를 선택하세요!</h2>
                    <p class="text-sm text-gray-500 mt-1">오늘 탄수화물, 고기, 국물 중 어떤 주재료가 생각나나요?</p>
                </div>

                <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 my-6">
                    <button type="button" onclick="toggleOption('mainType', '밥', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🍚</span>
                        밥 종류
                    </button>
                    <button type="button" onclick="toggleOption('mainType', '면', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🍜</span>
                        면 종류
                    </button>
                    <button type="button" onclick="toggleOption('mainType', '빵', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🥪</span>
                        빵 / 샌드위치
                    </button>
                    <button type="button" onclick="toggleOption('mainType', '고기', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🥩</span>
                        고기류
                    </button>
                    <button type="button" onclick="toggleOption('mainType', '해산물', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🦐</span>
                        해산물
                    </button>
                    <button type="button" onclick="toggleOption('mainType', '국물/찌개', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🍲</span>
                        국물 / 찌개
                    </button>
                </div>

                <div class="flex justify-center mb-6">
                    <button type="button" onclick="selectAny('mainType', this)" class="tag-btn w-full sm:w-2/3 border-2 border-dashed border-diner-orange bg-orange-50 rounded-2xl p-3 text-center font-bold text-diner-orange hover:bg-orange-100">
                        ✨ 상관없음 (다 잘 먹어요!)
                    </button>
                </div>

                <div class="flex justify-between items-center mt-8 pt-4 border-t border-amber-100">
                    <button onclick="prevStep(1)" class="bg-amber-100 hover:bg-amber-200 text-diner-brown font-bold px-5 py-3 rounded-2xl transition-all">
                        <i class="fa-solid fa-arrow-left mr-1"></i> 이전
                    </button>
                    <button onclick="nextStep(3)" class="bg-diner-orange hover:bg-orange-600 text-white font-bold px-6 py-3 rounded-2xl shadow-md transition-transform active:scale-95 flex items-center gap-2">
                        다음 단계 <i class="fa-solid fa-arrow-right"></i>
                    </button>
                </div>
            </section>

            <section id="step-3" class="step-content hidden">
                <div class="text-center mb-6">
                    <span class="bg-amber-100 text-diner-orange text-xs font-bold px-3 py-1 rounded-full uppercase">Step 3</span>
                    <h2 class="text-2xl font-bold mt-2 text-diner-brown">음식의 출처(카테고리)를 선택하세요!</h2>
                    <p class="text-sm text-gray-500 mt-1">세계 각국의 풍성한 맛 중 어떤 스타일을 선호하시나요?</p>
                </div>

                <div class="grid grid-cols-2 sm:grid-cols-3 gap-3 my-6">
                    <button type="button" onclick="toggleOption('cuisine', '한식', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🇰🇷</span>
                        한식
                    </button>
                    <button type="button" onclick="toggleOption('cuisine', '중식', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🇨🇳</span>
                        중식
                    </button>
                    <button type="button" onclick="toggleOption('cuisine', '일식', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🇯🇵</span>
                        일식
                    </button>
                    <button type="button" onclick="toggleOption('cuisine', '양식', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🍕</span>
                        양식
                    </button>
                    <button type="button" onclick="toggleOption('cuisine', '분식/아시안', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🍢</span>
                        분식 / 아시안
                    </button>
                    <button type="button" onclick="toggleOption('cuisine', '패스트푸드', this)" class="tag-btn border-2 border-amber-200 bg-amber-50 rounded-2xl p-4 text-center font-bold text-diner-brown hover:bg-amber-100">
                        <span class="text-2xl block mb-1">🍔</span>
                        패스트푸드
                    </button>
                </div>

                <div class="flex justify-center mb-6">
                    <button type="button" onclick="selectAny('cuisine', this)" class="tag-btn w-full sm:w-2/3 border-2 border-dashed border-diner-orange bg-orange-50 rounded-2xl p-3 text-center font-bold text-diner-orange hover:bg-orange-100">
                        ✨ 상관없음 (어떤 나라든 환영!)
                    </button>
                </div>

                <div class="flex justify-between items-center mt-8 pt-4 border-t border-amber-100">
                    <button onclick="prevStep(2)" class="bg-amber-100 hover:bg-amber-200 text-diner-brown font-bold px-5 py-3 rounded-2xl transition-all">
                        <i class="fa-solid fa-arrow-left mr-1"></i> 이전
                    </button>
                    <button onclick="startRouletteStep()" class="bg-diner-orange hover:bg-orange-600 text-white font-bold px-7 py-3 rounded-2xl shadow-lg transition-transform active:scale-95 flex items-center gap-2">
                        룰렛 준비 완료! <i class="fa-solid fa-check"></i>
                    </button>
                </div>
            </section>

            <section id="step-4" class="step-content hidden">
                <div class="text-center mb-3">
                    <span class="bg-amber-100 text-diner-orange text-xs font-bold px-3 py-1 rounded-full uppercase">Step 4</span>
                    <h2 class="text-2xl font-bold mt-1 text-diner-brown">오늘의 행운 메뉴 룰렛!</h2>
                    <p class="text-xs text-gray-500 mt-0.5">선택하신 조건에 맞춰 준비된 메뉴 판입니다.</p>
                </div>

                <!-- 선택된 조건 요약 태그 -->
                <div id="matching-info" class="text-center mb-4 bg-amber-50 py-2 px-3 rounded-xl text-xs font-medium text-amber-800 border border-amber-200">
                    <!-- JS Dynamic insert -->
                </div>

                <!-- Interactive Wheel Container -->
                <div class="relative flex flex-col items-center justify-center my-2">
                    <div class="absolute -top-3 z-20 text-diner-orange text-3xl drop-shadow-md">
                        <i class="fa-solid fa-location-pin fa-rotate-180"></i>
                    </div>

                    <div class="relative p-2.5 bg-amber-100 rounded-full border-4 border-diner-yellow shadow-inner">
                        <canvas id="roulette-canvas" width="320" height="320" class="max-w-full h-auto rounded-full"></canvas>
                        <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-16 h-16 bg-white border-4 border-diner-orange rounded-full flex items-center justify-center shadow-lg pointer-events-none">
                            <span class="font-cute font-bold text-diner-orange text-lg">맛있어!</span>
                        </div>
                    </div>

                    <button id="spin-btn" onclick="spinRoulette()" class="mt-5 bg-diner-orange hover:bg-orange-600 text-white font-bold text-lg px-8 py-3.5 rounded-2xl shadow-xl transition-all transform active:scale-95 flex items-center gap-2">
                        <i class="fa-solid fa-rotate-right"></i> 룰렛 돌리기!
                    </button>
                </div>

                <!-- Final Result Dish & Nearby Restaurants Card -->
                <div id="result-card" class="hidden mt-6 bg-diner-cream border-2 border-diner-yellow rounded-3xl p-6 text-center transform transition-all duration-500 scale-95 opacity-0">
                    <div class="inline-block bg-diner-orange text-white text-xs font-bold px-3 py-1 rounded-full mb-2">
                        🎉 오늘의 추천 메뉴 결정!
                    </div>
                    <div id="result-emoji" class="text-6xl my-2">🍲</div>
                    <h3 id="result-title" class="text-3xl font-cute font-bold text-diner-brown mb-2">김치찌개</h3>
                    
                    <div id="result-tags" class="flex flex-wrap justify-center gap-1.5 mb-3">
                        <!-- dynamic tags -->
                    </div>

                    <p id="result-desc" class="text-gray-700 text-sm mb-4 bg-white/80 p-3 rounded-2xl border border-amber-200">
                        얼큰하고 깊은 맛! 언제 먹어도 호불호 없는 한국인의 영혼의 음식입니다.
                    </p>
                    
                    <div class="bg-white p-3 rounded-2xl border border-amber-200 text-left mb-4">
                        <div class="text-xs font-bold text-diner-wood mb-1"><i class="fa-solid fa-lightbulb text-amber-500 mr-1"></i> 추천 꿀조합 / 팁</div>
                        <p id="result-tip" class="text-xs text-gray-600">따끈한 계란말이나 김가루 밥과 함께 즐기면 풍미가 2배가 됩니다!</p>
                    </div>

                    <!-- NEW FEATURE: Nearby Restaurant Map Shortcuts -->
                    <div class="bg-white p-4 rounded-2xl border-2 border-diner-yellow/80 text-left mb-5 shadow-sm">
                        <div class="flex items-center justify-between mb-2">
                            <span class="text-sm font-bold text-diner-brown flex items-center gap-1.5">
                                <i class="fa-solid fa-map-location-dot text-diner-orange"></i> 내 주변 맛집 찾아보기
                            </span>
                            <button onclick="requestUserLocation()" class="text-xs bg-amber-100 hover:bg-amber-200 text-amber-900 font-bold px-2.5 py-1 rounded-lg transition-all flex items-center gap-1">
                                <i class="fa-solid fa-crosshairs text-diner-orange"></i> <span id="loc-btn-text">위치 갱신</span>
                            </button>
                        </div>

                        <p id="current-location-display" class="text-xs text-amber-800 bg-amber-50 p-2 rounded-xl border border-amber-200/60 mb-3 flex items-center gap-1.5">
                            <i class="fa-solid fa-street-view text-diner-orange"></i> <span>위치 정보 확인 중...</span>
                        </p>

                        <div class="grid grid-cols-1 sm:grid-cols-3 gap-2">
                            <a id="naver-map-btn" href="#" target="_blank" rel="noopener noreferrer" class="bg-[#03C75A] hover:bg-[#02b350] text-white font-bold text-xs py-2.5 px-3 rounded-xl flex items-center justify-center gap-1.5 shadow-sm transition-all active:scale-95">
                                <i class="fa-solid fa-square-n"></i> 네이버 지도 검색
                            </a>
                            <a id="kakao-map-btn" href="#" target="_blank" rel="noopener noreferrer" class="bg-[#FEE500] hover:bg-[#ebd300] text-[#3c1e1e] font-bold text-xs py-2.5 px-3 rounded-xl flex items-center justify-center gap-1.5 shadow-sm transition-all active:scale-95">
                                <i class="fa-solid fa-comment"></i> 카카오맵 검색
                            </a>
                            <a id="google-map-btn" href="#" target="_blank" rel="noopener noreferrer" class="bg-blue-500 hover:bg-blue-600 text-white font-bold text-xs py-2.5 px-3 rounded-xl flex items-center justify-center gap-1.5 shadow-sm transition-all active:scale-95">
                                <i class="fa-solid fa-g"></i> 구글 지도 검색
                            </a>
                        </div>
                    </div>

                    <!-- Action buttons & Share -->
                    <div class="flex flex-wrap gap-2 justify-center">
                        <button onclick="spinRoulette()" class="bg-amber-500 hover:bg-amber-600 text-white text-sm font-bold px-4 py-2.5 rounded-xl transition-all">
                            <i class="fa-solid fa-arrows-rotate mr-1"></i> 다시 돌리기
                        </button>
                        <button onclick="shareLink()" class="bg-diner-mint hover:bg-green-700 text-white text-sm font-bold px-4 py-2.5 rounded-xl transition-all">
                            <i class="fa-solid fa-share-nodes mr-1"></i> 결과 공유하기
                        </button>
                        <button onclick="resetAll()" class="bg-amber-200 hover:bg-amber-300 text-diner-brown text-sm font-bold px-4 py-2.5 rounded-xl transition-all">
                            <i class="fa-solid fa-filter-circle-xmark mr-1"></i> 처음부터 다시
                        </button>
                    </div>
                </div>

                <div class="flex justify-between items-center mt-6 pt-4 border-t border-amber-100">
                    <button onclick="prevStep(3)" class="bg-amber-100 hover:bg-amber-200 text-diner-brown font-bold px-4 py-2 rounded-xl transition-all text-xs">
                        <i class="fa-solid fa-arrow-left mr-1"></i> 조건 변경
                    </button>
                    <button onclick="pickRandomDirectly()" class="text-xs text-amber-800 underline hover:text-diner-orange font-medium">
                        조건 무시하고 아무거나 바로 뽑기
                    </button>
                </div>
            </section>
        </div>
    </main>

    <!-- Toast Notice -->
    <div id="toast" class="fixed bottom-6 left-1/2 -translate-x-1/2 bg-diner-brown text-white text-xs px-5 py-2.5 rounded-full shadow-2xl opacity-0 transition-opacity pointer-events-none z-50">
        링크가 클립보드에 복사되었습니다!
    </div>

    <!-- Footer -->
    <footer class="text-center py-4 text-xs text-amber-800/70">
        <p>오늘도 랜덤 플레이 푸드와 함께 맛있는 식사 되세요! 🍳</p>
    </footer>

    <script>
        // --- 50+ Detailed Food Database ---
        const FOOD_DATABASE = [
            // 한식
            { name: "김치찌개", emoji: "🍲", flavors: ["매운맛", "짠맛"], type: "국물/찌개", cuisine: "한식", desc: "얼큰하고 시원한 한국인의 영혼의 대표 소울푸드!", tip: "계란말이나 프라이를 곁들이면 환상조합입니다." },
            { name: "된장찌개", emoji: "🍲", flavors: ["짠맛", "담백한 맛"], type: "국물/찌개", cuisine: "한식", desc: "구수한 된장 국물에 두부와 차돌박이가 어우러진 요리.", tip: "밥에 쓱쓱 비벼 김치와 함께 드셔보세요." },
            { name: "제육볶음", emoji: "🥩", flavors: ["매운맛", "짠맛", "단맛"], type: "고기", cuisine: "한식", desc: "매콤달콤한 양념에 볶아낸 최고 인기 백반 메뉴.", tip: "신선한 상추 쌈과 마늘 한 조각을 올려 한입에!" },
            { name: "삼겹살 구이", emoji: "🥓", flavors: ["기름진 맛", "짠맛"], type: "고기", cuisine: "한식", desc: "지글지글 고소한 냄새부터 완벽한 고기 파티의 주역.", tip: "구운 김치와 파채 무침은 필수입니다." },
            { name: "비빔밥", emoji: "🥗", flavors: ["매운맛", "담백한 맛"], type: "밥", cuisine: "한식", desc: "각종 나물과 고추장이 만드는 오색빛깔 건강한 한 끼.", tip: "참기름 한 방울과 반숙 계란후라이를 올려주세요." },
            { name: "불고기덮밥", emoji: "🍚", flavors: ["단맛", "짠맛"], type: "밥", cuisine: "한식", desc: "단짠단짠 부드러운 소고기가 듬뿍 올라간 덮밥.", tip: "당면 사리가 있다면 더욱 풍성하게 즐길 수 있어요." },
            { name: "순두부찌개", emoji: "🌶️", flavors: ["매운맛", "국물/찌개"], type: "국물/찌개", cuisine: "한식", desc: "몽글몽글 부드러운 순두부와 얼큰한 해물 국물.", tip: "끓자마자 날계란 하나 톡 깨뜨려 넣어주세요." },
            { name: "갈비탕", emoji: "🥣", flavors: ["담백한 맛", "국물/찌개"], type: "국물/찌개", cuisine: "한식", desc: "푹 고아낸 진한 육수와 두툼한 갈빗살의 든든함.", tip: "깍두기와 잘 익은 겉절이를 꼭 곁들이세요." },
            { name: "닭갈비", emoji: "🍗", flavors: ["매운맛", "단맛"], type: "고기", cuisine: "한식", desc: "매콤한 양념 닭고기과 양배추, 떡 사리의 만남.", tip: "다 먹은 후 볶음밥과 치즈 사리는 국룰입니다!" },
            { name: "보쌈 / 족발", emoji: "🍖", flavors: ["기름진 맛", "담백한 맛"], type: "고기", cuisine: "한식", desc: "야들야들하게 삶아낸 고기와 보쌈김치의 조화.", tip: "막국수를 사이드로 함께 시키면 더욱 좋습니다." },
            { name: "부대찌개", emoji: "🥘", flavors: ["매운맛", "짠맛"], type: "국물/찌개", cuisine: "한식", desc: "햄과 소시지, 라면사리가 푸짐하게 들어간 찌개.", tip: "치즈 한 장을 올려 부드럽게 국물을 즐겨보세요." },
            { name: "냉면 (물/비빔)", emoji: "🍜", flavors: ["상큼한 맛", "매운맛"], type: "면", cuisine: "한식", desc: "살얼음 동동 띄운 육수나 매콤한 양념장의 시원함.", tip: "숯불고기와 함께 싸서 먹으면 제맛입니다." },
            { name: "칼국수 / 수제비", emoji: "🍜", flavors: ["담백한 맛", "국물/찌개"], type: "면", cuisine: "한식", desc: "쫄깃한 면발과 시원한 바지락 육수의 따뜻함.", tip: "매콤한 매운 김치와 조합이 아주 뛰어납니다." },

            // 일식
            { name: "초밥 (스시)", emoji: "🍣", flavors: ["담백한 맛", "상큼한 맛"], type: "해산물", cuisine: "일식", desc: "신선한 회와 새콤달콤 밥의 정갈한 조화.", tip: "와사비 간장에 살짝 찍어 깔끔하게 드세요." },
            { name: "돈카츠 (돈까스)", emoji: "🍱", flavors: ["기름진 맛", "단맛"], type: "고기", cuisine: "일식", desc: "겉은 바삭하고 속은 촉촉한 일식 돼지고기 튀김.", tip: "겨자를 곁들인 돈까스 소스나 와사비 소금을 살짝!" },
            { name: "돈부리 (가츠동)", emoji: "🍲", flavors: ["단맛", "짠맛"], type: "밥", cuisine: "일식", desc: "달콤짭조름한 타레 소스가 베어든 단란한 덮밥.", tip: "비비지 않고 밥과 고명을 함께 떠먹어야 더욱 맛납니다." },
            { name: "라멘 (돈코츠)", emoji: "🍜", flavors: ["기름진 맛", "짠맛"], type: "면", cuisine: "일식", desc: "진하게 우려낸 고기 육수와 차슈가 일품인 면 요리.", tip: "온센타마고(반숙란)를 추가해 보세요." },
            { name: "우동", emoji: "🍢", flavors: ["담백한 맛", "짠맛"], type: "면", cuisine: "일식", desc: "오통통하고 쫄깃한 면발과 깔끔한 가쓰오부시 국물.", tip: "바삭한 튀김을 올려 튀김우동으로 즐겨보세요." },
            { name: "카레라이스", emoji: "🍛", flavors: ["매운맛", "단맛"], type: "밥", cuisine: "일식", desc: "진하고 진득한 일본식 카레와 고소한 고명의 조합.", tip: "마늘 칩과 아삭한 대파를 듬뿍 얹으세요." },
            { name: "타코야끼", emoji: "🐙", flavors: ["단맛", "짠맛"], type: "해산물", cuisine: "일식", desc: "겉은 노릇 속은 촉촉한 문어 경단 간식/식사.", tip: "가쓰오부시와 마요네즈 소스를 듬뿍!" },

            // 중식
            { name: "짜장면", emoji: "🍜", flavors: ["단맛", "짠맛"], type: "면", cuisine: "중식", desc: "고소한 춘장에 볶아낸 국민 대표 배달 메뉴.", tip: "고춧가루를 솔솔 뿌려먹으면 느끼함이 가십니다." },
            { name: "짬뽕", emoji: "🌶️", flavors: ["매운맛", "국물/찌개"], type: "면", cuisine: "중식", desc: "해산물 불향이 가득 느껴지는 얼큰한 국물 면 요리.", tip: "단무지에 식초를 살짝 뿌려 곁들이세요." },
            { name: "탕수육", emoji: "🥩", flavors: ["단맛", "상큼한 맛"], type: "고기", cuisine: "중식", desc: "바삭한 바깥 옷과 새콤달콤한 과일 소스의 튀김.", tip: "부먹파 vs 찍먹파, 취향에 맞춰 즐겨보세요!" },
            { name: "마라탕 / 마라샹궈", emoji: "🔥", flavors: ["매운맛", "기름진 맛"], type: "국물/찌개", cuisine: "중식", desc: "얼얼한 알싸함과 옥수수면, 분모자의 매력적인 중독성.", tip: "땅콩 소스를 요청해 찍어 드시면 고소함이 늘어납니다." },
            { name: "볶음밥 (중화풍)", emoji: "🍳", flavors: ["기름진 맛", "짠맛"], type: "밥", cuisine: "중식", desc: "고슬고슬 불맛 나게 볶아낸 고소한 계란 볶음밥.", tip: "함께 제공되는 짜장 소스를 살짝 얹어 드세요." },
            { name: "꿔바로우", emoji: "🥩", flavors: ["상큼한 맛", "단맛"], type: "고기", cuisine: "중식", desc: "찹쌀 옷을 입혀 쫀득하고 큼직하게 튀겨낸 북경식 탕수육.", tip: "가위로 싹둑 잘라 뜨거울 때 드세요." },

            // 양식
            { name: "파스타", emoji: "🍝", flavors: ["기름진 맛", "상큼한 맛"], type: "면", cuisine: "양식", desc: "풍미 깊은 소스에 쫄깃한 면이 어우러진 양식의 꽃.", tip: "마늘빵으로 그릇의 소스를 싹싹 닦아 드세요." },
            { name: "피자", emoji: "🍕", flavors: ["기름진 맛", "짠맛"], type: "빵", cuisine: "양식", desc: "쭉쭉 늘어나는 치즈와 다채로운 토핑이 만드는 행복.", tip: "갈릭 디핑 소스를 피자 꼬투리에 찍어드세요." },
            { name: "스테이크", emoji: "🥩", flavors: ["기름진 맛", "담백한 맛"], type: "고기", cuisine: "양식", desc: "육즙이 팡팡 터지는 근사하고 풍부한 고기 요리.", tip: "매쉬드 포테이토나 구운 야채를 곁들이세요." },
            { name: "리조또", emoji: "🍚", flavors: ["기름진 맛", "담백한 맛"], type: "밥", cuisine: "양식", desc: "부드럽고 크리미하게 익혀낸 이탈리안 쌀 요리.", tip: "트러플 오일을 첨가하면 풍미가 극대화됩니다." },
            { name: "수제버거", emoji: "🍔", flavors: ["기름진 맛", "짠맛"], type: "빵", cuisine: "양식", desc: "두툼한 소고기 패티와 싱싱한 채소가 가득한 버거.", tip: "바삭한 감자튀김과 밀크셰이크 조합을 추천합니다." },
            { name: "샐러드 & 포케", emoji: "🥗", flavors: ["담백한 맛", "상큼한 맛"], type: "해산물", cuisine: "양식", desc: "신선한 야채와 연어, 닭가슴살이 선사하는 가벼운 건강식.", tip: "오리엔탈이나 발사믹 드레싱을 곁들여보세요." },

            // 분식 / 아시안
            { name: "떡볶이", emoji: "🍡", flavors: ["매운맛", "단맛"], type: "국물/찌개", cuisine: "분식/아시안", desc: "쫀득한 떡과 양념이 만들어내는 대표 국민 분식.", tip: "바삭한 모듬 튀김과 순대를 소스에 찍어 드세요!" },
            { name: "김밥", emoji: "🍙", flavors: ["담백한 맛", "짠맛"], type: "밥", cuisine: "분식/아시안", desc: "속재료가 알차게 들어간 언제 어디서나 맛있는 한 끼.", tip: "라면 국물에 김밥을 살짝 적셔 드세요." },
            { name: "쌀국수 (포)", emoji: "🍜", flavors: ["담백한 맛", "국물/찌개"], type: "면", cuisine: "분식/아시안", desc: "깊은 소고기 육수와 숙주의 아삭함이 일품인 베트남 면.", tip: "해선장 소스와 칠리 소스를 7:3 비율로 섞어 드세요." },
            { name: "팟타이", emoji: "🍤", flavors: ["단맛", "상큼한 맛"], type: "면", cuisine: "분식/아시안", desc: "새우와 계란, 땅콩 가루를 넣고 볶은 태국식 볶음면.", tip: "라임 즙을 싹 둘러 고소함과 상큼함을 업그레이드!" },
            { name: "나시고랭", emoji: "🍳", flavors: ["단맛", "짠맛"], type: "밥", cuisine: "분식/아시안", desc: "인도네시아 특제 소스에 해산물을 넣고 볶아낸 볶음밥.", tip: "알새우칩(크루푹)에 밥을 살짝 얹어 드세요." },
            { name: "순대 / 모듬튀김", emoji: "🥟", flavors: ["기름진 맛", "짠맛"], type: "고기", cuisine: "분식/아시안", desc: "고소하고 찰진 찰순대와 바삭한 튀김의 즐거움.", tip: "떡볶이 국물 소스에 찍어 먹는 것이 제맛!" },

            // 패스트푸드
            { name: "햄버거 세트", emoji: "🍔", flavors: ["기름진 맛", "짠맛"], type: "빵", cuisine: "패스트푸드", desc: "빠르고 언제나 맛있는 스피드 만점 햄버거 패키지.", tip: "감자튀김을 탄산음료나 아이스크림에 찍어 드세요." },
            { name: "치킨 (후라이드/양념)", emoji: "🍗", flavors: ["기름진 맛", "매운맛", "단맛"], type: "고기", cuisine: "패스트푸드", desc: "바삭한 튀김옷과 육즙 가득 치킨! 야식/식사 대표주자.", tip: "치킨무와 시원한 맥주/콜라를 반드시 준비하세요." },
            { name: "샌드위치", emoji: "🥪", flavors: ["담백한 맛", "상큼한 맛"], type: "빵", cuisine: "패스트푸드", desc: "갓 구운 빵에 신선한 야채와 햄이 꽉 찬 다이어트 식단.", tip: "랜치 소스와 스위트 칠리 조합을 적극 추천합니다." },
            { name: "핫도그", emoji: "🌭", flavors: ["단맛", "짠맛"], type: "빵", cuisine: "패스트푸드", desc: "바삭한 튀김 옷 속 뽀득뽀득 소시지가 숨어있는 간식.", tip: "설탕을 굴린 후 케첩과 머스타드를 듬뿍!" }
        ];

        // State variables
        let currentStep = 1;
        const userSelections = {
            flavors: [],
            mainTypes: [],
            cuisines: []
        };

        let userCoords = null; // { latitude, longitude }
        let userLocationName = "내 주변"; // Geocoded name or default

        let filteredList = [];
        let canvas, ctx;
        let startAngle = 0;
        let arc = 0;
        let spinTimeout = null;
        let spinAngleStart = 10;
        let spinTime = 0;
        let spinTimeTotal = 0;
        let selectedDish = null;

        const colors = [
            "#FF7B54", "#FFB26B", "#FFD93D", "#4E9F3D", 
            "#6BCB77", "#4D96FF", "#FF6B6B", "#9B51E0"
        ];

        window.onload = function() {
            canvas = document.getElementById("roulette-canvas");
            if (canvas) {
                ctx = canvas.getContext("2d");
            }
            // Auto request location silently on start
            requestUserLocation(true);
        };

        function toggleOption(category, value, btnElement) {
            let list = [];
            if (category === 'flavor') list = userSelections.flavors;
            if (category === 'mainType') list = userSelections.mainTypes;
            if (category === 'cuisine') list = userSelections.cuisines;

            const anyBtn = btnElement.parentElement.parentElement.querySelector('.border-dashed');
            if (anyBtn) anyBtn.classList.remove('selected');

            const index = list.indexOf(value);
            if (index > -1) {
                list.splice(index, 1);
                btnElement.classList.remove('selected');
            } else {
                list.push(value);
                btnElement.classList.add('selected');
            }
        }

        function selectAny(category, btnElement) {
            if (category === 'flavor') userSelections.flavors = [];
            if (category === 'mainType') userSelections.mainTypes = [];
            if (category === 'cuisine') userSelections.cuisines = [];

            const parent = btnElement.parentElement.parentElement;
            const tagBtns = parent.querySelectorAll('.tag-btn');
            tagBtns.forEach(b => b.classList.remove('selected'));

            btnElement.classList.add('selected');
        }

        // Navigation controls
        function nextStep(stepNum) {
            document.getElementById(`step-${currentStep}`).classList.add('hidden');
            document.getElementById(`step-${stepNum}`).classList.remove('hidden');
            
            currentStep = stepNum;
            updateStepIndicators();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function prevStep(stepNum) {
            document.getElementById(`step-${currentStep}`).classList.add('hidden');
            document.getElementById(`step-${stepNum}`).classList.remove('hidden');
            
            currentStep = stepNum;
            updateStepIndicators();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function updateStepIndicators() {
            const progressLine = document.getElementById('progress-line');
            const percent = ((currentStep - 1) / 3) * 100;
            progressLine.style.width = `${percent}%`;

            for (let i = 1; i <= 4; i++) {
                const ind = document.getElementById(`step-ind-${i}`);
                const circle = ind.querySelector('div');
                const label = ind.querySelector('span');

                if (i <= currentStep) {
                    circle.className = "w-10 h-10 rounded-full bg-diner-orange text-white flex items-center justify-center font-bold shadow-md transition-all";
                    label.className = "text-xs font-bold mt-1 text-diner-brown";
                } else {
                    circle.className = "w-10 h-10 rounded-full bg-amber-200 text-amber-800 flex items-center justify-center font-bold shadow-sm transition-all";
                    label.className = "text-xs font-medium mt-1 text-amber-700";
                }
            }
        }

        function requestUserLocation(silent = false) {
            const displayEl = document.getElementById('current-location-display');
            const btnText = document.getElementById('loc-btn-text');
            
            if (btnText) btnText.innerText = "조회 중...";

            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(
                    (position) => {
                        userCoords = {
                            latitude: position.coords.latitude,
                            longitude: position.coords.longitude
                        };
                        userLocationName = "내 위치 주변";
                        
                        if (displayEl) {
                            displayEl.innerHTML = `<i class="fa-solid fa-location-dot text-diner-orange"></i> <span>현재 GPS 위치 정보가 연동되었습니다! (근처 맛집)</span>`;
                        }
                        if (btnText) btnText.innerText = "위치 갱신";
                        
                        if (selectedDish) updateMapLinks(selectedDish.name);
                    },
                    (error) => {
                        if (!silent) showToast("위치 권한을 허용하시면 더 정확한 주변 맛집을 찾을 수 있습니다.");
                        userLocationName = "내 주변";
                        if (displayEl) {
                            displayEl.innerHTML = `<i class="fa-solid fa-street-view text-amber-600"></i> <span>일반 '내 주변' 지역을 기반으로 지도에서 찾습니다.</span>`;
                        }
                        if (btnText) btnText.innerText = "위치 갱신";
                        if (selectedDish) updateMapLinks(selectedDish.name);
                    },
                    { timeout: 8000 }
                );
            } else {
                if (!silent) showToast("이 브라우저는 위치 서비스를 지원하지 않습니다.");
                userLocationName = "내 주변";
                if (selectedDish) updateMapLinks(selectedDish.name);
            }
        }

        function updateMapLinks(dishName) {
            const query = `${userLocationName} ${dishName} 맛집`;
            const encodedQuery = encodeURIComponent(query);
            const encodedDishOnly = encodeURIComponent(`${dishName} 맛집`);

            // Naver Map Search URL
            const naverMapUrl = `https://map.naver.com/v5/search/${encodedQuery}`;
            // Kakao Map Search URL
            const kakaoMapUrl = `https://map.kakao.com/link/search/${encodedQuery}`;
            // Google Map Search URL
            let googleMapUrl = `https://www.google.com/maps/search/${encodedQuery}`;
            if (userCoords) {
                googleMapUrl = `https://www.google.com/maps/search/${encodedDishOnly}/@${userCoords.latitude},${userCoords.longitude},15z`;
            }

            document.getElementById('naver-map-btn').href = naverMapUrl;
            document.getElementById('kakao-map-btn').href = kakaoMapUrl;
            document.getElementById('google-map-btn').href = googleMapUrl;
        }

        function filterFoodDatabase() {
            return FOOD_DATABASE.filter(item => {
                const flavorMatch = userSelections.flavors.length === 0 || 
                    userSelections.flavors.some(f => item.flavors.includes(f));
                
                const typeMatch = userSelections.mainTypes.length === 0 || 
                    userSelections.mainTypes.includes(item.type);

                const cuisineMatch = userSelections.cuisines.length === 0 || 
                    userSelections.cuisines.includes(item.cuisine);

                return flavorMatch && typeMatch && cuisineMatch;
            });
        }

        function startRouletteStep() {
            filteredList = filterFoodDatabase();

            if (filteredList.length === 0) {
                filteredList = [...FOOD_DATABASE];
                document.getElementById('matching-info').innerHTML = `
                    <span class="text-diner-orange font-bold">💡 입력하신 조건에 들어맞는 메뉴가 없어 전체 50가지 메뉴 중 추천을 진행합니다!</span>
                `;
            } else {
                const fText = userSelections.flavors.length ? userSelections.flavors.join(',') : '전체';
                const tText = userSelections.mainTypes.length ? userSelections.mainTypes.join(',') : '전체';
                const cText = userSelections.cuisines.length ? userSelections.cuisines.join(',') : '전체';

                document.getElementById('matching-info').innerHTML = `
                    🎯 <strong>선택 조건:</strong> 맛(${fText}) | 종류(${tText}) | 출처(${cText}) 
                    <span class="text-diner-orange font-bold">(${filteredList.length}개 후보 세팅 완료!)</span>
                `;
            }

            nextStep(4);
            document.getElementById('result-card').classList.add('hidden');
            drawRouletteWheel();
        }

        function drawRouletteWheel() {
            if (!canvas || !ctx) return;

            const outsideRadius = 150;
            const textRadius = 100;
            const insideRadius = 35;

            ctx.clearRect(0, 0, 320, 320);

            arc = Math.PI / (filteredList.length / 2);

            for (let i = 0; i < filteredList.length; i++) {
                const angle = startAngle + i * arc;
                ctx.fillStyle = colors[i % colors.length];

                ctx.beginPath();
                ctx.arc(160, 160, outsideRadius, angle, angle + arc, false);
                ctx.arc(160, 160, insideRadius, angle + arc, angle, true);
                ctx.fill();

                ctx.save();
                ctx.fillStyle = "white";
                ctx.font = "bold 13px 'Noto Sans KR', sans-serif";
                ctx.translate(160 + Math.cos(angle + arc / 2) * textRadius, 
                              160 + Math.sin(angle + arc / 2) * textRadius);
                ctx.rotate(angle + arc / 2 + Math.PI / 2);
                
                const text = filteredList[i].name;
                ctx.fillText(text, -ctx.measureText(text).width / 2, 0);
                ctx.restore();
            }
        }

        function spinRoulette() {
            const spinBtn = document.getElementById('spin-btn');
            spinBtn.disabled = true;
            spinBtn.classList.add('opacity-50', 'cursor-not-allowed');

            document.getElementById('result-card').classList.add('hidden');

            spinAngleStart = Math.random() * 10 + 20;
            spinTime = 0;
            spinTimeTotal = Math.random() * 3000 + 4000;
            rotateWheel();
        }

        function rotateWheel() {
            spinTime += 20;
            if (spinTime >= spinTimeTotal) {
                stopRotateWheel();
                return;
            }
            const spinAngle = spinAngleStart - easeOut(spinTime, 0, spinAngleStart, spinTimeTotal);
            startAngle += (spinAngle * Math.PI / 180);
            drawRouletteWheel();
            spinTimeout = setTimeout(rotateWheel, 20);
        }

        function stopRotateWheel() {
            clearTimeout(spinTimeout);
            const degrees = startAngle * 180 / Math.PI + 90;
            const arcd = arc * 180 / Math.PI;
            const index = Math.floor((360 - (degrees % 360)) / arcd);
            
            selectedDish = filteredList[index < 0 ? index + filteredList.length : index];
            showResult(selectedDish);

            const spinBtn = document.getElementById('spin-btn');
            spinBtn.disabled = false;
            spinBtn.classList.remove('opacity-50', 'cursor-not-allowed');
        }

        function easeOut(t, b, c, d) {
            const ts = (t /= d) * t;
            const tc = ts * t;
            return b + c * (tc + -3 * ts + 3 * t);
        }

        function showResult(dish) {
            const resultCard = document.getElementById('result-card');
            
            document.getElementById('result-emoji').innerText = dish.emoji;
            document.getElementById('result-title').innerText = dish.name;
            document.getElementById('result-desc').innerText = dish.desc;
            document.getElementById('result-tip').innerText = dish.tip;

            // Update Map Links
            updateMapLinks(dish.name);

            // Generate tags
            const tagsContainer = document.getElementById('result-tags');
            tagsContainer.innerHTML = '';
            
            const allTags = [...dish.flavors, dish.type, dish.cuisine];
            allTags.forEach(t => {
                const badge = document.createElement('span');
                badge.className = "bg-amber-200/70 text-amber-900 text-xs px-2.5 py-1 rounded-full font-medium";
                badge.innerText = `#${t}`;
                tagsContainer.appendChild(badge);
            });

            resultCard.classList.remove('hidden');
            setTimeout(() => {
                resultCard.classList.remove('scale-95', 'opacity-0');
                resultCard.classList.add('scale-100', 'opacity-100');
            }, 50);

            // Confetti effect
            if (typeof confetti === 'function') {
                confetti({
                    particleCount: 80,
                    spread: 70,
                    origin: { y: 0.6 }
                });
            }
        }

        function pickRandomDirectly() {
            userSelections.flavors = [];
            userSelections.mainTypes = [];
            userSelections.cuisines = [];
            startRouletteStep();
        }

        function resetAll() {
            userSelections.flavors = [];
            userSelections.mainTypes = [];
            userSelections.cuisines = [];

            document.querySelectorAll('.tag-btn').forEach(btn => btn.classList.remove('selected'));
            nextStep(1);
        }

        function shareLink() {
            const textToCopy = window.location.href;
            
            const dummy = document.createElement('input');
            document.body.appendChild(dummy);
            dummy.value = textToCopy;
            dummy.select();
            document.execCommand('copy');
            document.body.removeChild(dummy);

            showToast("링크가 클립보드에 복사되었습니다! 친구들과 공유해보세요 🍕");
        }

        function showToast(msg) {
            const toast = document.getElementById('toast');
            toast.innerText = msg;
            toast.classList.remove('opacity-0', 'pointer-events-none');
            
            setTimeout(() => {
                toast.classList.add('opacity-0', 'pointer-events-none');
            }, 2500);
        }
    </script>
</body>
</html>
