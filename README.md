<meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- html2pdf.js for Invoice PDF Generation -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
  <style>
    * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
    body { margin: 0; background-color: #f4f6f8; color: #333; }

    /* Top Offer Banner */
    .top-banner { background-color: #08511a; color: #fff; font-size: 13px; text-align: center; padding: 6px; font-weight: 500; }

    /* Main Header */
    header { background-color: #0d7326; color: white; padding: 12px 16px; position: sticky; top: 0; z-index: 100; box-shadow: 0 2px 8px rgba(0,0,0,0.15); }
    .header-top { display: flex; justify-content: space-between; align-items: center; }
    .site-title { margin: 0; font-size: 20px; font-weight: bold; }
    .header-icons { display: flex; align-items: center; gap: 12px; }

    .icon-btn { background: none; border: none; color: white; font-size: 20px; cursor: pointer; position: relative; padding: 0; }
    .badge { position: absolute; top: -6px; right: -8px; background-color: #e74c3c; color: white; border-radius: 50%; padding: 2px 6px; font-size: 11px; font-weight: bold; }

    .admin-btn { background-color: #ff9800; color: white; border: none; padding: 6px 12px; border-radius: 20px; font-size: 12px; font-weight: bold; cursor: pointer; transition: 0.2s; }
    .admin-btn:hover { background-color: #e68a00; }

    /* Location & Search Bar */
    .location-bar { margin-top: 8px; display: flex; align-items: center; font-size: 13px; background: rgba(255,255,255,0.18); padding: 6px 12px; border-radius: 6px; cursor: pointer; }
    .search-box { margin-top: 10px; }
    .search-box input { width: 100%; padding: 10px 16px; border-radius: 20px; border: none; outline: none; font-size: 14px; box-shadow: 0 2px 4px rgba(0,0,0,0.08); }

    /* Hero Banner */
    .hero-headline { background: linear-gradient(135deg, #0d7326, #1b5e20); color: white; text-align: center; padding: 22px 15px; margin-bottom: 15px; }
    .hero-headline h1 { margin: 0 0 6px 0; font-size: 22px; }
    .hero-headline p { margin: 0; font-size: 13px; opacity: 0.9; }

    /* Product Grid */
    .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(155px, 1fr)); gap: 15px; padding: 0 15px 25px 15px; }
    .product-card { background: white; border: 1px solid #e2e8f0; border-radius: 10px; padding: 12px; text-align: center; box-shadow: 0 2px 5px rgba(0,0,0,0.03); display: flex; flex-direction: column; justify-content: space-between; }
    .product-img { width: 100%; height: 110px; background-color: #f8fafc; border-radius: 8px; margin-bottom: 8px; object-fit: cover; }
    .product-category { font-size: 10px; text-transform: uppercase; color: #0d7326; font-weight: bold; margin-bottom: 4px; display: block; }
    .btn-add { background-color: #0d7326; color: white; border: none; padding: 8px; border-radius: 6px; width: 100%; font-weight: bold; cursor: pointer; }
    .btn-add:hover { background-color: #0a5c1e; }

    /* Side Drawers */
    .drawer { position: fixed; top: 0; right: -340px; width: 320px; height: 100%; background: white; box-shadow: -2px 0 12px rgba(0,0,0,0.2); z-index: 200; transition: right 0.3s ease; padding: 16px; display: flex; flex-direction: column; }
    .drawer.open { right: 0; }
    .drawer-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #eee; padding-bottom: 10px; }
    .close-btn { background: none; border: none; font-size: 20px; cursor: pointer; color: #666; }

    .cart-item { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #eee; padding: 10px 0; }
    .qty-controls { display: flex; align-items: center; gap: 6px; margin-top: 6px; }
    .qty-btn { background: #edf2f7; border: none; width: 24px; height: 24px; border-radius: 4px; font-weight: bold; cursor: pointer; }
    .delete-btn { background: #fff5f5; color: #e53e3e; border: 1px solid #fed7d7; border-radius: 6px; padding: 4px 8px; font-size: 12px; cursor: pointer; }

    /* Modals & Admin Overlay */
    .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); display: none; justify-content: center; align-items: center; z-index: 300; }
    .modal { background: white; padding: 20px; border-radius: 12px; width: 92%; max-width: 450px; max-height: 90vh; overflow-y: auto; }
    .modal label { font-size: 12px; font-weight: bold; color: #4a5568; margin-top: 8px; display: block; }
    .modal input, .modal select { width: 100%; padding: 9px; margin-top: 4px; margin-bottom: 8px; border: 1px solid #cbd5e0; border-radius: 6px; font-size: 13px; }

    .admin-tab-btn { padding: 6px 10px; border: 1px solid #0d7326; background: #fff; color: #0d7326; font-weight: bold; border-radius: 6px; cursor: pointer; font-size: 11px; }
    .admin-tab-btn.active { background: #0d7326; color: white; }

    /* Hidden Printable Invoice Template */
    #invoice-template { display: none; padding: 20px; font-family: sans-serif; }
  </style>
</head>
<body>

  <!-- Top Banner -->
  <div class="top-banner">⚡ ১০ মিনিটে লোকাল হোম ডেলিভারি - The MDS Grocery</div>

  <!-- 1. Header -->
  <header>
    <div class="header-top">
      <h2 class="site-title">The MDS Grocery</h2>
      <div class="header-icons">
        <button class="admin-btn" onclick="checkAdminPassword()">⚙️ Admin Panel</button>
        <button class="icon-btn" onclick="toggleCart()">
          🛒 <span class="badge" id="cartCount">0</span>
        </button>
        <button class="icon-btn" onclick="toggleMenu()">⋮</button>
      </div>
    </div>

    <div class="location-bar" onclick="openAddressModal()">
      📍 <span id="currentLocation">লোকেশন সেট করুন (পিনকোড / এলাকা)</span> ▾
    </div>

    <div class="search-box">
      <input type="text" id="searchInput" onkeyup="filterProducts()" placeholder="পণ্য খুঁজুন (যেমন: চাল, ডাল, স্ন্যাক্স)...">
    </div>
  </header>

  <!-- 2. Hero Headline Banner -->
  <div class="hero-headline">
    <h1>সবচেয়ে কম দামে সেরা মুদি পণ্য!</h1>
    <p>আপনার ঘরের নিত্যপ্রয়োজনীয় জিনিসপত্র পান দ্রুততম সময়ে</p>
  </div>

  <!-- 3. Product Grid -->
  <div class="product-grid" id="productGridContainer">
    <!-- Dynamic Products -->
  </div>

  <!-- 4. Cart Side Drawer -->
  <div class="drawer" id="cartDrawer">
    <div class="drawer-header">
      <h3 style="margin:0;">আপনার কার্ট</h3>
      <button class="close-btn" onclick="toggleCart()">✕</button>
    </div>
    <div style="flex:1; overflow-y:auto; margin-top:10px;" id="cartItems">
      <p style="text-align:center; color:#777; margin-top:30px;">কার্ট খালি রয়েছে</p>
    </div>
    <div id="cartFooter" style="display:none; border-top:1px solid #eee; padding-top:10px;">
      <p style="display:flex; justify-content:space-between; font-size:16px;"><b>মোট:</b> <b id="totalPrice">₹0</b></p>
      <button class="btn-add" onclick="openAddressModal()">অর্ডার সম্পন্ন করুন</button>
    </div>
  </div>

  <!-- 5. Three-Dot Menu Drawer -->
  <div class="drawer" id="menuDrawer">
    <div class="drawer-header">
      <h3 style="margin:0;">মেনু</h3>
      <button class="close-btn" onclick="toggleMenu()">✕</button>
    </div>
    <ul style="list-style:none; padding:0; margin-top:15px;">
      <li style="padding:12px 0; border-bottom:1px solid #f0f0f0;">⚙️ <a href="#" onclick="checkAdminPassword(); toggleMenu();" style="color:#0d7326; font-weight:bold; text-decoration:none;">এডমিন প্যানেল (Admin)</a></li>
      <li style="padding:12px 0; border-bottom:1px solid #f0f0f0;">🎧 কাস্টমার সাপোর্ট</li>
      <li style="padding:12px 0; border-bottom:1px solid #f0f0f0;">📸 ফলো: <a href="https://www.instagram.com/themdsgrocery" target="_blank" style="color:#0d7326; font-weight:bold; text-decoration:none;">@themdsgrocery</a></li>
    </ul>
  </div>

  <!-- 6. Customer Checkout / Location Modal -->
  <div class="modal-overlay" id="addressModal">
    <div class="modal">
      <h3 style="margin-top:0; color:#0d7326;">📍 ডেলিভারি তথ্য ও অর্ডার</h3>
      <label>আপনার নাম:</label>
      <input type="text" id="custName" placeholder="পূর্ণ নাম লিখুন">
      
      <label>ফোন নম্বর:</label>
      <input type="text" id="custPhone" placeholder="১০ সংখ্যার ফোন নম্বর">
      
      <label>গ্রাম / এলাকার নাম:</label>
      <input type="text" id="custArea" placeholder="এলাকা বা ল্যান্ডমার্ক">
      
      <label>পিনকোড:</label>
      <input type="text" id="custPin" placeholder="৬ সংখ্যার পিনকোড">

      <button class="btn-add" style="margin-top:12px;" onclick="submitOrder()">কনফার্ম অর্ডার করুন</button>
      <button class="btn-add" style="background:#e2e8f0; color:#333; margin-top:6px;" onclick="closeAddressModal()">ক্যান্সেল</button>
    </div>
  </div>

  <!-- 7. Admin Panel Modal -->
  <div class="modal-overlay" id="adminModal">
    <div class="modal" style="max-width:500px;">
      <div style="display:flex; justify-content:space-between; align-items:center; border-bottom:1px solid #eee; padding-bottom:8px;">
        <h3 style="margin:0; color:#0d7326;">⚙️ এডমিন প্যানেল</h3>
        <button class="close-btn" onclick="closeAdminModal()">✕</button>
      </div>

      <div style="display:flex; gap:6px; margin:15px 0;">
        <button class="admin-tab-btn active" id="tabAddBtn" onclick="switchAdminTab('add')">➕ প্রোডাক্ট আপলোড</button>
        <button class="admin-tab-btn" id="tabManageBtn" onclick="switchAdminTab('manage')">🗑️ প্রোডাক্ট লিস্ট / বাদ দিন</button>
        <button class="admin-tab-btn" id="tabOrderBtn" onclick="switchAdminTab('orders')">📦 অর্ডারসমূহ</button>
      </div>

      <!-- Tab 1: Product Upload -->
      <div id="adminAddSection">
        <label>প্রোডাক্টের নাম:</label>
        <input type="text" id="pName" placeholder="যেমন: কুরকুরে স্ন্যাক্স, সরষের তেল">

        <label>ক্যাটাগরি:</label>
        <select id="pCategory">
          <option value="স্ন্যাক্স">স্ন্যাক্স & বিস্কুট</option>
          <option value="গ্রোসারী">গ্রোসারী & চাল-ডাল</option>
          <option value="পানীয়">পানীয় & জুস</option>
          <option value="ব্যক্তিগত যত্ন">ব্যক্তিগত যত্ন & সাবান</option>
        </select>

        <div style="display:flex; gap:10px;">
          <div style="flex:1;">
            <label>একক টাইপ:</label>
            <select id="pUnitType">
              <option value="গ্রাম">গ্রাম (g)</option>
              <option value="কেজি">কেজি (kg)</option>
              <option value="লিটার">লিটার (L)</option>
              <option value="পিস">পিস (Pcs)</option>
              <option value="জার">জার (Jar)</option>
            </select>
          </div>
          <div style="flex:1;">
            <label>পরিমাণ কাস্টমাইজ:</label>
            <input type="text" id="pCustomWeight" placeholder="যেমন: ১০০ গ্রাম, ৫ কেজি">
          </div>
        </div>

        <label>মূল্য (₹ টাকা):</label>
        <input type="number" id="pPrice" placeholder="যেমন: 20, 455">

        <label>প্রোডাক্টের ছবি (Upload / URL):</label>
        <input type="file" id="pImgFile" accept="image/*" onchange="convertImageToBase64(this)">
        <small style="color:#777;">অথবা ছবির লিংক (URL) দিন:</small>
        <input type="text" id="pImgUrl" placeholder="https://example.com/image.jpg">

        <button class="btn-add" style="margin-top:15px;" onclick="addNewProduct()">পণ্য আপলোড করুন</button>
      </div>

      <!-- Tab 2: Manage / Delete Products -->
      <div id="adminManageSection" style="display:none;">
        <p style="font-size:12px; color:#666; margin-top:0;">ভুল প্রোডাক্ট মুছে ফেলতে 'ডিলিট' বাটনে চাপুন:</p>
        <div id="adminProductList"></div>
      </div>

      <!-- Tab 3: Orders & Invoice Download -->
      <div id="adminOrderSection" style="display:none;">
        <div id="adminOrdersList">
          <p style="text-align:center; color:#777;">এখনো কোনো অর্ডার আসেনি</p>
        </div>
      </div>

    </div>
  </div>

  <!-- Hidden Printable PDF Cash Memo Template -->
  <div id="invoice-template">
    <div style="text-align:center; border-bottom:2px solid #0d7326; padding-bottom:10px; margin-bottom:15px;">
      <h2 style="color:#0d7326; margin:0;">The MDS Grocery</h2>
      <p style="margin:3px 0; font-size:12px;">১০ মিনিটে লোকাল হোম ডেলিভারি</p>
      <p style="margin:0; font-size:12px;">ক্যাশ মেমো / ইনভয়েস বিল</p>
    </div>

    <div style="display:flex; justify-content:space-between; font-size:12px; margin-bottom:15px;" id="invCustDetails"></div>

    <table style="width:100%; border-collapse:collapse; font-size:12px; margin-bottom:15px;">
      <thead>
        <tr style="background:#0d7326; color:white;">
          <th style="padding:6px; border:1px solid #ccc; text-align:left;">পণ্য</th>
          <th style="padding:6px; border:1px solid #ccc; text-align:center;">পরিমাণ</th>
          <th style="padding:6px; border:1px solid #ccc; text-align:right;">দাম</th>
          <th style="padding:6px; border:1px solid #ccc; text-align:right;">মোট</th>
        </tr>
      </thead>
      <tbody id="invTableItems"></tbody>
    </table>

    <div style="text-align:right; font-size:14px; font-weight:bold; margin-top:10px;">
      সর্বমোট পরিশোধযোগ্য মূল্য: <span id="invTotalVal" style="color:#0d7326;">₹0</span>
    </div>

    <div style="margin-top:30px; text-align:center; font-size:11px; color:#555;">
      *** The MDS Grocery থেকে কেনাকাটা করার জন্য ধন্যবাদ! ***
    </div>
  </div>

  <script>
    const ADMIN_PASS = 'themds@8101@2026';

    let products = JSON.parse(localStorage.getItem('mds_products')) || [
      { id: 1, name: 'কুরকুরে স্ন্যাক্স', category: 'স্ন্যাক্স', weight: '১০০ গ্রাম', price: 20, img: 'https://via.placeholder.com/150?text=Kurkure' },
      { id: 2, name: 'Gsgss', category: 'গ্রোসারী', weight: '100 কেজি', price: 455, img: 'https://via.placeholder.com/150?text=Grocery' }
    ];

    let cart = [];
    let orders = JSON.parse(localStorage.getItem('mds_orders')) || [];
    let uploadedBase64Img = '';

    function saveData() {
      localStorage.setItem('mds_products', JSON.stringify(products));
      localStorage.setItem('mds_orders', JSON.stringify(orders));
    }

    // Check Admin Password
    function checkAdminPassword() {
      let pass = prompt('এডমিন প্যানেলে প্রবেশ করতে পাসওয়ার্ড লিখুন:');
      if (pass === ADMIN_PASS) {
        openAdminModal();
      } else if (pass !== null) {
        alert('ভুল পাসওয়ার্ড! আবার চেষ্টা করুন।');
      }
    }

    // Render Store Products
    function renderProducts(listToRender = products) {
      let grid = document.getElementById('productGridContainer');
      if(listToRender.length === 0) {
        grid.innerHTML = '<p style="grid-column: 1/-1; text-align:center; color:#777;">কোনো পণ্য পাওয়া যায়নি</p>';
        return;
      }
      grid.innerHTML = listToRender.map(p => `
        <div class="product-card">
          <div>
            <img src="${p.img}" class="product-img" onerror="this.src='https://via.placeholder.com/150?text=No+Image'">
            <span class="product-category">${p.category || 'সাধারণ'}</span>
            <h4 style="margin: 4px 0;">${p.name}</h4>
            <p style="color:#666; font-size:12px; margin:0 0 6px 0;">${p.weight}</p>
          </div>
          <div>
            <p style="margin:0 0 8px 0; font-size:16px;"><b>₹${p.price}</b></p>
            <button class="btn-add" onclick="addToCart('${p.name}', ${p.price})">অর্ডার করুন</button>
          </div>
        </div>
      `).join('');
    }

    function filterProducts() {
      let q = document.getElementById('searchInput').value.toLowerCase();
      let filtered = products.filter(p => p.name.toLowerCase().includes(q) || (p.category && p.category.toLowerCase().includes(q)));
      renderProducts(filtered);
    }

    // Cart Functions
    function toggleCart() { document.getElementById('cartDrawer').classList.toggle('open'); }
    function toggleMenu() { document.getElementById('menuDrawer').classList.toggle('open'); }
    function openAddressModal() {
      if(cart.length === 0) { alert('আপনার কার্ট খালি!'); return; }
      document.getElementById('addressModal').style.display = 'flex';
    }
    function closeAddressModal() { document.getElementById('addressModal').style.display = 'none'; }

    function addToCart(name, price) {
      let item = cart.find(i => i.name === name);
      if(item) { item.qty++; } else { cart.push({ name, price, qty: 1 }); }
      updateCartUI();
    }

    function increaseQty(idx) { cart[idx].qty++; updateCartUI(); }
    function decreaseQty(idx) {
      if (cart[idx].qty > 1) { cart[idx].qty--; } else { removeItem(idx); }
      updateCartUI();
    }
    function removeItem(idx) { cart.splice(idx, 1); updateCartUI(); }

    function updateCartUI() {
      let cartCount = cart.reduce((sum, item) => sum + item.qty, 0);
      let totalPrice = cart.reduce((sum, item) => sum + (item.price * item.qty), 0);
      
      document.getElementById('cartCount').innerText = cartCount;
      document.getElementById('totalPrice').innerText = '₹' + totalPrice;

      let cartItemsContainer = document.getElementById('cartItems');
      let cartFooter = document.getElementById('cartFooter');

      if(cart.length === 0) {
        cartItemsContainer.innerHTML = '<p style="text-align:center; color:#777; margin-top:30px;">কার্ট খালি রয়েছে</p>';
        cartFooter.style.display = 'none';
      } else {
        cartFooter.style.display = 'block';
        cartItemsContainer.innerHTML = cart.map((item, index) => `
          <div class="cart-item">
            <div>
              <h5 style="margin:0; font-size:14px;">${item.name}</h5>
              <small style="color:#666;">₹${item.price} x ${item.qty} = ₹${item.qty * item.price}</small>
              <div class="qty-controls">
                <button class="qty-btn" onclick="decreaseQty(${index})">-</button>
                <span style="font-weight:bold; font-size:12px; padding:0 4px;">${item.qty}</span>
                <button class="qty-btn" onclick="increaseQty(${index})">+</button>
              </div>
            </div>
            <button class="delete-btn" onclick="removeItem(${index})">🗑️ বাদ দিন</button>
          </div>
        `).join('');
      }
    }

    function submitOrder() {
      let name = document.getElementById('custName').value;
      let phone = document.getElementById('custPhone').value;
      let area = document.getElementById('custArea').value;
      let pin = document.getElementById('custPin').value;

      if(!name || !phone || !area) {
        alert('দয়া করে আপনার নাম, ফোন নম্বর ও ঠিকানার স্থান পূরণ করুন!');
        return;
      }

      let newOrder = {
        id: 'ORD' + Math.floor(100000 + Math.random() * 900000),
        date: new Date().toLocaleDateString('bn-BD') + ' ' + new Date().toLocaleTimeString(),
        customer: { name, phone, area, pin },
        items: [...cart],
        total: cart.reduce((sum, item) => sum + (item.price * item.qty), 0),
        status: 'পেন্ডিং'
      };

      orders.unshift(newOrder);
      saveData();

      document.getElementById('currentLocation').innerText = area + (pin ? ' - ' + pin : '');
      alert('আপনার অর্ডার সফলভাবে গৃহীত হয়েছে!');
      cart = [];
      updateCartUI();
      closeAddressModal();
      toggleCart();
    }

    // Admin Functions
    function openAdminModal() { document.getElementById('adminModal').style.display = 'flex'; renderAdminOrders(); renderAdminProductList(); }
    function closeAdminModal() { document.getElementById('adminModal').style.display = 'none'; }

    function switchAdminTab(tab) {
      document.getElementById('adminAddSection').style.display = tab === 'add' ? 'block' : 'none';
      document.getElementById('adminManageSection').style.display = tab === 'manage' ? 'block' : 'none';
      document.getElementById('adminOrderSection').style.display = tab === 'orders' ? 'block' : 'none';

      document.getElementById('tabAddBtn').classList.toggle('active', tab === 'add');
      document.getElementById('tabManageBtn').classList.toggle('active', tab === 'manage');
      document.getElementById('tabOrderBtn').classList.toggle('active', tab === 'orders');

      if(tab === 'orders') renderAdminOrders();
      if(tab === 'manage') renderAdminProductList();
    }

    function convertImageToBase64(input) {
      if (input.files && input.files[0]) {
        let reader = new FileReader();
        reader.onload = function (e) { uploadedBase64Img = e.target.result; };
        reader.readAsDataURL(input.files[0]);
      }
    }

    function addNewProduct() {
      let name = document.getElementById('pName').value;
      let category = document.getElementById('pCategory').value;
      let weight = document.getElementById('pCustomWeight').value || document.getElementById('pUnitType').value;
      let price = parseFloat(document.getElementById('pPrice').value);
      let imgUrl = document.getElementById('pImgUrl').value;

      let finalImg = uploadedBase64Img || imgUrl || 'https://via.placeholder.com/150?text=Grocery';

      if(!name || isNaN(price)) {
        alert('দয়া করে পণ্যের নাম এবং সঠিক দাম লিখুন!');
        return;
      }

      let newP = { id: Date.now(), name, category, weight, price, img: finalImg };
      products.unshift(newP);
      saveData();
      renderProducts();

      document.getElementById('pName').value = '';
      document.getElementById('pCustomWeight').value = '';
      document.getElementById('pPrice').value = '';
      document.getElementById('pImgUrl').value = '';
      document.getElementById('pImgFile').value = '';
      uploadedBase64Img = '';

      alert('নতুন পণ্য সফলভাবে আপলোড হয়েছে!');
    }

    // Delete Product Function for Admin
    function deleteProduct(id) {
      if(confirm('আপনি কি নিশ্চিত যে এই প্রোডাক্টটি মুছে ফেলতে চান?')) {
        products = products.filter(p => p.id !== id);
        saveData();
        renderProducts();
        renderAdminProductList();
      }
    }

    function renderAdminProductList() {
      let container = document.getElementById('adminProductList');
      if(products.length === 0) {
        container.innerHTML = '<p style="text-align:center; color:#777;">কোনো প্রোডাক্ট আপলোড করা নেই</p>';
        return;
      }
      container.innerHTML = products.map(p => `
        <div style="display:flex; justify-content:space-between; align-items:center; border-bottom:1px solid #eee; padding:8px 0;">
          <div style="display:flex; align-items:center; gap:10px;">
            <img src="${p.img}" style="width:35px; height:35px; border-radius:4px; object-fit:cover;" onerror="this.src='https://via.placeholder.com/150?text=No+Img'">
            <div>
              <b style="font-size:13px; display:block;">${p.name}</b>
              <small style="color:#666;">${p.weight} - ₹${p.price}</small>
            </div>
          </div>
          <button class="delete-btn" onclick="deleteProduct(${p.id})">🗑️ ডিলিট</button>
        </div>
      `).join('');
    }

    function renderAdminOrders() {
      let container = document.getElementById('adminOrdersList');
      if(orders.length === 0) {
        container.innerHTML = '<p style="text-align:center; color:#777;">এখনো কোনো অর্ডার আসেনি</p>';
        return;
      }

      container.innerHTML = orders.map((o, idx) => `
        <div style="border:1px solid #e2e8f0; border-radius:8px; padding:12px; margin-bottom:12px; background:#fafafa;">
          <div style="display:flex; justify-content:space-between; font-size:12px; color:#666;">
            <b>ID: ${o.id}</b>
            <span>${o.date}</span>
          </div>
          <p style="margin:5px 0; font-size:14px;"><b>কাস্টমার:</b> ${o.customer.name} (${o.customer.phone})</p>
          <p style="margin:0 0 8px 0; font-size:12px; color:#555;"><b>ঠিকানা:</b> ${o.customer.area} - ${o.customer.pin}</p>
          
          <div style="font-size:12px; background:#fff; padding:6px; border-radius:4px; margin-bottom:8px;">
            ${o.items.map(i => `<div>• ${i.name} (${i.qty} পিস) = ₹${i.qty * i.price}</div>`).join('')}
            <div style="font-weight:bold; margin-top:4px; text-align:right;">মোট: ₹${o.total}</div>
          </div>

          <div style="display:flex; justify-content:space-between; align-items:center;">
            <span style="font-size:12px; font-weight:bold; color:${o.status === 'কনফার্ম' ? 'green' : '#e67e22'};">স্ট্যাটাস: ${o.status}</span>
            <button class="btn-add" style="width:auto; padding:6px 12px; font-size:12px; background-color:#ff9800;" onclick="confirmAndDownloadPDF(${idx})">
              📄 কনফার্ম ও ডাউনলোড PDF
            </button>
          </div>
        </div>
      `).join('');
    }

    // Confirm Order & Download Invoice PDF
    function confirmAndDownloadPDF(idx) {
      orders[idx].status = 'কনফার্ম';
      saveData();
      renderAdminOrders();

      let o = orders[idx];

      document.getElementById('invCustDetails').innerHTML = `
        <div>
          <b>কাস্টমার তথ্য:</b><br>
          নাম: ${o.customer.name}<br>
          ফোন: ${o.customer.phone}<br>
          ঠিকানা: ${o.customer.area} (${o.customer.pin})
        </div>
        <div style="text-align:right;">
          <b>ইনভয়েস আইডি:</b> ${o.id}<br>
          <b>তারিখ:</b> ${o.date}
        </div>
      `;

      document.getElementById('invTableItems').innerHTML = o.items.map(item => `
        <tr>
          <td style="padding:6px; border:1px solid #ccc;">${item.name}</td>
          <td style="padding:6px; border:1px solid #ccc; text-align:center;">${item.qty}</td>
          <td style="padding:6px; border:1px solid #ccc; text-align:right;">₹${item.price}</td>
          <td style="padding:6px; border:1px solid #ccc; text-align:right;">₹${item.qty * item.price}</td>
        </tr>
      `).join('');

      document.getElementById('invTotalVal').innerText = '₹' + o.total;

      let element = document.getElementById('invoice-template');
      element.style.display = 'block';

      let opt = {
        margin:       10,
        filename:     `Invoice_${o.id}.pdf`,
        image:        { type: 'jpeg', quality: 0.98 },
        html2canvas:  { scale: 2 },
        jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
      };

      html2pdf().set(opt).from(element).save().then(() => {
        element.style.display = 'none';
      });
    }

    renderProducts();
  </script>
