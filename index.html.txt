<!DOCTYPE html>
<html lang="km">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Khmer Stream - គេហទំព័រមើលរឿង និងវីដេអូ</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Battambang & Kantumruy Pro for Khmer typography -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Battambang:wght@400;700;900&family=Kantumruy+Pro:ital,wght@0,300;0,400;0,600;0,700;1,400&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            red: '#E50914',
                            dark: '#141414',
                            card: '#1f1f1f',
                            gray: '#2f2f2f'
                        }
                    },
                    fontFamily: {
                        khmer: ['Kantumruy Pro', 'Battambang', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Kantumruy Pro', 'Battambang', sans-serif;
            background-color: #141414;
            color: #ffffff;
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #141414;
        }
        ::-webkit-scrollbar-thumb {
            background: #333;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #E50914;
        }
        .hide-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .hide-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
    </style>
</head>
<body class="bg-brand-dark min-h-screen flex flex-col selection:bg-brand-red selection:text-white">

    <!-- Navigation Bar -->
    <nav id="navbar" class="fixed top-0 left-0 right-0 z-40 transition-all duration-300 bg-gradient-to-b from-black/90 via-black/60 to-transparent py-4 px-4 md:px-8">
        <div class="max-w-7xl mx-auto flex items-center justify-between gap-4">
            <!-- Brand Logo & Main Nav -->
            <div class="flex items-center space-x-6">
                <a href="#" onclick="showSection('home')" class="flex items-center gap-2">
                    <span class="text-2xl md:text-3xl font-black text-brand-red tracking-wider">KHMER<span class="text-white">STREAM</span></span>
                </a>
                <div class="hidden lg:flex items-center space-x-4 text-sm font-medium">
                    <button onclick="filterCategory('all')" class="nav-btn hover:text-brand-red transition text-white">ទំព័រដើម</button>
                    <button onclick="filterCategory('វាយប្រហារ')" class="nav-btn hover:text-brand-red transition text-gray-300">រឿងវាយប្រហារ</button>
                    <button onclick="filterCategory('កំប្លែង')" class="nav-btn hover:text-brand-red transition text-gray-300">រឿងកំប្លែង</button>
                    <button onclick="filterCategory('ស្នេហា')" class="nav-btn hover:text-brand-red transition text-gray-300">រឿងស្នេហា</button>
                    <button onclick="filterCategory('រឿងតុក្កតា')" class="nav-btn hover:text-brand-red transition text-gray-300">តុក្កតា/Anime</button>
                    <button onclick="filterCategory('វីដេអូទូទៅ')" class="nav-btn hover:text-brand-red transition text-gray-300">វីដេអូទូទៅ</button>
                </div>
            </div>

            <!-- Search, Admin Status, Login/Logout Button -->
            <div class="flex items-center gap-3 md:gap-4">
                <!-- Search Input -->
                <div class="relative">
                    <input type="text" id="searchInput" oninput="handleSearch()" placeholder="ស្វែងរករឿង/វីដេអូ..." class="bg-black/60 border border-gray-700 rounded-full py-1.5 px-4 pl-9 text-xs md:text-sm text-white focus:outline-none focus:border-brand-red w-36 sm:w-48 md:w-64 transition-all">
                    <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-gray-400 text-xs"></i>
                </div>

                <!-- Admin Button (Shows when logged in) -->
                <button id="adminPanelBtn" onclick="showSection('admin')" class="hidden bg-yellow-600 hover:bg-yellow-500 text-white text-xs md:text-sm font-semibold py-1.5 px-3 rounded-full flex items-center gap-1.5 transition">
                    <i class="fa-solid fa-gear"></i>
                    <span class="hidden sm:inline">គ្រប់គ្រងវីដេអូ</span>
                </button>

                <!-- Login Status / Button -->
                <div id="authContainer">
                    <button id="loginModalBtn" onclick="openLoginModal()" class="bg-brand-red hover:bg-red-700 text-white text-xs md:text-sm px-4 py-1.5 rounded-md font-medium transition flex items-center gap-1.5">
                        <i class="fa-solid fa-user"></i>
                        <span>ចូលប្រើប្រាស់</span>
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Toast Alert Notification -->
    <div id="toast" class="fixed bottom-5 right-5 z-50 transform translate-y-20 opacity-0 transition-all duration-300 bg-gray-900 border-l-4 border-brand-red text-white p-4 rounded-r shadow-2xl flex items-center gap-3 max-w-sm pointer-events-none">
        <i id="toastIcon" class="fa-solid fa-circle-info text-brand-red text-xl"></i>
        <div id="toastMessage" class="text-sm">សារជូនដំណឹង</div>
    </div>

    <!-- Main Content Container -->
    <main class="flex-grow pt-16">
        
        <!-- HOME SECTION -->
        <section id="homeSection">
            <!-- Hero Banner Slider/Feature -->
            <div id="heroBanner" class="relative w-full h-[65vh] md:h-[75vh] bg-cover bg-center flex items-end transition-all duration-700">
                <div class="absolute inset-0 bg-gradient-to-t from-brand-dark via-brand-dark/40 to-black/30"></div>
                
                <div class="relative max-w-7xl mx-auto px-4 md:px-8 pb-12 w-full z-10">
                    <span id="heroBadge" class="bg-brand-red text-white text-xs px-2.5 py-1 rounded font-bold uppercase tracking-wider mb-2 inline-block">កំពុងពេញនិយម</span>
                    <h1 id="heroTitle" class="text-2xl md:text-5xl font-black mb-3 text-white drop-shadow-md leading-tight">...</h1>
                    <p id="heroDesc" class="text-xs md:text-sm text-gray-300 max-w-xl line-clamp-3 mb-5 leading-relaxed drop-shadow">...</p>
                    
                    <div class="flex items-center gap-3">
                        <button id="heroPlayBtn" onclick="" class="bg-white hover:bg-gray-200 text-black font-bold px-6 py-2.5 rounded-md flex items-center gap-2 text-sm md:text-base transition">
                            <i class="fa-solid fa-play text-lg"></i> មើលឥឡូវនេះ
                        </button>
                        <button id="heroInfoBtn" onclick="" class="bg-gray-600/70 hover:bg-gray-600 text-white font-bold px-5 py-2.5 rounded-md flex items-center gap-2 text-sm md:text-base backdrop-blur-sm transition">
                            <i class="fa-solid fa-circle-info text-lg"></i> ព័ត៌មានបន្ថែម
                        </button>
                    </div>
                </div>
            </div>

            <!-- Video Catalog Sections -->
            <div class="max-w-7xl mx-auto px-4 md:px-8 py-8 space-y-10">
                
                <!-- Category Filters (Mobile Friendly Scrollable) -->
                <div class="flex items-center gap-2 overflow-x-auto pb-2 hide-scrollbar text-xs md:text-sm">
                    <button onclick="filterCategory('all')" class="cat-pill bg-brand-red text-white px-4 py-1.5 rounded-full whitespace-nowrap">ទាំងអស់</button>
                    <button onclick="filterCategory('វាយប្រហារ')" class="cat-pill bg-brand-card hover:bg-gray-800 text-gray-300 px-4 py-1.5 rounded-full whitespace-nowrap">រឿងវាយប្រហារ (Action)</button>
                    <button onclick="filterCategory('កំប្លែង')" class="cat-pill bg-brand-card hover:bg-gray-800 text-gray-300 px-4 py-1.5 rounded-full whitespace-nowrap">រឿងកំប្លែង (Comedy)</button>
                    <button onclick="filterCategory('ស្នេហា')" class="cat-pill bg-brand-card hover:bg-gray-800 text-gray-300 px-4 py-1.5 rounded-full whitespace-nowrap">រឿងស្នេហា (Drama)</button>
                    <button onclick="filterCategory('រឿងតុក្កតា')" class="cat-pill bg-brand-card hover:bg-gray-800 text-gray-300 px-4 py-1.5 rounded-full whitespace-nowrap">តុក្កតា (Anime)</button>
                    <button onclick="filterCategory('វីដេអូទូទៅ')" class="cat-pill bg-brand-card hover:bg-gray-800 text-gray-300 px-4 py-1.5 rounded-full whitespace-nowrap">វីដេអូទូទៅ</button>
                </div>

                <!-- Video Grid Container -->
                <div>
                    <div class="flex items-center justify-between mb-4">
                        <h2 id="catalogTitle" class="text-xl md:text-2xl font-bold border-l-4 border-brand-red pl-3">រឿងភាគ និងវីដេអូទាំងអស់</h2>
                        <span id="movieCount" class="text-xs text-gray-400"></span>
                    </div>

                    <div id="movieGrid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-3 md:gap-5">
                        <!-- Dynamic Movie Cards will be inserted here -->
                    </div>

                    <!-- Empty Search / Filter State -->
                    <div id="emptyState" class="hidden py-16 text-center text-gray-400">
                        <i class="fa-solid fa-film text-5xl mb-3 text-gray-600"></i>
                        <p class="text-lg font-medium">មិនមានវីដេអូ ឬរឿងភាគដែលអ្នកស្វែងរកទេ!</p>
                        <p class="text-xs text-gray-500 mt-1">សូមព្យាយាមស្វែងរកពាក្យផ្សេង ឬជ្រើសរើសប្រភេទផ្សេង</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- ADMIN DASHBOARD SECTION -->
        <section id="adminSection" class="hidden max-w-7xl mx-auto px-4 md:px-8 py-8">
            <!-- Owner Banner Bar -->
            <div class="bg-gradient-to-r from-yellow-900/40 via-yellow-800/20 to-transparent border border-yellow-600/40 p-4 rounded-xl mb-8 flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-full bg-yellow-500/20 flex items-center justify-center text-yellow-500 text-xl font-bold border border-yellow-500/40">
                        <i class="fa-solid fa-user-shield"></i>
                    </div>
                    <div>
                        <h3 class="font-bold text-yellow-400 text-base">អ្នកកំពុងស្ថិតនៅក្នុង Admin / Owner Mode</h3>
                        <p class="text-xs text-gray-300">អ្នកមានសិទ្ធិពេញលេញក្នុងការ បន្ថែម កែប្រែ ឬលុបវីដេអូ និងរឿងភាគចេញពីប្រព័ន្ធ</p>
                    </div>
                </div>
                <button onclick="showSection('home')" class="text-xs bg-gray-800 hover:bg-gray-700 text-white px-3 py-2 rounded-lg transition border border-gray-700">
                    <i class="fa-solid fa-arrow-left mr-1"></i> ត្រឡប់ទៅទំព័រដើម
                </button>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
                <!-- Add / Edit Video Form -->
                <div class="lg:col-span-1 bg-brand-card p-6 rounded-xl border border-gray-800 shadow-xl self-start">
                    <h3 id="formTitle" class="text-lg font-bold mb-4 flex items-center gap-2 border-b border-gray-800 pb-3">
                        <i class="fa-solid fa-plus-circle text-brand-red"></i> បន្ថែមវីដេអូថ្មី
                    </h3>
                    
                    <form id="movieForm" onsubmit="handleFormSubmit(event)" class="space-y-4">
                        <input type="hidden" id="movieId">
                        
                        <div>
                            <label class="block text-xs font-semibold text-gray-300 mb-1">ចំណងជើងរឿង/វីដេអូ *</label>
                            <input type="text" id="inputTitle" required placeholder="ឧទាហរណ៍៖ Avatar: The Way of Water" class="w-full bg-black/50 border border-gray-700 rounded-lg p-2.5 text-sm text-white focus:outline-none focus:border-brand-red">
                        </div>

                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-semibold text-gray-300 mb-1">ប្រភេទរឿង *</label>
                                <select id="inputCategory" required class="w-full bg-black/50 border border-gray-700 rounded-lg p-2.5 text-sm text-white focus:outline-none focus:border-brand-red">
                                    <option value="វាយប្រហារ">វាយប្រហារ (Action)</option>
                                    <option value="កំប្លែង">កំប្លែង (Comedy)</option>
                                    <option value="ស្នេហា">ស្នេហា (Drama)</option>
                                    <option value="រឿងតុក្កតា">តុក្កតា (Anime)</option>
                                    <option value="វីដេអូទូទៅ">វីដេអូទូទៅ</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-gray-300 mb-1">ពិន្ទុ (0 - 10) *</label>
                                <input type="number" id="inputRating" step="0.1" min="0" max="10" value="8.5" required class="w-full bg-black/50 border border-gray-700 rounded-lg p-2.5 text-sm text-white focus:outline-none focus:border-brand-red">
                            </div>
                        </div>

                        <div class="grid grid-cols-2 gap-3">
                            <div>
                                <label class="block text-xs font-semibold text-gray-300 mb-1">ឆ្នាំចេញផ្សាយ *</label>
                                <input type="text" id="inputYear" required placeholder="2024" value="2024" class="w-full bg-black/50 border border-gray-700 rounded-lg p-2.5 text-sm text-white focus:outline-none focus:border-brand-red">
                            </div>
                            <div>
                                <label class="block text-xs font-semibold text-gray-300 mb-1">រយៈពេល/ភាគ *</label>
                                <input type="text" id="inputDuration" required placeholder="120 នាទី ឬ ភាគ 1" value="2 ម៉ោង 15 នាទី" class="w-full bg-black/50 border border-gray-700 rounded-lg p-2.5 text-sm text-white focus:outline-none focus:border-brand-red">
                            </div>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-gray-300 mb-1">តំណភ្ជាប់រូបភាព (Thumbnail URL) *</label>
                            <input type="url" id="inputPoster" required placeholder="https://example.com/image.jpg" class="w-full bg-black/50 border border-gray-700 rounded-lg p-2.5 text-sm text-white focus:outline-none focus:border-brand-red">
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-gray-300 mb-1">តំណភ្ជាប់វីដេអូ (YouTube Embed ឬ MP4 URL) *</label>
                            <input type="url" id="inputVideoUrl" required placeholder="https://www.youtube.com/embed/..." class="w-full bg-black/50 border border-gray-700 rounded-lg p-2.5 text-sm text-white focus:outline-none focus:border-brand-red">
                            <span class="text-[10px] text-gray-400 mt-1 block">ចំណាំ៖ YouTube សូមប្រើប្រភេទ Embed (ឧទាហរណ៍៖ https://www.youtube.com/embed/VIDEO_ID)</span>
                        </div>

                        <div>
                            <label class="block text-xs font-semibold text-gray-300 mb-1">ការពិពណ៌នាសង្ខេប *</label>
                            <textarea id="inputDesc" rows="3" required placeholder="រៀបរាប់ពីសាច់រឿងសង្ខេប..." class="w-full bg-black/50 border border-gray-700 rounded-lg p-2.5 text-sm text-white focus:outline-none focus:border-brand-red"></textarea>
                        </div>

                        <div class="flex items-center gap-2 pt-2">
                            <button type="submit" id="submitBtn" class="flex-1 bg-brand-red hover:bg-red-700 text-white font-semibold py-2.5 rounded-lg transition text-sm">
                                <i class="fa-solid fa-save mr-1"></i> រក្សាទុកវីដេអូ
                            </button>
                            <button type="button" id="cancelBtn" onclick="resetForm()" class="hidden bg-gray-700 hover:bg-gray-600 text-white px-4 py-2.5 rounded-lg transition text-sm">
                                បោះបង់
                            </button>
                        </div>
                    </form>
                </div>

                <!-- Manage Video Table / List -->
                <div class="lg:col-span-2 bg-brand-card p-6 rounded-xl border border-gray-800 shadow-xl">
                    <div class="flex items-center justify-between mb-4 border-b border-gray-800 pb-3">
                        <h3 class="text-lg font-bold flex items-center gap-2">
                            <i class="fa-solid fa-list text-yellow-500"></i> បញ្ជីវីដេអូដែលមានស្រាប់
                        </h3>
                        <button onclick="resetToDefaultData()" class="text-xs text-red-400 hover:text-red-300 underline">
                            កំណត់ទិន្នន័យដើមឡើងវិញ (Reset Demo Data)
                        </button>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="w-full text-left text-xs md:text-sm text-gray-300">
                            <thead class="bg-black/60 text-gray-400 uppercase text-[10px] md:text-xs tracking-wider">
                                <tr>
                                    <th class="p-3">រូបភាព</th>
                                    <th class="p-3">ចំណងជើង</th>
                                    <th class="p-3">ប្រភេទ</th>
                                    <th class="p-3">ពិន្ទុ</th>
                                    <th class="p-3 text-right">សកម្មភាព</th>
                                </tr>
                            </thead>
                            <tbody id="adminMovieTable" class="divide-y divide-gray-800">
                                <!-- Dynamic Table Rows -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- VIDEO PLAYER MODAL -->
    <div id="videoModal" class="fixed inset-0 z-50 bg-black/90 backdrop-blur-md hidden flex items-center justify-center p-2 sm:p-4 md:p-6 overflow-y-auto">
        <div class="relative w-full max-w-4xl bg-brand-card rounded-2xl overflow-hidden border border-gray-800 shadow-2xl my-auto">
            
            <!-- Close Modal Button -->
            <button onclick="closeVideoModal()" class="absolute top-3 right-3 z-20 w-9 h-9 bg-black/70 hover:bg-brand-red text-white rounded-full flex items-center justify-center transition">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>

            <!-- Video Player Embed Container -->
            <div id="playerContainer" class="relative w-full aspect-video bg-black">
                <!-- Video/Iframe dynamically injected here -->
            </div>

            <!-- Video Info & Interactions -->
            <div class="p-4 sm:p-6 space-y-4 max-h-[40vh] overflow-y-auto">
                <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-2 border-b border-gray-800 pb-3">
                    <div>
                        <h2 id="modalTitle" class="text-xl md:text-2xl font-bold text-white"></h2>
                        <div class="flex items-center gap-3 text-xs text-gray-400 mt-1">
                            <span id="modalCategory" class="bg-gray-800 text-gray-200 px-2 py-0.5 rounded"></span>
                            <span id="modalYear"></span>
                            <span id="modalDuration"></span>
                            <span class="text-yellow-400 font-bold flex items-center gap-1">
                                <i class="fa-solid fa-star"></i> <span id="modalRating"></span>
                            </span>
                        </div>
                    </div>

                    <!-- Like / Share Actions -->
                    <div class="flex items-center gap-2">
                        <button id="likeBtn" onclick="toggleLike()" class="bg-gray-800 hover:bg-gray-700 text-white text-xs px-3.5 py-2 rounded-full flex items-center gap-1.5 transition">
                            <i id="likeIcon" class="fa-regular fa-thumbs-up"></i>
                            <span id="likeCount">128</span>
                        </button>
                        <button onclick="copyCurrentLink()" class="bg-gray-800 hover:bg-gray-700 text-white text-xs px-3.5 py-2 rounded-full flex items-center gap-1.5 transition">
                            <i class="fa-solid fa-share-nodes"></i> ចែករំលែក
                        </button>
                    </div>
                </div>

                <!-- Description -->
                <p id="modalDesc" class="text-xs md:text-sm text-gray-300 leading-relaxed"></p>

                <!-- Comments Section -->
                <div class="pt-2">
                    <h4 class="text-sm font-bold mb-3 text-white flex items-center gap-2">
                        <i class="fa-solid fa-comments text-brand-red"></i> មតិយោបល់ (<span id="commentCount">0</span>)
                    </h4>
                    
                    <!-- Comment Input -->
                    <div class="flex gap-2 mb-4">
                        <input type="text" id="commentInput" placeholder="បញ្ចេញមតិយោបល់របស់អ្នកនៅទីនេះ..." class="flex-1 bg-black/60 border border-gray-700 rounded-lg text-xs md:text-sm px-3 py-2 text-white focus:outline-none focus:border-brand-red">
                        <button onclick="addComment()" class="bg-brand-red hover:bg-red-700 text-white text-xs md:text-sm px-4 py-2 rounded-lg font-medium transition">
                            ផ្ញើ
                        </button>
                    </div>

                    <!-- Comment List -->
                    <div id="commentList" class="space-y-2 max-h-36 overflow-y-auto pr-1">
                        <!-- Dynamic Comments -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- LOGIN MODAL -->
    <div id="loginModal" class="fixed inset-0 z-50 bg-black/80 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="relative w-full max-w-md bg-brand-card rounded-2xl p-6 md:p-8 border border-gray-800 shadow-2xl">
            
            <button onclick="closeLoginModal()" class="absolute top-4 right-4 text-gray-400 hover:text-white text-xl">
                <i class="fa-solid fa-xmark"></i>
            </button>

            <div class="text-center mb-6">
                <div class="w-12 h-12 bg-brand-red/10 text-brand-red rounded-full flex items-center justify-center mx-auto mb-3 text-2xl border border-brand-red/30">
                    <i class="fa-solid fa-lock"></i>
                </div>
                <h3 class="text-xl font-bold text-white">ចូលប្រើប្រាស់គណនី</h3>
                <p class="text-xs text-gray-400 mt-1">សម្រាប់ម្ចាស់ Website (Admin/Owner Demo)</p>
            </div>

            <!-- Demo Credentials Hint Box -->
            <div class="bg-gray-800/80 border border-gray-700 rounded-lg p-3 mb-5 text-xs text-gray-300">
                <div class="font-bold text-yellow-400 mb-1 flex items-center gap-1">
                    <i class="fa-solid fa-key"></i> ព័ត៌មាន Login សាកល្បង (Demo):
                </div>
                <p class="font-mono text-gray-200">Email: <span class="text-white bg-black/50 px-1.5 py-0.5 rounded select-all">demo@gmail.com</span></p>
                <p class="font-mono text-gray-200 mt-1">Password: <span class="text-white bg-black/50 px-1.5 py-0.5 rounded select-all">password123</span></p>
            </div>

            <form onsubmit="handleLogin(event)" class="space-y-4">
                <div>
                    <label class="block text-xs font-semibold text-gray-300 mb-1">អាសយដ្ឋាន អ៊ីមែល (Email)</label>
                    <input type="email" id="loginEmail" required value="demo@gmail.com" class="w-full bg-black/60 border border-gray-700 rounded-lg p-3 text-sm text-white focus:outline-none focus:border-brand-red">
                </div>

                <div>
                    <label class="block text-xs font-semibold text-gray-300 mb-1">ពាក្យសម្ងាត់ (Password)</label>
                    <input type="password" id="loginPassword" required value="password123" class="w-full bg-black/60 border border-gray-700 rounded-lg p-3 text-sm text-white focus:outline-none focus:border-brand-red">
                </div>

                <button type="submit" class="w-full bg-brand-red hover:bg-red-700 text-white font-bold py-3 rounded-lg transition text-sm shadow-lg shadow-brand-red/30">
                    ចូលប្រព័ន្ធ
                </button>
            </form>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-black border-t border-gray-800 py-8 px-4 mt-12 text-center text-xs text-gray-500">
        <div class="max-w-7xl mx-auto space-y-3">
            <p class="text-sm font-bold text-gray-400">KHMER STREAM &copy; 2026 - រក្សាសិទ្ធិគ្រប់យ៉ាង</p>
            <p class="max-w-md mx-auto text-gray-500">គេហទំព័រគំរូសម្រាប់ទស្សនារឿងភាគ និងវីដេអូកម្សាន្ត។ បង្កើតឡើងដោយប្រើប្រាស់ HTML, Tailwind CSS និង JavaScript។</p>
        </div>
    </footer>

    <script>
        // Default Sample Movies Data
        const defaultMovies = [
            {
                id: '1',
                title: ' Avatar: The Way of Water',
                category: 'វាយប្រហារ',
                rating: 8.8,
                year: '2022',
                duration: '3 ម៉ោង 12 នាទី',
                poster: 'https://images.unsplash.com/photo-1534447677768-be436bb09401?q=80&w=600&auto=format&fit=crop',
                videoUrl: 'https://www.youtube.com/embed/d9MyW72ELq0',
                description: 'សាច់រឿងបន្តពីពិភព Pandora ដែលគ្រួសារ Sully ត្រូវប្រឈមមុខនឹងគ្រោះថ្នាក់ថ្មី និងស្វែងរកជម្រកនៅជាមួយកុលសម្ព័ន្ធសមុទ្រ Metkayina។'
            },
            {
                id: '2',
                title: ' Kung Fu Panda 4',
                category: 'រឿងតុក្កតា',
                rating: 8.2,
                year: '2024',
                duration: '1 ម៉ោង 34 នាទី',
                poster: 'https://images.unsplash.com/photo-1563089145-599997674d42?q=80&w=600&auto=format&fit=crop',
                videoUrl: 'https://www.youtube.com/embed/_inKs4eeHiI',
                description: 'Po ត្រូវតែហ្វឹកហ្វឺនអ្នកស្នងតំណែង Dragon Warrior ថ្មី ខណៈពេលដែលត្រូវប្រឈមមុខនឹងមេធ្មប់ Chameleon ដែលមានសមត្ថភាពក្លែងបន្លំខ្លួន។'
            },
            {
                id: '3',
                title: ' រឿងកំប្លែង៖ អ្នកបម្រើសែនវង្វេង',
                category: 'កំប្លែង',
                rating: 9.0,
                year: '2023',
                duration: '1 ម៉ោង 45 នាទី',
                poster: 'https://images.unsplash.com/photo-1517604931442-7e0c8ed2963c?q=80&w=600&auto=format&fit=crop',
                videoUrl: 'https://www.youtube.com/embed/L3oOldviIgY',
                description: 'រឿងភាគកំប្លែងសើចសប្បាយរីករាយជាមួយកាយវិការ និងការសម្ដែងដ៏ក្រមិចក្រមើមពីតារាកំប្លែងល្បីៗក្នុងស្រុក។'
            },
            {
                id: '4',
                title: ' ស្នេហាក្នុងក្ដីស្រមៃ (Melody of Love)',
                category: 'ស្នេហា',
                rating: 8.5,
                year: '2024',
                duration: 'ភាគ 16 ពេញ',
                poster: 'https://images.unsplash.com/photo-1518676599625-5829377484d4?q=80&w=600&auto=format&fit=crop',
                videoUrl: 'https://www.youtube.com/embed/dQw4w9WgXcQ',
                description: 'រឿងភាគស្នេហាយ៉ាងផ្អែមល្ហែម និងលាយឡំដោយការតស៊ូក្នុងជីវិតរបស់អ្នកសិល្បៈតន្ត្រីវ័យក្មេង។'
            },
            {
                id: '5',
                title: ' វីដេអូដំណើរកម្សាន្តធម្មជាតិកម្ពុជា',
                category: 'វីដេអូទូទៅ',
                rating: 9.5,
                year: '2024',
                duration: '25 នាទី',
                poster: 'https://images.unsplash.com/photo-1507525428034-b723cf961d3e?q=80&w=600&auto=format&fit=crop',
                videoUrl: 'https://www.youtube.com/embed/ScMzIvxBSi4',
                description: 'ទស្សនាទិដ្ឋភាពដ៏ស្រស់ស្អាតនៃព្រៃភ្នំ និងទឹកធ្លាក់ក្នុងប្រទេសកម្ពុជាជាមួយកម្រិតរូបភាពច្បាស់ 4K Ultra HD។'
            }
        ];

        // State variables
        let movies = [];
        let isLoggedIn = false;
        let activeCategory = 'all';
        let activeMovie = null;
        let movieComments = {};
        let likedMovies = new Set();

        window.onload = function() {
            // Load saved movies or load defaults
            const savedMovies = localStorage.getItem('khmer_stream_movies');
            if (savedMovies) {
                movies = JSON.parse(savedMovies);
            } else {
                movies = [...defaultMovies];
                saveMoviesToStorage();
            }

            // Check login state
            const authState = localStorage.getItem('khmer_stream_auth');
            if (authState === 'true') {
                isLoggedIn = true;
            }

            updateAuthUI();
            renderHero();
            renderCatalog();
            renderAdminTable();
        };

        // Save Movies to LocalStorage
        function saveMoviesToStorage() {
            localStorage.setItem('khmer_stream_movies', JSON.stringify(movies));
        }

        // Handle User Login
        function handleLogin(e) {
            e.preventDefault();
            const email = document.getElementById('loginEmail').value;
            const pass = document.getElementById('loginPassword').value;

            // Demo Admin Credentials
            if (email === 'demo@gmail.com' && pass === 'password123') {
                isLoggedIn = true;
                localStorage.setItem('khmer_stream_auth', 'true');
                updateAuthUI();
                closeLoginModal();
                showToast('ចូលប្រើប្រាស់ដោយជោគជ័យ! សូមស្វាគមន៍ Admin', 'success');
            } else {
                showToast('អ៊ីមែល ឬពាក្យសម្ងាត់មិនត្រឹមត្រូវទេ!', 'error');
            }
        }

        // Logout
        function handleLogout() {
            isLoggedIn = false;
            localStorage.removeItem('khmer_stream_auth');
            updateAuthUI();
            showSection('home');
            showToast('បានចាកចេញពីប្រព័ន្ធដោយជោគជ័យ', 'info');
        }

        // Update Nav UI according to Login Status
        function updateAuthUI() {
            const authContainer = document.getElementById('authContainer');
            const adminPanelBtn = document.getElementById('adminPanelBtn');

            if (isLoggedIn) {
                adminPanelBtn.classList.remove('hidden');
                authContainer.innerHTML = `
                    <div class="flex items-center gap-2">
                        <span class="hidden md:inline text-xs bg-yellow-500/20 text-yellow-400 px-2 py-1 rounded-full border border-yellow-500/30">
                            <i class="fa-solid fa-crown text-yellow-500"></i> Admin Mode
                        </span>
                        <button onclick="handleLogout()" class="bg-gray-800 hover:bg-gray-700 text-white text-xs md:text-sm px-3.5 py-1.5 rounded-md transition flex items-center gap-1.5">
                            <i class="fa-solid fa-right-from-bracket"></i>
                            <span>ចាកចេញ</span>
                        </button>
                    </div>
                `;
            } else {
                adminPanelBtn.classList.add('hidden');
                authContainer.innerHTML = `
                    <button onclick="openLoginModal()" class="bg-brand-red hover:bg-red-700 text-white text-xs md:text-sm px-4 py-1.5 rounded-md font-medium transition flex items-center gap-1.5">
                        <i class="fa-solid fa-user"></i>
                        <span>ចូលប្រើប្រាស់</span>
                    </button>
                `;
            }
        }

        // Navigation between Home and Admin Dashboard
        function showSection(sectionName) {
            const homeSection = document.getElementById('homeSection');
            const adminSection = document.getElementById('adminSection');

            if (sectionName === 'admin') {
                if (!isLoggedIn) {
                    openLoginModal();
                    return;
                }
                homeSection.classList.add('hidden');
                adminSection.classList.remove('hidden');
                window.scrollTo({ top: 0, behavior: 'smooth' });
            } else {
                adminSection.classList.add('hidden');
                homeSection.classList.remove('hidden');
            }
        }

        // Render Top Feature/Hero Movie
        function renderHero() {
            if (movies.length === 0) return;
            const heroMovie = movies[0]; // First movie as hero feature

            const heroBanner = document.getElementById('heroBanner');
            const heroTitle = document.getElementById('heroTitle');
            const heroDesc = document.getElementById('heroDesc');
            const heroPlayBtn = document.getElementById('heroPlayBtn');
            const heroInfoBtn = document.getElementById('heroInfoBtn');

            heroBanner.style.backgroundImage = `url('${heroMovie.poster}')`;
            heroTitle.innerText = heroMovie.title;
            heroDesc.innerText = heroMovie.description;
            heroPlayBtn.onclick = () => openVideoModal(heroMovie.id);
            heroInfoBtn.onclick = () => openVideoModal(heroMovie.id);
        }

        // Render Cards Grid
        function renderCatalog() {
            const movieGrid = document.getElementById('movieGrid');
            const emptyState = document.getElementById('emptyState');
            const movieCount = document.getElementById('movieCount');

            // Filter movies based on category and search
            const searchQuery = document.getElementById('searchInput').value.toLowerCase().trim();
            
            const filtered = movies.filter(m => {
                const matchCategory = activeCategory === 'all' || m.category === activeCategory;
                const matchSearch = m.title.toLowerCase().includes(searchQuery) || m.description.toLowerCase().includes(searchQuery);
                return matchCategory && matchSearch;
            });

            movieCount.innerText = `បង្ហាញ ${filtered.length} វីដេអូ`;

            if (filtered.length === 0) {
                movieGrid.innerHTML = '';
                emptyState.classList.remove('hidden');
                return;
            }

            emptyState.classList.add('hidden');
            movieGrid.innerHTML = filtered.map(m => `
                <div onclick="openVideoModal('${m.id}')" class="group relative bg-brand-card rounded-xl overflow-hidden cursor-pointer shadow-lg hover:shadow-2xl hover:scale-[1.03] transition-all duration-300 flex flex-col">
                    <div class="relative aspect-[2/3] w-full overflow-hidden bg-gray-900">
                        <img src="${m.poster}" alt="${m.title}" class="w-full h-full object-cover group-hover:opacity-80 transition duration-500" onerror="this.src='https://placehold.co/400x600/1f1f1f/ffffff?text=No+Cover'">
                        <div class="absolute top-2 right-2 bg-black/70 backdrop-blur-md text-yellow-400 font-bold text-[11px] px-2 py-0.5 rounded-full flex items-center gap-1">
                            <i class="fa-solid fa-star"></i> ${m.rating}
                        </div>
                        <div class="absolute inset-0 bg-gradient-to-t from-black via-transparent to-transparent opacity-80"></div>
                        <div class="absolute bottom-3 left-3 right-3 flex items-center justify-between text-[11px] text-gray-300">
                            <span class="bg-brand-red/90 text-white px-2 py-0.5 rounded font-semibold">${m.category}</span>
                            <span>${m.year}</span>
                        </div>
                        <!-- Hover Play Overlay -->
                        <div class="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity bg-black/40">
                            <div class="w-12 h-12 bg-brand-red rounded-full flex items-center justify-center text-white text-xl shadow-xl transform group-hover:scale-110 transition">
                                <i class="fa-solid fa-play ml-1"></i>
                            </div>
                        </div>
                    </div>
                    <div class="p-3 flex-1 flex flex-col justify-between">
                        <h3 class="font-bold text-sm text-white line-clamp-1 group-hover:text-brand-red transition">${m.title}</h3>
                        <p class="text-[11px] text-gray-400 mt-1 line-clamp-1">${m.duration}</p>
                    </div>
                </div>
            `).join('');
        }

        // Category Filter Function
        function filterCategory(cat) {
            activeCategory = cat;
            
            // Update active category pill styles
            document.querySelectorAll('.cat-pill').forEach(btn => {
                if (btn.innerText.includes(cat) || (cat === 'all' && btn.innerText === 'ទាំងអស់')) {
                    btn.className = 'cat-pill bg-brand-red text-white px-4 py-1.5 rounded-full whitespace-nowrap';
                } else {
                    btn.className = 'cat-pill bg-brand-card hover:bg-gray-800 text-gray-300 px-4 py-1.5 rounded-full whitespace-nowrap';
                }
            });

            const titleMap = {
                'all': 'រឿងភាគ និងវីដេអូទាំងអស់',
                'វាយប្រហារ': 'រឿងភាគវាយប្រហារ (Action Movies)',
                'កំប្លែង': 'រឿងភាគកំប្លែង (Comedy Movies)',
                'ស្នេហា': 'រឿងភាគស្នេហា (Drama / Romance)',
                'រឿងតុក្កតា': 'រឿងភាគតុក្កតា (Anime)',
                'វីដេអូទូទៅ': 'វីដេអូទូទៅ និងកម្សាន្ត'
            };
            document.getElementById('catalogTitle').innerText = titleMap[cat] || 'រឿងភាគ និងវីដេអូ';

            renderCatalog();
        }

        // Handle Search Input
        function handleSearch() {
            renderCatalog();
        }

        // Open Video Modal
        function openVideoModal(id) {
            const movie = movies.find(m => m.id === id);
            if (!movie) return;

            activeMovie = movie;

            document.getElementById('modalTitle').innerText = movie.title;
            document.getElementById('modalCategory').innerText = movie.category;
            document.getElementById('modalYear').innerText = movie.year;
            document.getElementById('modalDuration').innerText = movie.duration;
            document.getElementById('modalRating').innerText = movie.rating;
            document.getElementById('modalDesc').innerText = movie.description;

            // Embed player (Handle YouTube vs Direct MP4/Video)
            const playerContainer = document.getElementById('playerContainer');
            if (movie.videoUrl.includes('youtube.com') || movie.videoUrl.includes('youtu.be')) {
                playerContainer.innerHTML = `
                    <iframe class="w-full h-full" src="${movie.videoUrl}?autoplay=1" title="${movie.title}" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                `;
            } else {
                playerContainer.innerHTML = `
                    <video class="w-full h-full" controls autoplay>
                        <source src="${movie.videoUrl}" type="video/mp4">
                        កម្មវិធីរុករករបស់អ្នកមិនគាំទ្រការលេងវីដេអូនេះទេ។
                    </video>
                `;
            }

            // Update like state
            updateLikeUI();
            // Render Comments
            renderComments();

            document.getElementById('videoModal').classList.remove('hidden');
            document.body.style.overflow = 'hidden'; // Prevent background scroll
        }

        // Close Video Modal
        function closeVideoModal() {
            document.getElementById('videoModal').classList.add('hidden');
            document.getElementById('playerContainer').innerHTML = ''; // Stop video
            document.body.style.overflow = 'auto';
            activeMovie = null;
        }

        // Like Toggle
        function toggleLike() {
            if (!activeMovie) return;
            if (likedMovies.has(activeMovie.id)) {
                likedMovies.delete(activeMovie.id);
            } else {
                likedMovies.add(activeMovie.id);
            }
            updateLikeUI();
        }

        function updateLikeUI() {
            if (!activeMovie) return;
            const isLiked = likedMovies.has(activeMovie.id);
            const likeIcon = document.getElementById('likeIcon');
            const likeBtn = document.getElementById('likeBtn');
            const count = 128 + (isLiked ? 1 : 0);

            document.getElementById('likeCount').innerText = count;

            if (isLiked) {
                likeIcon.className = 'fa-solid fa-thumbs-up text-brand-red';
                likeBtn.classList.add('border', 'border-brand-red/50');
            } else {
                likeIcon.className = 'fa-regular fa-thumbs-up';
                likeBtn.classList.remove('border', 'border-brand-red/50');
            }
        }

        // Copy Share Link
        function copyCurrentLink() {
            const dummy = document.createElement('input');
            document.body.appendChild(dummy);
            dummy.value = window.location.href;
            dummy.select();
            document.execCommand('copy');
            document.body.removeChild(dummy);
            showToast('បានចម្លងតំណភ្ជាប់វីដេអូរួចរាល់!', 'success');
        }

        // Add Comment
        function addComment() {
            if (!activeMovie) return;
            const input = document.getElementById('commentInput');
            const text = input.value.trim();
            if (!text) return;

            if (!movieComments[activeMovie.id]) {
                movieComments[activeMovie.id] = [];
            }

            movieComments[activeMovie.id].unshift({
                user: isLoggedIn ? 'Admin (ម្ចាស់គេហទំព័រ)' : 'អ្នកទស្សនា',
                text: text,
                time: 'មុននេះបន្តិច'
            });

            input.value = '';
            renderComments();
        }

        function renderComments() {
            if (!activeMovie) return;
            const list = movieComments[activeMovie.id] || [];
            const container = document.getElementById('commentList');
            document.getElementById('commentCount').innerText = list.length;

            if (list.length === 0) {
                container.innerHTML = `<p class="text-xs text-gray-500 italic">មិនទាន់មានមតិយោបល់នៅឡើយទេ។ ក្លាយជាអ្នកដំបូងដែលបញ្ចេញមតិ!</p>`;
                return;
            }

            container.innerHTML = list.map(c => `
                <div class="bg-black/40 p-2.5 rounded-lg border border-gray-800 text-xs space-y-1">
                    <div class="flex items-center justify-between text-gray-400">
                        <span class="font-bold text-gray-200">${c.user}</span>
                        <span class="text-[10px]">${c.time}</span>
                    </div>
                    <p class="text-gray-300">${c.text}</p>
                </div>
            `).join('');
        }

        // Render Admin Data Table
        function renderAdminTable() {
            const tableBody = document.getElementById('adminMovieTable');
            tableBody.innerHTML = movies.map(m => `
                <tr class="hover:bg-gray-800/50 transition">
                    <td class="p-3">
                        <img src="${m.poster}" class="w-10 h-14 object-cover rounded" onerror="this.src='https://placehold.co/100x150/1f1f1f/ffffff?text=No+Img'">
                    </td>
                    <td class="p-3 font-semibold text-white max-w-xs truncate">${m.title}</td>
                    <td class="p-3"><span class="bg-gray-800 text-gray-300 text-[10px] px-2 py-1 rounded">${m.category}</span></td>
                    <td class="p-3 text-yellow-400 font-bold"><i class="fa-solid fa-star text-[10px]"></i> ${m.rating}</td>
                    <td class="p-3 text-right space-x-2">
                        <button onclick="editMovie('${m.id}')" class="bg-blue-600/80 hover:bg-blue-600 text-white px-2.5 py-1 rounded text-xs transition">
                            <i class="fa-solid fa-pen-to-square"></i>
                        </button>
                        <button onclick="deleteMovie('${m.id}')" class="bg-red-600/80 hover:bg-red-600 text-white px-2.5 py-1 rounded text-xs transition">
                            <i class="fa-solid fa-trash"></i>
                        </button>
                    </td>
                </tr>
            `).join('');
        }

        // Form Submit Handler (Add or Edit)
        function handleFormSubmit(e) {
            e.preventDefault();

            const id = document.getElementById('movieId').value;
            const title = document.getElementById('inputTitle').value;
            const category = document.getElementById('inputCategory').value;
            const rating = parseFloat(document.getElementById('inputRating').value);
            const year = document.getElementById('inputYear').value;
            const duration = document.getElementById('inputDuration').value;
            const poster = document.getElementById('inputPoster').value;
            const videoUrl = document.getElementById('inputVideoUrl').value;
            const description = document.getElementById('inputDesc').value;

            if (id) {
                // Edit Existing Movie
                const index = movies.findIndex(m => m.id === id);
                if (index !== -1) {
                    movies[index] = { id, title, category, rating, year, duration, poster, videoUrl, description };
                    showToast('បានធ្វើបច្ចុប្បន្នភាពវីដេអូដោយជោគជ័យ!', 'success');
                }
            } else {
                // Add New Movie
                const newMovie = {
                    id: Date.now().toString(),
                    title, category, rating, year, duration, poster, videoUrl, description
                };
                movies.unshift(newMovie);
                showToast('បានបន្ថែមវីដេអូថ្មីដោយជោគជ័យ!', 'success');
            }

            saveMoviesToStorage();
            resetForm();
            renderAdminTable();
            renderCatalog();
            renderHero();
        }

        // Edit Movie
        function editMovie(id) {
            const m = movies.find(item => item.id === id);
            if (!m) return;

            document.getElementById('movieId').value = m.id;
            document.getElementById('inputTitle').value = m.title;
            document.getElementById('inputCategory').value = m.category;
            document.getElementById('inputRating').value = m.rating;
            document.getElementById('inputYear').value = m.year;
            document.getElementById('inputDuration').value = m.duration;
            document.getElementById('inputPoster').value = m.poster;
            document.getElementById('inputVideoUrl').value = m.videoUrl;
            document.getElementById('inputDesc').value = m.description;

            document.getElementById('formTitle').innerHTML = `<i class="fa-solid fa-pen-to-square text-yellow-500"></i> កែប្រែវីដេអូ`;
            document.getElementById('submitBtn').innerHTML = `<i class="fa-solid fa-save mr-1"></i> កែប្រែទិន្នន័យ`;
            document.getElementById('cancelBtn').classList.remove('hidden');

            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        // Reset Add/Edit Form
        function resetForm() {
            document.getElementById('movieForm').reset();
            document.getElementById('movieId').value = '';
            document.getElementById('formTitle').innerHTML = `<i class="fa-solid fa-plus-circle text-brand-red"></i> បន្ថែមវីដេអូថ្មី`;
            document.getElementById('submitBtn').innerHTML = `<i class="fa-solid fa-save mr-1"></i> រក្សាទុកវីដេអូ`;
            document.getElementById('cancelBtn').classList.add('hidden');
        }

        // Delete Movie
        function deleteMovie(id) {
            if (confirm('តើអ្នកពិតជាចង់លុបវីដេអូនេះចេញពីប្រព័ន្ធមែនទេ?')) {
                movies = movies.filter(m => m.id !== id);
                saveMoviesToStorage();
                renderAdminTable();
                renderCatalog();
                renderHero();
                showToast('បានលុបវីដេអូចេញរួចរាល់', 'info');
            }
        }

        // Reset to Default Demo Data
        function resetToDefaultData() {
            if (confirm('តើអ្នកចង់កំណត់ទិន្នន័យវីដេអូទាំងអស់ទៅជា Demo ដើមវិញមែនទេ?')) {
                movies = [...defaultMovies];
                saveMoviesToStorage();
                renderAdminTable();
                renderCatalog();
                renderHero();
                showToast('បានកំណត់ទិន្នន័យដើមឡើងវិញរួចរាល់!', 'success');
            }
        }

        // Open & Close Login Modal
        function openLoginModal() {
            document.getElementById('loginModal').classList.remove('hidden');
        }

        function closeLoginModal() {
            document.getElementById('loginModal').classList.add('hidden');
        }

        // Show Custom Toast Alert
        function showToast(msg, type = 'info') {
            const toast = document.getElementById('toast');
            const toastMessage = document.getElementById('toastMessage');
            const toastIcon = document.getElementById('toastIcon');

            toastMessage.innerText = msg;

            if (type === 'success') {
                toastIcon.className = 'fa-solid fa-circle-check text-green-500 text-xl';
                toast.style.borderColor = '#22c55e';
            } else if (type === 'error') {
                toastIcon.className = 'fa-solid fa-circle-exclamation text-red-500 text-xl';
                toast.style.borderColor = '#ef4444';
            } else {
                toastIcon.className = 'fa-solid fa-circle-info text-blue-500 text-xl';
                toast.style.borderColor = '#3b82f6';
            }

            toast.classList.remove('translate-y-20', 'opacity-0');
            toast.classList.add('translate-y-0', 'opacity-100');

            setTimeout(() => {
                toast.classList.remove('translate-y-0', 'opacity-100');
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }
    </script>
</body>
</html>
