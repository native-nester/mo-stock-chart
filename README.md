# mo-stock-chart
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>MO Stock Price Chart</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
      text-align: center;
    }
    canvas {
      max-width: 700px;
      margin: auto;
    }
  </style>
</head>
<body>
  <h2>MO (Altria Group) Stock Price - Last 30 Days</h2>
  <canvas id="moChart"></canvas>

  <script>
    async function fetchStockData() {
      const response = await fetch('https://query1.finance.yahoo.com/v8/finance/chart/MO?range=1mo&interval=1d');
      const data = await response.json();
      const timestamps = data.chart.result[0].timestamp;
      const prices = data.chart.result[0].indicators.quote[0].close;

      // Convert UNIX timestamps to readable dates
      const labels = timestamps.map(ts => {
        const date = new Date(ts * 1000);
        return `${date.getMonth()+1}/${date.getDate()}`;
      });

      return { labels, prices };
    }

    async function renderChart() {
      const { labels, prices } = await fetchStockData();

      const ctx = document.getElementById('moChart').getContext('2d');
      new Chart(ctx, {
        type: 'line',
        data: {
          labels: labels,
          datasets: [{
            label: 'Price (USD)',
            data: prices,
            borderColor: 'green',
            backgroundColor: 'rgba(0, 128, 0, 0.1)',
            tension: 0.2,
            pointRadius: 2
          }]
        },
        options: {
          responsive: true,
          scales: {
            x: {
              title: { display: true, text: 'Date' }
            },
            y: {
              title: { display: true, text: 'Price ($)' }
            }
          }
        }
      });
    }

    renderChart();
  </script>
</body>
</html>
