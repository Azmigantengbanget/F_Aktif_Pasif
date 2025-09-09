<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


    { "en": "Apa Rangkaian Yang Melewatkan Frekuensi Tertentu?", "id": "Filter (Penyaring)." },
    { "en": "Apa Tujuan Utama Sebuah Filter?", "id": "Memisahkan Sinyal Berdasarkan Frekuensi." },
    { "en": "Apa Filter Yang Dibuat Dari Komponen Pasif?", "id": "Filter Pasif." },
    { "en": "Komponen Apa Yang Digunakan Filter Pasif?", "id": "Resistor (R), Induktor (L), Kapasitor (C)." },
    { "en": "Apa Filter Yang Menggunakan Komponen Aktif?", "id": "Filter Aktif." },
    { "en": "Komponen Aktif Apa Yang Digunakan Filter Aktif?", "id": "Op-Amp Atau Transistor." },
    { "en": "Filter Mana Yang Membutuhkan Catu Daya Eksternal?", "id": "Filter Aktif." },
    { "en": "Filter Mana Yang Dapat Memberikan Penguatan (Gain)?", "id": "Filter Aktif." },
    { "en": "Filter Mana Yang Tidak Dapat Memberi Penguatan?", "id": "Filter Pasif." },
    { "en": "Filter Mana Yang Rentan Efek Pemuatan (Loading)?", "id": "Filter Pasif." },
    { "en": "Apa Keuntungan Utama Filter Aktif?", "id": "Adanya Penguatan, Tanpa Induktor." },
    { "en": "Apa Keuntungan Utama Filter Pasif?", "id": "Sederhana, Tidak Butuh Catu Daya." },
    { "en": "Apa Filter Yang Melewatkan Frekuensi Rendah?", "id": "LPF (Low-Pass Filter)." },
    { "en": "Apa LPF (Low-Pass Filter) Paling Sederhana?", "id": "Rangkaian RC (Resistor-Capacitor) Seri." },
    { "en": "Apa Filter Yang Melewatkan Frekuensi Tinggi?", "id": "HPF (High-Pass Filter)." },
    { "en": "Apa HPF (High-Pass Filter) Paling Sederhana?", "id": "Rangkaian CR (Capacitor-Resistor) Seri." },
    { "en": "Apa Filter Yang Melewatkan Satu Pita Frekuensi?", "id": "BPF (Band-Pass Filter)." },
    { "en": "Bagaimana Cara Membuat BPF (Band-Pass Filter) Sederhana?", "id": "Menggabungkan LPF Dan HPF." },
    { "en": "Apa Filter Yang Menolak Satu Pita Frekuensi?", "id": "BSF (Band-Stop Filter)." },
    { "en": "Apa Nama Lain BSF (Band-Stop Filter)?", "id": "Filter Notch Atau Band-Reject." },
    { "en": "Apa Rentang Frekuensi Yang Dilewatkan Filter?", "id": "Passband (Pita Lolos)." },
    { "en": "Apa Rentang Frekuensi Yang Ditolak Filter?", "id": "Stopband (Pita Tolak)." },
    { "en": "Apa Frekuensi Batas Antara Passband Dan Stopband?", "id": "Frekuensi Cutoff (fc)." },
    { "en": "Berapa Penurunan Daya Pada Frekuensi Cutoff?", "id": "-3 dB (Desibel)." },
    { "en": "Apa Titik -3 dB (Desibel)?", "id": "Daya Turun Setengah Dari Maksimum." },
    { "en": "Apa Tingkat Ketajaman Transisi Filter?", "id": "Roll-off." },
    { "en": "Apa Satuan Kemiringan Roll-off?", "id": "dB/dekade Atau dB/oktaf." },
    { "en": "Apa Itu Orde Filter?", "id": "Menentukan Kemiringan Roll-off." },
    { "en": "Berapa Roll-off Filter Orde Pertama?", "id": "-20 dB/dekade." },
    { "en": "Berapa Roll-off Filter Orde Kedua?", "id": "-40 dB/dekade." },
    { "en": "Apa Hubungan Orde Filter Dan Komponen Reaktif?", "id": "Orde Sama Dengan Jumlah Komponen." },
    { "en": "Apa Itu Frekuensi Tengah?", "id": "f0 (Frekuensi Pusat) BPF/BSF." },
    { "en": "Apa Itu Bandwidth (BW)?", "id": "Lebar Pita Frekuensi Yang Dilewatkan." },
    { "en": "Apa Ukuran Selektivitas Atau Ketajaman Puncak Filter?", "id": "Faktor Kualitas (Q Factor)." },
    { "en": "Apa Hubungan Q Factor Dan Bandwidth?", "id": "Q Tinggi Berarti Bandwidth Sempit." },
    { "en": "Apa Filter Dengan Passband Paling Datar?", "id": "Filter Butterworth." },
    { "en": "Apa Nama Lain Filter Butterworth?", "id": "Filter Maximally Flat." },
    { "en": "Apa Filter Dengan Transisi Paling Tajam?", "id": "Filter Chebyshev." },
    { "en": "Apa Kelemahan Filter Chebyshev?", "id": "Memiliki Riak (Ripple) Di Passband." },
    { "en": "Apa Filter Dengan Respon Fasa Linear?", "id": "Filter Bessel." },
    { "en": "Apa Keuntungan Filter Bessel?", "id": "Tidak Mendistorsi Bentuk Gelombang." },
    { "en": "Apa Filter Dengan Roll-off Paling Curam?", "id": "Filter Eliptik (Cauer Filter)." },
    { "en": "Apa Kelemahan Filter Eliptik?", "id": "Memiliki Riak Di Passband-Stopband." },
    { "en": "Apa Itu Respon Frekuensi?", "id": "Grafik Penguatan Vs Frekuensi." },
    { "en": "Apa Itu Respon Fasa?", "id": "Grafik Pergeseran Fasa Vs Frekuensi." },
    { "en": "Apa Itu Bode Plot?", "id": "Kombinasi Grafik Respon Frekuensi-Fasa." },
    { "en": "Apa Aplikasi LPF (Low-Pass Filter)?", "id": "Subwoofer Crossover, Anti-Aliasing." },
    { "en": "Apa Aplikasi HPF (High-Pass Filter)?", "id": "Menghilangkan Dengung DC, Tweeter Crossover." },
    { "en": "Apa Aplikasi BPF (Band-Pass Filter)?", "id": "Penerima Radio, Equalizer Audio." },
    { "en": "Apa Aplikasi BSF (Band-Stop Filter)?", "id": "Menghilangkan Derau Listrik 50/60 Hz." },
    { "en": "Apa Komponen LPF (Low-Pass Filter) Pasif Sederhana?", "id": "Satu Resistor Dan Satu Kapasitor." },
    { "en": "Bagaimana Posisi Komponen LPF (Low-Pass Filter) RC?", "id": "Resistor Seri, Kapasitor Ke Ground." },
    { "en": "Bagaimana Posisi Komponen HPF (High-Pass Filter) RC?", "id": "Kapasitor Seri, Resistor Ke Ground." },
    { "en": "Apa Komponen BPF (Band-Pass Filter) Pasif Sederhana?", "id": "Resistor, Induktor, Dan Kapasitor." },
    { "en": "Apa Itu Resonansi Dalam Filter?", "id": "Puncak Respons Pada Frekuensi Tertentu." },
    { "en": "Apa Itu Topologi Filter Aktif?", "id": "Konfigurasi Rangkaian Filter Aktif." },
    { "en": "Apa Topologi Filter Aktif Paling Populer?", "id": "Topologi Sallen-Key." },
    { "en": "Apa Keuntungan Topologi Sallen-Key?", "id": "Sederhana, Penguatan Positif." },
    { "en": "Apa Topologi Filter Yang Menggunakan Banyak Umpan Balik?", "id": "Topologi Multiple Feedback (MFB)." },
    { "en": "Apa Keuntungan Topologi MFB (Multiple Feedback)?", "id": "Stabilitas Dan Kinerja Baik." },
    { "en": "Apa Topologi Filter Universal?", "id": "Topologi State-Variable." },
    { "en": "Output Apa Yang Dihasilkan Filter State-Variable?", "id": "LPF, HPF, Dan BPF Sekaligus." },
    { "en": "Apa Topologi Filter Berbasis Biquadratic?", "id": "Topologi Biquad." },
    { "en": "Apa Itu Filter All-Pass?", "id": "Menggeser Fasa Tanpa Mengubah Amplitudo." },
    { "en": "Apa Fungsi Filter All-Pass?", "id": "Koreksi Fasa Atau Phase Shifter." },
    { "en": "Apa Itu Phase Shifter?", "id": "Rangkaian Penggeser Fasa Sinyal." },
    { "en": "Apa Itu Crossover Audio?", "id": "Filter Yang Membagi Sinyal Audio." },
    { "en": "Bagaimana Crossover Membagi Sinyal?", "id": "Frekuensi Rendah Ke Woofer, Tinggi Ke Tweeter." },
    { "en": "Apa Itu Crossover Pasif?", "id": "Crossover Setelah Penguat Daya." },
    { "en": "Apa Itu Crossover Aktif?", "id": "Crossover Sebelum Penguat Daya." },
    { "en": "Apa Keuntungan Crossover Aktif?", "id": "Efisiensi Dan Kontrol Lebih Baik." },
    { "en": "Apa Itu Equalizer?", "id": "Filter Untuk Mengatur Respon Frekuensi." },
    { "en": "Apa Itu Equalizer Grafis?", "id": "Mengatur Gain Pada Pita Frekuensi Tetap." },
    { "en": "Apa Itu Equalizer Parametrik?", "id": "Mengatur Gain, Frekuensi, Dan Q." },
    { "en": "Apa Rangkaian Yang Mensimulasikan Induktor?", "id": "Gyrator." },
    { "en": "Kenapa Perlu Mensimulasikan Induktor?", "id": "Induktor Besar Dan Mahal." },
    { "en": "Apa Itu Filter Kapasitor-Saklar?", "id": "Filter Aktif Menggunakan Kapasitor Saklar." },
    { "en": "Apa Keuntungan Filter Kapasitor-Saklar?", "id": "Mudah Diintegrasikan Dalam IC (Integrated Circuit)." },
    { "en": "Apa Itu Filter Digital?", "id": "Memproses Sinyal Menggunakan Algoritma Digital." },
    { "en": "Apa Keuntungan Filter Digital?", "id": "Sangat Fleksibel Dan Stabil." },
    { "en": "Apa Itu FIR (Finite Impulse Response) Filter?", "id": "Filter Digital Tanpa Umpan Balik." },
    { "en": "Apa Itu IIR (Infinite Impulse Response) Filter?", "id": "Filter Digital Dengan Umpan Balik." },
    { "en": "Filter Digital Mana Yang Selalu Stabil?", "id": "FIR (Finite Impulse Response) Filter." },
    { "en": "Apa Itu Filter Anti-Aliasing?", "id": "LPF (Low-Pass Filter) Sebelum ADC (Analog-to-Digital Converter)." },
    { "en": "Apa Itu Filter Rekonstruksi?", "id": "LPF (Low-Pass Filter) Setelah DAC (Digital-to-Analog Converter)." },
    { "en": "Apa Aturan Praktis Untuk Orde Filter?", "id": "Semakin Tinggi Orde, Semakin Kompleks." },
    { "en": "Apa Pengaruh Toleransi Komponen Pada Filter?", "id": "Menggeser Frekuensi Cutoff." },
    { "en": "Komponen Mana Yang Paling Mempengaruhi Kinerja Filter?", "id": "Kapasitor Dan Resistor Presisi." },
    { "en": "Apa Itu Oktaf?", "id": "Penggandaan Atau Pembagian Frekuensi." },
    { "en": "Berapa Roll-off Filter Orde Pertama Dalam dB/oktaf?", "id": "-6 dB/oktaf." },
    { "en": "Apa Itu Dekade?", "id": "Perkalian Atau Pembagian Frekuensi Sepuluh Kali." },
    { "en": "Apa Itu Passband Ripple?", "id": "Variasi Gain Di Dalam Passband." },
    { "en": "Apa Itu Stopband Attenuation?", "id": "Tingkat Redaman Di Dalam Stopband." },
    { "en": "Filter Mana Yang Tidak Memiliki Ripple?", "id": "Filter Butterworth Dan Bessel." },
    { "en": "Apa Itu Group Delay?", "id": "Ukuran Penundaan Sinyal Melewati Filter." },
    { "en": "Filter Mana Yang Punya Group Delay Paling Konstan?", "id": "Filter Bessel." },
    { "en": "Apa Itu LPF (Low-Pass Filter) Orde Kedua?", "id": "Memiliki Roll-off -40 dB/dekade." },
    { "en": "Apa Itu HPF (High-Pass Filter) Orde Kedua?", "id": "Memiliki Roll-off +40 dB/dekade." },
    { "en": "Bagaimana Mengkaskade Filter?", "id": "Menghubungkan Filter Secara Seri." },
    { "en": "Bagaimana Menghitung Orde Filter Kaskade?", "id": "Menjumlahkan Orde Masing-masing Filter." },
    { "en": "Apa Itu Filter Keramik?", "id": "Filter Pasif Untuk Frekuensi Radio." },
    { "en": "Apa Itu Filter SAW (Surface Acoustic Wave)?", "id": "Filter Kompak Untuk Frekuensi Tinggi." },
    { "en": "Apa Itu Filter Kristal?", "id": "BPF (Band-Pass Filter) Sangat Tajam." },
    { "en": "Apa Itu Filter Mekanis?", "id": "Filter Menggunakan Resonansi Mekanis." },
    { "en": "Apa Itu Filter Aktif Universal?", "id": "Filter State-Variable Atau Biquad." },
    { "en": "Bagaimana Cara Mengubah LPF (Low-Pass Filter) Menjadi HPF (High-Pass Filter)?", "id": "Menukar Posisi Resistor Dan Kapasitor." },
    { "en": "Apa Pengaruh Menambah Orde Filter?", "id": "Roll-off Menjadi Semakin Curam." },
    { "en": "Apa Itu Respon Impuls?", "id": "Output Filter Terhadap Sinyal Impuls." },
    { "en": "Apa Itu Respon Step?", "id": "Output Filter Terhadap Sinyal Step." },
    { "en": "Filter Mana Yang Menunjukkan Overshoot Pada Respon Step?", "id": "Filter Chebyshev Dan Eliptik." },
    { "en": "Filter Mana Yang Tidak Menunjukkan Overshoot?", "id": "Filter Butterworth Dan Bessel." },
    { "en": "Apa Itu Ringing?", "id": "Osilasi Yang Mereda Pada Respon Step." },
    { "en": "Apa Itu Sensitivitas?", "id": "Seberapa Besar Parameter Berubah." },
    { "en": "Apa Itu Analisis Monte Carlo?", "id": "Simulasi Efek Toleransi Komponen." },
    { "en": "Apa Itu Filter Twin-T?", "id": "Filter Notch Pasif Menggunakan RC." },
    { "en": "Apa Itu Jembatan Wien?", "id": "BPF (Band-Pass Filter) Pasif." },
    { "en": "Apa Itu Filter Pi (π)?", "id": "Filter LPF (Low-Pass Filter) Dengan Bentuk Pi." },
    { "en": "Apa Itu Filter T?", "id": "Filter LPF (Low-Pass Filter) Dengan Bentuk T." },
    { "en": "Apa Itu Filter Celah (Gap Filter)?", "id": "Nama Lain Untuk Band-Stop Filter." },
    { "en": "Apa Itu Respon Amplitudo?", "id": "Magnitudo Penguatan Vs Frekuensi." },
    { "en": "Apa Itu Penundaan Fasa (Phase Delay)?", "id": "Penundaan Waktu Untuk Frekuensi Tunggal." },
    { "en": "Apa Itu Penundaan Grup (Group Delay)?", "id": "Penundaan Amplop Sinyal." },
    { "en": "Apa Itu Distorsi Fasa?", "id": "Penundaan Grup Tidak Konstan." },
    { "en": "Apa Itu Respon Frekuensi Normalisasi?", "id": "Plot Dengan Frekuensi Cutoff Di 1." },
    { "en": "Apa Fungsi Kapasitor Bypass?", "id": "Filter Lolos Bawah Untuk Catu Daya." },
    { "en": "Apa Fungsi Kapasitor Kopling?", "id": "Filter Lolos Atas Untuk Sinyal AC." },
    { "en": "Apa Fungsi Ferrite Bead?", "id": "Berperilaku Seperti LPF (Low-Pass Filter) Frekuensi Tinggi." },
    { "en": "Apa Itu Filter EMI (Electromagnetic Interference)?", "id": "Menekan Gangguan Frekuensi Tinggi." },
    { "en": "Apa Itu Common-Mode Choke?", "id": "Induktor Untuk Menekan Derau Common-mode." },
    { "en": "Apa Itu Kapasitor X?", "id": "Kapasitor Untuk Derau Diferensial." },
    { "en": "Apa Itu Kapasitor Y?", "id": "Kapasitor Untuk Derau Common-mode." },
    { "en": "Apa Itu Orde Prototipe Filter?", "id": "Desain Filter Dengan Nilai Normalisasi." },
    { "en": "Apa Itu Transformasi Frekuensi?", "id": "Mengubah LPF Menjadi HPF/BPF/BSF." },
    { "en": "Apa Itu Penskalaan Impedansi?", "id": "Mengubah Nilai Komponen Ke Impedansi Baru." },
    { "en": "Apa Itu LPF (Low-Pass Filter) Butterworth Orde Pertama?", "id": "Rangkaian RC (Resistor-Capacitor) Sederhana." },
    { "en": "Apa Itu HPF (High-Pass Filter) Butterworth Orde Pertama?", "id": "Rangkaian CR (Capacitor-Resistor) Sederhana." },
    { "en": "Apa Itu Damping Factor?", "id": "Menentukan Bentuk Respon Filter Orde Dua." },
    { "en": "Apa Filter Yang Critically Damped?", "id": "Respon Tercepat Tanpa Overshoot." },
    { "en": "Apa Filter Yang Underdamped?", "id": "Respon Dengan Overshoot Dan Ringing." },
    { "en": "Apa Filter Yang Overdamped?", "id": "Respon Lambat Tanpa Overshoot." },
    { "en": "Berapa Damping Factor Untuk Butterworth?", "id": "0.707." },
    { "en": "Apa Itu Pole Filter?", "id": "Frekuensi Kompleks Dalam Fungsi Transfer." },
    { "en": "Apa Itu Zero Filter?", "id": "Frekuensi Yang Menghasilkan Atenuasi Tak Terhingga." },
    { "en": "Filter Mana Yang Memiliki Zero?", "id": "Filter Eliptik Dan Notch." },
    { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Matematis Output Per Input." },
    { "en": "Apa Itu Domain-s?", "id": "Domain Frekuensi Kompleks (Laplace)." },
    { "en": "Apa Itu Domain-z?", "id": "Domain Frekuensi Diskrit (Untuk Filter Digital)." },
    { "en": "Apa Itu Filter Cascade?", "id": "Filter Orde Tinggi Dari Orde Rendah." },
    { "en": "Apa Itu Pole-Zero Plot?", "id": "Diagram Lokasi Pole Dan Zero." },
    { "en": "Apa Itu Stabilitas Filter?", "id": "Filter Tidak Berosilasi Secara Sendiri." },
    { "en": "Syarat Stabilitas Filter?", "id": "Semua Pole Di Sisi Kiri Bidang-s." },
    { "en": "Apa Itu VCVS (Voltage-Controlled Voltage Source) Filter?", "id": "Nama Lain Untuk Topologi Sallen-Key." },
    { "en": "Apa Itu Filter GIC (Generalized Impedance Converter)?", "id": "Menggunakan GIC Untuk Mensimulasikan Induktor." },
    { "en": "Apa Itu Filter Khzarian?", "id": "Jenis Filter Pasif Lolos Atas." },
    { "en": "Apa Itu Filter Constant-K?", "id": "Jenis Filter Pasif Awal." },
    { "en": "Apa Itu Filter M-Derived?", "id": "Penyempurnaan Dari Filter Constant-K." },
    { "en": "Apa Itu Filter Lattice?", "id": "Topologi Filter Berbentuk Jembatan." },
    { "en": "Apa Itu Filter Linier?", "id": "Output Proporsional Terhadap Input." },
    { "en": "Apa Itu Filter Non-Linier?", "id": "Contoh: Filter Median." },
    { "en": "Apa Itu Filter Adaptif?", "id": "Filter Yang Koefisiennya Dapat Berubah." },
    { "en": "Apa Aplikasi Filter Adaptif?", "id": "Pembatalan Derau (Noise Cancellation)." },
    { "en": "Apa Itu Filter Kalman?", "id": "Filter Optimal Untuk Estimasi Keadaan." },
    { "en": "Apa Itu Filter Wiener?", "id": "Filter Optimal Untuk Menghilangkan Derau." },
    { "en": "Apa Itu Ekualisasi?", "id": "Mengkompensasi Distorsi Frekuensi Kanal." },
    { "en": "Apa Itu Filter Pre-emphasis?", "id": "Menguatkan Frekuensi Tinggi Sebelum Transmisi." },
    { "en": "Apa Itu Filter De-emphasis?", "id": "Melemahkan Frekuensi Tinggi Setelah Penerimaan." },
    { "en": "Apa Itu Filter Digital?", "id": "Filter Diimplementasikan Dalam Perangkat Lunak." },
    { "en": "Apa Itu Koefisien Filter?", "id": "Konstanta Yang Menentukan Respon Filter." },
    { "en": "Apa Itu Kuantisasi Koefisien?", "id": "Error Akibat Representasi Digital Terbatas." },
    { "en": "Filter Mana Yang Lebih Sensitif Terhadap Kuantisasi?", "id": "IIR (Infinite Impulse Response) Filter." },
    { "en": "Apa Itu Desain Filter?", "id": "Proses Menentukan Tipe Dan Nilai Komponen." },
    { "en": "Apa Itu Tabel Desain Filter?", "id": "Tabel Nilai Komponen Normalisasi." },
    { "en": "Apa Itu Perangkat Lunak Desain Filter?", "id": "Membantu Menghitung Nilai Komponen." },
    { "en": "Apa Itu Passband Gain?", "id": "Penguatan Di Dalam Pita Lolos." },
    { "en": "Apa Itu DC Gain?", "id": "Penguatan Pada Frekuensi Nol (DC)." },
    { "en": "Apa Itu Atenuasi?", "id": "Pelemahan Sinyal (Kebalikan Penguatan)." },
    { "en": "Apa Itu Fasa Linear?", "id": "Pergeseran Fasa Proporsional Dengan Frekuensi." },
    { "en": "Apa Pentingnya Fasa Linear?", "id": "Menjaga Integritas Bentuk Gelombang." },
    { "en": "Apa Itu Distorsi Delay?", "id": "Disebabkan Oleh Fasa Non-Linear." },
    { "en": "Apa Itu Filter Analog?", "id": "Filter Yang Bekerja Pada Sinyal Kontinu." },
    { "en": "Apa Itu Filter Waktu Diskrit?", "id": "Filter Yang Bekerja Pada Sinyal Tersampel." },
    { "en": "Apa Itu Filter IIR (Infinite Impulse Response) Analog?", "id": "Semua Filter Pasif Dan Aktif." },
    { "en": "Apakah Filter FIR (Finite Impulse Response) Bisa Dibuat Analog?", "id": "Tidak, Secara Definisi Adalah Digital." },
    { "en": "Apa Itu Transformasi Bilinear?", "id": "Metode Mendesain Filter IIR Digital." },
    { "en": "Apa Itu Warping Frekuensi?", "id": "Efek Samping Dari Transformasi Bilinear." },
    { "en": "Apa Itu Metode Invarians Impuls?", "id": "Metode Lain Mendesain Filter IIR." },
    { "en": "Apa Itu Jendela (Windowing) Dalam Desain FIR?", "id": "Metode Untuk Mendesain Filter FIR." },
    { "en": "Contoh Fungsi Jendela?", "id": "Hamming, Hanning, Blackman, Kaiser." },
    { "en": "Apa Itu Efek Gibbs?", "id": "Overshoot Dekat Transisi Tajam." },
    { "en": "Bagaimana Jendela Mengurangi Efek Gibbs?", "id": "Dengan Memuluskan Transisi." },
    { "en": "Apa Itu Filter Rata-rata Bergerak?", "id": "Contoh Sederhana LPF (Low-Pass Filter) FIR." },
    { "en": "Apa Itu Filter CIC (Cascaded Integrator-Comb)?", "id": "Filter FIR Efisien Untuk Desimasi." },
    { "en": "Apa Itu Desimasi?", "id": "Mengurangi Laju Sampel Sinyal." },
    { "en": "Apa Itu Interpolasi?", "id": "Meningkatkan Laju Sampel Sinyal." },
    { "en": "Apa Itu Bank Filter?", "id": "Kumpulan Filter Untuk Memisahkan Sinyal." },
    { "en": "Apa Aplikasi Bank Filter?", "id": "Equalizer Grafis, Analisis Spektrum." },
    { "en": "Apa Itu Bank Filter Kuadratur Cermin?", "id": "QMF (Quadrature Mirror Filter) Bank." },
    { "en": "Apa Itu Transformasi Wavelet Diskrit?", "id": "DWT (Discrete Wavelet Transform)." },
    { "en": "Apa Itu Skalogram?", "id": "Representasi Waktu-Frekuensi Sinyal." },
    { "en": "Apa Itu Duplekser?", "id": "Filter Untuk Memisahkan Sinyal Kirim/Terima." },
    { "en": "Apa Itu Diplekser?", "id": "Filter Untuk Menggabungkan/Memisahkan Pita Frekuensi." },
    { "en": "Apa Itu Sirkuit Penentu Nada?", "id": "Tone Control Circuit." },
    { "en": "Apa Itu Filter Resonansi?", "id": "Filter BPF (Band-Pass Filter) Dengan Q Tinggi." },
    { "en": "Apa Itu Filter Pasif Orde Pertama?", "id": "Menggunakan Satu Komponen Reaktif." },
    { "en": "Apa Itu Filter Pasif Orde Kedua?", "id": "Menggunakan Dua Komponen Reaktif." },
    { "en": "Apa Itu Filter Aktif Orde Pertama?", "id": "Menggunakan Satu Kapasitor." },
    { "en": "Apa Itu Filter Aktif Orde Kedua?", "id": "Menggunakan Dua Kapasitor." },
    { "en": "Apa Itu Riak Passband?", "id": "Variasi Gain Di Dalam Passband." },
    { "en": "Apa Itu Atenuasi Stopband?", "id": "Tingkat Redaman Di Dalam Stopband." },
    { "en": "Filter Mana Yang Paling Kompleks?", "id": "Filter Eliptik." },
    { "en": "Filter Mana Yang Paling Sederhana Desainnya?", "id": "Filter Butterworth." },
    { "en": "Apa Itu Unity Gain Frequency?", "id": "Frekuensi Saat Gain Op-Amp Sama Dengan 1." },
    { "en": "Apa Pengaruh Gain-Bandwidth Product Op-Amp?", "id": "Membatasi Frekuensi Kerja Filter Aktif." },
    { "en": "Apa Itu Slew Rate?", "id": "Laju Perubahan Tegangan Output Maksimum." },
    { "en": "Bagaimana Slew Rate Mempengaruhi Filter?", "id": "Menyebabkan Distorsi Pada Frekuensi Tinggi." },
    { "en": "Apa Itu Filter Notch?", "id": "BSF (Band-Stop Filter) Dengan Bandwidth Sangat Sempit." },
    { "en": "Apa Tujuan Filter Notch?", "id": "Menghilangkan Satu Frekuensi Spesifik." },
    { "en": "Apa Itu Filter Tangga (Ladder Filter)?", "id": "Topologi Filter Pasif Berulang." },
    { "en": "Apa Itu Filter LC (Inductor-Capacitor)?", "id": "Filter Pasif Menggunakan Induktor-Kapasitor." },
    { "en": "Apa Keuntungan Filter LC (Inductor-Capacitor)?", "id": "Roll-off Lebih Tajam Dari Filter RC." },
    { "en": "Apa Kerugian Filter LC (Inductor-Capacitor)?", "id": "Induktor Mahal Dan Tidak Ideal." },
    { "en": "Apa Itu Filter RC (Resistor-Capacitor)?", "id": "Filter Pasif Hanya Menggunakan R dan C." },
    { "en": "Apa Itu Jaringan Lag?", "id": "Rangkaian RC Yang Berfungsi Sebagai LPF." },
    { "en": "Apa Itu Jaringan Lead?", "id": "Rangkaian RC Yang Berfungsi Sebagai HPF." },
    { "en": "Apa Itu Jaringan Lag-Lead?", "id": "Kombinasi Jaringan Lag Dan Lead." },
    { "en": "Apa Itu Kompensator?", "id": "Filter Yang Digunakan Dalam Sistem Kontrol." },
    { "en": "Apa Itu Ripple Tegangan?", "id": "Sisa Tegangan AC Pada Output Penyearah." },
    { "en": "Filter Apa Yang Mengurangi Ripple?", "id": "LPF (Low-Pass Filter) Kapasitif." },
    { "en": "Apa Itu Filter DC Block?", "id": "Nama Lain Untuk HPF (High-Pass Filter)." },
    { "en": "Apa Itu Filter Rumble?", "id": "HPF (High-Pass Filter) Untuk Menghilangkan Frekuensi Rendah." },
    { "en": "Apa Itu Filter Hiss?", "id": "LPF (Low-Pass Filter) Untuk Menghilangkan Desis." },
    { "en": "Apa Itu Filter Pita Sisi (Sideband Filter)?", "id": "BPF (Band-Pass Filter) Untuk Sinyal SSB." },
    { "en": "Apa Itu Filter IF (Intermediate Frequency)?", "id": "Filter Selektif Di Penerima Radio." },
    { "en": "Apa Itu Filter RF (Radio Frequency)?", "id": "Filter Awal Di Penerima Radio." },
    { "en": "Apa Itu Filter Citra (Image Filter)?", "id": "Menekan Frekuensi Citra (Image Frequency)." },
    { "en": "Apa Itu Filter Diskrit?", "id": "Filter Dibuat Dari Komponen Terpisah." },
    { "en": "Apa Itu Filter Terdistribusi?", "id": "Filter Menggunakan Elemen Saluran Transmisi." },
    { "en": "Di Mana Filter Terdistribusi Digunakan?", "id": "Aplikasi Frekuensi Microwave." },
    { "en": "Apa Itu Filter Mikrostrip?", "id": "Filter Planar Di Papan PCB." },
    { "en": "Apa Itu Filter Stripline?", "id": "Filter Planar Di Antara Dua Lapisan Ground." },
    { "en": "Apa Itu Filter Rongga (Cavity Filter)?", "id": "Resonator Gelombang Mikro Kualitas Tinggi." },
    { "en": "Apa Itu Filter Pemandu Gelombang (Waveguide Filter)?", "id": "Filter Untuk Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Filter YIG (Yttrium Iron Garnet)?", "id": "Filter Yang Dapat Disetel Secara Magnetis." },
    { "en": "Apa Itu Filter BAW (Bulk Acoustic Wave)?", "id": "Filter Kompak Untuk Ponsel." },
    { "en": "Apa Itu Penolakan Stopband?", "id": "Seberapa Baik Filter Menolak Sinyal." },
    { "en": "Apa Itu Insertion Loss?", "id": "Kehilangan Sinyal Di Dalam Passband." },
    { "en": "Apa Itu Return Loss?", "id": "Ukuran Pantulan Sinyal Di Passband." },
    { "en": "Apa Itu Biquadratic Filter Section?", "id": "Blok Bangunan Filter Orde Dua." },
    { "en": "Apa Itu Infinite Gain Multiple Feedback?", "id": "Topologi Filter MFB (Multiple Feedback)." },
    { "en": "Apa Itu Filter Tangga Aktif?", "id": "Simulasi Filter Tangga Pasif." },
    { "en": "Apa Itu Filter FLIE (Frequency-Locked Loop)?", "id": "Filter Yang Melacak Frekuensi Sinyal." },
    { "en": "Apa Itu Pole Dominan?", "id": "Pole Dengan Frekuensi Terendah." },
    { "en": "Apa Pengaruh Pole Dominan?", "id": "Menentukan Bandwidth Keseluruhan Sistem." },
    { "en": "Apa Itu Kompensasi Pole-Zero?", "id": "Teknik Menstabilkan Sistem Umpan Balik." },
    { "en": "Apa Itu Normalisasi?", "id": "Menskalakan Nilai Komponen Dan Frekuensi." },
    { "en": "Kenapa Normalisasi Digunakan?", "id": "Menyederhanakan Proses Desain Filter." },
    { "en": "Apa Itu Transformasi Low-pass ke Band-pass?", "id": "Mengubah Desain LPF Menjadi BPF." },
    { "en": "Apa Itu Transformasi Low-pass ke Band-stop?", "id": "Mengubah Desain LPF Menjadi BSF." },
    { "en": "Apa Itu Tanggapan Fasa Minimum?", "id": "Fasa Terkecil Untuk Respon Amplitudo." },
    { "en": "Apa Itu Tanggapan Fasa Non-Minimum?", "id": "Memiliki Zero Di Sisi Kanan Bidang-s." },
    { "en": "Apa Itu Fungsi Transfer Jaringan?", "id": "Mendeskripsikan Perilaku Jaringan." },
    { "en": "Apa Itu Filter Matched?", "id": "Filter Optimal Untuk Mendeteksi Pulsa." },
    { "en": "Apa Itu Filter Transversal?", "id": "Struktur Dasar Filter FIR." },
    { "en": "Apa Itu Jalur Tunda (Tapped Delay Line)?", "id": "Komponen Utama Filter Transversal." },
    { "en": "Apa Itu Filter Kisi (Lattice Filter)?", "id": "Struktur Lain Untuk Filter Digital." },
    { "en": "Apa Itu Desain Filter Parks-McClellan?", "id": "Algoritma Desain Filter FIR Optimal." },
    { "en": "Apa Itu Respon Equiripple?", "id": "Riak Dengan Amplitudo Sama." },
    { "en": "Filter Mana Yang Punya Respon Equiripple?", "id": "Filter Chebyshev Dan Eliptik." },
    { "en": "Apa Itu Power Spectral Density (PSD)?", "id": "Kepadatan Spektral Daya." },
    { "en": "Bagaimana Filter Mempengaruhi PSD Sinyal?", "id": "Membentuk Spektrum Sesuai Responnya." },
    { "en": "Apa Itu Pemutihan (Whitening)?", "id": "Membuat Spektrum Sinyal Menjadi Datar." },
    { "en": "Apa Itu Filter Gaussian?", "id": "Filter Dengan Respon Impuls Gaussian." },
    { "en": "Apa Sifat Filter Gaussian?", "id": "Tidak Ada Overshoot, Rise Time Cepat." },
    { "en": "Apa Itu Filter Raised-Cosine?", "id": "Filter Untuk Komunikasi Digital Bebas ISI." },
    { "en": "Apa Itu Filter Root-Raised-Cosine (RRC)?", "id": "Digunakan Di Pemancar Dan Penerima." },
    { "en": "Apa Itu Filter Switched-Capacitor (SC)?", "id": "Filter Waktu Diskrit Analog." },
    { "en": "Apa Prinsip Kerja Filter SC (Switched-Capacitor)?", "id": "Resistor Disimulasikan Dengan Kapasitor Saklar." },
    { "en": "Frekuensi Cutoff Filter SC Ditentukan Oleh Apa?", "id": "Rasio Kapasitansi Dan Frekuensi Clock." },
    { "en": "Apa Itu Filter DSP (Digital Signal Processor)?", "id": "Filter Yang Diimplementasikan Di DSP." },
    { "en": "Apa Itu Koefisien Kuantisasi?", "id": "Pembulatan Nilai Koefisien Filter." },
    { "en": "Apa Itu Batas Siklus (Limit Cycle)?", "id": "Osilasi Kecil Dalam Filter IIR." },
    { "en": "Apa Itu Overflow?", "id": "Hasil Perhitungan Melebihi Batas." },
    { "en": "Apa Itu Penskalaan (Scaling)?", "id": "Menyesuaikan Level Sinyal Internal." },
    { "en": "Apa Itu Arsitektur Direct Form?", "id": "Implementasi Langsung Fungsi Transfer." },
    { "en": "Apa Itu Arsitektur Transposed Form?", "id": "Struktur Alternatif Untuk Implementasi." },
    { "en": "Apa Itu Arsitektur State-Space?", "id": "Representasi Menggunakan Variabel Keadaan." },
    { "en": "Apa Itu Arsitektur Wave Digital Filter?", "id": "WDF (Wave Digital Filter)." },
    { "en": "Apa Keuntungan WDF (Wave Digital Filter)?", "id": "Sifat Insensitivitas Yang Baik." },
    { "en": "Apa Itu Filter Cermin Kuadratur?", "id": "QMF (Quadrature Mirror Filter)." },
    { "en": "Apa Itu Sub-band Coding?", "id": "Memisahkan Sinyal Ke Pita Frekuensi." },
    { "en": "Apa Itu Filter Brick-Wall?", "id": "Filter Ideal Dengan Transisi Seketika." },
    { "en": "Apakah Filter Brick-Wall Dapat Diwujudkan?", "id": "Tidak, Secara Fisik Tidak Mungkin." },
    { "en": "Apa Itu Kausalitas?", "id": "Output Tidak Bisa Mendahului Input." },
    { "en": "Apakah Filter Real-time Harus Kausal?", "id": "Ya, Harus." },
    { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Filter Linear." },
    { "en": "Apa Itu Passband Datar Maksimal?", "id": "Filter Butterworth." },
    { "en": "Apa Itu Phase-Linear?", "id": "Filter Bessel." },
    { "en": "Apa Itu Delay Konstan?", "id": "Sinonim Untuk Fasa Linear." },
    { "en": "Apa Itu Ripple Sama (Equiripple)?", "id": "Filter Chebyshev Dan Eliptik." },
    { "en": "Apa Itu Transformasi Z?", "id": "Alat Matematis Untuk Filter Digital." },
    { "en": "Apa Itu Transformasi Laplace?", "id": "Alat Matematis Untuk Filter Analog." },
    { "en": "Apa Itu Bidang-s (s-plane)?", "id": "Bidang Kompleks Untuk Analisis Laplace." },
    { "en": "Apa Itu Bidang-z (z-plane)?", "id": "Bidang Kompleks Untuk Analisis Transformasi Z." },
    { "en": "Apa Itu Lingkaran Satuan (Unit Circle)?", "id": "Batas Stabilitas Dalam Bidang-z." },
    { "en": "Di Mana Pole Harus Berada Untuk Stabilitas IIR?", "id": "Di Dalam Lingkaran Satuan." },
    { "en": "Apa Itu Jaringan T-Bridge?", "id": "Topologi Notch Filter Pasif." },
    { "en": "Apa Itu Filter Comb?", "id": "Filter Dengan Respon Seperti Sisir." },
    { "en": "Apa Fungsi Filter Comb?", "id": "Membuat Efek Gema Atau Flanger." },
    { "en": "Apa Itu Filter RLC Seri?", "id": "BPF (Band-Pass Filter) Atau BSF (Band-Stop Filter) Pasif." },
    { "en": "Apa Itu Filter RLC Paralel?", "id": "BPF (Band-Pass Filter) Atau BSF (Band-Stop Filter) Pasif." },
    { "en": "Apa Itu Sirkuit Resonansi?", "id": "Filter LC (Inductor-Capacitor) Sederhana." },
    { "en": "Apa Itu Sirkuit Anti-Resonansi?", "id": "Sirkuit LC Paralel." },
    { "en": "Apa Itu Faktor Disipasi?", "id": "Ukuran Kerugian Dalam Kapasitor." },
    { "en": "Apa Itu Koil Choke RF?", "id": "Induktor Yang Berfungsi Sebagai LPF." },
    { "en": "Apa Itu Kapasitor Blocking DC?", "id": "Kapasitor Yang Berfungsi Sebagai HPF." },
    { "en": "Apa Itu Jaringan Zobel?", "id": "Filter Untuk Menstabilkan Impedansi Speaker." },
    { "en": "Apa Itu Filter Bessel-Thomson?", "id": "Nama Lain Untuk Filter Bessel." },
    { "en": "Apa Itu Filter Gaussian?", "id": "Filter Dengan Respon Step Tanpa Overshoot." },
    { "en": "Apa Itu Filter Linkwitz-Riley?", "id": "Filter Crossover Populer." },
    { "en": "Berapa Atenuasi Crossover Linkwitz-Riley?", "id": "-6 dB Pada Frekuensi Crossover." },
    { "en": "Apa Keuntungan Filter Linkwitz-Riley?", "id": "Respon Fasa Cocok Antar Driver." },
    { "en": "Apa Itu Filter Orde Ganjil?", "id": "Filter Dengan Orde 1, 3, 5, dst." },
    { "en": "Apa Itu Filter Orde Genap?", "id": "Filter Dengan Orde 2, 4, 6, dst." },
    { "en": "Apa Itu Filter Sintesis Jaringan?", "id": "Desain Filter Berdasarkan Fungsi Matematis." },
    { "en": "Apa Itu Polinomial Butterworth?", "id": "Dasar Matematis Filter Butterworth." },
    { "en": "Apa Itu Polinomial Chebyshev?", "id": "Dasar Matematis Filter Chebyshev." },
    { "en": "Apa Itu Fungsi Rasional Eliptik?", "id": "Dasar Matematis Filter Eliptik." },
    { "en": "Apa Itu Realisasi Filter?", "id": "Proses Mengimplementasikan Fungsi Transfer." },
    { "en": "Apa Itu Realisasi Bentuk Langsung?", "id": "Implementasi Langsung Dari Koefisien." },
    { "en": "Apa Itu Realisasi Kaskade?", "id": "Implementasi Seri Dari Bagian Orde Dua." },
    { "en": "Apa Itu Realisasi Paralel?", "id": "Implementasi Paralel Dari Bagian Orde Dua." },
    { "en": "Apa Itu Struktur State-Space?", "id": "Representasi Menggunakan Matriks." },
    { "en": "Apa Itu Filter Waktu Kontinu?", "id": "Nama Lain Untuk Filter Analog." },
    { "en": "Apa Itu Filter Waktu Diskrit?", "id": "Nama Lain Untuk Filter Digital." },
    { "en": "Apa Itu Filter Invariant Skala?", "id": "Perilaku Sama Terlepas Skala." },
    { "en": "Apa Itu Filter Non-kausal?", "id": "Output Mendahului Input (Tidak Real-time)." },
    { "en": "Di Mana Filter Non-kausal Digunakan?", "id": "Pemrosesan Citra Dan Sinyal Offline." },
    { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Untuk Proses Filtering." },
    { "en": "Apa Itu Dekonvolusi?", "id": "Proses Membalikkan Efek Filter." },
    { "en": "Apa Itu Homomorphic Filtering?", "id": "Filtering Dalam Domain Berbeda." },
    { "en": "Apa Itu Filter Median?", "id": "Filter Non-Linear Untuk Menghilangkan Derau." },
    { "en": "Apa Itu Derau Salt-and-Pepper?", "id": "Derau Titik Hitam Putih Acak." },
    { "en": "Apa Itu Filter Anisotropik?", "id": "Filter Yang Beraksi Berbeda Arah." },
    { "en": "Apa Itu Filter Bilateral?", "id": "Mempertahankan Tepi Saat Menghaluskan." },
    { "en": "Apa Itu Unsharp Masking?", "id": "Teknik Penajaman Gambar." },
    { "en": "Apa Itu High-Boost Filtering?", "id": "Menekankan Frekuensi Tinggi Untuk Penajaman." },
    { "en": "Apa Itu Filter Emboss?", "id": "Filter Efek Visual Timbul." },
    { "en": "Apa Itu Filter Sobel?", "id": "Filter Deteksi Tepi." },
    { "en": "Apa Itu Filter Prewitt?", "id": "Filter Deteksi Tepi Lainnya." },
    { "en": "Apa Itu Filter Canny?", "id": "Algoritma Deteksi Tepi Multi-tahap." },
    { "en": "Apa Itu Laplacian of Gaussian (LoG)?", "id": "Filter Untuk Menemukan Area Perubahan Cepat." },
    { "en": "Apa Itu Filter Gabor?", "id": "Filter Terarah Untuk Analisis Tekstur." },
    { "en": "Apa Itu Domain Spasial?", "id": "Bekerja Langsung Pada Piksel Gambar." },
    { "en": "Apa Itu Domain Frekuensi?", "id": "Bekerja Pada Hasil Transformasi Fourier." },
    { "en": "Apa Itu Transformasi Fourier 2D?", "id": "Mengubah Gambar Ke Domain Frekuensi." },
    { "en": "Apa Itu Kernel Konvolusi?", "id": "Matriks Kecil Untuk Operasi Filtering." },
    { "en": "Apa Itu Padding Gambar?", "id": "Menambahkan Piksel Di Tepi Gambar." },
    { "en": "Apa Itu Filter Separable?", "id": "Kernel 2D Yang Dapat Dipisah." },
    { "en": "Apa Itu Histogram Gambar?", "id": "Distribusi Tingkat Kecerahan Piksel." },
    { "en": "Apa Itu Ekualisasi Histogram?", "id": "Meningkatkan Kontras Gambar." },
    { "en": "Apa Itu Morfologi Matematika?", "id": "Operasi Berbasis Bentuk (Shape)." },
    { "en": "Apa Itu Operasi Erosi?", "id": "Mengikis Batas Objek." },
    { "en": "Apa Itu Operasi Dilasi?", "id": "Memperluas Batas Objek." },
    { "en": "Apa Itu Operasi Opening?", "id": "Erosi Diikuti Dilasi (Menghaluskan)." },
    { "en": "Apa Itu Operasi Closing?", "id": "Dilasi Diikuti Erosi (Menutup Lubang)." },
    { "en": "Apa Itu Elemen Struktur?", "id": "Kernel Kecil Untuk Operasi Morfologi." },
    { "en": "Apa Itu Deteksi Objek?", "id": "Menemukan Objek Tertentu Dalam Gambar." },
    { "en": "Apa Itu Segmentasi Gambar?", "id": "Membagi Gambar Menjadi Beberapa Wilayah." },
    { "en": "Apa Itu Thresholding?", "id": "Mengubah Gambar Grayscale Menjadi Biner." },
    { "en": "Apa Itu Thresholding Adaptif?", "id": "Ambang Batas Berubah Di Seluruh Gambar." },
    { "en": "Apa Itu Restorasi Gambar?", "id": "Memperbaiki Gambar Yang Terdegradasi." },
    { "en": "Apa Itu Deblurring?", "id": "Menghilangkan Efek Buram." },
    { "en": "Apa Itu Denoising?", "id": "Menghilangkan Derau Dari Gambar." },
    { "en": "Apa Itu Inpainting?", "id": "Mengisi Bagian Gambar Yang Hilang." },
    { "en": "Apa Itu Super-Resolution?", "id": "Meningkatkan Resolusi Gambar." },
    { "en": "Apa Itu Filter Audio?", "id": "Filter Yang Bekerja Pada Sinyal Suara." },
    { "en": "Apa Itu Filter Video?", "id": "Filter Yang Bekerja Pada Sinyal Gambar." },
    { "en": "Apa Itu Filter Power Supply?", "id": "LPF (Low-Pass Filter) Untuk Meratakan Tegangan DC." },
    { "en": "Apa Itu Filter Jaringan Listrik?", "id": "Menghilangkan Derau Dari Jalur Listrik." },
    { "en": "Apa Itu Filter Selektivitas?", "id": "Kemampuan Memilih Frekuensi Yang Diinginkan." },
    { "en": "Apa Itu Filter Penolakan?", "id": "Kemampuan Menolak Frekuensi Tak Diinginkan." },
    { "en": "Apa Itu Atap Filter (Filter Skirt)?", "id": "Transisi Antara Passband Dan Stopband." },
    { "en": "Apa Itu Faktor Bentuk (Shape Factor)?", "id": "Rasio Bandwidth Stopband Per Passband." },
    { "en": "Apa Itu Ripple Stopband?", "id": "Variasi Atenuasi Di Dalam Stopband." },
    { "en": "Apa Itu Atenuasi Ultimate?", "id": "Tingkat Atenuasi Maksimum Di Stopband." },
    { "en": "Apa Itu Kedalaman Notch?", "id": "Atenuasi Maksimum Filter Notch." },
    { "en": "Apa Itu Penskalaan Amplitudo?", "id": "Menyesuaikan Level Gain Filter." },
    { "en": "Apa Itu Penskalaan Frekuensi?", "id": "Memindahkan Respon Filter Ke Frekuensi Baru." },
    { "en": "Apa Itu Implementasi Perangkat Keras Filter?", "id": "Menggunakan Komponen Fisik (Analog)." },
    { "en": "Apa Itu Implementasi Perangkat Lunak Filter?", "id": "Menggunakan Kode Program (Digital)." },
    { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
    { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
    { "en": "Apa Itu Efek Pemuatan (Loading Effect)?", "id": "Filter Mempengaruhi Sumber Sinyal." },
    { "en": "Bagaimana Filter Aktif Mengatasi Pemuatan?", "id": "Dengan Impedansi Input Tinggi." },
    { "en": "Apa Itu Impedansi Input?", "id": "Hambatan Yang Dilihat Oleh Sumber Sinyal." },
    { "en": "Apa Itu Impedansi Output?", "id": "Hambatan Internal Dari Output Filter." },
    { "en": "Impedansi Input Filter Aktif?", "id": "Sangat Tinggi." },
    { "en": "Impedansi Output Filter Aktif?", "id": "Sangat Rendah." },
    { "en": "Apa Itu Filter Pasif Tipe L?", "id": "Bentuk Sederhana LPF Atau HPF." },
    { "en": "Apa Itu Pembangkitan Derau (Noise Generation)?", "id": "Komponen Aktif Menambah Derau." },
    { "en": "Filter Mana Yang Menghasilkan Derau?", "id": "Filter Aktif." },
    { "en": "Apa Itu Rentang Dinamis?", "id": "Rasio Sinyal Terbesar Per Terkecil." },
    { "en": "Bagaimana Filter Aktif Mempengaruhi Rentang Dinamis?", "id": "Dibatasi Oleh Derau Dan Tegangan Catu." },
    { "en": "Apa Itu Distorsi?", "id": "Perubahan Bentuk Gelombang Yang Tidak Diinginkan." },
    { "en": "Apa Yang Menyebabkan Distorsi Di Filter Aktif?", "id": "Keterbatasan Op-Amp (Slew Rate, Clipping)." },
    { "en": "Apa Itu Clipping?", "id": "Pemotongan Sinyal Akibat Batas Tegangan." },
    { "en": "Apa Itu Filter Orde Campuran?", "id": "Menggabungkan Orde Berbeda (Contoh: Orde 3)." },
    { "en": "Bagaimana Membuat Filter Orde Ketiga?", "id": "Menggabungkan Orde Pertama Dan Kedua." },
    { "en": "Apa Itu Tanggapan Frekuensi Ideal?", "id": "Passband Datar, Transisi Seketika." },
    { "en": "Apa Itu Tanggapan Fasa Ideal?", "id": "Fasa Linear (Penundaan Konstan)." },
    { "en": "Apa Itu Aproksimasi Filter?", "id": "Pendekatan Matematis (Butterworth, Chebyshev)." },
    { "en": "Apa Itu Frekuensi Normalisasi?", "id": "Frekuensi Dinyatakan Relatif Terhadap Cutoff." },
    { "en": "Apa Itu Filter Invers Chebyshev?", "id": "Riak Di Stopband, Datar Di Passband." },
    { "en": "Apa Itu Filter Papoulis-Legendre?", "id": "Filter Monotonik Dengan Roll-off Tajam." },
    { "en": "Apa Itu Filter Hourglass?", "id": "Filter Dengan Respon Fasa Linear." },
    { "en": "Apa Itu Penolakan Desibel (dB)?", "id": "Ukuran Atenuasi Filter." },
    { "en": "Apa Itu Plot Pole-Zero?", "id": "Visualisasi Karakteristik Filter Di Bidang-s." },
    { "en": "Apa Efek Memindahkan Pole Lebih Jauh?", "id": "Meningkatkan Frekuensi Cutoff." },
    { "en": "Apa Efek Mengubah Sudut Pole?", "id": "Mengubah Faktor Q (Kualitas)." },
    { "en": "Apa Itu Filter OTA-C?", "id": "Filter Menggunakan OTA (Operational Transconductance Amplifier) Dan Kapasitor." },
    { "en": "Apa Keuntungan Filter OTA-C?", "id": "Frekuensi Cutoff Terkontrol Elektronik." },
    { "en": "Apa Itu Filter Konveyor Arus?", "id": "Menggunakan Konveyor Arus Sebagai Blok Aktif." },
    { "en": "Apa Itu Penskalaan?", "id": "Mengubah Nilai Komponen Dan Frekuensi." },
    { "en": "Apa Itu Penskalaan Magnitudo?", "id": "Mengubah Level Impedansi." },
    { "en": "Apa Itu Penskalaan Frekuensi?", "id": "Memindahkan Respon Ke Frekuensi Baru." },
    { "en": "Apa Itu Topologi?", "id": "Struktur Atau Konfigurasi Rangkaian." },
    { "en": "Apa Itu Sintesis?", "id": "Menurunkan Rangkaian Dari Fungsi Matematis." },
    { "en": "Apa Itu Analisis?", "id": "Menentukan Perilaku Rangkaian Yang Ada." },
    { "en": "Apa Itu Fungsi Transfer Tegangan?", "id": "Rasio Tegangan Output Per Tegangan Input." },
    { "en": "Apa Itu Jaringan Dua Port?", "id": "Jaringan Dengan Input Dan Output." },
    { "en": "Apa Itu Parameter-Y?", "id": "Parameter Admitansi." },
    { "en": "Apa Itu Parameter-Z?", "id": "Parameter Impedansi." },
    { "en": "Apa Itu Parameter-H?", "id": "Parameter Hibrida." },
    { "en": "Apa Itu Parameter-ABCD?", "id": "Parameter Transmisi." },
    { "en": "Apa Itu Jaringan Resiprokal?", "id": "Perilaku Sama Jika Input-Output Ditukar." },
    { "en": "Apa Itu Jaringan Simetris?", "id": "Impedansi Input Sama Dengan Output." },
    { "en": "Apa Itu Titik Setengah Daya?", "id": "Nama Lain Untuk Frekuensi Cutoff -3dB." },
    { "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Osilasi Natural." },
    { "en": "Apa Itu Frekuensi Anti-Resonansi?", "id": "Frekuensi Dengan Impedansi Maksimum." },
    { "en": "Apa Itu Resonator?", "id": "Sirkuit Dengan Respon Frekuensi Tajam." },
    { "en": "Apa Itu Sirkuit Penjebak (Trap Circuit)?", "id": "Filter Notch Untuk Menghilangkan Interferensi." },
    { "en": "Apa Itu Filter Harmonik?", "id": "Menekan Frekuensi Harmonik Yang Tidak Diinginkan." },
    { "en": "Apa Itu Filter Input?", "id": "Filter Di Bagian Masukan Sistem." },
    { "en": "Apa Itu Filter Output?", "id": "Filter Di Bagian Keluaran Sistem." },
    { "en": "Apa Itu Filter Penapisan (Smoothing Filter)?", "id": "LPF (Low-Pass Filter) Untuk Mengurangi Riak." },
    { "en": "Apa Itu Rangkaian Diferensiator?", "id": "Berperilaku Seperti HPF (High-Pass Filter)." },
    { "en": "Apa Itu Rangkaian Integrator?", "id": "Berperilaku Seperti LPF (Low-Pass Filter)." },
    { "en": "Apa Itu Jaringan Kompensasi Lag?", "id": "Meningkatkan Stabilitas Sistem Kontrol." },
    { "en": "Apa Itu Jaringan Kompensasi Lead?", "id": "Meningkatkan Kecepatan Respon Sistem." },
    { "en": "Apa Itu Filter Pembatas Pita (Band-limiting)?", "id": "Membatasi Sinyal Dalam Bandwidth Tertentu." },
    { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Memiliki Energi Sama Di Semua Frekuensi." },
    { "en": "Apa Itu Derau Merah Jambu (Pink Noise)?", "id": "Energi Sama Per Oktaf." },
    { "en": "Apa Itu Derau Biru (Blue Noise)?", "id": "Energi Meningkat Dengan Frekuensi." },
    { "en": "Apa Itu Derau Ungu (Violet Noise)?", "id": "Energi Meningkat Lebih Cepat Dari Biru." },
    { "en": "Apa Itu Derau Coklat (Brownian Noise)?", "id": "Energi Menurun Cepat Dengan Frekuensi." },
    { "en": "Apa Itu Filter Pemutih (Whitening Filter)?", "id": "Membuat Spektrum Sinyal Menjadi Datar." },
    { "en": "Apa Itu Filter Pewarna (Coloring Filter)?", "id": "Membentuk Spektrum Derau." },
    { "en": "Apa Itu Jalur Umpan Maju (Feedforward)?", "id": "Jalur Sinyal Langsung." },
    { "en": "Apa Itu Jalur Umpan Balik (Feedback)?", "id": "Jalur Sinyal Dari Output Kembali Ke Input." },
    { "en": "Apa Itu Umpan Balik Negatif?", "id": "Menstabilkan Sistem." },
    { "en": "Apa Itu Umpan Balik Positif?", "id": "Dapat Menyebabkan Osilasi." },
    { "en": "Apa Itu Kondisi Osilasi?", "id": "Gain Loop Sama Dengan 1, Fasa 0." },
    { "en": "Apa Itu Kriteria Stabilitas Nyquist?", "id": "Metode Grafis Untuk Menganalisis Stabilitas." },
    { "en": "Apa Itu Routh-Hurwitz Criterion?", "id": "Metode Matematis Untuk Stabilitas." },
    { "en": "Apa Itu Loop Gain?", "id": "Total Penguatan Di Dalam Loop Umpan Balik." },
    { "en": "Apa Itu Filter Audio?", "id": "Digunakan Pada Frekuensi Suara." },
    { "en": "Apa Itu Filter Video?", "id": "Digunakan Pada Frekuensi Gambar." },
    { "en": "Apa Itu Filter RF (Radio Frequency)?", "id": "Digunakan Pada Frekuensi Radio." },
    { "en": "Apa Itu Filter IF (Intermediate Frequency)?", "id": "Filter Sangat Selektif Di Penerima." },
    { "en": "Apa Itu Filter Power Line?", "id": "Menekan Derau Dari Jaringan Listrik." },
    { "en": "Apa Itu Filter Data?", "id": "Membentuk Pulsa Atau Menghilangkan Jitter." },
    { "en": "Apa Itu Filter Fasa?", "id": "Nama Lain Untuk Filter All-Pass." },
    { "en": "Apa Itu Jaringan Pembentuk Pulsa?", "id": "Menghasilkan Bentuk Pulsa Tertentu." },
    { "en": "Apa Itu Jaringan Penunda (Delay Network)?", "id": "Memperlambat Perambatan Sinyal." },
    { "en": "Apa Itu Ekualiser Penundaan?", "id": "Mengkompensasi Variasi Penundaan Grup." },
    { "en": "Apa Itu Penolakan Desibel (dB) Stopband?", "id": "Seberapa Efektif Filter Menolak." },
    { "en": "Apa Itu Lebar Transisi?", "id": "Rentang Frekuensi Antara Passband-Stopband." },
    { "en": "Bagaimana Q Factor Mempengaruhi Respon Step?", "id": "Q Tinggi Dapat Menyebabkan Ringing." },
    { "en": "Apa Itu Frekuensi Sudut?", "id": "Frekuensi Dalam Radian Per Detik (ω)." },
    { "en": "Apa Hubungan ω Dan f?", "id": "ω = 2πf." },
    { "en": "Apa Itu Kondisi Awal?", "id": "Tegangan Kapasitor/Arus Induktor Awal." },
    { "en": "Apa Itu Respon Keadaan Tunak?", "id": "Perilaku Filter Setelah Waktu Lama." },
    { "en": "Apa Itu Respon Transien?", "id": "Perilaku Awal Filter." },
    { "en": "Apa Itu Konstanta Waktu?", "id": "Ukuran Kecepatan Respon Filter." },
    { "en": "Apa Itu Filter Waktu Nyata?", "id": "Memproses Sinyal Saat Diterima." },
    { "en": "Apa Itu Filter Offline?", "id": "Memproses Sinyal Yang Sudah Direkam." },
    { "en": "Apa Itu Filter Kausal?", "id": "Output Hanya Bergantung Pada Input Masa Lalu." },
    { "en": "Filter Analog Selalu Bersifat Apa?", "id": "Kausal." },
    { "en": "Apa Itu Zero-Phase Filter?", "id": "Filter Tanpa Pergeseran Fasa (Non-kausal)." },
    { "en": "Bagaimana Mencapai Zero-Phase Filtering?", "id": "Memfilter Maju Dan Mundur (Offline)." },
    { "en": "Apa Itu Kompensasi Digital?", "id": "Menggunakan Filter Digital Memperbaiki Sistem." },
    { "en": "Apa Itu Filter Moving Average (MA)?", "id": "Jenis Filter FIR Sederhana." },
    { "en": "Apa Itu Filter Auto-Regressive (AR)?", "id": "Jenis Filter IIR Sederhana." },
    { "en": "Apa Itu Filter ARMA?", "id": "Gabungan Filter AR Dan MA." },
    { "en": "Apa Itu Model State-Space?", "id": "Representasi Matematis Sistem Dinamis." },
    { "en": "Apa Itu Observabilitas?", "id": "Kemampuan Mengamati Keadaan Internal." },
    { "en": "Apa Itu Kontrolabilitas?", "id": "Kemampuan Mengontrol Keadaan Internal." },
    { "en": "Apa Itu Filter Pita Frekuensi Audio?", "id": "Graphic Equalizer." },
    { "en": "Apa Itu Filter Parametrik?", "id": "Filter Dengan Kontrol Frekuensi, Q, Gain." },
    { "en": "Apa Itu Filter Resonansi Tegangan Terkontrol?", "id": "VCF (Voltage-Controlled Filter) Pada Synthesizer." },
    { "en": "Apa Itu Bank Filter?", "id": "Kumpulan Filter Paralel." },
    { "en": "Apa Itu Vocoder?", "id": "Analisis Dan Sintesis Suara Manusia." },
    { "en": "Apa Itu Jaringan Pembentuk (Shaping Network)?", "id": "Filter Untuk Membentuk Spektrum Sinyal." },
    { "en": "Apa Itu Jaringan Pemberat (Weighting Network)?", "id": "Filter Yang Meniru Persepsi Manusia." },
    { "en": "Contoh Jaringan Pemberat?", "id": "A-Weighting, C-Weighting (Untuk Suara)." },
    { "en": "Apa Itu Filter Digital Adaptif?", "id": "Koefisien Filter Berubah Seiring Waktu." },
    { "en": "Apa Itu Algoritma LMS (Least Mean Squares)?", "id": "Algoritma Untuk Filter Adaptif." },
    { "en": "Apa Aplikasi Utama Filter Adaptif?", "id": "Pembatalan Gema (Echo Cancellation)." },
    { "en": "Apa Itu Pembatalan Derau?", "id": "Mengurangi Derau Latar Belakang." },
    { "en": "Apa Itu Filter Diferensial?", "id": "Melewatkan Perbedaan Antara Dua Sinyal." },
    { "en": "Apa Itu Filter Mode Umum (Common-mode)?", "id": "Menolak Sinyal Yang Sama Di Kedua Input." },
    { "en": "Apa Itu Filter Saluran (Channel Filter)?", "id": "Filter Seleksi Kanal Di Penerima." },
    { "en": "Apa Itu Filter Atap (Roofing Filter)?", "id": "Filter Sangat Selektif Di Awal IF." },
    { "en": "Apa Itu Filter Kristal Monolitik?", "id": "Filter Kristal Terintegrasi." },
    { "en": "Apa Itu Pasangan Pole-Zero?", "id": "Kombinasi Pole Dan Zero." },
    { "en": "Apa Itu Kompensasi Dominan-Pole?", "id": "Menstabilkan Penguat Dengan LPF Internal." },
    { "en": "Apa Itu Laju Atentuasi Asimtotik?", "id": "Roll-off Maksimum Di Frekuensi Tinggi." },
    { "en": "Apa Itu Prototipe LPF (Low-Pass Filter) Normalisasi?", "id": "Desain Dasar Sebelum Penskalaan." },
    { "en": "Apa Itu Transformasi Impedansi?", "id": "Mengubah Filter Dari Satu Tipe Ke Tipe Lain." },
    { "en": "Apa Itu Jaringan Cauer?", "id": "Sintesis Filter Pasif." },
    { "en": "Apa Itu Jaringan Foster?", "id": "Sintesis Lain Untuk Filter Pasif." },
    { "en": "Apa Itu Fungsi Transfer Biquadratic?", "id": "Fungsi Orde Dua." },
    { "en": "Apa Itu Sensitivitas Pasif?", "id": "Sensitivitas Filter Pasif." },
    { "en": "Apa Itu Sensitivitas Aktif?", "id": "Sensitivitas Karena Komponen Aktif." },
    { "en": "Apa Itu Gain-Sensitivitas Produk?", "id": "Ukuran Kinerja Filter Aktif." },
    { "en": "Apa Itu Simulasi SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Simulasi Rangkaian Elektronik." },
    { "en": "Apa Itu Analisis AC (Alternating Current)?", "id": "Simulasi Respon Frekuensi." },
    { "en": "Apa Itu Analisis Transien?", "id": "Simulasi Respon Terhadap Waktu." },
    { "en": "Apa Itu Analisis Derau?", "id": "Simulasi Kinerja Derau Filter." },
    { "en": "Apa Itu Derau Flicker (1/f Noise)?", "id": "Derau Frekuensi Rendah." },
    { "en": "Apa Itu Derau Termal (Johnson Noise)?", "id": "Derau Dari Agitasi Termal." },
    { "en": "Apa Itu Filter Aktif Tanpa Induktor?", "id": "Keuntungan Utama Dibanding Pasif." },
    { "en": "Apa Itu Filter Diskrit Waktu?", "id": "Filter Digital Atau Switched-Capacitor." },
    { "en": "Apa Itu Filter Berbasis DSP (Digital Signal Processor)?", "id": "Implementasi Filter Dalam DSP." },
    { "en": "Apa Itu Aritmetika Titik Tetap (Fixed-point)?", "id": "Representasi Bilangan Dengan Presisi Tetap." },
    { "en": "Apa Itu Aritmetika Titik Mengambang (Floating-point)?", "id": "Representasi Bilangan Dengan Presisi Variabel." },
    { "en": "Apa Efek Presisi Terbatas?", "id": "Menyebabkan Error Kuantisasi Koefisien." },
    { "en": "Apa Itu Stabilitas Numerik?", "id": "Ketahanan Algoritma Terhadap Error." },
    { "en": "Apa Itu Filter Hilbert?", "id": "Filter All-pass Penggeser Fasa 90°." },
    { "en": "Apa Aplikasi Filter Hilbert?", "id": "Membangkitkan Sinyal Analitik (SSB)." },
    { "en": "Apa Itu Sinyal Analitik?", "id": "Sinyal Bernilai Kompleks." },
    { "en": "Apa Itu Filter Turunan (Differentiator)?", "id": "Responnya Sebanding Dengan Frekuensi." },
    { "en": "Apa Itu Filter Integral (Integrator)?", "id": "Responnya Berbanding Terbalik Frekuensi." },
    { "en": "Apa Itu Desain Berbantuan Komputer?", "id": "CAD (Computer-Aided Design) Untuk Filter." },
    { "en": "Apa Itu Efek Memori?", "id": "Output Tergantung Input Masa Lalu." },
    { "en": "Filter Mana Yang Punya Memori?", "id": "Semua Filter Praktis." },
    { "en": "Apa Itu Orde Relatif?", "id": "Selisih Antara Jumlah Pole-Zero." },
    { "en": "Apa Itu Filter Dengan Respon Fasa Nol?", "id": "Filter Non-kausal (Offline)." },
    { "en": "Apa Itu Filter Cermin Kuadratur?", "id": "QMF (Quadrature Mirror Filter)." },
    { "en": "Apa Itu Bank Filter?", "id": "Kumpulan Filter Untuk Analisis Spektral." },
    { "en": "Apa Itu Filter Wavelet?", "id": "Filter Untuk Analisis Waktu-Frekuensi." },
    { "en": "Apa Itu Filter Optik?", "id": "Filter Yang Bekerja Pada Cahaya." },
    { "en": "Contoh Filter Optik?", "id": "Filter Interferensi, Filter Warna." },
    { "en": "Apa Itu Filter Fabry-Pérot?", "id": "Resonator Optik." },
    { "en": "Apa Itu Filter Akustik?", "id": "Filter Yang Bekerja Pada Suara." },
    { "en": "Contoh Filter Akustik?", "id": "Knalpot Mobil, Ruang Konser." },
    { "en": "Apa Itu Filter Resonator Akustik Permukaan?", "id": "SAW (Surface Acoustic Wave) Filter." },
    { "en": "Apa Itu Filter Resonator Akustik Bulk?", "id": "BAW (Bulk Acoustic Wave) Filter." },
    { "en": "Apa Itu Filter Anti-Refleksi?", "id": "Lapisan Tipis Pada Lensa." },
    { "en": "Apa Itu Filter Polarisasi?", "id": "Melewatkan Cahaya Dengan Polarisasi Tertentu." },
    { "en": "Apa Itu Filter Spasial?", "id": "Filter Yang Bekerja Pada Pola Spasial." },
    { "en": "Apa Itu Apertur?", "id": "Contoh Filter Spasial Lolos Rendah." },
    { "en": "Apa Itu Filter Penajaman?", "id": "Filter Lolos Tinggi Dalam Pemrosesan Citra." },
    {"en": "Apa Itu Filter Penghalusan?", "id": "Filter Lolos Rendah Dalam Pemrosesan Citra." },
    { "en": "Apa Itu Konvolusi 2D?", "id": "Operasi Matematis Filter Gambar." },
    { "en": "Apa Itu Kernel Filter?", "id": "Matriks Koefisien Filter Gambar." },
    { "en": "Apa Itu Filter Rata-rata?", "id": "Filter Penghalusan Sederhana." },
    { "en": "Apa Itu Filter Gaussian 2D?", "id": "Filter Penghalusan Dengan Kernel Gaussian." },
    { "en": "Apa Itu Filter Laplacian?", "id": "Filter Deteksi Tepi." },
    { "en": "Apa Itu Filter Penolakan Pita?", "id": "BSF (Band-Stop Filter)." },
    { "en": "Apa Itu Penolakan Dengung (Hum)?", "id": "Aplikasi Umum Filter Notch." },
    { "en": "Apa Itu Catu Daya Tanpa Regulasi?", "id": "Tegangan Berubah Sesuai Beban." },
    { "en": "Apa Itu Filter Jalur Listrik?", "id": "Mengurangi EMI Dari Jalur Listrik." },
    { "en": "Apa Itu Filter Kapasitor Input?", "id": "Mengurangi Riak Pada Input Penyearah." },
    { "en": "Apa Itu Filter Choke Input?", "id": "Filter LPF Menggunakan Induktor." },
    { "en": "Apa Itu Filter CLC (Capacitor-Inductor-Capacitor)?", "id": "Filter Pi (π) Untuk Catu Daya." },
    { "en": "Apa Itu Filter LDR (Light Dependent Resistor)?", "id": "Filter Terkontrol Cahaya." },
    { "en": "Apa Itu Filter Tegangan Terkontrol?", "id": "Frekuensi Cutoff Tergantung Tegangan." },
    { "en": "Apa Itu Implementasi Filter Aktif?", "id": "Menggunakan Op-Amp." },
    { "en": "Apa Itu Batas Stabilitas?", "id": "Batas Di Mana Sistem Mulai Berosilasi." },
    { "en": "Apa Itu Gain Margin?", "id": "Ukuran Seberapa Jauh Dari Ketidakstabilan." },
    { "en": "Apa Itu Phase Margin?", "id": "Ukuran Lain Kestabilan Sistem." },
    { "en": "Apa Itu Jaringan Kompensasi?", "id": "Filter Untuk Menstabilkan Loop Kontrol." },
    { "en": "Apa Itu Filter Kalman Diperluas?", "id": "EKF (Extended Kalman Filter)." },
    { "en": "Apa Itu Filter Kalman Tanpa Aroma?", "id": "UKF (Unscented Kalman Filter)." },
    { "en": "Apa Itu Filter Partikel?", "id": "Filter Monte Carlo Sekuensial." },
    { "en": "Apa Itu Selektivitas?", "id": "Kemampuan Filter Memilih Frekuensi." },
    { "en": "Apa Itu Sensitivitas?", "id": "Perubahan Output Akibat Perubahan Komponen." },
    { "en": "Apa Itu Linearitas Fasa?", "id": "Penundaan Konstan Untuk Semua Frekuensi." },
    { "en": "Apa Itu Efek Tepi?", "id": "Distorsi Di Awal/Akhir Sinyal." },
    { "en": "Apa Itu Zero Padding?", "id": "Menambahkan Nol Ke Sinyal." },
    { "en": "Apa Itu Transformasi Diskrit Fourier?", "id": "DFT (Discrete Fourier Transform)." },
    { "en": "Apa Itu Algoritma Cepat Untuk DFT?", "id": "FFT (Fast Fourier Transform)." },
    { "en": "Apa Itu Kebocoran Spektral?", "id": "Penyebaran Energi Ke Frekuensi Lain." },
    { "en": "Apa Itu Resolusi Frekuensi?", "id": "Jarak Antara Titik Frekuensi FFT." },
    { "en": "Apa Itu Filter Digital Multirate?", "id": "Mengubah Laju Sampel Sinyal." },
    { "en": "Apa Itu Downsampling?", "id": "Mengurangi Laju Sampel (Desimasi)." },
    { "en": "Apa Itu Upsampling?", "id": "Meningkatkan Laju Sampel (Interpolasi)." },
    { "en": "Apa Itu Filter Polyphase?", "id": "Implementasi Efisien Filter Multirate." },
    { "en": "Apa Itu Filter Notch Adaptif?", "id": "Filter Notch Yang Melacak Frekuensi." },
    { "en": "Apa Itu Filter Equalizer?", "id": "Mengkompensasi Distorsi Kanal." },
    { "en": "Apa Itu Filter Audio Crossover?", "id": "Membagi Frekuensi Untuk Speaker." },
    { "en": "Apa Itu Filter Crossover Orde Pertama?", "id": "Roll-off 6 dB/oktaf." },
    { "en": "Apa Itu Filter Crossover Orde Kedua?", "id": "Roll-off 12 dB/oktaf." },
    { "en": "Apa Itu Filter Crossover Orde Keempat?", "id": "Roll-off 24 dB/oktaf." },
    { "en": "Apa Tipe Alignment Crossover?", "id": "Butterworth, Linkwitz-Riley." },
    { "en": "Apa Itu Jaringan Zobel?", "id": "Filter Kompensasi Impedansi." },
    { "en": "Apa Itu Koreksi Baffle Step?", "id": "Filter Untuk Mengkompensasi Efek Baffle." },
    { "en": "Apa Itu Jaringan Kontur?", "id": "Filter Untuk Membentuk Respon Frekuensi." },
    { "en": "Apa Itu L-Pad?", "id": "Attenuator Yang Menjaga Impedansi." },
    { "en": "Apa Itu Filter Subsonik?", "id": "HPF (High-Pass Filter) Untuk Menghilangkan Frekuensi Rendah." },
    { "en": "Apa Itu Filter RIAA (Recording Industry Association of America)?", "id": "Kurva Ekualisasi Piringan Hitam." },
    { "en": "Apa Itu Filter Rumble?", "id": "Menghilangkan Derau Frekuensi Rendah Turntable." },
    { "en": "Apa Itu Filter Scratch?", "id": "Menghilangkan Derau Frekuensi Tinggi Goresan." },
    { "en": "Apa Itu Topologi Rauch?", "id": "Jenis Filter Lolos Rendah Aktif." },
    { "en": "Apa Itu Filter Berbasis Girator?", "id": "Mensimulasikan Induktor Dengan Op-Amp." },
    { "en": "Apa Itu Filter UAF (Universal Active Filter)?", "id": "IC Filter Universal." },
    { "en": "Apa Itu Filter Elips?", "id": "Nama Lain Untuk Filter Cauer." },
    { "en": "Apa Itu Filter Monotonik?", "id": "Filter Tanpa Riak Di Passband." },
    { "en": "Contoh Filter Monotonik?", "id": "Butterworth Dan Bessel." },
    { "en": "Apa Itu Atentuasi Desibel (dB)?", "id": "Ukuran Pelemahan Sinyal." },
    { "en": "Apa Itu Plot Magnitudo?", "id": "Bagian Dari Bode Plot (Gain)." },
    { "en": "Apa Itu Plot Fasa?", "id": "Bagian Dari Bode Plot (Fasa)." },
    { "en": "Apa Itu Frekuensi Pojok (Corner Frequency)?", "id": "Nama Lain Untuk Frekuensi Cutoff." },
    { "en": "Apa Itu Asimtot?", "id": "Garis Lurus Pendekatan Kurva Respon." },
    { "en": "Apa Itu Fasa Minimum?", "id": "Sistem Tanpa Penundaan Berlebih." },
    { "en": "Apa Itu Fasa Non-Minimum?", "id": "Sistem Dengan Zero Di Sisi Kanan." },
    { "en": "Apa Itu Penundaan Amplop?", "id": "Sinonim Untuk Penundaan Grup." },
    { "en": "Apa Itu Distorsi Fasa Linear?", "id": "Pergeseran Waktu Konstan." },
    { "en": "Apa Itu Distorsi Fasa Non-Linear?", "id": "Menyebabkan Distorsi Bentuk Gelombang." },
    { "en": "Apa Itu Filter Penolakan Kembar-T?", "id": "Filter Notch Twin-T." },
    { "en": "Apa Itu Filter Jembatan Wien?", "id": "Filter Lolos Pita Jembatan Wien." },
    { "en": "Apa Itu Filter Chebyshev Tipe I?", "id": "Riak Di Passband, Datar Di Stopband." },
    { "en": "Apa Itu Filter Chebyshev Tipe II?", "id": "Datar Di Passband, Riak Di Stopband." },
    { "en": "Apa Itu Filter Eliptik?", "id": "Riak Di Passband Dan Stopband." },
    { "en": "Apa Itu Aproksimasi Butterworth?", "id": "Aproksimasi Datar Maksimal." },
    { "en": "Apa Itu Aproksimasi Chebyshev?", "id": "Aproksimasi Riak Sama (Equiripple)." },
    { "en": "Apa Itu Aproksimasi Bessel?", "id": "Aproksimasi Penundaan Grup Datar Maksimal." },
    { "en": "Apa Itu Penolakan Desibel (dB) Per Oktaf?", "id": "Ukuran Kemiringan Roll-off." },
    { "en": "Apa Itu Penolakan Desibel (dB) Per Dekade?", "id": "Ukuran Lain Kemiringan Roll-off." },
    { "en": "Apa Itu Noise Bandwidth?", "id": "Bandwidth Ekuivalen Untuk Derau Putih." },
    { "en": "Apa Itu Power-Bandwidth?", "id": "Bandwidth Frekuensi Penuh." },
    { "en": "Apa Itu Filter Analog Dapat Diprogram?", "id": "Filter Dengan Karakteristik Terkontrol Digital." },
    { "en": "Apa Itu Filter Reluktansi Variabel?", "id": "Filter Dengan Komponen Variabel." },
    { "en": "Apa Itu Filter Berbasis Mikrokontroler?", "id": "Filter Digital Diimplementasikan Di Mikrokontroler." },
    { "en": "Apa Itu Filter Pasif Terkopel?", "id": "Filter Dengan Beberapa Resonator." },
    { "en": "Apa Itu Filter Helikal?", "id": "Filter RF Berbentuk Heliks." },
    { "en": "Apa Itu Filter Interdigital?", "id": "Filter Microwave Planar." },
    { "en": "Apa Itu Filter Rambut Sisir (Hairpin)?", "id": "Filter Mikrostrip Berbentuk Jepit Rambut." },
    { "en": "Apa Itu Filter Resonator Celah-Cincin?", "id": "SRR (Split-Ring Resonator) Filter." },
    { "en": "Apa Itu Filter Metamaterial?", "id": "Menggunakan Struktur Buatan Sub-panjang gelombang." },
    { "en": "Apa Itu Filter Fotonik?", "id": "Filter Yang Bekerja Pada Sinyal Optik." },
    { "en": "Apa Itu Filter Kisi Serat Bragg?", "id": "FBG (Fiber Bragg Grating)." },
    { "en": "Apa Itu Filter Arrayed Waveguide Grating?", "id": "AWG (Arrayed Waveguide Grating)." },
    { "en": "Apa Itu Fabry-Pérot Interferometer?", "id": "Resonator Optik." },
    { "en": "Apa Itu Filter Lyot?", "id": "Filter Birefringent Optik." },
    { "en": "Apa Itu Filter Akusto-Optik Dapat Disetel?", "id": "AOTF (Acousto-Optic Tunable Filter)." },
    { "en": "Apa Itu Filter MEMS (Micro-Electro-Mechanical Systems)?", "id": "Filter Mekanis Mikroskopis." },
    { "en": "Apa Itu FBAR (Film Bulk Acoustic Resonator)?", "id": "Nama Lain Untuk Filter BAW." },
    { "en": "Apa Itu Penguat Umpan Balik?", "id": "Penguat Dengan Jalur Feedback." },
    { "en": "Apa Itu Penguat Transimpedansi?", "id": "TIA (Transimpedance Amplifier)." },
    { "en": "Apa Aplikasi TIA (Transimpedance Amplifier)?", "id": "Penguat Untuk Fotodioda." },
    { "en": "Apa Itu Kestabilan?", "id": "Kondisi Tanpa Osilasi." },
    { "en": "Apa Itu Kompensasi Frekuensi?", "id": "Menjamin Kestabilan Penguat." },
    { "en": "Apa Itu Jalur Distribusi Daya?", "id": "PDN (Power Distribution Network)." },
    { "en": "Apa Fungsi Kapasitor Decoupling?", "id": "Filter Lolos Rendah Untuk PDN." },
    { "en": "Apa Itu Impedansi Target?", "id": "Impedansi Maksimum PDN Yang Diizinkan." },
    { "en": "Apa Itu Resonansi PDN (Power Distribution Network)?", "id": "Puncak Impedansi Yang Tidak Diinginkan." },
    { "en": "Apa Itu Peredaman Resonansi?", "id": "Menekan Puncak Impedansi PDN." },
    { "en": "Apa Itu Filter Notch Aktif?", "id": "Menggunakan Op-Amp Untuk Notch Tajam." },
    { "en": "Apa Itu Filter Gyrator?", "id": "Menggunakan Op-Amp Mensimulasikan Induktor." },
    { "en": "Apa Itu Filter Bi-kuadratik?", "id": "Filter Orde Dua." },
    { "en": "Apa Itu Jaringan T-Bridge Ganda?", "id": "Dasar Dari Osilator Jembatan Wien." },
    { "en": "Apa Itu Filter Orde Keempat?", "id": "Roll-off -80 dB/dekade." },
    { "en": "Apa Itu Orde Filter Crossover?", "id": "Menentukan Kemiringan Pembagian Frekuensi." },
    { "en": "Apa Itu Filter Normalisasi?", "id": "Desain Dengan fc = 1 rad/s." },
    { "en": "Apa Itu Plot Respon Frekuensi?", "id": "Visualisasi Perilaku Filter." },
    { "en": "Apa Itu Filter Chebyshev Terbalik?", "id": "Nama Lain Untuk Tipe II." },
    { "en": "Apa Itu Respon Monotonik?", "id": "Selalu Turun, Tidak Pernah Naik." },
    { "en": "Apa Itu Filter Digital?", "id": "Operasi Matematis Pada Data Sampel." },
    { "en": "Apa Itu Transformasi-Z?", "id": "Alat Analisis Filter Digital." },
    { "en": "Apa Itu Respon Impuls?", "id": "Output Filter Untuk Input Impuls." },
    { "en": "Apa Itu Panjang Respon Impuls?", "id": "Menentukan Tipe FIR Atau IIR." },
    { "en": "Apa Itu Filter FIR Fasa Linear?", "id": "Koefisien Simetris." },
    { "en": "Apa Itu Desain Jendela?", "id": "Metode Sederhana Mendesain Filter FIR." },
    { "en": "Apa Itu Metode Penskalaan Frekuensi?", "id": "Metode Desain Filter IIR." },
    { "en": "Apa Itu Koefisien Filter?", "id": "Angka Yang Mendefinisikan Filter." },
    { "en": "Apa Itu Aritmetika Kuantisasi?", "id": "Efek Dari Presisi Terbatas." },
    { "en": "Apa Itu Batas Stabilitas?", "id": "Batas Dimana Sistem Mulai Berosilasi." },
    { "en": "Apa Itu Gain Margin?", "id": "Ukuran Seberapa Jauh Dari Ketidakstabilan." },
    { "en": "Apa Itu Phase Margin?", "id": "Ukuran Lain Kestabilan Sistem." },
    { "en": "Apa Itu Jaringan Kompensasi?", "id": "Filter Untuk Menstabilkan Loop Kontrol." },
    { "en": "Apa Itu Filter Kalman Diperluas?", "id": "EKF (Extended Kalman Filter)." },
    { "en": "Apa Itu Filter Kalman Tanpa Aroma?", "id": "UKF (Unscented Kalman Filter)." },
    { "en": "Apa Itu Filter Partikel?", "id": "Filter Monte Carlo Sekuensial." },
    { "en": "Apa Itu Selektivitas?", "id": "Kemampuan Filter Memilih Frekuensi." },
    { "en": "Apa Itu Sensitivitas?", "id": "Perubahan Output Akibat Perubahan Komponen." },
    { "en": "Apa Itu Linearitas Fasa?", "id": "Penundaan Konstan Untuk Semua Frekuensi." },
    { "en": "Apa Itu Efek Tepi?", "id": "Distorsi Di Awal/Akhir Sinyal." },
    { "en": "Apa Itu Zero Padding?", "id": "Menambahkan Nol Ke Sinyal." },
    { "en": "Apa Itu Transformasi Diskrit Fourier?", "id": "DFT (Discrete Fourier Transform)." },
    { "en": "Apa Itu Algoritma Cepat Untuk DFT?", "id": "FFT (Fast Fourier Transform)." },
    { "en": "Apa Itu Kebocoran Spektral?", "id": "Penyebaran Energi Ke Frekuensi Lain." },
    { "en": "Apa Itu Resolusi Frekuensi?", "id": "Jarak Antara Titik Frekuensi FFT." },
    { "en": "Apa Itu Filter Digital Multirate?", "id": "Mengubah Laju Sampel Sinyal." },
    { "en": "Apa Itu Downsampling?", "id": "Mengurangi Laju Sampel (Desimasi)." },
    { "en": "Apa Itu Upsampling?", "id": "Meningkatkan Laju Sampel (Interpolasi)." },
    { "en": "Apa Itu Filter Polyphase?", "id": "Implementasi Efisien Filter Multirate." },
    { "en": "Apa Itu Filter Notch Adaptif?", "id": "Filter Notch Yang Melacak Frekuensi." },
    { "en": "Apa Itu Filter Equalizer?", "id": "Mengkompensasi Distorsi Kanal." },
    { "en": "Apa Itu Filter Audio Crossover?", "id": "Membagi Frekuensi Untuk Speaker." },
    { "en": "Apa Itu Filter Crossover Orde Pertama?", "id": "Roll-off 6 dB/oktaf." },
    { "en": "Apa Itu Filter Crossover Orde Kedua?", "id": "Roll-off 12 dB/oktaf." },
    { "en": "Apa Itu Filter Crossover Orde Keempat?", "id": "Roll-off 24 dB/oktaf." },
    { "en": "Apa Tipe Alignment Crossover?", "id": "Butterworth, Linkwitz-Riley." },
    { "en": "Apa Itu Jaringan Zobel?", "id": "Filter Kompensasi Impedansi." },
    { "en": "Apa Itu Koreksi Baffle Step?", "id": "Filter Untuk Mengkompensasi Efek Baffle." },
    { "en": "Apa Itu Jaringan Kontur?", "id": "Filter Untuk Membentuk Respon Frekuensi." },
    { "en": "Apa Itu L-Pad?", "id": "Attenuator Yang Menjaga Impedansi." },
    { "en": "Apa Itu Filter Subsonik?", "id": "HPF (High-Pass Filter) Untuk Menghilangkan Frekuensi Rendah." },
    { "en": "Apa Itu Filter RIAA (Recording Industry Association of America)?", "id": "Kurva Ekualisasi Piringan Hitam." },
    { "en": "Apa Itu Filter Rumble?", "id": "Menghilangkan Derau Frekuensi Rendah Turntable." },
    { "en": "Apa Itu Filter Scratch?", "id": "Menghilangkan Derau Frekuensi Tinggi Goresan." },
    { "en": "Apa Itu Topologi Rauch?", "id": "Jenis Filter Lolos Rendah Aktif." },
    { "en": "Apa Itu Filter Berbasis Girator?", "id": "Mensimulasikan Induktor Dengan Op-Amp." },
    { "en": "Apa Itu Filter UAF (Universal Active Filter)?", "id": "IC Filter Universal." },
    { "en": "Apa Itu Filter Elips?", "id": "Nama Lain Untuk Filter Cauer." },
    { "en": "Apa Itu Filter Monotonik?", "id": "Filter Tanpa Riak Di Passband." },
    { "en": "Contoh Filter Monotonik?", "id": "Butterworth Dan Bessel." },
    { "en": "Apa Itu Atentuasi Desibel (dB)?", "id": "Ukuran Pelemahan Sinyal." },
    { "en": "Apa Itu Plot Magnitudo?", "id": "Bagian Dari Bode Plot (Gain)." },
    { "en": "Apa Itu Plot Fasa?", "id": "Bagian Dari Bode Plot (Fasa)." },
    { "en": "Apa Itu Frekuensi Pojok (Corner Frequency)?", "id": "Nama Lain Untuk Frekuensi Cutoff." },
    { "en": "Apa Itu Asimtot?", "id": "Garis Lurus Pendekatan Kurva Respon." },
    { "en": "Apa Itu Fasa Minimum?", "id": "Sistem Tanpa Penundaan Berlebih." },
    { "en": "Apa Itu Fasa Non-Minimum?", "id": "Sistem Dengan Zero Di Sisi Kanan." },
    { "en": "Apa Itu Penundaan Amplop?", "id": "Sinonim Untuk Penundaan Grup." },
    { "en": "Apa Itu Distorsi Fasa Linear?", "id": "Pergeseran Waktu Konstan." },
    { "en": "Apa Itu Distorsi Fasa Non-Linear?", "id": "Menyebabkan Distorsi Bentuk Gelombang." },
    { "en": "Apa Itu Filter Penolakan Kembar-T?", "id": "Filter Notch Twin-T." },
    { "en": "Apa Itu Filter Jembatan Wien?", "id": "Filter Lolos Pita Jembatan Wien." },
    { "en": "Apa Itu Filter Chebyshev Tipe I?", "id": "Riak Di Passband, Datar Di Stopband." },
    { "en": "Apa Itu Filter Chebyshev Tipe II?", "id": "Datar Di Passband, Riak Di Stopband." },
    { "en": "Apa Itu Filter Eliptik?", "id": "Riak Di Passband Dan Stopband." },
    { "en": "Apa Itu Aproksimasi Butterworth?", "id": "Aproksimasi Datar Maksimal." },
    { "en": "Apa Itu Aproksimasi Chebyshev?", "id": "Aproksimasi Riak Sama (Equiripple)." },
    { "en": "Apa Itu Aproksimasi Bessel?", "id": "Aproksimasi Penundaan Grup Datar Maksimal." },
    { "en": "Apa Itu Penolakan Desibel (dB) Per Oktaf?", "id": "Ukuran Kemiringan Roll-off." },
    { "en": "Apa Itu Penolakan Desibel (dB) Per Dekade?", "id": "Ukuran Lain Kemiringan Roll-off." },
    { "en": "Apa Itu Noise Bandwidth?", "id": "Bandwidth Ekuivalen Untuk Derau Putih." },
    { "en": "Apa Itu Power-Bandwidth?", "id": "Bandwidth Frekuensi Penuh." },
    { "en": "Apa Itu Filter Analog Dapat Diprogram?", "id": "Filter Dengan Karakteristik Terkontrol Digital." },
    { "en": "Apa Itu Filter Reluktansi Variabel?", "id": "Filter Dengan Komponen Variabel." },
    { "en": "Apa Itu Filter Berbasis Mikrokontroler?", "id": "Filter Digital Diimplementasikan Di Mikrokontroler." },
    { "en": "Apa Itu Filter Pasif Terkopel?", "id": "Filter Dengan Beberapa Resonator." },
    { "en": "Apa Itu Filter Helikal?", "id": "Filter RF Berbentuk Heliks." },
    { "en": "Apa Itu Filter Interdigital?", "id": "Filter Microwave Planar." },
    { "en": "Apa Itu Filter Rambut Sisir (Hairpin)?", "id": "Filter Mikrostrip Berbentuk Jepit Rambut." },
    { "en": "Apa Itu Filter Resonator Celah-Cincin?", "id": "SRR (Split-Ring Resonator) Filter." },
    { "en": "Apa Itu Filter Metamaterial?", "id": "Menggunakan Struktur Buatan Sub-panjang gelombang." },
    { "en": "Apa Itu Filter Fotonik?", "id": "Filter Yang Bekerja Pada Sinyal Optik." },
    { "en": "Apa Itu Filter Kisi Serat Bragg?", "id": "FBG (Fiber Bragg Grating)." },
    { "en": "Apa Itu Filter Arrayed Waveguide Grating?", "id": "AWG (Arrayed Waveguide Grating)." },
    { "en": "Apa Itu Fabry-Pérot Interferometer?", "id": "Resonator Optik." },
    { "en": "Apa Itu Filter Lyot?", "id": "Filter Birefringent Optik." },
    { "en": "Apa Itu Filter Akusto-Optik Dapat Disetel?", "id": "AOTF (Acousto-Optic Tunable Filter)." },
    { "en": "Apa Itu Filter MEMS (Micro-Electro-Mechanical Systems)?", "id": "Filter Mekanis Mikroskopis." },
    { "en": "Apa Itu FBAR (Film Bulk Acoustic Resonator)?", "id": "Nama Lain Untuk Filter BAW." },
    { "en": "Apa Itu Penguat Umpan Balik?", "id": "Penguat Dengan Jalur Feedback." },
    { "en": "Apa Itu Penguat Transimpedansi?", "id": "TIA (Transimpedance Amplifier)." },
    { "en": "Apa Aplikasi TIA (Transimpedance Amplifier)?", "id": "Penguat Untuk Fotodioda." },
    { "en": "Apa Itu Kestabilan?", "id": "Kondisi Tanpa Osilasi." },
    { "en": "Apa Itu Kompensasi Frekuensi?", "id": "Menjamin Kestabilan Penguat." },
    { "en": "Apa Itu Jalur Distribusi Daya?", "id": "PDN (Power Distribution Network)." },
    { "en": "Apa Fungsi Kapasitor Decoupling?", "id": "Filter Lolos Rendah Untuk PDN." },
    { "en": "Apa Itu Impedansi Target?", "id": "Impedansi Maksimum PDN Yang Diizinkan." },
    { "en": "Apa Itu Resonansi PDN (Power Distribution Network)?", "id": "Puncak Impedansi Yang Tidak Diinginkan." },
    { "en": "Apa Itu Peredaman Resonansi?", "id": "Menekan Puncak Impedansi PDN." },
    { "en": "Apa Itu Filter Notch Aktif?", "id": "Menggunakan Op-Amp Untuk Notch Tajam." },
    { "en": "Apa Itu Filter Gyrator?", "id": "Menggunakan Op-Amp Mensimulasikan Induktor." },
    { "en": "Apa Itu Sirkuit Penentu Nada?", "id": "Tone Control Circuit." },
    { "en": "Apa Itu Filter Resonansi?", "id": "Filter BPF (Band-Pass Filter) Dengan Q Tinggi." },
    { "en": "Apa Itu Filter Pasif Orde Pertama?", "id": "Menggunakan Satu Komponen Reaktif." },
    { "en": "Apa Itu Filter Pasif Orde Kedua?", "id": "Menggunakan Dua Komponen Reaktif." },
    { "en": "Apa Itu Filter Aktif Orde Pertama?", "id": "Menggunakan Satu Kapasitor." },
    { "en": "Apa Itu Filter Aktif Orde Kedua?", "id": "Menggunakan Dua Kapasitor." },
    { "en": "Apa Itu Riak Passband?", "id": "Variasi Gain Di Dalam Passband." },
    { "en": "Apa Itu Atenuasi Stopband?", "id": "Tingkat Redaman Di Dalam Stopband." },
    { "en": "Filter Mana Yang Paling Kompleks?", "id": "Filter Eliptik." },
    { "en": "Filter Mana Yang Paling Sederhana Desainnya?", "id": "Filter Butterworth." },
    { "en": "Apa Itu Unity Gain Frequency?", "id": "Frekuensi Saat Gain Op-Amp Sama Dengan 1." },
    { "en": "Apa Pengaruh Gain-Bandwidth Product Op-Amp?", "id": "Membatasi Frekuensi Kerja Filter Aktif." },
    { "en": "Apa Itu Slew Rate?", "id": "Laju Perubahan Tegangan Output Maksimum." },
    { "en": "Bagaimana Slew Rate Mempengaruhi Filter?", "id": "Menyebabkan Distorsi Pada Frekuensi Tinggi." },
    { "en": "Apa Itu Filter Notch?", "id": "BSF (Band-Stop Filter) Dengan Bandwidth Sangat Sempit." },
    { "en": "Apa Tujuan Filter Notch?", "id": "Menghilangkan Satu Frekuensi Spesifik." },
    { "en": "Apa Itu Filter Tangga (Ladder Filter)?", "id": "Topologi Filter Pasif Berulang." },
    { "en": "Apa Itu Filter LC (Inductor-Capacitor)?", "id": "Filter Pasif Menggunakan Induktor-Kapasitor." },
    { "en": "Apa Keuntungan Filter LC (Inductor-Capacitor)?", "id": "Roll-off Lebih Tajam Dari Filter RC." },
    { "en": "Apa Kerugian Filter LC (Inductor-Capacitor)?", "id": "Induktor Mahal Dan Tidak Ideal." },
    { "en": "Apa Itu Filter RC (Resistor-Capacitor)?", "id": "Filter Pasif Hanya Menggunakan R dan C." },
    { "en": "Apa Itu Jaringan Lag?", "id": "Rangkaian RC Yang Berfungsi Sebagai LPF." },
    { "en": "Apa Itu Jaringan Lead?", "id": "Rangkaian RC Yang Berfungsi Sebagai HPF." },
    { "en": "Apa Itu Jaringan Lag-Lead?", "id": "Kombinasi Jaringan Lag Dan Lead." },
    { "en": "Apa Itu Kompensator?", "id": "Filter Yang Digunakan Dalam Sistem Kontrol." },
    { "en": "Apa Itu Ripple Tegangan?", "id": "Sisa Tegangan AC Pada Output Penyearah." },
    { "en": "Filter Apa Yang Mengurangi Ripple?", "id": "LPF (Low-Pass Filter) Kapasitif." },
    { "en": "Apa Itu Filter DC Block?", "id": "Nama Lain Untuk HPF (High-Pass Filter)." },
    { "en": "Apa Itu Filter Rumble?", "id": "HPF (High-Pass Filter) Untuk Menghilangkan Frekuensi Rendah." },
    { "en": "Apa Itu Filter Hiss?", "id": "LPF (Low-Pass Filter) Untuk Menghilangkan Desis." },
    { "en": "Apa Itu Filter Pita Sisi (Sideband Filter)?", "id": "BPF (Band-Pass Filter) Untuk Sinyal SSB." },
    { "en": "Apa Itu Filter IF (Intermediate Frequency)?", "id": "Filter Selektif Di Penerima Radio." },
    { "en": "Apa Itu Filter RF (Radio Frequency)?", "id": "Filter Awal Di Penerima Radio." },
    { "en": "Apa Itu Filter Citra (Image Filter)?", "id": "Menekan Frekuensi Citra (Image Frequency)." },
    { "en": "Apa Itu Filter Diskrit?", "id": "Filter Dibuat Dari Komponen Terpisah." },
    { "en": "Apa Itu Filter Terdistribusi?", "id": "Filter Menggunakan Elemen Saluran Transmisi." },
    { "en": "Di Mana Filter Terdistribusi Digunakan?", "id": "Aplikasi Frekuensi Microwave." },
    { "en": "Apa Itu Filter Mikrostrip?", "id": "Filter Planar Di Papan PCB." },
    { "en": "Apa Itu Filter Stripline?", "id": "Filter Planar Di Antara Dua Lapisan Ground." },
    { "en": "Apa Itu Filter Rongga (Cavity Filter)?", "id": "Resonator Gelombang Mikro Kualitas Tinggi." },
    { "en": "Apa Itu Filter Pemandu Gelombang (Waveguide Filter)?", "id": "Filter Untuk Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Filter YIG (Yttrium Iron Garnet)?", "id": "Filter Yang Dapat Disetel Secara Magnetis." },
    { "en": "Apa Itu Filter BAW (Bulk Acoustic Wave)?", "id": "Filter Kompak Untuk Ponsel." },
    { "en": "Apa Itu Penolakan Stopband?", "id": "Seberapa Baik Filter Menolak Sinyal." },
    { "en": "Apa Itu Insertion Loss?", "id": "Kehilangan Sinyal Di Dalam Passband." },
    { "en": "Apa Itu Return Loss?", "id": "Ukuran Pantulan Sinyal Di Passband." },
    { "en": "Apa Itu Biquadratic Filter Section?", "id": "Blok Bangunan Filter Orde Dua." },
    { "en": "Apa Itu Infinite Gain Multiple Feedback?", "id": "Topologi Filter MFB (Multiple Feedback)." },
    { "en": "Apa Itu Filter Tangga Aktif?", "id": "Simulasi Filter Tangga Pasif." },
    { "en": "Apa Itu Filter FLIE (Frequency-Locked Loop)?", "id": "Filter Yang Melacak Frekuensi Sinyal." },
    { "en": "Apa Itu Pole Dominan?", "id": "Pole Dengan Frekuensi Terendah." },
    { "en": "Apa Pengaruh Pole Dominan?", "id": "Menentukan Bandwidth Keseluruhan Sistem." },
    { "en": "Apa Itu Kompensasi Pole-Zero?", "id": "Teknik Menstabilkan Sistem Umpan Balik." },
    { "en": "Apa Itu Normalisasi?", "id": "Menskalakan Nilai Komponen Dan Frekuensi." },
    { "en": "Kenapa Normalisasi Digunakan?", "id": "Menyederhanakan Proses Desain Filter." },
    { "en": "Apa Itu Transformasi Low-pass ke Band-pass?", "id": "Mengubah Desain LPF Menjadi BPF." },
    { "en": "Apa Itu Transformasi Low-pass ke Band-stop?", "id": "Mengubah Desain LPF Menjadi BSF." },
    { "en": "Apa Itu Tanggapan Fasa Minimum?", "id": "Fasa Terkecil Untuk Respon Amplitudo." },
    { "en": "Apa Itu Tanggapan Fasa Non-Minimum?", "id": "Memiliki Zero Di Sisi Kanan Bidang-s." },
    { "en": "Apa Itu Fungsi Transfer Jaringan?", "id": "Mendeskripsikan Perilaku Jaringan." },
    { "en": "Apa Itu Filter Matched?", "id": "Filter Optimal Untuk Mendeteksi Pulsa." },
    { "en": "Apa Itu Filter Transversal?", "id": "Struktur Dasar Filter FIR." },
    { "en": "Apa Itu Jalur Tunda (Tapped Delay Line)?", "id": "Komponen Utama Filter Transversal." },
    { "en": "Apa Itu Filter Kisi (Lattice Filter)?", "id": "Struktur Lain Untuk Filter Digital." },
    { "en": "Apa Itu Desain Filter Parks-McClellan?", "id": "Algoritma Desain Filter FIR Optimal." },
    { "en": "Apa Itu Respon Equiripple?", "id": "Riak Dengan Amplitudo Sama." },
    { "en": "Filter Mana Yang Punya Respon Equiripple?", "id": "Filter Chebyshev Dan Eliptik." },
    { "en": "Apa Itu Power Spectral Density (PSD)?", "id": "Kepadatan Spektral Daya." },
    { "en": "Bagaimana Filter Mempengaruhi PSD Sinyal?", "id": "Membentuk Spektrum Sesuai Responnya." },
    { "en": "Apa Itu Pemutihan (Whitening)?", "id": "Membuat Spektrum Sinyal Menjadi Datar." },
    { "en": "Apa Itu Filter Gaussian?", "id": "Filter Dengan Respon Impuls Gaussian." },
    { "en": "Apa Sifat Filter Gaussian?", "id": "Tidak Ada Overshoot, Rise Time Cepat." },
    { "en": "Apa Itu Filter Raised-Cosine?", "id": "Filter Untuk Komunikasi Digital Bebas ISI." },
    { "en": "Apa Itu Filter Root-Raised-Cosine (RRC)?", "id": "Digunakan Di Pemancar Dan Penerima." },
    { "en": "Apa Itu Filter Switched-Capacitor (SC)?", "id": "Filter Waktu Diskrit Analog." },
    { "en": "Apa Prinsip Kerja Filter SC (Switched-Capacitor)?", "id": "Resistor Disimulasikan Dengan Kapasitor Saklar." },
    { "en": "Frekuensi Cutoff Filter SC Ditentukan Oleh Apa?", "id": "Rasio Kapasitansi Dan Frekuensi Clock." },
    { "en": "Apa Itu Filter DSP (Digital Signal Processor)?", "id": "Filter Yang Diimplementasikan Di DSP." },
    { "en": "Apa Itu Koefisien Kuantisasi?", "id": "Pembulatan Nilai Koefisien Filter." },
    { "en": "Apa Itu Batas Siklus (Limit Cycle)?", "id": "Osilasi Kecil Dalam Filter IIR." },
    { "en": "Apa Itu Overflow?", "id": "Hasil Perhitungan Melebihi Batas." },
    { "en": "Apa Itu Penskalaan (Scaling)?", "id": "Menyesuaikan Level Sinyal Internal." },
    { "en": "Apa Itu Arsitektur Direct Form?", "id": "Implementasi Langsung Fungsi Transfer." },
    { "en": "Apa Itu Arsitektur Transposed Form?", "id": "Struktur Alternatif Untuk Implementasi." },
    { "en": "Apa Itu Arsitektur State-Space?", "id": "Representasi Menggunakan Variabel Keadaan." },
    { "en": "Apa Itu Arsitektur Wave Digital Filter?", "id": "WDF (Wave Digital Filter)." },
    { "en": "Apa Keuntungan WDF (Wave Digital Filter)?", "id": "Sifat Insensitivitas Yang Baik." },
    { "en": "Apa Itu Filter Cermin Kuadratur?", "id": "QMF (Quadrature Mirror Filter)." },
    { "en": "Apa Itu Sub-band Coding?", "id": "Memisahkan Sinyal Ke Pita Frekuensi." },
    { "en": "Apa Itu Filter Brick-Wall?", "id": "Filter Ideal Dengan Transisi Seketika." },
    { "en": "Apakah Filter Brick-Wall Dapat Diwujudkan?", "id": "Tidak, Secara Fisik Tidak Mungkin." },
    { "en": "Apa Itu Kausalitas?", "id": "Output Tidak Bisa Mendahului Input." },
    { "en": "Apakah Filter Real-time Harus Kausal?", "id": "Ya, Harus." },
    { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Filter Linear." },
    { "en": "Apa Itu Passband Datar Maksimal?", "id": "Filter Butterworth." },
    { "en": "Apa Itu Phase-Linear?", "id": "Filter Bessel." },
    { "en": "Apa Itu Bi-kuadratik?", "id": "Filter Orde Dua." },
    { "en": "Apa Itu Jaringan T-Bridge Ganda?", "id": "Dasar Dari Osilator Jembatan Wien." },
    { "en": "Apa Itu Filter Orde Keempat?", "id": "Roll-off -80 dB/dekade." },
    { "en": "Apa Itu Orde Filter Crossover?", "id": "Menentukan Kemiringan Pembagian Frekuensi." },
    { "en": "Apa Itu Filter Normalisasi?", "id": "Desain Dengan fc = 1 rad/s." },
    { "en": "Apa Itu Plot Respon Frekuensi?", "id": "Visualisasi Perilaku Filter." },
    { "en": "Apa Itu Filter Chebyshev Terbalik?", "id": "Nama Lain Untuk Tipe II." },
    { "en": "Apa Itu Respon Monotonik?", "id": "Selalu Turun, Tidak Pernah Naik." },
    { "en": "Apa Itu Filter Digital?", "id": "Operasi Matematis Pada Data Sampel." },
    { "en": "Apa Itu Transformasi-Z?", "id": "Alat Analisis Filter Digital." },
    { "en": "Apa Itu Respon Impuls?", "id": "Output Filter Untuk Input Impuls." },
    { "en": "Apa Itu Panjang Respon Impuls?", "id": "Menentukan Tipe FIR Atau IIR." },
    { "en": "Apa Itu Filter FIR Fasa Linear?", "id": "Koefisien Simetris." },
    { "en": "Apa Itu Desain Jendela?", "id": "Metode Sederhana Mendesain Filter FIR." },
    { "en": "Apa Itu Metode Penskalaan Frekuensi?", "id": "Metode Desain Filter IIR." },
    { "en": "Apa Itu Koefisien Filter?", "id": "Angka Yang Mendefinisikan Filter." },
    { "en": "Apa Itu Aritmetika Kuantisasi?", "id": "Efek Dari Presisi Terbatas." },
    { "en": "Apa Itu Sirkuit Penentu Nada?", "id": "Tone Control Circuit." },
    { "en": "Apa Itu Filter Resonansi?", "id": "Filter BPF (Band-Pass Filter) Dengan Q Tinggi." },
    { "en": "Apa Itu Filter Pasif Orde Pertama?", "id": "Menggunakan Satu Komponen Reaktif." },
    { "en": "Apa Itu Filter Pasif Orde Kedua?", "id": "Menggunakan Dua Komponen Reaktif." },
    { "en": "Apa Itu Filter Aktif Orde Pertama?", "id": "Menggunakan Satu Kapasitor." },
    { "en": "Apa Itu Filter Aktif Orde Kedua?", "id": "Menggunakan Dua Kapasitor." },
    { "en": "Apa Itu Riak Passband?", "id": "Variasi Gain Di Dalam Passband." },
    { "en": "Apa Itu Atenuasi Stopband?", "id": "Tingkat Redaman Di Dalam Stopband." },
    { "en": "Filter Mana Yang Paling Kompleks?", "id": "Filter Eliptik." },
    { "en": "Filter Mana Yang Paling Sederhana Desainnya?", "id": "Filter Butterworth." },
    { "en": "Apa Itu Unity Gain Frequency?", "id": "Frekuensi Saat Gain Op-Amp Sama Dengan 1." },
    { "en": "Apa Pengaruh Gain-Bandwidth Product Op-Amp?", "id": "Membatasi Frekuensi Kerja Filter Aktif." },
    { "en": "Apa Itu Slew Rate?", "id": "Laju Perubahan Tegangan Output Maksimum." },
    { "en": "Bagaimana Slew Rate Mempengaruhi Filter?", "id": "Menyebabkan Distorsi Pada Frekuensi Tinggi." },
    { "en": "Apa Itu Filter Notch?", "id": "BSF (Band-Stop Filter) Dengan Bandwidth Sangat Sempit." },
    { "en": "Apa Tujuan Filter Notch?", "id": "Menghilangkan Satu Frekuensi Spesifik." },
    { "en": "Apa Itu Filter Tangga (Ladder Filter)?", "id": "Topologi Filter Pasif Berulang." },
    { "en": "Apa Itu Filter LC (Inductor-Capacitor)?", "id": "Filter Pasif Menggunakan Induktor-Kapasitor." },
    { "en": "Apa Keuntungan Filter LC (Inductor-Capacitor)?", "id": "Roll-off Lebih Tajam Dari Filter RC." },
    { "en": "Apa Kerugian Filter LC (Inductor-Capacitor)?", "id": "Induktor Mahal Dan Tidak Ideal." },
    { "en": "Apa Itu Filter RC (Resistor-Capacitor)?", "id": "Filter Pasif Hanya Menggunakan R dan C." },
    { "en": "Apa Itu Jaringan Lag?", "id": "Rangkaian RC Yang Berfungsi Sebagai LPF." },
    { "en": "Apa Itu Jaringan Lead?", "id": "Rangkaian RC Yang Berfungsi Sebagai HPF." },
    { "en": "Apa Itu Jaringan Lag-Lead?", "id": "Kombinasi Jaringan Lag Dan Lead." },
    { "en": "Apa Itu Kompensator?", "id": "Filter Yang Digunakan Dalam Sistem Kontrol." },
    { "en": "Apa Itu Ripple Tegangan?", "id": "Sisa Tegangan AC Pada Output Penyearah." },
    { "en": "Filter Apa Yang Mengurangi Ripple?", "id": "LPF (Low-Pass Filter) Kapasitif." },
    { "en": "Apa Itu Filter DC Block?", "id": "Nama Lain Untuk HPF (High-Pass Filter)." },
    { "en": "Apa Itu Filter Rumble?", "id": "HPF (High-Pass Filter) Untuk Menghilangkan Frekuensi Rendah." },
    { "en": "Apa Itu Filter Hiss?", "id": "LPF (Low-Pass Filter) Untuk Menghilangkan Desis." },
    { "en": "Apa Itu Filter Pita Sisi (Sideband Filter)?", "id": "BPF (Band-Pass Filter) Untuk Sinyal SSB." },
    { "en": "Apa Itu Filter IF (Intermediate Frequency)?", "id": "Filter Selektif Di Penerima Radio." },
    { "en": "Apa Itu Filter RF (Radio Frequency)?", "id": "Filter Awal Di Penerima Radio." },
    { "en": "Apa Itu Filter Citra (Image Filter)?", "id": "Menekan Frekuensi Citra (Image Frequency)." },
    { "en": "Apa Itu Filter Diskrit?", "id": "Filter Dibuat Dari Komponen Terpisah." },
    { "en": "Apa Itu Filter Terdistribusi?", "id": "Filter Menggunakan Elemen Saluran Transmisi." },
    { "en": "Di Mana Filter Terdistribusi Digunakan?", "id": "Aplikasi Frekuensi Microwave." },
    { "en": "Apa Itu Filter Mikrostrip?", "id": "Filter Planar Di Papan PCB." },
    { "en": "Apa Itu Filter Stripline?", "id": "Filter Planar Di Antara Dua Lapisan Ground." },
    { "en": "Apa Itu Filter Rongga (Cavity Filter)?", "id": "Resonator Gelombang Mikro Kualitas Tinggi." },
    { "en": "Apa Itu Filter Pemandu Gelombang (Waveguide Filter)?", "id": "Filter Untuk Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu Filter YIG (Yttrium Iron Garnet)?", "id": "Filter Yang Dapat Disetel Secara Magnetis." },
    { "en": "Apa Itu Filter BAW (Bulk Acoustic Wave)?", "id": "Filter Kompak Untuk Ponsel." },
    { "en": "Apa Itu Penolakan Stopband?", "id": "Seberapa Baik Filter Menolak Sinyal." },
    { "en": "Apa Itu Insertion Loss?", "id": "Kehilangan Sinyal Di Dalam Passband." },
    { "en": "Apa Itu Return Loss?", "id": "Ukuran Pantulan Sinyal Di Passband." },
    { "en": "Apa Itu Biquadratic Filter Section?", "id": "Blok Bangunan Filter Orde Dua." },
    { "en": "Apa Itu Infinite Gain Multiple Feedback?", "id": "Topologi Filter MFB (Multiple Feedback)." },
    { "en": "Apa Itu Filter Tangga Aktif?", "id": "Simulasi Filter Tangga Pasif." },
    { "en": "Apa Itu Filter FLIE (Frequency-Locked Loop)?", "id": "Filter Yang Melacak Frekuensi Sinyal." },
    { "en": "Apa Itu Pole Dominan?", "id": "Pole Dengan Frekuensi Terendah." },
    { "en": "Apa Pengaruh Pole Dominan?", "id": "Menentukan Bandwidth Keseluruhan Sistem." },
    { "en": "Apa Itu Kompensasi Pole-Zero?", "id": "Teknik Menstabilkan Sistem Umpan Balik." },
    { "en": "Apa Itu Normalisasi?", "id": "Menskalakan Nilai Komponen Dan Frekuensi." },
    { "en": "Kenapa Normalisasi Digunakan?", "id": "Menyederhanakan Proses Desain Filter." },
    { "en": "Apa Itu Transformasi Low-pass ke Band-pass?", "id": "Mengubah Desain LPF Menjadi BPF." },
    { "en": "Apa Itu Transformasi Low-pass ke Band-stop?", "id": "Mengubah Desain LPF Menjadi BSF." },
    { "en": "Apa Itu Tanggapan Fasa Minimum?", "id": "Fasa Terkecil Untuk Respon Amplitudo." },
    { "en": "Apa Itu Tanggapan Fasa Non-Minimum?", "id": "Memiliki Zero Di Sisi Kanan Bidang-s." },
    { "en": "Apa Itu Fungsi Transfer Jaringan?", "id": "Mendeskripsikan Perilaku Jaringan." },
    { "en": "Apa Itu Filter Matched?", "id": "Filter Optimal Untuk Mendeteksi Pulsa." },
    { "en": "Apa Itu Filter Transversal?", "id": "Struktur Dasar Filter FIR." },
    { "en": "Apa Itu Jalur Tunda (Tapped Delay Line)?", "id": "Komponen Utama Filter Transversal." },
    { "en": "Apa Itu Filter Kisi (Lattice Filter)?", "id": "Struktur Lain Untuk Filter Digital." },
    { "en": "Apa Itu Desain Filter Parks-McClellan?", "id": "Algoritma Desain Filter FIR Optimal." },
    { "en": "Apa Itu Respon Equiripple?", "id": "Riak Dengan Amplitudo Sama." },
    { "en": "Filter Mana Yang Punya Respon Equiripple?", "id": "Filter Chebyshev Dan Eliptik." },
    { "en": "Apa Itu Power Spectral Density (PSD)?", "id": "Kepadatan Spektral Daya." },
    { "en": "Bagaimana Filter Mempengaruhi PSD Sinyal?", "id": "Membentuk Spektrum Sesuai Responnya." },
    { "en": "Apa Itu Pemutihan (Whitening)?", "id": "Membuat Spektrum Sinyal Menjadi Datar." },
    { "en": "Apa Itu Filter Gaussian?", "id": "Filter Dengan Respon Impuls Gaussian." },
    { "en": "Apa Sifat Filter Gaussian?", "id": "Tidak Ada Overshoot, Rise Time Cepat." },
    { "en": "Apa Itu Filter Raised-Cosine?", "id": "Filter Untuk Komunikasi Digital Bebas ISI." },
    { "en": "Apa Itu Filter Root-Raised-Cosine (RRC)?", "id": "Digunakan Di Pemancar Dan Penerima." },
    { "en": "Apa Itu Filter Switched-Capacitor (SC)?", "id": "Filter Waktu Diskrit Analog." },
    { "en": "Apa Prinsip Kerja Filter SC (Switched-Capacitor)?", "id": "Resistor Disimulasikan Dengan Kapasitor Saklar." },
    { "en": "Frekuensi Cutoff Filter SC Ditentukan Oleh Apa?", "id": "Rasio Kapasitansi Dan Frekuensi Clock." },
    { "en": "Apa Itu Filter DSP (Digital Signal Processor)?", "id": "Filter Yang Diimplementasikan Di DSP." },
    { "en": "Apa Itu Koefisien Kuantisasi?", "id": "Pembulatan Nilai Koefisien Filter." },
    { "en": "Apa Itu Batas Siklus (Limit Cycle)?", "id": "Osilasi Kecil Dalam Filter IIR." },
    { "en": "Apa Itu Overflow?", "id": "Hasil Perhitungan Melebihi Batas." },
    { "en": "Apa Itu Penskalaan (Scaling)?", "id": "Menyesuaikan Level Sinyal Internal." },
    { "en": "Apa Itu Arsitektur Direct Form?", "id": "Implementasi Langsung Fungsi Transfer." },
    { "en": "Apa Itu Arsitektur Transposed Form?", "id": "Struktur Alternatif Untuk Implementasi." },
    { "en": "Apa Itu Arsitektur State-Space?", "id": "Representasi Menggunakan Variabel Keadaan." },
    { "en": "Apa Itu Arsitektur Wave Digital Filter?", "id": "WDF (Wave Digital Filter)." },
    { "en": "Apa Keuntungan WDF (Wave Digital Filter)?", "id": "Sifat Insensitivitas Yang Baik." },
    { "en": "Apa Itu Filter Cermin Kuadratur?", "id": "QMF (Quadrature Mirror Filter)." },
    { "en": "Apa Itu Sub-band Coding?", "id": "Memisahkan Sinyal Ke Pita Frekuensi." },
    { "en": "Apa Itu Filter Brick-Wall?", "id": "Filter Ideal Dengan Transisi Seketika." },
    { "en": "Apakah Filter Brick-Wall Dapat Diwujudkan?", "id": "Tidak, Secara Fisik Tidak Mungkin." },
    { "en": "Apa Itu Kausalitas?", "id": "Output Tidak Bisa Mendahului Input." },
    { "en": "Apakah Filter Real-time Harus Kausal?", "id": "Ya, Harus." },
    { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Filter Linear." },
    { "en": "Apa Itu Dekonvolusi?", "id": "Proses Membalikkan Efek Filter." },
    { "en": "Apa Itu Homomorphic Filtering?", "id": "Filtering Dalam Domain Berbeda." },
    { "en": "Apa Itu Filter Median?", "id": "Filter Non-Linear Untuk Menghilangkan Derau." },
    { "en": "Apa Itu Derau Salt-and-Pepper?", "id": "Derau Titik Hitam Putih Acak." },
    { "en": "Apa Itu Filter Anisotropik?", "id": "Filter Yang Beraksi Berbeda Arah." },
    { "en": "Apa Itu Filter Bilateral?", "id": "Mempertahankan Tepi Saat Menghaluskan." },
    { "en": "Apa Itu Unsharp Masking?", "id": "Teknik Penajaman Gambar." },
    { "en": "Apa Itu High-Boost Filtering?", "id": "Menekankan Frekuensi Tinggi Untuk Penajaman." },
    { "en": "Apa Itu Filter Emboss?", "id": "Filter Efek Visual Timbul." },
    { "en": "Apa Itu Filter Sobel?", "id": "Filter Deteksi Tepi." },
    { "en": "Apa Itu Filter Prewitt?", "id": "Filter Deteksi Tepi Lainnya." },
    { "en": "Apa Itu Filter Canny?", "id": "Algoritma Deteksi Tepi Multi-tahap." },
    { "en": "Apa Itu Laplacian of Gaussian (LoG)?", "id": "Filter Untuk Menemukan Area Perubahan Cepat." },
    { "en": "Apa Itu Filter Gabor?", "id": "Filter Terarah Untuk Analisis Tekstur." },
    { "en": "Apa Itu Domain Spasial?", "id": "Bekerja Langsung Pada Piksel Gambar." },
    { "en": "Apa Itu Domain Frekuensi?", "id": "Bekerja Pada Hasil Transformasi Fourier." },
    { "en": "Apa Itu Transformasi Fourier 2D?", "id": "Mengubah Gambar Ke Domain Frekuensi." },
    { "en": "Apa Itu Kernel Konvolusi?", "id": "Matriks Kecil Untuk Operasi Filtering." },
    { "en": "Apa Itu Padding Gambar?", "id": "Menambahkan Piksel Di Tepi Gambar." },
    { "en": "Apa Itu Filter Separable?", "id": "Kernel 2D Yang Dapat Dipisah." },
    { "en": "Apa Itu Histogram Gambar?", "id": "Distribusi Tingkat Kecerahan Piksel." },
    { "en": "Apa Itu Ekualisasi Histogram?", "id": "Meningkatkan Kontras Gambar." },
    { "en": "Apa Itu Morfologi Matematika?", "id": "Operasi Berbasis Bentuk (Shape)." },
    { "en": "Apa Itu Operasi Erosi?", "id": "Mengikis Batas Objek." },
    { "en": "Apa Itu Operasi Dilasi?", "id": "Memperluas Batas Objek." },
    { "en": "Apa Itu Operasi Opening?", "id": "Erosi Diikuti Dilasi (Menghaluskan)." },
    { "en": "Apa Itu Operasi Closing?", "id": "Dilasi Diikuti Erosi (Menutup Lubang)." },
    { "en": "Apa Itu Elemen Struktur?", "id": "Kernel Kecil Untuk Operasi Morfologi." },
    { "en": "Apa Itu Deteksi Objek?", "id": "Menemukan Objek Tertentu Dalam Gambar." },
    { "en": "Apa Itu Segmentasi Gambar?", "id": "Membagi Gambar Menjadi Beberapa Wilayah." },
    { "en": "Apa Itu Thresholding?", "id": "Mengubah Gambar Grayscale Menjadi Biner." },
    { "en": "Apa Itu Thresholding Adaptif?", "id": "Ambang Batas Berubah Di Seluruh Gambar." },
    { "en": "Apa Itu Restorasi Gambar?", "id": "Memperbaiki Gambar Yang Terdegradasi." },
    { "en": "Apa Itu Deblurring?", "id": "Menghilangkan Efek Buram." },
    { "en": "Apa Itu Denoising?", "id": "Menghilangkan Derau Dari Gambar." },
    { "en": "Apa Itu Inpainting?", "id": "Mengisi Bagian Gambar Yang Hilang." },
    { "en": "Apa Itu Super-Resolution?", "id": "Meningkatkan Resolusi Gambar." },
    { "en": "Apa Itu Filter Audio?", "id": "Filter Yang Bekerja Pada Sinyal Suara." },
    { "en": "Apa Itu Filter Video?", "id": "Filter Yang Bekerja Pada Sinyal Gambar." },
    { "en": "Apa Itu Filter Power Supply?", "id": "LPF (Low-Pass Filter) Untuk Meratakan Tegangan DC." },
    { "en": "Apa Itu Filter Jaringan Listrik?", "id": "Menghilangkan Derau Dari Jalur Listrik." },
    { "en": "Apa Itu Filter Selektivitas?", "id": "Kemampuan Memilih Frekuensi Yang Diinginkan." },
    { "en": "Apa Itu Filter Penolakan?", "id": "Kemampuan Menolak Frekuensi Tak Diinginkan." },
    { "en": "Apa Itu Atap Filter (Filter Skirt)?", "id": "Transisi Antara Passband Dan Stopband." },
    { "en": "Apa Itu Faktor Bentuk (Shape Factor)?", "id": "Rasio Bandwidth Stopband Per Passband." },
    { "en": "Apa Itu Ripple Stopband?", "id": "Variasi Atenuasi Di Dalam Stopband." },
    { "en": "Apa Itu Atenuasi Ultimate?", "id": "Tingkat Atenuasi Maksimum Di Stopband." },
    { "en": "Apa Itu Kedalaman Notch?", "id": "Atenuasi Maksimum Filter Notch." },
    { "en": "Apa Itu Penskalaan Amplitudo?", "id": "Menyesuaikan Level Gain Filter." },
    { "en": "Apa Itu Penskalaan Frekuensi?", "id": "Memindahkan Respon Filter Ke Frekuensi Baru." },
    { "en": "Apa Itu Implementasi Perangkat Keras Filter?", "id": "Menggunakan Komponen Fisik (Analog)." },
    { "en": "Apa Itu Implementasi Perangkat Lunak Filter?", "id": "Menggunakan Kode Program (Digital)." },
    { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
    { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
    { "en": "Apa Itu Efek Pemuatan (Loading Effect)?", "id": "Filter Mempengaruhi Sumber Sinyal." },
    { "en": "Bagaimana Filter Aktif Mengatasi Pemuatan?", "id": "Dengan Impedansi Input Tinggi." },
    { "en": "Apa Itu Impedansi Input?", "id": "Hambatan Yang Dilihat Oleh Sumber Sinyal." },
    { "en": "Apa Itu Impedansi Output?", "id": "Hambatan Internal Dari Output Filter." },
    { "en": "Impedansi Input Filter Aktif?", "id": "Sangat Tinggi." },
    { "en": "Impedansi Output Filter Aktif?", "id": "Sangat Rendah." },
    { "en": "Apa Itu Filter Pasif Tipe L?", "id": "Bentuk Sederhana LPF Atau HPF." },
    { "en": "Apa Itu Pembangkitan Derau (Noise Generation)?", "id": "Komponen Aktif Menambah Derau." },
    { "en": "Filter Mana Yang Menghasilkan Derau?", "id": "Filter Aktif." },
    { "en": "Apa Itu Rentang Dinamis?", "id": "Rasio Sinyal Terbesar Per Terkecil." },
    { "en": "Bagaimana Filter Aktif Mempengaruhi Rentang Dinamis?", "id": "Dibatasi Oleh Derau Dan Tegangan Catu." },
    { "en": "Apa Itu Distorsi?", "id": "Perubahan Bentuk Gelombang Yang Tidak Diinginkan." },
    { "en": "Apa Yang Menyebabkan Distorsi Di Filter Aktif?", "id": "Keterbatasan Op-Amp (Slew Rate, Clipping)." },
    { "en": "Apa Itu Clipping?", "id": "Pemotongan Sinyal Akibat Batas Tegangan." },
    { "en": "Apa Itu Filter Orde Campuran?", "id": "Menggabungkan Orde Berbeda (Contoh: Orde 3)." },
    { "en": "Bagaimana Membuat Filter Orde Ketiga?", "id": "Menggabungkan Orde Pertama Dan Kedua." },
    { "en": "Apa Itu Tanggapan Frekuensi Ideal?", "id": "Passband Datar, Transisi Seketika." },
    { "en": "Apa Itu Tanggapan Fasa Ideal?", "id": "Fasa Linear (Penundaan Konstan)." },
    { "en": "Apa Itu Aproksimasi Filter?", "id": "Pendekatan Matematis (Butterworth, Chebyshev)." },
    { "en": "Apa Itu Frekuensi Normalisasi?", "id": "Frekuensi Dinyatakan Relatif Terhadap Cutoff." },
    { "en": "Apa Itu Filter Invers Chebyshev?", "id": "Riak Di Stopband, Datar Di Passband." },
    { "en": "Apa Itu Filter Papoulis-Legendre?", "id": "Filter Monotonik Dengan Roll-off Tajam." },
    { "en": "Apa Itu Filter Hourglass?", "id": "Filter Dengan Respon Fasa Linear." },
    { "en": "Apa Itu Penolakan Desibel (dB)?", "id": "Ukuran Atenuasi Filter." },
    { "en": "Apa Itu Plot Pole-Zero?", "id": "Visualisasi Karakteristik Filter Di Bidang-s." },
    { "en": "Apa Efek Memindahkan Pole Lebih Jauh?", "id": "Meningkatkan Frekuensi Cutoff." },
    { "en": "Apa Efek Mengubah Sudut Pole?", "id": "Mengubah Faktor Q (Kualitas)." },
    { "en": "Apa Itu Filter OTA-C?", "id": "Filter Menggunakan OTA Dan Kapasitor." }


        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
