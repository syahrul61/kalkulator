<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Kalkulator Lengkap</title>
  <style>
    :root {
      --bg-light: #ffffff;
      --text-light: #111;
      --btn-light: #f1f5f9;
      --btn-num-light: #dbeafe; /* biru muda */
      --btn-op-light: #fdba74; /* oranye pastel */
      --btn-func-light: #bbf7d0; /* hijau pastel */
      --btn-eq-light: #c084fc; /* ungu pastel */

      --bg-dark: #0f172a;
      --text-dark: #f8fafc;
      --btn-dark: #1e293b;
      --btn-op-dark: #334155;
      --btn-func-dark: #475569;
      --btn-eq-dark: #2563eb;
    }

    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      font-family: Arial, sans-serif;
      transition: background 0.3s, color 0.3s;
    }

    .calculator {
      width: 320px;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 8px 20px rgba(0,0,0,0.2);
      display: flex;
      flex-direction: column;
    }

    .display {
      padding: 20px;
      font-size: 2rem;
      text-align: right;
      min-height: 60px;
      word-wrap: break-word;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 5px;
      padding: 10px;
    }

    button {
      border: none;
      border-radius: 8px;
      font-size: 1.2rem;
      padding: 15px;
      cursor: pointer;
      transition: background 0.2s;
    }

    .history {
      max-height: 100px;
      overflow-y: auto;
      padding: 10px;
      font-size: 0.9rem;
    }

    .controls {
      display: flex;
      justify-content: space-around;
      padding: 10px;
    }

    .controls button {
      flex: 1;
      margin: 5px;
      padding: 10px;
      font-size: 0.9rem;
      border-radius: 6px;
    }

    .theme-toggle {
      position: absolute;
      top: 10px;
      right: 10px;
      background: transparent;
      border: none;
      font-size: 1.2rem;
      cursor: pointer;
    }

    /* Mode terang */
    body.light {
      background: var(--bg-light);
      color: var(--text-light);
    }
    body.light .calculator { background: var(--bg-light); }
    body.light .display { color: var(--text-light); }
    body.light button { background: var(--btn-light); color: var(--text-light); }
    body.light .num { background: var(--btn-num-light); }
    body.light .op { background: var(--btn-op-light); }
    body.light .func { background: var(--btn-func-light); }
    body.light .eq { background: var(--btn-eq-light); }

    /* Mode gelap */
    body.dark {
      background: var(--bg-dark);
      color: var(--text-dark);
    }
    body.dark .calculator { background: var(--bg-dark); }
    body.dark .display { color: var(--text-dark); }
    body.dark button { background: var(--btn-dark); color: var(--text-dark); }
    body.dark .op { background: var(--btn-op-dark); }
    body.dark .func { background: var(--btn-func-dark); }
    body.dark .eq { background: var(--btn-eq-dark); color: #fff; }
  </style>
</head>
<body class="light">
  <div class="calculator">
    <button class="theme-toggle" onclick="toggleTheme()">☀️</button>
    <div class="display" id="display">0</div>
    <div class="buttons">
      <button class="op" onclick="allClear()">AC</button>
      <button class="op" onclick="clearEntry()">C</button>
      <button class="op" onclick="insert('%')">%</button>
      <button class="op" onclick="insert('/')">÷</button>

      <button class="num" onclick="insert('7')">7</button>
      <button class="num" onclick="insert('8')">8</button>
      <button class="num" onclick="insert('9')">9</button>
      <button class="op" onclick="insert('*')">×</button>

      <button class="num" onclick="insert('4')">4</button>
      <button class="num" onclick="insert('5')">5</button>
      <button class="num" onclick="insert('6')">6</button>
      <button class="op" onclick="insert('-')">−</button>

      <button class="num" onclick="insert('1')">1</button>
      <button class="num" onclick="insert('2')">2</button>
      <button class="num" onclick="insert('3')">3</button>
      <button class="op" onclick="insert('+')">+</button>

      <button class="func" onclick="negate()">±</button>
      <button class="num" onclick="insert('0')">0</button>
      <button class="num" onclick="insert('.')">.</button>
      <button class="eq" onclick="calculate()">=</button>

      <button class="func" onclick="sqrt()">√</button>
      <button class="func" onclick="square()">x²</button>
      <button class="func" onclick="inverse()">1/x</button>
      <button class="func" onclick="insert('Math.PI')">π</button>

      <button class="func" onclick="insert('Math.E')">e</button>
      <button class="func" onclick="sin()">sin</button>
      <button class="func" onclick="cos()">cos</button>
      <button class="func" onclick="tan()">tan</button>
    </div>
    <div class="controls">
      <button onclick="copyResult()">Copy Hasil</button>
      <button onclick="clearHistory()" style="background:#dc2626;color:#fff;">Hapus Riwayat</button>
    </div>
    <div class="history" id="history"></div>
  </div>

  <script>
    let display = document.getElementById("display");
    let historyBox = document.getElementById("history");
    let current = "";

    function insert(val) {
      current += val;
      display.innerText = current;
    }
    function allClear() {
      current = "";
      display.innerText = "0";
    }
    function clearEntry() {
      current = current.slice(0, -1);
      display.innerText = current || "0";
    }
    function calculate() {
      try {
        let result = eval(current);
        historyBox.innerHTML += current + " = " + result + "<br>";
        current = result.toString();
        display.innerText = current;
      } catch {
        display.innerText = "Error";
      }
    }
    function negate() {
      if (current) {
        current = (parseFloat(current) * -1).toString();
        display.innerText = current;
      }
    }
    function sqrt() { calculateWith(Math.sqrt); }
    function square() { calculateWith((x)=>x*x); }
    function inverse() { calculateWith((x)=>1/x); }
    function sin() { calculateWith(Math.sin); }
    function cos() { calculateWith(Math.cos); }
    function tan() { calculateWith(Math.tan); }

    function calculateWith(fn) {
      try {
        let result = fn(eval(current));
        historyBox.innerHTML += fn.name + "(" + current + ") = " + result + "<br>";
        current = result.toString();
        display.innerText = current;
      } catch {
        display.innerText = "Error";
      }
    }

    function copyResult() {
      navigator.clipboard.writeText(display.innerText);
      alert("Hasil disalin!");
    }
    function clearHistory() {
      historyBox.innerHTML = "";
    }

    function toggleTheme() {
      if (document.body.classList.contains("light")) {
        document.body.classList.replace("light", "dark");
        document.querySelector(".theme-toggle").innerText = "🌙";
      } else {
        document.body.classList.replace("dark", "light");
        document.querySelector(".theme-toggle").innerText = "☀️";
      }
    }

    // Keyboard support
    document.addEventListener("keydown", (e) => {
      if ("0123456789.+-*/%".includes(e.key)) insert(e.key);
      else if (e.key === "Enter") calculate();
      else if (e.key === "Backspace") clearEntry();
      else if (e.key === "r") sqrt();
      else if (e.key === "^") square();
    });
  </script>
</body>
</html>
