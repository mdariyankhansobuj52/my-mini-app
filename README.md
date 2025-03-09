<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Telegram Mini App - Reward Task</title>
  <!-- 
    Monetag Integration:
    1. data-zone="2821162" এ আপনার মূল Monetag Zone ID বসান (এখানে 2821162 দেওয়া আছে)।
    2. Rewarded Interstitial Ad দেখানোর জন্য, আমরা show_9025714() ফাংশন ব্যবহার করছি। 
       যদি আপনার rewarded ad এর Zone ID পরিবর্তিত হয়, তাহলে সেই অনুযায়ী ফাংশন নামও পরিবর্তন করুন।
  -->
  <script src="//niphaumeenses.net/vignette.min.js" data-zone="2821162"></script>
  
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      background-color: #1a1a1a;
      color: #ffd700;
      padding: 20px;
      margin: 0;
    }
    header {
      padding: 20px;
    }
    .user-card {
      background: #333;
      border: 2px solid #ffd700;
      border-radius: 10px;
      padding: 20px;
      margin: 20px auto;
      width: 90%;
      max-width: 400px;
      text-align: left;
    }
    .user-card h2, .user-card p {
      margin: 5px 0;
    }
    .reward-section {
      margin: 20px auto;
      width: 90%;
      max-width: 400px;
    }
    .reward-button {
      padding: 15px 30px;
      font-size: 18px;
      background-color: #ffd700;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      margin-top: 20px;
      transition: transform 0.3s;
    }
    .reward-button:hover {
      transform: scale(1.05);
    }
    .countdown {
      font-size: 24px;
      margin-top: 20px;
      display: none;
    }
    nav {
      position: fixed;
      bottom: 0;
      width: 100%;
      background: #333;
      padding: 10px 0;
      text-align: center;
    }
    nav a {
      color: #ffd700;
      margin: 0 15px;
      text-decoration: none;
      font-size: 18px;
    }
    nav a:hover {
      color: #fff700;
    }
  </style>
</head>
<body>
  <header>
    <h1>Earn Rewards Mini App</h1>
  </header>
  
  <!-- User Card Section -->
  <section class="user-card">
    <h2 id="userName">Your Name</h2>
    <p>Number: <span id="userNumber">000000</span></p>
    <p>Points: <span id="userPoints">0</span></p>
    <p>Balance: $<span id="userBalance">0.00</span></p>
  </section>
  
  <!-- Reward Section -->
  <section class="reward-section">
    <button class="reward-button" id="rewardBtn">Watch Ad for Rewards</button>
    <div class="countdown" id="countdownTimer">15</div>
  </section>
  
  <!-- Navigation Bar -->
  <nav>
    <a href="#">Home</a>
    <a href="#">Profile</a>
    <a href="#">Contact</a>
  </nav>
  
  <script>
    // ১৫ সেকেন্ডের কাউন্টডাউন শুরু করার ফাংশন
    function startCountdown(seconds) {
      const countdownElement = document.getElementById('countdownTimer');
      countdownElement.style.display = 'block';
      let remaining = seconds;
      countdownElement.textContent = remaining;
      
      const intervalId = setInterval(() => {
        remaining--;
        countdownElement.textContent = remaining;
        if (remaining <= 0) {
          clearInterval(intervalId);
          countdownElement.style.display = 'none';
          // রিওয়ার্ড আপডেট ফাংশন কল করুন
          updateRewards();
          alert('Congratulations! You earned rewards!');
        }
      }, 1000);
    }
    
    // রিওয়ার্ড আপডেট করার ফাংশন (আপনার ইচ্ছা অনুযায়ী কাস্টমাইজ করুন)
    function updateRewards() {
      // উদাহরণস্বরূপ, ১০ পয়েন্ট এবং $0.05 যোগ করা হচ্ছে
      let points = parseInt(localStorage.getItem('userPoints')) || 0;
      let balance = parseFloat(localStorage.getItem('userBalance')) || 0.00;
      
      points += 10;  // ১০ পয়েন্ট যোগ করুন
      balance += 0.05;  // $0.05 যোগ করুন
      
      localStorage.setItem('userPoints', points);
      localStorage.setItem('userBalance', balance.toFixed(2));
      
      // ইউআই আপডেট করুন
      document.getElementById('userPoints').textContent = points;
      document.getElementById('userBalance').textContent = balance.toFixed(2);
    }
    
    // Monetag Rewarded Interstitial Ad দেখানোর ফাংশন
    async function showRewardedAd() {
      // এখানে rewarded interstitial এর Zone ID ব্যবহার করা হচ্ছে: 9025714
      if (typeof show_9025714 === 'function') {
        try {
          // rewarded ad প্লে করার চেষ্টা
          await show_9025714();
          // অ্যাড প্লে সফল হলে ১৫ সেকেন্ডের কাউন্টডাউন শুরু হবে
          startCountdown(15);
        } catch (error) {
          console.error('Error showing ad:', error);
          alert('Ad failed to load.');
        }
      } else {
        alert('Ads not available.');
      }
    }
    
    // Reward Button-এ ক্লিক ইভেন্ট সংযুক্তি
    document.getElementById('rewardBtn').addEventListener('click', showRewardedAd);
    
    // প্রাথমিকভাবে, যদি পূর্বের রিওয়ার্ড তথ্য থাকে, তা লোড করুন
    document.getElementById('userPoints').textContent = localStorage.getItem('userPoints') || '0';
    document.getElementById('userBalance').textContent = localStorage.getItem('userBalance') || '0.00';
  </script>
</body>
</html>
