<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>The Darpan Wears — Professional Frontend Store</title>

  <!-- Bootstrap (styling) -->
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">

  <!-- Chart.js for admin analytics -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>

  <style>
    :root{--brand:#0d6efd;--muted:#6c757d}
    body{background:#f6f7fb;font-family:Inter,ui-sans-serif,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial}
    .brand-box{width:44px;height:44px;border-radius:8px;background:linear-gradient(135deg,#0d6efd,#6610f2);display:inline-block;margin-right:.6rem}
    .site-title{font-weight:700;font-size:1.1rem}
    .product-grid{display:grid; gap:16px; grid-template-columns:1fr}
    @media(min-width:768px){ .product-grid{grid-template-columns:repeat(2,1fr);} }
    @media(min-width:1200px){ .product-grid{grid-template-columns:repeat(3,1fr);} }
    .card-ecom{border-radius:12px;overflow:hidden;background:#fff;border:1px solid rgba(0,0,0,.04);box-shadow:0 6px 18px rgba(12,15,20,.03)}
    .card-ecom img{width:100%;height:260px;object-fit:cover;background:#eef2f6}
    .price-old{text-decoration:line-through;color:var(--muted);margin-right:.6rem}
    .badge-sp{background:#ffc107;color:#000;padding:.18rem .45rem;border-radius:.35rem;font-size:.78rem}
    .cart-badge{position:relative}
    .cart-badge span{position:absolute;top:-8px;right:-8px;background:#dc3545;color:#fff;border-radius:50%;padding:4px 7px;font-size:.75rem}
    footer{padding:1rem 0;color:#6c757d;background:#fff;border-top:1px solid #eee;margin-top:24px}
    .admin-hidden{display:none}
    .small-muted{color:#6c757d}
    input[type="number"]::-webkit-outer-spin-button,input[type="number"]::-webkit-inner-spin-button{-webkit-appearance:none;margin:0}
  </style>
</head>
<body>
  <!-- NAV -->
  <nav class="navbar navbar-expand-lg navbar-light bg-white shadow-sm sticky-top">
    <div class="container">
      <a class="navbar-brand d-flex align-items-center" href="#">
        <span class="brand-box" aria-hidden="true"></span>
        <div class="d-flex flex-column">
          <span class="site-title">The Darpan Wears</span>
          <small class="small-muted">Quality that fits your style</small>
        </div>
      </a>

      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navMain">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navMain">
        <ul id="navCategories" class="navbar-nav me-auto mb-2 mb-lg-0" style="gap:.35rem"></ul>

        <form class="d-flex me-3" onsubmit="return false;">
          <input id="searchInput" class="form-control me-2" type="search" placeholder="Search products...">
          <button id="clearSearch" class="btn btn-outline-secondary" type="button" title="Clear search">✕</button>
        </form>

        <div class="d-flex gap-2">
          <a id="instaLink" href="https://www.instagram.com/darpan_wears?igsh=a2pkYXhpajVwNnR3" target="_blank" class="btn btn-outline-danger btn-sm">Instagram</a>

          <button id="cartBtn" class="btn btn-outline-primary cart-badge">
            🛒 Cart <span id="cartCountBadge">0</span>
          </button>

          <button id="adminBtn" class="btn btn-primary btn-sm">Admin</button>
        </div>
      </div>
    </div>
  </nav>

  <!-- MAIN -->
  <main class="container my-4">
    <div class="row">
      <section class="col-lg-9">
        <div class="d-flex justify-content-between align-items-center mb-3">
          <h4 class="mb-0">Products</h4>
          <div class="small-muted" id="resultsInfo"></div>
        </div>

        <div id="categoryPills" class="mb-3"></div>

        <div id="productGrid" class="product-grid"></div>
      </section>

      <aside class="col-lg-3">
        <div id="promoCard" class="card p-3 mb-3">
          <h6 class="mb-1">Welcome to The Darpan Wears</h6>
          <p class="small-muted mb-0">Shop quality clothing & electronics. Add to cart and checkout securely.</p>
        </div>

        <div id="adminPanel" class="card p-3 admin-hidden">
          <div class="d-flex justify-content-between align-items-start mb-2">
            <h6 class="mb-0">Admin Dashboard</h6>
            <button id="adminLogout" class="btn btn-sm btn-outline-secondary">Logout</button>
          </div>

          <form id="adminForm">
            <input type="hidden" id="prodId">
            <div class="mb-2"><input id="prodName" placeholder="Product name" class="form-control" required></div>
            <div class="mb-2"><textarea id="prodDesc" placeholder="Short description" class="form-control" rows="2"></textarea></div>
            <div class="mb-2"><select id="prodCat" class="form-select"><option>Shirts</option><option>T-Shirts</option><option>Jeans</option><option>Shoes</option><option>Electronic Devices</option></select></div>

            <div class="row g-2">
              <div class="col-6"><input id="prodOrig" type="number" placeholder="Original price" class="form-control"></div>
              <div class="col-6"><input id="prodSell" type="number" placeholder="Selling price" class="form-control" required></div>
            </div>

            <div class="row g-2 mt-2">
              <div class="col-6"><input id="prodSpec" type="number" placeholder="Special price (optional)" class="form-control"></div>
              <div class="col-6"><input id="prodSize" placeholder="Size (M,L,XL or One Size)" class="form-control"></div>
            </div>

            <div class="mb-2 mt-2"><input id="prodMat" placeholder="Material" class="form-control"></div>
            <div class="mb-2"><input id="prodImg" placeholder="Image URL / BBCode / <img> / Drive link" class="form-control"></div>

            <div class="d-grid gap-2 mt-2">
              <button id="saveProductBtn" class="btn btn-success" type="submit">Save Product</button>
              <button id="clearProductBtn" type="button" class="btn btn-outline-secondary">Clear</button>
            </div>
          </form>

          <hr />

          <div id="adminProductsList" style="max-height:260px;overflow:auto"></div>

          <hr />
          <h6 class="small mb-2">Sales Analytics</h6>
          <canvas id="adminChart" width="400" height="200"></canvas>
          <div id="recentOrders" style="max-height:160px;overflow:auto" class="mt-2 small-muted"></div>
        </div>

        <div id="loginPanel" class="card p-3">
          <h6 class="mb-2">Admin Login</h6>
          <div class="mb-2 small-muted">Enter admin password to unlock management.</div>
          <div class="input-group">
            <input id="adminPassword" type="password" class="form-control" placeholder="Password">
            <button id="adminUnlockBtn" class="btn btn-primary">Unlock</button>
          </div>
        </div>
      </aside>
    </div>
  </main>

  <footer class="text-center small">
    © 2025 The Darpan Wears
  </footer>

  <!-- CART MODAL -->
  <div class="modal fade" id="cartModal" tabindex="-1">
    <div class="modal-dialog modal-lg">
      <form class="modal-content" id="cartForm" onsubmit="return false;">
        <div class="modal-header"><h5 class="modal-title">Your Cart</h5></div>
        <div class="modal-body">
          <div id="cartItems"></div>
          <hr />
          <div class="d-flex justify-content-between align-items-center">
            <div class="small-muted">Total:</div>
            <div class="h5 mb-0" id="cartTotal">₹0</div>
          </div>
        </div>
        <div class="modal-footer">
          <button id="checkoutBtn" class="btn btn-success">Proceed to Checkout</button>
          <button class="btn btn-secondary" data-bs-dismiss="modal">Continue shopping</button>
        </div>
      </form>
    </div>
  </div>

  <!-- CHECKOUT MODAL -->
  <div class="modal fade" id="checkoutModal" tabindex="-1">
    <div class="modal-dialog modal-dialog-centered">
      <form class="modal-content" id="checkoutForm">
        <div class="modal-header"><h5 class="modal-title">Checkout</h5></div>
        <div class="modal-body">
          <div id="checkoutSummary" class="mb-3"></div>
          <input id="c_name" class="form-control mb-2" placeholder="Full name" required>
          <input id="c_phone" class="form-control mb-2" placeholder="Phone number" required>
          <input id="c_state" class="form-control mb-2" placeholder="State" required>
          <input id="c_city" class="form-control mb-2" placeholder="City" required>
          <input id="c_village" class="form-control mb-2" placeholder="Village / Locality">
          <input id="c_pin" class="form-control mb-2" placeholder="Pincode" required>
          <div class="small-muted mb-2">Choose Payment</div>
          <div class="d-grid gap-2">
            <button id="payCOD" class="btn btn-outline-primary" type="button">Cash on Delivery</button>
            <button id="payOnline" class="btn btn-success" type="button">Online Payment (Demo)</button>
          </div>
        </div>
        <div class="modal-footer"><button class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button></div>
      </form>
    </div>
  </div>

  <!-- CONFIRM DELETE -->
  <div class="modal fade" id="confirmDelModal" tabindex="-1">
    <div class="modal-dialog modal-sm modal-dialog-centered">
      <div class="modal-content">
        <div class="modal-body text-center">
          <p>Delete this product?</p>
          <div class="d-flex justify-content-center gap-2">
            <button id="confirmDelBtn" class="btn btn-danger">Delete</button>
            <button class="btn btn-secondary" data-bs-dismiss="modal">Cancel</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- BOOTSTRAP JS -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>

  <script>
  /**
   * Professional single-file front-end eCommerce
   * - Admin password (client-side): darpan2025
   * - Cart + Checkout + WhatsApp order with image preview link
   * - Image auto-detect supports direct links, BBCode, <img>, Google Drive, Imgur viewer
   *
   * SECURITY: Client-side password is not secure for production. Use server-side auth for real stores.
   */

  (function(){
    'use strict';

    // CONSTANTS
    const ADMIN_PW = 'darpan2025';
    const STORAGE_PRODUCTS = 'dw_products_v2';
    const STORAGE_CART = 'dw_cart_v2';
    const STORAGE_ORDERS = 'dw_orders_v2';
    const WHATSAPP_PHONE = '919332307996';
    const CATEGORIES = ['All','Shirts','T-Shirts','Jeans','Shoes','Electronic Devices'];

    // DOM refs
    const navCategories = document.getElementById('navCategories');
    const categoryPills = document.getElementById('categoryPills');
    const productGrid = document.getElementById('productGrid');
    const resultsInfo = document.getElementById('resultsInfo');
    const searchInput = document.getElementById('searchInput');
    const clearSearchBtn = document.getElementById('clearSearch');
    const cartBtn = document.getElementById('cartBtn');
    const cartCountBadge = document.getElementById('cartCountBadge');
    const cartModal = new bootstrap.Modal(document.getElementById('cartModal'));
    const checkoutModal = new bootstrap.Modal(document.getElementById('checkoutModal'));
    const loginPanel = document.getElementById('loginPanel');
    const adminPanel = document.getElementById('adminPanel');
    const adminUnlockBtn = document.getElementById('adminUnlockBtn');
    const adminBtn = document.getElementById('adminBtn');
    const adminPasswordInput = document.getElementById('adminPassword');
    const adminLogout = document.getElementById('adminLogout');

    // admin form refs
    const adminForm = document.getElementById('adminForm');
    const prodIdInput = document.getElementById('prodId');
    const prodName = document.getElementById('prodName');
    const prodDesc = document.getElementById('prodDesc');
    const prodCat = document.getElementById('prodCat');
    const prodOrig = document.getElementById('prodOrig');
    const prodSell = document.getElementById('prodSell');
    const prodSpec = document.getElementById('prodSpec');
    const prodSize = document.getElementById('prodSize');
    const prodMat = document.getElementById('prodMat');
    const prodImg = document.getElementById('prodImg');
    const adminProductsList = document.getElementById('adminProductsList');

    // cart refs
    const cartItemsContainer = document.getElementById('cartItems');
    const cartTotalEl = document.getElementById('cartTotal');
    const checkoutBtn = document.getElementById('checkoutBtn');

    // checkout refs
    const checkoutSummary = document.getElementById('checkoutSummary');
    const c_name = document.getElementById('c_name');
    const c_phone = document.getElementById('c_phone');
    const c_state = document.getElementById('c_state');
    const c_city = document.getElementById('c_city');
    const c_village = document.getElementById('c_village');
    const c_pin = document.getElementById('c_pin');
    const payCOD = document.getElementById('payCOD');
    const payOnline = document.getElementById('payOnline');

    // admin analytics
    const adminChartCtx = document.getElementById('adminChart').getContext('2d');
    const recentOrdersEl = document.getElementById('recentOrders');
    let adminChart = null;

    // delete confirm
    const confirmDelModal = new bootstrap.Modal(document.getElementById('confirmDelModal'));
    let deleteCandidateId = null;

    // state
    let products = [];
    let cart = [];
    let orders = [];
    let activeCategory = 'All';
    let adminUnlocked = false;

    // UTILITIES
    function uid(){ return 'id_' + Math.random().toString(36).slice(2,9); }
    function fmtINR(n){ return '₹' + (Number(n) || 0).toLocaleString('en-IN'); }
    function persist(key, data){ localStorage.setItem(key, JSON.stringify(data)); }
    function load(key){ try{ return JSON.parse(localStorage.getItem(key)) || []; } catch(e){ return []; } }

    // IMAGE EXTRACTOR: supports <img src="...">, [img]...[/img], Google Drive, Imgur viewer, direct links
    function extractImageUrl(input){
      if(!input) return '';
      let u = String(input).trim();

      // HTML <img src="...">
      const imgTag = u.match(/<img[^>]+src=(["'])(.*?)\1/i);
      if(imgTag && imgTag[2]) return normalizeImageUrl(imgTag[2]);

      // BBCode [img]...[/img]
      const bb = u.match(/\[img\](.*?)\[\/img\]/i);
      if(bb && bb[1]) return normalizeImageUrl(bb[1]);

      // Google Drive share links
      if(u.includes('drive.google.com')){
        const id = (u.match(/[-\w]{25,}/) || [null])[0];
        if(id) return `https://drive.google.com/uc?export=view&id=${id}`;
      }

      // Imgur viewer -> convert to i.imgur.com/ID.jpg
      if(u.includes('imgur.com') && !u.match(/\.(jpg|jpeg|png|webp)$/i)){
        const m = u.match(/imgur\.com\/(?:a\/)?([A-Za-z0-9]+)/);
        if(m && m[1]) return `https://i.imgur.com/${m[1]}.jpg`;
      }

      return normalizeImageUrl(u);
    }

    function normalizeImageUrl(u){
      u = u.trim();
      if(u.startsWith('//')) u = 'https:' + u;
      // if not http(s), return as-is (could be data:image)
      if(!/^https?:\/\//i.test(u) && !u.startsWith('data:')) return u;
      try{
        const url = new URL(u);
        url.search = ''; // strip query to increase chance of preview
        return url.toString();
      } catch(e){ return u; }
    }

    // INITIAL SEED
    function seed(){
      products = load(STORAGE_PRODUCTS);
      if(!products || products.length === 0){
        products = [
          { id: uid(), name:'Legendary T-Shirt', desc:'Comfort cotton tee with signature print', category:'T-Shirts', orig:799, sell:499, spec:449, material:'Cotton', size:'M,L,XL', img:'https://i.ibb.co/5WqFZr9/shirt.jpg' }
        ];
        persist(STORAGE_PRODUCTS, products);
      }
      cart = load(STORAGE_CART);
      orders = load(STORAGE_ORDERS);
    }

    // RENDER CATEGORIES (top nav and pills)
    function renderCategories(){
      navCategories.innerHTML = '';
      categoryPills.innerHTML = '';
      CATEGORIES.forEach(cat=>{
        const li = document.createElement('li'); li.className='nav-item';
        li.innerHTML = `<a class="nav-link nav-cat ${cat==='All'?'active':''}" href="#" data-cat="${cat}">${cat}</a>`;
        navCategories.appendChild(li);

        const btn = document.createElement('button'); btn.className = 'btn btn-sm me-2 mb-2 ' + (cat==='All' ? 'btn-primary' : 'btn-outline-secondary');
        btn.textContent = cat; btn.dataset.cat = cat;
        btn.onclick = ()=>{ document.querySelectorAll('#categoryPills button').forEach(b=>b.classList.remove('btn-primary')); btn.classList.add('btn-primary'); document.querySelectorAll('.nav-cat').forEach(n=>n.classList.remove('active')); const top = document.querySelector(`.nav-cat[data-cat="${cat}"]`); if(top) top.classList.add('active'); activeCategory = cat; renderProducts(); };
        categoryPills.appendChild(btn);
      });

      document.querySelectorAll('.nav-cat').forEach(el=>{
        el.addEventListener('click', e=>{
          e.preventDefault();
          document.querySelectorAll('.nav-cat').forEach(n=>n.classList.remove('active'));
          el.classList.add('active');
          activeCategory = el.dataset.cat || 'All';
          document.querySelectorAll('#categoryPills button').forEach(b=>b.classList.remove('btn-primary'));
          const pill = Array.from(document.querySelectorAll('#categoryPills button')).find(b=>b.dataset.cat===activeCategory);
          if(pill) pill.classList.add('btn-primary');
          renderProducts();
        });
      });
    }

    // RENDER PRODUCTS GRID
    function renderProducts(){
      const q = (searchInput.value || '').trim().toLowerCase();
      const visible = products.filter(p=>{
        const catOk = (activeCategory === 'All') || (p.category === activeCategory);
        const qOk = !q || (p.name && p.name.toLowerCase().includes(q));
        return catOk && qOk;
      });

      productGrid.innerHTML = '';
      if(visible.length === 0){
        productGrid.innerHTML = `<div class="p-4 text-center bg-white rounded">No products found.</div>`;
      } else {
        visible.forEach(p=>{
          const img = extractImageUrl(p.img) || '';
          const final = (p.spec && p.spec !== '') ? p.spec : p.sell;
          const priceHtml = (p.spec && p.spec !== '') ? `<div><span class="fw-bold">${fmtINR(p.spec)}</span> <span class="badge-sp">Special</span></div><div class="price-old">${fmtINR(p.sell)}</div>` : `<div class="fw-bold">${fmtINR(p.sell)}</div>`;

          const card = document.createElement('div'); card.className = 'card-ecom';
          card.innerHTML = `
            <img src="${img}" alt="${escapeHtml(p.name)}" onerror="this.onerror=null;this.src='data:image/svg+xml;utf8,${encodeURIComponent('<svg xmlns=\\'http://www.w3.org/2000/svg\\' width=600 height=400><rect width=100% height=100% fill=%23eef2f6 /><text x=50% y=50% dominant-baseline=middle text-anchor=middle fill=%23838a91 font-size=20>No Image</text></svg>')}'" />
            <div style="padding:12px;">
              <h6 class="mb-1">${escapeHtml(p.name)}</h6>
              <div class="small text-muted mb-2">${escapeHtml(p.desc || '')}</div>
              <div class="mb-2">${priceHtml}</div>
              <div class="small text-muted mb-2"><strong>Material:</strong> ${escapeHtml(p.material || '—')} &nbsp; <strong>Size:</strong> ${escapeHtml(p.size || '—')}</div>
              <div style="display:flex;gap:8px;">
                <button class="btn btn-outline-primary add-cart-btn" data-id="${p.id}">Add to cart</button>
                <button class="btn btn-primary buy-now-btn" data-id="${p.id}">Buy Now</button>
                ${adminUnlocked ? `<div style="margin-left:auto;display:flex;gap:6px"><button class="btn btn-sm btn-outline-secondary edit-prod" data-id="${p.id}">✎</button><button class="btn btn-sm btn-danger del-prod" data-id="${p.id}">🗑</button></div>` : ''}
              </div>
            </div>
          `;
          productGrid.appendChild(card);
        });
      }

      resultsInfo.textContent = `${visible.length} product(s)`;
      attachProductEvents();
      renderAdminList();
    }

    function escapeHtml(s){ if(s===null||s===undefined) return ''; return String(s).replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;').replaceAll('"','&quot;').replaceAll("'","&#039;"); }

    // Attach events for Add-to-cart, Buy Now, Admin edit/delete
    function attachProductEvents(){
      document.querySelectorAll('.add-cart-btn').forEach(b=>{
        b.removeEventListener('click', onAddToCart);
        b.addEventListener('click', onAddToCart);
      });
      document.querySelectorAll('.buy-now-btn').forEach(b=>{
        b.removeEventListener('click', onBuyNow);
        b.addEventListener('click', onBuyNow);
      });
      if(adminUnlocked){
        document.querySelectorAll('.edit-prod').forEach(b=>{ b.removeEventListener('click', onAdminEdit); b.addEventListener('click', onAdminEdit); });
        document.querySelectorAll('.del-prod').forEach(b=>{ b.removeEventListener('click', onAdminDelete); b.addEventListener('click', onAdminDelete); });
      }
    }

    /********** CART FUNCTIONS **********/
    function loadCart(){ cart = load(STORAGE_CART) || []; updateCartBadge(); }
    function saveCart(){ persist(STORAGE_CART, cart); updateCartBadge(); }

    function updateCartBadge(){
      const totalQty = cart.reduce((s,i)=>s + (i.qty || 1), 0);
      cartCountBadge.textContent = totalQty;
    }

    function onAddToCart(e){
      const id = e.currentTarget.dataset.id;
      const product = products.find(p=>p.id === id);
      if(!product) return alert('Product not found');
      // If size options available, ask size — simplified: default size first value
      const size = (product.size || '').split(',')[0] || '';
      const existing = cart.find(c=>c.productId === id && c.size === size);
      if(existing) existing.qty = (existing.qty || 1) + 1;
      else cart.push({ productId: id, qty: 1, size });
      saveCart();
      alert('Added to cart');
    }

    function onBuyNow(e){
      const id = e.currentTarget.dataset.id;
      const product = products.find(p=>p.id === id);
      if(!product) return alert('Product not found');
      // reset cart to single item for buy-now flow
      cart = [{ productId: id, qty: 1, size: (product.size||'').split(',')[0] || '' }];
      saveCart();
      openCartModal();
      // open checkout immediately
      setTimeout(()=> { openCheckout(); }, 300);
    }

    cartBtn.addEventListener('click', ()=> openCartModal());
    function openCartModal(){
      renderCartItems();
      cartModal.show();
    }

    function renderCartItems(){
      cartItemsContainer.innerHTML = '';
      if(cart.length === 0){ cartItemsContainer.innerHTML = '<div class="p-3 text-center small-muted">Cart is empty.</div>'; cartTotalEl.textContent = fmtINR(0); return; }
      let total = 0;
      cart.forEach((ci, idx)=>{
        const p = products.find(x=>x.id === ci.productId);
        if(!p) return;
        const price = Number((p.spec && p.spec!=='')?p.spec:p.sell) || 0;
        total += price * (ci.qty || 1);
        const item = document.createElement('div'); item.className = 'd-flex align-items-center gap-3 mb-3';
        item.innerHTML = `
          <img src="${extractImageUrl(p.img)}" alt="" style="width:72px;height:72px;object-fit:cover;border-radius:6px" onerror="this.onerror=null;this.src='data:image/svg+xml;utf8,${encodeURIComponent('<svg xmlns=\\'http://www.w3.org/2000/svg\\' width=72 height=72><rect width=100% height=100% fill=%23eef2f6 /></svg>')}'" />
          <div style="flex:1">
            <div><strong>${escapeHtml(p.name)}</strong></div>
            <div class="small text-muted">${escapeHtml(p.category)}</div>
            <div class="small text-muted">Size: ${escapeHtml(ci.size || '')}</div>
          </div>
          <div style="width:130px">
            <div class="input-group input-group-sm mb-1">
              <button class="btn btn-outline-secondary btn-sm" data-idx="${idx}" data-action="dec">-</button>
              <input type="number" min="1" value="${ci.qty}" data-idx="${idx}" class="form-control form-control-sm qty-input" style="width:50px;text-align:center" />
              <button class="btn btn-outline-secondary btn-sm" data-idx="${idx}" data-action="inc">+</button>
            </div>
            <div class="text-end">${fmtINR(price * (ci.qty || 1))}</div>
            <div class="text-end"><button class="btn btn-sm btn-link text-danger remove-item" data-idx="${idx}">Remove</button></div>
          </div>
        `;
        cartItemsContainer.appendChild(item);
      });
      cartTotalEl.textContent = fmtINR(total);

      // attach qty handlers
      document.querySelectorAll('.qty-input').forEach(inp=>{
        inp.addEventListener('change', (ev)=>{
          const i = Number(ev.target.dataset.idx);
          const v = Math.max(1, Number(ev.target.value) || 1);
          cart[i].qty = v; saveCart(); renderCartItems();
        });
      });
      document.querySelectorAll('[data-action]').forEach(btn=>{
        btn.addEventListener('click', (ev)=>{
          const i = Number(ev.target.dataset.idx);
          const a = ev.target.dataset.action;
          if(a === 'inc') cart[i].qty = (cart[i].qty || 1) + 1;
          else cart[i].qty = Math.max(1, (cart[i].qty || 1) - 1);
          saveCart(); renderCartItems();
        });
      });
      document.querySelectorAll('.remove-item').forEach(b=>{
        b.addEventListener('click', (ev)=>{ const i = Number(ev.target.dataset.idx); cart.splice(i,1); saveCart(); renderCartItems(); });
      });
    }

    checkoutBtn.addEventListener('click', ()=> openCheckout());

    function openCheckout(){
      cartModal.hide();
      if(cart.length === 0){ alert('Cart is empty'); return; }
      // build summary
      let html = '<div class="mb-2"><strong>Items:</strong></div><ul class="small">';
      let total = 0;
      cart.forEach(ci=>{
        const p = products.find(x=>x.id===ci.productId);
        if(!p) return;
        const price = Number((p.spec && p.spec!=='')?p.spec:p.sell) || 0;
        total += price * (ci.qty || 1);
        html += `<li>${escapeHtml(p.name)} × ${ci.qty || 1} — ${fmtINR(price * (ci.qty || 1))}</li>`;
      });
      html += `</ul><div class="mt-2"><strong>Total: ${fmtINR(total)}</strong></div>`;
      checkoutSummary.innerHTML = html;
      // clear previous checkout fields (not clearing name/phone to preserve user)
      // show modal
      checkoutModal.show();
    }

    // Checkout payments -> open WhatsApp with full order (items list + image URLs)
    payCOD.addEventListener('click', ()=> processCheckout('Cash on Delivery'));
    payOnline.addEventListener('click', ()=> processCheckout('Online Payment (demo)'));

    function processCheckout(paymentMethod){
      const name = c_name.value.trim();
      const phone = c_phone.value.trim();
      const state = c_state.value.trim();
      const city = c_city.value.trim();
      const village = c_village.value.trim();
      const pin = c_pin.value.trim();
      if(!name || !phone || !state || !city || !pin) return alert('Please fill all required fields.');
      // build message
      const lines = ['🚨 Alert: Darpan Wears — New Order!', ''];
      lines.push('Customer: ' + name);
      lines.push('Phone: ' + phone);
      lines.push(`Address: ${village || ''}, ${city}, ${state} - ${pin}`);
      lines.push('Payment: ' + paymentMethod);
      lines.push('');
      lines.push('Order Items:');
      let total = 0;
      cart.forEach(ci=>{
        const p = products.find(x=>x.id === ci.productId);
        if(!p) return;
        const price = Number((p.spec && p.spec!=='')?p.spec:p.sell) || 0;
        total += price * (ci.qty || 1);
        lines.push(`- ${p.name} × ${ci.qty || 1} • Size: ${ci.size || ''} • ${fmtINR(price * (ci.qty || 1))}`);
      });
      lines.push('');
      lines.push('Final Total: ' + fmtINR(total));
      lines.push('');
      lines.push('Product images (preview links):');
      // include unique image URLs for preview (WhatsApp will fetch thumbnail if direct)
      const imageUrls = [];
      cart.forEach(ci=>{
        const p = products.find(x=>x.id === ci.productId);
        if(p){
          const u = extractImageUrl(p.img);
          if(u && !imageUrls.includes(u)) imageUrls.push(u);
        }
      });
      imageUrls.forEach(u => lines.push(u));
      const message = lines.join('\n');

      // open WhatsApp
      const waUrl = `https://wa.me/${WHATSAPP_PHONE}?text=${encodeURIComponent(message)}`;
      window.open(waUrl, '_blank');

      // Save order record locally
      const order = {
        id: 'o_' + Date.now(),
        customer: { name, phone, state, city, village, pin },
        payment: paymentMethod,
        items: cart.map(ci => ({ productId: ci.productId, qty: ci.qty, size: ci.size })),
        images: imageUrls,
        total,
        datetime: new Date().toISOString()
      };
      orders.unshift(order);
      persist(STORAGE_ORDERS, orders);

      // reduce cart, save sales aggregated (for admin analytics)
      cart = [];
      saveCart();
      checkoutModal.hide();
      alert('WhatsApp opened. Order recorded in admin orders.');
      updateAdminAnalytics();
    }

    /********** ADMIN BEHAVIOR **********/
    adminBtn.addEventListener('click', ()=> window.scrollTo({top:0, behavior:'smooth'}));

    adminUnlockBtn.addEventListener('click', ()=>{
      const v = adminPasswordInput.value || '';
      if(v === ADMIN_PW){
        adminUnlocked = true;
        adminPasswordInput.value = '';
        loginPanel.classList.add('admin-hidden');
        adminPanel.classList.remove('admin-hidden');
        renderProducts();
        updateAdminAnalytics();
      } else {
        alert('Incorrect password.');
      }
    });

    adminLogout.addEventListener('click', ()=>{
      adminUnlocked = false;
      adminPanel.classList.add('admin-hidden');
      loginPanel.classList.remove('admin-hidden');
      renderProducts();
    });

    // admin form save product
    adminForm.addEventListener('submit', (ev)=>{
      ev.preventDefault();
      const id = prodIdInput.value;
      const obj = {
        id: id || uid(),
        name: prodName.value.trim(),
        desc: prodDesc.value.trim(),
        category: prodCat.value,
        orig: prodOrig.value ? Number(prodOrig.value) : '',
        sell: prodSell.value ? Number(prodSell.value) : 0,
        spec: prodSpec.value ? Number(prodSpec.value) : '',
        size: prodSize.value.trim(),
        material: prodMat.value.trim(),
        img: prodImg.value.trim()
      };
      if(!obj.name || !obj.sell) return alert('Please provide product name and selling price.');
      if(id){
        const idx = products.findIndex(p=>p.id === id);
        if(idx >= 0) { products[idx] = obj; }
      } else {
        products.unshift(obj);
      }
      persist(STORAGE_PRODUCTS, products);
      adminForm.reset();
      prodIdInput.value = '';
      renderProducts();
      renderAdminList();
      updateAdminAnalytics();
      alert('Product saved.');
    });

    document.getElementById('clearProductBtn').addEventListener('click', ()=>{
      adminForm.reset(); prodIdInput.value = ''; document.getElementById('saveProductBtn').textContent = 'Save Product';
    });

    // Render admin product list (editable)
    function renderAdminList(){
      adminProductsList.innerHTML = '';
      if(!adminUnlocked) return;
      if(!products || products.length === 0) { adminProductsList.innerHTML = '<div class="small-muted">No products</div>'; return; }
      products.forEach(p=>{
        const div = document.createElement('div'); div.className = 'd-flex justify-content-between align-items-start p-2 border-bottom';
        div.innerHTML = `<div><strong>${escapeHtml(p.name)}</strong><div class="small text-muted">${escapeHtml(p.category)} • ${fmtINR((p.spec && p.spec!=='')?p.spec:p.sell)}</div></div>
          <div>
            <button class="btn btn-sm btn-secondary me-1" data-id="${p.id}" data-action="edit">Edit</button>
            <button class="btn btn-sm btn-danger" data-id="${p.id}" data-action="del">Delete</button>
          </div>`;
        adminProductsList.appendChild(div);
      });
      adminProductsList.querySelectorAll('[data-action="edit"]').forEach(b=>b.addEventListener('click', ()=> {
        const id = b.dataset.id; const p = products.find(x=>x.id===id); if(!p) return;
        prodIdInput.value = p.id; prodName.value = p.name; prodDesc.value = p.desc || ''; prodCat.value = p.category || 'Shirts';
        prodOrig.value = p.orig || ''; prodSell.value = p.sell || ''; prodSpec.value = p.spec || ''; prodSize.value = p.size || ''; prodMat.value = p.material || ''; prodImg.value = p.img || '';
        document.getElementById('saveProductBtn').textContent = 'Update Product';
        window.scrollTo({top:0, behavior:'smooth'});
      }));
      adminProductsList.querySelectorAll('[data-action="del"]').forEach(b=>b.addEventListener('click', ()=>{
        deleteCandidateId = b.dataset.id; confirmDelModal.show();
      }));
    }

    document.getElementById('confirmDelBtn').addEventListener('click', ()=>{
      if(!deleteCandidateId) return confirmDelModal.hide();
      products = products.filter(p=>p.id !== deleteCandidateId);
      persist(STORAGE_PRODUCTS, products);
      deleteCandidateId = null;
      renderProducts(); renderAdminList(); updateAdminAnalytics(); confirmDelModal.hide();
    });

    /********** ADMIN ANALYTICS **********/
    function updateAdminAnalytics(){
      orders = load(STORAGE_ORDERS) || [];
      // aggregate by category
      const counts = {}; const totals = {};
      CATEGORIES.forEach(c => { if(c!=='All'){ counts[c]=0; totals[c]=0; }});
      orders.forEach(o=>{
        o.items.forEach(it=>{
          const p = products.find(x=>x.id === it.productId);
          const cat = p ? p.category : 'Other';
          counts[cat] = (counts[cat] || 0) + it.qty;
          totals[cat] = (totals[cat] || 0) + ( (p && (p.spec || p.sell)) ? Number(p.spec || p.sell) * it.qty : 0 );
        });
      });
      const labels = Object.keys(counts).filter(k => counts[k] > 0 || totals[k] > 0);
      const dataCounts = labels.map(l => counts[l] || 0);
      const dataTotals = labels.map(l => totals[l] || 0);

      if(!adminChart){
        adminChart = new Chart(adminChartCtx, {
          type: 'bar',
          data: { labels, datasets: [{label:'Qty Sold', data: dataCounts, backgroundColor:'rgba(13,110,253,0.85)'}, {label:'Total ₹', data:dataTotals, backgroundColor:'rgba(40,167,69,0.7)', yAxisID:'y1'}] },
          options:{ responsive:true, interaction:{mode:'index',intersect:false}, scales:{ y:{beginAtZero:true}, y1:{beginAtZero:true, position:'right', grid:{display:false}} } }
        });
      } else {
        adminChart.data.labels = labels;
        adminChart.data.datasets[0].data = dataCounts;
        adminChart.data.datasets[1].data = dataTotals;
        adminChart.update();
      }

      // recent orders list
      recentOrdersEl.innerHTML = '';
      if(orders.length === 0) { recentOrdersEl.innerHTML = '<div class="small-muted">No orders yet</div>'; return; }
      orders.slice(0,8).forEach(o=>{
        const div = document.createElement('div'); div.className = 'p-2 border-bottom';
        div.innerHTML = `<div><strong>${escapeHtml(o.customer.name)}</strong> • ${escapeHtml(o.payment)}</div>
          <div class="small-muted">${fmtINR(o.total)} • ${new Date(o.datetime).toLocaleString()}</div>`;
        recentOrdersEl.appendChild(div);
      });
    }

    /********** SEARCH & EVENTS **********/
    searchInput.addEventListener('input', debounce(()=> renderProducts(), 200));
    clearSearchBtn.addEventListener('click', ()=> { searchInput.value=''; renderProducts(); searchInput.focus(); });

    function debounce(fn, wait=200){ let t; return function(){ clearTimeout(t); t=setTimeout(()=>fn.apply(this, arguments), wait); }; }

    // helper escape
    function escapeHtml(s){ if(s===null||s===undefined) return ''; return String(s).replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;').replaceAll('"','&quot;').replaceAll("'",'&#039;'); }

    /********** STORAGE & INIT **********/
    function init(){
      seed();
      renderCategories();
      renderProducts();
      loadCart();
      updateAdminAnalytics();
      renderAdminList();
    }
    init();

    // expose for debugging
    window._DW = { products, cart, orders, persist };

  })();
  </script>

  <!-- Footer notes -->
  <!-- © 2025 The Darpan Wears -->
  <!-- Developer: Darpan Karki -->
  <!-- Security Note: Admin password is checked client-side only for convenience. Use server-side auth and a proper backend for production. -->
</body>
</html>
