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


  { "en": "Apa Simbol Huruf Untuk Kapasitansi?", "id": "C." },
  { "en": "Apa Simbol Huruf Untuk Induktansi?", "id": "L." },
  { "en": "Apa Satuan Dasar Muatan Listrik?", "id": "Coulomb (C)." },
  { "en": "Apa Satuan Dasar Energi Listrik (SI)?", "id": "Joule (J)." },
  { "en": "Apa Satuan Dasar Konduktansi Listrik?", "id": "Siemens (S)." },
  { "en": "Apa Kebalikan Dari Resistansi Listrik?", "id": "Konduktansi Listrik." },
  { "en": "Apa Kebalikan Dari Reaktansi Listrik?", "id": "Suseptansi Listrik." },
  { "en": "Apa Satuan Dasar Suseptansi Listrik?", "id": "Siemens (S)." },
  { "en": "Komponen Apa Menyimpan Muatan Listrik?", "id": "Kapasitor (Capacitor)." },
  { "en": "Komponen Apa Menentang Perubahan Arus Tiba-Tiba?", "id": "Induktor (Inductor)." },
  { "en": "Komponen Apa Menentang Perubahan Tegangan Tiba-Tiba?", "id": "Kapasitor (Capacitor)." },
  { "en": "Apa Itu Daya (Power) Sesaat Listrik?", "id": "Daya Listrik Pada Waktu Tertentu." },
  { "en": "Apa Satuan Daya (Power) Sesaat?", "id": "Watt (W)." },
  { "en": "Apa Itu Nilai Rata-Rata (Average Value) Arus AC?", "id": "Nilai Ekuivalen DC Sinyal AC." },
  { "en": "Apa Itu Nilai Efektif (Effective Value) Arus AC?", "id": "Nilai RMS (Root Mean Square) Arus." },
  { "en": "Alat Apa Mengukur Konsumsi Energi Listrik?", "id": "Kwh Meter (Kilowatt-Hour Meter)." },
  { "en": "Apa Kepanjangan kWh (Kilowatt-Hour)?", "id": "Kilowatt Jam." },
  { "en": "Apa Itu Bahan (Material) Dielektrik?", "id": "Bahan Isolator Antar Pelat Kapasitor." },
  { "en": "Apa Fungsi Utama Bahan (Material) Dielektrik?", "id": "Meningkatkan Nilai Kapasitansi." },
  { "en": "Apa Itu Permitivas (Permittivity) Bahan?", "id": "Ukuran Kemampuan Simpan Energi Listrik." },
  { "en": "Apa Itu Bahan (Material) Inti Induktor?", "id": "Material Bagian Pusat Kumparan Induktor." },
  { "en": "Apa Fungsi Bahan (Material) Inti Magnetik?", "id": "Meningkatkan Nilai Induktansi." },
  { "en": "Apa Itu Permeabilitas (Permeability) Bahan?", "id": "Ukuran Kemampuan Hantar Fluks Magnet." },
  { "en": "Apa Itu Rangkaian (Circuit) Resonansi?", "id": "Rangkaian Mengandung Induktor Dan Kapasitor." },
  { "en": "Kapan Rangkaian RLC (Resistor Induktor Kapasitor) Seri Resonansi?", "id": "Saat Reaktansi Induktif Sama Kapasitif." },
  { "en": "Apa Impedansi (Impedance) RLC Seri Saat Resonansi?", "id": "Impedansi Minimum (Hanya Resistansi)." },
  { "en": "Apa Arus (Current) RLC Seri Saat Resonansi?", "id": "Arus Mencapai Nilai Maksimum." },
  { "en": "Apa Itu Faktor (Factor) Kualitas (Q)?", "id": "Ukuran Selektivitas Frekuensi Rangkaian." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Rangkaian Resonansi?", "id": "Lebar Rentang Frekuensi Sekitar Resonansi." },
  { "en": "Apa Itu Filter (Filter) Lolos Pita (Bandpass)?", "id": "Melewatkan Satu Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter (Filter) Tolak Pita (Bandstop)?", "id": "Menolak Satu Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Frekuensi (Frequency) Cutoff?", "id": "Frekuensi Batas Operasi Efektif Filter." },
  { "en": "Pada Titik Apa Frekuensi (Frequency) Cutoff Diukur?", "id": "Titik Daya Setengah (-3 DB)." },
  { "en": "Apa Itu Penyearah (Rectifier) Jembatan?", "id": "Empat Dioda Penyearah Gelombang Penuh." },
  { "en": "Apa Keuntungan Penyearah (Rectifier) Jembatan?", "id": "Output DC Riak Lebih Kecil." },
  { "en": "Apa Itu Tegangan (Voltage) Puncak Balik (PIV)?", "id": "Tegangan Mundur Maksimum Dioda." },
  { "en": "Apa Kepanjangan PIV (Peak Inverse Voltage)?", "id": "Tegangan Puncak Terbalik." },
  { "en": "Apa Fungsi Utama Dioda (Diode) Zener?", "id": "Sebagai Regulator Tegangan Konstan." },
  { "en": "Apa Itu Tegangan (Voltage) Zener (Vz)?", "id": "Tegangan Breakdown Stabil Dioda Zener." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Semikonduktor Yang Memancarkan Cahaya." },
  { "en": "Apa Bahan (Material) Umum Semikonduktor LED?", "id": "Galium Arsenida (GaAs)." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Cahaya." },
  { "en": "Bagaimana Fotodioda (Photodiode) Umumnya Dibiaskan?", "id": "Diberi Bias Mundur (Reverse Bias)." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Bipolar Junction Transistor." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Field Effect Transistor." },
  { "en": "Apa Parameter Kontrol (Control) BJT?", "id": "Arus Basis (Current Controlled)." },
  { "en": "Apa Parameter Kontrol (Control) FET?", "id": "Tegangan Gate (Voltage Controlled)." },
  { "en": "Apa Itu Daerah (Region) Aktif Transistor?", "id": "Daerah Operasi Penguatan Linear." },
  { "en": "Apa Itu Daerah (Region) Saturasi Transistor?", "id": "Daerah Operasi Saklar Tertutup." },
  { "en": "Apa Itu Daerah (Region) Cutoff Transistor?", "id": "Daerah Operasi Saklar Terbuka." },
  { "en": "Apa Itu Penguatan (Gain) Arus (Beta)?", "id": "Rasio Ic Terhadap Ib (Ic/Ib)." },
  { "en": "Apa Itu Penguat (Amplifier) Common Emitter?", "id": "Konfigurasi Penguat BJT Paling Umum." },
  { "en": "Apa Karakteristik (Characteristic) Penguat Common Emitter?", "id": "Gain Tegangan Tinggi, Fasa Terbalik." },
  { "en": "Apa Itu Penguat (Amplifier) Common Collector?", "id": "Pengikut Emitor (Emitter Follower)." },
  { "en": "Apa Karakteristik (Characteristic) Penguat Common Collector?", "id": "Gain Tegangan Satu, Impedansi Input Tinggi." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET (Field Effect Transistor) Gerbang Terisolasi Oksida." },
  { "en": "Apa Keunggulan (Advantage) Utama MOSFET?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Logika Kombinasi NMOS Dan PMOS." },
  { "en": "Apa Itu Penguat (Amplifier) Operasional?", "id": "Op-Amp (Operational Amplifier)." },
  { "en": "Apa Itu Penguatan (Gain) Loop Terbuka?", "id": "Penguatan Op-Amp Tanpa Umpan Balik." },
  { "en": "Apa Fungsi Umpan Balik (Feedback) Negatif?", "id": "Menstabilkan Penguatan Op-Amp." },
  { "en": "Apa Itu Penguat (Amplifier) Inverting?", "id": "Output Membalik Fasa Sinyal Input." },
  { "en": "Apa Itu Penguat (Amplifier) Non-Inverting?", "id": "Output Sefasa Dengan Sinyal Input." },
  { "en": "Apa Itu Pengikut (Follower) Tegangan?", "id": "Buffer (Buffer) Op-Amp Gain Satu." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Sinyal Tegangan Input." },
  { "en": "Apa Itu Gerbang (Gate) Logika Digital?", "id": "Elemen Dasar Rangkaian Logika Digital." },
  { "en": "Apa Itu Aljabar (Algebra) Boolean?", "id": "Matematika Operasi Logika Biner." },
  { "en": "Apa Itu Tabel (Table) Kebenaran?", "id": "Tabel Output Untuk Kombinasi Input." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Register (Register) Digital?", "id": "Kumpulan Flip-Flop Penyimpan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Digital." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Pemilih Sinyal Input Digital." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Distributor Sinyal Output Digital." },
  { "en": "Apa Itu Memori (Memory) RAM?", "id": "Random Access Memory (Volatil)." },
  { "en": "Apa Itu Memori (Memory) ROM?", "id": "Read Only Memory (Non-Volatil)." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Alat Pengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Jaringan Pembangkitan Hingga Distribusi." },
  { "en": "Apa Itu Transmisi (Transmission) Tegangan Tinggi?", "id": "Penyaluran Daya Jarak Jauh." },
  { "en": "Apa Itu Distribusi (Distribution) Tegangan Rendah?", "id": "Penyaluran Daya Ke Konsumen." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Dari Gangguan." },
  { "en": "Apa Itu Pemutus (Breaker) Sirkuit?", "id": "Saklar Pengaman Otomatis." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keselamatan." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Konversi Daya Listrik." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Pengubah Daya DC Ke AC." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback) Kontrol?", "id": "Penggunaan Output Mengoreksi Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Sinyal Listrik." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur V, A, Ω." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Rangkaian (Circuit) Resonansi?", "id": "Rangkaian Mengandung L Dan C." },
  { "en": "Kapan Rangkaian RLC (Resistor Induktor Kapasitor) Seri Resonansi?", "id": "Saat Nilai XL Sama Xc." },
  { "en": "Apa Impedansi (Impedance) RLC Seri Saat Resonansi?", "id": "Impedansi Minimum (Sama Dengan R)." },
  { "en": "Apa Arus (Current) RLC Seri Saat Resonansi?", "id": "Arus Mencapai Nilai Maksimum." },
  { "en": "Apa Itu Faktor (Factor) Kualitas Q?", "id": "Ukuran Selektivitas Frekuensi Rangkaian." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Rangkaian Resonansi?", "id": "Lebar Rentang Frekuensi Resonansi." },
  { "en": "Apa Hubungan Q (Faktor Kualitas) Dan Bandwidth (Bandwidth)?", "id": "Q Tinggi Berarti Bandwidth Sempit." },
  { "en": "Apa Itu Filter (Filter) Lolos Rendah?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter (Filter) Lolos Tinggi?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter (Filter) Lolos Pita?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter (Filter) Tolak Pita?", "id": "Menolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Frekuensi (Frequency) Cutoff Filter?", "id": "Frekuensi Batas Daya Turun Setengah." },
  { "en": "Berapa Penurunan Daya (Power) Di Frekuensi Cutoff?", "id": "Turun Tiga Desibel (-3 DB)." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman Slope Filter." },
  { "en": "Apa Itu Filter (Filter) Pasif?", "id": "Filter Terbuat Komponen Pasif (R, L, C)." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Searah Arus." },
  { "en": "Apa Bahan (Material) Utama Dioda?", "id": "Silikon (Si) Atau Germanium (Ge)." },
  { "en": "Apa Itu Sambungan (Junction) P-N?", "id": "Pertemuan Bahan Tipe-P, Tipe-N." },
  { "en": "Apa Itu Bias (Bias) Maju Dioda?", "id": "Anoda Positif, Katoda Negatif." },
  { "en": "Apa Itu Bias (Bias) Mundur Dioda?", "id": "Anoda Negatif, Katoda Positif." },
  { "en": "Kapan Dioda (Diode) Menghantar Arus?", "id": "Saat Diberi Bias Maju." },
  { "en": "Apa Itu Tegangan (Voltage) Lutut Dioda?", "id": "Tegangan Minimum Konduksi Dioda." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Pengubah Sinyal AC Ke DC." },
  { "en": "Apa Itu Penyearah (Rectifier) Setengah Gelombang?", "id": "Menggunakan Satu Dioda Saja." },
  { "en": "Apa Itu Penyearah (Rectifier) Gelombang Penuh?", "id": "Menyearahkan Kedua Siklus AC." },
  { "en": "Berapa Dioda (Diode) Penyearah Jembatan?", "id": "Empat Buah Dioda." },
  { "en": "Apa Fungsi Kapasitor (Capacitor) Filter Penyearah?", "id": "Menghaluskan Tegangan DC (Riak)." },
  { "en": "Apa Itu Tegangan (Voltage) Riak (Ripple)?", "id": "Sisa Variasi AC Pada Output DC." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Dioda Regulator Tegangan Mundur." },
  { "en": "Apa Fungsi Utama Dioda (Diode) Zener?", "id": "Menjaga Tegangan Output Tetap Konstan." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Mengapa LED (Light Emitting Diode) Butuh Resistor Seri?", "id": "Untuk Membatasi Arus Listrik." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Semikonduktor Penguat, Saklar." },
  { "en": "Apa Jenis (Type) Transistor Utama?", "id": "BJT (Bipolar Junction Transistor) Dan FET (Field Effect Transistor)." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Bipolar Junction Transistor." },
  { "en": "Apa Terminal (Terminal) Transistor BJT?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Transistor BJT (Bipolar Junction Transistor) Digerakkan Oleh Apa?", "id": "Arus (Current) Basis." },
  { "en": "Apa Itu Penguatan (Gain) Arus BJT?", "id": "Beta (β) Atau Hfe." },
  { "en": "Apa Tiga Daerah (Region) Operasi BJT?", "id": "Cutoff, Aktif, Dan Saturasi." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Berfungsi Penguat?", "id": "Saat Di Daerah Aktif." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Berfungsi Saklar?", "id": "Saat Di Daerah Cutoff, Saturasi." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Field Effect Transistor." },
  { "en": "Apa Terminal (Terminal) Transistor FET?", "id": "Gate (Gerbang), Drain (Saluran), Source (Sumber)." },
  { "en": "Transistor FET (Field Effect Transistor) Digerakkan Oleh Apa?", "id": "Tegangan (Voltage) Gate." },
  { "en": "Apa Keunggulan (Advantage) FET Dibanding BJT?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Jenis FET (Field Effect Transistor) Paling Umum." },
  { "en": "Apa Itu Penguat (Amplifier) Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Karakteristik (Characteristic) Op-Amp Ideal?", "id": "Gain Tak Hingga, Input Z Tak Hingga." },
  { "en": "Apa Itu Umpan Balik (Feedback) Negatif Op-Amp?", "id": "Menstabilkan Penguatan (Gain) Op-Amp." },
  { "en": "Apa Itu Penguat (Amplifier) Inverting?", "id": "Output Berbeda Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat (Amplifier) Non-Inverting?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut (Follower) Tegangan Op-Amp?", "id": "Buffer (Buffer) Gain Satu." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Itu Sirkuit (Circuit) Digital?", "id": "Sirkuit Beroperasi Sinyal Digital." },
  { "en": "Apa Itu Logika (Logic) Biner?", "id": "Sistem Logika Dua Nilai (0, 1)." },
  { "en": "Apa Itu Gerbang (Gate) Logika?", "id": "Elemen Dasar Operasi Logika Digital." },
  { "en": "Apa Itu Gerbang (Gate) AND?", "id": "Output 1 Jika Semua Input 1." },
  { "en": "Apa Itu Gerbang (Gate) OR?", "id": "Output 1 Jika Ada Input 1." },
  { "en": "Apa Itu Gerbang (Gate) NOT (Inverter)?", "id": "Membalikkan Level Logika Input." },
  { "en": "Apa Itu Gerbang (Gate) NAND?", "id": "Inversi Dari Gerbang AND." },
  { "en": "Apa Itu Gerbang (Gate) NOR?", "id": "Inversi Dari Gerbang OR." },
  { "en": "Apa Itu Gerbang (Gate) XOR (Exclusive OR)?", "id": "Output 1 Jika Input Berbeda." },
  { "en": "Apa Itu Aljabar (Algebra) Boolean?", "id": "Matematika Untuk Logika Digital." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Komponen Pengubah Level Tegangan AC." },
  { "en": "Apa Prinsip (Principle) Kerja Transformator?", "id": "Induksi Mutual Elektromagnetik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Mengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Jaringan Listrik Pembangkit Hingga Konsumen." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keselamatan." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih (Sekali Pakai)." },
  { "en": "Apa Itu Pemutus (Breaker) Sirkuit?", "id": "Pelindung Arus Lebih (Bisa Reset)." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Mudah Hantarkan Listrik." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Sulit Hantarkan Listrik." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Sifat Antara Konduktor Isolator." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Aliran Daya Listrik." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Pengubah Daya DC Ke AC." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Loop (Loop) Tertutup Kontrol?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Deteksi Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Penggerak Mekanis." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Logika Terprogram Industri." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Alamat Logis Perangkat Di Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Kombinasi Listrik, Magnet." },
  { "en": "Apa Itu Gelombang (Wave) Elektromagnetik?", "id": "Getaran Medan Listrik, Magnet Merambat." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Listrik (Electricity) Statis?", "id": "Akumulasi Muatan Listrik Diam." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Hambatan Spesifik Suatu Material." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Kemudahan Material Hantarkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Dielektrik?", "id": "Bahan Isolator Dalam Kapasitor." },
  { "en": "Apa Itu Kekuatan (Strength) Dielektrik?", "id": "Medan Listrik Maksimum Sebelum Tembus." },
  { "en": "Apa Itu Bahan (Material) Magnetik?", "id": "Bahan Berinteraksi Dengan Medan Magnet." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Ketergantungan Magnetisasi Riwayat Medan." },
  { "en": "Apa Itu Efek (Effect) Hall?", "id": "Tegangan Timbul Konduktor Medan Magnet." },
  { "en": "Apa Itu Efek (Effect) Termoelektrik?", "id": "Konversi Langsung Panas Ke Listrik." },
  { "en": "Apa Itu Efek (Effect) Piezoelektrik?", "id": "Listrik Dihasilkan Tekanan Mekanis." },
  { "en": "Apa Itu Efek (Effect) Fotolistrik?", "id": "Elektron Dipancarkan Akibat Cahaya." },
  { "en": "Apa Itu Sel (Cell) Surya?", "id": "Perangkat Pengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Resistansi Nol Suhu Rendah." },
  { "en": "Apa Itu Sirkuit (Circuit) Terpadu (IC)?", "id": "Rangkaian Elektronik Dalam Satu Chip." },
  { "en": "Apa Itu Fabrikasi (Fabrication) IC?", "id": "Proses Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Fotolitografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit IC." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Impuritas Atur Konduktivitas." },
  { "en": "Apa Itu Sambungan (Junction) P-N?", "id": "Dasar Komponen Semikonduktor Dioda." },
  { "en": "Apa Itu Daerah (Region) Deplesi?", "id": "Area Sekitar Sambungan Kurang Muatan." },
  { "en": "Apa Itu Tegangan (Voltage) Maju (Vf) Dioda?", "id": "Jatuh Tegangan Saat Konduksi." },
  { "en": "Apa Itu Penyearah (Rectifier) Jembatan?", "id": "Empat Dioda Penyearah Gelombang Penuh." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Dioda Penstabil Tegangan Mundur." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Sensor Intensitas Cahaya." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Penguat/Saklar Terkontrol Arus Basis." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Penguat/Saklar Terkontrol Tegangan Gate." },
  { "en": "Apa Itu Penguat (Amplifier) Op-Amp?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Gerbang (Gate) Logika?", "id": "Komponen Dasar Sirkuit Digital." },
  { "en": "Apa Itu Aljabar (Algebra) Boolean?", "id": "Matematika Logika Biner (0/1)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Digital." },
  { "en": "Apa Itu Memori (Memory)?", "id": "Perangkat Penyimpanan Data Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil (Hilang Tanpa Daya)." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil (Data Tetap)." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Pengubah Sinyal Listrik Ke Aksi." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Jaringan Pembangkitan Hingga Distribusi." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Tegangan Arus Bolak-Balik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Konversi Daya Semikonduktor." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Output Pengaruhi Input." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Hasil Ukur Nilai Benar." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Keterulangan Hasil Ukur Sama." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Visualisasi Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Kombinasi Listrik, Magnet." },
  { "en": "Apa Itu Gelombang (Wave) Elektromagnetik?", "id": "Perambatan Energi Medan Elektromagnetik." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Komunikasi (Communication)?", "id": "Proses Pertukaran Informasi." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Antar Perangkat." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Pengenal Logis Unik Di Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Mudah Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Sulit Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Konduktivitas Antara." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Searah Arus Semikonduktor." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Penguat/Saklar Semikonduktor." },
  { "en": "Apa Itu Hukum (Law) Ohm?", "id": "V = I * R." },
  { "en": "Apa Itu Hukum (Law) Arus Kirchhoff?", "id": "Jumlah Arus Masuk Titik Nol." },
  { "en": "Apa Itu Hukum (Law) Tegangan Kirchhoff?", "id": "Jumlah Tegangan Loop Tertutup Nol." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Transfer Energi Listrik (Watt)." },
  { "en": "Apa Rumus (Formula) Daya Listrik?", "id": "P = V * I = I² * R = V² / R." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Komponen Penyimpan Energi Medan Magnet." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Total Hambatan Rangkaian AC." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Kapasitor/Induktor Di AC." },
  { "en": "Apa Itu Respons (Response) Frekuensi?", "id": "Perilaku Rangkaian Vs Frekuensi." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Frekuensi Reaktansi Saling Menghilangkan." },
  { "en": "Apa Itu Filter Lolos Band (Band Pass)?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Stop Band (Band Stop)?", "id": "Menolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Penguatan (Gain) Desibel (dB)?", "id": "Ukuran Logaritmik Rasio Daya/Amplitudo." },
  { "en": "Apa Itu Sistem (System) Linear?", "id": "Sistem Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Transformasi (Transform) Laplace?", "id": "Alat Analisis Sistem Domain-s." },
  { "en": "Apa Itu Transformasi (Transform) Z?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem?", "id": "Kemampuan Sistem Kembali Normal." },
  { "en": "Apa Itu Sirkuit (Circuit) Digital?", "id": "Sirkuit Bekerja Level Logika Biner." },
  { "en": "Apa Itu Tabel (Table) Kebenaran?", "id": "Menunjukkan Output Semua Kombinasi Input." },
  { "en": "Apa Itu Logika (Logic) Kombinasional?", "id": "Output Hanya Tergantung Input Saat Ini." },
  { "en": "Apa Itu Logika (Logic) Sekuensial?", "id": "Output Tergantung Input, Keadaan Internal." },
  { "en": "Apa Itu Sinyal (Signal) Clock?", "id": "Sinyal Sinkronisasi Rangkaian Sekuensial." },
  { "en": "Apa Itu Tepi (Edge) Naik Clock?", "id": "Transisi Sinyal Clock Dari Rendah Ke Tinggi." },
  { "en": "Apa Itu Tepi (Edge) Turun Clock?", "id": "Transisi Sinyal Clock Dari Tinggi Ke Rendah." },
  { "en": "Apa Itu Memori (Memory) Statis (SRAM)?", "id": "Memori RAM (Random Access Memory) Berbasis Flip-Flop." },
  { "en": "Apa Itu Memori (Memory) Dinamis (DRAM)?", "id": "Memori RAM (Random Access Memory) Berbasis Kapasitor." },
  { "en": "Apa Perlu Refresh (Refresh) DRAM?", "id": "Ya, Kapasitor Kehilangan Muatan." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "Chip Logika Dapat Dikonfigurasi Ulang." },
  { "en": "Apa Itu ASIC (Application Specific Integrated Circuit)?", "id": "Chip Dirancang Khusus Aplikasi." },
  { "en": "Apa Itu Prosesor (Processor) Sinyal Digital (DSP)?", "id": "Mikroprosesor Optimasi Operasi DSP." },
  { "en": "Apa Itu Konverter (Converter) Analog Ke Digital?", "id": "ADC (Analog-to-Digital Converter)." },
  { "en": "Apa Itu Konverter (Converter) Digital Ke Analog?", "id": "DAC (Digital-to-Analog Converter)." },
  { "en": "Apa Itu Resolusi (Resolution) ADC/DAC?", "id": "Jumlah Bit Output/Input Digital." },
  { "en": "Apa Itu Laju (Rate) Sampling ADC?", "id": "Frekuensi Pengambilan Sampel Analog." },
  { "en": "Apa Itu Motor (Motor) Stepper?", "id": "Motor Bergerak Dalam Langkah Sudut." },
  { "en": "Apa Itu Motor (Motor) Servo?", "id": "Motor Kontrol Posisi Loop Tertutup." },
  { "en": "Apa Itu Sistem (System) Tenaga Tiga Fasa?", "id": "Sistem AC Tiga Gelombang Terpisah 120°." },
  { "en": "Apa Keuntungan (Advantage) Tiga Fasa?", "id": "Efisiensi Transmisi, Daya Konstan." },
  { "en": "Apa Hubungan (Connection) Bintang (Y)?", "id": "Ujung Sama Tiga Fasa Terhubung Netral." },
  { "en": "Apa Hubungan (Connection) Delta (Δ)?", "id": "Ujung Fasa Terhubung Fasa Berikutnya." },
  { "en": "Apa Itu Tegangan (Voltage) Fasa?", "id": "Tegangan Antara Satu Fasa, Netral." },
  { "en": "Apa Itu Tegangan (Voltage) Line?", "id": "Tegangan Antara Dua Fasa Berbeda." },
  { "en": "Hubungan V_Line, V_Fasa Di Bintang?", "id": "V_Line = √3 * V_Fasa." },
  { "en": "Hubungan I_Line, I_Fasa Di Delta?", "id": "I_Line = √3 * I_Fasa." },
  { "en": "Apa Itu Daya (Power) Kompleks?", "id": "Representasi Fasor Daya AC (P+jQ)." },
  { "en": "Apa Itu Segitiga (Triangle) Daya?", "id": "Representasi Grafis Hubungan P, Q, S." },
  { "en": "Apa Itu Kompensasi (Compensation) Daya Reaktif?", "id": "Menambah Kapasitor/Induktor Perbaiki PF." },
  { "en": "Apa Itu Analisis (Analysis) Gangguan Sistem Tenaga?", "id": "Menghitung Arus, Tegangan Saat Gangguan." },
  { "en": "Apa Itu Komponen (Component) Simetris?", "id": "Metode Analisis Gangguan Asimetris." },
  { "en": "Apa Tiga Komponen (Component) Simetris?", "id": "Urutan Positif, Negatif, Nol." },
  { "en": "Apa Itu Stabilitas (Stability) Sistem Tenaga?", "id": "Kemampuan Sistem Kembali Normal." },
  { "en": "Apa Jenis (Type) Stabilitas Utama?", "id": "Sudut Rotor, Tegangan, Frekuensi." },
  { "en": "Apa Itu Stabilitas (Stability) Sudut Rotor?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas (Stability) Tegangan?", "id": "Kemampuan Sistem Jaga Level Tegangan." },
  { "en": "Apa Itu Stabilitas (Stability) Frekuensi?", "id": "Kemampuan Sistem Jaga Frekuensi Konstan." },
  { "en": "Apa Itu Kendali (Control) Beban Frekuensi (LFC)?", "id": "Mengatur Pembangkitan Jaga Frekuensi." },
  { "en": "Apa Itu Kendali (Control) Tegangan Otomatis (AVR)?", "id": "Mengatur Eksitasi Jaga Tegangan Generator." },
  { "en": "Apa Itu FACTS (Flexible AC Transmission Systems)?", "id": "Peralatan Elektronika Daya Kontrol Transmisi." },
  { "en": "Apa Itu HVDC (High Voltage Direct Current)?", "id": "Transmisi Daya Arus Searah Jarak Jauh." },
  { "en": "Apa Itu SCADA (Supervisory Control and Data Acquisition)?", "id": "Sistem Pemantauan, Kontrol Jarak Jauh." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Jaringan Listrik Modern Digitalisasi." },
  { "en": "Apa Itu Pengukuran (Measurement) Fase Sinkron (PMU)?", "id": "Phasor Measurement Unit (PMU)." },
  { "en": "Apa Fungsi PMU (Phasor Measurement Unit)?", "id": "Pemantauan Keadaan Sistem Real-Time." },
  { "en": "Apa Itu Elektronika (Electronics) Analog?", "id": "Sirkuit Memproses Sinyal Kontinu." },
  { "en": "Apa Itu Elektronika (Electronics) Digital?", "id": "Sirkuit Memproses Sinyal Diskrit." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Meningkatkan Amplitudo Sinyal." },
  { "en": "Apa Itu Penguatan (Gain)?", "id": "Rasio Output Terhadap Input Penguat." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengembalikan Sebagian Output Ke Input." },
  { "en": "Apa Itu Umpan Balik (Feedback) Negatif?", "id": "Menstabilkan Gain, Mengurangi Distorsi." },
  { "en": "Apa Itu Umpan Balik (Feedback) Positif?", "id": "Menyebabkan Osilasi (Membuat Osilator)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Seleksi Frekuensi." },
  { "en": "Apa Itu Filter (Filter) Lolos Rendah?", "id": "Melewatkan Frekuensi Di Bawah Cutoff." },
  { "en": "Apa Itu Filter (Filter) Lolos Tinggi?", "id": "Melewatkan Frekuensi Di Atas Cutoff." },
  { "en": "Apa Itu Filter (Filter) Lolos Pita?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter (Filter) Tolak Pita?", "id": "Menolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Searah Arus." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Pengubah Arus AC Menjadi DC." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Saklar/Penguat Terkontrol Arus Basis." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Saklar/Penguat Terkontrol Tegangan Gate." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Gerbang (Gate) Logika?", "id": "Blok Bangunan Dasar Sirkuit Digital." },
  { "en": "Apa Itu Logika (Logic) Boolean?", "id": "Aljabar Untuk Operasi Biner." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Penyimpan Satu Bit." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil Baca Tulis." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil Hanya Baca." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Sistem Komputer Kecil Dalam Chip." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Pengubah Sinyal Listrik Ke Aksi." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Konversi Daya Listrik." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Kombinasi Listrik, Magnet." },
  { "en": "Apa Itu Gelombang (Wave) Elektromagnetik?", "id": "Perambatan Energi Medan EM." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Komunikasi (Communication) Data?", "id": "Transfer Informasi Digital." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Jaringan." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Penghubung Antar Jaringan." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Mudah Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Sulit Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Konduktivitas Antara." },
  { "en": "Apa Itu Hukum (Law) Ohm?", "id": "V = I * R." },
  { "en": "Apa Itu Hukum (Law) Kirchhoff?", "id": "Hukum Arus Dan Tegangan." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Energi Listrik (Watt)." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Penyimpan Energi Magnetik." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total AC." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Non-Resistif AC." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Frekuensi Reaktansi Saling Hapus." },
  { "en": "Apa Itu Sinyal (Signal) Analog?", "id": "Sinyal Nilai Kontinu." },
  { "en": "Apa Itu Sinyal (Signal) Digital?", "id": "Sinyal Nilai Diskrit." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Gangguan Acak Sinyal." },
  { "en": "Apa Itu Pemrosesan (Processing) Sinyal?", "id": "Analisis, Modifikasi Sinyal." },
  { "en": "Apa Itu Transformasi (Transform) Fourier?", "id": "Analisis Frekuensi Sinyal." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Robot." },
  { "en": "Apa Itu Kecerdasan (Intelligence) Buatan (AI)?", "id": "Mesin Tiru Kecerdasan Manusia." },
  { "en": "Apa Itu Pembelajaran (Learning) Mesin (ML)?", "id": "AI (Artificial Intelligence) Belajar Dari Data." },
  { "en": "Apa Itu Internet (Internet) of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung." },
  { "en": "Apa Itu Keamanan (Security) Siber?", "id": "Perlindungan Sistem Digital." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Pengkodean Data Agar Aman." },
  { "en": "Apa Itu Energi (Energy) Terbarukan?", "id": "Sumber Energi Alam Berkelanjutan." },
  { "en": "Apa Itu Sel (Cell) Surya?", "id": "Pengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Turbin (Turbine) Angin?", "id": "Pengubah Energi Angin Listrik." },
  { "en": "Apa Itu Penyimpanan (Storage) Energi?", "id": "Menyimpan Energi Listrik (Baterai)." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Jaringan Listrik Digital Komunikasi." },
  { "en": "Apa Itu Kendaraan (Vehicle) Listrik?", "id": "Kendaraan Penggerak Motor Listrik." },
  { "en": "Apa Itu Pencitraan (Imaging) Medis?", "id": "Visualisasi Dalam Tubuh (MRI, CT)." },
  { "en": "Apa Itu Elektronika (Electronics) Biomedis?", "id": "Elektronika Aplikasi Medis." },
  { "en": "Apa Itu Sensor (Sensor) Biomedis?", "id": "Sensor Ukur Sinyal Tubuh (ECG)." },
  { "en": "Apa Itu Sinyal (Signal) ECG?", "id": "Rekaman Aktivitas Listrik Jantung." },
  { "en": "Apa Itu Sinyal (Signal) EEG?", "id": "Rekaman Aktivitas Listrik Otak." },
  { "en": "Apa Itu Sinyal (Signal) EMG?", "id": "Rekaman Aktivitas Listrik Otot." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Perangkat Stimulasi Ritme Jantung." },
  { "en": "Apa Itu Defibrilator (Defibrillator)?", "id": "Alat Kejut Listrik Atasi Aritmia." },
  { "en": "Apa Itu Teknik (Engineering) Material?", "id": "Studi Sifat, Aplikasi Material." },
  { "en": "Apa Itu Logam (Metal)?", "id": "Konduktif, Kuat, Ulet." },
  { "en": "Apa Itu Keramik (Ceramic)?", "id": "Keras, Rapuh, Tahan Panas, Isolator." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Molekul Rantai Panjang (Plastik)." },
  { "en": "Apa Itu Komposit (Composite)?", "id": "Gabungan Dua Material Berbeda." },
  { "en": "Apa Itu Mekatronika (Mechatronics)?", "id": "Integrasi Mekanik, Elektronik, Kontrol." },
  { "en": "Apa Itu Termodinamika (Thermodynamics)?", "id": "Studi Hubungan Panas, Kerja, Energi." },
  { "en": "Apa Itu Perpindahan (Heat Transfer) Panas?", "id": "Pergerakan Energi Termal." },
  { "en": "Apa Itu Mekanika (Mechanics) Fluida?", "id": "Studi Perilaku Fluida." },
  { "en": "Apa Itu Energi Kinetik (Kinetic Energy)?", "id": "Energi Yang Dimiliki Benda Bergerak." },
  { "en": "Apa Rumus Energi Kinetik (Kinetic Energy)?", "id": "EK = ½ * m * v²." },
  { "en": "Apa Itu Energi Potensial (Potential Energy)?", "id": "Energi Yang Dimiliki Benda Posisi." },
  { "en": "Apa Rumus Energi Potensial (Potential Energy) Gravitasi?", "id": "EP = m * g * h." },
  { "en": "Apa Itu Hukum Kekekalan Energi (Conservation of Energy)?", "id": "Energi Tidak Dapat Diciptakan/Dimusnahkan." },
  { "en": "Apa Itu Efisiensi (Efficiency)?", "id": "Rasio Energi Output Berguna Terhadap Input." },
  { "en": "Mengapa Efisiensi (Efficiency) Selalu Kurang Dari 100 Persen?", "id": "Selalu Ada Energi Hilang (Panas)." },
  { "en": "Apa Itu Daya (Power)?", "id": "Laju Energi Ditransfer (Energi/Waktu)." },
  { "en": "Apa Satuan Daya (Power)?", "id": "Watt (W)." },
  { "en": "Apa Hubungan Daya (Power), Energi (Energy), Waktu (Time)?", "id": "Daya = Energi / Waktu." },
  { "en": "Apa Itu Torsi (Torque)?", "id": "Gaya Putar Pada Suatu Benda." },
  { "en": "Apa Satuan Torsi (Torque) (SI)?", "id": "Newton-Meter (N·m)." },
  { "en": "Apa Hubungan Daya (Power), Torsi (Torque), Kecepatan Sudut (ω)?", "id": "P = τ * ω." },
  { "en": "Apa Itu Kecepatan Sudut (Angular Velocity) (ω)?", "id": "Laju Perubahan Sudut (Radian/Detik)." },
  { "en": "Apa Hubungan RPM (Revolutions Per Minute) Dan Radian/Detik?", "id": "ω = RPM * 2π / 60." },
  { "en": "Apa Itu Kawat Fasa (Phase Wire)?", "id": "Konduktor Pembawa Tegangan Listrik (Live)." },
  { "en": "Apa Itu Kawat Netral (Neutral Wire)?", "id": "Konduktor Kembali Arus (Biasanya 0V)." },
  { "en": "Apa Itu Kawat Ground (Ground Wire) (Arde)?", "id": "Konduktor Pengaman Terhubung Ke Bumi." },
  { "en": "Apa Fungsi Kawat Ground (Ground Wire)?", "id": "Mencegah Sengatan Listrik (Jalur Bocor)." },
  { "en": "Apa Warna Standar Kabel Fasa (PUIL)?", "id": "Hitam, Cokelat, Abu-Abu." },
  { "en": "Apa Warna Standar Kabel Netral (PUIL)?", "id": "Biru." },
  { "en": "Apa Warna Standar Kabel Ground (PUIL)?", "id": "Hijau-Kuning." },
  { "en": "Apa Itu Sistem (System) Fasa Tunggal (Single Phase)?", "id": "Sistem Listrik Satu Gelombang AC." },
  { "en": "Apa Itu Sistem (System) Tiga Fasa (Three Phase)?", "id": "Sistem Listrik Tiga Gelombang AC." },
  { "en": "Berapa Perbedaan Fasa (Phase Difference) Antar Fasa?", "id": "Seratus Dua Puluh Derajat (120°)." },
  { "en": "Apa Keuntungan Sistem (System) Tiga Fasa?", "id": "Transmisi Daya Lebih Efisien." },
  { "en": "Apa Itu Koneksi (Connection) Bintang (Y)?", "id": "Titik Netral Bersama Terhubung." },
  { "en": "Apa Itu Koneksi (Connection) Delta (Δ)?", "id": "Koneksi Bentuk Segitiga (Tanpa Netral)." },
  { "en": "Apa Itu Tegangan (Voltage) Saluran (Line)?", "id": "Tegangan Antara Dua Saluran Fasa." },
  { "en": "Apa Itu Tegangan (Voltage) Fasa?", "id": "Tegangan Antara Satu Fasa, Netral." },
  { "en": "Apa Itu Arus (Current) Saluran (Line)?", "id": "Arus Mengalir Di Saluran Fasa." },
  { "en": "Apa Itu Arus (Current) Fasa?", "id": "Arus Mengalir Lewat Beban Fasa." },
  { "en": "Apa Itu Beban (Load) Seimbang Tiga Fasa?", "id": "Impedansi Ketiga Fasa Sama Persis." },
  { "en": "Apa Arus (Current) Di Netral Beban Seimbang?", "id": "Nol Ampere (Idealnya)." },
  { "en": "Apa Itu Transformator (Transformer) Tiga Fasa?", "id": "Mengubah Tegangan Sistem Tiga Fasa." },
  { "en": "Apa Itu Pembangkit (Generator) Listrik?", "id": "Mengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Prinsip (Principle) Dasar Generator?", "id": "Induksi Elektromagnetik (Hukum Faraday)." },
  { "en": "Apa Bagian (Part) Utama Generator AC?", "id": "Stator (Diam) Dan Rotor (Berputar)." },
  { "en": "Apa Itu Stator (Stator)?", "id": "Bagian Diam (Kumparan Jangkar Output)." },
  { "en": "Apa Itu Rotor (Rotor)?", "id": "Bagian Berputar (Kumparan Medan/Magnet)." },
  { "en": "Apa Itu Alternator (Alternator)?", "id": "Nama Lain Generator AC Sinkron." },
  { "en": "Apa Itu Sistem (System) Eksitasi Generator?", "id": "Sistem Pembangkit Medan Magnet Rotor." },
  { "en": "Apa Fungsi Sistem (System) Eksitasi?", "id": "Mengontrol Tegangan Output Generator." },
  { "en": "Apa Itu AVR (Automatic Voltage Regulator)?", "id": "Regulator Tegangan Otomatis Generator." },
  { "en": "Apa Itu Penggerak (Prime Mover) Utama Generator?", "id": "Sumber Energi Mekanis (Turbin)." },
  { "en": "Apa Itu Turbin (Turbine) Uap?", "id": "Penggerak Utama PLTU (Pembangkit Listrik Tenaga Uap)." },
  { "en": "Apa Itu Turbin (Turbine) Air?", "id": "Penggerak Utama PLTA (Pembangkit Listrik Tenaga Air)." },
  { "en": "Apa Itu Turbin (Turbine) Gas?", "id": "Penggerak Utama PLTG (Pembangkit Listrik Tenaga Gas)." },
  { "en": "Apa Itu Turbin (Turbine) Pelton?", "id": "Turbin Air Tipe Impuls (Head Tinggi)." },
  { "en": "Apa Itu Turbin (Turbine) Francis?", "id": "Turbin Air Tipe Reaksi (Head Menengah)." },
  { "en": "Apa Itu Turbin (Turbine) Kaplan?", "id": "Turbin Air Tipe Reaksi (Head Rendah)." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Mengubah Energi Listrik Ke Mekanik." },
  { "en": "Prinsip (Principle) Dasar Motor Listrik?", "id": "Gaya Lorentz (Gaya Magnetik)." },
  { "en": "Apa Itu Motor (Motor) Induksi?", "id": "Motor AC (Alternating Current) Asinkron." },
  { "en": "Mengapa Disebut Asinkron (Asynchronous)?", "id": "Kecepatan Rotor Dibawah Kecepatan Sinkron." },
  { "en": "Apa Itu Kecepatan (Speed) Sinkron (Ns)?", "id": "Kecepatan Putaran Medan Magnet Stator." },
  { "en": "Apa Rumus Kecepatan (Speed) Sinkron?", "id": "Ns = (120 * Frekuensi) / Jumlah Kutub." },
  { "en": "Apa Itu Slip (Slip) Motor Induksi?", "id": "Perbedaan Persentase Kecepatan Sinkron, Rotor." },
  { "en": "Apa Itu Rotor (Rotor) Sangkar Tupai?", "id": "Jenis Rotor Motor Induksi Umum." },
  { "en": "Apa Itu Motor (Motor) Sinkron?", "id": "Motor AC (Alternating Current) Kecepatan Rotor = Sinkron." },
  { "en": "Aplikasi Motor (Motor) Sinkron?", "id": "Beban Kecepatan Konstan, Koreksi Faktor Daya." },
  { "en": "Apa Itu Motor (Motor) DC?", "id": "Motor (Motor) Arus Searah (Direct Current)." },
  { "en": "Bagian Kunci Motor (Motor) DC Ber-sikat?", "id": "Komutator (Commutator) Dan Sikat (Brush)." },
  { "en": "Apa Fungsi Komutator (Commutator)?", "id": "Membalik Arah Arus Jangkar Rotor." },
  { "en": "Apa Jenis (Type) Motor DC?", "id": "Seri, Shunt (Paralel), Kompon (Gabungan)." },
  { "en": "Karakteristik (Characteristic) Motor DC Seri?", "id": "Torsi Start Sangat Tinggi." },
  { "en": "Karakteristik (Characteristic) Motor DC Shunt?", "id": "Kecepatan Relatif Konstan." },
  { "en": "Apa Itu Motor (Motor) Universal?", "id": "Motor (Motor) Seri Dapat Beroperasi AC/DC." },
  { "en": "Dimana Motor (Motor) Universal Digunakan?", "id": "Alat Rumah Tangga (Bor, Blender)." },
  { "en": "Apa Itu Motor (Motor) BLDC?", "id": "Brushless DC (Direct Current) Motor (Tanpa Sikat)." },
  { "en": "Apa Keuntungan Motor (Motor) BLDC?", "id": "Efisien Tinggi, Awet (Tanpa Perawatan Sikat)." },
  { "en": "Apa Itu Motor (Motor) Stepper?", "id": "Motor (Motor) Bergerak Per Langkah Sudut." },
  { "en": "Aplikasi Motor (Motor) Stepper?", "id": "Printer 3D, Mesin CNC (Computer Numerical Control)." },
  { "en": "Apa Itu Motor (Motor) Servo?", "id": "Motor (Motor) Kontrol Posisi Loop Tertutup." },
  { "en": "Apa Komponen (Component) Umpan Balik Servo?", "id": "Encoder Atau Potensiometer." },
  { "en": "Apa Itu Gardu (Substation) Induk?", "id": "Fasilitas Sistem Tenaga Ubah Tegangan." },
  { "en": "Komponen (Component) Utama Gardu Induk?", "id": "Trafo, Pemutus, Pemisah, Busbar, Proteksi." },
  { "en": "Apa Itu Busbar (Busbar)?", "id": "Konduktor Utama Titik Kumpul Energi." },
  { "en": "Apa Itu Pemutus (Breaker) Tenaga (PMT)?", "id": "Saklar Otomatis Pemutus Arus Gangguan." },
  { "en": "Media Pemadam (Quenching) Busur Api PMT?", "id": "Udara, Minyak, Vakum, Gas SF6." },
  { "en": "Apa Kepanjangan SF6 (Sulfur Hexafluoride)?", "id": "Sulfur Heksafluorida (Gas Isolasi Kuat)." },
  { "en": "Apa Itu Pemisah (Disconnector) (PMS)?", "id": "Saklar Pemisah Tanpa Beban Arus." },
  { "en": "Apa Itu Trafo (Transformer) Arus (CT)?", "id": "Menurunkan Arus Tinggi Untuk Pengukuran/Proteksi." },
  { "en": "Apa Itu Trafo (Transformer) Tegangan (PT/VT)?", "id": "Menurunkan Tegangan Tinggi Pengukuran/Proteksi." },
  { "en": "Apa Itu Arester (Arrester) Surja?", "id": "Pelindung Peralatan Dari Tegangan Lebih Petir." },
  { "en": "Apa Itu Sistem (System) Proteksi?", "id": "Sistem Deteksi Gangguan, Isolasi Gangguan." },
  { "en": "Apa Itu Rele (Relay) Proteksi?", "id": "Perangkat Pendeteksi Kondisi Abnormal." },
  { "en": "Apa Itu Rele (Relay) Arus Lebih?", "id": "Overcurrent Relay (OCR)." },
  { "en": "Apa Itu Rele (Relay) Diferensial?", "id": "Membandingkan Arus Masuk, Keluar Peralatan." },
  { "en": "Apa Itu Rele (Relay) Jarak?", "id": "Mengukur Impedansi (Jarak) Gangguan Saluran." },
  { "en": "Apa Itu Rele (Relay) Gangguan Tanah?", "id": "Mendeteksi Arus Yang Bocor Ke Tanah." },
  { "en": "Apa Itu Sistem (System) SCADA?", "id": "Supervisory Control And Data Acquisition." },
  { "en": "Fungsi (Function) SCADA?", "id": "Pemantauan, Kontrol Sistem Jarak Jauh." },
  { "en": "Apa Itu RTU (Remote Terminal Unit)?", "id": "Unit Terminal Jarak Jauh (Akuisisi Data)." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Antarmuka Grafis Operator." },
  { "en": "Apa Itu Kualitas (Quality) Daya Listrik?", "id": "Karakteristik Tegangan, Arus Ideal." },
  { "en": "Masalah (Problem) Kualitas Daya Umum?", "id": "Sag, Swell, Harmonisa, Flicker." },
  { "en": "Apa Itu Sag (Sag) Tegangan?", "id": "Penurunan Tegangan Sesaat." },
  { "en": "Apa Itu Swell (Swell) Tegangan?", "id": "Kenaikan Tegangan Sesaat." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Fundamental." },
  { "en": "Penyebab (Cause) Harmonisa?", "id": "Beban Non-Linear (Elektronika Daya)." },
  { "en": "Apa Itu Flicker (Flicker) Tegangan?", "id": "Fluktuasi Tegangan Sebabkan Lampu Berkedip." },
  { "en": "Apa Itu Faktor (Factor) Daya?", "id": "Rasio Daya Nyata, Daya Semu." },
  { "en": "Apa Itu Koreksi (Correction) Faktor Daya?", "id": "Memperbaiki Faktor Daya (Pasang Kapasitor)." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Cadangan Darurat." },
  { "en": "Apa Fungsi Osiloskop (Oscilloscope)?", "id": "Menampilkan Bentuk Gelombang Sinyal." },
  { "en": "Apa Sumbu Vertikal (Vertical Axis) Osiloskop?", "id": "Tegangan (Volt)." },
  { "en": "Apa Sumbu Horizontal (Horizontal Axis) Osiloskop?", "id": "Waktu (Time)." },
  { "en": "Apa Fungsi Tombol Volt/Div?", "id": "Mengatur Skala Tegangan Vertikal." },
  { "en": "Apa Fungsi Tombol Time/Div?", "id": "Mengatur Skala Waktu Horizontal." },
  { "en": "Apa Itu Pemicuan (Triggering) Osiloskop?", "id": "Menstabilkan Tampilan Gelombang Berulang." },
  { "en": "Apa Itu Mode AC Coupling (Kopling AC)?", "id": "Hanya Menampilkan Komponen AC Sinyal." },
  { "en": "Apa Itu Mode DC Coupling (Kopling DC)?", "id": "Menampilkan Komponen AC Dan DC Sinyal." },
  { "en": "Apa Fungsi Probe (Probe) Osiloskop?", "id": "Menghubungkan Rangkaian Ke Input Osiloskop." },
  { "en": "Apa Arti Probe (Probe) 10X?", "id": "Melemahkan Sinyal 10 Kali Lipat." },
  { "en": "Mengapa Menggunakan Probe (Probe) 10X?", "id": "Impedansi Input Tinggi, Beban Rendah." },
  { "en": "Apa Itu Bandwidth (Bandwidth) Osiloskop?", "id": "Frekuensi Maksimum Dapat Diukur Akurat." },
  { "en": "Apa Itu Osiloskop Digital (DSO)?", "id": "Digital Storage Oscilloscope (DSO)." },
  { "en": "Apa Keuntungan Osiloskop Digital (DSO)?", "id": "Penyimpanan, Analisis, Pengukuran Otomatis." },
  { "en": "Apa Itu Laju Sampel (Sample Rate) DSO?", "id": "Kecepatan ADC (Analog-to-Digital Converter) Mengambil Sampel." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Besaran Apa Diukur Multimeter (Multimeter)?", "id": "Tegangan, Arus, Dan Resistansi." },
  { "en": "Bagaimana Cara Mengukur Tegangan (Voltage)?", "id": "Hubungkan Paralel Dengan Komponen." },
  { "en": "Bagaimana Cara Mengukur Arus (Current)?", "id": "Hubungkan Seri Dengan Rangkaian." },
  { "en": "Bagaimana Cara Mengukur Resistansi (Resistance)?", "id": "Ukur Komponen (Daya Rangkaian Mati)." },
  { "en": "Apa Itu True RMS (True RMS) Multimeter?", "id": "Mengukur Nilai Efektif Akurat Non-Sinus." },
  { "en": "Apa Itu Tang Ampere (Clamp Meter)?", "id": "Mengukur Arus Tanpa Memutus Sirkuit." },
  { "en": "Prinsip Kerja Tang Ampere (Clamp Meter)?", "id": "Induksi Magnetik (Transformator Arus)." },
  { "en": "Apa Itu Generator Sinyal (Signal Generator)?", "id": "Alat Penghasil Sinyal Uji." },
  { "en": "Bentuk Gelombang Apa Dihasilkan Generator Sinyal?", "id": "Sinus, Kotak, Segitiga, Gigi Gergaji." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Menampilkan Sinyal Domain Frekuensi." },
  { "en": "Sumbu Horizontal (Horizontal Axis) Penganalisis Spektrum?", "id": "Frekuensi (Frequency)." },
  { "en": "Sumbu Vertikal (Vertical Axis) Penganalisis Spektrum?", "id": "Amplitudo (Daya, dBm)." },
  { "en": "Apa Itu Catu Daya (Power Supply) DC?", "id": "Sumber Tegangan Searah Teregulasi." },
  { "en": "Apa Itu Mode CV (Constant Voltage) Catu Daya?", "id": "Menjaga Tegangan Output Konstan." },
  { "en": "Apa Itu Mode CC (Constant Current) Catu Daya?", "id": "Menjaga Arus Output Konstan." },
  { "en": "Apa Itu LCR Meter (LCR Meter)?", "id": "Mengukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Apa Itu Pengujian Isolasi (Insulation Test)?", "id": "Mengukur Resistansi Isolasi Sangat Tinggi." },
  { "en": "Alat Apa Untuk Pengujian Isolasi?", "id": "Megohmmeter (Megger)." },
  { "en": "Apa Itu Pengujian Pembumian (Earth Test)?", "id": "Mengukur Resistansi Sistem Pembumian." },
  { "en": "Apa Itu Transformasi Bintang-Delta (Y-Δ)?", "id": "Konversi Ekuivalen Jaringan Resistor/Impedansi." },
  { "en": "Apa Itu Teorema Superposisi (Superposition Theorem)?", "id": "Analisis Rangkaian Linear Multi Sumber." },
  { "en": "Apa Itu Teorema Thevenin (Thevenin's Theorem)?", "id": "Sederhanakan Rangkaian Jadi Vth, Rth Seri." },
  { "en": "Apa Itu Tegangan Thevenin (Vth)?", "id": "Tegangan Rangkaian Terbuka Terminal Beban." },
  { "en": "Apa Itu Resistansi Thevenin (Rth)?", "id": "Resistansi Ekuivalen Dilihat Dari Terminal." },
  { "en": "Apa Itu Teorema Norton (Norton's Theorem)?", "id": "Sederhanakan Rangkaian Jadi In, Rn Paralel." },
  { "en": "Apa Itu Arus Norton (In)?", "id": "Arus Hubung Singkat Terminal Beban." },
  { "en": "Apa Hubungan Rth Dan Rn?", "id": "Resistansi Thevenin Sama Resistansi Norton." },
  { "en": "Apa Hubungan Vth, In, Rth?", "id": "Vth = In * Rth." },
  { "en": "Apa Teorema Transfer Daya Maksimum?", "id": "RL = Rth (DC), ZL = Zth* (AC)." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Perilaku Rangkaian Terhadap Input Frekuensi." },
  { "en": "Apa Itu Diagram Bode (Bode Plot)?", "id": "Plot Logaritmik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Frekuensi Reaktansi Saling Menghilangkan." },
  { "en": "Apa Itu Faktor Kualitas (Q Factor)?", "id": "Ukuran Tajamnya Puncak Resonansi." },
  { "en": "Apa Itu Jaringan Dua Port (Two-Port Network)?", "id": "Rangkaian Listrik Dua Pasang Terminal." },
  { "en": "Parameter Apa Jaringan Dua Port?", "id": "Parameter Z, Y, h, ABCD, S." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda Sambungan P-N (P-N Junction)?", "id": "Komponen Semikonduktor Searah Arus." },
  { "en": "Apa Itu Daerah Deplesi (Depletion Region)?", "id": "Area Sekitar Sambungan Miskin Muatan." },
  { "en": "Apa Itu Potensial Penghalang (Barrier Potential)?", "id": "Tegangan Internal Daerah Deplesi." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Pengubah AC Ke DC." },
  { "en": "Apa Itu Clipper (Clipping Circuit)?", "id": "Rangkaian Pemotong Level Tegangan." },
  { "en": "Apa Itu Clamper (Clamping Circuit)?", "id": "Rangkaian Penggeser Level DC." },
  { "en": "Apa Itu Dioda Zener (Zener Diode)?", "id": "Regulator Tegangan Bias Mundur." },
  { "en": "Apa Itu Transistor BJT (Bipolar Junction Transistor)?", "id": "Perangkat Penguat/Saklar Terkontrol Arus." },
  { "en": "Apa Itu Penguatan Beta (β) BJT?", "id": "Rasio Arus Kolektor, Basis." },
  { "en": "Apa Itu Titik Q (Q-Point) Transistor?", "id": "Titik Operasi DC Transistor." },
  { "en": "Apa Itu Penguat Common Emitter (Common Emitter)?", "id": "Penguat BJT (Bipolar Junction Transistor) Paling Umum." },
  { "en": "Apa Itu Penguat Common Collector (Common Collector)?", "id": "Pengikut Emitor (Emitter Follower)." },
  { "en": "Apa Itu Penguat Common Base (Common Base)?", "id": "Penguat Impedansi Input Rendah." },
  { "en": "Apa Itu Transistor FET (Field Effect Transistor)?", "id": "Perangkat Penguat/Saklar Terkontrol Tegangan." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET (Field Effect Transistor) Gerbang Terisolasi." },
  { "en": "Apa Itu Penguat Common Source (Common Source)?", "id": "Penguat FET (Field Effect Transistor) Paling Umum." },
  { "en": "Apa Itu Penguat Common Drain (Common Drain)?", "id": "Pengikut Source (Source Follower)." },
  { "en": "Apa Itu Penguat Common Gate (Common Gate)?", "id": "Penguat Impedansi Input Rendah." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Penguatan Op-Amp." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Output Terbalik Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat Non-Inverting (Non-Inverting Amplifier)?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut Tegangan (Voltage Follower)?", "id": "Buffer (Buffer) Op-Amp Gain Satu." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter (Filter) Menggunakan Op-Amp." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Elemen Dasar Sirkuit Digital." },
  { "en": "Apa Itu Aljabar Boolean (Boolean Algebra)?", "id": "Matematika Operasi Logika." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Satu Bit." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Penyimpan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Digital." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Pemilih Data Digital." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Distributor Data Digital." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Tegangan AC." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem Tenaga (Power System)?", "id": "Jaringan Listrik Skala Besar." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Kontrol Konversi Daya Listrik." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Konverter AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Konverter DC Ke AC." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Pengubah Level Tegangan DC." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Pengaturan Perilaku Sistem." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Informasi Output Pengaruhi Input." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu Komunikasi (Communication) Data?", "id": "Transfer Informasi Digital." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Elektromagnetisme (Electromagnetism)?", "id": "Studi Interaksi Listrik, Magnet." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Energi Medan EM." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Proses Kuantifikasi Besaran." },
  { "en": "Apa Itu Gelombang Stasioner (Standing Wave)?", "id": "Hasil Interferensi Dua Gelombang Berlawanan." },
  { "en": "Apa Itu Simpul (Node) Gelombang Stasioner?", "id": "Titik Amplitudo Nol (Diam)." },
  { "en": "Apa Itu Perut (Antinode) Gelombang Stasioner?", "id": "Titik Amplitudo Maksimum Getaran." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Rasio Amplitudo Maksimum, Minimum Gelombang." },
  { "en": "Apa Nilai SWR (Standing Wave Ratio) Ideal?", "id": "Satu Banding Satu (1:1)." },
  { "en": "Apa Penyebab SWR (Standing Wave Ratio) Buruk?", "id": "Ketidakcocokan Impedansi (Mismatch)." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyamakan Impedansi Sumber, Saluran, Beban." },
  { "en": "Mengapa Pencocokan Impedansi (Impedance Matching) Penting?", "id": "Maksimalkan Transfer Daya, Hindari Refleksi." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Grafik Bantu Hitungan Impedansi RF." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient) (Γ)?", "id": "Rasio Gelombang Pantul Terhadap Datang." },
  { "en": "Apa Nilai Koefisien Refleksi (Γ) Ideal?", "id": "Nol (Tidak Ada Pantulan)." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Media Pemandu Gelombang Elektromagnetik." },
  { "en": "Contoh Saluran Transmisi (Transmission Line)?", "id": "Kabel Koaksial, Mikrostrip, Waveguide." },
  { "en": "Apa Itu Impedansi Karakteristik (Z₀)?", "id": "Impedansi Khas Saluran Transmisi." },
  { "en": "Berapa Impedansi (Impedance) Umum Kabel Koaksial?", "id": "Lima Puluh Ohm (50Ω) Atau 75Ω." },
  { "en": "Apa Itu Kabel Mikrostrip (Microstrip Line)?", "id": "Jalur Konduktif Di Atas Substrat PCB." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Pipa Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) Waveguide?", "id": "Frekuensi Minimum Dapat Dirambatkan." },
  { "en": "Apa Itu Mode Propagasi (Propagation Mode) Waveguide?", "id": "Pola Medan EM Dalam Pemandu." },
  { "en": "Apa Itu Mode Dominan (Dominant Mode) Waveguide?", "id": "Mode Frekuensi Cutoff Terendah." },
  { "en": "Mode Dominan (Dominant Mode) Waveguide Persegi Panjang?", "id": "Mode TE₁₀ (Transverse Electric 10)." },
  { "en": "Apa Itu Resonator Rongga (Cavity Resonator)?", "id": "Rongga Logam Penyimpan Energi EM." },
  { "en": "Aplikasi Resonator Rongga (Cavity Resonator)?", "id": "Filter Frekuensi Tinggi, Osilator." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Konverter Gelombang Terpandu Ke Ruang Bebas." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern)?", "id": "Plot Keterarahan Pancaran Energi Antena." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan Dibanding Isotropik." },
  { "en": "Apa Satuan Penguatan (Gain) Antena?", "id": "Desibel Isotropik (dBi)." },
  { "en": "Apa Itu Antena Isotropik (Isotropic Antenna)?", "id": "Antena Ideal Memancar Sama Rata." },
  { "en": "Apa Itu Antena Dipol (Dipole Antenna) Setengah Gelombang?", "id": "Antena Dasar Panjang Setengah Lambda (λ/2)." },
  { "en": "Apa Impedansi (Impedance) Input Dipol Setengah Gelombang?", "id": "Sekitar Tujuh Puluh Tiga Ohm (73Ω)." },
  { "en": "Apa Itu Antena Monopol (Monopole Antenna) Seperempat Gelombang?", "id": "Antena Panjang Seperempat Lambda (λ/4) Diatas Ground." },
  { "en": "Apa Impedansi (Impedance) Input Monopol Seperempat Gelombang?", "id": "Sekitar Tiga Puluh Enam Koma Lima Ohm (36.5Ω)." },
  { "en": "Apa Itu Antena Yagi-Uda (Yagi-Uda)?", "id": "Antena Direktif (Elemen Parasitik)." },
  { "en": "Elemen Apa Saja Pada Antena Yagi?", "id": "Driven, Reflektor, Dan Direktor." },
  { "en": "Apa Itu Antena Parabola (Parabolic Antenna)?", "id": "Antena Reflektor Penguatan Sangat Tinggi." },
  { "en": "Apa Itu Antena Patch (Patch Antenna) Mikrostrip?", "id": "Antena Dicetak Di Atas PCB." },
  { "en": "Apa Itu Antena Array (Antenna Array)?", "id": "Sekumpulan Elemen Antena Bekerja Sama." },
  { "en": "Apa Itu Beamforming (Pembentukan Berkas)?", "id": "Mengatur Fasa Array Arahkan Berkas." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Orientasi Medan Listrik Dipancarkan." },
  { "en": "Jenis Polarisasi (Polarization) Linear?", "id": "Vertikal Atau Horizontal." },
  { "en": "Jenis Polarisasi (Polarization) Sirkular?", "id": "Putar Kanan (RHCP) Putar Kiri (LHCP)." },
  { "en": "Apa Itu Redaman Ruang Bebas (Free Space Loss)?", "id": "Pelemahan Sinyal Akibat Jarak Propagasi." },
  { "en": "Apa Itu Link Budget (Anggaran Tautan) Komunikasi?", "id": "Perhitungan Total Gain, Loss Sistem." },
  { "en": "Apa Itu Radar (Radio Detection and Ranging)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Apa Prinsip Dasar Radar (Radar)?", "id": "Memancarkan Pulsa, Menerima Gema Pantulan." },
  { "en": "Apa Itu Radar Doppler (Doppler Radar)?", "id": "Mengukur Kecepatan Objek (Pergeseran Frekuensi)." },
  { "en": "Apa Itu Penampang Lintang Radar (RCS)?", "id": "Ukuran Kemampuan Objek Pantulkan Sinyal Radar." },
  { "en": "Apa Kepanjangan RCS (Radar Cross Section)?", "id": "Penampang Lintang Radar." },
  { "en": "Apa Itu Komunikasi (Communication) Optik?", "id": "Transmisi Informasi Menggunakan Cahaya." },
  { "en": "Apa Media Utama Komunikasi (Communication) Optik?", "id": "Serat Optik (Fiber Optic)." },
  { "en": "Apa Prinsip Dasar Serat Optik?", "id": "Pemantulan Internal Total Cahaya." },
  { "en": "Apa Itu Inti (Core) Serat Optik?", "id": "Bagian Tengah Serat (Indeks Bias Tinggi)." },
  { "en": "Apa Itu Selubung (Cladding) Serat Optik?", "id": "Lapisan Luar Inti (Indeks Bias Rendah)." },
  { "en": "Apa Itu Serat Mode Tunggal (Single Mode)?", "id": "Inti Sangat Kecil (Satu Jalur Cahaya)." },
  { "en": "Apa Itu Serat Mode Jamak (Multi Mode)?", "id": "Inti Lebih Besar (Banyak Jalur Cahaya)." },
  { "en": "Mana Jangkauan Lebih Jauh, Single Atau Multi Mode?", "id": "Serat Mode Tunggal (Single Mode)." },
  { "en": "Apa Itu Dispersi (Dispersion) Serat Optik?", "id": "Pelebaran Pulsa Cahaya (Batasi Bandwidth)." },
  { "en": "Apa Itu Dispersi Kromatik (Chromatic Dispersion)?", "id": "Beda Kecepatan Rambat Panjang Gelombang." },
  { "en": "Apa Itu Dispersi Modal (Modal Dispersion)?", "id": "Beda Waktu Tempuh Jalur Cahaya (MMF)." },
  { "en": "Apa Itu Atenuasi (Attenuation) Serat Optik?", "id": "Pelemahan Intensitas Cahaya Sepanjang Serat." },
  { "en": "Apa Satuan Atenuasi (Attenuation) Serat Optik?", "id": "Desibel Per Kilometer (dB/km)." },
  { "en": "Apa Sumber Cahaya (Light Source) Komunikasi Optik?", "id": "LED (Light Emitting Diode) Atau Dioda Laser (LD)." },
  { "en": "Apa Detektor (Detector) Komunikasi Optik?", "id": "Fotodioda (PIN Atau APD)." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Multiplexing Banyak Panjang Gelombang (Warna)." },
  { "en": "Apa Itu Penguat Optik (Optical Amplifier)?", "id": "Menguatkan Sinyal Cahaya Langsung." },
  { "en": "Contoh Penguat Optik (Optical Amplifier)?", "id": "EDFA (Erbium Doped Fiber Amplifier)." },
  { "en": "Apa Itu Penyambungan (Splicing) Fusi?", "id": "Menyambung Serat Optik Panas Lelehan." },
  { "en": "Apa Itu Konektor (Connector) Serat Optik?", "id": "Koneksi Serat Optik Dapat Dilepas Pasang." },
  { "en": "Contoh Konektor (Connector) Serat Optik?", "id": "SC (Subscriber Connector), LC (Lucent Connector)." },
  { "en": "Apa Itu OTDR (Optical Time Domain Reflectometer)?", "id": "Alat Uji Serat Optik (Panjang, Redaman, Lokasi Putus)." },
  { "en": "Apa Itu Sistem (System) Waktu Kontinu?", "id": "Sistem Sinyal Terdefinisi Setiap Saat." },
  { "en": "Apa Itu Sistem (System) Waktu Diskrit?", "id": "Sistem Sinyal Terdefinisi Waktu Diskrit." },
  { "en": "Apa Itu Sistem (System) Linear?", "id": "Sistem Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Sistem (System) Invarian Waktu?", "id": "Respon Sistem Tidak Berubah Waktu." },
  { "en": "Apa Itu Sistem (System) LTI?", "id": "Linear Time-Invariant." },
  { "en": "Apa Itu Respon Impuls (Impulse Response)?", "id": "Output Sistem LTI Terhadap Input Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Output Sistem LTI." },
  { "en": "Apa Itu Transformasi (Transform) Laplace?", "id": "Mengubah Persamaan Diferensial Ke Aljabar." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(s)?", "id": "Rasio Output(s) Terhadap Input(s)." },
  { "en": "Apa Itu Pole (Kutub) Sistem?", "id": "Akar Penyebut Fungsi Transfer." },
  { "en": "Apa Itu Zero (Nol) Sistem?", "id": "Akar Pembilang Fungsi Transfer." },
  { "en": "Apa Syarat Kestabilan (Stability) Sistem Kontinu?", "id": "Semua Pole Di Setengah Kiri Bidang-s." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Respons Sistem Terhadap Input Sinusoidal." },
  { "en": "Apa Itu Plot Bode (Bode Plot)?", "id": "Plot Logaritmik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Transformasi (Transform) Z?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(z)?", "id": "Rasio Output(z) Terhadap Input(z)." },
  { "en": "Apa Itu Lingkaran Satuan (Unit Circle)?", "id": "Lingkaran Jari-Jari Satu Bidang-Z." },
  { "en": "Apa Syarat Kestabilan (Stability) Sistem Diskrit?", "id": "Semua Pole Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Transformasi (Transform) Fourier Diskrit (DFT)?", "id": "Analisis Frekuensi Sinyal Waktu Diskrit." },
  { "en": "Apa Itu Transformasi (Transform) Fourier Cepat (FFT)?", "id": "Algoritma Efisien Menghitung DFT." },
  { "en": "Apa Itu Filter (Filter) Digital?", "id": "Algoritma Pemrosesan Sinyal Digital." },
  { "en": "Apa Itu Filter (Filter) FIR?", "id": "Finite Impulse Response (Tanpa Feedback)." },
  { "en": "Apa Itu Filter (Filter) IIR?", "id": "Infinite Impulse Response (Dengan Feedback)." },
  { "en": "Apa Keuntungan Filter (Filter) FIR?", "id": "Selalu Stabil, Fasa Linear Mudah." },
  { "en": "Apa Keuntungan Filter (Filter) IIR?", "id": "Komputasi Lebih Efisien (Orde Rendah)." },
  { "en": "Apa Itu Efek (Effect) Windowing?", "id": "Mengurangi Kebocoran Spektral Analisis FFT." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Loop (Loop) Terbuka?", "id": "Kontrol Tanpa Umpan Balik Output." },
  { "en": "Apa Itu Loop (Loop) Tertutup?", "id": "Kontrol Dengan Umpan Balik Output." },
  { "en": "Apa Itu Setpoint (Setpoint) Kontrol?", "id": "Nilai Referensi Yang Diinginkan." },
  { "en": "Apa Itu Error (Kesalahan) Kontrol?", "id": "Selisih Antara Setpoint, Nilai Aktual." },
  { "en": "Apa Itu Kontroler (Controller) PID?", "id": "Proporsional, Integral, Derivatif." },
  { "en": "Fungsi (Function) Aksi Proporsional (P)?", "id": "Respon Proporsional Terhadap Error." },
  { "en": "Fungsi (Function) Aksi Integral (I)?", "id": "Menghilangkan Error Keadaan Tunak." },
  { "en": "Fungsi (Function) Aksi Derivatif (D)?", "id": "Respon Antisipasi Laju Perubahan Error." },
  { "en": "Apa Itu Penalaan (Tuning) PID?", "id": "Menentukan Nilai Parameter Kp, Ki, Kd." },
  { "en": "Apa Itu Kestabilan (Stability) Sistem Kontrol?", "id": "Kemampuan Sistem Kembali Seimbang." },
  { "en": "Apa Itu Respon (Response) Transien?", "id": "Perilaku Sistem Sebelum Stabil." },
  { "en": "Apa Itu Respon (Response) Keadaan Tunak?", "id": "Perilaku Sistem Setelah Stabil." },
  { "en": "Apa Itu Error (Kesalahan) Keadaan Tunak?", "id": "Selisih Setpoint, Output Saat Stabil." },
  { "en": "Apa Itu Diagram (Diagram) Bode?", "id": "Plot Respon Frekuensi (Magnitudo, Fasa)." },
  { "en": "Apa Itu Gain Margin (GM) (Batas Penguatan)?", "id": "Ukuran Kestabilan Relatif Gain." },
  { "en": "Apa Itu Phase Margin (PM) (Batas Fasa)?", "id": "Ukuran Kestabilan Relatif Fasa." },
  { "en": "Apa Itu Kontrol (Control) Digital?", "id": "Implementasi Kontroler Komputer Digital." },
  { "en": "Apa Itu Transformasi (Transform) Z?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Industri Otomatisasi." },
  { "en": "Apa Bahasa (Language) Pemrograman PLC?", "id": "Umumnya Ladder Logic (Logika Tangga)." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Desain, Operasi Robot." },
  { "en": "Apa Itu Derajat (Degree) Kebebasan (DOF) Robot?", "id": "Jumlah Gerakan Sumbu Independen." },
  { "en": "Apa Itu Kinematika (Kinematics) Robot?", "id": "Studi Geometri Gerakan Robot." },
  { "en": "Apa Itu Dinamika (Dynamics) Robot?", "id": "Studi Gerakan Robot Terkait Gaya." },
  { "en": "Apa Itu Visi (Vision) Komputer?", "id": "Kemampuan Komputer Menginterpretasi Gambar." },
  { "en": "Apa Itu Pemrosesan (Processing) Sinyal?", "id": "Analisis, Modifikasi Sinyal Elektronik." },
  { "en": "Apa Itu Sinyal (Signal) Analog?", "id": "Sinyal Dengan Nilai Kontinu." },
  { "en": "Apa Itu Sinyal (Signal) Digital?", "id": "Sinyal Dengan Nilai Diskrit." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Mengubah Sinyal Kontinu Ke Diskrit." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Mengubah Nilai Sampel Diskrit Amplitudo." },
  { "en": "Apa Itu Transformasi (Transform) Fourier?", "id": "Mengubah Sinyal Domain Waktu Ke Frekuensi." },
  { "en": "Apa Itu Filter (Filter) Digital?", "id": "Memproses Sinyal Digital Ubah Spektrum." },
  { "en": "Apa Itu Komunikasi (Communication)?", "id": "Proses Pengiriman Informasi." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Gelombang Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Pemisahan Informasi Dari Gelombang Pembawa." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Jaringan." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Pengenal Logis Unik Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Penghubung Antar Jaringan." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Mudah Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Sulit Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Konduktivitas Antara." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Searah Arus." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Penguat/Saklar." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Sirkuit Elektronik Chip." },
  { "en": "Apa Itu Hukum (Law) Ohm?", "id": "V = I * R." },
  { "en": "Apa Itu Hukum (Law) Kirchhoff?", "id": "Hukum Arus Dan Tegangan." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Energi Listrik (Watt)." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Penyimpan Energi Magnetik." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total AC." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Non-Resistif AC." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Frekuensi Reaktansi Saling Hapus." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Seleksi Frekuensi." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Operasi." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Gangguan Acak Sinyal." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Kedekatan Pengukuran Nilai Benar." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Hasil Ukur Konsisten." },
  { "en": "Apa Itu Resolusi (Resolution)?", "id": "Perubahan Terkecil Terdeteksi." },
  { "en": "Apa Itu Sensor (Sensor) Suhu?", "id": "Mengukur Suhu." },
  { "en": "Apa Itu Sensor (Sensor) Cahaya?", "id": "Mengukur Intensitas Cahaya." },
  { "en": "Apa Itu Motor (Motor) DC?", "id": "Penggerak Putar DC." },
  { "en": "Apa Itu Motor (Motor) AC?", "id": "Penggerak Putar AC." },
  { "en": "Apa Itu Relay (Relay)?", "id": "Saklar Elektromekanis." },
  { "en": "Apa Itu Keselamatan (Safety) Listrik?", "id": "Pencegahan Bahaya Listrik." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih Putus." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit Otomatis." },
  { "en": "Apa Itu Ground (Ground) Fault?", "id": "Arus Bocor Ke Tanah." },
  { "en": "Apa Itu Resistor (Resistor) Film?", "id": "Resistor Lapisan Tipis Resistif." },
  { "en": "Apa Itu Resistor (Resistor) Komposisi Karbon?", "id": "Resistor Campuran Karbon Perekat." },
  { "en": "Apa Itu Resistor (Resistor) Variabel?", "id": "Resistansi Dapat Diubah (Potensiometer)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Keramik?", "id": "Kapasitor Dielektrik Keramik." },
  { "en": "Apa Itu Kapasitor (Capacitor) Elektrolit?", "id": "Kapasitor Polar Kapasitansi Tinggi." },
  { "en": "Apa Itu Kapasitor (Capacitor) Tantalum?", "id": "Jenis Kapasitor Elektrolit Padat." },
  { "en": "Apa Itu Kapasitor (Capacitor) Film?", "id": "Kapasitor Dielektrik Plastik Film." },
  { "en": "Apa Itu Induktor (Induktor) Inti Udara?", "id": "Induktor Tanpa Inti Magnetik." },
  { "en": "Apa Itu Induktor (Induktor) Inti Ferit?", "id": "Induktor Inti Bahan Ferit (HF)." },
  { "en": "Apa Itu Induktor (Induktor) Inti Besi?", "id": "Induktor Inti Besi Laminasi (LF)." },
  { "en": "Apa Itu Transformator (Transformer) Inti Besi?", "id": "Transformator Frekuensi Rendah." },
  { "en": "Apa Itu Transformator (Transformer) Inti Ferit?", "id": "Transformator Frekuensi Tinggi (SMPS)." },
  { "en": "Apa Itu Dioda (Diode) Penyearah?", "id": "Dioda Konversi AC Ke DC." },
  { "en": "Apa Itu Dioda (Diode) Sinyal?", "id": "Dioda Arus Kecil Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Dioda Regulator Tegangan Mundur." },
  { "en": "Apa Itu Dioda (Diode) Schottky?", "id": "Dioda Cepat Tegangan Rendah." },
  { "en": "Apa Itu Dioda (Diode) Varactor (Varicap)?", "id": "Dioda Kapasitansi Variabel." },
  { "en": "Apa Itu Transistor (Transistor) NPN?", "id": "Tipe BJT (Bipolar Junction Transistor) Umum." },
  { "en": "Apa Itu Transistor (Transistor) PNP?", "id": "Tipe BJT (Bipolar Junction Transistor) Komplementer NPN." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) N-Channel?", "id": "Tipe MOSFET (Metal-Oxide-Semiconductor FET) Umum." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET) P-Channel?", "id": "Tipe MOSFET (Metal-Oxide-Semiconductor FET) Komplementer N-Channel." },
  { "en": "Apa Itu Thyristor (Thyristor) (SCR)?", "id": "Saklar Daya Semikonduktor Terkunci." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Saklar Daya AC Dua Arah." },
  { "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Pemicu TRIAC (Triode for Alternating Current)." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Saklar Daya Tinggi (Gabungan MOSFET BJT)." },
  { "en": "Apa Itu IC (Integrated Circuit) Linear?", "id": "IC (Integrated Circuit) Proses Sinyal Analog." },
  { "en": "Contoh IC (Integrated Circuit) Linear?", "id": "Op-Amp, Regulator Tegangan, Timer 555." },
  { "en": "Apa Itu IC (Integrated Circuit) Digital?", "id": "IC (Integrated Circuit) Proses Sinyal Digital." },
  { "en": "Contoh IC (Integrated Circuit) Digital?", "id": "Gerbang Logika, Mikroprosesor, Memori." },
  { "en": "Apa Itu IC (Integrated Circuit) Sinyal Campuran?", "id": "IC (Integrated Circuit) Gabungkan Analog, Digital." },
  { "en": "Contoh IC (Integrated Circuit) Sinyal Campuran?", "id": "ADC (Analog-to-Digital Converter), DAC (Digital-to-Analog Converter)." },
  { "en": "Apa Itu Keluarga (Family) Logika TTL?", "id": "Transistor-Transistor Logic." },
  { "en": "Apa Itu Keluarga (Family) Logika CMOS?", "id": "Complementary Metal-Oxide-Semiconductor." },
  { "en": "Apa Itu FPGA (Field Programmable Gate Array)?", "id": "IC (Integrated Circuit) Logika Terprogram." },
  { "en": "Apa Itu CPLD (Complex Programmable Logic Device)?", "id": "Perangkat Logika Terprogram Kompleks." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Perangkat Deteksi Besaran Fisik." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Perangkat Konversi Energi." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Perangkat Hasilkan Gerakan/Aksi." },
  { "en": "Apa Itu Sensor (Sensor) Suhu?", "id": "Mengukur Panas Atau Dingin." },
  { "en": "Jenis Sensor (Sensor) Suhu?", "id": "Termokopel, RTD (Resistance Temperature Detector), Termistor." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Efek Seebeck." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Resistansi Logam." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Semikonduktor Peka Suhu." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Jaringan Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Tiga Komponen Utama Sistem Tenaga?", "id": "Pembangkitan, Transmisi, Distribusi." },
  { "en": "Apa Fungsi Pembangkitan (Generation)?", "id": "Menghasilkan Energi Listrik." },
  { "en": "Apa Fungsi Transmisi (Transmission)?", "id": "Menyalurkan Listrik Jarak Jauh (HV)." },
  { "en": "Apa Fungsi Distribusi (Distribution)?", "id": "Menyalurkan Listrik Ke Konsumen (LV/MV)." },
  { "en": "Mengapa Tegangan Transmisi Sangat Tinggi?", "id": "Mengurangi Rugi-Rugi Daya (I²R)." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Transformasi Tegangan, Switching." },
  { "en": "Apa Fungsi Transformator (Transformer) Gardu Induk?", "id": "Menaikkan Atau Menurunkan Tegangan." },
  { "en": "Apa Itu Peralatan Hubung Bagi (Switchgear)?", "id": "Peralatan Pemutus, Pemisah, Proteksi." },
  { "en": "Apa Itu Pemutus Tenaga (Circuit Breaker)?", "id": "Saklar Otomatis Pemutus Arus Gangguan." },
  { "en": "Apa Itu Pemisah (Disconnector/Isolator)?", "id": "Saklar Pemisah Tanpa Beban Arus." },
  { "en": "Apa Fungsi Pemisah (Disconnector)?", "id": "Mengisolasi Peralatan Untuk Pemeliharaan Aman." },
  { "en": "Apa Itu Busbar (Busbar)?", "id": "Konduktor Penghubung Utama Di Gardu." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Perangkat Pendeteksi Gangguan Sistem Tenaga." },
  { "en": "Apa Respon Rele Proteksi (Protection Relay)?", "id": "Memberi Sinyal Trip Ke Pemutus." },
  { "en": "Apa Itu Trafo Arus (Current Transformer) (CT)?", "id": "Sensor Arus Tinggi Untuk Pengukuran/Proteksi." },
  { "en": "Apa Itu Trafo Tegangan (Potential Transformer) (PT)?", "id": "Sensor Tegangan Tinggi Untuk Pengukuran/Proteksi." },
  { "en": "Mengapa Sekunder CT (Current Transformer) Tak Boleh Terbuka?", "id": "Tegangan Sangat Tinggi Berbahaya Muncul." },
  { "en": "Apa Itu Beban (Load) Sistem Tenaga?", "id": "Total Daya Dikonsumsi Pelanggan." },
  { "en": "Apa Itu Beban Puncak (Peak Load)?", "id": "Permintaan Daya Tertinggi Sistem." },
  { "en": "Apa Itu Beban Dasar (Base Load)?", "id": "Permintaan Daya Minimum Konstan." },
  { "en": "Pembangkit Apa Cocok Beban Dasar (Base Load)?", "id": "PLTU (Pembangkit Listrik Tenaga Uap), PLTN (Pembangkit Listrik Tenaga Nuklir)." },
  { "en": "Pembangkit Apa Cocok Beban Puncak (Peak Load)?", "id": "PLTG (Pembangkit Listrik Tenaga Gas), PLTD (Pembangkit Listrik Tenaga Diesel)." },
  { "en": "Apa Itu Faktor Beban (Load Factor)?", "id": "Rasio Beban Rata-Rata Terhadap Puncak." },
  { "en": "Apa Itu Stabilitas (Stability) Sistem Tenaga?", "id": "Kemampuan Sistem Kembali Normal Pasca Gangguan." },
  { "en": "Jenis Stabilitas (Stability) Sistem Tenaga Utama?", "id": "Sudut Rotor, Tegangan, Frekuensi." },
  { "en": "Apa Itu Stabilitas Sudut Rotor (Rotor Angle)?", "id": "Kemampuan Generator Tetap Sinkron." },
  { "en": "Apa Itu Stabilitas Tegangan (Voltage Stability)?", "id": "Kemampuan Sistem Jaga Level Tegangan." },
  { "en": "Apa Itu Stabilitas Frekuensi (Frequency Stability)?", "id": "Kemampuan Sistem Jaga Frekuensi Konstan." },
  { "en": "Apa Penyebab Utama Ketidakstabilan Frekuensi?", "id": "Ketidakseimbangan Pembangkitan Dan Beban." },
  { "en": "Apa Itu Gangguan (Fault) Sistem Tenaga?", "id": "Kondisi Abnormal (Hubung Singkat, Open)." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Kontak Arus Rendah Antar Konduktor." },
  { "en": "Apa Akibat Hubung Singkat (Short Circuit)?", "id": "Arus Sangat Tinggi, Tegangan Turun." },
  { "en": "Apa Itu Gangguan Simetris (Symmetrical Fault)?", "id": "Gangguan Tiga Fasa Seimbang." },
  { "en": "Apa Itu Gangguan Asimetris (Asymmetrical Fault)?", "id": "Gangguan Tidak Seimbang (Satu Fasa)." },
  { "en": "Metode Analisis Gangguan Asimetris (Asymmetrical Fault)?", "id": "Komponen Simetris (Symmetrical Components)." },
  { "en": "Apa Itu Komponen Simetris (Symmetrical Components)?", "id": "Urutan Positif, Negatif, Nol." },
  { "en": "Apa Itu Urutan Positif (Positive Sequence)?", "id": "Komponen Sistem Tiga Fasa Normal." },
  { "en": "Apa Itu Urutan Negatif (Negative Sequence)?", "id": "Komponen Akibat Ketidakseimbangan (Putaran Terbalik)." },
  { "en": "Apa Itu Urutan Nol (Zero Sequence)?", "id": "Komponen Sefasa (Hanya Ada Gangguan Tanah)." },
  { "en": "Apa Itu Grounding (Pembumian) Sistem Tenaga?", "id": "Menghubungkan Titik Netral Ke Tanah." },
  { "en": "Apa Tujuan Grounding (Pembumian) Sistem?", "id": "Keamanan, Stabilitas Tegangan, Deteksi Gangguan." },
  { "en": "Jenis Grounding (Pembumian) Sistem Utama?", "id": "Solid, Resistan, Reaktans, Tak Dibumikan." },
  { "en": "Apa Itu Sistem Solidly Grounded (Dibumikan Solid)?", "id": "Netral Langsung Terhubung Ke Tanah." },
  { "en": "Apa Keuntungan Solidly Grounded (Dibumikan Solid)?", "id": "Tegangan Lebih Rendah Saat Gangguan Tanah." },
  { "en": "Apa Kerugian Solidly Grounded (Dibumikan Solid)?", "id": "Arus Gangguan Tanah Sangat Tinggi." },
  { "en": "Apa Itu Sistem Resistance Grounded (Dibumikan Resistan)?", "id": "Netral Ke Tanah Lewat Resistor." },
  { "en": "Apa Keuntungan Resistance Grounded (Dibumikan Resistan)?", "id": "Membatasi Arus Gangguan Tanah." },
  { "en": "Apa Itu Sistem Ungrounded (Tak Dibumikan) (IT)?", "id": "Netral Tidak Sengaja Dihubungkan Tanah." },
  { "en": "Apa Keuntungan Sistem Ungrounded (Tak Dibumikan)?", "id": "Operasi Bisa Lanjut Gangguan Tanah Pertama." },
  { "en": "Apa Kerugian Sistem Ungrounded (Tak Dibumikan)?", "id": "Tegangan Lebih Tinggi, Sulit Deteksi Gangguan." },
  { "en": "Apa Itu Arester Surja (Surge Arrester)?", "id": "Pelindung Peralatan Dari Tegangan Lebih Surja." },
  { "en": "Apa Penyebab Surja (Surge) Tegangan?", "id": "Sambaran Petir Atau Operasi Switching." },
  { "en": "Bagaimana Arester Surja (Surge Arrester) Bekerja?", "id": "Menyalurkan Arus Surja Ke Tanah." },
  { "en": "Apa Itu Koordinasi Isolasi (Insulation Coordination)?", "id": "Menyesuaikan Kekuatan Isolasi, Level Proteksi." },
  { "en": "Apa Itu Level Isolasi Dasar (BIL)?", "id": "Basic Insulation Level (BIL)." },
  { "en": "Apa Kepanjangan BIL (Basic Insulation Level)?", "id": "Level Isolasi Dasar (Impuls Petir)." },
  { "en": "Apa Itu Kualitas Daya (Power Quality)?", "id": "Karakteristik Tegangan, Arus, Frekuensi." },
  { "en": "Masalah Kualitas Daya (Power Quality) Umum?", "id": "Sag, Swell, Harmonisa, Transien, Flicker." },
  { "en": "Apa Itu Sag (Sag) Tegangan?", "id": "Penurunan Tegangan Sesaat." },
  { "en": "Apa Itu Swell (Swell) Tegangan?", "id": "Kenaikan Tegangan Sesaat." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Dari Fundamental." },
  { "en": "Apa Penyebab Harmonisa (Harmonics)?", "id": "Beban Non-Linear (Elektronika Daya)." },
  { "en": "Apa Dampak Buruk Harmonisa (Harmonics)?", "id": "Pemanasan Berlebih, Gangguan Peralatan." },
  { "en": "Apa Itu Total Harmonic Distortion (THD)?", "id": "Ukuran Total Distorsi Harmonisa." },
  { "en": "Apa Itu Filter Harmonisa (Harmonic Filter)?", "id": "Mengurangi Atau Menghilangkan Harmonisa." },
  { "en": "Jenis Filter Harmonisa (Harmonic Filter)?", "id": "Pasif (LC) Dan Aktif (Elektronik)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Memperbaiki Faktor Daya Beban." },
  { "en": "Apa Itu Kapasitor Bank (Capacitor Bank)?", "id": "Kumpulan Kapasitor Koreksi Faktor Daya." },
  { "en": "Apa Itu Transien (Transient)?", "id": "Perubahan Tegangan/Arus Sangat Cepat." },
  { "en": "Apa Itu Flicker (Flicker) Tegangan?", "id": "Fluktuasi Tegangan Sebabkan Lampu Berkedip." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Tak Terputus." },
  { "en": "Apa Fungsi Utama UPS (Uninterruptible Power Supply)?", "id": "Memberi Daya Cadangan Saat Padam." },
  { "en": "Apa Itu FACTS (Flexible AC Transmission Systems)?", "id": "Peralatan Elektronika Daya Kontrol Transmisi." },
  { "en": "Apa Tujuan FACTS (Flexible AC Transmission Systems)?", "id": "Meningkatkan Kapasitas, Stabilitas Transmisi." },
  { "en": "Contoh Perangkat FACTS (Flexible AC Transmission Systems)?", "id": "SVC, STATCOM, TCSC, UPFC." },
  { "en": "Apa Itu HVDC (High Voltage Direct Current)?", "id": "Transmisi Daya Arus Searah Tegangan Tinggi." },
  { "en": "Apa Keunggulan HVDC (High Voltage Direct Current)?", "id": "Rugi Rendah Jarak Jauh, Interkoneksi Asinkron." },
  { "en": "Apa Itu Stasiun Konverter (Converter Station) HVDC?", "id": "Mengubah AC Ke DC Dan Sebaliknya." },
  { "en": "Apa Itu Energi Terbarukan (Renewable Energy)?", "id": "Energi Sumber Alam Tak Habis." },
  { "en": "Contoh Energi Terbarukan (Renewable Energy)?", "id": "Surya, Angin, Air, Panas Bumi, Biomassa." },
  { "en": "Apa Itu Sel Fotovoltaik (Photovoltaic Cell)?", "id": "Mengubah Cahaya Matahari Langsung Listrik." },
  { "en": "Apa Itu Panel Surya (Solar Panel)?", "id": "Kumpulan Sel Fotovoltaik." },
  { "en": "Apa Itu Inverter (Inverter) Surya?", "id": "Mengubah DC Panel Surya Ke AC." },
  { "en": "Apa Itu MPPT (Maximum Power Point Tracking)?", "id": "Optimasi Daya Output Panel Surya." },
  { "en": "Apa Itu Turbin Angin (Wind Turbine)?", "id": "Mengubah Energi Kinetik Angin Ke Listrik." },
  { "en": "Apa Komponen Utama Turbin Angin?", "id": "Bilah (Blade), Rotor, Generator, Menara." },
  { "en": "Apa Itu Pembangkit Listrik Tenaga Air (PLTA)?", "id": "Menggunakan Energi Potensial Air." },
  { "en": "Apa Itu Turbin Air (Hydro Turbine)?", "id": "Mengubah Energi Aliran Air Putaran Mekanis." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Jaringan Listrik Modern Komunikasi Digital." },
  { "en": "Apa Tujuan Smart Grid (Jaringan Cerdas)?", "id": "Efisiensi, Keandalan, Integrasi Terbarukan." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Listrik Komunikasi Otomatis." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Mengelola Pola Konsumsi Listrik." },
  { "en": "Apa Itu Penyimpanan Energi (Energy Storage)?", "id": "Menyimpan Energi Listrik Untuk Nanti." },
  { "en": "Contoh Teknologi Penyimpanan Energi?", "id": "Baterai, Pompa Hidro, Udara Terkompresi." },
  { "en": "Apa Itu BESS (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Baterai." },
  { "en": "Apa Fungsi BESS (Battery Energy Storage System) Jaringan?", "id": "Stabilisasi Frekuensi, Cadangan Daya." },
  { "en": "Apa Itu Kendaraan Listrik (Electric Vehicle) (EV)?", "id": "Kendaraan Penggerak Motor Listrik." },
  { "en": "Apa Itu Pengisian Cerdas (Smart Charging) EV?", "id": "Pengisian Diatur Optimalisasi Jaringan." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Kembalikan Daya Ke Jaringan." },
  { "en": "Apa Itu Efisiensi Energi (Energy Efficiency)?", "id": "Mengurangi Konsumsi Energi Hasil Sama." },
  { "en": "Apa Itu Audit Energi (Energy Audit)?", "id": "Analisis Penggunaan Energi Cari Penghematan." },
  { "en": "Apa Itu Manajemen Energi (Energy Management)?", "id": "Sistem Pemantauan, Kontrol Konsumsi Energi." },
  { "en": "Apa Itu Slew Rate (Laju Perubahan) Op-Amp?", "id": "Kecepatan Perubahan Output Maksimal Op-Amp." },
  { "en": "Apa Satuan Slew Rate (Laju Perubahan)?", "id": "Volt Per Mikrodetik (V/µs)." },
  { "en": "Apa Itu Tegangan Offset (Offset Voltage) Input Op-Amp?", "id": "Tegangan Diferensial Input Hasilkan Output Nol." },
  { "en": "Apa Itu Arus Bias (Bias Current) Input Op-Amp?", "id": "Arus DC Rata-Rata Masuk Terminal Input." },
  { "en": "Apa Itu Gain Bandwidth Product (GBP)?", "id": "Hasil Kali Gain Loop Tertutup, Bandwidth." },
  { "en": "Apa Kepanjangan GBP (Gain Bandwidth Product)?", "id": "Produk Penguatan Lebar Pita." },
  { "en": "Apa Itu Encoder (Penyandi)?", "id": "Mengubah Gerakan Menjadi Sinyal Digital." },
  { "en": "Apa Itu Encoder (Encoder) Inkremental?", "id": "Menghasilkan Pulsa Relatif Terhadap Gerakan." },
  { "en": "Apa Itu Encoder (Encoder) Absolut?", "id": "Memberikan Kode Posisi Digital Unik." },
  { "en": "Apa Itu Decoder (Dekoder)?", "id": "Mengubah Input Biner Menjadi Output Tertentu." },
  { "en": "Contoh Aplikasi Decoder (Dekoder)?", "id": "Dekoder BCD (Binary Coded Decimal) Ke 7-Segmen." },
  { "en": "Apa Itu Amplitudo (Amplitude) Sinyal?", "id": "Nilai Puncak Maksimum Sinyal." },
  { "en": "Apa Itu Fasa (Phase) Sinyal?", "id": "Posisi Relatif Sinyal Dalam Siklus." },
  { "en": "Apa Satuan Fasa (Phase) Sinyal?", "id": "Derajat (Degrees) Atau Radian." },
  { "en": "Apa Itu Pergeseran Fasa (Phase Shift)?", "id": "Perbedaan Fasa Antara Dua Sinyal." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Bergerak Dalam Langkah Sudut Diskrit." },
  { "en": "Bagaimana Kontrol Motor Stepper (Stepper Motor)?", "id": "Menggunakan Urutan Pulsa Digital." },
  { "en": "Apa Keuntungan Motor Stepper (Stepper Motor)?", "id": "Kontrol Posisi Presisi Loop Terbuka." },
  { "en": "Apa Itu Motor Servo (Servo Motor)?", "id": "Motor Kontrol Posisi Loop Tertutup Presisi." },
  { "en": "Komponen Apa Umpan Balik (Feedback) Servo?", "id": "Potensiometer Atau Encoder." },
  { "en": "Apa Itu Sinyal Kontrol (Control Signal) Servo?", "id": "Sinyal PWM (Pulse Width Modulation) Lebar Pulsa Variabel." },
  { "en": "Apa Itu Penyearah (Rectifier) Gelombang Penuh?", "id": "Mengubah Kedua Siklus AC Menjadi DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Mengubah Daya DC Menjadi Daya AC." },
  { "en": "Aplikasi Umum Inverter (Inverter)?", "id": "UPS (Uninterruptible Power Supply), Drive Motor AC." },
  { "en": "Apa Itu Resolusi (Resolution) Alat Ukur?", "id": "Perubahan Terkecil Dapat Diukur Alat." },
  { "en": "Apa Itu Akurasi (Accuracy) Alat Ukur?", "id": "Kedekatan Hasil Ukur Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision) Alat Ukur?", "id": "Kemampuan Alat Ukur Beri Hasil Sama." },
  { "en": "Apa Fungsi Sekring (Fuse)?", "id": "Proteksi Arus Lebih (Putus Sekali Pakai)." },
  { "en": "Apa Fungsi Pemutus Sirkuit (Circuit Breaker)?", "id": "Proteksi Arus Lebih (Dapat Direset)." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Mudah Hantarkan Listrik (Elektron Bebas)." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Sulit Hantarkan Listrik (Elektron Terikat)." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Konduktivitas Antara Konduktor, Isolator." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Hambatan Jenis Intrinsik Suatu Material." },
  { "en": "Apa Satuan Resistivitas (Resistivity)?", "id": "Ohm Meter (Ω·m)." },
  { "en": "Apa Itu Konduktivitas (Conductivity)?", "id": "Kemudahan Material Hantarkan Listrik." },
  { "en": "Apa Satuan Konduktivitas (Conductivity)?", "id": "Siemens Per Meter (S/m)." },
  { "en": "Apa Hubungan Konduktivitas, Resistivitas?", "id": "Konduktivitas Adalah Kebalikan Resistivitas." },
  { "en": "Apa Itu Efek Suhu Pada Konduktor Logam?", "id": "Resistansi Meningkat Seiring Kenaikan Suhu." },
  { "en": "Apa Itu Efek Suhu Pada Semikonduktor?", "id": "Resistansi Menurun Seiring Kenaikan Suhu." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Resistansi Nol Suhu Rendah." },
  { "en": "Apa Itu Suhu Kritis (Critical Temperature) Superkonduktor?", "id": "Suhu Transisi Menjadi Superkonduktor." },
  { "en": "Apa Itu Bahan (Material) Dielektrik?", "id": "Bahan Isolator Digunakan Dalam Kapasitor." },
  { "en": "Apa Itu Konstanta Dielektrik (Dielectric Constant)?", "id": "Ukuran Kemampuan Simpan Energi Listrik." },
  { "en": "Apa Itu Kekuatan Dielektrik (Dielectric Strength)?", "id": "Medan Listrik Maksimum Sebelum Breakdown." },
  { "en": "Apa Itu Bahan (Material) Magnetik?", "id": "Bahan Berinteraksi Kuat Medan Magnet." },
  { "en": "Apa Itu Bahan (Material) Feromagnetik?", "id": "Bahan Mudah Dimagnetisasi (Besi, Nikel)." },
  { "en": "Apa Itu Bahan (Material) Paramagnetik?", "id": "Bahan Sedikit Tertarik Medan Magnet." },
  { "en": "Apa Itu Bahan (Material) Diamagnetik?", "id": "Bahan Sedikit Menolak Medan Magnet." },
  { "en": "Apa Itu Permeabilitas (Permeability) Magnetik?", "id": "Ukuran Kemampuan Bahan Hantarkan Fluks." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Ketergantungan Magnetisasi Riwayat Medan Magnet." },
  { "en": "Apa Itu Bahan (Material) Magnet Lunak?", "id": "Mudah Dimagnetisasi, Didemagnetisasi (Inti Trafo)." },
  { "en": "Apa Itu Bahan (Material) Magnet Keras?", "id": "Sulit Dimagnetisasi, Tahan Demagnetisasi (Magnet Permanen)." },
  { "en": "Apa Itu Listrik Statis (Static Electricity)?", "id": "Akumulasi Muatan Listrik Diam Permukaan." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis Tiba-Tiba." },
  { "en": "Mengapa ESD (Electrostatic Discharge) Berbahaya Elektronik?", "id": "Dapat Merusak Komponen Semikonduktor Sensitif." },
  { "en": "Bagaimana Mencegah Kerusakan ESD (Electrostatic Discharge)?", "id": "Gunakan Grounding (Gelang Anti-Statis)." },
  { "en": "Apa Itu Efek Fotolistrik (Photoelectric Effect)?", "id": "Elektron Lepas Logam Disinari Cahaya." },
  { "en": "Apa Itu Efek Fotovoltaik (Photovoltaic Effect)?", "id": "Pembangkitan Tegangan Sambungan P-N Cahaya." },
  { "en": "Apa Aplikasi Efek Fotovoltaik (Photovoltaic Effect)?", "id": "Sel Surya (Solar Cell)." },
  { "en": "Apa Itu Efek Termoelektrik (Thermoelectric Effect)?", "id": "Konversi Langsung Beda Suhu Ke Listrik." },
  { "en": "Apa Itu Efek Seebeck (Seebeck Effect)?", "id": "Prinsip Dasar Termokopel." },
  { "en": "Apa Itu Efek Peltier (Peltier Effect)?", "id": "Transfer Panas Akibat Aliran Arus Listrik." },
  { "en": "Aplikasi Efek Peltier (Peltier Effect)?", "id": "Pendingin Termoelektrik (TEC)." },
  { "en": "Apa Itu Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Listrik Dihasilkan Tekanan Mekanis." },
  { "en": "Aplikasi Efek Piezoelektrik (Piezoelectric Effect)?", "id": "Sensor Tekanan, Kristal Osilator, Buzzer." },
  { "en": "Apa Itu Efek Hall (Hall Effect)?", "id": "Tegangan Timbul Konduktor Medan Magnet." },
  { "en": "Aplikasi Sensor Efek Hall (Hall Effect)?", "id": "Deteksi Posisi, Kecepatan Putar, Arus." },
  { "en": "Apa Itu Gelombang (Wave) Suara?", "id": "Gelombang Mekanik Merambat Lewat Medium." },
  { "en": "Apa Itu Frekuensi (Frequency) Suara?", "id": "Menentukan Tinggi Rendahnya Nada (Pitch)." },
  { "en": "Apa Itu Amplitudo (Amplitude) Suara?", "id": "Menentukan Keras Lemahnya Suara (Loudness)." },
  { "en": "Apa Satuan Keras Lemah (Loudness) Suara?", "id": "Desibel (dB)." },
  { "en": "Apa Rentang Pendengaran (Hearing Range) Manusia?", "id": "Dua Puluh Hertz Hingga Dua Puluh Kilohertz." },
  { "en": "Apa Itu Suara Ultrasonik (Ultrasonic)?", "id": "Suara Frekuensi Di Atas Pendengaran Manusia." },
  { "en": "Apa Itu Suara Infrasonik (Infrasonic)?", "id": "Suara Frekuensi Di Bawah Pendengaran Manusia." },
  { "en": "Apa Itu Mikrofon (Microphone)?", "id": "Transduser Mengubah Suara Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Pengeras Suara (Loudspeaker)?", "id": "Transduser Mengubah Sinyal Listrik Menjadi Suara." },
  { "en": "Apa Itu Sistem (System) Audio?", "id": "Sistem Reproduksi, Perekaman Suara." },
  { "en": "Apa Itu Penguat (Amplifier) Audio?", "id": "Menguatkan Sinyal Listrik Frekuensi Audio." },
  { "en": "Apa Itu Crossover (Crossover) Audio?", "id": "Filter Pembagi Frekuensi Speaker." },
  { "en": "Apa Itu Tweeter (Tweeter) Speaker?", "id": "Speaker Khusus Frekuensi Tinggi (Treble)." },
  { "en": "Apa Itu Woofer (Woofer) Speaker?", "id": "Speaker Khusus Frekuensi Rendah (Bass)." },
  { "en": "Apa Itu Subwoofer (Subwoofer)?", "id": "Speaker Khusus Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Impedansi (Impedance) Speaker?", "id": "Hambatan Total Speaker Terhadap Arus AC." },
  { "en": "Satuan Impedansi (Impedance) Speaker Umum?", "id": "Empat Ohm, Enam Ohm, Delapan Ohm." },
  { "en": "Apa Itu Distorsi Harmonik Total (THD) Audio?", "id": "Ukuran Distorsi Sinyal Audio." },
  { "en": "Apa Itu Rasio Sinyal-Derau (SNR) Audio?", "id": "Ukuran Kualitas Sinyal Audio Relatif Noise." },
  { "en": "Apa Itu Akustika (Acoustics)?", "id": "Ilmu Tentang Suara, Getaran." },
  { "en": "Apa Itu Gema (Echo)?", "id": "Pantulan Suara Jelas Terdengar." },
  { "en": "Apa Itu Gaung (Reverberation)?", "id": "Pantulan Suara Bertahan Dalam Ruangan." },
  { "en": "Apa Itu Penyerapan (Absorption) Suara?", "id": "Pengurangan Energi Suara Oleh Permukaan." },
  { "en": "Apa Itu Insulasi (Insulation) Suara?", "id": "Pencegahan Transmisi Suara Antar Ruang." },
  { "en": "Apa Itu Pembatalan Derau Aktif (ANC)?", "id": "Active Noise Cancellation (Kurangi Noise)." },
  { "en": "Bagaimana ANC (Active Noise Cancellation) Bekerja?", "id": "Menghasilkan Suara Anti-Fasa Derau." },
  { "en": "Apa Itu Cahaya (Light)?", "id": "Radiasi Elektromagnetik Dapat Terlihat." },
  { "en": "Apa Itu Optika (Optics)?", "id": "Studi Perilaku, Sifat Cahaya." },
  { "en": "Apa Itu Pemantulan (Reflection) Cahaya?", "id": "Cahaya Memantul Kembali Dari Permukaan." },
  { "en": "Apa Itu Pembiasan (Refraction) Cahaya?", "id": "Cahaya Membelok Lewati Medium Berbeda." },
  { "en": "Apa Itu Lensa (Lens)?", "id": "Benda Optik Membiaskan Cahaya." },
  { "en": "Apa Itu Lensa (Lens) Cembung (Konvergen)?", "id": "Mengumpulkan Sinar Cahaya." },
  { "en": "Apa Itu Lensa (Lens) Cekung (Divergen)?", "id": "Menyebarkan Sinar Cahaya." },
  { "en": "Apa Itu Komponen (Component) Aktif?", "id": "Komponen Membutuhkan Catu Daya (Transistor)." },
  { "en": "Apa Itu Komponen (Component) Pasif?", "id": "Komponen Tak Butuh Catu Daya (Resistor)." },
  { "en": "Apa Fungsi (Function) Kapasitor Kopling?", "id": "Melewatkan Sinyal AC, Memblokir DC." },
  { "en": "Apa Fungsi (Function) Kapasitor Bypass?", "id": "Melewatkan Noise AC Ke Ground." },
  { "en": "Apa Itu Konstanta (Constant) Waktu RC?", "id": "Ukuran Waktu Pengisian Kapasitor." },
  { "en": "Apa Rumus (Formula) Konstanta Waktu RC?", "id": "Tau (Τ) = R * C." },
  { "en": "Apa Itu Konstanta (Constant) Waktu RL?", "id": "Ukuran Waktu Arus Induktor Naik." },
  { "en": "Apa Rumus (Formula) Konstanta Waktu RL?", "id": "Tau (Τ) = L / R." },
  { "en": "Apa Itu Rangkaian (Circuit) Orde Pertama?", "id": "Rangkaian Mengandung Satu Elemen Penyimpan." },
  { "en": "Apa Itu Rangkaian (Circuit) Orde Kedua?", "id": "Rangkaian Mengandung Dua Elemen Penyimpan." },
  { "en": "Contoh Rangkaian (Circuit) Orde Kedua?", "id": "Rangkaian RLC (Resistor Induktor Kapasitor)." },
  { "en": "Apa Itu Respons (Response) Transien?", "id": "Respon Sementara Saat Terjadi Perubahan." },
  { "en": "Apa Itu Respons (Response) Keadaan Tunak?", "id": "Respon Stabil Setelah Waktu Lama." },
  { "en": "Apa Itu Frekuensi (Frequency) Resonansi Alami?", "id": "Frekuensi Osilasi Sistem Tanpa Redaman." },
  { "en": "Apa Itu Redaman (Damping)?", "id": "Pengurangan Amplitudo Osilasi." },
  { "en": "Apa Itu Underdamped (Redaman Kurang)?", "id": "Respon Berosilasi Sebelum Stabil." },
  { "en": "Apa Itu Overdamped (Redaman Lebih)?", "id": "Respon Lambat Tanpa Osilasi." },
  { "en": "Apa Itu Critically Damped (Redaman Kritis)?", "id": "Respon Tercepat Tanpa Osilasi." },
  { "en": "Apa Itu Nilai (Value) RMS?", "id": "Root Mean Square (Nilai Efektif)." },
  { "en": "Apa Nilai (Value) RMS Sinus Murni?", "id": "Nilai Puncak Dibagi Akar Dua." },
  { "en": "Apa Itu Faktor (Factor) Puncak (Crest Factor)?", "id": "Rasio Nilai Puncak Terhadap RMS." },
  { "en": "Apa Itu Faktor (Factor) Bentuk (Form Factor)?", "id": "Rasio Nilai RMS Terhadap Rata-Rata." },
  { "en": "Apa Itu Daya (Power) Nyata (P)?", "id": "Daya Rata-Rata Diserap Beban (Watt)." },
  { "en": "Apa Itu Daya (Power) Reaktif (Q)?", "id": "Daya Medan Magnet/Listrik (VAR)." },
  { "en": "Apa Itu Daya (Power) Semu (S)?", "id": "Daya Total Rangkaian AC (VA)." },
  { "en": "Apa Itu Segitiga (Triangle) Daya?", "id": "Hubungan Vektorial P, Q, Dan S." },
  { "en": "Apa Itu Faktor (Factor) Daya (PF)?", "id": "Rasio Daya Nyata, Daya Semu (P/S)." },
  { "en": "Apa Itu Faktor (Factor) Daya Leading?", "id": "Arus Mendahului Tegangan (Kapasitif)." },
  { "en": "Apa Itu Faktor (Factor) Daya Lagging?", "id": "Arus Tertinggal Tegangan (Induktif)." },
  { "en": "Mengapa Faktor (Factor) Daya Rendah Buruk?", "id": "Arus Saluran Tinggi, Rugi Besar." },
  { "en": "Bagaimana Perbaiki Faktor (Factor) Daya Lagging?", "id": "Pasang Kapasitor Bank Paralel." },
  { "en": "Apa Itu Tegangan (Voltage) Fasa (Vp)?", "id": "Tegangan Antara Fasa Dan Netral." },
  { "en": "Apa Itu Tegangan (Voltage) Line (Vl)?", "id": "Tegangan Antara Dua Fasa." },
  { "en": "Hubungan Vl Dan Vp Sistem Bintang?", "id": "Vl = Akar Tiga Kali Vp." },
  { "en": "Hubungan Il Dan Ip Sistem Bintang?", "id": "Il = Ip." },
  { "en": "Hubungan Vl Dan Vp Sistem Delta?", "id": "Vl = Vp." },
  { "en": "Hubungan Il Dan Ip Sistem Delta?", "id": "Il = Akar Tiga Kali Ip." },
  { "en": "Apa Itu Beban (Load) Seimbang?", "id": "Impedansi Ketiga Fasa Sama Besar." },
  { "en": "Arus (Current) Netral Beban Seimbang?", "id": "Nol Ampere." },
  { "en": "Apa Itu Komponen (Component) Simetris?", "id": "Metode Analisis Sistem Tiga Fasa Asimetris." },
  { "en": "Apa Saja Komponen (Component) Simetris?", "id": "Urutan Positif, Negatif, Nol." },
  { "en": "Apa Itu Dioda (Diode) Ideal?", "id": "Saklar Sempurna Satu Arah." },
  { "en": "Apa Itu Tegangan (Voltage) Cut-In Dioda?", "id": "Tegangan Minimum Konduksi." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Pengubah AC Menjadi DC." },
  { "en": "Apa Itu Riak (Ripple) Tegangan?", "id": "Sisa AC Pada Output DC." },
  { "en": "Fungsi (Function) Kapasitor Filter Penyearah?", "id": "Mengurangi Tegangan Riak (Ripple)." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Dioda Regulator Tegangan Mundur." },
  { "en": "Apa Itu Clipper (Clipping Circuit)?", "id": "Rangkaian Pemotong Level Tegangan." },
  { "en": "Apa Itu Clamper (Clamping Circuit)?", "id": "Rangkaian Penggeser Level DC." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Bipolar Junction Transistor." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Field Effect Transistor." },
  { "en": "Apa Itu Penguatan (Gain) Arus (Beta) BJT?", "id": "Rasio Arus Kolektor Terhadap Basis." },
  { "en": "Apa Daerah (Region) Operasi BJT?", "id": "Cutoff, Aktif, Saturasi." },
  { "en": "Kapan BJT (Bipolar Junction Transistor) Berfungsi Penguat?", "id": "Saat Berada Di Daerah Aktif." },
  { "en": "Apa Itu Garis (Line) Beban DC?", "id": "Grafik Titik Operasi DC Transistor." },
  { "en": "Apa Itu Titik (Point) Q (Quiescent)?", "id": "Titik Operasi Diam DC Transistor." },
  { "en": "Apa Itu Bias (Bias) Transistor?", "id": "Pemberian Tegangan DC Atur Titik Q." },
  { "en": "Apa Itu Penguat (Amplifier) Common Emitter?", "id": "Penguat BJT (Bipolar Junction Transistor) Umum (Gain Tinggi)." },
  { "en": "Apa Itu Penguat (Amplifier) Common Collector?", "id": "Pengikut Emitor (Buffer Impedansi)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET (Field Effect Transistor) Gerbang Terisolasi." },
  { "en": "Apa Keunggulan (Advantage) MOSFET?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu Penguat (Amplifier) Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Umpan Balik (Feedback) Negatif?", "id": "Menstabilkan Penguatan Op-Amp." },
  { "en": "Apa Itu Penguat (Amplifier) Inverting?", "id": "Output Terbalik Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat (Amplifier) Non-Inverting?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut (Follower) Tegangan?", "id": "Buffer (Buffer) Op-Amp Gain Satu." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Itu Gerbang (Gate) Logika?", "id": "Elemen Dasar Sirkuit Digital." },
  { "en": "Apa Itu Aljabar (Algebra) Boolean?", "id": "Matematika Logika Biner." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Memori Satu Bit." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Pencacah Urutan Digital." },
  { "en": "Apa Itu Memori (Memory) RAM?", "id": "Random Access Memory (Volatil)." },
  { "en": "Apa Itu Memori (Memory) ROM?", "id": "Read Only Memory (Non-Volatil)." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Jaringan Listrik Skala Besar." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Konversi Daya Listrik." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Pengubah Daya DC Ke AC." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Output Mempengaruhi Input Kontrol." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Kuantifikasi Besaran Fisik." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Visualisasi Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik V, A, Ω." },
  { "en": "Apa Itu Medan (Field) Elektromagnetik?", "id": "Medan Kombinasi Listrik, Magnet." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang EM." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Penumpangan Informasi Ke Pembawa." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Jaringan." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Mudah Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Sulit Alirkan Listrik." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Konduktivitas Antara." },
  { "en": "Apa Itu Hukum (Law) Ohm?", "id": "V = I * R." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Energi Listrik (Watt)." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total AC." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Non-Resistif AC." },
  { "en": "Apa Itu Sinyal (Signal) Analog?", "id": "Sinyal Nilai Kontinu." },
  { "en": "Apa Itu Sinyal (Signal) Digital?", "id": "Sinyal Nilai Diskrit." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Gangguan Acak Sinyal." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Teknologi Robot." },
  { "en": "Apa Itu Kecerdasan (Intelligence) Buatan (AI)?", "id": "Mesin Tiru Kecerdasan Manusia." },
  { "en": "Apa Itu Pembelajaran (Learning) Mesin (ML)?", "id": "AI (Artificial Intelligence) Belajar Dari Data." },
  { "en": "Apa Itu Internet (Internet) of Things (IoT)?", "id": "Jaringan Perangkat Fisik Terhubung Internet." },
  { "en": "Apa Itu Keamanan (Security) Siber?", "id": "Perlindungan Sistem Digital Dari Serangan." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Pengkodean Data Agar Aman." },
  { "en": "Apa Itu Energi (Energy) Terbarukan?", "id": "Sumber Energi Alam Berkelanjutan." },
  { "en": "Apa Itu Sel (Cell) Surya?", "id": "Pengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Turbin (Turbine) Angin?", "id": "Pengubah Energi Angin Listrik." },
  { "en": "Apa Itu Penyimpanan (Storage) Energi?", "id": "Menyimpan Energi Listrik (Baterai)." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Jaringan Listrik Digital Komunikasi." },
  { "en": "Apa Itu Kendaraan (Vehicle) Listrik?", "id": "Kendaraan Penggerak Motor Listrik." },
  { "en": "Apa Itu Pencitraan (Imaging) Medis?", "id": "Visualisasi Dalam Tubuh (MRI, CT)." },
  { "en": "Apa Itu Elektronika (Electronics) Biomedis?", "id": "Elektronika Aplikasi Medis." },
  { "en": "Apa Itu Sensor (Sensor) Biomedis?", "id": "Sensor Ukur Sinyal Tubuh (ECG)." },
  { "en": "Apa Itu Sinyal (Signal) ECG?", "id": "Rekaman Aktivitas Listrik Jantung." },
  { "en": "Apa Itu Sinyal (Signal) EEG?", "id": "Rekaman Aktivitas Listrik Otak." },
  { "en": "Apa Itu Sinyal (Signal) EMG?", "id": "Rekaman Aktivitas Listrik Otot." },
  { "en": "Apa Itu Alat Pacu Jantung (Pacemaker)?", "id": "Perangkat Stimulasi Ritme Jantung." },
  { "en": "Apa Itu Defibrilator (Defibrillator)?", "id": "Alat Kejut Listrik Atasi Aritmia." },
  { "en": "Apa Itu Teknik (Engineering) Material?", "id": "Studi Sifat, Aplikasi Material." },
  { "en": "Apa Itu Logam (Metal)?", "id": "Konduktif, Kuat, Ulet." },
  { "en": "Apa Itu Keramik (Ceramic)?", "id": "Keras, Rapuh, Tahan Panas." },
  { "en": "Apa Itu Polimer (Polymer)?", "id": "Molekul Rantai Panjang (Plastik)." },
  { "en": "Apa Itu Komposit (Composite)?", "id": "Gabungan Dua Material Berbeda." },
  { "en": "Apa Itu Mekatronika (Mechatronics)?", "id": "Integrasi Mekanik, Elektronik, Kontrol." },
  { "en": "Apa Itu Termodinamika (Thermodynamics)?", "id": "Studi Hubungan Panas, Kerja, Energi." },
  { "en": "Apa Itu Perpindahan (Heat Transfer) Panas?", "id": "Pergerakan Energi Termal." },
  { "en": "Mode Perpindahan (Heat Transfer) Panas?", "id": "Konduksi, Konveksi, Radiasi." },
  { "en": "Apa Itu Mekanika (Mechanics) Fluida?", "id": "Studi Perilaku Fluida." },
  { "en": "Apa Itu Tegangan (Voltage) Nominal?", "id": "Tegangan Operasi Desain Perangkat." },
  { "en": "Apa Itu Arus (Current) Nominal?", "id": "Arus Operasi Desain Perangkat." },
  { "en": "Apa Itu Daya (Power) Nominal?", "id": "Daya Operasi Desain Perangkat." },
  { "en": "Apa Itu Frekuensi (Frequency) Nominal?", "id": "Frekuensi Operasi Desain Perangkat AC." },
  { "en": "Apa Itu Efisiensi (Efficiency)?", "id": "Rasio Daya Output Berguna Terhadap Input." },
  { "en": "Apa Itu Rugi (Loss) Daya?", "id": "Energi Hilang (Biasanya Panas)." },
  { "en": "Apa Itu Siklus (Cycle) Kerja?", "id": "Duty Cycle (Persentase Waktu On)." },
  { "en": "Apa Itu Resistansi (Resistance) Kontak?", "id": "Hambatan Pada Titik Sambungan Listrik." },
  { "en": "Apa Itu Jatuh (Drop) Tegangan?", "id": "Penurunan Tegangan Akibat Resistansi." },
  { "en": "Apa Itu Kapasitansi (Capacitance) Parasitik?", "id": "Kapasitansi Tak Diinginkan Antar Komponen." },
  { "en": "Apa Itu Induktansi (Inductance) Parasitik?", "id": "Induktansi Tak Diinginkan Jalur Konduktor." },
  { "en": "Apa Itu Kopling (Coupling) Elektromagnetik?", "id": "Interaksi Medan Antar Rangkaian." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan Akibat Radiasi Elektromagnetik." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Kemampuan Perangkat Tahan Gangguan EMI." },
  { "en": "Apa Itu Shielding (Perisai)?", "id": "Bahan Pelindung Terhadap EMI." },
  { "en": "Apa Itu Filter (Filter) EMI?", "id": "Rangkaian Penekan Gangguan EMI Konduksi." },
  { "en": "Apa Itu Sirkuit (Circuit) Rangkaian Terbuka?", "id": "Jalur Listrik Tidak Terhubung Lengkap." },
  { "en": "Apa Arus (Current) Pada Sirkuit Terbuka?", "id": "Arus Mengalir Bernilai Nol." },
  { "en": "Apa Itu Sirkuit (Circuit) Hubungan Singkat?", "id": "Jalur Listrik Resistansi Sangat Rendah." },
  { "en": "Apa Tegangan (Voltage) Pada Hubungan Singkat Ideal?", "id": "Tegangan Bernilai Nol Volt." },
  { "en": "Apa Bahaya (Danger) Hubungan Singkat?", "id": "Arus Sangat Tinggi, Kebakaran." },
  { "en": "Apa Itu Rugi (Loss) Tembaga?", "id": "Rugi Daya Akibat Resistansi Kawat (I²R)." },
  { "en": "Apa Itu Rugi (Loss) Inti?", "id": "Rugi Histeresis Dan Arus Eddy." },
  { "en": "Apa Itu Arus Eddy (Eddy Current)?", "id": "Arus Berputar Di Inti Konduktif." },
  { "en": "Bagaimana Mengurangi Rugi Arus Eddy (Eddy Current)?", "id": "Menggunakan Inti Berlaminasi (Berlapis)." },
  { "en": "Apa Itu Histeresis (Hysteresis) Magnetik?", "id": "Keterlambatan Magnetisasi Bahan Inti." },
  { "en": "Apa Itu Fluks (Flux) Magnetik?", "id": "Ukuran Total Medan Magnet." },
  { "en": "Apa Satuan Fluks (Flux) Magnetik?", "id": "Weber (Wb)." },
  { "en": "Apa Itu Kerapatan (Density) Fluks Magnet?", "id": "Fluks Magnet Per Satuan Luas." },
  { "en": "Apa Satuan Kerapatan (Density) Fluks Magnet?", "id": "Tesla (T)." },
  { "en": "Apa Itu Gaya Gerak Magnet (MMF)?", "id": "Penyebab Timbulnya Fluks Magnet." },
  { "en": "Apa Itu Reluktansi (Reluctance) Magnetik?", "id": "Hambatan Terhadap Aliran Fluks Magnet." },
  { "en": "Apa Itu Permeabilitas (Permeability) Magnetik?", "id": "Kemampuan Bahan Hantarkan Fluks Magnet." },
  { "en": "Apa Itu Saturasi (Saturation) Magnetik?", "id": "Kondisi Inti Jenuh Medan Magnet." },
  { "en": "Apa Itu GGL (Gaya Gerak Listrik) Induksi?", "id": "Tegangan Timbul Akibat Perubahan Fluks." },
  { "en": "Apa Bunyi Hukum Faraday (Faraday's Law) Induksi?", "id": "GGL Proporsional Laju Perubahan Fluks." },
  { "en": "Apa Bunyi Hukum Lenz (Lenz's Law)?", "id": "Arah Arus Induksi Melawan Perubahan." },
  { "en": "Apa Itu Induktansi (Inductance) Diri?", "id": "GGL Induksi Diri Sendiri (Self-Inductance)." },
  { "en": "Apa Itu Induktansi (Inductance) Mutual?", "id": "GGL Induksi Akibat Kumparan Lain." },
  { "en": "Aplikasi Utama Induktansi (Inductance) Mutual?", "id": "Transformator (Transformer)." },
  { "en": "Apa Itu Resistor (Resistor) Film Karbon?", "id": "Resistor Umum Lapisan Karbon." },
  { "en": "Apa Itu Resistor (Resistor) Film Logam?", "id": "Resistor Presisi Lapisan Logam." },
  { "en": "Apa Itu Resistor (Resistor) Kawat Lilit?", "id": "Resistor Lilitan Kawat (Daya Tinggi)." },
  { "en": "Apa Itu Kapasitor (Capacitor) Keramik?", "id": "Kapasitor Non-Polar Frekuensi Tinggi." },
  { "en": "Apa Itu Kapasitor (Capacitor) Elektrolit (Elco)?", "id": "Kapasitor Polar Nilai Tinggi." },
  { "en": "Apa Itu Kapasitor (Capacitor) Variabel (Varco)?", "id": "Kapasitansi Dapat Diatur Mekanis." },
  { "en": "Apa Itu Dioda (Diode) Ideal?", "id": "Saklar Sempurna Satu Arah." },
  { "en": "Apa Itu Tegangan (Voltage) Lutut Dioda?", "id": "Tegangan Minimum Mulai Konduksi." },
  { "en": "Apa Itu Dioda (Diode) Penyearah?", "id": "Mengubah Arus AC Menjadi DC." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Menstabilkan Tegangan Pada Breakdown." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Memancarkan Cahaya." },
  { "en": "Apa Itu Fotodioda (Photodiode)?", "id": "Dioda Peka Terhadap Cahaya." },
  { "en": "Apa Itu Dioda (Diode) Schottky?", "id": "Dioda Cepat, Jatuh Tegangan Rendah." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Bipolar Junction Transistor." },
  { "en": "Bagaimana Transistor BJT (Bipolar Junction Transistor) Dikontrol?", "id": "Oleh Arus Basis (Current Controlled)." },
  { "en": "Apa Itu Penguatan (Gain) Arus BJT?", "id": "Beta (β) Atau hFE." },
  { "en": "Apa Itu Daerah (Region) Aktif BJT?", "id": "Transistor Sebagai Penguat Linear." },
  { "en": "Apa Itu Transistor (Transistor) FET?", "id": "Field Effect Transistor." },
  { "en": "Bagaimana Transistor FET (Field Effect Transistor) Dikontrol?", "id": "Oleh Tegangan Gate (Voltage Controlled)." },
  { "en": "Apa Keunggulan Utama FET (Field Effect Transistor)?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET (Field Effect Transistor) Gerbang Terisolasi Oksida." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Logika Kombinasi NMOS Dan PMOS." },
  { "en": "Apa Itu Thyristor (Thyristor)?", "id": "Keluarga Saklar Semikonduktor Empat Lapis." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Saklar DC Terkendali Gerbang." },
  { "en": "Apa Itu TRIAC (Triode for AC)?", "id": "Saklar AC Dua Arah Terkendali." },
  { "en": "Apa Itu IGBT (Insulated Gate Bipolar Transistor)?", "id": "Gabungan MOSFET (Input) BJT (Output)." },
  { "en": "Apa Itu Penguat (Amplifier) Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Umpan Balik (Feedback) Negatif?", "id": "Menstabilkan Gain Op-Amp." },
  { "en": "Apa Itu Penguat (Amplifier) Inverting?", "id": "Output Berbeda Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat (Amplifier) Non-Inverting?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Pengikut (Follower) Tegangan?", "id": "Buffer (Buffer) Gain Satu." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter Menggunakan Op-Amp (Bisa Gain)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Penghasil Sinyal Periodik." },
  { "en": "Apa Itu Analisis (Analysis) Rangkaian AC?", "id": "Analisis Rangkaian Dengan Sumber AC (Sinusoidal)." },
  { "en": "Apa Itu Fasor (Phasor)?", "id": "Representasi Bilangan Kompleks Sinyal Sinusoidal." },
  { "en": "Mengapa Menggunakan Fasor (Phasor)?", "id": "Ubah Persamaan Diferensial Jadi Aljabar." },
  { "en": "Apa Itu Impedansi (Impedance) (Z)?", "id": "Hambatan Total Rangkaian AC (Kompleks)." },
  { "en": "Apa Rumus (Formula) Impedansi (Z) Kompleks?", "id": "Z = R + Jx." },
  { "en": "Apa Itu Reaktansi (Reactance) (X)?", "id": "Bagian Imajiner Impedansi (L Atau C)." },
  { "en": "Apa Impedansi (Impedance) Resistor (R)?", "id": "Z = R (Riil)." },
  { "en": "Apa Impedansi (Impedance) Induktor (L)?", "id": "Z = Jωl." },
  { "en": "Apa Impedansi (Impedance) Kapasitor (C)?", "id": "Z = 1 / (Jωc) = -J / (Ωc)." },
  { "en": "Apa Itu Admitansi (Admittance) (Y)?", "id": "Kebalikan Dari Impedansi (Y = 1/Z)." },
  { "en": "Apa Itu Konduktansi (Conductance) (G)?", "id": "Bagian Riil Dari Admitansi." },
  { "en": "Apa Itu Suseptansi (Susceptance) (B)?", "id": "Bagian Imajiner Dari Admitansi." },
  { "en": "Apa Itu Daya (Power) Kompleks (S)?", "id": "Kombinasi Daya Nyata Dan Reaktif." },
  { "en": "Apa Rumus (Formula) Daya Kompleks (S)?", "id": "S = P + Jq." },
  { "en": "Apa Itu Daya (Power) Nyata (P)?", "id": "Daya Rata-Rata Diserap (Watt)." },
  { "en": "Apa Itu Daya (Power) Reaktif (Q)?", "id": "Daya Bolak-Balik Tersimpan (VAR)." },
  { "en": "Apa Itu Daya (Power) Semu (|S|)?", "id": "Magnitudo Daya Kompleks (VA)." },
  { "en": "Apa Itu Faktor (Factor) Daya (PF)?", "id": "Rasio Daya Nyata, Daya Semu (Cos φ)." },
  { "en": "Apa Itu Faktor (Factor) Daya Lagging?", "id": "Beban Induktif (Arus Tertinggal Tegangan)." },
  { "en": "Apa Itu Faktor (Factor) Daya Leading?", "id": "Beban Kapasitif (Arus Mendahului Tegangan)." },
  { "en": "Apa Itu Koreksi (Correction) Faktor Daya?", "id": "Menambah Komponen Reaktif (Kapasitor)." },
  { "en": "Apa Itu Sistem (System) Tiga Fasa?", "id": "Tiga Sumber Tegangan AC Beda Fasa 120°." },
  { "en": "Apa Keuntungan (Advantage) Sistem Tiga Fasa?", "id": "Transmisi Daya Efisien, Daya Konstan." },
  { "en": "Apa Itu Koneksi (Connection) Bintang (Y)?", "id": "Ujung Fasa Terhubung Titik Netral." },
  { "en": "Apa Itu Koneksi (Connection) Delta (Δ)?", "id": "Fasa Terhubung Membentuk Loop Segitiga." },
  { "en": "Apa Itu Tegangan (Voltage) Line?", "id": "Tegangan Antara Dua Saluran Fasa." },
  { "en": "Apa Itu Tegangan (Voltage) Fasa?", "id": "Tegangan Antara Satu Fasa Dan Netral." },
  { "en": "Hubungan Vl, Vp Di Koneksi Bintang?", "id": "Vl = √3 * Vp." },
  { "en": "Hubungan Vl, Vp Di Koneksi Delta?", "id": "Vl = Vp." },
  { "en": "Apa Itu Arus (Current) Line?", "id": "Arus Mengalir Di Saluran Fasa." },
  { "en": "Apa Itu Arus (Current) Fasa?", "id": "Arus Mengalir Lewat Beban Fasa." },
  { "en": "Hubungan Il, Ip Di Koneksi Bintang?", "id": "Il = Ip." },
  { "en": "Hubungan Il, Ip Di Koneksi Delta?", "id": "Il = √3 * Ip." },
  { "en": "Apa Rumus (Formula) Daya Tiga Fasa?", "id": "P = √3 * Vl * Il * Cos(φ)." },
  { "en": "Apa Itu Beban (Load) Seimbang?", "id": "Impedansi Ketiga Fasa Sama." },
  { "en": "Apa Itu Beban (Load) Tidak Seimbang?", "id": "Impedansi Ketiga Fasa Berbeda." },
  { "en": "Apa Itu Komponen (Component) Simetris?", "id": "Metode Analisis Sistem Tak Seimbang." },
  { "en": "Apa Tiga Komponen (Component) Simetris?", "id": "Urutan Positif, Negatif, Nol." },
  { "en": "Apa Itu Komponen (Component) Urutan Positif?", "id": "Sistem Tiga Fasa Seimbang Normal." },
  { "en": "Apa Itu Komponen (Component) Urutan Negatif?", "id": "Sistem Tiga Fasa Urutan Terbalik." },
  { "en": "Apa Itu Komponen (Component) Urutan Nol?", "id": "Sistem Tiga Fasa Sefasa (Gangguan Tanah)." },
  { "en": "Apa Itu Respons (Response) Frekuensi?", "id": "Perilaku Rangkaian Vs Frekuensi." },
  { "en": "Apa Itu Plot (Plot) Bode?", "id": "Grafik Logaritmik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Resonansi (Resonance) Seri?", "id": "Impedansi Minimum (Z=R)." },
  { "en": "Apa Itu Resonansi (Resonance) Paralel?", "id": "Impedansi Maksimum (Z=R)." },
  { "en": "Apa Itu Faktor (Factor) Kualitas (Q)?", "id": "Ukuran Tajamnya Resonansi." },
  { "en": "Apa Itu Bandwidth (Bandwidth)?", "id": "Rentang Frekuensi Antara Titik -3dB." },
  { "en": "Apa Itu Filter (Filter) Lolos Rendah?", "id": "Melewatkan Frekuensi Rendah." },
  { "en": "Apa Itu Filter (Filter) Lolos Tinggi?", "id": "Melewatkan Frekuensi Tinggi." },
  { "en": "Apa Itu Filter (Filter) Lolos Pita?", "id": "Melewatkan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter (Filter) Tolak Pita?", "id": "Menolak Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Transformasi (Transform) Laplace?", "id": "Mengubah Persamaan Diferensial Ke Aljabar." },
  { "en": "Apa Itu Domain-S (S-Domain)?", "id": "Domain Frekuensi Kompleks (σ + Jω)." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function)?", "id": "Rasio Output(s) Terhadap Input(s)." },
  { "en": "Apa Itu Pole (Kutub) Fungsi Transfer?", "id": "Akar Penyebut (Denominator)." },
  { "en": "Apa Itu Zero (Nol) Fungsi Transfer?", "id": "Akar Pembilang (Numerator)." },
  { "en": "Apa Hubungan Pole (Kutub), Kestabilan (Stability)?", "id": "Stabil Jika Pole Di Kiri Bidang-s." },
  { "en": "Apa Itu Respon (Response) Impuls?", "id": "Output Sistem Untuk Input Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Output Sistem LTI." },
  { "en": "Apa Itu Deret (Series) Fourier?", "id": "Representasi Sinyal Periodik Jumlahan Sinusoid." },
  { "en": "Apa Itu Transformasi (Transform) Fourier?", "id": "Representasi Frekuensi Sinyal Non-Periodik." },
  { "en": "Apa Itu Spektrum (Spectrum) Sinyal?", "id": "Plot Amplitudo/Fasa Vs Frekuensi." },
  { "en": "Apa Itu Transformasi (Transform) Fourier Diskrit (DFT)?", "id": "Transformasi Fourier Sinyal Waktu Diskrit." },
  { "en": "Apa Itu Transformasi (Transform) Fourier Cepat (FFT)?", "id": "Algoritma Efisien Hitung DFT." },
  { "en": "Apa Itu Jaringan (Network) Dua Port?", "id": "Rangkaian Listrik Dua Pasang Terminal." },
  { "en": "Parameter (Parameter) Apa Jaringan Dua Port?", "id": "Parameter Z, Y, H, ABCD." },
  { "en": "Apa Itu Parameter (Parameter) Z?", "id": "Parameter Impedansi (Tegangan Fungsi Arus)." },
  { "en": "Apa Itu Parameter (Parameter) Y?", "id": "Parameter Admitansi (Arus Fungsi Tegangan)." },
  { "en": "Apa Itu Parameter (Parameter) H?", "id": "Parameter Hibrid (Analisis Transistor)." },
  { "en": "Apa Itu Parameter (Parameter) ABCD?", "id": "Parameter Transmisi (Kaskade Jaringan)." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Konduktivitas Antara Logam, Isolator." },
  { "en": "Apa Itu Silikon (Silicon) (Si)?", "id": "Bahan Semikonduktor Paling Umum." },
  { "en": "Apa Itu Doping (Doping) Semikonduktor?", "id": "Penambahan Impuritas Atur Konduktivitas." },
  { "en": "Apa Itu Tipe-N (N-Type) Semikonduktor?", "id": "Doping Kelebihan Elektron (Donor)." },
  { "en": "Apa Itu Tipe-P (P-Type) Semikonduktor?", "id": "Doping Kekurangan Elektron (Akseptor)." },
  { "en": "Apa Itu Lubang (Hole)?", "id": "Pembawa Muatan Positif Semu Tipe-P." },
  { "en": "Apa Itu Sambungan (Junction) P-N?", "id": "Antarmuka Antara Tipe-P Dan Tipe-N." },
  { "en": "Apa Itu Daerah (Region) Deplesi?", "id": "Daerah Sekitar Sambungan P-N Miskin Muatan." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Sambungan P-N (Penyearah)." },
  { "en": "Apa Itu Bias (Bias) Maju?", "id": "Tegangan Positif Ke P, Negatif Ke N." },
  { "en": "Apa Itu Bias (Bias) Mundur?", "id": "Tegangan Negatif Ke P, Positif Ke N." },
  { "en": "Apa Itu Tegangan (Voltage) Lutut (Knee)?", "id": "Tegangan Maju Minimum Konduksi Dioda." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Pengubah AC Ke DC." },
  { "en": "Apa Itu Dioda (Diode) Zener?", "id": "Dioda Regulator Tegangan Mundur." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Memancarkan Cahaya." },
  { "en": "Apa Itu Transistor (Transistor) BJT?", "id": "Bipolar Junction Transistor." },
  { "en": "Apa Tiga Terminal (Terminal) BJT?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "BJT (Bipolar Junction Transistor) Digerakkan Oleh Apa?", "id": "Arus Basis (Current Controlled)." },
  { "en": "Apa Itu Penguatan (Gain) Arus (Beta)?", "id": "Rasio Arus Kolektor, Arus Basis." },
  { "en": "Apa Tiga Daerah (Region) Operasi BJT?", "id": "Cutoff, Aktif, Saturasi." },
  { "en": "Apa Itu FET (Field Effect Transistor)?", "id": "Transistor Efek Medan." },
  { "en": "FET (Field Effect Transistor) Digerakkan Oleh Apa?", "id": "Tegangan Gate (Voltage Controlled)." },
  { "en": "Apa Itu MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "FET (Field Effect Transistor) Gerbang Terisolasi Oksida." },
  { "en": "Apa Keunggulan (Advantage) MOSFET?", "id": "Impedansi Input Sangat Tinggi." },
  { "en": "Apa Itu CMOS (Complementary MOS)?", "id": "Logika Digital Kombinasi NMOS, PMOS." },
  { "en": "Apa Itu Penguat (Amplifier) Operasional (Op-Amp)?", "id": "Penguat Diferensial Gain Sangat Tinggi." },
  { "en": "Apa Itu Penguat (Amplifier) Inverting?", "id": "Penguat Op-Amp Output Terbalik Fasa." },
  { "en": "Apa Itu Penguat (Amplifier) Non-Inverting?", "id": "Penguat Op-Amp Output Sefasa." },
  { "en": "Apa Itu Pengikut (Follower) Tegangan?", "id": "Buffer (Buffer) Op-Amp Gain Satu." },
  { "en": "Apa Itu Komparator (Comparator)?", "id": "Membandingkan Dua Tegangan Input." },
  { "en": "Apa Output Komparator (Comparator) Ideal?", "id": "Tegangan Saturasi Positif Atau Negatif." },
  { "en": "Apa Itu Histeresis (Hysteresis) Komparator?", "id": "Perbedaan Tegangan Threshold Naik, Turun." },
  { "en": "Nama Lain Komparator (Comparator) Histeresis?", "id": "Schmitt Trigger (Schmitt Trigger)." },
  { "en": "Fungsi Histeresis (Hysteresis) Komparator?", "id": "Mencegah Osilasi Output Akibat Noise." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Keuntungan Filter (Filter) Aktif?", "id": "Bisa Punya Penguatan (Gain)." },
  { "en": "Apa Itu Filter (Filter) Butterworth?", "id": "Respons Passband Datar Maksimal." },
  { "en": "Apa Itu Filter (Filter) Chebyshev?", "id": "Respons Roll-Off Curam, Riak Passband." },
  { "en": "Apa Itu Filter (Filter) Bessel?", "id": "Respons Fasa Linear Terbaik (Delay Rata)." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman Roll-Off Filter." },
  { "en": "Berapa Roll-Off (Roll-Off) Filter Orde 1?", "id": "Dua Puluh Desibel Per Dekade (20 DB/Dec)." },
  { "en": "Berapa Roll-Off (Roll-Off) Filter Orde 2?", "id": "Empat Puluh Desibel Per Dekade (40 DB/Dec)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Syarat (Criterion) Osilasi Barkhausen?", "id": "Penguatan Loop = 1, Fasa Loop = 0°." },
  { "en": "Apa Itu Osilator (Oscillator) RC?", "id": "Osilator Penentu Frekuensi Resistor-Kapasitor." },
  { "en": "Contoh Osilator (Oscillator) RC?", "id": "Jembatan Wien, Geser Fasa." },
  { "en": "Apa Itu Osilator (Oscillator) LC?", "id": "Osilator Penentu Frekuensi Induktor-Kapasitor." },
  { "en": "Contoh Osilator (Oscillator) LC?", "id": "Colpitts, Hartley, Clapp." },
  { "en": "Apa Itu Osilator (Oscillator) Kristal?", "id": "Osilator Frekuensi Sangat Stabil." },
  { "en": "Apa Itu Timer (Timer) 555?", "id": "IC (Integrated Circuit) Serbaguna Timer, Osilator." },
  { "en": "Mode Operasi Timer (Timer) 555?", "id": "Astabil, Monostabil, Bistabil." },
  { "en": "Mode Astabil (Astable Mode) Timer 555?", "id": "Sebagai Osilator Gelombang Kotak." },
  { "en": "Mode Monostabil (Monostable Mode) Timer 555?", "id": "Sebagai Timer Satu Pulsa (One-Shot)." },
  { "en": "Apa Itu PLL (Phase-Locked Loop)?", "id": "Sistem Umpan Balik Pengunci Fasa." },
  { "en": "Aplikasi PLL (Phase-Locked Loop)?", "id": "Sintesis Frekuensi, Demodulasi FM." },
  { "en": "Apa Itu VCO (Voltage Controlled Oscillator)?", "id": "Osilator Frekuensi Diatur Tegangan." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Dasar Rangkaian Logika Digital." },
  { "en": "Apa Itu Logika (Logic) Biner?", "id": "Sistem Logika (0 Dan 1)." },
  { "en": "Apa Itu Aljabar (Algebra) Boolean?", "id": "Matematika Untuk Logika Digital." },
  { "en": "Apa Itu Gerbang (Gate) AND?", "id": "Output 1 Jika Semua Input 1." },
  { "en": "Apa Itu Gerbang (Gate) OR?", "id": "Output 1 Jika Salah Satu Input 1." },
  { "en": "Apa Itu Gerbang (Gate) NOT?", "id": "Inverter (Membalikkan Logika Input)." },
  { "en": "Apa Itu Gerbang (Gate) NAND?", "id": "Inversi Dari Gerbang AND." },
  { "en": "Apa Itu Gerbang (Gate) NOR?", "id": "Inversi Dari Gerbang OR." },
  { "en": "Apa Itu Gerbang (Gate) XOR?", "id": "Output 1 Jika Input Ganjil." },
  { "en": "Apa Itu Gerbang (Gate) XNOR?", "id": "Output 1 Jika Input Sama." },
  { "en": "Gerbang (Gate) Apa Yang Universal?", "id": "Gerbang NAND Dan NOR." },
  { "en": "Apa Itu Sirkuit (Circuit) Kombinasional?", "id": "Output Tergantung Input Saat Ini." },
  { "en": "Apa Itu Sirkuit (Circuit) Sekuensial?", "id": "Output Tergantung Input, Keadaan Internal." },
  { "en": "Apa Elemen (Element) Memori Sekuensial?", "id": "Flip-Flop (Flip-Flop) Atau Latch (Latch)." },
  { "en": "Apa Itu Sinyal (Signal) Clock?", "id": "Sinyal Sinkronisasi Rangkaian Sekuensial." },
  { "en": "Apa Beda Latch Dan Flip-Flop?", "id": "Latch (Sensitif Level), Flip-Flop (Sensitif Tepi)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) SR?", "id": "Set-Reset Flip-Flop." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) D?", "id": "Data Flip-Flop (Menyimpan Data)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) JK?", "id": "Flip-Flop Universal (Set, Reset, Toggle)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) T?", "id": "Toggle Flip-Flop (Membalik Keadaan)." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pencacah Urutan Digital." },
  { "en": "Apa Itu Counter (Pencacah) Sinkron?", "id": "Flip-Flop Di-Clock Bersamaan." },
  { "en": "Apa Itu Counter (Pencacah) Asinkron (Ripple)?", "id": "Output Memicu Clock Berikutnya." },
  { "en": "Apa Itu Modulus (Modulus) Counter?", "id": "Jumlah Keadaan (State) Counter." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Pemilih Data (Banyak Ke Satu)." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Distributor Data (Satu Ke Banyak)." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Konversi Input Aktif Ke Biner." },
  { "en": "Apa Itu Decoder (Decoder)?", "id": "Konversi Biner Ke Output Aktif." },
  { "en": "Apa Itu Memori (Memory) Komputer?", "id": "Penyimpanan Data Digital." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Volatil Baca Tulis." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Non-Volatil Hanya Baca." },
  { "en": "Apa Itu SRAM (Static RAM)?", "id": "RAM (Random Access Memory) Berbasis Flip-Flop (Cepat)." },
  { "en": "Apa Itu DRAM (Dynamic RAM)?", "id": "RAM (Random Access Memory) Berbasis Kapasitor (Perlu Refresh)." },
  { "en": "Apa Itu Memori (Memory) Flash?", "id": "Memori Non-Volatil Tipe EEPROM." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor) (MPU)?", "id": "CPU (Central Processing Unit) Dalam Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller) (MCU)?", "id": "Komputer Kecil Dalam Chip." },
  { "en": "Apa Beda MPU (Microprocessor Unit) Dan MCU (Microcontroller Unit)?", "id": "MCU (Microcontroller Unit) Punya Memori, I/O Internal." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Konverter Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Konverter Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Resolusi (Resolution) ADC/DAC?", "id": "Jumlah Bit Representasi Digital." },
  { "en": "Apa Itu Laju (Rate) Sampling?", "id": "Frekuensi Pengambilan Sampel Sinyal." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Pengubah Besaran Fisik Ke Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Pengubah Sinyal Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Level Tegangan AC." },
  { "en": "Prinsip (Principle) Kerja Transformator?", "id": "Induksi Mutual Elektromagnetik." },
  { "en": "Apa Itu Motor (Motor) Listrik?", "id": "Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Listrik?", "id": "Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Jaringan Listrik Pembangkitan Hingga Distribusi." },
  { "en": "Apa Itu Transmisi (Transmission) HV?", "id": "Penyaluran Daya Jarak Jauh." },
  { "en": "Apa Itu Distribusi (Distribution) LV?", "id": "Penyaluran Daya Ke Konsumen." },
  { "en": "Apa Itu Proteksi (Protection) Sistem Tenaga?", "id": "Pengamanan Sistem Dari Gangguan." },
  { "en": "Apa Itu Pemutus (Breaker) Sirkuit?", "id": "Saklar Otomatis Arus Gangguan." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Keselamatan." },
  { "en": "Apa Itu Elektronika (Electronics) Daya?", "id": "Kontrol Konversi Daya Listrik." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Konverter AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Konverter DC Ke AC." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Pengubah Level Tegangan DC." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Loop (Loop) Tertutup Kontrol?", "id": "Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Kontroler (Controller) PID?", "id": "Kontroler Umpan Balik Industri." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Otomatisasi Industri." },
  { "en": "Apa Itu Komunikasi (Communication) Data?", "id": "Transfer Informasi Digital." },
  { "en": "Apa Itu Jaringan (Network) Komputer?", "id": "Interkoneksi Perangkat Komunikasi." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu Protokol (Protocol)?", "id": "Aturan Komunikasi Jaringan." },
  { "en": "Apa Itu Alamat (Address) IP?", "id": "Alamat Logis Perangkat Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Penghubung Antar Jaringan." },
  { "en": "Apa Itu Bahan (Material) Konduktor?", "id": "Bahan Penghantar Listrik." },
  { "en": "Apa Itu Bahan (Material) Isolator?", "id": "Bahan Penghambat Listrik." },
  { "en": "Apa Itu Bahan (Material) Semikonduktor?", "id": "Bahan Sifat Antara." },
  { "en": "Apa Itu Hukum (Law) Ohm?", "id": "V = I * R." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Energi Listrik." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Penyimpan Energi Magnetik." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total AC." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Non-Resistif AC." },
  { "en": "Apa Itu Resonansi (Resonance)?", "id": "Frekuensi Reaktansi Saling Hapus." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Seleksi Frekuensi." },
  { "en": "Apa Itu Sinyal (Signal) Analog?", "id": "Sinyal Nilai Kontinu." },
  { "en": "Apa Itu Sinyal (Signal) Digital?", "id": "Sinyal Nilai Diskrit." },
  { "en": "Apa Itu Noise (Derau)?", "id": "Gangguan Acak Sinyal." },
  { "en": "Apa Itu Transformasi (Transform) Fourier?", "id": "Analisis Frekuensi Sinyal." },
  { "en": "Apa Itu Sistem Tenaga (Power System) AC?", "id": "Sistem Gunakan Arus Bolak-Balik." },
  { "en": "Apa Itu Sistem Tenaga (Power System) DC?", "id": "Sistem Gunakan Arus Searah." },
  { "en": "Apa Keuntungan Transmisi (Transmission) AC?", "id": "Mudah Ubah Tegangan (Transformator)." },
  { "en": "Apa Keuntungan Transmisi (Transmission) DC (HVDC)?", "id": "Rugi Rendah Jarak Sangat Jauh." },
  { "en": "Apa Itu Jaringan Interkoneksi (Interconnected Grid)?", "id": "Jaringan Listrik Luas Terhubung Bersama." },
  { "en": "Manfaat Jaringan Interkoneksi (Interconnected Grid)?", "id": "Keandalan Tinggi, Ekonomi Skala." },
  { "en": "Apa Itu Pembangkit Listrik (Power Plant)?", "id": "Fasilitas Konversi Energi Primer Ke Listrik." },
  { "en": "Jenis Pembangkit Listrik (Power Plant) Termal?", "id": "PLTU (Batubara), PLTG (Gas), PLTN (Nuklir)." },
  { "en": "Jenis Pembangkit Listrik (Power Plant) Terbarukan?", "id": "PLTA (Air), PLTS (Surya), PLTB (Angin)." },
  { "en": "Apa Itu Efisiensi (Efficiency) Pembangkit?", "id": "Rasio Energi Listrik Output, Energi Primer Input." },
  { "en": "Apa Itu Turbin (Turbine)?", "id": "Mesin Putar Ubah Energi Fluida Ke Mekanik." },
  { "en": "Apa Itu Generator (Generator) Sinkron?", "id": "Mesin Ubah Energi Mekanik Ke Listrik AC." },
  { "en": "Apa Itu Sistem Eksitasi (Excitation System)?", "id": "Menyuplai Arus DC Ke Medan Generator." },
  { "en": "Apa Fungsi Sistem Eksitasi (Excitation System)?", "id": "Mengontrol Tegangan Output Generator." },
  { "en": "Apa Itu Governor (Governor) Turbin?", "id": "Mengatur Kecepatan Putaran Turbin." },
  { "en": "Apa Fungsi Governor (Governor) Turbin?", "id": "Mengontrol Frekuensi Sistem Listrik." },
  { "en": "Apa Itu Sinkronisasi (Synchronization) Generator?", "id": "Menghubungkan Generator Ke Jaringan Paralel." },
  { "en": "Syarat Sinkronisasi (Synchronization)?", "id": "Tegangan, Frekuensi, Fasa, Urutan Sama." },
  { "en": "Apa Itu Pembagian Beban (Load Sharing)?", "id": "Distribusi Beban Antar Generator Paralel." },
  { "en": "Bagaimana Atur Pembagian Beban Aktif (P)?", "id": "Melalui Governor (Input Penggerak)." },
  { "en": "Bagaimana Atur Pembagian Beban Reaktif (Q)?", "id": "Melalui Eksitasi (AVR)." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Konduktor Penyalur Listrik Jarak Jauh." },
  { "en": "Jenis Saluran Transmisi (Transmission Line)?", "id": "Saluran Udara Dan Kabel Bawah Tanah." },
  { "en": "Apa Itu Menara Transmisi (Transmission Tower)?", "id": "Struktur Penyangga Konduktor Saluran Udara." },
  { "en": "Apa Itu Isolator (Insulator) Transmisi?", "id": "Komponen Isolasi Konduktor Dari Menara." },
  { "en": "Apa Itu Efek Korona (Corona Effect)?", "id": "Ionisasai Udara Sekitar Konduktor HV." },
  { "en": "Apa Akibat Efek Korona (Corona Effect)?", "id": "Rugi Daya, Noise Radio, Ozon." },
  { "en": "Apa Itu Kabel Bawah Tanah (Underground Cable)?", "id": "Kabel Berisolasi Ditanam Di Tanah." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Transformasi Tegangan, Switching." },
  { "en": "Apa Itu Transformator Daya (Power Transformer)?", "id": "Transformator Kapasitas Besar Gardu Induk." },
  { "en": "Apa Itu Pemutus Tenaga (Circuit Breaker)?", "id": "Saklar Otomatis Pemutus Arus Gangguan." },
  { "en": "Media Pemadam Busur Api (Arc Quenching)?", "id": "Udara, Minyak, Vakum, Gas SF6." },
  { "en": "Apa Itu Pemisah (Disconnector)?", "id": "Saklar Pemisah Tanpa Beban." },
  { "en": "Apa Itu Trafo Instrumen (Instrument Transformer)?", "id": "CT (Current Transformer), PT (Potential Transformer)." },
  { "en": "Apa Itu Rele Proteksi (Protection Relay)?", "id": "Pendeteksi Gangguan, Pemicu Pemutus." },
  { "en": "Apa Itu Sistem Proteksi (Protection System) Utama?", "id": "Proteksi Utama (Main Protection)." },
  { "en": "Apa Itu Sistem Proteksi (Protection System) Cadangan?", "id": "Proteksi Cadangan (Backup Protection)." },
  { "en": "Apa Itu Zona Proteksi (Zone of Protection)?", "id": "Area Dilindungi Oleh Rele Tertentu." },
  { "en": "Apa Itu Proteksi Diferensial (Differential Protection)?", "id": "Proteksi Cepat Sensitif Gangguan Internal." },
  { "en": "Dimana Proteksi Diferensial (Differential Protection) Digunakan?", "id": "Trafo, Generator, Busbar, Saluran." },
  { "en": "Apa Itu Proteksi Jarak (Distance Protection)?", "id": "Proteksi Saluran Transmisi Ukur Impedansi." },
  { "en": "Apa Itu Proteksi Arus Lebih (Overcurrent Protection)?", "id": "Proteksi Berdasarkan Tingkat Arus." },
  { "en": "Apa Itu Proteksi Gangguan Tanah (Ground Fault)?", "id": "Proteksi Mendeteksi Arus Ke Tanah." },
  { "en": "Apa Itu Jaringan Distribusi (Distribution Network)?", "id": "Menyalurkan Listrik Ke Konsumen Akhir." },
  { "en": "Apa Itu Jaringan Primer (Primary Distribution)?", "id": "Jaringan Tegangan Menengah (MV)." },
  { "en": "Apa Itu Jaringan Sekunder (Secondary Distribution)?", "id": "Jaringan Tegangan Rendah (LV)." },
  { "en": "Apa Itu Trafo Distribusi (Distribution Transformer)?", "id": "Menurunkan Tegangan Primer Ke Sekunder." },
  { "en": "Dimana Trafo Distribusi (Distribution Transformer) Dipasang?", "id": "Tiang Listrik Atau Gardu Beton." },
  { "en": "Apa Itu Jaringan Radial (Radial Network)?", "id": "Satu Arah Sumber Ke Beban." },
  { "en": "Apa Itu Jaringan Loop (Loop Network)?", "id": "Beban Dapat Disuplai Dua Arah." },
  { "en": "Apa Keuntungan Jaringan Loop (Loop Network)?", "id": "Keandalan Lebih Tinggi." },
  { "en": "Apa Itu Recloser (Recloser) Distribusi?", "id": "Pemutus Otomatis Menutup Kembali (Gangguan Sementara)." },
  { "en": "Apa Itu Sectionalizer (Sectionalizer) Distribusi?", "id": "Pemisah Otomatis Isolasi Gangguan Permanen." },
  { "en": "Apa Itu Sekring Pelebur (Fuse Cutout) Distribusi?", "id": "Pelindung Arus Lebih Sisi Primer Trafo." },
  { "en": "Apa Itu Arester Surja (Surge Arrester) Distribusi?", "id": "Pelindung Peralatan Dari Surja Petir." },
  { "en": "Apa Itu Regulator Tegangan (Voltage Regulator) Distribusi?", "id": "Menjaga Tegangan Jaringan Tetap Stabil." },
  { "en": "Bagaimana Regulator Tegangan (Voltage Regulator) Bekerja?", "id": "Mengubah Tap Transformator Otomatis." },
  { "en": "Apa Itu Kapasitor Bank (Capacitor Bank) Distribusi?", "id": "Memperbaiki Faktor Daya, Menaikkan Tegangan." },
  { "en": "Apa Itu Otomatisasi Distribusi (Distribution Automation)?", "id": "Penggunaan Teknologi Kontrol, Monitor Jaringan." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Modernisasi Jaringan Listrik Komunikasi Digital." },
  { "en": "Apa Komponen Utama Smart Grid (Jaringan Cerdas)?", "id": "Meter Cerdas, Sensor Jaringan, Komunikasi, Analitik." },
  { "en": "Apa Itu Meter Cerdas (Smart Meter)?", "id": "Meter Elektronik Komunikasi Dua Arah." },
  { "en": "Apa Manfaat Meter Cerdas (Smart Meter)?", "id": "Pembacaan Otomatis, Manajemen Beban, Deteksi Gangguan." },
  { "en": "Apa Itu Advanced Metering Infrastructure (AMI)?", "id": "Infrastruktur Jaringan Komunikasi Meter Cerdas." },
  { "en": "Apa Itu Manajemen Sisi Permintaan (DSM)?", "id": "Mempengaruhi Pola Konsumsi Listrik Pelanggan." },
  { "en": "Apa Itu Respon Beban (Demand Response)?", "id": "Pelanggan Mengurangi Beban Saat Sinyal." },
  { "en": "Apa Itu Pembangkit Terdistribusi (Distributed Generation) (DG)?", "id": "Pembangkit Skala Kecil Terhubung Distribusi." },
  { "en": "Apa Itu Jaringan Mikro (Microgrid)?", "id": "Bagian Jaringan Bisa Operasi Mandiri." },
  { "en": "Apa Itu Vehicle-to-Grid (V2G)?", "id": "Mobil Listrik Menyuplai Daya Jaringan." },
  { "en": "Apa Itu Elektronika Analog (Analog Electronics)?", "id": "Pemrosesan Sinyal Kontinu." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Meningkatkan Amplitudo Sinyal." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Memilih Atau Menolak Frekuensi." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Menghasilkan Sinyal Periodik." },
  { "en": "Apa Itu Op-Amp (Operational Amplifier)?", "id": "Komponen Penguat Serbaguna." },
  { "en": "Apa Itu Elektronika Digital (Digital Electronics)?", "id": "Pemrosesan Sinyal Diskrit (0, 1)." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Dasar Operasi Logika Digital." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop)?", "id": "Elemen Penyimpan Memori Satu Bit." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Terintegrasi Chip Tunggal." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengubah Besaran Fisik Ke Sinyal Listrik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Mengubah Sinyal Listrik Ke Aksi Fisik." },
  { "en": "Apa Itu Sistem Kontrol (Control System)?", "id": "Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Kontrol Umpan Balik (Feedback Control)?", "id": "Menggunakan Output Mengoreksi Input." },
  { "en": "Apa Itu Kontrol PID (PID Control)?", "id": "Jenis Kontroler Umpan Balik Umum." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Kontroler Digital Industri." },
  { "en": "Apa Itu Komunikasi (Communication)?", "id": "Proses Pertukaran Informasi." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Informasi Ke Sinyal Pembawa." },
  { "en": "Apa Itu Komunikasi Nirkabel (Wireless Communication)?", "id": "Komunikasi Tanpa Media Kabel." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Perangkat Pancar/Terima Gelombang Radio." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komputer." },
  { "en": "Apa Itu Internet (Internet)?", "id": "Jaringan Komputer Skala Global." },
  { "en": "Apa Itu IP Address (Alamat IP)?", "id": "Alamat Unik Perangkat Jaringan." },
  { "en": "Apa Itu Router (Router)?", "id": "Penghubung Antar Jaringan." },
  { "en": "Apa Itu Keamanan Siber (Cybersecurity)?", "id": "Perlindungan Sistem Komputer Dari Ancaman." },
  { "en": "Apa Itu Enkripsi (Encryption)?", "id": "Pengkodean Data Agar Tidak Terbaca." },
  { "en": "Apa Itu Firewall (Firewall)?", "id": "Penyaring Lalu Lintas Jaringan." },
  { "en": "Apa Itu Pengukuran (Measurement)?", "id": "Penentuan Nilai Besaran Kuantitatif." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Penyesuaian Alat Ukur Sesuai Standar." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Listrik Serbaguna." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Visualisasi Sinyal Listrik." },
  { "en": "Apa Itu Bahan (Material) Teknik Elektro?", "id": "Konduktor, Semikonduktor, Isolator, Magnetik." },
  { "en": "Apa Itu Superkonduktor (Superconductor)?", "id": "Bahan Resistansi Nol Suhu Rendah." },
  { "en": "Apa Itu Resistansi (Resistance) Negatif?", "id": "Arus Turun Saat Tegangan Naik." },
  { "en": "Komponen Apa Punya Resistansi Negatif?", "id": "Dioda Tunnel, UJT (Unijunction Transistor)." },
  { "en": "Apa Itu Efek (Effect) Miller?", "id": "Peningkatan Kapasitansi Input Penguat." },
  { "en": "Apa Akibat Efek (Effect) Miller?", "id": "Mengurangi Bandwidth Frekuensi Tinggi." },
  { "en": "Apa Itu Rangkaian (Circuit) Cascode?", "id": "Kombinasi Common Emitter, Common Base." },
  { "en": "Keuntungan Rangkaian (Circuit) Cascode?", "id": "Bandwidth Tinggi, Gain Tinggi." },
  { "en": "Apa Itu Pasangan (Pair) Darlington?", "id": "Dua Transistor BJT (Bipolar Junction Transistor) Terhubung." },
  { "en": "Keuntungan Pasangan (Pair) Darlington?", "id": "Penguatan Arus Beta Sangat Tinggi." },
  { "en": "Apa Itu Cermin (Mirror) Arus?", "id": "Rangkaian Menyalin Arus Referensi." },
  { "en": "Fungsi Cermin (Mirror) Arus?", "id": "Sebagai Beban Aktif Atau Bias Arus." },
  { "en": "Apa Itu Beban (Load) Aktif?", "id": "Menggunakan Transistor Sebagai Resistor Beban." },
  { "en": "Apa Itu Penguat (Amplifier) Diferensial?", "id": "Menguatkan Selisih Dua Sinyal Input." },
  { "en": "Apa Itu Sinyal (Signal) Mode Bersama?", "id": "Sinyal Yang Sama Pada Kedua Input." },
  { "en": "Apa Itu Sinyal (Signal) Mode Diferensial?", "id": "Sinyal Yang Berbeda Antara Input." },
  { "en": "Apa Itu CMRR (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Sinyal Mode Bersama." },
  { "en": "Nilai CMRR (Common Mode Rejection Ratio) Ideal?", "id": "Nilai Tak Terhingga (Sangat Tinggi)." },
  { "en": "Apa Itu Penguat (Amplifier) Instrumentasi?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Itu Arus (Current) Bias Input Op-Amp?", "id": "Arus DC Kecil Masuk Pin Input." },
  { "en": "Apa Itu Tegangan (Voltage) Offset Input Op-Amp?", "id": "Tegangan Input Hasilkan Output Nol." },
  { "en": "Apa Itu Slew Rate (Laju Perubahan) Op-Amp?", "id": "Laju Perubahan Output Maksimum." },
  { "en": "Apa Satuan Slew Rate (Laju Perubahan)?", "id": "Volt Per Mikrodetik (V/µs)." },
  { "en": "Apa Itu Gain Bandwidth Product (GBP)?", "id": "Hasil Kali Gain, Bandwidth Op-Amp." },
  { "en": "Apa Itu Power Supply Rejection Ratio (PSRR)?", "id": "Kemampuan Tolak Noise Catu Daya." },
  { "en": "Apa Kepanjangan PSRR (Power Supply Rejection Ratio)?", "id": "Rasio Penolakan Catu Daya." },
  { "en": "Apa Itu Osilator (Oscillator) Geser Fasa?", "id": "Osilator (Oscillator) RC (Resistor Kapasitor) Geser Fasa 180°." },
  { "en": "Apa Itu Osilator (Oscillator) Jembatan Wien?", "id": "Osilator (Oscillator) RC (Resistor Kapasitor) Hasilkan Sinus Murni." },
  { "en": "Apa Itu Osilator (Oscillator) Colpitts?", "id": "Osilator (Oscillator) LC (Induktor Kapasitor) Pembagi Kapasitif." },
  { "en": "Apa Itu Osilator (Oscillator) Hartley?", "id": "Osilator (Oscillator) LC (Induktor Kapasitor) Pembagi Induktif." },
  { "en": "Apa Itu Osilator (Oscillator) Kristal?", "id": "Osilator (Oscillator) Sangat Stabil Frekuensinya." },
  { "en": "Apa Bahan (Material) Kristal Osilator?", "id": "Kuarsa (Quartz) Piezoelektrik." },
  { "en": "Apa Itu Mode (Mode) Astabil Timer 555?", "id": "Sebagai Osilator Gelombang Kotak." },
  { "en": "Apa Itu Mode (Mode) Monostabil Timer 555?", "id": "Sebagai Timer Satu Pulsa." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Umpan Balik Pengunci Fasa." },
  { "en": "Komponen (Component) Utama PLL (Phase-Locked Loop)?", "id": "Detektor Fasa, Filter, VCO." },
  { "en": "Apa Itu Voltage-Controlled Oscillator (VCO)?", "id": "Osilator Frekuensi Diatur Tegangan." },
  { "en": "Apa Itu Filter (Filter) Aktif?", "id": "Filter Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Itu Filter (Filter) Pasif?", "id": "Filter Komponen Pasif (R, L, C)." },
  { "en": "Apa Respons (Response) Filter Butterworth?", "id": "Passband Datar Maksimal." },
  { "en": "Apa Respons (Response) Filter Chebyshev?", "id": "Roll-Off Curam, Riak Passband." },
  { "en": "Apa Respons (Response) Filter Bessel?", "id": "Respon Fasa Linear Terbaik." },
  { "en": "Apa Itu Orde (Order) Filter?", "id": "Menentukan Kecuraman Roll-Off." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Mengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Mengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Resolusi (Resolution) ADC?", "id": "Jumlah Bit Output Digital." },
  { "en": "Apa Itu Laju (Rate) Sampling ADC?", "id": "Jumlah Sampel Diambil Per Detik." },
  { "en": "Apa Teorema (Theorem) Sampling Nyquist?", "id": "Fs Minimal Dua Kali Fmax." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Akibat Laju Sampling Kurang." },
  { "en": "Apa Itu Filter (Filter) Anti-Aliasing?", "id": "Filter Lolos Rendah Sebelum ADC." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Pembulatan Nilai Sampel Diskrit." },
  { "en": "Apa Itu Kesalahan (Error) Kuantisasi?", "id": "Error Akibat Pembulatan Kuantisasi." },
  { "en": "Apa Tipe (Type) ADC Paling Cepat?", "id": "ADC Tipe Flash (Flash ADC)." },
  { "en": "Apa Tipe (Type) ADC Paling Umum?", "id": "ADC Tipe SAR (Successive Approximation)." },
  { "en": "Apa Tipe (Type) ADC Resolusi Tinggi?", "id": "ADC Tipe Sigma-Delta." },
  { "en": "Apa Tipe (Type) DAC Umum?", "id": "R-2R Ladder DAC." },
  { "en": "Apa Itu Multiplexer (Multiplexer) (MUX)?", "id": "Memilih Satu Input Dari Banyak." },
  { "en": "Apa Itu Demultiplexer (Demultiplexer) (DEMUX)?", "id": "Mengarahkan Input Ke Banyak Output." },
  { "en": "Apa Itu Encoder (Encoder)?", "id": "Mengubah Input Aktif Ke Biner." },
  { "en": "Apa Itu Decoder (Decoder)?", "id": "Mengubah Biner Ke Output Aktif." },
  { "en": "Apa Itu Half Adder (Penjumlah Setengah)?", "id": "Menjumlahkan Dua Bit (Sum, Carry)." },
  { "en": "Apa Itu Full Adder (Penjumlah Penuh)?", "id": "Menjumlahkan Tiga Bit (Dengan Carry In)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) SR?", "id": "Set-Reset Flip-Flop." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) D?", "id": "Data Flip-Flop (Menyimpan Input D)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) T?", "id": "Toggle Flip-Flop (Membalik Output)." },
  { "en": "Apa Itu Flip-Flop (Flip-Flop) JK?", "id": "Flip-Flop Universal (Set, Reset, Toggle)." },
  { "en": "Apa Itu Register (Register)?", "id": "Kumpulan Flip-Flop Simpan Data." },
  { "en": "Apa Itu Register (Register) Geser?", "id": "Register Menggeser Data Bit." },
  { "en": "Apa Itu Counter (Pencacah) Asinkron?", "id": "Counter Riak (Ripple Counter)." },
  { "en": "Apa Itu Counter (Pencacah) Sinkron?", "id": "Counter Di-Clock Bersamaan." },
  { "en": "Apa Itu Modulus (Modulus) Counter?", "id": "Jumlah Keadaan (State) Counter." },
  { "en": "Apa Itu Memori (Memory) RAM?", "id": "Random Access Memory (Volatil)." },
  { "en": "Apa Itu Memori (Memory) ROM?", "id": "Read Only Memory (Non-Volatil)." },
  { "en": "Apa Itu Memori (Memory) Flash?", "id": "Memori Non-Volatil Tipe EEPROM." },
  { "en": "Apa Itu Mikroprosesor (Microprocessor)?", "id": "CPU (Central Processing Unit) Dalam Satu Chip." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
  { "en": "Apa Itu Arsitektur (Architecture) Harvard?", "id": "Memori Program, Data Terpisah." },
  { "en": "Apa Itu Arsitektur (Architecture) Von Neumann?", "id": "Memori Program, Data Bersatu." },
  { "en": "Apa Itu GPIO (General Purpose Input Output)?", "id": "Pin Digital Serbaguna Mikrokontroler." },
  { "en": "Apa Itu Komunikasi (Communication) UART?", "id": "Komunikasi Serial Asinkron." },
  { "en": "Apa Itu Komunikasi (Communication) SPI?", "id": "Komunikasi Serial Sinkron." },
  { "en": "Apa Itu Komunikasi (Communication) I2C?", "id": "Bus Serial Dua Kawat." },
  { "en": "Apa Itu Motor (Motor) Stepper?", "id": "Motor Gerak Per Langkah Sudut." },
  { "en": "Apa Itu Motor (Motor) Servo?", "id": "Motor Kontrol Posisi Loop Tertutup." },
  { "en": "Apa Itu H-Bridge (Jembatan H)?", "id": "Rangkaian Kontrol Arah Motor DC." },
  { "en": "Apa Itu PWM (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa." },
  { "en": "Aplikasi PWM (Pulse Width Modulation)?", "id": "Kontrol Kecerahan LED, Kecepatan Motor." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Pengubah Level Tegangan AC." },
  { "en": "Apa Itu Penyearah (Rectifier)?", "id": "Rangkaian Pengubah AC Ke DC." },
  { "en": "Apa Itu Inverter (Inverter)?", "id": "Rangkaian Pengubah DC Ke AC." },
  { "en": "Apa Itu Konverter (Converter) DC-DC?", "id": "Pengubah Level Tegangan DC." },
  { "en": "Apa Itu Sistem (System) Tenaga Listrik?", "id": "Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Koneksi Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Pelindung Arus Lebih (Putus)." },
  { "en": "Apa Itu Pemutus (Breaker) Sirkuit?", "id": "Pelindung Arus Lebih (Bisa Reset)." },
  { "en": "Apa Itu Kualitas (Quality) Daya?", "id": "Karakteristik Ideal Tegangan, Arus." },
  { "en": "Apa Itu Harmonisa (Harmonics)?", "id": "Frekuensi Kelipatan Fundamental." },
  { "en": "Apa Itu Faktor (Factor) Daya?", "id": "Rasio Daya Nyata Terhadap Semu." },
  { "en": "Apa Itu Energi (Energy) Terbarukan?", "id": "Sumber Energi Alam Berkelanjutan." },
  { "en": "Apa Itu Sel (Cell) Surya?", "id": "Pengubah Cahaya Matahari Listrik." },
  { "en": "Apa Itu Turbin (Turbine) Angin?", "id": "Pengubah Energi Angin Listrik." },
  { "en": "Apa Itu Smart Grid (Jaringan Cerdas)?", "id": "Jaringan Listrik Modern Digital." },
  { "en": "Apa Itu Gelombang Stasioner (Standing Wave)?", "id": "Hasil Interferensi Dua Gelombang Berlawanan." },
  { "en": "Apa Itu Simpul (Node) Gelombang Stasioner?", "id": "Titik Amplitudo Nol (Diam)." },
  { "en": "Apa Itu Perut (Antinode) Gelombang Stasioner?", "id": "Titik Amplitudo Maksimum Getaran." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Rasio Amplitudo Maksimum, Minimum Gelombang." },
  { "en": "Apa Nilai SWR (Standing Wave Ratio) Ideal?", "id": "Satu Banding Satu (1:1)." },
  { "en": "Apa Penyebab SWR (Standing Wave Ratio) Buruk?", "id": "Ketidakcocokan Impedansi (Mismatch)." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyamakan Impedansi Sumber, Saluran, Beban." },
  { "en": "Mengapa Pencocokan Impedansi (Impedance Matching) Penting?", "id": "Maksimalkan Transfer Daya, Hindari Refleksi." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Grafik Bantu Hitungan Impedansi RF." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient) (Γ)?", "id": "Rasio Gelombang Pantul Terhadap Datang." },
  { "en": "Apa Nilai Koefisien Refleksi (Γ) Ideal?", "id": "Nol (Tidak Ada Pantulan)." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Media Pemandu Gelombang Elektromagnetik." },
  { "en": "Contoh Saluran Transmisi (Transmission Line)?", "id": "Kabel Koaksial, Mikrostrip, Waveguide." },
  { "en": "Apa Itu Impedansi Karakteristik (Z₀)?", "id": "Impedansi Khas Saluran Transmisi." },
  { "en": "Berapa Impedansi (Impedance) Umum Kabel Koaksial?", "id": "Lima Puluh Ohm (50Ω) Atau 75Ω." },
  { "en": "Apa Itu Kabel Mikrostrip (Microstrip Line)?", "id": "Jalur Konduktif Di Atas Substrat PCB." },
  { "en": "Apa Itu Pemandu Gelombang (Waveguide)?", "id": "Pipa Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu Frekuensi Cutoff (Cutoff Frequency) Waveguide?", "id": "Frekuensi Minimum Dapat Dirambatkan." },
  { "en": "Apa Itu Mode Propagasi (Propagation Mode) Waveguide?", "id": "Pola Medan EM Dalam Pemandu." },
  { "en": "Apa Itu Mode Dominan (Dominant Mode) Waveguide?", "id": "Mode Frekuensi Cutoff Terendah." },
  { "en": "Mode Dominan (Dominant Mode) Waveguide Persegi Panjang?", "id": "Mode TE₁₀ (Transverse Electric 10)." },
  { "en": "Apa Itu Resonator Rongga (Cavity Resonator)?", "id": "Rongga Logam Penyimpan Energi EM." },
  { "en": "Aplikasi Resonator Rongga (Cavity Resonator)?", "id": "Filter Frekuensi Tinggi, Osilator." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Konverter Gelombang Terpandu Ke Ruang Bebas." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern)?", "id": "Plot Keterarahan Pancaran Energi Antena." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Keterarahan Dibanding Isotropik." },
  { "en": "Apa Satuan Penguatan (Gain) Antena?", "id": "Desibel Isotropik (dBi)." },
  { "en": "Apa Itu Antena Isotropik (Isotropic Antenna)?", "id": "Antena Ideal Memancar Sama Rata." },
  { "en": "Apa Itu Antena Dipol (Dipole Antenna) Setengah Gelombang?", "id": "Antena Dasar Panjang Setengah Lambda (λ/2)." },
  { "en": "Apa Impedansi (Impedance) Input Dipol Setengah Gelombang?", "id": "Sekitar Tujuh Puluh Tiga Ohm (73Ω)." },
  { "en": "Apa Itu Antena Monopol (Monopole Antenna) Seperempat Gelombang?", "id": "Antena Panjang Seperempat Lambda (λ/4) Diatas Ground." },
  { "en": "Apa Impedansi (Impedance) Input Monopol Seperempat Gelombang?", "id": "Sekitar Tiga Puluh Enam Koma Lima Ohm (36.5Ω)." },
  { "en": "Apa Itu Antena Yagi-Uda (Yagi-Uda)?", "id": "Antena Direktif (Elemen Parasitik)." },
  { "en": "Elemen Apa Saja Pada Antena Yagi?", "id": "Driven, Reflektor, Dan Direktor." },
  { "en": "Apa Itu Antena Parabola (Parabolic Antenna)?", "id": "Antena Reflektor Penguatan Sangat Tinggi." },
  { "en": "Apa Itu Antena Patch (Patch Antenna) Mikrostrip?", "id": "Antena Dicetak Di Atas PCB." },
  { "en": "Apa Itu Antena Array (Antenna Array)?", "id": "Sekumpulan Elemen Antena Bekerja Sama." },
  { "en": "Apa Itu Beamforming (Pembentukan Berkas)?", "id": "Mengatur Fasa Array Arahkan Berkas." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Orientasi Medan Listrik Dipancarkan." },
  { "en": "Jenis Polarisasi (Polarization) Linear?", "id": "Vertikal Atau Horizontal." },
  { "en": "Jenis Polarisasi (Polarization) Sirkular?", "id": "Putar Kanan (RHCP) Putar Kiri (LHCP)." },
  { "en": "Apa Itu Redaman Ruang Bebas (Free Space Loss)?", "id": "Pelemahan Sinyal Akibat Jarak Propagasi." },
  { "en": "Apa Itu Link Budget (Anggaran Tautan) Komunikasi?", "id": "Perhitungan Total Gain, Loss Sistem." },
  { "en": "Apa Itu Radar (Radio Detection and Ranging)?", "id": "Sistem Deteksi Objek Gelombang Radio." },
  { "en": "Apa Prinsip Dasar Radar (Radar)?", "id": "Memancarkan Pulsa, Menerima Gema Pantulan." },
  { "en": "Apa Itu Radar Doppler (Doppler Radar)?", "id": "Mengukur Kecepatan Objek (Pergeseran Frekuensi)." },
  { "en": "Apa Itu Penampang Lintang Radar (RCS)?", "id": "Ukuran Kemampuan Objek Pantulkan Sinyal Radar." },
  { "en": "Apa Kepanjangan RCS (Radar Cross Section)?", "id": "Penampang Lintang Radar." },
  { "en": "Apa Itu Komunikasi (Communication) Optik?", "id": "Transmisi Informasi Menggunakan Cahaya." },
  { "en": "Apa Media Utama Komunikasi (Communication) Optik?", "id": "Serat Optik (Fiber Optic)." },
  { "en": "Apa Prinsip Dasar Serat Optik?", "id": "Pemantulan Internal Total Cahaya." },
  { "en": "Apa Itu Inti (Core) Serat Optik?", "id": "Bagian Tengah Serat (Indeks Bias Tinggi)." },
  { "en": "Apa Itu Selubung (Cladding) Serat Optik?", "id": "Lapisan Luar Inti (Indeks Bias Rendah)." },
  { "en": "Apa Itu Serat Mode Tunggal (Single Mode)?", "id": "Inti Sangat Kecil (Satu Jalur Cahaya)." },
  { "en": "Apa Itu Serat Mode Jamak (Multi Mode)?", "id": "Inti Lebih Besar (Banyak Jalur Cahaya)." },
  { "en": "Mana Jangkauan Lebih Jauh, Single Atau Multi Mode?", "id": "Serat Mode Tunggal (Single Mode)." },
  { "en": "Apa Itu Dispersi (Dispersion) Serat Optik?", "id": "Pelebaran Pulsa Cahaya (Batasi Bandwidth)." },
  { "en": "Apa Itu Dispersi Kromatik (Chromatic Dispersion)?", "id": "Beda Kecepatan Rambat Panjang Gelombang." },
  { "en": "Apa Itu Dispersi Modal (Modal Dispersion)?", "id": "Beda Waktu Tempuh Jalur Cahaya (MMF)." },
  { "en": "Apa Itu Atenuasi (Attenuation) Serat Optik?", "id": "Pelemahan Intensitas Cahaya Sepanjang Serat." },
  { "en": "Apa Satuan Atenuasi (Attenuation) Serat Optik?", "id": "Desibel Per Kilometer (dB/km)." },
  { "en": "Apa Sumber Cahaya (Light Source) Komunikasi Optik?", "id": "LED (Light Emitting Diode) Atau Dioda Laser (LD)." },
  { "en": "Apa Detektor (Detector) Komunikasi Optik?", "id": "Fotodioda (PIN Atau APD)." },
  { "en": "Apa Itu WDM (Wavelength Division Multiplexing)?", "id": "Multiplexing Banyak Panjang Gelombang (Warna)." },
  { "en": "Apa Itu Penguat Optik (Optical Amplifier)?", "id": "Menguatkan Sinyal Cahaya Langsung." },
  { "en": "Contoh Penguat Optik (Optical Amplifier)?", "id": "EDFA (Erbium Doped Fiber Amplifier)." },
  { "en": "Apa Itu Penyambungan (Splicing) Fusi?", "id": "Menyambung Serat Optik Panas Lelehan." },
  { "en": "Apa Itu Konektor (Connector) Serat Optik?", "id": "Koneksi Serat Optik Dapat Dilepas Pasang." },
  { "en": "Contoh Konektor (Connector) Serat Optik?", "id": "SC (Subscriber Connector), LC (Lucent Connector)." },
  { "en": "Apa Itu OTDR (Optical Time Domain Reflectometer)?", "id": "Alat Uji Serat Optik (Panjang, Redaman, Lokasi Putus)." },
  { "en": "Apa Itu Sistem (System) Waktu Kontinu?", "id": "Sistem Sinyal Terdefinisi Setiap Saat." },
  { "en": "Apa Itu Sistem (System) Waktu Diskrit?", "id": "Sistem Sinyal Terdefinisi Waktu Diskrit." },
  { "en": "Apa Itu Sistem (System) Linear?", "id": "Sistem Memenuhi Prinsip Superposisi." },
  { "en": "Apa Itu Sistem (System) Invarian Waktu?", "id": "Respon Sistem Tidak Berubah Waktu." },
  { "en": "Apa Itu Sistem (System) LTI?", "id": "Linear Time-Invariant." },
  { "en": "Apa Itu Respon Impuls (Impulse Response)?", "id": "Output Sistem LTI Terhadap Input Impuls." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Output Sistem LTI." },
  { "en": "Apa Itu Transformasi (Transform) Laplace?", "id": "Mengubah Persamaan Diferensial Ke Aljabar." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(s)?", "id": "Rasio Output(s) Terhadap Input(s)." },
  { "en": "Apa Itu Pole (Kutub) Sistem?", "id": "Akar Penyebut Fungsi Transfer." },
  { "en": "Apa Itu Zero (Nol) Sistem?", "id": "Akar Pembilang Fungsi Transfer." },
  { "en": "Apa Syarat Kestabilan (Stability) Sistem Kontinu?", "id": "Semua Pole Di Setengah Kiri Bidang-s." },
  { "en": "Apa Itu Respon Frekuensi (Frequency Response)?", "id": "Respons Sistem Terhadap Input Sinusoidal." },
  { "en": "Apa Itu Plot Bode (Bode Plot)?", "id": "Plot Logaritmik Magnitudo, Fasa Vs Frekuensi." },
  { "en": "Apa Itu Transformasi (Transform) Z?", "id": "Alat Analisis Sistem Waktu Diskrit." },
  { "en": "Apa Itu Fungsi Transfer (Transfer Function) H(z)?", "id": "Rasio Output(z) Terhadap Input(z)." },
  { "en": "Apa Itu Lingkaran Satuan (Unit Circle)?", "id": "Lingkaran Jari-Jari Satu Bidang-Z." },
  { "en": "Apa Syarat Kestabilan (Stability) Sistem Diskrit?", "id": "Semua Pole Di Dalam Lingkaran Satuan." },
  { "en": "Apa Itu Transformasi (Transform) Fourier Diskrit (DFT)?", "id": "Analisis Frekuensi Sinyal Waktu Diskrit." },
  { "en": "Apa Itu Transformasi (Transform) Fourier Cepat (FFT)?", "id": "Algoritma Efisien Menghitung DFT." },
  { "en": "Apa Itu Filter (Filter) Digital?", "id": "Algoritma Pemrosesan Sinyal Digital." },
  { "en": "Apa Itu Filter (Filter) FIR?", "id": "Finite Impulse Response (Tanpa Feedback)." },
  { "en": "Apa Itu Filter (Filter) IIR?", "id": "Infinite Impulse Response (Dengan Feedback)." },
  { "en": "Apa Keuntungan Filter (Filter) FIR?", "id": "Selalu Stabil, Fasa Linear Mudah." },
  { "en": "Apa Keuntungan Filter (Filter) IIR?", "id": "Komputasi Lebih Efisien (Orde Rendah)." },
  { "en": "Apa Itu Efek (Effect) Windowing?", "id": "Mengurangi Kebocoran Spektral Analisis FFT." },
  { "en": "Apa Itu Sistem (System) Kontrol?", "id": "Pengaturan Perilaku Sistem Dinamis." },
  { "en": "Apa Itu Loop (Loop) Terbuka?", "id": "Kontrol Tanpa Umpan Balik Output." },
  { "en": "Apa Itu Loop (Loop) Tertutup?", "id": "Kontrol Dengan Umpan Balik Output." },
  { "en": "Apa Itu Setpoint (Setpoint) Kontrol?", "id": "Nilai Referensi Yang Diinginkan." },
  { "en": "Apa Itu Error (Kesalahan) Kontrol?", "id": "Selisih Antara Setpoint, Nilai Aktual." },
  { "en": "Apa Itu Kontroler (Controller) PID?", "id": "Proporsional, Integral, Derivatif." },
  { "en": "Fungsi (Function) Aksi Proporsional (P)?", "id": "Respon Proporsional Terhadap Error." },
  { "en": "Fungsi (Function) Aksi Integral (I)?", "id": "Menghilangkan Error Keadaan Tunak." },
  { "en": "Fungsi (Function) Aksi Derivatif (D)?", "id": "Respon Antisipasi Laju Perubahan Error." },
  { "en": "Apa Itu Pemrosesan Sinyal (Signal Processing)?", "id": "Analisis, Modifikasi, Dan Sintesis Sinyal." },
  { "en": "Apa Itu Sinyal (Signal) Waktu Kontinu?", "id": "Sinyal Terdefinisi Setiap Saat Waktu." },
  { "en": "Apa Itu Sinyal (Signal) Waktu Diskrit?", "id": "Sinyal Terdefinisi Hanya Waktu Tertentu." },
  { "en": "Apa Itu Pencuplikan (Sampling)?", "id": "Mengubah Sinyal Kontinu Ke Diskrit." },
  { "en": "Apa Teorema (Theorem) Sampling Nyquist?", "id": "Frekuensi Sampling Minimal 2x Frekuensi Maksimum." },
  { "en": "Apa Itu Aliasing (Aliasing)?", "id": "Distorsi Akibat Frekuensi Sampling Terlalu Rendah." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Pembulatan Nilai Sampel Ke Level Diskrit." },
  { "en": "Apa Itu Filter Digital (Digital Filter)?", "id": "Algoritma Memproses Sinyal Digital." },
  { "en": "Apa Itu Transformasi Fourier (Fourier Transform)?", "id": "Mengubah Sinyal Domain Waktu Ke Frekuensi." },
  { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Algoritma Cepat Menghitung Transformasi Fourier Diskrit." },
  { "en": "Apa Itu Konvolusi (Convolution)?", "id": "Operasi Matematis Untuk Output Sistem LTI." },
  { "en": "Apa Itu Korelasi (Correlation)?", "id": "Ukuran Kemiripan Antar Sinyal." },
  { "en": "Apa Itu Sistem LTI (Linear Time-Invariant)?", "id": "Sistem Linear Dan Tidak Berubah Waktu." },
  { "en": "Apa Itu Respon Impuls (Impulse Response)?", "id": "Output Sistem LTI Terhadap Input Impuls." },
  { "en": "Apa Itu Persamaan Maxwell (Maxwell's Equations)?", "id": "Hukum Dasar Medan Elektromagnetik." },
  { "en": "Apa Itu Gelombang Elektromagnetik (Electromagnetic Wave)?", "id": "Perambatan Gangguan Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Kecepatan Cahaya (Speed of Light) (c)?", "id": "Kecepatan Gelombang EM Di Vakum." },
  { "en": "Apa Itu Spektrum Elektromagnetik (Electromagnetic Spectrum)?", "id": "Rentang Frekuensi Gelombang EM." },
  { "en": "Apa Itu Propagasi (Propagation) Gelombang?", "id": "Perambatan Gelombang Lewat Medium." },
  { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Pelemahan Kekuatan Sinyal." },
  { "en": "Apa Itu Refleksi (Reflection) Gelombang?", "id": "Pemantulan Gelombang Batas Medium." },
  { "en": "Apa Itu Refraksi (Refraction) Gelombang?", "id": "Pembelokan Gelombang Antar Medium." },
  { "en": "Apa Itu Difraksi (Diffraction) Gelombang?", "id": "Penyebaran Gelombang Melewati Celah/Ujung." },
  { "en": "Apa Itu Antena (Antenna)?", "id": "Konverter Antara Gelombang EM Dan Arus Listrik." },
  { "en": "Apa Itu Penguatan (Gain) Antena?", "id": "Ukuran Kemampuan Antena Memfokuskan Energi." },
  { "en": "Apa Itu Pola Radiasi (Radiation Pattern)?", "id": "Pola Pancaran Energi Antena." },
  { "en": "Apa Itu Impedansi Input (Input Impedance) Antena?", "id": "Impedansi Pada Terminal Antena." },
  { "en": "Apa Itu Polarisasi (Polarization) Antena?", "id": "Orientasi Medan Listrik Gelombang." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Media Pemandu Gelombang EM." },
  { "en": "Apa Itu Impedansi Karakteristik (Characteristic Impedance)?", "id": "Impedansi Khas Saluran Transmisi." },
  { "en": "Apa Itu Koefisien Refleksi (Reflection Coefficient)?", "id": "Rasio Amplitudo Gelombang Pantul Dan Datang." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Ukuran Rasio Gelombang Berdiri." },
  { "en": "Apa Itu Pencocokan Impedansi (Impedance Matching)?", "id": "Menyesuaikan Impedansi Untuk Transfer Daya Maksimal." },
  { "en": "Apa Itu Smith Chart (Bagan Smith)?", "id": "Alat Grafis Analisis Saluran Transmisi." },
  { "en": "Apa Itu Parameter S (Scattering Parameters)?", "id": "Parameter Karakterisasi Jaringan Frekuensi Tinggi." },
  { "en": "Apa Itu Microwave (Gelombang Mikro)?", "id": "Gelombang EM Frekuensi 300 MHz - 300 GHz." },
  { "en": "Apa Itu Waveguide (Pemandu Gelombang)?", "id": "Struktur Logam Pemandu Gelombang Mikro." },
  { "en": "Apa Itu Komunikasi Serat Optik (Fiber Optic)?", "id": "Transmisi Informasi Menggunakan Cahaya Dalam Serat." },
  { "en": "Apa Prinsip Dasar Serat Optik?", "id": "Pemantulan Internal Total (Total Internal Reflection)." },
  { "en": "Apa Itu Komunikasi Data (Data Communication)?", "id": "Pertukaran Data Digital Antar Perangkat." },
  { "en": "Apa Itu Jaringan Komputer (Computer Network)?", "id": "Interkoneksi Perangkat Komunikasi Data." },
  { "en": "Apa Itu Protokol (Protocol) Jaringan?", "id": "Aturan Komunikasi Jaringan." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Konseptual Lapisan Jaringan (7 Lapisan)." },
  { "en": "Apa Itu Model TCP/IP (TCP/IP Model)?", "id": "Model Praktis Lapisan Jaringan Internet (4/5 Lapisan)." },
  { "en": "Apa Itu Alamat IP (IP Address)?", "id": "Pengenal Numerik Perangkat Di Jaringan IP." },
  { "en": "Apa Itu Alamat MAC (MAC Address)?", "id": "Pengenal Fisik Unik Antarmuka Jaringan." },
  { "en": "Apa Itu Ethernet (Ethernet)?", "id": "Standar Jaringan Area Lokal (LAN) Berkabel." },
  { "en": "Apa Itu Wi-Fi (Wireless Fidelity)?", "id": "Standar Jaringan Area Lokal (LAN) Nirkabel." },
  { "en": "Apa Itu Router (Router)?", "id": "Perangkat Menghubungkan Jaringan Berbeda." },
  { "en": "Apa Itu Switch (Saklar)?", "id": "Perangkat Menghubungkan Perangkat Dalam Satu LAN." },
  { "en": "Apa Itu Listrik (Electricity)?", "id": "Fenomena Terkait Kehadiran Dan Aliran Muatan." },
  { "en": "Apa Itu Muatan (Charge) Listrik?", "id": "Sifat Dasar Materi (Positif/Negatif)." },
  { "en": "Apa Satuan Muatan (Charge)?", "id": "Coulomb (C)." },
  { "en": "Apa Itu Elektron (Electron)?", "id": "Partikel Subatomik Bermuatan Negatif." },
  { "en": "Apa Itu Proton (Proton)?", "id": "Partikel Subatomik Bermuatan Positif." },
  { "en": "Apa Itu Arus (Current) Listrik?", "id": "Laju Aliran Muatan Listrik." },
  { "en": "Apa Satuan Arus (Current)?", "id": "Ampere (A)." },
  { "en": "Apa Itu Tegangan (Voltage)?", "id": "Perbedaan Potensial Listrik Antara Dua Titik." },
  { "en": "Apa Satuan Tegangan (Voltage)?", "id": "Volt (V)." },
  { "en": "Apa Itu Resistansi (Resistance)?", "id": "Hambatan Terhadap Aliran Arus." },
  { "en": "Apa Satuan Resistansi (Resistance)?", "id": "Ohm (Ω)." },
  { "en": "Apa Itu Hukum Ohm (Ohm's Law)?", "id": "Hubungan Antara Tegangan, Arus, Resistansi (V=IR)." },
  { "en": "Apa Itu Daya (Power) Listrik?", "id": "Laju Transfer Energi Listrik." },
  { "en": "Apa Satuan Daya (Power)?", "id": "Watt (W)." },
  { "en": "Apa Itu Rangkaian Seri (Series Circuit)?", "id": "Komponen Terhubung Dalam Satu Jalur." },
  { "en": "Apa Itu Rangkaian Paralel (Parallel Circuit)?", "id": "Komponen Terhubung Pada Titik Cabang Yang Sama." },
  { "en": "Apa Itu Hukum Arus Kirchhoff (KCL)?", "id": "Jumlah Arus Masuk Simpul Sama Dengan Keluar." },
  { "en": "Apa Itu Hukum Tegangan Kirchhoff (KVL)?", "id": "Jumlah Tegangan Dalam Loop Tertutup Adalah Nol." },
  { "en": "Apa Itu Arus Searah (Direct Current) (DC)?", "id": "Arus Mengalir Satu Arah." },
  { "en": "Apa Itu Arus Bolak-Balik (Alternating Current) (AC)?", "id": "Arus Berubah Arah Secara Periodik." },
  { "en": "Apa Itu Frekuensi (Frequency) AC?", "id": "Jumlah Siklus Per Detik (Hz)." },
  { "en": "Apa Itu Kapasitor (Capacitor)?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Itu Induktor (Induktor)?", "id": "Komponen Penyimpan Energi Magnetik." },
  { "en": "Apa Itu Reaktansi (Reactance)?", "id": "Hambatan Komponen Kapasitif Atau Induktif Terhadap AC." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total Rangkaian Terhadap AC (Termasuk Resistansi)." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Dengan Konduktivitas Antara Konduktor Dan Isolator." },
  { "en": "Apa Itu Dioda (Diode)?", "id": "Komponen Semikonduktor Mengalirkan Arus Satu Arah." },
  { "en": "Apa Itu Transistor (Transistor)?", "id": "Komponen Semikonduktor Penguat Atau Saklar." },
  { "en": "Apa Itu Sirkuit Terpadu (Integrated Circuit) (IC)?", "id": "Rangkaian Elektronik Kompleks Dalam Satu Chip." },
  { "en": "Apa Itu Elektronika Analog (Analog Electronics)?", "id": "Elektronika Berurusan Dengan Sinyal Kontinu." },
  { "en": "Apa Itu Elektronika Digital (Digital Electronics)?", "id": "Elektronika Berurusan Dengan Sinyal Diskrit." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Menaikkan Kekuatan Sinyal." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Menghasilkan Sinyal Periodik." },
  { "en": "Apa Itu Filter (Filter)?", "id": "Rangkaian Memilih Frekuensi Tertentu." },
  { "en": "Apa Itu Catu Daya (Power Supply)?", "id": "Mengubah Sumber Listrik Menjadi Bentuk Yang Sesuai." },
  { "en": "Apa Itu Transformator (Transformer)?", "id": "Mengubah Level Tegangan AC." },
  { "en": "Apa Itu Motor Listrik (Electric Motor)?", "id": "Mengubah Energi Listrik Menjadi Energi Mekanik." },
  { "en": "Apa Itu Generator Listrik (Electric Generator)?", "id": "Mengubah Energi Mekanik Menjadi Energi Listrik." },
  { "en": "Apa Itu Pengukuran (Measurement) Listrik?", "id": "Menentukan Nilai Besaran Listrik." },
  { "en": "Apa Itu Voltmeter (Voltmeter)?", "id": "Alat Ukur Tegangan." },
  { "en": "Apa Itu Amperemeter (Ammeter)?", "id": "Alat Ukur Arus." },
  { "en": "Apa Itu Ohmmeter (Ohmmeter)?", "id": "Alat Ukur Resistansi." },
  { "en": "Apa Itu Multimeter (Multimeter)?", "id": "Alat Ukur Kombinasi (V, A, Ω)." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Menampilkan Bentuk Gelombang Tegangan Terhadap Waktu." },
  { "en": "Apa Itu Sistem Tenaga Listrik (Power System)?", "id": "Infrastruktur Pembangkitan, Transmisi, Distribusi Listrik." },
  { "en": "Apa Itu Pembangkit Listrik (Power Plant)?", "id": "Fasilitas Menghasilkan Energi Listrik." },
  { "en": "Apa Itu Saluran Transmisi (Transmission Line)?", "id": "Menyalurkan Listrik Jarak Jauh Tegangan Tinggi." },
  { "en": "Apa Itu Jaringan Distribusi (Distribution Network)?", "id": "Menyalurkan Listrik Ke Pelanggan Tegangan Rendah." },
  { "en": "Apa Itu Gardu Induk (Substation)?", "id": "Fasilitas Mengubah Level Tegangan." },
  { "en": "Apa Itu Sistem Proteksi (Protection System)?", "id": "Melindungi Sistem Tenaga Dari Gangguan." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Memutus Arus Gangguan." },
  { "en": "Apa Itu Rele (Relay) Proteksi?", "id": "Mendeteksi Gangguan Dan Memberi Sinyal Trip." },
  { "en": "Apa Itu Grounding (Pembumian)?", "id": "Menghubungkan Sistem Ke Tanah Untuk Keamanan." },
  { "en": "Apa Itu Teknik Kontrol (Control Engineering)?", "id": "Merancang Sistem Untuk Mengatur Perilaku Sistem Lain." },
  { "en": "Apa Itu Sistem Loop Terbuka (Open Loop)?", "id": "Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Loop Tertutup (Closed Loop)?", "id": "Kontrol Dengan Umpan Balik." },
  { "en": "Apa Itu Sensor (Sensor)?", "id": "Mengukur Besaran Fisik." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Menghasilkan Aksi Fisik Berdasarkan Sinyal Kontrol." },
  { "en": "Apa Itu Kontroler (Controller)?", "id": "Menghitung Sinyal Kontrol Berdasarkan Error." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Digital Untuk Otomatisasi Industri." }



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
