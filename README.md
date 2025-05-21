<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
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
        <h1>Pengetahuan Arduino Dasar</h1>
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
            // Kosakata 1-308 (dari PDF pertama)disini yahhhhh
            // SOAL-SOAL LAMA (1-209)
            { "en": "Apa yang dimaksud dengan \"konverter\" sebagai salah satu fungsi komponen aktif?", "id": "Komponen yang dapat mengubah satu bentuk sinyal atau level energi ke bentuk atau level lain (misalnya DC ke DC, DC ke AC)." },
            { "en": "Mengapa dioda sering disebut PN junction?", "id": "Karena dioda tersusun dari semikonduktor tipe P yang digabungkan dengan semikonduktor tipe N." },
            { "en": "Bagaimana bentuk gelombang output pada penyearah setengah gelombang jika dilihat dengan osiloskop?", "id": "Berbentuk setengah gelombang, atau arus DC tetapi belum rata melainkan berdenyut-denyut." },
            { "en": "Apa yang dimaksud dengan \"tegangan breakdown\" pada dioda Zener?", "id": "Tegangan reverse bias tertentu di mana dioda Zener mulai menghantarkan arus secara signifikan dan menjaga tegangan di terminalnya relatif konstan." },
            { "en": "Bagaimana proses emisi cahaya pada LED terkait dengan \"lubang\" (hole) dan elektron bebas?", "id": "Pada saat elektron bebas masuk ke lubang (hole) pada persambungan PN yang diberi bias maju, dioda akan memancarkan energi cahaya." },
            { "en": "Apakah Photo Diode lebih sensitif terhadap cahaya saat dibias maju atau dibias mundur?", "id": "Lebih umum digunakan dan sensitif saat dibias mundur (reverse bias) untuk aplikasi pendeteksian cahaya." },
            { "en": "Di mana saja Diode Varactor biasa digunakan?", "id": "Pada rangkaian atau peralatan elektronika yang berhubungan dengan frekuensi, misalnya ponsel, radio penerima, radio pemancar dan televisi." },
            { "en": "Apa karakteristik utama Diode Tunnel yang membedakannya dari dioda biasa terkait resistansinya saat dibias maju?", "id": "Diode tunnel menghasilkan resistansi kecil pada saat diode dibias maju (forward bias)." },
            { "en": "Siapa yang menemukan Diode Tunnel?", "id": "Seorang ilmuwan Jepang Dr. Leo Esaki." },
            { "en": "Apa saja bahan semikonduktor yang umum digunakan untuk membuat transistor?", "id": "Germanium atau silikon." },
            { "en": "Bagaimana dua tipe transistor BJT (PNP dan NPN) berbeda dalam hal susunan lapisan semikonduktornya?", "id": "Transistor PNP terdiri dari lapisan P-N-P, sedangkan NPN terdiri dari lapisan N-P-N." },
            { "en": "Apa yang dimaksud dengan \"tegangan knee\" pada karakteristik transistor bipolar?", "id": "Titik pada kurva karakteristik Ic vs Vce di mana transistor mulai keluar dari daerah saturasi dan masuk ke daerah aktif." },
            { "en": "Apa yang dimaksud dengan \"breakdown\" pada karakteristik transistor bipolar?", "id": "Tegangan Vce maksimum yang bisa ditangani transistor sebelum terjadi kerusakan atau arus kolektor meningkat tajam tak terkendali." },
            { "en": "Pada UJT, terminal mana yang berfungsi mirip dengan gate pada SCR atau basis pada BJT untuk memicu konduksi?", "id": "Emitor." },
            { "en": "Apa keunggulan FET dibandingkan BJT dalam hal impedansi input?", "id": "FET umumnya memiliki impedansi input yang jauh lebih tinggi karena gate-nya dikontrol oleh tegangan (medan listrik) bukan arus." },
            { "en": "Apa kepanjangan dari MOSFET?", "id": "Metal Oxide Semiconductor Field Effect Transistor." },
            { "en": "Untuk apa lapisan Metal Oxide (isolator) digunakan pada MOSFET antara terminal Gate dan kanal?", "id": "Untuk mengisolasi Gate dari kanal, sehingga Gate mengontrol konduktivitas kanal melalui medan listrik." },
            { "en": "Pada penguat Common Basis, apakah penguatan arusnya lebih besar atau lebih kecil dari satu?", "id": "Penguatan arusnya ($\\alpha_{dc}$) selalu sedikit lebih kecil dari satu." },
            { "en": "Apa fungsi utama dari konfigurasi penguat Common Collector (Emitter Follower)?", "id": "Sebagai buffer atau penyangga impedansi, karena memiliki impedansi input tinggi dan impedansi output rendah, serta penguatan arus yang baik." },
            { "en": "Mengapa transistor yang digunakan sebagai saklar dioperasikan pada daerah jenuh dan cut-off?", "id": "Pada daerah jenuh, resistansi transistor sangat rendah (seperti saklar tertutup). Pada daerah cut-off, resistansi sangat tinggi (seperti saklar terbuka)." },
            { "en": "Apa yang dimaksud dengan \"chip\" dalam konteks IC?", "id": "Sebuah keping kecil bahan semikonduktor (biasanya silikon) di mana seluruh rangkaian terintegrasi dibentuk." },
            { "en": "Apa yang dimaksud dengan \"beban speaker\" yang dipasang secara seri paralel pada Gambar 5.1?", "id": "Sejumlah speaker yang dihubungkan dalam kombinasi rangkaian seri dan paralel untuk mencapai impedansi tertentu dan distribusi daya yang diinginkan." },
            { "en": "Dalam rangkaian tahanan yang dihubung seri, jika satu resistor memiliki nilai jauh lebih besar dari yang lain, di resistor mana tegangan terbesar akan jatuh?", "id": "Tegangan terbesar akan jatuh pada resistor dengan nilai resistansi terbesar." },
            { "en": "Dalam rangkaian tahanan yang dihubung paralel, jika satu cabang memiliki resistansi jauh lebih kecil dari cabang lain, cabang mana yang akan dialiri arus terbesar?", "id": "Cabang dengan resistansi terkecil akan dialiri arus terbesar." },
            { "en": "Apa yang terjadi pada total arus yang dapat disuplai jika beberapa baterai identik dirangkai paralel dibandingkan satu baterai tunggal?", "id": "Total arus yang dapat disuplai akan meningkat (kemampuan suplai arus total lebih besar)." },
            { "en": "Mengapa saat merangkai kapasitor, tujuannya adalah untuk mendapatkan nilai kapasitas (kapasitansi) sesuai yang diinginkan?", "id": "Karena nilai kapasitansi tertentu diperlukan untuk fungsi rangkaian tertentu, seperti filtering, timing, atau penyimpanan energi." },
            { "en": "Bagaimana bentuk gelombang arus dan tegangan listrik jika nilainya selalu berubah terhadap waktu secara periodik dan mengalirnya secara bolak-balik?", "id": "Disebut arus bolak-balik atau alternating current (AC)." },
            { "en": "Apa yang dimaksud dengan \"puncak atas\" dan \"puncak bawah\" pada gelombang arus bolak-balik?", "id": "Nilai maksimum positif dan nilai maksimum negatif dari gelombang tersebut dalam satu siklus." },
            { "en": "Apa yang dimaksud dengan \"satu gelombang penuh\" pada arus bolak-balik?", "id": "Dicapainya puncak atas dan puncak bawah secara periodik (satu siklus lengkap)." },
            { "en": "Apa yang dimaksud dengan nilai dari \"puncak ke puncak\" (peak to peak) pada gelombang arus bolak-balik?", "id": "Besar nilai dari puncak atas hingga nilai puncak bawah." },
            { "en": "Apa satuan dari frekuensi sudut ($\\omega$)?", "id": "Radian per detik (rad/s)." },
            { "en": "Pada rangkaian resistor murni yang dihubungkan arus bolak-balik, apakah terjadi perbedaan fasa antara kuat arus dengan tegangan?", "id": "Tidak, kuat arus dan tegangan akan sefase." },
            { "en": "Apa yang dimaksud dengan \"diagram fasor\" untuk beban resistif?", "id": "Representasi grafis vektor di mana fasor arus dan fasor tegangan digambarkan searah (sefase)." },
            { "en": "Pada rangkaian induktor murni, apakah arus mendahului tegangan atau tegangan mendahului arus?", "id": "Tegangan mendahului arus sebesar $90^\\circ$." },
            { "en": "Pada rangkaian kapasitor murni, apakah arus mendahului tegangan atau tegangan mendahului arus?", "id": "Arus mendahului tegangan sebesar $90^\\circ$." },
            { "en": "Dalam rangkaian RC seri yang dihubungkan ke sumber AC, apakah arus yang melewati R sama dengan arus yang melewati C?", "id": "Ya, karena R dan C dirangkai seri, maka akan dilewati oleh arus I yang sama." },
            { "en": "Dalam rangkaian RL seri, apa yang dimaksud dengan $V_L$?", "id": "Tegangan pada induktor L." },
            { "en": "Dalam rangkaian RLC seri, jika $X_L = X_C$, rangkaian dikatakan bersifat apa dan bagaimana fasa antara tegangan dan arus?", "id": "Rangkaian mempunyai sifat resistif, tegangan akan sefase dengan arus listrik (tidak ada perbedaan fasa)." },
            { "en": "Pada rangkaian RL paralel, apakah ada perbedaan fasa antara tegangan pada masing-masing komponen (R dan L)?", "id": "Tidak ada perbedaan fase antara tegangan pada masing-masing komponen; tegangan pada R dan L sama dengan tegangan sumber ($V_R = V_L = V_T$)." },
            { "en": "Pada rangkaian RC paralel, arus cabang resistif ($I_R$) sefase dengan besaran apa?", "id": "Sefase dengan $V_T$ (tegangan total/sumber)." },
            { "en": "Pada rangkaian RLC paralel, bagaimana beda fasa antara arus pada kapasitor ($I_C$) dan arus pada induktor ($I_L$)?", "id": "$I_C$ dan $I_L$ berbeda fase sebesar $180^\\circ$." },
            { "en": "Apa nama lengkap Gustav Kirchhoff dan dari negara mana ia berasal?", "id": "Gustav Robert Kirchhoff, berasal dari Jerman." },
            { "en": "Selain Hukum Rangkaian Listrik, dalam bidang apa lagi Gustav Kirchhoff memberikan kontribusi besar?", "id": "Bidang spektroskopi." },
            { "en": "Apa bunyi Hukum Kirchhoff Pertama (Hukum Arus Kirchhoff)?", "id": "Arus yang masuk pada sebuah titik percabangan yang terdapat di dalam sebuah rangkaian listrik nilainya sama dengan arus yang keluar pada titik percabangannya." },
            { "en": "Apa bunyi Hukum Kirchhoff Kedua (Hukum Tegangan Kirchhoff)?", "id": "Pada rangkaian listrik tertutup, jumlah sumber tegangan dengan kerugian tegangan akan selalu sama dengan 0." },
            { "en": "Bagaimana gaya tarik magnet paling kuat terdistribusi pada sebuah magnet batang?", "id": "Gaya tarik magnet paling kuat terletak pada kedua kutubnya." },
            { "en": "Apa yang dimaksud dengan \"kaidah tangan kanan\" untuk menentukan arah medan magnetik di sekitar kawat berarus?", "id": "Arah arus listrik ditunjukkan dengan arah ibu jari (I), dan arah medan magnetik ditunjukkan dengan arah empat jari yang lain (B) yang melingkari kawat." },
            { "en": "Apa yang terjadi pada arus yang dihasilkan generator DC sederhana seperti dinamo sepeda jika kumparan berputar?", "id": "Akan dihasilkan arus searah." },
            { "en": "Dalam elemen Volta, mengapa lempeng tembaga dikatakan memiliki beda potensial yang lebih tinggi daripada lempeng seng?", "id": "Akibat reaksi kimia, lempeng tembaga bermuatan listrik positif dan lempeng seng bermuatan listrik negatif." },
            { "en": "Apa yang dimaksud dengan \"polarisasi gas hidrogen\" pada sel Volta dan apa akibatnya?", "id": "Gelembung gas hidrogen yang dihasilkan reaksi kimia melekat pada keping tembaga, yang menghambat aliran elektron dan menyebabkan lampu padam." },
            { "en": "Apa yang dimaksud dengan \"depolarisator\" pada batu baterai dan apa contohnya?", "id": "Zat yang berfungsi untuk menghilangkan efek polarisasi (penumpukan gas hidrogen). Contohnya adalah mangandioksida ($MnO_2$)." },
            { "en": "Mengapa perawatan aki meliputi pengecekan berat jenis larutan asam sulfat?", "id": "Karena jika berat jenisnya tidak sesuai atau sudah tidak pekat lagi, kinerja aki akan menurun dan perlu diganti atau ditambah." },
            { "en": "Apa fungsi utama dari belitan stator pada generator?", "id": "Terdiri dari beberapa batang konduktor di mana GGL induksi dibangkitkan ketika rotor berputar." },
            { "en": "Apa fungsi alur stator pada generator?", "id": "Merupakan bagian yang berperan sebagai tempat belitan stator ditempatkan." },
            { "en": "Mengapa konstruksi rotor dengan kutub silindris (\"cylinderical poles\") digunakan untuk generator dengan putaran tinggi?", "id": "Karena konstruksi ini dirancang tahan terhadap gaya-gaya yang lebih besar akibat putaran yang tinggi." },
            { "en": "Bagaimana prinsip dasar kerja generator arus bolak-balik berdasarkan Hukum Faraday?", "id": "Jika sebatang penghantar berada pada medan magnet yang berubah-ubah (akibat perputaran), maka pada penghantar tersebut akan terbentuk gaya gerak listrik." },
            { "en": "Apa yang menyebabkan arus AC pada generator AC memiliki bentuk sinusoidal?", "id": "Akibat perputaran kumparan yang kontinu dalam medan magnet, sudut antara kumparan dan garis gaya magnet berubah secara sinusoidal, menghasilkan GGL induksi sinusoidal." },
            { "en": "Apa yang dimaksud dengan \"sudut fase\" ($\\theta = \\omega t$) dalam persamaan tegangan atau arus AC?", "id": "Menunjukkan posisi sesaat dari fasor tegangan atau arus yang berputar dalam siklus gelombang." },
            { "en": "Bagaimana cara transformator daya mendistribusikan listrik AC dari PLN ke rumah-rumah?", "id": "Transformator digunakan untuk menaikkan atau menurunkan level tegangan AC agar sesuai untuk transmisi jarak jauh (tegangan tinggi) dan distribusi ke konsumen (tegangan rendah)." },
            { "en": "Alat ukur dengan asas kerja elektrodinamis dapat digunakan untuk mengukur jenis arus apa saja?", "id": "Dapat digunakan untuk mengukur arus AC maupun arus DC." },
            { "en": "Apa yang dimaksud dengan \"arus putar\" yang dimanfaatkan pada asas kerja alat ukur induksi?", "id": "Arus Eddy (eddy current) yang timbul pada konduktor akibat medan magnet bolak-balik." },
            { "en": "Dapatkah alat ukur dengan asas kerja kumparan putar digunakan untuk mengukur arus AC secara langsung tanpa komponen tambahan?", "id": "Tidak, alat ukur kumparan putar dasar hanya merespon nilai DC atau rata-rata. Untuk AC, biasanya memerlukan penyearah." },
            { "en": "Apakah galvanometer dapat mengukur arus dan tegangan AC secara langsung?", "id": "Galvanometer dasar (kumparan putar) hanya merespon nilai DC atau rata-rata. Untuk AC, biasanya memerlukan penyearah internal." },
            { "en": "Bagaimana cara yang benar memasang amperemeter untuk mengukur arus yang melalui sebuah lampu dalam rangkaian?", "id": "Amperemeter harus dipasang seri dengan lampu tersebut." },
            { "en": "Apa yang dimaksud dengan \"faktor kerja\" atau \"power factor\" yang diukur oleh Cos Phi meter?", "id": "Beda fase antara tegangan dan arus dalam rangkaian AC." },
            { "en": "Mengapa KWH Meter penting bagi penyedia layanan listrik seperti PLN?", "id": "Untuk mengukur konsumsi energi listrik pelanggan sehingga dapat menentukan tagihan yang akurat." },
            { "en": "Selain kabel instalasi, apa lagi yang tahanan isolasinya dapat diukur oleh Megger?", "id": "Kabel tegangan rendah, kabel tegangan tinggi, transformator, OCB, dan peralatan listrik lainnya." },
            { "en": "Apa keuntungan utama multimeter digital dibandingkan multimeter analog dalam hal pembacaan hasil ukur?", "id": "Multimeter digital menampilkan hasil ukur secara numerik langsung sehingga lebih mudah dibaca dan mengurangi kesalahan paralaks." },
            { "en": "Selain menampilkan bentuk gelombang, apa kegunaan osiloskop dalam menganalisis beda fasa antara dua sinyal listrik?", "id": "Osiloskop dapat menampilkan kedua sinyal secara bersamaan, memungkinkan pengukuran beda fasa di antara keduanya (misalnya menggunakan mode XY atau pengukuran kursor)." },
            { "en": "Apa yang dimaksud dengan \"kalibrasi tegangan\" menggunakan tombol CAL pada osiloskop?", "id": "Proses penyesuaian internal osiloskop menggunakan sinyal referensi standar (biasanya gelombang kotak dengan amplitudo tertentu) untuk memastikan skala Volt/Div akurat." },
            { "en": "Apa fungsi \"Variable\" atau tombol CAL (untuk VOLT/DIV atau TIME/DIV) yang dapat diputar pada osiloskop?", "id": "Untuk mengatur kepekaan (sensitivitas) secara halus. Saat kalibrasi, biasanya diputar ke posisi \"CAL\" atau maksimum untuk pembacaan skala yang standar." },
            { "en": "Mengapa osiloskop sering disebut sebagai \"mata\" bagi teknisi elektronika?", "id": "Karena kemampuannya untuk memvisualisasikan sinyal listrik yang tidak terlihat, membantu dalam analisis, diagnosis, dan perancangan rangkaian." },
            { "en": "Apa yang dimaksud dengan \"fleksibel dalam merancangnya\" sebagai kelebihan filter aktif?", "id": "Filter aktif memungkinkan perolehan (gain), frekuensi cut-off, dan Q-factor (kualitas faktor) untuk diatur secara independen dan lebih mudah disesuaikan." },
            { "en": "Dalam konteks filter, apa yang dimaksud dengan \"frekuensi pojok\"?", "id": "Istilah lain untuk frekuensi cut-off (fc)." },
            { "en": "Apa yang dimaksud dengan \"frekuensi putus\" dalam filter?", "id": "Istilah lain untuk frekuensi cut-off (fc)." },
            { "en": "Bagaimana impedansi masukan Op-amp yang besar pada filter aktif membantu mencegah pembebanan berlebih?", "id": "Impedansi masukan yang besar berarti filter aktif menarik arus yang sangat kecil dari sumber sinyal, sehingga tidak membebani sumber tersebut." },
            { "en": "Bagaimana impedansi keluaran Op-amp yang kecil pada filter aktif membantu menjaga stabilitas frekuensi cut-off?", "id": "Impedansi keluaran yang kecil mencegah titik frekuensi cut-off filter dari pengaruh perubahan impedansi beban yang terhubung ke output filter." },
            { "en": "Apa yang terjadi pada sinyal jika melewati filter pasif dalam hal penguatan?", "id": "Filter RC pasif menipiskan sinyal dan memiliki penguatan tidak lebih dari satu." },
            { "en": "Apa yang dimaksud dengan \"penguatan kurang dari satu\" pada filter pasif?", "id": "Amplitudo sinyal keluaran lebih kecil daripada sinyal masukan." },
            { "en": "Apa tujuan utama dari penggunaan filter pada sistem audio seperti speaker aktif?", "id": "Untuk memisahkan frekuensi audio ke driver speaker yang sesuai (woofer, tweeter) dan menghilangkan frekuensi yang tidak diinginkan atau yang dapat merusak speaker." },
            { "en": "Dapatkah filter aktif menghasilkan penguatan sinyal selain fungsi filteringnya?", "id": "Ya, karena menggunakan komponen aktif seperti Op-amp, filter aktif dapat dirancang untuk memberikan penguatan pada sinyal di pass-band." },
            { "en": "Mengapa filter pasif lebih cocok untuk aplikasi frekuensi sangat tinggi dibandingkan filter aktif?", "id": "Komponen aktif (Op-amp, transistor) pada filter aktif memiliki keterbatasan bandwidth yang membatasi kinerjanya pada frekuensi sangat tinggi, sedangkan filter pasif tidak memiliki batasan ini." },
            { "en": "Dalam sistem bilangan, apa yang dimaksud dengan \"basis\" atau \"radix\"?", "id": "Jumlah lambang atau digit unik yang digunakan dalam sistem bilangan tersebut (misalnya basis 2 untuk biner, basis 10 untuk desimal)." },
            { "en": "Bagaimana cara membaca bilangan biner $1001_{(2)}$?", "id": "Dibaca sebagai \"satu-nol-nol-satu basis dua\"." },
            { "en": "Apa nilai desimal untuk lambang A, B, C, D, E, dan F dalam sistem bilangan hexadesimal?", "id": "A = 10, B = 11, C = 12, D = 13, E = 14, dan F = 15." },
            { "en": "Jika sisa pembagian saat mengkonversi desimal ke hexadesimal adalah 10, lambang apa yang dituliskan?", "id": "A." },
            { "en": "Apa langkah pertama dalam mengubah bilangan oktal ke hexadesimal menurut cara mudah yang disebutkan teks?", "id": "Dengan cara mengubah bilangan oktal menjadi biner terlebih dahulu." },
            { "en": "Sebutkan hukum Idempoten dalam Aljabar Boolean!", "id": "A + A = A; A ⋅ A = A." },
            { "en": "Sebutkan hukum Komutatif dalam Aljabar Boolean!", "id": "A + B = B + A; A ⋅ B = B ⋅ A." },
            { "en": "Sebutkan hukum Asosiatif dalam Aljabar Boolean!", "id": "(A + B) + C = A + (B + C); (A ⋅ B) ⋅ C = A ⋅ (B ⋅ C)." },
            { "en": "Apa yang dimaksud dengan \"variabel masukan\" dalam konteks peta Karnaugh?", "id": "Jumlah sinyal input yang berbeda pada rangkaian logika yang akan disederhanakan." },
            { "en": "Jika sebuah fungsi logika memiliki 3 variabel masukan, berapa kotak yang akan ada pada peta Karnaugh-nya?", "id": "Peta akan terdiri atas $2^3 = 8$ kotak." },
            { "en": "Gerbang logika apa yang bersifat seperti rangkaian dua buah saklar yang disusun seri?", "id": "Gerbang AND." },
            { "en": "Gerbang logika apa yang bersifat seperti rangkaian dua buah saklar yang disusun paralel?", "id": "Gerbang OR." },
            { "en": "Jika salah satu input gerbang NAND bernilai 0, apa outputnya tanpa perlu mengetahui input lainnya?", "id": "Outputnya akan bernilai 1." },
            { "en": "Jika salah satu input gerbang NOR bernilai 1, apa outputnya tanpa perlu mengetahui input lainnya?", "id": "Outputnya akan bernilai 0." },
            { "en": "Apa yang dimaksud dengan \"rangkaian sekuensial\"?", "id": "Rangkaian yang dibentuk lebih dari satu gerbang dasar yang input dan outputnya saling berpengaruh atau dalam sistem kontrol dinyatakan sebagian outputnya diumpanbalikkan kembali ke input." },
            { "en": "Selain sebagai memori, untuk apa lagi flip-flop digunakan sebagai komponen dasar dalam pengembangan selanjutnya?", "id": "Sebagai komponen dasar pembentukan komponen-komponen digital lainnya, seperti register dan counter." },
            { "en": "Pada R-S Flip-Flop (berdasarkan tabel kebenaran di Gambar 9.9), apa kondisi input S dan R yang membuat output Q dan $\\overline{Q}$ tetap (memori)?", "id": "Tabel di Gambar 9.9 menunjukkan S=0, R=0 menghasilkan Q=Q, Q'=Q' (memori) atau Tidak digunakan." },
            { "en": "Apa yang dimaksud dengan clock aktif pada saat pulsa berubah dari \"rendah ke tinggi\" pada D Flip-Flop?", "id": "Perubahan output D Flip-Flop hanya terjadi pada transisi naik dari sinyal clock (positive edge-triggered)." },
            { "en": "Pada J-K Flip-Flop, kondisi input J dan K apa yang menyebabkan output \"toggle\" (berubah kondisi)?", "id": "Input J=1 dan K=1." },
            { "en": "Inspirasi apa yang mendasari Gottfried Wilhem Leibniz dalam memperkenalkan sistem bilangan biner?", "id": "Terinspirasi oleh kebesaran Tuhan yang menciptakan dunia seisinya dari suatu ada (1) dan tidak ada (0)." },
            { "en": "Apa saja bentuk energi yang dapat diubah atau dihasilkan oleh tranduser?", "id": "Energi listrik, energi mekanis, energi elektromagnetik, energi cahaya, energi kimia, energi akustik, dan energi panas." },
            { "en": "Sebutkan minimal dua contoh energi fisik yang dapat diubah menjadi sinyal listrik oleh tranduser input!", "id": "Tekanan, cahaya, suhu, atau gelombang suara." },
            { "en": "Dapatkah sensor dianggap sebagai jenis tranduser? Jelaskan!", "id": "Ya, sensor adalah jenis tranduser input karena mengubah besaran fisik (mekanis, magnetis, panas, sinar, kimia) menjadi besaran listrik." },
            { "en": "Sebutkan contoh tranduser electrochemical!", "id": "Hydrogen sensor, pH probes." },
            { "en": "Sebutkan contoh tranduser electro-optical!", "id": "Lampu LED, diode laser, lampu pijar, tabung CRT." },
            { "en": "Apa yang dimaksud jika sebuah sensor memiliki \"stabilitas yang tinggi\"?", "id": "Kesalahan pengukuran yang kecil dan tidak begitu banyak terpengaruh oleh faktor-faktor lingkungan." },
            { "en": "Bagaimana ketidaklinieran transduser yang tidak diketahui dapat menyulitkan penggunaannya?", "id": "Karena hubungan masukan dan keluaran tidak diketahui, sehingga sulit untuk menginterpretasikan hasil pengukuran atau kontrol." },
            { "en": "Selain menjadi bagian dari robotika, di mana lagi sensor suhu seperti Thermocouple atau RTD banyak digunakan?", "id": "Dalam industri untuk pengukuran dan kontrol suhu proses, dalam peralatan rumah tangga (oven, kulkas), dan laboratorium." },
            { "en": "Apa yang dimaksud dengan \"self heating\" pada sensor seperti RTD atau thermistor?", "id": "Pemanasan internal pada sensor akibat arus listrik yang mengalir melaluinya untuk pengukuran resistansi, yang dapat sedikit mempengaruhi akurasi pengukuran suhu." },
            { "en": "Bagaimana prinsip kerja potensiometer sebagai pembagi tegangan?", "id": "Dengan menggeser tuas (wiper), rasio resistansi antara salah satu ujung terminal dan wiper terhadap resistansi total potensiometer berubah, sehingga tegangan output yang diambil dari wiper juga berubah secara proporsional." },
            { "en": "Apa yang dimaksud dengan \"deformasi\" pada konteks penggunaan Strain Gauge?", "id": "Perubahan bentuk atau dimensi suatu benda akibat adanya tekanan atau gaya yang bekerja padanya." },
            { "en": "Mengapa LDR (Light Dependent Resistor) sering digunakan dalam rangkaian saklar lampu otomatis?", "id": "Karena resistansinya berubah signifikan terhadap ada tidaknya cahaya, sehingga dapat digunakan untuk memicu transistor atau rangkaian lain untuk menyalakan atau mematikan lampu." },
            { "en": "Selain untuk mengukur suhu, apa aplikasi lain dari sensor inframerah (photo transistor atau photo diode jenis inframerah)?", "id": "Remote control, sensor gerak, sensor jarak, komunikasi data nirkabel jarak dekat." },
            { "en": "Apa potensi bahaya dari paparan sinar ultraviolet yang berlebihan yang dapat dideteksi oleh sensor ultraviolet?", "id": "Dapat merusak kulit manusia, mata, dan material tertentu." },
            { "en": "Tranduser mana yang digunakan pada alat musik elektrik seperti gitar listrik untuk mengubah getaran senar menjadi sinyal listrik?", "id": "Pickup magnetik (jenis transducer electromagnetic)." },
            { "en": "Bagaimana sebuah loudspeaker mengubah sinyal listrik menjadi energi suara?", "id": "Sinyal listrik mengalir melalui kumparan suara (voice coil) yang berada dalam medan magnet permanen, menyebabkan kumparan bergerak maju mundur. Gerakan kumparan ini menggetarkan diafragma (cone) speaker, yang kemudian menghasilkan gelombang suara di udara." },
            { "en": "Apa yang dimaksud dengan \"petir\" dalam konteks jenis listrik?", "id": "Petir merupakan contoh fenomena listrik tetap/statis yang dihasilkan oleh alam." },
            { "en": "Apa yang terjadi pada kertas-kertas kecil ketika penggaris yang telah digosokkan ke rambut didekatkan padanya?", "id": "Kertas-kertas kecil akan menempel pada penggaris." },
            { "en": "Bagaimana aliran listrik dinamis bergerak dalam hal potensial?", "id": "Dari potensial tinggi ke potensial rendah." },
            { "en": "Apa lambang dimensi untuk besaran \"Massa\"?", "id": "M." },
            { "en": "Apa lambang dimensi untuk besaran \"Panjang\"?", "id": "L." },
            { "en": "Dalam Tabel 1.2, apa satuan untuk \"Kuat medan listrik\" dan simbol satuannya?", "id": "Newton/coulomb (N/C)." },
            { "en": "Dalam Tabel 1.2, apa satuan untuk \"Konduktansi\" dan simbol satuannya?", "id": "Siemens (S), dengan simbol satuan $1/\\Omega$." },
            { "en": "Konversikan 1 Giga Volt (GV) ke Volt!", "id": "$1 \\text{ GV} = 10^9 \\text{ Volt} = 1.000.000.000 \\text{ Volt}$." },
            { "en": "Konversikan 1 Tera Hertz (THz) ke Hertz!", "id": "$1 \\text{ THz} = 10^{12} \\text{ Hertz} = 1.000.000.000.000 \\text{ Hertz}$." },
            { "en": "Konversikan 1 piko Farad (pF) ke Farad!", "id": "$1 \\text{ pF} = 10^{-12} \\text{ Farad}$." },
            { "en": "Dalam analogi aliran air, jika tidak ada beda tekanan, apakah air akan mengalir?", "id": "Jika tidak ada beda tekanan maka air tidak akan mengalir." },
            { "en": "Apa yang dimaksud dengan \"tegangan\" dalam konteks beda potensial?", "id": "Tegangan listrik dengan satuan Volt adalah besar beda potensial antara potensial tinggi dengan potensial rendah." },
            { "en": "Siapa penemu Hukum Induksi Elektromagnetik selain Michael Faraday (karena hukum ini juga disebut Hukum Faraday)?", "id": "Michael Faraday adalah penemu Hukum Induksi Elektromagnetik, yang juga dikenal sebagai Hukum Faraday." },
            { "en": "Apakah sifat kemagnetan pada elektromagnet bersifat permanen?", "id": "Tidak, sifat kemagnetannya bersifat sementara dan akan hilang jika aliran listrik dimatikan." },
            { "en": "Pada tahun berapa Friederick Lenz pertama kali mengemukakan Hukum Lenz?", "id": "Tahun 1834." },
            { "en": "Dalam Hukum Kirchhoff I, apa yang dimaksud dengan \"titik percabangan\"?", "id": "Suatu titik dalam rangkaian listrik di mana beberapa jalur arus bertemu atau berpisah." },
            { "en": "Siapakah sastrawan dan negarawan terkemuka dari Perancis yang dikagumi oleh ayah André-Marie Ampère?", "id": "Jean Jacques Rousseau." },
            { "en": "Apakah André-Marie Ampère pernah mengenyam pendidikan formal di sekolah?", "id": "André-Marie Ampère tidak pernah duduk di bangku sekolah; sebagian besar pendidikannya diperoleh dari ayahnya sendiri." },
            { "en": "Selain Hukum Ampere, apa kontribusi besar lain dari André-Marie Ampère pada dunia fisika modern?", "id": "Penelitiannya menjadi dasar ilmu pengetahuan modern yang dikenal sebagai elektro-magnetik." },
            { "en": "Apa bahaya yang dapat timbul dari sengatan listrik bagi manusia selain cacat fisik?", "id": "Cacat secara psikis hingga kematian." },
            { "en": "Apa yang dimaksud dengan \"beban lebih\" (overload) pada sistem kelistrikan?", "id": "Kondisi di mana rangkaian listrik dialiri arus yang melebihi kapasitas normalnya untuk periode waktu tertentu." },
            { "en": "Bagaimana sekring tidak otomatis merespons lonjakan arus yang melebihi batas maksimalnya?", "id": "Kawat lebur di dalamnya akan diputus." },
            { "en": "Apa fungsi utama dari komponen thermis (bimetal) yang ada pada MCB?", "id": "Mempunyai fungsi untuk melindungi rangkaian listrik saat terjadi beban lebih." },
            { "en": "Selain memutuskan arus saat ada kebocoran ke tanah, ELCB juga berfungsi sebagai pemutus aliran listrik dalam kondisi apa terkait sentuhan manusia?", "id": "Ketika tubuh manusia terhubung dengan ground saat menyentuh peralatan yang teraliri arus listrik." },
            { "en": "Apa yang menjadi media pemadam busur api pada Air Circuit Breaker (ACB)?", "id": "Udara." },
            { "en": "Mengapa penting untuk memastikan komponen dalam sekring (fuse) dapat melebur pada kemampuan maksimalnya?", "id": "Agar rangkaian elektronika yang berada di depannya menjadi aman dari kerusakan akibat arus lebih." },
            { "en": "Bagaimana prinsip kerja bimetal pada thermostat yang digunakan untuk mencegah kerusakan akibat suhu berlebih?", "id": "Suatu logam (bimetal) akan memuai dan berubah bentuk jika dipengaruhi oleh temperatur tertentu, yang kemudian dapat memutus atau menghubungkan rangkaian." },
            { "en": "Persyaratan apa yang harus dipenuhi oleh produk ELCB agar dapat diandalkan sebagai pengaman?", "id": "Harus berstandar SNI dan pemasangannya mengikuti peraturan yang berlaku oleh instalatir bersertifikat." },
            { "en": "Apa perbedaan utama antara resistor tetap dan resistor variabel?", "id": "Resistor tetap mempunyai nilai tahanan relatif tetap, sedangkan resistor variabel nilai hambatannya dapat diubah." },
            { "en": "Apa fungsi dari Rheostat?", "id": "Sebagai resistor variabel yang nilai hambatannya dapat diubah untuk mengatur arus atau tegangan." },
            { "en": "Warna apa pada gelang resistor yang menunjukkan nilai 0, 1, 2, 3, 4, 5, 6, 8, 9 pada pita ke-1 atau ke-2?", "id": "Hitam (0), Coklat (1), Merah (2), Oranye (3), Kuning (4), Hijau (5), Biru (6), Abu-abu (8), Putih (9)." },
            { "en": "Bagaimana cara menghitung nilai toleransi absolut (dalam Ohm) dari sebuah resistor jika nilai nominal dan persentase toleransinya diketahui?", "id": "Dengan mengalikan persentase toleransi dengan nilai nominal resistor tersebut." },
            { "en": "Apa yang dimaksud dengan \"koefisien suhu\" yang ditunjukkan oleh gelang keenam pada resistor 6 gelang warna?", "id": "Menunjukkan seberapa besar perubahan nilai resistansi resistor tersebut per satuan perubahan suhu (biasanya dalam ppm/°C)." },
            { "en": "Pada kapasitor, apa yang dimaksud dengan bahan \"dielektrikum\"?", "id": "Bahan isolator yang memisahkan dua plat konduktor pada kapasitor." },
            { "en": "Apa fungsi kapasitor sebagai \"kopling\" dalam rangkaian elektronika?", "id": "Untuk menghubungkan (meng-couple) sinyal AC dari satu tahap rangkaian ke tahap berikutnya sambil memblokir komponen DC." },
            { "en": "Sebutkan jenis kapasitor yang bahan isolatornya terbuat dari poliester dan termasuk non polar!", "id": "Kapasitor poliester." },
            { "en": "Kapasitor jenis apa yang memiliki koefisien suhu lebih tinggi dari kapasitor elektrolit/elco dan kapasitasnya besar meskipun ukurannya kecil?", "id": "Kapasitor tantalum." },
            { "en": "Untuk apa Varco (variabel condensator) umumnya digunakan dalam penerima radio?", "id": "Digunakan dalam memilih gelombang frekuensi penerima radio (osilator)." },
            { "en": "Jika pada badan kapasitor non polar tertulis \"47\" saja, berapa nilai kapasitansinya dan apa satuannya?", "id": "Nilai kapasitor adalah 47 pF." },
            { "en": "Bagaimana cara mengetahui nilai kapasitansi kapasitor chip keramik yang tidak memiliki cetakan nilai pada badannya?", "id": "Diperlukan label kotaknya untuk mengetahui nilainya atau dapat diukur dengan menggunakan Capacitance meter." },
            { "en": "Apa fungsi utama induktor sebagai \"pelipat tegangan\"?", "id": "Dalam beberapa konfigurasi rangkaian tertentu (seperti pada konverter boost atau flyback), induktor dapat digunakan untuk meningkatkan level tegangan." },
            { "en": "Bagaimana cara kerja relai sebagai saklar otomatis menggunakan prinsip induksi?", "id": "Apabila relai (kumparan induktor) dialiri arus listrik, maka akan timbul medan magnet yang menarik sebuah armatur logam, yang kemudian menggerakkan kontak saklar untuk membuka atau menutup rangkaian lain." },
            { "en": "Apakah semua jenis komponen elektronika (pasif dan aktif) tersedia dalam bentuk SMD?", "id": "Teks menyebutkan resistor, kapasitor, induktor, IC, transistor, dioda dibuat SMD, menyiratkan bahwa banyak komponen umum tersedia dalam bentuk SMD." },
            { "en": "Pada kode resistor SMD sistem 3 digit, jika digit ketiga adalah 0, apa artinya?", "id": "Faktor pengalinya adalah $10^0$ atau 1. Contoh: 120 berarti $12 \\Omega \\times 10^0 = 12 \\Omega$." },
            { "en": "Pada kode resistor SMD sistem EIA-96, apakah semua kombinasi tiga digit (angka-angka-huruf) selalu ada?", "id": "Tidak, nilai resistansi dan faktor pengali didapatkan dari tabel khusus (Tabel 3.2 dan 3.3)." },
            { "en": "Menurut Tabel 3.4 (Kode Nilai Kapasitansi Kapasitor SMD), kode huruf \"T\" menunjukkan nilai kapasitansi berapa pF?", "id": "5.1 pF." },
            { "en": "Pada pengkodean induktor SMD, jika digit ketiga adalah 5, berapa faktor pengalinya?", "id": "Faktor pengalinya adalah $10^5$." },
            { "en": "Apa yang dimaksud dengan \"noise\" dalam konteks filter pasif yang memiliki keunggulan noise kecil?", "id": "Sinyal gangguan yang tidak diinginkan yang dapat mengganggu kualitas sinyal utama." },
            { "en": "Bagaimana fleksibilitas dalam merancang filter aktif dibandingkan filter pasif?", "id": "Filter aktif lebih fleksibel dalam merancangnya." },
            { "en": "Dalam Low Pass Filter pasif RC, bagaimana perbandingan antara tegangan pada kapasitor (VC) dan drop tegangan pada resistor (VR) pada frekuensi yang sangat tinggi (jauh di atas cut-off)?", "id": "Pada frekuensi tinggi, reaktansi kapasitor (Xc) akan sangat kecil, sehingga tegangan pada kapasitor (VC) akan jauh lebih kecil dibandingkan drop tegangan pada resistor (VR), dan Vout akan sangat kecil." },
            { "en": "Apa nama lain untuk frekuensi cut-off (fc) yang berkaitan dengan penurunan daya sebesar separuh?", "id": "Frekuensi 3dB (karena -3dB merepresentasikan daya setengah)." },
            { "en": "Mengapa filter RC pasif dikatakan \"menipiskan sinyal\"?", "id": "Karena sinyal keluaran mempunyai amplitudo yang lebih kecil daripada sinyal keluaran (masukan), atau memiliki penguatan tidak lebih dari satu." },
            { "en": "Pada High Pass Filter pasif RC, bagaimana perilaku kapasitor pada frekuensi yang sangat rendah (jauh di bawah cut-off)?", "id": "Nilai reaktansi kapasitif dari kapasitor pada frekuensi rendah akan sangat tinggi, sehingga kapasitor seperti sebuah rangkaian terbuka yang akan menahan sinyal masukan." },
            { "en": "Dalam Band Pass Filter yang merupakan gabungan LPF dan HPF, frekuensi cut-off LPF harus bagaimana dibandingkan frekuensi cut-off HPF?", "id": "Frekuensi cut-off pada Low Pass Filter harus lebih besar daripada frekuensi cut-off pada High Pass Filter." },
            { "en": "Apa yang dimaksud dengan \"respon frekuensi band pass tidak simetris yang datar\" pada filter aktif Band Pass Filter tertentu?", "id": "Menunjukkan bahwa pass-band filter memiliki penguatan yang relatif konstan di seluruh rentang frekuensi lolosnya, namun bentuk lereng (roll-off) di sisi frekuensi rendah dan sisi frekuensi tinggi mungkin tidak identik." },
            { "en": "Bagaimana cara kerja Band Stop Filter dalam melewatkan atau melemahkan frekuensi?", "id": "Hanya sinyal dengan frekuensi di luar frekuensi cut-off (Fc1 dan Fc2) yang akan diloloskan, dan sinyal dengan frekuensi di antara frekuensi cut-off yang dilemahkan." },
            { "en": "Apa tujuan dari menghubungkan rangkaian dasar low pass filter RC dengan Op-amp pada filter aktif LPF?", "id": "Untuk penguatan dan pengendalian penguatan." },
            { "en": "Apa fungsi utama dari bagian crossover audio yang menggunakan filter?", "id": "Untuk meneruskan frekuensi yang diinginkan ke loudspeaker yang sesuai (tweeter untuk frekuensi tinggi, woofer untuk frekuensi rendah) dan melemahkan sinyal frekuensi yang tidak diinginkan." },
            { "en": "Loudspeaker jenis apa yang bekerja pada frekuensi rendah (bass)?", "id": "Loudspeaker jenis woofer." },
            { "en": "Loudspeaker jenis apa yang bekerja merespon frekuensi tinggi (treble)?", "id": "Loudspeaker jenis tweeter." },
            { "en": "Dalam crossover, filter jenis apa yang menahan frekuensi tinggi dan meloloskan frekuensi rendah ke woofer?", "id": "Low pass filter." },
            { "en": "Dalam crossover, filter jenis apa yang meloloskan frekuensi tinggi ke tweeter dan menahan frekuensi rendah?", "id": "High pass filter." },
            { "en": "Dalam sistem bilangan, apa yang dimaksud dengan \"absolue value\" dan \"position value\"?", "id": "Absolue value merupakan nilai tiap-tiap digit bilangan tersebut dan position value adalah bobot dari tiap-tiap digit tergantung dari letak posisi digit tersebut." },
            { "en": "Apa yang dimaksud dengan \"kondisi don't care\" dalam peta Karnaugh dan bagaimana penggunaannya?", "id": "Kondisi di mana nilai output tidak relevan untuk kombinasi input tertentu. \"Don't care\" dapat dianggap sebagai '1' atau '0' untuk membantu membentuk kelompok yang lebih besar dan menyederhanakan fungsi logika." },
            { "en": "Gerbang logika apa yang outputnya akan selalu 1 jika setidaknya satu inputnya 0?", "id": "Gerbang NAND." },
            { "en": "Gerbang logika apa yang outputnya akan selalu 0 jika setidaknya satu inputnya 1?", "id": "Gerbang NOR." },
            { "en": "Apa perbedaan utama antara rangkaian kombinasional dan rangkaian sekuensial dalam elektronika digital?", "id": "Output rangkaian kombinasional hanya bergantung pada kondisi input saat itu juga, sedangkan output rangkaian sekuensial bergantung pada kondisi input saat ini dan kondisi output sebelumnya (memiliki memori). Flip-flop adalah contoh rangkaian sekuensial." },
            { "en": "Bagaimana karakteristik memori pada R-S Flip-Flop (dari gerbang NOR) diaktifkan menurut tabel kebenarannya (Gambar 9.9)?", "id": "Ketika input S=0 dan R=0 (setelah sebelumnya di-set atau di-reset), output Q akan mempertahankan kondisi terakhirnya." },
            { "en": "Apa fungsi dari \"clock edge\" (misalnya positive edge atau negative edge) pada flip-flop sinkron?", "id": "Menentukan momen transisi sinyal clock (naik atau turun) di mana flip-flop akan merespons input datanya dan mengubah outputnya." },
            { "en": "Mengapa J-K Flip-Flop sering dianggap sebagai flip-flop universal?", "id": "Karena dapat dikonfigurasi untuk meniru fungsi R-S Flip-Flop, D Flip-Flop, dan T Flip-Flop (toggle)." },
            { "en": "Apa yang terjadi pada output J-K Flip-Flop jika J=1, K=0, dan ada pulsa clock aktif?", "id": "Output Q akan menjadi 1 (SET)." },
            { "en": "Apa yang terjadi pada output J-K Flip-Flop jika J=0, K=1, dan ada pulsa clock aktif?", "id": "Output Q akan menjadi 0 (RESET)." },
            { "en": "Apa arti notasi 0 (off) dan 1 (on) dalam konteks peralatan elektronika yang terinspirasi dari Leibniz?", "id": "Notasi 0 (off) menunjukkan keadaan mati atau tidak aktif, dan notasi 1 (on) menunjukkan keadaan hidup atau aktif." },
            { "en": "Dapatkah sebuah termometer merkuri dianggap sebagai sensor dan tranduser? Jelaskan!", "id": "Ya, termometer merkuri adalah sensor karena mendeteksi perubahan suhu (energi panas) dan merupakan tranduser karena mengubah energi panas tersebut menjadi perubahan panjang kolom merkuri (energi mekanik/visual)." },
            { "en": "Mengapa penting untuk mempertimbangkan \"jangkauan kerja\" (range) sebuah sensor saat memilihnya untuk suatu aplikasi?", "id": "Agar sensor dapat mengukur seluruh rentang nilai yang diharapkan dari besaran fisik tanpa menjadi jenuh atau tidak responsif." },
            { "en": "Bagaimana sebuah sensor dapat \"mempengaruhi kuantitas yang sedang diukur\"? Berikan contoh!", "id": "Jika sensor memiliki massa atau volume yang signifikan, ia dapat mengubah kondisi lokal yang diukurnya. Contoh: termometer besar yang dimasukkan ke dalam cairan kecil dapat mengubah suhu cairan tersebut." },
            { "en": "Apa yang dimaksud dengan \"tingkat kehandalan\" (reliability) sebuah sensor?", "id": "Kemampuan sensor untuk beroperasi secara konsisten dan akurat dalam jangka waktu tertentu tanpa kegagalan." },
            { "en": "Apa saja jenis thermocouple yang disebutkan dalam teks berdasarkan komposisi bahannya?", "id": "Jenis E, J, K, N, T, U." },
            { "en": "Bagaimana prinsip kerja dasar dari Resistance Temperature Detector (RTD)?", "id": "Resistansi listrik dari material konduktor (biasanya logam murni seperti platina) berubah secara terukur seiring dengan perubahan suhu." },
            { "en": "Apa perbedaan utama antara NTC dan PTC thermistor?", "id": "NTC (Negative Temperature Coefficient) memiliki resistansi yang menurun saat suhu meningkat, sedangkan PTC (Positive Temperature Coefficient) memiliki resistansi yang meningkat saat suhu meningkat." },
            { "en": "Apa keunggulan utama IC sensor suhu seperti LM35 dalam hal karakteristik outputnya?", "id": "Memiliki output tegangan dan arus yang sangat linear terhadap perubahan suhu." },
            { "en": "Apa fungsi potensiometer saat digunakan sebagai \"joystick pada tranduser\"?", "id": "Mengubah gerakan mekanis joystick (posisi X-Y) menjadi perubahan resistansi atau tegangan yang dapat dibaca sebagai input oleh sistem elektronik." },
            { "en": "Apa yang dimaksud dengan \"grid metal-foil\" pada sensor strain gauge?", "id": "Pola konduktif tipis (grid) yang terbuat dari foil logam, yang akan mengalami perubahan resistansi saat terdeformasi." },
            { "en": "Bagaimana proses photoetching digunakan dalam pembuatan sensor strain gauge tipe metal-foil?", "id": "Untuk membentuk konfigurasi grid yang presisi pada foil logam." },
            { "en": "Apa fungsi utama dari lapisan P yang transparan pada sel surya silikon tipe fotovoltaik?", "id": "Agar cahaya dapat menembus dan mencapai persambungan PN untuk menghasilkan gerakan elektron." },
            { "en": "Pada aplikasi LDR sebagai saklar lampu otomatis, kapan transistor Q1 dan Q2 akan aktif untuk menyalakan lampu?", "id": "Pada saat LDR tidak mendapatkan cahaya (gelap), hambatannya menjadi besar, sehingga arus akan melewati kaki basis transistor Q1 dan mengaktifkan transistor Q2, yang kemudian mengaktifkan relei untuk menyalakan lampu." },
            { "en": "Apa yang terjadi pada terminal keluaran sensor cahaya inframerah ketika menerima pancaran cahaya inframerah?", "id": "Akan terjadi perubahan resistansi (jika berupa photo transistor atau photo diode) atau perubahan tegangan keluaran (jika berupa chip IC)." },
            { "en": "Apa nama salah satu jenis sensor ultraviolet yang disebutkan dalam teks yang akan mengalami perubahan tegangan keluaran saat menerima perubahan intensitas cahaya ultraviolet?", "id": "UVtron." },
            { "en": "Apakah semua tranduser memerlukan sumber daya eksternal untuk beroperasi? Jelaskan!", "id": "Tidak. Self-generating transducer (seperti thermocouple atau sel fotovoltaik) menghasilkan energi listriknya sendiri dari energi yang diukurnya, sedangkan external power transducer memerlukan sumber energi dari luar." },
            { "en": "Dapatkah sebuah antena dianggap sebagai tranduser? Jika ya, energi apa yang diubah menjadi energi apa?", "id": "Ya, antena adalah transducer electromagnetic yang mengubah gelombang elektromagnetik (energi elektromagnetik) menjadi sinyal listrik, atau sebaliknya." },
            { "en": "Apa yang dimaksud dengan \"pH probes\" sebagai transducer electrochemical?", "id": "Sensor yang mengubah aktivitas ion hidrogen dalam larutan (besaran kimia) menjadi sinyal listrik untuk mengukur tingkat keasaman atau kebasaan (pH)." },
            
            // AWAL DARI SOAL-SOAL BARU YANG DITAMBAHKAN (BATCH BESAR)
            { "en": "Apa fungsi utama dari 'flyback diode' yang dipasang paralel terbalik dengan kumparan relay?", "id": "Melindungi transistor pengendali dari lonjakan tegangan tinggi induktif saat relay dimatikan." },
            { "en": "Apa yang dimaksud dengan 'duty cycle' pada sinyal PWM (Pulse Width Modulation)?", "id": "Perbandingan antara waktu sinyal 'ON' terhadap periode total sinyal, biasanya dinyatakan dalam persen." },
            { "en": "Sebutkan salah satu keunggulan utama penggunaan MOSFET dibandingkan BJT sebagai saklar daya!", "id": "Kecepatan switching yang lebih tinggi dan rugi-rugi konduksi yang lebih rendah pada arus tinggi karena dikendalikan oleh tegangan." },
            { "en": "Apa fungsi dari IC regulator tegangan seri 78xx (misalnya 7805)?", "id": "Menghasilkan tegangan output positif yang tetap (misalnya +5V untuk 7805) dari tegangan input yang lebih tinggi." },
            { "en": "Apa perbedaan mendasar antara memori RAM (Random Access Memory) dan ROM (Read Only Memory)?", "id": "RAM bersifat volatil (data hilang saat daya mati) dan dapat ditulis ulang, sedangkan ROM bersifat non-volatil dan umumnya hanya bisa dibaca (atau sulit ditulis ulang)." },
            { "en": "Apa yang dimaksud dengan 'slew rate' pada operational amplifier (Op-Amp)?", "id": "Kecepatan maksimum perubahan tegangan output Op-Amp, biasanya dalam V/µs." },
            { "en": "Gerbang logika apa yang outputnya akan bernilai HIGH hanya jika semua inputnya bernilai HIGH?", "id": "Gerbang AND." },
            { "en": "Apa fungsi dari 'crystal oscillator' dalam rangkaian digital atau mikrokontroler?", "id": "Menghasilkan sinyal clock dengan frekuensi yang sangat stabil dan akurat untuk sinkronisasi operasi." },
            { "en": "Apa yang dimaksud dengan 'ADC' dalam konteks mikrokontroler?", "id": "Analog-to-Digital Converter, yaitu komponen yang mengubah sinyal analog (tegangan) menjadi nilai digital." },
            { "en": "Apa satuan dari kapasitansi?", "id": "Farad (F)." },
            { "en": "Apa yang dimaksud dengan 'common mode rejection ratio' (CMRR) pada Op-Amp?", "id": "Kemampuan Op-Amp untuk menolak sinyal yang sama (common mode) yang muncul di kedua inputnya." },
            { "en": "Dalam sistem komunikasi, apa fungsi dari modulasi?", "id": "Menumpangkan sinyal informasi (frekuensi rendah) ke sinyal pembawa (frekuensi tinggi) agar dapat ditransmisikan secara efisien." },
            { "en": "Apa bahan semikonduktor yang paling umum digunakan dalam pembuatan IC?", "id": "Silikon (Si)." },
            { "en": "Apa perbedaan antara sinyal analog dan sinyal digital?", "id": "Sinyal analog bersifat kontinu dan dapat memiliki nilai tak terbatas dalam rentangnya, sedangkan sinyal digital bersifat diskrit dan hanya memiliki level nilai tertentu (biasanya dua)." },
            { "en": "Apa fungsi 'heat sink' yang dipasang pada komponen seperti transistor daya atau IC regulator?", "id": "Membantu membuang panas yang dihasilkan oleh komponen ke lingkungan sekitar untuk mencegah overheating." },
            { "en": "Apa yang dimaksud dengan 'ground loop' dalam sistem audio atau elektronika?", "id": "Kondisi di mana terdapat lebih dari satu jalur pentanahan (ground) yang dapat menyebabkan noise atau hum." },
            { "en": "Apa jenis transistor yang memiliki tiga lapisan semikonduktor P-N-P atau N-P-N?", "id": "Transistor Bipolar Junction (BJT)." },
            { "en": "Dalam seven-segment display, segmen mana saja yang harus menyala untuk menampilkan angka '1'?", "id": "Segmen b dan c." },
            { "en": "Apa kepanjangan dari ESD dalam konteks penanganan komponen elektronika?", "id": "Electrostatic Discharge (Pelepasan Muatan Elektrostatis)." },
            { "en": "Apa fungsi dari 'pull-up resistor' pada input digital mikrokontroler?", "id": "Memastikan input memiliki level logika HIGH yang pasti ketika tidak ada sinyal eksternal yang terhubung atau saat saklar terbuka." },
            { "en": "Apa yang dimaksud dengan 'bandwidth' pada sebuah penguat (amplifier)?", "id": "Rentang frekuensi di mana penguat dapat memberikan penguatan yang signifikan (biasanya diukur pada titik -3dB dari penguatan maksimum)." },
            { "en": "Apa fungsi dari dioda Zener dalam rangkaian catu daya?", "id": "Sebagai regulator tegangan shunt, menjaga tegangan output tetap stabil pada nilai tegangan Zener-nya." },
            { "en": "Apa itu 'datasheet' untuk sebuah komponen elektronika?", "id": "Dokumen yang berisi spesifikasi teknis lengkap, karakteristik, parameter operasi, dan contoh aplikasi dari komponen tersebut." },
            { "en": "Mengapa diperlukan 'debouncing' untuk saklar mekanis yang terhubung ke input digital?", "id": "Karena saklar mekanis cenderung menghasilkan beberapa transisi sinyal (bouncing) saat ditekan atau dilepas, yang dapat dibaca sebagai beberapa input oleh sistem digital." },
            { "en": "Apa yang dimaksud dengan 'impedansi input' pada sebuah rangkaian atau perangkat?", "id": "Resistansi total yang dilihat oleh sumber sinyal saat terhubung ke input rangkaian tersebut, mencakup komponen resistif, kapasitif, dan induktif." },
            { "en": "Sebutkan salah satu aplikasi umum dari IC 555 timer!", "id": "Sebagai osilator (astable multivibrator), timer (monostable multivibrator), atau generator pulsa." },
            { "en": "Apa yang dimaksud dengan 'Fourier Transform' dalam analisis sinyal?", "id": "Metode matematika untuk menguraikan sinyal kompleks dalam domain waktu menjadi komponen-komponen frekuensinya." },
            { "en": "Apa fungsi kapasitor bypass atau decoupling?", "id": "Menstabilkan tegangan catu daya lokal pada IC dengan menyediakan jalur impedansi rendah untuk noise frekuensi tinggi ke ground." },
            { "en": "Dalam konteks PCB (Printed Circuit Board), apa yang dimaksud dengan 'via'?", "id": "Lubang berlapis konduktif yang menghubungkan jalur tembaga (trace) antara lapisan PCB yang berbeda." },
            { "en": "Apa yang dimaksud dengan 'arus bocor' (leakage current) pada kapasitor?", "id": "Arus DC kecil yang tetap mengalir melalui dielektrik kapasitor meskipun kapasitor seharusnya memblokir DC." },
            { "en": "Apa fungsi utama dari sebuah optocoupler (optoisolator)?", "id": "Memberikan isolasi elektrik antara dua bagian rangkaian dengan mentransmisikan sinyal menggunakan cahaya." },
            { "en": "Sebutkan salah satu jenis memori non-volatil yang umum digunakan untuk menyimpan firmware pada mikrokontroler.", "id": "Memori Flash (Flash Memory)." },
            { "en": "Apa yang dimaksud dengan 'sampling rate' pada ADC?", "id": "Jumlah sampel sinyal analog yang diambil per detik oleh ADC." },
            { "en": "Apa perbedaan utama antara transformator step-up dan step-down?", "id": "Transformator step-up menaikkan tegangan (jumlah lilitan sekunder lebih banyak dari primer), sedangkan step-down menurunkan tegangan (lilitan sekunder lebih sedikit)." },
            { "en": "Apa fungsi dari 'Schottky diode' dan apa keunggulannya dibandingkan dioda PN biasa?", "id": "Digunakan untuk switching frekuensi tinggi karena memiliki tegangan maju (forward voltage drop) yang rendah dan waktu pemulihan (recovery time) yang sangat cepat." },
            { "en": "Apa yang dimaksud dengan 'multiplexer' (MUX) dalam elektronika digital?", "id": "Rangkaian yang memilih salah satu dari beberapa sinyal input untuk diteruskan ke satu output, berdasarkan sinyal kontrol pemilih." },
            { "en": "Apa itu 'TRIAC' dan di mana biasanya digunakan?", "id": "Komponen semikonduktor yang dapat mengalirkan arus AC dua arah, sering digunakan untuk mengontrol daya AC seperti pada dimmer lampu atau kontrol kecepatan motor." },
            { "en": "Apa fungsi dari 'current limiting resistor' yang sering dipasang seri dengan LED?", "id": "Membatasi arus yang mengalir melalui LED agar tidak melebihi batas maksimumnya dan mencegah kerusakan." },
            { "en": "Apa yang dimaksud dengan 'hysteresis' pada rangkaian Schmitt Trigger?", "id": "Perbedaan antara ambang batas tegangan atas (upper threshold) dan ambang batas tegangan bawah (lower threshold) untuk mengubah kondisi output, berguna untuk imunitas noise." },
            { "en": "Apa itu 'IGBT' dan apa kombinasi sifat yang dimilikinya?", "id": "Insulated Gate Bipolar Transistor, menggabungkan impedansi input tinggi dari MOSFET dengan kemampuan arus tinggi dan rugi konduksi rendah dari BJT." },
            { "en": "Apa fungsi dari 'watchdog timer' (WDT) pada mikrokontroler?", "id": "Untuk mereset mikrokontroler jika program mengalami hang atau macet, dengan cara harus di-reset secara periodik oleh program yang berjalan normal." },
            { "en": "Dalam komunikasi serial, apa perbedaan antara mode sinkron dan asinkron?", "id": "Komunikasi sinkron menggunakan sinyal clock bersama antara pengirim dan penerima, sedangkan asinkron tidak menggunakan clock bersama melainkan bit start/stop untuk sinkronisasi." },
            { "en": "Apa yang dimaksud dengan 'common anode' dan 'common cathode' pada display seven-segment?", "id": "Common anode berarti semua anode LED terhubung bersama ke VCC, common cathode berarti semua katode terhubung bersama ke GND." },
            { "en": "Apa fungsi dari 'snubber circuit' yang sering dipasang pada saklar daya atau TRIAC?", "id": "Melindungi komponen saklar dari lonjakan tegangan (dv/dt) atau arus (di/dt) yang tinggi saat switching, biasanya terdiri dari resistor dan kapasitor seri." },
            { "en": "Apa yang dimaksud dengan 'gain-bandwidth product' (GBWP) pada Op-Amp?", "id": "Parameter yang menunjukkan bahwa hasil kali antara penguatan tegangan loop terbuka (open-loop gain) dan bandwidth Op-Amp adalah konstan." },
            { "en": "Apa itu 'logic analyzer' dan apa kegunaannya?", "id": "Alat ukur yang dapat menangkap dan menampilkan beberapa sinyal digital secara bersamaan, berguna untuk debugging sistem digital kompleks." },
            { "en": "Apa perbedaan antara filter orde pertama dan orde kedua?", "id": "Filter orde kedua memiliki roll-off (kemiringan atenuasi) yang lebih tajam (misalnya -40 dB/dekade) dibandingkan orde pertama (-20 dB/dekade)." },
            { "en": "Apa yang dimaksud dengan 'skin effect' pada konduktor frekuensi tinggi?", "id": "Kecenderungan arus AC frekuensi tinggi untuk mengalir lebih banyak di permukaan konduktor daripada di bagian tengahnya." },
            { "en": "Apa fungsi dari 'bypass capacitor' yang dipasang dekat pin VCC dan GND pada IC digital?", "id": "Menyediakan sumber arus sesaat untuk IC saat terjadi perubahan logika cepat dan menyaring noise frekuensi tinggi pada jalur catu daya." },
            { "en": "Apa itu 'FPGA' (Field-Programmable Gate Array)?", "id": "IC digital yang berisi blok logika yang dapat dikonfigurasi dan interkoneksi yang dapat diprogram oleh pengguna untuk mengimplementasikan fungsi digital kustom." },
            { "en": "Apa yang dimaksud dengan 'quiescent current' pada sebuah IC atau rangkaian?", "id": "Arus yang dikonsumsi oleh IC atau rangkaian saat dalam kondisi siaga atau tidak aktif melakukan fungsi utamanya." },
            { "en": "Apa fungsi 'DAC' (Digital-to-Analog Converter)?", "id": "Mengubah sinyal digital (representasi numerik) menjadi sinyal analog (tegangan atau arus) yang kontinu." },
            { "en": "Apa itu 'Hall Effect Sensor' dan apa yang dideteksinya?", "id": "Sensor yang mendeteksi keberadaan dan kekuatan medan magnet, menghasilkan tegangan output yang proporsional." },
            { "en": "Apa perbedaan antara 'half-duplex' dan 'full-duplex' dalam sistem komunikasi?", "id": "Half-duplex memungkinkan komunikasi dua arah tetapi tidak bersamaan (bergantian), full-duplex memungkinkan komunikasi dua arah secara bersamaan." },
            { "en": "Apa fungsi dari 'varistor' (Voltage Dependent Resistor)?", "id": "Melindungi rangkaian dari lonjakan tegangan transien (voltage spikes) dengan cara menurunkan resistansinya secara drastis saat tegangan melebihi ambang batas." },
            { "en": "Apa yang dimaksud dengan 'virtual ground' pada konfigurasi inverting amplifier Op-Amp?", "id": "Titik pada input inverting Op-Amp yang memiliki potensial mendekati nol volt (ground) karena umpan balik negatif, meskipun tidak terhubung langsung ke ground." },
            { "en": "Apa itu 'buck converter' dalam elektronika daya?", "id": "Jenis konverter DC-DC step-down yang menurunkan tegangan DC input menjadi tegangan DC output yang lebih rendah." },
            { "en": "Apa itu 'boost converter' dalam elektronika daya?", "id": "Jenis konverter DC-DC step-up yang meningkatkan tegangan DC input menjadi tegangan DC output yang lebih tinggi." },
            { "en": "Sebutkan tiga terminal utama pada Thyristor (SCR).", "id": "Anoda (Anode), Katoda (Cathode), dan Gerbang (Gate)." },
            { "en": "Apa yang dimaksud dengan 'noise margin' dalam gerbang logika digital?", "id": "Jumlah noise yang dapat ditoleransi pada input gerbang logika sebelum outputnya berubah kondisi secara tidak diinginkan." },
            { "en": "Apa perbedaan mendasar antara 'microprocessor' (MPU) dan 'microcontroller' (MCU)?", "id": "MPU adalah CPU saja dan memerlukan komponen eksternal (memori, I/O), sedangkan MCU adalah sistem komputer mini dalam satu chip (CPU, memori, I/O terintegrasi)." },
            { "en": "Apa fungsi dari 'bootloader' pada mikrokontroler?", "id": "Program kecil yang berjalan saat mikrokontroler pertama kali dinyalakan, bertugas memuat program utama (aplikasi pengguna) ke memori." },
            { "en": "Apa itu 'SPI' (Serial Peripheral Interface)?", "id": "Protokol komunikasi serial sinkron yang umum digunakan untuk komunikasi jarak pendek antara mikrokontroler dan perangkat periferal." },
            { "en": "Apa itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol komunikasi serial dua kawat (SDA dan SCL) yang digunakan untuk komunikasi antara IC atau modul dalam satu papan sirkuit." },
            { "en": "Apa yang dimaksud dengan 'latency' dalam sistem digital atau komunikasi?", "id": "Waktu tunda antara saat input diberikan atau permintaan dikirim hingga output yang sesuai diterima atau respons dimulai." },
            { "en": "Apa fungsi dari 'dioda clamping'?", "id": "Membatasi level tegangan suatu sinyal agar tidak melebihi atau kurang dari level tertentu." },
            { "en": "Apa yang dimaksud dengan 'Miller effect' pada transistor penguat?", "id": "Peningkatan kapasitansi input efektif pada penguat inverting akibat adanya kapasitansi antara input dan output." },
            { "en": "Apa itu 'spectrum analyzer' dan apa yang diukurnya?", "id": "Alat ukur yang menampilkan amplitudo sinyal terhadap frekuensi, berguna untuk menganalisis spektrum frekuensi sinyal." },
            { "en": "Apa itu 'LVDT' (Linear Variable Differential Transformer)?", "id": "Jenis transduser elektromekanis yang mengubah gerakan linear menjadi sinyal listrik proporsional." },
            { "en": "Apa yang dimaksud dengan 'standing wave ratio' (SWR) dalam sistem RF?", "id": "Ukuran ketidakcocokan impedansi antara jalur transmisi (misalnya kabel koaksial) dan bebannya (misalnya antena), nilai idealnya adalah 1:1." },
            { "en": "Apa fungsi dari 'balun' (balanced to unbalanced transformer)?", "id": "Mengubah sinyal antara jalur transmisi balanced (misalnya kabel twin-lead) dan jalur unbalanced (misalnya kabel koaksial), atau sebaliknya." },
            { "en": "Apa yang dimaksud dengan 'Q factor' pada rangkaian resonansi atau filter?", "id": "Faktor kualitas, yang menunjukkan seberapa selektif atau tajam respons frekuensi rangkaian tersebut di sekitar frekuensi resonansinya." },
            { "en": "Apa itu 'Butterworth filter'?", "id": "Jenis filter yang memiliki respons frekuensi paling datar di passband (tanpa ripple) dan roll-off yang moderat." },
            { "en": "Apa itu 'Chebyshev filter'?", "id": "Jenis filter yang memiliki roll-off yang lebih tajam dibandingkan Butterworth tetapi memiliki ripple di passband (Tipe I) atau stopband (Tipe II)." },
            { "en": "Apa itu 'Bessel filter'?", "id": "Jenis filter yang memiliki respons fasa linear terbaik (group delay konstan), penting untuk menjaga bentuk gelombang sinyal kompleks." },
            { "en": "Apa yang dimaksud dengan 'phase-locked loop' (PLL)?", "id": "Sistem kontrol umpan balik yang menghasilkan sinyal output yang fasanya terkait secara akurat dengan fasa sinyal input referensi." },
            { "en": "Apa itu 'DIAC' dan apa fungsinya dalam rangkaian pemicu TRIAC?", "id": "Komponen dua terminal yang akan menghantar (breakdown) pada tegangan tertentu di kedua arah, sering digunakan untuk memberikan pulsa pemicu ke gate TRIAC." },
            { "en": "Apa yang dimaksud dengan 'derating' pada komponen elektronika?", "id": "Praktik mengoperasikan komponen di bawah batas maksimum spesifikasinya (misalnya daya, tegangan, arus, suhu) untuk meningkatkan keandalan dan masa pakai." },
            { "en": "Apa itu 'crowbar circuit'?", "id": "Rangkaian proteksi yang dengan cepat membuat hubungan singkat (short circuit) pada output catu daya jika terjadi kondisi tegangan lebih (overvoltage) untuk melindungi beban." },
            { "en": "Apa fungsi dari 'ferrite bead' yang dipasang pada kabel?", "id": "Menekan noise frekuensi tinggi (EMI) dengan cara meningkatkan induktansi pada frekuensi tersebut tanpa mempengaruhi sinyal DC atau frekuensi rendah." },
            { "en": "Apa yang dimaksud dengan 'thermal runaway' pada transistor bipolar?", "id": "Kondisi di mana peningkatan suhu menyebabkan peningkatan arus kolektor, yang selanjutnya meningkatkan suhu lagi, berpotensi merusak transistor jika tidak dikendalikan." },
            { "en": "Apa itu 'GPIO' pada mikrokontroler?", "id": "General Purpose Input/Output, pin yang dapat dikonfigurasi oleh perangkat lunak sebagai input digital atau output digital." },
            { "en": "Apa yang dimaksud dengan 'interrupt' (interupsi) dalam sistem mikrokontroler?", "id": "Sinyal yang memberitahu prosesor tentang kejadian penting yang memerlukan perhatian segera, menghentikan sementara eksekusi program utama untuk menjalankan rutin layanan interupsi." },
            { "en": "Apa perbedaan antara 'polling' dan 'interrupt' untuk menangani event dari periferal?", "id": "Polling berarti CPU secara periodik memeriksa status periferal, sedangkan interrupt berarti periferal memberi sinyal ke CPU saat memerlukan layanan." },
            { "en": "Apa itu 'DMA' (Direct Memory Access) pada sistem komputer atau mikrokontroler?", "id": "Fitur yang memungkinkan periferal mentransfer data langsung ke atau dari memori tanpa melibatkan CPU secara intensif, meningkatkan efisiensi sistem." },
            { "en": "Apa yang dimaksud dengan 'endianness' (big-endian vs little-endian) dalam penyimpanan data multibyte?", "id": "Urutan byte yang disimpan dalam memori untuk representasi data numerik yang lebih besar dari satu byte." },
            { "en": "Apa fungsi dari 'analog comparator' pada mikrokontroler?", "id": "Membandingkan dua tegangan analog input dan menghasilkan output digital yang menunjukkan mana tegangan yang lebih besar." },
            { "en": "Apa itu 'class D amplifier'?", "id": "Jenis penguat audio yang menggunakan switching frekuensi tinggi (PWM) untuk mencapai efisiensi daya yang sangat tinggi." },
            { "en": "Apa yang dimaksud dengan 'aliasing' dalam proses sampling sinyal analog?", "id": "Efek di mana komponen frekuensi tinggi dalam sinyal asli tampak sebagai frekuensi yang lebih rendah setelah sampling jika sampling rate tidak memenuhi Kriteria Nyquist." },
            { "en": "Apa bunyi Kriteria Nyquist (atau Teorema Sampling Nyquist-Shannon)?", "id": "Frekuensi sampling harus setidaknya dua kali frekuensi tertinggi dalam sinyal yang di-sample untuk menghindari aliasing dan memungkinkan rekonstruksi sinyal asli." },
            { "en": "Apa itu 'Surface Mount Technology' (SMT)?", "id": "Metode pemasangan komponen elektronika langsung ke permukaan PCB, bukan melalui lubang (through-hole)." },
            { "en": "Apa keunggulan utama SMT dibandingkan teknologi through-hole?", "id": "Ukuran komponen lebih kecil, kepadatan komponen lebih tinggi, dan proses perakitan otomatis lebih mudah." },
            { "en": "Apa yang dimaksud dengan 'solder mask' pada PCB?", "id": "Lapisan pelindung tipis (biasanya berwarna hijau) yang diaplikasikan pada PCB untuk mencegah hubungan pendek antar pad solder dan melindungi jalur tembaga." },
            { "en": "Apa yang dimaksud dengan 'silkscreen' pada PCB?", "id": "Lapisan tinta (biasanya putih) pada PCB yang digunakan untuk mencetak label komponen, nomor referensi, logo, atau informasi lainnya." },
        // Lanjutkan penambahan soal di sini
        // Contoh soal tambahan:
            { "en": "Apa yang dimaksud dengan 'transmission line' dalam konteks RF?", "id": "Struktur fisik yang memandu gelombang elektromagnetik dari satu titik ke titik lain, seperti kabel koaksial atau microstrip pada PCB." },
            { "en": "Apa itu 'return loss' dalam sistem RF?", "id": "Ukuran seberapa banyak sinyal yang dipantulkan kembali dari ketidakcocokan impedansi, biasanya dinyatakan dalam dB." },
            { "en": "Apa itu 'insertion loss' dalam sistem RF atau filter?", "id": "Kehilangan daya sinyal akibat penyisipan komponen atau perangkat ke dalam jalur transmisi, dinyatakan dalam dB." },
            { "en": "Apa fungsi utama dari 'Low Noise Amplifier' (LNA)?", "id": "Menguatkan sinyal yang sangat lemah (misalnya dari antena) sambil menambahkan noise seminimal mungkin." },
            { "en": "Apa itu 'mixer' dalam rangkaian penerima radio?", "id": "Rangkaian non-linear yang menggabungkan dua frekuensi input (RF dan Local Oscillator) untuk menghasilkan frekuensi baru (Intermediate Frequency)." },
            { "en": "Apa yang dimaksud dengan 'image frequency' dalam penerima superheterodyne?", "id": "Frekuensi yang tidak diinginkan yang juga dapat menghasilkan frekuensi IF yang sama dengan sinyal yang diinginkan, memerlukan filter untuk menolaknya." },
            { "en": "Apa itu 'Automatic Gain Control' (AGC) dalam penerima radio?", "id": "Rangkaian yang secara otomatis menyesuaikan penguatan penerima untuk menjaga level output sinyal relatif konstan meskipun level sinyal input bervariasi." },
            { "en": "Apa yang dimaksud dengan 'software defined radio' (SDR)?", "id": "Sistem komunikasi radio di mana sebagian besar pemrosesan sinyal (seperti modulasi, demodulasi, filtering) dilakukan oleh perangkat lunak pada komputer atau prosesor." },
            { "en": "Apa perbedaan antara 'deterministic error' dan 'random error' dalam pengukuran?", "id": "Deterministic error bersifat sistematis dan dapat diprediksi/dikoreksi, sedangkan random error bersifat acak dan tidak dapat diprediksi sepenuhnya." },
            { "en": "Apa itu 'PID controller' (Proportional-Integral-Derivative)?", "id": "Mekanisme kontrol umpan balik loop tertutup yang banyak digunakan dalam sistem industri untuk mengatur proses agar mencapai setpoint yang diinginkan." },
            { "en": "Apa yang dimaksud dengan 'settling time' pada respons sistem kontrol atau Op-Amp?", "id": "Waktu yang dibutuhkan output sistem untuk mencapai dan tetap berada dalam rentang toleransi tertentu dari nilai akhirnya setelah adanya perubahan input." },
            { "en": "Apa itu 'overshoot' pada respons sistem kontrol?", "id": "Kondisi di mana output sistem melebihi nilai akhirnya (steady-state) sebelum akhirnya stabil." },
            { "en": "Apa fungsi 'zener barrier' dalam sistem instrumentasi intrinsically safe?", "id": "Membatasi energi listrik (tegangan dan arus) yang masuk ke area berbahaya untuk mencegah timbulnya percikan api atau ledakan." },
            { "en": "Apa yang dimaksud dengan 'electromagnetic compatibility' (EMC)?", "id": "Kemampuan peralatan elektronik untuk berfungsi dengan benar di lingkungan elektromagnetiknya tanpa menghasilkan interferensi elektromagnetik (EMI) yang tidak dapat diterima ke peralatan lain." },
            { "en": "Apa perbedaan antara 'conducted EMI' dan 'radiated EMI'?", "id": "Conducted EMI merambat melalui kabel atau konduktor, sedangkan radiated EMI merambat melalui udara sebagai gelombang elektromagnetik." },
            { "en": "Apa fungsi dari 'Faraday cage'?", "id": "Melindungi isi di dalamnya dari medan elektromagnetik eksternal atau mencegah emisi medan elektromagnetik dari dalam ke luar." },
            { "en": "Apa yang dimaksud dengan 'ground plane' pada PCB?", "id": "Lapisan tembaga besar pada PCB yang terhubung ke ground, berfungsi sebagai jalur balik arus, referensi tegangan, dan perisai EMI." },
            { "en": "Apa itu 'trace impedance' pada PCB frekuensi tinggi?", "id": "Karakteristik impedansi dari jalur tembaga pada PCB, yang penting untuk dikontrol agar sesuai dengan impedansi sumber dan beban untuk transfer daya maksimum dan integritas sinyal." },
            { "en": "Apa yang dimaksud dengan 'crosstalk' antara jalur sinyal pada PCB?", "id": "Interferensi yang tidak diinginkan antara jalur sinyal yang berdekatan akibat kopling kapasitif atau induktif." },
            { "en": "Apa itu 'differential signaling' dan mengapa digunakan?", "id": "Metode transmisi informasi menggunakan dua sinyal komplementer pada dua konduktor, lebih tahan terhadap noise common-mode." },
            { "en": "Apa yang dimaksud dengan 'eye diagram' dalam analisis sinyal digital kecepatan tinggi?", "id": "Tampilan osiloskop yang menumpuk beberapa siklus sinyal digital, digunakan untuk mengevaluasi kualitas sinyal dan mengukur parameter seperti jitter dan noise margin." },
            { "en": "Apa itu 'JTAG' (Joint Test Action Group)?", "id": "Standar industri untuk verifikasi desain dan pengujian papan sirkuit tercetak setelah perakitan, juga digunakan untuk memprogram IC seperti FPGA dan mikrokontroler." },
            { "en": "Apa perbedaan antara 'ASIC' (Application-Specific Integrated Circuit) dan FPGA?", "id": "ASIC dirancang untuk fungsi spesifik dan diproduksi secara massal (tidak dapat diprogram ulang oleh pengguna akhir), sedangkan FPGA lebih fleksibel dan dapat diprogram di lapangan." },
            { "en": "Apa yang dimaksud dengan 'power-on reset' (POR) circuit?", "id": "Rangkaian yang memastikan IC atau sistem memulai dalam kondisi yang diketahui dan stabil saat daya pertama kali diberikan." },
            { "en": "Apa itu 'brown-out detection' (BOD) pada mikrokontroler?", "id": "Fitur yang mendeteksi jika tegangan catu daya turun di bawah level aman dan dapat memicu reset atau tindakan pencegahan lainnya." },
            { "en": "Apa yang dimaksud dengan 'charge pump'?", "id": "Jenis konverter DC-DC yang menggunakan kapasitor untuk menyimpan dan mentransfer energi untuk menghasilkan tegangan output yang lebih tinggi atau lebih rendah dari input, atau tegangan negatif." },
            { "en": "Apa itu 'thermoelectric cooler' (TEC) atau Peltier device?", "id": "Perangkat semikonduktor yang memindahkan panas dari satu sisi ke sisi lain ketika dialiri arus listrik, dapat digunakan untuk pendinginan atau pemanasan presisi." },
            { "en": "Apa yang dimaksud dengan 'piezoelectric effect'?", "id": "Kemampuan material tertentu (seperti kristal kuarsa) untuk menghasilkan tegangan listrik saat mengalami tekanan mekanis, atau sebaliknya, berubah bentuk saat diberi tegangan." },
            { "en": "Sebutkan aplikasi umum dari efek piezoelektrik.", "id": "Sensor tekanan, mikrofon, buzzer, aktuator presisi, osilator kristal." },
            { "en": "Apa itu 'MEMS' (Micro-Electro-Mechanical Systems)?", "id": "Teknologi yang mengintegrasikan komponen mekanis dan elektris berukuran mikro pada substrat silikon, contohnya akselerometer dan giroskop pada smartphone." },
            { "en": "Apa yang dimaksud dengan 'quantum dot' dalam teknologi display?", "id": "Nanokristal semikonduktor yang memancarkan cahaya dengan warna tertentu (tergantung ukurannya) saat disinari cahaya atau dialiri listrik, digunakan untuk meningkatkan kualitas warna pada layar." },
            { "en": "Apa itu 'LiDAR' (Light Detection and Ranging)?", "id": "Metode penginderaan jauh yang menggunakan pulsa laser untuk mengukur jarak ke objek, banyak digunakan dalam mobil otonom dan pemetaan." },
            { "en": "Apa perbedaan antara 'inductive sensor' dan 'capacitive sensor' untuk deteksi jarak atau material?", "id": "Inductive sensor mendeteksi objek logam dengan perubahan medan magnet, capacitive sensor mendeteksi objek konduktif atau dielektrik dengan perubahan kapasitansi." },
            { "en": "Apa yang dimaksud dengan 'finite state machine' (FSM) dalam desain logika digital?", "id": "Model komputasi abstrak yang terdiri dari sejumlah state (kondisi), transisi antar state, dan aksi yang terkait, digunakan untuk merancang sistem sekuensial." },
            { "en": "Apa perbedaan antara FSM tipe Mealy dan Moore?", "id": "Pada FSM Mealy, output bergantung pada state saat ini dan input saat ini. Pada FSM Moore, output hanya bergantung pada state saat ini." },
            { "en": "Apa itu 'Verilog' atau 'VHDL'?", "id": "Hardware Description Language (HDL) yang digunakan untuk mendeskripsikan dan merancang sirkuit elektronik digital, terutama untuk FPGA dan ASIC." },
            { "en": "Apa yang dimaksud dengan 'clock skew' dalam sistem digital sinkron?", "id": "Perbedaan waktu kedatangan sinyal clock pada komponen-komponen yang berbeda dalam satu sistem, dapat menyebabkan masalah timing." },
            { "en": "Apa itu 'setup time' dan 'hold time' pada flip-flop?", "id": "Setup time adalah waktu minimum data harus stabil sebelum clock edge. Hold time adalah waktu minimum data harus stabil setelah clock edge, agar data dapat ditangkap dengan benar." },
            { "en": "Apa yang dimaksud dengan 'metastability' pada flip-flop?", "id": "Kondisi tidak stabil dan tidak dapat diprediksi pada output flip-flop (antara 0 dan 1) jika timing setup atau hold time dilanggar." },
            { "en": "Apa itu 'SRAM' (Static RAM) dan apa keunggulannya dibandingkan DRAM?", "id": "Jenis memori semikonduktor volatil yang menggunakan flip-flop untuk menyimpan setiap bit, lebih cepat dan tidak memerlukan refresh periodik seperti DRAM, tetapi lebih mahal dan kurang padat." },
            { "en": "Apa itu 'DRAM' (Dynamic RAM) dan mengapa memerlukan refresh?", "id": "Jenis memori semikonduktor volatil yang menyimpan setiap bit sebagai muatan pada kapasitor kecil, memerlukan refresh periodik karena muatan kapasitor dapat bocor." },
            { "en": "Apa itu 'EEPROM' (Electrically Erasable Programmable Read-Only Memory)?", "id": "Jenis memori non-volatil yang dapat dihapus dan diprogram ulang secara elektrik, sering digunakan untuk menyimpan konfigurasi atau data kecil." },
            { "en": "Apa fungsi dari 'voltage reference' (referensi tegangan) dalam ADC atau DAC?", "id": "Menyediakan level tegangan yang stabil dan akurat sebagai acuan untuk proses konversi analog-ke-digital atau digital-ke-analog." },
            { "en": "Apa itu 'class AB amplifier'?", "id": "Jenis penguat audio yang merupakan kompromi antara efisiensi Kelas B dan linearitas Kelas A, dengan sedikit arus bias untuk mengurangi distorsi crossover." },
            { "en": "Apa yang dimaksud dengan 'intermodulation distortion' (IMD)?", "id": "Jenis distorsi non-linear yang menghasilkan frekuensi baru (jumlah dan selisih) ketika dua atau lebih sinyal frekuensi berbeda melewati sistem non-linear." },
            { "en": "Apa itu 'harmonic distortion' (THD - Total Harmonic Distortion)?", "id": "Jenis distorsi yang menghasilkan harmonisa (kelipatan frekuensi) dari sinyal input asli." },
            { "en": "Apa fungsi dari 'de-emphasis' dan 'pre-emphasis' dalam sistem transmisi FM?", "id": "Pre-emphasis meningkatkan amplitudo frekuensi tinggi sebelum transmisi, de-emphasis menurunkannya kembali di penerima, untuk meningkatkan rasio sinyal-terhadap-noise (SNR) pada frekuensi tinggi." },
            { "en": "Apa itu 'negative feedback' (umpan balik negatif) dalam penguat dan apa manfaat utamanya?", "id": "Mengumpankan sebagian sinyal output kembali ke input dengan fasa terbalik, manfaatnya antara lain menstabilkan gain, mengurangi distorsi, dan memodifikasi impedansi input/output." },
            { "en": "Apa itu 'positive feedback' (umpan balik positif) dan di mana biasanya digunakan?", "id": "Mengumpankan sebagian sinyal output kembali ke input dengan fasa yang sama, digunakan dalam osilator untuk menghasilkan ayunan (osilasi) atau dalam rangkaian bistabil seperti Schmitt trigger." },
            { "en": "Apa itu 'Barkhausen criterion' untuk osilasi?", "id": "Dua syarat agar rangkaian osilator dapat berosilasi: penguatan loop total harus sama dengan satu, dan pergeseran fasa total dalam loop harus 0 atau kelipatan $360^\\circ$." },
            { "en": "Sebutkan salah satu jenis osilator RC.", "id": "Osilator Wien bridge, osilator phase-shift, atau osilator twin-T." },
            { "en": "Sebutkan salah satu jenis osilator LC.", "id": "Osilator Colpitts, osilator Hartley, atau osilator Clapp." },
            { "en": "Apa yang dimaksud dengan 'dropout voltage' pada LDO (Low Dropout Regulator)?", "id": "Perbedaan tegangan minimum yang diperlukan antara input dan output agar LDO tetap dapat meregulasi tegangan outputnya dengan baik." },
            { "en": "Apa itu 'switched-mode power supply' (SMPS)?", "id": "Jenis catu daya yang menggunakan komponen switching (seperti MOSFET) yang beroperasi pada frekuensi tinggi untuk mentransfer daya secara efisien, menghasilkan ukuran yang lebih kecil dan efisiensi lebih tinggi dibandingkan catu daya linear." },
            { "en": "Sebutkan salah satu topologi dasar SMPS.", "id": "Buck, Boost, Buck-Boost, Flyback, Forward, Cuk, SEPIC." },
            { "en": "Apa fungsi dari 'optocoupler' dalam SMPS?", "id": "Memberikan isolasi galvanik antara sisi primer (tegangan tinggi) dan sisi sekunder (tegangan rendah) sambil mentransmisikan sinyal umpan balik untuk regulasi." },
            { "en": "Apa yang dimaksud dengan 'inrush current' pada catu daya saat pertama kali dinyalakan?", "id": "Arus puncak sesaat yang tinggi yang ditarik oleh catu daya saat kapasitor input diisi, memerlukan proteksi untuk mencegah kerusakan." },
            { "en": "Apa itu 'Power Factor Correction' (PFC) pada catu daya AC-DC?", "id": "Teknik untuk membuat catu daya tampak seperti beban resistif murni bagi jala-jala AC, meningkatkan faktor daya mendekati 1 dan mengurangi distorsi harmonik." },
            { "en": "Apa perbedaan antara 'active PFC' dan 'passive PFC'?", "id": "Active PFC menggunakan komponen switching aktif (seperti konverter boost) untuk membentuk gelombang arus input, passive PFC menggunakan komponen pasif (induktor dan kapasitor)." },
            { "en": "Apa itu 'system on a chip' (SoC)?", "id": "IC yang mengintegrasikan sebagian besar atau semua komponen sistem komputer atau sistem elektronik lainnya (CPU, GPU, memori, periferal, I/O) ke dalam satu chip." },
            { "en": "Apa yang dimaksud dengan 'Internet of Things' (IoT)?", "id": "Jaringan perangkat fisik, kendaraan, peralatan rumah tangga, dan item lain yang disematkan dengan elektronika, perangkat lunak, sensor, aktuator, dan konektivitas yang memungkinkan objek-objek ini terhubung dan bertukar data." },
            { "en": "Apa itu 'Bluetooth Low Energy' (BLE)?", "id": "Versi teknologi nirkabel Bluetooth yang dirancang untuk konsumsi daya sangat rendah, cocok untuk aplikasi IoT dan perangkat wearable." },
            { "en": "Apa itu 'Wi-Fi HaLow' (IEEE 802.11ah)?", "id": "Standar Wi-Fi yang beroperasi pada pita frekuensi sub-1 GHz, menawarkan jangkauan lebih jauh dan penetrasi lebih baik dibandingkan Wi-Fi tradisional, cocok untuk IoT." },
            { "en": "Apa itu 'LoRaWAN'?", "id": "Protokol jaringan area luas berdaya rendah (LPWAN) yang dirancang untuk komunikasi nirkabel jarak jauh antar perangkat IoT bertenaga baterai." },
            { "en": "Apa itu 'NB-IoT' (Narrowband IoT)?", "id": "Standar teknologi radio LPWAN yang dikembangkan untuk memungkinkan berbagai perangkat dan layanan terhubung menggunakan jaringan seluler telekomunikasi." },
            { "en": "Apa yang dimaksud dengan 'edge computing' dalam konteks IoT?", "id": "Model komputasi terdistribusi di mana pemrosesan data dilakukan lebih dekat ke sumber data (perangkat IoT) daripada di pusat data terpusat atau cloud." },
            { "en": "Apa itu 'sensor fusion'?", "id": "Proses menggabungkan data dari beberapa sensor yang berbeda untuk menghasilkan informasi yang lebih akurat, andal, atau komprehensif daripada yang dapat diperoleh dari satu sensor saja." },
            { "en": "Apa yang dimaksud dengan 'haptic feedback'?", "id": "Umpan balik taktil (sentuhan) yang dihasilkan oleh perangkat elektronik, misalnya getaran pada smartphone atau kontroler game." },
            { "en": "Apa itu 'OLED' (Organic Light Emitting Diode)?", "id": "Jenis LED di mana lapisan emisif elektroluminesennya adalah film senyawa organik yang memancarkan cahaya sebagai respons terhadap arus listrik, digunakan dalam display." },
            { "en": "Apa keunggulan utama display OLED dibandingkan LCD tradisional?", "id": "Rasio kontras yang lebih tinggi (hitam yang lebih pekat), sudut pandang lebih lebar, waktu respons lebih cepat, dan potensi untuk display yang fleksibel." },
            { "en": "Apa itu 'quantum computing'?", "id": "Jenis komputasi yang menggunakan prinsip-prinsip mekanika kuantum (seperti superposisi dan keterkaitan kuantum) untuk melakukan operasi pada data, berpotensi menyelesaikan masalah kompleks yang tidak dapat diatasi oleh komputer klasik." },
            { "en": "Apa itu 'qubit'?", "id": "Unit dasar informasi kuantum, analog dengan bit pada komputasi klasik, tetapi dapat berada dalam superposisi 0 dan 1." },
            { "en": "Apa yang dimaksud dengan 'artificial intelligence' (AI) dalam elektronika?", "id": "Implementasi algoritma AI pada perangkat keras elektronik (misalnya chip AI khusus) untuk tugas-tugas seperti pengenalan gambar, pemrosesan bahasa alami, atau pengambilan keputusan otonom." },
            { "en": "Apa itu 'neural network' (jaringan saraf tiruan)?", "id": "Model komputasi yang terinspirasi oleh struktur dan fungsi otak biologis, terdiri dari node (neuron) yang saling terhubung dalam lapisan-lapisan, digunakan dalam machine learning." },
            { "en": "Apa itu 'machine learning' (ML)?", "id": "Subbidang AI yang berfokus pada pengembangan algoritma yang memungkinkan sistem komputer belajar dari data tanpa diprogram secara eksplisit." },
            { "en": "Apa yang dimaksud dengan 'deep learning'?", "id": "Bagian dari machine learning yang menggunakan jaringan saraf tiruan dengan banyak lapisan (deep neural networks) untuk mempelajari representasi data yang kompleks." },
            { "en": "Apa itu 'tensor processing unit' (TPU)?", "id": "ASIC yang dirancang khusus oleh Google untuk mempercepat beban kerja machine learning yang menggunakan TensorFlow." },
            { "en": "Apa yang dimaksud dengan 'wearable electronics' (elektronika sandang)?", "id": "Perangkat elektronik yang dirancang untuk dikenakan pada tubuh, seperti smartwatch, fitness tracker, atau kacamata pintar." },
// AWAL DARI BATCH SOAL BARU (Lanjutan untuk mencapai 619)
// Target: 150-200 soal baru di batch ini
{ "en": "Apa fungsi utama dari 'Schmitt Trigger' dalam rangkaian digital?", "id": "Mengubah sinyal analog atau sinyal digital yang terdistorsi (noisy) menjadi sinyal digital yang bersih dengan dua level ambang (threshold) yang berbeda untuk transisi naik dan turun, sehingga meningkatkan imunitas noise." },
{ "en": "Apa yang dimaksud dengan 'datasheet' komponen elektronika dan informasi penting apa saja yang biasanya terkandung di dalamnya?", "id": "Dokumen teknis yang disediakan pabrikan yang berisi spesifikasi lengkap, parameter operasional, rating maksimum, karakteristik listrik dan termal, dimensi fisik, serta contoh aplikasi komponen." },
{ "en": "Jelaskan prinsip dasar modulasi amplitudo (AM).", "id": "Proses mengubah amplitudo sinyal pembawa (carrier wave) frekuensi tinggi secara proporsional dengan amplitudo sinyal informasi (modulating signal) frekuensi rendah." },
{ "en": "Apa perbedaan antara BJT (Bipolar Junction Transistor) tipe NPN dan PNP dalam hal arah arus dan polaritas tegangan bias?", "id": "Pada NPN, arus mengalir dari Kolektor ke Emitor saat Basis diberi bias positif relatif terhadap Emitor. Pada PNP, arus mengalir dari Emitor ke Kolektor saat Basis diberi bias negatif relatif terhadap Emitor." },
{ "en": "Apa yang dimaksud dengan 'bandwidth' dalam konteks penguat (amplifier)?", "id": "Rentang frekuensi di mana penguat dapat memberikan penguatan yang relatif konstan, biasanya didefinisikan antara titik frekuensi di mana penguatan turun 3dB dari penguatan maksimum (half-power points)." },
{ "en": "Apa fungsi utama dari transformator (trafo) dalam rangkaian catu daya?", "id": "Untuk menaikkan (step-up) atau menurunkan (step-down) tegangan AC dari jala-jala ke level yang sesuai untuk rangkaian, serta memberikan isolasi galvanik antara jala-jala dan rangkaian." },
{ "en": "Sebutkan tiga mode operasi dasar transistor BJT sebagai penguat.", "id": "Common Emitter (CE), Common Collector (CC) atau Emitter Follower, dan Common Base (CB)." },
{ "en": "Apa yang dimaksud dengan 'impedansi input' dan 'impedansi output' pada sebuah penguat?", "id": "Impedansi input adalah impedansi total yang 'dilihat' oleh sumber sinyal saat terhubung ke input penguat. Impedansi output adalah impedansi internal yang 'dilihat' oleh beban saat terhubung ke output penguat." },
{ "en": "Jelaskan perbedaan mendasar antara RAM (Random Access Memory) dan ROM (Read-Only Memory).", "id": "RAM adalah memori volatil yang dapat dibaca dan ditulis, digunakan untuk penyimpanan data sementara saat perangkat beroperasi. ROM adalah memori non-volatil yang biasanya hanya dapat dibaca (atau sulit ditulis ulang), digunakan untuk menyimpan firmware atau data penting yang tidak boleh hilang saat daya mati." },
{ "en": "Apa fungsi dioda Zener dan bagaimana karakteristik utamanya?", "id": "Dioda Zener dirancang untuk beroperasi pada daerah breakdown tegangan balik (reverse breakdown). Fungsi utamanya adalah sebagai regulator tegangan, menjaga tegangan konstan pada terminalnya saat dibias mundur melebihi tegangan Zener-nya." },
{ "en": "Apa yang dimaksud dengan 'duty cycle' pada sinyal PWM (Pulse Width Modulation)?", "id": "Rasio antara durasi waktu pulsa 'ON' (tinggi) terhadap periode total sinyal PWM, biasanya dinyatakan dalam persentase." },
{ "en": "Sebutkan minimal tiga jenis sensor suhu yang umum digunakan dalam elektronika.", "id": "Thermistor (NTC/PTC), RTD (Resistance Temperature Detector), Thermocouple, IC Sensor Suhu (misalnya LM35)." },
{ "en": "Apa perbedaan antara rangkaian kombinasional dan rangkaian sekuensial dalam logika digital?", "id": "Output rangkaian kombinasional hanya bergantung pada kombinasi input saat itu juga. Output rangkaian sekuensial bergantung pada kombinasi input saat ini dan kondisi output sebelumnya (memiliki elemen memori)." },
{ "en": "Apa fungsi dari 'crystal oscillator' (osilator kristal) dalam sistem digital?", "id": "Menghasilkan sinyal clock (detak) dengan frekuensi yang sangat stabil dan akurat, yang digunakan untuk sinkronisasi operasi dalam sistem digital seperti mikrokontroler atau komputer." },
{ "en": "Apa yang dimaksud dengan ADC (Analog-to-Digital Converter) dan DAC (Digital-to-Analog Converter)?", "id": "ADC mengubah sinyal analog (kontinu) menjadi representasi digital (diskrit). DAC mengubah data digital menjadi sinyal analog." },
{ "en": "Jelaskan prinsip kerja dasar dari LDR (Light Dependent Resistor).", "id": "Resistansi LDR berubah tergantung pada intensitas cahaya yang diterimanya. Semakin banyak cahaya, semakin rendah resistansinya, dan sebaliknya." },
{ "en": "Apa fungsi utama dari kapasitor kopling (coupling capacitor) dan kapasitor bypass (bypass capacitor)?", "id": "Kapasitor kopling melewatkan sinyal AC antar tahap rangkaian sambil memblokir komponen DC. Kapasitor bypass menyediakan jalur impedansi rendah ke ground untuk noise frekuensi tinggi pada jalur catu daya, menstabilkan tegangan suplai lokal." },
{ "en": "Apa yang dimaksud dengan 'common mode rejection ratio' (CMRR) pada sebuah operational amplifier (Op-Amp)?", "id": "Kemampuan Op-Amp untuk menolak (melemahkan) sinyal yang sama (common-mode voltage) yang muncul pada kedua inputnya (inverting dan non-inverting)." },
{ "en": "Apa itu 'slew rate' pada Op-Amp dan mengapa penting?", "id": "Kecepatan maksimum perubahan tegangan output Op-Amp per satuan waktu (biasanya V/µs). Penting karena membatasi kemampuan Op-Amp untuk mereproduksi sinyal frekuensi tinggi dengan amplitudo besar tanpa distorsi." },
{ "en": "Sebutkan dua jenis utama MOSFET berdasarkan tipe kanalnya.", "id": "N-channel MOSFET (NMOS) dan P-channel MOSFET (PMOS)." },
{ "en": "Apa perbedaan antara MOSFET tipe enhancement dan tipe depletion?", "id": "Enhancement-mode MOSFET memerlukan tegangan gate-source (VGS) untuk membentuk kanal konduktif (normally OFF). Depletion-mode MOSFET memiliki kanal konduktif bahkan tanpa VGS (normally ON) dan memerlukan VGS untuk mematikan kanal." },
{ "en": "Apa fungsi dari 'flyback diode' (juga dikenal sebagai freewheeling diode atau snubber diode) yang dipasang paralel dengan beban induktif (seperti relay atau motor)?", "id": "Menyediakan jalur bagi arus induktif untuk mengalir saat beban induktif dimatikan, sehingga mencegah lonjakan tegangan tinggi (inductive kickback) yang dapat merusak komponen switching (misalnya transistor)." },
{ "en": "Apa yang dimaksud dengan 'ground loop' dan bagaimana cara menghindarinya?", "id": "Kondisi yang tidak diinginkan di mana terdapat lebih dari satu jalur pentanahan (ground) antara dua titik dalam sistem, dapat menyebabkan noise atau hum. Dihindari dengan menggunakan pentanahan satu titik (star grounding) atau isolasi." },
{ "en": "Apa itu IC regulator tegangan seri 78xx dan 79xx?", "id": "Seri 78xx adalah regulator tegangan positif tetap (misalnya 7805 untuk +5V, 7812 untuk +12V). Seri 79xx adalah regulator tegangan negatif tetap (misalnya 7905 untuk -5V)." },
{ "en": "Jelaskan apa itu 'seven-segment display' dan bagaimana cara kerjanya untuk menampilkan angka.", "id": "Komponen display yang terdiri dari tujuh segmen LED (ditambah titik desimal) yang dapat dinyalakan dalam kombinasi berbeda untuk membentuk angka numerik (0-9) dan beberapa huruf." },
{ "en": "Apa perbedaan antara seven-segment display tipe 'common anode' dan 'common cathode'?", "id": "Pada common anode, semua anode dari LED segmen terhubung bersama ke VCC, dan segmen menyala jika katodenya diberi logika LOW. Pada common cathode, semua katode terhubung bersama ke GND, dan segmen menyala jika anodenya diberi logika HIGH." },
{ "en": "Apa fungsi dari IC timer 555 dan sebutkan dua mode operasi utamanya.", "id": "IC serbaguna yang digunakan untuk aplikasi pewaktuan dan osilasi. Dua mode utamanya adalah mode Astable (sebagai osilator/generator gelombang kotak) dan mode Monostable (sebagai timer sekali picu/one-shot pulse generator)." },
{ "en": "Apa yang dimaksud dengan 'multiplexer' (MUX) dan 'demultiplexer' (DEMUX) dalam elektronika digital?", "id": "MUX adalah rangkaian yang memilih salah satu dari beberapa sinyal input untuk diteruskan ke satu output. DEMUX adalah rangkaian yang meneruskan satu sinyal input ke salah satu dari beberapa jalur output, berdasarkan sinyal pemilih." },
{ "en": "Apa itu 'shift register' dan apa saja aplikasinya?", "id": "Rangkaian digital sekuensial yang terdiri dari serangkaian flip-flop, di mana output satu flip-flop terhubung ke input berikutnya, digunakan untuk menyimpan dan menggeser data biner. Aplikasi: konversi serial-ke-paralel, paralel-ke-serial, delay, counter." },
{ "en": "Apa fungsi 'pull-up resistor' dan 'pull-down resistor' pada input digital?", "id": "Pull-up resistor menghubungkan input ke VCC untuk memastikan level HIGH default saat tidak ada sinyal input aktif. Pull-down resistor menghubungkan input ke GND untuk memastikan level LOW default." },
{ "en": "Jelaskan apa itu 'debouncing' pada saklar mekanis.", "id": "Proses menghilangkan efek pantulan (bouncing) kontak pada saklar mekanis yang dapat menghasilkan beberapa transisi sinyal palsu saat saklar ditekan atau dilepas, biasanya dilakukan dengan perangkat keras (RC filter, flip-flop) atau perangkat lunak." },
{ "en": "Apa yang dimaksud dengan 'sampling rate' dan 'resolution' pada ADC?", "id": "Sampling rate adalah seberapa sering ADC mengambil sampel sinyal analog per detik (misalnya dalam Samples/second). Resolution adalah jumlah bit yang digunakan ADC untuk merepresentasikan sinyal analog, menentukan seberapa halus level digital yang dapat dihasilkan (misalnya ADC 10-bit)." },
{ "en": "Apa itu 'aliasing' dalam konteks sampling sinyal analog dan bagaimana Teorema Nyquist berkaitan dengannya?", "id": "Aliasing adalah efek di mana komponen frekuensi tinggi dalam sinyal asli tampak sebagai frekuensi yang lebih rendah setelah sampling. Teorema Nyquist menyatakan bahwa frekuensi sampling harus setidaknya dua kali frekuensi maksimum dalam sinyal untuk menghindari aliasing." },
{ "en": "Apa fungsi dari 'heat sink' yang dipasang pada komponen elektronika seperti transistor daya atau IC regulator?", "id": "Untuk membuang panas yang dihasilkan oleh komponen ke lingkungan sekitar, sehingga menjaga suhu komponen tetap dalam batas aman dan mencegah kerusakan akibat overheating." },
{ "en": "Apa yang dimaksud dengan 'thermal resistance' (resistansi termal)?", "id": "Ukuran seberapa besar suatu material atau antarmuka menahan aliran panas. Semakin rendah resistansi termal, semakin baik material tersebut menghantarkan panas." },
{ "en": "Apa perbedaan antara 'passive components' dan 'active components' dalam elektronika? Berikan contoh masing-masing.", "id": "Komponen pasif tidak dapat menghasilkan penguatan daya (gain) dan tidak memerlukan sumber daya eksternal untuk operasinya (misalnya resistor, kapasitor, induktor). Komponen aktif dapat menghasilkan gain atau mengontrol aliran arus dan memerlukan sumber daya (misalnya transistor, dioda, IC)." },
{ "en": "Apa itu 'TRIAC' dan di mana biasanya digunakan?", "id": "Komponen semikonduktor tiga terminal yang dapat mengalirkan arus AC dua arah, sering digunakan untuk mengontrol daya AC seperti pada dimmer lampu, kontrol kecepatan motor AC, atau saklar solid-state." },
{ "en": "Apa itu 'DIAC' dan bagaimana hubungannya dengan TRIAC?", "id": "Komponen semikonduktor dua terminal yang akan menghantar (breakdown) pada tegangan tertentu di kedua arah. Sering digunakan sebagai pemicu (trigger) untuk gate TRIAC." },
{ "en": "Apa yang dimaksud dengan 'opto-isolator' atau 'optocoupler' dan apa fungsi utamanya?", "id": "Komponen yang mentransmisikan sinyal listrik antara dua rangkaian yang terisolasi secara elektrik menggunakan cahaya (LED sebagai pemancar dan phototransistor/photodiode sebagai penerima). Fungsi utamanya adalah isolasi galvanik dan proteksi." },
{ "en": "Apa itu 'Hall Effect sensor' dan apa yang dapat dideteksinya?", "id": "Sensor yang menghasilkan tegangan output sebagai respons terhadap medan magnet. Dapat digunakan untuk mendeteksi keberadaan, kekuatan, dan polaritas medan magnet, atau untuk mengukur arus (dengan konfigurasi tertentu)." },
{ "en": "Apa perbedaan antara komunikasi serial dan paralel?", "id": "Komunikasi serial mengirimkan data bit per bit secara berurutan melalui satu jalur. Komunikasi paralel mengirimkan beberapa bit data secara bersamaan melalui beberapa jalur paralel." },
{ "en": "Sebutkan contoh protokol komunikasi serial yang umum digunakan.", "id": "RS-232, RS-485, UART, SPI (Serial Peripheral Interface), I2C (Inter-Integrated Circuit)." },
{ "en": "Apa itu 'baud rate' dalam komunikasi serial?", "id": "Jumlah simbol atau perubahan sinyal per detik. Dalam banyak kasus (terutama biner), ini setara dengan bit per detik (bps)." },
{ "en": "Apa fungsi dari 'parity bit' dalam transmisi data serial?", "id": "Bit tambahan yang digunakan untuk deteksi kesalahan sederhana. Paritas bisa genap (even parity) atau ganjil (odd parity), memastikan jumlah bit '1' dalam satu byte (termasuk parity bit) selalu genap atau ganjil." },
{ "en": "Apa yang dimaksud dengan 'microprocessor' (MPU) dan 'microcontroller' (MCU)? Apa perbedaan utamanya?", "id": "MPU adalah unit pemrosesan pusat (CPU) saja dan memerlukan komponen eksternal seperti memori dan periferal I/O. MCU adalah sistem komputer mini dalam satu chip, mengintegrasikan CPU, memori (RAM, ROM/Flash), dan periferal I/O." },
{ "en": "Sebutkan beberapa periferal umum yang terdapat pada mikrokontroler.", "id": "Port GPIO (General Purpose Input/Output), Timer/Counter, ADC, DAC, UART, SPI, I2C, PWM generator, Watchdog Timer." },
{ "en": "Apa fungsi dari 'watchdog timer' (WDT) pada mikrokontroler?", "id": "Untuk mereset mikrokontroler secara otomatis jika program utama mengalami hang atau berhenti merespons. Program harus secara periodik 'menendang' (reset) WDT untuk mencegahnya timeout." },
{ "en": "Apa itu 'interrupt' (interupsi) dalam sistem mikrokontroler?", "id": "Sinyal yang memberitahu prosesor tentang adanya event (kejadian) penting yang memerlukan perhatian segera, menyebabkan prosesor menghentikan sementara eksekusi program utama untuk menjalankan rutin layanan interupsi (ISR)." },
{ "en": "Apa yang dimaksud dengan 'firmware'?", "id": "Perangkat lunak permanen yang diprogram ke dalam memori read-only (seperti ROM, EPROM, atau Flash memory) pada perangkat keras, memberikan instruksi level rendah untuk mengontrol perangkat tersebut." },
{ "en": "Apa itu 'bootloader' pada mikrokontroler atau sistem komputer?", "id": "Program kecil yang berjalan pertama kali saat perangkat dinyalakan, bertugas menginisialisasi perangkat keras dasar dan memuat sistem operasi atau program aplikasi utama ke memori." },
{ "en": "Apa yang dimaksud dengan 'ESD' (Electrostatic Discharge) dan mengapa berbahaya bagi komponen elektronika?", "id": "Pelepasan muatan listrik statis yang tiba-tiba antara dua objek dengan potensial berbeda. Berbahaya karena dapat merusak atau menghancurkan komponen semikonduktor yang sensitif akibat tegangan atau arus tinggi sesaat." },
{ "en": "Bagaimana cara mencegah kerusakan akibat ESD saat menangani komponen elektronika?", "id": "Menggunakan gelang antistatis (wrist strap), alas kerja antistatis (ESD mat), pakaian antistatis, menyimpan komponen dalam kantong antistatis, dan memastikan area kerja ter-ground dengan baik." },
{ "en": "Apa itu 'FPGA' (Field-Programmable Gate Array)?", "id": "IC digital yang berisi blok logika yang dapat dikonfigurasi dan interkoneksi yang dapat diprogram oleh pengguna untuk mengimplementasikan fungsi digital kustom yang kompleks, menawarkan fleksibilitas tinggi." },
{ "en": "Apa itu 'ASIC' (Application-Specific Integrated Circuit)?", "id": "IC yang dirancang dan diproduksi untuk aplikasi atau fungsi spesifik tertentu, biasanya menawarkan kinerja lebih tinggi dan biaya lebih rendah dalam volume produksi besar dibandingkan FPGA, tetapi tidak fleksibel (tidak dapat diprogram ulang)." },
{ "en": "Apa yang dimaksud dengan 'signal integrity' dalam desain PCB kecepatan tinggi?", "id": "Kualitas sinyal listrik saat merambat melalui jalur pada PCB. Isu integritas sinyal meliputi refleksi, crosstalk, ground bounce, dan timing skew, yang dapat menyebabkan kesalahan data." },
{ "en": "Apa itu 'impedance matching' dan mengapa penting dalam transmisi sinyal frekuensi tinggi?", "id": "Proses mencocokkan impedansi sumber, jalur transmisi, dan beban untuk memaksimalkan transfer daya dan meminimalkan refleksi sinyal. Penting untuk mencegah kehilangan sinyal dan distorsi." },
{ "en": "Apa fungsi dari 'terminating resistor' pada jalur transmisi?", "id": "Resistor yang dipasang di ujung jalur transmisi dengan nilai yang sesuai dengan impedansi karakteristik jalur, untuk menyerap energi sinyal dan mencegah refleksi." },
{ "en": "Apa yang dimaksud dengan 'latency' dalam sistem digital atau komunikasi?", "id": "Waktu tunda antara saat suatu event terjadi (misalnya input diberikan) hingga respons atau output yang sesuai dihasilkan." },
{ "en": "Apa itu 'class D amplifier' dalam audio dan apa keunggulan utamanya?", "id": "Jenis penguat audio yang menggunakan teknik switching frekuensi tinggi (seperti PWM) untuk mereproduksi sinyal audio. Keunggulan utamanya adalah efisiensi daya yang sangat tinggi, menghasilkan panas lebih sedikit dan ukuran lebih kecil." },
{ "en": "Apa itu 'PID controller' (Proportional-Integral-Derivative) dan di mana biasanya digunakan?", "id": "Mekanisme kontrol umpan balik loop tertutup yang menghitung nilai 'kesalahan' sebagai selisih antara setpoint yang diinginkan dan variabel proses yang diukur, lalu menerapkan koreksi berdasarkan istilah proporsional, integral, dan derivatif. Banyak digunakan dalam sistem kontrol industri." },
{ "en": "Apa yang dimaksud dengan 'settling time' dalam respons sistem kontrol?", "id": "Waktu yang dibutuhkan oleh output sistem untuk mencapai dan tetap berada dalam rentang toleransi tertentu dari nilai akhirnya (steady-state) setelah adanya perubahan pada input atau setpoint." },
{ "en": "Apa itu 'overshoot' dalam respons sistem kontrol?", "id": "Kondisi di mana output sistem melebihi nilai akhirnya (steady-state) sebelum akhirnya stabil pada nilai tersebut." },
{ "en": "Apa fungsi dari 'analog comparator' pada mikrokontroler?", "id": "Membandingkan dua tegangan input analog dan menghasilkan output digital yang menunjukkan mana tegangan input yang lebih besar." },
{ "en": "Apa perbedaan utama antara filter orde pertama dan filter orde kedua?", "id": "Filter orde kedua memiliki kemiringan roll-off (atenuasi) yang lebih tajam (misalnya -40 dB/dekade atau -12 dB/oktaf) dibandingkan filter orde pertama (-20 dB/dekade atau -6 dB/oktaf)." },
{ "en": "Sebutkan tiga jenis filter dasar berdasarkan respons frekuensinya.", "id": "Low-Pass Filter (LPF), High-Pass Filter (HPF), dan Band-Pass Filter (BPF). (Bisa juga Band-Stop/Notch Filter)." },
{ "en": "Apa yang dimaksud dengan 'Q factor' (faktor kualitas) pada filter atau rangkaian resonansi?", "id": "Parameter yang menunjukkan seberapa 'tajam' atau selektif respons frekuensi filter di sekitar frekuensi tengah (untuk BPF) atau frekuensi cut-off. Q tinggi berarti bandwidth sempit dan selektivitas tinggi." },
{ "en": "Apa perbedaan antara filter Butterworth, Chebyshev, dan Bessel?", "id": "Filter Butterworth memiliki respons paling datar di passband (tanpa ripple). Filter Chebyshev memiliki roll-off lebih tajam tetapi dengan ripple di passband (Tipe I) atau stopband (Tipe II). Filter Bessel memiliki respons fasa paling linear (group delay konstan), baik untuk menjaga bentuk gelombang." },
{ "en": "Apa itu 'phase-locked loop' (PLL) dan sebutkan salah satu aplikasinya.", "id": "Sistem kontrol umpan balik yang menghasilkan sinyal output yang fasanya terkunci (sinkron) dengan fasa sinyal input referensi. Aplikasi: sintesis frekuensi, demodulasi FM, pemulihan clock." },
{ "en": "Apa yang dimaksud dengan 'electromagnetic interference' (EMI) dan 'electromagnetic compatibility' (EMC)?", "id": "EMI adalah gangguan elektromagnetik yang tidak diinginkan yang dapat mempengaruhi kinerja peralatan elektronik. EMC adalah kemampuan peralatan untuk berfungsi dengan benar di lingkungan elektromagnetiknya tanpa menghasilkan EMI yang berlebihan atau terpengaruh oleh EMI dari sumber lain." },
{ "en": "Sebutkan dua cara EMI dapat merambat.", "id": "Melalui konduksi (conducted EMI) yaitu merambat lewat kabel atau jalur listrik, dan melalui radiasi (radiated EMI) yaitu merambat lewat udara sebagai gelombang elektromagnetik." },
{ "en": "Apa fungsi dari 'Faraday cage'?", "id": "Struktur pelindung yang terbuat dari bahan konduktif yang berfungsi untuk memblokir medan elektromagnetik eksternal atau menahan emisi medan elektromagnetik dari dalam." },
{ "en": "Apa itu 'ground plane' pada PCB dan apa manfaatnya?", "id": "Lapisan tembaga besar pada PCB yang terhubung ke ground. Manfaatnya menyediakan jalur balik arus impedansi rendah, referensi tegangan stabil, dan perisai EMI." },
{ "en": "Apa yang dimaksud dengan 'power supply rejection ratio' (PSRR) pada Op-Amp atau IC regulator?", "id": "Kemampuan perangkat untuk menolak variasi atau noise yang ada pada tegangan catu dayanya agar tidak mempengaruhi output." },
{ "en": "Apa itu 'LDO' (Low Dropout Regulator) dan apa keunggulannya dibandingkan regulator linear standar?", "id": "Jenis regulator tegangan linear yang dapat beroperasi dengan perbedaan tegangan input-output (dropout voltage) yang sangat kecil. Keunggulannya adalah efisiensi lebih baik saat tegangan input hanya sedikit lebih tinggi dari output yang diinginkan." },
{ "en": "Apa itu 'switched-mode power supply' (SMPS) dan apa keunggulan utamanya dibandingkan catu daya linear?", "id": "Jenis catu daya yang menggunakan komponen switching (MOSFET/transistor) yang beroperasi pada frekuensi tinggi untuk mentransfer daya. Keunggulannya adalah efisiensi tinggi, ukuran lebih kecil, dan berat lebih ringan." },
{ "en": "Sebutkan dua topologi dasar SMPS.", "id": "Buck (step-down), Boost (step-up), Buck-Boost (step-up/down inverting), Flyback (isolated), Forward (isolated)." },
{ "en": "Apa fungsi dari 'varistor' (Voltage Dependent Resistor) atau MOV (Metal Oxide Varistor)?", "id": "Melindungi rangkaian dari lonjakan tegangan transien (voltage spikes) dengan cara menurunkan resistansinya secara drastis ketika tegangan di atas ambang batasnya, sehingga menyerap energi lonjakan tersebut." },
{ "en": "Apa yang dimaksud dengan 'inrush current' pada catu daya saat pertama kali dinyalakan dan bagaimana cara mengatasinya?", "id": "Arus puncak sesaat yang tinggi yang ditarik oleh catu daya saat kapasitor input diisi. Dapat diatasi dengan NTC thermistor, resistor seri dengan relay bypass, atau rangkaian soft-start." },
{ "en": "Apa itu 'Power Factor Correction' (PFC) dan mengapa penting?", "id": "Teknik untuk membuat beban elektronik (seperti SMPS) tampak seperti beban resistif murni bagi jala-jala AC, sehingga faktor dayanya mendekati 1. Penting untuk efisiensi energi dan mengurangi distorsi harmonik pada jaringan listrik." },
{ "en": "Apa perbedaan antara 'active PFC' dan 'passive PFC'?", "id": "Active PFC menggunakan rangkaian switching aktif (seperti konverter boost) untuk membentuk gelombang arus input agar sefase dengan tegangan. Passive PFC menggunakan komponen pasif (induktor dan kapasitor) untuk memperbaiki faktor daya, biasanya kurang efektif dibandingkan active PFC." },
{ "en": "Apa itu 'System on a Chip' (SoC)?", "id": "IC yang mengintegrasikan sebagian besar atau semua komponen fungsional dari sebuah sistem komputer atau sistem elektronik lainnya (CPU, GPU, memori, periferal, I/O) ke dalam satu chip tunggal." },
{ "en": "Apa yang dimaksud dengan 'Internet of Things' (IoT)?", "id": "Jaringan perangkat fisik, kendaraan, peralatan rumah tangga, dan item lain yang disematkan dengan elektronika, perangkat lunak, sensor, aktuator, dan konektivitas jaringan yang memungkinkan objek-objek ini terhubung dan bertukar data." },
{ "en": "Sebutkan salah satu teknologi komunikasi nirkabel yang umum digunakan untuk IoT.", "id": "Wi-Fi, Bluetooth Low Energy (BLE), LoRaWAN, Sigfox, NB-IoT, Zigbee." },
{ "en": "Apa itu 'edge computing' dalam konteks IoT?", "id": "Model komputasi terdistribusi di mana pemrosesan data dan analisis dilakukan lebih dekat ke sumber data (perangkat IoT atau sensor) daripada di pusat data terpusat atau cloud, untuk mengurangi latensi dan penggunaan bandwidth." },
{ "en": "Apa yang dimaksud dengan 'sensor fusion'?", "id": "Proses menggabungkan data dari beberapa sensor yang berbeda (misalnya akselerometer, giroskop, magnetometer) untuk menghasilkan informasi yang lebih akurat, andal, atau komprehensif daripada yang dapat diperoleh dari satu sensor saja." },
{ "en": "Apa itu 'OLED' (Organic Light Emitting Diode) dan apa keunggulannya dalam teknologi display?", "id": "Jenis LED di mana lapisan emisifnya terbuat dari senyawa organik yang memancarkan cahaya. Keunggulannya: rasio kontras tinggi (hitam pekat), sudut pandang lebar, waktu respons cepat, potensi display fleksibel." },
{ "en": "Apa itu 'MEMS' (Micro-Electro-Mechanical Systems)? Berikan contoh aplikasinya.", "id": "Teknologi yang mengintegrasikan komponen mekanis dan elektris berukuran mikro pada substrat semikonduktor. Contoh: akselerometer, giroskop pada smartphone, mikrofon MEMS, inkjet printhead." },
{ "en": "Apa yang dimaksud dengan 'quantum dot' dalam teknologi display?", "id": "Nanokristal semikonduktor yang memancarkan cahaya dengan warna sangat murni (tergantung ukurannya) saat disinari atau dialiri listrik, digunakan untuk meningkatkan gamut warna dan efisiensi display." },
{ "en": "Apa itu 'LiDAR' (Light Detection and Ranging) dan bagaimana prinsip kerjanya?", "id": "Metode penginderaan jauh yang menggunakan pulsa laser untuk mengukur jarak ke objek dengan menghitung waktu tempuh pantulan cahaya. Banyak digunakan dalam mobil otonom dan pemetaan 3D." },
{ "en": "Apa fungsi dari 'voltage reference' (referensi tegangan) yang stabil dan akurat dalam ADC atau DAC?", "id": "Menyediakan level tegangan yang presisi sebagai acuan untuk proses konversi. Akurasi konversi sangat bergantung pada stabilitas dan akurasi referensi tegangan ini." },
{ "en": "Jelaskan apa itu 'charge pump' dalam rangkaian elektronik.", "id": "Jenis konverter DC-DC yang menggunakan kapasitor untuk menyimpan dan mentransfer muatan listrik guna menghasilkan tegangan output yang lebih tinggi, lebih rendah, atau berpolaritas terbalik dari tegangan input, tanpa menggunakan induktor." },
{ "en": "Apa yang dimaksud dengan 'haptic feedback' pada perangkat elektronik?", "id": "Umpan balik sensorik berupa sentuhan atau getaran yang dihasilkan oleh perangkat untuk memberikan respons atau informasi kepada pengguna, misalnya getaran pada smartphone." },
{ "en": "Apa perbedaan utama antara 'inductive proximity sensor' dan 'capacitive proximity sensor'?", "id": "Inductive sensor mendeteksi keberadaan objek logam dengan mengukur perubahan induktansi akibat medan magnet Eddy current. Capacitive sensor mendeteksi objek konduktif maupun non-konduktif (dielektrik) dengan mengukur perubahan kapasitansi." },
{ "en": "Apa itu 'Finite State Machine' (FSM) dalam desain logika digital?", "id": "Model komputasi abstrak yang terdiri dari sejumlah state (kondisi), transisi antar state yang dipicu oleh input, dan output yang terkait dengan state atau transisi. Digunakan untuk merancang sistem sekuensial." },
{ "en": "Apa perbedaan antara FSM tipe Mealy dan FSM tipe Moore?", "id": "Pada FSM Mealy, output bergantung pada state saat ini DAN input saat ini. Pada FSM Moore, output HANYA bergantung pada state saat ini." },
{ "en": "Apa itu 'Verilog' dan 'VHDL'?", "id": "Keduanya adalah Hardware Description Language (HDL) yang digunakan untuk mendeskripsikan, merancang, dan mensimulasikan sirkuit elektronik digital, terutama untuk implementasi pada FPGA dan ASIC." },
{ "en": "Apa yang dimaksud dengan 'clock skew' dalam sistem digital sinkron dan mengapa bisa menjadi masalah?", "id": "Perbedaan waktu kedatangan sinyal clock pada komponen-komponen atau flip-flop yang berbeda dalam satu sistem. Dapat menyebabkan masalah timing, seperti pelanggaran setup time atau hold time, yang mengakibatkan operasi yang salah." },
{ "en": "Jelaskan apa itu 'setup time' dan 'hold time' pada sebuah flip-flop.", "id": "Setup time adalah waktu minimum data input harus stabil SEBELUM tepi aktif sinyal clock tiba. Hold time adalah waktu minimum data input harus tetap stabil SETELAH tepi aktif sinyal clock tiba, agar data dapat ditangkap dengan benar oleh flip-flop." },
{ "en": "Apa yang dimaksud dengan 'metastability' pada flip-flop dan kapan bisa terjadi?", "id": "Kondisi tidak stabil dan tidak dapat diprediksi pada output flip-flop (antara logika 0 dan 1 untuk waktu yang tidak pasti) yang bisa terjadi jika timing setup atau hold time dilanggar, atau jika input clock atau data bersifat asinkron." },
{ "en": "Apa perbedaan mendasar antara memori SRAM (Static RAM) dan DRAM (Dynamic RAM)?", "id": "SRAM menggunakan sel memori berbasis flip-flop (biasanya 6 transistor) untuk menyimpan setiap bit, lebih cepat dan tidak memerlukan refresh, tetapi lebih mahal dan kurang padat. DRAM menggunakan sel memori berbasis kapasitor (1 transistor dan 1 kapasitor) untuk menyimpan bit sebagai muatan, lebih murah dan padat, tetapi lebih lambat dan memerlukan refresh periodik." },
{ "en": "Apa itu 'EEPROM' (Electrically Erasable Programmable Read-Only Memory)?", "id": "Jenis memori non-volatil yang dapat dihapus dan diprogram ulang secara elektrik per byte atau per blok kecil, sering digunakan untuk menyimpan data konfigurasi atau parameter yang jarang berubah." },
{ "en": "Apa itu 'Flash Memory' dan apa perbedaannya dengan EEPROM?", "id": "Jenis memori non-volatil yang mirip EEPROM tetapi biasanya dihapus per blok (sector) yang lebih besar, menawarkan kepadatan lebih tinggi dan biaya per bit lebih rendah. Umum digunakan di SSD, USB drive, dan kartu memori." },
{ "en": "Apa fungsi dari 'bus' dalam sistem komputer atau mikrokontroler?", "id": "Sekelompok jalur konduktif paralel yang digunakan untuk mentransfer data, alamat, dan sinyal kontrol antara berbagai komponen sistem (CPU, memori, periferal)." },
{ "en": "Sebutkan tiga jenis bus utama dalam arsitektur komputer.", "id": "Bus Data (Data Bus), Bus Alamat (Address Bus), dan Bus Kontrol (Control Bus)." },
{ "en": "Apa yang dimaksud dengan 'direct memory access' (DMA) controller?", "id": "Fitur perangkat keras yang memungkinkan periferal tertentu mentransfer data langsung ke atau dari memori utama tanpa melibatkan CPU secara terus-menerus, sehingga membebaskan CPU untuk tugas lain dan meningkatkan throughput data." },
{ "en": "Apa itu 'cache memory' dan apa tujuannya?", "id": "Memori kecil berkecepatan sangat tinggi yang terletak di antara CPU dan memori utama (RAM). Tujuannya untuk menyimpan data dan instruksi yang sering diakses oleh CPU, sehingga mengurangi waktu akses rata-rata ke memori." },
{ "en": "Jelaskan konsep 'pipelining' dalam desain prosesor.", "id": "Teknik di mana eksekusi instruksi dipecah menjadi beberapa tahap (stage) yang tumpang tindih, memungkinkan prosesor mengerjakan beberapa instruksi secara bersamaan dalam tahap yang berbeda, sehingga meningkatkan throughput instruksi." },
{ "en": "Apa perbedaan antara arsitektur prosesor CISC (Complex Instruction Set Computer) dan RISC (Reduced Instruction Set Computer)?", "id": "CISC memiliki set instruksi yang besar dan kompleks, di mana satu instruksi dapat melakukan beberapa operasi level rendah. RISC memiliki set instruksi yang lebih kecil, sederhana, dan seragam, di mana setiap instruksi biasanya dieksekusi dalam satu siklus clock." },
{ "en": "Apa itu 'Von Neumann architecture' dan 'Harvard architecture' dalam desain komputer?", "id": "Arsitektur Von Neumann menggunakan satu ruang alamat dan bus tunggal untuk instruksi dan data. Arsitektur Harvard menggunakan ruang alamat dan bus terpisah untuk instruksi dan data, memungkinkan pengambilan instruksi dan data secara bersamaan." },
{ "en": "Apa yang dimaksud dengan 'endianness' (misalnya Big-Endian vs Little-Endian)?", "id": "Urutan di mana byte-byte dari sebuah kata data multibyte disimpan dalam memori. Big-Endian menyimpan byte paling signifikan (MSB) di alamat memori terendah. Little-Endian menyimpan byte paling tidak signifikan (LSB) di alamat memori terendah." },
{ "en": "Apa itu 'JTAG' (Joint Test Action Group) dan apa kegunaan utamanya?", "id": "Standar antarmuka (IEEE 1149.1) yang digunakan untuk pengujian papan sirkuit tercetak (boundary scan), debugging perangkat keras, dan pemrograman IC seperti FPGA dan mikrokontroler di dalam sistem (in-system programming)." },
{ "en": "Apa yang dimaksud dengan 'SystemVerilog'?", "id": "Ekstensi dari Verilog HDL yang menambahkan banyak fitur baru untuk desain, verifikasi, dan pemodelan sistem elektronik yang lebih kompleks, termasuk fitur berorientasi objek dan constraint-random verification." },
{ "en": "Apa itu 'Universal Verification Methodology' (UVM)?", "id": "Metodologi standar industri yang berbasis SystemVerilog untuk membuat lingkungan verifikasi yang dapat digunakan kembali (reusable) dan interoperable untuk IC dan SoC yang kompleks." },
{ "en": "Apa yang dimaksud dengan 'Formal Verification' dalam desain IC?", "id": "Teknik verifikasi yang menggunakan metode matematika formal untuk membuktikan atau menyangkal kebenaran suatu desain terhadap spesifikasinya, tanpa memerlukan simulasi berbasis input test vector." },
{ "en": "Apa itu 'Low Power Design' dalam elektronika digital dan mengapa penting?", "id": "Sekumpulan teknik desain yang bertujuan untuk mengurangi konsumsi daya pada IC dan sistem elektronik. Penting untuk perangkat bertenaga baterai, mengurangi panas, dan meningkatkan keandalan." },
{ "en": "Sebutkan salah satu teknik untuk mengurangi konsumsi daya dinamis dalam rangkaian CMOS.", "id": "Mengurangi tegangan suplai (voltage scaling), mengurangi frekuensi clock (frequency scaling), clock gating (mematikan clock ke bagian yang tidak digunakan), mengurangi kapasitansi switching." },
{ "en": "Sebutkan salah satu teknik untuk mengurangi konsumsi daya statis (arus bocor) dalam rangkaian CMOS.", "id": "Menggunakan transistor dengan tegangan threshold lebih tinggi, power gating (mematikan suplai daya ke blok yang tidak aktif), body biasing." },
{ "en": "Apa itu 'Design for Testability' (DFT)?", "id": "Sekumpulan teknik desain yang dimasukkan ke dalam IC untuk mempermudah proses pengujian (testing) setelah fabrikasi, bertujuan untuk meningkatkan cakupan tes dan mengurangi biaya tes." },
{ "en": "Sebutkan salah satu teknik DFT yang umum.", "id": "Scan Chain (memungkinkan akses internal ke flip-flop), Built-In Self-Test (BIST) (IC dapat menguji dirinya sendiri)." },
{ "en": "Apa itu 'Yield' dalam fabrikasi semikonduktor?", "id": "Persentase chip yang berfungsi dengan benar (memenuhi spesifikasi) dari total chip yang diproduksi pada satu wafer silikon." },
{ "en": "Apa itu 'cleanroom' dalam pabrik semikonduktor dan mengapa sangat penting?", "id": "Ruangan dengan tingkat kontaminasi partikel yang sangat rendah (debu, mikroorganisme) yang dikontrol secara ketat. Sangat penting karena partikel sekecil apapun dapat menyebabkan cacat pada IC selama proses fabrikasi." },
{ "en": "Sebutkan salah satu langkah utama dalam proses fabrikasi IC (misalnya, litografi).", "id": "Litografi (photolithography), etsa (etching), deposisi (deposition), implantasi ion (ion implantation), oksidasi, planarization (CMP)." },
{ "en": "Apa yang dimaksud dengan 'Moore's Law'?", "id": "Observasi (bukan hukum fisika) yang dibuat oleh Gordon Moore pada tahun 1965 bahwa jumlah transistor pada sebuah IC (Integrated Circuit) berlipat ganda kira-kira setiap dua tahun, yang juga berarti peningkatan kinerja dan penurunan biaya." },
{ "en": "Apa tantangan utama yang dihadapi dalam melanjutkan Moore's Law saat ini?", "id": "Batas fisik ukuran transistor (efek kuantum, kebocoran), meningkatnya biaya fabrikasi, disipasi panas yang tinggi, kompleksitas desain yang ekstrem." },
{ "en": "Apa itu '3D IC' (Three-Dimensional Integrated Circuit)?", "id": "Pendekatan di mana beberapa lapisan die (chip) semikonduktor ditumpuk secara vertikal dan dihubungkan menggunakan Through-Silicon Vias (TSV) untuk meningkatkan kepadatan dan kinerja, serta mengurangi panjang interkoneksi." },
{ "en": "Apa itu 'neuromorphic computing'?", "id": "Bidang penelitian yang bertujuan untuk merancang dan membangun sistem komputer yang arsitekturnya terinspirasi oleh otak biologis, dengan fokus pada efisiensi energi dan kemampuan belajar." },
{ "en": "Apa itu 'spintronics' (spin transport electronics)?", "id": "Bidang elektronika yang memanfaatkan sifat intrinsik spin elektron (selain muatannya) untuk menyimpan, memproses, dan mentransmisikan informasi, berpotensi menghasilkan perangkat yang lebih cepat dan hemat energi." },
{ "en": "Apa itu 'photonic integrated circuit' (PIC)?", "id": "IC yang menggunakan foton (cahaya) daripada elektron untuk membawa dan memproses informasi, menawarkan potensi bandwidth sangat tinggi dan konsumsi daya rendah untuk aplikasi seperti komunikasi data kecepatan tinggi." },
{ "en": "Apa yang dimaksud dengan 'wireless power transfer' (WPT)?", "id": "Transmisi energi listrik dari sumber daya ke beban listrik tanpa menggunakan konduktor fisik (kabel), misalnya melalui induksi magnetik atau resonansi magnetik." },
{ "en": "Sebutkan dua metode utama WPT.", "id": "Inductive coupling (jarak dekat, seperti pada charger nirkabel Qi) dan resonant inductive coupling (jarak menengah)." },
{ "en": "Apa itu 'energy harvesting'?", "id": "Proses menangkap dan menyimpan energi dari sumber lingkungan sekitar yang tersedia secara bebas (seperti cahaya matahari, getaran, panas, gelombang RF) untuk memberi daya pada perangkat elektronik berdaya rendah." },
{ "en": "Sebutkan contoh sumber energi untuk energy harvesting.", "id": "Sel surya (cahaya), generator piezoelektrik (getaran/tekanan), generator termoelektrik (perbedaan suhu), antena RF (gelombang radio)." },
{ "en": "Apa itu 'smart grid'?", "id": "Jaringan listrik modern yang menggunakan teknologi informasi dan komunikasi untuk mengumpulkan dan bertindak berdasarkan informasi tentang perilaku pemasok dan konsumen guna meningkatkan efisiensi, keandalan, dan keberlanjutan produksi dan distribusi listrik." },
{ "en": "Apa peran elektronika daya dalam sistem energi terbarukan seperti panel surya atau turbin angin?", "id": "Mengkonversi daya DC yang dihasilkan (misalnya dari panel surya) menjadi AC yang sesuai untuk jala-jala (menggunakan inverter), mengatur tegangan dan arus, melakukan Maximum Power Point Tracking (MPPT)." },
{ "en": "Apa itu 'Maximum Power Point Tracking' (MPPT) pada sistem panel surya?", "id": "Teknik yang digunakan oleh inverter atau charge controller untuk secara dinamis menyesuaikan impedansi input agar panel surya beroperasi pada titik daya output maksimumnya, yang bervariasi tergantung kondisi penyinaran dan suhu." },
{ "en": "Apa itu 'electric vehicle' (EV) dan apa komponen elektronika utama di dalamnya?", "id": "Kendaraan yang ditenagai oleh satu atau lebih motor listrik, menggunakan energi yang disimpan dalam baterai. Komponen elektronika utama: baterai, Battery Management System (BMS), inverter (motor controller), on-board charger, motor listrik." },
{ "en": "Apa fungsi dari 'Battery Management System' (BMS) pada baterai EV atau perangkat portabel?", "id": "Memantau dan mengelola kondisi baterai (tegangan, arus, suhu sel), melindungi dari overcharge, over-discharge, overheating, menyeimbangkan sel (cell balancing), dan memperkirakan state-of-charge (SoC) dan state-of-health (SoH)." },
{ "en": "Apa itu 'regenerative braking' pada kendaraan listrik atau hibrida?", "id": "Proses di mana motor listrik bertindak sebagai generator saat pengereman atau deselerasi, mengubah energi kinetik kendaraan kembali menjadi energi listrik untuk mengisi ulang baterai." },
{ "en": "Apa yang dimaksud dengan 'avionics'?", "id": "Sistem elektronik yang digunakan pada pesawat terbang, satelit, dan wahana antariksa, meliputi sistem navigasi, komunikasi, display, kontrol penerbangan, dan sensor." },
{ "en": "Apa itu 'radar' (Radio Detection and Ranging) dan bagaimana prinsip kerjanya?", "id": "Sistem yang menggunakan gelombang radio untuk mendeteksi keberadaan, jarak, kecepatan, dan arah objek. Prinsipnya memancarkan gelombang radio dan menganalisis pantulannya dari objek." },
{ "en": "Apa itu 'sonar' (Sound Navigation and Ranging)?", "id": "Teknik yang menggunakan propagasi suara (biasanya di bawah air) untuk navigasi, komunikasi, atau mendeteksi objek di dalam atau di permukaan air, mirip dengan radar tetapi menggunakan gelombang suara." },
{ "en": "Apa yang dimaksud dengan 'Global Positioning System' (GPS)?", "id": "Sistem navigasi berbasis satelit yang menyediakan informasi lokasi geografis dan waktu kepada penerima GPS di mana saja di atau dekat Bumi yang memiliki garis pandang tanpa halangan ke empat atau lebih satelit GPS." },
{ "en": "Apa itu 'biomedical electronics' (elektronika biomedis)?", "id": "Aplikasi prinsip dan teknik elektronika dalam bidang medis, untuk diagnosis, terapi, monitoring, dan penelitian. Contoh: ECG, EEG, MRI, alat pacu jantung, implan koklea." },
{ "en": "Apa fungsi dari 'Electrocardiogram' (ECG atau EKG)?", "id": "Merekam aktivitas listrik jantung dari waktu ke waktu menggunakan elektroda yang ditempelkan pada kulit, digunakan untuk mendiagnosis berbagai kondisi jantung." },
{ "en": "Apa fungsi dari 'Electroencephalogram' (EEG)?", "id": "Merekam aktivitas listrik otak menggunakan elektroda yang ditempelkan pada kulit kepala, digunakan untuk mendiagnosis epilepsi, gangguan tidur, dan kondisi neurologis lainnya." },
{ "en": "Apa itu 'pulse oximeter'?", "id": "Perangkat medis non-invasif yang mengukur saturasi oksigen dalam darah (SpO2) dan denyut nadi seseorang." },
{ "en": "Apa itu 'pacemaker' (alat pacu jantung)?", "id": "Perangkat medis implan yang menghasilkan pulsa listrik untuk merangsang otot jantung agar berkontraksi dan menjaga denyut jantung yang adekuat ketika sistem konduksi alami jantung tidak dapat berfungsi dengan baik." },
{ "en": "Apa itu 'defibrillator'?", "id": "Perangkat medis yang mengirimkan dosis terapi energi listrik ke jantung pasien yang mengalami aritmia jantung berbahaya seperti fibrilasi ventrikel atau takikardia ventrikel tanpa denyut, untuk memulihkan ritme jantung normal." },
// AWAL DARI BATCH SOAL BARU (Lanjutan untuk mencapai 471 tambahan)
// Target: 150-200 soal baru di batch ini
{ "en": "Apa yang dimaksud dengan 'antena isotropik' dan mengapa ini merupakan konsep teoretis?", "id": "Antena isotropik adalah antena teoretis tanpa rugi-rugi yang memancarkan daya secara seragam ke segala arah (sferis). Ini adalah konsep teoretis karena antena praktis selalu memiliki pola radiasi tertentu (direktivitas)." },
{ "en": "Jelaskan perbedaan antara 'gain' dan 'direktivitas' pada antena.", "id": "Direktivitas adalah ukuran seberapa baik antena memusatkan daya pancar ke arah tertentu dibandingkan antena isotropik. Gain memperhitungkan direktivitas dan efisiensi antena (rugi-rugi)." },
{ "en": "Apa itu 'panjang gelombang' (wavelength) dan bagaimana hubungannya dengan frekuensi sinyal?", "id": "Panjang gelombang adalah jarak spasial satu siklus gelombang. Hubungannya dengan frekuensi adalah $\lambda = c/f$, di mana $\lambda$ adalah panjang gelombang, $c$ adalah kecepatan cahaya, dan $f$ adalah frekuensi." },
{ "en": "Sebutkan setidaknya tiga jenis antena yang umum digunakan.", "id": "Antena dipol (dipole), antena Yagi-Uda, antena parabola (parabolic dish), antena patch (microstrip), antena monopole, antena loop." },
{ "en": "Apa fungsi dari 'impedance matching network' pada sistem antena?", "id": "Untuk mencocokkan impedansi antena dengan impedansi jalur transmisi (kabel) dan pemancar/penerima, guna memaksimalkan transfer daya dan meminimalkan pantulan sinyal (SWR rendah)." },
{ "en": "Apa yang dimaksud dengan 'polarisasi' gelombang elektromagnetik yang dipancarkan antena?", "id": "Orientasi medan listrik dari gelombang radio yang dipancarkan. Polarisasi bisa linear (vertikal, horizontal) atau sirkular (kanan, kiri)." },
{ "en": "Apa itu 'bandwidth' antena?", "id": "Rentang frekuensi di mana antena dapat beroperasi secara efektif, biasanya ditentukan oleh parameter seperti SWR, gain, atau pola radiasi yang dapat diterima." },
{ "en": "Apa fungsi dari 'radome' pada antena?", "id": "Penutup pelindung antena (biasanya terbuat dari bahan dielektrik) yang melindungi antena dari cuaca dan lingkungan, sambil bersifat transparan terhadap gelombang radio." },
{ "en": "Apa yang dimaksud dengan 'near field' dan 'far field' region pada antena?", "id": "Near field adalah daerah dekat antena di mana medan elektromagnetik kompleks dan reaktif. Far field adalah daerah yang lebih jauh di mana pola radiasi stabil dan gelombang bersifat planar." },
{ "en": "Apa itu 'array antena' (antenna array)?", "id": "Susunan beberapa elemen antena individual yang diatur dan diberi fasa tertentu untuk menghasilkan pola radiasi gabungan yang diinginkan (misalnya gain lebih tinggi atau beam steering)." },
{ "en": "Jelaskan konsep 'beamforming' pada array antena.", "id": "Teknik memanipulasi fasa dan amplitudo sinyal pada setiap elemen array antena untuk mengarahkan berkas pancaran utama (main beam) ke arah tertentu dan/atau menekan interferensi dari arah lain." },
{ "en": "Apa itu 'RFID' (Radio-Frequency Identification) dan bagaimana cara kerjanya?", "id": "Teknologi identifikasi otomatis yang menggunakan gelombang radio untuk mentransfer data antara tag RFID (yang terpasang pada objek) dan pembaca RFID. Pembaca mengirimkan sinyal interogasi, dan tag merespons dengan informasi identifikasi." },
{ "en": "Apa perbedaan antara tag RFID pasif dan aktif?", "id": "Tag RFID pasif tidak memiliki sumber daya sendiri dan mendapatkan daya dari sinyal RF yang dipancarkan pembaca. Tag RFID aktif memiliki baterai sendiri dan dapat mentransmisikan sinyal dengan jangkauan lebih jauh." },
{ "en": "Apa itu 'NFC' (Near Field Communication) dan apa bedanya dengan RFID tradisional?", "id": "NFC adalah subset dari teknologi RFID yang beroperasi pada frekuensi tinggi (13.56 MHz) dan dirancang untuk komunikasi jarak sangat dekat (beberapa sentimeter). Memungkinkan komunikasi dua arah." },
{ "en": "Apa yang dimaksud dengan 'software-defined networking' (SDN)?", "id": "Arsitektur jaringan di mana bidang kontrol (control plane) dipisahkan dari bidang data (data plane), memungkinkan kontrol jaringan terpusat dan terprogram melalui perangkat lunak." },
{ "en": "Apa itu 'Network Function Virtualization' (NFV)?", "id": "Konsep memvirtualisasikan fungsi-fungsi jaringan (seperti firewall, load balancer, router) yang secara tradisional berjalan pada perangkat keras khusus, sehingga dapat dijalankan sebagai perangkat lunak pada server standar." },
{ "en": "Apa yang dimaksud dengan 'Quality of Service' (QoS) dalam jaringan komputer?", "id": "Kemampuan jaringan untuk memberikan prioritas atau jaminan layanan yang berbeda untuk berbagai jenis lalu lintas data, guna memastikan kinerja yang memadai untuk aplikasi sensitif (misalnya VoIP, video streaming)." },
{ "en": "Jelaskan perbedaan antara protokol TCP (Transmission Control Protocol) dan UDP (User Datagram Protocol).", "id": "TCP adalah protokol connection-oriented yang andal (reliable) dengan pengecekan error dan pengurutan paket. UDP adalah protokol connectionless yang tidak andal (unreliable, best-effort) tanpa jaminan pengiriman atau urutan, tetapi lebih cepat dan overhead lebih rendah." },
{ "en": "Apa itu 'IP address' (Internet Protocol address)?", "id": "Label numerik unik yang ditetapkan untuk setiap perangkat yang terhubung ke jaringan komputer yang menggunakan Internet Protocol untuk komunikasi." },
{ "en": "Apa perbedaan antara IPv4 dan IPv6?", "id": "IPv4 menggunakan alamat 32-bit (sekitar 4.3 miliar alamat). IPv6 menggunakan alamat 128-bit, menyediakan ruang alamat yang jauh lebih besar untuk mengatasi kelangkaan alamat IPv4." },
{ "en": "Apa fungsi dari 'Domain Name System' (DNS)?", "id": "Sistem terdistribusi yang menerjemahkan nama domain yang mudah diingat manusia (misalnya www.google.com) menjadi alamat IP numerik yang digunakan oleh komputer untuk saling menemukan di jaringan." },
{ "en": "Apa itu 'firewall' dalam keamanan jaringan?", "id": "Sistem keamanan jaringan yang memantau dan mengontrol lalu lintas jaringan masuk dan keluar berdasarkan aturan keamanan yang telah ditentukan, bertindak sebagai penghalang antara jaringan internal yang aman dan jaringan eksternal yang tidak aman." },
{ "en": "Apa yang dimaksud dengan 'VPN' (Virtual Private Network)?", "id": "Teknologi yang menciptakan koneksi aman dan terenkripsi melalui jaringan publik (seperti internet) seolah-olah perangkat terhubung secara langsung dalam jaringan pribadi, untuk privasi dan akses jarak jauh." },
{ "en": "Apa itu 'encryption' (enkripsi) dan 'decryption' (dekripsi)?", "id": "Enkripsi adalah proses mengubah data (plaintext) menjadi format yang tidak dapat dibaca (ciphertext) menggunakan algoritma dan kunci. Dekripsi adalah proses mengembalikan ciphertext ke plaintext asli menggunakan kunci yang sesuai." },
{ "en": "Apa perbedaan antara enkripsi simetris dan asimetris?", "id": "Enkripsi simetris menggunakan kunci yang sama untuk enkripsi dan dekripsi. Enkripsi asimetris (public-key cryptography) menggunakan sepasang kunci: kunci publik untuk enkripsi dan kunci privat untuk dekripsi." },
{ "en": "Apa itu 'digital signature' (tanda tangan digital)?", "id": "Skema matematika untuk memverifikasi keaslian dan integritas pesan atau dokumen digital, menggunakan enkripsi asimetris di mana pengirim menandatangani dengan kunci privatnya, dan penerima memverifikasi dengan kunci publik pengirim." },
{ "en": "Apa itu 'cryptographic hash function'?", "id": "Algoritma yang mengambil input data berukuran variabel dan menghasilkan output string berukuran tetap (hash value atau message digest) yang unik untuk input tersebut. Digunakan untuk verifikasi integritas data." },
{ "en": "Apa yang dimaksud dengan 'blockchain'?", "id": "Buku besar digital terdistribusi dan tidak dapat diubah (immutable) yang terdiri dari blok-blok transaksi yang dihubungkan secara kriptografis. Setiap blok berisi hash dari blok sebelumnya, timestamp, dan data transaksi." },
{ "en": "Apa itu 'cryptocurrency' (mata uang kripto)? Berikan contoh.", "id": "Aset digital yang dirancang untuk berfungsi sebagai media pertukaran yang menggunakan kriptografi kuat untuk mengamankan transaksi keuangan, mengontrol pembuatan unit tambahan, dan memverifikasi transfer aset. Contoh: Bitcoin, Ethereum." },
{ "en": "Apa yang dimaksud dengan 'smart contract' pada platform blockchain?", "id": "Program komputer atau protokol transaksi yang secara otomatis mengeksekusi, mengontrol, atau mendokumentasikan peristiwa dan tindakan yang relevan secara hukum sesuai dengan ketentuan perjanjian atau negosiasi." },
{ "en": "Apa itu 'augmented reality' (AR)?", "id": "Teknologi yang menggabungkan objek virtual yang dihasilkan komputer ke dalam pandangan dunia nyata pengguna secara real-time, meningkatkan persepsi pengguna tentang lingkungan sekitarnya." },
{ "en": "Apa itu 'virtual reality' (VR)?", "id": "Teknologi yang menciptakan pengalaman imersif bagi pengguna dengan menggantikan lingkungan dunia nyata mereka dengan lingkungan virtual yang dihasilkan komputer, biasanya melalui headset VR." },
{ "en": "Apa perbedaan utama antara AR dan VR?", "id": "AR menambahkan elemen digital ke dunia nyata pengguna, sedangkan VR sepenuhnya menggantikan dunia nyata dengan lingkungan virtual. AR memungkinkan interaksi dengan dunia nyata, VR memutus pengguna dari dunia nyata." },
{ "en": "Apa itu 'holography'?", "id": "Teknik yang memungkinkan gelombang cahaya dari suatu objek direkam dan kemudian direkonstruksi untuk menghasilkan tampilan tiga dimensi (hologram) dari objek tersebut, yang dapat dilihat tanpa memerlukan kacamata khusus." },
{ "en": "Apa yang dimaksud dengan 'User Interface' (UI) dan 'User Experience' (UX) dalam desain produk elektronik?", "id": "UI adalah tampilan visual dan interaktif dari sebuah produk yang memungkinkan pengguna berinteraksi dengannya. UX adalah keseluruhan pengalaman, persepsi, dan kepuasan pengguna saat menggunakan produk tersebut." },
{ "en": "Apa itu 'ergonomics' dalam desain perangkat elektronik?", "id": "Studi tentang bagaimana merancang peralatan dan perangkat agar sesuai dengan kemampuan dan batasan tubuh manusia, bertujuan untuk meningkatkan efisiensi, kenyamanan, dan mengurangi risiko cedera atau ketidaknyamanan." },
{ "en": "Apa yang dimaksud dengan 'planned obsolescence' (usang terencana) dalam produk elektronik?", "id": "Kebijakan merancang produk dengan masa pakai terbatas atau membuatnya menjadi usang secara artifisial (misalnya dengan menghentikan dukungan perangkat lunak atau ketersediaan suku cadang) untuk mendorong konsumen membeli model yang lebih baru." },
{ "en": "Apa itu 'e-waste' (sampah elektronik) dan mengapa menjadi masalah lingkungan?", "id": "Peralatan listrik dan elektronik yang dibuang. Menjadi masalah karena sering mengandung bahan berbahaya (timbal, merkuri, kadmium) yang dapat mencemari lingkungan jika tidak didaur ulang atau dibuang dengan benar." },
{ "en": "Sebutkan salah satu metode daur ulang e-waste.", "id": "Pembongkaran manual, pemisahan mekanis (shredding, magnetic separation), pemurnian logam (smelting, hydrometallurgy, pyrometallurgy)." },
{ "en": "Apa itu 'circular economy' (ekonomi sirkular) dalam konteks elektronika?", "id": "Model ekonomi yang bertujuan untuk menghilangkan limbah dan memaksimalkan penggunaan sumber daya dengan menjaga produk, komponen, dan material tetap dalam penggunaan selama mungkin melalui desain ulang, perbaikan, penggunaan kembali, dan daur ulang." },
{ "en": "Apa yang dimaksud dengan 'right to repair' (hak untuk memperbaiki)?", "id": "Gerakan yang mengadvokasi agar konsumen dan bengkel independen memiliki akses ke suku cadang, alat, dan informasi (manual, skematik) yang diperlukan untuk memperbaiki produk elektronik mereka sendiri, sebagai lawan dari praktik produsen yang membatasi perbaikan." },
{ "en": "Apa itu 'open source hardware' (perangkat keras sumber terbuka)?", "id": "Perangkat keras yang desainnya (skematik, layout PCB, bill of materials) tersedia secara publik sehingga siapa pun dapat mempelajari, memodifikasi, membuat, dan mendistribusikan perangkat keras tersebut dan turunannya." },
{ "en": "Berikan contoh platform open source hardware yang populer.", "id": "Arduino, Raspberry Pi (sebagian aspek), BeagleBone." },
{ "en": "Apa itu 'datasheet' komponen dan mengapa penting bagi seorang insinyur elektronika?", "id": "Dokumen teknis yang disediakan pabrikan yang merinci spesifikasi, karakteristik, parameter operasional, rating maksimum, dan informasi aplikasi dari suatu komponen. Sangat penting untuk memahami cara kerja komponen, memilih komponen yang tepat, dan merancang rangkaian dengan benar." },
{ "en": "Jelaskan apa itu 'derating' komponen elektronika.", "id": "Praktik mengoperasikan komponen di bawah batas maksimum yang ditentukan dalam datasheet-nya (misalnya, daya, tegangan, arus, suhu) untuk meningkatkan keandalan, memperpanjang masa pakai, dan memberikan margin keamanan." },
{ "en": "Apa yang dimaksud dengan 'Mean Time Between Failures' (MTBF) dan 'Mean Time To Failure' (MTTF)?", "id": "MTBF adalah waktu rata-rata yang diprediksi antara kegagalan yang melekat pada sistem mekanis atau elektronik yang dapat diperbaiki. MTTF adalah waktu rata-rata yang diharapkan hingga kegagalan pertama pada sistem yang tidak dapat diperbaiki." },
{ "en": "Apa itu 'Failure Mode and Effects Analysis' (FMEA)?", "id": "Pendekatan sistematis untuk mengidentifikasi potensi mode kegagalan dalam suatu sistem atau proses, dan menganalisis efek dari kegagalan tersebut, guna memprioritaskan tindakan pencegahan atau mitigasi." },
{ "en": "Apa yang dimaksud dengan 'redundancy' dalam desain sistem elektronik untuk meningkatkan keandalan?", "id": "Penyertaan komponen, subsistem, atau jalur fungsional duplikat atau cadangan, sehingga sistem dapat terus beroperasi meskipun salah satu bagian mengalami kegagalan." },
{ "en": "Apa itu 'fault tolerance' (toleransi kesalahan)?", "id": "Kemampuan sistem untuk terus beroperasi dengan benar, mungkin pada tingkat kinerja yang lebih rendah, meskipun terjadi satu atau lebih kesalahan atau kegagalan pada komponennya." },
{ "en": "Apa yang dimaksud dengan 'electromagnetic pulse' (EMP)?", "id": "Ledakan radiasi elektromagnetik yang sangat singkat dan intens, dapat dihasilkan secara alami (misalnya oleh petir atau badai matahari) atau buatan (misalnya oleh ledakan nuklir), yang berpotensi merusak peralatan elektronik." },
{ "en": "Bagaimana cara melindungi peralatan elektronik dari efek EMP?", "id": "Menggunakan perisai Faraday (Faraday cage), filter EMP pada jalur daya dan sinyal, komponen yang tahan radiasi (rad-hardened components), dan grounding yang baik." },
{ "en": "Apa itu 'signal-to-noise ratio' (SNR atau S/N)?", "id": "Ukuran perbandingan antara level daya sinyal yang diinginkan dengan level daya noise latar belakang, biasanya dinyatakan dalam desibel (dB). SNR yang lebih tinggi menunjukkan kualitas sinyal yang lebih baik." },
{ "en": "Apa yang dimaksud dengan 'noise figure' (NF) pada penguat atau sistem penerima?", "id": "Ukuran degradasi SNR yang disebabkan oleh komponen atau sistem. Noise figure yang lebih rendah menunjukkan kinerja yang lebih baik (noise yang ditambahkan lebih sedikit)." },
{ "en": "Sebutkan beberapa sumber noise umum dalam rangkaian elektronik.", "id": "Noise termal (Johnson-Nyquist noise), shot noise, flicker noise (1/f noise), noise interferensi (EMI/RFI)." },
{ "en": "Apa itu 'thermal noise' (Johnson-Nyquist noise)?", "id": "Noise elektronik yang dihasilkan oleh agitasi termal dari pembawa muatan (biasanya elektron) di dalam konduktor listrik pada kesetimbangan, terlepas dari tegangan yang diterapkan. Besarnya sebanding dengan suhu absolut dan bandwidth." },
{ "en": "Apa itu 'shot noise'?", "id": "Noise elektronik yang timbul karena sifat diskrit dari pembawa muatan listrik (elektron atau hole) saat melintasi potensial barrier, seperti pada dioda atau transistor. Terjadi pada arus DC." },
{ "en": "Apa itu 'flicker noise' (1/f noise atau pink noise)?", "id": "Jenis noise elektronik dengan spektrum frekuensi di mana kerapatan spektral dayanya berbanding terbalik dengan frekuensi (1/f). Lebih dominan pada frekuensi rendah." },
{ "en": "Apa yang dimaksud dengan 'grounding' dan 'shielding' untuk mengurangi noise?", "id": "Grounding adalah praktik menghubungkan berbagai bagian rangkaian atau sistem ke titik referensi potensial ground yang sama untuk menstabilkan tegangan dan menyediakan jalur balik arus. Shielding adalah penggunaan penghalang konduktif untuk memblokir atau mengurangi penetrasi medan elektromagnetik yang tidak diinginkan." },
{ "en": "Apa perbedaan antara 'analog ground' dan 'digital ground' dan mengapa terkadang dipisahkan?", "id": "Analog ground adalah referensi ground untuk sinyal analog yang sensitif. Digital ground adalah referensi untuk sinyal digital yang dapat menghasilkan noise switching. Dipisahkan untuk mencegah noise dari sirkuit digital mengganggu sirkuit analog yang sensitif, kemudian biasanya dihubungkan di satu titik (star ground)." },
{ "en": "Apa itu 'twisted pair cable' dan mengapa digunakan?", "id": "Jenis kabel di mana dua konduktor dari satu sirkuit dipilin bersama untuk mengurangi interferensi elektromagnetik (EMI) dari sumber eksternal dan crosstalk antar pasangan yang berdekatan." },
{ "en": "Apa itu 'coaxial cable' (kabel koaksial)?", "id": "Jenis kabel listrik yang memiliki konduktor dalam (inner conductor) yang dikelilingi oleh lapisan insulasi tubular, yang kemudian dikelilingi oleh perisai konduktif tubular (shield). Digunakan untuk mentransmisikan sinyal frekuensi tinggi dengan rugi-rugi rendah dan perlindungan EMI yang baik." },
{ "en": "Apa itu 'fiber optic cable' (kabel serat optik)?", "id": "Kabel yang terdiri dari satu atau lebih serat optik (helai kaca atau plastik tipis) yang digunakan untuk mentransmisikan informasi sebagai pulsa cahaya. Menawarkan bandwidth sangat tinggi, rugi-rugi rendah, dan kekebalan terhadap EMI." },
{ "en": "Apa perbedaan utama antara mode transmisi 'single-mode' dan 'multi-mode' pada serat optik?", "id": "Serat single-mode memiliki inti yang sangat kecil dan hanya memungkinkan satu mode cahaya merambat, cocok untuk jarak jauh dan bandwidth tinggi. Serat multi-mode memiliki inti lebih besar, memungkinkan beberapa mode cahaya merambat, lebih mudah dihubungkan tetapi memiliki bandwidth lebih rendah dan jangkauan lebih pendek karena dispersi modal." },
{ "en": "Apa itu 'attenuation' (atenuasi) dalam transmisi sinyal?", "id": "Pengurangan kekuatan atau amplitudo sinyal saat merambat melalui media transmisi (kabel, udara, serat optik) akibat penyerapan, hamburan, atau rugi-rugi lainnya." },
{ "en": "Apa itu 'dispersion' (dispersi) dalam transmisi sinyal optik atau RF?", "id": "Efek pelebaran pulsa sinyal saat merambat melalui media, disebabkan oleh komponen frekuensi yang berbeda dari sinyal merambat dengan kecepatan yang sedikit berbeda (dispersi kromatik) atau mode yang berbeda menempuh jalur berbeda (dispersi modal)." },
{ "en": "Apa fungsi dari 'repeater' dalam sistem komunikasi?", "id": "Perangkat elektronik yang menerima sinyal dan mentransmisikannya kembali dengan level daya yang lebih tinggi atau ke sisi lain dari suatu halangan, sehingga sinyal dapat mencakup jarak yang lebih jauh tanpa degradasi berlebihan." },
{ "en": "Apa itu 'amplifier' (penguat)?", "id": "Perangkat elektronik yang meningkatkan daya, tegangan, atau arus dari suatu sinyal." },
{ "en": "Apa itu 'oscillator' (osilator)?", "id": "Rangkaian elektronik yang menghasilkan sinyal periodik berulang (biasanya gelombang sinus atau gelombang kotak) tanpa memerlukan sinyal input eksternal, menggunakan umpan balik positif." },
{ "en": "Apa perbedaan antara 'linear power supply' dan 'switched-mode power supply' (SMPS)?", "id": "Linear power supply meregulasi tegangan output dengan cara membuang kelebihan daya sebagai panas pada elemen seri (transistor), efisiensinya rendah tetapi output noise rendah. SMPS menggunakan komponen switching frekuensi tinggi untuk mentransfer daya secara efisien, ukurannya lebih kecil dan efisiensi tinggi, tetapi dapat menghasilkan lebih banyak noise." },
{ "en": "Apa itu 'class A amplifier'?", "id": "Penguat di mana transistor output selalu menghantarkan arus (tidak pernah cut-off) selama seluruh siklus sinyal input. Menghasilkan linearitas tinggi tetapi efisiensi daya rendah." },
{ "en": "Apa itu 'class B amplifier'?", "id": "Penguat di mana setiap transistor output hanya menghantarkan arus selama setengah siklus (180 derajat) dari sinyal input. Biasanya menggunakan konfigurasi push-pull. Efisiensi lebih tinggi dari Kelas A, tetapi dapat mengalami distorsi crossover." },
{ "en": "Apa itu 'class AB amplifier'?", "id": "Kompromi antara Kelas A dan Kelas B. Setiap transistor output menghantarkan arus sedikit lebih dari setengah siklus, untuk mengurangi distorsi crossover sambil mempertahankan efisiensi yang lebih baik dari Kelas A." },
{ "en": "Apa itu 'class C amplifier'?", "id": "Penguat di mana transistor output menghantarkan arus kurang dari setengah siklus sinyal input. Efisiensi sangat tinggi, tetapi distorsi tinggi, sehingga biasanya digunakan dalam aplikasi RF frekuensi tetap (tuned amplifier)." },
{ "en": "Apa yang dimaksud dengan 'negative feedback' (umpan balik negatif) dalam penguat?", "id": "Proses mengumpankan sebagian sinyal output kembali ke input dengan fasa terbalik (berlawanan). Manfaatnya antara lain menstabilkan gain, mengurangi distorsi, memperlebar bandwidth, dan memodifikasi impedansi input/output." },
{ "en": "Apa yang dimaksud dengan 'positive feedback' (umpan balik positif) dan di mana biasanya digunakan?", "id": "Proses mengumpankan sebagian sinyal output kembali ke input dengan fasa yang sama. Digunakan dalam osilator untuk menghasilkan ayunan (osilasi) atau dalam rangkaian bistabil seperti Schmitt trigger untuk menciptakan hysteresis." },
{ "en": "Apa 'Barkhausen criterion' untuk osilasi?", "id": "Dua syarat yang harus dipenuhi agar rangkaian dapat berosilasi: 1. Penguatan loop total (magnitudo dari Aβ) harus sama dengan satu. 2. Pergeseran fasa total dalam loop umpan balik harus 0 derajat atau kelipatan integer dari 360 derajat." },
{ "en": "Sebutkan salah satu jenis osilator RC (menggunakan resistor dan kapasitor untuk elemen penentu frekuensi).", "id": "Osilator Wien bridge, osilator phase-shift, osilator twin-T." },
{ "en": "Sebutkan salah satu jenis osilator LC (menggunakan induktor dan kapasitor untuk elemen penentu frekuensi).", "id": "Osilator Colpitts, osilator Hartley, osilator Clapp, osilator Armstrong." },
{ "en": "Apa fungsi utama dari 'voltage-controlled oscillator' (VCO)?", "id": "Osilator elektronik yang frekuensi osilasinya dikendalikan oleh tegangan input DC. Digunakan dalam PLL, sintesis frekuensi, dan modulasi FM." },
{ "en": "Apa itu 'Boolean algebra' (aljabar Boolean)?", "id": "Cabang aljabar yang berurusan dengan variabel yang hanya memiliki dua nilai (biasanya benar/salah, atau 1/0) dan operasi logika (AND, OR, NOT). Dasar dari desain logika digital." },
{ "en": "Sebutkan tiga operasi dasar dalam aljabar Boolean.", "id": "AND (konjungsi), OR (disjungsi), NOT (negasi/inversi)." },
{ "en": "Apa itu 'truth table' (tabel kebenaran) dalam logika digital?", "id": "Tabel yang menunjukkan semua kemungkinan kombinasi input dari suatu fungsi logika dan output yang sesuai untuk setiap kombinasi tersebut." },
{ "en": "Apa itu 'logic gate' (gerbang logika)?", "id": "Blok bangunan dasar dari sirkuit digital, yang mengimplementasikan fungsi Boolean dasar. Contoh: gerbang AND, OR, NOT, NAND, NOR, XOR, XNOR." },
{ "en": "Gerbang logika apa yang disebut 'universal gate' dan mengapa?", "id": "Gerbang NAND dan gerbang NOR disebut universal gate karena kombinasi dari salah satu jenis gerbang ini saja dapat digunakan untuk mengimplementasikan semua fungsi logika Boolean lainnya (AND, OR, NOT)." },
{ "en": "Apa fungsi dari gerbang XOR (Exclusive OR)?", "id": "Output gerbang XOR akan bernilai HIGH (1) jika jumlah input yang bernilai HIGH adalah ganjil. Atau, outputnya HIGH jika input-inputnya berbeda." },
{ "en": "Apa fungsi dari gerbang XNOR (Exclusive NOR)?", "id": "Output gerbang XNOR akan bernilai HIGH (1) jika jumlah input yang bernilai HIGH adalah genap (termasuk nol). Atau, outputnya HIGH jika input-inputnya sama. Merupakan inversi dari XOR." },
{ "en": "Apa itu 'Karnaugh map' (peta Karnaugh) atau K-map?", "id": "Metode grafis yang digunakan untuk menyederhanakan ekspresi aljabar Boolean atau fungsi logika, dengan cara mengelompokkan minterm atau maxterm yang berdekatan." },
{ "en": "Apa itu 'flip-flop' dalam elektronika digital?", "id": "Rangkaian sekuensial bistabil yang dapat menyimpan satu bit informasi (0 atau 1). Memiliki dua state stabil dan dapat diubah dari satu state ke state lain melalui sinyal input (dan sinyal clock pada flip-flop sinkron)." },
{ "en": "Sebutkan beberapa jenis flip-flop yang umum.", "id": "SR flip-flop (Set-Reset), D flip-flop (Data/Delay), JK flip-flop, T flip-flop (Toggle)." },
{ "en": "Apa perbedaan antara 'latch' dan 'flip-flop'?", "id": "Latch adalah level-sensitive (outputnya dapat berubah selama input enable aktif dan input data berubah). Flip-flop adalah edge-triggered (outputnya hanya berubah pada transisi naik atau turun dari sinyal clock)." },
{ "en": "Apa itu 'register' dalam sistem digital?", "id": "Sekelompok flip-flop yang digunakan untuk menyimpan sekumpulan bit data secara bersamaan (misalnya, satu kata data)." },
{ "en": "Apa itu 'counter' (pencacah) dalam sistem digital?", "id": "Rangkaian sekuensial yang melalui urutan state tertentu sebagai respons terhadap pulsa input (biasanya pulsa clock), digunakan untuk menghitung event atau menghasilkan urutan timing." },
{ "en": "Apa perbedaan antara 'synchronous counter' dan 'asynchronous counter' (ripple counter)?", "id": "Pada synchronous counter, semua flip-flop dipicu oleh sinyal clock yang sama secara bersamaan. Pada asynchronous counter, output satu flip-flop memicu input clock flip-flop berikutnya, menyebabkan efek 'ripple' dan penundaan propagasi." },
{ "en": "Apa itu 'state diagram' (diagram keadaan) untuk rangkaian sekuensial?", "id": "Representasi grafis dari perilaku rangkaian sekuensial, menunjukkan state (kondisi) yang mungkin, transisi antar state berdasarkan input, dan output yang terkait dengan state atau transisi." },
{ "en": "Apa itu 'memory address' (alamat memori)?", "id": "Identifier unik yang digunakan untuk mengakses lokasi penyimpanan tertentu dalam memori komputer atau sistem digital." },
{ "en": "Apa yang dimaksud dengan 'address decoding' (pengkodean alamat)?", "id": "Proses menerjemahkan alamat logis yang dihasilkan oleh CPU menjadi sinyal fisik yang memilih chip memori atau lokasi memori tertentu." },
{ "en": "Apa itu 'read cycle' dan 'write cycle' pada operasi memori?", "id": "Read cycle adalah urutan langkah yang dilakukan untuk membaca data dari lokasi memori. Write cycle adalah urutan langkah untuk menulis data ke lokasi memori." },
{ "en": "Apa itu 'volatile memory' dan 'non-volatile memory'?", "id": "Volatile memory (misalnya RAM) kehilangan datanya saat daya dimatikan. Non-volatile memory (misalnya ROM, Flash, hard disk) mempertahankan datanya meskipun daya dimatikan." },
{ "en": "Apa itu 'programmable logic device' (PLD)?", "id": "IC yang berisi blok logika dan interkoneksi yang dapat dikonfigurasi atau diprogram oleh pengguna untuk mengimplementasikan fungsi digital kustom. Contoh: PAL, GAL, CPLD, FPGA." },
{ "en": "Apa itu 'Complex Programmable Logic Device' (CPLD)?", "id": "Jenis PLD yang lebih kompleks dari PAL/GAL, terdiri dari beberapa blok logika (mirip PAL) yang dihubungkan oleh matriks interkoneksi yang dapat diprogram. Arsitekturnya non-volatil (biasanya)." },
{ "en": "Apa itu 'microcontroller unit' (MCU)?", "id": "Sebuah komputer kecil dalam satu IC yang berisi inti prosesor (CPU), memori (RAM, Flash/ROM), dan periferal input/output (GPIO, timer, ADC, komunikasi serial) yang dapat diprogram." },
{ "en": "Apa itu 'Digital Signal Processor' (DSP)?", "id": "Mikroprosesor khusus yang dirancang dan dioptimalkan untuk melakukan operasi pemrosesan sinyal digital secara cepat dan efisien, seperti filtering, transformasi Fourier (FFT)." },
{ "en": "Apa aplikasi umum dari DSP?", "id": "Pemrosesan audio (kompresi, efek), pemrosesan gambar/video, telekomunikasi (modem, ponsel), radar, sonar, kontrol motor." },
{ "en": "Apa itu 'Application-Specific Instruction-set Processor' (ASIP)?", "id": "Prosesor yang set instruksinya disesuaikan atau dioptimalkan untuk domain aplikasi tertentu, merupakan kompromi antara prosesor general-purpose dan perangkat keras khusus (ASIC)." },
{ "en": "Apa yang dimaksud dengan 'Hardware Description Language' (HDL)?", "id": "Bahasa pemrograman khusus yang digunakan untuk mendeskripsikan struktur dan perilaku sirkuit elektronik digital. Contoh: Verilog, VHDL." },
{ "en": "Apa itu 'synthesis' (sintesis) dalam konteks HDL?", "id": "Proses otomatis menerjemahkan deskripsi HDL dari suatu desain digital menjadi implementasi level gerbang logika (netlist) yang dapat difabrikasi atau dipetakan ke PLD/FPGA." },
{ "en": "Apa itu 'simulation' (simulasi) dalam konteks HDL?", "id": "Proses memverifikasi fungsionalitas dan timing dari desain digital yang dideskripsikan dalam HDL dengan cara menjalankan model desain tersebut pada komputer menggunakan input test vector dan mengamati outputnya." },
{ "en": "Apa itu 'testbench' dalam simulasi HDL?", "id": "Kode HDL terpisah yang digunakan untuk menstimulasi input ke desain yang sedang diuji (Design Under Test - DUT) dan memverifikasi outputnya terhadap perilaku yang diharapkan." },
{ "en": "Apa yang dimaksud dengan 'power integrity' (integritas daya) dalam desain IC atau PCB?", "id": "Kualitas sistem distribusi daya (Power Distribution Network - PDN) dalam menyediakan tegangan suplai yang stabil dan bersih ke semua komponen aktif, bebas dari noise, droop, dan ground bounce yang berlebihan." },
{ "en": "Apa itu 'electromigration' pada jalur logam di IC dan mengapa menjadi masalah keandalan?", "id": "Perpindahan bertahap atom logam dalam konduktor akibat momentum yang ditransfer dari elektron yang bergerak (arus listrik). Dapat menyebabkan jalur menjadi tipis (void) atau menumpuk (hillock), yang akhirnya menyebabkan kegagalan sirkuit. Masalah pada kerapatan arus tinggi." },
{ "en": "Apa itu 'latch-up' pada rangkaian CMOS?", "id": "Kondisi arus tinggi yang tidak diinginkan (seperti hubungan singkat antara VDD dan GND) yang disebabkan oleh pemicuan struktur thyristor parasitik (PNPN atau NPNP) dalam substrat CMOS. Dapat merusak IC jika tidak dibatasi arusnya." },
{ "en": "Bagaimana cara mencegah latch-up pada desain CMOS?", "id": "Menggunakan guard ring, tata letak yang cermat (pemisahan yang cukup antara sumur N dan P), penggunaan substrat dengan doping yang tepat, dan memastikan suplai daya yang bersih." },
{ "en": "Apa itu 'FinFET' (Fin Field-Effect Transistor)?", "id": "Jenis transistor MOSFET multi-gate non-planar di mana gate membungkus kanal berbentuk sirip (fin) pada tiga sisinya, memungkinkan kontrol gate yang lebih baik atas kanal dan mengurangi efek kanal pendek serta arus bocor, digunakan pada teknologi node canggih." },
{ "en": "Apa itu 'Gate-All-Around' (GAA) FET?", "id": "Pengembangan lebih lanjut dari FinFET di mana gate sepenuhnya mengelilingi kanal (misalnya berbentuk nanowire atau nanosheet), menawarkan kontrol elektrostatik yang lebih baik lagi dan potensi untuk scaling lebih lanjut." },
{ "en": "Apa yang dimaksud dengan 'wafer' dalam fabrikasi semikonduktor?", "id": "Cakram tipis (slice) dari material semikonduktor (biasanya silikon kristal tunggal) yang digunakan sebagai substrat untuk fabrikasi sirkuit terpadu (IC)." },
{ "en": "Apa itu 'die' (plural: dice) dalam konteks IC?", "id": "Sebuah unit fungsional IC tunggal yang dipotong dari wafer setelah proses fabrikasi selesai." },
{ "en": "Apa itu 'packaging' (pengemasan) IC?", "id": "Proses menutup die IC dalam wadah pelindung (package) yang menyediakan koneksi listrik ke dunia luar (melalui pin atau bola solder), disipasi panas, dan perlindungan mekanis serta lingkungan." },
{ "en": "Sebutkan beberapa jenis paket IC yang umum.", "id": "DIP (Dual In-line Package), SOP/SOIC (Small Outline Package/IC), QFP (Quad Flat Package), BGA (Ball Grid Array), LGA (Land Grid Array)." },
{ "en": "Apa itu 'wire bonding' dalam pengemasan IC?", "id": "Proses membuat interkoneksi listrik antara pad pada die IC dan lead frame atau substrat paket menggunakan kawat halus (biasanya emas atau aluminium)." },
{ "en": "Apa itu 'flip-chip' bonding?", "id": "Metode interkoneksi di mana die IC dipasang menghadap ke bawah (flipped) langsung ke substrat paket atau PCB, dengan tonjolan solder (solder bumps) pada pad die yang menyambung ke pad yang sesuai pada substrat." },
// AWAL DARI BATCH SOAL BARU (Lanjutan untuk 351 tambahan - FOKUS MATERI MENDALAM)
// Target: 200-250 soal baru di batch ini
{ "en": "Jelaskan konsep 'channel length modulation' pada MOSFET dan bagaimana pengaruhnya terhadap karakteristik output.", "id": "Efek di mana panjang efektif kanal konduksi pada MOSFET berkurang seiring meningkatnya tegangan drain-source (VDS) di daerah saturasi. Ini menyebabkan peningkatan arus drain (ID) meskipun VGS konstan, menghasilkan resistansi output (rds) yang finit, bukan tak hingga seperti pada model ideal." },
{ "en": "Apa yang dimaksud dengan 'body effect' atau 'substrate bias effect' pada MOSFET?", "id": "Perubahan tegangan threshold (VT) MOSFET akibat adanya beda potensial antara source dan body (substrat). Jika VSB (tegangan source-body) meningkat, VT juga cenderung meningkat, mempengaruhi arus drain." },
{ "en": "Jelaskan mekanisme 'velocity saturation' pada MOSFET kanal pendek dan dampaknya pada arus drain.", "id": "Pada medan listrik tinggi di kanal pendek, kecepatan drift elektron (atau hole) mencapai batas maksimum (kecepatan saturasi). Ini menyebabkan arus drain tidak lagi meningkat secara kuadratik dengan VGS - VT, melainkan lebih linier, dan membatasi transkonduktansi." },
{ "en": "Apa itu 'hot carrier injection' (HCI) pada MOSFET dan bagaimana hal ini dapat menyebabkan degradasi perangkat?", "id": "Fenomena di mana elektron atau hole mendapatkan energi kinetik tinggi (menjadi 'panas') di daerah medan listrik kuat dekat drain, kemudian dapat diinjeksikan ke dalam dielektrik gate atau antarmuka Si-SiO2. Ini dapat menciptakan trap state, mengubah VT, dan mendegradasi kinerja perangkat seiring waktu." },
{ "en": "Jelaskan perbedaan antara 'Early voltage' pada BJT dan parameter yang setara pada MOSFET.", "id": "Early voltage (VA) pada BJT mengkuantifikasi kemiringan kurva karakteristik output (IC vs VCE) di daerah aktif akibat modulasi lebar basis. Parameter setara pada MOSFET adalah $1/\\lambda$ (di mana $\lambda$ adalah parameter channel length modulation), yang juga berkaitan dengan resistansi output finit ($r_o = V_A/I_C$ untuk BJT, dan $r_{ds} \approx 1/(\lambda I_D)$ untuk MOSFET)." },
{ "en": "Apa yang dimaksud dengan 'Gummel plot' untuk BJT dan informasi apa yang dapat diperoleh darinya?", "id": "Plot logaritmik dari arus kolektor (IC) dan arus basis (IB) sebagai fungsi dari tegangan base-emitter (VBE) pada VBC = 0. Digunakan untuk mengevaluasi kualitas persambungan, gain arus (beta), dan mengidentifikasi daerah operasi serta mekanisme arus non-ideal (misalnya arus rekombinasi)." },
{ "en": "Jelaskan konsep 'Kirk effect' atau 'base pushout' pada BJT daya tinggi.", "id": "Pada kerapatan arus kolektor tinggi, pembawa muatan mayoritas yang diinjeksikan ke daerah basis efektif memperlebar daerah basis secara elektrik ke dalam daerah kolektor yang terdoping ringan. Ini meningkatkan waktu transit basis, mengurangi frekuensi cut-off (fT), dan dapat meningkatkan resistansi saturasi." },
{ "en": "Apa yang dimaksud dengan 'transit frequency' ($f_T$) pada BJT atau MOSFET dan faktor apa saja yang mempengaruhinya?", "id": "$f_T$ adalah frekuensi di mana penguatan arus common-emitter (untuk BJT) atau common-source (untuk MOSFET) turun menjadi satu (0 dB). Dipengaruhi oleh waktu transit pembawa muatan melalui daerah aktif (basis/kanal) dan kapasitansi parasitik perangkat." },
{ "en": "Jelaskan perbedaan antara 'diffusion capacitance' dan 'depletion (junction) capacitance' pada dioda atau persambungan PN.", "id": "Depletion capacitance timbul akibat perubahan lebar daerah deplesi dengan tegangan bias balik. Diffusion capacitance timbul pada bias maju akibat injeksi pembawa muatan minoritas dan penyimpanan muatan di dekat persambungan, dominan pada frekuensi tinggi saat bias maju." },
{ "en": "Apa itu 'Ebers-Moll model' untuk BJT?", "id": "Model rangkaian ekuivalen skala besar untuk BJT yang merepresentasikan transistor sebagai dua dioda back-to-back dengan sumber arus terkontrol, menggambarkan perilaku DC transistor dalam semua mode operasi (aktif, saturasi, cut-off, aktif terbalik)." },
{ "en": "Jelaskan konsep 'bandgap narrowing' pada semikonduktor dengan doping tinggi.", "id": "Efek di mana celah pita (bandgap energy) efektif dari semikonduktor berkurang akibat interaksi antara pembawa muatan dan atom dopan pada konsentrasi doping yang sangat tinggi. Ini dapat mempengaruhi sifat listrik perangkat." },
{ "en": "Apa yang dimaksud dengan 'Auger recombination' pada semikonduktor?", "id": "Proses rekombinasi non-radiatif di mana energi yang dilepaskan oleh rekombinasi pasangan elektron-hole ditransfer ke pembawa muatan ketiga (elektron atau hole) sebagai energi kinetik. Menjadi signifikan pada konsentrasi pembawa muatan tinggi." },
{ "en": "Jelaskan mekanisme 'Zener breakdown' dan 'avalanche breakdown' pada dioda.", "id": "Zener breakdown terjadi pada dioda dengan doping sangat tinggi akibat tunneling kuantum elektron dari pita valensi ke pita konduksi di bawah medan listrik internal yang kuat. Avalanche breakdown terjadi pada dioda dengan doping lebih rendah, di mana pembawa muatan dipercepat oleh medan listrik dan mendapatkan energi cukup untuk menghasilkan pasangan elektron-hole baru melalui tumbukan (impact ionization), menyebabkan efek longsoran." },
{ "en": "Apa yang dimaksud dengan 'SPICE' (Simulation Program with Integrated Circuit Emphasis)?", "id": "Program simulator rangkaian elektronik analog general-purpose yang digunakan untuk memverifikasi desain rangkaian dan memprediksi perilakunya sebelum fabrikasi, menggunakan model matematis untuk komponen." },
{ "en": "Sebutkan beberapa level abstraksi model komponen dalam SPICE (misalnya, Level 1, BSIM).", "id": "Level 1 (Shichman-Hodges), Level 2 (Grove-Frohman), Level 3 (model empiris), BSIM (Berkeley Short-channel IGFET Model) seperti BSIM3, BSIM4, BSIM-CMG (untuk FinFET)." },
{ "en": "Apa itu 'corner analysis' dalam simulasi rangkaian IC?", "id": "Analisis simulasi yang dilakukan pada kombinasi ekstrem dari variasi proses fabrikasi (Process corners: Fast, Slow, Typical), variasi tegangan suplai (Voltage corners: High, Low), dan variasi suhu (Temperature corners: Hot, Cold) untuk memastikan desain berfungsi di bawah kondisi terburuk." },
{ "en": "Apa itu 'Monte Carlo analysis' dalam simulasi rangkaian?", "id": "Metode simulasi statistik di mana parameter komponen divariasikan secara acak berdasarkan distribusi statistiknya untuk mengevaluasi dampak variasi manufaktur terhadap kinerja rangkaian dan yield." },
{ "en": "Jelaskan konsep 'noise shaping' pada konverter data (ADC/DAC) sigma-delta.", "id": "Teknik yang digunakan dalam modulator sigma-delta untuk memindahkan noise kuantisasi dari pita frekuensi sinyal yang diinginkan (in-band) ke frekuensi yang lebih tinggi (out-of-band), sehingga dapat difilter, menghasilkan resolusi efektif yang lebih tinggi di in-band." },
{ "en": "Apa yang dimaksud dengan 'oversampling' pada ADC atau DAC?", "id": "Teknik mengambil sampel sinyal pada frekuensi yang jauh lebih tinggi dari frekuensi Nyquist. Digunakan bersama dengan noise shaping dan filtering digital untuk meningkatkan SNR dan resolusi efektif konverter data." },
{ "en": "Apa itu 'Dynamic Range' dan 'Spurious-Free Dynamic Range' (SFDR) pada ADC atau DAC?", "id": "Dynamic Range adalah rasio antara sinyal maksimum yang dapat diolah dan sinyal minimum yang dapat dideteksi (biasanya noise floor). SFDR adalah rasio antara daya sinyal fundamental dan daya komponen spurious (harmonik atau non-harmonik) terbesar dalam spektrum output." },
{ "en": "Jelaskan arsitektur 'Flash ADC'. Apa kelebihan dan kekurangannya?", "id": "Flash ADC menggunakan sejumlah besar komparator paralel ($2^N-1$ untuk N-bit) untuk membandingkan tegangan input dengan tegangan referensi secara bersamaan. Kelebihan: kecepatan konversi sangat tinggi. Kekurangan: kompleksitas tinggi, konsumsi daya besar, dan area chip besar seiring meningkatnya resolusi." },
{ "en": "Jelaskan arsitektur 'Successive Approximation Register' (SAR) ADC.", "id": "SAR ADC menggunakan algoritma pencarian biner untuk mengkonversi tegangan input. Terdiri dari komparator, DAC internal, dan register SAR. Menawarkan keseimbangan baik antara kecepatan, resolusi, dan konsumsi daya." },
{ "en": "Apa itu 'Pipeline ADC' dan bagaimana cara kerjanya?", "id": "Pipeline ADC membagi proses konversi menjadi beberapa tahap (stage) yang lebih sederhana, di mana setiap tahap mengkonversi sebagian bit dan meneruskan sisa residu ke tahap berikutnya. Memungkinkan resolusi tinggi pada kecepatan tinggi." },
{ "en": "Jelaskan arsitektur 'Sigma-Delta (ΔΣ) ADC'.", "id": "Menggunakan modulator sigma-delta (dengan oversampling dan noise shaping) untuk menghasilkan aliran bit digital kepadatan tinggi yang kemudian difilter secara digital untuk menghasilkan output resolusi tinggi. Sangat baik untuk aplikasi audio dan instrumentasi presisi." },
{ "en": "Apa yang dimaksud dengan 'Differential Non-Linearity' (DNL) dan 'Integral Non-Linearity' (INL) pada ADC/DAC?", "id": "DNL adalah deviasi lebar setiap kode output (untuk ADC) atau step tegangan output (untuk DAC) dari nilai idealnya (1 LSB). INL adalah deviasi kumulatif dari karakteristik transfer aktual terhadap garis lurus ideal." },
{ "en": "Apa itu 'settling time' pada DAC?", "id": "Waktu yang dibutuhkan output DAC untuk mencapai dan stabil dalam batas toleransi tertentu dari nilai akhirnya setelah input digital berubah." },
{ "en": "Apa itu 'glitch energy' pada DAC?", "id": "Pulsa transien yang tidak diinginkan (glitch) yang dapat muncul pada output DAC saat input digital berubah, terutama pada transisi kode mayor. Glitch energy mengkuantifikasi area dari pulsa glitch tersebut." },
{ "en": "Jelaskan konsep 'mismatch' pada komponen analog di IC dan bagaimana pengaruhnya.", "id": "Variasi acak yang tak terhindarkan dalam karakteristik komponen yang seharusnya identik (misalnya transistor, resistor, kapasitor) akibat ketidaksempurnaan proses fabrikasi. Menyebabkan offset, error gain, dan distorsi pada rangkaian analog presisi." },
{ "en": "Apa itu 'common-centroid layout' dan mengapa digunakan dalam desain IC analog?", "id": "Teknik tata letak di mana komponen-komponen kritis (misalnya pasangan diferensial) disusun secara simetris di sekitar titik pusat bersama. Digunakan untuk meminimalkan efek gradien proses (misalnya variasi ketebalan oksida atau doping) dan mengurangi mismatch." },
{ "en": "Apa yang dimaksud dengan 'Gilbert cell mixer'?", "id": "Jenis mixer analog empat kuadran yang banyak digunakan dalam aplikasi RF, berdasarkan prinsip transkonduktansi variabel dari pasangan diferensial BJT atau MOSFET. Dapat melakukan perkalian sinyal analog." },
{ "en": "Jelaskan prinsip kerja 'Phase-Locked Loop' (PLL) dan sebutkan komponen utamanya.", "id": "PLL adalah sistem kontrol umpan balik yang menghasilkan sinyal output yang fasanya terkunci (sinkron) dengan fasa sinyal input referensi. Komponen utama: Phase Detector (PD), Loop Filter (LF), Voltage-Controlled Oscillator (VCO), dan kadang-kadang Frequency Divider." },
{ "en": "Apa itu 'lock range' dan 'capture range' pada PLL?", "id": "Capture range adalah rentang frekuensi input di mana PLL dapat mengakuisisi lock (terkunci) dari kondisi unlocked. Lock range (tracking range) adalah rentang frekuensi input di mana PLL dapat mempertahankan lock setelah lock tercapai, biasanya lebih lebar dari capture range." },
{ "en": "Apa fungsi dari 'loop filter' pada PLL?", "id": "Filter low-pass yang menyaring komponen frekuensi tinggi dari output phase detector, menstabilkan loop, dan menentukan dinamika loop (bandwidth, damping factor)." },
{ "en": "Jelaskan konsep 'frequency synthesis' menggunakan PLL.", "id": "PLL dapat digunakan untuk menghasilkan berbagai frekuensi output yang stabil dan akurat dari satu frekuensi referensi stabil, dengan cara memasukkan pembagi frekuensi (frequency divider) dalam loop umpan balik antara output VCO dan phase detector." },
{ "en": "Apa itu 'jitter' pada sinyal clock atau data?", "id": "Deviasi sesaat dari posisi tepi (edge) sinyal clock atau data dari posisi idealnya dalam domain waktu. Dapat menyebabkan error timing pada sistem digital kecepatan tinggi." },
{ "en": "Sebutkan beberapa jenis jitter (misalnya, random jitter, deterministic jitter).", "id": "Random Jitter (RJ) bersifat acak dan tidak terbatas, biasanya disebabkan oleh noise termal. Deterministic Jitter (DJ) bersifat terbatas dan dapat diprediksi, disebabkan oleh crosstalk, interferensi, atau duty-cycle distortion. Periodik Jitter (PJ) adalah subkomponen dari DJ." },
{ "en": "Apa itu 'Voltage Standing Wave Ratio' (VSWR) atau SWR pada jalur transmisi?", "id": "Ukuran ketidakcocokan impedansi antara jalur transmisi dan bebannya (atau sumbernya). VSWR = 1:1 menunjukkan kecocokan sempurna (tidak ada pantulan). Nilai lebih tinggi menunjukkan pantulan yang lebih besar." },
{ "en": "Apa itu 'Smith Chart' dan apa kegunaannya dalam teknik RF?", "id": "Diagram grafis polar yang digunakan untuk merepresentasikan impedansi kompleks, koefisien refleksi, dan parameter jalur transmisi lainnya. Berguna untuk desain matching network, analisis stabilitas amplifier, dll." },
{ "en": "Jelaskan konsep 'scattering parameters' (S-parameters) untuk karakterisasi jaringan RF dua-port.", "id": "S-parameters menggambarkan bagaimana daya gelombang RF dipantulkan atau ditransmisikan melalui jaringan. Misalnya, S11 adalah koefisien refleksi input, S21 adalah gain transmisi maju, S12 adalah isolasi balik, S22 adalah koefisien refleksi output." },
{ "en": "Apa yang dimaksud dengan 'impedance matching' dalam rangkaian RF dan mengapa penting?", "id": "Proses menyesuaikan impedansi sumber, jalur transmisi, dan beban agar sama (atau konjugat kompleks untuk transfer daya maksimum). Penting untuk memaksimalkan transfer daya, meminimalkan pantulan sinyal (VSWR rendah), dan meningkatkan SNR." },
{ "en": "Sebutkan beberapa komponen yang digunakan untuk impedance matching pada frekuensi RF.", "id": "Induktor dan kapasitor (rangkaian L, Pi, T), stub (potongan jalur transmisi), transformator RF, balun." },
{ "en": "Apa itu 'Low Noise Amplifier' (LNA) dan di mana posisinya dalam rantai penerima RF?", "id": "Penguat pertama dalam rantai penerima RF, dirancang untuk menguatkan sinyal lemah yang diterima dari antena sambil menambahkan noise seminimal mungkin (memiliki noise figure rendah). Posisinya sangat penting karena noise figure LNA mendominasi noise figure keseluruhan sistem." },
{ "en": "Apa itu 'mixer' (frequency mixer) dalam sistem RF dan apa fungsi utamanya?", "id": "Rangkaian non-linear yang menggabungkan dua sinyal input frekuensi berbeda (misalnya sinyal RF dan sinyal Local Oscillator/LO) untuk menghasilkan sinyal output pada frekuensi baru (jumlah dan selisih frekuensi input, disebut Intermediate Frequency/IF)." },
{ "en": "Apa yang dimaksud dengan 'image frequency' dalam penerima superheterodyne dan bagaimana cara menolaknya?", "id": "Frekuensi yang tidak diinginkan yang berjarak $2 \times f_{IF}$ dari frekuensi sinyal RF yang diinginkan (pada sisi lain dari $f_{LO}$), yang juga akan menghasilkan frekuensi IF yang sama setelah mixing. Ditolak menggunakan filter RF preselector (image rejection filter) sebelum mixer." },
{ "en": "Apa itu 'Intermediate Frequency' (IF) dalam penerima superheterodyne?", "id": "Frekuensi tetap yang lebih rendah yang dihasilkan setelah proses mixing sinyal RF dengan sinyal LO. Sebagian besar penguatan dan filtering selektif dilakukan pada tahap IF ini." },
{ "en": "Jelaskan arsitektur penerima 'superheterodyne'.", "id": "Arsitektur penerima radio yang umum, di mana sinyal RF yang diterima pertama kali difilter, kemudian dikuatkan oleh LNA, lalu dicampur (mixed) dengan sinyal dari Local Oscillator (LO) untuk menghasilkan sinyal Intermediate Frequency (IF) yang tetap. Sinyal IF kemudian difilter dengan sangat selektif, dikuatkan lagi, dan didemodulasi." },
{ "en": "Apa itu 'direct conversion receiver' (zero-IF receiver atau homodyne receiver)?", "id": "Arsitektur penerima di mana sinyal RF yang diterima langsung dikonversi ke baseband (IF = 0 Hz) dengan cara mencampurnya dengan sinyal LO yang frekuensinya sama dengan frekuensi RF. Menghilangkan kebutuhan filter IF yang besar, tetapi memiliki tantangan seperti DC offset dan I/Q mismatch." },
{ "en": "Apa itu 'I/Q modulation' (In-phase/Quadrature modulation) dan mengapa digunakan?", "id": "Teknik modulasi di mana dua sinyal baseband (I dan Q, berbeda fasa $90^\circ$) memodulasi dua sinyal pembawa yang juga berbeda fasa $90^\circ$. Digunakan untuk mentransmisikan dua sinyal independen pada frekuensi pembawa yang sama, atau untuk skema modulasi digital kompleks (QPSK, QAM) yang efisien secara spektral." },
{ "en": "Apa itu 'Orthogonal Frequency Division Multiplexing' (OFDM)?", "id": "Skema modulasi digital multi-carrier di mana data ditransmisikan melalui sejumlah besar sub-carrier ortogonal yang berjarak dekat. Tahan terhadap multipath fading dan efisien secara spektral. Digunakan dalam Wi-Fi, LTE, DVB." },
{ "en": "Apa itu 'Error Vector Magnitude' (EVM) dalam sistem komunikasi digital?", "id": "Ukuran kualitas sinyal modulasi digital, yang mengkuantifikasi perbedaan antara simbol yang diterima (titik konstelasi aktual) dan simbol ideal (titik konstelasi referensi). EVM rendah menunjukkan kualitas sinyal baik." },
{ "en": "Apa yang dimaksud dengan 'bit error rate' (BER)?", "id": "Rasio jumlah bit yang diterima dengan error terhadap jumlah total bit yang ditransmisikan dalam sistem komunikasi digital. Ukuran kinerja sistem." },
{ "en": "Jelaskan konsep 'channel coding' (forward error correction - FEC).", "id": "Teknik menambahkan bit redundan ke data sebelum transmisi, sehingga penerima dapat mendeteksi dan/atau mengoreksi sejumlah error yang mungkin terjadi selama transmisi melalui kanal yang ber-noise, tanpa memerlukan retransmisi." },
{ "en": "Sebutkan contoh kode FEC yang umum.", "id": "Hamming code, Reed-Solomon code, convolutional code, turbo code, LDPC (Low-Density Parity-Check) code." },
{ "en": "Apa itu 'interleaving' dalam sistem komunikasi digital?", "id": "Teknik menyusun ulang urutan bit data sebelum transmisi (dan menyusun kembali di penerima) untuk menyebarkan efek burst error (error yang terjadi secara berurutan) menjadi error tunggal yang lebih mudah dikoreksi oleh kode FEC." },
{ "en": "Apa yang dimaksud dengan 'spread spectrum' techniques? Sebutkan dua jenis utama.", "id": "Teknik transmisi di mana bandwidth sinyal yang ditransmisikan disebar jauh lebih lebar dari bandwidth minimum yang diperlukan untuk membawa informasi. Tujuannya untuk meningkatkan ketahanan terhadap interferensi dan jamming, serta memungkinkan multiple access. Dua jenis utama: Direct Sequence Spread Spectrum (DSSS) dan Frequency Hopping Spread Spectrum (FHSS)." },
{ "en": "Apa itu 'Code Division Multiple Access' (CDMA)?", "id": "Skema multiple access di mana beberapa pengguna dapat mentransmisikan secara bersamaan pada pita frekuensi yang sama, dengan membedakan sinyal masing-masing pengguna menggunakan kode penyebaran (spreading code) yang unik." },
{ "en": "Apa itu 'Shannon-Hartley theorem' dan apa yang ditetapkannya?", "id": "Teorema dalam teori informasi yang menetapkan kapasitas kanal maksimum (C, dalam bit per detik) dari suatu kanal komunikasi dengan bandwidth tertentu (B, dalam Hz) dan rasio sinyal-terhadap-noise (S/N) tertentu: $C = B \log_2(1 + S/N)$." },
{ "en": "Apa yang dimaksud dengan 'Fourier Transform' dan 'Inverse Fourier Transform'?", "id": "Fourier Transform adalah operasi matematika yang menguraikan fungsi waktu (sinyal) menjadi komponen-komponen frekuensinya (spektrum). Inverse Fourier Transform melakukan sebaliknya, merekonstruksi sinyal domain waktu dari spektrum frekuensinya." },
{ "en": "Apa itu 'Laplace Transform' dan bagaimana penggunaannya dalam analisis rangkaian?", "id": "Transformasi integral yang mengubah fungsi waktu (misalnya, sinyal atau respons impuls rangkaian) menjadi fungsi frekuensi kompleks (s-domain). Digunakan untuk menganalisis rangkaian linear time-invariant (LTI), terutama untuk menemukan respons transien dan frekuensi, serta stabilitas sistem." },
{ "en": "Apa itu 'Z-Transform' dan di mana biasanya digunakan?", "id": "Transformasi matematika yang mengubah sinyal waktu diskrit (sekuens) menjadi representasi domain frekuensi kompleks (z-domain). Analog dengan Laplace Transform untuk sistem waktu kontinu, digunakan dalam analisis dan desain sistem waktu diskrit dan filter digital." },
{ "en": "Apa yang dimaksud dengan 'transfer function' (fungsi transfer) H(s) atau H(z) dari suatu sistem?", "id": "Rasio antara output sistem terhadap input sistem dalam domain frekuensi kompleks (s-domain untuk sistem kontinu, z-domain untuk sistem diskrit). Mengkarakterisasi respons sistem terhadap berbagai frekuensi." },
{ "en": "Apa itu 'poles' dan 'zeros' dari fungsi transfer dan bagaimana pengaruhnya terhadap respons sistem?", "id": "Poles adalah nilai s (atau z) di mana fungsi transfer menjadi tak hingga. Zeros adalah nilai s (atau z) di mana fungsi transfer menjadi nol. Posisi poles dan zeros menentukan stabilitas, respons frekuensi, dan respons transien sistem." },
{ "en": "Jelaskan kriteria stabilitas 'Bounded-Input, Bounded-Output' (BIBO).", "id": "Suatu sistem dikatakan stabil BIBO jika setiap input yang terbatas (bounded) selalu menghasilkan output yang juga terbatas. Untuk sistem LTI, ini setara dengan semua pole dari fungsi transfer berada di setengah bidang kiri (left-half plane) pada s-domain, atau di dalam unit circle pada z-domain." },
{ "en": "Apa itu 'Bode plot'?", "id": "Representasi grafis dari respons frekuensi sistem linear, terdiri dari dua plot: plot magnitudo (dalam dB) vs frekuensi (logaritmik) dan plot fasa (dalam derajat) vs frekuensi (logaritmik)." },
{ "en": "Apa itu 'Nyquist stability criterion'?", "id": "Kriteria grafis dalam teori kontrol untuk menentukan stabilitas sistem umpan balik loop tertutup dengan menganalisis plot Nyquist dari fungsi transfer loop terbuka (open-loop transfer function)." },
{ "en": "Apa yang dimaksud dengan 'gain margin' dan 'phase margin' pada sistem umpan balik?", "id": "Ukuran stabilitas relatif sistem. Gain margin adalah seberapa besar gain loop terbuka dapat ditingkatkan sebelum sistem menjadi tidak stabil. Phase margin adalah seberapa besar pergeseran fasa tambahan pada frekuensi gain crossover yang dapat ditoleransi sebelum sistem menjadi tidak stabil." },
{ "en": "Apa itu 'root locus' analysis?", "id": "Metode grafis dalam teori kontrol untuk menganalisis bagaimana pole dari sistem loop tertutup berubah saat parameter sistem (biasanya gain loop) divariasikan. Digunakan untuk desain controller." },
{ "en": "Apa perbedaan antara 'analog filter' dan 'digital filter'?", "id": "Analog filter memproses sinyal analog kontinu menggunakan komponen elektronik pasif (R, L, C) dan/atau aktif (Op-Amp). Digital filter memproses sinyal digital (atau sinyal analog yang telah di-ADC) menggunakan operasi aritmatika pada sampel sinyal, diimplementasikan dalam perangkat keras digital atau perangkat lunak." },
{ "en": "Sebutkan dua jenis utama arsitektur filter digital: FIR dan IIR.", "id": "FIR (Finite Impulse Response) filter memiliki respons impuls yang berdurasi terbatas, selalu stabil, dan dapat memiliki fasa linear. IIR (Infinite Impulse Response) filter memiliki respons impuls yang berdurasi tak terbatas (karena umpan balik), bisa lebih efisien (orde lebih rendah untuk spesifikasi yang sama), tetapi bisa tidak stabil dan biasanya tidak memiliki fasa linear." },
{ "en": "Apa itu 'windowing' dalam desain filter FIR?", "id": "Proses mengalikan respons impuls ideal (yang biasanya tak terbatas) dari suatu filter dengan fungsi window (yang berdurasi terbatas) untuk mendapatkan respons impuls FIR yang praktis. Bertujuan untuk mengurangi efek Gibbs (ripple) akibat pemotongan respons impuls." },
{ "en": "Sebutkan beberapa jenis window function yang umum (misalnya, Rectangular, Hamming, Hanning, Blackman).", "id": "Rectangular (Boxcar), Triangular (Bartlett), Hamming, Hanning (Hann), Blackman, Kaiser." },
{ "en": "Apa itu 'decimation' dan 'interpolation' dalam pemrosesan sinyal digital multi-rate?", "id": "Decimation adalah proses mengurangi sampling rate sinyal digital (downsampling), biasanya setelah filtering anti-aliasing. Interpolation adalah proses meningkatkan sampling rate sinyal digital (upsampling), biasanya diikuti oleh filtering anti-imaging." },
{ "en": "Apa itu 'Fast Fourier Transform' (FFT)?", "id": "Algoritma yang sangat efisien untuk menghitung Discrete Fourier Transform (DFT) dan inversnya. Mengurangi kompleksitas komputasi DFT dari $O(N^2)$ menjadi $O(N \log N)$." },
{ "en": "Apa itu 'convolution' (konvolusi) dalam pemrosesan sinyal?", "id": "Operasi matematika pada dua fungsi (misalnya, sinyal input dan respons impuls sistem) yang menghasilkan fungsi ketiga yang merepresentasikan output sistem. Dalam domain waktu, konvolusi setara dengan perkalian dalam domain frekuensi." },
{ "en": "Apa itu 'autocorrelation' dan 'cross-correlation' dari sinyal?", "id": "Autocorrelation mengukur kemiripan sinyal dengan versi dirinya yang digeser waktu, berguna untuk mendeteksi periodisitas. Cross-correlation mengukur kemiripan antara dua sinyal berbeda sebagai fungsi dari pergeseran waktu relatif, berguna untuk deteksi sinyal atau estimasi delay." },
{ "en": "Apa yang dimaksud dengan 'white noise' (noise putih)?", "id": "Sinyal acak yang memiliki kerapatan spektral daya (power spectral density) yang konstan di semua frekuensi (atau dalam bandwidth tertentu). Autokorelasinya adalah fungsi delta Dirac (impuls) pada lag nol." },
{ "en": "Apa itu 'matched filter'?", "id": "Filter linear optimal yang dirancang untuk memaksimalkan rasio sinyal-terhadap-noise (SNR) pada outputnya ketika sinyal tertentu yang diketahui (template) ditambah noise putih dilewatkan melaluinya. Respons impulsnya adalah versi terbalik waktu dan terkonjugasi dari sinyal template." },
{ "en": "Apa itu 'Kalman filter'?", "id": "Algoritma rekursif optimal yang menggunakan serangkaian pengukuran yang diamati dari waktu ke waktu, yang mengandung noise statistik dan ketidakakuratan lainnya, dan menghasilkan estimasi dari variabel keadaan (state variables) yang tidak diketahui yang cenderung lebih akurat daripada yang didasarkan pada satu pengukuran saja, dengan cara memprediksi keadaan berikutnya dan mengoreksinya dengan pengukuran baru." },
{ "en": "Apa itu 'adaptive filter'?", "id": "Filter digital yang koefisiennya dapat menyesuaikan diri secara otomatis (beradaptasi) terhadap perubahan karakteristik sinyal input atau noise. Digunakan dalam aplikasi seperti noise cancellation, echo cancellation, dan channel equalization." },
{ "en": "Sebutkan salah satu algoritma adaptif yang umum (misalnya, LMS, RLS).", "id": "LMS (Least Mean Squares) algorithm, RLS (Recursive Least Squares) algorithm, NLMS (Normalized LMS)." },
{ "en": "Apa yang dimaksud dengan 'sampling theorem' (teorema sampling Nyquist-Shannon)?", "id": "Menyatakan bahwa untuk merekonstruksi sinyal waktu kontinu dari sampel-sampelnya tanpa kehilangan informasi (aliasing), frekuensi sampling ($f_s$) harus setidaknya dua kali frekuensi komponen tertinggi ($f_{max}$) dalam sinyal tersebut ($f_s \ge 2f_{max}$). Frekuensi $2f_{max}$ disebut frekuensi Nyquist." },
{ "en": "Apa itu 'quantization' (kuantisasi) dalam konversi analog-ke-digital?", "id": "Proses memetakan nilai-nilai amplitudo sinyal analog yang kontinu ke himpunan terbatas dari nilai-nilai digital diskrit. Menimbulkan error kuantisasi." },
{ "en": "Apa itu 'quantization noise' (noise kuantisasi)?", "id": "Error yang diperkenalkan selama proses kuantisasi, yaitu selisih antara sinyal analog asli dan nilai digital yang dikuantisasi. Dianggap sebagai noise acak jika step kuantisasi cukup kecil." },
{ "en": "Apa yang dimaksud dengan 'dithering' dalam kuantisasi?", "id": "Proses menambahkan noise acak level rendah ke sinyal analog sebelum kuantisasi. Dapat meningkatkan kualitas persepsi sinyal yang dikuantisasi dengan cara mengubah sifat error kuantisasi (misalnya, dari harmonik menjadi noise yang lebih merata) dan memungkinkan representasi sinyal di bawah level LSB." },
{ "en": "Apa itu 'companding' (compressing-expanding)?", "id": "Teknik yang digunakan dalam sistem komunikasi (terutama suara) untuk meningkatkan dynamic range efektif. Sinyal dikompresi (amplitudo rendah dikuatkan lebih besar dari amplitudo tinggi) sebelum transmisi atau kuantisasi, dan diekspansi kembali di penerima. Contoh: A-law, µ-law." },
{ "en": "Jelaskan konsep 'metastability' dalam sistem digital sinkron.", "id": "Kondisi tidak stabil dan tidak dapat diprediksi pada output flip-flop atau latch (antara logika 0 dan 1 untuk waktu yang tidak pasti) yang dapat terjadi jika batasan waktu setup atau hold dilanggar, atau jika input ke elemen sinkronisasi bersifat asinkron. Dapat menyebabkan kegagalan sistem." },
{ "en": "Bagaimana cara menangani atau memitigasi metastability?", "id": "Menggunakan synchronizer (misalnya, dua atau lebih flip-flop yang dirangkai seri), memastikan sinyal asinkron hanya di-sample pada frekuensi clock yang relatif rendah terhadap perubahan sinyal, menggunakan flip-flop dengan Mean Time Between Failures (MTBF) yang tinggi terhadap metastability, dan desain timing yang cermat." },
{ "en": "Apa itu 'clock domain crossing' (CDC)?", "id": "Transfer sinyal atau data antara dua bagian dari sistem digital yang beroperasi dengan sinyal clock yang berbeda (frekuensi berbeda, fasa berbeda, atau tidak sinkron). Memerlukan teknik sinkronisasi khusus untuk mencegah metastability dan kehilangan data." },
{ "en": "Sebutkan salah satu teknik sinkronisasi untuk CDC.", "id": "Two-flop synchronizer (untuk sinyal kontrol tunggal), FIFO asinkron (untuk data bus), handshake protocol." },
{ "en": "Apa itu 'static timing analysis' (STA)?", "id": "Metode untuk memverifikasi timing (penundaan propagasi, setup time, hold time) dari desain sirkuit digital tanpa melakukan simulasi input-output. Menganalisis semua jalur timing berdasarkan model timing komponen dan interkoneksi." },
{ "en": "Apa yang dimaksud dengan 'critical path' dalam STA?", "id": "Jalur propagasi sinyal dalam rangkaian digital yang memiliki penundaan terbesar, yang menentukan frekuensi operasi maksimum rangkaian tersebut." },
{ "en": "Apa itu 'clock skew' dan 'clock jitter'?", "id": "Clock skew adalah perbedaan waktu kedatangan tepi aktif clock pada pin clock dari flip-flop yang berbeda dalam satu domain clock. Clock jitter adalah variasi sesaat dari posisi tepi clock dari posisi idealnya dalam domain waktu." },
{ "en": "Apa itu 'scan chain' dalam Design for Testability (DFT)?", "id": "Teknik di mana flip-flop dalam desain dihubungkan menjadi satu atau lebih shift register panjang (scan chain) selama mode tes. Memungkinkan nilai internal flip-flop dikontrol (di-scan in) dan diamati (di-scan out) secara serial, meningkatkan testability." },
{ "en": "Apa itu 'Built-In Self-Test' (BIST)?", "id": "Teknik DFT di mana sirkuit tambahan dimasukkan ke dalam IC untuk memungkinkan IC menguji dirinya sendiri, biasanya dengan pattern generator dan signature analyzer on-chip. Mengurangi ketergantungan pada peralatan tes eksternal yang mahal." },
{ "en": "Apa itu 'Automatic Test Pattern Generation' (ATPG)?", "id": "Proses otomatis menghasilkan set input test vector (pola tes) yang bertujuan untuk mendeteksi sebanyak mungkin jenis kesalahan (fault model, misalnya stuck-at fault) dalam desain sirkuit digital." },
{ "en": "Jelaskan 'stuck-at fault model'.", "id": "Model kesalahan umum dalam pengujian digital di mana suatu node (input atau output gerbang) diasumsikan 'terjebak' secara permanen pada logika 0 (stuck-at-0) atau logika 1 (stuck-at-1)." },
{ "en": "Apa yang dimaksud dengan 'fault coverage' dalam pengujian IC?", "id": "Persentase dari total kemungkinan kesalahan (berdasarkan fault model yang digunakan) yang dapat dideteksi oleh set test vector yang diberikan." },
{ "en": "Apa itu 'Analog-to-Digital Converter (ADC) testing' dan parameter apa yang diuji?", "id": "Proses memverifikasi kinerja ADC. Parameter yang diuji meliputi DNL, INL, offset error, gain error, SNR, SINAD (Signal-to-Noise and Distortion Ratio), SFDR, ENOB (Effective Number of Bits)." },
{ "en": "Apa itu 'Digital-to-Analog Converter (DAC) testing' dan parameter apa yang diuji?", "id": "Proses memverifikasi kinerja DAC. Parameter yang diuji meliputi DNL, INL, offset error, gain error, settling time, glitch energy, THD." },
{ "en": "Apa yang dimaksud dengan 'Y-parameters' (admittance parameters) untuk jaringan dua-port?", "id": "Sekelompok parameter yang menghubungkan arus port dengan tegangan port: $I_1 = Y_{11}V_1 + Y_{12}V_2$ dan $I_2 = Y_{21}V_1 + Y_{22}V_2$. Berguna untuk analisis rangkaian dengan koneksi paralel." },
{ "en": "Apa yang dimaksud dengan 'Z-parameters' (impedance parameters) untuk jaringan dua-port?", "id": "Sekelompok parameter yang menghubungkan tegangan port dengan arus port: $V_1 = Z_{11}I_1 + Z_{12}I_2$ dan $V_2 = Z_{21}I_1 + Z_{22}I_2$. Berguna untuk analisis rangkaian dengan koneksi seri." },
{ "en": "Apa yang dimaksud dengan 'ABCD parameters' (transmission parameters) untuk jaringan dua-port?", "id": "Sekelompok parameter yang menghubungkan tegangan dan arus di port input dengan tegangan dan arus di port output: $V_1 = AV_2 - BI_2$ dan $I_1 = CV_2 - DI_2$. Berguna untuk analisis jaringan yang dirangkai secara kaskade (cascade)." },
{ "en": "Jelaskan konsep 'duality' (dualitas) dalam teori rangkaian listrik.", "id": "Prinsip di mana terdapat korespondensi satu-satu antara hukum, elemen, dan konfigurasi dalam rangkaian listrik. Misalnya, seri adalah dual dari paralel, tegangan adalah dual dari arus, induktor adalah dual dari kapasitor, Hukum Tegangan Kirchhoff adalah dual dari Hukum Arus Kirchhoff." },
{ "en": "Apa itu 'superposition theorem' (teorema superposisi)?", "id": "Menyatakan bahwa dalam rangkaian linear, respons total (tegangan atau arus) di setiap elemen yang disebabkan oleh beberapa sumber independen adalah jumlah aljabar dari respons yang disebabkan oleh masing-masing sumber yang bekerja sendiri (dengan sumber lain dinonaktifkan: sumber tegangan di-short, sumber arus di-open)." },
{ "en": "Apa itu 'Thevenin's theorem' (teorema Thevenin)?", "id": "Menyatakan bahwa setiap jaringan linear dua-terminal dapat digantikan oleh rangkaian ekuivalen yang terdiri dari satu sumber tegangan (Vth) seri dengan satu resistor (Rth). Vth adalah tegangan open-circuit antar terminal, Rth adalah resistansi ekuivalen dilihat dari terminal dengan semua sumber independen dinonaktifkan." },
{ "en": "Apa itu 'Norton's theorem' (teorema Norton)?", "id": "Menyatakan bahwa setiap jaringan linear dua-terminal dapat digantikan oleh rangkaian ekuivalen yang terdiri dari satu sumber arus (IN) paralel dengan satu resistor (RN). IN adalah arus short-circuit antar terminal, RN sama dengan Rth." },
{ "en": "Apa itu 'maximum power transfer theorem' (teorema transfer daya maksimum)?", "id": "Menyatakan bahwa untuk sumber DC dengan resistansi internal, daya maksimum ditransfer ke beban ketika resistansi beban sama dengan resistansi internal sumber. Untuk sumber AC, daya maksimum ditransfer ketika impedansi beban adalah konjugat kompleks dari impedansi internal sumber ($Z_L = Z_S^*$) ." },
{ "en": "Apa yang dimaksud dengan 'resonance' (resonansi) dalam rangkaian RLC?", "id": "Kondisi dalam rangkaian RLC di mana reaktansi induktif ($X_L$) sama dengan reaktansi kapasitif ($X_C$). Pada resonansi seri, impedansi total minimum dan arus maksimum. Pada resonansi paralel, impedansi total maksimum dan arus total minimum." },
{ "en": "Apa itu 'quality factor' (Q) pada rangkaian RLC resonan?", "id": "Rasio antara energi yang tersimpan dalam reaktor (induktor atau kapasitor) terhadap energi yang didisipasikan per siklus oleh resistor. Mengindikasikan 'ketajaman' kurva resonansi; Q tinggi berarti bandwidth sempit." },
{ "en": "Apa itu 'bandwidth' dari rangkaian RLC resonan?", "id": "Rentang frekuensi di mana daya yang ditransfer ke resistor adalah setidaknya setengah dari daya pada frekuensi resonansi (titik -3dB). $BW = f_0/Q$, di mana $f_0$ adalah frekuensi resonansi." },
{ "en": "Jelaskan konsep 'magnetic hysteresis'.", "id": "Fenomena pada material feromagnetik di mana magnetisasi (B) tertinggal (lags) di belakang medan magnet luar (H) yang menyebabkannya. Menghasilkan loop histeresis pada kurva B-H, yang areanya merepresentasikan rugi energi per siklus magnetisasi." },
{ "en": "Apa itu 'eddy currents' (arus Eddy)?", "id": "Arus listrik lokal yang diinduksikan dalam konduktor akibat perubahan medan magnet di sekitarnya (sesuai Hukum Faraday). Arus ini mengalir dalam loop tertutup dan dapat menyebabkan rugi-rugi daya (pemanasan) dan efek peredaman." },
{ "en": "Bagaimana cara mengurangi rugi-rugi akibat arus Eddy pada inti transformator atau motor?", "id": "Menggunakan inti yang terbuat dari laminasi tipis bahan feromagnetik yang diisolasi satu sama lain, sehingga membatasi jalur arus Eddy dan meningkatkan resistansi efektif terhadap aliran arus tersebut." },
{ "en": "Apa itu 'skin effect' pada konduktor arus AC frekuensi tinggi?", "id": "Kecenderungan arus AC frekuensi tinggi untuk berkonsentrasi mengalir di dekat permukaan konduktor, bukan terdistribusi merata di seluruh penampang. Meningkatkan resistansi efektif konduktor pada frekuensi tinggi." },
{ "en": "Apa itu 'proximity effect' pada konduktor berdekatan yang membawa arus AC frekuensi tinggi?", "id": "Fenomena di mana distribusi arus dalam satu konduktor dipengaruhi oleh medan magnet dari konduktor lain yang berdekatan dan membawa arus. Dapat meningkatkan resistansi AC dan rugi-rugi." },
{ "en": "Jelaskan prinsip kerja 'Hall effect sensor'.", "id": "Ketika konduktor atau semikonduktor yang membawa arus ditempatkan dalam medan magnet yang tegak lurus terhadap arah arus, timbul beda potensial (tegangan Hall) yang melintang (tegak lurus terhadap arus dan medan magnet) akibat gaya Lorentz pada pembawa muatan. Tegangan Hall proporsional dengan kuat medan magnet dan arus." },
{ "en": "Apa itu 'piezoelectric effect'?", "id": "Kemampuan material tertentu (kristal piezoelektrik seperti kuarsa, atau keramik tertentu) untuk menghasilkan muatan listrik (tegangan) sebagai respons terhadap tekanan atau regangan mekanis yang diterapkan (efek piezoelektrik langsung), dan sebaliknya, mengalami deformasi mekanis saat dikenai medan listrik (efek piezoelektrik terbalik)." },
{ "en": "Sebutkan aplikasi dari efek piezoelektrik.", "id": "Sensor tekanan, akselerometer, mikrofon, buzzer, aktuator presisi, osilator kristal (untuk stabilitas frekuensi), pemantik gas, sonar." },
{ "en": "Apa itu 'thermoelectric effect' (efek termoelektrik)? Sebutkan salah satu contohnya (Seebeck, Peltier, Thomson).", "id": "Fenomena konversi langsung perbedaan suhu menjadi tegangan listrik, atau sebaliknya. Efek Seebeck: timbulnya tegangan pada persambungan dua logam berbeda jika ada perbedaan suhu. Efek Peltier: timbulnya penyerapan atau pelepasan panas pada persambungan dua logam berbeda saat dialiri arus. Efek Thomson: pemanasan atau pendinginan konduktor homogen saat dialiri arus dan terdapat gradien suhu." },
{ "en": "Apa itu 'thermocouple' (termokopel)?", "id": "Sensor suhu yang terdiri dari dua konduktor logam berbeda yang disambungkan di satu ujung (persambungan ukur). Menghasilkan tegangan kecil (berdasarkan efek Seebeck) yang proporsional dengan perbedaan suhu antara persambungan ukur dan persambungan referensi (cold junction)." },
{ "en": "Apa itu 'Peltier device' (elemen Peltier) atau 'Thermoelectric Cooler' (TEC)?", "id": "Perangkat semikonduktor yang memanfaatkan efek Peltier untuk memindahkan panas dari satu sisi ke sisi lain saat dialiri arus listrik. Dapat digunakan untuk pendinginan atau pemanasan presisi." },
{ "en": "Apa yang dimaksud dengan 'superconductivity' (superkonduktivitas)?", "id": "Fenomena yang diamati pada material tertentu pada suhu sangat rendah (di bawah suhu kritisnya) di mana resistansi listriknya menjadi nol dan medan magnet diusir dari interiornya (efek Meissner)." },
{ "en": "Apa itu 'critical temperature' ($T_c$) dan 'critical magnetic field' ($H_c$) pada superkonduktor?", "id": "$T_c$ adalah suhu di bawah mana material menjadi superkonduktif. $H_c$ adalah kuat medan magnet kritis di atas mana superkonduktivitas dihancurkan, bahkan pada suhu di bawah $T_c$." },
{ "en": "Apa itu 'Josephson junction'?", "id": "Persambungan yang terdiri dari dua superkonduktor yang dipisahkan oleh lapisan tipis isolator atau logam normal. Menunjukkan efek Josephson, yaitu aliran arus super (supercurrent) tanpa tegangan, dan osilasi arus AC saat diberi tegangan DC." },
{ "en": "Sebutkan aplikasi potensial dari superkonduktivitas.", "id": "Magnet superkonduktor (untuk MRI, akselerator partikel, kereta maglev), transmisi daya tanpa rugi, SQUID (Superconducting Quantum Interference Device) untuk mengukur medan magnet sangat lemah, komputasi kuantum." },
{ "en": "Apa itu 'spintronics' (spin electronics)?", "id": "Bidang ilmu dan teknologi yang memanfaatkan sifat intrinsik spin elektron (selain muatannya) untuk menyimpan, memproses, dan mentransmisikan informasi. Berpotensi menghasilkan perangkat yang lebih cepat, lebih kecil, dan lebih hemat energi." },
{ "en": "Apa itu 'Giant Magnetoresistance' (GMR)?", "id": "Efek mekanika kuantum yang diamati pada struktur multilayer tipis yang terdiri dari lapisan feromagnetik dan non-magnetik, di mana resistansi listriknya berubah secara signifikan sebagai respons terhadap medan magnet luar. Dasar dari read head pada hard disk modern." },
{ "en": "Apa itu 'Tunnel Magnetoresistance' (TMR)?", "id": "Efek mekanika kuantum yang mirip GMR tetapi terjadi pada magnetic tunnel junction (MTJ), di mana dua lapisan feromagnetik dipisahkan oleh lapisan isolator tipis (tunnel barrier). Resistansi junction bergantung pada orientasi magnetisasi relatif kedua lapisan feromagnetik." },
{ "en": "Apa itu 'memristor' (memory resistor)?", "id": "Komponen elektronik pasif dua-terminal non-linear teoretis (dan sekarang beberapa implementasi fisik) yang resistansinya bergantung pada sejarah arus yang telah mengalir melaluinya (memiliki 'memori'). Dianggap sebagai elemen rangkaian dasar keempat setelah resistor, kapasitor, dan induktor." },
{ "en": "Apa itu 'photonic crystal'?", "id": "Struktur periodik nanoskala (biasanya dielektrik) yang dirancang untuk mempengaruhi gerakan foton (cahaya) dengan cara yang mirip dengan bagaimana kristal semikonduktor mempengaruhi elektron. Dapat memiliki 'photonic bandgap', yaitu rentang frekuensi di mana cahaya tidak dapat merambat." },
{ "en": "Apa itu 'metamaterial'?", "id": "Material buatan yang direkayasa untuk memiliki sifat elektromagnetik (atau akustik) yang tidak ditemukan di alam, biasanya dengan merancang struktur sub-panjang gelombang periodik. Dapat memiliki indeks bias negatif, misalnya." },
{ "en": "Sebutkan aplikasi potensial dari metamaterial.", "id": "Lensa sempurna (superlens), jubah tembus pandang (invisibility cloak), antena yang lebih efisien dan kecil, sensor sensitif." },
{ "en": "Apa itu 'plasmonics'?", "id": "Bidang yang mempelajari interaksi antara radiasi elektromagnetik dan elektron bebas (plasmon) pada permukaan logam atau dalam nanostruktur logam. Memungkinkan manipulasi cahaya pada skala nanometer." },
{ "en": "Apa itu 'surface plasmon polariton' (SPP)?", "id": "Gelombang elektromagnetik yang merambat di sepanjang antarmuka antara logam dan dielektrik, digabungkan dengan osilasi kolektif elektron bebas di logam." },
{ "en": "Apa yang dimaksud dengan 'quantum well', 'quantum wire', dan 'quantum dot'?", "id": "Struktur semikonduktor nanoskala di mana gerakan pembawa muatan (elektron atau hole) terbatas (confined) dalam satu, dua, atau tiga dimensi secara spasial karena efek kuantum. Ini mengkuantisasi level energi mereka." },
{ "en": "Apa itu 'single-electron transistor' (SET)?", "id": "Perangkat switching elektronik yang memanfaatkan efek Coulomb blockade untuk mengontrol aliran elektron satu per satu. Terdiri dari pulau konduktif kecil yang dihubungkan ke source dan drain melalui tunnel junction, dan dikontrol oleh gate kapasitif." },
{ "en": "Apa itu 'Coulomb blockade'?", "id": "Peningkatan resistansi pada tegangan bias kecil pada perangkat elektronik yang memiliki setidaknya satu tunnel junction resistansi rendah dan pulau konduktif kecil. Karena efek elektrostatis, elektron tidak dapat melakukan tunneling ke pulau jika energi pengisian kapasitif ($e^2/2C$) lebih besar dari energi termal." },
{ "en": "Apa itu 'molecular electronics' (elektronika molekuler)?", "id": "Bidang interdisipliner yang mempelajari dan bertujuan untuk memanfaatkan molekul tunggal atau rakitan molekul sebagai komponen elektronik fungsional." },
{ "en": "Apa itu 'DNA nanotechnology'?", "id": "Bidang yang merancang dan memanufaktur struktur dan perangkat buatan pada skala nano menggunakan asam deoksiribonukleat (DNA) dan asam nukleat lainnya sebagai bahan konstruksi, memanfaatkan sifat pengenalan molekuler spesifik (pasangan basa Watson-Crick)." },
{ "en": "Apa itu 'lab-on-a-chip' (LOC)?", "id": "Perangkat terintegrasi berukuran kecil (beberapa milimeter hingga beberapa sentimeter persegi) yang mampu melakukan satu atau beberapa fungsi laboratorium, seperti analisis kimia atau biokimia, pada volume sampel yang sangat kecil. Sering menggunakan teknologi mikrofluida." },
{ "en": "Apa itu 'microfluidics'?", "id": "Ilmu dan teknologi sistem yang memproses atau memanipulasi volume cairan kecil (mikroliter hingga pikoliter) menggunakan saluran dengan dimensi puluhan hingga ratusan mikrometer." },
{ "en": "Apa itu 'Internet of Bio-Nano Things' (IoBNT)?", "id": "Konsep interkoneksi perangkat biologis dan/atau nanoteknologi dengan jaringan komunikasi yang ada, memungkinkan pengumpulan data biologis dan intervensi pada skala molekuler atau seluler." },
{ "en": "Apa yang dimaksud dengan 'reliability engineering' dalam konteks elektronika?", "id": "Sub-disiplin rekayasa sistem yang menekankan kemampuan peralatan untuk berfungsi tanpa kegagalan selama periode waktu tertentu di bawah kondisi yang ditentukan. Melibatkan prediksi, pencegahan, dan mitigasi kegagalan." },
{ "en": "Apa itu 'Arrhenius equation' dan bagaimana penggunaannya dalam memprediksi keandalan komponen elektronik?", "id": "Persamaan yang menghubungkan laju reaksi kimia (termasuk mekanisme degradasi pada komponen) dengan suhu. Digunakan untuk memperkirakan percepatan laju kegagalan komponen pada suhu yang lebih tinggi (accelerated life testing)." },
{ "en": "Apa itu 'Highly Accelerated Life Test' (HALT) dan 'Highly Accelerated Stress Screen' (HASS)?", "id": "HALT adalah proses pengujian destruktif untuk menemukan batas operasional dan destruktif suatu produk dengan menerapkan tegangan (stress) termal dan getaran yang jauh melebihi spesifikasi. HASS adalah proses penyaringan produksi untuk mendeteksi cacat manufaktur dengan menerapkan stress yang lebih rendah dari batas destruktif yang ditemukan di HALT." },
{ "en": "Apa yang dimaksud dengan 'functional safety' (keselamatan fungsional) dalam sistem elektronik?", "id": "Bagian dari keselamatan keseluruhan sistem yang bergantung pada sistem kontrol listrik/elektronik/programmable electronic (E/E/PE) untuk berfungsi dengan benar sebagai respons terhadap inputnya, guna mengurangi risiko bahaya. Diatur oleh standar seperti IEC 61508." },
{ "en": "Apa itu 'Safety Integrity Level' (SIL)?", "id": "Ukuran diskrit (biasanya 1 hingga 4) dari kinerja keselamatan fungsional yang diperlukan untuk suatu fungsi keselamatan. SIL yang lebih tinggi menunjukkan tingkat risiko yang lebih tinggi dan kinerja keselamatan yang lebih ketat yang diperlukan." },
{ "en": "Apa itu 'Hazard and Operability Study' (HAZOP)?", "id": "Teknik identifikasi bahaya dan masalah operabilitas yang terstruktur dan sistematis, menggunakan kata panduan (guide words) untuk menstimulasi pemikiran tentang potensi deviasi dari niat desain." },
{ "en": "Apa itu 'Failure Rate' (laju kegagalan, $\lambda$) dan 'Mean Time Between Failures' (MTBF)?", "id": "Laju kegagalan adalah frekuensi kegagalan per unit waktu (misalnya, kegagalan per juta jam). MTBF adalah waktu rata-rata yang diharapkan antara dua kegagalan berturut-turut untuk sistem yang dapat diperbaiki. Untuk laju kegagalan konstan, MTBF = $1/\lambda$." },
{ "en": "Jelaskan 'bathtub curve' yang menggambarkan laju kegagalan produk elektronik seiring waktu.", "id": "Kurva berbentuk bak mandi yang menggambarkan tiga fase laju kegagalan: 1. Infant mortality (kegagalan awal, laju kegagalan menurun) akibat cacat manufaktur. 2. Useful life (laju kegagalan konstan rendah) akibat kegagalan acak. 3. Wear-out (penuaan, laju kegagalan meningkat) akibat degradasi komponen." },
{ "en": "Apa itu 'derating' dalam desain elektronik untuk meningkatkan keandalan?", "id": "Praktik mengoperasikan komponen (resistor, kapasitor, transistor) di bawah batas maksimum spesifikasinya (tegangan, arus, daya, suhu) untuk mengurangi stress dan memperpanjang masa pakainya." },
{ "en": "Apa yang dimaksud dengan 'redundancy' dalam desain sistem keandalan tinggi?", "id": "Penyertaan komponen atau subsistem cadangan (duplikat) sehingga sistem dapat terus berfungsi meskipun salah satu unit utama mengalami kegagalan. Contoh: N+1 redundancy, hot standby, cold standby." },
{ "en": "Apa itu 'FMEA' (Failure Mode and Effects Analysis) dan 'FMECA' (Failure Mode, Effects, and Criticality Analysis)?", "id": "FMEA adalah metode sistematis untuk mengidentifikasi potensi mode kegagalan suatu sistem dan efeknya. FMECA menambahkan analisis kritikalitas untuk mengevaluasi tingkat keparahan dan probabilitas setiap mode kegagalan guna memprioritaskan tindakan." },
{ "en": "Apa itu 'Fault Tree Analysis' (FTA)?", "id": "Pendekatan deduktif top-down untuk analisis kegagalan sistem, di mana suatu kejadian puncak (top event) yang tidak diinginkan (misalnya kegagalan sistem) dianalisis dengan mengidentifikasi semua kemungkinan penyebab level bawahnya menggunakan gerbang logika Boolean." },
// AWAL DARI BATCH SOAL BARU FINAL (Lanjutan untuk 197 tambahan - FOKUS MATERI MENDALAM)
// Target: 197 soal baru di batch ini
{ "en": "Apa yang dimaksud dengan 'quantum Hall effect' (QHE) dan 'fractional quantum Hall effect' (FQHE)?", "id": "QHE adalah fenomena mekanika kuantum pada sistem elektron dua dimensi dalam medan magnet kuat dan suhu rendah, di mana konduktansi Hall terkuantisasi menjadi kelipatan integer dari $e^2/h$. FQHE adalah fenomena serupa di mana konduktansi Hall terkuantisasi menjadi kelipatan fraksional, menunjukkan adanya quasi-partikel dengan muatan fraksional." },
{ "en": "Jelaskan konsep 'Landauer formula' dalam konduktansi mesoskopik.", "id": "Formula yang menghubungkan konduktansi listrik suatu konduktor mesoskopik dengan probabilitas transmisi elektron melaluinya. Menyatakan bahwa konduktansi adalah jumlah dari probabilitas transmisi untuk semua mode (channel) konduksi yang tersedia, dikalikan dengan kuantum konduktansi ($e^2/h$ per spin)." },
{ "en": "Apa itu 'Kondo effect' dalam fisika benda terkondensasi?", "id": "Fenomena yang diamati pada logam yang mengandung sejumlah kecil impuritas magnetik, di mana resistivitas listrik meningkat secara logaritmik saat suhu diturunkan di bawah suhu Kondo. Disebabkan oleh hamburan elektron konduksi oleh spin impuritas magnetik yang membentuk keadaan singlet terkorelasi." },
{ "en": "Jelaskan perbedaan antara 'Fermi liquid theory' dan 'Luttinger liquid theory'.", "id": "Fermi liquid theory mendeskripsikan sistem elektron berinteraksi pada suhu rendah sebagai gas Fermi dari quasi-partikel (elektron 'berpakaian') yang berperilaku mirip elektron bebas. Luttinger liquid theory mendeskripsikan sistem elektron satu dimensi berinteraksi, di mana eksitasi elementernya bukan quasi-partikel tunggal melainkan mode kolektif (misalnya, pemisahan spin-muatan)." },
{ "en": "Apa itu 'Wigner crystal'?", "id": "Fase kristalin teoretis dari elektron dalam sistem elektron berdensitas rendah pada suhu sangat rendah, di mana repulsi Coulomb antar elektron mendominasi energi kinetik, menyebabkan elektron membentuk kisi periodik untuk meminimalkan energi potensial." },
{ "en": "Jelaskan fenomena 'Mott insulator'.", "id": "Material yang seharusnya bersifat konduktif berdasarkan teori pita elektron (misalnya, pita valensi terisi setengah) tetapi bersifat isolator akibat interaksi Coulomb elektron-elektron yang kuat, yang mencegah elektron bergerak bebas meskipun ada keadaan kosong." },
{ "en": "Apa yang dimaksud dengan 'Hubbard model' dalam fisika benda terkondensasi?", "id": "Model teoretis sederhana yang sering digunakan untuk mempelajari sistem elektron berinteraksi kuat, khususnya transisi metal-insulator Mott dan magnetisme. Memperhitungkan energi kinetik hopping elektron antar situs kisi dan energi repulsi Coulomb on-site (U) jika dua elektron menempati situs yang sama." },
{ "en": "Apa itu 'topological insulator'?", "id": "Material yang bersifat isolator di bagian dalamnya (bulk) tetapi memiliki keadaan permukaan (atau tepi) yang konduktif secara topologis terlindungi. Keadaan permukaan ini memiliki sifat unik seperti spin-momentum locking dan tahan terhadap hamburan dari impuritas non-magnetik." },
{ "en": "Jelaskan konsep 'Berry phase' dan 'Berry curvature' dalam fisika benda terkondensasi.", "id": "Berry phase adalah fasa geometris yang diperoleh oleh fungsi gelombang mekanika kuantum saat parameter dalam Hamiltonian sistem diubah secara adiabatik mengelilingi loop tertutup. Berry curvature adalah medan vektor terkait yang menggambarkan bagaimana Berry phase berubah terhadap parameter tersebut, berperan penting dalam fenomena seperti efek Hall kuantum anomali." },
{ "en": "Apa itu 'Weyl semimetal'?", "id": "Fase materi topologis di mana pita konduksi dan valensi bersentuhan di titik-titik tertentu dalam ruang momentum (Weyl nodes), yang bertindak sebagai sumber atau drain monopoli Berry curvature. Memiliki keadaan permukaan unik yang disebut 'Fermi arcs'." },
{ "en": "Jelaskan konsep 'Majorana fermion' dan potensinya dalam komputasi kuantum topologis.", "id": "Partikel fermion teoretis yang merupakan antipartikelnya sendiri. Dalam sistem benda terkondensasi, eksitasi quasi-partikel Majorana dapat muncul. Sifat non-abelion mereka saat dianyam (braided) berpotensi digunakan untuk membangun qubit topologis yang tahan terhadap dekoherensi." },
{ "en": "Apa itu 'skyrmion' dalam magnetisme?", "id": "Quasi-partikel topologis berupa tekstur spin non-kolinier yang stabil dalam material magnetik tertentu. Memiliki ukuran nanometer dan dapat dimanipulasi oleh arus listrik atau medan magnet, berpotensi untuk aplikasi memori magnetik densitas tinggi dan logika spintronik." },
{ "en": "Jelaskan fenomena 'spin Hall effect' dan 'inverse spin Hall effect'.", "id": "Spin Hall effect adalah timbulnya arus spin transversal sebagai respons terhadap arus muatan longitudinal dalam material dengan kopling spin-orbit kuat. Inverse spin Hall effect adalah konversi arus spin menjadi arus muatan." },
{ "en": "Apa itu 'spin-transfer torque' (STT) dan 'spin-orbit torque' (SOT)?", "id": "STT adalah transfer momentum sudut spin dari arus elektron terpolarisasi spin ke magnetisasi lokal lapisan feromagnetik, dapat digunakan untuk membalik magnetisasi (MRAM). SOT adalah torsi pada magnetisasi yang timbul akibat arus muatan dalam material dengan kopling spin-orbit kuat atau antarmuka dengan simetri terbalik, menawarkan efisiensi switching yang lebih tinggi." },
{ "en": "Apa yang dimaksud dengan 'antiferromagnetic spintronics'?", "id": "Bidang spintronik yang memanfaatkan material antiferomagnetik (di mana momen magnetik atom tetangga berlawanan arah, menghasilkan magnetisasi netto nol). Menawarkan potensi operasi frekuensi lebih tinggi, ketahanan terhadap gangguan medan magnet eksternal, dan kepadatan lebih tinggi." },
{ "en": "Jelaskan konsep 'magnonics'.", "id": "Bidang yang mempelajari propagasi gelombang spin (magnon, kuanta dari osilasi spin kolektif) dalam material magnetik dan interaksinya dengan medan lain, bertujuan untuk pemrosesan informasi menggunakan magnon sebagai pembawa data." },
{ "en": "Apa itu 'valleytronics'?", "id": "Bidang elektronika yang bertujuan untuk memanfaatkan 'valley degree of freedom' (minimum atau maksimum lokal dalam struktur pita elektronik) pada semikonduktor tertentu (seperti monolayer transition metal dichalcogenides) sebagai pembawa informasi, analog dengan muatan (elektronika) dan spin (spintronika)." },
{ "en": "Apa yang dimaksud dengan 'exciton' dalam semikonduktor?", "id": "Keadaan terikat (quasi-partikel) dari pasangan elektron-hole yang ditarik bersama oleh interaksi Coulomb elektrostatis. Dapat terbentuk saat foton diserap oleh semikonduktor." },
{ "en": "Jelaskan perbedaan antara 'direct bandgap' dan 'indirect bandgap' semikonduktor dan implikasinya pada efisiensi emisi cahaya.", "id": "Pada direct bandgap, minimum pita konduksi dan maksimum pita valensi berada pada nilai momentum kristal (k-vector) yang sama, memungkinkan rekombinasi elektron-hole langsung dengan emisi foton yang efisien (misalnya GaAs, InP). Pada indirect bandgap, keduanya berada pada k-vector berbeda, memerlukan partisipasi fonon untuk konservasi momentum, membuat emisi cahaya kurang efisien (misalnya Si, Ge)." },
{ "en": "Apa itu 'quantum well laser' dan 'quantum dot laser'?", "id": "Laser semikonduktor di mana daerah aktifnya menggunakan struktur quantum well (lapisan tipis yang mengurung pembawa muatan dalam satu dimensi) atau quantum dot (nanokristal yang mengurung pembawa muatan dalam tiga dimensi). Kuantisasi level energi menghasilkan karakteristik emisi yang lebih baik (threshold arus lebih rendah, efisiensi lebih tinggi, panjang gelombang lebih terkontrol)." },
{ "en": "Apa itu 'vertical-cavity surface-emitting laser' (VCSEL)?", "id": "Jenis laser dioda semikonduktor yang memancarkan cahaya secara tegak lurus terhadap permukaan chip, berbeda dengan laser edge-emitting konvensional. Memiliki keunggulan dalam pengujian wafer, fabrikasi array 2D, dan efisiensi kopling ke serat optik." },
{ "en": "Jelaskan prinsip kerja 'photodetector' (detektor foto). Sebutkan beberapa jenis.", "id": "Perangkat yang mengubah sinyal optik (cahaya) menjadi sinyal listrik. Jenisnya meliputi fotodioda (PN, PIN, avalanche), fototransistor, fotokonduktor (LDR), sel fotovoltaik, photomultiplier tube (PMT)." },
{ "en": "Apa itu 'avalanche photodiode' (APD)?", "id": "Jenis fotodioda yang dirancang untuk beroperasi pada bias balik tinggi, di mana foton yang diserap menghasilkan pasangan elektron-hole yang kemudian dipercepat oleh medan listrik kuat dan menyebabkan ionisasi tumbukan (efek avalanche), menghasilkan penguatan arus internal yang signifikan." },
{ "en": "Apa itu 'Charge-Coupled Device' (CCD) dan 'CMOS image sensor' (CIS)? Apa perbedaan utamanya?", "id": "Keduanya adalah sensor gambar solid-state. CCD mentransfer muatan yang terkumpul di setiap piksel secara serial ke satu atau beberapa node output untuk konversi ke tegangan. CIS (Active Pixel Sensor) biasanya memiliki sirkuit penguat dan konversi di setiap piksel. CIS umumnya memiliki konsumsi daya lebih rendah, integrasi lebih mudah, dan biaya lebih rendah, sementara CCD tradisional unggul dalam kualitas gambar (noise rendah, dynamic range tinggi) meskipun perbedaannya semakin kecil." },
{ "en": "Apa yang dimaksud dengan 'responsivity' dan 'quantum efficiency' pada fotodetektor?", "id": "Responsivity (R) adalah rasio arus foto output terhadap daya optik input (A/W). Quantum Efficiency (QE, η) adalah rasio jumlah pembawa muatan yang dihasilkan (elektron atau hole) terhadap jumlah foton insiden." },
{ "en": "Apa itu 'dark current' pada fotodetektor?", "id": "Arus listrik kecil yang mengalir melalui fotodetektor bahkan ketika tidak ada cahaya insiden. Merupakan sumber noise dan membatasi sensitivitas detektor." },
{ "en": "Jelaskan prinsip 'nonlinear optics'. Sebutkan contoh fenomena optik non-linear.", "id": "Bidang optik yang mempelajari interaksi cahaya dengan materi di mana respons materi (misalnya polarisasi) tidak proporsional secara linear terhadap kekuatan medan listrik cahaya insiden, biasanya terjadi pada intensitas cahaya tinggi. Contoh: second-harmonic generation (SHG), third-harmonic generation (THG), sum/difference frequency generation, optical Kerr effect, self-focusing, stimulated Raman scattering." },
{ "en": "Apa itu 'second-harmonic generation' (SHG)?", "id": "Proses optik non-linear di mana dua foton dengan frekuensi yang sama berinteraksi dengan material non-linear dan menghasilkan satu foton baru dengan frekuensi dua kali lipat (panjang gelombang setengahnya)." },
{ "en": "Apa itu 'optical fiber amplifier' (penguat serat optik)? Sebutkan contoh (misalnya, EDFA).", "id": "Perangkat yang menguatkan sinyal optik secara langsung tanpa perlu mengubahnya menjadi sinyal listrik terlebih dahulu, biasanya dengan men-doping serat optik dengan elemen tanah jarang. Contoh: Erbium-Doped Fiber Amplifier (EDFA), Raman amplifier, Semiconductor Optical Amplifier (SOA)." },
{ "en": "Apa itu 'wavelength division multiplexing' (WDM) dalam komunikasi serat optik?", "id": "Teknologi yang memungkinkan beberapa sinyal optik dengan panjang gelombang (warna) berbeda ditransmisikan secara bersamaan melalui satu serat optik tunggal, meningkatkan kapasitas transmisi secara signifikan. Variannya: CWDM (Coarse WDM), DWDM (Dense WDM)." },
{ "en": "Apa itu 'soliton' dalam konteks propagasi pulsa optik di serat?", "id": "Pulsa optik yang dapat merambat jarak jauh di media non-linear dispersif tanpa mengubah bentuknya, akibat keseimbangan antara efek dispersi (yang melebarkan pulsa) dan efek non-linearitas (self-phase modulation, yang menyempitkan pulsa)." },
{ "en": "Jelaskan konsep 'optical interconnects' untuk komunikasi data jarak pendek.", "id": "Penggunaan teknologi optik (cahaya) untuk interkoneksi data antara chip, papan sirkuit, atau rak dalam sistem komputer atau pusat data, menggantikan interkoneksi listrik tembaga tradisional. Menawarkan potensi bandwidth lebih tinggi, rugi-rugi lebih rendah, dan kekebalan EMI." },
{ "en": "Apa itu 'silicon photonics'?", "id": "Bidang yang bertujuan untuk merancang dan memfabrikasi komponen dan sirkuit fotonik (seperti waveguide, modulator, detektor) menggunakan silikon sebagai platform material, memanfaatkan infrastruktur fabrikasi CMOS yang sudah matang untuk integrasi skala besar dan biaya rendah." },
{ "en": "Apa yang dimaksud dengan 'micro-electro-mechanical systems' (MEMS) dan bagaimana aplikasinya dalam optik?", "id": "Teknologi yang membuat perangkat miniatur dengan komponen mekanis dan elektris. Dalam optik, MEMS digunakan untuk membuat cermin mikro yang dapat digerakkan (micromirrors) untuk switching optik, pemindaian berkas, display (DLP), dan spektrometer miniatur." },
{ "en": "Apa itu 'Digital Light Processing' (DLP) technology?", "id": "Teknologi display yang dikembangkan oleh Texas Instruments, menggunakan array besar cermin mikro (Digital Micromirror Device - DMD) yang dapat memantulkan cahaya secara individual untuk menciptakan gambar. Setiap cermin mewakili satu piksel." },
{ "en": "Jelaskan prinsip kerja 'atomic clock' (jam atom).", "id": "Jam yang menggunakan frekuensi resonansi transisi elektronik atom tertentu (seperti cesium-133 atau rubidium-87) sebagai standar waktu. Frekuensi osilasi internal atom sangat stabil dan tidak terpengaruh oleh lingkungan, menghasilkan akurasi waktu yang sangat tinggi." },
{ "en": "Apa itu 'Global Navigation Satellite System' (GNSS)? Berikan contoh selain GPS.", "id": "Istilah umum untuk sistem navigasi berbasis satelit yang menyediakan penentuan posisi, navigasi, dan waktu (PNT) otonom secara global. Contoh: GLONASS (Rusia), Galileo (Uni Eropa), BeiDou (Tiongkok)." },
{ "en": "Jelaskan prinsip dasar penentuan posisi menggunakan GNSS.", "id": "Penerima GNSS mengukur waktu tempuh sinyal dari setidaknya empat satelit yang berbeda yang posisi orbitnya diketahui. Dengan mengetahui waktu tempuh dan kecepatan cahaya, jarak ke setiap satelit (pseudorange) dapat dihitung. Menggunakan trilaterasi (atau multilaterasi) dari jarak-jarak ini, posisi tiga dimensi penerima dan offset waktu jam penerima dapat ditentukan." },
{ "en": "Apa sumber-sumber error utama dalam penentuan posisi GNSS?", "id": "Error jam satelit dan penerima, error ephemeris (posisi orbit satelit), delay atmosfer (ionosfer dan troposfer), multipath (pantulan sinyal), geometri satelit (Dilution of Precision - DOP)." },
{ "en": "Apa itu 'Differential GPS' (DGPS) atau 'Real-Time Kinematic' (RTK) positioning?", "id": "Teknik untuk meningkatkan akurasi penentuan posisi GNSS dengan menggunakan stasiun referensi (base station) yang posisinya diketahui secara akurat. Stasiun referensi menghitung error pada sinyal GNSS dan mengirimkan koreksi ke penerima rover, memungkinkan akurasi hingga level sentimeter (untuk RTK)." },
{ "en": "Apa yang dimaksud dengan 'power electronics' (elektronika daya)?", "id": "Bidang teknik elektro yang berkaitan dengan kontrol dan konversi daya listrik dari satu bentuk ke bentuk lain menggunakan perangkat semikonduktor daya yang beroperasi sebagai saklar (misalnya, dioda, thyristor, MOSFET, IGBT)." },
{ "en": "Sebutkan empat kuadran operasi untuk konverter daya.", "id": "Kuadran I: Tegangan positif, arus positif (daya positif, motor maju). Kuadran II: Tegangan positif, arus negatif (daya negatif, pengereman regeneratif motor maju). Kuadran III: Tegangan negatif, arus negatif (daya positif, motor mundur). Kuadran IV: Tegangan negatif, arus positif (daya negatif, pengereman regeneratif motor mundur)." },
{ "en": "Apa itu 'rectifier' (penyearah)? Jelaskan perbedaan antara half-wave dan full-wave rectifier.", "id": "Rectifier mengubah tegangan AC menjadi tegangan DC. Half-wave rectifier hanya melewatkan setengah siklus gelombang AC. Full-wave rectifier (misalnya bridge rectifier) melewatkan kedua setengah siklus (dengan membalik polaritas salah satunya) menghasilkan DC yang lebih halus dan efisiensi lebih tinggi." },
{ "en": "Apa fungsi dari 'filter kapasitor' setelah penyearah?", "id": "Untuk memperhalus (meratakan) tegangan DC yang berdenyut (ripple) hasil penyearahan, dengan cara menyimpan muatan saat tegangan puncak dan melepaskannya saat tegangan turun." },
{ "en": "Apa itu 'voltage regulator' (regulator tegangan)? Sebutkan jenis linear dan switching.", "id": "Rangkaian yang menjaga tegangan output tetap konstan meskipun ada variasi pada tegangan input atau perubahan beban. Jenis linear (misalnya LDO) meregulasi dengan membuang kelebihan daya. Jenis switching (SMPS) meregulasi dengan mengontrol siklus kerja saklar frekuensi tinggi." },
{ "en": "Apa itu 'DC-DC converter'? Sebutkan topologi dasar (Buck, Boost, Buck-Boost).", "id": "Rangkaian elektronika daya yang mengubah level tegangan DC dari satu nilai ke nilai lain. Buck converter: step-down (menurunkan tegangan). Boost converter: step-up (menaikkan tegangan). Buck-Boost converter: dapat menaikkan atau menurunkan tegangan, dengan polaritas output terbalik." },
{ "en": "Jelaskan prinsip kerja 'Buck converter' (step-down converter).", "id": "Menggunakan saklar (misalnya MOSFET), dioda, induktor, dan kapasitor. Saat saklar ON, arus mengalir dari input melalui induktor ke beban dan mengisi kapasitor, energi disimpan di induktor. Saat saklar OFF, energi yang tersimpan di induktor dilepaskan melalui dioda ke beban dan kapasitor. Tegangan output rata-rata lebih rendah dari input, dikontrol oleh duty cycle saklar." },
{ "en": "Jelaskan prinsip kerja 'Boost converter' (step-up converter).", "id": "Saat saklar ON, arus dari input mengalir melalui induktor, menyimpan energi di dalamnya. Saat saklar OFF, energi induktor dilepaskan bersama dengan tegangan input melalui dioda ke beban dan kapasitor, menghasilkan tegangan output lebih tinggi dari input. Dikontrol oleh duty cycle saklar." },
{ "en": "Apa itu 'inverter' dalam elektronika daya?", "id": "Rangkaian yang mengubah daya DC menjadi daya AC dengan frekuensi dan tegangan yang diinginkan. Digunakan dalam UPS, penggerak motor AC, sistem panel surya grid-tied." },
{ "en": "Apa itu 'Pulse Width Modulation' (PWM) dan bagaimana penggunaannya dalam inverter atau konverter DC-DC?", "id": "Teknik di mana lebar pulsa dari sinyal gelombang kotak divariasikan untuk mengontrol daya rata-rata yang dikirim ke beban, atau untuk mensintesis bentuk gelombang analog (misalnya gelombang sinus pada output inverter) dengan memfilter sinyal PWM frekuensi tinggi." },
{ "en": "Apa itu 'thyristor' (Silicon Controlled Rectifier - SCR)?", "id": "Perangkat semikonduktor empat lapis (PNPN) dengan tiga terminal (Anoda, Katoda, Gate). Berfungsi sebagai saklar terkontrol yang akan menghantar arus dari anoda ke katoda jika diberi pulsa pemicu pada gate (dan anoda positif terhadap katoda), dan akan tetap menghantar (latching) meskipun pulsa gate dihilangkan, hingga arus turun di bawah holding current atau tegangan dibalik." },
{ "en": "Apa itu 'Gate Turn-Off thyristor' (GTO)?", "id": "Jenis thyristor khusus yang dapat dihidupkan dengan pulsa gate positif dan dimatikan dengan pulsa gate negatif, memberikan kontrol penuh atas switching tanpa memerlukan rangkaian komutasi eksternal seperti pada SCR konvensional." },
{ "en": "Apa itu 'Insulated Gate Bipolar Transistor' (IGBT)?", "id": "Perangkat semikonduktor daya tiga terminal yang menggabungkan impedansi input tinggi dan kecepatan switching dari MOSFET dengan kemampuan arus tinggi dan tegangan saturasi rendah dari BJT. Banyak digunakan dalam aplikasi daya menengah hingga tinggi (motor drive, inverter, las)." },
{ "en": "Apa yang dimaksud dengan 'snubber circuit' dalam elektronika daya?", "id": "Rangkaian kecil (biasanya terdiri dari resistor dan kapasitor, atau dioda) yang dipasang paralel dengan perangkat switching (thyristor, transistor) untuk menekan lonjakan tegangan (dv/dt) atau arus (di/dt) transien saat switching, melindungi perangkat dan mengurangi EMI." },
{ "en": "Apa itu 'commutation' (komutasi) pada konverter berbasis thyristor?", "id": "Proses mematikan thyristor yang sedang menghantar. Dapat berupa natural commutation (pada rangkaian AC, tegangan input membalik dan mematikan thyristor) atau forced commutation (menggunakan rangkaian eksternal untuk memaksakan arus thyristor menjadi nol atau membalik tegangannya)." },
{ "en": "Apa itu 'cycloconverter'?", "id": "Konverter daya AC-ke-AC langsung (tanpa link DC perantara) yang dapat mengubah frekuensi daya AC input menjadi frekuensi output yang lebih rendah (dan kadang-kadang variabel), dengan cara mensintesis bentuk gelombang output dari segmen-segmen gelombang input AC." },
{ "en": "Apa itu 'matrix converter'?", "id": "Konverter daya AC-ke-AC langsung yang menggunakan array saklar dua arah untuk menghubungkan setiap fasa input ke setiap fasa output, memungkinkan kontrol penuh atas tegangan, frekuensi, dan fasa output, serta faktor daya input." },
{ "en": "Jelaskan konsep 'soft switching' pada konverter daya (ZVS, ZCS).", "id": "Teknik switching di mana transisi ON atau OFF perangkat semikonduktor daya terjadi saat tegangan melintasinya nol (Zero Voltage Switching - ZVS) atau saat arus melaluinya nol (Zero Current Switching - ZCS). Bertujuan untuk mengurangi rugi-rugi switching, stress pada perangkat, dan EMI, memungkinkan operasi frekuensi lebih tinggi." },
{ "en": "Apa itu 'resonant converter'?", "id": "Jenis konverter daya yang menggunakan rangkaian tangki resonan LC untuk membentuk bentuk gelombang tegangan atau arus sinusoidal atau quasi-sinusoidal pada perangkat switching, memungkinkan soft switching (ZVS atau ZCS)." },
{ "en": "Apa yang dimaksud dengan 'interleaved converter'?", "id": "Teknik di mana beberapa konverter daya identik dioperasikan secara paralel tetapi dengan pergeseran fasa pada sinyal switchingnya. Bertujuan untuk mengurangi ripple arus input dan output, meningkatkan respons transien, dan mendistribusikan panas." },
{ "en": "Apa itu 'multilevel inverter'?", "id": "Jenis inverter yang dapat menghasilkan tegangan output AC dengan beberapa level tegangan diskrit (lebih dari dua level pada inverter H-bridge standar), menghasilkan bentuk gelombang output yang lebih mendekati sinus dengan distorsi harmonik lebih rendah dan stress dv/dt lebih rendah pada saklar. Cocok untuk aplikasi tegangan tinggi." },
{ "en": "Apa yang dimaksud dengan 'Total Harmonic Distortion' (THD) pada bentuk gelombang AC?", "id": "Ukuran seberapa besar bentuk gelombang terdistorsi dari bentuk gelombang sinusoidal murni akibat adanya komponen harmonik. Didefinisikan sebagai rasio akar kuadrat dari jumlah kuadrat amplitudo semua harmonik terhadap amplitudo komponen fundamental." },
{ "en": "Apa itu 'power factor' (faktor daya)? Jelaskan 'displacement power factor' dan 'distortion power factor'.", "id": "Faktor daya adalah rasio antara daya nyata (real power, P) yang digunakan oleh beban terhadap daya semu (apparent power, S) yang disuplai ke beban. Displacement power factor ($cos \phi$) disebabkan oleh beda fasa antara tegangan dan arus fundamental. Distortion power factor disebabkan oleh distorsi harmonik pada arus. Faktor daya total adalah hasil kali keduanya." },
{ "en": "Apa itu 'active filter' (filter aktif) dalam konteks kualitas daya?", "id": "Perangkat elektronika daya yang digunakan untuk meningkatkan kualitas daya dengan cara mengkompensasi harmonik, daya reaktif, atau ketidakseimbangan beban. Bekerja dengan menginjeksikan arus kompensasi ke jaringan." },
{ "en": "Apa itu 'Uninterruptible Power Supply' (UPS)? Sebutkan jenis utama.", "id": "Perangkat yang menyediakan daya darurat ke beban dari sumber terpisah (biasanya baterai) saat daya utama (jala-jala) gagal. Jenis utama: Off-line (Standby) UPS, Line-interactive UPS, On-line (Double-conversion) UPS." },
{ "en": "Jelaskan perbedaan antara UPS Off-line, Line-interactive, dan On-line.", "id": "Off-line: beban normalnya disuplai dari jala-jala, inverter hanya aktif saat listrik padam. Line-interactive: mirip off-line tetapi memiliki transformator tap-changing atau buck/boost untuk regulasi tegangan. On-line: beban selalu disuplai dari inverter (yang ditenagai dari penyearah/baterai), memberikan isolasi penuh dan kualitas daya terbaik (double conversion)." },
{ "en": "Apa itu 'electric motor drive' (penggerak motor listrik)?", "id": "Sistem elektronika daya yang digunakan untuk mengontrol kecepatan, torsi, arah putaran, dan parameter lain dari motor listrik (DC atau AC) secara presisi dan efisien." },
{ "en": "Jelaskan metode kontrol kecepatan motor DC (misalnya, kontrol tegangan armature, kontrol medan).", "id": "Kontrol tegangan armature: kecepatan motor proporsional dengan tegangan armature pada fluks medan konstan (di bawah kecepatan dasar). Kontrol medan (field weakening): kecepatan dapat ditingkatkan di atas kecepatan dasar dengan mengurangi arus medan (fluks medan) pada tegangan armature konstan, tetapi torsi berkurang." },
{ "en": "Apa itu 'chopper' (pemangkas) DC?", "id": "Konverter DC-DC (biasanya step-down atau step-up) yang digunakan untuk mengontrol tegangan rata-rata yang diberikan ke beban DC, seperti motor DC, dengan cara menyalakan dan mematikan saklar semikonduktor pada frekuensi tinggi (menggunakan PWM)." },
{ "en": "Jelaskan metode kontrol kecepatan motor induksi AC (misalnya, V/f control, field-oriented control).", "id": "V/f control (Voltage/frequency control): menjaga rasio tegangan terhadap frekuensi konstan untuk mempertahankan fluks medan magnet stator kira-kira konstan, cocok untuk aplikasi sederhana. Field-Oriented Control (FOC) atau Vector Control: metode canggih yang mengontrol komponen arus stator secara independen untuk mengontrol fluks dan torsi secara terpisah (mirip kontrol motor DC), menghasilkan kinerja dinamis tinggi." },
{ "en": "Apa itu 'sensorless control' pada motor drive?", "id": "Teknik mengontrol motor (kecepatan atau posisi) tanpa menggunakan sensor mekanis terpisah (seperti encoder atau resolver) pada poros motor. Sebaliknya, parameter listrik motor (arus, tegangan, GGL balik) diukur dan diestimasi untuk menentukan keadaan motor." },
{ "en": "Apa yang dimaksud dengan 'regenerative braking' pada motor drive?", "id": "Proses di mana motor listrik bertindak sebagai generator selama deselerasi atau pengereman, mengubah energi kinetik beban kembali menjadi energi listrik yang dapat dikembalikan ke sumber daya (misalnya baterai atau jala-jala) atau didisipasikan dalam resistor pengereman." },
{ "en": "Apa itu 'Solid State Relay' (SSR)?", "id": "Jenis relai elektronik yang menggunakan komponen semikonduktor (seperti thyristor, TRIAC, atau MOSFET) untuk melakukan fungsi switching, tanpa bagian mekanis bergerak seperti pada relai elektromagnetik. Menawarkan kecepatan switching lebih tinggi, masa pakai lebih lama, dan operasi senyap." },
{ "en": "Apa perbedaan antara 'MOSFET driver' dan 'IGBT driver'?", "id": "Keduanya adalah rangkaian yang dirancang untuk menggerakkan (mengaktifkan dan menonaktifkan) gate MOSFET atau IGBT. Perbedaannya mungkin terletak pada level tegangan gate yang optimal, kemampuan arus gate drive, dan fitur proteksi spesifik yang disesuaikan dengan karakteristik masing-masing perangkat." },
{ "en": "Mengapa diperlukan 'gate drive circuit' untuk MOSFET atau IGBT daya?", "id": "Karena gate MOSFET/IGBT bersifat kapasitif, memerlukan arus yang cukup besar untuk mengisi dan mengosongkan kapasitansi gate dengan cepat agar switching cepat dan rugi-rugi switching minimal. Gate driver menyediakan arus puncak ini dan level tegangan yang sesuai, serta seringkali isolasi dan proteksi." },
{ "en": "Apa itu 'dead time' (waktu mati) dalam rangkaian half-bridge atau full-bridge inverter dan mengapa diperlukan?", "id": "Periode waktu singkat di mana kedua saklar dalam satu kaki bridge (atas dan bawah) sengaja dimatikan sebelum saklar komplementernya dihidupkan. Diperlukan untuk mencegah kondisi 'shoot-through' (hubungan singkat langsung antara suplai positif dan negatif) akibat waktu tunda turn-off dan turn-on yang tidak ideal pada saklar." },
{ "en": "Apa itu 'bootstrap power supply' untuk gate driver high-side?", "id": "Teknik untuk menyediakan suplai tegangan mengambang (floating supply) yang diperlukan untuk menggerakkan gate saklar sisi atas (high-side switch, misalnya N-channel MOSFET) dalam konfigurasi bridge, menggunakan dioda dan kapasitor yang diisi saat saklar sisi bawah ON." },
{ "en": "Apa yang dimaksud dengan 'EMI filter' pada input catu daya?", "id": "Filter pasif (terdiri dari induktor dan kapasitor) yang ditempatkan pada jalur input AC atau DC catu daya untuk menekan noise frekuensi tinggi (EMI) yang dihasilkan oleh catu daya itu sendiri (terutama SMPS) agar tidak merambat kembali ke jala-jala atau sumber, dan juga untuk menekan noise dari jala-jala agar tidak masuk ke catu daya." },
{ "en": "Sebutkan standar internasional yang berkaitan dengan EMC dan keselamatan produk elektronik.", "id": "EMC: Seri IEC 61000 (misalnya IEC 61000-4-2 untuk ESD, IEC 61000-4-4 untuk EFT), CISPR (misalnya CISPR 22/32 untuk emisi ITE), FCC Part 15 (AS). Keselamatan: Seri IEC 60950 (ITE), IEC 62368 (ITE dan AV), IEC 60601 (peralatan medis)." },
{ "en": "Apa itu 'isolation transformer' (transformator isolasi)?", "id": "Transformator yang lilitan primer dan sekundernya terisolasi secara elektrik (tidak ada koneksi DC langsung), biasanya dengan rasio lilitan 1:1. Digunakan untuk menyediakan isolasi galvanik, mengurangi noise common-mode, dan meningkatkan keselamatan." },
{ "en": "Apa yang dimaksud dengan 'creepage' dan 'clearance' dalam desain keselamatan PCB dan peralatan listrik?", "id": "Creepage adalah jarak terpendek antara dua bagian konduktif yang diukur sepanjang permukaan isolator. Clearance adalah jarak terpendek antara dua bagian konduktif yang diukur melalui udara (garis lurus). Keduanya penting untuk mencegah busur api (arcing) dan tracking akibat tegangan tinggi." },
{ "en": "Apa itu 'insulation class' (kelas isolasi) pada peralatan listrik?", "id": "Klasifikasi yang menunjukkan tingkat proteksi terhadap sengatan listrik yang disediakan oleh konstruksi isolasi peralatan. Contoh: Kelas I (memiliki koneksi ground protektif), Kelas II (double insulated, tidak memerlukan ground protektif)." },
{ "en": "Apa itu 'Ingress Protection' (IP) rating?", "id": "Sistem kode standar internasional (IEC 60529) yang mengklasifikasikan dan menilai tingkat proteksi yang disediakan oleh selungkup (enclosure) peralatan listrik terhadap intrusi benda padat (debu, jari) dan cairan (air)." },
{ "en": "Apa yang dimaksud dengan 'intrinsically safe' (IS) equipment?", "id": "Peralatan dan perkabelan yang dirancang untuk tidak mampu melepaskan energi listrik atau termal yang cukup, baik dalam kondisi normal maupun kondisi gangguan tertentu, untuk menyebabkan penyalaan campuran atmosfer berbahaya (misalnya gas atau debu yang mudah terbakar)." },
{ "en": "Apa itu 'explosion-proof' (Ex d) enclosure?", "id": "Jenis selungkup untuk peralatan listrik di area berbahaya yang dirancang untuk menahan ledakan internal dari campuran gas/uap yang mudah terbakar yang mungkin masuk ke dalamnya, dan mencegah penyebaran nyala api internal ke atmosfer luar." },
{ "en": "Apa itu 'ATEX directive' dan 'IECEx scheme'?", "id": "ATEX adalah dua arahan Uni Eropa yang mengatur peralatan dan sistem proteksi yang dimaksudkan untuk digunakan di atmosfer yang berpotensi meledak. IECEx adalah skema sertifikasi internasional untuk peralatan yang digunakan di lokasi berbahaya (Ex areas)." },
{ "en": "Apa yang dimaksud dengan 'RoHS' (Restriction of Hazardous Substances) directive?", "id": "Arahan Uni Eropa yang membatasi penggunaan zat berbahaya tertentu (seperti timbal, merkuri, kadmium, kromium heksavalen, PBB, PBDE) dalam pembuatan berbagai jenis peralatan listrik dan elektronik." },
{ "en": "Apa itu 'WEEE' (Waste Electrical and Electronic Equipment) directive?", "id": "Arahan Uni Eropa yang bertujuan untuk mengurangi jumlah limbah peralatan listrik dan elektronik yang dibuang dan untuk mendorong pengumpulan, penggunaan kembali, daur ulang, dan pemulihan limbah tersebut." },
{ "en": "Apa itu 'REACH' (Registration, Evaluation, Authorisation and Restriction of Chemicals) regulation?", "id": "Regulasi Uni Eropa yang bertujuan untuk meningkatkan perlindungan kesehatan manusia dan lingkungan dari risiko yang dapat ditimbulkan oleh bahan kimia, sambil meningkatkan daya saing industri kimia UE." },
{ "en": "Jelaskan perbedaan antara 'verification' dan 'validation' dalam proses pengembangan produk.", "id": "Verification (Are we building the product right?): Memastikan bahwa produk yang dikembangkan memenuhi semua spesifikasi dan persyaratan desain yang ditetapkan. Validation (Are we building theright product?): Memastikan bahwa produk yang dikembangkan memenuhi kebutuhan pengguna yang dimaksudkan dan tujuan bisnis." },
{ "en": "Apa itu 'model-based design' (MBD)?", "id": "Pendekatan pengembangan sistem di mana model (matematis dan visual) dari sistem digunakan secara ekstensif sepanjang proses desain, analisis, simulasi, pembuatan kode otomatis, dan verifikasi." },
{ "en": "Apa itu 'hardware-in-the-loop' (HIL) simulation?", "id": "Teknik pengujian dan validasi sistem kontrol tertanam (embedded control system) di mana beberapa komponen dari loop kontrol adalah nyata (perangkat keras, seperti ECU) dan sisanya disimulasikan (lingkungan, plant model)." },
{ "en": "Apa itu 'software-in-the-loop' (SIL) simulation?", "id": "Teknik pengujian di mana kode perangkat lunak kontrol (misalnya, yang akan dijalankan pada ECU) diuji dalam lingkungan simulasi murni di komputer host, sebelum di-deploy ke perangkat keras target." },
{ "en": "Apa itu 'rapid prototyping' dalam pengembangan sistem kontrol?", "id": "Proses membuat prototipe fungsional dari sistem kontrol dengan cepat, seringkali menggunakan alat desain berbasis model dan platform perangkat keras yang dapat dikonfigurasi ulang (misalnya, dSPACE, NI CompactRIO), untuk iterasi desain dan validasi awal." },
{ "en": "Apa itu 'digital twin'?", "id": "Representasi virtual dari objek, proses, atau sistem fisik yang diperbarui secara dinamis dengan data dari kembaran fisiknya. Digunakan untuk analisis, simulasi, prediksi, dan optimalisasi kinerja." },
{ "en": "Apa yang dimaksud dengan 'artificial neural network' (ANN) atau jaringan saraf tiruan?", "id": "Model komputasi yang terinspirasi oleh struktur dan fungsi jaringan saraf biologis, terdiri dari unit pemrosesan sederhana yang saling terhubung (neuron atau node) yang dapat belajar dari data untuk melakukan tugas seperti klasifikasi atau regresi." },
{ "en": "Apa itu 'deep learning'?", "id": "Sub-bidang machine learning yang menggunakan jaringan saraf tiruan dengan banyak lapisan (deep neural networks) untuk mempelajari representasi fitur yang kompleks secara hierarkis dari data mentah. Sangat sukses dalam aplikasi seperti pengenalan gambar dan pemrosesan bahasa alami." },
{ "en": "Apa itu 'convolutional neural network' (CNN atau ConvNet)?", "id": "Jenis jaringan saraf tiruan deep learning yang sangat efektif untuk menganalisis data visual (gambar). Menggunakan lapisan konvolusi untuk mengekstrak fitur spasial hierarkis, lapisan pooling untuk reduksi dimensi, dan lapisan fully connected untuk klasifikasi." },
{ "en": "Apa itu 'recurrent neural network' (RNN)?", "id": "Jenis jaringan saraf tiruan yang dirancang untuk memproses data sekuensial (misalnya, teks, deret waktu, ucapan) dengan memiliki koneksi umpan balik (recurrent connections) yang memungkinkan informasi dari langkah waktu sebelumnya mempengaruhi pemrosesan saat ini, memberikan 'memori' pada jaringan." },
{ "en": "Apa itu 'Long Short-Term Memory' (LSTM) network?", "id": "Jenis arsitektur RNN canggih yang dirancang untuk mengatasi masalah vanishing gradient pada RNN tradisional, memungkinkannya belajar dependensi jangka panjang dalam data sekuensial. Menggunakan unit memori khusus dengan mekanisme gating." },
{ "en": "Apa itu 'transformer model' dalam deep learning?", "id": "Arsitektur jaringan saraf yang mengandalkan mekanisme 'self-attention' untuk memproses data sekuensial secara paralel, sangat sukses dalam tugas pemrosesan bahasa alami (NLP) seperti terjemahan mesin dan generasi teks. Contoh: BERT, GPT." },
{ "en": "Apa itu 'reinforcement learning' (pembelajaran penguatan)?", "id": "Area machine learning di mana agen (agent) belajar untuk membuat urutan keputusan (aksi) dalam suatu lingkungan (environment) untuk memaksimalkan imbalan (reward) kumulatif. Agen belajar melalui trial and error, tanpa data latih berlabel eksplisit." },
{ "en": "Apa itu 'edge AI' atau 'AI at the edge'?", "id": "Implementasi dan eksekusi algoritma kecerdasan buatan (AI) langsung pada perangkat keras lokal (perangkat edge, sensor, mikrokontroler) daripada di cloud atau pusat data terpusat. Bertujuan untuk mengurangi latensi, penggunaan bandwidth, dan meningkatkan privasi." },
{ "en": "Apa tantangan utama dalam mengimplementasikan AI di perangkat edge?", "id": "Keterbatasan sumber daya komputasi (daya pemrosesan, memori), konsumsi daya rendah, optimasi model AI agar efisien (model compression, quantization), dan keamanan." },
{ "en": "Apa itu 'neuromorphic engineering' atau 'neuromorphic computing'?", "id": "Pendekatan untuk merancang sirkuit komputer dan arsitektur yang meniru struktur dan fungsi sistem saraf biologis (otak). Bertujuan untuk menciptakan sistem AI yang lebih efisien secara energi dan mampu belajar secara adaptif, sering menggunakan event-based processing (spiking neural networks)." },
{ "en": "Apa itu 'spiking neural network' (SNN)?", "id": "Generasi ketiga dari model jaringan saraf tiruan yang lebih mendekati model biologis, di mana neuron berkomunikasi melalui pulsa diskrit (spike) yang bergantung waktu, bukan nilai kontinu. Berpotensi untuk efisiensi energi yang lebih tinggi pada perangkat keras neuromorfik." },
{ "en": "Apa itu 'analog AI' atau 'in-memory computing' untuk AI?", "id": "Pendekatan untuk melakukan komputasi AI (terutama operasi matriks-vektor dalam jaringan saraf) langsung di dalam perangkat memori analog (seperti memori resistif non-volatil, RRAM, PCM) dengan memanfaatkan hukum fisika (misalnya Hukum Ohm dan Kirchhoff), berpotensi mengurangi perpindahan data dan meningkatkan efisiensi energi secara drastis." },
{ "en": "Apa itu 'quantum machine learning'?", "id": "Bidang interdisipliner yang mengeksplorasi penggunaan prinsip-prinsip komputasi kuantum untuk meningkatkan algoritma machine learning atau mengembangkan algoritma ML baru yang memanfaatkan fenomena kuantum, berpotensi untuk percepatan komputasi atau kemampuan pemodelan yang lebih baik." },
// AWAL DARI BATCH SOAL BARU FINAL (Lanjutan untuk 89 tambahan - FOKUS MATERI DASAR)
{ "en": "Apa satuan dasar untuk mengukur arus listrik?", "id": "Ampere (A)." },
{ "en": "Apa satuan dasar untuk mengukur tegangan listrik?", "id": "Volt (V)." },
{ "en": "Apa satuan dasar untuk mengukur resistansi listrik?", "id": "Ohm ($\\Omega$)." },
{ "en": "Apa hukum yang menghubungkan tegangan, arus, dan resistansi dalam sebuah rangkaian?", "id": "Hukum Ohm ($V = IR$)." },
{ "en": "Alat apa yang digunakan untuk mengukur arus listrik?", "id": "Amperemeter (atau multimeter pada mode amperemeter)." },
{ "en": "Alat apa yang digunakan untuk mengukur tegangan listrik?", "id": "Voltmeter (atau multimeter pada mode voltmeter)." },
{ "en": "Alat apa yang digunakan untuk mengukur resistansi?", "id": "Ohmmeter (atau multimeter pada mode ohmmeter)." },
{ "en": "Apa yang dimaksud dengan rangkaian seri?", "id": "Rangkaian di mana komponen-komponen dihubungkan satu demi satu sehingga hanya ada satu jalur bagi arus untuk mengalir." },
{ "en": "Apa yang dimaksud dengan rangkaian paralel?", "id": "Rangkaian di mana komponen-komponen dihubungkan berdampingan sehingga arus terbagi ke beberapa jalur." },
{ "en": "Bagaimana cara menghitung total resistansi pada resistor yang dihubungkan seri?", "id": "$R_{total} = R_1 + R_2 + R_3 + \dots$" },
{ "en": "Bagaimana cara menghitung total resistansi pada resistor yang dihubungkan paralel?", "id": "$1/R_{total} = 1/R_1 + 1/R_2 + 1/R_3 + \dots$" },
{ "en": "Apa fungsi utama dari sebuah resistor dalam rangkaian?", "id": "Membatasi aliran arus listrik dan/atau menghasilkan penurunan tegangan." },
{ "en": "Apa fungsi utama dari sebuah kapasitor dalam rangkaian?", "id": "Menyimpan muatan listrik atau energi medan listrik, memblokir arus DC, dan melewatkan arus AC." },
{ "en": "Apa satuan dasar untuk kapasitansi?", "id": "Farad (F)." },
{ "en": "Apa fungsi utama dari sebuah induktor dalam rangkaian?", "id": "Menyimpan energi dalam bentuk medan magnet, menentang perubahan arus, melewatkan arus DC, dan menahan arus AC frekuensi tinggi." },
{ "en": "Apa satuan dasar untuk induktansi?", "id": "Henry (H)." },
{ "en": "Apa yang dimaksud dengan arus searah (DC)?", "id": "Arus listrik yang mengalir hanya dalam satu arah." },
{ "en": "Apa yang dimaksud dengan arus bolak-balik (AC)?", "id": "Arus listrik yang arah alirannya berubah-ubah secara periodik." },
{ "en": "Apa sumber umum dari arus DC?", "id": "Baterai, sel surya, adaptor AC-DC." },
{ "en": "Apa sumber umum dari arus AC?", "id": "Generator di pembangkit listrik, stop kontak PLN." },
{ "en": "Apa yang dimaksud dengan frekuensi pada arus AC?", "id": "Jumlah siklus gelombang AC yang terjadi dalam satu detik, diukur dalam Hertz (Hz)." },
{ "en": "Apa fungsi dari dioda semikonduktor?", "id": "Memungkinkan arus listrik mengalir hanya dalam satu arah (sebagai penyearah) dan memblokir arus dari arah sebaliknya." },
{ "en": "Apa dua terminal pada dioda?", "id": "Anoda dan Katoda." },
{ "en": "Ke arah mana arus konvensional mengalir melalui dioda saat dibias maju?", "id": "Dari Anoda ke Katoda." },
{ "en": "Apa fungsi utama transistor?", "id": "Sebagai saklar elektronik atau sebagai penguat sinyal listrik." },
{ "en": "Sebutkan dua jenis utama transistor bipolar junction (BJT).", "id": "NPN dan PNP." },
{ "en": "Sebutkan tiga terminal pada transistor BJT.", "id": "Basis (Base), Kolektor (Collector), dan Emitor (Emitter)." },
{ "en": "Apa fungsi dari Integrated Circuit (IC) atau chip?", "id": "Sebuah komponen elektronik yang terdiri dari ribuan atau jutaan transistor, resistor, dan kapasitor yang terintegrasi dalam satu keping kecil semikonduktor, membentuk fungsi rangkaian yang kompleks." },
{ "en": "Apa yang dimaksud dengan 'ground' atau 'tanah' dalam rangkaian listrik?", "id": "Titik referensi umum dalam rangkaian listrik yang dianggap memiliki potensial nol volt." },
{ "en": "Apa fungsi dari sekring (fuse) dalam rangkaian listrik?", "id": "Melindungi rangkaian dari arus berlebih dengan cara melebur dan memutus rangkaian jika arus melebihi batas amannya." },
{ "en": "Apa perbedaan antara konduktor dan isolator?", "id": "Konduktor adalah bahan yang mudah menghantarkan arus listrik (misalnya tembaga, perak). Isolator adalah bahan yang sangat sulit menghantarkan arus listrik (misalnya karet, plastik, kaca)." },
{ "en": "Apa itu semikonduktor?", "id": "Bahan yang memiliki konduktivitas listrik antara konduktor dan isolator, dan konduktivitasnya dapat diubah dengan menambahkan impuritas (doping) atau dengan kondisi eksternal (suhu, cahaya)." },
{ "en": "Sebutkan dua jenis doping pada semikonduktor.", "id": "Tipe-N (kelebihan elektron) dan Tipe-P (kelebihan hole/lubang)." },
{ "en": "Apa yang dimaksud dengan 'daya listrik' dan apa satuannya?", "id": "Laju di mana energi listrik ditransfer atau dikonsumsi dalam rangkaian, diukur dalam Watt (W)." },
{ "en": "Bagaimana rumus dasar untuk menghitung daya listrik jika tegangan dan arus diketahui?", "id": "$P = VI$ (Daya = Tegangan x Arus)." },
{ "en": "Apa yang dimaksud dengan 'transformator' atau 'trafo'?", "id": "Komponen pasif yang mentransfer energi listrik dari satu rangkaian ke rangkaian lain melalui induksi elektromagnetik, biasanya digunakan untuk menaikkan atau menurunkan tegangan AC." },
{ "en": "Apa dua jenis lilitan utama pada transformator?", "id": "Lilitan primer (input) dan lilitan sekunder (output)." },
{ "en": "Apa yang dimaksud dengan 'relay'?", "id": "Saklar yang dioperasikan secara elektromagnetik, di mana arus kecil pada kumparan dapat mengontrol saklar untuk menghubungkan atau memutuskan rangkaian lain yang mungkin berarus lebih besar." },
{ "en": "Apa fungsi LED (Light Emitting Diode)?", "id": "Jenis dioda yang memancarkan cahaya ketika dialiri arus listrik pada bias maju." },
{ "en": "Mengapa LED biasanya memerlukan resistor seri?", "id": "Untuk membatasi arus yang mengalir melaluinya agar tidak melebihi batas maksimum dan mencegah kerusakan." },
{ "en": "Apa itu potensiometer?", "id": "Resistor variabel tiga terminal yang berfungsi sebagai pembagi tegangan yang dapat diatur." },
{ "en": "Apa perbedaan antara potensiometer dan rheostat?", "id": "Potensiometer adalah resistor variabel tiga terminal yang biasanya digunakan untuk mengontrol tegangan. Rheostat adalah resistor variabel dua terminal (atau tiga terminal dengan satu tidak digunakan/terhubung ke wiper) yang biasanya digunakan untuk mengontrol arus." },
{ "en": "Apa fungsi dari saklar (switch) dalam rangkaian?", "id": "Untuk membuka atau menutup jalur aliran arus listrik dalam rangkaian." },
{ "en": "Apa yang dimaksud dengan 'rangkaian terbuka' (open circuit)?", "id": "Kondisi di mana jalur aliran arus terputus sehingga tidak ada arus yang dapat mengalir." },
{ "en": "Apa yang dimaksud dengan 'hubungan singkat' (short circuit)?", "id": "Kondisi di mana terdapat jalur beresistansi sangat rendah yang memungkinkan arus yang sangat besar mengalir, seringkali menyebabkan kerusakan." },
{ "en": "Apa itu Printed Circuit Board (PCB) atau Papan Sirkuit Cetak?", "id": "Papan yang digunakan untuk menghubungkan komponen elektronik secara fisik dan elektrik menggunakan jalur konduktif (trace) yang dicetak pada bahan non-konduktif." },
{ "en": "Apa fungsi dari multimeter?", "id": "Alat ukur elektronik serbaguna yang dapat mengukur beberapa besaran listrik seperti tegangan (volt), arus (ampere), dan resistansi (ohm)." },
{ "en": "Apa itu osiloskop?", "id": "Alat ukur elektronik yang menampilkan bentuk gelombang sinyal listrik (tegangan terhadap waktu) secara visual." },
{ "en": "Apa yang dimaksud dengan 'sinyal analog'?", "id": "Sinyal yang amplitudonya dapat bervariasi secara kontinu dalam rentang tertentu." },
{ "en": "Apa yang dimaksud dengan 'sinyal digital'?", "id": "Sinyal yang amplitudonya hanya dapat memiliki nilai diskrit tertentu (biasanya dua level, yaitu HIGH dan LOW, atau 1 dan 0)." },
{ "en": "Apa itu gerbang logika (logic gate)?", "id": "Blok bangunan dasar dari sirkuit digital yang melakukan operasi logika Boolean dasar pada satu atau lebih input biner untuk menghasilkan satu output biner." },
{ "en": "Sebutkan tiga gerbang logika dasar.", "id": "AND, OR, dan NOT." },
{ "en": "Apa output dari gerbang AND jika kedua inputnya HIGH?", "id": "HIGH (1)." },
{ "en": "Apa output dari gerbang OR jika salah satu inputnya HIGH?", "id": "HIGH (1)." },
{ "en": "Apa output dari gerbang NOT jika inputnya HIGH?", "id": "LOW (0)." },
{ "en": "Apa itu breadboard (project board)?", "id": "Papan konstruksi tanpa solder yang digunakan untuk membuat prototipe rangkaian elektronik sementara." },
{ "en": "Apa fungsi dari baterai?", "id": "Menyimpan energi kimia dan mengubahnya menjadi energi listrik sebagai sumber tegangan DC." },
{ "en": "Apa yang dimaksud dengan 'kapasitas baterai' (misalnya dalam mAh)?", "id": "Jumlah muatan yang dapat disimpan oleh baterai, menunjukkan berapa lama baterai dapat menyuplai arus tertentu sebelum habis. (milliampere-hour)." },
{ "en": "Apa perbedaan antara sel primer dan sel sekunder pada baterai?", "id": "Sel primer adalah baterai sekali pakai yang tidak dapat diisi ulang. Sel sekunder adalah baterai yang dapat diisi ulang." },
{ "en": "Apa itu LDR (Light Dependent Resistor)?", "id": "Jenis resistor yang nilai resistansinya berubah tergantung pada intensitas cahaya yang diterimanya." },
{ "en": "Apa itu termistor?", "id": "Jenis resistor yang nilai resistansinya berubah secara signifikan terhadap perubahan suhu. Ada NTC (Negative Temperature Coefficient) dan PTC (Positive Temperature Coefficient)." },
{ "en": "Apa fungsi dari mikrofon?", "id": "Mengubah gelombang suara (energi akustik) menjadi sinyal listrik (energi listrik)." },
{ "en": "Apa fungsi dari loudspeaker (speaker)?", "id": "Mengubah sinyal listrik (energi listrik) menjadi gelombang suara (energi akustik)." },
{ "en": "Apa itu 'filter' dalam elektronika?", "id": "Rangkaian yang dirancang untuk melewatkan sinyal pada rentang frekuensi tertentu (passband) dan menolak atau melemahkan sinyal pada frekuensi lain (stopband)." },
{ "en": "Sebutkan jenis filter dasar berdasarkan frekuensi yang dilewatkan.", "id": "Low-pass filter (LPF), High-pass filter (HPF), Band-pass filter (BPF), Band-stop filter (BSF) atau Notch filter." },
{ "en": "Apa fungsi dari low-pass filter (LPF)?", "id": "Melewatkan sinyal frekuensi rendah dan menolak atau melemahkan sinyal frekuensi tinggi." },
{ "en": "Apa fungsi dari high-pass filter (HPF)?", "id": "Melewatkan sinyal frekuensi tinggi dan menolak atau melemahkan sinyal frekuensi rendah." },
{ "en": "Apa yang dimaksud dengan 'frekuensi cut-off' pada filter?", "id": "Frekuensi di mana daya output filter turun menjadi setengah dari daya di passband (atau tegangan turun sekitar 70.7% atau -3dB)." },
{ "en": "Apa itu solder (timah solder)?", "id": "Logam paduan yang dapat meleleh (biasanya timah dan timbal, atau bebas timbal) yang digunakan untuk membuat sambungan listrik permanen antara komponen elektronik dan PCB." },
{ "en": "Alat apa yang digunakan untuk melelehkan solder?", "id": "Solder iron (besi solder atau patri)." },
{ "en": "Apa itu 'fluks' (flux) dalam proses penyolderan?", "id": "Bahan kimia yang digunakan untuk membersihkan permukaan logam dari oksidasi dan membantu aliran solder yang lebih baik saat penyolderan." },
{ "en": "Apa yang dimaksud dengan 'tegangan efektif' (RMS voltage) pada AC?", "id": "Nilai tegangan DC ekuivalen yang akan menghasilkan disipasi daya yang sama pada resistor seperti yang dihasilkan oleh tegangan AC tersebut." },
{ "en": "Apa hubungan antara tegangan puncak (peak voltage) dan tegangan RMS untuk gelombang sinus murni?", "id": "$V_{RMS} = V_{peak} / \sqrt{2}$ (atau sekitar $0.707 \times V_{peak}$)." },
{ "en": "Apa itu 'reaktansi kapasitif' ($X_C$)?", "id": "Hambatan yang ditawarkan oleh kapasitor terhadap aliran arus AC, nilainya berbanding terbalik dengan frekuensi dan kapasitansi ($X_C = 1 / (2\pi fC)$)." },
{ "en": "Apa itu 'reaktansi induktif' ($X_L$)?", "id": "Hambatan yang ditawarkan oleh induktor terhadap aliran arus AC, nilainya sebanding dengan frekuensi dan induktansi ($X_L = 2\pi fL$)." },
{ "en": "Apa itu 'impedansi' (Z) dalam rangkaian AC?", "id": "Hambatan total terhadap aliran arus AC dalam rangkaian yang mungkin mengandung resistor, kapasitor, dan induktor. Merupakan kombinasi vektor dari resistansi dan reaktansi." },
{ "en": "Apa satuan dari reaktansi dan impedansi?", "id": "Ohm ($\\Omega$)." },
{ "en": "Bagaimana perilaku kapasitor terhadap arus DC setelah terisi penuh?", "id": "Bertindak sebagai rangkaian terbuka (memblokir aliran arus DC lebih lanjut)." },
{ "en": "Bagaimana perilaku induktor terhadap arus DC setelah beberapa saat arus mengalir stabil?", "id": "Bertindak sebagai hubungan singkat (resistansi sangat rendah terhadap arus DC)." },
{ "en": "Apa itu 'arus bocor' (leakage current) pada komponen elektronik?", "id": "Arus kecil yang tidak diinginkan yang mengalir melalui jalur isolasi atau melalui komponen saat seharusnya dalam kondisi OFF." },
{ "en": "Apa fungsi dari penyearah jembatan (bridge rectifier)?", "id": "Mengubah seluruh siklus gelombang AC (positif dan negatif) menjadi gelombang DC berdenyut dengan polaritas tunggal, menggunakan empat dioda." },
{ "en": "Apa itu 'penguat operasional' (operational amplifier atau Op-Amp)?", "id": "Penguat diferensial tegangan DC-coupled dengan gain sangat tinggi, impedansi input tinggi, dan impedansi output rendah, biasanya digunakan dengan umpan balik eksternal untuk berbagai aplikasi pengolahan sinyal analog." },
{ "en": "Sebutkan dua input utama pada Op-Amp.", "id": "Input non-inverting (+) dan input inverting (-)." },
{ "en": "Apa yang dimaksud dengan 'gain loop terbuka' (open-loop gain) pada Op-Amp?", "id": "Penguatan Op-Amp tanpa adanya umpan balik eksternal, nilainya sangat besar." },
{ "en": "Apa itu 'common-mode signal' pada input diferensial Op-Amp?", "id": "Sinyal yang muncul dengan amplitudo dan fasa yang sama pada kedua input (non-inverting dan inverting) Op-Amp secara bersamaan." },
{ "en": "Apa itu 'differential-mode signal' pada input diferensial Op-Amp?", "id": "Perbedaan sinyal antara input non-inverting dan input inverting Op-Amp." },
{ "en": "Apa fungsi dari 'voltage follower' (buffer) menggunakan Op-Amp?", "id": "Menghasilkan tegangan output yang sama dengan tegangan input (gain = 1) dengan impedansi input sangat tinggi dan impedansi output sangat rendah, digunakan untuk isolasi atau matching impedansi." },
{ "en": "Apa itu 'osilator'?", "id": "Rangkaian elektronik yang menghasilkan sinyal periodik berulang (misalnya gelombang sinus atau kotak) tanpa memerlukan sinyal input eksternal." },
{ "en": "Syarat apa yang harus dipenuhi agar rangkaian osilator dapat berosilasi (Kriteria Barkhausen)?", "id": "Penguatan loop total harus sama dengan satu (atau sedikit lebih besar dari satu untuk memulai osilasi) dan pergeseran fasa total dalam loop harus 0 atau kelipatan $360^\\circ$." },
{ "en": "Apa itu mikrokontroler (MCU)?", "id": "Sebuah komputer kecil dalam satu chip IC yang berisi CPU, memori (RAM, Flash/ROM), dan periferal input/output yang dapat diprogram." },
{ "en": "Apa perbedaan mendasar antara mikroprosesor dan mikrokontroler?", "id": "Mikroprosesor hanya berisi CPU dan memerlukan komponen eksternal (memori, I/O). Mikrokontroler mengintegrasikan CPU, memori, dan I/O dalam satu chip." },
{ "en": "Apa itu 'Arduino'?", "id": "Platform elektronika open-source yang terdiri dari papan mikrokontroler (hardware) dan lingkungan pengembangan perangkat lunak (IDE) untuk memprogram mikrokontroler tersebut." }
// AKHIR DARI BATCH SOAL BARU FINAL INI

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
            }, 4000);
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
