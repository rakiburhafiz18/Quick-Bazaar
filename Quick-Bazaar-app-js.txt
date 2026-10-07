
const products=[
{id:1,name:'Aashirvaad Atta 5kg',price:260,old:300,discount:'₹40 OFF',cat:'grocery',emoji:'🌾'},
{id:2,name:'India Gate Rice 5kg',price:420,old:480,discount:'12% OFF',cat:'grocery',emoji:'🍚'},
{id:3,name:'Fortune Sunflower Oil 1L',price:140,old:165,discount:'15% OFF',cat:'grocery',emoji:'🫗'},
{id:4,name:'Maggi Noodles Pack',price:90,old:100,discount:'10% OFF',cat:'grocery',emoji:'🍜'},
{id:5,name:'Fresh Banana 1kg',price:50,old:60,discount:'10% OFF',cat:'fresh',emoji:'🍌'},
{id:6,name:'Fresh Tomato 1kg',price:40,old:50,discount:'20% OFF',cat:'fresh',emoji:'🍅'},
{id:7,name:'Fresh Potato 1kg',price:35,old:42,discount:'15% OFF',cat:'fresh',emoji:'🥔'},
{id:8,name:'Fresh Onion 1kg',price:45,old:52,discount:'12% OFF',cat:'fresh',emoji:'🧅'},
{id:9,name:'Face Wash',price:149,old:180,discount:'17% OFF',cat:'beauty',emoji:'🧴'},
{id:10,name:'Body Lotion',price:199,old:240,discount:'17% OFF',cat:'beauty',emoji:'🧼'},
{id:11,name:'Storage Box',price:249,old:299,discount:'16% OFF',cat:'home',emoji:'📦'},
{id:12,name:'Kitchen Organizer',price:299,old:349,discount:'14% OFF',cat:'home',emoji:'🧺'},
{id:13,name:'Smartphone',price:8999,old:9999,discount:'10% OFF',cat:'electronics',emoji:'📱'},
{id:14,name:'Wireless Earbuds',price:1299,old:1799,discount:'28% OFF',cat:'electronics',emoji:'🎧'},
{id:15,name:'Power Bank',price:899,old:1199,discount:'25% OFF',cat:'electronics',emoji:'🔋'},
{id:16,name:'Gift Hamper',price:499,old:699,discount:'29% OFF',cat:'gifting',emoji:'🎁'},
{id:17,name:'Teddy Gift',price:399,old:599,discount:'33% OFF',cat:'gifting',emoji:'🧸'},
{id:18,name:"Men's T-Shirt",price:299,old:499,discount:'40% OFF',cat:'clothes',emoji:'👕'},
{id:19,name:"Women's Kurti",price:399,old:799,discount:'50% OFF',cat:'clothes',emoji:'👗'},
{id:20,name:"Men's Jeans",price:699,old:1299,discount:'45% OFF',cat:'clothes',emoji:'👖'},
{id:21,name:"Women's Top",price:299,old:499,discount:'40% OFF',cat:'clothes',emoji:'👚'},
{id:22,name:'Kids Wear Set',price:349,old:699,discount:'50% OFF',cat:'clothes',emoji:'🧒'},
{id:23,name:"Kids Dress",price:399,old:729,discount:'45% OFF',cat:'clothes',emoji:'👗'},
{id:24,name:'Mother Dairy Milk 1L',price:58,old:60,discount:'3% OFF',cat:'fresh',emoji:'🥛'},
{id:25,name:'Eggs 30 Pcs',price:180,old:210,discount:'14% OFF',cat:'fresh',emoji:'🥚'}
];
let cart=JSON.parse(localStorage.getItem('qb_cart')||'{}');let wishlist=JSON.parse(localStorage.getItem('qb_wishlist')||'[]');
function save(){localStorage.setItem('qb_cart',JSON.stringify(cart));localStorage.setItem('qb_wishlist',JSON.stringify(wishlist));updateBadges()}
function money(n){return '₹'+n.toLocaleString('en-IN')}
function toast(msg){let t=document.getElementById('toast');t.innerText=msg;t.style.display='block';clearTimeout(window.tt);window.tt=setTimeout(()=>t.style.display='none',1800)}
function cartTotal(){return Object.entries(cart).reduce((s,[id,q])=>{let p=products.find(x=>x.id==id);return s+(p?p.price*q:0)},0)}
function cartItems(){return Object.entries(cart).filter(([id,q])=>q>0).map(([id,q])=>({p:products.find(x=>x.id==id),q:+q})).filter(x=>x.p)}
function updateBadges(){let count=Object.values(cart).reduce((a,b)=>a+b,0);let cb=document.getElementById('cartBadge'),wb=document.getElementById('wishBadge');cb.innerText=count;cb.classList.toggle('hidden',count===0);wb.innerText=wishlist.length;wb.classList.toggle('hidden',wishlist.length===0)}
function productCard(p){let added=!!cart[p.id];return `<div class="card"><span class="discount">${p.discount}</span><div class="pic">${p.emoji}</div><h3>${p.name}</h3><span class="price">${money(p.price)}</span><span class="old">${money(p.old)}</span><button class="add ${added?'added':''}" onclick="addToCart(${p.id})">${added?'✓ Added':'🛒 Add'}</button></div>`}
function render(list=products){document.getElementById('productList').innerHTML=list.slice(0,8).map(productCard).join('');document.getElementById('trendingList').innerHTML=list.filter(p=>['fresh','grocery'].includes(p.cat)).slice(0,8).map(productCard).join('');document.getElementById('clothesList').innerHTML=products.filter(p=>p.cat==='clothes').map(productCard).join('')}
function addToCart(id){cart[id]=(cart[id]||0)+1;save();renderFiltered();toast('Added to cart ✓')}
function renderFiltered(){let q=document.getElementById('search').value.toLowerCase();render(products.filter(p=>p.name.toLowerCase().includes(q)))}
document.getElementById('search').addEventListener('input',renderFiltered);
function openCart(){let items=cartItems();document.getElementById('sheet').innerHTML=`<div class="sheethead"><h2>🛒 Your Cart</h2><button class="close" onclick="closeSheet()">✕</button></div>`+(items.length?items.map(({p,q})=>`<div class="cartrow"><div class="cartemoji">${p.emoji}</div><div class="cartinfo"><b>${p.name}</b><span>${money(p.price)}</span></div><div class="qty"><button onclick="changeQty(${p.id},-1)">−</button><b>${q}</b><button onclick="changeQty(${p.id},1)">+</button></div></div>`).join('')+`<div class="totalbox"><div class="totalline"><span>Items</span><b>${items.reduce((a,x)=>a+x.q,0)}</b></div><div class="totalline"><span>Subtotal</span><b>${money(cartTotal())}</b></div><div class="totalline"><span>Delivery</span><b>FREE</b></div><div class="totalline grand"><span>Total</span><b>${money(cartTotal())}</b></div></div><button class="checkout" onclick="demoCheckout()">Proceed to Checkout</button>`:`<div class="empty"><div>🛒</div><h3>Your cart is empty</h3><p>Add some products to continue.</p></div>`);document.getElementById('overlay').style.display='block'}
function changeQty(id,n){cart[id]=(cart[id]||0)+n;if(cart[id]<=0)delete cart[id];save();renderFiltered();openCart()}
function openWishlist(){let items=products.filter(p=>wishlist.includes(p.id));document.getElementById('sheet').innerHTML=`<div class="sheethead"><h2>❤️ Wishlist</h2><button class="close" onclick="closeSheet()">✕</button></div>`+(items.length?items.map(p=>`<div class="cartrow"><div class="cartemoji">${p.emoji}</div><div class="cartinfo"><b>${p.name}</b><span>${money(p.price)}</span></div><button class="add" style="width:80px;margin:0" onclick="addToCart(${p.id});openWishlist()">Add</button></div>`).join(''):`<div class="empty"><div>♡</div><h3>Wishlist is empty</h3><p>Your favourite products will appear here.</p></div>`);document.getElementById('overlay').style.display='block'}
function closeSheet(e){if(!e||e.target.id==='overlay')document.getElementById('overlay').style.display='none'}function goHome(){window.scrollTo({top:0,behavior:'smooth'})}

function toggleWish(id){if(wishlist.includes(id)){wishlist=wishlist.filter(x=>x!==id);toast('Removed from wishlist')}else{wishlist.push(id);toast('Added to wishlist ♥')}save();renderFiltered()}
function openProduct(id){let p=products.find(x=>x.id===id);if(!p)return;let liked=wishlist.includes(id);document.getElementById('sheet').innerHTML=`<div class="sheethead"><h2>Product Details</h2><button class="close" onclick="closeSheet()">✕</button></div><div class="detailbox"><div class="detailpic">${p.emoji}</div><div class="detailinfo"><h2>${p.name}</h2><div class="stars">★★★★★ <span style="color:#777">4.7</span></div><p>Fresh quality product with great value. Product details and packaging information will be updated with the final catalogue.</p><div class="price" style="margin-top:8px">${money(p.price)} <span class="old">${money(p.old)}</span></div></div></div><div class="specs"><b>Highlights</b><br>✓ Quality checked<br>✓ Easy ordering<br>✓ Fast local delivery<br>✓ ${p.discount}</div><button class="bigadd" onclick="addToCart(${p.id});closeSheet()">🛒 Add to Cart</button><button class="add" style="margin-top:8px" onclick="toggleWish(${p.id});openProduct(${p.id})">${liked?'♥ Remove from Wishlist':'♡ Add to Wishlist'}</button>`;document.getElementById('overlay').style.display='block'}
function openCategories(){let cats=[['all','🛒','All Products'],['fresh','🥬','Fresh'],['beauty','🧴','Beauty'],['home','🛋️','Home'],['electronics','📱','Electronics'],['gifting','🎁','Gifting'],['clothes','👕','Clothes'],['kids','🧸','Kids']];document.getElementById('sheet').innerHTML=`<div class="sheethead"><h2>▦ Categories</h2><button class="close" onclick="closeSheet()">✕</button></div><div class="catgrid">${cats.map(c=>`<button onclick="filterCategory('${c[0]}');closeSheet()">${c[1]}<br><span style="display:block;margin-top:7px">${c[2]}</span></button>`).join('')}</div>`;document.getElementById('overlay').style.display='block'}
function openOrders(){let orders=JSON.parse(localStorage.getItem('qb_orders')||'[]');document.getElementById('sheet').innerHTML=`<div class="sheethead"><h2>📦 My Orders</h2><button class="close" onclick="closeSheet()">✕</button></div>`+(orders.length?orders.map(o=>`<div class="ordercard"><div class="orderhead"><span>Order #${o.id}</span><span>${money(o.total)}</span></div><div class="orderitems">${o.items}</div><div class="status">✓ ${o.status}</div></div>`).join(''):`<div class="empty"><div>📦</div><h3>No orders yet</h3><p>Your orders will appear here after checkout.</p></div>`);document.getElementById('overlay').style.display='block'}
function demoCheckout(){let items=cartItems();if(!items.length){toast('Your cart is empty');return}let orders=JSON.parse(localStorage.getItem('qb_orders')||'[]');orders.unshift({id:String(Date.now()).slice(-6),total:cartTotal(),items:items.map(x=>`${x.p.name} × ${x.q}`).join(' • '),status:'Demo order placed'});localStorage.setItem('qb_orders',JSON.stringify(orders));cart={};save();renderFiltered();closeSheet();toast('Demo order placed ✓')}
function filterCategory(cat){document.getElementById('search').value='';let list=cat==='all'?products:products.filter(p=>p.cat===cat);render(list);document.getElementById('productsSection').scrollIntoView({behavior:'smooth'});toast(cat==='all'?'All products':cat.charAt(0).toUpperCase()+cat.slice(1))}

render();updateBadges();
