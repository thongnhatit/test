<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Music Box</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            overflow: hidden;
            margin: 50px;
            background-color: #000000;
        }
        h1 {
            color: #333;
        }
        #playlist {
            margin-top: 20px;
            list-style: none;
            padding: 0;
        }
        .song-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #fff;
            padding: 10px;
            margin: 5px 0;
            border-radius: 5px;
            box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.2);
        }
        .delete-btn {
            background: red;
            color: white;
            border: none;
            padding: 5px 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        .delete-btn:hover {
            background: darkred;
        }
        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            border-radius: 50%;
            box-shadow: 0 0 5px white;
            animation: fall linear infinite;
        }
        @keyframes fall {
            0% {
                transform: translateY(0) translateX(0);
                opacity: 1;
            }
            100% {
                transform: translateY(100vh) translateX(50px);
                opacity: 0;
            }
        }
    </style>
</head>
<body>
    <h1>Music Box 🎵</h1>
    
    <input type="file" id="fileInput" accept="audio/*">
    <ul id="playlist"></ul>
    
    <audio id="audioPlayer" controls></audio>
    <script>
        function createStar() {
            const star = document.createElement("div");
            star.classList.add("star");

            // Vị trí ngẫu nhiên
            star.style.left = Math.random() * window.innerWidth + "px";
            star.style.top = Math.random() * -50 + "px"; // Xuất hiện từ trên cao
            star.style.animationDuration = (Math.random() * 2 + 1) + "s";

            document.body.appendChild(star);

            // Xóa sao sau khi rơi xong
            setTimeout(() => {
                star.remove();
            }, 3000);
        }

        // Tạo sao liên tục
        setInterval(createStar, 100);
    </script>
    <script>
        let playlist = [];

        document.getElementById("fileInput").addEventListener("change", function(event) {
            const file = event.target.files[0];
            if (file) {
                const url = URL.createObjectURL(file);
                playlist.push({ name: file.name, url: url });
                updatePlaylist();
            }
        });

        function updatePlaylist() {
            const list = document.getElementById("playlist");
            list.innerHTML = "";
            playlist.forEach((song, index) => {
                const li = document.createElement("li");
                li.classList.add("song-item");
                
                const songName = document.createElement("span");
                songName.textContent = song.name;
                songName.style.cursor = "pointer";
                songName.onclick = () => playSong(song.url);

                const deleteBtn = document.createElement("button");
                deleteBtn.textContent = "Xóa";
                deleteBtn.classList.add("delete-btn");
                deleteBtn.onclick = () => removeSong(index);

                li.appendChild(songName);
                li.appendChild(deleteBtn);
                list.appendChild(li);
            });
        }

        function playSong(url) {
            const player = document.getElementById("audioPlayer");
            player.src = url;
            player.play();
        }

        function removeSong(index) {
            playlist.splice(index, 1);
            updatePlaylist();
        }
    </script>
</body>
</html>
