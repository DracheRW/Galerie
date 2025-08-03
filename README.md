
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Private Galerie</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f4f4f4; margin: 0; padding: 20px; }
        h1 { text-align: center; }
        .gallery { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 15px; }
        .gallery img { width: 100%; border-radius: 8px; cursor: pointer; transition: 0.3s; }
        .gallery img:hover { transform: scale(1.05); }
        .download-btn { display: block; text-align: center; margin-top: 10px; background: #333; color: white; padding: 8px; border-radius: 5px; text-decoration: none; }
        .download-btn:hover { background: #555; }
    </style>
</head>
<body>
    <script>
        // Einfacher Passwortschutz (nur Client-Seite)
        const pass = prompt("Passwort eingeben:");
        if(pass !== "4705") { 
            document.body.innerHTML = "<h1>Zugriff verweigert</h1>";
            throw new Error("Falsches Passwort");
        }
    </script>

    <h1>Galerie</h1>
    <div class="gallery" id="gallery"></div>

    <script>
        async function loadImages() {
            const response = await fetch('https://api.github.com/repos/USERNAME/REPO/contents/images');
            const files = await response.json();
            const gallery = document.getElementById('gallery');
            files.forEach(file => {
                if(file.name.match(/\.(jpg|jpeg|png|gif)$/i)) {
                    const container = document.createElement('div');
                    const image = document.createElement('img');
                    image.src = file.download_url;
                    const link = document.createElement('a');
                    link.href = file.download_url;
                    link.download = file.name;
                    link.className = 'download-btn';
                    link.innerText = 'Download';
                    container.appendChild(image);
                    container.appendChild(link);
                    gallery.appendChild(container);
                }
            });
        }
        loadImages();
    </script>
</body>
</html>
