<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Beobachtung oder Bewertung</title>
<style>
    .correct {
        color: green;
    }
    .incorrect {
        color: red;
    }
    .hidden {
        display: none;
    }
</style>
</head>
<body>
<h2>Beobachtung oder Bewertung</h2>
<div id="test-container"></div>
<button onclick="submitTest()">Test abschließen</button>
<p id="final-result"></p>

<script>
const sentences = [
    { text: "Er ist chaotisch.", correctAnswer: "bewertung", example: "Sein Schreibtisch ist mit Stapeln von ungeordneten Papieren bedeckt." },
    { text: "Sie hat den Raum betreten, ohne jemanden zu grüßen.", correctAnswer: "beobachtung", example: "" },
    { text: "Er ist faul.", correctAnswer: "bewertung", example: "Er hat von 9 Uhr morgens bis 17 Uhr abends auf der Couch gelegen." },
    { text: "Sie spricht ständig über ihre Erfolge.", correctAnswer: "bewertung", example: "Sie hat heute 3 Mal von ihrem Bestseller erzählt" },
    { text: "Er kam 20 Minuten später als verabredet zum Meeting.", correctAnswer: "beobachtung", example: "" },
    { text: "Sie ist verantwortungslos.", correctAnswer: "bewertung", example: "Sie hat gestern Abend den Schlüssel im Türschloss stecken lassen." },
    { text: "Er spricht nicht mit ihr seit ihrem Streit vor einem Monat.", correctAnswer: "beobachtung", example: "" },
    { text: "Sie hat diese Woche drei neue, Kleider gekauft.", correctAnswer: "beobachtung", example: "" },
    { text: "Er ist egoistisch.", correctAnswer: "bewertung", example: "Er nahm sich die größte Portion vom Essen." },
    { text: "Sie hat gestern Nacht um 24 Uhr laute Musik gespielt, während alle anderen geschlafen haben.", correctAnswer: "beobachtung", example: "" }
];

function createTest() {
    const container = document.getElementById('test-container');
    sentences.forEach((sentence, index) => {
        const div = document.createElement('div');
        div.className = 'sentence-block';
        div.innerHTML = `
            <p>${index + 1}. ${sentence.text}</p>
            <input type="radio" id="bewertung${index}" name="choice${index}" value="bewertung">
            <label for="bewertung${index}">Bewertung</label>
            <input type="radio" id="beobachtung${index}" name="choice${index}" value="beobachtung">
            <label for="beobachtung${index}">Beobachtung</label><br>
            <textarea id="observation${index}" class="hidden" placeholder="Formuliere eine Beobachtung..."></textarea>
            <button onclick="checkAnswer(${index})">Überprüfen</button>
            <p id="result${index}"></p>
            <p id="example${index}" class="hidden"></p>
        `;
        container.appendChild(div);
    });
}

function checkAnswer(index) {
    const sentence = sentences[index];
    const chosenOption = document.querySelector(`input[name="choice${index}"]:checked`);
    const result = document.getElementById(`result${index}`);
    const example = document.getElementById(`example${index}`);
    const observationTextarea = document.getElementById(`observation${index}`);

    if (!chosenOption) {
        result.innerText = "Bitte wähle eine Option.";
        result.className = "";
        return;
    }

    if (chosenOption.value === sentence.correctAnswer) {
        result.innerText = "Richtig!";
        result.className = "correct";
    } else {
        result.innerText = "Falsch!";
        result.className = "incorrect";
    }

    example.innerText = `Eine mögliche Beobachtung wäre: ${sentence.example}`;
    example.classList.remove("hidden");

    if (chosenOption.value === "bewertung") {
        observationTextarea.classList.remove("hidden");
    } else {
        observationTextarea.classList.add("hidden");
    }
}

function submitTest() {
    const finalResult = document.getElementById("final-result");
    let score = 0;
    
    sentences.forEach((sentence, index) => {
        const chosenOption = document.querySelector(`input[name="choice${index}"]:checked`);
        if (chosenOption && chosenOption.value === sentence.correctAnswer) {
            score++;
        }
    });

    finalResult.innerText = `Sie haben ${score} von ${sentences.length} richtig beantwortet.`;
}

createTest();
</script>
</body>
</html>
