<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Netflix Clone</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Font Awesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        /* Custom styles for Inter font and scrollbar hiding */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            /* Subtle gradient background for the whole page */
            background: linear-gradient(180deg, #141414 0%, #000000 100%);
            min-height: 100vh;
        }

        /* Hide scrollbar for horizontal movie rows */
        .hide-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .hide-scrollbar {
            -ms-overflow-style: none;  /* IE and Edge */
            scrollbar-width: none;  /* Firefox */
        }

        /* Hero section background image with gradient overlay */
        .hero-section {
            background-image: url('https://m.media-amazon.com/images/M/MV5BN2U3NzNmNjYtZTM0MC00MjAyLWEyODYtMGY4ZjdhNjliNTQ5XkEyXkFqcGc@._V1_.jpg');
            background-size: cover;
            background-position: center;
            position: relative;
        }

        .hero-section::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, rgba(0,0,0,0) 50%, rgba(0,0,0,0.8) 100%);
            z-index: 1;
        }

        .hero-content {
            position: relative;
            z-index: 2;
        }

        /* Movie card hover effect */
        .movie-card-container {
            transition: transform 0.3s ease-in-out, z-index 0.3s ease-in-out;
            transform-origin: center center;
            position: relative; /* Needed for z-index on hover */
        }

        .movie-card-container:hover {
            transform: scale(1.15); /* Slightly larger scale for better visibility */
            z-index: 10; /* Bring hovered card to front */
            box-shadow: 0 0 20px rgba(0,0,0,0.8); /* Add shadow for depth */
        }

        .movie-card-info {
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
            position: absolute;
            bottom: 0;
            left: 0;
            right: 0;
            background: linear-gradient(to top, rgba(0,0,0,0.9) 0%, rgba(0,0,0,0) 100%);
            padding: 1rem;
            border-bottom-left-radius: 0.5rem;
            border-bottom-right-radius: 0.5rem;
        }

        .movie-card-container:hover .movie-card-info {
            opacity: 1;
        }

        /* Modal specific styles */
        .modal-overlay {
            background-color: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(5px);
            z-index: 50;
        }

        .modal-content {
            max-width: 90%;
            max-height: 90%;
            overflow-y: auto;
        }

        /* Custom button styles */
        .btn-primary {
            background: linear-gradient(to right, #e50914, #ff0000);
            transition: background 0.3s ease-in-out, transform 0.2s ease-in-out;
        }
        .btn-primary:hover {
            background: linear-gradient(to right, #ff0000, #e50914);
            transform: translateY(-2px);
        }

        .btn-secondary {
            background-color: rgba(109, 109, 110, 0.7);
            transition: background-color 0.3s ease-in-out, transform 0.2s ease-in-out;
        }
        .btn-secondary:hover {
            background-color: rgba(109, 109, 110, 0.9);
            transform: translateY(-2px);
        }
    </style>
</head>
<body class="bg-gray-900 text-white">

    <!-- Header / Navigation Bar -->
    <header class="fixed top-0 left-0 right-0 z-40 bg-gradient-to-b from-black to-transparent p-4 flex items-center justify-between">
        <div class="flex items-center space-x-8">
            <!-- Netflix Logo -->
            <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/08/Netflix_2015_logo.svg/1920px-Netflix_2015_logo.svg.png" alt="Netflix Logo" class="h-6 md:h-8">
            <!-- Navigation Links -->
            <nav class="hidden md:flex space-x-6 text-sm font-medium">
                <a href="#" class="hover:text-gray-300 transition-colors duration-200">Home</a>
                <a href="#" class="hover:text-gray-300 transition-colors duration-200">TV Shows</a>
                <a href="#" class="hover:text-gray-300 transition-colors duration-200">Movies</a>
                <a href="#" class="hover:text-gray-300 transition-colors duration-200">New & Popular</a>
                <a href="#" class="hover:text-gray-300 transition-colors duration-200">My List</a>
            </nav>
        </div>
        <!-- Right side icons -->
        <div class="flex items-center space-x-6">
            <i class="fas fa-search text-lg cursor-pointer hover:text-gray-300 transition-colors duration-200"></i>
            <i class="fas fa-bell text-lg cursor-pointer hover:text-gray-300 transition-colors duration-200"></i>
            <div class="w-8 h-8 rounded-full bg-gray-600 flex items-center justify-center text-sm font-bold cursor-pointer hover:opacity-80 transition-opacity duration-200">
                <img src="https://placehold.co/32x32/FF0000/FFFFFF?text=U" alt="User Avatar" class="rounded-full">
            </div>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="hero-section h-[70vh] md:h-[85vh] flex items-end pb-20 px-4 md:px-16 relative">
        <div class="hero-content text-white max-w-xl">
            <h1 class="text-4xl md:text-6xl font-extrabold mb-4 drop-shadow-lg">The Witcher</h1>
            <p class="text-base md:text-lg mb-6 drop-shadow-md">Geralt of Rivia, a mutated monster-hunter for hire, journeys toward his destiny in a turbulent world where people often prove more wicked than beasts.</p>
            <div class="flex space-x-4">
                <button class="btn-primary flex items-center px-6 py-3 rounded-lg font-semibold text-lg hover:shadow-lg">
                    <i class="fas fa-play mr-2"></i> Play
                </button>
                <button class="btn-secondary flex items-center px-6 py-3 rounded-lg font-semibold text-lg hover:shadow-lg">
                    <i class="fas fa-info-circle mr-2"></i> More Info
                </button>
            </div>
        </div>
    </section>

    <!-- Movie Content Rows -->
    <main class="py-8 px-4 md:px-16 -mt-24 relative z-30">
        <!-- Trending Now Row -->
        <section class="mb-12">
            <h2 class="text-2xl md:text-3xl font-bold mb-4">Trending Now</h2>
            <div id="trending-now-row" class="flex overflow-x-scroll hide-scrollbar space-x-4 pb-4">
                <!-- Movie cards will be injected here by JavaScript -->
            </div>
        </section>

        <!-- Netflix Originals Row -->
        <section class="mb-12">
            <h2 class="text-2xl md:text-3xl font-bold mb-4">Netflix Originals</h2>
            <div id="netflix-originals-row" class="flex overflow-x-scroll hide-scrollbar space-x-4 pb-4">
                <!-- Movie cards will be injected here by JavaScript -->
            </div>
        </section>

        <!-- Top Rated Row -->
        <section class="mb-12">
            <h2 class="text-2xl md:text-3xl font-bold mb-4">Top Rated</h2>
            <div id="top-rated-row" class="flex overflow-x-scroll hide-scrollbar space-x-4 pb-4">
                <!-- Movie cards will be injected here by JavaScript -->
            </div>
        </section>
    </main>

    <!-- Movie Details Modal -->
    <div id="movie-modal" class="fixed inset-0 hidden items-center justify-center modal-overlay transition-opacity duration-300 opacity-0">
        <div class="modal-content bg-gray-800 rounded-lg shadow-xl overflow-hidden transform scale-95 transition-transform duration-300 w-full lg:w-3/5">
            <div class="relative">
                <img id="modal-image" src="https://placehold.co/800x450/000000/FFFFFF?text=Movie+Poster" alt="Movie Poster" class="w-full h-auto object-cover rounded-t-lg">
                <button id="close-modal" class="absolute top-4 right-4 text-white text-3xl bg-gray-900 rounded-full w-10 h-10 flex items-center justify-center hover:bg-gray-700 transition-colors duration-200">
                    <i class="fas fa-times"></i>
                </button>
            </div>
            <div class="p-6 md:p-8">
                <h3 id="modal-title" class="text-3xl md:text-4xl font-bold mb-4">Movie Title</h3>
                <p id="modal-description" class="text-gray-300 text-base md:text-lg mb-6">This is a detailed description of the movie, providing more context and plot points. It highlights key aspects and character development.</p>
                <div class="flex items-center mb-4 text-gray-400">
                    <span id="modal-year" class="mr-4">2023</span>
                    <span id="modal-rating" class="mr-4 border border-gray-500 px-2 py-0.5 rounded text-sm">TV-MA</span>
                    <span id="modal-duration">2h 15m</span>
                </div>
                <div class="flex space-x-4">
                    <button class="btn-primary flex items-center px-6 py-3 rounded-lg font-semibold text-lg hover:shadow-lg">
                        <i class="fas fa-play mr-2"></i> Play
                    </button>
                    <button class="btn-secondary flex items-center px-6 py-3 rounded-lg font-semibold text-lg hover:shadow-lg">
                        <i class="fas fa-plus mr-2"></i> My List
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-black py-10 px-4 md:px-16 text-gray-400 text-sm">
        <div class="max-w-6xl mx-auto grid grid-cols-2 md:grid-cols-4 gap-4">
            <div>
                <a href="#" class="block mb-2 hover:underline">Audio Description</a>
                <a href="#" class="block mb-2 hover:underline">Investor Relations</a>
                <a href="#" class="block mb-2 hover:underline">Legal Notices</a>
            </div>
            <div>
                <a href="#" class="block mb-2 hover:underline">Help Centre</a>
                <a href="#" class="block mb-2 hover:underline">Jobs</a>
                <a href="#" class="block mb-2 hover:underline">Cookie Preferences</a>
            </div>
            <div>
                <a href="#" class="block mb-2 hover:underline">Gift Cards</a>
                <a href="#" class="block mb-2 hover:underline">Terms of Use</a>
                <a href="#" class="block mb-2 hover:underline">Corporate Information</a>
            </div>
            <div>
                <a href="#" class="block mb-2 hover:underline">Media Centre</a>
                <a href="#" class="block mb-2 hover:underline">Privacy</a>
                <a href="#" class="block mb-2 hover:underline">Contact Us</a>
            </div>
        </div>
        <div class="max-w-6xl mx-auto mt-8 text-center md:text-left">
            <p>&copy; 2023 Netflix, Inc. This is a clone for educational purposes only.</p>
        </div>
    </footer>

    <script>
        // Mock movie data - In a real application, this would come from an API
        const movies = [
            {
                id: 1,
                title: "Stranger Things",
                description: "When a young boy vanishes, a small town uncovers a mystery involving secret experiments, terrifying supernatural forces, and one strange little girl.",
                image: "https://m.media-amazon.com/images/M/MV5BMjg2NmM0MTEtYWY2Yy00NmFlLTllNTMtMjVkZjEwMGVlNzdjXkEyXkFqcGc@._V1_.jpg",
                year: "2016",
                rating: "TV-14",
                duration: "4 Seasons",
                category: ["trending", "originals"]
            },
            {
                id: 2,
                title: "The Crown",
                description: "Follows the political rivalries and romance of Queen Elizabeth II's reign and the events that shaped the second half of the 20th century.",
                image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTu4aeLDs_0KVSWVNRquoGkTCGSi-jav4BUlg&s",
                year: "2016",
                rating: "TV-MA",
                duration: "5 Seasons",
                category: ["trending", "originals", "top-rated"]
            },
            {
                id: 3,
                title: "Money Heist",
                description: "A criminal mastermind who goes by 'The Professor' has a plan to pull off the biggest heist in recorded history -- to print billions of euros in the Royal Mint of Spain.",
                image: "https://resizing.flixster.com/ITt1FPrFePNR6FSqZrZK7BocG2U=/ems.cHJkLWVtcy1hc3NldHMvdHZzZWFzb24vUlRUVjEwMTMyOTMud2VicA==",
                year: "2017",
                rating: "TV-MA",
                duration: "5 Parts",
                category: ["trending", "top-rated"]
            },
            {
                id: 4,
                title: "Squid Game",
                description: "Hundreds of cash-strapped players accept a strange invitation to compete in children's games. Inside, a tempting prize awaits — with deadly high stakes.",
                image: "https://resizing.flixster.com/loBSjHPma1bgf42w9FsZYde6Ez8=/ems.cHJkLWVtcy1hc3NldHMvdHZzZXJpZXMvN2M5M2VmNjAtYmY1MS00ZjNlLThkYmMtMmYyNTE3YjI3YmE5LmpwZw==",
                year: "2021",
                rating: "TV-MA",
                duration: "1 Season",
                category: ["trending", "originals"]
            },
            {
                id: 5,
                title: "Bridgerton",
                description: "The eight close-knit siblings of the Bridgerton family look for love and happiness in London high society. Inspired by Julia Quinn's bestselling novels.",
                image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTN0Z9lyXqWMAQpi3VBF1w0nLjYyzw0VJqm2w&s",
                year: "2020",
                rating: "TV-14",
                duration: "2 Seasons",
                category: ["trending", "originals"]
            },
            {
                id: 6,
                title: "Ozark",
                description: "A financial advisor drags his family from Chicago to the Missouri Ozarks, where he must launder $500 million in five years to appease a drug boss.",
                image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQmfy4iKzxA0cuKEY0TgUpXv1G3uP3etT2Q3Q&s",
                year: "2017",
                rating: "TV-MA",
                duration: "4 Seasons",
                category: ["originals", "top-rated"]
            },
            {
                id: 7,
                title: "The Queen's Gambit",
                description: "In a 1950s orphanage, a young chess prodigy struggles with addiction while aspiring to become the world's greatest chess player.",
                image: "https://m.media-amazon.com/images/M/MV5BMmRlNjQxNWQtMjk1OS00N2QxLTk0YWQtMzRhYjY5YTFhNjMxXkEyXkFqcGc@._V1_FMjpg_UX1000_.jpg",
                year: "2020",
                rating: "TV-MA",
                duration: "1 Season",
                category: ["originals", "top-rated"]
            },
            {
                id: 8,
                title: "Lupin",
                description: "Inspired by the adventures of Arsène Lupin, gentleman thief Assane Diop sets out to avenge his father for an injustice inflicted by a wealthy family.",
                image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSsr-5MOFOV4m2rlmp-Gw5NDTtrlm3ojH1vMQ&s",
                year: "2021",
                rating: "TV-MA",
                duration: "3 Parts",
                category: ["trending"]
            },
            {
                id: 9,
                title: "Dark",
                description: "A family saga with a supernatural twist, set in a German town where the disappearance of two young children exposes the double lives and fractured relationships among four families.",
                image: "https://dnm.nflximg.net/api/v6/mAcAr9TxZIVbINe88xb3Teg5_OA/AAAABRJalm21U50T3QgwaxwMjDBPcckg5jDmdc6D836PFZ7zt3hU2dllsX6DBNfSXBmyiQTNNNH6Qny35fAccjISzXBeWJUICcfwTEqkrP7VrvTDkg3d0XxWWcq4lSK_aX_s__0fqQ.jpg?r=621",
                year: "2017",
                rating: "TV-MA",
                duration: "3 Seasons",
                category: ["top-rated"]
            },
            {
                id: 10,
                title: "Cobra Kai",
                description: "Decades after their karate showdown, a down-and-out Johnny Lawrence seeks redemption by reopening the infamous Cobra Kai dojo, reigniting his rivalry with a now-successful Daniel LaRusso.",
                image: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSSeBwVKu_GBIgeVlj2Zp4iUKL3j29fIsRx6A&s",
                year: "2018",
                rating: "TV-14",
                duration: "5 Seasons",
                category: ["trending", "top-rated"]
            }
        ];

        // Get elements
        const trendingNowRow = document.getElementById('trending-now-row');
        const netflixOriginalsRow = document.getElementById('netflix-originals-row');
        const topRatedRow = document.getElementById('top-rated-row');
        const movieModal = document.getElementById('movie-modal');
        const closeModalBtn = document.getElementById('close-modal');
        const modalImage = document.getElementById('modal-image');
        const modalTitle = document.getElementById('modal-title');
        const modalDescription = document.getElementById('modal-description');
        const modalYear = document.getElementById('modal-year');
        const modalRating = document.getElementById('modal-rating');
        const modalDuration = document.getElementById('modal-duration');

        /**
         * Creates a movie card HTML element.
         * @param {Object} movie - The movie object.
         * @returns {HTMLElement} The created movie card element.
         */
        function createMovieCard(movie) {
            const cardContainer = document.createElement('div');
            cardContainer.className = 'movie-card-container flex-none w-40 md:w-52 h-auto rounded-lg overflow-hidden cursor-pointer relative';
            cardContainer.setAttribute('data-movie-id', movie.id);

            cardContainer.innerHTML = `
                <img src="${movie.image}" alt="${movie.title}" class="w-full h-auto object-cover rounded-lg">
                <div class="movie-card-info absolute bottom-0 left-0 right-0 p-3 bg-gradient-to-t from-black to-transparent text-white opacity-0 transition-opacity duration-300 rounded-b-lg">
                    <h4 class="text-sm md:text-base font-semibold truncate">${movie.title}</h4>
                    <div class="flex items-center justify-between mt-2">
                        <button class="text-white hover:text-red-500 transition-colors duration-200">
                            <i class="fas fa-play-circle text-xl"></i>
                        </button>
                        <button class="text-white hover:text-red-500 transition-colors duration-200">
                            <i class="fas fa-plus-circle text-xl"></i>
                        </button>
                        <button class="text-white hover:text-red-500 transition-colors duration-200">
                            <i class="fas fa-info-circle text-xl"></i>
                        </button>
                    </div>
                </div>
            `;

            // Event listener to open modal on card click
            cardContainer.addEventListener('click', () => openModal(movie));
            return cardContainer;
        }

        /**
         * Populates a given row with movie cards based on category.
         * @param {HTMLElement} rowElement - The HTML element for the row.
         * @param {string} category - The category to filter movies by.
         */
        function populateMovieRow(rowElement, category) {
            const filteredMovies = movies.filter(movie => movie.category.includes(category));
            filteredMovies.forEach(movie => {
                rowElement.appendChild(createMovieCard(movie));
            });
        }

        /**
         * Opens the movie details modal.
         * @param {Object} movie - The movie object to display.
         */
        function openModal(movie) {
            modalImage.src = movie.image.replace('300x450', '800x450'); // Use a wider placeholder for modal
            modalTitle.textContent = movie.title;
            modalDescription.textContent = movie.description;
            modalYear.textContent = movie.year;
            modalRating.textContent = movie.rating;
            modalDuration.textContent = movie.duration;

            movieModal.classList.remove('hidden', 'opacity-0');
            movieModal.classList.add('flex', 'opacity-100');
            // Add scale animation for modal content
            movieModal.querySelector('.modal-content').classList.remove('scale-95');
            movieModal.querySelector('.modal-content').classList.add('scale-100');
        }

        /**
         * Closes the movie details modal.
         */
        function closeModal() {
            movieModal.classList.remove('flex', 'opacity-100');
            movieModal.classList.add('opacity-0');
            // Reset scale animation for modal content
            movieModal.querySelector('.modal-content').classList.remove('scale-100');
            movieModal.querySelector('.modal-content').classList.add('scale-95');

            // Hide after transition
            setTimeout(() => {
                movieModal.classList.add('hidden');
            }, 300);
        }

        // Event listener for closing modal
        closeModalBtn.addEventListener('click', closeModal);
        // Close modal when clicking outside of the content
        movieModal.addEventListener('click', (event) => {
            if (event.target === movieModal) {
                closeModal();
            }
        });

        // Initialize movie rows on page load
        window.onload = function() {
            populateMovieRow(trendingNowRow, 'trending');
            populateMovieRow(netflixOriginalsRow, 'originals');
            populateMovieRow(topRatedRow, 'top-rated');
        };
    </script>
</body>
</html>
