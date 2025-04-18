# Bot
<!DOCTYPE html>
<html>
<head>
  <title>Trading Assistant Bot</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 500px;
      margin: 50px auto;
      padding: 20px;
      border-radius: 10px;
      background: #f2f2f2;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
    }
    label, select, input {
      display: block;
      width: 100%;
      margin-bottom: 15px;
      font-size: 16px;
    }
    button {
      background: #007bff;
      color: white;
      padding: 10px;
      width: 100%;
      border: none;
      font-size: 16px;
      cursor: pointer;
    }
    button:hover {
      background: #0056b3;
    }
    #result {
      margin-top: 20px;
      font-size: 18px;
      font-weight: bold;
      text-align: center;
      color: #333;
    }
  </style>
</head>
<body>

  <h2>Manual Trading Assistant Bot</h2>

  <label>Trend Direction:</label>
  <select id="trend">
    <option value="up">Up</option>
    <option value="down">Down</option>
    <option value="sideways">Sideways</option>
  </select>

  <label>RSI Value (0–100):</label>
  <input type="number" id="rsi" min="0" max="100">

  <label>MACD Signal:</label>
  <select id="macd">
    <option value="buy">Buy</option>
    <option value="sell">Sell</option>
    <option value="neutral">Neutral</option>
  </select>

  <label>MA Crossover:</label>
  <select id="ma">
    <option value="bullish">Bullish</option>
    <option value="bearish">Bearish</option>
    <option value="none">None</option>
  </select>

  <label>Candlestick Signal:</label>
  <select id="candle">
    <option value="bullish">Bullish</option>
    <option value="bearish">Bearish</option>
    <option value="none">None</option>
  </select>

  <label>Support/Resistance Action:</label>
  <select id="sr">
    <option value="bounce">Bounce</option>
    <option value="break">Break</option>
    <option value="none">None</option>
  </select>

  <button onclick="evaluateTrade()">Evaluate Trade</button>

  <div id="result"></div>

  <script>
    function evaluateTrade() {
      const trend = document.getElementById("trend").value;
      const rsi = parseFloat(document.getElementById("rsi").value);
      const macd = document.getElementById("macd").value;
      const ma = document.getElementById("ma").value;
      const candle = document.getElementById("candle").value;
      const sr = document.getElementById("sr").value;

      let buyScore = 0;
      let sellScore = 0;

      if (trend === "up") buyScore++;
      else if (trend === "down") sellScore++;

      if (rsi < 30) buyScore++;
      else if (rsi > 70) sellScore++;

      if (macd === "buy") buyScore++;
      else if (macd === "sell") sellScore++;

      if (ma === "bullish") buyScore++;
      else if (ma === "bearish") sellScore++;

      if (candle === "bullish") buyScore++;
      else if (candle === "bearish") sellScore++;

      if (sr === "bounce" || sr === "break") {
        if (trend === "up") buyScore++;
        else if (trend === "down") sellScore++;
      }

      const result = document.getElementById("result");

      if (buyScore >= 4 && buyScore > sellScore) {
        result.textContent = ">>> SIGNAL: BUY (CALL)";
        result.style.color = "green";
      } else if (sellScore >= 4 && sellScore > buyScore) {
        result.textContent = ">>> SIGNAL: SELL (PUT)";
        result.style.color = "red";
      } else {
        result.textContent = ">>> SIGNAL: WAIT / NO TRADE";
        result.style.color = "gray";
      }
    }
  </script>

</body>
</html>
