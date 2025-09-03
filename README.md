<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Recovery Tracker</title>
<style>
    * {margin:0; padding:0; box-sizing:border-box; font-family: 'Segoe UI', sans-serif;}
    body {background: linear-gradient(135deg, #6a11cb, #2575fc); color:#fff; display:flex; flex-direction:column; min-height:100vh; transition: background 1s ease;}
    .container {max-width:600px; margin:40px auto; padding:30px; background: rgba(0,0,0,0.5); border-radius:20px; text-align:center; box-shadow: 0 8px 25px rgba(0,0,0,0.3);}
    h1 {margin-bottom:20px; font-size:2rem;}
    input[type="date"] {padding:12px; border-radius:8px; border:none; margin-bottom:20px; font-size:16px; width:80%;}
    button {padding:12px 25px; border:none; border-radius:8px; background:#ff6b6b; color:#fff; font-size:16px; cursor:pointer; margin:10px 0; transition: transform 0.2s, background 0.3s;}
    button:hover {transform: scale(1.05); background:#ff4757;}
    .quote, .reflection {margin:20px 0; padding:20px; background: rgba(255,255,255,0.1); border-radius:15px; box-shadow: 0 4px 15px rgba(0,0,0,0.2); cursor:pointer; transition: transform 0.3s, opacity 0.5s;}
    .quote:hover {transform: scale(1.02);}
    .quote.fade {opacity:0;}
    .quote {font-style:italic; font-size:18px;}
    .reflection a {color:#ffd700; text-decoration:none; font-weight:bold;}
    .hotline {margin-top:auto; padding:20px; background: rgba(0,0,0,0.7); font-size:14px; line-height:1.5; text-align:center;}
    .share-btn {background:#1dd1a1; margin-top:10px;}
    .share-btn:hover {background:#10ac84;}
</style>
</head>
<body>
<div class="container">
    <h1>Recovery Tracker</h1>

    <!-- Sober Date Input -->
    <div id="sober-section">
        <label for="soberDate">Enter your sober date:</label><br>
        <input type="date" id="soberDate">
        <br>
        <button onclick="saveDate()">Save Date</button>
        <p id="daysSober" style="font-weight:bold; font-size:18px; margin-top:10px;"></p>
    </div>

    <!-- Daily Quote -->
    <div class="quote" id="dailyQuote" onclick="newQuote()"></div>

    <!-- Daily Reflection -->
    <div class="reflection">
        <a href="https://www.aa.org/pages/en_US/daily-reflections" target="_blank" id="reflectionLink">Today's Daily Reflection</a>
        <br>
        <button class="share-btn" onclick="shareReflection()">Share with a Friend</button>
    </div>
</div>

<!-- Hotlines and Design Credit -->
<div class="hotline">
    AA Hotline: 1-800-662-4357 <br>
    Suicide & Crisis Hotline: 988 <br>
    Design by Corey <br>
    @runforrecovery
</div>

<script>
    const quotes = [
        "Progress, not perfection.",
        "One day at a time.",
        "Strength grows when you think you can't go on but keep going.",
        "Recovery is a journey, not a destination.",
        "Let go of what you can't control.",
        "Every day sober is a victory.",
        "You are stronger than your struggles.",
        "Small steps every day lead to big changes.",
        "Your past does not define your future.",
        "Keep moving forward no matter what."
    ];

    const quoteEl = document.getElementById('dailyQuote');

    // Display deterministic daily quote
    function getDailyQuote() {
        const today = new Date();
        const dayOfYear = Math.floor((today - new Date(today.getFullYear(),0,0)) / 1000/60/60/24);
        return quotes[dayOfYear % quotes.length];
    }

    function displayQuote(text) {
        quoteEl.classList.add('fade');
        setTimeout(() => {
            quoteEl.innerText = text;
            quoteEl.classList.remove('fade');
        }, 300);
    }

    function newQuote() {
        let next;
        do {
            next = quotes[Math.floor(Math.random() * quotes.length)];
        } while(next === quoteEl.innerText);
        displayQuote(next);
    }

    displayQuote(getDailyQuote());

    // Save Sober Date
    function saveDate() {
        const date = document.getElementById('soberDate').value;
        if(!date) { alert("Please select a date!"); return; }
        localStorage.setItem('soberDate', date);
        calculateDaysSober();
    }

    // Calculate Days Sober
    function calculateDaysSober() {
        const date = localStorage.getItem('soberDate');
        if(!date) return;
        const start = new Date(date);
        const now = new Date();
        const diffTime = now - start;
        const diffDays = Math.floor(diffTime / (1000*60*60*24));
        document.getElementById('daysSober').innerText = `Days sober: ${diffDays}`;
        document.getElementById('soberDate').value = date;
    }

    // Share Reflection
    function shareReflection() {
        const url = document.getElementById('reflectionLink').href;
        navigator.clipboard.writeText(`Check out today's AA Daily Reflection: ${url}`)
            .then(() => alert("Reflection link copied to clipboard!"))
            .catch(() => alert("Unable to copy. Please copy manually."));
    }

    window.onload = calculateDaysSober;
</script>
</body>
</html>
