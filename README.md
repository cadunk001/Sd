<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Um Pedido Especial</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            background-color: #ffe6f0; /* Um tom de rosa claro */
            color: #8c184b; /* Um tom de rosa mais escuro */
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            position: relative;
            overflow: hidden; /* Esconde os corações que saem da tela */
        }

        #heart-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none; /* Permite interagir com os elementos abaixo */
        }

        .heart {
            color: #e91e63;
            font-size: 20px;
            position: absolute;
            animation: float 5s linear infinite;
            opacity: 0;
        }

        @keyframes float {
            0% {
                transform: translateY(100vh) rotate(0deg);
                opacity: 0;
            }
            50% {
                opacity: 1;
            }
            100% {
                transform: translateY(-10vh) rotate(360deg);
                opacity: 0;
            }
        }

        #content {
            background-color: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            text-align: center;
            z-index: 10; /* Garante que o conteúdo fique acima dos corações */
        }

        #image-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .image-container {
            width: 100%;
            overflow: hidden;
            border-radius: 8px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
        }

        .image-container img {
            width: 100%;
            height: auto;
            display: block;
        }

        #upload-section {
            margin-bottom: 20px;
        }

        #message-section {
            margin-bottom: 20px;
        }

        #message {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 5px;
            box-sizing: border-box;
            margin-top: 10px;
            font-size: 16px;
            text-align: center;
        }

        #proposal-button {
            background-color: #e91e63;
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 30px;
            font-size: 18px;
            cursor: pointer;
            transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
        }

        #proposal-button:hover {
            transform: scale(1.05);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.1);
        }

        #proposal-button.clicked {
            background-color: #c2185b;
            transform: scale(0.95);
        }

        #proposal-response {
            margin-top: 20px;
            font-size: 1.2em;
            font-weight: bold;
        }

        audio {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div id="heart-container"></div>
    <div id="content">
        <h1>Um Pedido Especial para Você</h1>

        <div id="upload-section">
            <h2>Nossas Lembranças</h2>
            <input type="file" id="image-upload" multiple accept="image/*">
            <p>Dicas para o Upload:</p>
            <ul>
                <li>Tamanho das Fotos: Use imagens pequenas (redimensione para 500x500 pixels ou menos).</li>
                <li>Formatos: JPG, PNG e outros formatos são aceitos.</li>
                <li>Privacidade: As fotos são salvas localmente no seu navegador.</li>
            </ul>
            <div id="image-grid">
                </div>
        </div>

        <div id="message-section">
            <h2>Minha Mensagem</h2>
            <textarea id="message" rows="4" placeholder="Digite aqui a sua mensagem especial..."></textarea>
        </div>

        <button id="proposal-button">
            <span id="button-text">Quer namorar comigo?</span>
            <span id="heart-effect" style="display: none; font-size: 1.5em;">❤️</span>
        </button>
        <div id="proposal-response"></div>

        <audio controls loop>
            <source src="romantic_music.mp3" type="audio/mpeg">
            Seu navegador não suporta o elemento de áudio.
        </audio>
    </div>

    <script>
        const imageUpload = document.getElementById('image-upload');
        const imageGrid = document.getElementById('image-grid');
        const messageInput = document.getElementById('message');
        const proposalButton = document.getElementById('proposal-button');
        const proposalResponse = document.getElementById('proposal-response');
        const heartContainer = document.getElementById('heart-container');
        const buttonText = document.getElementById('button-text');
        const heartEffect = document.getElementById('heart-effect');

        let images = [];

        // Carregar imagens salvas localmente
        if (localStorage.getItem('romantic_images')) {
            images = JSON.parse(localStorage.getItem('romantic_images'));
            displayImages();
        }

        // Carregar mensagem salva localmente
        if (localStorage.getItem('romantic_message')) {
            messageInput.value = localStorage.getItem('romantic_message');
        }

        imageUpload.addEventListener('change', (event) => {
            const files = event.target.files;
            for (const file of files) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    images.push(e.target.result);
                    localStorage.setItem('romantic_images', JSON.stringify(images));
                    displayImages();
                };
                reader.readAsDataURL(file);
            }
            // Limpar o input para permitir carregar a mesma imagem novamente
            imageUpload.value = '';
        });

        messageInput.addEventListener('input', () => {
            localStorage.setItem('romantic_message', messageInput.value);
        });

        function displayImages() {
            imageGrid.innerHTML = '';
            images.forEach(imageUrl => {
                const container = document.createElement('div');
                container.classList.add('image-container');
                const img = document.createElement('img');
                img.src = imageUrl;
                container.appendChild(img);
                imageGrid.appendChild(container);
            });
        }

        function createHeart() {
            const heart = document.createElement('div');
            heart.classList.add('heart');
            heart.innerHTML = '❤️';
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.animationDuration = Math.random() * 3 + 2 + 's';
            heart.style.fontSize = Math.random() * 20 + 15 + 'px';
            heartContainer.appendChild(heart);

            setTimeout(() => {
                heart.remove();
            }, 5000);
        }

        setInterval(createHeart, 200); // Cria corações a cada 200ms

        proposalButton.addEventListener('click', () => {
            proposalButton.classList.add('clicked');
            buttonText.style.display = 'none';
            heartEffect.style.display = 'inline';

            setTimeout(() => {
                heartEffect.style.display = 'none';
                proposalButton.classList.remove('clicked');
                proposalResponse.textContent = 'Estou ansioso pela sua resposta! 🥰';
                // Aqui você pode adicionar mais ações, como redirecionar para outra página
            }, 2000);
        });
    </script>
</body>
</html>

