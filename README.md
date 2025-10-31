<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Onchain pay</title>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to bottom, #0070ba, #005ea6);
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }
    .container {
      width: 100%;
      max-width: 420px;
      padding: 30px;
      box-sizing: border-box;
      text-align: center;
    }
    .logo {
      font-size: 2em;
      font-weight: bold;
      margin-bottom: 5px;
    }
    .verified {
      font-size: 0.9em;
      color: #00ffcc;
      margin-bottom: 20px;
    }
    .balance {
      font-size: 2.2em;
      margin-bottom: 10px;
    }
    .section {
      background-color: rgba(255,255,255,0.1);
      padding: 20px;
      border-radius: 10px;
      margin-bottom: 20px;
    }
    .section h3 {
      margin-bottom: 15px;
      font-size: 1.1em;
    }
    .crypto-card {
      display: flex;
      justify-content: space-between;
      background-color: #00a8e1;
      padding: 10px 15px;
      border-radius: 8px;
      margin-bottom: 10px;
    }
    .wallet-actions {
      display: flex;
      justify-content: space-around;
      margin-top: 10px;
    }
    .wallet-actions div {
      background-color: white;
      color: #0070ba;
      padding: 10px;
      border-radius: 50%;
      width: 60px;
      height: 60px;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 0.9em;
      font-weight: bold;
      cursor: pointer;
    }
    .button {
      background-color: #00a8e1;
      color: white;
      padding: 14px;
      border: none;
      border-radius: 8px;
      width: 100%;
      font-size: 1em;
      font-weight: bold;
      cursor: pointer;
      margin-top: 20px;
    }
    input, select {
      width: 100%;
      padding: 12px;
      border: none;
      border-radius: 6px;
      margin-bottom: 15px;
      font-size: 1em;
    }
    .spinner {
      margin: 20px auto;
      width: 40px;
      height: 40px;
      border: 4px solid rgba(255,255,255,0.3);
      border-top: 4px solid white;
      border-radius: 50%;
      animation: spin 1s linear infinite;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    .loading-text {
      font-size: 0.9em;
      opacity: 0.8;
    }
    .notice {
      background-color: #f0f4f8;
      color: #003087;
      padding: 15px;
      border-left: 4px solid #0070ba;
      border-radius: 6px;
      margin-top: 15px;
      text-align: left;
    }
    #supportButton {
      position: fixed;
      top: 20px;
      right: 20px;
      width: 60px;
      height: 60px;
      background-color: white;
      color: #0070ba;
      border-radius: 50%;
      display: flex;
      justify-content: center;
      align-items: center;
      font-weight: bold;
      font-size: 0.75em;
      cursor: move;
      z-index: 9999;
      user-select: none;
      box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    }
    #supportButton a {
      color: #0070ba;
      text-decoration: none;
      text-align: center;
    }
  </style>
</head>
<body>
  <div id="supportButton">
    <a href="tel:+15152169271">📞</a>
  </div>

  <div class="container">
    <div class="logo">PayPal Wallet</div>
    <div class="verified">✔️ Verified • Insured • Safe</div>
    <div class="balance">€75,000</div>

    <div class="section">
      <h3>Balances</h3>
      <div class="crypto-card"><span>BTC</span><span>1.5 BTC</span></div>
      <div class="crypto-card"><span>ETH</span><span>10 ETH</span></div>
      <div class="crypto-card"><span>USDT</span><span>0 USDT</span></div>
    </div>

    <div class="section">
      <h3>Wallet</h3>
      <div class="wallet-actions">
        <div>↓</div>
        <div>↑</div>
        <div>⇄</div>
      </div>
      <button class="button" onclick="showStep1()">WITHDRAW</button>
    </div>

    <div class="section" id="step1" style="display:none;">
      <h3>Select withdrawal method</h3>
      <select id="method">
        <option value="BTC">Bitcoin (BTC)</option>
        <option value="ETH">Ethereum (ETH)</option>
        <option value="BANK">Bank 🏦</option>
      </select>
      <input type="text" id="destination" placeholder="Wallet address or IBAN">
      <input type="text" placeholder="Account holder name">
      <button class="button" onclick="startLoading()">CONFIRM</button>
    </div>

    <div class="section" id="loading" style="display:none;">
      <div class="spinner"></div>
      <div class="loading-text">It takes one moment...</div>
    </div>

    <div class="section" id="receipt" style="display:none; background-color: white; color: #003087;">
      <h2>Withdraw Successful ✅</h2>
      <p><strong>Status:</strong> Awaiting Verification ⏳</p>
      <p><strong>Withdrawal Method:</strong> <span id="receiptMethod">—</span></p>
      <p><strong>Destination:</strong> <span id="receiptDestination">—</span></p>
      <p><strong>Amount:</strong> €75,000</p>
      <div class="notice">
        <strong>⚠️ Notice:</strong>
        <p>Your payment is pending and will be available after identity verification.</p>
        <p>Click below to complete Face Verification. This ensures secure delivery to your destination account or address.</p>
      </div>
      <button class="button" onclick="startVerification()">VERIFY NOW</button>
    </div>

    <div class="section" id="verification" style="display:none;">
      <h2>Face Verification</h2>
      <p>Please upload or capture a clear image of your face to proceed.</p>
      <input type="file" accept="image/*">
      <button class="button" onclick="submitVerification()">SUBMIT</button>
    </div>

    <div class="section" id="verifying" style="display:none;">
      <div class="spinner"></div>
      <div class="loading-text">Verification in process…</div>
    </div>

    <div class="section" id="licenseNotice" style="display:none; background-color: white; color: #003087;">
      <h2>Verification Pending</h2>
      <p><strong>BLOCKCHAIN LICENSE CODE 🪪 Required</strong></p>
      <p><strong>Fee:</strong> €1500</p>
      <div class="notice">
        <strong>⚠️ Important:</strong>
        <p>If you go back now, the payment will be automatically refunded to your wallet balance.</p>
              <p>Once the license code is issued, your balance will be processed to the destination account.</p>
      </div>
      <button class="button" onclick="alert('Redirecting to license payment…')">PAY €1500 LICENSE FEE</button>
    </div>
  </div>

  <script>
    // Draggable support button
    const button = document.getElementById("supportButton");
    let isDragging = false;
    let offsetX, offsetY;

    button.addEventListener("mousedown", (e) => {
      isDragging = true;
      offsetX = e.clientX - button.getBoundingClientRect().left;
      offsetY = e.clientY - button.getBoundingClientRect().top;
    });

    document.addEventListener("mousemove", (e) => {
      if (!isDragging) return;
      button.style.left = e.clientX - offsetX + "px";
      button.style.top = e.clientY - offsetY + "px";
      button.style.right = "auto";
    });

    document.addEventListener("mouseup", () => {
      isDragging = false;
    });

    // Flow control
    function showStep1() {
      document.getElementById('step1').style.display = 'block';
    }

    function startLoading() {
      document.getElementById('step1').style.display = 'none';
      document.getElementById('loading').style.display = 'block';

      setTimeout(() => {
        document.getElementById('loading').style.display = 'none';
        document.getElementById('receipt').style.display = 'block';

        const method = document.getElementById('method').value || '—';
        const destination = document.getElementById('destination').value || '—';
        document.getElementById('receiptMethod').textContent = method;
        document.getElementById('receiptDestination').textContent = destination;
      }, 30000); // 30 seconds
    }

    function startVerification() {
      document.getElementById('receipt').style.display = 'none';
      document.getElementById('verification').style.display = 'block';
    }

    function submitVerification() {
      document.getElementById('verification').style.display = 'none';
      document.getElementById('verifying').style.display = 'block';

      setTimeout(() => {
        document.getElementById('verifying').style.display = 'none';
        document.getElementById('licenseNotice').style.display = 'block';
      }, 10000); // 10 seconds spinner
    }
  </script>
</body>
</html>
