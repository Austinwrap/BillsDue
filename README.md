<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bill Slayer HQ</title>
    <style>
        body {
            font-family: 'Bebas Neue', Arial, sans-serif;
            margin: 0;
            padding: 10px;
            background: linear-gradient(135deg, #ffcc00, #ff00cc, #00ff99); /* Yellow-magenta-green */
            color: #fff;
            max-width: 375px;
            margin-left: auto;
            margin-right: auto;
            overflow-x: hidden;
            box-sizing: border-box;
            animation: bgPulse 6s infinite alternate;
        }
        h1 {
            font-size: 2.2em;
            text-align: center;
            color: #ff00cc; /* Neon magenta */
            text-shadow: 0 0 10px #00ff99, 0 0 20px #ffcc00;
            margin: 10px 0;
            animation: titleSlam 1.5s infinite;
        }
        #current-date {
            text-align: center;
            font-size: 1.1em;
            color: #fff;
            margin-bottom: 15px;
            text-shadow: 0 0 5px #000;
        }
        .container {
            display: flex;
            flex-direction: column;
            gap: 12px;
            width: 100%;
        }
        .card {
            background: rgba(0, 0, 0, 0.85);
            padding: 12px;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.5), inset 0 0 5px #ff00cc;
            text-align: center;
            transition: transform 0.2s, box-shadow 0.2s;
            cursor: pointer;
            width: 100%;
            box-sizing: border-box;
        }
        .card:hover {
            transform: scale(1.04);
            box-shadow: 0 8px 20px rgba(0,0,0,0.7), inset 0 0 10px #00ff99;
        }
        .card h3 {
            font-size: 1.4em;
            margin: 0;
            color: #fff;
            text-shadow: 0 0 6px #000;
        }
        .card p {
            font-size: 0.9em;
            margin: 4px 0;
            color: #e0e0e0;
            text-shadow: 0 0 3px #000;
        }
        .card.urgent {
            border: 4px solid #ff00cc; /* Neon magenta */
            padding: 15px;
            transform: scale(1.06);
            background: rgba(50, 0, 50, 0.9);
            box-shadow: 0 0 20px #ff00cc, 0 0 30px #00ff99, inset 0 0 10px #ff00cc;
            animation: urgentScream 0.5s infinite;
        }
        .card.near-due {
            border: 3px solid #00ff99; /* Neon green */
            padding: 14px;
            transform: scale(1.03);
            box-shadow: 0 0 15px #00ff99, inset 0 0 5px #00ff99;
            animation: nearBuzz 1s infinite;
        }
        .card.small {
            padding: 10px;
            font-size: 0.8em;
        }
        .card.small h3 { font-size: 1.2em; }
        .card.completed {
            background: linear-gradient(135deg, #ffcc00, #ff8c00); /* Yellow-orange victory */
            border: 4px solid #ffcc00;
            box-shadow: 0 0 20px #ffcc00, inset 0 0 10px #ffcc00;
            animation: victoryGlow 1.2s infinite;
        }
        .card.completed h3::after {
            content: " 🤘 SLAYED";
            color: #fff;
            font-size: 0.8em;
            text-shadow: 0 0 5px #000;
        }
        .section {
            margin: 20px 0;
            background: rgba(0, 0, 0, 0.75);
            padding: 12px;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.5), inset 0 0 5px #00ff99;
            width: 100%;
            box-sizing: border-box;
            animation: fadeIn 0.8s ease-in;
            position: relative;
        }
        .section h2 {
            font-size: 1.6em;
            color: #ff00cc;
            text-align: center;
            margin: 8px 0;
            text-shadow: 0 0 8px #00ff99;
        }
        canvas {
            max-width: 100%;
            margin: 10px 0;
            border: 1px solid #ff00cc;
            border-radius: 5px;
            cursor: pointer;
        }
        .chart-popout {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 350px;
            padding: 15px;
            background: rgba(0, 0, 0, 0.9);
            border: 3px solid #ffcc00;
            box-shadow: 0 0 30px #ff00cc;
            z-index: 1000;
            border-radius: 10px;
        }
        .chart-popout canvas {
            max-width: 100%;
        }
        .chart-popout h2 {
            font-size: 1.8em;
            margin-bottom: 10px;
        }
        .progress-bar {
            width: 100%;
            margin: 10px 0;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 6px;
            overflow: hidden;
            box-shadow: 0 0 8px #ff00cc;
        }
        .progress {
            height: 18px;
            text-align: center;
            color: #fff;
            font-size: 0.8em;
            transition: width 1s ease-in-out;
            text-shadow: 0 0 4px #000;
        }
        .progress.success { background: #ffcc00; }
        .progress.warning { background: #ff4500; }
        .sobriety-section {
            background: linear-gradient(135deg, #ffcc00, #ff8c00);
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 0 20px #ffcc00, inset 0 0 10px #00ff99;
            width: 100%;
            box-sizing: border-box;
        }
        .sobriety-section h2 {
            font-size: 1.7em;
            text-shadow: 0 0 10px #ff00cc;
        }
        .sobriety-section p {
            font-size: 1em;
            margin: 6px 0;
        }
        .checklist {
            margin-top: 10px;
            text-align: left;
            font-size: 0.9em;
            color: #e0e0e0;
        }
        .checklist-item {
            display: flex;
            align-items: center;
            margin: 5px 0;
        }
        .checklist-item input {
            margin-right: 8px;
        }
        @keyframes bgPulse {
            0% { background-position: 0% 0%; }
            100% { background-position: 100% 100%; }
        }
        @keyframes titleSlam {
            0% { transform: scale(1); text-shadow: 0 0 10px #00ff99, 0 0 20px #ffcc00; }
            50% { transform: scale(1.05); text-shadow: 0 0 15px #00ff99, 0 0 30px #ffcc00; }
            100% { transform: scale(1); text-shadow: 0 0 10px #00ff99, 0 0 20px #ffcc00; }
        }
        @keyframes urgentScream {
            0% { box-shadow: 0 0 15px #ff00cc, 0 0 25px #00ff99, inset 0 0 10px #ff00cc; }
            50% { box-shadow: 0 0 30px #ff00cc, 0 0 40px #00ff99, inset 0 0 15px #ff00cc; }
            100% { box-shadow: 0 0 15px #ff00cc, 0 0 25px #00ff99, inset 0 0 10px #ff00cc; }
        }
        @keyframes nearBuzz {
            0% { box-shadow: 0 0 10px #00ff99, inset 0 0 5px #00ff99; }
            50% { box-shadow: 0 0 20px #00ff99, inset 0 0 10px #00ff99; }
            100% { box-shadow: 0 0 10px #00ff99, inset 0 0 5px #00ff99; }
        }
        @keyframes victoryGlow {
            0% { box-shadow: 0 0 15px #ffcc00, inset 0 0 10px #ffcc00; }
            50% { box-shadow: 0 0 25px #ffcc00, inset 0 0 15px #ffcc00; }
            100% { box-shadow: 0 0 15px #ffcc00, inset 0 0 10px #ffcc00; }
        }
        @keyframes fadeIn {
            0% { opacity: 0; transform: translateY(10px); }
            100% { opacity: 1; transform: translateY(0); }
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap" rel="stylesheet">
</head>
<body>
    <h1>Bill Slayer HQ</h1>
    <div id="current-date">Loading date...</div>
    <div class="container" id="bill-container"></div>

    <div class="section">
        <h2>💸 Cash Flow 💸</h2>
        <p>Income: $<span id="total-income">0</span></p>
        <p>Expenses: $<span id="total-expenses">0</span></p>
        <p>Net: $<span id="net-cash-flow">0</span></p>
        <canvas id="incomeExpenseChart"></canvas>
        <div class="checklist" id="bill-checklist"></div>
    </div>

    <div class="section">
        <h2>📈 Bill Breakdown 📈</h2>
        <canvas id="expensePieChart"></canvas>
    </div>

    <div class="section">
        <h2>⚡ Goals ⚡</h2>
        <p>Savings: $<span id="savings">7500</span> / $10,000</p>
        <div class="progress-bar"><div class="progress" id="savings-progress"></div></div>
        <p>HELOC: $<span id="heloc-owed">31000</span> / $0</p>
        <div class="progress-bar"><div class="progress" id="heloc-progress"></div></div>
        <canvas id="goalsChart"></canvas>
    </div>

    <div class="section sobriety-section">
        <h2>🍺 Sobriety Kick 🍺</h2>
        <p>Days: <span id="days-sober">0</span> (<span id="years-sober">0</span> yrs)</p>
        <p>Savings: $<span id="sobriety-savings">0</span></p>
        <canvas id="sobrietySavingsChart"></canvas>
    </div>

    <div class="section">
        <h2>🏍️ Assets & Debts 🏍️</h2>
        <p>House: $310,000 (Equity: $<span id="house-equity">0</span>)</p>
        <p>Tundra: $10,000+</p>
        <p>HELOC: $<span id="heloc-owed-display">31000</span> / $40,000</p>
        <canvas id="netWorthChart"></canvas>
    </div>

    <div class="section">
        <h2>🐔 Egg Hustle 🐔</h2>
        <p>Today: <span id="eggs-today">0</span> ($0.50/egg)</p>
        <p>Monthly: $<span id="egg-income">0</span></p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        // Current date
        const today = new Date();
        document.getElementById('current-date').textContent = `Today: ${today.toDateString()}`;

        // Bill data
        const bills = [
            { name: "Student Loans", dueDay: 6, amount: 260, completed: false },
            { name: "State Farm Insurance", dueDay: 6, amount: 130, completed: false },
            { name: "Mortgage", dueDay: 4, amount: 1690, interest: 3.5, completed: false },
            { name: "Eversource", dueDay: 13, amount: 175, completed: false },
            { name: "Verizon", dueDay: 6, amount: 100, completed: false },
            { name: "GitHub", dueDay: 15, amount: 4, completed: false },
            { name: "ChatGPT", dueDay: 15, amount: 20, completed: false },
            { name: "Printing", dueDay: 20, amount: 40, completed: false },
            { name: "Shopify", dueDay: 20, amount: 40, completed: false },
            { name: "HELOC", dueDay: 25, amount: 200, interest: 7.5, completed: false },
            { name: "Natural Gas", dueDay: 13, amount: 30, completed: false },
            { name: "Groceries", dueDay: 1, amount: 400, completed: false },
            { name: "Entertainment", dueDay: 1, amount: 200, completed: false }
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
        const savings = 7500;
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

        // Render bills with interactivity
        const billContainer = document.getElementById('bill-container');
        bills.forEach(bill => {
            const daysLeft = daysUntilDue(bill.dueDay);
            const billDiv = document.createElement('div');
            billDiv.classList.add('card');
            if (daysLeft <= 3 && daysLeft >= 0) billDiv.classList.add('urgent');
            else if (daysLeft <= 7) billDiv.classList.add('near-due');
            if (bill.amount < 100 && daysLeft > 7) billDiv.classList.add('small');
            if (bill.completed) billDiv.classList.add('completed');
            billDiv.innerHTML = `
                <h3>${bill.name}</h3>
                <p>$${bill.amount}${bill.interest ? ` (${bill.interest}%)` : ''}</p>
                <p>Due in ${daysLeft >= 0 ? daysLeft : "Overdue by " + Math.abs(daysLeft)} days</p>
            `;
            billDiv.addEventListener('click', () => {
                if (!billDiv.classList.contains('completed')) {
                    billDiv.classList.add('completed');
                    bill.completed = true;
                    updateChecklist();
                }
            });
            billContainer.appendChild(billDiv);
        });

        // Bill checklist
        function updateChecklist() {
            const checklist = document.getElementById('bill-checklist');
            checklist.innerHTML = '<h3>BILL SLAY LIST</h3>';
            bills.forEach(bill => {
                const item = document.createElement('div');
                item.classList.add('checklist-item');
                item.innerHTML = `
                    <input type="checkbox" ${bill.completed ? 'checked' : ''} disabled>
                    ${bill.name} - $${bill.amount}
                `;
                checklist.appendChild(item);
            });
        }
        updateChecklist();

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

        // Chart pop-out functionality
        const charts = [
            { id: 'incomeExpenseChart', title: 'Cash Flow', data: { labels: ['Income', 'Expenses'], datasets: [{ label: 'Monthly ($)', data: [monthlyIncome, totalExpenses], backgroundColor: ['#00ff99', '#ff00cc'], borderWidth: 1 }] }, type: 'bar' },
            { id: 'expensePieChart', title: 'Bill Breakdown', data: { labels: bills.map(b => b.name), datasets: [{ data: bills.map(b => b.amount), backgroundColor: ['#00ff99', '#ff00cc', '#ffcc00', '#ffd700', '#ff4500', '#1a0033', '#00b894', '#ff8c00', '#a0aec0', '#f56565', '#b0c4de', '#9400d3', '#e0e0e0'], borderWidth: 1 }] }, type: 'pie' },
            { id: 'goalsChart', title: 'Goals', data: { labels: ['Savings', 'HELOC Payoff'], datasets: [{ label: 'Current ($)', data: [savings, helocOwed], backgroundColor: ['#00ff99', '#ff00cc'], borderWidth: 1 }, { label: 'Target ($)', data: [savingsGoal, 0], backgroundColor: ['#00cc66', '#d63031'], borderWidth: 1 }] }, type: 'bar' },
            { id: 'sobrietySavingsChart', title: 'Sobriety Savings', data: { labels: Array.from({ length: weeksSober + 1 }, (_, i) => `W${i}`), datasets: [{ label: 'Savings ($)', data: Array.from({ length: weeksSober + 1 }, (_, i) => i * sobrietySavingsPerWeek), borderColor: '#fff', backgroundColor: 'rgba(255, 255, 255, 0.3)', fill: true, tension: 0.3 }] }, type: 'line' },
            { id: 'netWorthChart', title: 'Net Worth', data: { labels: ['House Equity', 'Tundra', 'Savings', 'HELOC Debt'], datasets: [{ data: [houseEquity, tundraValue, savings, helocOwed], backgroundColor: ['#ff00cc', '#ffd700', '#00ff99', '#ff4500'], borderWidth: 1 }] }, type: 'doughnut' }
        ];

        charts.forEach(chart => {
            const canvas = document.getElementById(chart.id);
            const ctx = canvas.getContext('2d');
            const chartInstance = new Chart(ctx, {
                type: chart.type,
                data: chart.data,
                options: { scales: chart.type === 'bar' || chart.type === 'line' ? { y: { beginAtZero: true } } : {} }
            });

            canvas.addEventListener('click', () => {
                const existingPopout = document.querySelector('.chart-popout');
                if (existingPopout) existingPopout.remove();

                const popout = document.createElement('div');
                popout.classList.add('chart-popout');
                popout.innerHTML = `<h2>${chart.title}</h2><canvas id="popout-${chart.id}"></canvas>`;
                document.body.appendChild(popout);

                const popoutCanvas = document.getElementById(`popout-${chart.id}`);
                const popoutCtx = popoutCanvas.getContext('2d');
                new Chart(popoutCtx, {
                    type: chart.type,
                    data: chart.data,
                    options: {
                        scales: chart.type === 'bar' || chart.type === 'line' ? { y: { beginAtZero: true } } : {},
                        plugins: { legend: { labels: { font: { size: 16 } } } },
                        animation: { duration: 1000, easing: 'easeInOutBounce' }
                    }
                });

                popout.addEventListener('click', () => popout.remove());
            });
        });
    </script>
</body>
</html>
