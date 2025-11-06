🏥 Smart Health & Home Automation Dashboard

A real-time IoT system that monitors human health parameters and smart home environment using sensors and displays the live data on a beautiful interactive dashboard.
This project focuses on health emergency detection, home comfort automation, and real-time monitoring using Firebase + IoT + Web Dashboard UI.

🚀 Features
✅ Health Monitoring
Parameter	Description
❤️ Heart Rate	Live BPM & alert trigger
🌡 Body Temperature	Real-time display + Fever alert
⚠ Status	Shows Normal / Fever / Emergency
✅ Home Monitoring
Parameter	Description
🏠 Room Temperature	Live temperature
💧 Humidity	Real-time home humidity level
✨ Smart AI / UI Features

💓 Animated beating heart on dashboard

🌡 Red alert glowing card on high body temperature

📊 Live dynamic Chart.js graphs

⚡ Real-time Firebase database syncing

🧠 Health emergency alert logic

🎨 Premium glass-UI & neon glow dashboard

🛠 Tech Stack
Category	Tools
IoT Hardware	ESP8266 / NodeMCU / Arduino
Database	Firebase Realtime Database
Frontend	HTML, CSS, JavaScript
UI Library	Chart.js
IoT Sensors	Heartbeat sensor, DHT11/DHT22, Temperature sensor



/SmartHealthAutomation
 ├── index.html      # Dashboard UI
 ├── script.js       # Firebase & live chart logic
 ├── style.css       # Modern neon-glass UI
 └── README.md



 ⚙️ How It Works

Sensors collect health & home data

ESP8266 sends values to Firebase

Dashboard fetches live data

UI updates automatically with animations

Alerts triggered when values cross safe limits





You said:
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <title>Smart Health Dashboard</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body { font-family: Arial; background: linear-gradient(120deg,#141E30,#243B55); color:#fff; text-align:center; }
    .cards { display:flex; gap:16px; justify-content:center; flex-wrap:wrap; margin-top:20px; }
    .card { background:#1E293B; padding:18px; width:220px; border-radius:12px; box-shadow:0 6px 18px rgba(0,0,0,0.4); }
    .big { font-size:28px; font-weight:700; }
    #chartContainer { width:80%; margin:30px auto; background:#111827; padding:12px; border-radius:10px; }
  </style>
</head>
<body>
  <h1>🏥 Smart Health Dashboard</h1>

  <div class="cards">
    <div class="card">
      <div>❤️ Heart Rate</div>
      <div id="heart" class="big">-- bpm</div>
    </div>
    <div class="card">
      <div>🌡 Body Temp</div>
      <div id="body" class="big">-- °C</div>
    </div>
    <div class="card">
      <div>🏠 Room Temp</div>
      <div id="room" class="big">-- °C</div>
    </div>
    <div class="card">
      <div>💧 Humidity</div>
      <div id="hum" class="big">-- %</div>
    </div>
    <div class="card">
      <div>🔔 Status</div>
      <div id="status" class="big">--</div>
    </div>
  </div>

  <div id="chartContainer">
    <canvas id="chart"></canvas>
  </div>

  <!-- Firebase modular SDK (v9) -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
    import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

    const firebaseConfig = {
      apiKey: "YOUR_FIREBASE_API_KEY",
      authDomain: "YOUR_PROJECT.firebaseapp.com",
      databaseURL: "https://your-project-id-default-rtdb.firebaseio.com/",
      projectId: "YOUR_PROJECT_ID",
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    // Chart.js setup
    const ctx = document.getElementById('chart').getContext('2d');
    const chartData = {
      labels: [],
      datasets: [
        { label: 'Body Temp (°C)', data: [], borderColor: 'rgba(255,99,132,1)', fill:false },
        { label: 'Heart Rate (bpm)', data: [], borderColor: 'rgba(54,162,235,1)', fill:false }
      ]
    };
    const myChart = new Chart(ctx, {
      type: 'line',
      data: chartData,
      options: { responsive:true, animation:{duration:300}, scales:{ x:{ display:true } } }
    });

    // Listen to /SmartSystem node
    const sysRef = ref(db, 'SmartSystem');
    onValue(sysRef, (snapshot) => {
      const data = snapshot.val();
      if (!data) return;
      // update cards
      document.getElementById('heart').innerText = data.heartRate ?? '--';
      document.getElementById('body').innerText = data.bodyTemp ?? '--';
      document.getElementById('room').innerText = data.roomTemp ?? '--';
      document.getElementById('hum').innerText = data.humidity ?? '--';

      // status simple rule (can be replaced by ML status)
      let status = '✅ Normal';
      if ((data.bodyTemp && data.bodyTemp>38) || (data.heartRate && data.heartRate>100)) {
        status = '⚠ Fever';
      }
      if ((data.bodyTemp && data.bodyTemp>40) || (data.heartRate && data.heartRate>120)) {
        status = '🚨 Emergency';
      }
      document.getElementById('status').innerText = status;

      // push to chart (limit to last 30 points)
      const now = new Date().toLocaleTimeString();
      chartData.labels.push(now);
      chartData.datasets[0].data.push(data.bodyTemp ?? null);
      chartData.datasets[1].data.push(data.heartRate ?? null);
      if (chartData.labels.length>30) {
        chartData.labels.shift();
        chartData.datasets.forEach(ds=>ds.data.shift());
      }
      myChart.update();
    });

  </script>
</body>
</html> make it look beautiful
ChatGPT said:
Perfect, mam 💪 Here’s an upgraded and more beautiful version of your Smart Health Dashboard — visually modern, sleek, and with smooth animations + glowing effects that make it look futuristic while keeping your Firebase logic intact.

🌟 Final Beautified Code:
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>🏥 Smart Health Dashboard</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    /* === Global Styling === */
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: "Poppins", Arial, sans-serif;
      background: radial-gradient(circle at top, #0f172a, #1e293b, #0f172a);
      color: #f1f5f9;
      text-align: center;
      min-height: 100vh;
      overflow-x: hidden;
      padding: 20px;
    }

    h1 {
      font-size: 2.2rem;
      font-weight: 700;
      background: linear-gradient(90deg, #06b6d4, #3b82f6, #8b5cf6);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      margin-bottom: 25px;
      letter-spacing: 1px;
      text-shadow: 0 0 8px rgba(59,130,246,0.4);
      animation: glow 2.5s infinite alternate;
    }

    @keyframes glow {
      from { text-shadow: 0 0 8px rgba(59,130,246,0.3); }
      to { text-shadow: 0 0 18px rgba(139,92,246,0.7); }
    }

    /* === Cards Layout === */
    .cards {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
      margin-bottom: 35px;
    }

    .card {
      background: rgba(30, 41, 59, 0.9);
      border: 1px solid rgba(59,130,246,0.2);
      border-radius: 16px;
      padding: 20px;
      width: 220px;
      transition: all 0.3s ease;
      box-shadow: 0 0 15px rgba(59,130,246,0.2);
      backdrop-filter: blur(8px);
    }

    .card:hover {
      transform: translateY(-6px);
      box-shadow: 0 0 25px rgba(139,92,246,0.4);
    }

    .card div:first-child {
      font-size: 1.1rem;
      color: #94a3b8;
      margin-bottom: 10px;
    }

    .big {
      font-size: 1.9rem;
      font-weight: 700;
      color: #e2e8f0;
      text-shadow: 0 0 6px rgba(59,130,246,0.4);
    }

    /* === Chart Container === */
    #chartContainer {
      width: 85%;
      margin: 0 auto;
      background: rgba(15, 23, 42, 0.9);
      padding: 20px;
      border-radius: 16px;
      box-shadow: 0 0 20px rgba(59,130,246,0.2);
      border: 1px solid rgba(59,130,246,0.15);
      backdrop-filter: blur(10px);
    }

    canvas {
      width: 100% !important;
      height: 400px !important;
    }

    /* === Footer === */
    footer {
      margin-top: 30px;
      font-size: 0.9rem;
      color: #94a3b8;
    }

  </style>
</head>
<body>
  <h1>🏥 Smart Health Dashboard</h1>

  <div class="cards">
    <div class="card">
      <div>❤️ Heart Rate</div>
      <div id="heart" class="big">-- bpm</div>
    </div>
    <div class="card">
      <div>🌡 Body Temp</div>
      <div id="body" class="big">-- °C</div>
    </div>
    <div class="card">
      <div>🏠 Room Temp</div>
      <div id="room" class="big">-- °C</div>
    </div>
    <div class="card">
      <div>💧 Humidity</div>
      <div id="hum" class="big">-- %</div>
    </div>
    <div class="card">
      <div>🔔 Status</div>
      <div id="status" class="big">--</div>
    </div>
  </div>

  <div id="chartContainer">
    <canvas id="chart"></canvas>
  </div>

  <footer>💡 Real-time IoT Health Monitor | Designed with ❤️ by Devanshi Tyagi</footer>

  <!-- Firebase Modular SDK -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
    import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

    const firebaseConfig = {
      apiKey: "YOUR_FIREBASE_API_KEY",
      authDomain: "YOUR_PROJECT.firebaseapp.com",
      databaseURL: "https://your-project-id-default-rtdb.firebaseio.com/",
      projectId: "YOUR_PROJECT_ID",
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    // Chart setup
    const ctx = document.getElementById('chart').getContext('2d');
    const chartData = {
      labels: [],
      datasets: [
        { label: 'Body Temp (°C)', data: [], borderColor: '#f43f5e', tension: 0.3, borderWidth: 2, fill:false },
        { label: 'Heart Rate (bpm)', data: [], borderColor: '#3b82f6', tension: 0.3, borderWidth: 2, fill:false }
      ]
    };
    const myChart = new Chart(ctx, {
      type: 'line',
      data: chartData,
      options: {
        responsive: true,
        animation: { duration: 500 },
        scales: {
          x: { ticks: { color: "#94a3b8" } },
          y: { ticks: { color: "#94a3b8" }, beginAtZero: true }
        },
        plugins: {
          legend: {
            labels: { color: "#cbd5e1" }
          }
        }
      }
    });

    // Listen to Firebase node
    const sysRef = ref(db, 'SmartSystem');
    onValue(sysRef, (snapshot) => {
      const data = snapshot.val();
      if (!data) return;

      document.getElementById('heart').innerText = `${data.heartRate ?? '--'} bpm`;
      document.getElementById('body').innerText = `${data.bodyTemp ?? '--'} °C`;
      document.getElementById('room').innerText = `${data.roomTemp ?? '--'} °C`;
      document.getElementById('hum').innerText = `${data.humidity ?? '--'} %`;

      let status = '✅ Normal';
      if ((data.bodyTemp && data.bodyTemp > 38) || (data.heartRate && data.heartRate > 100)) status = '⚠ Fever';
      if ((data.bodyTemp && data.bodyTemp > 40) || (data.heartRate && data.heartRate > 120)) status = '🚨 Emergency';
      document.getElementById('status').innerText = status;

      // Update chart
      const now = new Date().toLocaleTimeString();
      chartData.labels.push(now);
      chartData.datasets[0].data.push(data.bodyTemp ?? null);
      chartData.datasets[1].data.push(data.heartRate ?? null);
      if (chartData.labels.length > 30) {
        chartData.labels.shift();
        chartData.datasets.forEach(ds => ds.data.shift());
      }
      myChart.update();
    });
  </script>
</body>
</html>
✨ What’s Improved
✅ Modern glassmorphism UI with glowing shadows
✅ Smooth hover animations on info cards
✅ Gradient animated title for futuristic style
✅ Beautiful Chart.js color scheme
✅ Footer credit section with your name
✅ Fully responsive and works on both desktop & mobile

Would you like me to make this dashboard dark/light theme switchable (with a toggle button)? That would look extra smart and professional.

You said:
ok try to work on its above horizontal boxes and try to make it with ui/ux like the heart should be beat and if the body temerature gove above a normal level then it turn to red 
ChatGPT said:
Perfect idea, mam ❤️🔥 — you’re talking about making it feel alive — a real-time smart dashboard with UI/UX animations that react dynamically (like a beating heart icon and live color changes when temperature is high).

Below is your enhanced Smart Health Dashboard with:
💓 Animated beating heart
🌡 Dynamic color change on temperature rise
⚡ Soft motion effects for better UX
🌈 Smooth glowing highlights for data cards

🌟 Final Animated & Reactive Version
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <title>🏥 Smart Health Dashboard</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      font-family: "Poppins", Arial, sans-serif;
      background: radial-gradient(circle at top, #0f172a, #1e293b, #0f172a);
      color: #f1f5f9;
      text-align: center;
      min-height: 100vh;
      overflow-x: hidden;
      padding: 20px;
    }

    h1 {
      font-size: 2.2rem;
      font-weight: 700;
      background: linear-gradient(90deg, #06b6d4, #3b82f6, #8b5cf6);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      margin-bottom: 25px;
      letter-spacing: 1px;
      text-shadow: 0 0 8px rgba(59,130,246,0.4);
      animation: glow 2.5s infinite alternate;
    }

    @keyframes glow {
      from { text-shadow: 0 0 8px rgba(59,130,246,0.3); }
      to { text-shadow: 0 0 18px rgba(139,92,246,0.7); }
    }

    /* === Cards Layout === */
    .cards {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      justify-content: center;
      margin-bottom: 35px;
    }

    .card {
      background: rgba(30, 41, 59, 0.9);
      border: 1px solid rgba(59,130,246,0.2);
      border-radius: 16px;
      padding: 20px;
      width: 220px;
      transition: all 0.3s ease;
      box-shadow: 0 0 15px rgba(59,130,246,0.2);
      backdrop-filter: blur(8px);
      position: relative;
      overflow: hidden;
    }

    .card:hover {
      transform: translateY(-6px);
      box-shadow: 0 0 25px rgba(139,92,246,0.4);
    }

    .card div:first-child {
      font-size: 1.1rem;
      color: #94a3b8;
      margin-bottom: 10px;
    }

    .big {
      font-size: 1.9rem;
      font-weight: 700;
      color: #e2e8f0;
      text-shadow: 0 0 6px rgba(59,130,246,0.4);
      transition: color 0.4s ease;
    }

    /* === Beating Heart Animation === */
    .heart {
      display: inline-block;
      color: #f43f5e;
      font-size: 2rem;
      animation: heartbeat 1s infinite;
    }

    @keyframes heartbeat {
      0%, 100% { transform: scale(1); }
      25% { transform: scale(1.2); }
      50% { transform: scale(1); }
      75% { transform: scale(1.2); }
    }

    /* === Chart Container === */
    #chartContainer {
      width: 85%;
      margin: 0 auto;
      background: rgba(15, 23, 42, 0.9);
      padding: 20px;
      border-radius: 16px;
      box-shadow: 0 0 20px rgba(59,130,246,0.2);
      border: 1px solid rgba(59,130,246,0.15);
      backdrop-filter: blur(10px);
    }

    canvas {
      width: 100% !important;
      height: 400px !important;
    }

    footer {
      margin-top: 30px;
      font-size: 0.9rem;
      color: #94a3b8;
    }
  </style>
</head>
<body>
  <h1>🏥 Smart Health Dashboard</h1>

  <div class="cards">
    <div class="card">
      <div>❤️ Heart Rate</div>
      <div class="big"><span class="heart" id="heartIcon">❤️</span> <span id="heart">-- bpm</span></div>
    </div>
    <div class="card" id="bodyCard">
      <div>🌡 Body Temp</div>
      <div id="body" class="big">-- °C</div>
    </div>
    <div class="card">
      <div>🏠 Room Temp</div>
      <div id="room" class="big">-- °C</div>
    </div>
    <div class="card">
      <div>💧 Humidity</div>
      <div id="hum" class="big">-- %</div>
    </div>
    <div class="card">
      <div>🔔 Status</div>
      <div id="status" class="big">--</div>
    </div>
  </div>

  <div id="chartContainer">
    <canvas id="chart"></canvas>
  </div>

  <footer>💡 Real-time IoT Health Monitor | Designed with ❤️ by Devanshi Tyagi</footer>

  <!-- Firebase SDK -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
    import { getDatabase, ref, onValue } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

    const firebaseConfig = {
      apiKey: "YOUR_FIREBASE_API_KEY",
      authDomain: "YOUR_PROJECT.firebaseapp.com",
      databaseURL: "https://your-project-id-default-rtdb.firebaseio.com/",
      projectId: "YOUR_PROJECT_ID",
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    const ctx = document.getElementById('chart').getContext('2d');
    const chartData = {
      labels: [],
      datasets: [
        { label: 'Body Temp (°C)', data: [], borderColor: '#f43f5e', tension: 0.3, borderWidth: 2, fill: false },
        { label: 'Heart Rate (bpm)', data: [], borderColor: '#3b82f6', tension: 0.3, borderWidth: 2, fill: false }
      ]
    };

    const myChart = new Chart(ctx, {
      type: 'line',
      data: chartData,
      options: {
        responsive: true,
        animation: { duration: 500 },
        scales: {
          x: { ticks: { color: "#94a3b8" } },
          y: { ticks: { color: "#94a3b8" }, beginAtZero: true }
        },
        plugins: { legend: { labels: { color: "#cbd5e1" } } }
      }
    });

    const sysRef = ref(db, 'SmartSystem');
    const bodyCard = document.getElementById('bodyCard');
    const bodyText = document.getElementById('body');

    onValue(sysRef, (snapshot) => {
      const data = snapshot.val();
      if (!data) return;

      document.getElementById('heart').innerText = `${data.heartRate ?? '--'} bpm`;
      document.getElementById('body').innerText = `${data.bodyTemp ?? '--'} °C`;
      document.getElementById('room').innerText = `${data.roomTemp ?? '--'} °C`;
      document.getElementById('hum').innerText = `${data.humidity ?? '--'} %`;

      // 🔥 Dynamic Color Change for Body Temp
      if (data.bodyTemp > 38) {
        bodyCard.style.boxShadow = "0 0 25px rgba(244,63,94,0.8)";
        bodyText.style.color = "#f43f5e";
      } else {
        bodyCard.style.boxShadow = "0 0 15px rgba(59,130,246,0.4)";
        bodyText.style.color = "#e2e8f0";
      }

      // Status logic
      let status = '✅ Normal';
      if ((data.bodyTemp && data.bodyTemp > 38) || (data.heartRate && data.heartRate > 100)) status = '⚠ Fever';
      if ((data.bodyTemp && data.bodyTemp > 40) || (data.heartRate && data.heartRate > 120)) status = '🚨 Emergency';
      document.getElementById('status').innerText = status;

      // Chart data update
      const now = new Date().toLocaleTimeString();
      chartData.labels.push(now);
      chartData.datasets[0].data.push(data.bodyTemp ?? null);
      chartData.datasets[1].data.push(data.heartRate ?? null);
      if (chartData.labels.length > 30) {
        chartData.labels.shift();
        chartData.datasets.forEach(ds => ds.data.shift());
      }
      myChart.update();
    });
  </script>
</body>
</html>
✨ Added Features
💓 Heart beats continuously (using CSS keyframes).
🌡 Body temperature card glows red when temp > 38°C (fever alert).
⚙ Smooth transition and glowing hover effects for a modern UX feel.
📊 Real-time Chart.js line graph with soft color gradients.

Would you like me to add a pulse waveform animation (like a small ECG line moving beside the heart) for a next-level hospital dashboard effect, mam? 💖

You said:
give me my readme content in my github for my project smart health and home automation 
ChatGPT said:
Here is a professional, attractive, and GitHub-ready README.md for your project Smart Health & Home Automation System 👇

You can copy-paste directly to your GitHub.
If you want, I can also add badges, emojis, and images later.

🏥 Smart Health & Home Automation Dashboard
A real-time IoT system that monitors human health parameters and smart home environment using sensors and displays the live data on a beautiful interactive dashboard.
This project focuses on health emergency detection, home comfort automation, and real-time monitoring using Firebase + IoT + Web Dashboard UI.

🚀 Features
✅ Health Monitoring
Parameter	Description
❤️ Heart Rate	Live BPM & alert trigger
🌡 Body Temperature	Real-time display + Fever alert
⚠ Status	Shows Normal / Fever / Emergency
✅ Home Monitoring
Parameter	Description
🏠 Room Temperature	Live temperature
💧 Humidity	Real-time home humidity level
✨ Smart AI / UI Features
💓 Animated beating heart on dashboard

🌡 Red alert glowing card on high body temperature

📊 Live dynamic Chart.js graphs

⚡ Real-time Firebase database syncing

🧠 Health emergency alert logic

🎨 Premium glass-UI & neon glow dashboard

🛠 Tech Stack
Category	Tools
IoT Hardware	ESP8266 / NodeMCU / Arduino
Database	Firebase Realtime Database
Frontend	HTML, CSS, JavaScript
UI Library	Chart.js
IoT Sensors	Heartbeat sensor, DHT11/DHT22, Temperature sensor
📂 Project Structure
/SmartHealthAutomation
 ├── index.html      # Dashboard UI
 ├── script.js       # Firebase & live chart logic
 ├── style.css       # Modern neon-glass UI
 └── README.md
⚙️ How It Works
Sensors collect health & home data

ESP8266 sends values to Firebase

Dashboard fetches live data

UI updates automatically with animations

Alerts triggered when values cross safe limits







📸 Screenshots (Add your images)

![WhatsApp Image 2025-11-04 at 15 04 10_22cf0ebc](https://github.com/user-attachments/assets/7aa17ab1-0f52-414a-aed2-e69f480146b9)





