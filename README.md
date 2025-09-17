<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Planet Zoo Planner</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .scrollbar-thin::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        .scrollbar-thin::-webkit-scrollbar-track {
            background: #f1f1f1;
            border-radius: 10px;
        }
        .scrollbar-thin::-webkit-scrollbar-thumb {
            background: #888;
            border-radius: 10px;
        }
        .scrollbar-thin::-webkit-scrollbar-thumb:hover {
            background: #555;
        }
        .fade-in {
            animation: fadeIn 0.5s ease-in-out;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .status-badge {
            display: inline-flex;
            align-items: center;
            padding: 0.25em 0.6em;
            font-size: 0.75rem;
            font-weight: 500;
            line-height: 1;
            text-align: center;
            white-space: nowrap;
            vertical-align: baseline;
            border-radius: 9999px;
            color: white;
        }
        .mod-badge {
            background-color: #6366f1; /* Indigo-500 */
        }
        .tab-btn {
            padding: 0.75rem 1.5rem;
            border-radius: 0.5rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease-in-out;
            border: 2px solid transparent;
        }
        .tab-btn.active {
            background-color: white;
            color: #166534; /* Green-800 */
            border-color: #d1d5db; /* Gray-300 */
            box-shadow: 0 1px 3px 0 rgba(0, 0, 0, 0.1), 0 1px 2px 0 rgba(0, 0, 0, 0.06);
        }
        .tab-btn:not(.active) {
            background-color: transparent;
            color: #374151; /* Gray-700 */
        }
    </style>
</head>
<body class="bg-gray-100 text-gray-800">

    <div class="container mx-auto p-4 md:p-8">
        <header class="text-center mb-6">
            <h1 class="text-4xl md:text-5xl font-bold text-green-800">Ultimate Planet Zoo Planner</h1>
            <p class="text-green-600 mt-2 text-lg">Plan your habitats and exhibits all in one place.</p>
        </header>

        <!-- Tab Navigation -->
        <div class="mb-6 bg-gray-200 p-1 rounded-lg flex justify-center max-w-md mx-auto">
            <button id="habitat-tab-btn" class="tab-btn active w-1/2">Habitat Planner</button>
            <button id="exhibit-tab-btn" class="tab-btn w-1/2">Exhibit Planner</button>
        </div>

        <!-- Planner Content -->
        <div id="planner-content">
            <!-- Habitat Planner -->
            <div id="habitat-planner">
                <div class="flex flex-col lg:flex-row gap-8">
                    <!-- Left Panel: Controls -->
                    <aside class="lg:w-1/3 w-full bg-white p-6 rounded-2xl shadow-lg">
                        <h2 class="text-2xl font-bold mb-4 text-gray-700">Build Your Habitat</h2>
                        
                        <!-- Habitat Filters -->
                        <div class="space-y-3 mb-4 p-4 bg-gray-50 rounded-lg border">
                            <h3 class="font-semibold text-gray-600 mb-2">Filters</h3>
                            <input type="text" id="habitat-search" placeholder="Search by name..." class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500">
                            <select id="continent-filter" class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"></select>
                            <select id="biome-filter" class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"></select>
                            <select id="status-filter" class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"></select>
                            <div class="flex items-center pt-2">
                                <input type="checkbox" id="mod-filter-toggle" class="h-4 w-4 rounded border-gray-300 text-green-600 focus:ring-green-500" checked>
                                <label for="mod-filter-toggle" class="ml-2 block text-sm text-gray-900">Show Modded Animals</label>
                            </div>
                        </div>

                        <div class="space-y-4">
                            <div>
                                <label for="animal-select" class="block text-sm font-medium text-gray-600 mb-1">Select Animal</label>
                                <select id="animal-select" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 transition">
                                    <!-- Options populated by JS -->
                                </select>
                            </div>
                            <button id="add-animal-btn" class="w-full bg-green-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-green-700 transition-colors duration-300 shadow active:scale-95">
                                Add to Habitat
                            </button>
                            <button id="reset-habitat-btn" class="w-full bg-gray-200 text-gray-700 font-bold py-3 px-4 rounded-lg hover:bg-gray-300 transition-colors duration-300">
                                Clear Habitat
                            </button>
                        </div>
                        
                        <hr class="my-6 border-gray-200">

                        <h3 class="text-xl font-bold mb-3 text-gray-700">Selected Animals</h3>
                        <div id="selected-animals-list" class="space-y-3 max-h-[32rem] overflow-y-auto scrollbar-thin pr-2">
                            <p id="habitat-empty-state" class="text-gray-500">No animals added yet.</p>
                        </div>
                    </aside>

                    <!-- Right Panel: Results -->
                    <main class="lg:w-2/3 w-full">
                        <div class="bg-white p-6 rounded-2xl shadow-lg min-h-[500px]">
                            <div id="habitat-results-placeholder" class="flex flex-col items-center justify-center h-full text-center">
                                <svg xmlns="http://www.w3.org/2000/svg" width="80" height="80" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1" stroke-linecap="round" stroke-linejoin="round" class="text-green-300 mb-4"><path d="M21.92 14.74a1 1 0 0 0-.2-1.06l-2.6-2.97a1 1 0 0 0-1.12-.22l-2.98 1.3a1 1 0 0 0-.58 1.12l1.3 2.98a1 1 0 0 0 1.12.58l2.97-1.3a1 1 0 0 0 1.05-.21z"/><path d="m11 11-1.3 2.98a1 1 0 0 0 .58 1.12l2.97 1.3a1 1 0 0 0 1.12-.58L17 12.86"/><path d="m14 14 2.24.96"/><path d="m16.5 11.5-2.52 5.76"/><path d="M4.26 10.26a1 1 0 0 0-1.06.2l-1.3 2.98a1 1 0 0 0 .58 1.12l2.97 1.3a1 1 0 0 0 1.12-.58l1.3-2.98a1 1 0 0 0-.22-1.12l-2.97-1.3a1 1 0 0 0-.42 0z"/><path d="m10 10-2.98 1.3a1 1 0 0 0-.58 1.12L7.7 15.4a1 1 0 0 0 1.12.58L11.8 14"/><path d="M12 22v-2"/><path d="m7 20-2-1"/><path d="m17 20 2-1"/><path d="M4 14H2"/><path d="M3 9V7a2 2 0 0 1 2-2h14a2 2 0 0 1 2 2v2"/><path d="M12 5V2"/><path d="M8 5V2"/><path d="M16 5V2"/></svg>
                                <h2 class="text-2xl font-bold text-gray-600">Your Habitat Awaits</h2>
                                <p class="text-gray-500 mt-2 max-w-md">Add animals to see their combined needs for a perfect enclosure.</p>
                            </div>
                            <div id="habitat-results-content" class="hidden"></div>
                        </div>
                    </main>
                </div>
            </div>

            <!-- Exhibit Planner -->
            <div id="exhibit-planner" class="hidden">
                 <div class="flex flex-col lg:flex-row gap-8">
                    <!-- Left Panel: Controls -->
                    <aside class="lg:w-1/3 w-full bg-white p-6 rounded-2xl shadow-lg">
                        <h2 class="text-2xl font-bold mb-4 text-gray-700">Plan Your Exhibits</h2>

                        <!-- Exhibit Filters -->
                        <div class="space-y-3 mb-4 p-4 bg-gray-50 rounded-lg border">
                            <h3 class="font-semibold text-gray-600 mb-2">Filters</h3>
                            <input type="text" id="exhibit-search" placeholder="Search by name..." class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500">
                            <select id="region-filter" class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"></select>
                            <select id="type-filter" class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"></select>
                            <select id="exhibit-status-filter" class="w-full p-2 border border-gray-300 rounded-md focus:ring-2 focus:ring-green-500"></select>
                             <div class="flex items-center pt-2">
                                <input type="checkbox" id="exhibit-mod-filter-toggle" class="h-4 w-4 rounded border-gray-300 text-green-600 focus:ring-green-500" checked>
                                <label for="exhibit-mod-filter-toggle" class="ml-2 block text-sm text-gray-900">Show Modded Animals</label>
                            </div>
                        </div>
                        
                        <div class="space-y-4">
                            <div>
                                <label for="exhibit-animal-select" class="block text-sm font-medium text-gray-600 mb-1">Select Exhibit Animal</label>
                                <select id="exhibit-animal-select" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-green-500 transition"></select>
                            </div>
                            <button id="add-exhibit-animal-btn" class="w-full bg-green-600 text-white font-bold py-3 px-4 rounded-lg hover:bg-green-700 transition-colors duration-300 shadow active:scale-95">
                                Add to Plan
                            </button>
                            <button id="reset-exhibit-btn" class="w-full bg-gray-200 text-gray-700 font-bold py-3 px-4 rounded-lg hover:bg-gray-300 transition-colors duration-300">
                                Clear Plan
                            </button>
                        </div>
                    </aside>

                    <!-- Right Panel: Results -->
                    <main class="lg:w-2/3 w-full">
                        <div class="bg-white p-6 rounded-2xl shadow-lg min-h-[500px]">
                             <div id="exhibit-results-placeholder" class="flex flex-col items-center justify-center h-full text-center">
                                <svg xmlns="http://www.w3.org/2000/svg" width="80" height="80" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1" stroke-linecap="round" stroke-linejoin="round" class="text-green-300 mb-4"><rect x="3" y="3" width="18" height="18" rx="2"/><path d="M3 9h18"/><path d="M9 21V9"/></svg>
                                <h2 class="text-2xl font-bold text-gray-600">Your Exhibit Hall</h2>
                                <p class="text-gray-500 mt-2 max-w-md">Add small animals to your plan to keep track of your exhibits.</p>
                            </div>
                            <div id="exhibit-results-content" class="hidden">
                                <h2 class="text-3xl font-bold text-green-700 mb-4">Exhibit Plan Summary</h2>
                                <p id="exhibit-summary" class="text-lg text-gray-600 mb-6"></p>
                                <div id="selected-exhibits-list" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                    <!-- Exhibit cards go here -->
                                </div>
                            </div>
                        </div>
                    </main>
                </div>
            </div>
        </div>
    </div>

    <script>
    // --- DATA START ---
    const animalData = [
        // --- Official Animal Data ---
        { name: "Aardvark", land: 330, landPer: 60, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false },
        { name: "Addax", land: 370, landPer: 45, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 3, tempMax: 45, continents: ["Africa"], biomes: ["Desert"], maxM: 1, maxF: 24, group: "2-25", status: "CR", predator: false },
        { name: "African Buffalo", land: 440, landPer: 100, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 16, tempMax: 40, continents: ["Africa"], biomes: ["Aquatic", "Grassland", "Tropical"], maxM: 1, maxF: 14, group: "3-15", status: "NT", predator: false },
        { name: "African Crested Porcupine", land: 310, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: 10, tempMax: 44, continents: ["Africa", "Europe"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 5, maxF: 5, group: "1-6", status: "LC", predator: false },
        { name: "African Leopard", land: 750, landPer: 125, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 3, barrierHeight: 3, tempMin: 10, tempMax: 44, continents: ["Africa"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true },
        { name: "African Penguin", land: 180, landPer: 5, water: 135, waterPer: 2, deepWater: 67, deepWaterPer: 2, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 4, tempMax: 38, continents: ["Africa"], biomes: ["Aquatic", "Desert"], maxM: 500, maxF: 500, group: "6-500", status: "EN", predator: false },
        { name: "African Savannah Elephant", land: 2151, landPer: 667, water: 148, waterPer: 74, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 0.5, tempMin: 18, tempMax: 42, continents: ["Africa"], biomes: ["Grassland", "Desert"], maxM: 1, maxF: 14, group: "3-15", status: "EN", predator: false },
        { name: "African Spurred Tortoise", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: 10, tempMax: 44, continents: ["Africa"], biomes: ["Grassland"], maxM: 4, maxF: 4, group: "1-8", status: "EN", predator: false },
        { name: "African Wild Dog", land: 1162, landPer: 142, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: 14, tempMax: 44, continents: ["Africa"], biomes: ["Grassland", "Desert"], maxM: 12, maxF: 12, group: "2-12", status: "EN", predator: true },
        { name: "Aldabra Giant Tortoise", land: 310, landPer: 55, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: 22, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 4, maxF: 4, group: "1-8", status: "VU", predator: false },
        { name: "Alpaca", land: 290, landPer: 20, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -15, tempMax: 35, continents: ["S. America"], biomes: ["Temperate", "Grassland", "Tundra", "Taiga"], maxM: 1, maxF: 9, group: "2-10", status: "LC", predator: false },
        { name: "Alpine Goat", land: 290, landPer: 20, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 1.25, tempMin: -7, tempMax: 38, continents: ["Africa", "Asia", "Europe"], biomes: ["Temperate", "Grassland", "Tundra", "Taiga"], maxM: 1, maxF: 9, group: "2-10", status: "LC", predator: false },
        { name: "Alpine Ibex", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 1.75, tempMin: -7, tempMax: 38, continents: ["Europe"], biomes: ["Tundra", "Taiga"], maxM: 20, maxF: 20, group: "3-20", status: "LC", predator: false },
        { name: "American Alligator", land: 160, landPer: 40, water: 230, waterPer: 60, deepWater: 115, deepWaterPer: 30, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 0.5, tempMin: 4, tempMax: 40, continents: ["N. America"], biomes: ["Aquatic", "Temperate"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "American Bison", land: 630, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -15, tempMax: 35, continents: ["N. America"], biomes: ["Grassland", "Temperate", "Taiga"], maxM: 1, maxF: 14, group: "3-15", status: "NT", predator: false },
        { name: "American Flamingo", land: 402, landPer: 13, water: 211, waterPer: 8, deepWater: 0, deepWaterPer: 0, deepWaterDepth: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1, tempMin: 4, tempMax: 40, continents: ["N. America", "C. America", "S. America"], biomes: ["Temperate", "Grassland", "Tropical"], maxM: 500, maxF: 500, group: "5-500", status: "LC", predator: false },
        { name: "American Standard Donkey", land: 330, landPer: 60, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 3, tempMax: 45, continents: ["Africa"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 1, maxF: 9, group: "2-10", status: "LC", predator: false },
        { name: "Amur Leopard", land: 750, landPer: 125, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 3, barrierHeight: 3, tempMin: -25, tempMax: 25, continents: ["Asia"], biomes: ["Temperate", "Taiga", "Tundra"], maxM: 1, maxF: 1, group: "1-2", status: "CR", predator: true },
        { name: "Arctic Fox", land: 370, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: -40, tempMax: 22, continents: ["N. America", "Europe", "Asia"], biomes: ["Tundra", "Taiga"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Arctic Wolf", land: 1254, landPer: 92, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: -40, tempMax: 22, continents: ["N. America", "Europe", "Asia"], biomes: ["Tundra", "Taiga"], maxM: 12, maxF: 12, group: "2-12", status: "LC", predator: true },
        { name: "Asian Small-Clawed Otter", land: 160, landPer: 20, water: 130, waterPer: 25, deepWater: 65, deepWaterPer: 12, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 10, tempMax: 40, continents: ["Asia"], biomes: ["Aquatic", "Temperate", "Tropical"], maxM: 9, maxF: 9, group: "2-18", status: "VU", predator: false },
        { name: "Asian Water Monitor", land: 160, landPer: 20, water: 70, waterPer: 10, deepWater: 35, deepWaterPer: 5, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 16, tempMax: 44, continents: ["Asia"], biomes: ["Aquatic", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Bactrian Camel", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -20, tempMax: 40, continents: ["Asia"], biomes: ["Temperate", "Desert"], maxM: 1, maxF: 12, group: "3-13", status: "CR", predator: false },
        { name: "Baird's Tapir", land: 390, landPer: 60, water: 45, waterPer: 15, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 18, tempMax: 42, continents: ["C. America", "S. America"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: false },
        { name: "Bengal Tiger", land: 1162, landPer: 142, water: 95, waterPer: 30, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 4, tempMin: 10, tempMax: 42, continents: ["Asia"], biomes: ["Temperate", "Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: true },
        { name: "Bighorn Sheep", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 1.75, tempMin: -20, tempMax: 35, continents: ["N. America"], biomes: ["Taiga", "Temperate", "Desert"], maxM: 20, maxF: 20, group: "3-20", status: "LC", predator: false },
        { name: "Binturong", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 1.75, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: false },
        { name: "Black Rhinoceros", land: 835, landPer: 125, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 0.5, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "CR", predator: false },
        { name: "Black Wildebeest", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 10, tempMax: 42, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 29, group: "3-30", status: "LC", predator: false },
        { name: "Black-and-white Ruffed Lemur", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 10, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 3, maxF: 3, group: "2-6", status: "CR", predator: false },
        { name: "Black-tailed Prairie Dog", land: 260, landPer: 25, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: -15, tempMax: 35, continents: ["N. America"], biomes: ["Grassland"], maxM: 2, maxF: 4, group: "2-6", status: "LC", predator: false },
        { name: "Blue Wildebeest", land: 640, landPer: 100, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 10, tempMax: 42, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 29, group: "3-30", status: "LC", predator: false },
        { name: "Bongo", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 17, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 1, maxF: 19, group: "3-20", status: "NT", predator: false },
        { name: "Bonobo", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 260, climbPer: 55, barrierGrade: 3, barrierHeight: 3, tempMin: 17, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 10, maxF: 10, group: "5-20", status: "EN", predator: false },
        { name: "Bornean Orangutan", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 230, climbPer: 45, barrierGrade: 4, barrierHeight: 3, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "CR", predator: false },
        { name: "Bush Dog", land: 260, landPer: 40, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 16, tempMax: 42, continents: ["C. America", "S. America"], biomes: ["Grassland", "Tropical"], maxM: 5, maxF: 5, group: "2-10", status: "NT", predator: true },
        { name: "California Sea Lion", land: 260, landPer: 25, water: 340, waterPer: 45, deepWater: 170, deepWaterPer: 22, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 4, tempMax: 35, continents: ["N. America"], biomes: ["Aquatic", "Temperate"], maxM: 1, maxF: 11, group: "3-12", status: "LC", predator: false },
        { name: "Capybara", land: 220, landPer: 20, water: 90, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1, tempMin: 16, tempMax: 42, continents: ["S. America"], biomes: ["Aquatic", "Grassland", "Tropical"], maxM: 1, maxF: 14, group: "3-15", status: "LC", predator: false },
        { name: "Caracal", land: 370, landPer: 55, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 2, tempMin: 10, tempMax: 44, continents: ["Africa", "Asia"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Cheetah", land: 1254, landPer: 158, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 3, tempMin: 14, tempMax: 44, continents: ["Africa"], biomes: ["Grassland"], maxM: 5, maxF: 1, group: "1-6", status: "VU", predator: true },
        { name: "Chinese Pangolin", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 10, tempMax: 40, continents: ["Asia"], biomes: ["Temperate", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "CR", predator: false },
        { name: "Clouded Leopard", land: 370, landPer: 55, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 3, barrierHeight: 3, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true },
        { name: "Collared Peccary", land: 330, landPer: 60, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: -15, tempMax: 35, continents: ["N. America", "C. America", "S. America"], biomes: ["Temperate", "Grassland", "Tropical", "Desert"], maxM: 1, maxF: 14, group: "3-15", status: "LC", predator: false },
        { name: "Colombian White-faced Capuchin", land: 330, landPer: 60, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2.5, tempMin: 18, tempMax: 42, continents: ["C. America", "S. America"], biomes: ["Tropical"], maxM: 20, maxF: 20, group: "5-40", status: "VU", predator: false },
        { name: "Common Ostrich", land: 700, landPer: 110, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 14, tempMax: 44, continents: ["Africa"], biomes: ["Grassland", "Desert"], maxM: 1, maxF: 7, group: "2-8", status: "LC", predator: false },
        { name: "Common Warthog", land: 330, landPer: 60, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 6, group: "2-7", status: "LC", predator: false },
        { name: "Common Wombat", land: 310, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: -10, tempMax: 35, continents: ["Oceania"], biomes: ["Temperate", "Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false },
        { name: "Coquerel's Sifaka", land: 310, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 10, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 4, maxF: 4, group: "2-8", status: "EN", predator: false },
        { name: "Cougar", land: 705, landPer: 110, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 3, barrierHeight: 3, tempMin: -20, tempMax: 35, continents: ["N. America", "S. America"], biomes: ["Tundra", "Taiga", "Temperate", "Grassland", "Tropical", "Desert"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Coyote", land: 1162, landPer: 92, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: -20, tempMax: 35, continents: ["N. America", "C. America"], biomes: ["Taiga", "Temperate", "Grassland", "Desert"], maxM: 6, maxF: 6, group: "2-6", status: "LC", predator: true },
        { name: "Cuvier's Dwarf Caiman", land: 160, landPer: 20, water: 101, waterPer: 14, deepWater: 50, deepWaterPer: 7, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 10, tempMax: 42, continents: ["S. America"], biomes: ["Aquatic", "Temperate", "Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Dall Sheep", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 1.75, tempMin: -30, tempMax: 22, continents: ["N. America"], biomes: ["Tundra", "Taiga"], maxM: 20, maxF: 20, group: "3-20", status: "LC", predator: false },
        { name: "Dama Gazelle", land: 370, landPer: 45, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 3, tempMax: 45, continents: ["Africa"], biomes: ["Desert"], maxM: 1, maxF: 14, group: "3-15", status: "CR", predator: false },
        { name: "Dhole", land: 1162, landPer: 92, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: -25, tempMax: 30, continents: ["Asia"], biomes: ["Tundra", "Taiga", "Temperate", "Grassland", "Tropical"], maxM: 12, maxF: 12, group: "2-12", status: "EN", predator: true },
        { name: "Dingo", land: 1162, landPer: 92, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: -10, tempMax: 44, continents: ["Oceania"], biomes: ["Temperate", "Grassland", "Tropical", "Desert"], maxM: 12, maxF: 12, group: "2-12", status: "VU", predator: true },
        { name: "Dromedary Camel", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 3, tempMax: 45, continents: ["Africa", "Asia"], biomes: ["Desert"], maxM: 1, maxF: 19, group: "3-20", status: "LC", predator: false },
        { name: "Emu", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -10, tempMax: 44, continents: ["Oceania"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false },
        { name: "Eurasian Lynx", land: 705, landPer: 110, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 3, tempMin: -25, tempMax: 30, continents: ["Asia", "Europe"], biomes: ["Tundra", "Taiga", "Temperate"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "European Badger", land: 310, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: -7, tempMax: 38, continents: ["Asia", "Europe"], biomes: ["Taiga", "Temperate", "Grassland"], maxM: 6, maxF: 6, group: "2-12", status: "LC", predator: false },
        { name: "European Fallow Deer", land: 370, landPer: 45, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -7, tempMax: 38, continents: ["Europe"], biomes: ["Temperate", "Grassland"], maxM: 1, maxF: 29, group: "3-30", status: "LC", predator: false },
        { name: "Fennec Fox", land: 260, landPer: 25, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1, tempMin: 16, tempMax: 44, continents: ["Africa"], biomes: ["Desert"], maxM: 2, maxF: 2, group: "2-4", status: "LC", predator: false },
        { name: "Formosan Black Bear", land: 976, landPer: 170, water: 45, waterPer: 15, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 2.5, tempMin: 0, tempMax: 35, continents: ["Asia"], biomes: ["Taiga", "Temperate", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: true },
        { name: "Fossa", land: 310, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 10, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true },
        { name: "Galapagos Giant Tortoise", land: 310, landPer: 55, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: 22, tempMax: 42, continents: ["S. America"], biomes: ["Tropical"], maxM: 4, maxF: 4, group: "1-8", status: "VU", predator: false },
        { name: "Gemsbok", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 14, tempMax: 45, continents: ["Africa"], biomes: ["Grassland", "Desert"], maxM: 1, maxF: 14, group: "3-15", status: "LC", predator: false },
        { name: "Gharial", land: 130, landPer: 25, water: 260, waterPer: 55, deepWater: 130, deepWaterPer: 28, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 10, tempMax: 42, continents: ["Asia"], biomes: ["Aquatic", "Tropical"], maxM: 1, maxF: 5, group: "1-6", status: "CR", predator: true },
        { name: "Giant Anteater", land: 370, landPer: 70, water: 45, waterPer: 15, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 16, tempMax: 42, continents: ["C. America", "S. America"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: false },
        { name: "Giant Otter", land: 260, landPer: 40, water: 340, waterPer: 56, deepWater: 170, deepWaterPer: 28, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.75, tempMin: 16, tempMax: 42, continents: ["S. America"], biomes: ["Aquatic", "Grassland", "Tropical"], maxM: 9, maxF: 9, group: "2-18", status: "EN", predator: false },
        { name: "Giant Panda", land: 835, landPer: 125, water: 0, waterPer: 0, climb: 85, climbPer: 25, barrierGrade: 3, barrierHeight: 1.25, tempMin: 0, tempMax: 28, continents: ["Asia"], biomes: ["Temperate"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: false },
        { name: "Gray Seal", land: 260, landPer: 25, water: 340, waterPer: 45, deepWater: 170, deepWaterPer: 22, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: -15, tempMax: 28, continents: ["N. America", "Europe"], biomes: ["Aquatic", "Tundra", "Taiga", "Temperate"], maxM: 1, maxF: 11, group: "3-12", status: "LC", predator: false },
        { name: "Greater Flamingo", land: 402, landPer: 13, water: 211, waterPer: 8, deepWater: 0, deepWaterPer: 0, deepWaterDepth: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1, tempMin: 4, tempMax: 42, continents: ["Africa", "Asia", "Europe"], biomes: ["Aquatic", "Temperate", "Grassland", "Tropical"], maxM: 500, maxF: 500, group: "5-500", status: "LC", predator: false },
        { name: "Greater Rhea", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -15, tempMax: 35, continents: ["S. America"], biomes: ["Temperate", "Grassland"], maxM: 1, maxF: 7, group: "2-8", status: "NT", predator: false },
        { name: "Grizzly Bear", land: 1254, landPer: 158, water: 95, waterPer: 30, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 4, tempMin: -20, tempMax: 25, continents: ["N. America"], biomes: ["Tundra", "Taiga", "Temperate", "Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Hamadryas Baboon", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 85, climbPer: 25, barrierGrade: 2, barrierHeight: 2.5, tempMin: 3, tempMax: 45, continents: ["Africa", "Asia"], biomes: ["Desert"], maxM: 1, maxF: 19, group: "5-20", status: "LC", predator: false },
        { name: "Highland Cattle", land: 330, landPer: 60, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -25, tempMax: 28, continents: ["Europe"], biomes: ["Temperate", "Grassland", "Tundra", "Taiga"], maxM: 1, maxF: 9, group: "2-10", status: "LC", predator: false },
        { name: "Hill Radnor Sheep", land: 290, landPer: 20, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -7, tempMax: 38, continents: ["Europe"], biomes: ["Temperate", "Grassland"], maxM: 1, maxF: 9, group: "2-10", status: "LC", predator: false },
        { name: "Himalayan Brown Bear", land: 976, landPer: 170, water: 45, waterPer: 15, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 2.5, tempMin: -15, tempMax: 25, continents: ["Asia"], biomes: ["Tundra", "Taiga", "Temperate", "Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "CR", predator: true },
        { name: "Hippopotamus", land: 280, landPer: 55, water: 570, waterPer: 110, deepWater: 285, deepWaterPer: 55, deepWaterDepth: 2.5, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 0.5, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Aquatic", "Grassland", "Tropical"], maxM: 1, maxF: 14, group: "3-15", status: "VU", predator: false },
        { name: "Indian Elephant", land: 1622, landPer: 500, water: 110, waterPer: 55, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 0.5, tempMin: 18, tempMax: 42, continents: ["Asia"], biomes: ["Temperate", "Grassland", "Tropical"], maxM: 1, maxF: 14, group: "3-15", status: "EN", predator: false },
        { name: "Indian Peafowl", land: 260, landPer: 25, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 2, tempMin: 10, tempMax: 42, continents: ["Asia"], biomes: ["Temperate", "Grassland", "Tropical"], maxM: 1, maxF: 6, group: "2-7", status: "LC", predator: false },
        { name: "Indian Rhinoceros", land: 835, landPer: 125, water: 95, waterPer: 30, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 0.5, tempMin: 16, tempMax: 42, continents: ["Asia"], biomes: ["Aquatic", "Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: false },
        { name: "Jaguar", land: 750, landPer: 125, water: 95, waterPer: 30, climb: 45, climbPer: 15, barrierGrade: 4, barrierHeight: 4, tempMin: 16, tempMax: 42, continents: ["N. America", "C. America", "S. America"], biomes: ["Aquatic", "Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "NT", predator: true },
        { name: "Japanese Macaque", land: 330, landPer: 60, water: 45, waterPer: 15, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: -15, tempMax: 28, continents: ["Asia"], biomes: ["Taiga", "Temperate"], maxM: 20, maxF: 20, group: "5-40", status: "LC", predator: false },
        { name: "King Penguin", land: 180, landPer: 5, water: 135, waterPer: 2, deepWater: 67, deepWaterPer: 2, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: -30, tempMax: 10, continents: ["Antarctica"], biomes: ["Aquatic", "Tundra"], maxM: 500, maxF: 500, group: "6-500", status: "LC", predator: false },
        { name: "Kirk's Dik-Dik", land: 260, landPer: 25, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false },
        { name: "Koala", land: 200, landPer: 90, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 1, tempMin: -10, tempMax: 35, continents: ["Oceania"], biomes: ["Temperate"], maxM: 1, maxF: 3, group: "1-4", status: "VU", predator: false },
        { name: "Komodo Dragon", land: 705, landPer: 110, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 22, tempMax: 44, continents: ["Asia", "Oceania"], biomes: ["Tropical", "Desert"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: true },
        { name: "Lar Gibbon", land: 310, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: false },
        { name: "Llama", land: 330, landPer: 60, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -15, tempMax: 35, continents: ["S. America"], biomes: ["Temperate", "Grassland", "Taiga"], maxM: 1, maxF: 9, group: "2-10", status: "LC", predator: false },
        { name: "Malayan Tapir", land: 390, landPer: 60, water: 45, waterPer: 15, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: false },
        { name: "Mandrill", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 17, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 1, maxF: 19, group: "5-20", status: "VU", predator: false },
        { name: "Maned Wolf", land: 370, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 16, tempMax: 42, continents: ["S. America"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "NT", predator: false },
        { name: "Markhor", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 1.75, tempMin: -15, tempMax: 25, continents: ["Asia"], biomes: ["Temperate", "Taiga", "Tundra"], maxM: 20, maxF: 20, group: "3-20", status: "NT", predator: false },
        { name: "Meerkat", land: 260, landPer: 25, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: 14, tempMax: 44, continents: ["Africa"], biomes: ["Grassland", "Desert"], maxM: 15, maxF: 15, group: "5-30", status: "LC", predator: false },
        { name: "Moose", land: 835, landPer: 125, water: 95, waterPer: 30, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -25, tempMax: 28, continents: ["N. America", "Europe", "Asia"], biomes: ["Temperate", "Taiga", "Tundra"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false },
        { name: "Mute Swan", land: 200, landPer: 12, water: 100, waterPer: 6, deepWater: 0, deepWaterPer: 0, deepWaterDepth: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1.25, tempMin: -25, tempMax: 30, continents: ["N. America", "Europe", "Oceania"], biomes: ["Aquatic", "Temperate", "Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false },
        { name: "Nile Lechwe", land: 370, landPer: 45, water: 100, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Aquatic", "Grassland"], maxM: 1, maxF: 14, group: "3-15", status: "EN", predator: false },
        { name: "Nile Monitor", land: 160, landPer: 20, water: 70, waterPer: 10, deepWater: 35, deepWaterPer: 5, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 16, tempMax: 44, continents: ["Africa"], biomes: ["Aquatic", "Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "North Island Brown Kiwi", land: 200, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: -10, tempMax: 35, continents: ["Oceania"], biomes: ["Temperate", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: false },
        { name: "Nyala", land: 370, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 16, tempMax: 40, continents: ["Africa"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 29, group: "3-30", status: "LC", predator: false },
        { name: "Ocelot", land: 370, landPer: 55, water: 20, waterPer: 10, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 2, tempMin: 16, tempMax: 42, continents: ["N. America", "C. America", "S. America"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Okapi", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 17, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: false },
        { name: "Pallas's Cat", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1, tempMin: -15, tempMax: 25, continents: ["Asia"], biomes: ["Temperate", "Grassland", "Desert", "Tundra", "Taiga"], maxM: 1, maxF: 1, group: "1-2", status: "NT", predator: true },
        { name: "Plains Zebra", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 5, group: "2-6", status: "NT", predator: false },
        { name: "Platypus", land: 260, landPer: 40, water: 130, waterPer: 25, deepWater: 65, deepWaterPer: 12, deepWaterDepth: 1.5, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: -10, tempMax: 35, continents: ["Oceania"], biomes: ["Aquatic", "Temperate"], maxM: 1, maxF: 1, group: "1-2", status: "NT", predator: false },
        { name: "Polar Bear", land: 976, landPer: 170, water: 148, waterPer: 74, deepWater: 74, deepWaterPer: 37, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 4, tempMin: -40, tempMax: 10, continents: ["N. America", "Europe", "Asia"], biomes: ["Aquatic", "Tundra"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true },
        { name: "Proboscis Monkey", land: 330, landPer: 60, water: 45, waterPer: 15, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Aquatic", "Tropical"], maxM: 1, maxF: 11, group: "3-12", status: "EN", predator: false },
        { name: "Pronghorn Antelope", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -20, tempMax: 35, continents: ["N. America"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 1, maxF: 14, group: "3-15", status: "LC", predator: false },
        { name: "Przewalski's Horse", land: 630, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -20, tempMax: 40, continents: ["Asia"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 1, maxF: 14, group: "3-15", status: "EN", predator: false },
        { name: "Pygmy Hippo", land: 330, landPer: 60, water: 130, waterPer: 25, deepWater: 65, deepWaterPer: 12, deepWaterDepth: 1.5, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1, tempMin: 17, tempMax: 42, continents: ["Africa"], biomes: ["Aquatic", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: false },
        { name: "Quokka", land: 200, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: -10, tempMax: 35, continents: ["Oceania"], biomes: ["Temperate", "Grassland", "Tropical"], maxM: 15, maxF: 15, group: "5-30", status: "VU", predator: false },
        { name: "Raccoon", land: 260, landPer: 40, water: 20, waterPer: 10, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 1, tempMin: -20, tempMax: 35, continents: ["N. America", "C. America", "Europe", "Asia"], biomes: ["Temperate", "Grassland", "Tropical", "Taiga"], maxM: 2, maxF: 4, group: "2-6", status: "LC", predator: false },
        { name: "Red Deer", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -25, tempMax: 30, continents: ["Africa", "Asia", "Europe"], biomes: ["Temperate", "Grassland", "Tundra", "Taiga"], maxM: 1, maxF: 29, group: "3-30", status: "LC", predator: false },
        { name: "Red Fox", land: 370, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: -25, tempMax: 30, continents: ["N. America", "Europe", "Asia", "Africa", "Oceania"], biomes: ["Desert", "Grassland", "Temperate", "Taiga", "Tundra"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Red Kangaroo", land: 630, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -10, tempMax: 44, continents: ["Oceania"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 1, maxF: 9, group: "2-10", status: "LC", predator: false },
        { name: "Red Panda", land: 230, landPer: 55, water: 0, waterPer: 0, climb: 110, climbPer: 35, barrierGrade: 2, barrierHeight: 1, tempMin: 0, tempMax: 28, continents: ["Asia"], biomes: ["Taiga", "Temperate"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: false },
        { name: "Red River Hog", land: 310, landPer: 40, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 17, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 1, maxF: 5, group: "2-6", status: "LC", predator: false },
        { name: "Red Ruffed Lemur", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 10, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 3, maxF: 3, group: "2-6", status: "CR", predator: false },
        { name: "Red-Crowned Crane", land: 310, landPer: 40, water: 100, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 2, tempMin: -25, tempMax: 30, continents: ["Asia"], biomes: ["Aquatic", "Taiga", "Temperate", "Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: false },
        { name: "Red-necked Wallaby", land: 370, landPer: 45, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1, tempMin: -10, tempMax: 35, continents: ["Oceania"], biomes: ["Temperate", "Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false },
        { name: "Reindeer", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -40, tempMax: 22, continents: ["N. America", "Europe", "Asia"], biomes: ["Tundra", "Taiga"], maxM: 1, maxF: 24, group: "3-25", status: "VU", predator: false },
        { name: "Reticulated Giraffe", land: 976, landPer: 170, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 3, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 8, group: "2-9", status: "EN", predator: false },
        { name: "Ring Tailed Lemur", land: 370, landPer: 55, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 10, tempMax: 42, continents: ["Africa"], biomes: ["Temperate", "Tropical"], maxM: 15, maxF: 15, group: "5-30", status: "EN", predator: false },
        { name: "Sable Antelope", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 29, group: "3-30", status: "LC", predator: false },
        { name: "Saiga", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: -20, tempMax: 40, continents: ["Asia"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 1, maxF: 29, group: "3-30", status: "CR", predator: false },
        { name: "Saltwater Crocodile", land: 420, landPer: 90, water: 243, waterPer: 49, deepWater: 122, deepWaterPer: 24, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 0.5, tempMin: 16, tempMax: 44, continents: ["Asia", "Oceania"], biomes: ["Aquatic", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Sand Cat", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1, tempMin: 3, tempMax: 45, continents: ["Africa", "Asia"], biomes: ["Desert"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Scimitar-horned Oryx", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 3, tempMax: 45, continents: ["Africa"], biomes: ["Desert"], maxM: 1, maxF: 29, group: "3-30", status: "EN", predator: false },
        { name: "Siamang", land: 310, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: false },
        { name: "Siberian Tiger", land: 1162, landPer: 142, water: 95, waterPer: 30, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 4, tempMin: -25, tempMax: 25, continents: ["Asia"], biomes: ["Temperate", "Taiga", "Tundra"], maxM: 1, maxF: 1, group: "1-2", status: "EN", predator: true },
        { name: "Sloth Bear", land: 976, landPer: 170, water: 45, waterPer: 15, climb: 45, climbPer: 15, barrierGrade: 4, barrierHeight: 2.5, tempMin: 10, tempMax: 42, continents: ["Asia"], biomes: ["Temperate", "Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true },
        { name: "Snow Leopard", land: 705, landPer: 110, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 3, barrierHeight: 3, tempMin: -30, tempMax: 25, continents: ["Asia"], biomes: ["Tundra", "Taiga"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true },
        { name: "Somali Wild Ass", land: 420, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 3, tempMax: 45, continents: ["Africa"], biomes: ["Desert"], maxM: 1, maxF: 4, group: "2-5", status: "CR", predator: false },
        { name: "Southern Cassowary", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 17, tempMax: 42, continents: ["Oceania"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false },
        { name: "Southern White Rhinoceros", land: 835, landPer: 125, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 0.5, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 4, group: "2-5", status: "NT", predator: false },
        { name: "Spectacled Bear", land: 976, landPer: 170, water: 45, waterPer: 15, climb: 85, climbPer: 25, barrierGrade: 4, barrierHeight: 2.5, tempMin: 16, tempMax: 42, continents: ["S. America"], biomes: ["Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true },
        { name: "Spectacled Caiman", land: 160, landPer: 20, water: 101, waterPer: 14, deepWater: 50, deepWaterPer: 7, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 10, tempMax: 42, continents: ["C. America", "S. America"], biomes: ["Aquatic", "Temperate", "Grassland", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        { name: "Spotted Hyena", land: 1162, landPer: 92, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.75, tempMin: 16, tempMax: 44, continents: ["Africa"], biomes: ["Grassland", "Tropical", "Desert"], maxM: 10, maxF: 10, group: "3-10", status: "LC", predator: true },
        { name: "Springbok", land: 425, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 14, tempMax: 44, continents: ["Africa"], biomes: ["Grassland", "Desert"], maxM: 1, maxF: 29, group: "3-30", status: "LC", predator: false },
        { name: "Striped Hyena", land: 1162, landPer: 92, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 10, tempMax: 44, continents: ["Africa", "Asia"], biomes: ["Temperate", "Grassland", "Desert"], maxM: 5, maxF: 5, group: "1-6", status: "NT", predator: true },
        { name: "Striped Skunk", land: 180, landPer: 42, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: -20, tempMax: 35, continents: ["N. America"], biomes: ["Taiga", "Temperate", "Grassland", "Desert"], maxM: 1, maxF: 9, group: "2-10", status: "LC", predator: false },
        { name: "Sun Bear", land: 920, landPer: 142, water: 56, waterPer: 28, climb: 63, climbPer: 40, barrierGrade: 4, barrierHeight: 0.5, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true },
        { name: "Sussex Chicken", land: 200, landPer: 12, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1.25, tempMin: -15, tempMax: 35, continents: ["Europe"], biomes: ["Temperate", "Grassland"], maxM: 8, maxF: 72, group: "2-80", status: "LC", predator: false },
        { name: "Takin", land: 390, landPer: 60, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 3, barrierHeight: 1.25, tempMin: -15, tempMax: 25, continents: ["Asia"], biomes: ["Temperate", "Taiga", "Tundra"], maxM: 1, maxF: 19, group: "2-20", status: "VU", predator: false },
        { name: "Tamworth Pig", land: 290, landPer: 20, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -15, tempMax: 35, continents: ["Europe"], biomes: ["Temperate", "Grassland"], maxM: 1, maxF: 4, group: "1-5", status: "LC", predator: false },
        { name: "Tasmanian Devil", land: 200, landPer: 90, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: -10, tempMax: 35, continents: ["Oceania"], biomes: ["Temperate", "Grassland"], maxM: 1, maxF: 4, group: "1-5", status: "EN", predator: true },
        { name: "Thomson's Gazelle", land: 535, landPer: 55, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 16, tempMax: 42, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 35, group: "3-36", status: "LC", predator: false },
        { name: "Timber Wolf", land: 1254, landPer: 92, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: -25, tempMax: 30, continents: ["N. America", "Europe", "Asia"], biomes: ["Temperate", "Taiga", "Tundra"], maxM: 12, maxF: 12, group: "2-12", status: "LC", predator: true },
        { name: "West African Lion", land: 1162, landPer: 142, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 4, tempMin: 16, tempMax: 44, continents: ["Africa"], biomes: ["Grassland", "Desert"], maxM: 1, maxF: 29, group: "2-30", status: "CR", predator: true },
        { name: "Western Chimpanzee", land: 615, landPer: 125, water: 0, waterPer: 0, climb: 260, climbPer: 55, barrierGrade: 3, barrierHeight: 4, tempMin: 15, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 10, maxF: 10, group: "5-15", status: "CR", predator: false },
        { name: "Western Lowland Gorilla", land: 1102, landPer: 185, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 3, barrierHeight: 4, tempMin: 15, tempMax: 42, continents: ["Africa"], biomes: ["Tropical"], maxM: 1, maxF: 5, group: "3-6", status: "CR", predator: false },
        { name: "White-faced Saki", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2.5, tempMin: 17, tempMax: 42, continents: ["S. America"], biomes: ["Tropical"], maxM: 5, maxF: 5, group: "2-10", status: "LC", predator: false },
        { name: "Wild Boar", land: 340, landPer: 15, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -4, tempMax: 43, continents: ["Africa", "Asia", "Europe"], biomes: ["Taiga", "Temperate", "Grassland", "Tropical", "Desert"], maxM: 1, maxF: 29, group: "3-30", status: "LC", predator: false },
        { name: "Wild Water Buffalo", land: 570, landPer: 80, water: 200, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 17, tempMax: 42, continents: ["Asia"], biomes: ["Aquatic", "Grassland", "Tropical"], maxM: 1, maxF: 29, group: "3-30", status: "EN", predator: false },
        { name: "Wisent", land: 525, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -7, tempMax: 38, continents: ["Europe"], biomes: ["Taiga", "Temperate", "Grassland"], maxM: 1, maxF: 12, group: "2-13", status: "NT", predator: false },
        { name: "Wolverine", land: 1162, landPer: 92, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 1.25, tempMin: -40, tempMax: 22, continents: ["N. America", "Asia", "Europe"], biomes: ["Tundra", "Taiga"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true },
        
        // --- Modded Animal Data (Stats derived from Base Model) ---
        { name: "American Black Bear", land: 976, landPer: 170, water: 45, waterPer: 15, climb: 0, climbPer: 0, barrierGrade: 4, barrierHeight: 2.5, tempMin: 0, tempMax: 35, continents: ["N. America"], biomes: ["Taiga", "Temperate", "Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true, isMod: true },
        { name: "Australian Sea Lion", land: 260, landPer: 25, water: 340, waterPer: 45, deepWater: 170, deepWaterPer: 22, deepWaterDepth: 2, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 4, tempMax: 35, continents: ["Oceania"], biomes: ["Aquatic"], maxM: 1, maxF: 11, group: "3-12", status: "EN", predator: false, isMod: true },
        { name: "Black Swan", land: 200, landPer: 12, water: 100, waterPer: 6, deepWater: 0, deepWaterPer: 0, deepWaterDepth: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 1.25, tempMin: -25, tempMax: 30, continents: ["Oceania"], biomes: ["Aquatic", "Temperate", "Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false, isMod: true },
        { name: "Black-footed Ferret", land: 260, landPer: 25, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: 14, tempMax: 44, continents: ["N. America"], biomes: ["Grassland"], maxM: 15, maxF: 15, group: "5-30", status: "EN", predator: true, isMod: true },
        { name: "Carpathian Lynx", land: 705, landPer: 110, water: 0, waterPer: 0, climb: 45, climbPer: 15, barrierGrade: 2, barrierHeight: 3, tempMin: -25, tempMax: 30, continents: ["Europe"], biomes: ["Taiga", "Temperate"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true, isMod: true },
        { name: "Common Genet", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 1.75, tempMin: 17, tempMax: 42, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: true, isMod: true },
        { name: "Crested Gecko", land: 260, landPer: 40, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2, tempMin: 10, tempMax: 42, continents: ["Oceania"], biomes: ["Tropical"], maxM: 3, maxF: 3, group: "2-6", status: "VU", predator: false, isMod: true },
        { name: "Emperor Tamarin", land: 330, landPer: 60, water: 0, waterPer: 0, climb: 170, climbPer: 25, barrierGrade: 2, barrierHeight: 2.5, tempMin: 18, tempMax: 42, continents: ["S. America"], biomes: ["Tropical"], maxM: 20, maxF: 20, group: "5-40", status: "LC", predator: false, isMod: true },
        { name: "European Wild Horse", land: 630, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -20, tempMax: 40, continents: ["Europe"], biomes: ["Grassland"], maxM: 1, maxF: 14, group: "3-15", status: "DD", predator: false, isMod: true },
        { name: "Gerenuk", land: 370, landPer: 45, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 3, tempMax: 45, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 14, group: "3-15", status: "NT", predator: false, isMod: true },
        { name: "Golden Snub-Nosed Monkey", land: 330, landPer: 60, water: 45, waterPer: 15, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: -15, tempMax: 28, continents: ["Asia"], biomes: ["Temperate"], maxM: 20, maxF: 20, group: "5-40", status: "EN", predator: false, isMod: true },
        { name: "Indian Giant Squirrel", land: 230, landPer: 55, water: 0, waterPer: 0, climb: 110, climbPer: 35, barrierGrade: 2, barrierHeight: 1, tempMin: 0, tempMax: 28, continents: ["Asia"], biomes: ["Tropical"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false, isMod: true },
        { name: "Manatee", land: 330, landPer: 60, water: 130, waterPer: 25, deepWater: 65, deepWaterPer: 12, deepWaterDepth: 1.5, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1, tempMin: 17, tempMax: 42, continents: ["N. America"], biomes: ["Aquatic"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: false, isMod: true },
        { name: "Musk Ox", land: 630, landPer: 105, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: -40, tempMax: 22, continents: ["N. America"], biomes: ["Tundra"], maxM: 1, maxF: 14, group: "3-15", status: "LC", predator: false, isMod: true },
        { name: "Rock Hyrax", land: 260, landPer: 25, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 0.5, tempMin: 10, tempMax: 42, continents: ["Africa"], biomes: ["Grassland", "Desert"], maxM: 2, maxF: 4, group: "2-6", status: "LC", predator: false, isMod: true },
        { name: "Secretarybird", land: 700, landPer: 110, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1.25, tempMin: 14, tempMax: 44, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 7, group: "1-2", status: "EN", predator: true, isMod: true },
        { name: "Shoebill", land: 310, landPer: 40, water: 100, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 1, barrierHeight: 2, tempMin: 17, tempMax: 42, continents: ["Africa"], biomes: ["Aquatic"], maxM: 1, maxF: 1, group: "1-2", status: "VU", predator: true, isMod: true },
        { name: "Short-Beaked Echidna", land: 330, landPer: 60, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 16, tempMax: 42, continents: ["Oceania"], biomes: ["Grassland"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false, isMod: true },
        { name: "Spotted-necked Otter", land: 160, landPer: 20, water: 130, waterPer: 25, deepWater: 65, deepWaterPer: 12, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 10, tempMax: 40, continents: ["Africa"], biomes: ["Aquatic"], maxM: 9, maxF: 9, group: "2-18", status: "NT", predator: false, isMod: true },
        { name: "Watusi Cattle", land: 440, landPer: 100, water: 20, waterPer: 10, climb: 0, climbPer: 0, barrierGrade: 3, barrierHeight: 1.25, tempMin: 16, tempMax: 40, continents: ["Africa"], biomes: ["Grassland"], maxM: 1, maxF: 14, group: "3-15", status: "LC", predator: false, isMod: true },
        // Fictional / Extinct Animals
        { name: "Dire Wolf", land: 1254, landPer: 92, water: 0, waterPer: 0, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 2, tempMin: -25, tempMax: 30, continents: ["N. America"], biomes: ["Temperate", "Taiga", "Tundra"], maxM: 12, maxF: 12, group: "2-12", status: "EX", predator: true, isMod: true },
        { name: "Giant Diplocaulus", land: 160, landPer: 20, water: 70, waterPer: 10, deepWater: 35, deepWaterPer: 5, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 1, tempMin: 16, tempMax: 44, continents: ["N. America"], biomes: ["Aquatic"], maxM: 1, maxF: 1, group: "1-2", status: "EX", predator: true, isMod: true },
        // Fish - using caiman base model as a placeholder
        { name: "Silver Moony", land: 160, landPer: 20, water: 101, waterPer: 14, deepWater: 50, deepWaterPer: 7, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 10, tempMax: 42, continents: ["Asia"], biomes: ["Aquatic"], maxM: 1, maxF: 1, group: "1-2", status: "LC", predator: false, isMod: true },
        { name: "Black Marlin", land: 160, landPer: 20, water: 101, waterPer: 14, deepWater: 50, deepWaterPer: 7, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 10, tempMax: 42, continents: ["Asia", "Oceania"], biomes: ["Aquatic"], maxM: 1, maxF: 1, group: "1-2", status: "DD", predator: false, isMod: true },
        { name: "Yellowtail Amberjack", land: 160, landPer: 20, water: 101, waterPer: 14, deepWater: 50, deepWaterPer: 7, deepWaterDepth: 1, climb: 0, climbPer: 0, barrierGrade: 2, barrierHeight: 0.5, tempMin: 10, tempMax: 42, continents: ["Oceania"], biomes: ["Aquatic"], maxM: 1, maxF: 1, group: "1-2", status: "DD", predator: false, isMod: true },
    ];
    
    const exhibitAnimalData = [
        { name: "Amazonian Giant Centipede", tempMin: 22.8, tempMax: 28.9, humidityMin: 50, humidityMax: 84, groupMax: 2, walkable: false, status: "DD", regions: ["S. America"] },
        { name: "American Bullfrog", tempMin: 22.8, tempMax: 28.9, humidityMin: 60, humidityMax: 80, groupMax: 4, walkable: false, status: "LC", regions: ["N. America"] },
        { name: "Axolotl", tempMin: 15, tempMax: 20, humidityMin: 80, humidityMax: 90, groupMax: 5, walkable: false, status: "CR", regions: ["N. America"] },
        { name: "Boa Constrictor", tempMin: 22.8, tempMax: 31.1, humidityMin: 60, humidityMax: 80, groupMax: 2, walkable: false, status: "LC", regions: ["C. America", "S. America"] },
        { name: "Brazilian Salmon Pink Tarantula", tempMin: 23.9, tempMax: 27.2, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "NT", regions: ["S. America"] },
        { name: "Brazilian Wandering Spider", tempMin: 23.9, tempMax: 32.2, humidityMin: 60, humidityMax: 80, groupMax: 2, walkable: false, status: "LC", regions: ["S. America"] },
        { name: "Cloudless Sulphur", tempMin: 23.9, tempMax: 35, humidityMin: null, humidityMax: null, groupMax: 40, walkable: true, status: "LC", regions: ["N. America", "S. America"] },
        { name: "Common Death Adder", tempMin: 23.9, tempMax: 35, humidityMin: 20, humidityMax: 40, groupMax: 2, walkable: false, status: "LC", regions: ["Oceania"] },
        { name: "Costa Rican Zebra Tarantula", tempMin: 21.1, tempMax: 26.1, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "NT", regions: ["C. America"] },
        { name: "Danube Crested Newt", tempMin: 4.4, tempMax: 23.9, humidityMin: 60, humidityMax: 80, groupMax: 4, walkable: false, status: "NT", regions: ["Europe"] },
        { name: "Diamondback Terrapin", tempMin: 21.1, tempMax: 28.9, humidityMin: 50, humidityMax: 80, groupMax: 5, walkable: false, status: "VU", regions: ["N. America"] },
        { name: "Eastern Brown Snake", tempMin: 23.9, tempMax: 32.2, humidityMin: 20, humidityMax: 40, groupMax: 2, walkable: false, status: "LC", regions: ["Oceania"] },
        { name: "Egyptian Fruit Bat", tempMin: 21.1, tempMax: 30, humidityMin: null, humidityMax: null, groupMax: 40, walkable: true, status: "LC", regions: ["Africa", "Asia"] },
        { name: "European Peacock Butterfly", tempMin: 18.9, tempMax: 23.9, humidityMin: null, humidityMax: null, groupMax: 30, walkable: true, status: "LC", regions: ["Europe", "Asia"] },
        { name: "Fire Salamander", tempMin: 12.2, tempMax: 17.8, humidityMin: 70, humidityMax: 90, groupMax: 5, walkable: false, status: "LC", regions: ["Europe"] },
        { name: "Giant Burrowing Cockroach", tempMin: 21.1, tempMax: 26.1, humidityMin: 50, humidityMax: 60, groupMax: 50, walkable: false, status: "LC", regions: ["Oceania"] },
        { name: "Giant Desert Hairy Scorpion", tempMin: 23.9, tempMax: 37.8, humidityMin: 10, humidityMax: 20, groupMax: 2, walkable: false, status: "LC", regions: ["N. America"] },
        { name: "Giant Forest Scorpion", tempMin: 21.1, tempMax: 23.9, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "LC", regions: ["Asia"] },
        { name: "Giant Malaysian Leaf Insect", tempMin: 23.9, tempMax: 30, humidityMin: 80, humidityMax: 90, groupMax: 10, walkable: false, status: "LC", regions: ["Asia"] },
        { name: "Giant Tiger Land Snail", tempMin: 21.1, tempMax: 23.9, humidityMin: 70, humidityMax: 80, groupMax: 10, walkable: false, status: "LC", regions: ["Africa"] },
        { name: "Gila Monster", tempMin: 23.9, tempMax: 32.2, humidityMin: 10, humidityMax: 30, groupMax: 2, walkable: false, status: "NT", regions: ["N. America"] },
        { name: "Golden Poison Frog", tempMin: 23.9, tempMax: 26.1, humidityMin: 80, humidityMax: 90, groupMax: 5, walkable: false, status: "EN", regions: ["S. America"] },
        { name: "Goliath Birdeater", tempMin: 23.9, tempMax: 27.2, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "LC", regions: ["S. America"] },
        { name: "Goliath Frog", tempMin: 21.1, tempMax: 23.9, humidityMin: 80, humidityMax: 90, groupMax: 2, walkable: false, status: "EN", regions: ["Africa"] },
        { name: "Green Iguana", tempMin: 23.9, tempMax: 35, humidityMin: 60, humidityMax: 80, groupMax: 2, walkable: false, status: "LC", regions: ["C. America", "S. America"] },
        { name: "Lehmann's Poison Frog", tempMin: 23.9, tempMax: 26.1, humidityMin: 80, humidityMax: 90, groupMax: 5, walkable: false, status: "CR", regions: ["S. America"] },
        { name: "Lesser Antillean Iguana", tempMin: 23.9, tempMax: 35, humidityMin: 60, humidityMax: 80, groupMax: 2, walkable: false, status: "CR", regions: ["C. America"] },
        { name: "Mexican Red Knee Tarantula", tempMin: 23.9, tempMax: 27.2, humidityMin: 60, humidityMax: 70, groupMax: 2, walkable: false, status: "NT", regions: ["N. America"] },
        { name: "Monarch Butterfly", tempMin: 23.9, tempMax: 35, humidityMin: null, humidityMax: null, groupMax: 40, walkable: true, status: "EN", regions: ["N. America", "C. America", "S. America"] },
        { name: "Old World Swallowtail", tempMin: 17.8, tempMax: 23.9, humidityMin: null, humidityMax: null, groupMax: 30, walkable: true, status: "LC", regions: ["Europe", "Asia", "N. America"] },
        { name: "Puff Adder", tempMin: 22.8, tempMax: 31.1, humidityMin: 60, humidityMax: 80, groupMax: 5, walkable: false, status: "LC", regions: ["Africa", "Asia"] },
        { name: "Red-Eyed Tree Frog", tempMin: 26.1, tempMax: 31.1, humidityMin: 80, humidityMax: 90, groupMax: 5, walkable: false, status: "LC", regions: ["C. America", "S. America"] },
        { name: "Sacred Scarab Beetle", tempMin: 20, tempMax: 35, humidityMin: 40, humidityMax: 60, groupMax: 25, walkable: false, status: "DD", regions: ["Africa", "Asia", "Europe"] },
        { name: "Spectacled Flying Fox", tempMin: 21.1, tempMax: 32.2, humidityMin: null, humidityMax: null, groupMax: 35, walkable: true, status: "EN", regions: ["Oceania"] },
        { name: "Titan Beetle", tempMin: 22.8, tempMax: 36.1, humidityMin: 40, humidityMax: 60, groupMax: 7, walkable: false, status: "DD", regions: ["S. America"] },
        { name: "Western Diamondback Rattlesnake", tempMin: 23.9, tempMax: 30, humidityMin: 10, humidityMax: 30, groupMax: 2, walkable: false, status: "LC", regions: ["N. America"] },
        { name: "Yellow Anaconda", tempMin: 22.8, tempMax: 28.9, humidityMin: 60, humidityMax: 80, groupMax: 2, walkable: false, status: "LC", regions: ["S. America"] },
        // Modded Exhibit Animals
        { name: "Arizona Coral Snake", tempMin: 23.9, tempMax: 32.2, humidityMin: 20, humidityMax: 40, groupMax: 2, walkable: false, status: "LC", regions: ["N. America"], isMod: true },
        { name: "Bahia Scarlett Tarantula", tempMin: 23.9, tempMax: 27.2, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "NT", regions: ["S. America"], isMod: true },
        { name: "Brazilian Black and White Tarantula", tempMin: 23.9, tempMax: 27.2, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "NT", regions: ["S. America"], isMod: true },
        { name: "Brazilian Red and White Tarantula", tempMin: 23.9, tempMax: 27.2, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "NT", regions: ["S. America"], isMod: true },
        { name: "Burgundy Goliath Birdeater", tempMin: 23.9, tempMax: 27.2, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "LC", regions: ["S. America"], isMod: true },
        { name: "California Kingsnake", tempMin: 23.9, tempMax: 32.2, humidityMin: 20, humidityMax: 40, groupMax: 2, walkable: false, status: "LC", regions: ["N. America"], isMod: true },
        { name: "Pinkfoot Goliath Birdeater", tempMin: 23.9, tempMax: 27.2, humidityMin: 70, humidityMax: 80, groupMax: 2, walkable: false, status: "LC", regions: ["S. America"], isMod: true }
    ];

    // Compatibility data based on the "Bonus Group Species" column in Thrall's Social sheet. 2 = Enrichment
    const compatibility = {
        "Aardvark-Meerkat": 2, "Meerkat-Aardvark": 2,
        "Addax-Common Ostrich": 2, "Common Ostrich-Addax": 2,
        "Addax-Dama Gazelle": 2, "Dama Gazelle-Addax": 2,
        "Addax-Dromedary Camel": 2, "Dromedary Camel-Addax": 2,
        "Addax-Somali Wild Ass": 2, "Somali Wild Ass-Addax": 2,
        "Baird's Tapir-Capybara": 2, "Capybara-Baird's Tapir": 2,
        "Bongo-Red River Hog": 2, "Red River Hog-Bongo": 2,
        "Common Ostrich-Reticulated Giraffe": 2, "Reticulated Giraffe-Common Ostrich": 2,
        "Common Warthog-Springbok": 2, "Springbok-Common Warthog": 2,
        "European Fallow Deer-Mute Swan": 2, "Mute Swan-European Fallow Deer": 2,
        "European Fallow Deer-Red Deer": 2, "Red Deer-European Fallow Deer": 2,
        "European Fallow Deer-Wisent": 2, "Wisent-European Fallow Deer": 2,
        "Indian Rhinoceros-Malayan Tapir": 2, "Malayan Tapir-Indian Rhinoceros": 2,
        "Plains Zebra-Sable Antelope": 2, "Sable Antelope-Plains Zebra": 2,
        "Plains Zebra-Thomson's Gazelle": 2, "Thomson's Gazelle-Plains Zebra": 2,
        "Plains Zebra-Southern White Rhinoceros": 2, "Southern White Rhinoceros-Plains Zebra": 2,
        "Red Deer-Wisent": 2, "Wisent-Red Deer": 2
    };

    // --- SCRIPT START ---
    document.addEventListener('DOMContentLoaded', () => {
        // General UI elements
        const habitatTabBtn = document.getElementById('habitat-tab-btn');
        const exhibitTabBtn = document.getElementById('exhibit-tab-btn');
        const habitatPlannerDiv = document.getElementById('habitat-planner');
        const exhibitPlannerDiv = document.getElementById('exhibit-planner');
        
        // Habitat Planner Elements
        const habitatSearch = document.getElementById('habitat-search');
        const continentFilter = document.getElementById('continent-filter');
        const biomeFilter = document.getElementById('biome-filter');
        const statusFilter = document.getElementById('status-filter');
        const modFilterToggle = document.getElementById('mod-filter-toggle');
        const animalSelect = document.getElementById('animal-select');
        const addAnimalBtn = document.getElementById('add-animal-btn');
        const resetHabitatBtn = document.getElementById('reset-habitat-btn');
        const selectedAnimalsList = document.getElementById('selected-animals-list');
        const habitatResultsPlaceholder = document.getElementById('habitat-results-placeholder');
        const habitatResultsContent = document.getElementById('habitat-results-content');
        const habitatEmptyState = document.getElementById('habitat-empty-state');

        // Exhibit Planner Elements
        const exhibitSearch = document.getElementById('exhibit-search');
        const regionFilter = document.getElementById('region-filter');
        const typeFilter = document.getElementById('type-filter');
        const exhibitStatusFilter = document.getElementById('exhibit-status-filter');
        const exhibitModFilterToggle = document.getElementById('exhibit-mod-filter-toggle');
        const exhibitAnimalSelect = document.getElementById('exhibit-animal-select');
        const addExhibitAnimalBtn = document.getElementById('add-exhibit-animal-btn');
        const resetExhibitBtn = document.getElementById('reset-exhibit-btn');
        const exhibitResultsPlaceholder = document.getElementById('exhibit-results-placeholder');
        const exhibitResultsContent = document.getElementById('exhibit-results-content');
        const selectedExhibitsList = document.getElementById('selected-exhibits-list');
        const exhibitSummary = document.getElementById('exhibit-summary');

        let habitatAnimals = [];
        let exhibitAnimals = [];

        const statusMap = {
            "CR": { text: "Critically Endangered", color: "bg-red-600" },
            "EN": { text: "Endangered", color: "bg-orange-500" },
            "VU": { text: "Vulnerable", color: "bg-yellow-500" },
            "NT": { text: "Near Threatened", color: "bg-blue-500" },
            "LC": { text: "Least Concern", color: "bg-green-500" },
            "DD": { text: "Data Deficient", color: "bg-gray-500" },
            "EX": { text: "Extinct", color: "bg-black" }
        };

        function init() {
            // Tab listeners
            habitatTabBtn.addEventListener('click', () => switchTab('habitat'));
            exhibitTabBtn.addEventListener('click', () => switchTab('exhibit'));
            
            // Init Habitat Planner
            populateHabitatFilters();
            addAnimalBtn.addEventListener('click', addAnimalToHabitat);
            resetHabitatBtn.addEventListener('click', resetHabitat);
            
            // Init Exhibit Planner
            populateExhibitFilters();
            addExhibitAnimalBtn.addEventListener('click', addAnimalToExhibit);
            resetExhibitBtn.addEventListener('click', resetExhibitPlan);
            
            // Initial population
            populateAnimalSelects();

            // Filter listeners
            [habitatSearch, continentFilter, biomeFilter, statusFilter, modFilterToggle].forEach(el => 
                el.addEventListener(el.tagName === 'INPUT' ? 'input' : 'change', populateAnimalSelects)
            );
            [exhibitSearch, regionFilter, typeFilter, exhibitStatusFilter, exhibitModFilterToggle].forEach(el =>
                el.addEventListener(el.tagName === 'INPUT' ? 'input' : 'change', populateAnimalSelects)
            );
        }

        function populateHabitatFilters() {
            // Continents
            const continents = [...new Set(animalData.flatMap(a => a.continents))].sort();
            continentFilter.innerHTML = '<option value="all">All Continents</option>' + continents.map(c => `<option value="${c}">${c}</option>`).join('');

            // Biomes
            const biomes = [...new Set(animalData.flatMap(a => a.biomes))].sort();
            biomeFilter.innerHTML = '<option value="all">All Biomes</option>' + biomes.map(b => `<option value="${b}">${b}</option>`).join('');
            
            // Status
            const statuses = [...new Set(animalData.map(a => a.status))].sort();
            statusFilter.innerHTML = '<option value="all">All Statuses</option>' + statuses.map(s => `<option value="${s}">${s} - ${statusMap[s]?.text || 'Unknown'}</option>`).join('');
        }

        function populateExhibitFilters() {
            // Regions
            const regions = [...new Set(exhibitAnimalData.flatMap(a => a.regions))].sort();
            regionFilter.innerHTML = '<option value="all">All Regions</option>' + regions.map(r => `<option value="${r}">${r}</option>`).join('');

            // Type
            typeFilter.innerHTML = `
                <option value="all">All Exhibit Types</option>
                <option value="standard">Standard Exhibit</option>
                <option value="walkable">Walk-through</option>
            `;

            // Status
            const statuses = [...new Set(exhibitAnimalData.map(a => a.status))].sort();
            exhibitStatusFilter.innerHTML = '<option value="all">All Statuses</option>' + statuses.map(s => `<option value="${s}">${s} - ${statusMap[s]?.text || 'Unknown'}</option>`).join('');
        }

        function populateAnimalSelects() {
            // Habitat Animals
            const hSearch = habitatSearch.value.toLowerCase();
            const contF = continentFilter.value;
            const biomeF = biomeFilter.value;
            const statusF = statusFilter.value;
            const showMods = modFilterToggle.checked;

            const filteredHabitat = animalData.filter(a => {
                if (!showMods && a.isMod) return false;
                return (hSearch === '' || a.name.toLowerCase().includes(hSearch)) &&
                       (contF === 'all' || a.continents.includes(contF)) &&
                       (biomeF === 'all' || a.biomes.includes(biomeF)) &&
                       (statusF === 'all' || a.status === statusF);
            }).sort((a,b) => a.name.localeCompare(b.name));

            animalSelect.innerHTML = filteredHabitat.map(animal => 
                `<option value="${animal.name}">${animal.isMod ? `${animal.name} (Mod)` : animal.name}</option>`
            ).join('');
            if(filteredHabitat.length === 0) animalSelect.innerHTML = `<option value="">No animals match filters</option>`;


            // Exhibit Animals
            const eSearch = exhibitSearch.value.toLowerCase();
            const regionF = regionFilter.value;
            const typeF = typeFilter.value;
            const eStatusF = exhibitStatusFilter.value;
            const showExhibitMods = exhibitModFilterToggle.checked;

            const filteredExhibit = exhibitAnimalData.filter(a => {
                if (!showExhibitMods && a.isMod) return false;
                const typeMatch = (typeF === 'all') || (typeF === 'walkable' && a.walkable) || (typeF === 'standard' && !a.walkable);
                return (eSearch === '' || a.name.toLowerCase().includes(eSearch)) &&
                       (regionF === 'all' || a.regions.includes(regionF)) &&
                       typeMatch &&
                       (eStatusF === 'all' || a.status === eStatusF);
            }).sort((a,b) => a.name.localeCompare(b.name));
            
            exhibitAnimalSelect.innerHTML = filteredExhibit.map(animal => 
                `<option value="${animal.name}">${animal.isMod ? `${animal.name} (Mod)` : animal.name}</option>`
            ).join('');
            if(filteredExhibit.length === 0) exhibitAnimalSelect.innerHTML = `<option value="">No animals match filters</option>`;
        }
        
        function switchTab(tab) {
            if (tab === 'habitat') {
                habitatTabBtn.classList.add('active');
                exhibitTabBtn.classList.remove('active');
                habitatPlannerDiv.classList.remove('hidden');
                exhibitPlannerDiv.classList.add('hidden');
            } else {
                habitatTabBtn.classList.remove('active');
                exhibitTabBtn.classList.add('active');
                habitatPlannerDiv.classList.add('hidden');
                exhibitPlannerDiv.classList.remove('hidden');
            }
        }
        
        // --- HABITAT PLANNER FUNCTIONS ---
        function addAnimalToHabitat() {
            const selectedName = animalSelect.value;
            if (!selectedName) return;
            if (habitatAnimals.some(a => a.name === selectedName)) {
                const existingCard = document.querySelector(`[data-animal-name="${selectedName}"]`);
                if (existingCard) {
                    existingCard.classList.add('border-yellow-400', 'bg-yellow-50');
                    setTimeout(() => {
                        existingCard.classList.remove('border-yellow-400', 'bg-yellow-50');
                    }, 1500);
                }
                return;
            }
            const animal = animalData.find(a => a.name === selectedName);
            if (animal) {
                const [minGroup] = animal.group.split('-').map(Number);
                let defaultM = 1;
                let defaultF = Math.max(0, minGroup - 1);
                if (animal.maxM === 0) { defaultM = 0; defaultF = minGroup; }
                if (animal.group === "1-2" || animal.group === "1-1") { defaultF = 0; }
                
                habitatAnimals.push({ ...animal, m: defaultM, f: defaultF });
                updateHabitat();
            }
        }
        
        function removeAnimalFromHabitat(name) {
            habitatAnimals = habitatAnimals.filter(a => a.name !== name);
            updateHabitat();
        }

        function updateAnimalCount(name, gender, value) {
            const animal = habitatAnimals.find(a => a.name === name);
            if (animal) {
                animal[gender] = Math.max(0, parseInt(value, 10));
                updateHabitat();
            }
        }
        
        function resetHabitat() {
            habitatAnimals = [];
            updateHabitat();
        }

        function updateHabitat() {
            renderSelectedAnimals();
            calculateAndRenderHabitatResults();
        }

        function calculateAndRenderHabitatResults() {
            if (habitatAnimals.length === 0) {
                habitatResultsPlaceholder.classList.remove('hidden');
                habitatResultsContent.classList.add('hidden');
                return;
            }

            habitatResultsPlaceholder.classList.add('hidden');
            habitatResultsContent.classList.remove('hidden');

            let totalLand = 0, totalWater = 0, totalDeepWater = 0, totalClimb = 0;
            let maxBarrierGrade = 0, maxBarrierHeight = 0, maxDeepWaterDepth = 0;
            let tempRange = { min: -100, max: 100 };
            let allContinents = new Set(), allBiomes = new Set();
            let predatorCount = 0, preyCount = 0;

            habitatAnimals.forEach(animal => {
                const count = animal.m + animal.f;
                if (count > 0) {
                    totalLand += (animal.land || 0) + Math.max(0, count - 1) * (animal.landPer || 0);
                    totalWater += (animal.water || 0) + Math.max(0, count - 1) * (animal.waterPer || 0);
                    totalClimb += (animal.climb || 0) + Math.max(0, count - 1) * (animal.climbPer || 0);
                    if (animal.deepWater) {
                        totalDeepWater += (animal.deepWater || 0) + Math.max(0, count - 1) * (animal.deepWaterPer || 0);
                        maxDeepWaterDepth = Math.max(maxDeepWaterDepth, animal.deepWaterDepth || 0);
                    }
                }
                
                maxBarrierGrade = Math.max(maxBarrierGrade, animal.barrierGrade);
                maxBarrierHeight = Math.max(maxBarrierHeight, animal.barrierHeight);
                tempRange.min = Math.max(tempRange.min, animal.tempMin);
                tempRange.max = Math.min(tempRange.max, animal.tempMax);
                animal.continents.forEach(c => allContinents.add(c));
                animal.biomes.forEach(b => allBiomes.add(b));
                if (animal.predator) predatorCount++; else preyCount++;
            });

            let compatMessages = [];
            if (predatorCount > 0 && preyCount > 0) {
                compatMessages.push({ type: 'danger', text: 'Predators and prey animals are mixed. This is extremely dangerous!' });
            }
            if (habitatAnimals.length > 1) {
                const checkedPairs = new Set();
                for (let i = 0; i < habitatAnimals.length; i++) {
                    for (let j = i + 1; j < habitatAnimals.length; j++) {
                        const a1 = habitatAnimals[i], a2 = habitatAnimals[j];
                        const pairKey = [a1.name, a2.name].sort().join('-');
                        if (checkedPairs.has(pairKey)) continue;
                        const key = `${a1.name}-${a2.name}`;
                        if (compatibility[key] === 2 || compatibility[`${a2.name}-${a1.name}`] === 2) {
                            compatMessages.push({ type: 'success', text: `${a1.name} & ${a2.name} receive an enrichment bonus.` });
                        }
                        checkedPairs.add(pairKey);
                    }
                }
            }
            if (compatMessages.length === 0 && habitatAnimals.length > 1 && !(predatorCount > 0 && preyCount > 0)) {
                compatMessages.push({ type: 'info', text: 'All selected species are neutral towards each other.' });
            }

            const tempValid = tempRange.min <= tempRange.max;
            habitatResultsContent.innerHTML = `
                <h2 class="text-3xl font-bold text-green-700 mb-6">Habitat Requirements</h2>
                <div class="mb-6">
                    <h3 class="text-xl font-semibold mb-3 text-gray-700">Interspecies Compatibility</h3>
                    <div class="space-y-2">
                        ${compatMessages.map(msg => `
                            <div class="p-3 rounded-lg flex items-center ${
                                msg.type === 'danger' ? 'bg-red-100 text-red-800' : 
                                msg.type === 'success' ? 'bg-green-100 text-green-800' : 'bg-blue-100 text-blue-800'
                            }">
                                <svg class="mr-3 flex-shrink-0" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                                ${ msg.type === 'danger' ? '<circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/>' :
                                   msg.type === 'success' ? '<path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/>' :
                                   '<circle cx="12" cy="12" r="10"/><line x1="12" y1="16" x2="12" y2="12"/><line x1="12" y1="8" x2="12.01" y2="8"/>'
                                }
                                </svg>
                                <span>${msg.text}</span>
                            </div>
                        `).join('') || '<p class="text-gray-500">Add another animal to check compatibility.</p>'}
                    </div>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
                    ${renderStatCard('Total Land Area', `${totalLand.toFixed(0)} m²`, 'grass')}
                    ${renderStatCard('Total Water Area', `${totalWater.toFixed(0)} m²`, 'water')}
                    ${totalClimb > 0 ? renderStatCard('Total Climbing Area', `${totalClimb.toFixed(0)} m²`, 'climb') : ''}
                    ${totalDeepWater > 0 ? renderStatCard('Deep Water Area', `${totalDeepWater.toFixed(0)} m² at ${maxDeepWaterDepth}m`, 'deepwater') : ''}
                    ${renderStatCard('Barrier Grade', `Grade ${maxBarrierGrade}`, 'barrier')}
                    ${renderStatCard('Barrier Height', `${maxBarrierHeight} m`, 'barrier')}
                    ${renderStatCard('Temperature', tempValid ? `${tempRange.min}°C to ${tempRange.max}°C` : 'Incompatible', 'temp', !tempValid)}
                </div>
                <div class="mt-6">
                    <h3 class="text-xl font-semibold mb-3 text-gray-700">Required Tags</h3>
                    <div class="flex flex-wrap gap-2 items-center">
                        <span class="font-semibold mr-2">Continents:</span>
                        ${[...allContinents].map(c => `<span class="bg-yellow-200 text-yellow-800 text-sm font-medium px-3 py-1 rounded-full">${c}</span>`).join('')}
                    </div>
                    <div class="flex flex-wrap gap-2 mt-2 items-center">
                        <span class="font-semibold mr-2">Biomes:</span>
                        ${[...allBiomes].map(b => `<span class="bg-indigo-200 text-indigo-800 text-sm font-medium px-3 py-1 rounded-full">${b}</span>`).join('')}
                    </div>
                </div>
            `;
        }
        
        function renderSelectedAnimals() {
            if (habitatAnimals.length === 0) {
                habitatEmptyState.classList.remove('hidden');
                selectedAnimalsList.innerHTML = '';
                selectedAnimalsList.appendChild(habitatEmptyState);
            } else {
                habitatEmptyState.classList.add('hidden');
                selectedAnimalsList.innerHTML = habitatAnimals.map(animal => {
                    const totalCount = animal.m + animal.f;
                    const [minGroup, maxGroup] = animal.group.split('-').map(Number);
                    const groupOk = totalCount >= minGroup && totalCount <= maxGroup;
                    const maleOk = animal.m <= animal.maxM;
                    const femaleOk = animal.f <= animal.maxF;
                    const statusInfo = statusMap[animal.status] || { text: 'Unknown', color: 'bg-gray-400' };
                    
                    return `
                    <div class="bg-gray-50 p-4 rounded-lg border border-gray-200 fade-in" data-animal-name="${animal.name}">
                        <div class="flex justify-between items-start">
                            <div>
                                <div class="flex items-center gap-2 mb-1 flex-wrap">
                                    <h4 class="font-bold text-lg text-gray-800">${animal.name}</h4>
                                    ${animal.isMod ? '<span class="status-badge mod-badge" title="Modded Animal">Mod</span>' : ''}
                                    <span class="status-badge ${statusInfo.color}" title="${statusInfo.text}">${animal.status}</span>
                                </div>
                                <p class="text-sm text-gray-500" title="Min/Max Group Size">Group: ${animal.group} ${groupOk ? '<span class="text-green-500">✔</span>' : '<span class="text-red-500">✘</span>'}</p>
                            </div>
                            <button class="remove-btn text-gray-400 hover:text-red-500 transition-colors flex-shrink-0" data-name="${animal.name}" title="Remove ${animal.name}">
                                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="15" y1="9" x2="9" y2="15"/><line x1="9" y1="9" x2="15" y2="15"/></svg>
                            </button>
                        </div>
                        <div class="flex gap-4 mt-3">
                            <div class="w-1/2">
                                <label class="block text-xs font-medium text-gray-500">Males (Max: ${animal.maxM})</label>
                                <input type="number" min="0" value="${animal.m}" data-name="${animal.name}" data-gender="m" class="count-input w-full p-1 border rounded-md mt-1 text-center ${!maleOk ? 'border-red-500 bg-red-50' : 'border-gray-300'}">
                            </div>
                            <div class="w-1/2">
                                <label class="block text-xs font-medium text-gray-500">Females (Max: ${animal.maxF})</label>
                                <input type="number" min="0" value="${animal.f}" data-name="${animal.name}" data-gender="f" class="count-input w-full p-1 border rounded-md mt-1 text-center ${!femaleOk ? 'border-red-500 bg-red-50' : 'border-gray-300'}">
                            </div>
                        </div>
                    </div>`;
                }).join('');
                document.querySelectorAll('.remove-btn').forEach(btn => btn.addEventListener('click', (e) => removeAnimalFromHabitat(e.currentTarget.dataset.name)));
                document.querySelectorAll('.count-input').forEach(input => input.addEventListener('change', (e) => updateAnimalCount(e.target.dataset.name, e.target.dataset.gender, e.target.value)));
            }
        }
        
        function renderStatCard(title, value, type, isError = false) {
            const icons = {
                grass: '<path d="M2 22v-6c0-1.1.9-2 2-2h16a2 2 0 0 1 2 2v6H2z"/><path d="M12 14v-2a2 2 0 0 1 2-2h0a2 2 0 0 1 2 2v2"/><path d="M12 10V8"/><path d="M12 2v2"/>',
                water: '<path d="M17.2 22H6.8a4 4 0 0 1-3.6-2.4L1.2 14a4 4 0 0 1 0-3.2l2-5.2A4 4 0 0 1 6.8 2h10.4a4 4 0 0 1 3.6 3.4l2 5.2a4 4 0 0 1 0 3.2l-2 5.2a4 4 0 0 1-3.6 2.4z"/><path d="M5 12h14"/>',
                climb: '<path d="M12 3H5a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-7"/><path d="M18.37 3.63L14 8l-1.59-1.59a2 2 0 0 0-2.82 0L8 8"/>',
                deepwater: '<path d="M2.5 12.5a10 10 0 1 1 19 0a10 10 0 0 1-19 0Z"/><path d="M12 12v6"/><path d="m15 15-3 3-3-3"/>',
                barrier: '<path d="M18 10h4v10h-4"/><path d="M12 4h4v16h-4"/><path d="M6 14h4v6h-4"/><path d="M2 18h4v2H2z"/>',
                temp: '<path d="M14 4v10.54a4 4 0 1 1-4 0V4a2 2 0 0 1 4 0Z"/>'
            };
            return `<div class="p-4 rounded-lg ${isError ? 'bg-red-100' : 'bg-gray-50'} border ${isError ? 'border-red-300' : 'border-gray-200'}">
                <div class="flex items-center">
                    <div class="p-2 bg-green-100 text-green-600 rounded-full mr-4">
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">${icons[type]}</svg>
                    </div>
                    <div>
                        <p class="text-sm text-gray-500">${title}</p>
                        <p class="text-xl font-bold ${isError ? 'text-red-600' : 'text-gray-800'}">${value}</p>
                    </div>
                </div>
            </div>`;
        }

        // --- EXHIBIT PLANNER FUNCTIONS ---
        function addAnimalToExhibit() {
            const selectedName = exhibitAnimalSelect.value;
            if (!selectedName) return;
            const animal = exhibitAnimalData.find(a => a.name === selectedName);
            if(animal && !exhibitAnimals.some(a => a.name === selectedName)) {
                exhibitAnimals.push(animal);
                updateExhibitPlan();
            }
        }
        
        function removeAnimalFromExhibit(name) {
            exhibitAnimals = exhibitAnimals.filter(a => a.name !== name);
            updateExhibitPlan();
        }

        function resetExhibitPlan() {
            exhibitAnimals = [];
            updateExhibitPlan();
        }

        function updateExhibitPlan() {
            if (exhibitAnimals.length === 0) {
                exhibitResultsPlaceholder.classList.remove('hidden');
                exhibitResultsContent.classList.add('hidden');
            } else {
                exhibitResultsPlaceholder.classList.add('hidden');
                exhibitResultsContent.classList.remove('hidden');
                
                const walkableCount = exhibitAnimals.filter(a => a.walkable).length;
                const standardCount = exhibitAnimals.length - walkableCount;
                
                exhibitSummary.textContent = `You have planned for ${standardCount} standard exhibit(s) and ${walkableCount} walk-through exhibit(s).`;
                
                selectedExhibitsList.innerHTML = exhibitAnimals.map(animal => {
                    const statusInfo = statusMap[animal.status] || { text: 'Unknown', color: 'bg-gray-400' };
                    const humidityText = (animal.humidityMin !== null) ? `${animal.humidityMin}-${animal.humidityMax}%` : 'N/A';
                    return `
                    <div class="bg-gray-50 p-4 rounded-lg border border-gray-200 flex flex-col justify-between fade-in">
                        <div>
                            <div class="flex justify-between items-start mb-2">
                                <h4 class="font-bold text-lg text-gray-800">${animal.name}</h4>
                                <button class="remove-exhibit-btn text-gray-400 hover:text-red-500 transition-colors" data-name="${animal.name}" title="Remove ${animal.name}">
                                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><line x1="15" y1="9" x2="9" y2="15"/><line x1="9" y1="9" x2="15" y2="15"/></svg>
                                </button>
                            </div>
                            <div class="flex items-center gap-2 mb-3 flex-wrap">
                                ${animal.isMod ? '<span class="status-badge mod-badge" title="Modded Animal">Mod</span>' : ''}
                                <span class="status-badge ${statusInfo.color}" title="${statusInfo.text}">${animal.status}</span>
                                <span class="text-xs font-medium ${animal.walkable ? 'bg-blue-100 text-blue-800' : 'bg-gray-200 text-gray-700'} px-2 py-1 rounded-full">${animal.walkable ? 'Walk-through' : 'Standard Exhibit'}</span>
                            </div>
                            <p class="text-sm text-gray-600"><strong>Temperature:</strong> ${animal.tempMin}°C to ${animal.tempMax}°C</p>
                            <p class="text-sm text-gray-600"><strong>Humidity:</strong> ${humidityText}</p>
                            <p class="text-sm text-gray-600"><strong>Max Group Size:</strong> ${animal.groupMax}</p>
                        </div>
                    </div>
                    `;
                }).join('');

                document.querySelectorAll('.remove-exhibit-btn').forEach(btn => btn.addEventListener('click', (e) => removeAnimalFromExhibit(e.currentTarget.dataset.name)));
            }
        }


        init();
    });
    </script>
</body>
</html>