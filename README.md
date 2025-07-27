<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cek Kelulusan SMAN 11</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 20px;
            background-color: #f5f5f5;
            color: #333;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 30px;
            background-color: #fff;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
        }
        
        header {
            text-align: center;
            padding-bottom: 20px;
            margin-bottom: 30px;
            border-bottom: 2px solid #eaeaea;
        }
        
        h1 {
            color: #2c3e50;
            margin-bottom: 10px;
        }
        
        .school-logo {
            max-width: 120px;
            height: auto;
            margin-bottom: 15px;
        }
        
        .search-box {
            background-color: #f8f9fa;
            padding: 25px;
            border-radius: 8px;
            margin-bottom: 30px;
            text-align: center;
        }
        
        .search-box input {
            padding: 12px 15px;
            width: 70%;
            max-width: 400px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }
        
        .search-box button {
            padding: 12px 20px;
            background-color: #3498db;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            margin-left: 10px;
            transition: background-color 0.3s;
        }
        
        .search-box button:hover {
            background-color: #2980b9;
        }
        
        .result-container {
            display: none;
            background-color: #e8f4fc;
            padding: 25px;
            border-radius: 8px;
            margin-top: 20px;
            border-left: 5px solid #3498db;
        }
        
        .graduate-info {
            margin-bottom: 15px;
        }
        
        .graduate-info p {
            margin: 8px 0;
        }
        
        .status-lulus {
            font-weight: bold;
            color: #27ae60;
        }
        
        .status-tidak-lulus {
            font-weight: bold;
            color: #e74c3c;
        }
        
        .error-message {
            color: #e74c3c;
            font-weight: bold;
            text-align: center;
            margin-top: 20px;
        }
        
        footer {
            text-align: center;
            margin-top: 40px;
            padding-top: 20px;
            border-top: 1px solid #eaeaea;
            font-size: 0.9em;
            color: #7f8c8d;
        }
        
        @media (max-width: 600px) {
            .container {
                padding: 15px;
            }
            
            .search-box input {
                width: 100%;
                margin-bottom: 10px;
            }
            
            .search-box button {
                width: 100%;
                margin-left: 0;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <img src="https://upload.wikimedia.org/wikipedia/id/3/33/Logo_11Jakarta.png" alt="Logo SMAN 11" class="school-logo">
            <h1>CEK KELULUSAN SISWA</h1>
            <h2>SMA NEGERI 11 TAHUN AJARAN 2023/2024</h2>
        </header>
        
        <div class="search-box">
            <h3>CEK STATUS KELULUSAN</h3>
            <p>Masukkan NISN siswa untuk memeriksa status kelulusan:</p>
            <input type="text" id="nisnInput" placeholder="Masukkan NISN" maxlength="10">
            <button onclick="checkGraduation()">Cek Kelulusan</button>
            <div id="errorMessage" class="error-message"></div>
        </div>
        
        <div id="resultContainer" class="result-container">
            <div class="graduate-info">
                <h3>HASIL PENCARIAN</h3>
                <p><strong>NISN:</strong> <span id="resultNISN"></span></p>
                <p><strong>Nama:</strong> <span id="resultNama"></span></p>
                <p><strong>Kelas:</strong> <span id="resultKelas"></span></p>
                <p><strong>Status:</strong> <span id="resultStatus"></span></p>
            </div>
            <div id="additionalInfo"></div>
        </div>
        
        <div class="info-box">
            <h3>PENGUMUMAN PENTING</h3>
            <p>Pengumuman kelulusan siswa SMA Negeri 11 akan dilaksanakan pada:</p>
            <p><strong>Tanggal:</strong> 15 Mei 2024</p>
            <p><strong>Pukul:</strong> 10.00 WIB</p>
            <p><strong>Tempat:</strong> Aula SMA Negeri 11 dan dapat diakses melalui website ini</p>
        </div>
        
        <footer>
            <p>Created by Tyo Marsel INF-1</p>
            <p>Jl. Pendidikan No. 11, Kota Anda</p>
            <p>Telp: (021) 1234567 | Email: sman11@example.com</p>
        </footer>
    </div>

    <script>
        // Database siswa (bisa diganti dengan data asli)
        const studentDatabase = [
            { nisn: "1234567890", nama: "Andi Wijaya", kelas: "XII IPA 1", lulus: true, nilai: "86,5" },
            { nisn: "2345678901", nama: "Budi Santoso", kelas: "XII IPA 1", lulus: true, nilai: "82,0" },
            { nisn: "3456789012", nama: "Citra Dewi", kelas: "XII IPA 2", lulus: true, nilai: "90,2" },
            { nisn: "4567890123", nama: "Dian Pratama", kelas: "XII IPS 1", lulus: true, nilai: "78,8" },
            { nisn: "5678901234", nama: "Eka Putri", kelas: "XII IPS 2", lulus: false, nilai: "65,3" }
        ];

        function checkGraduation() {
            const nisn = document.getElementById('nisnInput').value.trim();
            const errorMessage = document.getElementById('errorMessage');
            const resultContainer = document.getElementById('resultContainer');
            
            // Reset tampilan
            errorMessage.textContent = '';
            resultContainer.style.display = 'none';
            
            // Validasi input
            if (!nisn) {
                errorMessage.textContent = 'Silakan masukkan NISN';
                return;
            }
            
            if (!/^\d{10}$/.test(nisn)) {
                errorMessage.textContent = 'NISN harus terdiri dari 10 digit angka';
                return;
            }
            
            // Cari siswa di database
            const student = studentDatabase.find(s => s.nisn === nisn);
            
            if (student) {
                // Tampilkan hasil
                document.getElementById('resultNISN').textContent = student.nisn;
                document.getElementById('resultNama').textContent = student.nama;
                document.getElementById('resultKelas').textContent = student.kelas;
                
                const statusElement = document.getElementById('resultStatus');
                if (student.lulus) {
                    statusElement.textContent = 'LULUS';
                    statusElement.className = 'status-lulus';
                    
                    // Tambahan info untuk yang lulus
                    const additionalInfo = document.getElementById('additionalInfo');
                    additionalInfo.innerHTML = `
                        <p><strong>Nilai Akhir:</strong> ${student.nilai}</p>
                        <p><strong>Keterangan:</strong> Selamat! Anda dinyatakan lulus dari SMA Negeri 11</p>
                        <p>Silakan datang ke sekolah pada tanggal yang telah ditentukan untuk pengambilan ijazah.</p>
                    `;
                } else {
                    statusElement.textContent = 'TIDAK LULUS';
                    statusElement.className = 'status-tidak-lulus';
                    
                    // Tambahan info untuk yang tidak lulus
                    const additionalInfo = document.getElementById('additionalInfo');
                    additionalInfo.innerHTML = `
                        <p><strong>Nilai Akhir:</strong> ${student.nilai}</p>
                        <p><strong>Keterangan:</strong> Maaf, Anda belum memenuhi kriteria kelulusan.</p>
                        <p>Silakan menghubungi pihak sekolah untuk informasi lebih lanjut.</p>
                    `;
                }
                
                resultContainer.style.display = 'block';
            } else {
                errorMessage.textContent = 'NISN tidak ditemukan. Silakan cek kembali.';
            }
        }
        
        // Tambahkan event listener untuk tombol enter
        document.getElementById('nisnInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                checkGraduation();
            }
        });
    </script>
</body>
</html>
