<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Rasa Nusantara - Pesan Makan Online</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Poppins', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#fff1f2',
                            100: '#ffe4e6',
                            500: '#f43f5e',
                            600: '#e11d48',
                            700: '#be123c',
                        }
                    }
                }
            }
        }
    </script>
    <style>
        body { font-family: 'Poppins', sans-serif; scroll-behavior: smooth; }
        .cart-overlay { background-color: rgba(0, 0, 0, 0.5); }
        .hide-scrollbar::-webkit-scrollbar { display: none; }
        .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">

    <nav class="bg-white shadow-sm fixed w-full z-40 top-0">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-16">
                <div class="flex items-center gap-2 cursor-pointer" onclick="window.scrollTo(0,0)">
                    <i class="fa-solid fa-fire-burner text-brand-500 text-2xl"></i>
                    <span class="font-bold text-xl tracking-tight text-gray-900">Rasa Nusantara</span>
                </div>
                <div>
                    <button onclick="toggleCart()" class="relative p-2 text-gray-600 hover:text-brand-500 transition-colors focus:outline-none">
                        <i class="fa-solid fa-cart-shopping text-xl"></i>
                        <span id="cart-badge" class="absolute top-0 right-0 inline-flex items-center justify-center px-2 py-1 text-xs font-bold leading-none text-white transform translate-x-1/4 -translate-y-1/4 bg-brand-500 rounded-full hidden">0</span>
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <header class="pt-24 pb-12 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto text-center">
        <h1 class="text-4xl md:text-5xl font-extrabold text-gray-900 mb-4 tracking-tight">
            Sensasi Kuliner <span class="text-brand-500">Khas Indonesia</span>
        </h1>
        <p class="text-lg text-gray-600 max-w-2xl mx-auto mb-8">
            Pesan hidangan favoritmu sekarang. Nikmati kelezatan rempah pilihan langsung dari dapur kami ke mejamu.
        </p>
        <a href="#menu" class="inline-flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-full text-white bg-brand-500 hover:bg-brand-600 transition-colors shadow-lg shadow-brand-500/30">
            Lihat Menu
        </a>
    </header>

    <main id="menu" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <div class="flex items-center justify-between mb-8">
            <h2 class="text-2xl font-bold text-gray-900">Menu Spesial Kami</h2>
        </div>
        
        <!-- Menu Grid -->
        <div id="product-container" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
            <!-- Products will be injected here by JS -->
        </div>
    </main>

    <div id="cart-sidebar-overlay" class="fixed inset-0 cart-overlay z-50 hidden transition-opacity duration-300 opacity-0" onclick="toggleCart()"></div>
    
    <div id="cart-sidebar" class="fixed inset-y-0 right-0 max-w-md w-full bg-white shadow-2xl z-50 transform translate-x-full transition-transform duration-300 ease-in-out flex flex-col">
        <div class="flex items-center justify-between p-4 border-b">
            <h2 class="text-lg font-bold flex items-center gap-2">
                <i class="fa-solid fa-basket-shopping text-brand-500"></i> Keranjang Anda
            </h2>
            <button onclick="toggleCart()" class="text-gray-400 hover:text-gray-600 focus:outline-none">
                <i class="fa-solid fa-xmark text-xl"></i>
            </button>
        </div>
        
        <!-- Cart Items -->
        <div id="cart-items" class="flex-1 overflow-y-auto p-4 hide-scrollbar flex flex-col gap-4">
            <!-- Cart empty state -->
            <div id="empty-cart-msg" class="flex flex-col items-center justify-center h-full text-gray-400">
                <i class="fa-solid fa-cart-arrow-down text-5xl mb-4 text-gray-200"></i>
                <p>Keranjang belanja masih kosong.</p>
            </div>
            <!-- Dynamic cart items will go here -->
        </div>
        
        <!-- Checkout Section -->
        <div class="border-t p-4 bg-gray-50">
            <div class="flex justify-between items-center mb-4 text-lg font-bold text-gray-900">
                <span>Total:</span>
                <span id="cart-total">Rp 0</span>
            </div>
            <button id="checkout-btn" onclick="checkoutWhatsApp()" disabled class="w-full flex items-center justify-center gap-2 bg-[#25D366] hover:bg-[#128C7E] disabled:bg-gray-300 disabled:cursor-not-allowed text-white py-3 px-4 rounded-xl font-semibold transition-colors shadow-lg shadow-green-500/20">
                <i class="fa-brands fa-whatsapp text-xl"></i>
                Pesan via WhatsApp
            </button>
            <p class="text-xs text-center text-gray-500 mt-3">
                Pembayaran dilakukan setelah konfirmasi pesanan.
            </p>
        </div>
    </div>

    <div id="toast-container" class="fixed bottom-4 left-1/2 transform -translate-x-1/2 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <footer class="bg-gray-900 text-white py-8 mt-12">
        <div class="max-w-7xl mx-auto px-4 text-center">
            <div class="flex items-center justify-center gap-2 mb-4 text-xl font-bold">
                <i class="fa-solid fa-fire-burner text-brand-500"></i>
                <span>Rasa Nusantara</span>
            </div>
            <p class="text-gray-400 text-sm">© 2026 Rasa Nusantara. Hak Cipta Dilindungi.</p>
        </div>
    </footer>

    <script>
        // Data Produk
        const products = [
            {
                id: 1,
                name: 'Nasi Goreng Spesial',
                description: 'Nasi goreng dengan telur, suwiran ayam, sosis, dan kerupuk renyah.',
                price: 25000,
                image: 'https://images.unsplash.com/photo-1603133872878-684f208fb84b?ixlib=rb-4.0.3&auto=format&fit=crop&w=500&q=80'
            },
            {
                id: 2,
                name: 'Mie Goreng Jawa',
                description: 'Mie goreng manis gurih khas Jawa dengan sayuran segar dan telur.',
                price: 22000,
                image: 'https://images.unsplash.com/photo-1585032226651-759b368d7246?ixlib=rb-4.0.3&auto=format&fit=crop&w=500&q=80'
            },
            {
                id: 3,
                name: 'Ayam Bakar Madu',
                description: 'Ayam bakar dengan olesan madu murni, disajikan dengan sambal dan lalapan.',
                price: 35000,
                image: 'https://images.unsplash.com/photo-1604908176997-125f25cc6f3d?ixlib=rb-4.0.3&auto=format&fit=crop&w=500&q=80'
            },
            {
                id: 4,
                name: 'Sate Ayam Madura',
                description: '10 Tusuk sate ayam empuk dengan baluran bumbu kacang yang legit.',
                price: 30000,
                image: 'https://images.unsplash.com/photo-1555939594-58d7cb561ad1?ixlib=rb-4.0.3&auto=format&fit=crop&w=500&q=80'
            }
        ];

        let cart = [];
        let isCartOpen = false;

        function formatRupiah(number) {
            return new Intl.NumberFormat('id-ID', { style: 'currency', currency: 'IDR', minimumFractionDigits: 0 }).format(number);
        }

        function renderProducts() {
            const container = document.getElementById('product-container');
            container.innerHTML = '';

            products.forEach(product => {
                const card = document.createElement('div');
                card.className = 'bg-white rounded-2xl shadow-sm hover:shadow-md transition-shadow overflow-hidden border border-gray-100 flex flex-col';
                card.innerHTML = `
                    <div class="relative h-48 overflow-hidden">
                        <img src="${product.image}" alt="${product.name}" class="w-full h-full object-cover transform hover:scale-105 transition-transform duration-500">
                    </div>
                    <div class="p-5 flex-1 flex flex-col">
                        <h3 class="text-lg font-bold text-gray-900 mb-1">${product.name}</h3>
                        <p class="text-sm text-gray-500 line-clamp-2 mb-4 flex-1">${product.description}</p>
                        <div class="flex items-center justify-between mt-auto">
                            <span class="font-bold text-brand-600 text-lg">${formatRupiah(product.price)}</span>
                            <button onclick="addToCart(${product.id})" class="bg-gray-900 hover:bg-brand-500 text-white w-10 h-10 rounded-full flex items-center justify-center transition-colors focus:outline-none focus:ring-2 focus:ring-brand-500 focus:ring-offset-2">
                                <i class="fa-solid fa-plus"></i>
                            </button>
                        </div>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        function addToCart(productId) {
            const product = products.find(p => p.id === productId);
            const existingItem = cart.find(item => item.id === productId);

            if (existingItem) {
                existingItem.quantity += 1;
            } else {
                cart.push({ ...product, quantity: 1 });
            }

            updateCartUI();
            showToast(`${product.name} ditambahkan!`);
        }

        function updateQuantity(productId, delta) {
            const item = cart.find(i => i.id === productId);
            if (item) {
                item.quantity += delta;
                if (item.quantity <= 0) {
                    cart = cart.filter(i => i.id !== productId);
                }
                updateCartUI();
            }
        }

        function toggleCart() {
            const sidebar = document.getElementById('cart-sidebar');
            const overlay = document.getElementById('cart-sidebar-overlay');
            
            isCartOpen = !isCartOpen;
            
            if (isCartOpen) {
                sidebar.classList.remove('translate-x-full');
                overlay.classList.remove('hidden');
                setTimeout(() => overlay.classList.remove('opacity-0'), 10);
                document.body.style.overflow = 'hidden'; // Prevent scrolling
            } else {
                sidebar.classList.add('translate-x-full');
                overlay.classList.add('opacity-0');
                setTimeout(() => overlay.classList.add('hidden'), 300);
                document.body.style.overflow = '';
            }
        }

        function updateCartUI() {
            const cartItemsContainer = document.getElementById('cart-items');
            const emptyMsg = document.getElementById('empty-cart-msg');
            const cartBadge = document.getElementById('cart-badge');
            const cartTotal = document.getElementById('cart-total');
            const checkoutBtn = document.getElementById('checkout-btn');

            // Update Badge
            const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
            if (totalItems > 0) {
                cartBadge.textContent = totalItems;
                cartBadge.classList.remove('hidden');
            } else {
                cartBadge.classList.add('hidden');
            }

            // Calculate Total
            const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
            cartTotal.textContent = formatRupiah(total);

            // Toggle Checkout Button
            checkoutBtn.disabled = cart.length === 0;

            // Render Items
            if (cart.length === 0) {
                cartItemsContainer.innerHTML = '';
                cartItemsContainer.appendChild(emptyMsg);
                emptyMsg.style.display = 'flex';
            } else {
                emptyMsg.style.display = 'none';
                cartItemsContainer.innerHTML = '';
                
                cart.forEach(item => {
                    const itemEl = document.createElement('div');
                    itemEl.className = 'flex gap-3 bg-white p-3 rounded-xl border border-gray-100 shadow-sm';
                    itemEl.innerHTML = `
                        <img src="${item.image}" alt="${item.name}" class="w-16 h-16 object-cover rounded-lg">
                        <div class="flex-1 flex flex-col justify-between">
                            <h4 class="font-semibold text-gray-900 text-sm leading-tight">${item.name}</h4>
                            <div class="font-medium text-brand-600 text-sm">${formatRupiah(item.price)}</div>
                        </div>
                        <div class="flex flex-col items-center justify-between">
                            <button onclick="updateQuantity(${item.id}, 1)" class="text-gray-400 hover:text-gray-800 bg-gray-50 hover:bg-gray-100 w-6 h-6 rounded flex items-center justify-center transition-colors"><i class="fa-solid fa-plus text-xs"></i></button>
                            <span class="text-sm font-bold w-6 text-center">${item.quantity}</span>
                            <button onclick="updateQuantity(${item.id}, -1)" class="text-gray-400 hover:text-gray-800 bg-gray-50 hover:bg-gray-100 w-6 h-6 rounded flex items-center justify-center transition-colors"><i class="fa-solid fa-minus text-xs"></i></button>
                        </div>
                    `;
                    cartItemsContainer.appendChild(itemEl);
                });
            }
        }

        function showToast(message) {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = 'bg-gray-900 text-white px-4 py-2 rounded-full shadow-lg transform transition-all duration-300 translate-y-10 opacity-0 flex items-center gap-2 text-sm';
            toast.innerHTML = `<i class="fa-solid fa-check-circle text-green-400"></i> ${message}`;
            
            container.appendChild(toast);
            
            // Animate in
            requestAnimationFrame(() => {
                toast.classList.remove('translate-y-10', 'opacity-0');
            });

            // Remove after 2 seconds
            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => {
                    if (container.contains(toast)) {
                        container.removeChild(toast);
                    }
                }, 300);
            }, 2000);
        }

        function checkoutWhatsApp() {
            if (cart.length === 0) return;

            const waNumber = "62895402925717"; // Nomor yang direquest: 0895402925717
            let total = 0;
            
            // Format waktu saat ini
            const now = new Date();
            const dateStr = now.toLocaleDateString('id-ID', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });

            let message = `*HALO, SAYA INGIN MEMESAN MAKANAN* ??\n`;
            message += `Tanggal: ${dateStr}\n`;
            message += `-----------------------------------\n\n`;
            message += `*RINCIAN PESANAN:*\n`;

            cart.forEach((item, index) => {
                const subtotal = item.price * item.quantity;
                total += subtotal;
                message += `${index + 1}. ${item.name}\n`;
                message += `   ${item.quantity} x ${formatRupiah(item.price)} = *${formatRupiah(subtotal)}*\n`;
            });

            message += `\n-----------------------------------\n`;
            message += `*TOTAL TAGIHAN: ${formatRupiah(total)}*\n`;
            message += `-----------------------------------\n\n`;
            message += `Mohon konfirmasi pesanan ini dan informasikan cara pembayarannya. Terima kasih! ??`;

            // Encode text untuk URL
            const encodedMessage = encodeURIComponent(message);
            const waURL = `https://wa.me/${waNumber}?text=${encodedMessage}`;

            // Buka WhatsApp di tab baru
            window.open(waURL, "_blank");
            
            // Opsional: Tutup keranjang setelah checkout
            toggleCart();
        }

        window.onload = () => {
            renderProducts();
            updateCartUI();
        };

    </script>
</body>
</html>
