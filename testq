<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Web3 ETH Sender</title>
    <script src="https://cdn.ethers.io/lib/ethers-5.7.2.umd.min.js" type="application/javascript"></script>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            color: #fff;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            max-width: 500px;
            width: 100%;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }
        
        h1 {
            text-align: center;
            margin-bottom: 30px;
            color: #4fbdba;
        }
        
        .wallet-info {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 25px;
            display: none;
        }
        
        .wallet-info.active {
            display: block;
        }
        
        .info-row {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .info-row:last-child {
            margin-bottom: 0;
            border-bottom: none;
        }
        
        .label {
            color: #7ec8e3;
        }
        
        .value {
            font-weight: bold;
            word-break: break-all;
        }
        
        .btn {
            background: linear-gradient(45deg, #4fbdba, #7ec8e3);
            border: none;
            border-radius: 50px;
            color: white;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            padding: 12px 25px;
            width: 100%;
            margin-bottom: 20px;
            transition: all 0.3s ease;
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(79, 189, 186, 0.4);
        }
        
        .btn:disabled {
            background: #555;
            cursor: not-allowed;
            transform: none;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            color: #7ec8e3;
        }
        
        input {
            width: 100%;
            padding: 12px 15px;
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            background: rgba(255, 255, 255, 0.05);
            color: white;
            font-size: 16px;
        }
        
        input:focus {
            outline: none;
            border-color: #4fbdba;
            box-shadow: 0 0 0 2px rgba(79, 189, 186, 0.2);
        }
        
        .status-message {
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            text-align: center;
            display: none;
        }
        
        .status-message.success {
            background: rgba(72, 187, 120, 0.2);
            border: 1px solid rgba(72, 187, 120, 0.5);
            color: #48bb78;
            display: block;
        }
        
        .status-message.error {
            background: rgba(245, 101, 101, 0.2);
            border: 1px solid rgba(245, 101, 101, 0.5);
            color: #f56565;
            display: block;
        }
        
        .status-message.info {
            background: rgba(66, 153, 225, 0.2);
            border: 1px solid rgba(66, 153, 225, 0.5);
            color: #4299e1;
            display: block;
        }
        
        .loader {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255, 255, 255, 0.3);
            border-radius: 50%;
            border-top-color: #4fbdba;
            animation: spin 1s ease-in-out infinite;
            margin-right: 10px;
            vertical-align: middle;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .hidden {
            display: none;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Web3 ETH Sender</h1>
        
        <button id="connectWallet" class="btn">Hubungkan Dompet</button>
        
        <div id="walletInfo" class="wallet-info">
            <div class="info-row">
                <span class="label">Alamat Dompet:</span>
                <span id="walletAddress" class="value"></span>
            </div>
            <div class="info-row">
                <span class="label">Saldo ETH:</span>
                <span id="walletBalance" class="value"></span>
            </div>
            <div class="info-row">
                <span class="label">Jaringan:</span>
                <span id="networkName" class="value"></span>
            </div>
        </div>
        
        <div id="sendForm" class="hidden">
            <div class="form-group">
                <label for="recipientAddress">Alamat Penerima</label>
                <input type="text" id="recipientAddress" placeholder="0x...">
            </div>
            
            <div class="form-group">
                <label for="amount">Jumlah ETH</label>
                <input type="number" id="amount" placeholder="0.01" step="0.0001" min="0">
            </div>
            
            <button id="sendButton" class="btn">Kirim ETH</button>
        </div>
        
        <div id="statusMessage" class="status-message"></div>
    </div>

    <script>
        // Elemen DOM
        const connectWalletBtn = document.getElementById('connectWallet');
        const walletInfo = document.getElementById('walletInfo');
        const walletAddress = document.getElementById('walletAddress');
        const walletBalance = document.getElementById('walletBalance');
        const networkName = document.getElementById('networkName');
        const sendForm = document.getElementById('sendForm');
        const recipientInput = document.getElementById('recipientAddress');
        const amountInput = document.getElementById('amount');
        const sendButton = document.getElementById('sendButton');
        const statusMessage = document.getElementById('statusMessage');
        
        // Variabel global
        let provider;
        let signer;
        let account;
        
        // Event Listeners
        connectWalletBtn.addEventListener('click', connectWallet);
        sendButton.addEventListener('click', sendETH);
        
        // Fungsi untuk menghubungkan dompet
        async function connectWallet() {
            if (typeof window.ethereum !== 'undefined') {
                try {
                    // Meminta akses ke dompet
                    const accounts = await window.ethereum.request({ method: 'eth_requestAccounts' });
                    account = accounts[0];
                    
                    // Membuat provider dan signer
                    provider = new ethers.providers.Web3Provider(window.ethereum);
                    signer = provider.getSigner();
                    
                    // Update UI
                    walletAddress.textContent = account;
                    await updateBalance();
                    await updateNetwork();
                    
                    // Tampilkan informasi dompet dan form kirim
                    walletInfo.classList.add('active');
                    sendForm.classList.remove('hidden');
                    connectWalletBtn.textContent = 'Terhubung';
                    connectWalletBtn.disabled = true;
                    
                    showStatus('Dompet berhasil terhubung!', 'success');
                    
                    // Setup event listener untuk perubahan akun
                    window.ethereum.on('accountsChanged', (accounts) => {
                        if (accounts.length === 0) {
                            // Dompet terputus
                            disconnectWallet();
                        } else {
                            // Akun berubah
                            account = accounts[0];
                            walletAddress.textContent = account;
                            updateBalance();
                        }
                    });
                    
                    // Setup event listener untuk perubahan jaringan
                    window.ethereum.on('chainChanged', () => {
                        updateNetwork();
                        updateBalance();
                    });
                    
                } catch (error) {
                    console.error("Error connecting wallet:", error);
                    showStatus('Gagal menghubungkan dompet: ' + error.message, 'error');
                }
            } else {
                showStatus('Dompet Web3 tidak terdeteksi. Silakan instal MetaMask!', 'error');
            }
        }
        
        // Fungsi untuk memperbarui saldo
        async function updateBalance() {
            try {
                const balance = await provider.getBalance(account);
                const ethBalance = ethers.utils.formatEther(balance);
                walletBalance.textContent = `${parseFloat(ethBalance).toFixed(4)} ETH`;
            } catch (error) {
                console.error("Error fetching balance:", error);
                walletBalance.textContent = "Error";
            }
        }
        
        // Fungsi untuk memperbarui informasi jaringan
        async function updateNetwork() {
            try {
                const network = await provider.getNetwork();
                let networkDisplayName = network.name;
                
                // Menampilkan nama jaringan yang lebih ramah pengguna
                if (network.chainId === 1) {
                    networkDisplayName = "Ethereum Mainnet";
                } else if (network.chainId === 5) {
                    networkDisplayName = "Goerli Testnet";
                } else if (network.chainId === 11155111) {
                    networkDisplayName = "Sepolia Testnet";
                }
                
                networkName.textContent = `${networkDisplayName} (Chain ID: ${network.chainId})`;
            } catch (error) {
                console.error("Error fetching network:", error);
                networkName.textContent = "Unknown";
            }
        }
        
        // Fungsi untuk mengirim ETH
        async function sendETH() {
            const recipientAddress = recipientInput.value.trim();
            const amount = amountInput.value;
            
            // Validasi input
            if (!recipientAddress || !amount) {
                showStatus('Silakan masukkan alamat penerima dan jumlah ETH', 'error');
                return;
            }
            
            if (!ethers.utils.isAddress(recipientAddress)) {
                showStatus('Alamat Ethereum tidak valid', 'error');
                return;
            }
            
            if (parseFloat(amount) <= 0) {
                showStatus('Jumlah ETH harus lebih besar dari 0', 'error');
                return;
            }
            
            try {
                // Tampilkan status loading
                sendButton.disabled = true;
                sendButton.innerHTML = '<span class="loader"></span> Memproses...';
                showStatus('Memproses transaksi...', 'info');
                
                // Buat transaksi
                const tx = {
                    to: recipientAddress,
                    value: ethers.utils.parseEther(amount)
                };
                
                // Kirim transaksi
                const transaction = await signer.sendTransaction(tx);
                
                // Tunggu hingga transaksi dikonfirmasi
                showStatus('Transaksi dikirim. Menunggu konfirmasi...', 'info');
                await transaction.wait();
                
                // Update saldo setelah transaksi
                await updateBalance();
                
                // Reset form
                recipientInput.value = '';
                amountInput.value = '';
                
                // Tampilkan pesan sukses
                showStatus(`Transaksi berhasil! Hash: ${transaction.hash}`, 'success');
                
                // Reset tombol
                sendButton.disabled = false;
                sendButton.textContent = 'Kirim ETH';
                
            } catch (error) {
                console.error("Error sending ETH:", error);
                showStatus('Gagal mengirim ETH: ' + error.message, 'error');
                
                // Reset tombol
                sendButton.disabled = false;
                sendButton.textContent = 'Kirim ETH';
            }
        }
        
        // Fungsi untuk menampilkan pesan status
        function showStatus(message, type) {
            statusMessage.textContent = message;
            statusMessage.className = `status-message ${type}`;
        }
        
        // Fungsi untuk memutuskan koneksi dompet
        function disconnectWallet() {
            walletInfo.classList.remove('active');
            sendForm.classList.add('hidden');
            connectWalletBtn.textContent = 'Hubungkan Dompet';
            connectWalletBtn.disabled = false;
            showStatus('Dompet terputus', 'info');
        }
    </script>
</body>
</html>
