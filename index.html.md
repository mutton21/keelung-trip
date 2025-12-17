<!DOCTYPE html>  
<html lang="zh-TW">  
<head>  
    <meta charset="UTF-8">  
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">  
    <title>Keelung Adventure - Alice in Snowland</title>  
    <script src="https://cdn.tailwindcss.com"></script>  
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>  
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;700&family=Pacifico&display=swap" rel="stylesheet">  
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>  
    <style>  
        body { font-family: 'Noto Sans TC', sans-serif; background: #e0f2fe; }  
        .alice-font { font-family: 'Pacifico', cursive; }  
        /* 冰雪毛玻璃效果 */  
        .snow-glass {  
            background: rgba(255, 255, 255, 0.7);  
            backdrop-filter: blur(10px);  
            border: 1px solid rgba(255, 255, 255, 0.4);  
        }  
        .timeline-line {  
            background: linear-gradient(to bottom, #7dd3fc, #3b82f6, #7dd3fc);  
        }  
        /* 隱藏滾動條 */  
        ::-webkit-scrollbar { display: none; }  
    </style>  
</head>  
<body class="bg-[#f0f9ff]">  
  
<div id="app" class="max-w-md mx-auto min-h-screen relative shadow-2xl overflow-hidden bg-alice-pattern">  
      
    <header class="relative h-72 rounded-b-[3rem] overflow-hidden shadow-lg animate__animated animate__fadeIn">  
        <img src="https://images.unsplash.com/photo-1476820865390-c52aeebb9891?q=80&w=1000" class="w-full h-full object-cover shadow-inner" alt="Winter Keelung">  
        <div class="absolute inset-0 bg-gradient-to-t from-[#0c4a6e]/80 to-transparent"></div>  
        <div class="absolute bottom-10 left-8 text-white">  
            <h2 class="alice-font text-2xl text-sky-200 mb-1 tracking-widest">Alice's Journey</h2>  
            <h1 class="text-3xl font-bold tracking-tight">基隆 · 冰雪冒險</h1>  
            <div class="flex items-center mt-3 bg-white/20 backdrop-blur-md rounded-full px-4 py-1 text-sm border border-white/30">  
                <span class="mr-2">❄️</span> 18°C · 基隆市  
            </div>  
        </div>  
    </header>  
  
    <main class="px-6 py-10 relative">  
        <div class="absolute left-[2.4rem] top-12 bottom-20 w-1 timeline-line rounded-full opacity-30"></div>  
  
        <div class="space-y-10">  
            <div v-for="(item, index) in itinerary" :key="index"   
                 class="flex gap-4 relative animate__animated animate__fadeInUp" :style="{ animationDelay: index * 0.1 + 's' }">  
                  
                <div class="z-10 w-12 h-12 rounded-full snow-glass flex items-center justify-center shadow-md border-2 border-sky-300 transform hover:scale-110 transition-transform">  
                    <span class="text-2xl">{{ item.icon }}</span>  
                </div>  
  
                <div class="flex-1 snow-glass rounded-[2rem] p-5 shadow-sm border border-white active:scale-95 transition-all">  
                    <div class="flex justify-between items-start mb-2">  
                        <span class="bg-sky-500 text-white text-xs px-3 py-1 rounded-full font-bold shadow-sm">{{ item.time }}</span>  
                        <button @click="showInfo(item)" class="text-sky-400 hover:rotate-12 transition-transform">  
                            🪄  
                        </button>  
                    </div>  
                    <h3 class="font-bold text-lg text-sky-900 mb-1">{{ item.activity }}</h3>  
                    <p class="text-xs text-sky-700/70 mb-4 leading-relaxed italic">{{ item.desc }}</p>  
                      
                    <a :href="'https://www.google.com/maps/dir/?api=1&destination=' + encodeURIComponent(item.location)"   
                       target="_blank"  
                       class="flex items-center justify-center gap-2 w-full py-2.5 bg-gradient-to-r from-sky-400 to-blue-500 text-white rounded-2xl font-bold text-sm shadow-md hover:shadow-sky-200 transition-shadow">  
                        <span class="text-xs">🗺️</span> 開啟導航  
                    </a>  
                </div>  
            </div>  
        </div>  
    </main>  
  
    <nav class="fixed bottom-6 left-1/2 -translate-x-1/2 w-[90%] max-w-md snow-glass rounded-[2.5rem] py-4 px-10 flex justify-between items-center shadow-2xl border border-white/50 z-50">  
        <button class="flex flex-col items-center gap-1 text-sky-600 scale-110">  
            <span class="text-xl">🍄</span>  
            <span class="text-[10px] font-bold">行程</span>  
        </button>  
        <div class="h-14 w-14 bg-gradient-to-tr from-sky-400 to-blue-600 rounded-full flex items-center justify-center text-white shadow-xl -mt-12 border-4 border-[#f0f9ff] animate-bounce">  
            <span class="text-2xl">🐇</span>  
        </div>  
        <button class="flex flex-col items-center gap-1 text-slate-400">  
            <span class="text-xl">🃏</span>  
            <span class="text-[10px] font-bold">地圖</span>  
        </button>  
    </nav>  
  
    <div v-if="selectedItem" class="fixed inset-0 z-[100] flex items-center justify-center p-6 bg-sky-900/40 backdrop-blur-sm" @click.self="selectedItem = null">  
        <div class="bg-white rounded-[3rem] p-8 max-w-sm w-full shadow-2xl animate__animated animate__zoomIn relative overflow-hidden">  
            <div class="absolute top-0 right-0 p-4 text-2xl alice-font text-sky-100 opacity-20 underline">Alice</div>  
            <h2 class="text-2xl font-bold text-sky-800 mb-4 flex items-center gap-2">  
                {{ selectedItem.icon }} 詳細資訊  
            </h2>  
            <div class="space-y-4 text-slate-600">  
                <div class="bg-sky-50 p-4 rounded-2xl border border-sky-100">  
                    <p class="text-xs text-sky-400 font-bold mb-1">地點小指南</p>  
                    <p class="text-sm">{{ selectedItem.note || '帶著冒險的心出發吧！' }}</p>  
                </div>  
                <div class="flex justify-between items-center">  
                    <span class="text-xs text-slate-400 italic">行程時間: {{ selectedItem.time }}</span>  
                    <button @click="selectedItem = null" class="bg-slate-800 text-white px-6 py-2 rounded-full text-sm font-bold">關閉</button>  
                </div>  
            </div>  
        </div>  
    </div>  
</div>  
  
<script>  
    const { createApp, ref } = Vue;  
    createApp({  
        setup() {  
            const selectedItem = ref(null);  
            const itinerary = ref([  
                { time: "10:00", activity: "和平島地質公園", icon: "🌅", desc: "穿過樹洞，看見世界級的海蝕地景。", location: "和平島地質公園", note: "門票可折抵消費，記得拍等嶼亭。" },  
                { time: "12:30", activity: "正濱漁港彩色屋", icon: "📸", desc: "瘋帽子最愛的彩色房屋，在這享用午餐。", location: "正濱漁港彩色屋", note: "推薦吉古拉與海藻拿鐵。" },  
                { time: "14:15", activity: "海洋廣場休息", icon: "☕", desc: "在藍調港口邊，來杯紅心皇后的咖啡。", location: "海洋廣場", note: "可以看大郵輪進港喔！" },  
                { time: "15:30", activity: "外木山海岸步道", icon: "🌊", desc: "沿著雪白浪花散步，享受冬日海風。", location: "外木山濱海風景區", note: "步道平緩好走，約停留 40 分鐘。" },  
                { time: "16:30", activity: "鯊到基隆互動區", icon: "✨", desc: "光影魔法降臨，巨大的鯊魚在港邊現身。", location: "基隆港東岸", note: "互動展覽 16:30-18:30 最美。" },  
                { time: "18:30", activity: "基隆廟口夜市", icon: "🍢", desc: "愛麗絲的盛宴！結束這奇幻的一天。", location: "基隆廟口夜市", note: "營養三明治與螃蟹羹必吃。" }  
            ]);  
  
            const showInfo = (item) => {  
                selectedItem.ref = item; // 修正 Vue ref 指派方式  
                selectedItem.value = item;  
            };  
  
            return { itinerary, selectedItem, showInfo };  
        }  
    }).mount('#app');  
</script>  
</body>  
</html>  
