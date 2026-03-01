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
        }

 
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }

        body {
            background-color: var(--bg);
            color: white;
            font-family: 'Segoe UI', system-ui, sans-serif;
            margin: 0;
            overflow-x: hidden;
        }

        header {
            padding: 40px 20px;
            text-align: center;
            animation: fadeInUp 0.8s ease-out;
        }

        h1 { font-size: 3rem; margin: 0; background: linear-gradient(to right, var(--primary), var(--secondary)) text; -webkit-text-fill-color: transparent; }

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
            border: 1px solid #333;
            position: relative;
        }

        .game-card:hover {
            transform: scale(1.05);
            border-color: var(--primary);
            box-shadow: 0 10px 30px rgba(255, 75, 43, 0.3);
        }

        .game-card img {
            width: 100%;
            height: 380px;
            object-fit: cover;
        }

        .game-info { padding: 20px; text-align: center; }
        .game-info h3 { margin: 0 0 10px 0; font-size: 1.5rem; }
  
        .modal-overlay {
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.9);
            backdrop-filter: blur(10px);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }

        .modal-content {
            background: #252525;
            padding: 40px;
            border-radius: 30px;
            max-width: 500px;
            width: 90%;
            text-align: center;
            position: relative;
            border: 2px solid var(--primary);
        }

        .stars { color: #ffd700; font-size: 1.5rem; margin: 15px 0; }
        
        .buy-btn {
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            color: white; border: none;
            padding: 15px 40px;
            border-radius: 50px;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            margin-top: 20px;
            width: 100%;
        }

        .close-btn {
            position: absolute; top: 20px; right: 20px;
            color: #888; cursor: pointer; font-size: 1.5rem;
        }
    </style>
</head>
<body>

    <header>
        <h1>ULTRA GAME STORE</h1>
        <p>Обирай свою наступну пригоду</p>
    </header>

    <div class="container" id="shop"></div>
 
    <div class="modal-overlay" id="modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeModal()">&times;</span>
            <h2 id="m-title">Назва гри</h2>
            <div class="stars" id="m-stars"></div>
            <p id="m-desc">Детальний опис гри та її особливості...</p>
            <h3 id="m-price" style="color: var(--primary); font-size: 2rem;"></h3>
            <button class="buy-btn" onclick="alert('Дякуємо за покупку ми дуже раді!')">ПРИДБАТИ ЗАРАЗ</button>
        </div>
    </div>

    <script>
        const games = [
            { id: 1, name: "Cyberpunk 2077", price: "899 грн", rating: 5, img: "https://www.cyberpunk.net/build/images/social-thumbnail-en-ddcf4d23.jpg", desc: "Відкритий світ майбутнього у Найт-Сіті." },
            { id: 2, name: "Elden Ring", price: "1599 грн", rating: 5, img: "https://img.youtube.com/vi/a8k8R0Q2ubY/maxresdefault.jpg", desc: "Епічна пригода в Міжзем'ї від FromSoftware." },
            { id: 3, name: "The Witcher 3", price: "499 грн", rating: 5, img: "https://staticdelivery.nexusmods.com/mods/952/images/thumbnails/4996/4996-1595252155-1133603551.jpeg", desc: "Легендарна історія Геральта з Рівії." },
            { id: 4, name: "God of War", price: "1199 грн", rating: 4, img: "https://static0.gamerantimages.com/wordpress/wp-content/uploads/2024/12/mixcollage-08-dec-2024-01-22-pm-530.jpg", desc: "Шлях Кратоса та Атрея у світі скандинавських міфів." } 
            ,{ id: 5, name: "Grand Theft Auto V", price: "1537 грн", rating: 5, img: "https://c-ssl.dtstatic.com/uploads/item/201501/24/20150124110653_nR2ti.thumb.400_0.jpeg", desc: "гра дети граєш за 3 злопця і проходиш місій." } 
        ];

        const shop = document.getElementById('shop');
        const modal = document.getElementById('modal'); 
        games.forEach(game => {
            const card = document.createElement('div');
            card.className = 'game-card';
            card.innerHTML = `
                <img src="${game.img}" alt="${game.name}">
                <div class="game-info">
                    <h3>${game.name}</h3>
                    <div style="color: var(--primary)">${game.price}</div>
                </div>
            `;
            card.onclick = () => openModal(game);
            shop.appendChild(card);
        });

        function openModal(game) {
            document.getElementById('m-title').innerText = game.name;
            document.getElementById('m-price').innerText = game.price;
            document.getElementById('m-desc').innerText = game.desc;
            document.getElementById('m-stars').innerHTML = '★'.repeat(game.rating) + '☆'.repeat(5 - game.rating); 
            document.getElementById('m-desc').innerHTML = game.name;
            modal.style.display = 'flex';
        }

        function closeModal() {
            modal.style.display = 'none';
        } 
        window.onclick = (e) => { if(e.target == modal) closeModal(); }
    </script>
</body>
</html>
 
        
