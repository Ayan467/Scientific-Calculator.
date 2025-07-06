# Scientific-Calculator.
A Modern Scientific Calculator  using  HTML CSS &amp; JS

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Unique Scientific Calculator</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/mathjs/11.11.2/math.min.js"></script>
  <style>
    :root {
      --bg: rgba(255, 255, 255, 0.05);
      --glass: rgba(255, 255, 255, 0.1);
      --shadow: rgba(0, 0, 0, 0.25);
      --text: #ffffff;
      --btn: rgba(255, 255, 255, 0.15);
      --hover: rgba(255, 255, 255, 0.25);
      --accent: #00ffe7;
      --transition: all 0.3s ease;
    }

    body.dark {
      --bg: rgba(0, 0, 0, 0.7);
      --glass: rgba(0, 0, 0, 0.2);
      --shadow: rgba(255, 255, 255, 0.1);
      --text: #ffffff;
      --btn: rgba(0, 0, 0, 0.4);
      --hover: rgba(255, 255, 255, 0.1);
      --accent: #ffb800;
    }

    * {
      box-sizing: border-box;
      transition: var(--transition);
    }

    body {
      margin: 0;
      padding: 0;
      height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background: radial-gradient(circle at top left, #1e1e2f, #0c0c0c);
      font-family: 'Segoe UI', sans-serif;
      color: var(--text);
    }

    .calculator {
      background: var(--glass);
      backdrop-filter: blur(15px);
      border-radius: 20px;
      padding: 30px;
      box-shadow: 0 0 30px var(--shadow);
      width: 420px;
      max-width: 90%;
    }

    .display {
      background: rgba(0, 0, 0, 0.4);
      padding: 20px;
      border-radius: 10px;
      font-size: 2rem;
      text-align: right;
      min-height: 60px;
      margin-bottom: 10px;
      color: var(--text);
      word-wrap: break-word;
    }

    .preview {
      font-size: 1rem;
      text-align: right;
      color: var(--accent);
      margin-bottom: 15px;
      min-height: 20px;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(5, 1fr);
      gap: 12px;
    }

    button {
      background: var(--btn);
      border: none;
      border-radius: 12px;
      padding: 15px;
      font-size: 1.1rem;
      color: var(--text);
      cursor: pointer;
      box-shadow: 0 0 10px transparent;
      transition: var(--transition);
    }

    button:hover {
      background: var(--hover);
      box-shadow: 0 0 15px var(--accent), inset 0 0 5px var(--accent);
    }

    .equal {
      background: var(--accent);
      color: #000;
      font-weight: bold;
      box-shadow: 0 0 15px var(--accent), 0 0 10px var(--accent) inset;
    }

    .clear {
      background: #ff4d4d;
      color: #fff;
      font-weight: bold;
    }

    .toggle {
      text-align: center;
      margin-top: 20px;
    }

    .toggle button {
      padding: 10px 25px;
      background: var(--accent);
      color: #000;
      font-weight: bold;
      border-radius: 25px;
      font-size: 1rem;
      cursor: pointer;
      box-shadow: 0 0 10px var(--accent);
    }
  </style>
</head>
<body class="dark">
  <div class="calculator">
    <div class="display" id="display">0</div>
    <div class="preview" id="preview"></div>

    <div class="buttons">
      <button class="clear" onclick="clearDisplay()">C</button>
      <button onclick="append('(')">(</button>
      <button onclick="append(')')">)</button>
      <button onclick="append('mod')">mod</button>
      <button onclick="append('/')">÷</button>

      <button onclick="append('7')">7</button>
      <button onclick="append('8')">8</button>
      <button onclick="append('9')">9</button>
      <button onclick="append('*')">×</button>
      <button onclick="append('^')">^</button>

      <button onclick="append('4')">4</button>
      <button onclick="append('5')">5</button>
      <button onclick="append('6')">6</button>
      <button onclick="append('-')">−</button>
      <button onclick="append('sqrt(')">√</button>

      <button onclick="append('1')">1</button>
      <button onclick="append('2')">2</button>
      <button onclick="append('3')">3</button>
      <button onclick="append('+')">+</button>
      <button onclick="append('pi')">π</button>

      <button onclick="append('0')">0</button>
      <button onclick="append('.')">.</button>
      <button onclick="calculate()" class="equal">=</button>
      <button onclick="append('e')">e</button>
      <button onclick="append('log(')">log</button>

      <button onclick="append('sin(')">sin</button>
      <button onclick="append('cos(')">cos</button>
      <button onclick="append('tan(')">tan</button>
      <button onclick="append('diff(')">d/dx</button>
      <button onclick="append('integrate(')">∫</button>
    </div>

    <div class="toggle">
      <button onclick="toggleMode()">🌗 Toggle Mode</button>
    </div>
  </div>

  <script>
    let display = document.getElementById("display");
    let preview = document.getElementById("preview");
    let input = "";

    function append(val) {
      input += val;
      updateDisplay();
      updatePreview();
    }

    function updateDisplay() {
      display.textContent = input || "0";
    }

    function updatePreview() {
      try {
        const expr = input.replace(/mod/g, '%');
        const result = math.evaluate(expr);
        preview.textContent = "= " + result;
      } catch {
        preview.textContent = "";
      }
    }

    function clearDisplay() {
      input = "";
      updateDisplay();
      preview.textContent = "";
    }

    function calculate() {
      try {
        if (input.startsWith("diff(") || input.startsWith("integrate(")) {
          input = math.evaluate(input).toString();
        } else {
          input = math.evaluate(input.replace(/mod/g, '%')).toString();
        }
        updateDisplay();
        preview.textContent = "";
      } catch {
        input = "Error";
        updateDisplay();
      }
    }

    function toggleMode() {
      document.body.classList.toggle("dark");
    }

    document.addEventListener("keydown", (e) => {
      const map = {
        Enter: calculate,
        Backspace: () => {
          input = input.slice(0, -1);
          updateDisplay();
          updatePreview();
        },
        Escape: clearDisplay
      };
      if (map[e.key]) {
        map[e.key]();
      } else if ((e.key >= "0" && e.key <= "9") || "+-*/().^%".includes(e.key)) {
        append(e.key);
      }
    });
  </script>
</body>
</html>
