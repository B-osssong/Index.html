# Index.html
ShardVault is the name of the project and it describes as the decentralized IP address data collection for a users whenever visiting the site  to encrypted data.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Decentralized Data Collector</title>
    <style>
        :root{--primary:#4a6fa5;--secondary:#166088;--bg:#f5f7fa;--text:#333;--light:#666;--border:#ddd}*{margin:0;padding:0;box-sizing:border-box;font-family:'Segoe UI',Tahoma,sans-serif}body{background:var(--bg);color:var(--text);line-height:1.6}.container{max-width:1200px;margin:0 auto;padding:20px}header{text-align:center;padding:30px 0;border-bottom:1px solid var(--border);margin-bottom:30px}header h1{color:var(--secondary);margin-bottom:10px}main{display:flex;flex-wrap:wrap;gap:30px;margin-bottom:30px}.data-form,.info-panel{flex:1;min-width:300px;background:#fff;padding:25px;border-radius:8px;box-shadow:0 2px 10px rgba(0,0,0,0.1)}.data-form h2,.info-panel h3{color:var(--primary);margin-bottom:20px;padding-bottom:10px;border-bottom:1px solid var(--border)}.form-group{margin-bottom:20px}.form-group label{display:block;margin-bottom:5px;font-weight:600}.form-group input{width:100%;padding:10px;border:1px solid var(--border);border-radius:4px;font-size:16px}button{background:var(--primary);color:#fff;border:none;padding:12px 20px;border-radius:4px;cursor:pointer;font-size:16px;transition:background 0.3s}button:hover{background:var(--secondary)}#submitBtn{width:100%}#accessDataBtn{margin-top:20px}.info-panel p{margin-bottom:15px;color:var(--light)}#encryptionStatus,#ipInfo,#dataHash{margin-top:20px;padding:15px;background:var(--bg);border-radius:4px;font-size:14px}footer{text-align:center;padding:20px;border-top:1px solid var(--border)}@media (max-width:768px){main{flex-direction:column}}
    </style>
</head>
<body>
    <div class="container">
        <header><h1>Decentralized Data Collection</h1><p>Your information is encrypted before storage</p></header>
        <main>
            <div class="data-form">
                <h2>Share information (optional)</h2>
                <form id="dataForm">
                    <div class="form-group"><label for="name">Name:</label><input type="text" id="name" placeholder="Optional"></div>
                    <div class="form-group"><label for="email">Email:</label><input type="email" id="email" placeholder="Optional"></div>
                    <button type="submit" id="submitBtn">Submit Securely</button>
                </form>
            </div>
            <div class="info-panel">
                <h3>How This Works</h3>
                <p>Data is encrypted locally before transmission using decentralized encryption.</p>
                <div id="encryptionStatus"><p>Generating keys...</p></div>
                <div id="ipInfo"><p>IP: <span id="ipAddress">Detecting...</span></p></div>
                <div id="dataHash"><p>Data hash: <span id="hashValue">None</span></p></div>
            </div>
        </main>
        <footer>
            <p>Using decentralized encryption to protect privacy</p>
            <button id="accessDataBtn">Access My Data</button>
        </footer>
    </div>

    <script>
        // Load JSEncrypt dynamically
        const script = document.createElement('script');
        script.src = 'https://cdnjs.cloudflare.com/ajax/libs/jsencrypt/3.3.2/jsencrypt.min.js';
        script.onload = initApp;
        document.head.appendChild(script);

        function initApp() {
            // Initialize encryption
            const crypt = new JSEncrypt({default_key_size: 2048});
            const publicKey = crypt.getPublicKey();
            const privateKey = crypt.getPrivateKey();

            document.getElementById('encryptionStatus').innerHTML = 
                `<p>Encryption ready! Keys generated locally in your browser.</p>`;

            // Get IP address
            fetch('https://api.ipify.org?format=json')
                .then(response => response.json())
                .then(data => {
                    document.getElementById('ipAddress').textContent = data.ip;
                    return data.ip;
                })
                .catch(() => {
                    document.getElementById('ipAddress').textContent = 'Unknown';
                });

            // Form submission
            document.getElementById('dataForm').addEventListener('submit', function(e) {
                e.preventDefault();
                
                const formData = {
                    name: document.getElementById('name').value,
                    email: document.getElementById('email').value,
                    timestamp: new Date().toISOString(),
                    ip: document.getElementById('ipAddress').textContent
                };
                
                // Encrypt data
                crypt.setPublicKey(publicKey);
                const encryptedData = crypt.encrypt(JSON.stringify(formData));
                
                if (encryptedData) {
                    // Simulate decentralized storage
                    const dataHash = sha256(encryptedData);
                    document.getElementById('hashValue').textContent = dataHash.substring(0, 12) + '...';
                    
                    // Simulate sending to multiple nodes
                    simulateDecentralizedStorage(encryptedData);
                    
                    alert('Data encrypted and stored decentralized!');
                } else {
                    alert('Encryption failed');
                }
            });

            // Access data button
            document.getElementById('accessDataBtn').addEventListener('click', function() {
                const key = prompt('Enter your private key to decrypt data:');
                if (key) {
                    crypt.setPrivateKey(key);
                    alert('In a real implementation, this would decrypt your data');
                }
            });

            // Helper functions
            function simulateDecentralizedStorage(data) {
                console.log('Simulating storage across decentralized network:', {
                    node1: {status: 'stored', location: 'US'},
                    node2: {status: 'stored', location: 'EU'},
                    node3: {status: 'stored', location: 'ASIA'}
                });
            }

            function sha256(input) {
                // Simplified hash simulation
                return Array.from(input)
                    .reduce((hash, char) => (hash << 5) - hash + char.charCodeAt(0), 0)
                    .toString(16);
            }
        }
    </script>
</body>
</html>
