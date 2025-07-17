<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Candle Euphoria - Business Transformation Dashboard</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
        }

        .header {
            text-align: center;
            margin-bottom: 40px;
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
            color: white;
            padding: 30px;
            border-radius: 15px;
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: repeating-linear-gradient(
                45deg,
                transparent,
                transparent 10px,
                rgba(255, 255, 255, 0.1) 10px,
                rgba(255, 255, 255, 0.1) 20px
            );
            animation: slide 20s linear infinite;
        }

        @keyframes slide {
            0% { transform: translateX(-50px); }
            100% { transform: translateX(50px); }
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .metric-card {
            background: white;
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            border-left: 5px solid #ff6b6b;
            transition: transform 0.3s ease, box-shadow 0.3s ease;
        }

        .metric-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.15);
        }

        .metric-card h3 {
            color: #333;
            margin-bottom: 15px;
            font-size: 1.1em;
        }

        .metric-value {
            font-size: 2.5em;
            font-weight: bold;
            color: #ff6b6b;
            margin-bottom: 10px;
        }

        .metric-change {
            font-size: 0.9em;
            color: #666;
        }

        .positive { color: #27ae60; }
        .negative { color: #e74c3c; }

        .progress-section {
            background: white;
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        }

        .progress-bar {
            background: #ecf0f1;
            height: 30px;
            border-radius: 15px;
            overflow: hidden;
            margin: 15px 0;
            position: relative;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #ff6b6b, #ee5a24);
            border-radius: 15px;
            transition: width 2s ease;
            position: relative;
        }

        .progress-fill::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
            animation: shimmer 2s infinite;
        }

        @keyframes shimmer {
            0% { transform: translateX(-100%); }
            100% { transform: translateX(100%); }
        }

        .phase-section {
            background: white;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
        }

        .phase-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 2px solid #ecf0f1;
        }

        .phase-title {
            font-size: 1.4em;
            color: #333;
            font-weight: bold;
        }

        .phase-status {
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.9em;
            font-weight: bold;
        }

        .status-active { background: #e8f5e8; color: #27ae60; }
        .status-pending { background: #fff3cd; color: #856404; }
        .status-completed { background: #d4edda; color: #155724; }

        .services-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
        }

        .service-item {
            background: #f8f9fa;
            padding: 20px;
            border-radius: 10px;
            border-left: 4px solid #ff6b6b;
            transition: all 0.3s ease;
        }

        .service-item:hover {
            background: #e9ecef;
            transform: translateX(5px);
        }

        .service-title {
            font-weight: bold;
            color: #333;
            margin-bottom: 8px;
        }

        .service-price {
            color: #ff6b6b;
            font-weight: bold;
            margin-bottom: 8px;
        }

        .service-description {
            font-size: 0.9em;
            color: #666;
            line-height: 1.4;
        }

        .chart-container {
            background: white;
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
        }

        .chart-title {
            text-align: center;
            font-size: 1.5em;
            color: #333;
            margin-bottom: 20px;
        }

        .roi-calculator {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            border-radius: 15px;
            margin-bottom: 30px;
        }

        .roi-calculator h3 {
            text-align: center;
            font-size: 1.5em;
            margin-bottom: 20px;
        }

        .calculator-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }

        .calculator-item {
            background: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 10px;
            text-align: center;
        }

        .calculator-item label {
            display: block;
            margin-bottom: 10px;
            font-weight: bold;
        }

        .calculator-item input {
            width: 100%;
            padding: 10px;
            border: none;
            border-radius: 5px;
            text-align: center;
            font-size: 1.1em;
        }

        .roi-result {
            background: rgba(255, 255, 255, 0.2);
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            font-size: 1.2em;
            font-weight: bold;
        }

        .action-buttons {
            display: flex;
            gap: 15px;
            justify-content: center;
            margin-top: 30px;
        }

        .btn {
            padding: 12px 30px;
            border: none;
            border-radius: 25px;
            font-size: 1em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #ff6b6b, #ee5a24);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(255, 107, 107, 0.3);
        }

        .btn-secondary {
            background: linear-gradient(135deg, #667eea, #764ba2);
            color: white;
        }

        .btn-secondary:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 20px rgba(102, 126, 234, 0.3);
        }

        .timeline {
            position: relative;
            padding: 20px 0;
        }

        .timeline::before {
            content: '';
            position: absolute;
            left: 50%;
            top: 0;
            bottom: 0;
            width: 3px;
            background: linear-gradient(to bottom, #ff6b6b, #ee5a24);
            transform: translateX(-50%);
        }

        .timeline-item {
            position: relative;
            margin-bottom: 30px;
            display: flex;
            align-items: center;
        }

        .timeline-item:nth-child(odd) {
            flex-direction: row-reverse;
        }

        .timeline-content {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            width: 45%;
            position: relative;
        }

        .timeline-date {
            background: #ff6b6b;
            color: white;
            padding: 5px 15px;
            border-radius: 15px;
            font-size: 0.9em;
            font-weight: bold;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            z-index: 2;
        }

        @media (max-width: 768px) {
            .timeline::before {
                left: 20px;
            }
            
            .timeline-item {
                flex-direction: column;
                align-items: flex-start;
            }
            
            .timeline-content {
                width: calc(100% - 40px);
                margin-left: 40px;
            }
            
            .timeline-date {
                left: 20px;
                transform: none;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🕯️ Candle Euphoria Business Dashboard</h1>
            <p>Path to $10K Monthly Profit Transformation</p>
        </div>

        <!-- Key Metrics -->
        <div class="metrics-grid">
            <div class="metric-card">
                <h3>Monthly Revenue</h3>
                <div class="metric-value">$3,000</div>
                <div class="metric-change">Target: $15,000 <span class="positive">↗</span></div>
            </div>
            <div class="metric-card">
                <h3>Monthly Profit</h3>
                <div class="metric-value">$200</div>
                <div class="metric-change">Target: $10,000 <span class="positive">↗</span></div>
            </div>
            <div class="metric-card">
                <h3>Conversion Rate</h3>
                <div class="metric-value">1%</div>
                <div class="metric-change">Target: 3.5% <span class="positive">↗</span></div>
            </div>
            <div class="metric-card">
                <h3>Average Order Value</h3>
                <div class="metric-value">$87</div>
                <div class="metric-change">Target: $120 <span class="positive">↗</span></div>
            </div>
        </div>

        <!-- Progress to Goal -->
        <div class="progress-section">
            <h3 style="margin-bottom: 20px; color: #333;">Progress to $10K Monthly Profit Goal</h3>
            <div class="progress-bar">
                <div class="progress-fill" style="width: 2%"></div>
            </div>
            <p style="text-align: center; color: #666; margin-top: 10px;">
                Current: $200 | Target: $10,000 | Progress: 2%
            </p>
        </div>

        <!-- ROI Calculator -->
        <div class="roi-calculator">
            <h3>Investment ROI Calculator</h3>
            <div class="calculator-grid">
                <div class="calculator-item">
                    <label>Monthly Revenue Increase</label>
                    <input type="number" id="revenueIncrease" value="2000" placeholder="$2,000">
                </div>
                <div class="calculator-item">
                    <label>Service Investment</label>
                    <input type="number" id="serviceInvestment" value="1500" placeholder="$1,500">
                </div>
                <div class="calculator-item">
                    <label>Implementation Time (months)</label>
                    <input type="number" id="implementationTime" value="3" placeholder="3">
                </div>
            </div>
            <div class="roi-result" id="roiResult">
                ROI: 400% | Break-even: 2.3 months
            </div>
        </div>

        <!-- Implementation Timeline -->
        <div class="chart-container">
            <h3 class="chart-title">Implementation Timeline</h3>
            <div class="timeline">
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h4>Phase 1: Foundation (Months 1-3)</h4>
                        <p>Financial restructuring, pricing optimization, website CRO</p>
                        <strong>Investment: $4,200 | Expected ROI: 300%</strong>
                    </div>
                    <div class="timeline-date">Jan-Mar 2025</div>
                </div>
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h4>Phase 2: Growth (Months 4-6)</h4>
                        <p>Email marketing, social media optimization, local expansion</p>
                        <strong>Investment: $3,800 | Expected ROI: 250%</strong>
                    </div>
                    <div class="timeline-date">Apr-Jun 2025</div>
                </div>
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h4>Phase 3: Scale (Months 7-12)</h4>
                        <p>Automation, revenue diversification, brand development</p>
                        <strong>Investment: $5,500 | Expected ROI: 200%</strong>
                    </div>
                    <div class="timeline-date">Jul-Dec 2025</div>
                </div>
                <div class="timeline-item">
                    <div class="timeline-content">
                        <h4>Full-Time Transition</h4>
                        <p>Complete transition to entrepreneurship</p>
                        <strong>Target: $12,000+ monthly profit</strong>
                    </div>
                    <div class="timeline-date">Aug 2026</div>
                </div>
            </div>
        </div>

        <!-- Phase 1: Immediate Priority Services -->
        <div class="phase-section">
            <div class="phase-header">
                <div class="phase-title">Phase 1: Immediate Priority Services (0-3 months)</div>
                <div class="phase-status status-active">Active</div>
            </div>
            <div class="services-grid">
                <div class="service-item">
                    <div class="service-title">Financial Restructuring</div>
                    <div class="service-price">$2,600 total</div>
                    <div class="service-description">Cost analysis, pricing strategy, cash flow management, break-even analysis</div>
                </div>
                <div class="service-item">
                    <div class="service-title">Website Optimization</div>
                    <div class="service-price">$3,200 total</div>
                    <div class="service-description">CRO, UX improvements, SEO, mobile optimization</div>
                </div>
                <div class="service-item">
                    <div class="service-title">Operations Management</div>
                    <div class="service-price">$1,800 total</div>
                    <div class="service-description">Inventory tracking, workflow optimization, demand forecasting</div>
                </div>
            </div>
        </div>

        <!-- Phase 2: Revenue Growth Services -->
        <div class="phase-section">
            <div class="phase-header">
                <div class="phase-title">Phase 2: Revenue Growth Services (1-6 months)</div>
                <div class="phase-status status-pending">Pending</div>
            </div>
            <div class="services-grid">
                <div class="service-item">
                    <div class="service-title">Customer Retention</div>
                    <div class="service-price">$2,100 total</div>
                    <div class="service-description">Email automation, segmentation, loyalty programs</div>
                </div>
                <div class="service-item">
                    <div class="service-title">Social Media Marketing</div>
                    <div class="service-price">$1,900 total</div>
                    <div class="service-description">Content strategy, scheduling, engagement tactics</div>
                </div>
                <div class="service-item">
                    <div class="service-title">Local Market Expansion</div>
                    <div class="service-price">$2,050 total</div>
                    <div class="service-description">Partnerships, community outreach, corporate packages</div>
                </div>
            </div>
        </div>

        <!-- Phase 3: Scaling Services -->
        <div class="phase-section">
            <div class="phase-header">
                <div class="phase-title">Phase 3: Scaling & Automation (3-12 months)</div>
                <div class="phase-status status-pending">Pending</div>
            </div>
            <div class="services-grid">
                <div class="service-item">
                    <div class="service-title">Business Automation</div>
                    <div class="service-price">$1,850 total</div>
                    <div class="service-description">Customer service automation, scheduling tools, fulfillment</div>
                </div>
                <div class="service-item">
                    <div class="service-title">Revenue Diversification</div>
                    <div class="service-price">$2,700 total</div>
                    <div class="service-description">New products, B2B sales, subscription services</div>
                </div>
                <div class="service-item">
                    <div class="service-title">Brand Development</div>
                    <div class="service-price">$2,900 total</div>
                    <div class="service-description">Photography, content marketing, paid advertising</div>
                </div>
            </div>
        </div>

        <!-- Revenue Projection Chart -->
        <div class="chart-container">
            <h3 class="chart-title">Revenue & Profit Projection</h3>
            <canvas id="projectionChart" width="400" height="200"></canvas>
        </div>

        <!-- Action Buttons -->
        <div class="action-buttons">
            <button class="btn btn-primary" onclick="scheduleConsultation()">Schedule Consultation</button>
            <button class="btn btn-secondary" onclick="downloadProposal()">Download Proposal</button>
        </div>
    </div>

    <script>
        // ROI Calculator
        function calculateROI() {
            const revenueIncrease = parseFloat(document.getElementById('revenueIncrease').value) || 0;
            const serviceInvestment = parseFloat(document.getElementById('serviceInvestment').value) || 0;
            const implementationTime = parseFloat(document.getElementById('implementationTime').value) || 1;
            
            const monthlyProfit = revenueIncrease * 0.4; // Assuming 40% profit margin
            const totalReturn = monthlyProfit * 12;
            const roi = ((totalReturn - serviceInvestment) / serviceInvestment) * 100;
            const breakEven = serviceInvestment / monthlyProfit;
            
            document.getElementById('roiResult').innerHTML = 
                `ROI: ${roi.toFixed(0)}% | Break-even: ${breakEven.toFixed(1)} months`;
        }

        // Event listeners for ROI calculator
        document.getElementById('revenueIncrease').addEventListener('input', calculateROI);
        document.getElementById('serviceInvestment').addEventListener('input', calculateROI);
        document.getElementById('implementationTime').addEventListener('input', calculateROI);

        // Revenue Projection Chart
        const ctx = document.getElementById('projectionChart').getContext('2d');
        const chart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'],
                datasets: [{
                    label: 'Monthly Revenue',
                    data: [3000, 4200, 5800, 7500, 9200, 11000, 12800, 14200, 15000, 15500, 16000, 16500],
                    borderColor: '#ff6b6b',
                    backgroundColor: 'rgba(255, 107, 107, 0.1)',
                    tension: 0.4,
                    fill: true
                }, {
                    label: 'Monthly Profit',
                    data: [200, 800, 1800, 3200, 4800, 6800, 8200, 9500, 10000, 10500, 11000, 11500],
                    borderColor: '#27ae60',
                    backgroundColor: 'rgba(39, 174, 96, 0.1)',
                    tension: 0.4,
                    fill: true
                }]
            },
            options: {
                responsive: true,
                plugins: {
                    legend: {
                        position: 'top',
                    },
                    title: {
                        display: false
                    }
                },
                scales: {
                    y: {
                        beginAtZero: true,
                        ticks: {
                            callback: function(value) {
                                return '$' + value.toLocaleString();
                            }
                        }
                    }
                }
            }
        });

        // Action button functions
        function scheduleConsultation() {
            alert('Consultation scheduling feature would redirect to booking system');
        }

        function downloadProposal() {
            alert('Proposal download feature would generate PDF proposal');
        }

        // Animate progress bar on load
        window.addEventListener('load', function() {
            setTimeout(() => {
                const progressBar = document.querySelector('.progress-fill');
                progressBar.style.width = '2%';
            }, 500);
        });

        // Initialize ROI calculator
        calculateROI();
    </script>
</body>
</html>
