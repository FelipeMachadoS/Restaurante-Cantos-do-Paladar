<template>
  <div class="main">
    <div class="container">
      <div class="user-info">
        <p><strong>Nome:</strong> <span id="nome"></span></p>
        <p><strong>Endereço:</strong> <span id="endereco"></span></p>
        <p><strong>Forma de pagamento:</strong> <span id="formaPagamento"></span></p>
      </div>

      <div class="pedido-info">
        <div class="item" id="lanche">
          <img id="lancheFoto" alt="Foto do Lanche">
          <div class="item-details">
            <h3 id="lancheNome"></h3>
            <p id="lanchePreco"></p>
          </div>
        </div>

        <div class="item" id="bebida">
          <img id="bebidaFoto" alt="Foto da Bebida">
          <div class="item-details">
            <h3 id="bebidaNome"></h3>
            <p id="bebidaPreco"></p>
          </div>
        </div>

        <div class="acompanhamentos" id="acompanhamentos"></div>
      </div>

      <div class="total">
        <p><strong>Total:</strong> R$ <span id="total">0,00</span></p>
        <button id="confirmarPedido">Confirmar Pedido</button>
      </div>
    </div>
  </div>

  <!-- Modal de notificação -->
  <div id="notificationModal" class="modal">
    <div class="modal-content">
      <span class="close-btn" id="closeModal">&times;</span>
      <h2>Finalize seu Pedido</h2>
      <p>Falta pouco, confirme seu pedido via WhatsApp.</p>
      <p>Entre em contato pelo botão abaixo para finalizar sua compra.</p>
      <button id="whatsappLink">Abrir no WhatsApp</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Dashboard",
  mounted() {
    const pedidoJson = JSON.parse(localStorage.getItem('pedido'))

    document.getElementById('nome').innerText = pedidoJson.nome;
    document.getElementById('endereco').innerText = pedidoJson.endereco;
    document.getElementById('formaPagamento').innerText = pedidoJson.formaPagamento;

    // Preenchendo as informações do lanche
    document.getElementById('lancheNome').innerText = pedidoJson.lanche.nome;
    document.getElementById('lanchePreco').innerText = `R$ ${pedidoJson.lanche.preco.toFixed(2)}`;
    document.getElementById('lancheFoto').src = pedidoJson.lanche.foto;

    // Preenchendo as informações da bebida
    document.getElementById('bebidaNome').innerText = pedidoJson.bebida.nome;
    document.getElementById('bebidaPreco').innerText = `R$ ${pedidoJson.bebida.preco.toFixed(2)}`;
    document.getElementById('bebidaFoto').src = pedidoJson.bebida.foto;

    // Preenchendo os acompanhamentos
    const acompanhamentosContainer = document.getElementById('acompanhamentos');
    pedidoJson.acompanhamentos.forEach(acompanhamento => {
      const itemDiv = document.createElement('div');
      itemDiv.classList.add('acompanhamento-item');
      itemDiv.innerHTML = `
    <img src="${acompanhamento.foto}" alt="${acompanhamento.nome}">
    <p><strong>${acompanhamento.nome}</strong></p>
    <p>R$ ${acompanhamento.preco.toFixed(2)}</p>
  `;
      acompanhamentosContainer.appendChild(itemDiv);
    });

    // Calculando o total
    const total = pedidoJson.lanche.preco + pedidoJson.bebida.preco + pedidoJson.acompanhamentos.reduce((acc, item) => acc + item.preco, 0);
    document.getElementById('total').innerText = total.toFixed(2);

    // Obtendo o modal e o botão de confirmação
    const modal = document.getElementById('notificationModal');
    const btnConfirmar = document.getElementById('confirmarPedido');
    const closeModal = document.getElementById('closeModal');
    const whatsappButton = document.getElementById('whatsappLink');

    // Função para abrir o modal
    btnConfirmar.addEventListener('click', () => {
      modal.style.display = "block";
    });

    // Função para fechar o modal
    closeModal.addEventListener('click', () => {
      modal.style.display = "none";
    });

    // Função para abrir o WhatsApp (personalize o número e a mensagem)
    whatsappButton.addEventListener('click', () => {
      const nomeAcompanhamentos = pedidoJson.acompanhamentos.map(acompanhamento => `${acompanhamento.nome}`);
      const mensagem = `Olá, eu me chamo ${pedidoJson.nome} e gostaria de finalizar meu pedido!\n\nLanche: ${pedidoJson.lanche.nome}\nBebida: ${pedidoJson.bebida.nome}\nAcompanhamentos: ${nomeAcompanhamentos.join(", ")}\n\nForma de pagamento: ${pedidoJson.formaPagamento}\nEntregar em: ${pedidoJson.endereco}`;
      const numerosWhatsApp = ["+5511956876830", "+5511913456811", "+5511942814438"];
      const numeroDaVez = numerosWhatsApp[Math.floor(Math.random() * numerosWhatsApp.length)]
      const url = `https://wa.me/${numeroDaVez}?text=${encodeURIComponent(mensagem)}`;
      window.open(url, "_blank");
      // localStorage.removeItem('pedido');
    });

    // Fechar o modal se o usuário clicar fora dele
    window.onclick = function (event) {
      if (event.target === modal) {
        modal.style.display = "none";
      }
    };
  }
}
</script>

<style scoped>
/* Reset básico */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Arial', sans-serif;
  background-color: #f4f4f9;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  padding: 20px;
}

.main {
  display: flex;
  justify-content: center;
  height: 550px;
}

.container {
  background-color: #fff;
  width: 90%;
  max-width: 800px;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  text-align: center;
  margin-top: 10px;
}

h1 {
  font-size: 28px;
  margin-bottom: 30px;
  color: #333;
}

.user-info p {
  font-size: 16px;
  color: #555;
  margin-bottom: 10px;
}

.pedido-info {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}

.item {
  width: 30%;
  background-color: #f9f9f9;
  padding: 15px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.item img {
  width: 100%;
  height: auto;
  border-radius: 8px;
  margin-bottom: 10px;
}

.item-details h3 {
  font-size: 18px;
  color: #333;
}

.item-details p {
  font-size: 16px;
  color: #888;
}

.acompanhamentos {
  margin-top: 20px;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 20px;
}

.acompanhamento-item {
  background-color: #f9f9f9;
  padding: 10px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  width: 140px;
  text-align: center;
}

.acompanhamento-item img {
  width: 100%;
  border-radius: 6px;
  margin-bottom: 10px;
}

.total {
  margin-top: 30px;
  font-size: 18px;
  color: #333;
}

button {
  background-color: #28a745;
  color: #fff;
  padding: 12px 20px;
  font-size: 18px;
  border: none;
  border-radius: 8px;
  width: 100%;
  margin-top: 15px;
  cursor: pointer;
}

button:hover {
  background-color: #218838;
}

/* Modal (pop-up) */
.modal {
  display: none;
  /* Inicialmente invisível */
  position: fixed;
  z-index: 1;
  /* Fica sobre outros conteúdos */
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
  /* Semitransparente */
  padding-top: 100px;
  text-align: center;
}

.modal-content {
  background-color: #fff;
  margin: auto;
  padding: 20px;
  border: 1px solid #888;
  width: 60%;
  max-width: 500px;
  border-radius: 8px;
}

.close-btn {
  color: #aaa;
  font-size: 28px;
  font-weight: bold;
  position: absolute;
  top: 10px;
  right: 15px;
  cursor: pointer;
}

.close-btn:hover,
.close-btn:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

button {
  background-color: #28a745;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #218838;
}
</style>