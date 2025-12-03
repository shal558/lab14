<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <title>Функции и Отладка — Demo</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f9;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-direction: column;
      color: #333;
    }
    h1 { text-align: center; margin-bottom: 30px; }
    button {
      padding: 15px 25px;
      margin: 10px;
      font-size: 16px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      background: #0077ff;
      color: #fff;
      transition: background 0.3s;
    }
    button:hover { background: #005fd4; }
    input {
      padding: 8px;
      font-size: 14px;
      border-radius: 6px;
      border: 1px solid #aaa;
      margin-left: 10px;
      width: 100px;
    }
    pre {
      background: #222;
      color: #0f0;
      padding: 15px;
      border-radius: 8px;
      margin-top: 15px;
      max-width: 600px;
      overflow-x: auto;
      white-space: pre-wrap;
      text-align: left;
    }
    .section { margin-top: 20px; text-align: center; }
    .label { font-weight: bold; margin-left: 10px; }
  </style>
</head>
<body>
  <h1>Демонстрация функций и отладки</h1>

  <div class="section">
    <label class="label">Число для факториала:</label>
    <input id="factorialInput" type="number" value="5" min="0">
    <button onclick="createFunction()">Создать и протестировать новую функцию</button>
  </div>

  <div class="section">
    <label class="label">Массив для сломанной функции (через запятую):</label>
    <input id="arrayInput" type="text" value="1,2,3,4">
    <button onclick="createBrokenFunction()">Создать сломанную функцию с ошибкой</button>
    <button onclick="fixBrokenFunction()">Исправить ошибку</button>
  </div>

  <div class="section">
    <pre id="outputNewFunction"></pre>
    <pre id="outputBrokenFunction"></pre>
    <pre id="outputErrorExplanation"></pre>
  </div>

  <script>
    // 1. Создание новой функции и тесты
    function createFunction() {
      const n = parseInt(document.getElementById('factorialInput').value);

      function factorial(n) {
        if (n < 0) return null;
        let result = 1;
        for (let i = 1; i <= n; i++) result *= i;
        return result;
      }

      let output = "=== Тестирование факториала ===\n";
      output += `factorial(${n}) = ${factorial(n)} → ✔ OK\n`;
      output += `factorial(0) = ${factorial(0)} → ${factorial(0) === 1 ? "✔ OK" : "✖ FAIL"}\n`;
      output += `factorial(-1) = ${factorial(-1)} → ${factorial(-1) === null ? "✔ OK" : "✖ FAIL"}\n`;

      document.getElementById("outputNewFunction").textContent = output;
      document.getElementById("outputBrokenFunction").textContent = "";
      document.getElementById("outputErrorExplanation").textContent = "";
    }

    // 2. Создание сломанной функции
    let brokenActive = true;

    function createBrokenFunction() {
      const arr = document.getElementById('arrayInput').value.split(',').map(Number);

      function brokenSum(arr) {
        let sum = 0;
        for (let i = 0; i <= arr.length; i++) { // Ошибка
          sum += arr[i];
        }
        return sum;
      }

      let result;
      try { result = brokenSum(arr); } 
      catch(e) { result = "Ошибка выполнения!"; }

      document.getElementById("outputBrokenFunction").textContent =
        "=== Сломанная функция ===\n" +
        "Входной массив: [" + arr + "]\n" +
        "Результат: " + result;

      document.getElementById("outputErrorExplanation").textContent =
        "Цикл использует i <= arr.length, что вызывает лишнюю итерацию.\n" +
        "На последнем шаге arr[i] = undefined, результат становится NaN.\n" +
        "Исправление: заменить <= на <.";

      brokenActive = true;
    }

    // 3. Исправление
    function fixBrokenFunction() {
      if (!brokenActive) return;

      const arr = document.getElementById('arrayInput').value.split(',').map(Number);

      function fixedSum(arr) {
        let sum = 0;
        for (let i = 0; i < arr.length; i++) {
          sum += arr[i];
        }
        return sum;
      }

      const result = fixedSum(arr);

      document.getElementById("outputBrokenFunction").textContent =
        "=== Исправленная функция ===\n" +
        "Входной массив: [" + arr + "]\n" +
        "Результат: " + result;

      document.getElementById("outputErrorExplanation").textContent =
        "Ошибка исправлена. Цикл корректный.";

      brokenActive = false;
    }
  </script>
</body>
</html>

