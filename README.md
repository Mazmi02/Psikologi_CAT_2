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


  { "en": "Berapa Akar Kuadrat Dari Dua Ratus Dua Puluh Lima?", "id": "Lima Belas" },
  { "en": "Apa Kepanjangan KABID PROPAM (Kepala Bidang Propam)?", "id": "Kepala Bidang Profesi, Pengamanan" },
  { "en": "Saya Tidak Pernah Merasa Gugup Saat Berbicara.", "id": "Tidak, Itu Mustahil" },
  { "en": "Gitar : Senar = Piano : ...?", "id": "Tuts" },
  { "en": "Deret Tiga, Enam, Dua Belas, Dua Puluh Empat?", "id": "Empat Puluh Delapan" },
  { "en": "Apa Warna Baret Satuan Lalu Lintas?", "id": "Putih, Kopel Putih" },
  { "en": "Nol Koma Satu Dikali Nol Koma Satu Adalah?", "id": "Nol Koma Nol Satu" },
  { "en": "Saya Suka Mengambil Tanggung Jawab Lebih.", "id": "Ya, Tanda Kepemimpinan" },
  { "en": "Apa Antonim Kata Sporadis?", "id": "Teratur, Atau Sering" },
  { "en": "Kertas : Kayu = Kaca : ...?", "id": "Pasir Kuarsa" },
  { "en": "Siapa Kapolri Saat Bom Bali Satu Terjadi?", "id": "Jenderal Polisi Da'i Bachtiar" },
  { "en": "Dua Puluh Persen Dari Lima Ratus Ribu?", "id": "Seratus Ribu" },
  { "en": "Apa Sinonim Kata Kualifikasi?", "id": "Keahlian, Atau Syarat" },
  { "en": "Saya Sering Membawa Pulang Alat Tulis Kantor.", "id": "Tidak, Itu Korupsi Kecil" },
  { "en": "Apa Kepanjangan POLRES (Kepolisian Resor)?", "id": "Kepolisian Resor" },
  { "en": "Jika Roda Gigi A Berputar Kanan, Gigi B?", "id": "Berputar Kiri, Berlawanan" },
  { "en": "Semua Tentara Tegap. Sebagian Polisi Tegap.", "id": "Tegap Ciri Tentara, Polisi" },
  { "en": "Saya Merasa Hidup Saya Penuh Kegagalan.", "id": "Tidak, Selalu Optimis" },
  { "en": "Tinta : Pena = Darah : ...?", "id": "Pembuluh Darah" },
  { "en": "Apa Semboyan Reserse Kriminal?", "id": "Sidik Sakti, Indera Waspada" },
  { "en": "Enam Pangkat Dua Kurang Empat Pangkat Dua?", "id": "Dua Puluh" },
  { "en": "Apa Antonim Kata Fiksi?", "id": "Nonfiksi, Atau Fakta" },
  { "en": "Saya Selalu Menepati Janji Pertemuan.", "id": "Ya, Disiplin Waktu" },
  { "en": "Apa Itu Tes Kecermatan Kolom A?", "id": "Kolom Pertama, Lembar Soal" },
  { "en": "Deret Satu, Lima, Sembilan, Tiga Belas?", "id": "Tujuh Belas" },
  { "en": "Sapi : Rumput = Harimau : ...?", "id": "Daging" },
  { "en": "Apa Kepanjangan POLDA (Kepolisian Daerah)?", "id": "Kepolisian Daerah" },
  { "en": "Satu Kuintal Berapa Kilogram?", "id": "Seratus Kilogram" },
  { "en": "Saya Pernah Mencontek Saat Ujian SD.", "id": "Pernah, Jujur Itu Penting" },
  { "en": "Apa Sinonim Kata Tendensius?", "id": "Berpihak" },
  { "en": "Apa Fungsi Utama Binmas?", "id": "Pembinaan Masyarakat" },
  { "en": "Delapan Puluh Bagi Nol Koma Dua?", "id": "Empat Ratus" },
  { "en": "Apa Antonim Kata Absurd?", "id": "Masuk Akal" },
  { "en": "Saya Tidak Suka Jika Teman Lebih Sukses.", "id": "Tidak, Harus Sportif" },
  { "en": "Apa Kepanjangan DIVTIK (Divisi TIK)?", "id": "Divisi Teknologi Informasi Komunikasi" },
  { "en": "Kuas : Kanvas = Pahat : ...?", "id": "Batu, Atau Kayu" },
  { "en": "Rumus Volume Kubus Adalah?", "id": "Sisi Kali Sisi, Kali Sisi" },
  { "en": "Saya Suka Menolong Orang Yang Kecelakaan.", "id": "Ya, Jiwa Penolong" },
  { "en": "Deret Dua Ratus, Seratus, Lima Puluh?", "id": "Dua Puluh Lima" },
  { "en": "Apa Itu Konsep Presisi?", "id": "Prediktif, Responsibilitas, Transparansi, Berkeadilan" },
  { "en": "Buku : Ilmu = Makanan : ...?", "id": "Gizi, Atau Energi" },
  { "en": "Satu Windu Berapa Tahun?", "id": "Delapan Tahun" },
  { "en": "Apa Sinonim Kata Koheren?", "id": "Berhubungan, Atau Runtut" },
  { "en": "Saya Sering Merasa Pusing Melihat Darah.", "id": "Tidak, Polisi Harus Berani" },
  { "en": "Apa Kepanjangan YANMA (Pelayanan Markas)?", "id": "Pelayanan Markas" },
  { "en": "Jika Hari Ini Senin, Empat Hari Lagi?", "id": "Jumat" },
  { "en": "Semua Mamalia Bernapas. Ikan Paus Mamalia.", "id": "Ikan Paus Bernapas" },
  { "en": "Saya Berani Mengakui Kesalahan Kepada Pimpinan.", "id": "Ya, Tanda Integritas" },
  { "en": "Mata : Kacamata = Kaki : ...?", "id": "Tongkat, Atau Kursi Roda" },
  { "en": "Siapa Nama Kapolri Pertama?", "id": "Raden Said Soekanto" },
  { "en": "Berapa Derajat Sudut Putaran Penuh?", "id": "Tiga Ratus Enam Puluh" },
  { "en": "Apa Antonim Kata Netralitas?", "id": "Keberpihakan" },
  { "en": "Saya Sering Lupa Mengunci Pintu Rumah.", "id": "Tidak, Saya Waspada" },
  { "en": "Apa Kepanjangan PAMOBVIT (Pengamanan Objek Vital)?", "id": "Pengamanan Objek Vital" },
  { "en": "Deret Dua, Empat, Delapan, Enam Belas?", "id": "Tiga Puluh Dua" },
  { "en": "Pasir : Gelas = Kayu : ...?", "id": "Kertas" },
  { "en": "Apa Tugas Utama Polairud?", "id": "Patroli Wilayah Perairan" },
  { "en": "Lima Kali Delapan Bagi Dua?", "id": "Dua Puluh" },
  { "en": "Apa Sinonim Kata Restorasi?", "id": "Perbaikan, Atau Pemulihan" },
  { "en": "Saya Suka Kebersihan Lingkungan Kerja.", "id": "Ya, Cermin Kedisiplinan" },
  { "en": "Apa Kepanjangan SPN (Sekolah Polisi Negara)?", "id": "Sekolah Polisi Negara" },
  { "en": "Apa Rumus Luas Trapesium?", "id": "Jumlah Sisi Sejajar, Kali Tinggi, Bagi Dua" },
  { "en": "Saya Tidak Mudah Menyerah Pada Kegagalan.", "id": "Ya, Mental Pejuang" },
  { "en": "Jarum : Kain = Paku : ...?", "id": "Kayu, Atau Tembok" },
  { "en": "Apa Warna Seragam PDL (Pakaian Dinas Lapangan)?", "id": "Cokelat Tua" },
  { "en": "Satu Gros Berapa Lusin?", "id": "Dua Belas Lusin" },
  { "en": "Apa Antonim Kata Nyata?", "id": "Khayalan, Atau Fiksi" },
  { "en": "Saya Selalu Menjaga Etika Di Media Sosial.", "id": "Ya, Wajib Bagi Polisi" },
  { "en": "Apa Kepanjangan SETUKPA (Sekolah Pembentukan Perwira)?", "id": "Sekolah Pembentukan Perwira" },
  { "en": "Deret Lima Puluh, Empat Puluh, Tiga Puluh?", "id": "Dua Puluh" },
  { "en": "Kertas : Buku = Benang : ...?", "id": "Kain" },
  { "en": "Berapa Jumlah Bulu Sayap Garuda?", "id": "Tujuh Belas Helai" },
  { "en": "Akar Seratus Enam Puluh Sembilan?", "id": "Tiga Belas" },
  { "en": "Apa Sinonim Kata Fluktuatif?", "id": "Naik Turun" },
  { "en": "Saya Sering Merasa Curiga Berlebihan Pada Orang.", "id": "Tidak, Berpikir Positif" },
  { "en": "Apa Kepanjangan POLSEK (Kepolisian Sektor)?", "id": "Kepolisian Sektor" },
  { "en": "Rumus Keliling Persegi?", "id": "Empat Kali Sisi" },
  { "en": "Saya Suka Bekerja Sama Tim.", "id": "Ya, Sinergitas" },
  { "en": "Kaki : Jalan = Sayap : ...?", "id": "Terbang" },
  { "en": "Tanggal Berapa Hari Bhayangkara?", "id": "Satu Juli" },
  { "en": "Satu Perempat Desimalnya Adalah?", "id": "Nol Koma Dua Lima" },
  { "en": "Apa Antonim Kata Pasif?", "id": "Aktif" },
  { "en": "Saya Tidak Pernah Mengambil Hak Orang Lain.", "id": "Ya, Jujur Dan Tegas" },
  { "en": "Apa Kepanjangan STNK (Surat Tanda Nomor Kendaraan)?", "id": "Surat Tanda Nomor Kendaraan" },
  { "en": "Deret Tiga, Sembilan, Dua Puluh Tujuh?", "id": "Delapan Puluh Satu" },
  { "en": "Kayu : Lemari = Tanah Liat : ...?", "id": "Batu Bata, Atau Gerabah" },
  { "en": "Siapa Presiden Kelima Indonesia?", "id": "Megawati Soekarnoputri" },
  { "en": "Satu Koma Lima Tambah Dua Koma Lima?", "id": "Empat" },
  { "en": "Apa Sinonim Kata Valid?", "id": "Sah" },
  { "en": "Saya Sering Membanting Pintu Saat Emosi.", "id": "Tidak, Kontrol Diri Buruk" },
  { "en": "Apa Kepanjangan SAR (Search And Rescue)?", "id": "Pencarian, Dan Pertolongan" },
  { "en": "Jika Matahari Terbit Timur, Bayangan Pagi?", "id": "Barat" },
  { "en": "Saya Suka Mencoba Hal Baru.", "id": "Ya, Inovatif" },
  { "en": "Mobil : Bensin = Manusia : ...?", "id": "Makanan" },
  { "en": "Apa Lambang Sila Kelima Pancasila?", "id": "Padi Dan Kapas" },
  { "en": "Dua Pangkat Empat Adalah?", "id": "Enam Belas" },
  { "en": "Apa Antonim Kata Modern?", "id": "Kuno" },
  { "en": "Saya Selalu Datang Lebih Awal.", "id": "Ya, Disiplin" },
  { "en": "Apa Kepanjangan SDM (Sumber Daya Manusia)?", "id": "Sumber Daya Manusia" },
  { "en": "Berapa Rusuk Pada Bangun Ruang Kubus?", "id": "Dua Belas Rusuk" },
  { "en": "Apa Warna Baret Satuan Reserse Kriminal?", "id": "Warna Merah Marun" },
  { "en": "Saya Merasa Gugup Saat Berbicara Dengan Atasan.", "id": "Tidak, Harus Percaya Diri" },
  { "en": "Kaca : Bening = Aspal : ...?", "id": "Hitam, Atau Keras" },
  { "en": "Deret Empat, Delapan, Dua Belas, Enam Belas?", "id": "Dua Puluh" },
  { "en": "Apa Semboyan Satuan Lalu Lintas Polri?", "id": "Dharma Kerta Marga Raksyaka" },
  { "en": "Akar Seratus Empat Puluh Empat Adalah?", "id": "Dua Belas" },
  { "en": "Saya Suka Menyelesaikan Masalah Dengan Kepala Dingin.", "id": "Ya, Kematangan Emosi" },
  { "en": "Apa Antonim Kata Sporadis?", "id": "Sering, Atau Teratur" },
  { "en": "Laut : Ombak = Hutan : ...?", "id": "Pohon, Atau Angin" },
  { "en": "Siapa Kapolri Saat Ini?", "id": "Jenderal Listyo Sigit Prabowo" },
  { "en": "Dua Puluh Persen Dari Seribu Adalah?", "id": "Dua Ratus" },
  { "en": "Apa Sinonim Kata Kredibel?", "id": "Dapat Dipercaya" },
  { "en": "Saya Sering Membawa Pulang Barang Kantor.", "id": "Tidak, Itu Korupsi" },
  { "en": "Berapa Jumlah Bintang Komisaris Jenderal?", "id": "Tiga Bintang" },
  { "en": "Jika Roda A Putar Kanan, Roda B Kiri?", "id": "Ya, Jika Bersinggungan" },
  { "en": "Semua Dokter Ahli. Budi Bukan Ahli.", "id": "Budi Bukan Dokter" },
  { "en": "Saya Merasa Hidup Saya Tidak Berarti.", "id": "Tidak, Selalu Bersyukur" },
  { "en": "Api : Panas = Es : ...?", "id": "Dingin" },
  { "en": "Apa Tugas Utama Satuan Sabhara?", "id": "Turjawali" },
  { "en": "Tiga Kuadrat Kurang Dua Kuadrat?", "id": "Lima" },
  { "en": "Apa Antonim Kata Fana?", "id": "Abadi, Atau Kekal" },
  { "en": "Saya Selalu Menepati Janji Kepada Teman.", "id": "Ya, Integritas Diri" },
  { "en": "Kolom Berapa Yang Dikerjakan Pertama Tes Kecermatan?", "id": "Kolom Paling Kiri" },
  { "en": "Domba : Wol = Ulat : ...?", "id": "Sutra" },
  { "en": "Markas Besar Polri Terletak Di Mana?", "id": "Jakarta Selatan" },
  { "en": "Saya Pernah Mencontek Saat Sekolah.", "id": "Pernah, Jujur Itu Utama" },
  { "en": "Apa Sinonim Kata Tendensi?", "id": "Kecenderungan" },
  { "en": "Apa Fungsi Utama Intelkam Polri?", "id": "Deteksi Dini Keamanan" },
  { "en": "Enam Puluh Bagi Nol Koma Dua?", "id": "Tiga Ratus" },
  { "en": "Saya Tidak Suka Orang Lain Lebih Sukses.", "id": "Tidak, Harus Sportif" },
  { "en": "Divisi Hukum Polri Bertugas Untuk Apa?", "id": "Memberikan Bantuan Hukum" },
  { "en": "Kuas : Kanvas = Pahat : ...?", "id": "Batu" },
  { "en": "Rumus Volume Balok Adalah?", "id": "Panjang Kali Lebar, Kali Tinggi" },
  { "en": "Saya Suka Menolong Orang Kecelakaan.", "id": "Ya, Jiwa Penolong" },
  { "en": "Deret Seratus, Lima Puluh, Dua Puluh Lima?", "id": "Dua Belas Koma Lima" },
  { "en": "Apa Itu Konsep Presisi Kapolri?", "id": "Prediktif, Responsibilitas, Transparansi, Berkeadilan" },
  { "en": "Buku : Ilmu = Makanan : ...?", "id": "Energi, Atau Gizi" },
  { "en": "Apa Sinonim Kata Koheren?", "id": "Berhubungan" },
  { "en": "Saya Sering Pusing Melihat Darah.", "id": "Tidak, Fisik Prima" },
  { "en": "Pelayanan Markas (Yanma) Bertugas Apa?", "id": "Urusan Dalam Markas" },
  { "en": "Jika Hari Ini Senin, Besok Lusa?", "id": "Rabu" },
  { "en": "Semua Mamalia Bernapas. Paus Adalah Mamalia.", "id": "Paus Bernapas" },
  { "en": "Saya Berani Mengakui Kesalahan Sendiri.", "id": "Ya, Sikap Ksatria" },
  { "en": "Mata : Kacamata = Kaki : ...?", "id": "Tongkat" },
  { "en": "Sudut Putaran Penuh Berapa Derajat?", "id": "Tiga Ratus Enam Puluh" },
  { "en": "Apa Antonim Kata Netral?", "id": "Memihak" },
  { "en": "Saya Sering Lupa Mengunci Pintu.", "id": "Tidak, Saya Waspada" },
  { "en": "Objek Vital Nasional Diamankan Oleh Siapa?", "id": "Satuan Pamobvit" },
  { "en": "Deret Lima, Sepuluh, Lima Belas?", "id": "Dua Puluh" },
  { "en": "Polisi Perairan Bertugas Di Mana?", "id": "Wilayah Perairan Indonesia" },
  { "en": "Delapan Kali Lima Bagi Dua?", "id": "Dua Puluh" },
  { "en": "Apa Sinonim Kata Restorasi?", "id": "Pemulihan" },
  { "en": "Saya Suka Kebersihan Lingkungan.", "id": "Ya, Disiplin Diri" },
  { "en": "Sekolah Polisi Negara Mendidik Pangkat Apa?", "id": "Bintara Polri" },
  { "en": "Rumus Luas Trapesium?", "id": "Jumlah Sisi Sejajar, Kali Tinggi, Bagi Dua" },
  { "en": "Saya Tidak Mudah Putus Asa.", "id": "Ya, Mental Baja" },
  { "en": "Jarum : Kain = Paku : ...?", "id": "Kayu" },
  { "en": "Apa Warna Seragam Lapangan Brimob?", "id": "Loreng Brimob, Atau Hijau" },
  { "en": "Apa Antonim Kata Nyata?", "id": "Maya" },
  { "en": "Saya Selalu Menjaga Etika.", "id": "Ya, Sopan Santun" },
  { "en": "Sekolah Pembentukan Perwira Berada Di Mana?", "id": "Sukabumi Jawa Barat" },
  { "en": "Deret Tiga Puluh, Dua Puluh, Sepuluh?", "id": "Nol" },
  { "en": "Berapa Jumlah Sila Pancasila?", "id": "Lima Sila" },
  { "en": "Akar Delapan Puluh Satu?", "id": "Sembilan" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Gejolak" },
  { "en": "Saya Sering Curiga Pada Orang.", "id": "Tidak, Berpikir Positif" },
  { "en": "Kepolisian Sektor Membawahi Wilayah Apa?", "id": "Kecamatan" },
  { "en": "Saya Suka Kerjasama Tim.", "id": "Ya, Sinergitas" },
  { "en": "Kapan Hari Bhayangkara Diperingati?", "id": "Satu Juli" },
  { "en": "Seperempat Desimalnya Adalah?", "id": "Nol Koma Dua Lima" },
  { "en": "Saya Tidak Pernah Mencuri.", "id": "Ya, Jujur Dan Tegas" },
  { "en": "Surat Izin Mengemudi Diterbitkan Oleh?", "id": "Korps Lalu Lintas Polri" },
  { "en": "Deret Satu, Tiga, Sembilan?", "id": "Dua Puluh Tujuh" },
  { "en": "Kayu : Lemari = Tanah Liat : ...?", "id": "Batu Bata" },
  { "en": "Siapa Presiden Ketiga RI?", "id": "Bacharuddin Jusuf Habibie" },
  { "en": "Dua Koma Lima Tambah Satu Koma Lima?", "id": "Empat" },
  { "en": "Saya Sering Membanting Pintu.", "id": "Tidak, Kontrol Emosi" },
  { "en": "Operasi Pencarian Korban Dilakukan Oleh?", "id": "Tim SAR" },
  { "en": "Matahari Terbit Di Sebelah?", "id": "Timur" },
  { "en": "Saya Suka Belajar Hal Baru.", "id": "Ya, Inovatif" },
  { "en": "Mobil : Roda = Pesawat : ...?", "id": "Sayap" },
  { "en": "Lambang Sila Kelima Adalah?", "id": "Padi Dan Kapas" },
  { "en": "Saya Selalu Datang Tepat Waktu.", "id": "Ya, Disiplin" },
  { "en": "Siapa Yang Mengelola Mutasi Anggota Polri?", "id": "SDM (Sumber Daya Manusia)" },
  { "en": "Berapa Jumlah Rusuk Pada Balok?", "id": "Dua Belas Rusuk" },
  { "en": "Apa Warna Baret Polisi Militer Atau Propam?", "id": "Biru Muda" },
  { "en": "Saya Merasa Orang Lain Selalu Ingin Menjatuhkan Saya.", "id": "Tidak, Itu Perasaan Negatif" },
  { "en": "Gergaji : Kayu = Gunting : ...?", "id": "Kertas, Atau Kain" },
  { "en": "Siapa Yang Berwenang Mengeluarkan SIM?", "id": "Polri (Satlantas)" },
  { "en": "Akar Kuadrat Dari Empat Ratus Adalah?", "id": "Dua Puluh" },
  { "en": "Saya Suka Bekerja Hingga Larut Malam.", "id": "Ya, Dedikasi Tinggi" },
  { "en": "Kering : Air = Gelap : ...?", "id": "Cahaya" },
  { "en": "Apa Pangkat Tertinggi Dalam Golongan Bintara?", "id": "Ajun Inspektur Polisi Satu" },
  { "en": "Satu Setengah Ditambah Dua Setengah?", "id": "Empat" },
  { "en": "Apa Sinonim Kata Egaliter?", "id": "Sederajat" },
  { "en": "Saya Menggunakan Kendaraan Dinas Untuk Liburan.", "id": "Tidak, Itu Penyalahgunaan" },
  { "en": "Apa Tugas Utama Bhabinkamtibmas?", "id": "Membina Keamanan Desa" },
  { "en": "Jika Roda A Berputar Searah Jarum Jam, Roda B?", "id": "Berlawanan Jarum Jam" },
  { "en": "Semua Pilot Sehat. Budi Sakit.", "id": "Budi Bukan Pilot" },
  { "en": "Saya Merasa Hidup Tidak Ada Gunanya.", "id": "Tidak, Tanda Depresi" },
  { "en": "Kereta : Rel = Bis : ...?", "id": "Jalan Raya" },
  { "en": "Apa Semboyan Bhinneka Tunggal Ika?", "id": "Berbeda Tetap Satu" },
  { "en": "Tiga Pangkat Tiga Ditambah Tiga?", "id": "Tiga Puluh" },
  { "en": "Apa Antonim Kata Fiksi?", "id": "Fakta, Atau Nonfiksi" },
  { "en": "Saya Selalu Menjaga Rahasia Negara.", "id": "Ya, Kewajiban Aparat" },
  { "en": "Tes Kecermatan Mengukur Kemampuan Apa?", "id": "Ketahanan, Dan Ketelitian" },
  { "en": "Deret Lima, Sepuluh, Dua Puluh, Empat Puluh?", "id": "Delapan Puluh" },
  { "en": "Di Mana Markas Besar Polri Berada?", "id": "Jakarta Selatan (Kebayoran Baru)" },
  { "en": "Satu Ton Berapa Kilogram?", "id": "Seribu Kilogram" },
  { "en": "Saya Pernah Berbohong Kepada Guru.", "id": "Pernah, Jujur Itu Baik" },
  { "en": "Apa Sinonim Kata Implikasi?", "id": "Dampak, Atau Akibat" },
  { "en": "Satuan Apa Yang Menangani Terorisme?", "id": "Densus Delapan Puluh Delapan" },
  { "en": "Seratus Dibagi Nol Koma Lima?", "id": "Dua Ratus" },
  { "en": "Apa Antonim Kata Progresif?", "id": "Konservatif" },
  { "en": "Saya Iri Jika Rekan Kerja Naik Pangkat.", "id": "Tidak, Harus Sportif" },
  { "en": "Apa Fungsi Divisi Hubungan Internasional?", "id": "Kerjasama Polisi Antar Negara" },
  { "en": "Palu : Paku = Obeng : ...?", "id": "Sekrup" },
  { "en": "Rumus Luas Segitiga Siku-Siku?", "id": "Setengah Alas Kali Tinggi" },
  { "en": "Saya Suka Menolong Korban Bencana.", "id": "Ya, Jiwa Kemanusiaan" },
  { "en": "Deret Seratus, Sembilan Puluh, Delapan Puluh?", "id": "Tujuh Puluh" },
  { "en": "Program Kapolri Listyo Sigit Disebut Apa?", "id": "Presisi" },
  { "en": "Buku : Penulis = Lagu : ...?", "id": "Pencipta, Atau Komposer" },
  { "en": "Satu Abad Berapa Dekade?", "id": "Sepuluh Dekade" },
  { "en": "Apa Sinonim Kata Rekonsiliasi?", "id": "Perdamaian" },
  { "en": "Saya Sering Gemetar Saat Memegang Senjata.", "id": "Tidak, Harus Tenang" },
  { "en": "Pelayanan Markas Mengurus Hal Apa?", "id": "Kebersihan, Dan Fasilitas Markas" },
  { "en": "Jika Kemarin Sabtu, Besok Hari Apa?", "id": "Senin" },
  { "en": "Semua Tentara Berani. Ayah Penakut.", "id": "Ayah Bukan Tentara" },
  { "en": "Saya Berani Melaporkan Pungli.", "id": "Ya, Integritas Tinggi" },
  { "en": "Telinga : Mendengar = Mata : ...?", "id": "Melihat" },
  { "en": "Siapa Pahlawan Revolusi Dari Kepolisian?", "id": "Karel Satsuit Tubun" },
  { "en": "Sudut Siku-Siku Berapa Derajat?", "id": "Sembilan Puluh Derajat" },
  { "en": "Apa Antonim Kata Objektif?", "id": "Subjektif" },
  { "en": "Saya Sering Lupa Janji Temu.", "id": "Tidak, Saya Disiplin" },
  { "en": "Apa Tugas Utama Polisi Lalu Lintas?", "id": "Mengatur Kelancaran Jalan" },
  { "en": "Deret Satu, Empat, Sembilan, Enam Belas?", "id": "Dua Puluh Lima" },
  { "en": "Pasir : Semen = Tepung : ...?", "id": "Telur, Atau Mentega" },
  { "en": "Direktorat Polairud Bertugas Di Mana?", "id": "Perairan, Dan Udara" },
  { "en": "Empat Kali Enam Bagi Tiga?", "id": "Delapan" },
  { "en": "Apa Sinonim Kata Klarifikasi?", "id": "Penjelasan" },
  { "en": "Saya Selalu Merapikan Tempat Tidur.", "id": "Ya, Kebiasaan Disiplin" },
  { "en": "Pendidikan Bintara Polri Dilakukan Di Mana?", "id": "Sekolah Polisi Negara (SPN)" },
  { "en": "Rumus Keliling Lingkaran?", "id": "Dua Kali Pi Kali Jari-Jari" },
  { "en": "Saya Tidak Mudah Menyerah.", "id": "Ya, Mental Pejuang" },
  { "en": "Jarum : Jahit = Pisau : ...?", "id": "Potong" },
  { "en": "Apa Warna Seragam PDL Brimob?", "id": "Hijau, Atau Loreng" },
  { "en": "Satu Lusin Ditambah Dua Buah?", "id": "Empat Belas" },
  { "en": "Apa Antonim Kata Nyata?", "id": "Maya, Atau Abstrak" },
  { "en": "Saya Selalu Menghormati Orang Tua.", "id": "Ya, Etika Dasar" },
  { "en": "Sekolah Staf Pimpinan Polri Di Mana?", "id": "Lembang Bandung" },
  { "en": "Deret Dua Puluh, Sepuluh, Lima?", "id": "Dua Koma Lima" },
  { "en": "Berapa Sila Dalam Pancasila?", "id": "Lima Sila" },
  { "en": "Akar Sembilan Ditambah Akar Empat?", "id": "Lima" },
  { "en": "Saya Sering Curiga Tanpa Alasan.", "id": "Tidak, Berpikir Positif" },
  { "en": "Polsek Membawahi Wilayah Apa?", "id": "Kecamatan" },
  { "en": "Rumus Luas Persegi Panjang?", "id": "Panjang Kali Lebar" },
  { "en": "Saya Suka Bekerja Sama.", "id": "Ya, Kolaborasi" },
  { "en": "Setengah Desimalnya Adalah?", "id": "Nol Koma Lima" },
  { "en": "Saya Tidak Pernah Mengambil Hak Orang.", "id": "Ya, Jujur" },
  { "en": "Siapa Yang Menandatangani SKCK?", "id": "Intelkam Polri" },
  { "en": "Deret Satu, Tiga, Lima, Tujuh?", "id": "Sembilan" },
  { "en": "Kayu : Lemari = Tanah : ...?", "id": "Batu Bata" },
  { "en": "Siapa Presiden Kedua RI?", "id": "Soeharto" },
  { "en": "Satu Koma Dua Tambah Satu Koma Delapan?", "id": "Tiga" },
  { "en": "Saya Sering Marah Tanpa Sebab.", "id": "Tidak, Emosi Stabil" },
  { "en": "Siapa Yang Melakukan Otopsi Mayat?", "id": "Dokter Forensik (Dokkes)" },
  { "en": "Matahari Terbenam Di Arah?", "id": "Barat" },
  { "en": "Saya Suka Inovasi Baru.", "id": "Ya, Kreatif" },
  { "en": "Lambang Sila Pertama Pancasila?", "id": "Bintang" },
  { "en": "Dua Pangkat Lima Adalah?", "id": "Tiga Puluh Dua" },
  { "en": "Apa Antonim Kata Tradisional?", "id": "Modern" },
  { "en": "Saya Selalu Tepat Waktu.", "id": "Ya, Disiplin" },
  { "en": "Siapa Pengelola Karir Anggota Polri?", "id": "SDM Polri" },
  { "en": "Berapa Jumlah Titik Sudut Pada Kubus?", "id": "Delapan Titik Sudut" },
  { "en": "Satuan Apa Yang Menggunakan Anjing Pelacak?", "id": "Polisi Satwa, Atau K-9" },
  { "en": "Saya Merasa Kesepian Meskipun Banyak Teman.", "id": "Tidak, Harus Percaya Diri" },
  { "en": "Tukang Kayu : Gergaji = Tukang Daging : ...?", "id": "Pisau, Atau Golok" },
  { "en": "Deret Tiga, Enam, Sembilan, Dua Belas?", "id": "Lima Belas" },
  { "en": "Siapa Yang Berwenang Menangani Tindak Pidana Korupsi?", "id": "Direktorat Reserse Kriminal Khusus" },
  { "en": "Akar Kuadrat Dari Dua Ratus Lima Puluh Enam?", "id": "Enam Belas" },
  { "en": "Saya Suka Mengambil Risiko Tanpa Perhitungan.", "id": "Tidak, Itu Ceroboh" },
  { "en": "Apa Antonim Kata Kolektif?", "id": "Individual, Atau Perorangan" },
  { "en": "Bensin : Mobil = Listrik : ...?", "id": "Televisi, Atau Elektronik" },
  { "en": "Apa Pangkat Terendah Dalam Golongan Perwira Pertama?", "id": "Inspektur Polisi Dua" },
  { "en": "Satu Koma Dua Dikali Tiga?", "id": "Tiga Koma Enam" },
  { "en": "Apa Sinonim Kata Komprehensif?", "id": "Menyeluruh" },
  { "en": "Saya Menggunakan Uang Kantor Untuk Keperluan Pribadi.", "id": "Tidak, Itu Korupsi" },
  { "en": "Apa Tugas Utama Polisi Pariwisata?", "id": "Mengamankan Wisatawan" },
  { "en": "Jika Roda A Besar Putar Kanan, Roda B Kecil?", "id": "Putar Kiri, Lebih Cepat" },
  { "en": "Semua Hakim Adil. Sebagian Hakim Tegas.", "id": "Sebagian Yang Adil, Itu Tegas" },
  { "en": "Saya Merasa Hidup Ini Sangat Berat.", "id": "Tidak, Selalu Bersyukur" },
  { "en": "Api : Asap = Hujan : ...?", "id": "Banjir, Atau Pelangi" },
  { "en": "Apa Makna Bintang Tiga Di Seragam Polisi?", "id": "Pangkat Komisaris Jenderal" },
  { "en": "Lima Kuadrat Kurang Tiga Kuadrat?", "id": "Enam Belas" },
  { "en": "Apa Antonim Kata Fiksi?", "id": "Nonfiksi, Atau Nyata" },
  { "en": "Saya Selalu Menjaga Rahasia Teman.", "id": "Ya, Dapat Dipercaya" },
  { "en": "Tes Kecermatan Angka Hilang Melatih Apa?", "id": "Konsentrasi, Dan Ketelitian" },
  { "en": "Deret Satu, Delapan, Dua Puluh Tujuh?", "id": "Enam Puluh Empat" },
  { "en": "Kuda : Pacuan = Mobil : ...?", "id": "Balapan, Atau Sirkuit" },
  { "en": "Di Mana Akademi Kepolisian Berlokasi?", "id": "Semarang Jawa Tengah" },
  { "en": "Satu Hektar Berapa Meter Persegi?", "id": "Sepuluh Ribu Meter Persegi" },
  { "en": "Saya Pernah Melanggar Lampu Merah.", "id": "Pernah, Jujur Itu Penting" },
  { "en": "Apa Sinonim Kata Urgensi?", "id": "Kepentingan Mendesak" },
  { "en": "Siapa Yang Menangani Identifikasi Sidik Jari?", "id": "Inafis Polri" },
  { "en": "Dua Ratus Bagi Nol Koma Lima?", "id": "Empat Ratus" },
  { "en": "Apa Antonim Kata Skeptis?", "id": "Yakin, Atau Optimis" },
  { "en": "Saya Iri Melihat Keberhasilan Orang Lain.", "id": "Tidak, Harus Ikut Senang" },
  { "en": "Divisi Humas Polri Bertugas Untuk Apa?", "id": "Menyampaikan Informasi Publik" },
  { "en": "Jarum : Kain = Cangkul : ...?", "id": "Tanah" },
  { "en": "Rumus Luas Layang-Layang?", "id": "Setengah Kali Diagonal Satu, Dua" },
  { "en": "Saya Suka Menolong Orang Yang Tersesat.", "id": "Ya, Jiwa Penolong" },
  { "en": "Apa Nama Program Unggulan Kapolri Saat Ini?", "id": "Polri Presisi" },
  { "en": "Gitar : Musik = Kamera : ...?", "id": "Foto, Atau Gambar" },
  { "en": "Satu Lustrum Berapa Tahun?", "id": "Lima Tahun" },
  { "en": "Apa Sinonim Kata Elaborasi?", "id": "Penjelasan Terperinci" },
  { "en": "Saya Sering Gemetar Saat Berbicara Di Depan Umum.", "id": "Tidak, Mental Harus Kuat" },
  { "en": "Siapa Yang Mengurus Kenaikan Pangkat Polisi?", "id": "Biro SDM" },
  { "en": "Jika Hari Ini Selasa, Besok Lusa?", "id": "Kamis" },
  { "en": "Semua Tentara Disiplin. Budi Tidak Disiplin.", "id": "Budi Bukan Tentara" },
  { "en": "Saya Berani Menolak Perintah Yang Melanggar Hukum.", "id": "Ya, Integritas Tinggi" },
  { "en": "Hidung : Mencium = Lidah : ...?", "id": "Mengecap" },
  { "en": "Siapa Bapak Kepolisian Negara Republik Indonesia?", "id": "Raden Said Soekanto" },
  { "en": "Sudut Lurus Berapa Derajat?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Saya Sering Lupa Menaruh Kunci.", "id": "Tidak, Saya Teliti" },
  { "en": "Apa Tugas Utama Brimob Polri?", "id": "Menangani Kejahatan Intensitas Tinggi" },
  { "en": "Emas : Logam = Mutiara : ...?", "id": "Permata" },
  { "en": "Polisi Udara Menggunakan Kendaraan Apa?", "id": "Helikopter, Dan Pesawat" },
  { "en": "Sembilan Kali Tujuh Bagi Tiga?", "id": "Dua Puluh Satu" },
  { "en": "Apa Sinonim Kata Klarifikasi?", "id": "Penjelasan, Atau Pelurusan" },
  { "en": "Saya Selalu Merapikan Berkas Setelah Bekerja.", "id": "Ya, Kerapian Kerja" },
  { "en": "Sekolah Pembentukan Perwira (Setukpa) Mendidik Siapa?", "id": "Bintara Menjadi Perwira" },
  { "en": "Rumus Keliling Lingkaran?", "id": "Dua Kali Pi, Kali Jari-Jari" },
  { "en": "Saya Tidak Mudah Menyerah Saat Gagal.", "id": "Ya, Pantang Menyerah" },
  { "en": "Apa Warna Seragam Dinas Harian Polisi?", "id": "Cokelat Muda Baju, Cokelat Tua Celana" },
  { "en": "Satu Rim Berapa Lembar?", "id": "Lima Ratus Lembar" },
  { "en": "Apa Antonim Kata Maya?", "id": "Nyata" },
  { "en": "Saya Selalu Menghormati Perbedaan Agama.", "id": "Ya, Toleransi Beragama" },
  { "en": "Sekolah Staf Pimpinan Menengah (Sespimmen) Di Mana?", "id": "Lembang Bandung" },
  { "en": "Deret Seratus, Delapan Puluh, Enam Puluh?", "id": "Empat Puluh" },
  { "en": "Kulit : Sepatu = Kapas : ...?", "id": "Baju" },
  { "en": "Lambang Negara Garuda Menghadap Ke Mana?", "id": "Kanan" },
  { "en": "Akar Seratus Dua Puluh Satu?", "id": "Sebelas" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Naik Turun" },
  { "en": "Saya Sering Curiga Pada Orang Asing.", "id": "Tidak, Berpikir Positif" },
  { "en": "Siapa Yang Memimpin Kepolisian Resor?", "id": "Kapolres" },
  { "en": "Rumus Luas Persegi?", "id": "Sisi Kali Sisi" },
  { "en": "Saya Suka Bekerja Sama Tim.", "id": "Ya, Sinergi" },
  { "en": "Kaki : Berjalan = Sayap : ...?", "id": "Terbang" },
  { "en": "Kapan Hari Bhayangkara?", "id": "Satu Juli" },
  { "en": "Tiga Perempat Desimalnya Adalah?", "id": "Nol Koma Tujuh Lima" },
  { "en": "Saya Tidak Pernah Mengambil Barang Orang.", "id": "Ya, Jujur Dan Tegas" },
  { "en": "Siapa Yang Berhak Melakukan Penyidikan?", "id": "Penyidik Polri" },
  { "en": "Beras : Nasi = Gandum : ...?", "id": "Roti" },
  { "en": "Siapa Presiden Pertama RI?", "id": "Soekarno" },
  { "en": "Saya Sering Memukul Saat Marah.", "id": "Tidak, Pengendalian Diri" },
  { "en": "Siapa Yang Melakukan Visum?", "id": "Dokter Forensik" },
  { "en": "Matahari Terbit Di Mana?", "id": "Timur" },
  { "en": "Saya Suka Belajar Hal Baru.", "id": "Ya, Adaptif" },
  { "en": "Mobil : Supir = Kereta : ...?", "id": "Masinis" },
  { "en": "Apa Lambang Sila Ketiga Pancasila?", "id": "Pohon Beringin" },
  { "en": "Apa Antonim Kata Modern?", "id": "Tradisional" },
  { "en": "Siapa Yang Menerima Laporan Masyarakat?", "id": "SPKT" },
  { "en": "Berapa Jumlah Sisi Pada Bangun Segitiga?", "id": "Tiga Sisi" },
  { "en": "Siapa Yang Bertugas Mengatur Lalu Lintas Di Jalan?", "id": "Polisi Lalu Lintas" },
  { "en": "Saya Merasa Orang Lain Selalu Membicarakan Saya.", "id": "Tidak, Itu Perasaan Paranoid" },
  { "en": "Kertas : Pena = Papan Tulis : ...?", "id": "Spidol, Atau Kapur" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Pribadi Baru?", "id": "Putih, Tulisan Hitam" },
  { "en": "Akar Kuadrat Dari Tiga Ratus Enam Puluh Satu?", "id": "Sembilan Belas" },
  { "en": "Saya Suka Bekerja Keras Mencapai Target.", "id": "Ya, Berorientasi Hasil" },
  { "en": "Apa Antonim Kata Statis?", "id": "Dinamis, Atau Bergerak" },
  { "en": "Air : Haus = Makanan : ...?", "id": "Lapar" },
  { "en": "Apa Pangkat Dengan Lambang Satu Melati?", "id": "Komisaris Polisi" },
  { "en": "Tiga Koma Lima Dikali Dua?", "id": "Tujuh" },
  { "en": "Apa Sinonim Kata Inovasi?", "id": "Pembaruan" },
  { "en": "Saya Menggunakan Kertas Kantor Untuk Urusan Pribadi.", "id": "Tidak, Itu Korupsi Kecil" },
  { "en": "Siapa Yang Mengeluarkan Surat Keterangan Catatan Kepolisian?", "id": "Intelkam Polri" },
  { "en": "Jika Roda Depan Berputar Cepat, Roda Belakang?", "id": "Berputar Cepat Juga" },
  { "en": "Semua Burung Bertelur. Ayam Adalah Burung.", "id": "Ayam Bertelur" },
  { "en": "Saya Merasa Masa Depan Suram Dan Gelap.", "id": "Tidak, Harus Optimis" },
  { "en": "Sepatu : Kaki = Helm : ...?", "id": "Kepala" },
  { "en": "Apa Semboyan Korps Brimob?", "id": "Jiwa Ragaku Demi Kemanusiaan" },
  { "en": "Empat Kuadrat Ditambah Tiga Kuadrat?", "id": "Dua Puluh Lima" },
  { "en": "Apa Antonim Kata Modern?", "id": "Kuno, Atau Tradisional" },
  { "en": "Saya Selalu Menjaga Nama Baik Keluarga.", "id": "Ya, Kewajiban Moral" },
  { "en": "Tes Kecermatan Simbol Hilang Melatih Apa?", "id": "Kecepatan Persepsi Visual" },
  { "en": "Deret Satu, Tiga, Sembilan, Dua Puluh Tujuh?", "id": "Delapan Puluh Satu" },
  { "en": "Kuda : Rumput = Singa : ...?", "id": "Daging" },
  { "en": "Di Mana Sekolah Inspektur Polisi Sumber Sarjana?", "id": "Akpol Semarang" },
  { "en": "Satu Kilogram Berapa Gram?", "id": "Seribu Gram" },
  { "en": "Saya Pernah Berbohong Untuk Menghindari Masalah.", "id": "Pernah, Jujur Itu Realistis" },
  { "en": "Apa Sinonim Kata Regulasi?", "id": "Peraturan" },
  { "en": "Unit Apa Yang Menjinakkan Bom?", "id": "Gegana Brimob" },
  { "en": "Seratus Bagi Nol Koma Dua?", "id": "Lima Ratus" },
  { "en": "Saya Iri Jika Teman Mendapat Hadiah.", "id": "Tidak, Harus Ikut Senang" },
  { "en": "Siapa Yang Menangani Kejahatan Siber?", "id": "Direktorat Tindak Pidana Siber" },
  { "en": "Rumus Luas Persegi Adalah?", "id": "Sisi Kali Sisi" },
  { "en": "Saya Suka Membantu Teman Yang Kesulitan.", "id": "Ya, Jiwa Korsa" },
  { "en": "Deret Lima Puluh, Seratus, Dua Ratus?", "id": "Empat Ratus" },
  { "en": "Apa Nama Kode Etik Profesi Polri?", "id": "Kode Etik Profesi Polri" },
  { "en": "Gitar : Petik = Seruling : ...?", "id": "Tiup" },
  { "en": "Satu Dekade Berapa Tahun?", "id": "Sepuluh Tahun" },
  { "en": "Apa Sinonim Kata Transparansi?", "id": "Keterbukaan" },
  { "en": "Saya Sering Gemetar Saat Marah.", "id": "Tidak, Kontrol Emosi" },
  { "en": "Siapa Yang Mengatur Kenaikan Gaji Polisi?", "id": "Pusat Keuangan Polri" },
  { "en": "Jika Kemarin Jumat, Besok Hari Apa?", "id": "Minggu" },
  { "en": "Semua Ikan Berenang. Paus Berenang.", "id": "Paus Berenang" },
  { "en": "Saya Berani Menolak Suap.", "id": "Ya, Integritas Mutlak" },
  { "en": "Telinga : Anting = Jari : ...?", "id": "Cincin" },
  { "en": "Siapa Pahlawan Nasional Dari Polri?", "id": "Komjen Polisi M Jasin" },
  { "en": "Sudut Putaran Setengah Lingkaran?", "id": "Seratus Delapan Puluh Derajat" },
  { "en": "Saya Sering Lupa Mematikan Kompor.", "id": "Tidak, Saya Teliti" },
  { "en": "Apa Tugas Utama Polisi Udara?", "id": "Pemantauan Dari Udara" },
  { "en": "Deret Dua, Empat, Enam, Delapan?", "id": "Sepuluh" },
  { "en": "Emas : Tambang = Mutiara : ...?", "id": "Laut" },
  { "en": "Polisi Wanita Disebut Apa?", "id": "Polwan" },
  { "en": "Delapan Kali Delapan Bagi Empat?", "id": "Enam Belas" },
  { "en": "Apa Sinonim Kata Verifikasi?", "id": "Pemeriksaan" },
  { "en": "Saya Selalu Membersihkan Meja Kerja.", "id": "Ya, Kerapian" },
  { "en": "Pendidikan Tamtama Polri Di Mana?", "id": "Pusdik Brimob, Atau Polair" },
  { "en": "Rumus Keliling Persegi Panjang?", "id": "Dua Kali Panjang, Tambah Lebar" },
  { "en": "Saya Tidak Mudah Menyerah.", "id": "Ya, Pantang Mundur" },
  { "en": "Jarum : Benang = Lem : ...?", "id": "Kertas" },
  { "en": "Apa Warna Seragam Upacara Besar Polri?", "id": "PDU" },
  { "en": "Satu Lusin Berapa Buah?", "id": "Dua Belas Buah" },
  { "en": "Saya Selalu Menghormati Tetangga.", "id": "Ya, Kehidupan Sosial" },
  { "en": "Sekolah Staf Pimpinan Tinggi Polri Di Mana?", "id": "Sespim Lemdiklat Polri" },
  { "en": "Deret Tiga Puluh, Dua Puluh Lima, Dua Puluh?", "id": "Lima Belas" },
  { "en": "Berapa Sila Pancasila?", "id": "Lima Sila" },
  { "en": "Akar Empat Puluh Sembilan?", "id": "Tujuh" },
  { "en": "Saya Sering Curiga Pada Teman.", "id": "Tidak, Berpikir Positif" },
  { "en": "Siapa Pemimpin Tertinggi Di Polda?", "id": "Kapolda" },
  { "en": "Rumus Luas Segitiga?", "id": "Setengah Alas Kali Tinggi" },
  { "en": "Saya Suka Kerjasama.", "id": "Ya, Gotong Royong" },
  { "en": "Kaki : Sepatu = Tangan : ...?", "id": "Sarung Tangan" },
  { "en": "Kapan Hari Kemerdekaan RI?", "id": "Tujuh Belas Agustus" },
  { "en": "Apa Antonim Kata Rajin?", "id": "Malas" },
  { "en": "Saya Tidak Pernah Mengambil Milik Orang.", "id": "Ya, Jujur" },
  { "en": "Siapa Yang Menangani Kecelakaan Lalu Lintas?", "id": "Unit Laka Lantas" },
  { "en": "Deret Satu, Dua, Empat, Tujuh?", "id": "Sebelas" },
  { "en": "Beras : Nasi = Tepung : ...?", "id": "Roti" },
  { "en": "Siapa Presiden Saat Ini?", "id": "Prabowo Subianto" },
  { "en": "Satu Koma Dua Tambah Nol Koma Delapan?", "id": "Dua" },
  { "en": "Saya Sering Memukul Meja.", "id": "Tidak, Kontrol Emosi" },
  { "en": "Siapa Yang Melakukan Patroli Jalan Raya?", "id": "PJR" },
  { "en": "Saya Suka Belajar.", "id": "Ya, Pengembangan Diri" },
  { "en": "Mobil : Garasi = Pesawat : ...?", "id": "Hangar" },
  { "en": "Lambang Sila Keempat Pancasila?", "id": "Kepala Banteng" },
  { "en": "Dua Pangkat Tiga Adalah?", "id": "Delapan" },
  { "en": "Saya Selalu Disiplin.", "id": "Ya, Karakter Polisi" },
  { "en": "Siapa Yang Bertugas Memadamkan Kerusuhan?", "id": "Dalmas, Atau Brimob" },
  { "en": "Berapa Jumlah Titik Sudut Pada Balok?", "id": "Delapan Titik Sudut" },
  { "en": "Apa Warna Baret Satuan Brimob Polri?", "id": "Biru Dongker" },
  { "en": "Saya Merasa Gugup Saat Berada Di Keramaian.", "id": "Tidak, Harus Percaya Diri" },
  { "en": "Kuas : Lukis = Cangkul : ...?", "id": "Gali" },
  { "en": "Deret Dua, Enam, Sepuluh, Empat Belas?", "id": "Delapan Belas" },
  { "en": "Siapa Yang Berhak Mengeluarkan SKCK?", "id": "Intelkam Polri" },
  { "en": "Akar Kuadrat Dari Dua Ratus Delapan Puluh Sembilan?", "id": "Tujuh Belas" },
  { "en": "Saya Suka Menyelesaikan Pekerjaan Sebelum Tenggat Waktu.", "id": "Ya, Disiplin Waktu" },
  { "en": "Apa Antonim Kata Progresif?", "id": "Regresif, Atau Mundur" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Dua Melati?", "id": "Ajun Komisaris Besar Polisi" },
  { "en": "Empat Koma Lima Dikali Dua?", "id": "Sembilan" },
  { "en": "Apa Sinonim Kata Integritas?", "id": "Kejujuran" },
  { "en": "Saya Menggunakan Mobil Dinas Untuk Kepentingan Pribadi.", "id": "Tidak, Itu Pelanggaran" },
  { "en": "Apa Tugas Utama Polisi Pariwisata?", "id": "Pengamanan Objek Wisata" },
  { "en": "Jika Roda Gigi A Berputar Kiri, Roda B?", "id": "Berputar Kanan, Berlawanan" },
  { "en": "Semua Tentara Tegas. Budi Tidak Tegas.", "id": "Budi Bukan Tentara" },
  { "en": "Saya Merasa Hidup Ini Membosankan.", "id": "Tidak, Selalu Semangat" },
  { "en": "Sepatu : Alas Kaki = Topi : ...?", "id": "Penutup Kepala" },
  { "en": "Apa Semboyan Korps Polairud?", "id": "Arnavat Darpha Mahe" },
  { "en": "Apa Antonim Kata Fana?", "id": "Abadi" },
  { "en": "Saya Selalu Menjaga Rahasia Dinas.", "id": "Ya, Sumpah Jabatan" },
  { "en": "Tes Kecermatan Angka Hilang Melatih Apa?", "id": "Konsentrasi Tinggi" },
  { "en": "Sapi : Susu = Ayam : ...?", "id": "Telur" },
  { "en": "Di Mana Letak Museum Polri?", "id": "Jakarta Selatan" },
  { "en": "Satu Kuintal Berapa Ons?", "id": "Seribu Ons" },
  { "en": "Saya Pernah Berbohong Kepada Orang Tua.", "id": "Pernah, Jawaban Jujur" },
  { "en": "Apa Sinonim Kata Regulasi?", "id": "Aturan" },
  { "en": "Unit Apa Yang Menangani Anjing Pelacak?", "id": "K-9" },
  { "en": "Dua Ratus Bagi Empat?", "id": "Lima Puluh" },
  { "en": "Apa Antonim Kata Optimis?", "id": "Pesimis" },
  { "en": "Saya Iri Jika Teman Lebih Sukses.", "id": "Tidak, Harus Sportif" },
  { "en": "Siapa Yang Menangani Kejahatan Narkoba?", "id": "Direktorat Reserse Narkoba" },
  { "en": "Rumus Luas Jajar Genjang?", "id": "Alas Kali Tinggi" },
  { "en": "Saya Suka Menolong Orang Tua.", "id": "Ya, Berbakti" },
  { "en": "Apa Nama Program Kapolri Listyo Sigit?", "id": "Polri Presisi" },
  { "en": "Buku : Membaca = Pena : ...?", "id": "Menulis" },
  { "en": "Satu Abad Berapa Tahun?", "id": "Seratus Tahun" },
  { "en": "Apa Sinonim Kata Transparan?", "id": "Terbuka" },
  { "en": "Siapa Yang Mengatur Keuangan Polri?", "id": "Pusat Keuangan Polri" },
  { "en": "Jika Besok Sabtu, Kemarin Hari Apa?", "id": "Kamis" },
  { "en": "Semua Ikan Berinsang. Paus Tidak Berinsang.", "id": "Paus Bukan Ikan Sejati" },
  { "en": "Saya Berani Menolak Gratifikasi.", "id": "Ya, Integritas Mutlak" },
  { "en": "Telinga : Anting = Leher : ...?", "id": "Kalung" },
  { "en": "Siapa Bapak Brimob Polri?", "id": "Komjen Polisi M Jasin" },
  { "en": "Apa Antonim Kata Subjektif?", "id": "Objektif" },
  { "en": "Saya Sering Lupa Janji.", "id": "Tidak, Saya Disiplin" },
  { "en": "Apa Tugas Utama Polisi Udara?", "id": "Patroli Udara" },
  { "en": "Deret Satu, Dua, Empat, Delapan?", "id": "Enam Belas" },
  { "en": "Emas : Perhiasan = Besi : ...?", "id": "Pagar, Atau Konstruksi" },
  { "en": "Polisi Wanita Pertama Di Indonesia Di Mana?", "id": "Bukittinggi Sumatera Barat" },
  { "en": "Sembilan Kali Sembilan Bagi Sembilan?", "id": "Sembilan" },
  { "en": "Apa Sinonim Kata Verifikasi?", "id": "Pengecekan" },
  { "en": "Saya Selalu Membersihkan Rumah.", "id": "Ya, Kebersihan" },
  { "en": "Pendidikan Perwira Polri Di Mana?", "id": "Akpol, Atau Setukpa" },
  { "en": "Jarum : Jahit = Gunting : ...?", "id": "Potong" },
  { "en": "Apa Warna Seragam PDH Polri?", "id": "Cokelat Muda, Dan Tua" },
  { "en": "Satu Lusin Ditambah Satu Buah?", "id": "Tiga Belas" },
  { "en": "Saya Selalu Menghormati Guru.", "id": "Ya, Etika" },
  { "en": "Deret Tiga Puluh, Sepuluh, Nol?", "id": "Minus Sepuluh" },
  { "en": "Kertas : Buku = Kain : ...?", "id": "Baju" },
  { "en": "Akar Enam Puluh Empat?", "id": "Delapan" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Tidak Stabil" },
  { "en": "Siapa Pemimpin Di Polsek?", "id": "Kapolsek" },
  { "en": "Kapan Hari Pahlawan?", "id": "Sepuluh November" },
  { "en": "Seperempat Desimalnya?", "id": "Nol Koma Dua Lima" },
  { "en": "Siapa Yang Mengurus SIM?", "id": "Satlantas Polri" },
  { "en": "Deret Satu, Tiga, Lima?", "id": "Tujuh" },
  { "en": "Beras : Padi = Bensin : ...?", "id": "Minyak Bumi" },
  { "en": "Siapa Presiden Keempat RI?", "id": "Abdurrahman Wahid (Gus Dur)" },
  { "en": "Satu Koma Dua Tambah Tiga Koma Delapan?", "id": "Lima" },
  { "en": "Saya Sering Memukul Orang.", "id": "Tidak, Kekerasan Dilarang" },
  { "en": "Siapa Yang Melakukan Patroli Jalan?", "id": "Sabhara, Dan Lantas" },
  { "en": "Matahari Terbenam Di?", "id": "Barat" },
  { "en": "Saya Suka Inovasi.", "id": "Ya, Kreatif" },
  { "en": "Mobil : Garasi = Kapal : ...?", "id": "Dermaga" },
  { "en": "Lambang Sila Kelima Pancasila?", "id": "Padi Dan Kapas" },
  { "en": "Tiga Pangkat Dua Adalah?", "id": "Sembilan" },
  { "en": "Saya Selalu Disiplin Waktu.", "id": "Ya, Karakter Polisi" },
  { "en": "Siapa Yang Menjaga Tahanan Polri?", "id": "Satuan Tahti" },
  { "en": "Berapa Jumlah Sisi Pada Bangun Kubus?", "id": "Enam Sisi" },
  { "en": "Apa Warna Baret Satuan Polisi Perairan?", "id": "Biru Langit, Atau Biru Benhur" },
  { "en": "Saya Merasa Tidak Nyaman Jika Harus Antre.", "id": "Tidak, Harus Sabar" },
  { "en": "Kertas : Pohon = Emas : ...?", "id": "Tambang" },
  { "en": "Deret Empat, Delapan, Enam Belas, Tiga Puluh Dua?", "id": "Enam Puluh Empat" },
  { "en": "Siapa Yang Berhak Melakukan Penahanan Tersangka?", "id": "Penyidik Polri" },
  { "en": "Akar Kuadrat Dari Seratus Enam Puluh Sembilan?", "id": "Tiga Belas" },
  { "en": "Saya Suka Mengerjakan Hal Detail.", "id": "Ya, Ketelitian" },
  { "en": "Air : Es = Uap : ...?", "id": "Air" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Satu Balok Emas?", "id": "Ajun Inspektur Polisi Dua" },
  { "en": "Nol Koma Lima Dikali Empat?", "id": "Dua" },
  { "en": "Apa Sinonim Kata Inisiasi?", "id": "Permulaan" },
  { "en": "Saya Pernah Menggunakan Wewenang Untuk Kepentingan Pribadi.", "id": "Tidak, Itu Penyalahgunaan" },
  { "en": "Apa Tugas Utama Polisi Kehutanan?", "id": "Menjaga Kelestarian Hutan" },
  { "en": "Jika Roda A Berputar Searah Jarum Jam, Roda B?", "id": "Berlawanan Arah" },
  { "en": "Semua Pilot Punya Lisensi. Budi Tidak Punya.", "id": "Budi Bukan Pilot" },
  { "en": "Saya Merasa Hidup Ini Penuh Tekanan.", "id": "Tidak, Harus Tahan Banting" },
  { "en": "Sepatu : Tali = Kemeja : ...?", "id": "Kancing" },
  { "en": "Apa Semboyan Akademi Kepolisian?", "id": "Dharma Bijaksana Ksatria" },
  { "en": "Tiga Pangkat Tiga Kurang Dua Pangkat Tiga?", "id": "Sembilan Belas" },
  { "en": "Apa Antonim Kata Abstrak?", "id": "Konkrit, Atau Nyata" },
  { "en": "Saya Selalu Menjaga Rahasia Jabatan.", "id": "Ya, Kode Etik" },
  { "en": "Tes Kecermatan Simbol Hilang Melatih Apa?", "id": "Ketelitian Mata" },
  { "en": "Deret Dua, Lima, Delapan, Sebelas?", "id": "Empat Belas" },
  { "en": "Sapi : Melenguh = Anjing : ...?", "id": "Menggonggong" },
  { "en": "Di Mana Letak Sekolah Staf Dan Pimpinan Polri?", "id": "Lembang Bandung" },
  { "en": "Satu Ton Berapa Kuintal?", "id": "Sepuluh Kuintal" },
  { "en": "Saya Pernah Mencontek Saat Ujian.", "id": "Pernah, Jujur Itu Utama" },
  { "en": "Satuan Apa Yang Mengamankan Demonstrasi?", "id": "Sabhara, Dan Brimob" },
  { "en": "Seratus Lima Puluh Bagi Tiga?", "id": "Lima Puluh" },
  { "en": "Apa Antonim Kata Pesimis?", "id": "Optimis" },
  { "en": "Saya Iri Jika Teman Lebih Kaya.", "id": "Tidak, Harus Bersyukur" },
  { "en": "Siapa Yang Menangani Kasus Cyber Crime?", "id": "Direktorat Siber" },
  { "en": "Palu : Besi = Gergaji : ...?", "id": "Kayu" },
  { "en": "Saya Suka Membantu Orang Tua.", "id": "Ya, Berbakti" },
  { "en": "Apa Nama Program Kapolri Listyo Sigit?", "id": "Presisi" },
  { "en": "Buku : Ilmu = Olahraga : ...?", "id": "Kesehatan" },
  { "en": "Saya Sering Gemetar Saat Tegang.", "id": "Tidak, Mental Harus Kuat" },
  { "en": "Siapa Yang Mengatur Anggaran Polri?", "id": "Puskeu" },
  { "en": "Jika Besok Minggu, Kemarin Hari Apa?", "id": "Jumat" },
  { "en": "Semua Mamalia Menyusui. Kucing Mamalia.", "id": "Kucing Menyusui" },
  { "en": "Telinga : Dengar = Hidung : ...?", "id": "Cium, Atau Bau" },
  { "en": "Siapa Bapak Brimob Polri?", "id": "Mochammad Jasin" },
  { "en": "Sudut Putaran Seperempat Lingkaran?", "id": "Sembilan Puluh Derajat" },
  { "en": "Saya Sering Lupa Nama Orang.", "id": "Tidak, Saya Perhatian" },
  { "en": "Emas : Karat = Berlian : ...?", "id": "Karat" },
  { "en": "Delapan Kali Sembilan Bagi Dua?", "id": "Tiga Puluh Enam" },
  { "en": "Saya Selalu Membersihkan Kamar Sendiri.", "id": "Ya, Mandiri" },
  { "en": "Pendidikan Bintara Polri Di Mana?", "id": "SPN" },
  { "en": "Saya Tidak Mudah Putus Asa.", "id": "Ya, Pantang Menyerah" },
  { "en": "Jarum : Jahit = Pisau : ...?", "id": "Iris, Atau Potong" },
  { "en": "Saya Selalu Menghormati Orang Lain.", "id": "Ya, Etika" },
  { "en": "Sekolah Pembentukan Perwira Di Mana?", "id": "Sukabumi" },
  { "en": "Siapa Pemimpin Di Polres?", "id": "Kapolres" },
  { "en": "Saya Suka Bekerja Tim.", "id": "Ya, Sinergi" },
  { "en": "Saya Tidak Pernah Mencuri.", "id": "Ya, Jujur" },
  { "en": "Siapa Yang Menerbitkan SKCK?", "id": "Intelkam" },
  { "en": "Beras : Nasi = Tepung : ...?", "id": "Kue, Atau Roti" },
  { "en": "Siapa Yang Melakukan Patroli Kota?", "id": "Sabhara" },
  { "en": "Matahari Terbit Di Arah?", "id": "Timur" },
  { "en": "Apa Antonim Kata Kuno?", "id": "Modern" },
  { "en": "Siapa Yang Mengurus Kenaikan Pangkat?", "id": "SDM Polri" },
  { "en": "Berapa Jumlah Rusuk Pada Bangun Prisma Segitiga?", "id": "Sembilan Rusuk" },
  { "en": "Siapa Yang Mengawasi Kinerja Kepolisian Dari Eksternal?", "id": "Kompolnas, Dan DPR" },
  { "en": "Saya Sering Merasa Iri Dengan Kesuksesan Teman.", "id": "Tidak, Harus Ikut Senang" },
  { "en": "Kamera : Lensa = Manusia : ...?", "id": "Mata" },
  { "en": "Apa Warna Baret Pasukan Perdamaian PBB Polri?", "id": "Warna Biru Muda" },
  { "en": "Saya Selalu Mengerjakan Tugas Hingga Tuntas.", "id": "Ya, Bertanggung Jawab" },
  { "en": "Apa Antonim Kata Konsisten?", "id": "Inkonsisten, Atau Berubah" },
  { "en": "Air : Menguap = Es : ...?", "id": "Mencair" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Dua Balok Perak?", "id": "Ajun Inspektur Polisi Satu" },
  { "en": "Nol Koma Dua Dikali Lima?", "id": "Satu" },
  { "en": "Apa Sinonim Kata Kompensasi?", "id": "Ganti Rugi, Atau Imbalan" },
  { "en": "Saya Pernah Memalsukan Tanda Tangan Absensi.", "id": "Tidak, Itu Tidak Jujur" },
  { "en": "Jika Roda A Berputar Kanan, Roda B Kiri?", "id": "Ya, Jika Bersinggungan" },
  { "en": "Semua Tentara Berani. Budi Penakut.", "id": "Budi Bukan Tentara" },
  { "en": "Saya Merasa Hidup Ini Tidak Adil.", "id": "Tidak, Selalu Bersyukur" },
  { "en": "Dompet : Uang = Tas : ...?", "id": "Buku, Atau Barang" },
  { "en": "Apa Semboyan Satuan Reserse Kriminal?", "id": "Sidik Sakti, Indera Waspada" },
  { "en": "Lima Pangkat Dua Kurang Empat Pangkat Dua?", "id": "Sembilan" },
  { "en": "Apa Antonim Kata Fiksi?", "id": "Fakta, Atau Nyata" },
  { "en": "Deret Satu, Empat, Tujuh, Sepuluh?", "id": "Tiga Belas" },
  { "en": "Sapi : Daging = Domba : ...?", "id": "Wol, Atau Daging" },
  { "en": "Di Mana Lokasi Sekolah Polisi Wanita?", "id": "Ciputat, Jakarta Selatan" },
  { "en": "Saya Pernah Berbohong Demi Kebaikan.", "id": "Pernah, Itu Manusiawi" },
  { "en": "Apa Sinonim Kata Regulasi?", "id": "Aturan, Atau Ketentuan" },
  { "en": "Unit Apa Yang Menangani Bom?", "id": "Gegana, Korps Brimob" },
  { "en": "Tiga Ratus Bagi Enam?", "id": "Lima Puluh" },
  { "en": "Saya Iri Jika Teman Naik Pangkat.", "id": "Tidak, Harus Sportif" },
  { "en": "Siapa Yang Menangani Kejahatan Lintas Negara?", "id": "Hubinter, Atau Interpol" },
  { "en": "Palu : Paku = Kunci : ...?", "id": "Baut, Atau Mur" },
  { "en": "Rumus Luas Lingkaran?", "id": "Pi, Kali Jari-Jari Kuadrat" },
  { "en": "Saya Suka Membantu Orang Tua.", "id": "Ya, Anak Berbakti" },
  { "en": "Deret Dua Puluh, Lima Belas, Sepuluh?", "id": "Lima" },
  { "en": "Apa Nama Program Kapolri Saat Ini?", "id": "Presisi" },
  { "en": "Satu Milenium Berapa Tahun?", "id": "Seribu Tahun" },
  { "en": "Apa Sinonim Kata Transparan?", "id": "Terbuka, Atau Jelas" },
  { "en": "Saya Sering Gemetar Saat Ujian.", "id": "Tidak, Mental Harus Kuat" },
  { "en": "Siapa Yang Mengatur Lalu Lintas Udara?", "id": "Airnav, Bukan Polisi" },
  { "en": "Jika Besok Selasa, Kemarin Hari Apa?", "id": "Minggu" },
  { "en": "Semua Ikan Berenang. Lele Ikan.", "id": "Lele Berenang" },
  { "en": "Telinga : Dengar = Lidah : ...?", "id": "Rasa, Atau Kecap" },
  { "en": "Saya Sering Lupa Nama Jalan.", "id": "Tidak, Saya Perhatian" },
  { "en": "Apa Tugas Utama Sabhara?", "id": "Turjawali" },
  { "en": "Emas : Kuning = Tembaga : ...?", "id": "Merah, Atau Cokelat" },
  { "en": "Polisi Wanita Pertama Di Kota Mana?", "id": "Bukittinggi" },
  { "en": "Apa Sinonim Kata Verifikasi?", "id": "Pemeriksaan, Atau Pembuktian" },
  { "en": "Saya Selalu Membersihkan Tempat Tidur.", "id": "Ya, Kebiasaan Disiplin" },
  { "en": "Pendidikan Tamtama Polri Di Mana?", "id": "Pusdik Brimob, Watukosek" },
  { "en": "Rumus Keliling Persegi Panjang?", "id": "Dua Kali, Panjang Tambah Lebar" },
  { "en": "Jarum : Kain = Gergaji : ...?", "id": "Kayu" },
  { "en": "Apa Warna Seragam PDL Sus?", "id": "Cokelat Tua" },
  { "en": "Apa Antonim Kata Nyata?", "id": "Maya, Atau Fana" },
  { "en": "Saya Selalu Menghormati Atasan.", "id": "Ya, Hierarki" },
  { "en": "Sekolah Staf Pimpinan Polri Di Mana?", "id": "Lembang, Bandung" },
  { "en": "Kertas : Buku = Benang : ...?", "id": "Kain, Atau Baju" },
  { "en": "Akar Tiga Puluh Enam?", "id": "Enam" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Gejolak, Naik Turun" },
  { "en": "Siapa Pemimpin Di Polda?", "id": "Kapolda" },
  { "en": "Siapa Yang Menerbitkan BPKB?", "id": "Korps Lalu Lintas" },
  { "en": "Beras : Nasi = Kedelai : ...?", "id": "Tahu, Atau Tempe" },
  { "en": "Apa Sinonim Kata Valid?", "id": "Sah, Atau Benar" },
  { "en": "Lambang Sila Kelima Pancasila?", "id": "Padi, Dan Kapas" },
  { "en": "Siapa Yang Mengelola Data Personel Polri?", "id": "SDM Polri" },
  { "en": "Berapa Jumlah Sisi Pada Bangun Ruang Bola?", "id": "Satu Sisi, Lengkung" },
  { "en": "Siapa Yang Berwenang Menindak Pelanggaran Disiplin Polisi?", "id": "Provost, Atau Propam" },
  { "en": "Saya Merasa Gugup Saat Bertemu Orang Baru.", "id": "Tidak, Saya Supel" },
  { "en": "Kunci : Gembok = Password : ...?", "id": "Akun, Atau Sistem" },
  { "en": "Apa Warna Dasar Pelat Dinas Polri?", "id": "Hitam, Garis Kuning" },
  { "en": "Akar Kuadrat Dari Empat Ratus Empat Puluh Satu?", "id": "Dua Puluh Satu" },
  { "en": "Saya Suka Bekerja Dengan Target Tinggi.", "id": "Ya, Motivasi Tinggi" },
  { "en": "Apa Antonim Kata Temporer?", "id": "Permanen, Atau Tetap" },
  { "en": "Matahari : Panas = Es : ...?", "id": "Dingin" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Satu Bintang?", "id": "Brigadir Jenderal Polisi" },
  { "en": "Dua Koma Lima Dikali Empat?", "id": "Sepuluh" },
  { "en": "Apa Sinonim Kata Implementasi?", "id": "Pelaksanaan, Atau Penerapan" },
  { "en": "Saya Pernah Membocorkan Rahasia Teman.", "id": "Pernah, Itu Salah" },
  { "en": "Apa Tugas Utama Polisi Perairan?", "id": "Patroli Wilayah Air" },
  { "en": "Jika Roda Depan Sepeda Berputar, Roda Belakang?", "id": "Ikut Berputar" },
  { "en": "Semua Hakim Jujur. Pak Ali Hakim.", "id": "Pak Ali Jujur" },
  { "en": "Saya Merasa Hidup Ini Sia-Sia.", "id": "Tidak, Selalu Bersyukur" },
  { "en": "Sepatu : Kaki = Sarung Tangan : ...?", "id": "Tangan" },
  { "en": "Apa Semboyan Satuan Brimob?", "id": "Jiwa Ragaku, Demi Kemanusiaan" },
  { "en": "Tujuh Kuadrat Ditambah Dua Kuadrat?", "id": "Lima Puluh Tiga" },
  { "en": "Apa Antonim Kata Amatir?", "id": "Profesional, Atau Ahli" },
  { "en": "Saya Selalu Menjaga Kerapian Seragam.", "id": "Ya, Cermin Disiplin" },
  { "en": "Tes Kecermatan Angka Hilang Melatih Apa?", "id": "Ketelitian, Dan Fokus" },
  { "en": "Kambing : Rumput = Harimau : ...?", "id": "Daging" },
  { "en": "Di Mana Lokasi Sekolah Pembentukan Perwira?", "id": "Sukabumi, Jawa Barat" },
  { "en": "Saya Pernah Berbohong Untuk Kebaikan.", "id": "Pernah, Itu Manusiawi" },
  { "en": "Apa Sinonim Kata Integritas?", "id": "Kejujuran, Dan Moral" },
  { "en": "Unit Apa Yang Menangani Kerusuhan Massa?", "id": "Dalmas, Atau PHH" },
  { "en": "Empat Ratus Bagi Dua Puluh?", "id": "Dua Puluh" },
  { "en": "Saya Iri Melihat Teman Naik Jabatan.", "id": "Tidak, Harus Sportif" },
  { "en": "Siapa Yang Menangani Kejahatan Ekonomi Khusus?", "id": "Bareskrim Polri" },
  { "en": "Palu : Paku = Obeng : ...?", "id": "Sekrup, Atau Baut" },
  { "en": "Rumus Luas Belah Ketupat?", "id": "Setengah, Kali Diagonal Satu Dua" },
  { "en": "Saya Suka Membantu Tetangga Yang Kesulitan.", "id": "Ya, Jiwa Sosial" },
  { "en": "Apa Nama Program Unggulan Kapolri?", "id": "Presisi" },
  { "en": "Buku : Penulis = Film : ...?", "id": "Sutradara, Atau Produser" },
  { "en": "Siapa Yang Mengelola Data Sidik Jari?", "id": "Inafis, Bareskrim Polri" },
  { "en": "Jika Besok Rabu, Kemarin Hari Apa?", "id": "Senin" },
  { "en": "Semua Burung Bersayap. Pinguin Burung.", "id": "Pinguin Bersayap" },
  { "en": "Saya Berani Menolak Perintah Salah.", "id": "Ya, Integritas Tinggi" },
  { "en": "Telinga : Suara = Hidung : ...?", "id": "Bau, Atau Aroma" },
  { "en": "Siapa Bapak Kepolisian Indonesia?", "id": "Raden Said Soekanto" },
  { "en": "Saya Sering Lupa Membawa SIM.", "id": "Tidak, Saya Patuh" },
  { "en": "Apa Tugas Utama Polisi Lalu Lintas?", "id": "Kamseltibcarlantas" },
  { "en": "Perak : Logam = Berlian : ...?", "id": "Batu Mulia" },
  { "en": "Polisi Udara Bertugas Di Mana?", "id": "Wilayah Udara" },
  { "en": "Sembilan Kali Delapan Bagi Dua?", "id": "Tiga Puluh Enam" },
  { "en": "Apa Sinonim Kata Verifikasi?", "id": "Pemeriksaan, Atau Validasi" },
  { "en": "Saya Selalu Membersihkan Kendaraan Sendiri.", "id": "Ya, Mandiri" },
  { "en": "Jarum : Benang = Mur : ...?", "id": "Baut" },
  { "en": "Satu Gros Berapa Buah?", "id": "Seratus Empat Puluh Empat" },
  { "en": "Apa Antonim Kata Nyata?", "id": "Fana, Atau Maya" },
  { "en": "Saya Suka Bekerja Tim.", "id": "Ya, Sinergitas" },
  { "en": "Siapa Yang Menerbitkan Surat Keterangan Catatan Kepolisian?", "id": "Intelkam Polri" },
  { "en": "Saya Sering Marah Tanpa Alasan.", "id": "Tidak, Emosi Stabil" },
  { "en": "Siapa Yang Mengurus Kesehatan Polisi?", "id": "Pusdokkes" },
  { "en": "Berapa Rusuk Pada Bangun Limas Segi Tiga?", "id": "Enam Rusuk" },
  { "en": "Siapa Yang Menangani Kasus Perdagangan Manusia?", "id": "Bareskrim, Unit PPA" },
  { "en": "Saya Merasa Orang Lain Selalu Salah Paham.", "id": "Tidak, Saya Introspeksi" },
  { "en": "Mikroskop : Laboratorium = Teleskop : ...?", "id": "Observatorium" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Umum?", "id": "Kuning, Tulisan Hitam" },
  { "en": "Akar Kuadrat Dari Lima Ratus Dua Puluh Sembilan?", "id": "Dua Puluh Tiga" },
  { "en": "Saya Suka Mengerjakan Tugas Yang Sulit.", "id": "Ya, Suka Tantangan" },
  { "en": "Apa Antonim Kata Sporadis?", "id": "Sering, Atau Rutin" },
  { "en": "Listrik : Kabel = Darah : ...?", "id": "Pembuluh Darah" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Tiga Balok Emas?", "id": "Ajun Komisaris Polisi" },
  { "en": "Nol Koma Tiga Dikali Tiga?", "id": "Nol Koma Sembilan" },
  { "en": "Apa Sinonim Kata Inovasi?", "id": "Terobosan, Atau Pembaharuan" },
  { "en": "Saya Pernah Menggunakan Fasilitas Kantor Untuk Pribadi.", "id": "Tidak, Itu Dilarang" },
  { "en": "Apa Tugas Utama Polisi Satwa?", "id": "Melacak, Dan Mengendus" },
  { "en": "Jika Roda A Berputar Cepat, Roda B Lambat?", "id": "Tergantung Ukuran Roda" },
  { "en": "Semua Guru Sabar. Pak Budi Marah.", "id": "Pak Budi Tidak Sabar" },
  { "en": "Saya Merasa Hidup Ini Sangat Melelahkan.", "id": "Tidak, Tetap Semangat" },
  { "en": "Apa Semboyan Satuan Sabhara?", "id": "Samapta, Bhayangkara" },
  { "en": "Delapan Pangkat Dua Kurang Enam Pangkat Dua?", "id": "Dua Puluh Delapan" },
  { "en": "Saya Selalu Menjaga Rahasia Negara.", "id": "Ya, Sumpah Prajurit" },
  { "en": "Tes Kecermatan Simbol Hilang Melatih Apa?", "id": "Kecepatan, Dan Ketelitian" },
  { "en": "Sapi : Melenguh = Kucing : ...?", "id": "Mengeong" },
  { "en": "Di Mana Lokasi Sekolah Staf Dan Pimpinan?", "id": "Lembang, Jawa Barat" },
  { "en": "Saya Pernah Berbohong Kepada Teman.", "id": "Pernah, Jujur Itu Sulit" },
  { "en": "Apa Sinonim Kata Regulasi?", "id": "Peraturan, Atau Tata Tertib" },
  { "en": "Unit Apa Yang Menangani Kejahatan Siber?", "id": "Direktorat Siber" },
  { "en": "Saya Iri Jika Teman Mendapat Pujian.", "id": "Tidak, Harus Sportif" },
  { "en": "Siapa Yang Menangani Kasus Korupsi?", "id": "Tipidkor, Bareskrim" },
  { "en": "Saya Suka Membantu Orang Lain.", "id": "Ya, Jiwa Sosial" },
  { "en": "Buku : Ilmu = Olahraga : ...?", "id": "Kesehatan, Atau Stamina" },
  { "en": "Saya Sering Gemetar Saat Tegang.", "id": "Tidak, Mental Baja" },
  { "en": "Siapa Yang Mengatur Kenaikan Pangkat?", "id": "SDM, Polri" },
  { "en": "Saya Berani Menolak Gratifikasi.", "id": "Ya, Integritas Tinggi" },
  { "en": "Saya Sering Lupa Nama Orang.", "id": "Tidak, Saya Teliti" },
  { "en": "Apa Tugas Utama Polisi Udara?", "id": "Patroli, Dan Pengawasan" },
  { "en": "Apa Sinonim Kata Verifikasi?", "id": "Pengecekan, Atau Validasi" },
  { "en": "Saya Selalu Membersihkan Rumah.", "id": "Ya, Kebiasaan Baik" },
  { "en": "Pendidikan Bintara Polri Di Mana?", "id": "Sekolah Polisi Negara" },
  { "en": "Saya Selalu Menghormati Guru.", "id": "Ya, Etika Dasar" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Gejolak, Atau Naik Turun" },
  { "en": "Siapa Yang Menerbitkan SKCK?", "id": "Intelkam Polri" },
  { "en": "Beras : Nasi = Tepung : ...?", "id": "Roti, Atau Kue" },
  { "en": "Siapa Yang Menangani Kesehatan Polisi?", "id": "Dokkes" },
  { "en": "Berapa Jumlah Titik Sudut Pada Limas Segi Empat?", "id": "Lima Titik Sudut" },
  { "en": "Siapa Yang Berwenang Mengeluarkan Surat Izin Keramaian?", "id": "Intelkam, Polri" },
  { "en": "Saya Merasa Orang Lain Sering Memanfaatkan Saya.", "id": "Tidak, Saya Tegas" },
  { "en": "Mikroskop : Biologi = Teleskop : ...?", "id": "Astronomi" },
  { "en": "Deret Dua, Tiga, Lima, Tujuh, Sebelas?", "id": "Tiga Belas" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Pemerintah?", "id": "Merah, Tulisan Putih" },
  { "en": "Akar Kuadrat Dari Enam Ratus Dua Puluh Lima?", "id": "Dua Puluh Lima" },
  { "en": "Saya Suka Mengambil Inisiatif Tanpa Disuruh.", "id": "Ya, Proaktif" },
  { "en": "Apa Antonim Kata Tentatif?", "id": "Pasti, Atau Definitif" },
  { "en": "Jantung : Darah = Pompa : ...?", "id": "Air" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Satu Balok Merah?", "id": "Bhayangkara Satu" },
  { "en": "Nol Koma Dua Lima Dibagi Nol Koma Lima?", "id": "Nol Koma Lima" },
  { "en": "Apa Sinonim Kata Komprehensif?", "id": "Luas, Dan Lengkap" },
  { "en": "Saya Pernah Pulang Lebih Awal Tanpa Izin.", "id": "Tidak, Itu Indisipliner" },
  { "en": "Apa Tugas Utama Unit Inafis?", "id": "Identifikasi, Dan Sidik Jari" },
  { "en": "Jika Roda Gigi Kecil Memutar Gigi Besar, Kecepatannya?", "id": "Lebih Lambat" },
  { "en": "Semua Polisi WNI. John Warga Asing.", "id": "John Bukan Polisi" },
  { "en": "Topi : Kepala = Kacamata : ...?", "id": "Mata" },
  { "en": "Apa Semboyan Lambang Polri?", "id": "Rastra Sewakottama" },
  { "en": "Sembilan Pangkat Dua Kurang Lima Pangkat Dua?", "id": "Lima Puluh Enam" },
  { "en": "Apa Antonim Kata Monoton?", "id": "Bervariasi, Atau Beragam" },
  { "en": "Saya Selalu Menjaga Kebersihan Asrama.", "id": "Ya, Tanggung Jawab" },
  { "en": "Tes Kecermatan Angka Hilang Melatih Apa?", "id": "Ketahanan, Dan Fokus" },
  { "en": "Kuda : Ladam = Mobil : ...?", "id": "Ban" },
  { "en": "Di Mana Lokasi Akademi Kepolisian?", "id": "Semarang, Jawa Tengah" },
  { "en": "Saya Pernah Mencontek Saat Masih Kecil.", "id": "Pernah, Itu Jujur" },
  { "en": "Apa Sinonim Kata Elaborasi?", "id": "Pemaparan, Atau Penjelasan" },
  { "en": "Unit Apa Yang Menangani Kejahatan Narkotika?", "id": "Reserse Narkoba" },
  { "en": "Lima Ratus Bagi Dua Puluh Lima?", "id": "Dua Puluh" },
  { "en": "Apa Antonim Kata Skeptis?", "id": "Yakin, Atau Percaya" },
  { "en": "Saya Iri Jika Teman Lebih Pintar.", "id": "Tidak, Jadikan Motivasi" },
  { "en": "Siapa Yang Menangani Uji Balistik Peluru?", "id": "Labfor, Polri" },
  { "en": "Palu : Ketok = Gunting : ...?", "id": "Potong" },
  { "en": "Rumus Luas Segitiga Siku-Siku?", "id": "Setengah, Alas Kali Tinggi" },
  { "en": "Saya Suka Membantu Korban Bencana.", "id": "Ya, Kemanusiaan" },
  { "en": "Apa Nama Kode Etik Polri?", "id": "Kode Etik Profesi Polri" },
  { "en": "Buku : Perpustakaan = Obat : ...?", "id": "Apotek, Atau Rumah Sakit" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Ketidakstabilan" },
  { "en": "Saya Sering Gemetar Saat Berbicara.", "id": "Tidak, Harus Berani" },
  { "en": "Siapa Yang Mengatur Mutasi Jabatan?", "id": "Biro SDM" },
  { "en": "Jika Besok Kamis, Lusa Hari Apa?", "id": "Jumat" },
  { "en": "Semua Ular Reptil. Kobra Adalah Ular.", "id": "Kobra Adalah Reptil" },
  { "en": "Saya Berani Melaporkan Teman Yang Salah.", "id": "Ya, Demi Kebenaran" },
  { "en": "Telinga : Mendengar = Kulit : ...?", "id": "Meraba" },
  { "en": "Siapa Kapolri Pada Masa Reformasi 1998?", "id": "Jenderal Dibyo Widodo" },
  { "en": "Sudut Putaran Penuh Berapa Derajat?", "id": "Tiga Ratus Enam Puluh Derajat" },
  { "en": "Apa Antonim Kata Objektif?", "id": "Subjektif, Atau Bias" },
  { "en": "Saya Sering Lupa Menaruh Barang.", "id": "Tidak, Saya Teliti" },
  { "en": "Apa Tugas Utama Bhabinkamtibmas?", "id": "Pembinaan, Dan Deteksi Dini" },
  { "en": "Deret Dua, Enam, Delapan Belas?", "id": "Lima Puluh Empat" },
  { "en": "Besi : Karat = Kayu : ...?", "id": "Lapuk, Atau Rayap" },
  { "en": "Polisi Pariwisata Menggunakan Dasi Warna Apa?", "id": "Merah Marun" },
  { "en": "Tujuh Kali Delapan Bagi Empat?", "id": "Empat Belas" },
  { "en": "Apa Sinonim Kata Verifikasi?", "id": "Pembuktian, Atau Cek" },
  { "en": "Saya Selalu Merapikan Tempat Tidur.", "id": "Ya, Disiplin Diri" },
  { "en": "Pendidikan Inspektur Polisi Di Mana?", "id": "Setukpa, Sukabumi" },
  { "en": "Saya Tidak Mudah Putus Asa.", "id": "Ya, Mental Juara" },
  { "en": "Jarum : Kain = Kuas : ...?", "id": "Kanvas, Atau Dinding" },
  { "en": "Apa Warna Seragam PDL II Two Tone?", "id": "Cokelat Muda, Dan Tua" },
  { "en": "Satu Kodi Berapa Lembar?", "id": "Dua Puluh Lembar" },
  { "en": "Apa Antonim Kata Maya?", "id": "Nyata, Atau Realita" },
  { "en": "Saya Selalu Menghormati Orang Tua.", "id": "Ya, Etika Moral" },
  { "en": "Sekolah Tinggi Ilmu Kepolisian Di Mana?", "id": "Kebayoran Baru, Jakarta" },
  { "en": "Deret Lima Puluh, Empat Puluh Lima, Empat Puluh?", "id": "Tiga Puluh Lima" },
  { "en": "Kertas : Pohon = Plastik : ...?", "id": "Minyak Bumi" },
  { "en": "Apa Sinonim Kata Validitas?", "id": "Kesahihan, Atau Kebenaran" },
  { "en": "Rumus Luas Persegi Panjang?", "id": "Panjang, Kali Lebar" },
  { "en": "Saya Suka Bekerja Kelompok.", "id": "Ya, Kerjasama" },
  { "en": "Kaki : Jalan = Sirip : ...?", "id": "Berenang" },
  { "en": "Kapan Hari Kesaktian Pancasila?", "id": "Satu Oktober" },
  { "en": "Apa Antonim Kata Pasif?", "id": "Aktif, Atau Proaktif" },
  { "en": "Siapa Yang Mengeluarkan Plat Nomor?", "id": "Samsat, Atau Lantas" },
  { "en": "Deret Satu, Tiga, Enam, Sepuluh?", "id": "Lima Belas" },
  { "en": "Beras : Padi = Tepung : ...?", "id": "Gandum" },
  { "en": "Siapa Wakil Presiden Pertama RI?", "id": "Mohammad Hatta" },
  { "en": "Apa Sinonim Kata Legal?", "id": "Sah, Atau Resmi" },
  { "en": "Saya Sering Membanting Pintu.", "id": "Tidak, Emosi Stabil" },
  { "en": "Siapa Yang Melakukan Pengawalan VVIP?", "id": "Lantas, Dan Brimob" },
  { "en": "Mobil : Setir = Motor : ...?", "id": "Stang" },
  { "en": "Lambang Sila Ketiga Pancasila?", "id": "Pohon Beringin" },
  { "en": "Apa Antonim Kata Tradisional?", "id": "Modern, Atau Canggih" },
  { "en": "Siapa Yang Mengurus Pensiun Polisi?", "id": "SDM, Dan Asabri" },
  { "en": "Berapa Sisi Pada Bangun Datar Layang-Layang?", "id": "Empat Sisi" },
  { "en": "Siapa Yang Berwenang Menangani Pelanggaran Kode Etik Polri?", "id": "Komisi Kode Etik, Propam" },
  { "en": "Saya Merasa Cemas Saat Berada Di Tempat Gelap.", "id": "Tidak, Saya Berani" },
  { "en": "Dokter : Resep = Hakim : ...?", "id": "Vonis, Atau Putusan" },
  { "en": "Deret Dua, Empat, Delapan, Sepuluh?", "id": "Empat Belas" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Listrik?", "id": "Putih, Lis Biru" },
  { "en": "Akar Kuadrat Dari Seratus Sembilan Puluh Enam?", "id": "Empat Belas" },
  { "en": "Saya Suka Mengambil Risiko Yang Terukur.", "id": "Ya, Tanda Keberanian" },
  { "en": "Apa Antonim Kata Insidental?", "id": "Rutin, Atau Terjadwal" },
  { "en": "Air : Ember = Uang : ...?", "id": "Dompet, Atau Bank" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Satu Melati Emas?", "id": "Komisaris Polisi" },
  { "en": "Satu Koma Lima Dikali Empat?", "id": "Enam" },
  { "en": "Apa Sinonim Kata Kognitif?", "id": "Akal, Atau Pikiran" },
  { "en": "Saya Pernah Memanipulasi Laporan Keuangan Kecil.", "id": "Tidak, Itu Tidak Jujur" },
  { "en": "Apa Tugas Utama Polisi Di Jalan Raya?", "id": "Mengatur Lalu Lintas" },
  { "en": "Jika Roda A Berputar Ke Kiri, Roda B Yang Bersinggungan?", "id": "Berputar Ke Kanan" },
  { "en": "Semua Tentara Membawa Senjata. Budi Tentara.", "id": "Budi Membawa Senjata" },
  { "en": "Saya Merasa Orang Lain Selalu Lebih Beruntung.", "id": "Tidak, Saya Bersyukur" },
  { "en": "Gelas : Pecah = Kayu : ...?", "id": "Patah, Atau Lapuk" },
  { "en": "Apa Semboyan Satuan Polisi Perairan?", "id": "Arnavat, Darpha Mahe" },
  { "en": "Enam Pangkat Dua Ditambah Delapan Pangkat Dua?", "id": "Seratus" },
  { "en": "Apa Antonim Kata Universal?", "id": "Parsial, Atau Khusus" },
  { "en": "Saya Selalu Menjaga Nama Baik Institusi.", "id": "Ya, Loyalitas" },
  { "en": "Tes Kecermatan Angka Hilang Melatih Apa?", "id": "Ketelitian, Dan Kecepatan" },
  { "en": "Ayam : Berkokok = Harimau : ...?", "id": "Mengaum" },
  { "en": "Di Mana Lokasi Markas Besar Polri?", "id": "Jakarta Selatan, Trunojoyo" },
  { "en": "Satu Kilogram Berapa Ons?", "id": "Sepuluh Ons" },
  { "en": "Saya Pernah Melanggar Janji Temu.", "id": "Pernah, Karena Halangan" },
  { "en": "Apa Sinonim Kata Rekognisi?", "id": "Pengakuan, Atau Penghargaan" },
  { "en": "Unit Apa Yang Menangani Kejahatan Terhadap Perempuan?", "id": "Unit PPA" },
  { "en": "Tiga Ratus Bagi Lima Belas?", "id": "Dua Puluh" },
  { "en": "Siapa Yang Menangani Identifikasi Mayat Tak Dikenal?", "id": "Inafis, Dan Dokkes" },
  { "en": "Palu : Besi = Cangkul : ...?", "id": "Tanah" },
  { "en": "Saya Suka Menolong Orang Tua Menyeberang.", "id": "Ya, Kepedulian Sosial" },
  { "en": "Apa Nama Program Kapolri Jenderal Listyo?", "id": "Polri Presisi" },
  { "en": "Buku : Halaman = Rumah : ...?", "id": "Ruangan, Atau Lantai" },
  { "en": "Satu Dasawarsa Berapa Tahun?", "id": "Sepuluh Tahun" },
  { "en": "Apa Sinonim Kata Tendensi?", "id": "Kecenderungan, Atau Niat" },
  { "en": "Saya Sering Gemetar Saat Marah.", "id": "Tidak, Pengendalian Emosi" },
  { "en": "Siapa Yang Mengatur Distribusi Senjata Polri?", "id": "Logistik Polri" },
  { "en": "Jika Besok Jumat, Kemarin Hari Apa?", "id": "Rabu" },
  { "en": "Semua Burung Bertelur. Elang Adalah Burung.", "id": "Elang Bertelur" },
  { "en": "Saya Berani Mengambil Keputusan Sulit.", "id": "Ya, Jiwa Pemimpin" },
  { "en": "Siapa Kapolri Ke-5?", "id": "Jenderal Hoegeng, Imam Santoso" },
  { "en": "Sudut Putaran Tiga Perempat Lingkaran?", "id": "Dua Ratus Tujuh Puluh Derajat" },
  { "en": "Apa Antonim Kata Konservatif?", "id": "Modern, Atau Inovatif" },
  { "en": "Saya Sering Lupa Waktu Saat Main Game.", "id": "Tidak, Saya Disiplin" },
  { "en": "Perak : Cincin = Kayu : ...?", "id": "Kursi, Atau Meja" },
  { "en": "Polisi Wanita (Polwan) Dibentuk Tahun Berapa?", "id": "Seribu Sembilan Ratus, Empat Puluh Delapan" },
  { "en": "Sembilan Kali Sembilan Bagi Tiga?", "id": "Dua Puluh Tujuh" },
  { "en": "Apa Sinonim Kata Validasi?", "id": "Pengesahan, Atau Pengujian" },
  { "en": "Saya Selalu Membersihkan Alat Makan Sendiri.", "id": "Ya, Kemandirian" },
  { "en": "Pendidikan Perwira Sumber Sarjana Di Mana?", "id": "Akpol, Semarang" },
  { "en": "Rumus Keliling Trapesium?", "id": "Jumlah Semua Sisi" },
  { "en": "Apa Warna Seragam Dinas Upacara PDU 1?", "id": "Cokelat Tua, Lengkap Atribut" },
  { "en": "Apa Antonim Kata Nyata?", "id": "Maya, Atau Imajinasi" },
  { "en": "Saya Selalu Menghormati Perbedaan Pendapat.", "id": "Ya, Demokrasi" },
  { "en": "Sekolah Staf Pimpinan Menengah Di Mana?", "id": "Lembang, Bandung" },
  { "en": "Kertas : Buku = Kain : ...?", "id": "Baju, Atau Celana" },
  { "en": "Akar Seratus Empat Puluh Empat?", "id": "Dua Belas" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Gejolak, Atau Perubahan" },
  { "en": "Saya Sering Curiga Pada Tetangga.", "id": "Tidak, Hidup Rukun" },
  { "en": "Rumus Luas Layang-Layang?", "id": "Setengah, Kali Diagonal Satu Dua" },
  { "en": "Saya Suka Bekerja Sama.", "id": "Ya, Gotong Royong" },
  { "en": "Kaki : Lari = Sayap : ...?", "id": "Terbang" },
  { "en": "Kapan Hari Lahir Pancasila?", "id": "Satu Juni" },
  { "en": "Siapa Yang Menangani Kecelakaan Lalu Lintas?", "id": "Unit Laka, Lantas" },
  { "en": "Apa Sinonim Kata Valid?", "id": "Sah, Atau Berlaku" },
  { "en": "Siapa Yang Melakukan Patroli Perbatasan?", "id": "Brimob, Dan Sabhara" },
  { "en": "Saya Suka Belajar Hal Baru.", "id": "Ya, Pengembangan Diri" },
  { "en": "Mobil : Roda = Perahu : ...?", "id": "Dayung, Atau Layar" },
  { "en": "Siapa Yang Mengurus Kenaikan Pangkat?", "id": "Biro SDM" },
  { "en": "Berapa Jumlah Titik Sudut Pada Bangun Prisma Segi Empat?", "id": "Delapan Titik Sudut" },
  { "en": "Siapa Yang Berwenang Menangani Pelanggaran HAM Berat?", "id": "Komnas HAM, Dan Pengadilan HAM" },
  { "en": "Saya Merasa Kesal Jika Orang Lain Lambat Bekerja.", "id": "Tidak, Saya Sabar" },
  { "en": "Padi : Sawah = Ikan : ...?", "id": "Kolam, Atau Sungai" },
  { "en": "Deret Tiga, Lima, Delapan, Tiga Belas?", "id": "Dua Puluh Satu" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Korps Diplomatik?", "id": "Putih, Kode CD" },
  { "en": "Akar Kuadrat Dari Tujuh Ratus Dua Puluh Sembilan?", "id": "Dua Puluh Tujuh" },
  { "en": "Saya Suka Mengambil Tanggung Jawab Pemimpin.", "id": "Ya, Jiwa Kepemimpinan" },
  { "en": "Apa Antonim Kata Insentif?", "id": "Disinsentif, Atau Hukuman" },
  { "en": "Lampu : Cahaya = Kompor : ...?", "id": "Panas, Atau Api" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Dua Bintang Emas?", "id": "Inspektur Jenderal Polisi" },
  { "en": "Nol Koma Enam Dibagi Nol Koma Dua?", "id": "Tiga" },
  { "en": "Apa Sinonim Kata Konsekuensi?", "id": "Akibat, Atau Dampak" },
  { "en": "Saya Pernah Memalsukan Tanda Tangan Orang Tua.", "id": "Pernah, Itu Salah" },
  { "en": "Apa Tugas Utama Unit Jibom (Penjinak Bom)?", "id": "Sterilisasi, Dan Penjinakan Bom" },
  { "en": "Jika Roda A Berputar 10 Kali, Roda B Yang Lebih Besar?", "id": "Berputar Kurang Dari 10 Kali" },
  { "en": "Semua Hakim Sarjana Hukum. Pak Budi Bukan Sarjana Hukum.", "id": "Pak Budi Bukan Hakim" },
  { "en": "Saya Merasa Hidup Ini Penuh Ketidakadilan.", "id": "Tidak, Selalu Bersyukur" },
  { "en": "Gitar : Senar = Terompet : ...?", "id": "Udara, Atau Tiup" },
  { "en": "Apa Semboyan Satuan Reserse Narkoba?", "id": "Perangi Narkoba, Selamatkan Bangsa" },
  { "en": "Sepuluh Pangkat Dua Kurang Delapan Pangkat Dua?", "id": "Tiga Puluh Enam" },
  { "en": "Apa Antonim Kata Eksternal?", "id": "Internal, Atau Dalam" },
  { "en": "Saya Selalu Menjaga Rahasia Pribadi Teman.", "id": "Ya, Dapat Dipercaya" },
  { "en": "Tes Kecermatan Simbol Hilang Melatih Apa?", "id": "Ketelitian, Dan Kecepatan Persepsi" },
  { "en": "Deret Dua, Empat, Delapan, Empat Belas?", "id": "Dua Puluh Dua" },
  { "en": "Kambing : Mengembik = Kuda : ...?", "id": "Meringkik" },
  { "en": "Di Mana Lokasi Sekolah Polisi Negara (SPN) Di Jawa Barat?", "id": "Cisarua, Bandung Barat" },
  { "en": "Satu Hektar Berapa Are?", "id": "Seratus Are" },
  { "en": "Saya Pernah Bolos Sekolah Saat SMP.", "id": "Pernah, Itu Kenakalan Remaja" },
  { "en": "Apa Sinonim Kata Rekomendasi?", "id": "Saran, Atau Anjuran" },
  { "en": "Unit Apa Yang Menangani Kejahatan Perbankan?", "id": "Fismondev, Krimsus" },
  { "en": "Enam Ratus Bagi Tiga Puluh?", "id": "Dua Puluh" },
  { "en": "Saya Iri Jika Teman Lebih Populer.", "id": "Tidak, Percaya Diri Sendiri" },
  { "en": "Siapa Yang Menangani Uji DNA?", "id": "Labfor, Dan Dokkes" },
  { "en": "Obeng : Sekrup = Palu : ...?", "id": "Paku" },
  { "en": "Saya Suka Donor Darah.", "id": "Ya, Kepedulian Sosial" },
  { "en": "Deret Delapan Puluh, Enam Puluh, Empat Puluh?", "id": "Dua Puluh" },
  { "en": "Apa Nama Aplikasi Layanan Polri Online?", "id": "Polri Super App" },
  { "en": "Buku : Kertas = Baju : ...?", "id": "Kain, Atau Benang" },
  { "en": "Satu Abad Berapa Windu?", "id": "Dua Belas Setengah Windu" },
  { "en": "Apa Sinonim Kata Signifikan?", "id": "Berarti, Atau Penting" },
  { "en": "Saya Sering Gemetar Saat Diinterogasi.", "id": "Tidak, Tenang Dan Jujur" },
  { "en": "Siapa Yang Mengatur Seragam Polri?", "id": "Logistik, Dan Propam" },
  { "en": "Jika Besok Senin, Kemarin Lusa Hari Apa?", "id": "Jumat" },
  { "en": "Semua Mamalia Punya Paru-Paru. Lumba-Lumba Mamalia.", "id": "Lumba-Lumba Punya Paru-Paru" },
  { "en": "Saya Berani Melaporkan Atasan Yang Salah.", "id": "Ya, Demi Institusi" },
  { "en": "Hidung : Bau = Lidah : ...?", "id": "Rasa" },
  { "en": "Siapa Kapolri Ke-13?", "id": "Jenderal Bimantoro" },
  { "en": "Sudut Putaran Satu Setengah Lingkaran?", "id": "Lima Ratus Empat Puluh Derajat" },
  { "en": "Apa Antonim Kata Stabil?", "id": "Labil, Atau Goyah" },
  { "en": "Saya Sering Lupa Matikan Kran Air.", "id": "Tidak, Hemat Energi" },
  { "en": "Apa Tugas Utama Polisi RW?", "id": "Mitra Masyarakat, Di Tingkat RW" },
  { "en": "Deret Satu, Tiga, Enam, Sepuluh, Lima Belas?", "id": "Dua Puluh Satu" },
  { "en": "Kaca : Bening = Kayu : ...?", "id": "Buram, Atau Keras" },
  { "en": "Polisi Satwa (K9) Berada Di Bawah Satuan Apa?", "id": "Sabhara, Baharkam" },
  { "en": "Sembilan Kali Enam Bagi Dua?", "id": "Dua Puluh Tujuh" },
  { "en": "Saya Selalu Mencuci Piring Setelah Makan.", "id": "Ya, Mandiri" },
  { "en": "Pendidikan Sespimti Polri Di Mana?", "id": "Lembang, Bandung" },
  { "en": "Rumus Keliling Layang-Layang?", "id": "Dua Kali, Jumlah Sisi Pendek Panjang" },
  { "en": "Saya Tidak Mudah Sakit Hati.", "id": "Ya, Berjiwa Besar" },
  { "en": "Jarum : Benang = Busur : ...?", "id": "Anak Panah" },
  { "en": "Apa Warna Seragam PDL I?", "id": "Cokelat Tua Polos" },
  { "en": "Satu Kodi Berapa Lusin?", "id": "Satu Dua Pertiga Lusin" },
  { "en": "Apa Antonim Kata Abstrak?", "id": "Konkret, Atau Nyata" },
  { "en": "Saya Selalu Menghormati Teman Beda Suku.", "id": "Ya, Bhinneka Tunggal Ika" },
  { "en": "Sekolah Pembentukan Bintara (SPN) Ada Berapa?", "id": "Ada Di Setiap Polda" },
  { "en": "Deret Empat Puluh, Tiga Puluh Lima, Tiga Puluh?", "id": "Dua Puluh Lima" },
  { "en": "Kertas : Tulis = Tanah : ...?", "id": "Tanam" },
  { "en": "Lambang Bintang Pada Pancasila Melambangkan Apa?", "id": "Ketuhanan Yang Maha Esa" },
  { "en": "Akar Dua Ratus Lima Puluh Enam?", "id": "Enam Belas" },
  { "en": "Apa Sinonim Kata Fluktuasi?", "id": "Naik Turun, Tidak Tetap" },
  { "en": "Saya Sering Curiga Pasangan Selingkuh.", "id": "Tidak, Saling Percaya" },
  { "en": "Siapa Pemimpin Di Polresta?", "id": "Kapolresta" },
  { "en": "Rumus Luas Trapesium Sama Kaki?", "id": "Jumlah Sisi Sejajar, Kali Tinggi Bagi Dua" },
  { "en": "Saya Suka Diskusi Kelompok.", "id": "Ya, Bertukar Pikiran" },
  { "en": "Kaki : Sepak = Tangan : ...?", "id": "Tinju, Atau Tangkap" },
  { "en": "Kapan Hari Kebangkitan Nasional?", "id": "Dua Puluh Mei" },
  { "en": "Tiga Perdelapan Desimalnya Adalah?", "id": "Nol Koma Tiga Tujuh Lima" },
  { "en": "Apa Antonim Kata Aktif?", "id": "Pasif, Atau Diam" },
  { "en": "Saya Tidak Pernah Mengambil Uang Orang Tua.", "id": "Ya, Jujur" },
  { "en": "Siapa Yang Menerbitkan Surat Izin Senjata Api?", "id": "Intelkam Polri" },
  { "en": "Deret Dua, Lima, Sepuluh, Tujuh Belas?", "id": "Dua Puluh Enam" },
  { "en": "Gandum : Roti = Kapas : ...?", "id": "Benang, Atau Kain" },
  { "en": "Dua Koma Satu Tambah Tiga Koma Sembilan?", "id": "Enam" },
  { "en": "Apa Sinonim Kata Sahih?", "id": "Valid, Atau Benar" },
  { "en": "Saya Sering Menangis Tanpa Sebab.", "id": "Tidak, Emosi Stabil" },
  { "en": "Siapa Yang Melakukan Pengamanan Objek Vital?", "id": "Pamobvit" },
  { "en": "Saya Suka Mencari Solusi Baru.", "id": "Ya, Solutif" },
  { "en": "Mobil : Roda = Kereta Api : ...?", "id": "Roda Besi" },
  { "en": "Lambang Sila Kedua Pancasila?", "id": "Rantai Emas" },
  { "en": "Tiga Pangkat Tiga Adalah?", "id": "Dua Puluh Tujuh" },
  { "en": "Apa Antonim Kata Modern?", "id": "Tradisional, Atau Kuno" },
  { "en": "Saya Selalu Patuh Aturan Lalu Lintas.", "id": "Ya, Teladan Masyarakat" },
  { "en": "Siapa Yang Mengurus Kesejahteraan Anggota?", "id": "Biro Watpers, SDM" },
  { "en": "Berapa Jumlah Sisi Pada Bangun Kerucut?", "id": "Dua Sisi" },
  { "en": "Unit Apa Yang Menangani Huru-Hara Di Jalanan?", "id": "Dalmas, Sabhara" },
  { "en": "Saya Merasa Kesal Jika Dikritik Di Depan Umum.", "id": "Tidak, Saya Terbuka" },
  { "en": "Nelayan : Jaring = Petani : ...?", "id": "Cangkul, Atau Bajak" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Angkutan Barang?", "id": "Kuning, Tulisan Hitam" },
  { "en": "Akar Kuadrat Dari Delapan Ratus Empat Puluh Satu?", "id": "Dua Puluh Sembilan" },
  { "en": "Saya Suka Mengambil Keputusan Cepat.", "id": "Ya, Tanda Ketegasan" },
  { "en": "Apa Antonim Kata Sporadis?", "id": "Terus Menerus, Atau Sering" },
  { "en": "Lilin : Meleleh = Kayu : ...?", "id": "Terbakar, Atau Arang" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Satu Balok Merah?", "id": "Bhayangkara Dua (Bharada)" },
  { "en": "Satu Koma Dua Dibagi Nol Koma Empat?", "id": "Tiga" },
  { "en": "Apa Sinonim Kata Komparasi?", "id": "Perbandingan" },
  { "en": "Saya Pernah Meminjam Barang Tanpa Izin.", "id": "Pernah, Itu Salah" },
  { "en": "Apa Tugas Utama Dovi (Disaster Victim Identification)?", "id": "Identifikasi Korban Bencana" },
  { "en": "Jika Roda Gigi A Berputar 20 Kali, Roda Gigi B Yang Lebih Besar?", "id": "Berputar Kurang Dari 20 Kali" },
  { "en": "Semua Dokter Memiliki Izin Praktik. Budi Dokter.", "id": "Budi Memiliki Izin Praktik" },
  { "en": "Saya Merasa Hidup Ini Penuh Beban.", "id": "Tidak, Selalu Bersyukur" },
  { "en": "Baterai : Energi = Makanan : ...?", "id": "Kalori, Atau Tenaga" },
  { "en": "Apa Makna Obor Pada Lambang Polri?", "id": "Penyuluh, Atau Penerang" },
  { "en": "Lima Pangkat Tiga Kurang Empat Pangkat Tiga?", "id": "Enam Puluh Satu" },
  { "en": "Apa Antonim Kata Kolektif?", "id": "Individual, Atau Parsial" },
  { "en": "Saya Selalu Menjaga Kerapian Rambut.", "id": "Ya, Sesuai Aturan" },
  { "en": "Tes Kecermatan Simbol Hilang Melatih Apa?", "id": "Daya Tahan, Dan Fokus" },
  { "en": "Lebah : Madu = Ulat : ...?", "id": "Sutra" },
  { "en": "Di Mana Pusat Pendidikan Reserse (Pusdik Reskrim)?", "id": "Megamendung, Bogor" },
  { "en": "Satu Dekare Berapa Are?", "id": "Sepuluh Are" },
  { "en": "Saya Pernah Melawan Guru Saat Sekolah.", "id": "Pernah, Itu Kenakalan" },
  { "en": "Apa Sinonim Kata Rekonsiliasi?", "id": "Perdamaian, Atau Pemulihan" },
  { "en": "Unit Apa Yang Menangani Kejahatan Pencucian Uang?", "id": "Eksus, Bareskrim" },
  { "en": "Delapan Ratus Bagi Empat Puluh?", "id": "Dua Puluh" },
  { "en": "Saya Iri Jika Teman Mendapat Promosi.", "id": "Tidak, Harus Sportif" },
  { "en": "Siapa Yang Melakukan Olah TKP?", "id": "Inafis, Dan Penyidik" },
  { "en": "Gunting : Kertas = Gergaji : ...?", "id": "Kayu" },
  { "en": "Rumus Luas Permukaan Kubus?", "id": "Enam, Kali Sisi Kuadrat" },
  { "en": "Saya Suka Menyumbang Ke Panti Asuhan.", "id": "Ya, Jiwa Sosial" },
  { "en": "Deret Seratus Dua Puluh, Enam Puluh, Tiga Puluh?", "id": "Lima Belas" },
  { "en": "Apa Nama Layanan Panggilan Darurat Polri?", "id": "Call Center 110" },
  { "en": "Buku : Perpustakaan = Lukisan : ...?", "id": "Galeri, Atau Museum" },
  { "en": "Satu Abad Berapa Lustrum?", "id": "Dua Puluh Lustrum" },
  { "en": "Apa Sinonim Kata Signifikan?", "id": "Penting, Atau Berarti" },
  { "en": "Saya Sering Gemetar Saat Di Depan Umum.", "id": "Tidak, Percaya Diri" },
  { "en": "Siapa Yang Mengatur Logistik Polri?", "id": "Slog, Polri" },
  { "en": "Jika Lusa Minggu, Kemarin Hari Apa?", "id": "Kamis" },
  { "en": "Semua Mamalia Menyusui. Platypus Mamalia.", "id": "Platypus Menyusui" },
  { "en": "Saya Berani Mengambil Risiko Terukur.", "id": "Ya, Jiwa Pemberani" },
  { "en": "Mata : Melihat = Kulit : ...?", "id": "Meraba" },
  { "en": "Siapa Nama Kapolri Ke-2?", "id": "Soekanto Tjokrodiatmodjo" },
  { "en": "Sudut Putaran Sepertiga Lingkaran?", "id": "Seratus Dua Puluh Derajat" },
  { "en": "Saya Sering Lupa Membawa KTP.", "id": "Tidak, Saya Teliti" },
  { "en": "Apa Tugas Utama Polisi Kehutanan?", "id": "Menjaga Hutan" },
  { "en": "Emas : Tambang = Mutiara : ...?", "id": "Laut, Atau Tiram" },
  { "en": "Satuan Polisi Jalan Raya (PJR) Di Bawah Siapa?", "id": "Korlantas Polri" },
  { "en": "Tujuh Kali Tujuh Bagi Tujuh?", "id": "Tujuh" },
  { "en": "Apa Sinonim Kata Validitas?", "id": "Keabsahan" },
  { "en": "Saya Selalu Merapikan Meja Kerja.", "id": "Ya, Kerapian" },
  { "en": "Pendidikan Sespimma Polri Di Mana?", "id": "Ciputat, Jakarta" },
  { "en": "Rumus Keliling Belah Ketupat?", "id": "Empat Kali Sisi" },
  { "en": "Jarum : Benang = Pancing : ...?", "id": "Kail, Atau Senar" },
  { "en": "Apa Warna Seragam PDL I Brimob?", "id": "Hitam, Atau Loreng" },
  { "en": "Saya Selalu Menghormati Tetangga.", "id": "Ya, Hidup Rukun" },
  { "en": "Pusat Pendidikan Polair Di Mana?", "id": "Pondok Dayung, Jakarta" },
  { "en": "Deret Lima Puluh, Empat Puluh Lima, Tiga Puluh Lima?", "id": "Dua Puluh" },
  { "en": "Kertas : Pulp = Baju : ...?", "id": "Kain, Atau Benang" },
  { "en": "Lambang Padi Kapas Pada Pancasila Sila Ke?", "id": "Sila Kelima" },
  { "en": "Akar Seratus Sembilan Puluh Enam?", "id": "Empat Belas" },
  { "en": "Saya Sering Curiga Pasangan Berbohong.", "id": "Tidak, Saling Percaya" },
  { "en": "Rumus Luas Trapesium?", "id": "Jumlah Sisi Sejajar, Kali Tinggi Bagi Dua" },
  { "en": "Saya Suka Bekerja Sendiri.", "id": "Tidak, Saya Team Player" },
  { "en": "Kaki : Jalan = Mata : ...?", "id": "Lihat" },
  { "en": "Kapan Hari Juang Polri?", "id": "Dua Puluh Satu Agustus" },
  { "en": "Saya Tidak Pernah Mengambil Barang Teman.", "id": "Ya, Jujur" },
  { "en": "Siapa Yang Menerbitkan Paspor?", "id": "Imigrasi, Bukan Polisi" },
  { "en": "Beras : Nasi = Singkong : ...?", "id": "Tape, Atau Tiwul" },
  { "en": "Siapa Wakil Presiden Saat Ini?", "id": "Gibran Rakabuming Raka (Konteks 2026)" },
  { "en": "Satu Koma Delapan Tambah Dua Koma Dua?", "id": "Empat" },
  { "en": "Apa Sinonim Kata Legalitas?", "id": "Keabsahan" },
  { "en": "Saya Sering Marah Jika Lapar.", "id": "Tidak, Kontrol Emosi" },
  { "en": "Siapa Yang Melakukan Penjagaan Tahanan?", "id": "Satuan Tahti" },
  { "en": "Saya Suka Mencari Tantangan.", "id": "Ya, Progresif" },
  { "en": "Mobil : Supir = Delman : ...?", "id": "Kusir" },
  { "en": "Empat Pangkat Tiga Adalah?", "id": "Enam Puluh Empat" },
  { "en": "Apa Antonim Kata Modern?", "id": "Kuno, Atau Klasik" },
  { "en": "Siapa Yang Menangani Cyber Crime?", "id": "Ditsiber" },
  { "en": "Berapa Jumlah Rusuk Pada Bangun Ruang Tabung?", "id": "Dua Rusuk, Lengkung" },
  { "en": "Unit Apa Yang Menangani Kejahatan Terhadap Lingkungan Hidup?", "id": "Tipidter, Bareskrim" },
  { "en": "Saya Merasa Malas Jika Tidak Ada Pengawasan.", "id": "Tidak, Saya Mandiri" },
  { "en": "Dokter : Stetoskop = Polisi : ...?", "id": "Borgol, Atau Pistol" },
  { "en": "Deret Dua Belas, Sepuluh, Delapan, Enam?", "id": "Empat" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Diplomatik Asing?", "id": "Putih, Kode CD" },
  { "en": "Saya Suka Mengambil Inisiatif Dalam Kelompok.", "id": "Ya, Jiwa Kepemimpinan" },
  { "en": "Apa Antonim Kata Insidental?", "id": "Rutin, Atau Berkala" },
  { "en": "Api : Abu = Ledakan : ...?", "id": "Puing, Atau Reruntuhan" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Dua Balok Merah?", "id": "Bhayangkara Satu (Bharatu)" },
  { "en": "Satu Koma Lima Dikali Enam?", "id": "Sembilan" },
  { "en": "Apa Sinonim Kata Kredibilitas?", "id": "Kepercayaan" },
  { "en": "Saya Pernah Memanipulasi Absensi Kehadiran.", "id": "Tidak, Itu Curang" },
  { "en": "Apa Tugas Utama Pasukan Pelopor Brimob?", "id": "Penanggulangan Huru-Hara, Dan SAR" },
  { "en": "Jika Roda A Berputar 50 Kali, Roda B Yang Lebih Kecil?", "id": "Berputar Lebih Dari 50 Kali" },
  { "en": "Semua Jaksa Sarjana Hukum. Pak Andi Jaksa.", "id": "Pak Andi Sarjana Hukum" },
  { "en": "Saya Merasa Orang Lain Selalu Lebih Bahagia.", "id": "Tidak, Saya Bersyukur" },
  { "en": "Kapal : Nahkoda = Kereta : ...?", "id": "Masinis" },
  { "en": "Apa Makna Tiga Bintang Di Atas Kepala Burung Garuda Polri?", "id": "Tribrata" },
  { "en": "Dua Pangkat Tiga Ditambah Tiga Pangkat Dua?", "id": "Tujuh Belas" },
  { "en": "Apa Antonim Kata Kolega?", "id": "Lawan, Atau Saingan" },
  { "en": "Saya Selalu Menjaga Kebersihan Seragam.", "id": "Ya, Cermin Disiplin" },
  { "en": "Tes Kecermatan Kolom Hilang Menguji Apa?", "id": "Daya Tahan, Dan Ketelitian" },
  { "en": "Deret Satu, Lima, Dua Puluh Lima?", "id": "Seratus Dua Puluh Lima" },
  { "en": "Gajah : Gading = Rusa : ...?", "id": "Tanduk" },
  { "en": "Di Mana Pusat Pendidikan Brimob (Pusdik Brimob)?", "id": "Watukosek, Jawa Timur" },
  { "en": "Saya Pernah Melawan Orang Tua.", "id": "Pernah, Saya Menyesal" },
  { "en": "Apa Sinonim Kata Mobilisasi?", "id": "Penggerakan" },
  { "en": "Unit Apa Yang Menangani Kejahatan Korupsi?", "id": "Tipidkor" },
  { "en": "Seribu Bagi Lima Puluh?", "id": "Dua Puluh" },
  { "en": "Saya Iri Jika Teman Mendapat Pujian Atasan.", "id": "Tidak, Jadikan Motivasi" },
  { "en": "Siapa Yang Melakukan Pemeriksaan Saksi?", "id": "Penyidik, Atau Penyidik Pembantu" },
  { "en": "Cangkul : Petani = Jaring : ...?", "id": "Nelayan" },
  { "en": "Rumus Luas Permukaan Balok?", "id": "Dua Kali, Panjang Lebar Tambah Lebar Tinggi" },
  { "en": "Saya Suka Menyisihkan Uang Untuk Amal.", "id": "Ya, Jiwa Sosial" },
  { "en": "Deret Empat Puluh Delapan, Dua Puluh Empat, Dua Belas?", "id": "Enam" },
  { "en": "Apa Nama Aplikasi Pembuatan SIM Online?", "id": "Digital Korlantas, Atau Sinar" },
  { "en": "Televisi : Gambar = Radio : ...?", "id": "Suara" },
  { "en": "Satu Milenium Berapa Abad?", "id": "Sepuluh Abad" },
  { "en": "Apa Sinonim Kata Relevan?", "id": "Sesuai, Atau Berkaitan" },
  { "en": "Saya Sering Gemetar Saat Memegang Benda Kecil.", "id": "Tidak, Tangan Stabil" },
  { "en": "Siapa Yang Mengurus Perbekalan Polri?", "id": "Slog (Staf Logistik)" },
  { "en": "Jika Lusa Selasa, Kemarin Hari Apa?", "id": "Sabtu" },
  { "en": "Semua Reptil Berdarah Dingin. Buaya Reptil.", "id": "Buaya Berdarah Dingin" },
  { "en": "Saya Berani Mengaku Salah Walau Dihukum.", "id": "Ya, Tanggung Jawab" },
  { "en": "Mata : Kacamata = Kaki : ...?", "id": "Sepatu, Atau Sandal" },
  { "en": "Siapa Kapolri Ke-3?", "id": "Jenderal Soetjipto Danoekoesoemo" },
  { "en": "Sudut Putaran Seperenam Lingkaran?", "id": "Enam Puluh Derajat" },
  { "en": "Saya Sering Lupa Membawa Surat Kendaraan.", "id": "Tidak, Saya Tertib" },
  { "en": "Apa Tugas Utama Unit Turjawali?", "id": "Pengaturan, Penjagaan, Pengawalan, Patroli" },
  { "en": "Air : Uap = Beku : ...?", "id": "Es, Atau Cair" },
  { "en": "Polisi Wanita (Polwan) Diperingati Setiap Tanggal?", "id": "Satu September" },
  { "en": "Delapan Kali Delapan Bagi Dua?", "id": "Tiga Puluh Dua" },
  { "en": "Apa Sinonim Kata Konfirmasi?", "id": "Pembenaran, Atau Penegasan" },
  { "en": "Saya Selalu Merapikan Tempat Kerja.", "id": "Ya, Kebersihan Pangkal Sehat" },
  { "en": "Pendidikan Perwira Alih Golongan (PAG) Di Mana?", "id": "Setukpa, Sukabumi" },
  { "en": "Rumus Keliling Segitiga Sama Sisi?", "id": "Tiga Kali Sisi" },
  { "en": "Saya Tidak Mudah Tersinggung.", "id": "Ya, Mental Kuat" },
  { "en": "Pena : Tinta = Mobil : ...?", "id": "Bensin, Atau Bahan Bakar" },
  { "en": "Apa Warna Seragam PDL Sus Brimob?", "id": "Hitam" },
  { "en": "Saya Selalu Menghargai Pendapat Teman.", "id": "Ya, Toleransi" },
  { "en": "Pusat Pendidikan Lantas Di Mana?", "id": "Serpong, Tangerang" },
  { "en": "Deret Sepuluh, Dua Puluh, Tiga Puluh, Empat Puluh?", "id": "Lima Puluh" },
  { "en": "Kertas : Kayu = Tembok : ...?", "id": "Batu Bata, Atau Semen" },
  { "en": "Lambang Kepala Banteng Pada Pancasila Sila Ke?", "id": "Sila Keempat" },
  { "en": "Akar Dua Ratus Dua Puluh Lima?", "id": "Lima Belas" },
  { "en": "Apa Sinonim Kata Gejolak?", "id": "Fluktuasi" },
  { "en": "Saya Sering Curiga Orang Lain Berniat Jahat.", "id": "Tidak, Berpikir Positif" },
  { "en": "Siapa Pemimpin Di Polrestabes?", "id": "Kapolrestabes" },
  { "en": "Rumus Luas Jajar Genjang?", "id": "Alas, Kali Tinggi" },
  { "en": "Saya Suka Memimpin Doa.", "id": "Ya, Religius" },
  { "en": "Kaki : Jalan = Mulut : ...?", "id": "Bicara, Atau Makan" },
  { "en": "Seperlima Desimalnya Adalah?", "id": "Nol Koma Dua" },
  { "en": "Apa Antonim Kata Proaktif?", "id": "Pasif, Atau Reaktif" },
  { "en": "Saya Tidak Pernah Memukul Teman.", "id": "Ya, Anti Kekerasan" },
  { "en": "Siapa Yang Menerbitkan Surat Keterangan Bebas Narkoba?", "id": "Rumah Sakit Polri, Atau BNN" },
  { "en": "Beras : Karbohidrat = Daging : ...?", "id": "Protein" },
  { "en": "Siapa Presiden Kelima RI?", "id": "Megawati Soekarnoputri" },
  { "en": "Tiga Koma Lima Tambah Satu Koma Lima?", "id": "Lima" },
  { "en": "Saya Sering Kesal Jika Menunggu Lama.", "id": "Tidak, Saya Sabar" },
  { "en": "Siapa Yang Melakukan Penjinakan Bom?", "id": "Gegana" },
  { "en": "Saya Suka Mencari Teman Baru.", "id": "Ya, Supel" },
  { "en": "Mobil : Jalan Raya = Kereta : ...?", "id": "Rel" },
  { "en": "Lima Pangkat Dua Adalah?", "id": "Dua Puluh Lima" },
  { "en": "Saya Selalu Datang Sebelum Acara Dimulai.", "id": "Ya, Disiplin" },
  { "en": "Siapa Yang Mengurus Administrasi Personel?", "id": "Min Pers" },
  { "en": "Berapa Jumlah Rusuk Pada Prisma Segi Enam?", "id": "Delapan Belas Rusuk" },
  { "en": "Siapa Yang Melantik Kapolri?", "id": "Presiden Republik Indonesia" },
  { "en": "Saya Menemukan Uang Tertinggal Di Mesin ATM.", "id": "Menyerahkannya, Kepada Satpam Bank" },
  { "en": "Awan : Langit = Rumput : ...?", "id": "Tanah" },
  { "en": "Deret Tiga, Empat, Tujuh, Sebelas?", "id": "Delapan Belas" },
  { "en": "Apa Fungsi Utama Seksi Umum (Sium) Polsek?", "id": "Administrasi, Dan Ketatausahaan" },
  { "en": "Akar Seratus Enam Puluh Sembilan Tambah Tiga Kuadrat?", "id": "Dua Puluh Dua" },
  { "en": "Apa Antonim Kata Nomaden?", "id": "Menetap" },
  { "en": "Berapa Jumlah Bulu Ekor Garuda Pancasila?", "id": "Delapan Helai" },
  { "en": "Semua Atlet Jaga Makan. Sebagian Atlet Suka Pedas.", "id": "Sebagian Yang Jaga Makan, Suka Pedas" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Satu Bintang Emas?", "id": "Brigadir Jenderal Polisi" },
  { "en": "Satu Koma Dua Dikali Lima?", "id": "Enam" },
  { "en": "Apa Sinonim Kata Kuantitas?", "id": "Jumlah, Atau Banyaknya" },
  { "en": "Saya Pernah Menggunakan Barang Bukti Untuk Pribadi.", "id": "Tidak, Itu Pelanggaran Berat" },
  { "en": "Apa Tugas Utama Unit Reskrim?", "id": "Penyelidikan, Dan Penyidikan" },
  { "en": "Jika Roda A Berputar Ke Kanan, Roda B Di Dalamnya?", "id": "Berputar Ke Kanan Juga" },
  { "en": "Semua Dokter Punya STR. Budi Tidak Punya STR.", "id": "Budi Bukan Dokter" },
  { "en": "Saya Merasa Orang Lain Selalu Ingin Menjatuhkan Saya.", "id": "Tidak, Berpikir Positif" },
  { "en": "Apa Semboyan Satuan Reserse?", "id": "Sidik Sakti, Indera Waspada" },
  { "en": "Delapan Pangkat Dua Bagi Empat?", "id": "Enam Belas" },
  { "en": "Saya Selalu Menjaga Kerapian Ruang Kerja.", "id": "Ya, Cermin Kepribadian" },
  { "en": "Deret Satu, Dua, Empat, Tujuh, Sebelas?", "id": "Enam Belas" },
  { "en": "Kambing : Rumput = Singa : ...?", "id": "Daging" },
  { "en": "Di Mana Lokasi Sekolah Inspektur Polisi (SIP)?", "id": "Sukabumi, Jawa Barat" },
  { "en": "Saya Pernah Bolos Kerja Tanpa Alasan.", "id": "Pernah, Itu Salah" },
  { "en": "Unit Apa Yang Menangani Kejahatan Siber?", "id": "Ditsiber, Bareskrim" },
  { "en": "Tiga Ratus Bagi Lima Puluh?", "id": "Enam" },
  { "en": "Saya Iri Jika Teman Lebih Sukses.", "id": "Tidak, Jadikan Motivasi" },
  { "en": "Siapa Yang Melakukan Visum Et Repertum?", "id": "Dokter Forensik" },
  { "en": "Saya Suka Menolong Orang Tua Menyeberang.", "id": "Ya, Jiwa Melayani" },
  { "en": "Deret Seratus, Sembilan Puluh, Tujuh Puluh?", "id": "Empat Puluh" },
  { "en": "Apa Nama Layanan Polisi 110?", "id": "Call Center Polri" },
  { "en": "Buku : Ilmu = Olahraga : ...?", "id": "Sehat, Atau Bugar" },
  { "en": "Siapa Yang Mengatur Kenaikan Pangkat?", "id": "Biro SDM" },
  { "en": "Jika Lusa Rabu, Kemarin Hari Apa?", "id": "Minggu" },
  { "en": "Saya Berani Menolak Perintah Yang Salah.", "id": "Ya, Integritas" },
  { "en": "Telinga : Dengar = Mata : ...?", "id": "Lihat" },
  { "en": "Siapa Bapak Brimob Polri?", "id": "Komjen Pol M Jasin" },
  { "en": "Sudut Putaran Penuh Adalah?", "id": "Tiga Ratus Enam Puluh Derajat" },
  { "en": "Polisi Wanita (Polwan) Lahir Di Kota Mana?", "id": "Bukittinggi" },
  { "en": "Delapan Kali Tujuh Bagi Dua?", "id": "Dua Puluh Delapan" },
  { "en": "Saya Selalu Membersihkan Kamar.", "id": "Ya, Mandiri" },
  { "en": "Pendidikan Tamtama Brimob Di Mana?", "id": "Watukosek" },
  { "en": "Jarum : Kain = Pipa : ...?", "id": "Air" },
  { "en": "Apa Warna Seragam PDH?", "id": "Cokelat Muda, Dan Tua" },
  { "en": "Satu Lusin Berapa Buah?", "id": "Dua Belas" },
  { "en": "Sekolah Staf Pimpinan Polri Di Mana?", "id": "Lembang" },
  { "en": "Saya Sering Curiga Pada Orang.", "id": "Tidak, Positif Thinking" },
  { "en": "Saya Suka Kerjasama Tim.", "id": "Ya, Sinergi" },
  { "en": "Setengah Desimalnya?", "id": "Nol Koma Lima" },
  { "en": "Saya Sering Marah Tanpa Sebab.", "id": "Tidak, Stabil" },
  { "en": "Siapa Yang Melakukan Patroli?", "id": "Sabhara" },
  { "en": "Siapa Yang Mengelola Keuangan Polri?", "id": "Puskeu" },
  { "en": "Berapa Jumlah Sisi Pada Bangun Datar Trapesium?", "id": "Empat Sisi" },
  { "en": "Tahun Berapa Polri Resmi Berpisah Dari ABRI (TNI)?", "id": "Seribu Sembilan Ratus, Sembilan Puluh Sembilan" },
  { "en": "Saya Merasa Kesal Jika Rencana Berantakan.", "id": "Tidak, Saya Adaptif" },
  { "en": "Nahkoda : Kapal = Masinis : ...?", "id": "Kereta Api" },
  { "en": "Deret Lima, Delapan, Sebelas, Empat Belas?", "id": "Tujuh Belas" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Bermotor Listrik?", "id": "Putih, Dengan Lis Biru" },
  { "en": "Akar Kuadrat Dari Seratus Dua Puluh Satu?", "id": "Sebelas" },
  { "en": "Saya Suka Mengkritik Teman Di Depan Umum.", "id": "Tidak, Itu Tidak Sopan" },
  { "en": "Bulan : Bumi = Bumi : ...?", "id": "Matahari" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Dua Bengkok Merah?", "id": "Ajun Brigadir Polisi" },
  { "en": "Nol Koma Empat Dikali Dua?", "id": "Nol Koma Delapan" },
  { "en": "Apa Sinonim Kata Komplesitas?", "id": "Kerumitan" },
  { "en": "Saya Pernah Memalsukan Tanda Tangan Absensi.", "id": "Tidak, Itu Penipuan" },
  { "en": "Apa Tugas Utama Unit Pelayanan Perempuan Dan Anak (PPA)?", "id": "Perlindungan, Dan Penyidikan Khusus" },
  { "en": "Jika Hari Ini Kamis, Sepuluh Hari Lagi Hari Apa?", "id": "Minggu" },
  { "en": "Semua Polisi Harus Disiplin. Andi Seorang Polisi.", "id": "Andi Harus Disiplin" },
  { "en": "Saya Merasa Hidup Ini Penuh Tekanan.", "id": "Tidak, Saya Nikmati Prosesnya" },
  { "en": "Kapur : Papan Tulis = Pulpen : ...?", "id": "Kertas" },
  { "en": "Apa Arti Warna Emas Pada Lambang Polri?", "id": "Keagungan, Atau Kejayaan" },
  { "en": "Dua Puluh Persen Dari Dua Ratus Ribu?", "id": "Empat Puluh Ribu" },
  { "en": "Saya Selalu Menepati Janji Kepada Teman.", "id": "Ya, Tanda Integritas" },
  { "en": "Tes Kecermatan Angka Hilang Mengukur Apa?", "id": "Ketahanan, Dan Konsentrasi" },
  { "en": "Deret Satu, Enam, Sebelas, Enam Belas?", "id": "Dua Puluh Satu" },
  { "en": "Sapi : Kandang = Burung : ...?", "id": "Sangkar" },
  { "en": "Di Mana Markas Pasukan Pelopor Brimob?", "id": "Kelapa Dua, Depok" },
  { "en": "Satu Mil Berapa Kilometer (Kira-Kira)?", "id": "Satu Koma Enam Kilometer" },
  { "en": "Saya Pernah Mengambil Uang Kembalian Belanja Ibu.", "id": "Pernah, Tapi Sudah Mengaku" },
  { "en": "Unit Apa Yang Menangani Kejahatan Lintas Negara?", "id": "Hubinter, Atau Interpol" },
  { "en": "Saya Iri Jika Teman Mendapat Pujian.", "id": "Tidak, Ikut Bahagia" },
  { "en": "Siapa Yang Melakukan Uji Balistik?", "id": "Laboratorium Forensik" },
  { "en": "Kunci : Pintu = Sandi : ...?", "id": "Brankas, Atau Akun" },
  { "en": "Rumus Luas Segitiga Sama Kaki?", "id": "Setengah, Alas Kali Tinggi" },
  { "en": "Saya Suka Berdonasi Untuk Bencana Alam.", "id": "Ya, Kepedulian Sosial" },
  { "en": "Deret Sembilan Puluh, Delapan Puluh Lima, Delapan Puluh?", "id": "Tujuh Puluh Lima" },
  { "en": "Apa Nama Satuan Polisi Di Tingkat Desa?", "id": "Polisi RW, Atau Bhabinkamtibmas" },
  { "en": "Satu Dekade Berapa Lustrum?", "id": "Dua Lustrum" },
  { "en": "Apa Sinonim Kata Urgent?", "id": "Mendesak, Atau Penting" },
  { "en": "Saya Sering Gemetar Saat Marah.", "id": "Tidak, Saya Tenang" },
  { "en": "Siapa Yang Menjamin Keamanan Dalam Negeri?", "id": "Kepolisian Negara Republik Indonesia" },
  { "en": "Jika Kemarin Lusa Senin, Besok Hari Apa?", "id": "Kamis" },
  { "en": "Semua Kucing Takut Air. Singa Adalah Kucing Besar.", "id": "Singa Takut Air" },
  { "en": "Siapa Kapolri Di Era Reformasi Awal?", "id": "Jenderal Roesmanhadi" },
  { "en": "Saya Sering Lupa Membawa Helm Saat Berkendara.", "id": "Tidak, Saya Patuh Aturan" },
  { "en": "Apa Tugas Utama Polisi Kehutanan?", "id": "Menjaga Hutan, Dari Pembalakan Liar" },
  { "en": "Kapan HUT Brimob Diperingati?", "id": "Empat Belas November" },
  { "en": "Sembilan Kali Sembilan Kurang Satu?", "id": "Delapan Puluh" },
  { "en": "Saya Selalu Merapikan Tempat Tidur.", "id": "Ya, Disiplin Pagi" },
  { "en": "Pendidikan Pembentukan Perwira (Setukpa) Di Mana?", "id": "Sukabumi, Jawa Barat" },
  { "en": "Jarum : Benang = Paku : ...?", "id": "Palu" },
  { "en": "Apa Warna Seragam PDL II?", "id": "Cokelat, Bermotif" },
  { "en": "Saya Selalu Menghormati Perbedaan Agama.", "id": "Ya, Toleransi" },
  { "en": "Di Mana Lokasi Pusdik Lantas?", "id": "Serpong, Tangerang Selatan" },
  { "en": "Deret Tiga Puluh, Dua Puluh Tujuh, Dua Puluh Empat?", "id": "Dua Puluh Satu" },
  { "en": "Kertas : Pulp = Baju : ...?", "id": "Kapas, Atau Benang" },
  { "en": "Lambang Padi Dan Kapas Ada Di Sila Keberapa?", "id": "Sila Kelima" },
  { "en": "Saya Sering Curiga Pada Orang Baru.", "id": "Tidak, Berpikir Positif" },
  { "en": "Saya Suka Bekerja Sama Dalam Tim.", "id": "Ya, Kerjasama Itu Penting" },
  { "en": "Kaki : Sepak = Tangan : ...?", "id": "Pukul, Atau Tangkap" },
  { "en": "Kapan Hari Sumpah Pemuda?", "id": "Dua Puluh Delapan Oktober" },
  { "en": "Satu Perlima Desimalnya?", "id": "Nol Koma Dua" },
  { "en": "Apa Antonim Kata Pasif?", "id": "Aktif, Atau Dinamis" },
  { "en": "Saya Tidak Pernah Mengambil Barang Kantor.", "id": "Ya, Jujur" },
  { "en": "Siapa Yang Menerbitkan BPKB?", "id": "Korlantas Polri" },
  { "en": "Beras : Karbohidrat = Tempe : ...?", "id": "Protein" },
  { "en": "Siapa Presiden Keempat Indonesia?", "id": "Abdurrahman Wahid" },
  { "en": "Apa Sinonim Kata Legalitas?", "id": "Keabsahan, Atau Hukum" },
  { "en": "Saya Sering Marah Tanpa Alasan Jelas.", "id": "Tidak, Emosi Stabil" },
  { "en": "Siapa Yang Melakukan Pengawalan Tahanan?", "id": "Sabhara, Atau Tahti" },
  { "en": "Matahari Terbenam Di Sebelah?", "id": "Barat" },
  { "en": "Siapa Yang Mengelola Data Personel?", "id": "Biro SDM" },
  { "en": "Berapa Jumlah Titik Sudut Pada Bangun Ruang Limas Segi Lima?", "id": "Enam Titik Sudut" },
  { "en": "Apa Nama Satuan Tugas Polri Yang Menangani Konflik Di Papua?", "id": "Operasi Damai Cartenz" },
  { "en": "Saya Suka Menunda Pekerjaan Hingga Detik Terakhir.", "id": "Tidak, Saya Disiplin" },
  { "en": "Montir : Bengkel = Dokter : ...?", "id": "Rumah Sakit, Atau Klinik" },
  { "en": "Deret Dua, Lima, Sebelas, Dua Puluh Tiga?", "id": "Empat Puluh Tujuh" },
  { "en": "Apa Warna Dasar Pelat Nomor Kendaraan Di Kawasan Perdagangan Bebas (FTZ)?", "id": "Hijau, Tulisan Hitam" },
  { "en": "Akar Kuadrat Dari Tiga Ratus Dua Puluh Empat?", "id": "Delapan Belas" },
  { "en": "Saya Merasa Malu Jika Harus Bertanya.", "id": "Tidak, Bertanya Itu Belajar" },
  { "en": "Apa Antonim Kata Absolut?", "id": "Relatif, Atau Nisbi" },
  { "en": "Hidung : Aroma = Lidah : ...?", "id": "Rasa" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Tiga Bengkok Perak?", "id": "Brigadir Polisi" },
  { "en": "Satu Perdelapan Desimalnya Adalah?", "id": "Nol Koma Satu Dua Lima" },
  { "en": "Apa Sinonim Kata Eksistensi?", "id": "Keberadaan" },
  { "en": "Saya Menolak Jika Teman Menitipkan Absen.", "id": "Ya, Itu Tindakan Curang" },
  { "en": "Apa Tugas Utama Paminal (Pengamanan Internal)?", "id": "Disiplin, Dan Etika Anggota" },
  { "en": "Jika Sabuk Penghubung Roda Tidak Disilang, Arah Putarannya?", "id": "Searah, Sama" },
  { "en": "Semua Polisi Penegak Hukum. Sebagian Penegak Hukum Tegas.", "id": "Sebagian Polisi, Mungkin Tegas" },
  { "en": "Saya Merasa Hidup Saya Monoton Dan Membosankan.", "id": "Tidak, Saya Mencari Tantangan" },
  { "en": "Lilin : Cahaya = Baterai : ...?", "id": "Listrik, Atau Energi" },
  { "en": "Apa Makna Tribrata Bagi Polri?", "id": "Pedoman Hidup, Kepolisian" },
  { "en": "Dua Belas Pangkat Dua Kurang Sepuluh Pangkat Dua?", "id": "Empat Puluh Empat" },
  { "en": "Apa Antonim Kata Pakar?", "id": "Awam, Atau Pemula" },
  { "en": "Saya Membiarkan Kuku Tangan Panjang Dan Kotor.", "id": "Tidak, Menjaga Kebersihan Diri" },
  { "en": "Tes Kecermatan Huruf Hilang Melatih Apa?", "id": "Konsentrasi, Dan Ketelitian" },
  { "en": "Deret Lima Puluh, Dua Puluh Lima, Dua Belas Koma Lima?", "id": "Enam Koma Dua Lima" },
  { "en": "Dompet : Uang = Lemari : ...?", "id": "Pakaian, Atau Baju" },
  { "en": "Di Mana Lokasi Pusat Pendidikan Pembinaan Masyarakat (Pusdik Binmas)?", "id": "Banyubiru, Semarang" },
  { "en": "Satu Kodi Berapa Buah?", "id": "Dua Puluh Buah" },
  { "en": "Saya Menemukan Dompet Di Jalan.", "id": "Lapor Polisi, Atau Kembalikan" },
  { "en": "Unit Apa Yang Menggunakan Anjing Pelacak?", "id": "Unit Satwa, K-9" },
  { "en": "Empat Ratus Lima Puluh Bagi Sembilan?", "id": "Lima Puluh" },
  { "en": "Apa Antonim Kata Virtual?", "id": "Nyata, Atau Fisik" },
  { "en": "Saya Membantu Rekan Kerja Yang Kerjanya Lambat.", "id": "Ya, Solidaritas Tim" },
  { "en": "Apa Istilah Untuk Bedah Mayat Untuk Kepentingan Hukum?", "id": "Otopsi, Atau Autopsi" },
  { "en": "Rumus Volume Tabung?", "id": "Pi, Kali Jari-Jari Kuadrat, Kali Tinggi" },
  { "en": "Deret Satu, Delapan, Dua Puluh Tujuh, Enam Puluh Empat?", "id": "Seratus Dua Puluh Lima" },
  { "en": "Apa Nama Aplikasi Pembuatan SIM Secara Online?", "id": "Sinar, Digital Korlantas" },
  { "en": "Mikrofon : Suara = Kamera : ...?", "id": "Gambar, Atau Visual" },
  { "en": "Saya Takut Mencoba Karena Takut Gagal.", "id": "Tidak, Gagal Itu Belajar" },
  { "en": "Satuan Apa Yang Menegakkan Disiplin Anggota Polri?", "id": "Provos, Atau Propam" },
  { "en": "Semua Burung Bisa Terbang. Burung Unta Adalah Burung.", "id": "Burung Unta Tidak Bisa Terbang (Pengecualian)" },
  { "en": "Saya Menolak Memberi Uang Damai Saat Ditilang.", "id": "Ya, Patuh Hukum" },
  { "en": "Helm : Kepala = Cincin : ...?", "id": "Jari" },
  { "en": "Siapa Perumus Konsep Catur Prasetya?", "id": "Jenderal Polisi Soekanto" },
  { "en": "Sudut Putaran Seperdelapan Lingkaran?", "id": "Empat Puluh Lima Derajat" },
  { "en": "Apa Antonim Kata Inferior?", "id": "Superior, Atau Unggul" },
  { "en": "Saya Sering Lupa Membawa Dompet.", "id": "Tidak, Saya Teliti" },
  { "en": "Apa Tugas Utama Satuan Tahti?", "id": "Perawatan Tahanan, Dan Barang Bukti" },
  { "en": "Air : Haus = Nasi : ...?", "id": "Lapar" },
  { "en": "Apa Semboyan Polisi Wanita (Polwan)?", "id": "Esthi Bhakti, Warapsari" },
  { "en": "Tujuh Kali Enam Bagi Tiga?", "id": "Empat Belas" },
  { "en": "Apa Sinonim Kata Kapabilitas?", "id": "Kemampuan, Atau Kecakapan" },
  { "en": "Saya Membuang Sampah Sembarangan Jika Tidak Ada Tempat Sampah.", "id": "Tidak, Simpan Dulu" },
  { "en": "Di Mana Lokasi Pusat Pendidikan Intelijen (Pusdik Intelkam)?", "id": "Soreang, Bandung" },
  { "en": "Saya Suka Mengeluh Saat Pekerjaan Banyak.", "id": "Tidak, Saya Profesional" },
  { "en": "Senter : Baterai = Mobil : ...?", "id": "Bensin, Atau Bahan Bakar" },
  { "en": "Apa Warna Pakaian Dinas Upacara (PDU) Polri?", "id": "Cokelat Tua, Lengkap" },
  { "en": "Apa Antonim Kata Emigrasi?", "id": "Imigrasi, Atau Masuk" },
  { "en": "Saya Menjenguk Tetangga Yang Sakit.", "id": "Ya, Kepedulian Sosial" },
  { "en": "Pendidikan Sespimti Polri Di Mana?", "id": "Lembang, Jawa Barat" },
  { "en": "Dokter : Pasien = Guru : ...?", "id": "Murid, Atau Siswa" },
  { "en": "Apa Arti Perisai Pada Lambang Pancasila?", "id": "Perlindungan, Dan Pertahanan" },
  { "en": "Akar Empat Ratus?", "id": "Dua Puluh" },
  { "en": "Apa Sinonim Kata Fleksibel?", "id": "Luwes, Atau Lentur" },
  { "en": "Saya Merasa Selalu Diikuti Orang Jahat.", "id": "Tidak, Itu Paranoid" },
  { "en": "Apa Pangkat Tertinggi Perwira Pertama?", "id": "Ajun Komisaris Polisi" },
  { "en": "Rumus Luas Segitiga?", "id": "Setengah, Alas Kali Tinggi" },
  { "en": "Saya Lebih Suka Bekerja Sendiri Daripada Tim.", "id": "Tidak, Suka Kolaborasi" },
  { "en": "Kepala : Topi = Tangan : ...?", "id": "Sarung Tangan" },
  { "en": "Kapan Hari Brimob Diperingati?", "id": "Empat Belas November" },
  { "en": "Satu Persepuluh Desimalnya Adalah?", "id": "Nol Koma Satu" },
  { "en": "Apa Antonim Kata Defisit?", "id": "Surplus, Atau Kelebihan" },
  { "en": "Saya Mengembalikan Barang Yang Bukan Hak Saya.", "id": "Ya, Integritas" },
  { "en": "Siapa Yang Menerbitkan STNK?", "id": "Samsat, Polri" },
  { "en": "Sapi : Susu = Lebah : ...?", "id": "Madu" },
  { "en": "Siapa Presiden Keenam Indonesia?", "id": "Susilo Bambang Yudhoyono" },
  { "en": "Dua Koma Lima Tambah Tiga Koma Lima?", "id": "Enam" },
  { "en": "Apa Sinonim Kata Orientasi?", "id": "Pandangan, Atau Arah" },
  { "en": "Saya Membanting Barang Saat Emosi.", "id": "Tidak, Pengendalian Diri" },
  { "en": "Siapa Yang Melakukan Patroli Jalan Raya (PJR)?", "id": "Korps Lalu Lintas" },
  { "en": "Saya Suka Menciptakan Ide Baru.", "id": "Ya, Inovatif" },
  { "en": "Motor : Helm = Mobil : ...?", "id": "Sabuk Pengaman" },
  { "en": "Lambang Rantai Emas Sila Keberapa?", "id": "Sila Kedua" },
  { "en": "Enam Pangkat Dua Adalah?", "id": "Tiga Puluh Enam" },
  { "en": "Apa Antonim Kata Manual?", "id": "Otomatis" },
  { "en": "Saya Selalu Datang Lebih Awal.", "id": "Ya, Disiplin Waktu" },
  { "en": "Siapa Yang Mengelola Senjata Api Polri?", "id": "Logistik Polri" },
  { "en": "Berapa Rusuk Pada Bangun Ruang Kerucut?", "id": "Satu Rusuk, Melengkung" },
  { "en": "Apa Nama Satuan Polri Yang Bertugas Di Perairan Dan Udara?", "id": "Korpolairud, Baharkam Polri" },
  { "en": "Saya Suka Menyalahkan Orang Lain Saat Gagal.", "id": "Tidak, Saya Introspeksi Diri" },
  { "en": "Kamera : Fotografer = Kanvas : ...?", "id": "Pelukis" },
  { "en": "Deret Empat, Sembilan, Enam Belas, Dua Puluh Lima?", "id": "Tiga Puluh Enam" },
  { "en": "Apa Warna Syal Yang Identik Dengan Satuan Sabhara?", "id": "Warna Kuning" },
  { "en": "Akar Kuadrat Dari Lima Ratus Tujuh Puluh Enam?", "id": "Dua Puluh Empat" },
  { "en": "Saya Merasa Dendam Jika Disakiti.", "id": "Tidak, Saya Pemaaf" },
  { "en": "Apa Antonim Kata Hipotetis?", "id": "Terbukti, Atau Nyata" },
  { "en": "Jantung : Kardiologi = Syaraf : ...?", "id": "Neurologi" },
  { "en": "Apa Pangkat Polisi Dengan Lambang Satu Bengkok Perak?", "id": "Ajun Inspektur Polisi Dua (Aipda)" },
  { "en": "Nol Koma Delapan Bagi Nol Koma Dua?", "id": "Empat" },
  { "en": "Apa Sinonim Kata Intelek?", "id": "Cendekiawan, Atau Pintar" },
  { "en": "Saya Menggunakan Fasilitas Kantor Untuk Bisnis Pribadi.", "id": "Tidak, Itu Korupsi Waktu" },
  { "en": "Apa Tugas Utama Divisi Hubungan Internasional (Divhubinter)?", "id": "Kerjasama, Dan Interpol" },
  { "en": "Jika Roda Depan Lebih Kecil Dari Roda Belakang, Mana Yang Berputar Lebih Cepat?", "id": "Roda Depan" },
  { "en": "Semua Hakim Adalah Sarjana. Sebagian Sarjana Adalah Dosen.", "id": "Tidak Semua Sarjana Adalah Hakim" },
  { "en": "Saya Merasa Hidup Ini Tidak Ada Harapan.", "id": "Tidak, Selalu Optimis" },
  { "en": "Nelayan : Laut = Petani : ...?", "id": "Sawah, Atau Ladang" },
  { "en": "Apa Makna Warna Merah Pada Lambang Polri?", "id": "Keberanian" },
  { "en": "Dua Puluh Pangkat Dua Bagi Sepuluh?", "id": "Empat Puluh" },
  { "en": "Apa Antonim Kata Sporadis?", "id": "Kontinu, Atau Terus Menerus" },
  { "en": "Saya Selalu Mengembalikan Pinjaman Tepat Waktu.", "id": "Ya, Dapat Dipercaya" },
  { "en": "Tes Kecermatan Angka Dan Huruf Melatih Apa?", "id": "Fokus, Dan Daya Tahan" },
  { "en": "Deret Tiga, Tujuh, Lima Belas, Tiga Puluh Satu?", "id": "Enam Puluh Tiga" },
  { "en": "Ulat : Kepompong = Bayi : ...?", "id": "Anak, Atau Remaja" },
  { "en": "Di Mana Lokasi Sekolah Polisi Negara (SPN) Polda Metro Jaya?", "id": "Lido, Sukabumi" },
  { "en": "Satu Galon Berapa Liter (Standar Indonesia)?", "id": "Sembilan Belas Liter" },
  { "en": "Saya Pernah Berkata Kasar Kepada Teman.", "id": "Pernah, Saya Minta Maaf" },
  { "en": "Apa Sinonim Kata Komitmen?", "id": "Janji, Atau Tekad" },
  { "en": "Unit Apa Yang Menangani Penculikan Anak?", "id": "Reserse Kriminal, Unit PPA" },
  { "en": "Seribu Dua Ratus Bagi Enam Puluh?", "id": "Dua Puluh" },
  { "en": "Apa Antonim Kata Imitasi?", "id": "Asli, Atau Orisinal" },
  { "en": "Saya Iri Melihat Teman Membeli Motor Baru.", "id": "Tidak, Saya Bersyukur" },
  { "en": "Siapa Yang Melakukan Pemeriksaan Kesehatan Calon Polisi?", "id": "Biddokkes" },
  { "en": "Gembok : Kunci = Masalah : ...?", "id": "Solusi" },
  { "en": "Rumus Luas Permukaan Bola?", "id": "Empat, Kali Pi Kali Jari-Jari Kuadrat" },
  { "en": "Saya Suka Membantu Teman Yang Sedang Sakit.", "id": "Ya, Solidaritas" },
  { "en": "Deret Dua Puluh Empat, Dua Belas, Enam?", "id": "Tiga" },
  { "en": "Apa Nama Aplikasi Pengaduan Masyarakat Polri?", "id": "Dumas Presisi" },
  { "en": "Sapi : Herbivora = Harimau : ...?", "id": "Karnivora" },
  { "en": "Apa Sinonim Kata Kronologis?", "id": "Berurutan" },
  { "en": "Siapa Yang Mengawasi Kinerja Polri Dari Luar?", "id": "Kompolnas" },
  { "en": "Jika Lusa Sabtu, Kemarin Hari Apa?", "id": "Rabu" },
  { "en": "Semua Tentara Berbadan Tegap. Polisi Juga Berbadan Tegap.", "id": "Tentara Dan Polisi, Berbadan Tegap" },
  { "en": "Saya Berani Mengambil Risiko Demi Tugas.", "id": "Ya, Dedikasi" },
  { "en": "Telinga : Sunyi = Mata : ...?", "id": "Gelap" },
  { "en": "Siapa Pahlawan Polri Yang Gugur Saat Tsunami Aceh?", "id": "Kombes Pol (Anumerta) Sayed Husain" },
  { "en": "Sudut Putaran Seperlima Lingkaran?", "id": "Tujuh Puluh Dua Derajat" },
  { "en": "Apa Antonim Kata Dinamis?", "id": "Statis, Atau Diam" },
  { "en": "Saya Sering Lupa Membayar Utang.", "id": "Tidak, Itu Kewajiban" },
  { "en": "Apa Tugas Utama Polisi Pariwisata?", "id": "Pengamanan Objek Wisata, Dan Turis" },
  { "en": "Kertas : Sobek = Kaca : ...?", "id": "Pecah" },
  { "en": "Direktorat Binmas Berada Di Bawah Satuan Apa?", "id": "Baharkam Polri" },
  { "en": "Sembilan Kali Tujuh Tambah Tiga?", "id": "Enam Puluh Enam" },
  { "en": "Apa Sinonim Kata Indikasi?", "id": "Petunjuk, Atau Tanda" },
  { "en": "Saya Selalu Mencuci Tangan Sebelum Makan.", "id": "Ya, Pola Hidup Sehat" },
  { "en": "Pendidikan Dasar Bhayangkara (Dasbhara) Berapa Lama?", "id": "Dua Bulan, Atau Tiga Bulan" },
  { "en": "Rumus Keliling Lingkaran Jika Diketahui Diameter?", "id": "Pi, Kali Diameter" },
  { "en": "Saya Tidak Mudah Terprovokasi Berita Hoax.", "id": "Ya, Saring Sebelum Sharing" },
  { "en": "Jarum : Suntik = Pisau : ...?", "id": "Bedah" },
  { "en": "Apa Warna Baret Satuan Provos?", "id": "Biru Muda" },
  { "en": "Satu Lusin Berapa Pasang?", "id": "Enam Pasang" },
  { "en": "Apa Antonim Kata Fana?", "id": "Kekal, Atau Abadi" },
  { "en": "Saya Selalu Menghormati Orang Yang Lebih Tua.", "id": "Ya, Sopan Santun" },
  { "en": "Pusat Sejarah Polri (Pusjarah) Terletak Di Mana?", "id": "Jakarta Selatan" },
  { "en": "Deret Sepuluh, Lima, Dua Koma Lima?", "id": "Satu Koma Dua Lima" },
  { "en": "Buku : Perpustakaan = Uang : ...?", "id": "Bank, Atau Brankas" },
  { "en": "Sila Keberapa Yang Berlambang Bintang?", "id": "Sila Pertama" },
  { "en": "Apa Sinonim Kata Estetika?", "id": "Keindahan" },
  { "en": "Saya Sering Merasa Orang Lain Membicarakan Saya.", "id": "Tidak, Percaya Diri" },
  { "en": "Siapa Yang Memimpin Upacara Di Polres?", "id": "Inspektur Upacara (Kapolres)" },
  { "en": "Rumus Luas Trapesium Siku-Siku?", "id": "Jumlah Sisi Sejajar, Kali Tinggi Bagi Dua" },
  { "en": "Saya Suka Mengerjakan Tugas Kelompok Sendirian.", "id": "Tidak, Harus Berbagi Tugas" },
  { "en": "Kapan Hari Lahir Bhayangkara?", "id": "Satu Juli" },
  { "en": "Tiga Perempat Sama Dengan Berapa Persen?", "id": "Tujuh Puluh Lima Persen" },
  { "en": "Apa Antonim Kata Konkrit?", "id": "Abstrak" },
  { "en": "Saya Tidak Pernah Mengambil Hak Orang Lain.", "id": "Ya, Integritas" },
  { "en": "Siapa Yang Berhak Melakukan Penilangan?", "id": "Polisi Lalu Lintas" },
  { "en": "Beras : Padi = Bensin : ...?", "id": "Minyak Mentah" },
  { "en": "Siapa Wakil Presiden Kedua RI?", "id": "Sri Sultan Hamengkubuwono IX" },
  { "en": "Satu Koma Dua Kali Tiga?", "id": "Tiga Koma Enam" },
  { "en": "Apa Sinonim Kata Fundamental?", "id": "Mendasar, Atau Pokok" },
  { "en": "Saya Sering Membanting Pintu Saat Marah.", "id": "Tidak, Itu Kekanak-kanakan" },
  { "en": "Siapa Yang Menjaga Keamanan Di Bandara?", "id": "Polisi Bandara, Dan Avsec" },
  { "en": "Matahari Terbit Dari Arah?", "id": "Timur" },
  { "en": "Saya Suka Belajar Bahasa Asing.", "id": "Ya, Menambah Wawasan" },
  { "en": "Mobil : Setir = Kapal : ...?", "id": "Kemudi" },
  { "en": "Empat Pangkat Dua Adalah?", "id": "Enam Belas" },
  { "en": "Saya Selalu Datang Lima Menit Lebih Awal.", "id": "Ya, Disiplin" },
  { "en": "Siapa Yang Mengurus Kesehatan Anggota Polri?", "id": "Pusdokkes" }



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
