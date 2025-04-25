<!DOCTYPE html><html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Conceito</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f4f8;
            padding: 20px;
            max-width: 600px;
            margin: auto;
        }
        h1 {
            color: #333;
        }
        label, input, select, button {
            display: block;
            margin: 10px 0;
            width: 100%;
            font-size: 1rem;
        }
        .resultado {
            background-color: #fff;
            padding: 15px;
            margin-top: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
    </style>
</head>
<body>
    <h1>Calculadora de Conceito</h1>
    <label for="avaliacao">Em qual avaliação você está? (1, 2 ou 3):</label>
    <select id="avaliacao">
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
    </select><div id="notas"></div>
<button onclick="calcular()">Calcular Conceito</button>

<div id="resultado" class="resultado"></div>

<script>
    const avaliacaoSelect = document.getElementById('avaliacao');
    const notasDiv = document.getElementById('notas');

    avaliacaoSelect.addEventListener('change', atualizarCampos);

    function atualizarCampos() {
        const quant = parseInt(avaliacaoSelect.value);
        notasDiv.innerHTML = '';
        for (let i = 1; i <= quant; i++) {
            notasDiv.innerHTML += `
                <label for="nota${i}">Nota da Avaliação ${i}:</label>
                <input type="number" id="nota${i}" min="0" max="10" step="0.1">
            `;
        }
    }

    function calcular() {
        const quant = parseInt(avaliacaoSelect.value);
        let notaTotal = 0;
        for (let i = 1; i <= quant; i++) {
            const nota = parseFloat(document.getElementById(`nota${i}`).value) || 0;
            notaTotal += nota;
        }

        let conceito = '';
        if (notaTotal < 15) conceito = 'insuficiente';
        else if (notaTotal < 21) conceito = 'regular';
        else if (notaTotal < 27) conceito = 'bom';
        else conceito = 'excelente';

        const media = (notaTotal / quant).toFixed(2);

        let mensagem = `<p>Seu conceito até o momento é <strong>${conceito}</strong> com média <strong>${media}</strong>.</p>`;
        if (conceito !== 'excelente') {
            mensagem += `<p>Para alcançar os próximos conceitos você precisa de:</p><ul>`;
            if (conceito === 'insuficiente') mensagem += `<li>Regular: ${(15 - notaTotal).toFixed(2)} pontos</li>`;
            if (['insuficiente', 'regular'].includes(conceito)) mensagem += `<li>Bom: ${(21 - notaTotal).toFixed(2)} pontos</li>`;
            if (['insuficiente', 'regular', 'bom'].includes(conceito)) mensagem += `<li>Excelente: ${(27 - notaTotal).toFixed(2)} pontos</li>`;
            mensagem += `</ul>`;
        } else {
            mensagem += `<p>Parabéns! Você já foi aprovado com Excelente.</p>`;
        }

        document.getElementById('resultado').innerHTML = mensagem;
    }

    // Inicializar campos de nota com base no valor padrão
    atualizarCampos();
</script>

</body>
</html>
