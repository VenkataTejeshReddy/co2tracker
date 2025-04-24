<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>CO2 Tracker</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: white;
      color: black;
      padding: 20px;
      max-width: 600px;
      margin: auto;
    }
    h1, h2 {
      text-align: center;
      color: #28a745;
    }
    .card, .section {
      background: lightcyan;
      padding: 20px;
      margin-bottom: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    }
    label {
      display: block;
      margin-bottom: 5px;
    }
    select, input[type="number"] {
      width: 100%;
      padding: 10px;
      margin-bottom: 15px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    button {
      background: #28a745;
      color: white;
      padding: 10px 15px;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      margin: 5px;
    }
    .history-item, .leaderboard-entry, .coupon-entry {
      border-bottom: 1px solid #eee;
      padding: 5px 0;
    }
    #history, #challenges, #leaderboard, #coupons {
      display: none;
    }
    .highlight {
      font-weight: bold;
      color: #28a745;
    }
    .coupon-card {
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 15px;
      margin: 10px 0;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .tier-1 {
      border-left: 4px solid #4CAF50;
    }
    .tier-2 {
      border-left: 4px solid #2196F3;
    }
    .tier-3 {
      border-left: 4px solid #FF9800;
    }
    .tier-4 {
      border-left: 4px solid #F44336;
    }
    .coupon-footer {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-top: 10px;
    }
    .points-badge {
      background: #eee;
      padding: 3px 8px;
      border-radius: 12px;
      font-size: 0.8em;
    }
    .redeem-btn {
      background: #4CAF50;
      color: white;
      border: none;
      padding: 5px 15px;
      border-radius: 4px;
      cursor: pointer;
    }
    .no-coupons {
      text-align: center;
      color: #666;
      padding: 20px;
    }
    /* Pie Chart Styles */
    .challenge {
      display: flex;
      align-items: center;
      margin-bottom: 15px;
      padding: 10px;
      background: #f9f9f9;
      border-radius: 8px;
    }
    .pie-chart-container {
      width: 60px;
      height: 60px;
      margin-right: 15px;
      position: relative;
    }
    .pie-chart {
      width: 100%;
      height: 100%;
      transform: rotate(-90deg);
    }
    .progress-circle {
      fill: none;
      stroke: #28a745;
      stroke-width: 32;
      stroke-dashoffset: 25;
      transition: stroke-dasharray 0.3s ease;
    }
    .pie-text {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
      font-size: 12px;
      font-weight: bold;
    }
    .challenge-info {
      flex-grow: 1;
    }
    .challenge-title {
      font-weight: bold;
      margin-bottom: 5px;
    }
    .challenge-progress {
      font-size: 0.9em;
      color: #666;
    }
    .completed {
      color: #28a745;
      font-weight: bold;
    }
    /* CO2 Savings Meter */
    .co2-meter {
      text-align: center;
      margin: 20px 0;
    }
    .co2-pie-container {
      width: 150px;
      height: 150px;
      margin: 0 auto 15px;
      position: relative;
    }
    .co2-pie {
      width: 100%;
      height: 100%;
      transform: rotate(-90deg);
    }
    .co2-circle {
      fill: none;
      stroke: #28a745;
      stroke-width: 32;
      stroke-dashoffset: 25;
    }
    .co2-text {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
    }
    .co2-amount {
      font-size: 24px;
      font-weight: bold;
      color: #28a745;
    }
    .co2-label {
      font-size: 14px;
      color: #666;
    }
  </style>
</head>
<body>
  <h1>CO2 Tracker</h1>
  <div class="card">
    <label for="transportMode">Transport Mode</label>
    <select id="transportMode">
      <option value="car">Car</option>
      <option value="bike">Bike</option>
    </select>

    <label for="fuelType">Fuel Type</label>
    <select id="fuelType">
      <option value="petrol">Petrol</option>
      <option value="diesel">Diesel</option>
      <option value="electric">Electric</option>
    </select>

    <label for="distance">Distance (km)</label>
    <input type="number" id="distance" placeholder="Enter distance">

    <label for="mileage">Mileage (km/l)</label>
    <input type="number" id="mileage" placeholder="Enter mileage">

    <button id="logHabitBtn">Log Habit</button>
  </div>

  <div class="card">
    <p><strong>Eco Points:</strong> <span id="ecoPoints">0</span></p>
    <div class="co2-meter">
      <div class="co2-pie-container">
        <svg class="co2-pie" viewBox="0 0 32 32">
          <circle r="16" cx="16" cy="16" class="co2-circle" id="co2Circle" />
        </svg>
        <div class="co2-text">
          <div class="co2-amount" id="totalCO2">0 kg</div>
          <div class="co2-label">CO₂ Saved</div>
        </div>
      </div>
    </div>
  </div>

  <div class="navigation">
    <button id="showChallenges">Weekly Challenges</button>
    <button id="showLeaderboard">Leader Board</button>
    <button id="showHistory">History</button>
    <button id="showCoupons">Coupons</button>
  </div>

  <div id="challenges" class="section"></div>
  <div id="leaderboard" class="section"></div>
  <div id="history" class="section"></div>
  <div id="coupons" class="section"></div>

  <script>
    // Constants
    const carbonFactors = {
      petrol: 2.31,
      diesel: 2.68,
      electric: 0
    };

    // State variables
    let ecoPoints = 0;
    let totalCO2Saved = 0;
    let history = [];
    let leaderboard = [
      { name: "Vikash", points: 50 },
      { name: "amaan", points: 60 },
      { name: "Nani", points: 200 },
      { name: "Ganesh", points: 150 }
    ];
    let coupons = [];
    let challenges = [
      { id: 1, task: "Log 5 transport entries", goal: 5, progress: 0, type: "transport" },
      { id: 2, task: "Save 10kg CO₂", goal: 10, progress: 0, type: "carbon" }
    ];

    // DOM Elements
    const logHabitBtn = document.getElementById('logHabitBtn');
    const showChallengesBtn = document.getElementById('showChallenges');
    const showLeaderboardBtn = document.getElementById('showLeaderboard');
    const showHistoryBtn = document.getElementById('showHistory');
    const showCouponsBtn = document.getElementById('showCoupons');
    const co2Circle = document.getElementById('co2Circle');

    // Initialize the app
    document.addEventListener('DOMContentLoaded', function() {
      renderAll();
      setupEventListeners();
      updateCO2Meter();
    });

    function setupEventListeners() {
      logHabitBtn.addEventListener('click', logHabit);
      showChallengesBtn.addEventListener('click', () => toggleSection('challenges'));
      showLeaderboardBtn.addEventListener('click', () => toggleSection('leaderboard'));
      showHistoryBtn.addEventListener('click', () => toggleSection('history'));
      showCouponsBtn.addEventListener('click', () => toggleSection('coupons'));
    }

    function toggleSection(sectionId) {
      // Hide all sections first
      document.querySelectorAll('.section').forEach(section => {
        section.style.display = 'none';
      });
      
      // Show the selected section
      document.getElementById(sectionId).style.display = 'block';
    }

    function updateCouponsBasedOnPoints() {
      coupons = []; // Reset coupons array
      
      if (ecoPoints > 30 && ecoPoints <= 100) {
          coupons.push({ 
              name: "free coffee at cafe B", 
              points: 30,
              description: "Get free coffee at cafe B",
              tier: 1
          });
      }
      else if (ecoPoints > 100 && ecoPoints <= 200) {
          coupons.push({ 
              name: "50% Discount on movie tickets", 
              points: 100,
              description: "Get 50% off on movie tickets for your family",
              tier: 2
          });
      }
      else if (ecoPoints > 200 && ecoPoints <= 500) {
          coupons.push({ 
              name: "Free Movie Tickets (${totalTickets} ${totalTickets > 1 ? 'people' : 'person'})", 
              points:500, 
              points: 200,
              description: "Get free movie tickets for ${totalTickets} ${totalTickets > 1 ? 'people' : 'person'} (you${extraTickets > 0 ? ` + ${extraTickets} family members` : ''})",
              tier: 3
          });
      }
      else if (ecoPoints > 500) {
          const extraTickets = Math.floor((ecoPoints - 500) / 500);
          const totalTickets = 1 + extraTickets; // Base ticket + additional tickets
          
          coupons.push({ 
              name:"free goa trip tickets for a family of four", 
              points: 500,
              description: "Get free goa tickets for a family of four",
              tier: 4,
              quantity: totalTickets
          });
      }
    }

    function updateCO2Meter() {
      const percentage = Math.min((totalCO2Saved / 20) * 100, 100); // Assuming 20kg as max for visualization
      const circumference = 2 * Math.PI * 16;
      const offset = circumference - (percentage / 100) * circumference;
      
      co2Circle.style.strokeDasharray = `${circumference} ${circumference}`;
      co2Circle.style.strokeDashoffset = offset;
      
      document.getElementById('totalCO2').textContent = `${totalCO2Saved.toFixed(1)} kg`;
    }

    function renderAll() {
      updateCouponsBasedOnPoints();
      renderChallenges();
      renderLeaderboard();
      renderHistory();
      renderCoupons();
      updateEcoPointsDisplay();
      updateCO2Meter();
    }

    function calculateCO2(distance, mileage, fuel, transport) {
      if (fuel === 'electric') return 0;
      const co2PerLiter = carbonFactors[fuel];
      const factor = transport === 'bike' ? 0.05 : 1;
      const litersUsed = distance / mileage;
      return litersUsed * co2PerLiter * factor;
    }

    function calculateEcoPoints(co2) {
      return co2 === 0 ? 100 : Math.max(0, 100 - co2 * 2);
    }

    function updateChallenges(co2, type) {
      challenges.forEach(c => {
        if (c.type === type && type !== "carbon") {
          c.progress++;
        } else if (c.type === "carbon") {
          c.progress += co2;
        }
      });
    }

    function logHabit() {
      const distance = parseFloat(document.getElementById("distance").value);
      const mileage = parseFloat(document.getElementById("mileage").value);
      const transport = document.getElementById("transportMode").value;
      const fuel = document.getElementById("fuelType").value;

      if (!distance || !mileage || mileage <= 0) {
        alert("Please enter valid distance and mileage values");
        return;
      }

      const co2 = calculateCO2(distance, mileage, fuel, transport);
      const points = Math.round(calculateEcoPoints(co2));

      ecoPoints += points;
      totalCO2Saved += co2;
      updateEcoPointsDisplay();

      history.push({ 
        habitType: "transport", 
        distance, 
        mileage, 
        transport, 
        fuel, 
        co2, 
        points,
        date: new Date().toLocaleString()
      });

      // Update leaderboard
      leaderboard = leaderboard.map(user =>
        user.name === "You" ? { ...user, points: user.points + points } : user
      );

      updateChallenges(co2, "transport");

      renderAll();
      clearForm();
    }

    function updateEcoPointsDisplay() {
      document.getElementById("ecoPoints").innerText = ecoPoints;
    }

    function clearForm() {
      document.getElementById("distance").value = "";
      document.getElementById("mileage").value = "";
    }

    function renderHistory() {
      const container = document.getElementById("history");
      container.innerHTML = history.map(h =>
        `<div class='history-item'>
          ${h.date} | ${h.habitType} | Distance: ${h.distance}km | 
          CO₂: ${h.co2.toFixed(2)}kg | Points: ${h.points}
        </div>`
      ).join("");
    }

    function renderChallenges() {
      const container = document.getElementById("challenges");
      container.innerHTML = challenges.map(c => {
        const percentage = Math.min((c.progress / c.goal) * 100, 100);
        const circumference = 2 * Math.PI * 16;
        const offset = circumference - (percentage / 100) * circumference;
        
        return `
          <div class='challenge'>
            <div class="pie-chart-container">
              <svg class="pie-chart" viewBox="0 0 32 32">
                <circle r="16" cx="16" cy="16" class="progress-circle" 
                  stroke-dasharray="${circumference} ${circumference}"
                  stroke-dashoffset="${offset}" />
              </svg>
              <div class="pie-text">${Math.round(percentage)}%</div>
            </div>
            <div class="challenge-info">
              <div class="challenge-title">${c.task}</div>
              <div class="challenge-progress ${c.progress >= c.goal ? 'completed' : ''}">
                ${Math.min(c.progress.toFixed(1), c.goal)} / ${c.goal} ${c.type === 'carbon' ? 'kg' : ''}
                ${c.progress >= c.goal ? '✓ Completed!' : ''}
              </div>
            </div>
          </div>
        `;
      }).join("");
    }

    function renderLeaderboard() {
      const container = document.getElementById("leaderboard");
      container.innerHTML = leaderboard
        .sort((a, b) => b.points - a.points)
        .map((u, i) => 
          `<div class='leaderboard-entry ${u.name === "You" ? "highlight" : ""}'>
            ${i + 1}. ${u.name}: ${u.points} pts
          </div>`
        )
        .join("");
    }

    function renderCoupons() {
      const container = document.getElementById("coupons");
      if (coupons.length === 0) {
          container.innerHTML = `<div class="no-coupons">Earn more eco points to unlock rewards!</div>`;
          return;
      }
      
      container.innerHTML = coupons.map(coupon => `
          <div class="coupon-card tier-${coupon.tier}">
              <h3>${coupon.name}</h3>
              <p>${coupon.description}</p>
              <div class="coupon-footer">
                  <span class="points-badge">${coupon.points}+ points</span>
                  <button class="redeem-btn" data-tier="${coupon.tier}">Redeem</button>
              </div>
          </div>
      `).join("");
    }
  </script>
</body>
</html>
