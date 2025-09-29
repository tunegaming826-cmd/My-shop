# My-shop
To buy latest and brand new useful product
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>🌸 My Shop</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
<style>
  /* Theme */
  body { margin:0; font-family:'Poppins',sans-serif; background: linear-gradient(135deg,#ffe6f0,#fff8e7); }
  header { background: linear-gradient(90deg,#ff4081,#ff85a2); color:white; text-align:center; padding:2rem 0; box-shadow:0 10px 25px rgba(0,0,0,0.15); position:relative; }
  header h1 { margin:0; font-size:2.5rem; letter-spacing:1px; }

  /* My Orders button in header */
  #myOrdersBtn { position:absolute; right:16px; top:18px; display:none; }

  /* Cards */
  .container { display:flex; flex-wrap:wrap; justify-content:center; padding:2rem; }
  .item { background:white; border-radius:25px; margin:1rem; padding:1.5rem; width:260px; text-align:center; box-shadow:0 15px 40px rgba(0,0,0,0.1); transition: all 0.4s ease; }
  .item:hover { transform: translateY(-10px); box-shadow:0 20px 50px rgba(0,0,0,0.15); }
  .item img { width:100%; border-radius:20px; margin-bottom:12px; transition:0.3s; }
  .item img:hover { transform: scale(1.05); }
  .item h2 { font-size:1.5rem; margin:10px 0; color:#ff4081; }
  .item p { font-weight:bold; color:#ff4081; font-size:1.2rem; }

  /* Buttons */
  .btn { background:linear-gradient(90deg,#ff4081,#ff85a2); color:white; border:none; padding:12px 20px; border-radius:15px; cursor:pointer; margin-top:12px; font-weight:600; transition:0.25s; }
  .btn:hover { background:linear-gradient(90deg,#e73370,#ff6290); transform:scale(1.03); }

  /* Checkout/Admin layout */
  .checkout, #adminPage { max-width:520px; margin:40px auto; background:white; padding:30px; border-radius:20px; box-shadow:0 15px 40px rgba(0,0,0,0.12); }
  .checkout h2 { text-align:center; margin-bottom:20px; color:#ff4081; font-size:1.8rem; }
  .checkout input, .checkout textarea { width:100%; padding:12px; margin:10px 0; border-radius:12px; border:1px solid #e9d3df; font-size:1rem; }
  .checkout textarea { resize:vertical; min-height:80px; }

  .payment { margin-top:18px; padding:18px; border:2px dashed #ffb0cd; border-radius:14px; display:none; background:#fff0f5; }
  #upiDetails, #codDetails { margin-top:12px; display:none; text-align:center; }
  #upiQR { display:block; margin:8px auto; width:180px; height:180px; border-radius:12px; }

  /* Popups */
  .popup {
    display:none;
    position:fixed;
    top:50%; left:50%;
    transform:translate(-50%,-50%) scale(0.85);
    background:white;
    border-radius:18px;
    box-shadow:0 20px 50px rgba(0,0,0,0.25);
    padding:28px;
    text-align:center;
    z-index:10002;
    opacity:0;
    transition:0.28s ease;
    max-width:90%;
  }
  .popup.show { display:block; opacity:1; transform:translate(-50%,-50%) scale(1); }

  /* Loading overlay */
  #loadingOverlay { position:fixed; inset:0; display:none; background:rgba(0,0,0,0.28); justify-content:center; align-items:center; z-index:10003; }
  #loadingOverlay .spinner { border:6px solid #f3f3f3; border-top:6px solid #ff4081; border-radius:50%; width:64px; height:64px; animation:spin 0.6s linear infinite; }
  @keyframes spin { 0%{transform:rotate(0deg);} 100%{transform:rotate(360deg);} }

  /* Role overlay */
  #roleSelectOverlay { position:fixed; inset:0; background:rgba(255,240,245,0.95); display:flex; justify-content:center; align-items:center; flex-direction:column; z-index:10001; }
  #roleSelectOverlay h2 { color:#ff4081; font-size:2rem; margin-bottom:22px; }
  #roleSelectOverlay button { margin:8px; padding:14px 26px; border-radius:16px; font-weight:700; }

  /* Admin list */
  .order-card { border-radius:12px; padding:16px; margin:12px 0; box-shadow:0 8px 20px rgba(0,0,0,0.06); }
  .order-card.completed { background:#e6f9ea; border-left:4px solid #3bb54a; }

  /* small */
  .small { font-size:0.95rem; color:#666; }
  .muted { color:#888; font-size:0.95rem; }

  /* responsive */
  @media (max-width:600px){
    .item{ width:92%; max-width:420px; }
    .checkout, #adminPage { margin:18px; padding:20px; }
    #myOrdersBtn { right:8px; top:12px; padding:8px 12px; }
  }
</style>
</head>
<body>

<header>
  <h1>🌸 My Shop</h1>
  <button id="myOrdersBtn" class="btn" onclick="openBuyerOrdersPopup()">My Orders</button>
</header>

<!-- Role selection overlay -->
<div id="roleSelectOverlay">
  <h2>Select Role</h2>
  <div>
    <button class="btn" onclick="chooseRole('buyer')">Buyer</button>
    <button class="btn" onclick="chooseRole('admin')">Admin</button>
  </div>
</div>

<!-- Shop page -->
<div id="shopPage" class="container" style="display:none;"></div>

<!-- Checkout -->
<div id="checkoutPage" style="display:none;">
  <div class="checkout">
    <button class="btn" onclick="goBack('shop')">← Back</button>
    <h2>Checkout</h2>

    <form id="checkoutForm">
      <input id="custName" type="text" placeholder="Full Name" required>
      <input id="custEmail" type="email" placeholder="Email ID" required>
      <input id="custPhone" type="text" placeholder="Phone Number" required>
      <textarea id="custAddress" placeholder="Full Address" required></textarea>
      <input id="custCity" type="text" placeholder="City" required>
      <input id="custPincode" type="text" placeholder="Pincode" required>
      <button type="submit" class="btn">Submit</button>
    </form>

    <div id="paymentSection" class="payment">
      <h3>Select Payment Method</h3>
      <label style="margin-right:14px;"><input name="paymentMethod" type="radio" value="UPI" onchange="showPaymentDetails()"> UPI</label>
      <label><input name="paymentMethod" type="radio" value="COD" onchange="showPaymentDetails()"> Cash on Delivery</label>

      <div id="upiDetails">
        <p class="small"><strong>UPI ID:</strong> 8968182990@fam</p>
        <p>Scan QR to pay for the product:</p>
        <img id="upiQR" alt="UPI QR">
        <p class="muted">Follow us on Instagram: <strong>@punjab_astars22</strong></p>
        <button class="btn" onclick="confirmUPI()">Confirm Payment</button>
      </div>

      <div id="codDetails">
        <p class="small">Cash on Delivery selected.</p>
        <p class="muted">Follow us on Instagram: <strong>@punjab_astars22</strong></p>
        <button id="codConfirm" class="btn" onclick="showCODFeePopup()">Proceed</button>
      </div>
    </div>
  </div>
</div>

<!-- COD fee popup -->
<div id="codFeePopup" class="popup" role="dialog" aria-modal="true" aria-hidden="true">
  <h2>COD Delivery Fee - ₹50</h2>
  <p class="small">Please pay ₹50 as delivery & shipping fee via UPI before confirming your order.</p>
  <p><strong>UPI ID:</strong> 8968182990@fam</p>
  <img id="codQR" alt="COD Fee QR" style="width:200px; height:200px; border-radius:12px; margin:12px 0;">
  <p class="muted">Follow us on Instagram: <strong>@punjab_astars22</strong></p>
  <div style="margin-top:12px;">
    <button class="btn" onclick="confirmCOD()">Done</button>
    <button class="btn" style="background:transparent; color:#ff4081; border:2px solid #ff4081; margin-left:12px;" onclick="closeCODPopup()">Cancel</button>
  </div>
</div>

<!-- Buyer Orders popup -->
<div id="buyerOrdersPopup" class="popup" role="dialog" aria-modal="true" aria-hidden="true">
  <h2>Your Orders</h2>
  <p class="small">Enter the Email OR Phone used when placing the order to view your orders.</p>
  <input id="searchEmailOrPhone" type="text" placeholder="Email or Phone" style="width:100%; padding:10px; border-radius:10px; border:1px solid #e9d3df; margin-top:8px;">
  <div style="margin-top:12px;">
    <button class="btn" onclick="viewBuyerOrders()">View Orders</button>
    <button class="btn" style="background:transparent; color:#ff4081; border:2px solid #ff4081; margin-left:10px;" onclick="closeBuyerOrdersPopup()">Close</button>
  </div>
  <div id="buyerOrdersResults" style="text-align:left; margin-top:14px; max-height:300px; overflow:auto;"></div>
</div>

<!-- Order confirmation popup -->
<div id="orderPopup" class="popup" role="alertdialog" aria-modal="true" aria-hidden="true"></div>

<!-- Admin -->
<div id="adminPage" style="display:none;">
  <div style="max-width:960px; margin:30px auto; padding:18px;">
    <button class="btn" onclick="goBack('role')">← Back</button>

    <h2 style="color:#ff4081; margin-top:16px;">All Orders</h2>
    <div id="ordersList"></div>

    <h2 style="color:#ff4081; margin-top:12px;">Add New Item</h2>
    <div id="addItemSection" style="display:none; background:#fff0f5; padding:12px; border-radius:12px; border:2px dashed #ffb0cd;">
      <input id="newItemName" type="text" placeholder="Item Name">
      <input id="newItemPrice" type="number" placeholder="Price">
      <input id="newItemImg" type="file" accept="image/*">
      <div style="margin-top:8px;"><button class="btn" onclick="addNewItem()">Add Item</button></div>
    </div>
    <div style="margin-top:10px;"><button class="btn" onclick="toggleAddItem()">Show/Hide Add Item</button></div>

    <h2 style="color:#ff4081; margin-top:18px;">Manage Products</h2>
    <div id="productsList"></div>
  </div>
</div>

<!-- Loading overlay -->
<div id="loadingOverlay"><div class="spinner"></div></div>

<script>
/* ---------- State ---------- */
let selectedItem = "";
let selectedPrice = 0;
let items = JSON.parse(localStorage.getItem("shopItems") || "[]");
if (!items || items.length === 0) {
  items = [
    { name: "Item 1", price: 499, img: "https://via.placeholder.com/400x300?text=Item+1" },
    { name: "Item 2", price: 799, img: "https://via.placeholder.com/400x300?text=Item+2" }
  ];
  localStorage.setItem("shopItems", JSON.stringify(items));
}

/* ---------- Render Shop ---------- */
function renderShop() {
  const shop = document.getElementById("shopPage");
  shop.innerHTML = "";
  items.forEach((it, idx) => {
    const div = document.createElement("div");
    div.className = "item";
    div.innerHTML = `
      <img src="${it.img}" alt="${escapeHtml(it.name)}">
      <h2>${escapeHtml(it.name)}</h2>
      <p>₹${Number(it.price).toFixed(0)}</p>
      <button class="btn" onclick="goToCheckout(${idx})">Buy Now</button>
    `;
    shop.appendChild(div);
  });
}

/* ---------- Role and Navigation ---------- */
function chooseRole(role) {
  document.getElementById("roleSelectOverlay").style.display = "none";
  // ensure My Orders button visibility for buyers
  if (role === "buyer") {
    document.getElementById("shopPage").style.display = "flex";
    document.getElementById("myOrdersBtn").style.display = "inline-block";
    renderShop();
  } else if (role === "admin") {
    document.getElementById("myOrdersBtn").style.display = "none";
    const pwd = prompt("Enter Admin Password:");
    if (pwd === "namanmittal22") {
      document.getElementById("adminPage").style.display = "block";
      renderOrders();
      renderProducts();
    } else {
      alert("Wrong Password! Redirecting to Buyer.");
      document.getElementById("shopPage").style.display = "flex";
      document.getElementById("myOrdersBtn").style.display = "inline-block";
      renderShop();
    }
  }
}

function goToCheckout(index) {
  selectedItem = items[index].name;
  selectedPrice = items[index].price;
  document.getElementById("shopPage").style.display = "none";
  document.getElementById("checkoutPage").style.display = "block";
  document.getElementById("paymentSection").style.display = "none";
  document.querySelectorAll('input[name="paymentMethod"]').forEach(r => r.checked = false);
}

function goBack(page) {
  if (page === "shop") {
    document.getElementById("checkoutPage").style.display = "none";
    document.getElementById("shopPage").style.display = "flex";
  }
  if (page === "role") {
    document.getElementById("adminPage").style.display = "none";
    document.getElementById("roleSelectOverlay").style.display = "flex";
    document.getElementById("myOrdersBtn").style.display = "none";
  }
}

/* ---------- Loading helper ---------- */
function showLoading(callback) {
  const overlay = document.getElementById("loadingOverlay");
  overlay.style.display = "flex";
  setTimeout(() => {
    overlay.style.display = "none";
    if (callback) callback();
  }, 450);
}

/* ---------- Checkout submit ---------- */
document.getElementById("checkoutForm").addEventListener("submit", (e) => {
  e.preventDefault();
  showLoading(() => {
    document.getElementById("paymentSection").style.display = "block";
    document.getElementById("upiDetails").style.display = "none";
    document.getElementById("codDetails").style.display = "none";
  });
});

/* ---------- Payment details ---------- */
function showPaymentDetails() {
  const checked = document.querySelector('input[name="paymentMethod"]:checked');
  if (!checked) return;
  const method = checked.value;
  document.getElementById("upiDetails").style.display = (method === "UPI") ? "block" : "none";
  document.getElementById("codDetails").style.display = (method === "COD") ? "block" : "none";
  if (method === "UPI") {
    const upiURL = `upi://pay?pa=8968182990@fam&pn=MyShop&am=${encodeURIComponent(selectedPrice)}&cu=INR&tn=${encodeURIComponent(selectedItem)}`;
    document.getElementById("upiQR").src = "https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=" + encodeURIComponent(upiURL);
  }
}

/* ---------- COD fee popup ---------- */
function showCODFeePopup() {
  const qrImg = document.getElementById("codQR");
  const codUpi = `upi://pay?pa=8968182990@fam&pn=MyShop&am=50&cu=INR&tn=DeliveryFee`;
  qrImg.src = "https://api.qrserver.com/v1/create-qr-code/?size=200x200&data=" + encodeURIComponent(codUpi);
  const popup = document.getElementById("codFeePopup");
  popup.classList.add("show");
  popup.setAttribute("aria-hidden", "false");
}
function closeCODPopup(){
  const popup = document.getElementById("codFeePopup");
  popup.classList.remove("show");
  popup.setAttribute("aria-hidden", "true");
}

/* ---------- Orders, save & popups ---------- */
function saveOrder(method) {
  const orders = JSON.parse(localStorage.getItem("orders") || "[]");
  const order = {
    item: selectedItem,
    price: selectedPrice,
    name: document.getElementById("custName").value,
    email: document.getElementById("custEmail").value,
    phone: document.getElementById("custPhone").value,
    address: document.getElementById("custAddress").value,
    city: document.getElementById("custCity").value,
    pincode: document.getElementById("custPincode").value,
    method,
    date: new Date().toLocaleString(),
    completed: false
  };
  orders.push(order);
  localStorage.setItem("orders", JSON.stringify(orders));
}

function showPopup(html) {
  const popup = document.getElementById("orderPopup");
  popup.innerHTML = html;
  popup.classList.add("show");
  popup.setAttribute("aria-hidden", "false");
}

/* Confirm flows */
function confirmUPI() {
  showLoading(() => {
    saveOrder("UPI");
    showPopup(`<h2>Order Confirmed!</h2>
      <p>Thank you for your payment 🌸</p>
      <p class="muted">Follow us on Instagram: <strong>@punjab_astars22</strong></p>
      <button class="btn" onclick="closeOrderPopupAndGotoShop()">Close</button>`);
  });
}

function confirmCOD() {
  closeCODPopup();
  showLoading(() => {
    saveOrder("COD + ₹50 (paid via UPI)");
    showPopup(`<h2>Order Confirmed!</h2>
      <p>Thank you for choosing COD 🌸</p>
      <p class="muted">Follow us on Instagram: <strong>@punjab_astars22</strong></p>
      <button class="btn" onclick="closeOrderPopupAndGotoShop()">Close</button>`);
  });
}

function closeOrderPopupAndGotoShop(){
  const popup = document.getElementById("orderPopup");
  popup.classList.remove("show");
  popup.setAttribute("aria-hidden", "true");
  // go back to shop
  document.getElementById("checkoutPage").style.display = "none";
  document.getElementById("shopPage").style.display = "flex";
  renderShop();
}

/* ---------- Buyer Orders feature ---------- */
function openBuyerOrdersPopup() {
  // Prefill with email from checkout form if available
  const email = document.getElementById("custEmail")?.value || "";
  const phone = document.getElementById("custPhone")?.value || "";
  const input = document.getElementById("searchEmailOrPhone");
  input.value = email || phone || "";
  document.getElementById("buyerOrdersResults").innerHTML = "";
  const popup = document.getElementById("buyerOrdersPopup");
  popup.classList.add("show");
  popup.setAttribute("aria-hidden", "false");
  // Scroll to top of popup results
  popup.scrollTop = 0;
}

function closeBuyerOrdersPopup() {
  const popup = document.getElementById("buyerOrdersPopup");
  popup.classList.remove("show");
  popup.setAttribute("aria-hidden", "true");
}

function viewBuyerOrders() {
  const q = (document.getElementById("searchEmailOrPhone").value || "").trim();
  if (!q) {
    alert("Please enter Email or Phone used for the order.");
    return;
  }
  const orders = JSON.parse(localStorage.getItem("orders") || "[]");
  const matched = orders.filter(o => {
    if (!o) return false;
    const emailMatch = (o.email || "").toLowerCase() === q.toLowerCase();
    const phoneMatch = (o.phone || "").replace(/\s+/g,'') === q.replace(/\s+/g,'');
    return emailMatch || phoneMatch;
  });
  const container = document.getElementById("buyerOrdersResults");
  if (!matched || matched.length === 0) {
    container.innerHTML = "<p class='small'>No orders found for that Email/Phone.</p>";
    return;
  }
  container.innerHTML = matched.map((o, i) => `
    <div class="order-card ${o.completed ? 'completed' : ''}" style="margin-bottom:10px;">
      <b>Order</b> &nbsp; <span class="muted">${escapeHtml(o.date)}</span><br>
      <strong>Item:</strong> ${escapeHtml(o.item)} &nbsp; | &nbsp; <strong>Price:</strong> ₹${o.price}<br>
      <strong>Payment:</strong> ${escapeHtml(o.method)} <br>
      <strong>Status:</strong> <b>${o.completed ? 'Completed' : 'Pending'}</b>
      <div style="margin-top:8px; font-size:0.95rem;">
        <strong>Ship to:</strong> ${escapeHtml(o.name)} — ${escapeHtml(o.phone)}<br>
        ${escapeHtml(o.address)}, ${escapeHtml(o.city)} - ${escapeHtml(o.pincode)}
      </div>
    </div>
  `).join("");
}

/* ---------- Admin: Orders ---------- */
function renderOrders() {
  const orders = JSON.parse(localStorage.getItem("orders") || "[]");
  const listDiv = document.getElementById("ordersList");
  if (!orders || orders.length === 0) {
    listDiv.innerHTML = "<p class='small'>No orders yet.</p>";
    return;
  }
  listDiv.innerHTML = orders.map((o, i) => `
    <div class="order-card ${o.completed?'completed':''}">
      <div style="display:flex; justify-content:space-between; align-items:center;">
        <div>
          <b>Order #${i+1}</b> &nbsp; <span class="muted">(${o.date})</span><br>
          <div style="margin-top:6px;">
            <strong>Item:</strong> ${escapeHtml(o.item)} &nbsp; | &nbsp; <strong>Price:</strong> ₹${o.price}
          </div>
          <div style="margin-top:6px;">
            <strong>Name:</strong> ${escapeHtml(o.name)} <br>
            <strong>Phone:</strong> ${escapeHtml(o.phone)} <br>
            <strong>Address:</strong> ${escapeHtml(o.address)}, ${escapeHtml(o.city)} - ${escapeHtml(o.pincode)}
          </div>
        </div>
      </div>
      <div style="margin-top:10px;">
        <strong>Payment:</strong> ${escapeHtml(o.method)} <br>
        <strong>Status:</strong> <b>${o.completed ? 'Completed' : 'Pending'}</b>
      </div>
      <div style="margin-top:12px;">
        <button class="btn" onclick="markCompleted(${i})" ${o.completed? 'disabled' : ''}>Mark as Completed</button>
        <button class="btn" style="margin-left:8px;" onclick="deleteOrder(${i})">Delete</button>
      </div>
    </div>
  `).join("");
}

function markCompleted(index) {
  const orders = JSON.parse(localStorage.getItem("orders") || "[]");
  if (orders[index]) {
    orders[index].completed = true;
    localStorage.setItem("orders", JSON.stringify(orders));
    renderOrders();
  }
}

function deleteOrder(index) {
  const orders = JSON.parse(localStorage.getItem("orders") || "[]");
  if (!orders[index]) return;
  if (!confirm("Delete this order?")) return;
  orders.splice(index, 1);
  localStorage.setItem("orders", JSON.stringify(orders));
  renderOrders();
}

/* ---------- Admin: Products ---------- */
function toggleAddItem() {
  const sec = document.getElementById("addItemSection");
  sec.style.display = (sec.style.display === "block") ? "none" : "block";
}

function addNewItem() {
  const name = (document.getElementById("newItemName").value || "").trim();
  const price = parseFloat(document.getElementById("newItemPrice").value || "");
  const file = document.getElementById("newItemImg").files[0];
  if (!name || isNaN(price) || !file) {
    alert("Please fill name, price and upload an image.");
    return;
  }
  const reader = new FileReader();
  reader.onload = function(e) {
    items.push({ name, price, img: e.target.result });
    localStorage.setItem("shopItems", JSON.stringify(items));
    renderProducts();
    renderShop();
    document.getElementById("newItemName").value = "";
    document.getElementById("newItemPrice").value = "";
    document.getElementById("newItemImg").value = "";
    alert("Item added!");
  };
  reader.readAsDataURL(file);
}

function renderProducts() {
  const listDiv = document.getElementById("productsList");
  if (!items || items.length === 0) {
    listDiv.innerHTML = "<p class='small'>No products yet.</p>";
    return;
  }
  listDiv.innerHTML = items.map((it, i) => `
    <div class="order-card" style="display:flex; gap:12px; align-items:center;">
      <img src="${it.img}" alt="${escapeHtml(it.name)}" style="width:80px; height:80px; object-fit:cover; border-radius:10px;">
      <div style="flex:1;">
        <input id="editName${i}" type="text" value="${escapeHtml(it.name)}" style="padding:8px; border-radius:8px; border:1px solid #e9d3df; width:60%;">
        <input id="editPrice${i}" type="number" value="${it.price}" style="padding:8px; border-radius:8px; border:1px solid #e9d3df; width:30%; margin-left:8px;">
        <div style="margin-top:8px;">
          <button class="btn" onclick="saveEdit(${i})">Save</button>
          <button class="btn" style="margin-left:8px;" onclick="deleteItem(${i})">Delete</button>
        </div>
      </div>
    </div>
  `).join("");
}

function saveEdit(index) {
  const newName = (document.getElementById(`editName${index}`).value || "").trim();
  const newPrice = parseFloat(document.getElementById(`editPrice${index}`).value || "");
  if (!newName || isNaN(newPrice)) { alert("Enter valid name & price"); return; }
  items[index].name = newName;
  items[index].price = newPrice;
  localStorage.setItem("shopItems", JSON.stringify(items));
  renderProducts();
  renderShop();
  alert("Product updated!");
}

function deleteItem(index) {
  if (!confirm("Delete this product?")) return;
  items.splice(index, 1);
  localStorage.setItem("shopItems", JSON.stringify(items));
  renderProducts();
  renderShop();
}

/* ---------- Utilities ---------- */
function escapeHtml(str) {
  if (!str && str !== 0) return "";
  return String(str).replaceAll("&","&amp;").replaceAll("<","&lt;").replaceAll(">","&gt;").replaceAll('"',"&quot;");
}

/* ---------- Initialize ---------- */
renderShop();
</script>
</body>
</html>
