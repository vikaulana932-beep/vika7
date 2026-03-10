<!DOCTYPE html>
<html lang="uk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ultimate GameStore</title>
    <style>
        :root {
            --primary: #4b2bff;
            --secondary: #ff416c;
            --bg: #0f0f0f;
            --card-bg: #1e1e1e;
            --text: #ffffff;
            --border: #333;
        }

        body.light-theme {
            --bg: #f0f2f5;
            --card-bg: #ffffff;
            --text: #1a1a1a;
            --border: #ddd;
        }

        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        body {
            background-color: var(--bg);
            color: var(--text);
            font-family: 'Segoe UI', system-ui, sans-serif;
            margin: 0;
            overflow-x: hidden;
            transition: background 0.3s, color 0.3s;
        }

        .nav-controls {
            position: fixed;
            top: 20px;
            right: 20px;
            display: flex;
            gap: 10px;
            z-index: 1001;
        }

        .icon-btn {
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            border: none;
            color: white;
            padding: 12px 18px;
            border-radius: 50px;
            cursor: pointer;
            font-weight: bold;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        header {
            padding: 60px 20px 20px;
            text-align: center;
            animation: fadeInUp 0.8s ease-out;
        }

        h1 { font-size: 3rem; margin: 0; background: linear-gradient(to right, var(--primary), var(--secondary)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }

        .search-container {
            max-width: 600px;
            margin: 20px auto;
            position: relative;
            padding: 0 20px;
        }

        #gameSearch {
            width: 100%;
            padding: 15px 25px;
            border-radius: 30px;
            background: var(--card-bg);
            border: 2px solid var(--border);
            color: var(--text);
            font-size: 1.1rem;
            outline: none;
            box-sizing: border-box;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 30px;
            animation: fadeInUp 1s ease-out;
        }
  
        .game-card {
            background: var(--card-bg);
            border-radius: 20px;
            overflow: hidden;
            cursor: pointer;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            border: 1px solid var(--border);
        }

        .game-card:hover {
            transform: scale(1.05);
            border-color: var(--primary);
            box-shadow: 0 10px 30px rgba(75, 43, 255, 0.3);
        }

        .game-card img { width: 100%; height: 380px; object-fit: cover; }
        .game-info { padding: 20px; text-align: center; }

        .cart-overlay {
            position: fixed;
            top: 0; right: -400px;
            width: 350px; height: 100%;
            background: var(--card-bg);
            z-index: 2000;
            transition: 0.4s;
            box-shadow: -5px 0 20px rgba(0,0,0,0.5);
            padding: 30px;
            box-sizing: border-box;
        }
        .cart-overlay.active { right: 0; }
        .cart-items { margin-top: 20px; max-height: 70vh; overflow-y: auto; }
        .cart-item { display: flex; justify-content: space-between; margin-bottom: 15px; border-bottom: 1px solid var(--border); padding-bottom: 10px; }

        .modal-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.8);
            backdrop-filter: blur(10px);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1500;
        }

        .modal-content {
            background: var(--card-bg);
            padding: 30px;
            border-radius: 30px;
            max-width: 500px;
            width: 95%;
            max-height: 90vh;
            overflow-y: auto;
            text-align: center;
            position: relative;
            border: 2px solid var(--primary);
        }

        .stars { color: #ffd700; font-size: 1.5rem; margin: 10px 0; letter-spacing: 5px; }
        .comments-box { text-align: left; margin-top: 20px; background: rgba(128,128,128,0.1); padding: 15px; border-radius: 15px; }
        .comment-item { margin-bottom: 10px; border-bottom: 1px solid var(--border); padding-bottom: 5px; }
        .comment-user { color: var(--secondary); font-weight: bold; font-size: 0.9rem; }
        .comment-text { font-size: 0.85rem; margin-top: 2px; }

        .buy-btn {
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            color: white; border: none; padding: 15px 40px; border-radius: 50px;
            font-size: 1.2rem; font-weight: bold; cursor: pointer; margin-top: 20px; width: 100%;
        }

        .close-btn { position: absolute; top: 15px; right: 20px; color: #888; cursor: pointer; font-size: 1.5rem; } 


    </style> 
</head>
<body>

    <div class="nav-controls">
        <button class="icon-btn" onclick="toggleTheme()">🌓</button>
        <button class="icon-btn" onclick="toggleCart()">🛒 <span id="cart-count">0</span></button>
    </div>

    <div class="cart-overlay" id="cartOverlay">
        <span class="close-btn" onclick="toggleCart()">&times;</span>
        <h2>Ваш кошик</h2>
        <div class="cart-items" id="cartItems"></div>
        <div style="margin-top: 20px; border-top: 2px solid var(--primary); padding-top: 10px;">
            <h3>Разом: <span id="cart-total">0</span> грн</h3>
            <button class="buy-btn" onclick="checkout()">ОФОРМИТИ ЗАМОВЛЕННЯ</button>
        </div>
    </div>

    <header>
        <h1>ULTRA GAME STORE</h1>
        <p>Обирай свою наступну пригоду</p>
        <div class="search-container">
            <input type="text" id="gameSearch" placeholder="Пошук гри...">
        </div> 




<div class="genres-section">
    <button class="genre-item">Усі</button>
    <button class="genre-item">Екшн</button>
    <button class="genre-item">RPG</button>
    <button class="genre-item">Шутери</button>
    <button class="genre-item">Стратегії</button>
    <button class="genre-item">Пригоди</button>
</div>

<style>
    .genres-section {
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        gap: 12px;
        padding: 20px;
        max-width: 1200px;
        margin: 0 auto;
        animation: fadeInUp 1s ease-out;
    }

    .genre-item {
        background: var(--card-bg);
        color: var(--text);
        border: 1px solid var(--border);
        padding: 10px 22px;
        border-radius: 25px;
        cursor: pointer;
        font-size: 0.95rem;
        font-weight: 500;
        transition: all 0.3s ease;
    }

    .genre-item:hover {
        background: var(--primary);
        border-color: var(--primary);
        color: white;
        transform: translateY(-3px);
        box-shadow: 0 5px 15px rgba(75, 43, 255, 0.3);
    }

    
    .genre-item.active {
        background: linear-gradient(45deg, var(--primary), var(--secondary));
        border: none;
        color: white;
    }
</style>

    </header>

    <div class="container" id="shop"></div>
 
    <div class="modal-overlay" id="modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal()">&times;</span>
            <h2 id="m-title"></h2>
            <div class="stars" id="m-stars"></div>
            <p id="m-desc"></p>
            <div class="comments-box" id="m-comments"></div>
            <h3 id="m-price" style="color: var(--primary); font-size: 1.8rem; margin: 20px 0 0 0;"></h3>
            <button class="buy-btn" id="addToCartBtn">ДОДАТИ В КОШИК</button>
        </div>
    </div>

    <script> 



window.gameStore = [
    { title: "Cyberpunk 2077", genre: "Екшн", price: 899, rating: 5, desc: "Майбутнє вже тут.", img: "https://www.cyberpunk.net/build/images/social-thumbnail-en-ddcf4d23.jpg", reviews: [{user: "V_Hunter", text: "Гра року!"}, {user: "Silverhand", text: "Прокинься, самурай!"}] },
    { title: "The Witcher 3", genre: "RPG", price: 550, rating: 5, desc: "Найкраща RPG століття.", img: "https://staticdelivery.nexusmods.com/mods/952/images/thumbnails/4996/4996-1595252155-1133603551.jpeg", reviews: [{user: "Geralt", text: "Ніяк ви не навчитеся..."}, {user: "Ciri", text: "10/10"}] },
    { title: "CS 2", genre: "Шутери", price: 0, rating: 5, desc: "Легендарний тактичний шутер.", img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTTjaHPErWlltkGzzy2gDUaM9abGnpaVTo5Fg&s", reviews: [{user: "GlobalElite", text: "Тільки скіни дорогі."}] },
    { title: "Dota 2", genre: "Стратегії", price: 0, rating: 4, desc: "Битва древніх.", img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTlcSP-l1bnas4nAPnZPRVSzmD4p7A3LkoKYg&s", reviews: [{user: "Pudge", text: "Свіже м'ясо!"}] },
    { title: "Elden Ring", genre: "RPG", price: 1400, rating: 5, desc: "Світ від Хідетаки Міядзакі.", img: "https://upload.wikimedia.org/wikipedia/ru/thumb/7/7c/Elden_Ring_-_cover.jpg/330px-Elden_Ring_-_cover.jpg", reviews: [{user: "Tarnished", text: "Дуже складно, але круто."}] },
    { title: "GTA V", genre: "Екшн", price: 600, rating: 5, desc: "Пограбування у Лос-Сантосі.", img: "https://tse2.mm.bing.net/th/id/OIP.SebWKPO36WWlvFk3eu80FgAAAA?w=205&h=256&rs=1&pid=ImgDetMain&o=7&rm=3", reviews: [{user: "Franklin", text: "Класика."}] },
    { title: "God of War", genre: "Екшн", price: 950, rating: 5, desc: "Пригода Кратоса.", img: "https://upload.wikimedia.org/wikipedia/ru/6/6e/God_of_War-_Ragnar%C3%B6k.jpg", reviews: [{user: "Atreus", text: "Батьку!"}] },
    { title: "Minecraft", genre: "Пригоди", price: 800, rating: 5, desc: "Будуй свою мрію.", img: "https://i.redd.it/jncj9eqr9bad1.jpeg", reviews: [{user: "Steve", text: "Квадратний світ!"}] },
    { title: "RDR 2", genre: "Пригоди", price: 1100, rating: 5, desc: "Дикий Захід.", img: "https://cdn1.epicgames.com/epic/offer/RDR2PC1227_Epic%20Games_860x1148-860x1148-b4c2210ee0c3c3b843a8de399bfe7f5c.jpg", reviews: [{user: "Arthur", text: "I have a plan!"}] },
    { title: "Hogwarts Legacy", genre: "RPG", price: 1200, rating: 5, desc: "Магія чекає.", img: "https://image.api.playstation.com/vulcan/ap/rnd/202503/2716/f6b1e4512ee6061913f7d604da8f5f39566be56ca32a68ee.png", reviews: [{user: "PotterFan", text: "Нарешті мій лист прийшов!"}] },
    { title: "Apex Legends", genre: "Шутери", price: 0, rating: 4, desc: "Королівська битва.", img: "https://cdn1.epicgames.com/spt-assets/5dcd88f4e2094a698ebffa43438edc33/apex-legends-1f4hz.jpg", reviews: [{user: "Wraith", text: "Швидкий геймплей."}] },
    { title: "Valorant", genre: "Шутери", price: 0, rating: 4, desc: "Тактика та магія.", img: "https://cdn1.epicgames.com/offer/cbd5b3d310a54b12bf3fe8c41994174f/EGS_VALORANT_RiotGames_S2_1200x1600-773ea2de7951435e977ba5f5f6934442", reviews: [{user: "Sage", text: "Тільки для про."}] },
    { title: "Diablo IV", genre: "RPG", price: 1600, rating: 4, desc: "Темний світ Санктуарію.", img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRf38vcf0vkKVCozHitV0K07XcP0_uKtbHGsQ&s", reviews: [{user: "Lilith_Stalker", text: "Дуже атмосферно."}] },
    { title: "StarCraft II", genre: "Стратегії", price: 0, rating: 5, desc: "Найкраща RTS.", img: "https://upload.wikimedia.org/wikipedia/ru/0/07/StarCraft_II.jpg", reviews: [{user: "ZergRush", text: "Керріган на зв'язку."}] },
    { title: "Uncharted 4", genre: "Пригоди", price: 750, rating: 5, desc: "Фінал Дрейка.", img: "https://upload.wikimedia.org/wikipedia/ru/5/58/Uncharted_4_cover.jpg", reviews: [{user: "Nate", text: "Sic Parvis Magna."}] } 
    ,{ title:  "S.T.A.L.K.E.R. 2", genre: "Пригоди", price: 2540, rating: 5, desc:"Пригоди чорнобиля", img: "https://upload.wikimedia.org/wikipedia/ru/5/5c/S.T.A.L.K.E.R._2_Logo.png", reviews:  "Зона відчуження чекає.", reviews: [{ user: "Сталкер мен", text:"прикрасна гра про чорнобиль" }] },
];

let  = [];


window.openModal = function(index) {
    const game = window.gameStore[index];
    document.getElementById('m-title').innerText = game.title;
    document.getElementById('m-desc').innerText = game.desc;
    document.getElementById('m-stars').innerText = "⭐".repeat(game.rating);
    
    
    const reviewsBox = document.querySelector('.comments-box');
    reviewsBox.innerHTML = '<h4>Відгуки:</h4>' + game.reviews.map(r => `
        <div class="comment-item">
            <div class="comment-user">${r.user}</div>
            <div class="comment-text">${r.text}</div>
        </div>`).join('');

    
    const buyBtn = document.querySelector('.modal-content .buy-btn');
    buyBtn.innerText = `ДОДАТИ В КОШИК (${game.price} грн)`;
    buyBtn.onclick = () => addToCart(index);

    document.getElementById('modal').style.display = 'flex';
};

window.closeModal = () => document.getElementById('modal').style.display = 'none';


function addToCart(index) {
    const game = window.gameStore[index];
    cart.push(game);
    updateCartUI();
    alert(`${game.title} додано до кошика!`);
}

function updateCartUI() {
    const cartCount = document.getElementById('cart-count');
    const cartItems = document.getElementById('cartItems');
    const cartTotal = document.getElementById('cart-total');
    
    cartCount.innerText = cart.length;
    cartItems.innerHTML = cart.map((item, i) => `
        <div class="cart-item">
            <span>${item.title}</span>
            <span>${item.price} грн</span>
        </div>`).join('');
    
    const total = cart.reduce((sum, item) => sum + item.price, 0);
    cartTotal.innerText = total;
}


function renderStore(filter = "Усі") {
    const shop = document.getElementById('shop');
    shop.innerHTML = "";
    window.gameStore.forEach((game, index) => {
        if (filter !== "Усі" && game.genre !== filter) return;
        const card = document.createElement('div');
        card.className = 'game-card';
        card.onclick = () => window.openModal(index);
        card.innerHTML = `
            <img src="${game.img}" alt="${game.title}">
            <div class="game-info">
                <h3>${game.title}</h3>
                <p style="color: var(--secondary); font-weight:bold;">${game.genre}</p>
                <p>${game.price > 0 ? game.price + ' грн' : 'Безкоштовно'}</p>
                <button class="buy-btn">ДЕТАЛЬНІШЕ</button>
            </div>`;
        shop.appendChild(card);
    });
}

document.addEventListener('DOMContentLoaded', () => {
    renderStore();
    document.querySelectorAll('.genre-item').forEach(btn => {
        btn.addEventListener('click', (e) => {
            e.stopPropagation();
            document.querySelectorAll('.genre-item').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            renderStore(btn.textContent.trim());
        });
    });
});

function toggleTheme() { document.body.classList.toggle('light-theme'); }
function toggleCart() { document.getElementById('cartOverlay').classList.toggle('active'); }
function checkout() { alert('Замовлення оформлено! Дякуємо!'); cart = []; updateCartUI(); toggleCart(); }


document.addEventListener('DOMContentLoaded', () => {
    const searchInput = document.getElementById('gameSearch');

    if (searchInput) {
        searchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase().trim();
            const cards = document.querySelectorAll('.game-card');

            cards.forEach(card => {
                const title = card.querySelector('h3').innerText.toLowerCase();
                
                if (title.includes(searchTerm)) {
                    card.style.display = 'block';
                    card.style.animation = 'fadeInUp 0.3s ease-out';
                } else {
                    card.style.display = 'none';
                }
            });
        });
    }
});


        let cart = [];
        const shop = document.getElementById('shop');
        const cartOverlay = document.getElementById('cartOverlay');

        function displayGames(filteredGames) {
            shop.innerHTML = ''; 
            filteredGames.forEach(game => {
                const card = document.createElement('div');
                card.className = 'game-card';
                card.innerHTML = `
                    <img src="${game.img}" alt="${game.name}">
                    <div class="game-info">
                        <h3>${game.name}</h3>
                        <div style="color: var(--primary); font-weight: bold;">${game.price} грн</div>
                    </div>
                `;
                card.onclick = () => openModal(game);
                shop.appendChild(card);
            });
        }

        function openModal(game) {
            document.getElementById('m-title').innerText = game.name;
            document.getElementById('m-price').innerText = game.price + " грн";
            document.getElementById('m-desc').innerText = game.desc;
            document.getElementById('m-stars').innerHTML = '★'.repeat(game.rating) + '☆'.repeat(5 - game.rating);
            
            const commentsContainer = document.getElementById('m-comments');
            commentsContainer.innerHTML = '<strong>Відгуки:</strong><br><br>' + 
                game.reviews.map(r => `
                    <div class="comment-item">
                        <div class="comment-user">User_${Math.floor(Math.random()*1000)}:</div>
                        <div class="comment-text">${r}</div>
                    </div>
                `).join('');
            
            document.getElementById('addToCartBtn').onclick = () => {
                cart.push(game);
                updateCart();
                closeModal();
                toggleCart();
            };
            document.getElementById('modal').style.display = 'flex';
        }

        function updateCart() {
            const container = document.getElementById('cartItems');
            document.getElementById('cart-count').innerText = cart.length;
            container.innerHTML = cart.map((item, index) => `
                <div class="cart-item">
                    <span>${item.name}</span>
                    <b>${item.price} грн</b>
                    <button onclick="removeFromCart(${index})" style="background:none; border:none; color:red; cursor:pointer; font-size: 1.2rem;">✕</button>
                </div>
            `).join('');
            const total = cart.reduce((sum, item) => sum + item.price, 0);
            document.getElementById('cart-total').innerText = total;
        }

        function removeFromCart(index) {
            cart.splice(index, 1);
            updateCart();
        }

        function toggleTheme() { document.body.classList.toggle('light-theme'); }
        function toggleCart() { cartOverlay.classList.toggle('active'); }
        function closeModal() { document.getElementById('modal').style.display = 'none'; }
        function checkout() { 
            if(cart.length === 0) return alert('Кошик порожній!');
            alert('Дякуємо! Ваше замовлення прийнято.'); 
            cart = []; updateCart(); toggleCart(); 
        }

        displayGames(games);

        document.getElementById('gameSearch').addEventListener('input', (e) => {
            const term = e.target.value.toLowerCase();
            displayGames(games.filter(g => g.name.toLowerCase().includes(term)));
        });

        window.onclick = (e) => { if(e.target.classList.contains('modal-overlay')) closeModal(); }
    </script> 

<div id="simple-chat" style="position: fixed; bottom: 20px; right: 20px; z-index: 9999; font-family: 'Segoe UI', sans-serif;">
    <button id="chat-open" style="background: linear-gradient(45deg, #4b2bff, #ff416c); color: white; border: none; width: 60px; height: 60px; border-radius: 50%; cursor: pointer; box-shadow: 0 5px 15px rgba(0,0,0,0.3); font-size: 24px; transition: 0.3s;" onmouseover="this.style.transform='scale(1.1)'" onmouseout="this.style.transform='scale(1)'">💬</button>

    <div id="chat-window" style="display: none; width: 320px; height: 450px; background: #1a1a1a; border: 1px solid #333; border-radius: 20px; flex-direction: column; overflow: hidden; box-shadow: 0 15px 40px rgba(0,0,0,0.6);">
        <div style="background: linear-gradient(to right, #4b2bff, #ff416c); color: white; padding: 15px; display: flex; justify-content: space-between; align-items: center;">
            <div>
                <div style="font-weight: bold; font-size: 14px;">Гейм-консультант вася</div>
                <div style="font-size: 10px; opacity: 0.8;">● В мережі (відповідає миттєво)</div>
            </div>
            <span id="chat-close" style="cursor: pointer; font-size: 24px;">&times;</span>
        </div>
        <div id="chat-messages" style="flex: 1; padding: 15px; overflow-y: auto; color: white; font-size: 14px; display: flex; flex-direction: column; gap: 10px; background: #0f0f0f;">
            <div style="background: #252525; padding: 10px; border-radius: 15px 15px 15px 0; align-self: flex-start; max-width: 85%;">Вітаю в Ultimate GameStore! 👋 Обираєш у що пограти ввечері?</div>
        </div>
        <div id="typing-indicator" style="display: none; padding: 5px 15px; color: #888; font-size: 12px; font-style: italic;">Макс друкує...</div>
        <div style="padding: 15px; background: #1a1a1a; border-top: 1px solid #333; display: flex; gap: 8px;">
            <input type="text" id="chat-input" placeholder="Запитай про гру..." style="flex: 1; background: #252525; border: 1px solid #444; color: white; padding: 10px; border-radius: 10px; outline: none; font-size: 14px;">
            <button id="chat-send" style="background: #4b2bff; border: none; color: white; border-radius: 10px; cursor: pointer; padding: 0 15px; font-weight: bold;">➔</button>
        </div>
    </div>
</div>

<script>
    const chatBtn = document.getElementById('chat-open');
    const chatWin = document.getElementById('chat-window');
    const chatClose = document.getElementById('chat-close');
    const chatInput = document.getElementById('chat-input');
    const chatMsgs = document.getElementById('chat-messages');
    const chatSend = document.getElementById('chat-send');
    const typing = document.getElementById('typing-indicator');

    chatBtn.onclick = () => { chatWin.style.display = 'flex'; chatBtn.style.display = 'none'; };
    chatClose.onclick = () => { chatWin.style.display = 'none'; chatBtn.style.display = 'block'; };

    const scenarios = [
        { keywords: ['привіт', 'добрий день', 'хай'], reply: "Привіт-привіт! Я допомагаю знайти найкращі ігри. Що тебе цікавить: RPG, екшн чи, можливо, класика?" },
        { keywords: ['ціна', 'скільки', 'коштує'], reply: "У нас зараз діють круті ціни! Від 499 грн за класику до 1599 грн за новинки. При покупці двох ігор — секретний бонус! 😉" },
        { keywords: ['cyberpunk', 'кіберпанк'], reply: "Cyberpunk 2077 — це шедевр! Найт-Сіті чекає. До речі, гра йде вже з усіма оновленнями. Береш?" },
        { keywords: ['відьмак', 'witcher', 'геральт'], reply: "The Witcher 3 — це база. Кращі квести, які я бачив. За таку ціну (499 грн) це майже дарунок!" },
        { keywords: ['elden ring', 'елден'], reply: "О, ти любиш хардкор? Elden Ring — це гра року. Буде складно, але воно того варте!" },
        { keywords: ['дякую', 'спасибі'], reply: "Завжди радий допомогти! Якщо обереш гру — просто тисни кнопку 'Додати в кошик'. Вдалих забігів! 🎮" },
        { keywords: ['хто ти', 'бот'], reply: "Я твій ігровий бро! Допомагаю не витратити гроші на нудні ігри. Тільки хіти!" },
        { keywords: ['купити', 'оформити'], reply: "Все просто: додавай гру до кошика 🛒 (іконка вгорі), а потім тисни оформити і грай вкруті ігри" }
    ];

    function botReply(text) {
        typing.style.display = 'block';
        chatMsgs.scrollTop = chatMsgs.scrollHeight;

        setTimeout(() => {
            typing.style.display = 'none';
            let msg = text.toLowerCase();
            let foundReply = "Цікаве питання! На жаль, я поки вчуся, але можу сказати одне — всі ігри у нашому списку точно варті твоєї уваги. Спробуй запитати про конкретну гру!";

            for (let s of scenarios) {
                if (s.keywords.some(k => msg.includes(k))) {
                    foundReply = s.reply;
                    break;
                }
            }
            addMessage(foundReply, 'bot');
        }, 1500);
    }

    function addMessage(text, type) {
        const div = document.createElement('div');
        div.innerText = text;
        div.style.padding = '12px';
        div.style.borderRadius = type === 'user' ? '15px 15px 0 15px' : '15px 15px 15px 0';
        div.style.maxWidth = '85%';
        div.style.lineHeight = '1.4';
        
        if(type === 'user') {
            div.style.background = 'linear-gradient(45deg, #4b2bff, #6e52ff)';
            div.style.alignSelf = 'flex-end';
        } else {
            div.style.background = '#252525';
            div.style.alignSelf = 'flex-start';
        }
        chatMsgs.appendChild(div);
        chatMsgs.scrollTop = chatMsgs.scrollHeight;
    }

    chatSend.onclick = () => {
        if(!chatInput.value.trim()) return;
        addMessage(chatInput.value, 'user');
        botReply(chatInput.value);
        chatInput.value = '';
    };

    chatInput.onkeypress = (e) => { if(e.key === 'Enter') chatSend.onclick(); };
</script>

</body>
</html>
 
