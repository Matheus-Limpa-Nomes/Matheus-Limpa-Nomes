<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Quiz Interativo - WhatsApp</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            text-align: center;
            padding: 50px;
        }
        .container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.1);
            max-width: 400px;
            margin: auto;
        }
        h2 {
            color: #333;
        }
        label, p {
            font-weight: bold;
            color: #555;
        }
        input, select, button {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            border-radius: 5px;
            border: 1px solid #ccc;
        }
        button {
            background-color: #28a745;
            color: white;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #218838;
        }
        .question {
            display: none;
        }
        .question.active {
            display: block;
        }
        .progress-bar {
            width: 100%;
            background-color: #ddd;
            height: 10px;
            border-radius: 5px;
            margin-bottom: 15px;
        }
        .progress {
            height: 10px;
            width: 0%;
            background-color: #28a745;
            border-radius: 5px;
        }
        .result {
            display: none;
            padding: 20px;
            background: #e9ffe9;
            border-radius: 10px;
            margin-top: 20px;
        }
    </style>
    <script>
        let step = 0;
        let nome = "", tipoDivida = "", valorDivida = "", score = 0;
        
        function nextStep() {
            let questions = document.getElementsByClassName("question");
            if (step < questions.length - 1) {
                questions[step].classList.remove("active");
                step++;
                questions[step].classList.add("active");
                updateProgress();
            } else {
                calcularScore();
            }
        }
        
        function updateProgress() {
            let progress = document.getElementById("progress");
            let totalSteps = document.getElementsByClassName("question").length;
            progress.style.width = ((step / (totalSteps - 1)) * 100) + "%";
        }
        
        document.addEventListener("DOMContentLoaded", function() {
            let questions = document.getElementsByClassName("question");
            questions[0].classList.add("active");
        });
        
        function calcularScore() {
            nome = document.getElementById("nome").value;
            tipoDivida = document.getElementById("tipoDivida").value;
            valorDivida = document.getElementById("valor").value;
            
            let scoreBase = {"Abaixo de R$7.000,00": 92, "De R$7.000 a R$20.000": 90, "De R$20.000 a R$35.000": 95, "De R$35.000 a R$50.000": 95, "De R$50.000 a R$80.000": 98, "De R$80.000 a R$100.000": 90, "Acima de R$100.000,00": 95};
            score = scoreBase[valorDivida] || 50;
            
            document.getElementById("quiz-container").style.display = "none";
            document.getElementById("result-container").style.display = "block";
            document.getElementById("result-message").innerHTML = Parabéns, ${nome}! Seu perfil se encaixa em <strong>${score}%</strong> dos casos que conseguimos resolver com sucesso! 🎯 Vamos dar o próximo passo?;
        }
        
        function enviarParaWhatsApp() {
            let mensagem = Olá, meu nome é ${nome}, tenho dívidas no ${tipoDivida} no valor aproximado de ${valorDivida}. Gostaria de saber mais informações sobre o processo de vocês...;
            let telefoneWhatsApp = "5511973293775";
            let url = https://wa.me/${telefoneWhatsApp}?text=${encodeURIComponent(mensagem)};
            window.location.href = url;
        }
    </script>
</head>
<body>
    <div class="container" id="quiz-container">
        <h2>New Life Consultoria Financeira - Recuperamos seu crédito com rapidez e segurança!</h2>
        <p>Responda essas 3 perguntas rápidas e fale com um especialista agora mesmo!</p>
        
        <div class="progress-bar">
            <div id="progress" class="progress"></div>
        </div>
        
        <div class="question active">
            <label for="nome">Qual seu nome?</label>
            <input type="text" id="nome" required>
            <button onclick="nextStep()">Próxima</button>
        </div>
        
        <div class="question">
            <label for="tipoDivida">Suas dívidas estão em:</label>
            <select id="tipoDivida">
                <option value="CPF">CPF</option>
                <option value="CNPJ">CNPJ</option>
                <option value="CPF e CNPJ">CPF e CNPJ</option>
            </select>
            <button onclick="nextStep()">Próxima</button>
        </div>
        
        <div class="question">
            <label for="valor">Qual o valor aproximado?</label>
            <select id="valor">
                <option value="Abaixo de R$7.000">Abaixo de R$7.000</option>
                <option value="De R$7.000 a R$20.000">De R$7.000 a R$20.000</option>
                <option value="De R$20.000 a R$35.000">De R$20.000 a R$35.000</option>
                <option value="De R$35.000 a R$50.000">De R$35.000 a R$50.000</option>
                <option value="De R$50.000 a R$80.000">De R$50.000 a R$80.000</option>
                <option value="De R$80.000 a R$100.000">De R$80.000 a R$100.000</option>
                 <option value="Acima de R$100.000">Acima de R$100.000</option>
            </select>
            <button onclick="calcularScore()">Ver resultado</button>
        </div>
    </div>
    <div class="container result" id="result-container">
        <p id="result-message"></p>
        <button onclick="enviarParaWhatsApp()">🚀 Converse agora com um atendente!</button>
    </div>
</body>
</html>
