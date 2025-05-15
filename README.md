<!DOCTYPE html>
<html lang="uk">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Маркет Електроніки</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      background-color: #f0f2f5;
    }
    header {
      background-color: #007bff;
      color: white;
      padding: 20px;
      text-align: center;
      position: relative;
    }
    .form-box {
      max-width: 400px;
      margin: 30px auto;
      background-color: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    .form-box input {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    .form-box button {
      width: 100%;
      padding: 10px;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .form-box button:hover {
      background-color: #0056b3;
    }
    .products {
      display: none;
      grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
      gap: 20px;
      padding: 20px;
    }
    .product {
      background-color: white;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      padding: 15px;
      text-align: center;
    }
    .product img {
      max-width: 100%;
      height: 180px;
      object-fit: cover;
      border-radius: 5px;
    }
    .product h3 {
      margin: 10px 0 5px;
    }
    .product p {
      color: #333;
      margin: 5px 0;
    }
    .product button {
      margin: 5px;
      padding: 8px 12px;
      background-color: #28a745;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    .share-button {
      background-color: #17a2b8;
    }
    #logoutBtn {
      display: none;
      position: fixed;
      top: 20px;
      right: 20px;
      background-color: #dc3545;
      color: white;
      border: none;
      padding: 10px 15px;
      border-radius: 5px;
      cursor: pointer;
      z-index: 10;
    }
    /* Кнопка поділитися сайтом */
    #shareSiteBtn {
      position: fixed;
      top: 20px;
      left: 20px;
      background-color: #ffc107;
      color: black;
      border: none;
      padding: 10px 15px;
      border-radius: 5px;
      cursor: pointer;
      z-index: 10;
    }
  </style>
</head>
<body>

<header>
  <h1>Маркет Електроніки</h1>
</header>

<button id="shareSiteBtn" onclick="shareSite()">Поділитися сайтом</button>
<button id="logoutBtn" onclick="logout()">Вийти</button>

<div class="form-box" id="register">
  <h2>Реєстрація</h2>
  <input type="email" id="regEmail" placeholder="Електронна пошта" required />
  <input type="password" id="regPassword" placeholder="Пароль (мін. 8 символів)" required />
  <button onclick="register()">Зареєструватися</button>
  <p>Вже зареєстровані? <a href="#" onclick="showLogin()">Увійти</a></p>
</div>

<div class="form-box" id="login" style="display:none;">
  <h2>Вхід</h2>
  <input type="email" id="loginEmail" placeholder="Електронна пошта" required />
  <input type="password" id="loginPassword" placeholder="Пароль" required />
  <button onclick="login()">Увійти</button>
  <p>Ще не маєте акаунту? <a href="#" onclick="showRegister()">Зареєструватися</a></p>
</div>

<div class="products" id="productList"></div>

<script>
  const products = [
    {
      name: "iPhone 14 Pro",
      price: "42 000 грн",
      desc: "6.1\" OLED, A16 Bionic, 128 ГБ",
      img: "https://images.unsplash.com/photo-1603791440384-56cd371ee9a7?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "MacBook Air M2",
      price: "52 000 грн",
      desc: "13.6\" Retina, M2, 256 ГБ SSD",
      img: "https://images.unsplash.com/photo-1555617980-9e8ecd15f4d1?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "Samsung Galaxy Tab S8",
      price: "28 000 грн",
      desc: "11\" LCD, Snapdragon 8 Gen 1, 128 ГБ",
      img: "https://images.unsplash.com/photo-1587825140708-ec88c9d65c1b?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "Sony PlayStation 5",
      price: "26 000 грн",
      desc: "AMD Ryzen, SSD 825 ГБ",
      img: "https://images.unsplash.com/photo-1606813904294-19d6a46b57c2?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "DJI Mini 3 Pro",
      price: "32 000 грн",
      desc: "4K камера, до 34 хв польоту",
      img: "https://images.unsplash.com/photo-1633966885794-0d218266f267?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "Sony WH-1000XM4",
      price: "8 500 грн",
      desc: "Bluetooth, ANC, до 30 год",
      img: "https://images.unsplash.com/photo-1610541381892-fb7f1918b1f9?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "Dell XPS 13",
      price: "46 000 грн",
      desc: "13.4\" FHD+, Intel i7, 512 ГБ SSD",
      img: "https://images.unsplash.com/photo-1606818893285-6e798d76e84c?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "HP Spectre x360",
      price: "48 000 грн",
      desc: "13.3\" Touch, Intel i7, 16 ГБ RAM",
      img: "https://images.unsplash.com/photo-1556909190-efc56b7b5d1f?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "ASUS ZenBook 14",
      price: "40 000 грн",
      desc: "14\" FHD, Ryzen 7, SSD 512 ГБ",
      img: "https://images.unsplash.com/photo-1587202372775-9892625d7ba2?auto=format&fit=crop&w=400&q=80"
    },
    {
      name: "Lenovo Yoga Slim 7",
      price: "42 500 грн",
      desc: "14\" IPS, Intel i5, 8 ГБ RAM",
      img: "https://images.unsplash.com/photo-1610484826904-47d90e6067f3?auto=format&fit=crop&w=400&q=80"
    }
  ];

  function renderProducts() {
    const container = document.getElementById("productList");
    container.innerHTML = "";
    products.forEach((product) => {
      container.innerHTML += `
        <div class="product">
          <img src="${product.img}" alt="${product.name}" />
          <h3>${product.name}</h3>
          <p><strong>Ціна:</strong> ${product.price}</p>
          <p><strong>Характеристики:</strong> ${product.desc}</p>
          <button onclick="alert('Замовлення оформлено!')">Замовити</button>
          <button class="share-button" onclick="shareProduct('${product.name}', '${product.img}')">Поділитися</button>
        </div>
      `;
    });
  }

  function register() {
    const email = document.getElementById("regEmail").value.trim();
    const password = document.getElementById("regPassword").value;

    if (!email.includes("@") || !email.includes(".")) {
      alert("Введіть коректну електронну пошту.");
      return;
    }
    if (password.length < 8) {
      alert("Пароль повинен містити мінімум 8 символів.");
      return;
    }

    let users = JSON.parse(localStorage.getItem("users")) || [];
    if (users.some(user => user.email === email)) {
      alert("Користувач із такою електронною поштою вже зареєстрований.");
      return;
    }

    users.push({ email, password });
    localStorage.setItem("users", JSON.stringify(users));
    alert("Реєстрація успішна! Тепер увійдіть.");

    showLogin();
  }

  function login() {
    const email = document.getElementById("loginEmail").value.trim();
    const password = document.getElementById("loginPassword").value;

    let users = JSON.parse(localStorage.getItem("users")) || [];
    const user = users.find(user => user.email === email && user.password === password);

    if (user) {
      localStorage.setItem("loggedInUser", email);
      alert("Вхід успішний!");
      document.getElementById("register").style.display = "none";
      document.getElementById("login").style.display = "none";
      document.getElementById("productList").style.display = "grid";
      document.getElementById("logoutBtn").style.display = "block";
      renderProducts();
    } else {
      alert("Невірна електронна пошта або пароль.");
    }
  }

  function logout() {
    localStorage.removeItem("loggedInUser");
    alert("Ви вийшли з акаунту.");
    document.getElementById("register").style.display = "block";
    document.getElementById("login").style.display = "none";
    document.getElementById("productList").style.display = "none";
    document.getElementById("logoutBtn").style.display = "none";
  }

  function showLogin() {
    document.getElementById("register").style.display = "none";
    document.getElementById("login").style.display = "block";
  }

  function showRegister() {
    document.getElementById("login").style.display = "none";
    document.getElementById("register").style.display = "block";
  }

  function shareProduct(name, imgUrl) {
    const text = `Переглянь цей товар: ${name} - ${imgUrl}`;
    if (navigator.share) {
      navigator.share({
        title: name,
        text: `Переглянь цей товар: ${name}`,
        url: imgUrl
      }).catch(err => alert('Помилка під час спроби поділитися: ' + err));
    } else {
      navigator.clipboard.writeText(text).then(() => {
        alert("Посилання на товар скопійовано в буфер обміну!");
      }).catch(() => {
        alert("Не вдалося скопіювати посилання. Скопіюйте вручну: " + text);
      });
    }
  }

  function shareSite() {
    const shareData = {
      title: document.title,
      text: 'Переглянь цей крутий сайт з товарами!',
      url: window.location.href
    };

    if (navigator.share) {
      navigator.share(shareData).catch(err => alert('Помилка під час спроби поділитися: ' + err));
    } else {
      navigator.clipboard.writeText(window.location.href).then(() => {
        alert('Посилання на сайт скопійовано в буфер обміну!');
      }).catch(() => {
        alert('Не вдалося скопіювати посилання, скопіюй його вручну: ' + window.location.href);
      });
    }
  }

  // При завантаженні сторінки перевіряємо, чи користувач уже увійшов
  window.onload = () => {
    const loggedInUser = localStorage.getItem("loggedInUser");
    if (loggedInUser) {
      document.getElementById("register").style.display = "none";
      document.getElementById("login").style.display = "none";
      document.getElementById("productList").style.display = "grid";
      document.getElementById("logoutBtn").style.display = "block";
      renderProducts();
    }
  };
</script>

</body>
</html>
