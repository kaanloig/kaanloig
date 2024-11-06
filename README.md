<!-- Gadget de HTML/JS para Blogger -->
<div id="diamond-generator" class="generator-container">
  <h2 class="title">Generador de Diamantes</h2>
  <label for="user-id" class="label">Introduce tu ID:</label>
  <input type="text" id="user-id" placeholder="ID de usuario" class="input-field">
  
  <label for="diamond-quantity" class="label">¿Cuántos diamantes quieres generar?</label>
  <input type="number" id="diamond-quantity" placeholder="Número de diamantes" class="input-field" min="1" required>
  
  <button onclick="generateDiamonds()" class="generate-btn">Generar Diamantes</button>

  <div id="diamond-options" class="diamond-options"></div>
  <div id="diamond-display" class="diamond-display"></div>
  <div id="success-message" class="success-message"></div>
</div>

<!-- Estilos -->
<style>
  /* Fondo con la imagen proporcionada */
  body {
    margin: 0;
    padding: 0;
    background-image: url('https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiVZYq5gJlp7tyImo-BtpAuq4Ps7xZj_YuYdjQ5_t-qnXz5IY-jjVOWZJTUrViE5MdiZoog4UgR8DXvUhfaV7xgLBqvSW6NJkpVY-f19HhJeHP-ynD6ixzQXcDuWE2HDIfP9zC0Rg42Bin9PJqv-LyT-qFoB5WsUJrlShVEzjvMuKNfFX9cSgDvUfsF9q3d/s563/0e5711ceaa5a21c528c3f8c3aab2f00c.jpg');
    background-size: cover;
    background-position: center center;
    background-attachment: fixed;
    overflow: hidden;
    font-family: 'Arial', sans-serif;
  }

  .generator-container {
    background-color: rgba(0, 0, 0, 0.7);
    padding: 40px;
    border-radius: 15px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
    color: white;
    text-align: center;
    position: relative;
    z-index: 2;
    width: 80%;
    max-width: 600px;
    margin: 100px auto;
  }

  /* Estilo del título */
  .title {
    font-size: 2.5em;
    margin-bottom: 20px;
    text-transform: uppercase;
    font-weight: bold;
    letter-spacing: 1px;
    color: #ffbf00;
    text-shadow: 2px 2px 10px rgba(255, 255, 255, 0.8);
  }

  /* Estilo de las etiquetas */
  .label {
    font-size: 1.2em;
    margin-bottom: 10px;
  }

  /* Estilo del input */
  .input-field {
    padding: 12px;
    font-size: 1.1em;
    margin: 10px 0;
    width: 80%;
    border: none;
    border-radius: 5px;
    background-color: rgba(255, 255, 255, 0.1);
    color: white;
    outline: none;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
  }

  /* Estilo del botón */
  .generate-btn {
    padding: 10px 30px;
    font-size: 1.3em;
    color: #fff;
    background-color: #ff6347;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    margin-top: 20px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    transition: background-color 0.3s ease;
  }

  .generate-btn:hover {
    background-color: #e52e00;
  }

  /* Estilo de las opciones de diamantes */
  .diamond-options {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    gap: 15px;
    margin-top: 20px;
  }

  .diamond-option {
    font-size: 3em;
    cursor: pointer;
    transition: transform 0.2s ease-in-out;
  }

  .diamond-option:hover {
    transform: scale(1.2);
  }

  /* Área donde se mostrarán los diamantes elegidos */
  .diamond-display {
    margin-top: 20px;
    font-size: 2.5em;
    display: flex;
    justify-content: center;
    gap: 20px;
  }

  /* Estilo del mensaje de éxito */
  .success-message {
    margin-top: 20px;
    font-size: 1.5em;
    color: #32cd32;
    font-weight: bold;
    display: none;
  }

  /* Animación de caída de diamantes */
  @keyframes fall {
    0% {
      transform: translateY(-100px);
    }
    100% {
      transform: translateY(100vh);
    }
  }

  /* Diamantes cayendo */
  .falling-diamond {
    position: absolute;
    top: -50px;
    font-size: 2em;
    color: #3af;
    animation: fall linear infinite;
  }
</style>

<!-- Script JavaScript -->
<script>
// Función para generar diamantes y opciones
function generateDiamonds() {
  const userId = document.getElementById("user-id").value.trim();
  let diamondQuantity = parseInt(document.getElementById("diamond-quantity").value); // Obtener la cantidad de diamantes deseada por el usuario
  
  if (!userId) {
    alert("Por favor, ingresa un ID válido.");
    return;
  }

  if (!diamondQuantity || diamondQuantity <= 0) {
    alert("Por favor, ingresa una cantidad válida de diamantes.");
    return;
  }

  // Limitar la cantidad de diamantes a 1000
  if (diamondQuantity > 100) {
    diamondQuantity = 100;
    alert("Espera, se está enviando los diamantes a tu cuenta.");
  }

  // Mostrar el mensaje de éxito
  const successMessage = document.getElementById("success-message");
  successMessage.innerHTML = "¡Tu recompensa se ha generado con éxito!";
  successMessage.style.display = "block";

  // Mostrar las opciones de diamantes
  const diamondOptions = document.getElementById("diamond-options");
  const diamondTypes = ['💎', '✅'];
  let optionsHTML = '';
  
  for (let i = 0; i < diamondTypes.length; i++) {
    optionsHTML += `<div class="diamond-option" onclick="chooseDiamond('${diamondTypes[i]}')">${diamondTypes[i]}</div>`;
  }
  
  diamondOptions.innerHTML = optionsHTML;
  
  // Crear la animación de diamantes cayendo
  const diamondDisplay = document.getElementById("diamond-display");
  diamondDisplay.innerHTML = '';  // Limpiar los diamantes anteriores
  
  for (let i = 0; i < diamondQuantity; i++) {
    const fallingDiamond = document.createElement('div');
    fallingDiamond.classList.add('falling-diamond');
    fallingDiamond.innerText = '💎';
    fallingDiamond.style.left = `${Math.random() * 100}vw`;
    fallingDiamond.style.animationDuration = `${Math.random() * 3 + 2}s`; // Duración aleatoria
    document.body.appendChild(fallingDiamond);
  }
}

// Función para elegir diamante
function chooseDiamond(diamondType) {
  const diamondDisplay = document.getElementById("diamond-display");
  diamondDisplay.innerHTML = `<p>Elegiste el diamante: ${diamondType}</p>`;
  const diamondCount = Math.floor(Math.random() * 5) + 1;  // Número aleatorio de diamantes para mostrar
  let diamondsHTML = '';

  for (let i = 0; i < diamondCount; i++) {
    diamondsHTML += `<span style="font-size: 2em;">${diamondType}</span> `;
  }

  diamondDisplay.innerHTML += `<div>${diamondsHTML}</div>`;
}
</script>
