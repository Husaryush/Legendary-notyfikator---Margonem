<!DOCTYPE html>
<html lang="pl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Music Player</title>

  <style>
    * {
      box-sizing: border-box;
    }

    html, body {
      margin: 0;
      width: 100%;
      height: 100%;
      background: #000;
      overflow: hidden;
      font-family: Arial, sans-serif;
    }

    #player {
      position: absolute;
      width: 1px;
      height: 1px;
      opacity: 0;
      pointer-events: none;
    }

    .container {
      width: 100%;
      height: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      color: white;
    }

    button {
      width: 90px;
      height: 90px;
      border: 2px solid white;
      border-radius: 50%;
      background: transparent;
      color: white;
      font-size: 35px;
      cursor: pointer;
      transition: 0.2s;
    }

    button:hover {
      background: white;
      color: black;
      transform: scale(1.05);
    }

    #status {
      margin-top: 25px;
      font-size: 14px;
      color: #888;
    }

    input[type="range"] {
      margin-top: 20px;
      width: 180px;
      accent-color: white;
    }
  </style>
</head>

<body>

  <div id="player"></div>

  <div class="container">
    <button id="playButton">▶</button>

    <div id="status">Kliknij Play</div>

    <input
      id="volume"
      type="range"
      min="0"
      max="100"
      value="100"
    >
  </div>

  <script>
    let player;

    const videoId = "n0QT_teLcNk";

    const playButton = document.getElementById("playButton");
    const status = document.getElementById("status");
    const volume = document.getElementById("volume");

    // Ładowanie YouTube IFrame API
    const tag = document.createElement("script");
    tag.src = "https://www.youtube.com/iframe_api";
    document.head.appendChild(tag);

    function onYouTubeIframeAPIReady() {
      player = new YT.Player("player", {
        height: "1",
        width: "1",
        videoId: videoId,

        playerVars: {
          autoplay: 0,
          controls: 0,
          disablekb: 1,
          fs: 0,
          modestbranding: 1,
          rel: 0,
          iv_load_policy: 3
        },

        events: {
          onReady: () => {
            player.setVolume(100);
            status.textContent = "Gotowe";
          },

          onStateChange: event => {
            if (event.data === YT.PlayerState.PLAYING) {
              playButton.textContent = "Ⅱ";
              status.textContent = "Odtwarzanie";
            }

            if (event.data === YT.PlayerState.PAUSED) {
              playButton.textContent = "▶";
              status.textContent = "Pauza";
            }

            if (event.data === YT.PlayerState.ENDED) {
              playButton.textContent = "▶";
              status.textContent = "Koniec";
            }
          }
        }
      });
    }

    playButton.addEventListener("click", () => {
      if (!player) return;

      const state = player.getPlayerState();

      if (
        state === YT.PlayerState.PLAYING
      ) {
        player.pauseVideo();
      } else {
        player.playVideo();
      }
    });

    volume.addEventListener("input", () => {
      if (player) {
        player.setVolume(volume.value);
      }
    });
  </script>

</body>
</html>
