 Prontu
#EducaçãoDigital #PlanejamentoFácil #GestãoEscolar #Professores #TarefasAutomatizadas #TecnologiaNaEducação #PlataformaEducacional #RA #Ensino #EducaçãoInteligente

<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>PlanejaFácil - Simulador de Tarefas</title>
<style>
  body { font-family: Arial, sans-serif; background: #f5f5f5; padding: 20px; max-width: 800px; margin: auto; }
  h1 { text-align: center; color: #333; }
  label { display: block; margin-top: 15px; }
  input[type=text] { width: 100%; padding: 8px; margin-top: 5px; box-sizing: border-box; }
  button { margin-top: 15px; padding: 10px 20px; background-color: #4CAF50; border:none; color:white; cursor:pointer; border-radius: 5px; font-size: 16px; }
  button:hover { background-color: #45a049; }
  #results { margin-top: 30px; background: white; padding: 15px; border-radius: 8px; max-height: 400px; overflow-y: auto; }
  .ra-block { margin-bottom: 15px; border-bottom: 1px solid #ddd; padding-bottom: 10px; }
  .platform-result { margin-left: 15px; }
  #credit-info { margin-top: 20px; font-weight: bold; }
  select { padding: 8px; margin-left: 10px; }
</style>
</head>
<body>

<h1>PlanejaFácil - Simulador de Tarefas</h1>

<div>
  <p>RAs liberados: <span id="credit-count">15</span></p>
  <label for="ra-count">Quantos RAs deseja cadastrar? (max: <span id="max-ra">15</span>)</label>
  <input type="number" id="ra-count" min="1" max="15" value="1" />
  <button onclick="gerarCampos()">Gerar campos de RA</button>
</div>

<form id="ra-form" style="margin-top:20px;"></form>

<button onclick="simularTarefas()" id="start-btn" disabled>Simular tarefas para todos os RAs</button>

<div id="results"></div>

<hr>

<div>
  <h3>Comprar créditos extras</h3>
  <label for="credit-package">Pacote de créditos:</label>
  <select id="credit-package">
    <option value="5|3">5 RAs - R$3,00</option>
    <option value="10|5">10 RAs - R$5,00</option>
    <option value="15|7">15 RAs - R$7,00</option>
    <option value="20|8">20 RAs - R$8,00</option>
    <option value="30|9">30 RAs - R$9,00</option>
    <option value="40|10">40 RAs - R$10,00</option>
  </select>
  <button onclick="comprarCreditos()">Comprar créditos</button>
  <p id="purchase-message" style="color:green;"></p>
</div>

<script>
  let maxRAs = 15;
  let creditCount = 15; // começa com 15 liberados
  let raList = [];

  const raForm = document.getElementById('ra-form');
  const creditCountSpan = document.getElementById('credit-count');
  const maxRaSpan = document.getElementById('max-ra');
  const startBtn = document.getElementById('start-btn');
  const resultsDiv = document.getElementById('results');
  const purchaseMessage = document.getElementById('purchase-message');

  maxRaSpan.textContent = maxRAs;
  creditCountSpan.textContent = creditCount;

  function gerarCampos() {
    const count = parseInt(document.getElementById('ra-count').value);
    if (count < 1 || count > maxRAs) {
      alert('Informe um número válido de RAs (1 a ' + maxRAs + ').');
      return;
    }
    if (count > creditCount) {
      alert('Você só tem ' + creditCount + ' créditos liberados. Compre mais créditos para adicionar mais RAs.');
      return;
    }
    raForm.innerHTML = '';
    raList = [];
    for (let i = 1; i <= count; i++) {
      const div = document.createElement('div');
      div.className = 'ra-block';
      div.innerHTML = `<label>RA do aluno ${i}:</label><input type="text" placeholder="Digite o RA do aluno" id="ra${i}" required />`;
      raForm.appendChild(div);
      raList.push('');
    }
    startBtn.disabled = false;
    resultsDiv.innerHTML = '';
    purchaseMessage.textContent = '';
  }

  function simularTarefas() {
    resultsDiv.innerHTML = '';
    let valid = true;
    for(let i=1; i<=raList.length; i++) {
      const ra = document.getElementById('ra' + i).value.trim();
      if (!ra) {
        alert('Preencha todos os RAs.');
        valid = false;
        break;
      }
      raList[i-1] = ra;
    }
    if (!valid) return;

    startBtn.disabled = true;
    purchaseMessage.textContent = '';

    // Plataformas que serão simuladas
    const plataformas = ['Khan Academy', 'Matific', 'Alura', 'Leia para uma Criança'];

    raList.forEach((ra, idx) => {
      const raDiv = document.createElement('div');
      raDiv.className = 'ra-block';
      raDiv.innerHTML = `<strong>RA: ${ra}</strong>`;

      plataformas.forEach(plataforma => {
        const platDiv = document.createElement('div');
        platDiv.className = 'platform-result';
        platDiv.textContent = `Iniciando simulação na plataforma ${plataforma}...`;
        raDiv.appendChild(platDiv);

        // Tempo aleatório 3-5 min em ms
        const tempo = (Math.floor(Math.random() * 3) + 3) * 60 * 1000;

        setTimeout(() => {
          const porcentagem = Math.floor(Math.random() * 21) + 60; // 60-80%
          platDiv.textContent = `✅ ${plataforma}: tarefas simuladas com sucesso - ${porcentagem}% concluído.`;
        }, tempo);
      });

      resultsDiv.appendChild(raDiv);
    });

    // desconta créditos usados
    creditCount -= raList.length;
    creditCountSpan.textContent = creditCount;
  }

  function comprarCreditos() {
    const select = document.getElementById('credit-package');
    const [qtd, valor] = select.value.split('|').map(Number);

    // Aqui pode entrar integração com pagamento real depois
    // Por enquanto só simula compra e adiciona créditos

    creditCount += qtd;
    maxRAs = creditCount;
    maxRaSpan.textContent = maxRAs;
    creditCountSpan.textContent = creditCount;
    purchaseMessage.textContent = `Compra simulada! Você adicionou ${qtd} créditos por R$${valor.toFixed(2)}.`;
  }
</script>

</body>
</html>

