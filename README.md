# calculator
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Animated Calculator</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: linear-gradient(135deg, #1a73e8, #4caf50);
      font-family: 'Poppins', sans-serif;
      animation: bgShift 8s infinite alternate ease-in-out;
    }

    @keyframes bgShift {
      0% { background: linear-gradient(135deg, #1a73e8, #4caf50); }
      50% { background: linear-gradient(135deg, #4caf50, #ff9800); }
      100% { background: linear-gradient(135deg, #ff9800, #1a73e8); }
    }

    .calculator {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 20px;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
      padding: 20px;
      width: 320px;
      backdrop-filter: blur(10px);
      animation: fadeIn 1s ease-in-out;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: scale(0.8); }
      to { opacity: 1; transform: scale(1); }
    }

    .display {
      background: rgba(0, 0, 0, 0.3);
      border-radius: 10px;
      text-align: right;
      padding: 15px;
      font-size: 2rem;
      color: #fff;
      margin-bottom: 15px;
      overflow-x: auto;
      box-shadow: inset 0 0 10px rgba(255, 255, 255, 0.2);
      animation: glow 3s infinite alternate;
    }

    @keyframes glow {
      from { box-shadow: 0 0 5px #4caf50, 0 0 10px #4caf50; }
      to { box-shadow: 0 0 10px #1a73e8, 0 0 20px #1a73e8; }
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
    }

    button {
      padding: 20px;
      font-size: 1.2rem;
      border: none;
      border-radius: 10px;
      background-color: rgba(255, 255, 255, 0.15);
      color: #fff;
      cursor: pointer;
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;
    }

    button:hover {
      background-color: rgba(255, 255, 255, 0.3);
      transform: translateY(-3px);
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    }

    button:active {
      transform: scale(0.95);
    }

    button::after {
      content: '';
      position: absolute;
      top: 50%;
      left: 50%;
      width: 0;
      height: 0;
      background: rgba(255, 255, 255, 0.5);
      border-radius: 50%;
      transform: translate(-50%, -50%);
      transition: width 0.4s ease, height 0.4s ease;
      z-index: 0;
    }

    button:active::after {
      width: 200px;
      height: 200px;
    }

    .operator { background-color: #ff9800; }
    .equal { background-color: #4caf50; }
    .clear { background-color: #f44336; }
    .del { background-color: #2196f3; }

    @keyframes shake {
      0%, 100% { transform: translateX(0); }
      25% { transform: translateX(-5px); }
      75% { transform: translateX(5px); }
    }
  </style>
</head>
<body>
  <div class="calculator">
    <div id="display" class="display">0</div>
    <div class="buttons">
      <button class="clear">C</button>
      <button class="del">⌫</button>
      <button class="operator">%</button>
      <button class="operator">/</button>

      <button>7</button>
      <button>8</button>
      <button>9</button>
      <button class="operator">*</button>

      <button>4</button>
      <button>5</button>
      <button>6</button>
      <button class="operator">-</button>

      <button>1</button>
      <button>2</button>
      <button>3</button>
      <button class="operator">+</button>

      <button>0</button>
      <button>.</button>
      <button class="equal" colspan="2">=</button>
    </div>
  </div>

  <script>
    const display = document.getElementById('display');
    const buttons = document.querySelectorAll('button');
    let input = '';

    function updateDisplay(value) {
      display.textContent = value || '0';
    }

    function handleInput(value) {
      if (value === 'C') {
        input = '';
      } else if (value === '⌫') {
        input = input.slice(0, -1);
      } else if (value === '=') {
        try {
          const result = eval(input);
          if (result === Infinity || isNaN(result)) throw new Error('Invalid');
          input = result.toString();
        } catch (error) {
          display.style.animation = 'shake 0.3s';
          setTimeout(() => display.style.animation = '', 300);
          input = '';
          updateDisplay('Error');
          return;
        }
      } else {
        input += value;
      }
      updateDisplay(input);
    }

    buttons.forEach(button => {
      button.addEventListener('click', () => handleInput(button.textContent));
    });

    document.addEventListener('keydown', (event) => {
      const key = event.key;
      if (/^[0-9+\-*/.%]$/.test(key)) {
        handleInput(key);
      } else if (key === 'Enter') {
        handleInput('=');
      } else if (key === 'Backspace') {
        handleInput('⌫');
      } else if (key.toLowerCase() === 'c') {
        handleInput('C');
      }
    });
  </script>
</body>
</html>
