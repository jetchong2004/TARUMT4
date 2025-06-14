<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Society Exchange Market</title>
  <style>
    body {
      background-color: #000;
      color: #fff;
      font-family: 'Courier New', Courier, monospace;
      padding: 20px;
    }

    h1 {
      text-align: center;
      color: #0f0;
      margin-bottom: 30px;
    }

    .ticker {
      display: grid;
      grid-template-columns: 1fr 1fr;
      max-width: 500px;
      margin: auto;
      gap: 10px;
    }

    .society {
      padding: 10px;
      background: #111;
      border: 1px solid #333;
      display: flex;
      justify-content: space-between;
      font-size: 20px;
      transition: color 0.3s;
    }

    .up {
      color: #0f0;
    }

    .down {
      color: #f00;
    }
  </style>
</head>
<body>

<h1>Society Exchange Market</h1>

<div class="ticker" id="ticker">Loading...</div>

<script>
  const URL = "https://script.google.com/macros/s/AKfycbxV1Zf6I2j43Ss-NDOkv-2It4UFb2FpfP9lZI2tr5tYDuaiOGh1KV35nQdRsMWw5kIhzA/exec";

  let previousCounts = {};

  async function fetchSocietyData() {
    try {
      const res = await fetch(URL);
      const data = await res.json();
      updateDisplay(data);
    } catch (err) {
      document.getElementById("ticker").innerHTML = "<div>Error loading data</div>";
    }
  }

  function updateDisplay(data) {
    const ticker = document.getElementById('ticker');
    ticker.innerHTML = '';

    data.forEach(soc => {
      const prev = previousCounts[soc.name] ?? soc.count;
      const statusClass = soc.count > prev ? 'up' :
                          soc.count < prev ? 'down' : '';

      previousCounts[soc.name] = soc.count;

      const div = document.createElement('div');
      div.className = `society ${statusClass}`;
      div.innerHTML = `<span>${soc.name}</span><span>${soc.count}</span>`;
      ticker.appendChild(div);
    });
  }

  fetchSocietyData();
  setInterval(fetchSocietyData, 5000);
</script>

</body>
</html>
