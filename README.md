<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multi-Calculator App</title>
    <link href="fonts.googleapis.com" rel="stylesheet">
    <style>
        /* --- CSS Reset and Variables --- */
        :root {
            --bg-main: #ffffff;
            --bg-card: #f9f9f9;
            --text-color: #121212;
            --btn-bg: #e0e0e0;
            --btn-text: #121212;
            --accent-color: #007bff;
            --border-color: #ddd;
            --shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            --header-bg: #fff;
        }

        [data-theme="dark"] {
            --bg-main: #121212;
            --bg-card: #1e1e1e;
            --text-color: #e0e0e0;
            --btn-bg: #333333;
            --btn-text: #ffffff;
            --accent-color: #4da6ff;
            --border-color: #444;
            --shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
            --header-bg: #1e1e1e;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: Arial, sans-serif;
            background-color: var(--bg-main);
            color: var(--text-color);
            transition: background-color 0.3s, color 0.3s;
            padding-bottom: 60px; /* Space for bottom nav */
        }

        /* --- Header and Settings --- */
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background-color: var(--header-bg);
            box-shadow: var(--shadow);
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            z-index: 10;
        }

        .settings-btn {
            background: none;
            border: none;
            color: var(--text-color);
            cursor: pointer;
            font-size: 24px;
            padding: 5px;
        }

        .settings-modal {
            display: none;
            position: fixed;
            z-index: 20;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background-color: var(--bg-card);
            padding: 20px;
            border-radius: 8px;
            width: 80%;
            max-width: 300px;
            box-shadow: var(--shadow);
        }

        .close-btn {
            color: var(--text-color);
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }

        .settings-option {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
        }

        /* --- Main Content Area --- */
        main {
            padding: 70px 10px 10px; /* Offset for header and bottom nav */
        }

        .tab-content {
            display: none;
            animation: fadeIn 0.5s;
        }

        .tab-content.active {
            display: block;
        }

        @keyframes fadeIn {
            from {opacity: 0;}
            to {opacity: 1;}
        }

        .card {
            background-color: var(--bg-card);
            padding: 20px;
            border-radius: 8px;
            box-shadow: var(--shadow);
            margin-bottom: 20px;
        }

        h2 {
            color: var(--accent-color);
            margin-bottom: 15px;
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 10px;
        }

        /* --- Calculator Specific Styles (Tab 1) --- */
        .calculator-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
        }

        .calc-display {
            grid-column: 1 / -1;
            background-color: var(--bg-main);
            color: var(--text-color);
            padding: 15px;
            text-align: right;
            border-radius: 5px;
            font-size: 2.5rem;
            margin-bottom: 10px;
            min-height: 70px;
            word-wrap: break-word;
        }

        .calc-btn {
            background-color: var(--btn-bg);
            color: var(--btn-text);
            border: none;
            padding: 15px;
            font-size: 1.2rem;
            border-radius: 5px;
            cursor: pointer;
            transition: background-color 0.2s;
            min-height: 50px;
        }

        .calc-btn:hover {
            background-color: var(--border-color);
        }

        .span-2 {
            grid-column: span 2;
        }

        .operator-btn {
            background-color: var(--accent-color);
            color: white;
        }

        /* --- Sliders and Results (Tabs 2 & 3) --- */
        .input-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        .slider-container {
            display: flex;
            align-items: center;
        }

        input[type="range"] {
            flex-grow: 1;
            margin-right: 15px;
        }
        
        .slider-value {
            font-weight: bold;
            min-width: 70px;
            text-align: right;
            color: var(--accent-color);
        }

        .results {
            margin-top: 20px;
            padding-top: 15px;
            border-top: 1px solid var(--border-color);
        }

        .result-item {
            display: flex;
            justify-content: space-between;
            padding: 5px 0;
            font-size: 1.1rem;
        }

        .result-value {
            font-weight: bold;
            color: var(--accent-color);
        }

        /* --- Bottom Navigation --- */
        nav.bottom-nav {
            display: flex;
            justify-content: space-around;
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background-color: var(--header-bg);
            box-shadow: var(--shadow);
            height: 60px;
            border-top: 1px solid var(--border-color);
        }

        .nav-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            flex: 1;
            cursor: pointer;
            color: var(--text-color);
            text-decoration: none;
            font-size: 0.8rem;
            transition: color 0.3s;
        }

        .nav-item.active, .nav-item:hover {
            color: var(--accent-color);
        }

        .nav-item i {
            font-size: 24px;
            margin-bottom: 2px;
        }
    </style>
</head>
<body data-theme="light">

    <header>
        <h1>Calculator Hub</h1>
        <button class="settings-btn" onclick="openSettings()">⚙️</button>
    </header>

    <main>
        <!-- Tab 1: Calculator -->
        <div id="tab-calculator" class="tab-content active">
            <div class="card">
                <h2>Scientific Calculator</h2>
                <div class="calculator-grid">
                    <div class="calc-display" id="calc-display">0</div>
                    <button class="calc-btn operator-btn span-2" onclick="calcClear()">C</button>
                    <button class="calc-btn operator-btn" onclick="calcInput('%')">%</button>
                    <button class="calc-btn operator-btn" onclick="calcInput('/')">÷</button>
                    <button class="calc-btn" onclick="calcInput('7')">7</button>
                    <button class="calc-btn" onclick="calcInput('8')">8</button>
                    <button class="calc-btn" onclick="calcInput('9')">9</button>
                    <button class="calc-btn operator-btn" onclick="calcInput('*')">×</button>
                    <button class="calc-btn" onclick="calcInput('4')">4</button>
                    <button class="calc-btn" onclick="calcInput('5')">5</button>
                    <button class="calc-btn" onclick="calcInput('6')">6</button>
                    <button class="calc-btn operator-btn" onclick="calcInput('-')">−</button>
                    <button class="calc-btn" onclick="calcInput('1')">1</button>
                    <button class="calc-btn" onclick="calcInput('2')">2</button>
                    <button class="calc-btn" onclick="calcInput('3')">3</button>
                    <button class="calc-btn operator-btn" onclick="calcInput('+')">+</button>
                    <button class="calc-btn span-2" onclick="calcInput('0')">0</button>
                    <button class="calc-btn" onclick="calcInput('.')">.</button>
                    <button class="calc-btn operator-btn" onclick="calcCalculate()">=</button>
                    <!-- Scientific Functions -->
                    <button class="calc-btn" onclick="calcScientific('sqrt')">√x</button>
                    <button class="calc-btn" onclick="calcScientific('sq')">x²</button>
                    <button class="calc-btn" onclick="calcScientific('pow')">xʸ</button>
                    <button class="calc-btn" onclick="calcScientific('sin')">sin</button>
                    <button class="calc-btn" onclick="calcScientific('cos')">cos</button>
                    <button class="calc-btn" onclick="calcScientific('tan')">tan</button>
                </div>
            </div>
        </div>

        <!-- Tab 2: SIP Calculator -->
        <div id="tab-sip" class="tab-content">
            <div class="card">
                <h2>SIP Calculator</h2>
                <div class="input-group">
                    <label for="monthlyInvestment">Monthly Investment: <span id="inv-value" class="slider-value"></span></label>
                    <div class="slider-container">
                        <input type="range" id="monthlyInvestment" min="500" max="100000" value="10000" step="100" oninput="updateSIP()">
                    </div>
                </div>
                <div class="input-group">
                    <label for="annualReturn">Expected Annual Return (%): <span id="return-value" class="slider-value"></span></label>
                    <div class="slider-container">
                        <input type="range" id="annualReturn" min="1" max="30" value="12" step="0.1" oninput="updateSIP()">
                    </div>
                </div>
                <div class="input-group">
                    <label for="timePeriod">Time Period (Years): <span id="time-value" class="slider-value"></span></label>
                    <div class="slider-container">
                        <input type="range" id="timePeriod" min="1" max="50" value="10" step="1" oninput="updateSIP()">
                    </div>
                </div>

                <div class="results">
                    <h3>Results</h3>
                    <div class="result-item"><span>Total Investment</span><span id="totalInvestment" class="result-value"></span></div>
                    <div class="result-item"><span>Estimated Returns</span><span id="estimatedReturns" class="result-value"></span></div>
                    <div class="result-item"><span>Total Value</span><span id="totalValue" class="result-value"></span></div>
                </div>
            </div>
        </div>

        <!-- Tab 3: Stock Calculator -->
        <div id="tab-stock" class="tab-content">
            <div class="card">
                <h2>Stock Calculator</h2>
                <div class="input-group">
                    <label for="buyPrice">Buy Price (per share): <span id="buy-price-value" class="slider-value"></span></label>
                    <div class="slider-container">
                        <input type="range" id="buyPrice" min="1" max="5000" value="100" step="0.1" oninput="updateStock()">
                    </div>
                </div>
                <div class="input-group">
                    <label for="quantity">Quantity: <span id="quantity-value" class="slider-value"></span></label>
                    <div class="slider-container">
                        <input type="range" id="quantity" min="1" max="1000" value="10" step="1" oninput="updateStock()">
                    </div>
                </div>
                <div class="input-group">
                    <label for="sellPrice">Sell Price (per share): <span id="sell-price-value" class="slider-value"></span></label>
                    <div class="slider-container">
                        <input type="range" id="sellPrice" min="1" max="5000" value="120" step="0.1" oninput="updateStock()">
                    </div>
                </div>

                <div class="results">
                    <h3>Results</h3>
                    <div class="result-item"><span>Invested Amount</span><span id="investedAmount" class="result-value"></span></div>
                    <div class="result-item"><span>Profit / Loss</span><span id="profitLoss" class="result-value"></span></div>
                    <div class="result-item"><span>Percentage Gain / Loss</span><span id="percentGainLoss" class="result-value"></span></div>
                </div>
            </div>
        </div>
    </main>

    <!-- Settings Modal -->
    <div id="settingsModal" class="settings-modal">
        <div class="modal-content">
            <span class="close-btn" onclick="closeSettings()">&times;</span>
            <h3>Settings</h3>
            <div class="settings-option">
                <span>Dark Mode</span>
                <label class="switch">
                    <input type="checkbox" id="darkModeToggle">
                </label>
            </div>
        </div>
    </div>


    <nav class="bottom-nav">
        <a href="#calculator" class="nav-item active" onclick="switchTab(event, 'tab-calculator')">
            <i class="material-icons">calculate</i>
            <span>Calculator</span>
        </a>
        <a href="#sip" class="nav-item" onclick="switchTab(event, 'tab-sip')">
            <i class="material-icons">trending_up</i>
            <span>SIP</span>
        </a>
        <a href="#stock" class="nav-item" onclick="switchTab(event, 'tab-stock')">
            <i class="material-icons">show_chart</i>
            <span>Stock</span>
        </a>
    </nav>

    <script>
        /* --- General JS and Dark Mode Logic --- */
        document.addEventListener('DOMContentLoaded', (event) => {
            loadTheme();
            // Initialize calculators on load with default values
            updateSIP();
            updateStock();
            // Setup Dark Mode Toggle listener
            document.getElementById('darkModeToggle').addEventListener('change', toggleDarkMode);
        });

        function switchTab(event, tabId) {
            event.preventDefault();
            // Hide all tabs
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            // Deactivate all nav items
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('active');
            });
            // Show the selected tab and activate the nav item
            document.getElementById(tabId).classList.add('active');
            event.currentTarget.classList.add('active');
        }

        // Settings Modal Handlers
        function openSettings() {
            document.getElementById('settingsModal').style.display = 'flex';
        }

        function closeSettings() {
            document.getElementById('settingsModal').style.display = 'none';
        }

        // Dark Mode Handlers
        function loadTheme() {
            const savedTheme = localStorage.getItem('theme') || 'light';
            document.body.setAttribute('data-theme', savedTheme);
            document.getElementById('darkModeToggle').checked = (savedTheme === 'dark');
        }

        function toggleDarkMode(event) {
            const isChecked = event.target.checked;
            const newTheme = isChecked ? 'dark' : 'light';
            document.body.setAttribute('data-theme', newTheme);
            localStorage.setItem('theme', newTheme);
        }

        // Helper function for currency formatting
        const formatter = new Intl.NumberFormat('en-IN', {
            style: 'currency',
            currency: 'INR',
            minimumFractionDigits: 0,
            maximumFractionDigits: 0,
        });
        
        const floatFormatter = new Intl.NumberFormat('en-US', {
            minimumFractionDigits: 2,
            maximumFractionDigits: 2,
        });

        /* --- Tab 1: Scientific Calculator Logic --- */
        let currentInput = '0';
        let operator = null;
        let previousInput = null;

        function calcUpdateDisplay() {
            document.getElementById('calc-display').innerText = currentInput;
        }

        function calcInput(value) {
            if (currentInput === '0') {
                currentInput = value;
            } else {
                currentInput += value;
            }
            calcUpdateDisplay();
        }

        function calcClear() {
            currentInput = '0';
            operator = null;
            previousInput = null;
            calcUpdateDisplay();
        }

        function calcCalculate() {
            let result;
            const prev = parseFloat(previousInput);
            const current = parseFloat(currentInput);
            if (isNaN(prev) || isNaN(current)) return;

            switch (operator) {
                case '+': result = prev + current; break;
                case '-': result = prev - current; break;
                case '*': result = prev * current; break;
                case '/': result = prev / current; break;
                case '%': result = prev % current; break;
                default: return;
            }
            currentInput = result.toString();
            operator = null;
            previousInput = null;
            calcUpdateDisplay();
        }

        function calcScientific(func) {
            const current = parseFloat(currentInput);
            if (isNaN(current)) return;
            let result;

            switch (func) {
                case 'sqrt': result = Math.sqrt(current); break;
                case 'sq': result = Math.pow(current, 2); break;
                case 'pow': 
                    // Handle x^y operation setup
                    previousInput = current;
                    operator = '^'; // Custom operator for power
                    currentInput = '0';
                    return;
                case 'sin': result = Math.sin(current * Math.PI / 180); break;
                case 'cos': result = Math.cos(current * Math.PI / 180); break;
                case 'tan': result = Math.tan(current * Math.PI / 180); break;
                default: return;
            }
            currentInput = result.toString();
            calcUpdateDisplay();
        }


        /* --- Tab 2: SIP Calculator Logic --- */
        function updateSIP() {
            const monthlyInvestment = parseFloat(document.getElementById('monthlyInvestment').value);
            const annualReturn = parseFloat(document.getElementById('annualReturn').value);
            const timePeriod = parseFloat(document.getElementById('timePeriod').value);

            // Update slider value displays
            document.getElementById('inv-value').textContent = formatter.format(monthlyInvestment);
            document.getElementById('return-value').textContent = `${annualReturn}%`;
            document.getElementById('time-value').textContent = `${timePeriod} Yrs`;

            // Calculate SIP
            const monthlyRate = annualReturn / 12 / 100;
            const months = timePeriod * 12;
            
            // Formula: FV = P * ((((1 + r)^n) - 1) / r) * (1 + r)
            if (monthlyRate === 0) {
                 // Handle zero rate case gracefully
                 const totalInvestment = monthlyInvestment * months;
                 const estimatedReturns = 0;
                 const totalValue = totalInvestment;
                 displaySIPResults(totalInvestment, estimatedReturns, totalValue);
                 return;
            }

            const futureValue = monthlyInvestment * (Math.pow(1 + monthlyRate, months) - 1) / monthlyRate * (1 + monthlyRate);
            const totalInvestment = monthlyInvestment * months;
            const estimatedReturns = futureValue - totalInvestment;
            const totalValue = futureValue;

            displaySIPResults(totalInvestment, estimatedReturns, totalValue);
        }

        function displaySIPResults(inv, returns, total) {
            document.getElementById('totalInvestment').textContent = formatter.format(inv);
            document.getElementById('estimatedReturns').textContent = formatter.format(returns);
            document.getElementById('totalValue').textContent = formatter.format(total);
        }

        /* --- Tab 3: Stock Calculator Logic --- */
        function updateStock() {
            const buyPrice = parseFloat(document.getElementById('buyPrice').value);
            const quantity = parseFloat(document.getElementById('quantity').value);
            const sellPrice = parseFloat(document.getElementById('sellPrice').value);

            // Update slider value displays
            document.getElementById('buy-price-value').textContent = floatFormatter.format(buyPrice);
            document.getElementById('quantity-value').textContent = quantity;
            document.getElementById('sell-price-value').textContent = floatFormatter.format(sellPrice);

            // Calculations
            const investedAmount = buyPrice * quantity;
            const profitLoss = (sellPrice - buyPrice) * quantity;
            const percentGainLoss = ((sellPrice - buyPrice) / buyPrice) * 100;

            displayStockResults(investedAmount, profitLoss, percentGainLoss);
        }

        function displayStockResults(invested, pl, percent) {
            document.getElementById('investedAmount').textContent = formatter.format(invested);
            document.getElementById('profitLoss').textContent = formatter.format(pl);
            document.getElementById('percentGainLoss').textContent = `${floatFormatter.format(percent)}%`;
            // Optional: Change color of P/L result based on value
            const plElement = document.getElementById('profitLoss');
            if (pl > 0) {
                plElement.style.color = 'green';
            } else if (pl < 0) {
                plElement.style.color = 'red';
            } else {
                plElement.style.color = 'var(--accent-color)';
            }
        }
    </script>
</body>
</html>
