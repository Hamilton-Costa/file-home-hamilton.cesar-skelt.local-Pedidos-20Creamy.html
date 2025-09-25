<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ofertas Especiais</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8;
            color: #1a202c;
        }
        .product-card {
            @apply bg-white p-4 rounded-xl shadow-lg transition-transform transform hover:scale-105 duration-200 flex flex-col items-center text-center;
        }
        .quantity-control button {
            @apply w-8 h-8 rounded-full flex items-center justify-center border border-gray-300;
        }
        .view-container {
            display: none;
        }
        .view-container.active {
            display: block;
        }
    </style>
</head>
<body class="bg-gray-100">

    <header class="bg-white shadow-md py-4">
        <div class="container mx-auto px-4 flex justify-between items-center">
            <h1 class="text-3xl font-bold text-gray-800">Skelt Demos</h1>
            <div class="flex items-center space-x-4">
                <div class="flex items-center space-x-2 bg-gray-100 p-2 rounded-xl border border-gray-200 cursor-pointer" id="cart-icon-container">
                    <i class="fas fa-shopping-cart text-gray-600 text-lg"></i>
                    <span id="cart-count" class="text-xs font-bold text-gray-600">0</span>
                    <span id="cart-total-display" class="font-bold text-gray-800">R$ 0,00</span>
                </div>
            </div>
        </div>
    </header>

    <!-- Product View -->
    <div id="product-view" class="view-container active">
        <main class="container mx-auto px-4 py-8">
            <h2 class="text-4xl font-extrabold text-center mb-10 text-gray-800">Nossos Produtos</h2>
            <div id="product-list" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
                <!-- Product cards will be generated here by JavaScript -->
            </div>
        </main>
    </div>

    <!-- Cart View -->
    <div id="cart-view" class="view-container">
        <div class="container mx-auto px-4 py-8">
            <div class="bg-white p-6 rounded-xl shadow-lg">
                <div class="flex justify-between items-center mb-6 border-b pb-4">
                    <h3 class="text-3xl font-bold">Resumo do Pedido</h3>
                    <button id="back-to-products-btn" class="bg-gray-200 hover:bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded-xl transition-colors duration-200">Voltar aos Produtos</button>
                </div>

                <div id="cart-items-list" class="space-y-4">
                    <!-- Cart items will be generated here -->
                </div>

                <div class="mt-8 pt-6 border-t space-y-2">
                    <div class="flex justify-between font-semibold text-lg">
                        <span>Subtotal:</span>
                        <span id="cart-subtotal">R$ 0,00</span>
                    </div>
                    <div class="flex justify-between font-semibold text-lg text-red-500">
                        <span>Desconto:</span>
                        <span id="cart-discount">- R$ 0,00</span>
                    </div>
                </div>

                <div class="mt-4 flex flex-col space-y-4">
                    <input type="text" id="coupon-input" placeholder="Cupom de Desconto" class="px-4 py-2 border rounded-xl w-full focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <button id="apply-coupon-btn" class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-6 rounded-xl shadow-md transition-all duration-200">Aplicar</button>
                </div>

                <div class="mt-6 pt-4 border-t">
                    <div class="flex justify-between font-extrabold text-2xl mb-4 text-blue-600">
                        <span>Total a Pagar:</span>
                        <span id="cart-total">R$ 0,00</span>
                    </div>
                    <button id="checkout-whatsapp-btn" class="w-full bg-blue-500 hover:bg-blue-600 text-white font-bold py-3 rounded-xl transition-colors duration-200">
                        Fechar Pedido
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        const products = [
            { name: "Limpador Antioleosidade", price: 52.62, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158130-300-300?v=638933018441470000&width=300&height=300&aspect=true" },
            { name: "Lip Balm Incolor", price: 31.49, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158119-300-300?v=638907754361800000&width=300&height=300&aspect=true" },
            { name: "Calming Cream", price: 52.62, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157983-300-300?v=638902569437500000&width=300&height=300&aspect=true" },
            { name: "Gel de Limpeza", price: 63.15, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157830-300-300?v=638905143508700000&width=300&height=300&aspect=true" },
            { name: "Protetor Solar", price: 63.15, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157987-300-300?v=638902569599400000&width=300&height=300&aspect=true" },
            { name: "Ácido Mandélico", price: 84.20, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157961-300-300?v=638902526406700000&width=300&height=300&aspect=true" },
            { name: "Vitamina C", price: 105.25, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158047-300-300?v=638902591541030000&width=300&height=300&aspect=true" },
            { name: "Limpador Glicerinado", price: 63.15, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157835-300-300?v=638901986008330000&width=300&height=300&aspect=true" },
            { name: "Emulsão de Limpeza", price: 63.15, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157995-300-300?v=638902570356770000&width=300&height=300&aspect=true" },
            { name: "Ácido Tranexâmico", price: 94.73, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158013-300-300?v=638902579799130000&width=300&height=300&aspect=true" },
            { name: "Ácido Glicólico", price: 84.20, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158007-300-300?v=638902579335770000&width=300&height=300&aspect=true" },
            { name: "Sérum Hidratante", price: 63.15, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158030-300-300?v=638902590530770000&width=300&height=300&aspect=true" },
            { name: "Vitamina C Gold", price: 126.31, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157878-300-300?v=638901992301470000&width=300&height=300&aspect=true" },
            { name: "Retinol", price: 105.25, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158043-300-300?v=638902590999200000&width=300&height=300&aspect=true" },
            { name: "Ácido Lático", price: 73.67, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157766-300-300?v=638901971806870000&width=300&height=300&aspect=true" },
            { name: "Retinal", price: 136.83, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158038-300-300?v=638902590812670000&width=300&height=300&aspect=true" },
            { name: "Ácido Salicílico", price: 94.73, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157775-300-300?v=638901975006200000&width=300&height=300&aspect=true" },
            { name: "Eye Cream", price: 136.83, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157883-300-300?v=638901992983700000&width=300&height=300&aspect=true" },
            { name: "Ceramide 200ml", price: 42.09, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157971-300-300?v=638902568976470000&width=300&height=300&aspect=true" },
            { name: "Niacinamide", price: 94.73, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157793-300-300?v=638901978865330000&width=300&height=300&aspect=true" },
            { name: "Lip Balms Coffee", price: 36.83, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157951-300-300?v=638902092111130000&width=300&height=300&aspect=true" },
            { name: "Sérum Anti-Aging", price: 73.67, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158034-300-300?v=638902590656430000&width=300&height=300&aspect=true" },
            { name: "Peptide Cream", price: 199.99, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157788-300-300?v=638901977780370000&width=300&height=300&aspect=true" },
            { name: "Glicointense Peel", price: 105.25, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157815-300-300?v=638901983437900000&width=300&height=300&aspect=true" },
            { name: "Pró Adapalenato", price: 126.31, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157820-300-300?v=638901983860800000&width=300&height=300&aspect=true" },
            { name: "Ceramide 400ml", price: 63.15, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157979-300-300?v=638902569292270000&width=300&height=300&aspect=true" },
            { name: "Intensive Repair", price: 105.25, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157843-300-300?v=638901987089200000&width=300&height=300&aspect=true" },
            { name: "Calming Body", price: 73.67, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158051-300-300?v=638902592368130000&width=300&height=300&aspect=true" },
            { name: "Fragrância Cosmic Love", price: 157.88, image: "https://creamyb2b.vtexassets.com/arquivos/ids/158063-300-300?v=638902593054170000&width=300&height=300&aspect=true" },
            { name: "Fragrância Eletric Heart", price: 157.88, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157897-300-300?v=638901994468030000&width=300&height=300&aspect=true" },
            { name: "Creme de Reconstrução Capilar", price: 73.67, image: "https://creamyb2b.vtexassets.com/arquivos/ids/157020-300-300?v=638439495210800000&width=300&height=300&aspect=true" }
        ];

        let cart = {};
        const discountThreshold = 500;
        const autoDiscountRate = 0.25;
        const couponCode = 'GANHE35';
        const couponDiscountRate = 0.35;
        let couponApplied = false;

        const productView = document.getElementById('product-view');
        const cartView = document.getElementById('cart-view');
        const productList = document.getElementById('product-list');
        const cartIconContainer = document.getElementById('cart-icon-container');
        const cartCount = document.getElementById('cart-count');
        const cartTotalDisplay = document.getElementById('cart-total-display');
        const cartItemsList = document.getElementById('cart-items-list');
        const cartSubtotalEl = document.getElementById('cart-subtotal');
        const cartDiscountEl = document.getElementById('cart-discount');
        const cartTotalEl = document.getElementById('cart-total');
        const couponInput = document.getElementById('coupon-input');
        const applyCouponBtn = document.getElementById('apply-coupon-btn');
        const checkoutWhatsappBtn = document.getElementById('checkout-whatsapp-btn');
        const backToProductsBtn = document.getElementById('back-to-products-btn');

        function formatCurrency(value) {
            return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(value);
        }

        function calculateCart() {
            let subtotal = 0;
            let totalItems = 0;
            for (const productId in cart) {
                const item = cart[productId];
                subtotal += item.price * item.quantity;
                totalItems += item.quantity;
            }

            let discount = 0;
            let finalTotal = subtotal;

            if (subtotal >= discountThreshold) {
                if (couponApplied) {
                    discount = subtotal * couponDiscountRate;
                } else {
                    discount = subtotal * autoDiscountRate;
                }
            }

            finalTotal = subtotal - discount;

            // Update header display
            cartCount.textContent = totalItems;
            cartTotalDisplay.textContent = formatCurrency(finalTotal);

            // Update cart page display
            cartSubtotalEl.textContent = formatCurrency(subtotal);
            cartDiscountEl.textContent = `- ${formatCurrency(discount)}`;
            cartTotalEl.textContent = formatCurrency(finalTotal);
        }

        function renderProducts() {
            productList.innerHTML = '';
            products.forEach((product, index) => {
                const card = document.createElement('div');
                card.classList.add('product-card');
                card.innerHTML = `
                    <img src="${product.image}" alt="${product.name}" class="h-40 w-40 object-contain mb-4">
                    <h3 class="text-base font-bold mb-1">${product.name}</h3>
                    <p class="text-sm text-gray-600 mb-4">${formatCurrency(product.price)}</p>
                    <div class="quantity-control flex items-center space-x-2">
                        <button data-id="${index}" class="decrease-qty text-blue-500">-</button>
                        <span id="qty-${index}" class="font-bold w-6 text-center">${cart[index]?.quantity || 0}</span>
                        <button data-id="${index}" class="increase-qty text-blue-500">+</button>
                    </div>
                `;
                productList.appendChild(card);
            });
        }

        function renderCartItems() {
            cartItemsList.innerHTML = '';
            let hasItems = false;
            for (const productId in cart) {
                const item = cart[productId];
                if (item.quantity > 0) {
                    hasItems = true;
                    const cartItemEl = document.createElement('div');
                    cartItemEl.classList.add('flex', 'justify-between', 'items-center', 'py-2', 'border-b', 'last:border-b-0');
                    cartItemEl.innerHTML = `
                        <div class="flex items-center space-x-4">
                            <img src="${item.image}" alt="${item.name}" class="w-16 h-16 object-contain rounded">
                            <div>
                                <h4 class="font-bold">${item.name}</h4>
                                <p class="text-sm text-gray-600">${formatCurrency(item.price)} x ${item.quantity}</p>
                            </div>
                        </div>
                        <span class="font-bold">${formatCurrency(item.price * item.quantity)}</span>
                    `;
                    cartItemsList.appendChild(cartItemEl);
                }
            }

            if (!hasItems) {
                cartItemsList.innerHTML = '<p class="text-center text-gray-500">Seu carrinho está vazio.</p>';
            }
        }

        function toggleView(view) {
            if (view === 'cart') {
                productView.classList.remove('active');
                cartView.classList.add('active');
                renderCartItems();
                calculateCart();
            } else {
                cartView.classList.remove('active');
                productView.classList.add('active');
                renderProducts(); // Re-render products to update quantities
                calculateCart();
            }
        }

        function generateWhatsAppMessage() {
            const phoneNumber = '5541987184505';
            let message = "Olá! Gostaria de fazer o seguinte pedido:%0a%0a";
            let subtotal = 0;
            let hasItems = false;

            for (const productId in cart) {
                const item = cart[productId];
                if (item.quantity > 0) {
                    hasItems = true;
                    message += `* ${item.name} (x${item.quantity}) - ${formatCurrency(item.price * item.quantity)}%0a`;
                    subtotal += item.price * item.quantity;
                }
            }

            if (!hasItems) {
                const popup = document.createElement('div');
                popup.textContent = 'Adicione itens ao carrinho antes de fechar o pedido.';
                popup.style.cssText = `
                    position: fixed;
                    top: 50%;
                    left: 50%;
                    transform: translate(-50%, -50%);
                    background-color: #ef4444;
                    color: white;
                    padding: 20px;
                    border-radius: 10px;
                    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
                    z-index: 1000;
                    text-align: center;
                `;
                document.body.appendChild(popup);
                setTimeout(() => popup.remove(), 2000);
                return;
            }

            let discount = 0;
            if (subtotal >= discountThreshold) {
                if (couponApplied) {
                    discount = subtotal * couponDiscountRate;
                } else {
                    discount = subtotal * autoDiscountRate;
                }
            }

            const finalTotal = subtotal - discount;

            message += `%0aSubtotal: ${formatCurrency(subtotal)}%0a`;
            if (discount > 0) {
                message += `Desconto: - ${formatCurrency(discount)}%0a`;
            }
            message += `*TOTAL: ${formatCurrency(finalTotal)}*%0a%0a`;
            message += "Aguardando confirmação. Obrigado!";

            const whatsappUrl = `https://wa.me/${phoneNumber}?text=${encodeURIComponent(message)}`;
            window.open(whatsappUrl, '_blank');
            toggleView('products');
        }

        applyCouponBtn.addEventListener('click', () => {
            if (couponInput.value.toUpperCase() === couponCode) {
                couponApplied = true;
                calculateCart();
                renderCartItems();
                const popup = document.createElement('div');
                popup.textContent = 'Cupom aplicado com sucesso!';
                popup.style.cssText = `
                    position: fixed;
                    top: 50%;
                    left: 50%;
                    transform: translate(-50%, -50%);
                    background-color: #22c55e;
                    color: white;
                    padding: 20px;
                    border-radius: 10px;
                    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
                    z-index: 1000;
                    text-align: center;
                `;
                document.body.appendChild(popup);
                setTimeout(() => popup.remove(), 2000);
            } else {
                couponApplied = false;
                calculateCart();
                renderCartItems();
                const popup = document.createElement('div');
                popup.textContent = 'Cupom inválido.';
                popup.style.cssText = `
                    position: fixed;
                    top: 50%;
                    left: 50%;
                    transform: translate(-50%, -50%);
                    background-color: #ef4444;
                    color: white;
                    padding: 20px;
                    border-radius: 10px;
                    box-shadow: 0 4px 6px rgba(0,0,0,0.1);
                    z-index: 1000;
                    text-align: center;
                `;
                document.body.appendChild(popup);
                setTimeout(() => popup.remove(), 2000);
            }
        });

        // Event listeners
        cartIconContainer.addEventListener('click', () => toggleView('cart'));
        backToProductsBtn.addEventListener('click', () => toggleView('products'));
        checkoutWhatsappBtn.addEventListener('click', generateWhatsAppMessage);

        document.addEventListener('DOMContentLoaded', () => {
            // Initialize cart state
            products.forEach((product, index) => {
                cart[index] = { ...product, quantity: 0 };
            });
            renderProducts();
            calculateCart();
        });

        // Add event listener to a parent element to handle dynamically added buttons
        document.addEventListener('click', (e) => {
            if (e.target.classList.contains('increase-qty')) {
                const id = e.target.dataset.id;
                cart[id].quantity++;
                document.getElementById(`qty-${id}`).textContent = cart[id].quantity;
                calculateCart();
            } else if (e.target.classList.contains('decrease-qty')) {
                const id = e.target.dataset.id;
                if (cart[id].quantity > 0) {
                    cart[id].quantity--;
                    document.getElementById(`qty-${id}`).textContent = cart[id].quantity;
                    calculateCart();
                }
            }
        });
    </script>

</body>
</html>
