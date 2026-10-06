<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Chat</title>

<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: Arial, sans-serif;
  background: #f5f5f5;
  color: #222;
  padding-bottom: 75px;
}

/* Верхняя панель */
.header {
  height: 65px;
  background: white;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 15px;
  border-bottom: 1px solid #ddd;
  position: relative;
}

.menu {
  font-size: 27px;
  cursor: pointer;
}

/* Профиль по центру */
.profile {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 45px;
  height: 45px;
  border-radius: 50%;
  background: #ddd;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 25px;
  cursor: pointer;
}

.actions {
  display: flex;
  gap: 18px;
  font-size: 25px;
}

/* Поиск */
.search {
  margin: 12px;
}

.search input {
  width: 100%;
  padding: 13px 18px;
  border: none;
  border-radius: 25px;
  background: white;
  font-size: 16px;
  outline: none;
}

/* Контакты */
.contacts {
  padding: 5px 12px;
}

.contact {
  background: white;
  display: flex;
  align-items: center;
  padding: 12px;
  margin-bottom: 8px;
  border-radius: 12px;
  position: relative;
}

.avatar {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: #ddd;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 25px;
  margin-right: 12px;
}

.name {
  font-size: 17px;
  font-weight: bold;
}

.dots {
  margin-left: auto;
  font-size: 24px;
  cursor: pointer;
}

/* Страницы */
.page {
  display: none;
}

.page.active {
  display: block;
}

.page-title {
  text-align: center;
  font-size: 23px;
  font-weight: bold;
  padding: 20px;
}

/* Нижняя навигация */
.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 70px;
  background: white;
  border-top: 1px solid #ddd;
  display: flex;
  justify-content: space-around;
  align-items: center;
  z-index: 100;
}

.nav-btn {
  border: none;
  background: none;
  font-size: 12px;
  color: #555;
  cursor: pointer;
  text-align: center;
}

.nav-btn .icon {
  display: block;
  font-size: 25px;
  margin-bottom: 3px;
}

.nav-btn.active {
  color: #1877f2;
}

/* Статус */
.status-box,
.photo-box,
.game-box {
  margin: 15px;
  padding: 30px;
  background: white;
  border-radius: 15px;
  text-align: center;
}
</style>
</head>

<body>

<!-- Верх -->
<header class="header">
  <div class="menu">☰</div>

  <!-- Профиль по центру -->
  <div class="profile" onclick="openProfile()">👤</div>

  <div class="actions">
    <span>✏️</span>
    <span>⋮</span>
  </div>
</header>


<!-- ЧАТ -->
<section id="chat" class="page active">

  <div class="search">
    <input type="text" placeholder="Поиск контакта..." onkeyup="searchContacts()">
  </div>

  <div class="contacts" id="contacts">

    <div class="contact">
      <div class="avatar">👤</div>
      <div class="name">АТАМ</div>
      <div class="dots">⋮</div>
    </div>

    <div class="contact">
      <div class="avatar">👤</div>
      <div class="name">АТАМ</div>
      <div class="dots">⋮</div>
    </div>

    <div class="contact">
      <div class="avatar">👤</div>
      <div class="name">ЭМЕЛИ</div>
      <div class="dots">⋮</div>
    </div>

  </div>

</section>


<!-- КОНТАКТЫ -->
<section id="contactsPage" class="page">
  <div class="page-title">Контакты</div>

  <div class="contacts">

    <div class="contact">
      <div class="avatar">👤</div>
      <div class="name">АТАМ</div>
    </div>

    <div class="contact">
      <div class="avatar">👤</div>
      <div class="name">АТАМ</div>
    </div>

    <div class="contact">
      <div class="avatar">👤</div>
      <div class="name">ЭМЕЛИ</div>
    </div>

  </div>
</section>


<!-- СТАТУС -->
<section id="status" class="page">
  <div class="page-title">Статус</div>

  <div class="status-box">
    ➕<br><br>
    Добавить статус
  </div>
</section>


<!-- ФОТО -->
<section id="photos" class="page">
  <div class="page-title">Фото</div>

  <div class="photo-box">
    🖼️<br><br>
    Фотоальбом
  </div>
</section>


<!-- ИГРА -->
<section id="game" class="page">
  <div class="page-title">Игра</div>

  <div class="game-box">
    🎮<br><br>
    Онлайн игры
  </div>
</section>


<!-- НИЖНЕЕ МЕНЮ: РОВНО 5 КНОПОК -->
<nav class="bottom-nav">

  <button class="nav-btn active" onclick="showPage('chat', this)">
    <span class="icon">💬</span>
    Чат
  </button>

  <button class="nav-btn" onclick="showPage('contactsPage', this)">
    <span class="icon">👥</span>
    Контакты
  </button>

  <button class="nav-btn" onclick="showPage('status', this)">
    <span class="icon">➕</span>
    Статус
  </button>

  <button class="nav-btn" onclick="showPage('photos', this)">
    <span class="icon">🖼️</span>
    Фото
  </button>

  <button class="nav-btn" onclick="showPage('game', this)">
    <span class="icon">🎮</span>
    Игра
  </button>

</nav>


<script>

function showPage(pageId, button) {

  document.querySelectorAll(".page").forEach(function(page) {
    page.classList.remove("active");
  });

  document.getElementById(pageId).classList.add("active");

  document.querySelectorAll(".nav-btn").forEach(function(btn) {
    btn.classList.remove("active");
  });

  button.classList.add("active");
}


function searchContacts() {

  let input = document.querySelector(".search input");
  let text = input.value.toLowerCase();

  document.querySelectorAll("#contacts .contact").forEach(function(contact) {

    let name = contact.querySelector(".name").textContent.toLowerCase();

    if (name.includes(text)) {
      contact.style.display = "flex";
    } else {
      contact.style.display = "none";
    }

  });
}


function openProfile() {
  alert("Профиль");
}

</script>

</body>
</html>
