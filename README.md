<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bill Tracker Deluxe</title>
    <style>
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            margin: 0;
            padding: 10px;
            background: linear-gradient(135deg, #b2fefa, #ff9a8b); /* Bright, jazzy gradient */
            color: #1a202c; /* Deep gray for contrast */
            max-width: 375px; /* iPhone screen width */
            margin-left: auto;
            margin-right: auto;
            overflow-x: hidden; /* No horizontal scroll */
        }
        h1 {
            font-size: 2.5em;
            text-align: center;
            color: #fff;
            text-shadow: 2px 2px 8px rgba(0,0,0,0.4);
            margin: 10px 0;
            animation: pulse 2s infinite;
        }
        #current-date {
            text-align: center;
            font-size: 1.2em;
            color: #fff;
            margin-bottom: 15px;
            font-weight: bold;
        }
        .container {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        .card {
            background: rgba(255, 255, 255, 0.9);
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
            text-align: center;
            transition: transform 0.3s, box-shadow 0.3s;
        }
        .card:hover {
            transform: scale(1.05);
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }
        .card h3 {
            font-size: 1.5em;
            margin: 0;
            color: #1a202c;
        }
        .card p {
            font-size: 1em;
            margin: 5px 0;
        }
        .card.urgent {
            border: 4px solid #ff00ff; /* Neon magenta */
            padding: 20px;
            transform: scale(1.1);
            background: rgba(255, 240, 245, 0.9);
            box-shadow: 0 0 15px #ff00ff, 0 0 25px #00ffff; /* Neon glow */
            animation: vibrate 0.1s infinite, pulse 1s infinite;
        }
        .card.near-due {
            border: 3px solid #00ffff; /* Neon cyan */
            padding: 18px;
            transform: scale(1.05);
            background: rgba(240, 255, 255, 0.9);
            box-shadow: 0 0 10px #00ffff;
        }
        .card.small {
            padding: 10px;
            font-size: 0.8em;
        }
        .card.small h3 { font-size: 1.2em; }
        .section {
            margin: 20px 0;
            background: rgba(255, 255, 255, 0.85);
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
            animation: fadeIn 1s ease-in;
        }
        .section h2 {
            font-size: 1.8em;
            color: #2b6cb0;
            text-align: center;
            margin: 10px 0;
            font-weight: bold;
        }
        canvas {
            max-width: 100%;
            margin: 15px 0;
        }
        .progress-bar {
            width: 100%;
            margin: 15px 0;
            background: #e2e8f0;
            border-radius: 8px;
            overflow: hidden;
        }
        .progress {
            height: 20px;
            text-align: center;
            color: #fff;
            font-size: 0.9em;
            transition: width 1s ease-in-out;
        }
        .progress.success { background: #00ff7f; /* Neon spring green */ }
        .progress.warning { background: #ff4500; /* Neon orange-red */ }
        .sobriety-section {
            background: linear-gradient(135deg, #00ff7f, #00b894); /* Neon green vibe */
            color: #fff;
            padding: 20px;
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3), 0 0 10px #00ff7f;
        }
        .sobriety-section h2 {
            font-size: 2em;
            color: #fff;
            text-shadow: 2px 2px 5px rgba(0,0,0,0.5);
        }
        .sobriety-section p {
            font-size: 1.1em;
            margin: 8px 0;
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        @keyframes vibrate {
            0% { transform: translateX(0); }
            25% { transform: translateX(-2px); }
            50% { transform: translateX(2px); }
            75% { transform: translateX(-2px); }
            100% { transform: translateX(0); }
        }
        @keyframes fadeIn {
            0% { opacity: 0; }
            100% { opacity: 1; }
        }
    </style>
</head>
<body>
    <h1>Bill Tracker Deluxe</h1>
    <div id="current-date">Loading date...</div>
    <div class="container" id="bill-container"></div>

    <div class="section">
        <h2>Monthly Summary</h2>
        <p>Total Income: $<span id="total-income">0</span></p>
        <p>Total Expenses: $<span id="total-expenses">0</span></p>
        <p>Net Cash Flow: $<span id="net-cash-flow">0</span></p>
        <canvas id="incomeExpenseChart"></canvas>
    </div>

    <div class="section">
        <h2>Expense Breakdown</h2>
        <canvas id="expensePieChart"></canvas>
    </div>

    <div class="section">
        <h2>Goals & Progress</h2>
        <p>Savings: $<span id="savings">7500</span> (Goal: $10,000)</p>
        <div class="progress-bar"><div class="progress" id="savings-progress"></div></div>
        <p>HELOC: $<span id="heloc-owed">31000</span> owed (Goal: $0)</p>
        <div class="progress-bar"><div class="progress" id="heloc-progress"></div></div>
        <canvas id="goalsChart"></canvas>
    </div>

    <div class="section sobriety-section">
        <h2>🔥 Sobriety Journey 🔥</h2>
        <p>Days Sober: <span id="days-sober">0</span> (<span id="years-sober">0</span> yrs)</p>
        <p>Savings: $<span id="sobriety-savings">0</span> 💰</p>
        <canvas id="sobrietySavingsChart"></canvas>
    </div>

    <div class="section">
        <h2>Assets & Liabilities</h2>
        <p>House: $310,000 (Equity: $<span id="house-equity">0</span>)</p>
        <p>Tundra: $10,000+</p>
        <p>HELOC: $<span id="heloc-owed-display">31000</span> / $40,000</p>
        <canvas id="netWorthChart"></canvas>
    </div>

    <div class="section">
        <h2>Egg Income</h2>
        <p>Eggs Today: <span id="eggs-today">0</span> ($0.50/egg)</p>
        <p>Monthly: $<span id="egg-income">0</span></p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // Current date
        const today = new Date();
        document.getElementById('current-date').textContent = `Today: ${today.toDateString()}`;

        // Bill data
        const bills = [
            { name: "Student Loans", dueDay: 6, amount: 260 },
            { name: "State Farm Insurance", dueDay: 6, amount: 130 },
            { name: "Mortgage", dueDay: 4, amount: 1690, interest: 3.5 },
            { name: "Eversource", dueDay: 13, amount: 175 },
            { name: "Verizon", dueDay: 6, amount: 100 },
            { name: "GitHub", dueDay: 15, amount: 4 },
            { name: "ChatGPT", dueDay: 15, amount: 20 },
            { name: "Printing", dueDay: 20, amount: 40 },
            { name: "Shopify", dueDay: 20, amount: 40 },
            { name: "HELOC", dueDay: 25, amount: 200, interest: 7.5 },
            { name: "Natural Gas", dueDay: 13, amount: 30 },
            { name: "Groceries", dueDay: 1, amount: 400 },
            { name: "Entertainment", dueDay: 1, amount: 200 }
        ];

        // Income data
        const weeklyIncome = 1100;
        const tenant1Income = 1500;
        const tenant2Income = 700;
        const eggsPerDay = 6;
        const eggPrice = 0.50;
        const eggIncome = eggsPerDay * eggPrice * 30;
        const sobrietyStart = new Date("2024-03-01");
        const sobrietySavingsPerWeek = 60;
        const daysSober = Math.floor((today - sobrietyStart) / (1000 * 60 * 60 * 24));
        const weeksSober = Math.floor(daysSober / 7);
        const sobrietySavings = weeksSober * sobrietySavingsPerWeek;
        const monthlyIncome = (weeklyIncome * 4) + tenant1Income + tenant2Income + eggIncome + (sobrietySavingsPerWeek * 4);

        // Financial data
        const savings = 7500; // Updated to $7,500 base
        const savingsGoal = 10000;
        const helocOwed = 31000;
        const helocAvailable = 40000;
        const houseValue = 310000;
        const houseBought = 171000;
        const tundraValue = 10000;

        // Days until due
        function daysUntilDue(dueDay) {
            const currentDay = today.getDate();
            const currentMonth = today.getMonth();
            const currentYear = today.getFullYear();
            let dueDate = new Date(currentYear, currentMonth, dueDay);
            if (currentDay > dueDay) dueDate.setMonth(currentMonth + 1);
            const diffTime = dueDate - today;
            return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
        }

        // Sort bills by days until due
        bills.sort((a, b) => daysUntilDue(a.dueDay) - daysUntilDue(b.dueDay));

        // Render bills
        const billContainer = document.getElementById('bill-container');
        bills.forEach(bill => {
            const daysLeft = daysUntilDue(bill.dueDay);
            const billDiv = document.createElement('div');
            billDiv.classList.add('card');
            if (daysLeft <= 3 && daysLeft >= 0) billDiv.classList.add('urgent');
            else if (daysLeft <= 7) billDiv.classList.add('near-due');
            if (bill.amount < 100 && daysLeft > 7) billDiv.classList.add('small');
            billDiv.innerHTML = `
                <h3>${bill.name}</h3>
                <p>$${bill.amount}${bill.interest ? ` (${bill.interest}%)` : ''}</p>
                <p>Due in ${daysLeft >= 0 ? daysLeft : "Overdue by " + Math.abs(daysLeft)} days</p>
            `;
            billContainer.appendChild(billDiv);
        });

        // Summary
        const totalExpenses = bills.reduce((sum, bill) => sum + bill.amount, 0);
        const netCashFlow = monthlyIncome - totalExpenses;
        document.getElementById('total-income').textContent = monthlyIncome.toFixed(2);
        document.getElementById('total-expenses').textContent = totalExpenses.toFixed(2);
        document.getElementById('net-cash-flow').textContent = netCashFlow.toFixed(2);

        // Goals Progress
        const savingsProgress = (savings / savingsGoal) * 100;
        const helocProgress = ((helocAvailable - helocOwed) / helocAvailable) * 100;
        const monthsLeft = Math.ceil((new Date("2024-12-31") - today) / (1000 * 60 * 60 * 24 * 30));
        const savingsTargetMonthly = (savingsGoal - savings) / monthsLeft;
        const helocTargetMonthly = helocOwed / monthsLeft;

        document.getElementById('savings').textContent = savings.toFixed(2);
        document.getElementById('savings-progress').style.width = `${savingsProgress}%`;
        document.getElementById('savings-progress').textContent = `${savingsProgress.toFixed(1)}% ($${savingsTargetMonthly.toFixed(2)}/mo)`;
        document.getElementById('savings-progress').classList.add('success');

        document.getElementById('heloc-progress').style.width = `${helocProgress}%`;
        document.getElementById('heloc-progress').textContent = `${helocProgress.toFixed(1)}% ($${helocTargetMonthly.toFixed(2)}/mo)`;
        document.getElementById('heloc-progress').classList.add('warning');

        // Assets
        const houseEquity = houseValue - houseBought;
        document.getElementById('house-equity').textContent = houseEquity;
        document.getElementById('heloc-owed-display').textContent = helocOwed;

        // Egg Income
        const eggsToday = eggsPerDay;
        const eggIncomeToday = eggsToday * eggPrice;
        document.getElementById('eggs-today').textContent = `${eggsToday} ($${eggIncomeToday.toFixed(2)})`;
        document.getElementById('egg-income').textContent = eggIncome.toFixed(2);

        // Sobriety Tracker
        document.getElementById('days-sober').textContent = daysSober;
        document.getElementById('years-sober').textContent = (daysSober / 365).toFixed(1);
        document.getElementById('sobriety-savings').textContent = sobrietySavings.toFixed(2);

        // Charts
        // 1. Income vs Expenses
        new Chart(document.getElementById('incomeExpenseChart').getContext('2d'), {
            type: 'bar',
            data: {
                labels: ['Income', 'Expenses'],
                datasets: [{
                    label: 'Monthly ($)',
                    data: [monthlyIncome, totalExpenses],
                    backgroundColor: ['#00ff7f', '#ff4500'],
                    borderWidth: 1
                }]
            },
            options: { scales: { y: { beginAtZero: true } } }
        });

        // 2. Expense Breakdown Pie
        new Chart(document.getElementById('expensePieChart').getContext('2d'), {
            type: 'pie',
            data: {
                labels: bills.map(b => b.name),
                datasets: [{
                    data: bills.map(b => b.amount),
                    backgroundColor: ['#00b7eb', '#ff4500', '#00ff7f', '#ffd700', '#ff00ff', '#1a202c', '#00ced1', '#ff8c00', '#a9a9a9', '#ff69b4', '#b0c4de', '#9400d3', '#f0f8ff'],
                    borderWidth: 1
                }]
            }
        });

        // 3. Goals Progress
        new Chart(document.getElementById('goalsChart').getContext('2d'), {
            type: 'bar',
            data: {
                labels: ['Savings', 'HELOC Payoff'],
                datasets: [{
                    label: 'Current ($)',
                    data: [savings, helocOwed],
                    backgroundColor: ['#00b7eb', '#ff4500'],
                    borderWidth: 1
                }, {
                    label: 'Target ($)',
                    data: [savingsGoal, 0],
                    backgroundColor: ['#008bb5', '#d63031'],
                    borderWidth: 1
                }]
            },
            options: { scales: { y: { beginAtZero: true } } }
        });

        // 4. Sobriety Savings Line
        new Chart(document.getElementById('sobrietySavingsChart').getContext('2d'), {
            type: 'line',
            data: {
                labels: Array.from({ length: weeksSober + 1 }, (_, i) => `W${i}`),
                datasets: [{
                    label: 'Savings ($)',
                    data: Array.from({ length: weeksSober + 1 }, (_, i) => i * sobrietySavingsPerWeek),
                    borderColor: '#fff',
                    backgroundColor: 'rgba(255, 255, 255, 0.3)',
                    fill: true,
                    tension: 0.3
                }]
            },
            options: { scales: { y: { beginAtZero: true } } }
        });

        // 5. Net Worth
        const netWorth = houseEquity + tundraValue + savings - helocOwed;
        new Chart(document.getElementById('netWorthChart').getContext('2d'), {
            type: 'doughnut',
            data: {
                labels: ['House Equity', 'Tundra', 'Savings', 'HELOC Debt'],
                datasets: [{
                    data: [houseEquity, tundraValue, savings, helocOwed],
                    backgroundColor: ['#ff00ff', '#ffd700', '#00ff7f', '#ff4500'],
                    borderWidth: 1
                }]
            }
        });
    </script>
</body>
</html>
