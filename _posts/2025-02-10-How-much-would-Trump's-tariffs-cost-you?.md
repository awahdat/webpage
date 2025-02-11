---
author: Ahmad Zia Wahdat
title: How much would Trump's tariffs cost you?
---

Remember the empty shelves during the COVID-19 pandemic? Or perhaps you have seen people rushing to stock up before a hurricane? Or perhaps that one excited consumer stocking up ten boxes of pasta on sale on a given weekday? While grocery stockpiling often gets bad press, [my new research study](https://doi.org/10.1111/cjag.12379) .

### Trump's Tariff Burden Calculator

Enter your after-tax income:

<input type="number" id="incomeInput" value="50000" min="0" step="1000">
<button onclick="updateChart()">Calculate</button>

<canvas id="taxChart" width="400" height="300"></canvas>

<script>
  function calculateTaxBurden(income) {
    return income <= 18780 ? income * 0.056752 :
           income <= 31830 ? income * 0.027083 :
           income <= 43840 ? income * 0.025004 :
           income <= 55550 ? income * 0.020013 :
           income <= 69240 ? income * 0.018159 :
           income <= 85400 ? income * 0.016873 :
           income <= 105500 ? income * 0.015073 :
           income <= 132600 ? income * 0.013664 :
           income <= 181800 ? income * 0.013647 :
           income <= 237200 ? income * 0.010883 :  income * 0.010883 ;
  }

  function updateChart() {
    const income = parseFloat(document.getElementById("incomeInput").value);
    const userTax = calculateTaxBurden(income);
    const medianIncome = 69240;
    const medianTax = calculateTaxBurden(medianIncome);

    if (typeof userTax === "string") {
      alert(userTax);
      return;
    }

    const canvas = document.getElementById("taxChart");
    const ctx = canvas.getContext("2d");

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Chart properties
    const barWidth = 100;
    const barSpacing = 50;
    const startX = 50;
    const maxHeight = 200;
    const maxTax = Math.max(userTax, medianTax);
    
    const userBarHeight = (userTax / maxTax) * maxHeight;
    const medianBarHeight = (medianTax / maxTax) * maxHeight;

    ctx.fillStyle = "steelblue";
    ctx.fillRect(startX, canvas.height - userBarHeight - 30, barWidth, userBarHeight);
    ctx.fillText("Your Tariff Burden", startX + 7, canvas.height - 5);

    ctx.fillStyle = "darkred";
    ctx.fillRect(startX + barWidth + barSpacing, canvas.height - medianBarHeight - 30, barWidth, medianBarHeight);
    ctx.fillText("Tariff Burden at Median Income", startX + barWidth + barSpacing -20, canvas.height - 5);

    ctx.fillStyle = "black";
    ctx.fillText(`$${userTax.toFixed(2)}`, startX + 20, canvas.height - userBarHeight - 35);
    ctx.fillText(`$${medianTax.toFixed(2)}`, startX + barWidth + barSpacing + 10, canvas.height - medianBarHeight - 35);
  }

  updateChart(); // Initial render
</script>
