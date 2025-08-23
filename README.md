<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Aranha Pipas</title>
  <style>
    /* ... (seu código CSS continua aqui) ... */
    :root {
      --cor-primaria: #005f99;
      --cor-secundaria: #004d7a;
      --cor-fundo-claro: #f0f8ff;
      --cor-fundo-escuro: #d0eaff;
      --cor-texto: #333;
    }
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(to bottom, var(--cor-fundo-claro), var(--cor-fundo-escuro));
      margin: 0;
      padding: 0;
      color: var(--cor-texto);
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }
    header {
      background-color: var(--cor-primaria);
      color: white;
      padding: 20px 0;
      text-align: center;
    }
    main {
      padding: 20px;
      text-align: center;
      flex-grow: 1;
    }
    footer {
      background-color: var(--cor-primaria);
      color: white;
      text-align: center;
      padding: 15px 0;
    }
    .info {
      font-size: 18px;
      line-height: 1.6;
      margin-top: 20px;
    }
    .produtos-container {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
    }
    .produto {
      border: 1px solid #ccc;
      padding: 20px;
      margin: 20px auto;
      max-width: 400px;
      border-radius: 8px;
      background-color: white;
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
    .produto h3 {
      color: var(--cor-secundaria);
    }
    button {
      background-color: var(--cor-secundaria);
      color: white;
      border: none;
      padding: 10px 20px;
      cursor: pointer;
      border-radius: 5px;
      font-size: 16px;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: var(--cor-primaria);
    }
    #cart-items {
      list-style-type: none;
      padding: 0;
      margin: 20px 0;
      text-align: left;
      max-width: 400px;
      margin: 20px auto;
    }
    #cart-items li {
      background-color: #e9f5ff;
      border-bottom: 1px solid #d0eaff;
      padding: 8px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    button.remove-from-cart {
      background-color: #dc3545;
      padding: 5px 10px;
      margin-left: 10px;
    }
    button.remove-from-cart:hover {
      background-color: #c82333;
    }
    .checkout-button {
      background-color: #28a745;
      margin-top: 20px;
      padding: 15px 30px;
      font-size: 18px;
    }
    .checkout-button:hover {
      background-color: #218838;
    }
  </style>
</head>
<body>
  <header>
    <h1>Aranha Pipas</h1>
    <p>Pipas e diversão garantida!</p>
  </header>
  <main>
    <h2>Nossos Produtos</h2>
    <div id="produtos-container">
      </div>
    <hr>
    <h2>Seu Carrinho</h2>
    <ul id="cart-items">
      </ul>
    <h3>Total: R$ <span id="cart-total">0.00</span></h3>
    <button class="checkout-button">Finalizar Compra</button>
  </main>
  <footer>
    <address class="info">
      <p>📞 Telefone: <a href="tel:+5518997117725">(18) 99711-7725</a></p>
      <p>📧 Email: <a href="mailto:crenatoatanasio@gmail.com">crenatoatanasio@gmail.com</a></p>
      <p>📍 Localização: Birigui - Bairro Candeias</p>
    </address>
    <p>&copy; 2025 Aranha Pipas. Todos os direitos reservados.</p>
  </footer>
  <script>
    document.addEventListener('DOMContentLoaded', () => {
      const produtos = [
        { nome: 'Pipa de 33cm', preco: 3.00, id: 1 },
        { nome: 'Pipa de 35cm', preco: 3.00, id: 2 },
        { nome: 'Pipa de 45cm', preco: 4.00, id: 3 },
        { nome: 'Pipa de 50cm', preco: 5.00, id: 4 },
        { nome: 'Pipa de 60cm', preco: 6.00, id: 5 },
        { nome: 'Pipa de 70cm', preco: 10.00, id: 6 },
        { nome: 'Pipa de 80cm', preco: 12.00, id: 7 },
        { nome: 'Pipa de 90cm', preco: 15.00, id: 8 }
      ];  
      const produtosContainer = document.getElementById('produtos-container');
      const cartItems = document.getElementById('cart-items');
      const cartTotal = document.getElementById('cart-total');
      const checkoutButton = document.querySelector('.checkout-button');
    
      let carrinho = [];

      function renderizarProdutos() {
        produtos.forEach(produto => {
          const produtoDiv = document.createElement('div');
          produtoDiv.classList.add('produto');
          produtoDiv.innerHTML = `
            <h3>${produto.nome}</h3>
            <p>Preço: R$ ${produto.preco.toFixed(2)}</p>
            <button class="add-to-cart" data-id="${produto.id}">Adicionar ao Carrinho</button>
          `;
          produtosContainer.appendChild(produtoDiv);
        });
      }

      function updateCartDisplay() {
        cartItems.innerHTML = '';
        let total = 0;
        
        carrinho.forEach(item => {
          const li = document.createElement('li');
          li.textContent = `${item.nome} - R$ ${item.preco.toFixed(2)}`;
          
          const removeButton = document.createElement('button');
          removeButton.textContent = 'X';
          removeButton.classList.add('remove-from-cart');
          removeButton.addEventListener('click', () => {
            removeItem(item.id);
          });
          
          li.appendChild(removeButton);
          cartItems.appendChild(li);
          total += item.preco;
        });
        
        cartTotal.textContent = total.toFixed(2);
      }

      function removeItem(itemId) {
        const itemIndex = carrinho.findIndex(item => item.id === itemId);
        if (itemIndex > -1) {
          carrinho.splice(itemIndex, 1);
        }
        updateCartDisplay();
      }

      produtosContainer.addEventListener('click', (event) => {
        if (event.target.classList.contains('add-to-cart')) {
          const produtoId = parseInt(event.target.dataset.id);
          const produto = produtos.find(p => p.id === produtoId);
          carrinho.push(produto);
          updateCartDisplay();
        }
      });

      checkoutButton.addEventListener('click', () => {
        if (carrinho.length > 0) {
          let mensagem = `Olá, gostaria de fazer o seguinte pedido:\n\n`;
          let total = 0;
          carrinho.forEach(item => {
            mensagem += `- ${item.nome}: R$ ${item.preco.toFixed(2)}\n`;
            total += item.preco;
          });
          mensagem += `\nTotal a pagar: R$ ${total.toFixed(2)}\n\nAguardando confirmação.`;
          
          const numeroTelefone = '5518997117725'; // Seu número de telefone no formato internacional
          const urlWhatsApp = `https://wa.me/${numeroTelefone}?text=${encodeURIComponent(mensagem)}`;
          
          window.open(urlWhatsApp, '_blank');
          
          // Limpa o carrinho após o envio da mensagem
          carrinho = [];
          updateCartDisplay();
        } else {
          alert('Seu carrinho está vazio!');
        }
      });
      
      renderizarProdutos();
    });
  </script>
</body>
</html>
