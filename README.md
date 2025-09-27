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


  { "en": "Apa Akibat Arus AC Mengalir Di Permukaan?", "id": "Efek Kulit (Skin Effect)." },
  { "en": "Apa Akibat Cahaya Melepaskan Elektron Dari Logam?", "id": "Efek Fotolistrik." },
  { "en": "Apa Akibat Perbedaan Suhu Menghasilkan Tegangan?", "id": "Efek Seebeck." },
  { "en": "Apa Akibat Tegangan Listrik Menghasilkan Panas/Dingin?", "id": "Efek Peltier." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Resistansi?", "id": "Efek Magnetoresistansi." },
  { "en": "Apa Akibat Medan Magnet Menghasilkan Tegangan?", "id": "Efek Hall." },
  { "en": "Apa Akibat Tekanan Mekanis Menghasilkan Listrik?", "id": "Efek Piezoelektrik." },
  { "en": "Apa Akibat Perubahan Suhu Menghasilkan Listrik?", "id": "Efek Piroelektrik." },
  { "en": "Apa Akibat Medan Magnet Memutar Polarisasi Cahaya?", "id": "Efek Faraday." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Indeks Bias?", "id": "Efek Elektro-Optik." },
  { "en": "Apa Akibat Arus Tinggi Menyebabkan Kegagalan?", "id": "Elektromigrasi." },
  { "en": "Apa Akibat Foton Menghamburkan Elektron?", "id": "Efek Compton." },
  { "en": "Apa Akibat Gelombang Melewati Celah Sempit?", "id": "Difraksi." },
  { "en": "Apa Akibat Dua Gelombang Bertemu?", "id": "Interferensi." },
  { "en": "Apa Akibat Osilasi Amplitudo Besar?", "id": "Resonansi." },
  { "en": "Apa Akibat Medan Magnet Pada Garis Spektrum?", "id": "Efek Zeeman." },
  { "en": "Apa Akibat Medan Listrik Pada Garis Spektrum?", "id": "Efek Stark." },
  { "en": "Apa Akibat Konduktor Dekat Mempengaruhi Distribusi?", "id": "Efek Proksimitas." },
  { "en": "Apa Akibat Medan Listrik Kuat Mengubah Indeks?", "id": "Efek Kerr." },
  { "en": "Apa Akibat Medan Listrik Linier Mengubah Indeks?", "id": "Efek Pockels." },
  { "en": "Apa Akibat Suara Mempengaruhi Cahaya?", "id": "Efek Akusto-Optik." },
  { "en": "Apa Akibat Perubahan Bentuk Akibat Magnet?", "id": "Magnetostriksi." },
  { "en": "Apa Akibat Pantulan Sinyal Di Jalur?", "id": "Gelombang Stasioner." },
  { "en": "Apa Akibat Partikel Bergerak Cepat Melebihi Cahaya?", "id": "Radiasi Cherenkov." },
  { "en": "Apa Akibat Gelombang Elektromagnetik Memberi Tekanan?", "id": "Tekanan Radiasi." },
  { "en": "Apa Akibat Frekuensi Tampak Berubah?", "id": "Efek Doppler." },
  { "en": "Apa Akibat Arus Eddy Meredam Gerakan?", "id": "Pengereman Arus Eddy." },
  { "en": "Apa Akibat Panas Menyebabkan Emisi Elektron?", "id": "Emisi Termionik." },
  { "en": "Apa Akibat Perubahan Bentuk Akibat Listrik?", "id": "Efek Piezoelektrik Terbalik." },
  { "en": "Apa Akibat Sambaran Petir Menciptakan Medan?", "id": "Efek Elektromagnetik Petir (LEMP)." },
  { "en": "Apa Akibat Cahaya Diserap Menghasilkan Suara?", "id": "Efek Fotoakustik." },
  { "en": "Apa Akibat Cahaya Menyebabkan Perubahan Warna?", "id": "Efek Fotokromik." },
  { "en": "Apa Akibat Tegangan Menyebabkan Perubahan Warna?", "id": "Efek Elektrokromik." },
  { "en": "Apa Akibat Resistansi Negatif Pada Dioda?", "id": "Efek Terobosan (Tunneling)." },
  { "en": "Apa Akibat Multiplikasi Elektron Dalam Semikonduktor?", "id": "Efek Avalanche." },
  { "en": "Apa Akibat Penurunan Tegangan Di Basis BJT?", "id": "Efek Early." },
  { "en": "Apa Akibat Cahaya Dihamburkan Molekul?", "id": "Hamburan Rayleigh." },
  { "en": "Apa Akibat Cahaya Dihamburkan Secara Inelastis?", "id": "Efek Raman." },
  { "en": "Apa Akibat Perubahan Posisi Sumber Suara?", "id": "Efek Doppler Akustik." },
  { "en": "Apa Akibat Sinyal Lemah Ditingkatkan Derau?", "id": "Resonansi Stokastik." },
  { "en": "Apa Akibat Sinar Tergeser Saat Pemantulan?", "id": "Efek Goos-Hänchen." },
  { "en": "Apa Akibat Medan Magnet Pada Superkonduktor?", "id": "Efek Meissner." },
  { "en": "Apa Akibat Arus DC Mengalir Di Superkonduktor?", "id": "Efek Josephson." },
  { "en": "Apa Akibat Arus Superkonduktor Menciptakan Kuantum?", "id": "Interferensi Kuantum." },
  { "en": "Apa Akibat Cahaya Meningkatkan Konduktivitas?", "id": "Fotokonduktivitas." },
  { "en": "Apa Akibat Putaran Elektron Mempengaruhi Transport?", "id": "Spintronik." },
  { "en": "Apa Akibat Medan Magnet Pada Resistansi Film?", "id": "Efek Magnetoresistansi Raksasa (GMR)." },
  { "en": "Apa Akibat Terobosan Kuantum Melalui Isolator?", "id": "Efek Magnetoresistansi Terowongan (TMR)." },
  { "en": "Apa Akibat Spin Foton Mempengaruhi Jalur?", "id": "Efek Spin Hall Cahaya." },
  { "en": "Apa Akibat Medan Listrik Menghasilkan Emisi?", "id": "Emisi Medan Elektron." },
  { "en": "Apa Akibat Material Memancarkan Elektron Sekunder?", "id": "Emisi Sekunder." },
  { "en": "Apa Akibat Ledakan Nuklir Menciptakan Pulsa?", "id": "Efek Elektromagnetik Nuklir (NEMP)." },
  { "en": "Apa Akibat Arus Mengalir Tanpa Resistansi?", "id": "Superkonduktivitas." },
  { "en": "Apa Akibat Medan Listrik Menghasilkan Birefringence?", "id": "Efek Kerr." },
  { "en": "Apa Akibat Arus Mengalir Menghasilkan Panas?", "id": "Efek Joule." },
  { "en": "Apa Akibat Panas Menghasilkan Arus Di Konduktor?", "id": "Efek Thomson." },
  { "en": "Apa Akibat Gelombang Merambat Melalui Apertur?", "id": "Difraksi Fraunhofer." },
  { "en": "Apa Akibat Gelombang Merambat Dekat Objek?", "id": "Difraksi Fresnel." },
  { "en": "Apa Akibat Pola Periodik Menciptakan Pola?", "id": "Efek Talbot." },
  { "en": "Apa Akibat Medan Listrik Mengubah Absorpsi?", "id": "Efek Franz-Keldysh." },
  { "en": "Apa Akibat Medan Listrik Di Sumur Kuantum?", "id": "Efek Stark Terkurung Kuantum." },
  { "en": "Apa Akibat Perubahan Medan Magnet Menghasilkan Arus?", "id": "Hukum Induksi Faraday." },
  { "en": "Apa Akibat Arus Menghasilkan Medan Magnet?", "id": "Hukum Ampere." },
  { "en": "Apa Akibat Suara Dari Gelembung Runtuh?", "id": "Sonoluminesensi." },
  { "en": "Apa Akibat Gesekan Mekanis Menghasilkan Cahaya?", "id": "Triboluminesensi." },
  { "en": "Apa Akibat Reaksi Kimia Menghasilkan Cahaya?", "id": "Kemiluminesensi." },
  { "en": "Apa Akibat Material Mengingat Bentuknya?", "id": "Efek Memori Bentuk." },
  { "en": "Apa Akibat Keterlambatan Menyebabkan Osilasi?", "id": "Efek Slingshot." },
  { "en": "Apa Akibat Perubahan Resistansi Akibat Regangan?", "id": "Efek Piezoresistif." },
  { "en": "Apa Akibat Muatan Terakumulasi Saat Fabrikasi?", "id": "Efek Antena." },
  { "en": "Apa Akibat Kapasitansi Umpan Balik Meningkat?", "id": "Efek Miller." },
  { "en": "Apa Akibat Interaksi Medan Dan Materi Kuantum?", "id": "Efek Kuantum." },
  { "en": "Apa Akibat Hamburan Inelastis Foton Tinggi?", "id": "Efek Compton Terbalik." },
  { "en": "Apa Akibat Medan Magnet Kuat Pada Atom?", "id": "Efek Paschen-Back." },
  { "en": "Apa Akibat Cahaya Menyebabkan Perubahan Indeks?", "id": "Efek Fotorefraktif." },
  { "en": "Apa Akibat Elektron Menumbuk Bahan Menghasilkan?", "id": "Katodoluminesensi." },
  { "en": "Apa Akibat Material Panas Memancarkan Cahaya?", "id": "Pijaran (Incandescence)." },
  { "en": "Apa Akibat Hamburan Cahaya Oleh Partikel?", "id": "Efek Tyndall." },
  { "en": "Apa Akibat Muatan Mengalir Dalam Plasma?", "id": "Efek Pinch." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Plasma?", "id": "Magnetohidrodinamika." },
  { "en": "Apa Akibat Radiasi Mengubah Resistansi?", "id": "Efek Fotokonduktif." },
  { "en": "Apa Akibat Pembawa Muatan Melewati Sambungan?", "id": "Efek Difusi." },
  { "en": "Apa Akibat Medan Listrik Menggerakkan Pembawa?", "id": "Efek Drift." },
  { "en": "Apa Akibat Ketidakcocokan Impedansi Menyebabkan?", "id": "Pantulan Sinyal." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Konduktivitas?", "id": "Efek Magnetoresistansi Biasa." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Viskositas?", "id": "Efek Magnetoreologi." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Viskositas?", "id": "Efek Elektroreologi." },
  { "en": "Apa Akibat Penyerapan Cahaya Mengubah Transparansi?", "id": "Penyerapan Saturable." },
  { "en": "Apa Akibat Foton Menghasilkan Pasangan Elektron-Lubang?", "id": "Generasi Fotolistrik." },
  { "en": "Apa Akibat Dua Foton Diserap Bersamaan?", "id": "Penyerapan Dua Foton." },
  { "en": "Apa Akibat Elektron Menghasilkan Getaran Kisi?", "id": "Interaksi Elektron-Fonon." },
  { "en": "Apa Akibat Getaran Kisi Menghamburkan Elektron?", "id": "Hamburan Fonon." },
  { "en": "Apa Akibat Medan Listrik Kuat Menghasilkan?", "id": "Efek Zener." },
  { "en": "Apa Akibat Arus Bocor Meningkat Dengan Suhu?", "id": "Arus Saturasi Terbalik." },
  { "en": "Apa Akibat Medan Listrik Menghasilkan Suara?", "id": "Efek Elektroakustik." },
  { "en": "Apa Akibat Elektron Melintasi Celah Vakum?", "id": "Efek Termionik Schottky." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Arus Termal?", "id": "Efek Ettingshausen." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Aliran Panas?", "id": "Efek Nernst." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Arus Listrik?", "id": "Efek Corbino." },
  { "en": "Apa Akibat Medan Listrik Menyebabkan Aliran?", "id": "Elektro-osmosis." },
  { "en": "Apa Akibat Suara Menghasilkan Aliran Fluida?", "id": "Aliran Akustik." },
  { "en": "Apa Akibat Cahaya Memanaskan Bahan?", "id": "Efek Fototermal." },
  { "en": "Apa Akibat Cahaya Menghasilkan Gaya Mekanis?", "id": "Gaya Optik." },
  { "en": "Apa Akibat Cahaya Menghasilkan Pasangan Foton?", "id": "Spontaneous Parametric Down-Conversion (SPDC)." },
  { "en": "Apa Akibat Medan Listrik Menginduksi Polarisasi?", "id": "Efek Dielektrik." },
  { "en": "Apa Akibat Panas Di Konduktor Menghasilkan Tegangan?", "id": "Efek Seebeck." },
  { "en": "Apa Akibat Tegangan Menyebabkan Perubahan Bentuk?", "id": "Efek Elektrostriktif." },
  { "en": "Apa Akibat Medan Magnet Mengubah Ukuran?", "id": "Efek Magnetostriktif." },
  { "en": "Apa Akibat Cahaya Membuat Medium Transparan?", "id": "Electromagnetically Induced Transparency (EIT)." },
  { "en": "Apa Akibat Gelombang Berakselerasi Sendiri?", "id": "Sinar Airy." },
  { "en": "Apa Akibat Sinar Tidak Mengalami Difraksi?", "id": "Sinar Bessel." },
  { "en": "Apa Akibat Dua Frekuensi Optik Menghasilkan?", "id": "Pembuatan Frekuensi Beda." },
  { "en": "Apa Akibat Dua Frekuensi Optik Menjumlah?", "id": "Pembuatan Frekuensi Jumlah." },
  { "en": "Apa Akibat Kecepatan Grup Cahaya Melambat?", "id": "Cahaya Lambat (Slow Light)." },
  { "en": "Apa Akibat Kecepatan Grup Cahaya Melebihi C?", "id": "Cahaya Cepat (Fast Light)." },
  { "en": "Apa Akibat Foton Terperangkap Dalam Struktur?", "id": "Efek Purcell." },
  { "en": "Apa Akibat Gerakan Acak Partikel Di Fluida?", "id": "Gerak Brown." },
  { "en": "Apa Akibat Medan Kuat Mengionisasi Atom?", "id": "Ionisasi Medan." },
  { "en": "Apa Akibat Foton Menendang Elektron Dari Atom?", "id": "Ionisasi Foto." },
  { "en": "Apa Akibat Elektron Berenergi Menumbuk Atom?", "id": "Ionisasi Tumbukan." },
  { "en": "Apa Akibat Medan Listrik Menarik Ion?", "id": "Drift Ion." },
  { "en": "Apa Akibat Gradien Konsentrasi Menyebabkan Aliran?", "id": "Difusi." },
  { "en": "Apa Akibat Medan Listrik Tinggi Di Katoda?", "id": "Emisi Medan Dingin." },
  { "en": "Apa Akibat Elektron Menembus Penghalang Potensial?", "id": "Terobosan Kuantum." },
  { "en": "Apa Akibat Suhu Mempengaruhi Resistansi Logam?", "id": "Hamburan Elektron-Fonon." },
  { "en": "Apa Akibat Ketidakmurnian Mempengaruhi Resistansi?", "id": "Hamburan Ketidakmurnian." },
  { "en": "Apa Akibat Gerbang Mengontrol Arus Di Bawahnya?", "id": "Efek Medan (Field Effect)." },
  { "en": "Apa Akibat Saluran Menyempit Di Dekat Drain?", "id": "Pinch-Off." },
  { "en": "Apa Akibat Tegangan Drain Mempengaruhi Ambang?", "id": "Drain-Induced Barrier Lowering (DIBL)." },
  { "en": "Apa Akibat Pembawa Panas Merusak Oksida?", "id": "Hot Carrier Injection (HCI)." },
  { "en": "Apa Akibat Tegangan Tinggi Merusak Dielektrik?", "id": "Time-Dependent Dielectric Breakdown (TDDB)." },
  { "en": "Apa Akibat Stres Menyebabkan Arus Bocor?", "id": "Stress-Induced Leakage Current (SILC)." },
  { "en": "Apa Akibat Ion Bergerak Dalam Oksida?", "id": "Mobile Ion Contamination." },
  { "en": "Apa Akibat Elektron Terperangkap Dalam Oksida?", "id": "Electron Trapping." },
  { "en": "Apa Akibat Pelepasan Listrik Statis Merusak?", "id": "Electrostatic Discharge (ESD)." },
  { "en": "Apa Akibat Struktur Parasitik SCR Menyala?", "id": "Latch-Up." },
  { "en": "Apa Akibat Sinyal Clock Tiba Berbeda Waktu?", "id": "Clock Skew." },
  { "en": "Apa Akibat Tepi Clock Bervariasi?", "id": "Clock Jitter." },
  { "en": "Apa Akibat Output Bergantung Urutan Sinyal?", "id": "Race Condition." },
  { "en": "Apa Akibat Pulsa Palsu Pada Output?", "id": "Glitch (Hazard)." },
  { "en": "Apa Akibat Dua Driver Menggerakkan Bus?", "id": "Bus Contention." },
  { "en": "Apa Akibat Medan Magnet Menembus Superkonduktor?", "id": "Efek Proksimitas (Superkonduktor)." },
  { "en": "Apa Akibat Arus Melebihi Arus Kritis?", "id": "Kehilangan Superkonduktivitas." },
  { "en": "Apa Akibat Cahaya Menghasilkan Arus Super?", "id": "Efek Fotovoltaik Josephson." },
  { "en": "Apa Akibat Cahaya Meningkatkan Arus Kritis?", "id": "Efek Optik Proksimitas." },
  { "en": "Apa Akibat Medan Listrik Menembus Logam?", "id": "Penetrasi Medan." },
  { "en": "Apa Akibat Resistansi Negatif Diferensial?", "id": "Efek Gunn." },
  { "en": "Apa Akibat Medan Magnet Mengubah Sifat Termal?", "id": "Efek Magnetokalorik." },
  { "en": "Apa Akibat Medan Listrik Mengubah Sifat Termal?", "id": "Efek Elektrokalorik." },
  { "en": "Apa Akibat Gas Terionisasi Menghantarkan Listrik?", "id": "Pelepasan Gas (Gas Discharge)." },
  { "en": "Apa Akibat Pelepasan Listrik Berkilau?", "id": "Pelepasan Korona." },
  { "en": "Apa Akibat Pelepasan Listrik Menjadi Busur?", "id": "Busur Api (Arc Flash)." },
  { "en": "Apa Akibat Partikel Alfa Mengubah Bit Memori?", "id": "Soft Error." },
  { "en": "Apa Akibat Sinyal Kuat Mengganggu Sinyal Lemah?", "id": "Intermodulasi." },
  { "en": "Apa Akibat Sinyal Kuat Mengurangi Penguatan?", "id": "Kompresi Penguatan." },
  { "en": "Apa Akibat Derau Ditambahkan Oleh Amplifier?", "id": "Noise Figure." },
  { "en": "Apa Akibat Arus Mengubah Sifat Magnetik?", "id": "Spin-Transfer Torque." },
  { "en": "Apa Akibat Gelombang Terpantul Di Jalur?", "id": "Voltage Standing Wave Ratio (VSWR)." },
  { "en": "Apa Akibat Bahan Menyerap Energi Elektromagnetik?", "id": "Pemanasan Dielektrik." },
  { "en": "Apa Akibat Gelombang Terjebak Dalam Struktur?", "id": "Mode Resonansi." },
  { "en": "Apa Akibat Perbedaan Jalur Optik?", "id": "Interferensi Konstruktif Atau Destruktif." },
  { "en": "Apa Akibat Hamburan Cahaya Mempertahankan Energi?", "id": "Hamburan Elastis." },
  { "en": "Apa Akibat Hamburan Cahaya Kehilangan Energi?", "id": "Hamburan Inelastis." },
  { "en": "Apa Akibat Cairan Berpendar Di Medan Listrik?", "id": "Efek Kristal Cair." },
  { "en": "Apa Akibat Sinar Gamma Menghasilkan Pasangan?", "id": "Produksi Pasangan." },
  { "en": "Apa Akibat Foton Diserap Menghasilkan Fonon?", "id": "Penyerapan Optik." },
  { "en": "Apa Akibat Suara Menghasilkan Cahaya?", "id": "Sonoluminesensi." },
  { "en": "Apa Akibat Panas Radiasi Dari Atmosfer?", "id": "Efek Rumah Kaca." },
  { "en": "Apa Akibat Putaran Bumi Membelokkan Benda?", "id": "Efek Coriolis." },
  { "en": "Apa Akibat Arus Listrik Mempengaruhi Otot?", "id": "Stimulasi Listrik." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Jaringan?", "id": "Stimulasi Magnetik." },
  { "en": "Apa Akibat Partikel Bertumbukan Menghasilkan Cahaya?", "id": "Scintillation." },
  { "en": "Apa Akibat Elektron Bergetar Memancarkan Gelombang?", "id": "Radiasi Dipol." },
  { "en": "Apa Akibat Muatan Dipercepat Memancarkan Radiasi?", "id": "Radiasi Bremsstrahlung." },
  { "en": "Apa Akibat Partikel Melambat Memancarkan Radiasi?", "id": "Pengereman Radiatif." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Arus?", "id": "Gaya Lorentz." },
  { "en": "Apa Akibat Permukaan Memantulkan Cahaya Berbeda?", "id": "Refleksi Difus Dan Spekular." },
  { "en": "Apa Akibat Sinar Paralel Tidak Fokus?", "id": "Aberasi Sferis." },
  { "en": "Apa Akibat Warna Berbeda Fokus Berbeda?", "id": "Aberasi Kromatik." },
  { "en": "Apa Akibat Titik Tidak Terbentuk Titik?", "id": "Astigmatisme." },
  { "en": "Apa Akibat Gambar Melengkung?", "id": "Distorsi Barel Atau Pincushion." },
  { "en": "Apa Akibat Kecepatan Cahaya Bergantung Medium?", "id": "Pembiasan (Refraksi)." },
  { "en": "Apa Akibat Pemisahan Cahaya Menjadi Warna?", "id": "Dispersi." },
  { "en": "Apa Akibat Cahaya Tersebar Merata?", "id": "Hamburan Mie." },
  { "en": "Apa Akibat Arus Dalam Plasma Menciptakan?", "id": "Efek Pinch Magnetik." },
  { "en": "Apa Akibat Gelombang Terpantul Dari Ionosfer?", "id": "Propagasi Gelombang Langit." },
  { "en": "Apa Akibat Gelombang Merambat Di Permukaan?", "id": "Propagasi Gelombang Tanah." },
  { "en": "Apa Akibat Penyerapan Selektif Polarisasi?", "id": "Dichroism." },
  { "en": "Apa Akibat Rotasi Polarisasi Dalam Bahan?", "id": "Aktivitas Optik." },
  { "en": "Apa Akibat Bahan Menjadi Birefringent?", "id": "Efek Fotoelastis." },
  { "en": "Apa Akibat Arus Melingkar Di Konduktor?", "id": "Arus Eddy." },
  { "en": "Apa Akibat Penurunan Tegangan Di Terminal?", "id": "Resistansi Internal." },
  { "en": "Apa Akibat Aliran Udara Menghasilkan Listrik?", "id": "Efek Triboelektrik." },
  { "en": "Apa Akibat Gelombang Suara Mempengaruhi Indeks?", "id": "Efek Akusto-Optik." },
  { "en": "Apa Akibat Medan Listrik Kuat Di Vakum?", "id": "Breakdown Vakum." },
  { "en": "Apa Akibat Material Memancarkan Elektron Saat Panas?", "id": "Emisi Termionik." },
  { "en": "Apa Akibat Elektron Berenergi Menghasilkan Sinar-X?", "id": "Radiasi Bremsstrahlung." },
  { "en": "Apa Akibat Foton Menghasilkan Getaran Kisi?", "id": "Interaksi Foton-Fonon." },
  { "en": "Apa Akibat Dua Foton Bertumbukan?", "id": "Hamburan Cahaya-Cahaya." },
  { "en": "Apa Akibat Partikel Terperangkap Dalam Potensial?", "id": "Kuantisasi Energi." },
  { "en": "Apa Akibat Elektron Terkurung Dalam Dimensi?", "id": "Sumur Kuantum." },
  { "en": "Apa Akibat Elektron Menembus Penghalang?", "id": "Efek Terowongan." },
  { "en": "Apa Akibat Elektron Bergerak Melalui Kisi?", "id": "Osilasi Bloch." },
  { "en": "Apa Akibat Tegangan Menyebabkan Emisi Cahaya?", "id": "Elektroluminesensi." },
  { "en": "Apa Akibat Penyerapan Cahaya Menghasilkan Fluoresensi?", "id": "Efek Fluoresensi." },
  { "en": "Apa Akibat Emisi Foton Dari Rekombinasi?", "id": "Luminesensi." },
  { "en": "Apa Akibat Cahaya Mempengaruhi Sifat Magnetik?", "id": "Efek Fotomagnetik." },
  { "en": "Apa Akibat Cahaya Mengubah Resistansi?", "id": "Efek Fotokonduktif." },
  { "en": "Apa Akibat Tegangan Menghasilkan Gaya?", "id": "Efek Elektromekanik." },
  { "en": "Apa Akibat Panas Menghasilkan Suara?", "id": "Efek Termoakustik." },
  { "en": "Apa Akibat Gelombang Berdiri Di Resonator?", "id": "Mode Resonansi." },
  { "en": "Apa Akibat Medan Listrik Mengarahkan Kristal?", "id": "Efek Kristal Cair." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Fluida?", "id": "Efek Elektrohidrodinamik." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Fluida?", "id": "Efek Magnetohidrodinamik." },
  { "en": "Apa Akibat Elektron Menghasilkan Panas Di Anoda?", "id": "Pemanasan Anoda." },
  { "en": "Apa Akibat Emisi Elektron Dari Logam Cair?", "id": "Emisi Ion Logam Cair." },
  { "en": "Apa Akibat Gelombang Terpantul Berinterferensi?", "id": "Pola Bintik (Speckle Pattern)." },
  { "en": "Apa Akibat Elektron Menghasilkan Elektron Sekunder?", "id": "Multiplikasi Elektron." },
  { "en": "Apa Akibat Foton Menghasilkan Fonon?", "id": "Hamburan Raman." },
  { "en": "Apa Akibat Foton Menyerap Fonon?", "id": "Hamburan Anti-Stokes." },
  { "en": "Apa Akibat Medan Listrik Tinggi Menyebabkan?", "id": "Kerusakan Dielektrik." },
  { "en": "Apa Akibat Arus Tinggi Melelehkan Sekering?", "id": "Efek Sekering." },
  { "en": "Apa Akibat Arus Tinggi Di Plasma?", "id": "Efek Pinch." },
  { "en": "Apa Akibat Elektron Terkurung Dalam Potensial?", "id": "Tingkat Energi Kuantum." },
  { "en": "Apa Akibat Sinyal Kuat Memblokir Penerima?", "id": "Blocking." },
  { "en": "Apa Akibat Pencampuran Frekuensi Non-linier?", "id": "Distorsi Intermodulasi." },
  { "en": "Apa Akibat Sinyal Tercermin Kembali Ke Sumber?", "id": "Kerugian Kembali (Return Loss)." },
  { "en": "Apa Akibat Sinyal Kehilangan Daya Di Komponen?", "id": "Kerugian Penyisipan (Insertion Loss)." },
  { "en": "Apa Akibat Arus Listrik Memisahkan Campuran?", "id": "Elektroforesis." },
  { "en": "Apa Akibat Medan Listrik Memompa Fluida?", "id": "Pompa Elektrohidrodinamik." },
  { "en": "Apa Akibat Medan Magnet Memompa Logam Cair?", "id": "Pompa Magnetohidrodinamik." },
  { "en": "Apa Akibat Cahaya Menghasilkan Polimerisasi?", "id": "Fotopolimerisasi." },
  { "en": "Apa Akibat Radiasi Mengubah Sifat Bahan?", "id": "Kerusakan Radiasi." },
  { "en": "Apa Akibat Elektron Menghasilkan Cahaya Di Fosfor?", "id": "Pendaran Katoda." },
  { "en": "Apa Akibat Bahan Menyerap Energi Dan Memancarkan?", "id": "Fotoluminesensi." },
  { "en": "Apa Akibat Emisi Cahaya Cepat?", "id": "Fluoresensi." },
  { "en": "Apa Akibat Emisi Cahaya Lambat?", "id": "Fosforesensi." },
  { "en": "Apa Akibat Reaksi Kimia Memancarkan Cahaya?", "id": "Kemiluminesensi." },
  { "en": "Apa Akibat Organisme Hidup Memancarkan Cahaya?", "id": "Bioluminesensi." },
  { "en": "Apa Akibat Busur Listrik Menghasilkan Cahaya?", "id": "Lampu Busur (Arc Lamp)." },
  { "en": "Apa Akibat Gas Jarang Memancarkan Cahaya?", "id": "Pelepasan Pijar (Glow Discharge)." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Aliran?", "id": "Efek Elektrokinetik." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Reaksi?", "id": "Efek Kimia Magnetik." },
  { "en": "Apa Akibat Arus Tinggi Menguapkan Kawat?", "id": "Ledakan Kawat Listrik." },
  { "en": "Apa Akibat Gelombang Kejut Mengionisasi Gas?", "id": "Ionisasi Kejut." },
  { "en": "Apa Akibat Medan Listrik Kuat Memisahkan Air?", "id": "Elektrolisis Air." },
  { "en": "Apa Akibat Suara Mempengaruhi Aliran Listrik?", "id": "Efek Akustoelektrik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Elastisitas?", "id": "Efek Elektroelastis." },
  { "en": "Apa Akibat Cahaya Mempengaruhi Sifat Mekanis?", "id": "Efek Fotomekanik." },
  { "en": "Apa Akibat Perubahan Fasa Menyerap Panas?", "id": "Efek Kalor Laten." },
  { "en": "Apa Akibat Arus Mengalir Melalui Resistansi?", "id": "Pemanasan Joule." },
  { "en": "Apa Akibat Medan Magnet Mengubah Keadaan Kuantum?", "id": "Resonansi Magnetik." },
  { "en": "Apa Akibat Medan Listrik Mengubah Keadaan Kuantum?", "id": "Resonansi Listrik." },
  { "en": "Apa Akibat Elektron Berinteraksi Dengan Getaran?", "id": "Kopling Elektron-Fonon." },
  { "en": "Apa Akibat Rotasi Mempengaruhi Sifat Listrik?", "id": "Efek Gyroelektrik." },
  { "en": "Apa Akibat Medan Magnet Kuat Mengubah Spektrum?", "id": "Efek Voigt." },
  { "en": "Apa Akibat Cahaya Kuat Merusak Material?", "id": "Kerusakan Optik." },
  { "en": "Apa Akibat Elektron Menembus Dielektrik?", "id": "Injeksi Muatan." },
  { "en": "Apa Akibat Muatan Terperangkap Dalam Isolator?", "id": "Penumpukan Muatan Ruang." },
  { "en": "Apa Akibat Medan Tinggi Menyebabkan Arus?", "id": "Emisi Fowler-Nordheim." },
  { "en": "Apa Akibat Panas Membantu Emisi Medan?", "id": "Emisi Termal-Medan." },
  { "en": "Apa Akibat Tumbukan Ion Menghasilkan Emisi?", "id": "Emisi Elektron Terinduksi Ion." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Arus Terowongan?", "id": "Efek Magnetoterowongan." },
  { "en": "Apa Akibat Cahaya Mempengaruhi Sifat Ferroelektrik?", "id": "Efek Fotoferroelektrik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Permeabilitas?", "id": "Efek Magnetoelektrik." },
  { "en": "Apa Akibat Regangan Mempengaruhi Permitivitas?", "id": "Efek Piezodielektrik." },
  { "en": "Apa Akibat Arus Listrik Di Semikonduktor?", "id": "Efek Suhl." },
  { "en": "Apa Akibat Medan Listrik Tinggi Di Semikonduktor?", "id": "Efek Gunn." },
  { "en": "Apa Akibat Gelombang Suara Di Semikonduktor?", "id": "Efek Akustoelektrik." },
  { "en": "Apa Akibat Cahaya Mempengaruhi Sifat Semikonduktor?", "id": "Efek Fotodielektrik." },
  { "en": "Apa Akibat Arus Mempengaruhi Indeks Bias?", "id": "Efek Elektro-Optik Arus-Injeksi." },
  { "en": "Apa Akibat Panas Mempengaruhi Indeks Bias?", "id": "Efek Termo-Optik." },
  { "en": "Apa Akibat Aliran Fluida Menghasilkan Tegangan?", "id": "Potensial Aliran." },
  { "en": "Apa Akibat Tegangan Menghasilkan Aliran Fluida?", "id": "Elektro-osmosis." },
  { "en": "Apa Akibat Suhu Mempengaruhi Kapasitansi?", "id": "Efek Piroelektrik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Konduktivitas?", "id": "Efek Medan Konduktivitas." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Emisi Cahaya?", "id": "Efek Magnetoluminesensi." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Emisi Cahaya?", "id": "Efek Elektroluminesensi." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Permukaan?", "id": "Efek Elektrokapiler." },
  { "en": "Apa Akibat Gelombang Radio Menghasilkan Arus?", "id": "Efek Penyearahan." },
  { "en": "Apa Akibat Arus Mengubah Resistansi Superkonduktor?", "id": "Efek Kinetik Induktansi." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Gelombang Suara?", "id": "Efek Magnetoakustik." },
  { "en": "Apa Akibat Cahaya Menyebabkan Penguapan Material?", "id": "Ablasi Laser." },
  { "en": "Apa Akibat Cahaya Memanipulasi Partikel?", "id": "Pinset Optik." },
  { "en": "Apa Akibat Elektron Berputar Dalam Medan?", "id": "Radiasi Siklotron." },
  { "en": "Apa Akibat Elektron Bergerak Cepat Berkelok?", "id": "Radiasi Synchrotron." },
  { "en": "Apa Akibat Partikel Terionisasi Memancarkan Cahaya?", "id": "Spektroskopi Emisi." },
  { "en": "Apa Akibat Partikel Menyerap Cahaya Tertentu?", "id": "Spektroskopi Absorpsi." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Permitivitas?", "id": "Efek Kerr." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Kapasitansi?", "id": "Efek Magnetokapasitansi." },
  { "en": "Apa Akibat Regangan Mempengaruhi Sifat Magnetik?", "id": "Efek Villari." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Konduktivitas?", "id": "Efek Hall Biasa." },
  { "en": "Apa Akibat Spin Elektron Mempengaruhi Konduktivitas?", "id": "Efek Hall Anomali." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Arus Spin?", "id": "Efek Spin Seebeck." },
  { "en": "Apa Akibat Arus Spin Menghasilkan Panas?", "id": "Efek Spin Peltier." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Arus Spin?", "id": "Efek Rashba-Edelstein." },
  { "en": "Apa Akibat Medan Magnet Mengubah Keadaan Spin?", "id": "Resonansi Spin Elektron." },
  { "en": "Apa Akibat Osilasi Kolektif Elektron?", "id": "Plasmon." },
  { "en": "Apa Akibat Osilasi Kolektif Getaran Kisi?", "id": "Fonon." },
  { "en": "Apa Akibat Kopling Cahaya Dan Eksiton?", "id": "Polariton." },
  { "en": "Apa Akibat Pasangan Elektron-Lubang Terikat?", "id": "Eksiton." },
  { "en": "Apa Akibat Elektron Berinteraksi Dengan Dirinya?", "id": "Efek Lamb Shift." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Polarisasi?", "id": "Efek Elektro-girasi." },
  { "en": "Apa Akibat Regangan Mempengaruhi Polarisasi?", "id": "Efek Piezo-optik." },
  { "en": "Apa Akibat Suhu Mempengaruhi Polarisasi?", "id": "Efek Piro-optik." },
  { "en": "Apa Akibat Cahaya Menghasilkan Sinyal Listrik?", "id": "Efek Fotovoltaik." },
  { "en": "Apa Akibat Medan Magnet Mengubah Hambatan?", "id": "Efek Magnetoresistif." },
  { "en": "Apa Akibat Panas Di Semikonduktor Menghasilkan?", "id": "Efek Termoelektrik." },
  { "en": "Apa Akibat Getaran Menghasilkan Listrik?", "id": "Efek Piezoelektrik." },
  { "en": "Apa Akibat Medan Listrik Mengubah Polarisasi?", "id": "Efek Pockels Dan Kerr." },
  { "en": "Apa Akibat Arus Tinggi Menggerakkan Atom?", "id": "Efek Elektromigrasi." },
  { "en": "Apa Akibat Partikel Bertumbukan Dengan Elektron?", "id": "Efek Compton." },
  { "en": "Apa Akibat Gelombang Cahaya Melewati Celah?", "id": "Efek Difraksi." },
  { "en": "Apa Akibat Gelombang Cahaya Saling Tumpang Tindih?", "id": "Efek Interferensi." },
  { "en": "Apa Akibat Frekuensi Paksa Sama Dengan Frekuensi?", "id": "Efek Resonansi." },
  { "en": "Apa Akibat Arus Eddy Melawan Perubahan?", "id": "Hukum Lenz." },
  { "en": "Apa Akibat Suhu Mempengaruhi Medan Magnet?", "id": "Efek Piro-magnetik." },
  { "en": "Apa Akibat Tekanan Mempengaruhi Medan Magnet?", "id": "Efek Piezo-magnetik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Sifat?", "id": "Efek Elektrostriktif." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Dimensi?", "id": "Efek Magnetostriktif." },
  { "en": "Apa Akibat Medan Magnet Mengubah Konduktivitas?", "id": "Efek Hall." },
  { "en": "Apa Akibat Medan Listrik Mengubah Ukuran?", "id": "Efek Piezoelektrik Terbalik." },
  { "en": "Apa Akibat Bahan Menyerap Cahaya Dan Memancarkan?", "id": "Efek Fluoresensi." },
  { "en": "Apa Akibat Cahaya Matahari Menghasilkan Listrik?", "id": "Efek Fotovoltaik." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Arus?", "id": "Efek Magnetoresistansi." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Hambatan?", "id": "Efek Varistor." },
  { "en": "Apa Akibat Arus Mengalir Dalam Gas?", "id": "Pelepasan Pijar (Glow Discharge)." },
  { "en": "Apa Akibat Arus Sangat Tinggi Dalam Gas?", "id": "Busur Api (Electric Arc)." },
  { "en": "Apa Akibat Medan Magnet Menolak Superkonduktor?", "id": "Efek Meissner." },
  { "en": "Apa Akibat Medan Listrik Tinggi Di Ujung?", "id": "Efek Korona." },
  { "en": "Apa Akibat Arus Melebihi Batas Sekering?", "id": "Efek Peleburan (Fusing)." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Permukaan?", "id": "Tegangan Permukaan." },
  { "en": "Apa Akibat Arus Melalui Sambungan Semikonduktor?", "id": "Efek Penyearahan." },
  { "en": "Apa Akibat Cahaya Menghasilkan Pasangan?", "id": "Generasi Pembawa Foto." },
  { "en": "Apa Akibat Panas Menghasilkan Pasangan?", "id": "Generasi Termal." },
  { "en": "Apa Akibat Medan Listrik Menghasilkan Pasangan?", "id": "Generasi Avalanche." },
  { "en": "Apa Akibat Elektron Dan Lubang Bergabung?", "id": "Rekombinasi." },
  { "en": "Apa Akibat Rekombinasi Memancarkan Cahaya?", "id": "Rekombinasi Radiatif." },
  { "en": "Apa Akibat Rekombinasi Menghasilkan Panas?", "id": "Rekombinasi Non-Radiatif." },
  { "en": "Apa Akibat Medan Listrik Mempercepat Pembawa?", "id": "Drift Pembawa Muatan." },
  { "en": "Apa Akibat Gradien Konsentrasi Menyebabkan?", "id": "Difusi Pembawa Muatan." },
  { "en": "Apa Akibat Pembawa Terperangkap Dalam Cacat?", "id": "Penangkapan Pembawa." },
  { "en": "Apa Akibat Pembawa Panas Merusak Perangkat?", "id": "Degradasi Pembawa Panas." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Arus?", "id": "Gaya Lorentz." },
  { "en": "Apa Akibat Arus Listrik Menciptakan Medan?", "id": "Hukum Ampere." },
  { "en": "Apa Akibat Perubahan Medan Magnet Menciptakan?", "id": "Hukum Induksi Faraday." },
  { "en": "Apa Akibat Muatan Listrik Menciptakan Medan?", "id": "Hukum Gauss." },
  { "en": "Apa Akibat Tidak Ada Monopol Magnetik?", "id": "Hukum Gauss Untuk Magnetisme." },
  { "en": "Apa Akibat Hamburan Cahaya Oleh Partikel?", "id": "Hamburan Mie." },
  { "en": "Apa Akibat Getaran Molekul Menyerap Radiasi?", "id": "Penyerapan Inframerah." },
  { "en": "Apa Akibat Elektron Berpindah Tingkat Energi?", "id": "Penyerapan Atom." },
  { "en": "Apa Akibat Elektron Jatuh Ke Tingkat Energi?", "id": "Emisi Atom." },
  { "en": "Apa Akibat Emisi Foton Spontan?", "id": "Emisi Spontan." },
  { "en": "Apa Akibat Foton Memicu Emisi Foton Lain?", "id": "Emisi Terstimulasi." },
  { "en": "Apa Akibat Lebih Banyak Elektron Di Tingkat?", "id": "Inversi Populasi." },
  { "en": "Apa Akibat Cahaya Diperkuat Oleh Emisi?", "id": "Penguatan Cahaya." },
  { "en": "Apa Akibat Elektron Bergerak Melalui Kisi?", "id": "Pita Energi." },
  { "en": "Apa Akibat Celah Antara Pita Energi?", "id": "Celah Pita (Band Gap)." },
  { "en": "Apa Akibat Elektron Melompati Celah Pita?", "id": "Eksitasi." },
  { "en": "Apa Akibat Elektron Dan Lubang Terikat?", "id": "Eksiton." },
  { "en": "Apa Akibat Getaran Kisi Kristal?", "id": "Fonon." },
  { "en": "Apa Akibat Osilasi Elektron Kolektif?", "id": "Plasmon." },
  { "en": "Apa Akibat Medan Listrik Mengubah Kapasitansi?", "id": "Efek Varaktor." },
  { "en": "Apa Akibat Arus Tinggi Menyebabkan Osilasi?", "id": "Efek Gunn." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Sifat?", "id": "Efek Elektrokalorik." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Sifat?", "id": "Efek Magnetokalorik." },
  { "en": "Apa Akibat Stres Mekanis Mengubah Resistansi?", "id": "Efek Piezoresistif." },
  { "en": "Apa Akibat Suara Menghasilkan Listrik?", "id": "Efek Akustoelektrik." },
  { "en": "Apa Akibat Cahaya Mengubah Sifat Mekanis?", "id": "Efek Fotomekanik." },
  { "en": "Apa Akibat Gelombang Radio Dipantulkan Ionosfer?", "id": "Propagasi Gelombang Langit." },
  { "en": "Apa Akibat Gelombang Radio Mengikuti Lengkungan?", "id": "Propagasi Gelombang Permukaan." },
  { "en": "Apa Akibat Sinyal Melewati Banyak Jalur?", "id": "Fading Multipath." },
  { "en": "Apa Akibat Suhu Mempengaruhi Arus Dioda?", "id": "Arus Saturasi Terbalik." },
  { "en": "Apa Akibat Tegangan Menyebabkan Aliran Fluida?", "id": "Efek Elektro-osmosis." },
  { "en": "Apa Akibat Aliran Fluida Menghasilkan Tegangan?", "id": "Potensial Streaming." },
  { "en": "Apa Akibat Medan Listrik Memisahkan Partikel?", "id": "Dielektroforesis." },
  { "en": "Apa Akibat Gelombang Suara Memanipulasi Partikel?", "id": "Pinset Akustik." },
  { "en": "Apa Akibat Arus Listrik Menghasilkan Suara?", "id": "Efek Termoakustik." },
  { "en": "Apa Akibat Suhu Mempengaruhi Permeabilitas Magnetik?", "id": "Efek Piromagnetik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Permeabilitas?", "id": "Efek Magnetoelektrik." },
  { "en": "Apa Akibat Stres Mempengaruhi Permitivitas Dielektrik?", "id": "Efek Piezodielektrik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Rotasi?", "id": "Efek Elektro-girasi." },
  { "en": "Apa Akibat Stres Mekanis Mempengaruhi Polarisasi?", "id": "Efek Piezo-optik." },
  { "en": "Apa Akibat Suhu Mempengaruhi Polarisasi Cahaya?", "id": "Efek Piro-optik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Emisi?", "id": "Efek Schottky." },
  { "en": "Apa Akibat Medan Listrik Tinggi Menarik?", "id": "Emisi Medan Fowler-Nordheim." },
  { "en": "Apa Akibat Elektron Menghasilkan Elektron Auger?", "id": "Efek Auger." },
  { "en": "Apa Akibat Bahan Menyerap Gas Ke Permukaan?", "id": "Adsorpsi." },
  { "en": "Apa Akibat Bahan Menyerap Gas Ke Dalam?", "id": "Absorpsi." },
  { "en": "Apa Akibat Permukaan Katalis Mempercepat Reaksi?", "id": "Katalisis Heterogen." },
  { "en": "Apa Akibat Perubahan Fasa Menyerap Panas?", "id": "Efek Kalor Laten." },
  { "en": "Apa Akibat Pelarutan Zat Menyerap Panas?", "id": "Efek Endotermik." },
  { "en": "Apa Akibat Reaksi Kimia Melepaskan Panas?", "id": "Efek Eksotermik." },
  { "en": "Apa Akibat Tekanan Mempengaruhi Titik Didih?", "id": "Hukum Clausius-Clapeyron." },
  { "en": "Apa Akibat Aliran Cepat Menurunkan Tekanan?", "id": "Efek Bernoulli." },
  { "en": "Apa Akibat Viskositas Menahan Aliran Fluida?", "id": "Gaya Gesek Viskos." },
  { "en": "Apa Akibat Benda Berputar Dalam Fluida?", "id": "Efek Magnus." },
  { "en": "Apa Akibat Suara Bergerak Lebih Cepat?", "id": "Ledakan Sonik (Sonic Boom)." },
  { "en": "Apa Akibat Getaran Menyebar Melalui Benda?", "id": "Gelombang Mekanis." },
  { "en": "Apa Akibat Energi Tersimpan Dalam Deformasi?", "id": "Energi Potensial Elastis." },
  { "en": "Apa Akibat Osilasi Dengan Amplitudo Berkurang?", "id": "Redaman (Damping)." },
  { "en": "Apa Akibat Gaya Luar Memicu Osilasi?", "id": "Resonansi Paksa." },
  { "en": "Apa Akibat Dua Frekuensi Dekat Berinterferensi?", "id": "Layangan (Beats)." },
  { "en": "Apa Akibat Gelombang Berdiri Dalam Senar?", "id": "Mode Harmonik." },
  { "en": "Apa Akibat Perbedaan Jalur Menyebabkan Pola?", "id": "Pola Interferensi." },
  { "en": "Apa Akibat Medan Listrik Mengubah Ukuran?", "id": "Efek Elektrostriktif." },
  { "en": "Apa Akibat Cahaya Membuat Medium Opak Transparan?", "id": "Electromagnetically Induced Transparency (EIT)." },
  { "en": "Apa Akibat Sinar Cahaya Berakselerasi Sendiri?", "id": "Sinar Airy." },
  { "en": "Apa Akibat Sinar Cahaya Tidak Mengalami Difraksi?", "id": "Sinar Bessel." },
  { "en": "Apa Akibat Dua Frekuensi Cahaya Menciptakan Frekuensi Beda?", "id": "Difference Frequency Generation (DFG)." },
  { "en": "Apa Akibat Dua Frekuensi Cahaya Menciptakan Frekuensi Jumlah?", "id": "Sum Frequency Generation (SFG)." },
  { "en": "Apa Akibat Kecepatan Grup Pulsa Cahaya Melambat?", "id": "Cahaya Lambat (Slow Light)." },
  { "en": "Apa Akibat Kecepatan Grup Pulsa Melebihi C?", "id": "Cahaya Cepat (Fast Light)." },
  { "en": "Apa Akibat Resonator Meningkatkan Emisi Spontan?", "id": "Efek Purcell." },
  { "en": "Apa Akibat Gerakan Acak Partikel Dalam Fluida?", "id": "Gerak Brown." },
  { "en": "Apa Akibat Medan Listrik Kuat Mengionisasi Atom?", "id": "Ionisasi Medan." },
  { "en": "Apa Akibat Foton Berenergi Mengionisasi Atom?", "id": "Ionisasi Foto (Photoionization)." },
  { "en": "Apa Akibat Elektron Berenergi Menumbuk Atom?", "id": "Ionisasi Tumbukan." },
  { "en": "Apa Akibat Medan Listrik Menggerakkan Ion Dalam Larutan?", "id": "Drift Ion." },
  { "en": "Apa Akibat Gradien Konsentrasi Menyebabkan Aliran Partikel?", "id": "Difusi." },
  { "en": "Apa Akibat Medan Sangat Kuat Di Ujung Tajam?", "id": "Emisi Medan Dingin." },
  { "en": "Apa Akibat Elektron Melintasi Celah Vakum?", "id": "Efek Schottky." },
  { "en": "Apa Akibat Saluran MOSFET Menyempit Di Dekat Drain?", "id": "Pinch-Off." },
  { "en": "Apa Akibat Tegangan Drain Mempengaruhi Tegangan Ambang?", "id": "Drain-Induced Barrier Lowering (DIBL)." },
  { "en": "Apa Akibat Pembawa Muatan Panas Merusak Oksida Gerbang?", "id": "Hot Carrier Injection (HCI)." },
  { "en": "Apa Akibat Tegangan Tinggi Merusak Lapisan Dielektrik?", "id": "Time-Dependent Dielectric Breakdown (TDDB)." },
  { "en": "Apa Akibat Stres Listrik Menyebabkan Arus Bocor?", "id": "Stress-Induced Leakage Current (SILC)." },
  { "en": "Apa Akibat Kontaminasi Ion Bergerak Dalam Oksida?", "id": "Mobile Ion Contamination." },
  { "en": "Apa Akibat Elektron Terperangkap Dalam Lapisan Oksida?", "id": "Electron Trapping." },
  { "en": "Apa Akibat Struktur Parasitik Thyristor Menyala?", "id": "Latch-Up." },
  { "en": "Apa Akibat Sinyal Clock Tiba Di Waktu Berbeda?", "id": "Clock Skew." },
  { "en": "Apa Akibat Tepi Sinyal Clock Bervariasi?", "id": "Clock Jitter." },
  { "en": "Apa Akibat Output Bergantung Pada Urutan Penundaan?", "id": "Race Condition." },
  { "en": "Apa Akibat Pulsa Palsu Muncul Pada Output Logika?", "id": "Glitch Atau Hazard." },
  { "en": "Apa Akibat Dua Output Menggerakkan Satu Bus Bersamaan?", "id": "Bus Contention." },
  { "en": "Apa Akibat Medan Magnet Menembus Lapisan Superkonduktor?", "id": "Efek Proksimitas (Superkonduktor)." },
  { "en": "Apa Akibat Arus Melebihi Batas Kritis Superkonduktor?", "id": "Kehilangan Sifat Superkonduktivitas." },
  { "en": "Apa Akibat Resistansi Negatif Pada Dioda Tertentu?", "id": "Efek Gunn." },
  { "en": "Apa Akibat Medan Magnet Mengubah Suhu Bahan?", "id": "Efek Magnetokalorik." },
  { "en": "Apa Akibat Medan Listrik Mengubah Suhu Bahan?", "id": "Efek Elektrokalorik." },
  { "en": "Apa Akibat Gas Terionisasi Menghantarkan Listrik?", "id": "Pelepasan Gas (Gas Discharge)." },
  { "en": "Apa Akibat Pelepasan Listrik Berbentuk Cahaya Berkilau?", "id": "Pelepasan Korona." },
  { "en": "Apa Akibat Partikel Kosmik Mengubah Bit Memori?", "id": "Soft Error." },
  { "en": "Apa Akibat Sinyal Kuat Mengurangi Penguatan Amplifier?", "id": "Kompresi Penguatan." },
  { "en": "Apa Akibat Amplifier Menambahkan Derau Ke Sinyal?", "id": "Noise Figure." },
  { "en": "Apa Akibat Gelombang Terpantul Di Jalur Transmisi?", "id": "Voltage Standing Wave Ratio (VSWR)." },
  { "en": "Apa Akibat Gelombang Terjebak Di Dalam Struktur Optik?", "id": "Mode Resonansi." },
  { "en": "Apa Akibat Cairan Berpendar Dalam Medan Listrik?", "id": "Efek Kristal Cair." },
  { "en": "Apa Akibat Suara Dari Gelembung Yang Runtuh?", "id": "Efek Sonoluminesensi." },
  { "en": "Apa Akibat Panas Radiasi Terperangkap Di Atmosfer?", "id": "Efek Rumah Kaca." },
  { "en": "Apa Akibat Putaran Bumi Membelokkan Gerakan?", "id": "Efek Coriolis." },
  { "en": "Apa Akibat Arus Listrik Merangsang Jaringan Saraf?", "id": "Stimulasi Listrik." },
  { "en": "Apa Akibat Medan Magnet Merangsang Jaringan Saraf?", "id": "Stimulasi Magnetik." },
  { "en": "Apa Akibat Elektron Bergetar Memancarkan Gelombang?", "id": "Radiasi Dipol." },
  { "en": "Apa Akibat Muatan Yang Dipercepat Memancarkan Radiasi?", "id": "Radiasi Bremsstrahlung." },
  { "en": "Apa Akibat Permukaan Memantulkan Cahaya Secara Berbeda?", "id": "Refleksi Difus Dan Spekular." },
  { "en": "Apa Akibat Titik Fokus Tidak Terbentuk Titik?", "id": "Efek Astigmatisme." },
  { "en": "Apa Akibat Gambar Terlihat Melengkung Melalui Lensa?", "id": "Distorsi Barel Atau Pincushion." },
  { "en": "Apa Akibat Hamburan Cahaya Merata Oleh Partikel?", "id": "Hamburan Mie." },
  { "en": "Apa Akibat Gelombang Radio Terpantul Dari Ionosfer?", "id": "Propagasi Gelombang Langit." },
  { "en": "Apa Akibat Gelombang Radio Mengikuti Lengkungan Bumi?", "id": "Propagasi Gelombang Permukaan." },
  { "en": "Apa Akibat Penyerapan Cahaya Berbeda Untuk Polarisasi?", "id": "Efek Dichroism." },
  { "en": "Apa Akibat Bahan Kiral Memutar Polarisasi Cahaya?", "id": "Aktivitas Optik." },
  { "en": "Apa Akibat Bahan Menjadi Birefringent Akibat Stres?", "id": "Efek Fotoelastis." },
  { "en": "Apa Akibat Penurunan Tegangan Di Terminal Baterai?", "id": "Resistansi Internal." },
  { "en": "Apa Akibat Medan Listrik Kuat Di Ruang Hampa?", "id": "Breakdown Vakum." },
  { "en": "Apa Akibat Elektron Berenergi Menghasilkan Foton Sinar-X?", "id": "Radiasi Bremsstrahlung." },
  { "en": "Apa Akibat Dua Foton Saling Bertumbukan?", "id": "Hamburan Cahaya-Cahaya." },
  { "en": "Apa Akibat Partikel Terperangkap Dalam Ruang Sempit?", "id": "Kuantisasi Energi." },
  { "en": "Apa Akibat Elektron Bergerak Melalui Kisi Periodik?", "id": "Osilasi Bloch." },
  { "en": "Apa Akibat Cahaya Mempengaruhi Sifat Magnetik Bahan?", "id": "Efek Fotomagnetik." },
  { "en": "Apa Akibat Suhu Mempengaruhi Kapasitansi Bahan?", "id": "Efek Piroelektrik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Konduktivitas Bahan?", "id": "Efek Medan Konduktivitas." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Emisi Cahaya?", "id": "Efek Magnetoluminesensi." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Tegangan Permukaan?", "id": "Efek Elektrokapiler." },
  { "en": "Apa Akibat Arus Mengubah Resistansi Lapisan Superkonduktor?", "id": "Efek Kinetik Induktansi." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Gelombang Suara?", "id": "Efek Magnetoakustik." },
  { "en": "Apa Akibat Cahaya Menyebabkan Penguapan Material?", "id": "Efek Ablasi Laser." },
  { "en": "Apa Akibat Cahaya Mendorong Dan Memanipulasi Partikel?", "id": "Efek Pinset Optik." },
  { "en": "Apa Akibat Elektron Berputar Dalam Medan Magnet?", "id": "Radiasi Siklotron." },
  { "en": "Apa Akibat Elektron Berkelok Dengan Kecepatan Tinggi?", "id": "Radiasi Synchrotron." },
  { "en": "Apa Akibat Atom T tereksitasi Memancarkan Foton?", "id": "Spektroskopi Emisi." },
  { "en": "Apa Akibat Atom Menyerap Foton Frekuensi Tertentu?", "id": "Spektroskopi Absorpsi." },
  { "en": "Apa Akibat Medan Magnet Mengubah Kapasitansi?", "id": "Efek Magnetokapasitansi." },
  { "en": "Apa Akibat Regangan Mekanis Mempengaruhi Sifat Magnetik?", "id": "Efek Villari." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Konduktivitas Termal?", "id": "Efek Righi-Leduc." },
  { "en": "Apa Akibat Spin Elektron Menghasilkan Gradien Suhu?", "id": "Efek Spin Seebeck." },
  { "en": "Apa Akibat Gradien Suhu Menghasilkan Arus Spin?", "id": "Efek Spin Peltier." },
  { "en": "Apa Akibat Medan Listrik Menghasilkan Arus Spin?", "id": "Efek Rashba-Edelstein." },
  { "en": "Apa Akibat Medan Magnet Mengubah Orientasi Spin Nuklir?", "id": "Resonansi Magnetik Nuklir (NMR)." },
  { "en": "Apa Akibat Kopling Cahaya Dan Eksiton Dalam Semikonduktor?", "id": "Polariton." },
  { "en": "Apa Akibat Pasangan Elektron-Lubang Terikat Dalam Semikonduktor?", "id": "Eksiton." },
  { "en": "Apa Akibat Interaksi Elektron Dengan Medan Vakum?", "id": "Efek Lamb Shift." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Rotasi Polarisasi?", "id": "Efek Elektro-girasi." },
  { "en": "Apa Akibat Regangan Mekanis Mempengaruhi Rotasi Polarisasi?", "id": "Efek Piezo-optik." },
  { "en": "Apa Akibat Suhu Mempengaruhi Rotasi Polarisasi Cahaya?", "id": "Efek Piro-optik." },
  { "en": "Apa Akibat Tumbukan Elektron Menghasilkan Elektron Lain?", "id": "Efek Auger." },
  { "en": "Apa Akibat Benda Berputar Dalam Aliran Fluida?", "id": "Efek Magnus." },
  { "en": "Apa Akibat Gelombang Suara Melebihi Kecepatan Suara?", "id": "Ledakan Sonik (Sonic Boom)." },
  { "en": "Apa Akibat Getaran Menyebar Melalui Benda Padat?", "id": "Gelombang Mekanis." },
  { "en": "Apa Akibat Energi Tersimpan Dalam Deformasi Elastis?", "id": "Energi Potensial Elastis." },
  { "en": "Apa Akibat Osilasi Berkurang Amplitudonya?", "id": "Efek Redaman (Damping)." },
  { "en": "Apa Akibat Gaya Luar Mendorong Osilasi Sistem?", "id": "Resonansi Paksa." },
  { "en": "Apa Akibat Dua Frekuensi Dekat Menghasilkan Pola?", "id": "Efek Layangan (Beats)." },
  { "en": "Apa Akibat Gelombang Berdiri Dalam Senar Bergetar?", "id": "Mode Harmonik." },
  { "en": "Apa Akibat Medan Listrik Kuat Menghasilkan Emisi?", "id": "Efek Schottky." },
  { "en": "Apa Akibat Medan Listrik Menghasilkan Aliran?", "id": "Efek Elektro-osmosis." },
  { "en": "Apa Akibat Partikel Bergerak Dalam Medan Magnet?", "id": "Gaya Lorentz." },
  { "en": "Apa Akibat Arus Listrik Menciptakan Medan Magnet?", "id": "Hukum Ampere." },
  { "en": "Apa Akibat Perubahan Fluks Magnetik Menciptakan?", "id": "Hukum Induksi Faraday." },
  { "en": "Apa Akibat Muatan Listrik Menciptakan Medan?", "id": "Hukum Gauss." },
  { "en": "Apa Akibat Medan Magnet Selalu Membentuk Loop?", "id": "Hukum Gauss Untuk Magnetisme." },
  { "en": "Apa Akibat Sinyal Kuat Mengurangi Sensitivitas?", "id": "Efek Desensitisasi." },
  { "en": "Apa Akibat Pencampuran Dua Sinyal Menciptakan?", "id": "Distorsi Intermodulasi." },
  { "en": "Apa Akibat Tegangan Tinggi Melompati Isolator?", "id": "Busur Api (Arc Over)." },
  { "en": "Apa Akibat Pelepasan Listrik Di Permukaan?", "id": "Flashover." },
  { "en": "Apa Akibat Kerusakan Internal Isolasi?", "id": "Breakdown." },
  { "en": "Apa Akibat Panas Merusak Isolasi?", "id": "Penuaan Termal (Thermal Aging)." },
  { "en": "Apa Akibat Tegangan Merusak Isolasi?", "id": "Penuaan Listrik (Electrical Aging)." },
  { "en": "Apa Akibat Lingkungan Merusak Isolasi?", "id": "Penuaan Lingkungan." },
  { "en": "Apa Akibat Elektron Terlepas Dari Atom?", "id": "Ionisasi." },
  { "en": "Apa Akibat Elektron Bergabung Kembali Dengan Ion?", "id": "Rekombinasi." },
  { "en": "Apa Akibat Medan Listrik Mempercepat Elektron?", "id": "Percepatan Elektron." },
  { "en": "Apa Akibat Tumbukan Elektron Menciptakan Elektron?", "id": "Ionisasi Tumbukan (Avalanche)." },
  { "en": "Apa Akibat Gas Menyala Pada Tegangan Tertentu?", "id": "Hukum Paschen." },
  { "en": "Apa Akibat Plasma Melindungi Dirinya Sendiri?", "id": "Pelindung Debye (Debye Shielding)." },
  { "en": "Apa Akibat Elektron Dalam Plasma Berosilasi?", "id": "Osilasi Plasma." },
  { "en": "Apa Akibat Medan Magnet Mengurung Plasma?", "id": "Pengurungan Magnetik." },
  { "en": "Apa Akibat Resonansi Siklotron Memanaskan Plasma?", "id": "Pemanasan Siklotron." },
  { "en": "Apa Akibat Medan Listrik Tinggi Di Ujung?", "id": "Peningkatan Medan Listrik." },
  { "en": "Apa Akibat Cahaya Berinteraksi Dengan Dirinya?", "id": "Efek Optik Non-linier." },
  { "en": "Apa Akibat Indeks Bias Bergantung Intensitas?", "id": "Efek Kerr Optik." },
  { "en": "Apa Akibat Sinar Cahaya Memfokuskan Dirinya?", "id": "Pemfokusan Diri (Self-Focusing)." },
  { "en": "Apa Akibat Spektrum Pulsa Melebar?", "id": "Modulasi Fasa Diri (SPM)." },
  { "en": "Apa Akibat Pulsa Berbeda Merambat Berbeda?", "id": "Modulasi Lintas Fasa (XPM)." },
  { "en": "Apa Akibat Dua Gelombang Menciptakan Gelombang?", "id": "Pencampuran Empat Gelombang (FWM)." },
  { "en": "Apa Akibat Cahaya Menghasilkan Pasangan Foton?", "id": "Generasi Parametrik." },
  { "en": "Apa Akibat Hamburan Cahaya Tidak Elastis?", "id": "Hamburan Raman Dan Brillouin." },
  { "en": "Apa Akibat Getaran Molekul Menyebarkan Cahaya?", "id": "Hamburan Raman." },
  { "en": "Apa Akibat Gelombang Akustik Menyebarkan Cahaya?", "id": "Hamburan Brillouin." },
  { "en": "Apa Akibat Sinar Terperangkap Dalam Pandu?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Akibat Mode Berbeda Bergerak Berbeda?", "id": "Dispersi Modal." },
  { "en": "Apa Akibat Warna Berbeda Bergerak Berbeda?", "id": "Dispersi Kromatik." },
  { "en": "Apa Akibat Polarisasi Berbeda Bergerak Berbeda?", "id": "Dispersi Mode Polarisasi (PMD)." },
  { "en": "Apa Akibat Ketidakmurnian Dalam Kisi Kristal?", "id": "Cacat Titik." },
  { "en": "Apa Akibat Barisan Atom Hilang?", "id": "Dislokasi." },
  { "en": "Apa Akibat Batas Antara Butiran Kristal?", "id": "Batas Butir." },
  { "en": "Apa Akibat Elektron Dan Lubang Terperangkap?", "id": "Pusat Rekombinasi." },
  { "en": "Apa Akibat Medan Magnet Rendah Mengubah Resistansi?", "id": "Osilasi Shubnikov-de Haas." },
  { "en": "Apa Akibat Medan Magnet Mengkuantisasi Konduktansi?", "id": "Efek Hall Kuantum." },
  { "en": "Apa Akibat Ketidakmurnian Magnetik Pada Logam?", "id": "Efek Kondo." },
  { "en": "Apa Akibat Gradien Suhu Menggerakkan Fonon?", "id": "Efek Seret Fonon (Phonon Drag)." },
  { "en": "Apa Akibat Medan Magnet Menghasilkan Arus Abadi?", "id": "Arus Persisten." },
  { "en": "Apa Akibat Fluks Magnetik Terkuantisasi?", "id": "Kuantisasi Fluks." },
  { "en": "Apa Akibat Superkonduktor Menolak Fluks Magnetik?", "id": "Penjepitan Fluks (Flux Pinning)." },
  { "en": "Apa Akibat Gelombang Terpantul Mengubah Frekuensi?", "id": "Efek Doppler." },
  { "en": "Apa Akibat Cahaya Memantul Dari Cermin?", "id": "Efek Doppler Relativistik." },
  { "en": "Apa Akibat Gravitasi Mengubah Frekuensi Cahaya?", "id": "Pergeseran Merah Gravitasi." },
  { "en": "Apa Akibat Ruang Waktu Melengkung Membelokkan?", "id": "Lensa Gravitasi." },
  { "en": "Apa Akibat Arus Tinggi Menghasilkan Medan?", "id": "Medan Magnet Azimuthal." },
  { "en": "Apa Akibat Arus Listrik Menghasilkan Gaya?", "id": "Gaya Elektromotif." },
  { "en": "Apa Akibat Muatan Statis Menghasilkan Gaya?", "id": "Gaya Elektrostatik." },
  { "en": "Apa Akibat Kutub Magnet Menghasilkan Gaya?", "id": "Gaya Magnetostatik." },
  { "en": "Apa Akibat Permukaan Memancarkan Elektron?", "id": "Kerja Fungsi (Work Function)." },
  { "en": "Apa Akibat Elektron Melewati Sambungan Logam?", "id": "Potensial Kontak." },
  { "en": "Apa Akibat Bahan Berbeda Menyatu?", "id": "Sambungan Hetero (Heterojunction)." },
  { "en": "Apa Akibat Sambungan Memancarkan Cahaya?", "id": "Dioda Pemancar Cahaya (LED)." },
  { "en": "Apa Akibat Sambungan Menghasilkan Laser?", "id": "Dioda Laser." },
  { "en": "Apa Akibat Sambungan Mendeteksi Cahaya?", "id": "Fotodioda." },
  { "en": "Apa Akibat Sambungan Menghasilkan Listrik?", "id": "Sel Surya." },
  { "en": "Apa Akibat Gerbang Mengontrol Aliran Arus?", "id": "Transistor Efek Medan." },
  { "en": "Apa Akibat Arus Kecil Mengontrol Arus?", "id": "Transistor Bipolar." },
  { "en": "Apa Akibat Panas Merusak Sambungan Semikonduktor?", "id": "Kegagalan Termal." },
  { "en": "Apa Akibat Tegangan Terbalik Merusak Dioda?", "id": "Breakdown Terbalik." },
  { "en": "Apa Akibat Arus Maju Berlebih Merusak?", "id": "Kegagalan Arus Maju." },
  { "en": "Apa Akibat Osilasi Parasitik Dalam Amplifier?", "id": "Ketidakstabilan." },
  { "en": "Apa Akibat Umpan Balik Positif?", "id": "Osilasi Atau Latching." },
  { "en": "Apa Akibat Umpan Balik Negatif?", "id": "Stabilisasi Dan Pengurangan Distorsi." },
  { "en": "Apa Akibat Input Melebihi Rentang Suplai?", "id": "Clipping." },
  { "en": "Apa Akibat Perubahan Sinyal Terlalu Cepat?", "id": "Distorsi Slew Rate." },
  { "en": "Apa Akibat Sinyal Menyeberang Di Tengah?", "id": "Distorsi Crossover." },
  { "en": "Apa Akibat Derau Membatasi Sensitivitas?", "id": "Dasar Derau (Noise Floor)." },
  { "en": "Apa Akibat Sinyal Non-linier Menghasilkan?", "id": "Distorsi Harmonik." },
  { "en": "Apa Akibat Sampling Sinyal Analog?", "id": "Aliasing." },
  { "en": "Apa Akibat Representasi Digital Terbatas?", "id": "Derau Kuantisasi." },
  { "en": "Apa Akibat Arus Mengalir Melalui Resistansi?", "id": "Pemanasan Joule." },
  { "en": "Apa Akibat Elektron Menghasilkan Medan Listrik?", "id": "Hukum Coulomb." },
  { "en": "Apa Akibat Arus Listrik Menghasilkan Medan?", "id": "Hukum Biot-Savart." },
  { "en": "Apa Akibat Tegangan Tinggi Di Udara?", "id": "Breakdown Udara." },
  { "en": "Apa Akibat Arus Listrik Mengionisasi Gas?", "id": "Plasma." },
  { "en": "Apa Akibat Elektron Panas Di Semikonduktor?", "id": "Efek Pembawa Panas." },
  { "en": "Apa Akibat Elektron Bergerak Balistik?", "id": "Transport Balistik." },
  { "en": "Apa Akibat Sinyal Terdistorsi Saat Merambat?", "id": "Dispersi." },
  { "en": "Apa Akibat Sinyal Kehilangan Amplitudo?", "id": "Atenuasi." },
  { "en": "Apa Akibat Arus Menciptakan Gaya Mekanis?", "id": "Prinsip Motor." },
  { "en": "Apa Akibat Gerakan Menciptakan Arus?", "id": "Prinsip Generator." },
  { "en": "Apa Akibat Getaran Mengganggu Sinyal?", "id": "Efek Mikrofonik." },
  { "en": "Apa Akibat Medan Magnet Mengubah Resistansi?", "id": "Efek Wiegand." },
  { "en": "Apa Akibat Suara Merambat Dalam Padatan?", "id": "Gelombang Akustik." },
  { "en": "Apa Akibat Gelombang Permukaan Merambat?", "id": "Gelombang Akustik Permukaan (SAW)." },
  { "en": "Apa Akibat Gelombang Akustik Melalui Material?", "id": "Gelombang Akustik Bulk (BAW)." },
  { "en": "Apa Akibat Medan Listrik Tinggi Mengubah?", "id": "Kerusakan Dielektrik." },
  { "en": "Apa Akibat Panas Merusak Isolasi?", "id": "Penuaan Termal (Thermal Aging)." },
  { "en": "Apa Akibat Tegangan Merusak Isolasi?", "id": "Penuaan Listrik (Electrical Aging)." },
  { "en": "Apa Akibat Elektron Terlepas Dari Atom?", "id": "Ionisasi." },
  { "en": "Apa Akibat Elektron Bergabung Kembali Dengan Ion?", "id": "Rekombinasi." },
  { "en": "Apa Akibat Medan Listrik Mempercepat Elektron?", "id": "Percepatan Elektron." },
  { "en": "Apa Akibat Tumbukan Elektron Menciptakan Elektron?", "id": "Ionisasi Tumbukan (Avalanche)." },
  { "en": "Apa Akibat Gas Menyala Pada Tegangan Tertentu?", "id": "Hukum Paschen." },
  { "en": "Apa Akibat Plasma Melindungi Dirinya Sendiri?", "id": "Pelindung Debye (Debye Shielding)." },
  { "en": "Apa Akibat Elektron Dalam Plasma Berosilasi?", "id": "Osilasi Plasma." },
  { "en": "Apa Akibat Medan Magnet Mengurung Plasma?", "id": "Pengurungan Magnetik." },
  { "en": "Apa Akibat Resonansi Siklotron Memanaskan Plasma?", "id": "Pemanasan Siklotron." },
  { "en": "Apa Akibat Medan Listrik Tinggi Di Ujung?", "id": "Peningkatan Medan Listrik." },
  { "en": "Apa Akibat Cahaya Berinteraksi Dengan Dirinya?", "id": "Efek Optik Non-linier." },
  { "en": "Apa Akibat Indeks Bias Bergantung Intensitas?", "id": "Efek Kerr Optik." },
  { "en": "Apa Akibat Sinar Cahaya Memfokuskan Dirinya?", "id": "Pemfokusan Diri (Self-Focusing)." },
  { "en": "Apa Akibat Spektrum Pulsa Melebar?", "id": "Modulasi Fasa Diri (SPM)." },
  { "en": "Apa Akibat Pulsa Berbeda Merambat Berbeda?", "id": "Modulasi Lintas Fasa (XPM)." },
  { "en": "Apa Akibat Dua Gelombang Menciptakan Gelombang?", "id": "Pencampuran Empat Gelombang (FWM)." },
  { "en": "Apa Akibat Cahaya Menghasilkan Pasangan Foton?", "id": "Generasi Parametrik." },
  { "en": "Apa Akibat Hamburan Cahaya Tidak Elastis?", "id": "Hamburan Raman Dan Brillouin." },
  { "en": "Apa Akibat Getaran Molekul Menyebarkan Cahaya?", "id": "Hamburan Raman." },
  { "en": "Apa Akibat Gelombang Akustik Menyebarkan Cahaya?", "id": "Hamburan Brillouin." },
  { "en": "Apa Akibat Sinar Terperangkap Dalam Pandu?", "id": "Pemantulan Internal Total." },
  { "en": "Apa Akibat Mode Berbeda Bergerak Berbeda?", "id": "Dispersi Modal." },
  { "en": "Apa Akibat Warna Berbeda Bergerak Berbeda?", "id": "Dispersi Kromatik." },
  { "en": "Apa Akibat Polarisasi Berbeda Bergerak Berbeda?", "id": "Dispersi Mode Polarisasi (PMD)." },
  { "en": "Apa Akibat Ketidakmurnian Dalam Kisi Kristal?", "id": "Cacat Titik." },
  { "en": "Apa Akibat Barisan Atom Hilang?", "id": "Dislokasi." },
  { "en": "Apa Akibat Batas Antara Butiran Kristal?", "id": "Batas Butir." },
  { "en": "Apa Akibat Elektron Dan Lubang Terperangkap?", "id": "Pusat Rekombinasi." },
  { "en": "Apa Akibat Medan Magnet Rendah Mengubah Resistansi?", "id": "Osilasi Shubnikov-de Haas." },
  { "en": "Apa Akibat Medan Magnet Mengkuantisasi Konduktansi?", "id": "Efek Hall Kuantum." },
  { "en": "Apa Akibat Ketidakmurnian Magnetik Pada Logam?", "id": "Efek Kondo." },
  { "en": "Apa Akibat Gradien Suhu Menggerakkan Fonon?", "id": "Efek Seret Fonon (Phonon Drag)." },
  { "en": "Apa Akibat Medan Magnet Menghasilkan Arus Abadi?", "id": "Arus Persisten." },
  { "en": "Apa Akibat Fluks Magnetik Terkuantisasi?", "id": "Kuantisasi Fluks." },
  { "en": "Apa Akibat Superkonduktor Menolak Fluks Magnetik?", "id": "Penjepitan Fluks (Flux Pinning)." },
  { "en": "Apa Akibat Gelombang Terpantul Mengubah Frekuensi?", "id": "Efek Doppler." },
  { "en": "Apa Akibat Cahaya Memantul Dari Cermin?", "id": "Efek Doppler Relativistik." },
  { "en": "Apa Akibat Gravitasi Mengubah Frekuensi Cahaya?", "id": "Pergeseran Merah Gravitasi." },
  { "en": "Apa Akibat Ruang Waktu Melengkung Membelokkan?", "id": "Lensa Gravitasi." },
  { "en": "Apa Akibat Arus Tinggi Menghasilkan Medan?", "id": "Medan Magnet Azimuthal." },
  { "en": "Apa Akibat Arus Listrik Menghasilkan Gaya?", "id": "Gaya Elektromotif." },
  { "en": "Apa Akibat Muatan Statis Menghasilkan Gaya?", "id": "Gaya Elektrostatik." },
  { "en": "Apa Akibat Kutub Magnet Menghasilkan Gaya?", "id": "Gaya Magnetostatik." },
  { "en": "Apa Akibat Permukaan Memancarkan Elektron?", "id": "Kerja Fungsi (Work Function)." },
  { "en": "Apa Akibat Elektron Melewati Sambungan Logam?", "id": "Potensial Kontak." },
  { "en": "Apa Akibat Bahan Berbeda Menyatu?", "id": "Sambungan Hetero (Heterojunction)." },
  { "en": "Apa Akibat Sambungan Memancarkan Cahaya?", "id": "Dioda Pemancar Cahaya (LED)." },
  { "en": "Apa Akibat Sambungan Menghasilkan Laser?", "id": "Dioda Laser." },
  { "en": "Apa Akibat Sambungan Mendeteksi Cahaya?", "id": "Fotodioda." },
  { "en": "Apa Akibat Sambungan Menghasilkan Listrik?", "id": "Sel Surya." },
  { "en": "Apa Akibat Gerbang Mengontrol Aliran Arus?", "id": "Transistor Efek Medan." },
  { "en": "Apa Akibat Arus Kecil Mengontrol Arus?", "id": "Transistor Bipolar." },
  { "en": "Apa Akibat Panas Merusak Sambungan Semikonduktor?", "id": "Kegagalan Termal." },
  { "en": "Apa Akibat Tegangan Terbalik Merusak Dioda?", "id": "Breakdown Terbalik." },
  { "en": "Apa Akibat Arus Maju Berlebih Merusak?", "id": "Kegagalan Arus Maju." },
  { "en": "Apa Akibat Osilasi Parasitik Dalam Amplifier?", "id": "Ketidakstabilan." },
  { "en": "Apa Akibat Umpan Balik Positif?", "id": "Osilasi Atau Latching." },
  { "en": "Apa Akibat Umpan Balik Negatif?", "id": "Stabilisasi Dan Pengurangan Distorsi." },
  { "en": "Apa Akibat Input Melebihi Rentang Suplai?", "id": "Clipping." },
  { "en": "Apa Akibat Perubahan Sinyal Terlalu Cepat?", "id": "Distorsi Slew Rate." },
  { "en": "Apa Akibat Sinyal Menyeberang Di Tengah?", "id": "Distorsi Crossover." },
  { "en": "Apa Akibat Derau Membatasi Sensitivitas?", "id": "Dasar Derau (Noise Floor)." },
  { "en": "Apa Akibat Sinyal Non-linier Menghasilkan?", "id": "Distorsi Harmonik." },
  { "en": "Apa Akibat Sampling Sinyal Analog?", "id": "Aliasing." },
  { "en": "Apa Akibat Representasi Digital Terbatas?", "id": "Derau Kuantisasi." },
  { "en": "Apa Akibat Elektron Menghasilkan Medan Listrik?", "id": "Hukum Coulomb." },
  { "en": "Apa Akibat Arus Listrik Menghasilkan Medan?", "id": "Hukum Biot-Savart." },
  { "en": "Apa Akibat Tegangan Tinggi Di Udara?", "id": "Breakdown Udara." },
  { "en": "Apa Akibat Arus Listrik Mengionisasi Gas?", "id": "Plasma." },
  { "en": "Apa Akibat Elektron Panas Di Semikonduktor?", "id": "Efek Pembawa Panas." },
  { "en": "Apa Akibat Elektron Bergerak Balistik?", "id": "Transport Balistik." },
  { "en": "Apa Akibat Sinyal Terdistorsi Saat Merambat?", "id": "Dispersi." },
  { "en": "Apa Akibat Sinyal Kehilangan Amplitudo?", "id": "Atenuasi." },
  { "en": "Apa Akibat Arus Menciptakan Gaya Mekanis?", "id": "Prinsip Motor." },
  { "en": "Apa Akibat Gerakan Menciptakan Arus?", "id": "Prinsip Generator." },
  { "en": "Apa Akibat Getaran Mengganggu Sinyal?", "id": "Efek Mikrofonik." },
  { "en": "Apa Akibat Medan Magnet Mengubah Resistansi?", "id": "Efek Wiegand." },
  { "en": "Apa Akibat Suara Merambat Dalam Padatan?", "id": "Gelombang Akustik." },
  { "en": "Apa Akibat Gelombang Permukaan Merambat?", "id": "Gelombang Akustik Permukaan (SAW)." },
  { "en": "Apa Akibat Gelombang Akustik Melalui Material?", "id": "Gelombang Akustik Bulk (BAW)." },
  { "en": "Apa Akibat Medan Listrik Menghasilkan Aliran?", "id": "Efek Elektro-osmosis." },
  { "en": "Apa Akibat Partikel Bergerak Dalam Medan Magnet?", "id": "Gaya Lorentz." },
  { "en": "Apa Akibat Perubahan Fluks Magnetik Menciptakan?", "id": "Hukum Induksi Faraday." },
  { "en": "Apa Akibat Muatan Listrik Menciptakan Medan?", "id": "Hukum Gauss." },
  { "en": "Apa Akibat Medan Magnet Selalu Membentuk Loop?", "id": "Hukum Gauss Untuk Magnetisme." },
  { "en": "Apa Akibat Sinyal Kuat Mengurangi Sensitivitas?", "id": "Efek Desensitisasi." },
  { "en": "Apa Akibat Pencampuran Dua Sinyal Menciptakan?", "id": "Distorsi Intermodulasi." },
  { "en": "Apa Akibat Tegangan Tinggi Melompati Isolator?", "id": "Busur Api (Arc Over)." },
  { "en": "Apa Akibat Pelepasan Listrik Di Permukaan?", "id": "Flashover." },
  { "en": "Apa Akibat Kerusakan Internal Isolasi?", "id": "Breakdown." },
  { "en": "Apa Akibat Panas Merusak Isolasi?", "id": "Penuaan Termal (Thermal Aging)." },
  { "en": "Apa Akibat Tegangan Merusak Isolasi?", "id": "Penuaan Listrik (Electrical Aging)." },
  { "en": "Apa Akibat Lingkungan Merusak Isolasi?", "id": "Penuaan Lingkungan." },
  { "en": "Apa Akibat Arus Tinggi Melelehkan Sekering?", "id": "Efek Peleburan (Fusing)." },
  { "en": "Apa Akibat Rekombinasi Memancarkan Cahaya?", "id": "Rekombinasi Radiatif." },
  { "en": "Apa Akibat Rekombinasi Menghasilkan Panas?", "id": "Rekombinasi Non-Radiatif." },
  { "en": "Apa Akibat Medan Listrik Mempercepat Pembawa?", "id": "Drift Pembawa Muatan." },
  { "en": "Apa Akibat Gradien Konsentrasi Menyebabkan?", "id": "Difusi Pembawa Muatan." },
  { "en": "Apa Akibat Pembawa Terperangkap Dalam Cacat?", "id": "Penangkapan Pembawa." },
  { "en": "Apa Akibat Getaran Molekul Menyerap Radiasi?", "id": "Penyerapan Inframerah." },
  { "en": "Apa Akibat Elektron Berpindah Tingkat Energi?", "id": "Penyerapan Atom." },
  { "en": "Apa Akibat Dua Pelat Konduktif Di Vakum?", "id": "Efek Casimir." },
  { "en": "Apa Akibat Partikel Melewati Potensial Vektor?", "id": "Efek Aharonov-Bohm." },
  { "en": "Apa Akibat Fluktuasi Kuantum Di Ruang Hampa?", "id": "Energi Titik Nol." },
  { "en": "Apa Akibat Elektron Berinteraksi Dengan Vakuo?", "id": "Efek Lamb Shift." },
  { "en": "Apa Akibat Medan Listrik Kuat Menghasilkan?", "id": "Efek Schwinger." },
  { "en": "Apa Akibat Medan Gravitasi Membelokkan Cahaya?", "id": "Lensa Gravitasi." },
  { "en": "Apa Akibat Gravitasi Mempengaruhi Waktu?", "id": "Dilatasi Waktu Gravitasi." },
  { "en": "Apa Akibat Kecepatan Mempengaruhi Waktu?", "id": "Dilatasi Waktu Relativistik." },
  { "en": "Apa Akibat Gerakan Cepat Mengubah Panjang?", "id": "Kontraksi Lorentz." },
  { "en": "Apa Akibat Muatan Bergerak Menghasilkan Potensial?", "id": "Potensial Liénard-Wiechert." },
  { "en": "Apa Akibat Getaran Kisi Kristal Terkuantisasi?", "id": "Fonon." },
  { "en": "Apa Akibat Osilasi Spin Kolektif?", "id": "Magnon." },
  { "en": "Apa Akibat Kopling Kuat Foton-Eksiton?", "id": "Polariton." },
  { "en": "Apa Akibat Elektron Berpakaian Fonon?", "id": "Polaron." },
  { "en": "Apa Akibat Pasangan Elektron Terikat Fonon?", "id": "Pasangan Cooper." },
  { "en": "Apa Akibat Konduktansi Terkuantisasi Dalam Kawat?", "id": "Konduktansi Kuantum." },
  { "en": "Apa Akibat Fluktuasi Konduktansi Universal?", "id": "Universal Conductance Fluctuations (UCF)." },
  { "en": "Apa Akibat Arus Abadi Dalam Cincin?", "id": "Arus Persisten." },
  { "en": "Apa Akibat Pemblokiran Arus Karena Muatan?", "id": "Blokade Coulomb." },
  { "en": "Apa Akibat Medan Listrik Menembus Superkonduktor?", "id": "Penetrasi Medan Listrik." },
  { "en": "Apa Akibat Medan Magnet Menembus Superkonduktor?", "id": "Kedalaman Penetrasi London." },
  { "en": "Apa Akibat Arus AC Mengalir Tanpa Disipasi?", "id": "Efek Josephson AC." },
  { "en": "Apa Akibat Dua Superkonduktor Digabungkan?", "id": "Sambungan Josephson." },
  { "en": "Apa Akibat Medan Listrik Membuka Pori?", "id": "Elektroporasi." },
  { "en": "Apa Akibat Pelepasan Listrik Menghasilkan Angin?", "id": "Angin Ionik (Ionic Wind)." },
  { "en": "Apa Akibat Parameter Sistem Berubah Tiba-tiba?", "id": "Bifurkasi." },
  { "en": "Apa Akibat Sistem Menunjukkan Keteraturan Aneh?", "id": "Penarik Aneh (Strange Attractor)." },
  { "en": "Apa Akibat Sistem Menggandakan Periodenya?", "id": "Kaskade Penggandaan Periode." },
  { "en": "Apa Akibat Regangan Menghasilkan Polarisasi?", "id": "Efek Flexoelektrik." },
  { "en": "Apa Akibat Permukaan Material Menjadi Konduktif?", "id": "Isolator Topologis." },
  { "en": "Apa Akibat Arus Mengalir Tanpa Disipasi?", "id": "Transport Balistik." },
  { "en": "Apa Akibat Elektron Menghasilkan Panas?", "id": "Efek Termionik." },
  { "en": "Apa Akibat Perubahan Entropi Menghasilkan Tegangan?", "id": "Efek Seebeck." },
  { "en": "Apa Akibat Tegangan Menyebabkan Perubahan Entropi?", "id": "Efek Peltier." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Entropi?", "id": "Efek Magnetokalorik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Entropi?", "id": "Efek Elektrokalorik." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Aliran?", "id": "Gaya Magnetohidrodinamik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Aliran?", "id": "Gaya Elektrohidrodinamik." },
  { "en": "Apa Akibat Medan Listrik Tinggi Mengubah?", "id": "Modulasi Diri Fasa." },
  { "en": "Apa Akibat Cahaya Mengubah Sifat Mekanis?", "id": "Efek Fotostriksi." },
  { "en": "Apa Akibat Dua Laser Menciptakan Kisi?", "id": "Kisi Optik." },
  { "en": "Apa Akibat Atom Terperangkap Dalam Kisi?", "id": "Pendinginan Samping Raman." },
  { "en": "Apa Akibat Gelombang Berdiri Mendinginkan Atom?", "id": "Pendinginan Sisyphus." },
  { "en": "Apa Akibat Cahaya Mendorong Atom?", "id": "Tekanan Radiasi." },
  { "en": "Apa Akibat Medan Magnet Memperlambat Atom?", "id": "Pelambat Zeeman." },
  { "en": "Apa Akibat Arus Listrik Menciptakan Ledakan?", "id": "Ledakan Kawat Listrik." },
  { "en": "Apa Akibat Arus Tinggi Menggerakkan Rel?", "id": "Railgun." },
  { "en": "Apa Akibat Arus Tinggi Menghasilkan Medan?", "id": "Coilgun." },
  { "en": "Apa Akibat Muatan Bergerak Di Udara?", "id": "Busur Api (Electric Arc)." },
  { "en": "Apa Akibat Pelepasan Listrik Di Vakum?", "id": "Busur Vakum." },
  { "en": "Apa Akibat Pelepasan Listrik Di Cairan?", "id": "Busur Cairan." },
  { "en": "Apa Akibat Sambungan Logam Berbeda Dipanaskan?", "id": "Efek Seebeck." },
  { "en": "Apa Akibat Arus Melewati Sambungan Logam?", "id": "Efek Peltier." },
  { "en": "Apa Akibat Panas Mengalir Di Konduktor?", "id": "Efek Thomson." },
  { "en": "Apa Akibat Foton Menumbuk Elektron Diam?", "id": "Efek Compton." },
  { "en": "Apa Akibat Elektron Menumbuk Foton?", "id": "Efek Compton Terbalik." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Bahan?", "id": "Efek Piezoresistif." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Sifat?", "id": "Efek Elektro-optik Kerr." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Indeks?", "id": "Efek Pockels." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Resistansi?", "id": "Efek Magnetoresistansi." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Resistansi?", "id": "Efek Elektroresistansi." },
  { "en": "Apa Akibat Tekanan Mempengaruhi Resistansi?", "id": "Efek Piezoresistansi." },
  { "en": "Apa Akibat Cahaya Mempengaruhi Resistansi?", "id": "Efek Fotokonduktif." },
  { "en": "Apa Akibat Panas Mempengaruhi Resistansi?", "id": "Efek Termoresistif." },
  { "en": "Apa Akibat Suara Mempengaruhi Resistansi?", "id": "Efek Akustoresistif." },
  { "en": "Apa Akibat Cahaya Menghasilkan Medan Magnet?", "id": "Efek Faraday Terbalik." },
  { "en": "Apa Akibat Cahaya Mempengaruhi Sifat Kimia?", "id": "Efek Fotokimia." },
  { "en": "Apa Akibat Suara Mempengaruhi Sifat Kimia?", "id": "Efek Sonokimia." },
  { "en": "Apa Akibat Listrik Mempengaruhi Sifat Kimia?", "id": "Efek Elektrokimia." },
  { "en": "Apa Akibat Panas Mempengaruhi Sifat Kimia?", "id": "Efek Termokimia." },
  { "en": "Apa Akibat Tekanan Mempengaruhi Sifat Kimia?", "id": "Efek Piezokimia." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Sifat?", "id": "Efek Magnetokimia." },
  { "en": "Apa Akibat Cahaya Menyebabkan Polimerisasi?", "id": "Efek Fotopolimerisasi." },
  { "en": "Apa Akibat Cahaya Menyebabkan Depolimerisasi?", "id": "Efek Fotodepolimerisasi." },
  { "en": "Apa Akibat Cahaya Menyebabkan Oksidasi?", "id": "Efek Foto-oksidasi." },
  { "en": "Apa Akibat Cahaya Menyebabkan Reduksi?", "id": "Efek Foto-reduksi." },
  { "en": "Apa Akibat Elektron Menghasilkan Cahaya?", "id": "Efek Katodoluminesensi." },
  { "en": "Apa Akibat Panas Menghasilkan Cahaya?", "id": "Efek Termoluminesensi." },
  { "en": "Apa Akibat Gesekan Menghasilkan Cahaya?", "id": "Efek Triboluminesensi." },
  { "en": "Apa Akibat Suara Menghasilkan Cahaya?", "id": "Efek Sonoluminesensi." },
  { "en": "Apa Akibat Medan Listrik Menghasilkan Cahaya?", "id": "Efek Elektroluminesensi." },
  { "en": "Apa Akibat Reaksi Kimia Menghasilkan Cahaya?", "id": "Efek Kemiluminesensi." },
  { "en": "Apa Akibat Organisme Menghasilkan Cahaya?", "id": "Efek Bioluminesensi." },
  { "en": "Apa Akibat Kristalisasi Menghasilkan Cahaya?", "id": "Efek Kristaloluminesensi." },
  { "en": "Apa Akibat Bahan Menyerap Radiasi?", "id": "Efek Radioluminesensi." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Reaksi?", "id": "Efek Elektrokimia." },
  { "en": "Apa Akibat Medan Magnet Mempengaruhi Arus?", "id": "Efek Magnetohidrodinamika." },
  { "en": "Apa Akibat Medan Listrik Mempengaruhi Fluida?", "id": "Efek Elektrohidrodinamika." },
  { "en": "Apa Akibat Permukaan Panas Memancarkan Elektron?", "id": "Efek Emisi Termionik." },
  { "en": "Apa Akibat Elektron Menumbuk Permukaan?", "id": "Efek Emisi Sekunder." },
  { "en": "Apa Akibat Medan Listrik Menarik Elektron?", "id": "Efek Emisi Medan." },
  { "en": "Apa Akibat Cahaya Menarik Elektron?", "id": "Efek Emisi Foto." },
  { "en": "Apa Akibat Foton Menghasilkan Pasangan?", "id": "Efek Produksi Pasangan." },
  { "en": "Apa Akibat Elektron Dan Positron Bertemu?", "id": "Efek Anihilasi." },
  { "en": "Apa Akibat Gelombang Tersebar Oleh Elektron?", "id": "Hamburan Thomson." },
  { "en": "Apa Akibat Cahaya Terhambur Oleh Atmosfer?", "id": "Efek Langit Biru." },
  { "en": "Apa Akibat Cahaya Terhambur Saat Matahari?", "id": "Efek Matahari Terbenam Merah." }



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
