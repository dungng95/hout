<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>GreenThread Marketplace</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f8f8f8;
    }
    header {
      background-color: #2d5c44;
      color: white;
      padding: 1rem 2rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    header h1 {
      margin: 0;
    }
    nav a {
      color: white;
      margin: 0 1rem;
      text-decoration: none;
      font-weight: bold;
    }
    .nav-right {
      display: flex;
      gap: 1rem;
      align-items: center;
    }
    .tabs {
      display: flex;
      justify-content: center;
      margin: 1rem 0;
    }
    .tabs button {
      padding: 1rem 2rem;
      margin: 0 1rem;
      border: none;
      background-color: #ffffff;
      box-shadow: 0 0 5px rgba(0,0,0,0.1);
      cursor: pointer;
      font-weight: bold;
    }
    .tabs button.active {
      background-color: #2d5c44;
      color: white;
    }
    .section {
      display: none;
      max-width: 1200px;
      margin: auto;
      padding: 2rem;
    }
    .section.active {
      display: block;
    }
    .products, .dashboard {
      display: flex;
      flex-wrap: wrap;
      gap: 2rem;
      justify-content: center;
    }
    .product, .seller-item {
      background: white;
      padding: 1rem;
      width: 250px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    .product img, .seller-item img {
      width: 100%;
      border-radius: 8px;
    }
    .form-group {
      margin-bottom: 1rem;
    }
    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
    }
    .form-group input, .form-group textarea, .form-group select {
      width: 100%;
      padding: 0.5rem;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    .form-group button {
      padding: 0.5rem 1rem;
      background-color: #2d5c44;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    #loginModal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      display: none;
      justify-content: center;
      align-items: center;
    }
    #loginModal .modal-content {
      background-color: white;
      padding: 2rem;
      border-radius: 10px;
      width: 300px;
    }
    footer {
      background-color: #2d5c44;
      color: white;
      text-align: center;
      padding: 1rem;
      margin-top: 2rem;
    }
  </style>
</head>
<body>
  <header>
    <h1>GreenThread</h1>
    <div class="nav-right">
      <nav>
        <a href="#">Home</a>
        <a href="#">Contact</a>
        <a href="#" onclick="openLoginModal()">Login / Signup</a>
      </nav>
    </div>
  </header>

  <div class="tabs">
    <button onclick="showSection('buyer')" class="active">Buyer Page</button>
    <button onclick="showSection('seller')">Seller Page</button>
  </div>

  <section id="buyer" class="section active">
    <h2 style="text-align:center">Shop Sustainable Products</h2>
    <div class="products" id="buyer-products"></div>
  </section>

  <section id="seller" class="section">
    <h2 style="text-align:center">Seller Dashboard</h2>
    <form id="addProductForm">
      <div class="form-group">
        <label for="productName">Product Name</label>
        <input type="text" id="productName" required />
      </div>
      <div class="form-group">
        <label for="productPrice">Price</label>
        <input type="text" id="productPrice" required />
      </div>
      <div class="form-group">
        <label for="productImage">Image URL</label>
        <input type="text" id="productImage" required />
      </div>
      <div class="form-group">
        <label for="productDesc">Description</label>
        <textarea id="productDesc" rows="3" required></textarea>
      </div>
      <div class="form-group">
        <button type="submit">Add Product</button>
      </div>
    </form>
    <div class="dashboard" id="seller-products"></div>
  </section>

  <div id="loginModal">
    <div class="modal-content">
      <h3>Login / Signup</h3>
      <div class="form-group">
        <label for="userEmail">Email</label>
        <input type="email" id="userEmail" />
      </div>
      <div class="form-group">
        <label for="userPassword">Password</label>
        <input type="password" id="userPassword" />
      </div>
      <div class="form-group">
        <label for="userRole">Role</label>
        <select id="userRole">
          <option value="buyer">Buyer</option>
          <option value="seller">Seller</option>
        </select>
      </div>
      <div class="form-group">
        <button onclick="closeLoginModal()">Submit</button>
      </div>
    </div>
  </div>

  <footer>
    <p>&copy; 2025 GreenThread. All rights reserved.</p>
  </footer>

  <script>
    function showSection(section) {
      document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active'));
      document.querySelectorAll('.tabs button').forEach(btn => btn.classList.remove('active'));
      document.getElementById(section).classList.add('active');
      event.target.classList.add('active');
    }

    function openLoginModal() {
      document.getElementById('loginModal').style.display = 'flex';
    }

    function closeLoginModal() {
      document.getElementById('loginModal').style.display = 'none';
    }

    const form = document.getElementById('addProductForm');
    const sellerProducts = document.getElementById('seller-products');
    const buyerProducts = document.getElementById('buyer-products');
    let products = [];

    form.addEventListener('submit', function(e) {
      e.preventDefault();
      const name = document.getElementById('productName').value;
      const price = document.getElementById('productPrice').value;
      const image = document.getElementById('productImage').value;
      const desc = document.getElementById('productDesc').value;

      const product = { name, price, image, desc };
      products.push(product);
      displayProducts();
      form.reset();
    });

    function displayProducts() {
      sellerProducts.innerHTML = '';
      buyerProducts.innerHTML = '';
      products.forEach(p => {
        const html = `
          <div class="product">
            <img src="${p.image}" alt="${p.name}">
            <h3>${p.name}</h3>
            <p>${p.price} – ${p.desc}</p>
          </div>
        `;
        sellerProducts.innerHTML += html;
        buyerProducts.innerHTML += html;
      });
    }
  </script>
</body>
</html>
