document.addEventListener('DOMContentLoaded', () => {
    // 1. Mobile Navigation Toggle
    const menuToggle = document.getElementById('menuToggle');
    const navLinks = document.getElementById('navLinks');

    menuToggle.addEventListener('click', () => {
        navLinks.classList.toggle('active');
    });

    // Tutup menu mobile saat link diklik
    navLinks.querySelectorAll('a').forEach(link => {
        link.addEventListener('click', () => navLinks.classList.remove('active'));
    });

    // 2. Konfigurasi Tarif & Satuan Kalkulator
    const pricingConfig = {
        novel: {
            unit: 'Kata',
            min: 500,
            max: 50000,
            step: 500,
            defaultVal: 2500,
            rateEnId: 150,
            rateIdEn: null,
            capacityPerDay: 1500
        },
        manga: {
            unit: 'Halaman',
            min: 10,
            max: 300,
            step: 5,
            defaultVal: 25,
            rateEnId: 6000,
            rateIdEn: null,
            capacityPerDay: 15
        },
        subtitle: {
            unit: 'Menit Video',
            min: 5,
            max: 240,
            step: 5,
            defaultVal: 24,
            rateEnId: 12000,
            rateIdEn: null,
            capacityPerDay: 20
        },
        document: {
            unit: 'Halaman Standar',
            min: 1,
            max: 100,
            step: 1,
            defaultVal: 5,
            rateEnId: 45000,
            rateIdEn: 55000, // Tersedia 2 arah khusus dokumen
            capacityPerDay: 5
        }
    };

    const calcCategory = document.getElementById('calcCategory');
    const btnEnToId = document.getElementById('btnEnToId');
    const btnIdToEn = document.getElementById('btnIdToEn');
    const directionNotice = document.getElementById('directionNotice');
    const wordCountRange = document.getElementById('wordCountRange');
    const unitLabel = document.getElementById('unitLabel');
    const unitValue = document.getElementById('unitValue');
    const minUnit = document.getElementById('minUnit');
    const maxUnit = document.getElementById('maxUnit');
    const totalPrice = document.getElementById('totalPrice');
    const estDays = document.getElementById('estDays');
    const unitRate = document.getElementById('unitRate');

    let currentDirection = 'en-id';

    function formatRupiah(angka) {
        return 'Rp ' + angka.toLocaleString('id-ID');
    }

    function updateDirectionLock() {
        const category = calcCategory.value;
        if (category === 'document') {
            btnIdToEn.disabled = false;
            directionNotice.textContent = '*Kategori Dokumen & Surat Resmi mendukung terjemahan 2 arah (EN ↔ ID).';
        } else {
            btnIdToEn.disabled = true;
            currentDirection = 'en-id';
            btnEnToId.classList.add('active');
            btnIdToEn.classList.remove('active');
            directionNotice.textContent = '*Arah Indonesia → Inggris hanya tersedia untuk kategori Dokumen & Surat Resmi.';
        }
    }

    function resetRangeBounds() {
        const cfg = pricingConfig[calcCategory.value];
        wordCountRange.min = cfg.min;
        wordCountRange.max = cfg.max;
        wordCountRange.step = cfg.step;
        wordCountRange.value = cfg.defaultVal;

        unitLabel.textContent = `Jumlah ${cfg.unit}`;
        minUnit.textContent = `${cfg.min.toLocaleString('id-ID')} ${cfg.unit}`;
        maxUnit.textContent = `${cfg.max.toLocaleString('id-ID')} ${cfg.unit}`;
    }

    function calculateQuote() {
        const category = calcCategory.value;
        const cfg = pricingConfig[category];
        const qty = parseInt(wordCountRange.value, 10);

        const rate = (currentDirection === 'id-en' && cfg.rateIdEn) ? cfg.rateIdEn : cfg.rateEnId;
        const total = qty * rate;
        const days = Math.max(1, Math.ceil(qty / cfg.capacityPerDay));

        unitValue.textContent = `${qty.toLocaleString('id-ID')} ${cfg.unit}`;
        totalPrice.textContent = formatRupiah(total);
        unitRate.textContent = `${formatRupiah(rate)} / ${cfg.unit.toLowerCase()}`;
        estDays.textContent = days === 1 ? '1 Hari Kerja' : `${days} - ${days + 1} Hari Kerja`;
    }

    // Event Listeners Kalkulator
    calcCategory.addEventListener('change', () => {
        updateDirectionLock();
        resetRangeBounds();
        calculateQuote();
    });

    btnEnToId.addEventListener('click', () => {
        currentDirection = 'en-id';
        btnEnToId.classList.add('active');
        btnIdToEn.classList.remove('active');
        calculateQuote();
    });

    btnIdToEn.addEventListener('click', () => {
        if (!btnIdToEn.disabled) {
            currentDirection = 'id-en';
            btnIdToEn.classList.add('active');
            btnEnToId.classList.remove('active');
            calculateQuote();
        }
    });

    wordCountRange.addEventListener('input', calculateQuote);

    // 3. Sinkronisasi ke Form Kontak
    const applyQuoteBtn = document.getElementById('applyQuoteBtn');
    const formCategory = document.getElementById('formCategory');
    const formDirection = document.getElementById('formDirection');
    const clientMessage = document.getElementById('clientMessage');

    function syncFormDirectionLock() {
        const idToEnOption = formDirection.querySelector('option[value="id-en"]');
        if (formCategory.value === 'document') {
            idToEnOption.disabled = false;
        } else {
            idToEnOption.disabled = true;
            formDirection.value = 'en-id';
        }
    }

    formCategory.addEventListener('change', syncFormDirectionLock);

    applyQuoteBtn.addEventListener('click', () => {
        formCategory.value = calcCategory.value;
        syncFormDirectionLock();
        formDirection.value = currentDirection;

        const dirLabel = currentDirection === 'en-id' ? 'Inggris → Indonesia' : 'Indonesia → Inggris';
        clientMessage.value = `Halo Magish Studio, saya ingin mengajukan lokalisasi untuk kategori ${calcCategory.options[calcCategory.selectedIndex].text} (${dirLabel}) dengan volume ${unitValue.textContent}. Estimasi dari kalkulator: ${totalPrice.textContent}.`;

        document.getElementById('kontak').scrollIntoView({ behavior: 'smooth' });
    });

    // 4. Filter Portofolio
    const filterButtons = document.querySelectorAll('.filter-btn');
    const portfolioCards = document.querySelectorAll('.portfolio-card');

    filterButtons.forEach(btn => {
        btn.addEventListener('click', () => {
            filterButtons.forEach(b => b.classList.remove('active'));
            btn.classList.add('active');

            const filter = btn.getAttribute('data-filter');
            portfolioCards.forEach(card => {
                if (filter === 'all' || card.getAttribute('data-category') === filter) {
                    card.classList.remove('hidden');
                } else {
                    card.classList.add('hidden');
                }
            });
        });
    });

    // 5. Submit Form Kontak & Toast
    const contactForm = document.getElementById('contactForm');
    const toast = document.getElementById('toast');

    contactForm.addEventListener('submit', (e) => {
        e.preventDefault();
        toast.classList.add('show');
        contactForm.reset();
        syncFormDirectionLock();

        setTimeout(() => {
            toast.classList.remove('show');
        }, 3500);
    });

    // Inisialisasi awal
    updateDirectionLock();
    resetRangeBounds();
    calculateQuote();
    syncFormDirectionLock();
});