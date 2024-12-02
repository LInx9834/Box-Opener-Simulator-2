# Box-Opener-Simulator-2
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shop</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
            background-color: #000;
            color: white;
        }
        h1 {
            color: #fff;
            margin: 20px 0;
        }
        .container {
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
            background: #333;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
            border-radius: 8px;
        }
        button {
            font-size: 18px;
            padding: 10px 20px;
            margin: 10px;
            border: none;
            border-radius: 5px;
            background-color: #007BFF;
            color: white;
            cursor: pointer;
        }
        button:hover:not(:disabled) {
            background-color: #0056b3;
        }
        #closeButton {
            position: absolute;
            top: 10px;
            right: 20px;
            font-size: 30px;
            color: #fff;
            background: transparent;
            border: none;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Shop</h1>
        <p>Münzen: <span id="coins">0</span></p>
        <button id="buyNormalBox">Normale Box (2 Münzen)</button>
        <button id="buyRareBox">Seltene Box (5 Münzen)</button>
        <button id="buyDuplikator">Duplikator (50 Münzen)</button>
        <button id="claimFreeBox">Gratis Box (Täglich verfügbar)</button>
        <button id="backToGame">Zurück zum Spiel</button>
        <p id="result"></p>
    </div>

    <button id="closeButton">X</button>

    <script>
        let coins = parseInt(localStorage.getItem("coins")) || 10;
        let duplikator = localStorage.getItem("duplikator") === "true";
        const coinsElement = document.getElementById("coins");
        const resultElement = document.getElementById("result");

        const updateCoins = (amount) => {
            coins += amount;
            if (coins < 0) coins = 0;
            coinsElement.textContent = coins;
            localStorage.setItem("coins", coins);
        };

        const updateDuplikator = () => {
            localStorage.setItem("duplikator", duplikator);
        };

        const buyItem = (cost, item) => {
            if (coins >= cost) {
                updateCoins(-cost);
                resultElement.textContent = `Du hast ${item} für ${cost} Münzen gekauft!`;
                if (item === "Duplikator") {
                    duplikator = true;
                    updateDuplikator();
                }
            } else {
                resultElement.textContent = "Nicht genug Münzen!";
            }
        };

        const openFreeBox = () => {
            // Gratis Box öffnen (keine Münzen)
            resultElement.textContent = "Du hast die Gratis Box geöffnet! Du hast 5 Münzen erhalten!";
            updateCoins(5);
        };

        document.getElementById("buyNormalBox").addEventListener("click", () => {
            if (coins >= 2) {
                updateCoins(-2);
                resultElement.textContent = "Du hast eine normale Box geöffnet!";
                // Boxeninhalt anzeigen, z.B. Münzen
            }
        });

        document.getElementById("buyRareBox").addEventListener("click", () => {
            if (coins >= 5) {
                updateCoins(-5);
                resultElement.textContent = "Du hast eine seltene Box geöffnet!";
                // Boxeninhalt anzeigen, z.B. Münzen oder Gegenstände
            }
        });

        document.getElementById("buyDuplikator").addEventListener("click", () => {
            buyItem(50, "Duplikator");
        });

        document.getElementById("claimFreeBox").addEventListener("click", () => {
            openFreeBox();
        });

        document.getElementById("backToGame").addEventListener("click", () => {
            window.location.href = "game.html"; // Zurück ins Spiel
        });

        document.getElementById("closeButton").addEventListener("click", () => {
            window.location.href = "index.html"; // Geht zurück zur Startseite
        });

        coinsElement.textContent = coins;
    </script>
</body>
</html>
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shop</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            text-align: center;
            background-color: #000;
            color: white;
        }
        h1 {
            color: #fff;
            margin: 20px 0;
        }
        .container {
            max-width: 600px;
            margin: 20px auto;
            padding: 20px;
            background: #333;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
            border-radius: 8px;
        }
        button {
            font-size: 18px;
            padding: 10px 20px;
            margin: 10px;
            border: none;
            border-radius: 5px;
            background-color: #007BFF;
            color: white;
            cursor: pointer;
        }
        button:hover:not(:disabled) {
            background-color: #0056b3;
        }
        #closeButton {
            position: absolute;
            top: 10px;
            right: 20px;
            font-size: 30px;
            color: #fff;
            background: transparent;
            border: none;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Shop</h1>
        <p>Münzen: <span id="coins">0</span></p>
        <button id="buyNormalBox">Normale Box (2 Münzen)</button>
        <button id="buyRareBox">Seltene Box (5 Münzen)</button>
        <button id="buyDuplikator">Duplikator (50 Münzen)</button>
        <button id="claimFreeBox">Gratis Box (Täglich verfügbar)</button>
        <button id="claimCoins">100 Münzen abholen (alle 10 Minuten)</button>
        <button id="backToGame">Zurück zum Spiel</button>
        <p id="result"></p>
        <p id="timer"></p>
    </div>

    <button id="closeButton">X</button>

    <script>
        let coins = parseInt(localStorage.getItem("coins")) || 10;
        let duplikator = localStorage.getItem("duplikator") === "true";
        const coinsElement = document.getElementById("coins");
        const resultElement = document.getElementById("result");
        const timerElement = document.getElementById("timer");

        const updateCoins = (amount) => {
            coins += amount;
            if (coins < 0) coins = 0;
            coinsElement.textContent = coins;
            localStorage.setItem("coins", coins);
        };

        const updateDuplikator = () => {
            localStorage.setItem("duplikator", duplikator);
        };

        const buyItem = (cost, item) => {
            if (coins >= cost) {
                updateCoins(-cost);
                resultElement.textContent = `Du hast ${item} für ${cost} Münzen gekauft!`;
                if (item === "Duplikator") {
                    duplikator = true;
                    updateDuplikator();
                }
            } else {
                resultElement.textContent = "Nicht genug Münzen!";
            }
        };

        const openFreeBox = () => {
            // Gratis Box öffnen (keine Münzen)
            resultElement.textContent = "Du hast die Gratis Box geöffnet! Du hast 5 Münzen erhalten!";
            updateCoins(5);
        };

        // Funktion um die Zeit zu überprüfen und die Münzen zu vergeben
        const checkTimeForCoins = () => {
            const lastClaimTime = parseInt(localStorage.getItem("lastClaimTime")) || 0;
            const currentTime = Date.now();
            const timeDifference = currentTime - lastClaimTime;
            const remainingTime = 600000 - timeDifference; // 10 Minuten in Millisekunden

            if (remainingTime <= 0) {
                // Wenn 10 Minuten vergangen sind
                document.getElementById("claimCoins").disabled = false;
                document.getElementById("timer").textContent = "Du kannst jetzt 100 Münzen abholen!";
            } else {
                const minutesRemaining = Math.ceil(remainingTime / 60000);
                document.getElementById("claimCoins").disabled = true;
                document.getElementById("timer").textContent = `Noch ${minutesRemaining} Minuten bis du 100 Münzen abholen kannst.`;
            }
        };

        const claimCoins = () => {
            updateCoins(100); // 100 Münzen vergeben
            const currentTime = Date.now();
            localStorage.setItem("lastClaimTime", currentTime); // Speichern des aktuellen Zeitpunkts
            checkTimeForCoins(); // Zeit neu berechnen
        };

        // Boxen-Kauf-Logik
        document.getElementById("buyNormalBox").addEventListener("click", () => {
            if (coins >= 2) {
                updateCoins(-2);
                resultElement.textContent = "Du hast eine normale Box geöffnet!";
                // Boxeninhalt anzeigen, z.B. Münzen
            }
        });

        document.getElementById("buyRareBox").addEventListener("click", () => {
            if (coins >= 5) {
                updateCoins(-5);
                resultElement.textContent = "Du hast eine seltene Box geöffnet!";
                // Boxeninhalt anzeigen, z.B. Münzen oder Gegenstände
            }
        });

        document.getElementById("buyDuplikator").addEventListener("click", () => {
            buyItem(50, "Duplikator");
        });

        document.getElementById("claimFreeBox").addEventListener("click", () => {
            openFreeBox();
        });

        document.getElementById("claimCoins").addEventListener("click", () => {
            claimCoins();
        });

        document.getElementById("backToGame").addEventListener("click", () => {
            window.location.href = "game.html"; // Zurück ins Spiel
        });

        document.getElementById("closeButton").addEventListener("click", () => {
            window.location.href = "index.html"; // Geht zurück zur Startseite
        });

        coinsElement.textContent = coins;
        checkTimeForCoins(); // Beim Laden der Seite die Zeit für die Münzenüberprüfung aufrufen
    </script>
</body>
</html>
