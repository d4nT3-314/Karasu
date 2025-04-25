<!DOCTYPE html><html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Calculadora de Conceito</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 600px;
      margin: 0 auto;
      padding: 20px;
    }
    h1 {
      text-align: center;
    }
    label, input, select {
      display: block;
      margin-top: 10px;
      width: 100%;
    }
    button {
      margin-top: 20px;
      padding: 10px;
      background-color: #4CAF50;
      color: white;
      border: none;
      cursor: pointer;
    }
    #resultado {
      margin-top: 20px;
      padding: 15px;
      background-color: #f2f2f2;
      border-radius: 5px;
    }
  </style>
</head>
<body>
  <h1>Calculadora de Conceito</h1>
  <label for="avaliacao">Em qual avaliação você está? (1, 2 ou 3)</label>
  <select id="avaliacao">
    <option value="1">1</option>
    <option value="2">2</option>
    <option value="3">3</option>
  </select>  <div id="notas"></div>
  <button onclick="calcularResultado()">Calcular Resultado</button>  <div id="resultado"></div>  <script>
    document.getElementById("avaliacao").addEventListener("change", gerarCamposDeNota);

    function gerarCamposDeNota() {
      const container = document.getElementById("notas");
      container.innerHTML = "";
      const avaliacao = parseInt(document.getElementById("avaliacao").value);

      for (let i = 1; i <= avaliacao; i++) {
        const label = document.createElement("label");
        label.textContent = `Nota da Avaliação ${i}`;
        const input = document.createElement("input");
        input.type = "number";
        input.id = `nota${i}`;
        input.min = 0;
        input.max = 10;
        input.step = 0.1;
        container.appendChild(label);
        container.appendChild(input);
      }
    }

    function calcularConceito(nota) {
      if (nota < 15) return "Insuficiente";
      if (nota < 21) return "Regular";
      if (nota < 27) return "Bom";
      return "Excelente";
    }

    function calcularResultado() {
      const avaliacao = parseInt(document.getElementById("avaliacao").value);
      let notaTotal = 0;

      for (let i = 1; i <= avaliacao; i++) {
        const nota = parseFloat(document.getElementById(`nota${i}`).value) || 0;
        notaTotal += nota;
      }

      const conceito = calcularConceito(notaTotal);
      const media = (notaTotal / 3).toFixed(2);

      let resultado = `<strong>Seu conceito até o momento é:</strong> ${conceito} <br> Média: ${media}<br>`;

      if (conceito !== "Excelente") {
        resultado += "<br>Para alcançar os próximos conceitos você precisa de:";
        if (conceito === "Insuficiente") resultado += `<br>- Regular: ${(15 - notaTotal).toFixed(2)} pontos`;
        if (["Insuficiente", "Regular"].includes(conceito)) resultado += `<br>- Bom: ${(21 - notaTotal).toFixed(2)} pontos`;
        if (["Insuficiente", "Regular", "Bom"].includes(conceito)) resultado += `<br>- Excelente: ${(27 - notaTotal).toFixed(2)} pontos`;
      } else {
        resultado += "<br>Parabéns! Você já foi aprovado com Excelente.";
      }

      document.getElementById("resultado").innerHTML = resultado;
    }

    gerarCamposDeNota(); // inicializa com campos de nota para avaliação 1
  </script></body>
</html>
