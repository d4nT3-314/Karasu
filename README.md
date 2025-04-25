<!DOCTYPE html><html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Calculadora de Conceito</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      background-color: #f9f9f9;
    }
    .container {
      max-width: 500px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    }
    input, select, button {
      width: 100%;
      margin-top: 10px;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 8px;
    }
    button {
      background-color: #007BFF;
      color: white;
      border: none;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
    .resultado {
      margin-top: 20px;
      background: #f1f1f1;
      padding: 15px;
      border-radius: 8px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Calculadora de Conceito</h2>
    <label>Quantas avaliações você fez?</label>
    <select id="avaliacao">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select><div id="notas"></div>
<button onclick="calcular()">Calcular Conceito</button>

<div class="resultado" id="resultado"></div>

  </div>  <script>
    const select = document.getElementById("avaliacao");
    const notasDiv = document.getElementById("notas");
    select.addEventListener("change", atualizarCampos);

    function atualizarCampos() {
      notasDiv.innerHTML = "";
      const num = parseInt(select.value);
      for (let i = 1; i <= num; i++) {
        notasDiv.innerHTML += `
          <label>Nota da Avaliação ${i}</label>
          <input type="number" step="0.1" id="nota${i}" />
        `;
      }
    }

    function calcularConceito(notaTotal) {
      if (notaTotal < 15) return "insuficiente";
      else if (notaTotal < 21) return "regular";
      else if (notaTotal < 27) return "bom";
      else return "excelente";
    }

    function calcular() {
      const totalAvaliacoes = parseInt(select.value);
      let notaTotal = 0;
      for (let i = 1; i <= totalAvaliacoes; i++) {
        const nota = parseFloat(document.getElementById(`nota${i}`).value) || 0;
        notaTotal += nota;
      }

      const media = (notaTotal / 3).toFixed(2);
      const conceito = calcularConceito(notaTotal);

      let msg = `<p>Seu conceito é <strong>${conceito}</strong> com média <strong>${media}</strong>.</p>`;

      if (conceito !== "excelente") {
        msg += `<p>Para alcançar os próximos conceitos, você precisa de:</p><ul>`;
        if (conceito === "insuficiente") msg += `<li>Regular: ${(15 - notaTotal).toFixed(2)} pontos</li>`;
        if (["insuficiente", "regular"].includes(conceito)) msg += `<li>Bom: ${(21 - notaTotal).toFixed(2)} pontos</li>`;
        if (["insuficiente", "regular", "bom"].includes(conceito)) msg += `<li>Excelente: ${(27 - notaTotal).toFixed(2)} pontos</li>`;
        msg += `</ul>`;
      } else {
        msg += `<p>Parabéns! Você já foi aprovado com Excelente.</p>`;
      }

      document.getElementById("resultado").innerHTML = msg;
    }

    atualizarCampos(); // inicializa campos
  </script></body>
</html>
