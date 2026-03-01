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
        const games = [
            { id: 1, name: "Cyberpunk 2077", price: 899, rating: 5, img: "https://www.cyberpunk.net/build/images/social-thumbnail-en-ddcf4d23.jpg", desc: "Відкритий світ майбутнього у Найт-Сіті.", reviews: ["Найкраща гра про майбутнє!", "Графіка космос 🚀", "Сюжет затягнув на 100 годин."] },
            { id: 2, name: "Elden Ring", price: 1599, rating: 5, img: "https://img.youtube.com/vi/a8k8R0Q2ubY/maxresdefault.jpg", desc: "Епічна пригода в Міжзем'ї.", reviews: ["Дуже складно, але круто!", "GOTY без заперечень.", "Світ гри неймовірний."] },
            { id: 3, name: "The Witcher 3", price: 499, rating: 5, img: "https://staticdelivery.nexusmods.com/mods/952/images/thumbnails/4996/4996-1595252155-1133603551.jpeg", desc: "Легендарна історія Геральта.", reviews: ["Класика на всі часи.", "Кращі квести в історії RPG.", "Музика — це щось!"] },
            { id: 4, name: "God of War", price: 1199, rating: 4, img: "https://static0.gamerantimages.com/wordpress/wp-content/uploads/2024/12/mixcollage-08-dec-2024-01-22-pm-530.jpg", desc: "Шлях Кратоса та Атрея.", reviews: ["Боївка дуже соковита.", "Постановка на вищому рівні.", "Сильна історія про батька і сина."] },
            { id: 5, name: "GTA V", price: 1537, rating: 5, img: "https://tse2.mm.bing.net/th/id/OIP.SebWKPO36WWlvFk3eu80FgAAAA?w=205&h=256&rs=1&pid=ImgDetMain&o=7&rm=3", desc: "Кримінал у Лос-Сантосі.", reviews: ["Завжди весело!", "Онлайн — це окремий світ.", "Кращий симулятор хаосу."] },
            { id: 6, name: "S.T.A.L.K.E.R. 2", price: 2549, rating: 5, img: "https://upload.wikimedia.org/wikipedia/ru/5/5c/S.T.A.L.K.E.R._2_Logo.png", desc: "Зона відчуження чекає.", reviews: ["Рідна атмосфера Зони.", "Батони тепер ще реалістичніші.", "Графіка — справжній некстген."] },
            { id: 7, name: "Spider-Man 2", price: 1999, rating: 5, img: "https://upload.wikimedia.org/wikipedia/ru/8/8e/Marvel%E2%80%99sSpiderMan2.png", desc: "Пітер та Майлз проти Венома.", reviews: ["Польоти по Нью-Йорку вражають.", "Веном дуже крутий.", "Грається на одному диханні."] },
            { id: 8, name: "Red Dead Redemption 2", price: 1249, rating: 5, img: "https://upload.wikimedia.org/wikipedia/ru/0/03/Red_Dead_Redemption_2_coverart.jpg", desc: "Епопея про Дикий Захід.", reviews: ["Найкращий сюжет у відеоіграх.", "Артур Морган — легенда.", "Деталізація просто божевільна."] },
            { id: 9, name: "Hogwarts Legacy", price: 1799, rating: 4, img: "https://upload.wikimedia.org/wikipedia/ru/3/33/Hogwarts_Legacy_cover.jpg", desc: "Ваш шлях у Гоґвортсі.", reviews: ["Мрія дитинства здійснилася!", "Дуже гарний замок.", "Магія відчувається круто."] },
            { id: 10, name: "Forza Horizon 5", price: 1399, rating: 5, img: "https://store-images.s-microsoft.com/image/apps.56329.13734397844529069.202e3fc9-37d6-4853-a58b-fabe504b71e8.b2447b97-7903-48de-8a49-9669d0495c4f?h=210", desc: "Фестиваль швидкості в Мексиці.", reviews: ["Краща гоночна гра.", "Машини звучать ідеально.", "Неймовірні краєвиди."] },
            { id: 11, name: "Mortal Kombat 1", price: 1699, rating: 5, img: "https://upload.wikimedia.org/wikipedia/ru/f/f4/Mortal_Kombat_1_%282023%29_cover.jpeg", desc: "Нова ера файтингу.", reviews: ["Фаталіті — вогонь!", "Камео-бійці — цікава механіка.", "Сюжетний режим дуже кінематографічний."] },
            { id: 12, name: "Resident Evil 4", price: 1450, rating: 5, img: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRzcU3ML7bIdwSHZfYyQ2PIBoWexL3n25orIg&s", desc: "Виживання в іспанському селищі.", reviews: ["Зразковий ремейк.", "Леон — красень.", "Екшн та страх у балансі."] },
            { id: 13, name: "Ghost of Tsushima", price: 1299, rating: 5, img: "https://image.api.playstation.com/vulcan/ap/rnd/202010/0222/niMUubpU9y1PxNvYmDfb8QFD.png", desc: "Шлях самурая на острові Цусіма.", reviews: ["Естетика Японії заворожує.", "Бої на мечах — одні з кращих.", "Вітер як навігатор — геніально."] },
            { id: 14, name: "The Last of Us Part I", price: 1899, rating: 5, img: "https://upload.wikimedia.org/wikipedia/ru/2/21/The_Last_of_Us_Part_I_-_%D0%9E%D0%B1%D0%BB%D0%BE%D0%B6%D0%BA%D0%B0.png", desc: "Подорож через спустошену Америку.", reviews: ["Гра, яка змушує плакати.", "Емоційний шедевр.", "Ремейк зробив її ідеальною."] },
            { id: 15, name: "Minecraft", price: 799, rating: 5, img: "https://i.redd.it/jncj9eqr9bad1.jpeg", desc: "Твій світ — твої правила.", reviews: ["Гра для будь-якого віку.", "Тут можна побудувати все.", "Безкінечні пригоди."] },
            { id: 16, name: "Starfield", price: 1500, rating: 3, img: "https://upload.wikimedia.org/wikipedia/ru/thumb/9/97/Starfield_2022.png/330px-Starfield_2022.png", desc: "Космічна подорож серед зірок.", reviews: ["Масштабно, але іноді порожньо.", "Для фанатів ігор Bethesda.", "Кораблі будувати цікаво."] }
        ];

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
                <div style="font-weight: bold; font-size: 14px;">Гейм-консультант вася </div>
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
        { keywords: ['купити', 'оформити'], reply: "Все просто: додавай гру до кошика 🛒 (іконка вгорі), а потім тисни 'Оформити'. Доставка ключа — миттєва!" }
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

 
