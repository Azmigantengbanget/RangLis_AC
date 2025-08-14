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



  { "en": "Apa kepanjangan dari AC?", "id": "Alternating Current atau Arus Bolak-balik." },
  { "en": "Apa itu arus AC?", "id": "Arus yang arahnya berubah-ubah periodik." },
  { "en": "Bentuk gelombang AC paling umum?", "id": "Bentuk gelombang sinusoidal (sinus wave)." },
  { "en": "Apa itu frekuensi (f)?", "id": "Jumlah siklus per detik." },
  { "en": "Apa satuan dari frekuensi?", "id": "Hertz (Hz) adalah satuan frekuensi." },
  { "en": "Apa itu periode (T)?", "id": "Waktu untuk menyelesaikan satu siklus penuh." },
  { "en": "Apa satuan dari periode?", "id": "Detik (second) adalah satuan periode." },
  { "en": "Apa hubungan frekuensi dan periode?", "id": "Frekuensi adalah kebalikan dari periode (f=1/T)." },
  { "en": "Apa itu amplitudo?", "id": "Nilai puncak atau maksimum dari gelombang." },
  { "en": "Apa itu tegangan puncak (peak voltage)?", "id": "Nilai tegangan maksimum pada gelombang." },
  { "en": "Apa itu tegangan puncak-ke-puncak (peak-to-peak)?", "id": "Selisih antara puncak positif dan negatif." },
  { "en": "Frekuensi standar PLN di Indonesia?", "id": "Frekuensi standarnya adalah 50 Hertz." },
  { "en": "Apa itu nilai sesaat (instantaneous value)?", "id": "Nilai tegangan/arus pada waktu tertentu." },
  { "en": "Apa itu nilai RMS?", "id": "Root Mean Square atau nilai efektif." },
  { "en": "Apa arti fisis dari nilai RMS?", "id": "Nilai DC ekuivalen yang hasilkan panas sama." },
  { "en": "Hubungan RMS dan tegangan puncak (Vp)?", "id": "Vrms sama dengan Vp dibagi akar dua." },
  { "en": "Tegangan 220V dari PLN itu nilai apa?", "id": "Itu adalah nilai tegangan RMS." },
  { "en": "Berapa tegangan puncak PLN jika RMS 220V?", "id": "Sekitar 311 Volt puncak." },
  { "en": "Apa itu nilai rata-rata (average value)?", "id": "Nilai rata-rata gelombang selama satu periode." },
  { "en": "Berapa nilai rata-rata gelombang sinus murni?", "id": "Nilai rata-ratanya adalah nol." },
  { "en": "Apa itu fasa (phase)?", "id": "Posisi relatif gelombang terhadap waktu." },
  { "en": "Apa itu beda fasa (phase difference)?", "id": "Perbedaan sudut fasa antara dua gelombang." },
  { "en": "Apa artinya dua gelombang sefasa?", "id": "Puncak dan lembahnya terjadi bersamaan." },
  { "en": "Apa artinya gelombang 'leading'?", "id": "Gelombang yang mendahului gelombang referensi." },
  { "en": "Apa artinya gelombang 'lagging'?", "id": "Gelombang yang tertinggal dari gelombang referensi." },
  { "en": "Berapa derajat beda fasa 'leading' 90°?", "id": "Mendahului sebesar 90 derajat." },
  { "en": "Apa itu fasor (phasor)?", "id": "Vektor untuk merepresentasikan gelombang sinus." },
  { "en": "Apa yang direpresentasikan panjang fasor?", "id": "Panjang fasor merepresentasikan amplitudo (RMS)." },
  { "en": "Apa yang direpresentasikan sudut fasor?", "id": "Sudut fasor merepresentasikan sudut fasa." },
  { "en": "Apa itu impedansi (Z)?", "id": "Hambatan total pada rangkaian AC." },
  { "en": "Apa satuan dari impedansi?", "id": "Ohm (Ω) adalah satuan impedansi." },
  { "en": "Komponen apa saja yang membentuk impedansi?", "id": "Resistansi dan reaktansi." },
  { "en": "Apa itu resistansi (R)?", "id": "Hambatan murni, tidak bergantung frekuensi." },
  { "en": "Apa itu reaktansi (X)?", "id": "Hambatan akibat kapasitor atau induktor." },
  { "en": "Apa satuan dari reaktansi?", "id": "Ohm (Ω) juga satuan reaktansi." },
  { "en": "Apa itu reaktansi induktif (XL)?", "id": "Reaktansi yang disebabkan oleh induktor." },
  { "en": "Apa rumus reaktansi induktif?", "id": "XL sama dengan 2π kali f kali L." },
  { "en": "Bagaimana hubungan XL dengan frekuensi?", "id": "Reaktansi induktif sebanding dengan frekuensi." },
  { "en": "Pada frekuensi tinggi, XL bagaimana?", "id": "Nilai reaktansi induktifnya sangat besar." },
  { "en": "Pada frekuensi DC (0 Hz), XL bagaimana?", "id": "Nilai reaktansi induktifnya nol (hubung singkat)." },
  { "en": "Apa itu reaktansi kapasitif (XC)?", "id": "Reaktansi yang disebabkan oleh kapasitor." },
  { "en": "Apa rumus reaktansi kapasitif?", "id": "XC sama dengan 1 dibagi (2πfC)." },
  { "en": "Bagaimana hubungan XC dengan frekuensi?", "id": "Reaktansi kapasitif berbanding terbalik frekuensi." },
  { "en": "Pada frekuensi tinggi, XC bagaimana?", "id": "Nilai reaktansi kapasitifnya sangat kecil." },
  { "en": "Pada frekuensi DC (0 Hz), XC bagaimana?", "id": "Nilai reaktansinya tak terhingga (terbuka)." },
  { "en": "Apa hubungan fasa V dan I di resistor?", "id": "Tegangan dan arus pada resistor sefasa." },
  { "en": "Apa hubungan fasa V dan I di induktor?", "id": "Tegangan mendahului arus 90 derajat." },
  { "en": "Apa hubungan fasa V dan I di kapasitor?", "id": "Arus mendahului tegangan 90 derajat." },
  { "en": "Mnemonic untuk fasa induktor?", "id": "ELI: E (tegangan) mendahului I di L." },
  { "en": "Mnemonic untuk fasa kapasitor?", "id": "ICE: I (arus) mendahului E di C." },
  { "en": "Apa itu segitiga impedansi?", "id": "Representasi grafis dari R, X, dan Z." },
  { "en": "Sisi datar segitiga impedansi?", "id": "Sisi datar adalah resistansi (R)." },
  { "en": "Sisi tegak segitiga impedansi?", "id": "Sisi tegak adalah reaktansi (X)." },
  { "en": "Sisi miring segitiga impedansi?", "id": "Sisi miring adalah impedansi (Z)." },
  { "en": "Rumus impedansi dari segitiga?", "id": "Z kuadrat sama dengan R kuadrat + X kuadrat." },
  { "en": "Apa itu admintansi (Y)?", "id": "Kebalikan dari impedansi (Y = 1/Z)." },
  { "en": "Apa satuan dari admintansi?", "id": "Siemens (S) adalah satuan admintansi." },
  { "en": "Apa itu konduktansi (G)?", "id": "Bagian riil dari admintansi." },
  { "en": "Apa itu suseptansi (B)?", "id": "Bagian imajiner dari admintansi." },
  { "en": "Apa itu bilangan kompleks?", "id": "Bilangan dengan bagian riil dan imajiner." },
  { "en": "Mengapa bilangan kompleks digunakan di AC?", "id": "Untuk merepresentasikan fasor (magnitudo dan fasa)." },
  { "en": "Apa bentuk rektangular bilangan kompleks?", "id": "Bentuk z sama dengan a + jb." },
  { "en": "Apa bentuk polar bilangan kompleks?", "id": "Bentuk z sama dengan r sudut θ." },
  { "en": "Apa arti 'j' dalam bilangan kompleks?", "id": "Operator imajiner, akar dari -1." },
  { "en": "Bagaimana impedansi resistor dalam bentuk kompleks?", "id": "Z sama dengan R (hanya riil)." },
  { "en": "Bagaimana impedansi induktor dalam bentuk kompleks?", "id": "Z sama dengan jωL atau jXL." },
  { "en": "Bagaimana impedansi kapasitor dalam bentuk kompleks?", "id": "Z sama dengan -j/ωC atau -jXC." },
  { "en": "Apa itu 'ω' (omega)?", "id": "Frekuensi sudut (angular frequency)." },
  { "en": "Apa rumus dari frekuensi sudut (ω)?", "id": "Omega sama dengan 2π kali f." },
  { "en": "Apa satuan dari frekuensi sudut?", "id": "Radian per detik (rad/s)." },
  { "en": "Hukum Ohm untuk rangkaian AC?", "id": "V sama dengan I kali Z (semua fasor)." },
  { "en": "Bagaimana cara menjumlahkan impedansi seri?", "id": "Jumlahkan saja semua impedansinya (Z1+Z2)." },
  { "en": "Bagaimana cara menjumlahkan admintansi paralel?", "id": "Jumlahkan saja semua admintansinya (Y1+Y2)." },
  { "en": "Apa itu rangkaian RLC seri?", "id": "Resistor, Induktor, Kapasitor dihubungkan seri." },
  { "en": "Apa itu daya sesaat (instantaneous power)?", "id": "Daya pada suatu waktu tertentu (p(t))." },
  { "en": "Apa itu daya rata-rata (P)?", "id": "Daya nyata yang dikonsumsi oleh rangkaian." },
  { "en": "Apa satuan daya rata-rata?", "id": "Watt (W) adalah satuan daya rata-rata." },
  { "en": "Komponen apa yang menyerap daya rata-rata?", "id": "Hanya resistor yang menyerap daya nyata." },
  { "en": "Berapa daya rata-rata di induktor ideal?", "id": "Daya rata-ratanya adalah nol." },
  { "en": "Berapa daya rata-rata di kapasitor ideal?", "id": "Daya rata-ratanya adalah nol." },
  { "en": "Apa itu daya reaktif (Q)?", "id": "Daya yang disimpan dan dikembalikan oleh reaktansi." },
  { "en": "Apa satuan daya reaktif?", "id": "Volt-Ampere Reactive (VAR)." },
  { "en": "Daya reaktif induktor positif atau negatif?", "id": "Daya reaktif induktor bernilai positif." },
  { "en": "Daya reaktif kapasitor positif atau negatif?", "id": "Daya reaktif kapasitor bernilai negatif." },
  { "en": "Apa itu daya semu (S)?", "id": "Kombinasi vektor dari daya nyata dan reaktif." },
  { "en": "Apa satuan daya semu?", "id": "Volt-Ampere (VA) adalah satuan daya semu." },
  { "en": "Apa itu segitiga daya?", "id": "Representasi grafis dari P, Q, dan S." },
  { "en": "Sisi datar segitiga daya?", "id": "Daya nyata atau daya aktif (P)." },
  { "en": "Sisi tegak segitiga daya?", "id": "Daya reaktif (Q)." },
  { "en": "Sisi miring segitiga daya?", "id": "Daya semu (S)." },
  { "en": "Apa itu faktor daya (power factor)?", "id": "Rasio daya nyata terhadap daya semu (P/S)." },
  { "en": "Rumus lain faktor daya?", "id": "Cosinus dari sudut fasa (cos θ)." },
  { "en": "Berapa nilai faktor daya ideal?", "id": "Nilai idealnya adalah 1 (satu)." },
  { "en": "Faktor daya 'leading' artinya apa?", "id": "Arus mendahului tegangan (beban kapasitif)." },
  { "en": "Faktor daya 'lagging' artinya apa?", "id": "Arus tertinggal dari tegangan (beban induktif)." },
  { "en": "Apa itu koreksi faktor daya?", "id": "Memperbaiki faktor daya mendekati satu." },
  { "en": "Bagaimana cara memperbaiki faktor daya 'lagging'?", "id": "Dengan menambahkan kapasitor secara paralel." },
  { "en": "Apa itu daya kompleks (S)?", "id": "Kombinasi daya nyata dan reaktif." },
  { "en": "Bagaimana bentuk rektangular daya kompleks?", "id": "S sama dengan P ditambah jQ." },
  { "en": "Bagian riil dari daya kompleks?", "id": "Bagian riil adalah daya nyata (P)." },
  { "en": "Bagian imajiner dari daya kompleks?", "id": "Bagian imajiner adalah daya reaktif (Q)." },
  { "en": "Rumus daya kompleks dari fasor?", "id": "S sama dengan V dikali I konjugat." },
  { "en": "Apa itu konjugat kompleks (I*)?", "id": "Tanda pada bagian imajiner dibalik." },
  { "en": "Kenapa faktor daya penting bagi PLN?", "id": "Faktor daya buruk sebabkan rugi-rugi transmisi." },
  { "en": "Beban industri umumnya bersifat apa?", "id": "Umumnya bersifat induktif (motor listrik)." },
  { "en": "Apa itu kapasitor bank?", "id": "Sekumpulan kapasitor untuk koreksi faktor daya." },
  { "en": "Apa itu resonansi dalam rangkaian RLC?", "id": "Saat reaktansi induktif sama dengan kapasitif." },
  { "en": "Apa yang terjadi saat resonansi?", "id": "Reaktansi total menjadi nol (XL = XC)." },
  { "en": "Impedansi rangkaian seri RLC saat resonansi?", "id": "Impedansi mencapai nilai minimum (Z = R)." },
  { "en": "Arus rangkaian seri RLC saat resonansi?", "id": "Arus mencapai nilai maksimum." },
  { "en": "Faktor daya saat resonansi?", "id": "Faktor daya sama dengan satu (unity)." },
  { "en": "Apa itu frekuensi resonansi (fr)?", "id": "Frekuensi di mana resonansi terjadi." },
  { "en": "Rumus frekuensi resonansi RLC seri?", "id": "fr = 1 / (2π√(LC))." },
  { "en": "Apa itu resonansi paralel?", "id": "Resonansi pada rangkaian RLC paralel." },
  { "en": "Impedansi rangkaian paralel RLC saat resonansi?", "id": "Impedansi mencapai nilai maksimum." },
  { "en": "Arus rangkaian paralel RLC saat resonansi?", "id": "Arus total mencapai nilai minimum." },
  { "en": "Apa itu 'quality factor' (Q)?", "id": "Ukuran kualitas atau selektivitas resonansi." },
  { "en": "Nilai Q tinggi artinya apa?", "id": "Resonansi sangat tajam dan selektif." },
  { "en": "Apa itu 'bandwidth' (BW)?", "id": "Lebar rentang frekuensi di sekitar resonansi." },
  { "en": "Hubungan Q dan bandwidth?", "id": "Bandwidth berbanding terbalik dengan faktor Q." },
  { "en": "Apa itu 'frekuensi cut-off'?", "id": "Frekuensi di mana daya turun setengah." },
  { "en": "Apa itu filter lolos-rendah (low-pass)?", "id": "Melewatkan frekuensi rendah, menahan frekuensi tinggi." },
  { "en": "Contoh rangkaian filter lolos-rendah pasif?", "id": "Rangkaian RC atau RL seri." },
  { "en": "Apa itu filter lolos-tinggi (high-pass)?", "id": "Melewatkan frekuensi tinggi, menahan frekuensi rendah." },
  { "en": "Contoh rangkaian filter lolos-tinggi pasif?", "id": "Rangkaian CR atau LR seri." },
  { "en": "Apa itu filter lolos-pita (band-pass)?", "id": "Hanya melewatkan rentang frekuensi tertentu." },
  { "en": "Apa itu filter stop-pita (band-stop)?", "id": "Menahan rentang frekuensi tertentu." },
  { "en": "Bagaimana KVL berlaku di rangkaian AC?", "id": "Jumlah fasor tegangan dalam loop nol." },
  { "en": "Bagaimana KCL berlaku di rangkaian AC?", "id": "Jumlah fasor arus di simpul nol." },
  { "en": "Rumus pembagi tegangan untuk impedansi?", "id": "Sama seperti DC, tapi menggunakan impedansi." },
  { "en": "Rumus pembagi arus untuk impedansi?", "id": "Sama seperti DC, tapi menggunakan impedansi." },
  { "en": "Apa itu analisis Mesh di rangkaian AC?", "id": "Menggunakan KVL dengan fasor dan impedansi." },
  { "en": "Apa itu analisis Node di rangkaian AC?", "id": "Menggunakan KCL dengan fasor dan impedansi." },
  { "en": "Apa itu Teorema Thevenin di AC?", "id": "Menyederhanakan rangkaian menjadi satu sumber Vth & Zth." },
  { "en": "Apa itu Teorema Norton di AC?", "id": "Menyederhanakan rangkaian menjadi satu sumber In & Zn." },
  { "en": "Apa hubungan Zth dan Zn?", "id": "Hambatan Thevenin dan Norton sama (Zth = Zn)." },
  { "en": "Apa itu 'transfer daya maksimum' di AC?", "id": "Saat impedansi beban sama dengan konjugat Thevenin." },
  { "en": "Syarat transfer daya maksimum AC?", "id": "Z_L sama dengan Z_th konjugat." },
  { "en": "Apa itu transformator (trafo)?", "id": "Alat untuk menaikkan/menurunkan tegangan AC." },
  { "en": "Prinsip kerja transformator?", "id": "Berdasarkan induksi elektromagnetik bersama." },
  { "en": "Apakah transformator bekerja pada DC?", "id": "Tidak, trafo memerlukan perubahan fluks magnet." },
  { "en": "Apa itu lilitan primer?", "id": "Lilitan yang terhubung ke sumber tegangan." },
  { "en": "Apa itu lilitan sekunder?", "id": "Lilitan yang terhubung ke beban." },
  { "en": "Apa itu trafo 'step-up'?", "id": "Transformator yang menaikkan tegangan." },
  { "en": "Apa itu trafo 'step-down'?", "id": "Transformator yang menurunkan tegangan." },
  { "en": "Karakteristik lilitan trafo 'step-up'?", "id": "Lilitan sekunder lebih banyak dari primer." },
  { "en": "Karakteristik lilitan trafo 'step-down'?", "id": "Lilitan primer lebih banyak dari sekunder." },
  { "en": "Apa itu 'perbandingan lilitan' (turns ratio)?", "id": "Perbandingan jumlah lilitan primer dan sekunder." },
  { "en": "Hubungan tegangan dan lilitan di trafo?", "id": "Rasio tegangan sama dengan rasio lilitan." },
  { "en": "Hubungan arus dan lilitan di trafo ideal?", "id": "Rasio arus berbanding terbalik rasio lilitan." },
  { "en": "Apa itu trafo ideal?", "id": "Transformator tanpa ada kerugian daya." },
  { "en": "Apa itu 'rugi tembaga' pada trafo?", "id": "Kerugian daya akibat resistansi lilitan." },
  { "en": "Apa itu 'rugi inti' pada trafo?", "id": "Gabungan rugi histeresis dan arus eddy." },
  { "en": "Apa itu 'impedansi refleksi'?", "id": "Impedansi beban dilihat dari sisi primer." },
  { "en": "Apa itu sistem tiga fasa?", "id": "Sistem listrik menggunakan tiga gelombang AC." },
  { "en": "Berapa beda fasa antar gelombang tiga fasa?", "id": "Beda fasanya adalah 120 derajat." },
  { "en": "Keuntungan sistem tiga fasa?", "id": "Lebih efisien untuk transmisi daya besar." },
  { "en": "Apa itu koneksi 'Wye' (Y) atau Bintang?", "id": "Ujung-ujung lilitan terhubung di titik netral." },
  { "en": "Apa itu koneksi 'Delta' (Δ) atau Segitiga?", "id": "Lilitan terhubung secara ujung-ke-ujung." },
  { "en": "Apa itu 'tegangan fasa' (V_ph)?", "id": "Tegangan pada satu lilitan sumber/beban." },
  { "en": "Apa itu 'tegangan line' (V_L)?", "id": "Tegangan antara dua jalur transmisi." },
  { "en": "Apa itu 'arus fasa' (I_ph)?", "id": "Arus yang mengalir pada satu lilitan." },
  { "en": "Apa itu 'arus line' (I_L)?", "id": "Arus yang mengalir pada jalur transmisi." },
  { "en": "Hubungan tegangan pada koneksi Wye?", "id": "Tegangan line = √3 x tegangan fasa." },
  { "en": "Hubungan arus pada koneksi Wye?", "id": "Arus line sama dengan arus fasa." },
  { "en": "Hubungan tegangan pada koneksi Delta?", "id": "Tegangan line sama dengan tegangan fasa." },
  { "en": "Hubungan arus pada koneksi Delta?", "id": "Arus line = √3 x arus fasa." },
  { "en": "Apa itu 'urutan fasa' (phase sequence)?", "id": "Urutan gelombang mencapai puncak (e.g., abc)." },
  { "en": "Apa itu 'beban seimbang' (balanced load)?", "id": "Impedansi pada ketiga fasa nilainya sama." },
  { "en": "Arus di kabel netral pada beban seimbang?", "id": "Arus di kabel netral adalah nol." },
  { "en": "Rumus daya total tiga fasa?", "id": "P = √3 x V_L x I_L x cos(θ)." },
  { "en": "Apa itu generator AC?", "id": "Mesin yang mengubah energi mekanik ke listrik AC." },
  { "en": "Apa itu motor AC?", "id": "Mesin yang mengubah energi listrik AC ke mekanik." },
  { "en": "Apa itu 'slip' pada motor induksi?", "id": "Perbedaan antara kecepatan rotor dan medan putar." },
  { "en": "Apa itu 'starting capacitor'?", "id": "Kapasitor untuk membantu motor satu fasa berputar." },
  { "en": "Apa itu VFD (Variable Frequency Drive)?", "id": "Alat untuk mengatur kecepatan motor AC." },
  { "en": "Bagaimana cara kerja VFD?", "id": "Dengan mengubah frekuensi tegangan suplai." },
  { "en": "Apa itu harmonisa?", "id": "Gelombang sinus dengan frekuensi kelipatan fundamental." },
  { "en": "Dampak negatif dari harmonisa?", "id": "Menyebabkan panas berlebih dan distorsi gelombang." },
  { "en": "Apa itu 'Total Harmonic Distortion' (THD)?", "id": "Ukuran tingkat distorsi harmonisa." },
  { "en": "Apa itu 'skin effect'?", "id": "Kecenderungan arus AC mengalir di permukaan konduktor." },
  { "en": "Apa itu osiloskop?", "id": "Alat untuk menampilkan bentuk gelombang tegangan." },
  { "en": "Apa itu 'function generator'?", "id": "Alat untuk menghasilkan berbagai bentuk gelombang." },
  { "en": "Apa itu LCR meter?", "id": "Alat ukur Induktansi, Kapasitansi, dan Resistansi." },
  { "en": "Apa itu 'power quality analyzer'?", "id": "Alat untuk menganalisis kualitas daya listrik." },
  { "en": "Apa itu 'gelombang kotak' (square wave)?", "id": "Gelombang yang beralih antara dua level." },
  { "en": "Apa itu 'gelombang segitiga' (triangle wave)?", "id": "Gelombang yang naik dan turun secara linier." },
  { "en": "Apa itu 'duty cycle'?", "id": "Rasio waktu ON terhadap total periode." },
  { "en": "Apa itu 'Fourier analysis'?", "id": "Memecah sinyal periodik menjadi gelombang sinus." },
  { "en": "Apa itu 'fundamental frequency'?", "id": "Frekuensi terendah dalam dekomposisi sinyal." },
  { "en": "Apa itu 'gelombang non-sinusoidal'?", "id": "Gelombang periodik yang bukan sinus murni." },
  { "en": "Bagaimana menganalisis rangkaian dengan gelombang non-sinusoidal?", "id": "Gunakan superposisi untuk setiap komponen harmonisa." },
  { "en": "Apa itu 'true RMS' multimeter?", "id": "Mengukur RMS akurat untuk gelombang non-sinusoidal." },
  { "en": "Apa itu 'rangkaian kopling' (coupling)?", "id": "Rangkaian untuk mentransfer sinyal antar tingkat." },
  { "en": "Kapasitor untuk kopling bersifat apa pada DC?", "id": "Memblokir komponen tegangan DC." },
  { "en": "Apa itu 'kapasitor bypass'?", "id": "Kapasitor untuk 'melewatkan' sinyal AC ke ground." },
  { "en": "Apa itu 'decoupling capacitor'?", "id": "Menstabilkan tegangan catu daya dari noise." },
  { "en": "Apakah admintansi adalah kuantitas fasor?", "id": "Ya, admintansi adalah kuantitas kompleks/fasor." },
  { "en": "Satuan konduktansi (G) dan suseptansi (B)?", "id": "Keduanya menggunakan satuan Siemens (S)." },
  { "en": "Suseptansi induktif (BL) nilainya positif/negatif?", "id": "Suseptansi induktif bernilai negatif." },
  { "en": "Suseptansi kapasitif (BC) nilainya positif/negatif?", "id": "Suseptansi kapasitif bernilai positif." },
  { "en": "Rumus daya rata-rata (P)?", "id": "P = Vrms * Irms * cos(θ)." },
  { "en": "Rumus daya reaktif (Q)?", "id": "Q = Vrms * Irms * sin(θ)." },
  { "en": "Rumus daya semu (S)?", "id": "S = Vrms * Irms." },
  { "en": "Apa itu sudut impedansi (θ)?", "id": "Sudut fasa antara tegangan dan arus." },
  { "en": "Jika beban resistif murni, sudutnya berapa?", "id": "Sudut fasanya adalah nol derajat." },
  { "en": "Jika beban induktif murni, sudutnya berapa?", "id": "Sudut fasanya adalah +90 derajat." },
  { "en": "Jika beban kapasitif murni, sudutnya berapa?", "id": "Sudut fasanya adalah -90 derajat." },
  { "en": "Apa itu 'wattmeter'?", "id": "Alat untuk mengukur daya nyata (Watt)." },
  { "en": "Apa itu 'VARmeter'?", "id": "Alat untuk mengukur daya reaktif (VAR)." },
  { "en": "Daya rata-rata disebut juga daya apa?", "id": "Disebut juga daya nyata atau daya aktif." },
  { "en": "Mengapa daya reaktif penting?", "id": "Dibutuhkan untuk membangkitkan medan magnet/listrik." },
  { "en": "Apakah daya reaktif melakukan kerja nyata?", "id": "Tidak, daya reaktif tidak melakukan kerja." },
  { "en": "Faktor daya 0.8 lagging artinya apa?", "id": "Beban bersifat induktif." },
  { "en": "Faktor daya 0.9 leading artinya apa?", "id": "Beban bersifat kapasitif." },
  { "en": "Bagaimana cara mengubah bentuk polar ke rektangular?", "id": "Gunakan fungsi kosinus dan sinus." },
  { "en": "Bagaimana cara mengubah bentuk rektangular ke polar?", "id": "Gunakan teorema Pythagoras dan tangen." },
  { "en": "Operasi apa yang mudah dalam bentuk polar?", "id": "Perkalian dan pembagian fasor." },
  { "en": "Operasi apa yang mudah dalam bentuk rektangular?", "id": "Penjumlahan dan pengurangan fasor." },
  { "en": "Bagaimana cara mengalikan fasor bentuk polar?", "id": "Kalikan magnitudo, jumlahkan sudutnya." },
  { "en": "Bagaimana cara membagi fasor bentuk polar?", "id": "Bagi magnitudo, kurangi sudutnya." },
  { "en": "Nilai RMS dari gelombang kotak simetris?", "id": "Nilai RMS sama dengan amplitudonya." },
  { "en": "Nilai RMS dari gelombang segitiga simetris?", "id": "Vp dibagi dengan akar tiga." },
  { "en": "Apa itu 'crest factor'?", "id": "Rasio nilai puncak terhadap nilai RMS." },
  { "en": "Crest factor gelombang sinus?", "id": "Nilai crest factor-nya adalah akar dua (~1.414)." },
  { "en": "Apa itu 'form factor'?", "id": "Rasio nilai RMS terhadap nilai rata-rata." },
  { "en": "Apa itu 'rangkaian tangki' (tank circuit)?", "id": "Rangkaian LC paralel, digunakan di osilator." },
  { "en": "Apa itu 'mutual inductance' (induktansi bersama)?", "id": "Efek GGL terinduksi di satu kumparan oleh kumparan lain." },
  { "en": "Apa satuan dari 'mutual inductance'?", "id": "Satuannya adalah Henry (H)." },
  { "en": "Apa itu 'koefisien kopling' (k)?", "id": "Ukuran seberapa erat dua kumparan terkopling." },
  { "en": "Berapa nilai 'k' untuk kopling sempurna?", "id": "Nilai k sama dengan 1." },
  { "en": "Apa itu 'dot convention'?", "id": "Aturan titik untuk menentukan polaritas GGL induksi." },
  { "en": "Apa itu 'trafo isolasi'?", "id": "Trafo dengan perbandingan lilitan 1:1." },
  { "en": "Fungsi utama trafo isolasi?", "id": "Untuk keamanan, mengisolasi rangkaian dari ground." },
  { "en": "Apa itu 'autotransformer'?", "id": "Trafo yang hanya memiliki satu lilitan." },
  { "en": "Kelebihan autotransformer?", "id": "Ukuran lebih kecil dan lebih efisien." },
  { "en": "Kelemahan autotransformer?", "id": "Tidak ada isolasi antara primer dan sekunder." },
  { "en": "Apa itu 'current transformer' (CT)?", "id": "Trafo untuk mengukur arus yang sangat besar." },
  { "en": "Apa itu 'potential transformer' (PT)?", "id": "Trafo untuk mengukur tegangan yang sangat tinggi." },
  { "en": "Mengapa sekunder CT tidak boleh terbuka?", "id": "Dapat menghasilkan tegangan sangat tinggi berbahaya." },
  { "en": "Apa itu 'sistem polifasa'?", "id": "Sistem yang menggunakan lebih dari satu fasa." },
  { "en": "Apa itu 'daya sesaat' tiga fasa?", "id": "Daya sesaatnya bernilai konstan." },
  { "en": "Mengapa motor tiga fasa tidak butuh kapasitor?", "id": "Medan putar dihasilkan oleh beda fasa." },
  { "en": "Apa itu 'medan putar magnet'?", "id": "Medan magnet yang berputar dihasilkan sistem 3 fasa." },
  { "en": "Bagaimana cara membalik arah putaran motor 3 fasa?", "id": "Tukar koneksi dua dari tiga fasanya." },
  { "en": "Apa itu 'analisis per fasa'?", "id": "Analisis sistem 3 fasa dengan menyederhanakannya." },
  { "en": "Kapan 'analisis per fasa' bisa digunakan?", "id": "Hanya jika sistemnya seimbang (balanced)." },
  { "en": "Apa itu 'wattmeter' metode dua watt?", "id": "Metode mengukur daya 3 fasa dengan 2 wattmeter." },
  { "en": "Apa itu 'urutan fasa negatif'?", "id": "Urutan fasa terbalik (misal, acb)." },
  { "en": "Apa itu 'komponen simetris'?", "id": "Metode analisis sistem tiga fasa tak seimbang." },
  { "en": "Apa itu 'urutan nol' (zero sequence)?", "id": "Komponen di mana ketiga fasa sefasa." },
  { "en": "Apa itu 'urutan positif' (positive sequence)?", "id": "Komponen dengan urutan fasa normal (abc)." },
  { "en": "Apa itu 'urutan negatif' (negative sequence)?", "id": "Komponen dengan urutan fasa terbalik (acb)." },
  { "en": "Apa itu 'gangguan tak simetris'?", "id": "Gangguan yang hanya mempengaruhi satu atau dua fasa." },
  { "en": "Apa itu 'frekuensi angular' (ω)?", "id": "Kecepatan sudut dari fasor (ω = 2πf)." },
  { "en": "Bagaimana cara menjumlahkan fasor?", "id": "Ubah ke bentuk rektangular, lalu jumlahkan." },
  { "en": "Sudut -90° sama dengan operator apa?", "id": "Sama dengan operator -j." },
  { "en": "Sudut +90° sama dengan operator apa?", "id": "Sama dengan operator +j." },
  { "en": "Sudut 180° sama dengan operator apa?", "id": "Sama dengan -1 (negatif satu)." },
  { "en": "Apa itu 'domain frekuensi'?", "id": "Representasi sinyal atau rangkaian dalam frekuensi." },
  { "en": "Apa itu 'domain waktu'?", "id": "Representasi sinyal atau rangkaian dalam waktu." },
  { "en": "Analisis fasor dilakukan di domain apa?", "id": "Analisis fasor dilakukan di domain frekuensi." },
  { "en": "Apa itu 'transformasi Laplace'?", "id": "Metode matematika untuk analisis rangkaian." },
  { "en": "Apa itu 'transformasi Fourier'?", "id": "Memecah sinyal menjadi spektrum frekuensinya." },
  { "en": "Apa itu 'spektrum frekuensi'?", "id": "Plot amplitudo sinyal terhadap frekuensi." },
  { "en": "Gelombang sinus murni punya berapa frekuensi?", "id": "Hanya punya satu komponen frekuensi." },
  { "en": "Gelombang kotak punya berapa frekuensi?", "id": "Punya frekuensi fundamental dan harmonisa ganjil." },
  { "en": "Apa itu 'decibel' (dB)?", "id": "Satuan logaritmik untuk rasio." },
  { "en": "Apa itu 'dekade' (decade) dalam frekuensi?", "id": "Perubahan frekuensi sebesar 10 kali lipat." },
  { "en": "Apa itu 'oktaf' (octave) dalam frekuensi?", "id": "Perubahan frekuensi sebesar 2 kali lipat." },
  { "en": "Apa itu 'Bode plot'?", "id": "Grafik respons frekuensi (magnitudo dan fasa)." },
  { "en": "Sumbu X pada 'Bode plot'?", "id": "Sumbu X adalah frekuensi dalam skala logaritmik." },
  { "en": "Sumbu Y (magnitudo) pada 'Bode plot'?", "id": "Sumbu Y adalah gain dalam satuan desibel (dB)." },
  { "en": "Apa itu 'gain'?", "id": "Rasio antara sinyal output dan sinyal input." },
  { "en": "Apa itu 'atenuasi'?", "id": "Pelemahan sinyal, gain kurang dari 1." },
  { "en": "Gain 0 dB artinya apa?", "id": "Output sama dengan input (penguatan 1x)." },
  { "en": "Apa itu 'pole' dalam fungsi transfer?", "id": "Frekuensi di mana respons mulai turun." },
  { "en": "Apa itu 'zero' dalam fungsi transfer?", "id": "Frekuensi di mana respons mulai naik." },
  { "en": "Apa itu 'fungsi transfer'?", "id": "Rasio fasor output terhadap fasor input." },
  { "en": "Apa itu 'filter pasif'?", "id": "Filter yang hanya menggunakan R, L, C." },
  { "en": "Apa itu 'filter aktif'?", "id": "Filter yang menggunakan komponen aktif (misal, op-amp)." },
  { "en": "Kelebihan filter aktif?", "id": "Bisa punya gain, tidak butuh induktor." },
  { "en": "Apa itu 'orde' filter?", "id": "Menentukan tingkat kecuraman (roll-off) filter." },
  { "en": "Berapa 'roll-off' filter orde pertama?", "id": "Sebesar -20 dB per dekade." },
  { "en": "Apa itu 'kapasitor variabel'?", "id": "Kapasitor yang nilainya dapat diubah-ubah." },
  { "en": "Di mana kapasitor variabel digunakan?", "id": "Pada rangkaian penala (tuner) radio." },
  { "en": "Apa itu 'induktor variabel'?", "id": "Induktor yang nilainya dapat diubah-ubah." },
  { "en": "Apa itu 'rangkaian penala' (tuned circuit)?", "id": "Rangkaian resonansi untuk memilih satu frekuensi." },
  { "en": "Bagaimana impedansi total rangkaian seri dihitung?", "id": "Jumlahkan semua impedansi secara vektorial." },
  { "en": "Apa itu 'susceptance' (B)?", "id": "Bagian imajiner dari admintansi (Y)." },
  { "en": "Adakah daya yang hilang di kapasitor ideal?", "id": "Tidak, daya rata-ratanya nol." },
  { "en": "Adakah daya yang hilang di induktor ideal?", "id": "Tidak, daya rata-ratanya nol." },
  { "en": "Apa yang dimaksud dengan 'beban kapasitif'?", "id": "Beban di mana arus mendahului tegangan." },
  { "en": "Apa yang dimaksud dengan 'beban induktif'?", "id": "Beban di mana arus tertinggal dari tegangan." },
  { "en": "Apa yang dimaksud dengan 'beban resistif'?", "id": "Beban di mana arus dan tegangan sefasa." },
  { "en": "Apa itu 'gelombang termodulasi amplitudo' (AM)?", "id": "Amplitudo pembawa diubah oleh sinyal informasi." },
  { "en": "Apa itu 'gelombang termodulasi frekuensi' (FM)?", "id": "Frekuensi pembawa diubah oleh sinyal informasi." },
  { "en": "Apa itu 'demodulasi' atau 'deteksi'?", "id": "Proses mengambil sinyal informasi dari pembawa." },
  { "en": "Apa itu 'dioda detektor'?", "id": "Dioda yang digunakan dalam proses demodulasi AM." },
  { "en": "Apa itu 'nilai efektif'?", "id": "Nama lain untuk nilai RMS." },
  { "en": "Tegangan listrik rumah tangga AC atau DC?", "id": "Listrik rumah tangga adalah AC." },
  { "en": "Mengapa transmisi daya menggunakan tegangan tinggi?", "id": "Untuk mengurangi rugi-rugi daya (I²R)." },
  { "en": "Apa itu 'gardu induk'?", "id": "Tempat menaikkan dan menurunkan tegangan transmisi." },
  { "en": "Apa itu 'inverter'?", "id": "Alat yang mengubah tegangan DC menjadi AC." },
  { "en": "Apa itu 'rectifier' (penyearah)?", "id": "Alat yang mengubah tegangan AC menjadi DC." },
  { "en": "Apa itu 'cycloconverter'?", "id": "Mengubah AC satu frekuensi ke frekuensi lain." },
  { "en": "Apa itu 'listrik statis'?", "id": "Listrik yang muatannya tidak bergerak (DC)." },
  { "en": "Apa itu 'arus eddy'?", "id": "Arus sirkulasi yang terinduksi pada konduktor." },
  { "en": "Bagaimana cara mengurangi 'arus eddy' di trafo?", "id": "Menggunakan inti besi berlapis (laminasi)." },
  { "en": "Apa itu 'rugi histeresis'?", "id": "Energi yang hilang untuk memagnetisasi inti." },
  { "en": "Apa itu 'saturasi magnetik'?", "id": "Kondisi inti tidak bisa dimagnetisasi lebih lanjut." },
  { "en": "Satuan apa yang digunakan untuk daya semu?", "id": "Volt-Ampere (VA) atau kilovolt-ampere (kVA)." },
  { "en": "Satuan apa yang digunakan untuk daya reaktif?", "id": "Volt-Ampere Reactive (VAR)." },
  { "en": "Berapa beda fasa antara daya nyata dan reaktif?", "id": "Beda fasanya adalah 90 derajat." },
  { "en": "Faktor daya 1 artinya sudut fasanya berapa?", "id": "Sudut fasa antara V dan I adalah nol." },
  { "en": "Faktor daya 0 artinya sudut fasanya berapa?", "id": "Sudut fasa antara V dan I adalah 90°." },
  { "en": "Apakah daya reaktif bisa diukur dengan wattmeter?", "id": "Tidak, wattmeter hanya mengukur daya nyata." },
  { "en": "Apa itu 'penindasan riak' (ripple rejection)?", "id": "Kemampuan menekan sisa gelombang AC pada DC." },
  { "en": "Bagaimana cara merepresentasikan fasor secara grafis?", "id": "Sebagai anak panah yang berputar." },
  { "en": "Kecepatan putaran fasor ditentukan oleh apa?", "id": "Ditentukan oleh frekuensi sudut (ω)." },
  { "en": "Apa itu 'rangkaian orde pertama' di AC?", "id": "Rangkaian dengan satu elemen penyimpan energi." },
  { "en": "Apa itu 'rangkaian orde kedua' di AC?", "id": "Rangkaian dengan dua elemen penyimpan energi." },
  { "en": "Apa itu 'damping' pada rangkaian RLC?", "id": "Efek peredaman osilasi oleh resistor." },
  { "en": "Apa itu 'frekuensi natural' tak teredam?", "id": "Frekuensi osilasi rangkaian LC tanpa R." },
  { "en": "Apa itu 'faktor redaman' (damping factor)?", "id": "Ukuran seberapa cepat osilasi meluruh." },
  { "en": "Apa itu 'osilasi teredam' (damped oscillation)?", "id": "Osilasi yang amplitudonya mengecil seiring waktu." },
  { "en": "Apa itu 'kondisi tunak' (steady state) AC?", "id": "Kondisi setelah semua efek transien hilang." },
  { "en": "Berapa lama waktu transien berlangsung?", "id": "Biasanya sekitar lima kali konstanta waktu." },
  { "en": "Apa itu 'analisis kondisi-tunak sinusoidal'?", "id": "Analisis rangkaian AC dalam kondisi tunak." },
  { "en": "Apa itu 'jembatan AC'?", "id": "Jembatan Wheatstone yang diadaptasi untuk AC." },
  { "en": "Apa fungsi jembatan AC?", "id": "Untuk mengukur impedansi (L atau C) tak diketahui." },
  { "en": "Contoh jembatan AC?", "id": "Jembatan Maxwell, Hay, dan Schering." },
  { "en": "Apa itu 'Q-meter'?", "id": "Alat untuk mengukur faktor kualitas (Q)." },
  { "en": "Apa itu 'isolasi galvanik'?", "id": "Memisahkan dua bagian rangkaian secara listrik." },
  { "en": "Mengapa isolasi galvanik penting?", "id": "Untuk keamanan dan mengurangi gangguan (noise)." },
  { "en": "Apa itu 'common-mode choke'?", "id": "Induktor untuk menekan gangguan 'common-mode'." },
  { "en": "Apa itu 'filter saluran' (line filter)?", "id": "Filter untuk menekan gangguan dari jala-jala listrik." },
  { "en": "Apa itu 'gelombang termodifikasi sinus'?", "id": "Gelombang keluaran dari inverter murah." },
  { "en": "Apa itu 'gelombang sinus murni'?", "id": "Gelombang keluaran dari inverter kualitas tinggi." },
  { "en": "Apa itu 'tegangan offset DC'?", "id": "Komponen DC yang ada pada sinyal AC." },
  { "en": "Bagaimana kapasitor memblokir DC?", "id": "Reaktansi kapasitif tak terhingga pada 0 Hz." },
  { "en": "Bagaimana induktor melewatkan DC?", "id": "Reaktansi induktif nol pada 0 Hz." },
  { "en": "Apa itu 'kapasitansi parasitik'?", "id": "Kapasitansi yang tidak diinginkan dalam rangkaian." },
  { "en": "Apa itu 'induktansi parasitik'?", "id": "Induktansi yang tidak diinginkan dalam rangkaian." },
  { "en": "Apa itu 'frekuensi resonansi-diri' (SRF)?", "id": "Frekuensi di mana efek parasitik beresonansi." },
  { "en": "Di atas SRF, kapasitor bersifat apa?", "id": "Kapasitor mulai bersifat seperti induktor." },
  { "en": "Di atas SRF, induktor bersifat apa?", "id": "Induktor mulai bersifat seperti kapasitor." },
  { "en": "Apa itu 'matching network'?", "id": "Rangkaian untuk mencocokkan impedansi." },
  { "en": "Mengapa 'matching' impedansi penting?", "id": "Untuk transfer daya maksimum dan mengurangi pantulan." },
  { "en": "Apa itu 'gelombang berdiri' (standing wave)?", "id": "Gelombang akibat pantulan pada saluran transmisi." },
  { "en": "Apa itu 'VSWR (Voltage Standing Wave Ratio)'?", "id": "Ukuran ketidakcocokan impedansi." },
  { "en": "Nilai VSWR yang ideal berapa?", "id": "Nilai idealnya adalah 1:1." },
  { "en": "Apa itu 'Smith Chart'?", "id": "Alat bantu grafis untuk analisis impedansi." },
  { "en": "Apa itu 'saluran transmisi'?", "id": "Media untuk memandu gelombang elektromagnetik." },
  { "en": "Contoh saluran transmisi?", "id": "Kabel koaksial, 'waveguide', 'microstrip'." },
  { "en": "Apa itu 'impedansi karakteristik'?", "id": "Impedansi yang dilihat oleh gelombang." },
  { "en": "Apa itu 'balun'?", "id": "Transformator untuk menghubungkan saluran seimbang & tak seimbang." },
  { "en": "Apa itu 'saluran seimbang'?", "id": "Contohnya adalah kabel 'twin-lead'." },
  { "en": "Apa itu 'saluran tak seimbang'?", "id": "Contohnya adalah kabel koaksial." },
  { "en": "Apa itu 'grounding' frekuensi radio (RF)?", "id": "Sistem pentanahan untuk aplikasi frekuensi tinggi." },
  { "en": "Apa itu 'transformasi Fourier cepat' (FFT)?", "id": "Algoritma efisien untuk menghitung spektrum." },
  { "en": "Apa itu 'penganalisis spektrum' (spectrum analyzer)?", "id": "Alat untuk menampilkan spektrum frekuensi sinyal." },
  { "en": "Apa itu 'penganalisis jaringan' (network analyzer)?", "id": "Alat untuk mengukur parameter jaringan." },
  { "en": "Apa itu 'S-parameters'?", "id": "Parameter untuk mengkarakterisasi jaringan frekuensi tinggi." },
  { "en": "Apa itu 'daya rata-rata' dalam konteks sinyal?", "id": "Nilai kuadrat dari nilai RMS." },
  { "en": "Bagaimana cara mengukur beda fasa?", "id": "Menggunakan osiloskop dengan mode XY (Lissajous)." },
  { "en": "Apa itu 'angka Lissajous'?", "id": "Pola yang terbentuk saat menampilkan dua sinus." },
  { "en": "Apa itu 'powerline communication' (PLC)?", "id": "Mengirim data melalui kabel jala-jala listrik." },
  { "en": "Apa itu 'pemadaman' (blackout)?", "id": "Kehilangan total daya listrik di suatu area." },
  { "en": "Apa itu 'penurunan tegangan' (brownout)?", "id": "Penurunan tegangan listrik yang disengaja." },
  { "en": "Apa itu 'lonjakan tegangan' (surge)?", "id": "Kenaikan tegangan sesaat yang sangat tinggi." },
  { "en": "Apa itu 'pelindung lonjakan' (surge protector)?", "id": "Alat untuk melindungi dari lonjakan tegangan." },
  { "en": "Apa itu 'UPS (Uninterruptible Power Supply)'?", "id": "Catu daya darurat saat listrik padam." },
  { "en": "Apa itu 'gelombang sinus termodifikasi'?", "id": "Gelombang AC yang bukan sinus murni." },
  { "en": "Apa itu 'faktor daya leading'?", "id": "Arus mendahului tegangan (beban kapasitif)." },
  { "en": "Apa itu 'faktor daya lagging'?", "id": "Arus tertinggal tegangan (beban induktif)." },
  { "en": "Apakah motor listrik beban kapasitif atau induktif?", "id": "Motor listrik adalah beban induktif." },
  { "en": "Apakah lampu pijar beban resistif?", "id": "Ya, lampu pijar bersifat resistif murni." },
  { "en": "Apa itu 'sirkuit terkopel magnetik'?", "id": "Sirkuit yang dihubungkan melalui medan magnet." },
  { "en": "Apa itu 'magnetisasi'?", "id": "Proses membuat suatu bahan menjadi magnet." },
  { "en": "Apa itu 'permeabilitas magnetik'?", "id": "Kemampuan bahan untuk mendukung medan magnet." },
  { "en": "Apa itu 'transformator sentuh-tengah' (center-tapped)?", "id": "Trafo dengan koneksi di tengah lilitan." },
  { "en": "Apa itu 'frekuensi sudut natural'?", "id": "Frekuensi osilasi rangkaian tanpa redaman." },
  { "en": "Apa itu 'arus loop'?", "id": "Arus imajiner yang digunakan dalam analisis Mesh." },
  { "en": "Apa itu 'tegangan simpul'?", "id": "Tegangan suatu simpul terhadap titik referensi." },
  { "en": "Bagaimana resistansi murni mempengaruhi fasa?", "id": "Resistansi murni tidak menggeser fasa." },
  { "en": "Bagaimana reaktansi murni mempengaruhi daya?", "id": "Reaktansi murni tidak menyerap daya nyata." },
  { "en": "Apa itu 'beban non-linier'?", "id": "Beban yang arusnya tidak sinusoidal." },
  { "en": "Contoh beban non-linier?", "id": "Penyearah, catu daya switching, lampu neon." },
  { "en": "Apa yang dihasilkan oleh beban non-linier?", "id": "Menghasilkan arus harmonisa ke dalam jala-jala." },
  { "en": "Apa itu 'kualitas daya' (power quality)?", "id": "Ukuran seberapa baik suplai listrik." },
  { "en": "Masalah umum kualitas daya?", "id": "Harmonisa, lonjakan, penurunan tegangan." },
  { "en": "Apa itu 'sistem tenaga listrik'?", "id": "Jaringan pembangkitan, transmisi, dan distribusi." },
  { "en": "Apa itu 'pembangkitan' (generation)?", "id": "Proses mengubah energi lain menjadi listrik." },
  { "en": "Apa itu 'transmisi' (transmission)?", "id": "Menyalurkan listrik tegangan tinggi jarak jauh." },
  { "en": "Apa itu 'distribusi' (distribution)?", "id": "Menyalurkan listrik tegangan rendah ke pelanggan." },
  { "en": "Apa itu 'jaringan' (grid)?", "id": "Jaringan interkoneksi sistem tenaga listrik." },
  { "en": "Apa itu 'arus sirkulasi'?", "id": "Arus yang mengalir antar sumber paralel." },
  { "en": "Apa itu 'kapasitansi saluran'?", "id": "Kapasitansi yang ada antara kabel transmisi." },
  { "en": "Apa itu 'induktansi saluran'?", "id": "Induktansi yang ada pada kabel transmisi." },
  { "en": "Apa itu 'efek Ferranti'?", "id": "Tegangan ujung penerima lebih tinggi dari pengirim." },
  { "en": "Kapan 'efek Ferranti' terjadi?", "id": "Pada saluran transmisi panjang tanpa beban." },
  { "en": "Apa itu 'korona' pada saluran transmisi?", "id": "Pelepasan muatan listrik ke udara sekitar." },
  { "en": "Apa itu 'busbar'?", "id": "Konduktor pusat untuk distribusi daya." },
  { "en": "Apa itu 'switchgear'?", "id": "Peralatan saklar dan proteksi tegangan tinggi." },
  { "en": "Apa itu 'relai proteksi'?", "id": "Perangkat yang mendeteksi gangguan dan trip." },
  { "en": "Apa itu 'pemutus sirkuit' (circuit breaker)?", "id": "Saklar otomatis untuk memutus arus gangguan." },
  { "en": "Apa itu 'arus gangguan' (fault current)?", "id": "Arus sangat besar saat terjadi hubung singkat." },
  { "en": "Apa itu 'stabilitas sistem tenaga'?", "id": "Kemampuan sistem untuk tetap sinkron." },
  { "en": "Apa itu 'pemadaman bergilir'?", "id": "Pemadaman terencana untuk menyeimbangkan beban." },
  { "en": "Apa itu 'manajemen beban'?", "id": "Upaya untuk mengontrol permintaan listrik." },
  { "en": "Apa itu 'jaringan pintar' (smart grid)?", "id": "Jaringan listrik dengan komunikasi dua arah." },
  { "en": "Apa itu 'meteran pintar' (smart meter)?", "id": "Meteran yang bisa mengirim data konsumsi." },
  { "en": "Apa itu 'energi terbarukan'?", "id": "Energi dari sumber daya alam terbarukan." },
  { "en": "Contoh energi terbarukan?", "id": "Tenaga surya, angin, air, panas bumi." },
  { "en": "Apa itu 'panel surya fotovoltaik' (PV)?", "id": "Mengubah cahaya matahari langsung menjadi listrik." },
  { "en": "Listrik yang dihasilkan panel surya AC atau DC?", "id": "Panel surya menghasilkan listrik DC." },
  { "en": "Apa itu 'inverter jaringan' (grid-tie inverter)?", "id": "Menyinkronkan dan memasukkan daya ke jaringan." },
  { "en": "Apa itu 'pembangkit listrik tenaga angin'?", "id": "Menggunakan turbin angin untuk menghasilkan listrik." },
  { "en": "Apa itu 'pembangkit listrik tenaga air'?", "id": "Menggunakan aliran air untuk memutar turbin." },
  { "en": "Apa itu 'motor sinkron'?", "id": "Motor AC yang putarannya sinkron dengan frekuensi." },
  { "en": "Apa itu 'motor induksi'?", "id": "Motor AC yang putarannya sedikit lebih lambat." },
  { "en": "Motor AC yang paling umum di industri?", "id": "Motor induksi tiga fasa." },
  { "en": "Apa itu 'kondensor sinkron'?", "id": "Motor sinkron untuk memperbaiki faktor daya." },
  { "en": "Bagaimana cara kerja kondensor sinkron?", "id": "Menyuplai daya reaktif ke jala-jala." },
  { "en": "Apa itu 'kapasitor kopling'?", "id": "Kapasitor yang melewatkan sinyal AC." },
  { "en": "Apa itu 'induktor kopling' (choke)?", "id": "Induktor yang menahan sinyal AC." },
  { "en": "Apa itu 'transformator audio'?", "id": "Trafo untuk mencocokkan impedansi di rangkaian audio." },
  { "en": "Apa itu 'transformator pulsa'?", "id": "Trafo untuk mentransfer pulsa digital." },
  { "en": "Apa itu 'transformator variabel' (variac)?", "id": "Autotransformer dengan tegangan output variabel." },
  { "en": "Apa itu 'efisiensi' transformator?", "id": "Rasio daya output terhadap daya input." },
  { "en": "Apa itu 'regulasi tegangan' transformator?", "id": "Perubahan tegangan sekunder dari tanpa beban." },
  { "en": "Apa itu 'arus magnetisasi' trafo?", "id": "Arus yang dibutuhkan untuk menciptakan fluks." },
  { "en": "Apa itu 'arus tanpa beban' trafo?", "id": "Arus primer saat sekunder terbuka." },
  { "en": "Apa itu 'tes hubung singkat' trafo?", "id": "Tes untuk menentukan parameter seri trafo." },
  { "en": "Apa itu 'tes rangkaian terbuka' trafo?", "id": "Tes untuk menentukan parameter paralel trafo." },
  { "en": "Apa itu 'rangkaian ekuivalen' trafo?", "id": "Model rangkaian untuk menganalisis trafo." },
  { "en": "Apa itu 'pergeseran fasa' pada filter?", "id": "Filter juga mengubah fasa sinyal." },
  { "en": "Apa itu 'delay grup'?", "id": "Ukuran distorsi fasa pada filter." },
  { "en": "Apa itu 'filter Butterworth'?", "id": "Filter dengan respons magnitudo paling datar." },
  { "en": "Apa itu 'filter Chebyshev'?", "id": "Filter dengan 'roll-off' lebih curam tapi ada riak." },
  { "en": "Apa itu 'filter Bessel'?", "id": "Filter dengan respons fasa paling linier." },
  { "en": "Apa itu 'filter eliptik' (Cauer)?", "id": "Filter dengan 'roll-off' paling curam." },
  { "en": "Apa itu 'riak' (ripple) pada filter?", "id": "Variasi kecil pada 'passband' filter." },
  { "en": "Apa itu 'passband'?", "id": "Rentang frekuensi yang dilewatkan oleh filter." },
  { "en": "Apa itu 'stopband'?", "id": "Rentang frekuensi yang ditahan oleh filter." },
  { "en": "Apa itu 'roll-off'?", "id": "Tingkat kecuraman transisi dari passband ke stopband." },
  { "en": "Apa itu 'noise' dalam sinyal?", "id": "Sinyal acak yang tidak diinginkan." },
  { "en": "Apa itu 'thermal noise'?", "id": "Noise yang dihasilkan oleh gerakan termal elektron." },
  { "en": "Apa itu 'white noise'?", "id": "Noise yang spektrum dayanya datar." },
  { "en": "Apa itu 'pink noise'?", "id": "Noise yang dayanya turun per oktaf." },
  { "en": "Apa itu 'SNR (Signal-to-Noise Ratio)'?", "id": "Rasio daya sinyal terhadap daya noise." },
  { "en": "Apa itu 'noise figure'?", "id": "Ukuran seberapa banyak noise ditambahkan komponen." },
  { "en": "Apa itu 'amplitudo modulasi' (AM)?", "id": "Informasi mengubah amplitudo sinyal pembawa." },
  { "en": "Apa itu 'frekuensi modulasi' (FM)?", "id": "Informasi mengubah frekuensi sinyal pembawa." },
  { "en": "Apa itu 'fasa modulasi' (PM)?", "id": "Informasi mengubah fasa sinyal pembawa." },
  { "en": "Apa itu 'sinyal pembawa' (carrier)?", "id": "Gelombang frekuensi tinggi untuk membawa informasi." },
  { "en": "Apa itu 'sinyal pemodulasi' (modulating signal)?", "id": "Sinyal informasi (misal, suara)." },
  { "en": "Apa itu 'sidebands'?", "id": "Frekuensi baru yang terbentuk akibat modulasi." },
  { "en": "Apa itu 'bandwidth' sinyal AM?", "id": "Dua kali frekuensi pemodulasi tertinggi." },
  { "en": "Apa itu 'bandwidth' sinyal FM?", "id": "Lebih lebar dari AM, ditentukan oleh Aturan Carson." },
  { "en": "Mana yang lebih tahan noise, AM atau FM?", "id": "FM lebih tahan terhadap gangguan noise." },
  { "en": "Apa itu 'penerima superheterodyne'?", "id": "Jenis arsitektur penerima radio yang umum." },
  { "en": "Apa itu 'frekuensi antara' (IF)?", "id": "Frekuensi tetap hasil pencampuran sinyal." },
  { "en": "Apa itu 'osilator lokal' (LO)?", "id": "Osilator internal pada penerima radio." },
  { "en": "Apa itu 'mixer'?", "id": "Rangkaian untuk mencampur dua frekuensi." },
  { "en": "Apa itu 'penerima radio kristal'?", "id": "Penerima radio pasif yang paling sederhana." },
  { "en": "Apa itu 'grounding' RF?", "id": "Pentanahan khusus untuk frekuensi radio." },
  { "en": "Apa itu 'kabel koaksial'?", "id": "Jenis kabel untuk mentransmisikan sinyal RF." },
  { "en": "Apa itu 'konektor BNC'?", "id": "Jenis konektor RF yang umum digunakan." },
  { "en": "Apa itu 'konektor SMA'?", "id": "Konektor RF kecil untuk frekuensi tinggi." },
  { "en": "Apa itu 'terminator' 50-ohm?", "id": "Resistor untuk mengakhiri saluran transmisi." },
  { "en": "Apa itu 'pencocokan impedansi'?", "id": "Menyamakan impedansi sumber dengan beban." },
  { "en": "Mengapa pencocokan impedansi diperlukan?", "id": "Untuk transfer daya maksimum." },
  {
    "en": "Apa itu 'transformator seperempat gelombang'?",
    "id": "Saluran transmisi untuk mencocokan impedansi."
  },
  { "en": "Apa itu 'stub tuning'?", "id": "Teknik pencocokan impedansi menggunakan 'stub'." },
  { "en": "Apa itu 'stub'?", "id": "Potongan saluran transmisi hubung singkat/terbuka." },
  { "en": "Apa itu 'Diagram Smith' (Smith Chart)?", "id": "Alat grafis untuk analisis impedansi RF." },
  { "en": "Apa itu 'gelombang berjalan' (traveling wave)?", "id": "Gelombang yang merambat sepanjang saluran." },
  { "en": "Apa itu 'gelombang berdiri' (standing wave)?", "id": "Hasil interferensi gelombang datang dan pantul." },
  { "en": "Apa itu 'VSWR'?", "id": "Voltage Standing Wave Ratio." },
  { "en": "Apa yang diukur oleh VSWR?", "id": "Tingkat ketidakcocokan impedansi saluran." },
  { "en": "Nilai VSWR yang sempurna berapa?", "id": "Nilai sempurna adalah 1:1." },
  { "en": "Nilai VSWR tinggi artinya apa?", "id": "Banyak daya yang dipantulkan kembali." },
  { "en": "Apa itu 'koefisien pantul' (reflection coefficient)?", "id": "Rasio antara gelombang pantul dan datang." },
  { "en": "Apa itu 'rugi-rugi pantulan' (return loss)?", "id": "Ukuran daya yang hilang akibat pantulan." },
  { "en": "Apa itu 'daya maju' (forward power)?", "id": "Daya yang dikirim dari sumber ke beban." },
  { "en": "Apa itu 'daya pantul' (reflected power)?", "id": "Daya yang dipantulkan dari beban." },
  {
    "en": "Apa yang diukur oleh SWR meter?",
    "id": "Mengukur rasio gelombang berdiri (VSWR)."
  },
  { "en": "Apa itu 'antena'?", "id": "Transduser antara gelombang terbimbing dan bebas." },
  { "en": "Apa fungsi antena pemancar?", "id": "Mengubah sinyal listrik menjadi gelombang radio." },
  { "en": "Apa fungsi antena penerima?", "id": "Mengubah gelombang radio menjadi sinyal listrik." },
  { "en": "Apa itu 'antena dipol'?", "id": "Antena sederhana terdiri dari dua elemen." },
  { "en": "Berapa panjang 'antena dipol setengah gelombang'?", "id": "Panjangnya kira-kira setengah panjang gelombang." },
  { "en": "Apa itu 'impedansi antena'?", "id": "Impedansi yang terlihat di terminal antena." },
  { "en": "Apa itu 'pola radiasi'?", "id": "Pola penyebaran energi dari antena." },
  { "en": "Apa itu 'antena isotropik'?", "id": "Antena ideal yang memancar ke segala arah." },
  { "en": "Apa itu 'gain' antena?", "id": "Ukuran kemampuan antena memfokuskan energi." },
  { "en": "Apa satuan 'gain' antena?", "id": "Desibel isotropik (dBi) atau desibel dipol (dBd)." },
  { "en": "Apa itu 'antena Yagi-Uda'?", "id": "Antena direktif dengan banyak elemen." },
  { "en": "Apa itu 'elemen driven' pada Yagi?", "id": "Elemen yang terhubung ke saluran transmisi." },
  { "en": "Apa itu 'reflektor' pada Yagi?", "id": "Elemen di belakang untuk memantulkan sinyal." },
  { "en": "Apa itu 'direktor' pada Yagi?", "id": "Elemen di depan untuk mengarahkan sinyal." },
  { "en": "Apa itu 'antena parabola'?", "id": "Antena dengan reflektor berbentuk parabola." },
  { "en": "Apa itu 'LNB (Low-Noise Block)'?", "id": "Penguat sinyal di antena parabola." },
  {
    "en": "Apa itu 'polarisasi' gelombang radio?",
    "id": "Orientasi medan listrik gelombang radio."
  },
  { "en": "Apa itu 'polarisasi vertikal'?", "id": "Medan listrik berorientasi vertikal." },
  { "en": "Apa itu 'polarisasi horizontal'?", "id": "Medan listrik berorientasi horizontal." },
  { "en": "Apa itu 'polarisasi sirkular'?", "id": "Medan listrik berputar seiring waktu." },
  { "en": "Apa itu 'ground plane' antena?", "id": "Permukaan konduktif sebagai 'tanah' buatan." },
  { "en": "Apa itu 'antena monopoli'?", "id": "Setengah dari antena dipol di atas ground." },
  { "en": "Apa itu 'balun'?", "id": "Menghubungkan saluran seimbang dan tak seimbang." },
  { "en": "Contoh saluran seimbang (balanced)?", "id": "Kabel 'twin-lead' atau 'ladder line'." },
  { "en": "Contoh saluran tak seimbang (unbalanced)?", "id": "Kabel koaksial." },
  { "en": "Apa itu 'analisis transien'?", "id": "Analisis rangkaian saat terjadi perubahan." },
  { "en": "Apa itu 'kondisi awal' (initial condition)?", "id": "Kondisi tegangan/arus sesaat sebelum perubahan." },
  { "en": "Tegangan pada kapasitor bisa berubah instan?", "id": "Tidak, tegangan kapasitor kontinu." },
  { "en": "Arus pada induktor bisa berubah instan?", "id": "Tidak, arus induktor kontinu." },
  { "en": "Apa itu 'transformasi Laplace'?", "id": "Metode matematika untuk analisis transien." },
  { "en": "Apa itu 'variabel s'?", "id": "Variabel kompleks dalam domain Laplace." },
  { "en": "Bagaimana impedansi kapasitor di domain-s?", "id": "Impedansinya adalah 1/(sC)." },
  { "en": "Bagaimana impedansi induktor di domain-s?", "id": "Impedansinya adalah sL." },
  { "en": "Apa itu 'fungsi transfer' H(s)?", "id": "Rasio output terhadap input di domain-s." },
  { "en": "Apa itu 'kutub' (pole) dari H(s)?", "id": "Nilai s yang membuat H(s) tak terhingga." },
  { "en": "Apa itu 'nol' (zero) dari H(s)?", "id": "Nilai s yang membuat H(s) nol." },
  { "en": "Apa hubungan kutub dengan stabilitas?", "id": "Sistem stabil jika kutub di sisi kiri." },
  {
    "en": "Apa itu 'respons impuls' h(t)?",
    "id": "Output sistem saat inputnya impuls."
  },
  { "en": "Apa itu 'respons tangga' (step response)?", "id": "Output sistem saat inputnya fungsi tangga." },
  { "en": "Apa itu 'konvolusi'?", "id": "Operasi matematika untuk mencari output sistem." },
  { "en": "Apa itu 'sirkuit kopling kritis'?", "id": "Kopling yang memberikan transfer daya maksimum." },
  { "en": "Apa itu 'rangkaian resonansi ganda'?", "id": "Rangkaian dengan dua frekuensi resonansi." },
  {
    "en": "Apa itu 'filter kristal' (crystal filter)?",
    "id": "Filter sangat selektif menggunakan kristal kuarsa."
  },
  { "en": "Apa itu 'efek piezoelektrik'?", "id": "Sifat kristal menghasilkan tegangan saat ditekan." },
  { "en": "Apa itu 'osilator kristal'?", "id": "Osilator sangat stabil menggunakan kristal." },
  { "en": "Apa itu 'filter SAW'?", "id": "Surface Acoustic Wave filter." },
  { "en": "Apa itu 'faktor daya'?", "id": "Rasio daya nyata terhadap daya semu." },
  { "en": "Mengapa koreksi faktor daya diperlukan?", "id": "Untuk mengurangi arus dan rugi-rugi daya." },
  {
    "en": "Apa itu 'denda kVArh' dari PLN?",
    "id": "Denda untuk pemakaian daya reaktif berlebih."
  },
  { "en": "Apa itu 'kapasitor bank otomatis'?", "id": "Kapasitor yang aktif sesuai kebutuhan beban." },
  { "en": "Apa itu 'reaktor shunt'?", "id": "Induktor untuk mengkompensasi efek kapasitif." },
  { "en": "Kapan reaktor shunt digunakan?", "id": "Pada saluran transmisi panjang beban ringan." },
  { "en": "Apa itu 'kapasitor seri'?", "id": "Kapasitor di saluran untuk kompensasi induktif." },
  { "en": "Apa itu 'sistem AC fleksibel' (FACTS)?", "id": "Teknologi untuk mengontrol aliran daya." },
  { "en": "Apa itu 'gelombang persegi'?", "id": "Gelombang non-sinusoidal dengan transisi tajam." },
  {
    "en": "Apa saja komponen frekuensi gelombang persegi?",
    "id": "Fundamental dan harmonisa ganjil."
  },
  { "en": "Apa itu 'deret Fourier'?", "id": "Mendekomposisi sinyal periodik menjadi sinus." },
  { "en": "Apa itu 'koefisien Fourier'?", "id": "Amplitudo dari setiap komponen sinus." },
  { "en": "Apa itu 'simetri setengah gelombang'?", "id": "Sifat sinyal yang paruh keduanya negatif." },
  { "en": "Apa itu 'simetri genap'?", "id": "Sinyal yang simetris terhadap sumbu y." },
  { "en": "Apa itu 'simetri ganjil'?", "id": "Sinyal yang simetris terhadap titik asal." },
  {
    "en": "Apa itu 'daya rata-rata' gelombang non-sinusoidal?",
    "id": "Jumlah daya rata-rata setiap harmonisa."
  },
  { "en": "Apa itu 'nilai RMS' gelombang non-sinusoidal?", "id": "Akar dari jumlah kuadrat RMS harmonisa." },
  {
    "en": "Apa itu 'distorsi harmonik total' (THD)?",
    "id": "Ukuran seberapa terdistorsi sebuah gelombang."
  },
  {
    "en": "Apa itu 'jaringan dua-port' (two-port)?",
    "id": "Model rangkaian dengan sepasang terminal input/output."
  },
  { "en": "Apa itu 'parameter Z' (impedansi)?", "id": "Parameter untuk mengkarakterisasi jaringan dua-port." },
  { "en": "Apa itu 'parameter Y' (admitansi)?", "id": "Parameter lain untuk jaringan dua-port." },
  { "en": "Apa itu 'parameter h' (hibrida)?", "id": "Parameter campuran untuk model transistor." },
  { "en": "Apa itu 'parameter ABCD' (transmisi)?", "id": "Parameter untuk analisis saluran transmisi." },
  {
    "en": "Apa itu 'rangkaian timbal-balik' (reciprocal)?",
    "id": "Jaringan di mana Z12 sama dengan Z21."
  },
  { "en": "Apa itu 'rangkaian simetris'?", "id": "Jaringan di mana Z11 sama dengan Z22." },
  { "en": "Apa itu 'transformasi T-Pi' (Y-Δ)?", "id": "Mengubah topologi jaringan T menjadi Pi." },
  { "en": "Apa itu 'model T' dari saluran transmisi?", "id": "Model saluran transmisi jarak menengah." },
  { "en": "Apa itu 'model Pi' dari saluran transmisi?", "id": "Model lain saluran transmisi jarak menengah." },
  { "en": "Apa itu 'saluran transmisi pendek'?", "id": "Saluran yang efek kapasitansinya diabaikan." },
  { "en": "Apa itu 'saluran transmisi panjang'?", "id": "Saluran yang parameternya terdistribusi." },
  { "en": "Apa itu 'panjang gelombang' (λ)?", "id": "Jarak fisik satu siklus gelombang." },
  {
    "en": "Apa itu 'kecepatan propagasi'?",
    "id": "Kecepatan rambat gelombang di suatu medium."
  },
  { "en": "Apa itu 'permitivitas' (ε)?", "id": "Ukuran respons dielektrik terhadap medan listrik." },
  { "en": "Apa itu 'permeabilitas' (μ)?", "id": "Ukuran respons bahan terhadap medan magnet." },
  { "en": "Apa itu 'permitivitas relatif' (εr)?", "id": "Rasio permitivitas bahan terhadap vakum." },
  { "en": "Apa itu 'konstanta dielektrik'?", "id": "Nama lain untuk permitivitas relatif." },
  { "en": "Apa itu 'rugi-rugi dielektrik'?", "id": "Energi yang hilang sebagai panas di dielektrik." },
  { "en": "Apa itu 'faktor disipasi' (tan δ)?", "id": "Ukuran rugi-rugi dielektrik pada kapasitor." },
  { "en": "Apa itu 'arus perpindahan' (displacement current)?", "id": "Arus yang mengalir melalui kapasitor." },
  { "en": "Apa itu 'Hukum Ampere-Maxwell'?", "id": "Hukum Ampere yang dimodifikasi oleh Maxwell." },
  { "en": "Apa itu 'Persamaan Maxwell'?", "id": "Empat persamaan dasar dari elektromagnetisme." },
  { "en": "Apa itu 'gelombang elektromagnetik'?", "id": "Getaran medan listrik dan magnet yang merambat." },
  { "en": "Apa kecepatan gelombang elektromagnetik di vakum?", "id": "Kecepatan cahaya (sekitar 3x10⁸ m/s)." },
  { "en": "Apa itu 'spektrum elektromagnetik'?", "id": "Rentang semua frekuensi gelombang elektromagnetik." },
  { "en": "Contoh gelombang elektromagnetik?", "id": "Cahaya tampak, gelombang radio, sinar-X." },
  { "en": "Apa itu 'vektor Poynting'?", "id": "Vektor yang merepresentasikan aliran energi EM." },
  { "en": "Apa itu 'analisis fasor'?", "id": "Metode analisis AC menggunakan fasor." },
  { "en": "Apa keuntungan analisis fasor?", "id": "Mengubah persamaan diferensial menjadi aljabar." },
  { "en": "Apa itu 'rangkaian setara Thevenin' AC?", "id": "Satu sumber tegangan AC dan satu impedansi seri." },
  { "en": "Apa itu 'rangkaian setara Norton' AC?", "id": "Satu sumber arus AC dan satu impedansi paralel." },
  { "en": "Bagaimana cara menggabungkan sumber tegangan AC seri?", "id": "Jumlahkan fasor tegangannya." },
  { "en": "Bagaimana cara menggabungkan sumber arus AC paralel?", "id": "Jumlahkan fasor arusnya." },
  { "en": "Apa itu 'ground loop'?", "id": "Masalah noise akibat adanya banyak jalur ground." },
  { "en": "Bagaimana 'ground loop' dihindari?", "id": "Menggunakan teknik pentanahan bintang (star ground)." },
  { "en": "Apa itu 'arus bocor' (leakage current)?", "id": "Arus kecil yang mengalir melalui isolasi." },
  { "en": "Apa itu 'tes resistansi isolasi'?", "id": "Mengukur resistansi isolasi dengan tegangan tinggi." },
  { "en": "Alat apa yang digunakan untuk tes isolasi?", "id": "Insulation tester atau Megger." },
  { "en": "Apa itu 'transformator instrumen'?", "id": "Trafo CT dan PT untuk pengukuran." },
  { "en": "Apa itu 'beban hantu' (phantom load)?", "id": "Daya yang dikonsumsi perangkat saat standby." },
  { "en": "Apa itu 'vampire power'?", "id": "Nama lain untuk beban hantu." },
  { "en": "Apa itu 'faktor bentuk' (form factor)?", "id": "Rasio nilai RMS terhadap nilai rata-rata." },
  { "en": "Apa itu 'faktor puncak' (crest factor)?", "id": "Rasio nilai puncak terhadap nilai RMS." },
  { "en": "Berapa 'crest factor' untuk gelombang sinus?", "id": "Nilainya adalah akar dua (~1.414)." },
  { "en": "Apa itu 'catu daya tak terinterupsi' (UPS)?", "id": "Menyediakan daya darurat saat listrik padam." },
  { "en": "Apa itu 'UPS online'?", "id": "Selalu menyuplai daya melalui baterai dan inverter." },
  { "en": "Apa itu 'UPS offline' (standby)?", "id": "Beralih ke baterai saat listrik padam." },
  { "en": "Apa itu 'UPS line-interactive'?", "id": "UPS offline dengan regulator tegangan." },
  { "en": "Apa itu 'inverter gelombang sinus murni'?", "id": "Inverter yang menghasilkan gelombang sinus ideal." },
  { "en": "Apa itu 'inverter gelombang kotak'?", "id": "Inverter yang menghasilkan gelombang kotak." },
  { "en": "Apa itu 'motor universal'?", "id": "Motor yang bisa beroperasi pada AC dan DC." },
  { "en": "Apa itu 'kapasitor start' pada motor?", "id": "Memberikan torsi awal untuk motor satu fasa." },
  { "en": "Apa itu 'kapasitor run' pada motor?", "id": "Memperbaiki efisiensi motor saat berjalan." },
  { "en": "Apa itu 'saklar sentrifugal' pada motor?", "id": "Memutus kapasitor start setelah motor berputar." },
  { "en": "Apa itu 'belitan bantu' (auxiliary winding)?", "id": "Belitan kedua untuk memulai motor satu fasa." },
  { "en": "Apa itu 'motor fasa-terpisah' (split-phase)?", "id": "Jenis motor induksi satu fasa." },
  { "en": "Apa itu 'motor shaded-pole'?", "id": "Motor satu fasa kecil untuk kipas angin." },
  { "en": "Apa itu 'rotor sangkar tupai'?", "id": "Jenis rotor yang paling umum pada motor induksi." },
  { "en": "Apa itu 'stator'?", "id": "Bagian motor yang diam." },
  { "en": "Apa itu 'rotor'?", "id": "Bagian motor yang berputar." },
  { "en": "Apa itu 'celah udara' (air gap)?", "id": "Celah antara stator dan rotor." },
  { "en": "Apa itu 'daya kuda' (horsepower)?", "id": "Satuan daya, sekitar 746 Watt." },
  { "en": "Apa itu 'torsi'?", "id": "Gaya putar yang dihasilkan oleh motor." },
  { "en": "Apa itu 'kecepatan sinkron'?", "id": "Kecepatan putaran medan magnet stator." },
  { "en": "Apa itu 'slip'?", "id": "Perbedaan persentase antara kecepatan sinkron dan rotor." },
  { "en": "Nilai slip untuk motor induksi?", "id": "Selalu lebih besar dari nol." },
  { "en": "Apa itu 'pengereman regeneratif'?", "id": "Motor berfungsi sebagai generator untuk mengerem." },
  { "en": "Apa itu 'pengereman dinamis'?", "id": "Menggunakan resistor untuk membuang energi pengereman." },
  { "en": "Apa itu 'soft starter'?", "id": "Alat untuk mengurangi arus start motor." },
  { "en": "Apa itu 'arus start' (inrush current) motor?", "id": "Arus sangat tinggi saat motor mulai berputar." },
  { "en": "Berapa besar arus start motor?", "id": "Bisa 5-8 kali arus normalnya." },
  { "en": "Apa itu 'proteksi beban lebih' (overload)?", "id": "Proteksi saat motor menarik arus berlebih." },
  { "en": "Apa itu 'thermal overload relay'?", "id": "Relai yang trip berdasarkan suhu motor." },
  { "en": "Apa itu 'nameplate' motor?", "id": "Pelat berisi spesifikasi teknis motor." },
  { "en": "Apa itu 'kelas isolasi' motor?", "id": "Menunjukkan batas suhu operasi maksimum." },
  { "en": "Apa itu 'siklus kerja' (duty cycle) motor?", "id": "Pola operasi motor (kontinu atau intermiten)." },
  { "en": "Apa itu 'service factor' motor?", "id": "Faktor pengali daya yang diizinkan." },
  { "en": "Apa itu 'koneksi bintang-segitiga' (star-delta)?", "id": "Metode untuk mengurangi arus start motor." },
  { "en": "Apa itu 'rangkaian kontrol'?", "id": "Rangkaian untuk mengontrol operasi motor." },
  { "en": "Apa itu 'rangkaian daya'?", "id": "Rangkaian yang menyuplai daya ke motor." },
  { "en": "Apa itu 'kontaktor'?", "id": "Relai daya tinggi untuk menyalakan motor." },
  { "en": "Apa itu 'tombol tekan' (push button)?", "id": "Saklar sesaat untuk kontrol." },
  { "en": "Apa itu 'rangkaian pengunci' (latching circuit)?", "id": "Rangkaian untuk menjaga kontaktor tetap aktif." },
  { "en": "Apa itu 'interlock'?", "id": "Mekanisme pengaman untuk mencegah dua aksi berlawanan." },
  { "en": "Apa itu 'lampu pilot'?", "id": "Lampu indikator untuk status operasi." },
  { "en": "Apa itu 'diagram tangga' (ladder diagram)?", "id": "Diagram skematik untuk logika kontrol relai." },
  { "en": "Apa itu 'PLC (Programmable Logic Controller)'?", "id": "Komputer industri untuk otomasi." },
  { "en": "Apa itu 'harmonisa orde ketiga'?", "id": "Harmonisa dengan frekuensi 3 kali fundamental." },
  { "en": "Masalah harmonisa orde ketiga di sistem 3 fasa?", "id": "Menyebabkan arus netral yang sangat tinggi." },
  { "en": "Apa itu 'filter harmonik'?", "id": "Filter untuk mengurangi atau menghilangkan harmonisa." },
  { "en": "Apa itu 'filter pasif' harmonik?", "id": "Menggunakan kombinasi L dan C." },
  { "en": "Apa itu 'filter aktif' harmonik?", "id": "Menggunakan elektronika daya untuk membatalkan harmonisa." },
  { "en": "Apa itu 'K-factor' pada transformator?", "id": "Menunjukkan kemampuan trafo menangani beban harmonisa." },
  { "en": "Apa itu 'pemanasan berlebih' (overheating)?", "id": "Masalah umum akibat arus harmonisa." },
  { "en": "Apa itu 'resonansi paralel' harmonik?", "id": "Kondisi berbahaya akibat interaksi kapasitor dan jala-jala." },
  { "en": "Apa itu 'titik kopling bersama' (PCC)?", "id": "Titik di mana konsumen terhubung ke jala-jala." },
  { "en": "Standar apa yang mengatur harmonisa?", "id": "Standar IEEE 519." },
  { "en": "Apa itu 'flicker' tegangan?", "id": "Variasi tegangan yang menyebabkan lampu berkedip." },
  { "en": "Apa itu 'transien' tegangan?", "id": "Perubahan tegangan sangat cepat dan singkat." },
  { "en": "Penyebab transien tegangan?", "id": "Sambaran petir, switching, atau gangguan." },
  { "en": "Apa itu 'MOV (Metal Oxide Varistor)'?", "id": "Komponen untuk memotong lonjakan tegangan transien." },
  { "en": "Apa itu 'arrester' petir?", "id": "Perangkat untuk melindungi dari sambaran petir." },
  { "en": "Apa itu 'pentanahan' (grounding/earthing)?", "id": "Koneksi ke bumi untuk keamanan." },
  { "en": "Apa itu 'ikatan ekuipotensial' (equipotential bonding)?", "id": "Menghubungkan semua bagian logam untuk potensial sama." },
  { "en": "Apa itu 'resistansi pentanahan'?", "id": "Resistansi antara elektroda tanah dan bumi." },
  { "en": "Nilai resistansi pentanahan yang baik?", "id": "Nilainya serendah mungkin (misal, < 5 Ohm)." },
  { "en": "Apa itu 'elektroda batang' (rod electrode)?", "id": "Batang logam yang ditanam untuk pentanahan." },
  { "en": "Apa itu 'sistem pentanahan TT'?", "id": "Sistem di mana sumber dan beban dibumikan terpisah." },
  { "en": "Apa itu 'sistem pentanahan TN'?", "id": "Sistem di mana sumber dibumikan dan bodi dihubungkan." },
  { "en": "Apa itu 'sistem pentanahan IT'?", "id": "Sistem di mana sumber tidak dibumikan langsung." },
  { "en": "Apa itu 'frekuensi resonansi'?", "id": "Frekuensi saat reaktansi induktif dan kapasitif sama." },
  { "en": "Impedansi pada resonansi seri?", "id": "Mencapai nilai minimum, sama dengan R." },
  { "en": "Impedansi pada resonansi paralel?", "id": "Mencapai nilai maksimum." },
  { "en": "Faktor daya pada saat resonansi?", "id": "Sama dengan 1 (unity power factor)." },
  { "en": "Apa itu 'faktor kualitas' (Q)?", "id": "Ukuran ketajaman kurva resonansi." },
  { "en": "Faktor Q tinggi berarti apa?", "id": "Resonansi sangat selektif dan tajam." },
  { "en": "Apa itu 'bandwidth'?", "id": "Lebar frekuensi di mana daya setengah." },
  { "en": "Hubungan antara Q dan bandwidth?", "id": "Bandwidth berbanding terbalik dengan faktor Q." },
  { "en": "Apa itu 'filter lolos-rendah'?", "id": "Melewatkan frekuensi rendah, memblokir tinggi." },
  { "en": "Apa itu 'filter lolos-tinggi'?", "id": "Melewatkan frekuensi tinggi, memblokir rendah." },
  { "en": "Apa itu 'filter lolos-pita'?", "id": "Hanya melewatkan rentang frekuensi tertentu." },
  { "en": "Apa itu 'filter stop-pita'?", "id": "Memblokir rentang frekuensi tertentu." },
  { "en": "Apa itu 'frekuensi cut-off'?", "id": "Frekuensi di mana gain turun 3 dB." },
  { "en": "Apa itu 'orde' dari sebuah filter?", "id": "Menentukan kecuraman 'roll-off' filter." },
  { "en": "Apa itu 'roll-off'?", "id": "Tingkat atenuasi di luar passband." },
  { "en": "Apa itu 'transformator inti udara'?", "id": "Trafo tanpa inti feromagnetik." },
  { "en": "Apa itu 'transformator inti besi'?", "id": "Trafo dengan inti besi laminasi." },
  { "en": "Apa itu 'transformator toroidal'?", "id": "Trafo dengan inti berbentuk donat." },
  { "en": "Keuntungan trafo toroidal?", "id": "Medan magnet liar lebih sedikit." },
  { "en": "Apa itu 'induktansi bocor' (leakage)?", "id": "Fluks magnet yang tidak terkopling." },
  { "en": "Apa itu 'kapasitansi antar lilitan'?", "id": "Kapasitansi parasitik antara lilitan trafo." },
  { "en": "Apa itu 'arus masuk' (inrush current) trafo?", "id": "Arus sesaat yang tinggi saat dinyalakan." },
  { "en": "Apa itu 'saturasi inti'?", "id": "Kondisi inti tidak dapat menyimpan fluks lagi." },
  { "en": "Apa itu 'koneksi delta-wye'?", "id": "Koneksi trafo tiga fasa." },
  { "en": "Apa itu 'pergeseran fasa' pada trafo delta-wye?", "id": "Menghasilkan pergeseran fasa 30 derajat." },
  { "en": "Apa itu 'transformator zig-zag'?", "id": "Trafo khusus untuk grounding atau harmonisa." },
  { "en": "Apa itu 'ketukan' (tap) pada trafo?", "id": "Koneksi tambahan untuk mengubah rasio lilitan." },
  { "en": "Apa itu 'tap changer'?", "id": "Mekanisme untuk mengubah tap pada trafo." },
  { "en": "Apa itu 'on-load tap changer' (OLTC)?", "id": "Bisa mengubah tap saat trafo berbeban." },
  { "en": "Apa itu 'minyak trafo'?", "id": "Sebagai pendingin dan isolator dalam trafo." },
  { "en": "Apa itu 'uji gas terlarut' (DGA)?", "id": "Menganalisis gas dalam minyak trafo." },
  { "en": "Apa itu 'konservator' pada trafo?", "id": "Tangki ekspansi untuk minyak trafo." },
  { "en": "Apa itu 'relai Buchholz'?", "id": "Relai proteksi untuk trafo terendam minyak." },
  { "en": "Apa itu 'pernapasan' (breather) trafo?", "id": "Filter udara untuk konservator." },
  { "en": "Apa isi dari 'breather'?", "id": "Berisi silika gel untuk menyerap kelembaban." },
  { "en": "Apa itu 'bushing' pada trafo?", "id": "Isolator untuk mengeluarkan terminal tegangan tinggi." },
  { "en": "Apa itu 'pendinginan ONAN'?", "id": "Oil Natural Air Natural." },
  { "en": "Apa itu 'pendinginan ONAF'?", "id": "Oil Natural Air Forced." },
  { "en": "Apa itu 'kelas suhu' trafo?", "id": "Menunjukkan batas kenaikan suhu yang diizinkan." },
  { "en": "Apa itu 'diagram fasor'?", "id": "Diagram yang merepresentasikan fasor tegangan/arus." },
  { "en": "Mengapa menggunakan diagram fasor?", "id": "Memudahkan visualisasi hubungan fasa." },
  { "en": "Apa itu 'lokus impedansi'?", "id": "Grafik impedansi sebagai fungsi frekuensi." },
  { "en": "Apa itu 'lingkaran admintansi'?", "id": "Lokus admintansi untuk rangkaian paralel." },
  { "en": "Apa itu 'daya aktif'?", "id": "Nama lain untuk daya nyata (P)." },
  { "en": "Apa itu 'daya tak berguna'?", "id": "Nama lain untuk daya reaktif (Q)." },
  { "en": "Apa itu 'kompensasi daya reaktif'?", "id": "Nama lain untuk koreksi faktor daya." },
  { "en": "Apa itu 'kondensor sinkron'?", "id": "Motor sinkron untuk kompensasi daya reaktif." },
  { "en": "Apa itu 'SVC (Static VAR Compensator)'?", "id": "Alat elektronik untuk kompensasi daya reaktif." },
  { "en": "Apa itu 'STATCOM'?", "id": "SVC berbasis inverter." },
  { "en": "Apa itu 'sistem tiga-kawat'?", "id": "Sistem tiga fasa tanpa netral." },
  { "en": "Apa itu 'sistem empat-kawat'?", "id": "Sistem tiga fasa dengan netral." },
  { "en": "Tegangan fasa ke netral berapa?", "id": "Tegangan line dibagi dengan akar tiga." },
  { "en": "Apa itu 'beban tak seimbang'?", "id": "Impedansi pada setiap fasa tidak sama." },
  { "en": "Akibat dari beban tak seimbang?", "id": "Munculnya arus di kawat netral." },
  { "en": "Apa itu 'komponen urutan nol'?", "id": "Komponen yang ada pada gangguan tak seimbang." },
  { "en": "Apa itu 'transformator urutan nol'?", "id": "Trafo untuk mengukur arus urutan nol." },
  { "en": "Apa itu 'ground fault'?", "id": "Hubung singkat dari fasa ke tanah." },
  { "en": "Apa itu 'line-to-line fault'?", "id": "Hubung singkat antara dua fasa." },
  { "en": "Apa itu 'three-phase fault'?", "id": "Hubung singkat antara ketiga fasa." },
  { "en": "Jenis gangguan mana yang paling sering terjadi?", "id": "Gangguan fasa-ke-tanah (ground fault)." },
  { "en": "Jenis gangguan mana yang paling parah?", "id": "Gangguan tiga fasa." },
  { "en": "Apa itu 'studi hubung singkat'?", "id": "Analisis untuk menghitung arus gangguan." },
  { "en": "Apa itu 'impedansi urutan nol'?", "id": "Impedansi rangkaian terhadap arus urutan nol." },
  { "en": "Apa itu 'impedansi urutan positif'?", "id": "Impedansi rangkaian terhadap arus urutan positif." },
  { "en": "Apa itu 'impedansi urutan negatif'?", "id": "Impedansi rangkaian terhadap arus urutan negatif." },
  { "en": "Impedansi urutan positif dan negatif sama?", "id": "Biasanya sama untuk komponen statis." },
  { "en": "Apa itu 'analisis aliran daya' (power flow)?", "id": "Analisis aliran daya dalam jaringan listrik." },
  { "en": "Apa itu 'bus slack'?", "id": "Bus referensi dalam analisis aliran daya." },
  { "en": "Apa itu 'bus PQ'?", "id": "Bus di mana daya P dan Q diketahui." },
  { "en": "Apa itu 'bus PV'?", "id": "Bus di mana daya P dan V diketahui." },
  { "en": "Apa itu 'metode Newton-Raphson'?", "id": "Metode iteratif untuk analisis aliran daya." },
  { "en": "Apa itu 'metode Gauss-Seidel'?", "id": "Metode iteratif lain untuk aliran daya." },
  { "en": "Apa itu 'matriks admitansi bus' (Ybus)?", "id": "Matriks yang merepresentasikan jaringan listrik." },
  { "en": "Apa itu 'stabilitas transien'?", "id": "Kemampuan sistem pulih dari gangguan besar." },
  { "en": "Apa itu 'sudut daya' (power angle)?", "id": "Sudut antara tegangan internal generator." },
  { "en": "Apa itu 'kurva sudut-daya'?", "id": "Kurva yang menunjukkan hubungan daya dan sudut." },
  { "en": "Apa itu 'kriteria area sama'?", "id": "Metode grafis untuk analisis stabilitas transien." },
  { "en": "Apa itu 'waktu pemutusan kritis'?", "id": "Waktu maksimum untuk menghilangkan gangguan." },
  { "en": "Apa itu 'sistem eksitasi' generator?", "id": "Sistem yang menyediakan arus medan DC." },
  { "en": "Apa itu 'pengatur tegangan otomatis' (AVR)?", "id": "Mengatur tegangan terminal generator." },
  { "en": "Apa itu 'stabilisator sistem tenaga' (PSS)?", "id": "Meningkatkan redaman osilasi daya." },
  { "en": "Apa itu 'osilasi daya'?", "id": "Ayun daya antara generator-generator." },
  { "en": "Apa itu 'penjatuhan frekuensi' (frequency droop)?", "id": "Karakteristik kontrol gubernur turbin." },
  { "en": "Apa itu 'kontrol pembangkitan otomatis' (AGC)?", "id": "Mengatur output generator untuk menjaga frekuensi." },
  { "en": "Apa itu 'operasi ekonomis' (economic dispatch)?", "id": "Membagi beban antar generator secara ekonomis." },
  { "en": "Apa itu 'biaya bahan bakar inkremental'?", "id": "Biaya untuk menghasilkan satu megawatt tambahan." },
  { "en": "Apa itu 'rugi-rugi transmisi'?", "id": "Daya yang hilang pada saluran transmisi." },
  { "en": "Apa itu 'faktor penalti'?", "id": "Faktor yang memperhitungkan rugi-rugi transmisi." },
  { "en": "Apa itu 'unit commitment'?", "id": "Menjadwalkan unit pembangkit mana yang beroperasi." },
  { "en": "Apa itu 'cadangan berputar' (spinning reserve)?", "id": "Kapasitas pembangkit online yang tidak terpakai." },
  { "en": "Apa itu 'pasar listrik'?", "id": "Mekanisme jual beli energi listrik." },
  { "en": "Apa itu 'operator sistem independen' (ISO)?", "id": "Entitas yang mengelola jaringan transmisi." },
  { "en": "Apa itu 'HVDC (High Voltage DC)'?", "id": "Transmisi daya menggunakan tegangan tinggi DC." },
  { "en": "Keuntungan transmisi HVDC?", "id": "Rugi-rugi lebih kecil untuk jarak sangat jauh." },
  { "en": "Apa itu 'stasiun konverter'?", "id": "Mengubah AC ke DC dan sebaliknya." },
  { "en": "Apa itu 'thyristor'?", "id": "Komponen semikonduktor untuk switching daya tinggi." },
  { "en": "Apa itu 'IGBT'?", "id": "Insulated Gate Bipolar Transistor." },
  { "en": "Apa itu 'GTO (Gate Turn-Off Thyristor)'?", "id": "Thyristor yang bisa dimatikan lewat gerbang." },
  { "en": "Apa itu 'elektronika daya'?", "id": "Aplikasi elektronika untuk konversi dan kontrol daya." },
  { "en": "Apa itu 'penyearah terkendali'?", "id": "Penyearah yang tegangan outputnya bisa diatur." },
  { "en": "Apa itu 'sudut penyulutan' (firing angle)?", "id": "Sudut di mana thyristor mulai menyala." },
  { "en": "Apa itu 'inverter sumber tegangan' (VSI)?", "id": "Inverter yang disuplai oleh sumber tegangan DC." },
  { "en": "Apa itu 'inverter sumber arus' (CSI)?", "id": "Inverter yang disuplai oleh sumber arus DC." },
  { "en": "Apa itu 'PWM (Pulse Width Modulation)'?", "id": "Teknik modulasi lebar pulsa." },
  { "en": "Bagaimana PWM menghasilkan tegangan AC?", "id": "Dengan mengubah-ubah lebar pulsa DC." },
  { "en": "Apa itu 'SPWM (Sinusoidal PWM)'?", "id": "PWM yang lebar pulsanya mengikuti fungsi sinus." },
  { "en": "Apa itu 'chopper' DC?", "id": "Sirkuit untuk mengubah tegangan DC ke level lain." },
  { "en": "Apa itu 'buck converter'?", "id": "Chopper untuk menurunkan tegangan DC." },
  { "en": "Apa itu 'boost converter'?", "id": "Chopper untuk menaikkan tegangan DC." },
  { "en": "Apa itu 'buck-boost converter'?", "id": "Bisa menaikkan atau menurunkan tegangan DC." },
  { "en": "Apa itu 'Cuk converter'?", "id": "Jenis konverter DC-DC dengan polaritas terbalik." },
  { "en": "Apa itu 'mode konduksi kontinu' (CCM)?", "id": "Arus induktor tidak pernah nol." },
  { "en": "Apa itu 'mode konduksi diskontinu' (DCM)?", "id": "Arus induktor sempat menjadi nol." },
  { "en": "Apa itu 'riak' (ripple) pada konverter?", "id": "Variasi kecil pada tegangan atau arus." },
  { "en": "Apa itu 'siklus kerja' (duty cycle)?", "id": "Rasio waktu ON terhadap total periode." },
  { "en": "Apa itu 'soft switching'?", "id": "Teknik switching saat tegangan/arus nol." },
  { "en": "Keuntungan 'soft switching'?", "id": "Mengurangi rugi-rugi switching, efisiensi tinggi." },
  { "en": "Apa itu 'zero-voltage switching' (ZVS)?", "id": "Switching saat tegangan melintasi saklar nol." },
  { "en": "Apa itu 'zero-current switching' (ZCS)?", "id": "Switching saat arus melalui saklar nol." },
  { "en": "Apa itu 'catu daya switching' (SMPS)?", "id": "Catu daya yang menggunakan switching frekuensi tinggi." },
  { "en": "Keuntungan SMPS dibanding linier?", "id": "Lebih efisien, kecil, dan ringan." },
  { "en": "Kelemahan SMPS?", "id": "Lebih kompleks dan menghasilkan noise (EMI)." },
  { "en": "Apa itu 'topologi flyback'?", "id": "Topologi SMPS terisolasi yang sederhana." },
  { "en": "Apa itu 'topologi forward'?", "id": "Topologi SMPS terisolasi untuk daya lebih tinggi." },
  { "en": "Apa itu 'topologi push-pull'?", "id": "Topologi SMPS menggunakan dua saklar." },
  { "en": "Apa itu 'topologi half-bridge'?", "id": "Topologi SMPS untuk daya menengah." },
  { "en": "Apa itu 'topologi full-bridge'?", "id": "Topologi SMPS untuk daya sangat tinggi." },
  { "en": "Apa itu 'umpan balik' (feedback) pada SMPS?", "id": "Untuk menjaga tegangan output tetap stabil." },
  { "en": "Komponen apa yang digunakan untuk umpan balik terisolasi?", "id": "Optocoupler atau transformator kecil." },
  { "en": "Apa itu 'snubber circuit'?", "id": "Sirkuit untuk melindungi saklar dari lonjakan." },
  { "en": "Apa itu 'pemanas induksi'?", "id": "Memanaskan logam menggunakan medan magnet AC." },
  { "en": "Apa itu 'las busur listrik'?", "id": "Menggunakan busur listrik untuk menyambung logam." },
  { "en": "Apa itu 'tanur busur listrik'?", "id": "Tanur untuk melebur baja menggunakan busur listrik." },
  { "en": "Apa itu 'penggerak motor AC' (AC drive)?", "id": "Nama lain untuk VFD (Variable Frequency Drive)." },
  { "en": "Apa itu 'kontrol V/Hz'?", "id": "Metode kontrol sederhana untuk AC drive." },
  { "en": "Apa itu 'kontrol vektor' (vector control)?", "id": "Metode kontrol motor AC performa tinggi." },
  { "en": "Apa itu 'kontrol torsi langsung' (DTC)?", "id": "Metode kontrol motor AC sangat cepat." },
  { "en": "Apa itu 'rem regeneratif'?", "id": "Mengembalikan energi pengereman ke suplai." },
  { "en": "Apa itu 'penerangan' (lighting)?", "id": "Aplikasi penting dari energi listrik." },
  { "en": "Apa itu 'lampu pijar' (incandescent)?", "id": "Menghasilkan cahaya dari filamen panas." },
  { "en": "Apa itu 'lampu fluoresen' (neon)?", "id": "Menggunakan gas dan lapisan fosfor." },
  { "en": "Apa itu 'ballast' pada lampu neon?", "id": "Komponen untuk membatasi arus dan start." },
  { "en": "Apa itu 'ballast elektronik'?", "id": "Ballast modern yang lebih efisien." },
  { "en": "Apa itu 'LED (Light Emitting Diode)'?", "id": "Dioda semikonduktor yang memancarkan cahaya." },
  { "en": "Keuntungan lampu LED?", "id": "Sangat efisien dan umurnya panjang." },
  { "en": "Apa itu 'driver' LED?", "id": "Catu daya khusus untuk lampu LED." },
  { "en": "Apa itu 'lumen'?", "id": "Satuan fluks cahaya (jumlah cahaya)." },
  { "en": "Apa itu 'lux'?", "id": "Satuan iluminansi (cahaya per area)." },
  { "en": "Apa itu 'efikasi luminous'?", "id": "Ukuran efisiensi lampu (lumen per Watt)." },
  { "en": "Apa itu 'suhu warna' (color temperature)?", "id": "Warna cahaya yang dipancarkan (misal, warm white)." },
  { "en": "Apa itu 'indeks rendering warna' (CRI)?", "id": "Kemampuan lampu menunjukkan warna asli objek." },
  { "en": "Apa itu 'lampu HID' (High-Intensity Discharge)?", "id": "Lampu yang menggunakan busur listrik dalam gas." },
  { "en": "Contoh lampu HID?", "id": "Merkuri, sodium, metal halide." },
  { "en": "Apa itu 'sistem pemanasan' (heating)?", "id": "Menggunakan listrik untuk menghasilkan panas." },
  { "en": "Apa itu 'pemanasan resistif'?", "id": "Panas dihasilkan dari aliran arus di resistor." },
  { "en": "Apa itu 'pemanasan dielektrik'?", "id": "Memanaskan bahan isolator dengan medan listrik." },
  { "en": "Apa itu 'pemanasan inframerah'?", "id": "Memanaskan menggunakan radiasi inframerah." },
  { "en": "Apa itu 'AC (Air Conditioning)'?", "id": "Sistem pendingin udara." },
  { "en": "Apa prinsip dasar AC?", "id": "Siklus refrigerasi kompresi uap." },
  { "en": "Apa itu 'refrigeran'?", "id": "Zat pendingin yang bersirkulasi dalam sistem." },
  { "en": "Apa itu 'kompresor'?", "id": "Memompa dan menekan refrigeran." },
  { "en": "Apa itu 'kondensor'?", "id": "Membuang panas dari refrigeran ke luar." },
  { "en": "Apa itu 'katup ekspansi'?", "id": "Menurunkan tekanan dan suhu refrigeran." },
  { "en": "Apa itu 'evaporator'?", "id": "Menyerap panas dari dalam ruangan." },
  { "en": "Apa itu 'AC inverter'?", "id": "AC yang kecepatan kompresornya bisa diatur." },
  { "en": "Keuntungan AC inverter?", "id": "Lebih hemat energi dan suhunya stabil." },
  { "en": "Apa itu 'koefisien performa' (COP)?", "id": "Ukuran efisiensi sistem pendingin/pemanas." },
  { "en": "Apa itu 'rating EER'?", "id": "Energy Efficiency Ratio." },
  { "en": "Apa itu 'pompa kalor' (heat pump)?", "id": "AC yang bisa membalik siklusnya untuk memanaskan." },
  { "en": "Apa itu 'grounding'?", "id": "Koneksi ke bumi untuk keamanan." },
  { "en": "Apa itu 'ikatan' (bonding)?", "id": "Menghubungkan semua bagian logam menjadi satu." },
  { "en": "Apa itu 'proteksi diferensial' (RCD/GFCI)?", "id": "Mendeteksi arus bocor kecil ke tanah." },
  { "en": "Berapa arus bocor yang dideteksi RCD?", "id": "Biasanya sekitar 30 miliampere." },
  { "en": "Apa itu 'sengatan listrik'?", "id": "Efek fisiologis akibat aliran arus listrik." },
  { "en": "Faktor bahaya sengatan listrik?", "id": "Besar arus, durasi, dan jalur aliran." },
  { "en": "Batas aman arus yang melewati tubuh?", "id": "Di bawah 10 miliampere." },
  { "en": "Apa itu 'fibrilasi ventrikel'?", "id": "Kontraksi jantung tak teratur yang mematikan." },
  { "en": "Apa itu 'busur api' (arc flash)?", "id": "Ledakan energi akibat hubung singkat." },
  { "en": "Bahaya dari busur api?", "id": "Suhu sangat tinggi, tekanan, cahaya intens." },
  { "en": "Apa itu 'APD (Alat Pelindung Diri)'?", "id": "Peralatan untuk keselamatan kerja listrik." },
  { "en": "Apa itu 'standar IEC'?", "id": "Standar kelistrikan internasional." },
  { "en": "Apa itu 'standar NEMA'?", "id": "Standar kelistrikan Amerika Utara." },
  { "en": "Apa itu 'PUIL'?", "id": "Persyaratan Umum Instalasi Listrik di Indonesia." },
  { "en": "Apa itu 'kelas proteksi IP'?", "id": "Ingress Protection, proteksi terhadap benda padat/cair." },
  { "en": "Apa itu 'kelas peralatan listrik' (I, II, III)?", "id": "Klasifikasi keamanan berdasarkan isolasi." },
  { "en": "Apa itu 'peralatan kelas I'?", "id": "Memerlukan koneksi ke ground pengaman." },
  { "en": "Apa itu 'peralatan kelas II'?", "id": "Memiliki isolasi ganda, tidak perlu ground." },
  { "en": "Apa itu 'peralatan kelas III'?", "id": "Beroperasi pada tegangan ekstra rendah (SELV)." },
  { "en": "Apa itu 'SELV'?", "id": "Separated Extra-Low Voltage." },
  { "en": "Berapa batas tegangan SELV?", "id": "Biasanya di bawah 50V AC atau 120V DC." },
  { "en": "Apa itu 'listrik tiga fasa'?", "id": "Sistem listrik AC dengan tiga gelombang." },
  { "en": "Berapa beda fasa antar gelombang?", "id": "Beda fasanya 120 derajat." },
  { "en": "Apa itu koneksi 'bintang' (wye)?", "id": "Ujung kumparan terhubung di titik netral." },
  { "en": "Apa itu koneksi 'segitiga' (delta)?", "id": "Kumparan terhubung dari ujung ke ujung." },
  { "en": "Apa itu 'tegangan line'?", "id": "Tegangan antara dua jalur fasa." },
  { "en": "Apa itu 'tegangan fasa'?", "id": "Tegangan pada satu kumparan." },
  { "en": "Hubungan tegangan di koneksi bintang?", "id": "V_line sama dengan akar 3 V_fasa." },
  { "en": "Hubungan arus di koneksi bintang?", "id": "I_line sama dengan I_fasa." },
  { "en": "Hubungan tegangan di koneksi segitiga?", "id": "V_line sama dengan V_fasa." },
  { "en": "Hubungan arus di koneksi segitiga?", "id": "I_line sama dengan akar 3 I_fasa." },
  { "en": "Apa itu 'beban seimbang'?", "id": "Impedansi di ketiga fasa sama." },
  { "en": "Arus di netral pada beban seimbang?", "id": "Arus di netral bernilai nol." },
  { "en": "Apa itu 'urutan fasa'?", "id": "Urutan gelombang mencapai puncaknya (ABC)." },
  { "en": "Bagaimana membalik putaran motor 3 fasa?", "id": "Tukar dua dari tiga koneksi fasa." },
  { "en": "Apa itu 'medan magnet putar'?", "id": "Medan magnet yang berputar dari sistem 3 fasa." },
  { "en": "Rumus daya total 3 fasa?", "id": "P = akar 3 x V_line x I_line x PF." },
  { "en": "Apa itu metode 'dua wattmeter'?", "id": "Mengukur daya 3 fasa dengan 2 wattmeter." },
  { "en": "Apa itu 'analisis per-fasa'?", "id": "Menyederhanakan analisis sistem seimbang." },
  { "en": "Apa itu 'komponen simetris'?", "id": "Metode analisis sistem tidak seimbang." },
  { "en": "Apa itu 'urutan positif'?", "id": "Komponen dengan urutan fasa normal." },
  { "en": "Apa itu 'urutan negatif'?", "id": "Komponen dengan urutan fasa terbalik." },
  { "en": "Apa itu 'urutan nol'?", "id": "Komponen di mana ketiga fasa sefasa." },
  { "en": "Penyebab urutan negatif?", "id": "Beban tidak seimbang atau gangguan." },
  { "en": "Penyebab urutan nol?", "id": "Gangguan fasa ke tanah (ground fault)." },
  { "en": "Apa itu 'transformator'?", "id": "Mengubah level tegangan AC." },
  { "en": "Prinsip kerja transformator?", "id": "Induksi elektromagnetik bersama." },
  { "en": "Apa itu 'lilitan primer'?", "id": "Sisi yang terhubung ke sumber." },
  { "en": "Apa itu 'lilitan sekunder'?", "id": "Sisi yang terhubung ke beban." },
  { "en": "Apa itu 'transformator step-up'?", "id": "Menaikkan level tegangan." },
  { "en": "Apa itu 'transformator step-down'?", "id": "Menurunkan level tegangan." },
  { "en": "Apa itu 'rasio lilitan'?", "id": "Perbandingan jumlah lilitan primer dan sekunder." },
  { "en": "Apa itu 'transformator ideal'?", "id": "Trafo tanpa kerugian daya sama sekali." },
  { "en": "Apa itu 'rugi tembaga'?", "id": "Kerugian daya di lilitan (I²R)." },
  { "en": "Apa itu 'rugi inti'?", "id": "Rugi histeresis dan arus eddy." },
  { "en": "Apa itu 'impedansi refleksi'?", "id": "Impedansi sekunder dilihat dari sisi primer." },
  { "en": "Apa itu 'generator sinkron'?", "id": "Pembangkit utama listrik AC di dunia." },
  { "en": "Apa nama lain generator sinkron?", "id": "Disebut juga alternator." },
  { "en": "Apa itu 'eksitasi' generator?", "id": "Menyuplai arus DC ke kumparan medan." },
  { "en": "Apa itu 'kecepatan sinkron'?", "id": "Kecepatan putar medan magnet." },
  { "en": "Apa itu 'motor sinkron'?", "id": "Motor AC yang berputar pada kecepatan sinkron." },
  { "en": "Apa itu 'motor induksi'?", "id": "Motor AC yang paling umum digunakan." },
  { "en": "Apa nama lain motor induksi?", "id": "Disebut juga motor asinkron." },
  { "en": "Apa itu 'slip' motor induksi?", "id": "Perbedaan kecepatan antara rotor dan medan putar." },
  { "en": "Apa itu 'rotor sangkar tupai'?", "id": "Jenis rotor motor induksi yang umum." },
  { "en": "Apa itu 'rotor belitan' (wound rotor)?", "id": "Rotor dengan belitan untuk kontrol kecepatan." },
  { "en": "Apa itu 'daya aktif' (P)?", "id": "Daya nyata yang melakukan kerja (Watt)." },
  { "en": "Apa itu 'daya reaktif' (Q)?", "id": "Daya untuk medan magnet/listrik (VAR)." },
  { "en": "Apa itu 'daya semu' (S)?", "id": "Kombinasi vektor P dan Q (VA)." },
  { "en": "Apa itu 'segitiga daya'?", "id": "Hubungan grafis antara P, Q, dan S." },
  { "en": "Apa itu 'faktor daya' (PF)?", "id": "Rasio antara daya aktif dan daya semu." },
  { "en": "Nilai faktor daya yang baik?", "id": "Mendekati 1." },
  { "en": "Apa itu 'koreksi faktor daya'?", "id": "Menambahkan kapasitor untuk memperbaiki PF." },
  { "en": "Mengapa PF perlu dikoreksi?", "id": "Mengurangi arus, rugi daya, dan denda." },
  { "en": "Apa itu 'kapasitor bank'?", "id": "Kumpulan kapasitor untuk koreksi PF." },
  { "en": "Apa itu 'beban induktif'?", "id": "Menyerap daya reaktif (motor, trafo)." },
  { "en": "Apa itu 'beban kapasitif'?", "id": "Menyuplai daya reaktif (kapasitor)." },
  { "en": "Faktor daya 'lagging' berarti?", "id": "Arus tertinggal dari tegangan (induktif)." },
  { "en": "Faktor daya 'leading' berarti?", "id": "Arus mendahului tegangan (kapasitif)." },
  { "en": "Apa itu 'harmonisa'?", "id": "Frekuensi kelipatan dari frekuensi fundamental." },
  { "en": "Apa penyebab harmonisa?", "id": "Beban non-linier seperti elektronik daya." },
  { "en": "Apa itu 'distorsi harmonik total' (THD)?", "id": "Ukuran total distorsi akibat harmonisa." },
  { "en": "Apa itu 'filter harmonik'?", "id": "Rangkaian untuk mengurangi atau menghilangkan harmonisa." },
  { "en": "Apa itu 'resonansi'?", "id": "Saat reaktansi induktif sama dengan kapasitif." },
  { "en": "Apa itu 'frekuensi resonansi'?", "id": "Frekuensi di mana XL sama dengan XC." },
  { "en": "Apa itu 'faktor kualitas' (Q)?", "id": "Ukuran ketajaman puncak resonansi." },
  { "en": "Apa itu 'bandwidth'?", "id": "Rentang frekuensi antara titik setengah daya." },
  { "en": "Apa itu 'filter'?", "id": "Rangkaian yang melewatkan frekuensi tertentu." },
  { "en": "Apa itu 'filter pasif'?", "id": "Filter yang terbuat dari R, L, C." },
  { "en": "Apa itu 'filter aktif'?", "id": "Filter yang menggunakan op-amp atau transistor." },
  { "en": "Apa itu 'rangkaian RLC seri'?", "id": "Resistor, Induktor, Kapasitor terhubung seri." },
  { "en": "Apa itu 'rangkaian RLC paralel'?", "id": "Resistor, Induktor, Kapasitor terhubung paralel." },
  { "en": "Apa itu 'fasor'?", "id": "Representasi bilangan kompleks dari gelombang sinus." },
  { "en": "Apa itu 'impedansi' (Z)?", "id": "Hambatan total pada rangkaian AC." },
  { "en": "Apa itu 'reaktansi' (X)?", "id": "Bagian imajiner dari impedansi." },
  { "en": "Apa itu 'admitansi' (Y)?", "id": "Kebalikan dari impedansi (1/Z)." },
  { "en": "Apa itu 'konduktansi' (G)?", "id": "Bagian riil dari admitansi." },
  { "en": "Apa itu 'suseptansi' (B)?", "id": "Bagian imajiner dari admitansi." },
  { "en": "Apa itu 'nilai RMS'?", "id": "Nilai efektif dari gelombang AC." },
  { "en": "Hubungan RMS dengan nilai puncak?", "id": "V_rms sama dengan V_puncak dibagi akar 2." },
  { "en": "Tegangan 220V PLN nilai apa?", "id": "Itu adalah nilai RMS." },
  { "en": "Apa itu 'frekuensi'?", "id": "Jumlah siklus per detik (Hertz)." },
  { "en": "Apa itu 'periode'?", "id": "Waktu untuk satu siklus (detik)." },
  { "en": "Apa itu 'frekuensi sudut' (omega)?", "id": "Omega sama dengan 2 pi f." },
  { "en": "Apa itu 'fasa'?", "id": "Pergeseran sudut dari titik acuan." },
  { "en": "Apa itu 'beda fasa'?", "id": "Perbedaan fasa antara tegangan dan arus." },
  { "en": "Pada induktor, siapa yang mendahului?", "id": "Tegangan mendahului arus 90 derajat." },
  { "en": "Pada kapasitor, siapa yang mendahului?", "id": "Arus mendahului tegangan 90 derajat." },
  { "en": "Pada resistor, bagaimana fasanya?", "id": "Tegangan dan arus sefasa." },
  { "en": "Apa itu 'Hukum Ohm' untuk AC?", "id": "V sama dengan I kali Z." },
  { "en": "Apa itu 'Hukum Arus Kirchhoff' (KCL)?", "id": "Jumlah fasor arus di simpul nol." },
  { "en": "Apa itu 'Hukum Tegangan Kirchhoff' (KVL)?", "id": "Jumlah fasor tegangan di loop nol." },
  { "en": "Apa itu 'Teorema Thevenin'?", "id": "Menyederhanakan rangkaian menjadi Vth dan Zth." },
  { "en": "Apa itu 'Teorema Norton'?", "id": "Menyederhanakan rangkaian menjadi In dan Zn." },
  { "en": "Apa itu 'Teorema Superposisi'?", "id": "Menganalisis kontribusi setiap sumber secara terpisah." },
  { "en": "Apa syarat 'transfer daya maksimum'?", "id": "Impedansi beban sama dengan konjugat Thevenin." },
  { "en": "Apa itu 'rangkaian kopling magnetik'?", "id": "Rangkaian dihubungkan melalui induktansi bersama." },
  { "en": "Apa itu 'induktansi bersama' (M)?", "id": "GGL terinduksi di satu kumparan oleh kumparan lain." },
  { "en": "Apa itu 'konvensi titik' (dot convention)?", "id": "Menentukan polaritas tegangan induksi bersama." },
  { "en": "Apa itu 'koefisien kopling' (k)?", "id": "Ukuran seberapa erat dua kumparan terkopling." },
  { "en": "Apa itu 'jaringan dua-port'?", "id": "Model rangkaian dengan input dan output." },
  { "en": "Apa itu 'parameter z'?", "id": "Parameter impedansi untuk jaringan dua-port." },
  { "en": "Apa itu 'parameter y'?", "id": "Parameter admitansi untuk jaringan dua-port." },
  { "en": "Apa itu 'parameter h'?", "id": "Parameter hibrida, digunakan untuk model transistor." },
  { "en": "Apa itu 'parameter ABCD'?", "id": "Parameter transmisi untuk saluran daya." },
  { "en": "Apa itu 'transformasi Fourier'?", "id": "Memecah sinyal menjadi spektrum frekuensinya." },
  { "en": "Apa itu 'spektrum' sinyal?", "id": "Representasi sinyal dalam domain frekuensi." },
  { "en": "Apa itu 'deret Fourier'?", "id": "Untuk sinyal periodik." },
  { "en": "Apa itu 'transformasi Fourier'?", "id": "Untuk sinyal non-periodik." },
  { "en": "Apa itu 'frekuensi fundamental'?", "id": "Frekuensi terendah dalam deret Fourier." },
  { "en": "Apa itu 'harmonisa'?", "id": "Komponen frekuensi kelipatan dari fundamental." },
  { "en": "Gelombang kotak hanya punya harmonisa apa?", "id": "Hanya memiliki harmonisa ganjil." },
  { "en": "Apa itu 'transformasi Laplace'?", "id": "Metode analisis rangkaian dalam domain-s." },
  { "en": "Apa itu 'fungsi transfer'?", "id": "Rasio output terhadap input (domain-s)." },
  { "en": "Apa itu 'kutub' (pole)?", "id": "Nilai 's' yang membuat fungsi transfer tak hingga." },
  { "en": "Apa itu 'nol' (zero)?", "id": "Nilai 's' yang membuat fungsi transfer nol." },
  { "en": "Apa itu 'diagram Bode'?", "id": "Grafik magnitudo dan fasa vs frekuensi." },
  { "en": "Apa itu 'dekade'?", "id": "Perubahan frekuensi 10 kali lipat." },
  { "en": "Apa itu 'oktaf'?", "id": "Perubahan frekuensi 2 kali lipat." },
  { "en": "Apa itu 'gain margin'?", "id": "Ukuran stabilitas relatif dalam umpan balik." },
  { "en": "Apa itu 'phase margin'?", "id": "Ukuran lain stabilitas relatif." },
  { "en": "Apa itu 'sistem tiga fasa seimbang'?", "id": "Ketiga fasa punya magnitudo dan beda fasa sama." },
  { "en": "Apa itu 'daya sesaat' 3 fasa?", "id": "Nilainya konstan, tidak berdenyut." },
  { "en": "Apa keuntungan daya konstan 3 fasa?", "id": "Mengurangi getaran pada mesin-mesin besar." },
  { "en": "Apa itu 'koneksi delta terbuka'?", "id": "Koneksi darurat menggunakan dua trafo." },
  { "en": "Apa itu 'analisis aliran daya'?", "id": "Menghitung aliran daya dalam jaringan listrik." },
  { "en": "Apa itu 'studi stabilitas'?", "id": "Menganalisis kemampuan sistem pulih dari gangguan." },
  { "en": "Apa itu 'osilasi daya'?", "id": "Ayun daya antar generator." },
  { "en": "Apa itu 'sistem eksitasi'?", "id": "Menyuplai arus DC ke rotor generator." },
  { "en": "Apa itu 'AVR'?", "id": "Automatic Voltage Regulator." },
  { "en": "Apa itu 'PSS'?", "id": "Power System Stabilizer." },
  { "en": "Apa itu 'motor induksi satu fasa'?", "id": "Membutuhkan cara untuk memulai putaran." },
  { "en": "Bagaimana cara start motor satu fasa?", "id": "Menggunakan belitan bantu dan kapasitor." },
  { "en": "Apa itu 'kapasitor start'?", "id": "Memberikan beda fasa untuk torsi awal." },
  { "en": "Apa itu 'kapasitor run'?", "id": "Memperbaiki faktor daya saat motor berjalan." },
  { "en": "Apa itu 'saklar sentrifugal'?", "id": "Memutus belitan bantu setelah kecepatan tercapai." },
  { "en": "Apa itu 'arus bolak-balik' (AC)?", "id": "Arus yang arahnya berubah secara periodik." },
  { "en": "Apa itu 'nilai puncak' (Vp)?", "id": "Amplitudo maksimum dari gelombang AC." },
  { "en": "Apa itu 'nilai puncak-ke-puncak' (Vpp)?", "id": "Selisih antara puncak positif dan negatif." },
  { "en": "Apa itu 'nilai RMS'?", "id": "Nilai efektif, setara dengan DC." },
  { "en": "Apa itu 'frekuensi' (f)?", "id": "Jumlah siklus per detik dalam Hertz (Hz)." },
  { "en": "Apa itu 'periode' (T)?", "id": "Waktu untuk satu siklus (T=1/f)." },
  { "en": "Apa itu 'impedansi'?", "id": "Hambatan total pada rangkaian AC (R, L, C)." },
  { "en": "Apa itu 'reaktansi induktif' (XL)?", "id": "Hambatan dari induktor (XL=2πfL)." },
  { "en": "Apa itu 'reaktansi kapasitif' (XC)?", "id": "Hambatan dari kapasitor (XC=1/(2πfC))." },
  { "en": "Bagaimana fasa arus di resistor?", "id": "Sefasa dengan tegangan." },
  { "en": "Bagaimana fasa arus di induktor?", "id": "Tertinggal 90° dari tegangan." },
  { "en": "Bagaimana fasa arus di kapasitor?", "id": "Mendahului 90° dari tegangan." },
  { "en": "Apa itu 'daya nyata' (P)?", "id": "Daya yang melakukan kerja (Watt)." },
  { "en": "Apa itu 'daya reaktif' (Q)?", "id": "Daya yang disimpan dan dilepas (VAR)." },
  { "en": "Apa itu 'daya semu' (S)?", "id": "Daya total dalam rangkaian (VA)." },
  { "en": "Apa itu 'faktor daya'?", "id": "Rasio daya nyata terhadap daya semu." },
  { "en": "Bagaimana cara memperbaiki faktor daya?", "id": "Menambahkan kapasitor (untuk beban induktif)." },
  { "en": "Apa itu 'resonansi seri'?", "id": "Impedansi minimum, arus maksimum (XL=XC)." },
  { "en": "Apa itu 'resonansi paralel'?", "id": "Impedansi maksimum, arus minimum (XL=XC)." },
  { "en": "Apa fungsi transformator?", "id": "Menaikkan atau menurunkan tegangan AC." },
  { "en": "Apa itu sistem 'tiga fasa'?", "id": "Tiga gelombang AC dengan beda fasa 120°." },
  { "en": "Apa itu koneksi 'bintang' (wye)?", "id": "Punya titik netral." },
  { "en": "Apa itu koneksi 'segitiga' (delta)?", "id": "Tidak punya titik netral." },
  { "en": "Apa itu 'nilai rata-rata' gelombang sinus?", "id": "Nol selama satu siklus penuh." },
  { "en": "Apa itu 'admitansi'?", "id": "Kebalikan dari impedansi." },
  { "en": "Apa itu 'suseptansi'?", "id": "Bagian imajiner dari admitansi." },
  { "en": "Apa itu 'konduktansi'?", "id": "Bagian riil dari admitansi." },
  { "en": "Apa mnemonic untuk fasa?", "id": "ELI the ICE man." },
  { "en": "Apa itu 'filter aktif'?", "id": "Filter yang menggunakan op-amp." },
  { "en": "Apa itu 'orde filter'?", "id": "Menentukan kecuraman atenuasi." },
  { "en": "Apa itu 'bandwidth' filter?", "id": "Rentang frekuensi yang dilewatkan." },
  { "en": "Apa itu 'faktor kualitas' (Q)?", "id": "Menentukan selektivitas rangkaian resonansi." },
  { "en": "Apa itu 'daya kompleks'?", "id": "Representasi vektor dari daya (P+jQ)." },
  { "en": "Apa itu 'daya tiga fasa'?", "id": "Daya total dari ketiga fasa." },
  { "en": "Bagaimana cara mengukur daya 3 fasa?", "id": "Menggunakan metode dua wattmeter." },
  { "en": "Apa itu 'beban seimbang'?", "id": "Ketiga fasa memiliki impedansi yang sama." },
  { "en": "Apa itu 'beban tidak seimbang'?", "id": "Impedansi antar fasa tidak sama." },
  { "en": "Apa itu 'harmonisa'?", "id": "Gelombang sinus kelipatan frekuensi fundamental." },
  { "en": "Apa penyebab harmonisa?", "id": "Beban non-linier seperti penyearah." },
  { "en": "Apa itu 'THD'?", "id": "Total Harmonic Distortion." },
  { "en": "Apa itu 'koreksi faktor daya'?", "id": "Menambahkan kapasitor untuk mengurangi daya reaktif." },
  { "en": "Apa itu 'motor sinkron'?", "id": "Berputar pada kecepatan sinkron dengan frekuensi." },
  { "en": "Apa itu 'motor induksi'?", "id": "Berputar sedikit lebih lambat dari kecepatan sinkron." },
  { "en": "Apa itu 'slip'?", "id": "Perbedaan kecepatan antara rotor dan medan putar." },
  { "en": "Apa itu 'generator AC'?", "id": "Mengubah energi mekanik menjadi listrik AC." },
  { "en": "Apa nama lain generator AC?", "id": "Alternator." },
  { "en": "Apa itu 'inverter'?", "id": "Mengubah DC menjadi AC." },
  { "en": "Apa itu 'penyearah'?", "id": "Mengubah AC menjadi DC." },
  { "en": "Apa itu 'VFD'?", "id": "Variable Frequency Drive, untuk mengatur kecepatan motor." },
  { "en": "Apa itu 'saluran transmisi'?", "id": "Media untuk menyalurkan energi frekuensi tinggi." },
  { "en": "Apa itu 'impedansi karakteristik'?", "id": "Impedansi yang dilihat oleh gelombang." },
  { "en": "Apa itu 'pencocokan impedansi'?", "id": "Menyesuaikan impedansi untuk transfer daya maksimal." },
  { "en": "Apa itu 'VSWR'?", "id": "Voltage Standing Wave Ratio." },
  { "en": "Apa itu 'diagram Smith'?", "id": "Alat bantu untuk analisis impedansi." },
  { "en": "Apa itu 'antena'?", "id": "Mengubah sinyal listrik menjadi gelombang radio." },
  { "en": "Apa itu 'gain antena'?", "id": "Kemampuan antena memfokuskan energi." },
  { "en": "Apa itu 'polarisasi'?", "id": "Orientasi medan listrik gelombang." }





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
