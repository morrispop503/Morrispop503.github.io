<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Spellen</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f5f5f5;
            margin: 0;
            padding: 0;
        }

        .container {
            background-color: #fff;
            max-width: 400px;
            margin: 20px auto;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
        }

        h1 {
            color: #007bff;
        }

        p {
            font-size: 1.2em;
        }

        input {
            padding: 10px;
            font-size: 1.2em;
            border: 1px solid #ccc;
            width: 50%;
        }

        button {
            padding: 10px 20px;
            font-size: 1.2em;
            background-color: #007bff;
            color: #fff;
            border: none;
            cursor: pointer;
            margin-top: 10px;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #0056b3;
        }

        #message {
            font-size: 1.2em;
            color: #333;
            margin-top: 10px;
        }

        #word {
            font-size: 1.5em;
            font-weight: bold;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="container" id="container1">
        <h1>Raad het Getal</h1>
        <p>Ik heb een getal tussen 1 en 100 in gedachten. Probeer het te raden.</p>
        <input type="number" id="guess" placeholder="Doe een gok">
        <button id="check">Controleer</button>
        <button id="restart" style="display:none;">Opnieuw starten</button>
        <p id="message"></p>
    </div>

    <div class="container" id="container2">
        <h1>Galgje</h1>
        <p>Probeer het woord te raden door letters in te voeren.</p>
        <input type="text" id="letter" placeholder="Voer een letter in">
        <button id="guess2">Raad</button>
        <p id="message2"></p>
        <div id="word"></div>
        <button id="restart2" style="display:none;">Opnieuw starten</button>
    </div>

    <div class="container" id="container3">
        <h1>Woord Puzzel</h1>
        <p>Probeer het woord te raden door letters in te voeren.</p>
        <input type="text" id="guess3" placeholder="Voer een letter in">
        <button id="check3">Controleer</button>
        <button id="restart3" style="display:none;">Opnieuw starten</button>
        <button id="hint">Hint</button>
        <p id="message3"></p>
    </div>

    <script>
        // Raad het Getal spel
        const geheimGetal = Math.floor(Math.random() * 100) + 1;
        let aantalPogingen = 0;
        const guessInput = document.getElementById('guess');
        const checkButton = document.getElementById('check');
        const restartButton = document.getElementById('restart');
        const message = document.getElementById('message');
        checkButton.addEventListener('click', checkGuess);
        function checkGuess() {
            const gok = parseInt(guessInput.value);
            if (isNaN(gok) || gok < 1 || gok > 100) {
                message.textContent = 'Voer een geldig getal in tussen 1 en 100.';
                return;
            }
            aantalPogingen++;
            if (gok < geheimGetal) {
                message.textContent = 'Het geheime getal is hoger. Probeer opnieuw.';
            } else if (gok > geheimGetal) {
                message.textContent = 'Het geheime getal is lager. Probeer opnieuw.';
            } else {
                message.textContent = `Gefeliciteerd! Je hebt het geheime getal ${geheimGetal} geraden in ${aantalPogingen} pogingen.`;
                guessInput.setAttribute('disabled', 'true');
                checkButton.setAttribute('disabled', 'true');
                guessInput.style.backgroundColor = '#ccc';
                checkButton.style.backgroundColor = '#ccc';
                restartButton.style.display = 'inline-block';
            }
            guessInput.value = '';
        }
        restartButton.addEventListener('click', function() {
            location.reload();
        });

        // Galgje spel
        const woordenlijst2 = ["programmeren", "javascript", "computer", "webontwikkeling", "galgje", "html", "css"];
        let geheimWoord2 = woordenlijst2[Math.floor(Math.random() * woordenlijst2.length)];
        let geradenLetters2 = [];
        let maxFouten = 6;
        let fouten = 0;
        const letterInput2 = document.getElementById('letter');
        const raadButton = document.getElementById('guess2');
        const restartButton2 = document.getElementById('restart2');
        const hintButton2 = document.getElementById('hint');
        const message2 = document.getElementById('message2');
        const wordDisplay = document.getElementById('word');
        raadButton.addEventListener('click', raadLetter);
        function raadLetter() {
            const letter = letterInput2.value.toLowerCase();
            if (!letter.match(/[a-z]/i)) {
                message2.textContent = 'Voer een geldige letter in.';
                return;
            }
            if (geradenLetters2.includes(letter)) {
                message2.textContent = 'Je hebt deze letter al geraden.';
                return;
            }
            if (geheimWoord2.includes(letter)) {
                geradenLetters2.push(letter);
                updateDisplay();
                if (isWoordGeraden()) {
                    win();
                }
            } else {
                geradenLetters2.push(letter);
                fouten++;
                updateDisplay();
                if (fouten === maxFouten) {
                    verlies();
                }
            }
            letterInput2.value = '';
        }
        function updateDisplay() {
            let weergaveWoord = '';
            for (const letter of geheimWoord2) {
                if (geradenLetters2.includes(letter.toLowerCase())) {
                    weergaveWoord += letter;
                } else {
                    weergaveWoord += '_';
                }
            }
            wordDisplay.textContent = weergaveWoord;
            message2.textContent = `Fouten: ${fouten}/${maxFouten}`;
        }
        function isWoordGeraden() {
            return geheimWoord2.split('').every(letter => geradenLetters2.includes(letter.toLowerCase()));
        }
        function win() {
            message2.textContent = 'Gefeliciteerd! Je hebt het woord geraden!';
            raadButton.setAttribute('disabled', 'true');
            restartButton2.style.display = 'inline-block';
            hintButton2.setAttribute('disabled', 'true'); // Schakel hint-knop uit bij winst
        }
        function verlies() {
            message2.textContent = `Helaas, je hebt het woord niet geraden. Het juiste woord was "${geheimWoord2}".`;
            raadButton.setAttribute('disabled', 'true');
            restartButton2.style.display = 'inline-block';
            hintButton2.setAttribute('disabled', 'true'); // Schakel hint-knop uit bij verlies
        }
        restartButton2.addEventListener('click', function() {
            location.reload();
        });

        // Woord Puzzel spel
        const woordlijst3 = ["professioneel", "uitdaging", "creativiteit", "verrassing", "experimenteren"];
        let geheimWoord3 = woordlijst3[Math.floor(Math.random() * woordlijst3.length)].toLowerCase();
        let geradenLetters3 = new Set();
        let fouten3 = 0;
        let hints = 1; // Start met 1 hint
        const letterInput3 = document.getElementById('guess3');
        const checkButton3 = document.getElementById('check3');
        const restartButton3 = document.getElementById('restart3');
        const hintButton3 = document.getElementById('hint');
        const message3 = document.getElementById('message3');
        checkButton3.addEventListener('click', checkGuess3);
        hintButton3.addEventListener('click', geefHint); // Voeg een eventlistener toe voor de hint-knop
        function geefHint() {
            if (hints > 0) {
                // Toon een willekeurige hint uit het geheime woord
                const nogNietGeraden = geheimWoord3.split('').filter(letter => !geradenLetters3.has(letter));
                const willekeurigeHint = nogNietGeraden[Math.floor(Math.random() * nogNietGeraden.length)];
                message3.textContent = `Hint: ${willekeurigeHint}`;
                hints--; // Verminder het aantal hints
            } else {
                message3.textContent = 'Je hebt geen hints meer over.';
            }
        }
        function checkGuess3() { // Definieer checkGuess3-functie
            const gok3 = letterInput3.value.toLowerCase();
            if (!gok3.match(/[a-z]/i)) {
                message3.textContent = 'Voer een geldige letter in.';
                return;
            }
            if (geradenLetters3.has(gok3)) {
                message3.textContent = 'Je hebt deze letter al geraden.';
                return;
            }
            geradenLetters3.add(gok3);
            if (!geheimWoord3.includes(gok3)) {
                fouten3++;
            }
            updateDisplay3();
        }
        function updateDisplay3() {
            let weergaveWoord = '';
            for (const letter of geheimWoord3) {
                if (geradenLetters3.has(letter.toLowerCase())) {
                    weergaveWoord += letter;
                } else {
                    weergaveWoord += '_';
                }
            }
            message3.textContent = `Fouten: ${fouten3}/${geheimWoord3.length}`;
            if (fouten3 === geheimWoord3.length) {
                verlies3();
            } else if (!weergaveWoord.includes('_')) {
                win3();
            }
        }
        function win3() {
            message3.textContent = 'Gefeliciteerd! Je hebt het woord geraden!';
            checkButton3.setAttribute('disabled', 'true');
            restartButton3.style.display = 'inline-block';
            hintButton3.setAttribute('disabled', 'true'); // Schakel hint-knop uit bij winst
        }
        function verlies3() {
            message3.textContent = `Helaas, je hebt het woord niet geraden. Het juiste woord was "${geheimWoord3}".`;
            checkButton3.setAttribute('disabled', 'true');
            restartButton3.style.display = 'inline-block';
            hintButton3.setAttribute('disabled', 'true'); // Schakel hint-knop uit bij verlies
        }
        restartButton3.addEventListener('click', function() {
            location.reload();
        });
    </script>
</body>
</html>

