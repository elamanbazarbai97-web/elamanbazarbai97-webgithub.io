
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Мой Чат</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#eef1f5;color:#222}
button,input{font:inherit}
button{cursor:pointer;border:none}
.app{max-width:650px;height:100dvh;min-height:450px;margin:auto;background:#fff;display:flex;flex-direction:column;overflow:hidden;position:relative}
.header{padding:14px;display:flex;align-items:center;gap:12px;background:#fff;border-bottom:1px solid #ddd}
.avatar{width:58px;height:58px;border-radius:50%;background:#e1e1e1;object-fit:cover;display:flex;justify-content:center;align-items:center;font-size:30px;flex-shrink:0;overflow:hidden}
.avatar img{width:100%;height:100%;object-fit:cover}
.header-info{flex:1;min-width:0}
.header-info strong{font-size:21px;overflow-wrap:anywhere}
.header-info small{display:block;color:#777;margin-top:5px}
.icon-button{width:44px;height:44px;background:transparent;font-size:25px;border-radius:50%}
.icon-button:active{background:#eee}
.view{display:none;flex:1;min-height:0;flex-direction:column}
.view.active{display:flex}
.section-title{padding:18px;font-size:23px;font-weight:bold;background:#fff;border-bottom:1px solid #eee}
.contact-list{overflow-y:auto;flex:1}
.contact{width:100%;display:flex;align-items:center;gap:12px;padding:13px;background:#fff;border-bottom:1px solid #eee;text-align:left}
.contact:hover{background:#f6f7f8}
.contact-info{flex:1;min-width:0}
.contact-info strong{display:block;font-size:17px}
.contact-info small{display:block;margin-top:5px;color:#777;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.empty{padding:30px 20px;text-align:center;color:#888;line-height:1.5}
.messages{flex:1;overflow-y:auto;padding:15px;background:#f2f3f5;display:flex;flex-direction:column;gap:10px}
.message{max-width:82%;padding:11px 14px;border-radius:17px;background:#fff;overflow-wrap:anywhere;align-self:flex-start;box-shadow:0 1px 2px #0000000a}
.message.mine{background:#d9fdd3;align-self:flex-end}
.message-time{display:block;text-align:right;font-size:10px;color:#777;margin-top:5px}
.composer{padding:10px;display:flex;gap:8px;align-items:center;background:#fff;border-top:1px solid #eee}
.composer input{flex:1;min-width:0;padding:14px 16px;border:1px solid #ddd;border-radius:28px;outline:none}
.composer input:focus{border-color:#777}
.send-button{width:54px;height:54px;flex-shrink:0;background:#505050;color:#fff;border-radius:50%;font-size:23px}
.bottom-nav{display:flex;justify-content:space-around;background:#fff;border-top:1px solid #ddd;padding:6px 0;padding-bottom:max(6px,env(safe-area-inset-bottom))}
.nav-button{flex:1;height:55px;background:#fff;font-size:28px;color:#444}
.nav-button.active{background:#ededed;border-radius:12px}
.profile-content{padding:24px 18px;overflow-y:auto}
.profile-photo{width:110px;height:110px;margin:0 auto 18px;font-size:55px}
.field{margin:18px 0}
.field label{display:block;color:#666;margin-bottom:8px}
.field input,.field select{width:100%;padding:13px;border:1px solid #ddd;border-radius:10px;background:#fff}
.primary-button{width:100%;padding:14px;border-radius:10px;background:#444;color:#fff;margin-top:10px}
.secondary-button{padding:10px 14px;border-radius:10px;background:#eee;color:#222}
#photoInput{display:none}
.row{display:flex;gap:8px;align-items:center;margin:14px 0}
.row i{font-style:normal;font-size:24px;width:34px;text-align:center;flex-shrink:0}
.row input,.row select{flex:1;min-width:0;padding:13px;border:1px solid #ddd;border-radius:10px;background:#fff}
#profilePhone{background:#f4f4f4;color:#555}
.modal{position:absolute;inset:0;background:#0007;display:none;align-items:center;justify-content:center;padding:20px;z-index:10}
.modal.show{display:flex}
.modal-card{width:100%;max-width:380px;padding:22px;background:#fff;border-radius:18px}
.modal-card h3{margin-top:0}
.modal-card input{width:100%;padding:13px;border:1px solid #ddd;border-radius:10px;margin:8px 0}
.modal-actions{display:flex;gap:10px;margin-top:12px}
.modal-actions button{flex:1}
@media (min-width:700px){
body{padding:15px}
.app{height:calc(100dvh - 30px);border-radius:15px;box-shadow:0 3px 20px #00000010}
}
</style>
</head>
<body>
<div class="app">

  <div class="header">
    <div class="avatar" id="headerAvatar">👤</div>
    <div class="header-info">
      <strong id="headerName">Мой Чат</strong>
      <small id="headerStatus">Личные сообщения</small>
    </div>
    <button class="icon-button" id="headerMenu" aria-label="Меню">⋮</button>
  </div>

  <section class="view active" id="chatsView">
    <div class="section-title">Сообщения</div>
    <div class="contact-list" id="contactList"></div>
  </section>

  <section class="view" id="contactsView">
    <div class="section-title">Контакты</div>
    <div class="contact-list" id="allContacts"></div>
    <div style="padding:12px">
      <button class="primary-button" id="addContactButton">➕ Добавить контакт</button>
    </div>
  </section>

  <section class="view" id="chatView">
    <div class="header">
      <button class="icon-button" id="backButton" aria-label="Назад">‹</button>
      <div class="avatar" id="chatAvatar">👤</div>
      <div class="header-info">
        <strong id="chatName">Контакт</strong>
        <small>Личная переписка</small>
      </div>
      <button class="icon-button" id="chatMenu" aria-label="Меню">⋮</button>
    </div>
    <div class="messages" id="messageList"></div>
    <form class="composer" id="messageForm">
      <input id="messageInput" type="text" placeholder="Напишите сообщение..." autocomplete="off" maxlength="2000">
      <button class="send-button" type="submit" aria-label="Отправить">➤</button>
    </form>
  </section>

  <section class="view" id="profileView">
    <div class="section-title">Профиль</div>
    <div class="profile-content">
      <div class="avatar profile-photo" id="profileAvatar">👤</div>
      <button class="secondary-button" id="choosePhotoButton" style="display:block;margin:auto">🖼️ Выбрать фотографию</button>
      <input type="file" id="photoInput" accept="image/*">
      <div class="field"><input id="profileName" maxlength="40" placeholder="Имя Фамилия"></div>
      <div class="field"><input id="profilePhone" readonly placeholder="Тел. номер"></div>
      <div class="row"><i>🎂</i><input id="pBY" type="tel" inputmode="numeric" maxlength="4" placeholder="Год"><input id="pBD" type="tel" inputmode="numeric" maxlength="2" placeholder="День"><select id="pBM"><option value="">Месяц</option><option value="1">Январь</option><option value="2">Февраль</option><option value="3">Март</option><option value="4">Апрель</option><option value="5">Май</option><option value="6">Июнь</option><option value="7">Июль</option><option value="8">Август</option><option value="9">Сентябрь</option><option value="10">Октябрь</option><option value="11">Ноябрь</option><option value="12">Декабрь</option></select></div>
      <div class="row"><i>🏙️</i><input id="pCity" maxlength="40" placeholder="Город"><input id="pVillage" maxlength="40" placeholder="Село"></div>
      <div class="row"><i>🎓</i><input id="pEdu" maxlength="60" placeholder="Образование"></div>
      <div class="row"><i>💼</i><input id="pWork" maxlength="60" placeholder="Работа"></div>
      <div class="row"><i>🎯</i><input id="pHobby" maxlength="60" placeholder="Хобби"></div>
      <div class="row"><i>👤</i><select id="pGender"><option value="">Пол</option><option>Мужской</option><option>Женский</option></select></div>
      <div class="row"><i>💍</i><input id="pFamily" maxlength="40" placeholder="Семейное положение"></div>
      <button class="primary-button" id="saveProfileButton">Сохранить профиль</button>
    </div>
  </section>

  <section class="view" id="moreView">
    <div class="section-title">Дополнительно</div>
    <div class="profile-content">
      <button class="primary-button" id="moreAddContact">➕ Добавить контакт</button>
      <button class="primary-button" id="moreProfile">👤 Мой профиль</button>
      <p class="empty">Добро пожаловать в наш чат! ❤️</p>
    </div>
  </section>

  <nav class="bottom-nav">
    <button class="nav-button active" data-view="chatsView" aria-label="Сообщения">💬</button>
    <button class="nav-button" data-view="contactsView" aria-label="Контакты">👥</button>
    <button class="nav-button" id="bottomAddContact" aria-label="Добавить контакт">➕</button>
    <button class="nav-button" data-view="profileView" aria-label="Профиль">🖼️</button>
    <button class="nav-button" data-view="moreView" aria-label="Дополнительно">🎮</button>
  </nav>

  <div class="modal" id="loginModal">
    <div class="modal-card" style="text-align:center">
      <h3>Вход по номеру</h3>
      <div style="display:flex;gap:8px;align-items:center"><b>+996</b><input id="loginPhone" type="tel" inputmode="numeric" maxlength="12" placeholder="000 000 000"></div>
      <button class="primary-button" id="loginButton">Отправить</button>
    </div>
  </div>

  <div class="modal" id="contactModal">
    <div class="modal-card">
      <h3>Новый контакт</h3>
      <input id="newContactName" type="tel" inputmode="numeric" placeholder="Номер телефона (9 цифр)" maxlength="15">
      <div class="modal-actions">
        <button class="secondary-button" id="cancelContact">Отмена</button>
        <button class="primary-button" id="saveContact" style="margin-top:0">Добавить</button>
      </div>
    </div>
  </div>

  <div class="modal" id="menuModal">
    <div class="modal-card">
      <h3>Меню</h3>
      <button class="primary-button" id="menuProfile">👤 Мой профиль</button>
      <button class="primary-button" id="menuDelete" style="background:#a33">Удалить контакт</button>
      <button class="secondary-button" id="closeMenu" style="width:100%;margin-top:12px">Закрыть</button>
    </div>
  </div>

</div>

<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
import { getDatabase, ref, push, set, get, remove, onChildAdded, query, limitToLast, serverTimestamp }
  from "https://www.gstatic.com/firebasejs/10.12.0/firebase-database.js";

// ===== 1. ВСТАВЬТЕ СЮДА КОНФИГ ИЗ FIREBASE =====
const firebaseConfig = {
  apiKey: "AIzaSyCR1-javU3E3VnLJpEY5N61WggV03r24KM",
  authDomain: "my--chat-7db98.firebaseapp.com",
  databaseURL: "https://my--chat-7db98-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "my--chat-7db98",
  storageBucket: "my--chat-7db98.firebasestorage.app",
  messagingSenderId: "740490057583",
  appId: "1:740490057583:web:fddeb33ad95bf35047ba9a"
};
// ================================================

(function () {
"use strict";

const $ = id => document.getElementById(id);
const STORAGE_KEY = "my_chat_app_v2";
const UID_KEY = "my_chat_uid";

let state = { profile: { name: "", photo: "", phone: "", birth: "", city: "", village: "", edu: "", work: "", hobby: "", gender: "", family: "" }, contacts: [], messages: {}, activeContact: null };
let currentView = "chatsView";
const listening = {};

// ---------- Firebase ----------
let db = null;
if (!firebaseConfig.apiKey.startsWith("ВСТАВЬ")) {
  try { db = getDatabase(initializeApp(firebaseConfig)); }
  catch (e) { console.error(e); }
}

let myUid = "";
try { myUid = localStorage.getItem(UID_KEY) || ""; } catch (e) {}
if (!myUid) {
  myUid = "u_" + Date.now().toString(36) + Math.random().toString(36).slice(2, 10);
  try { localStorage.setItem(UID_KEY, myUid); } catch (e) {}
}

const normPhone = v => { const d = String(v).replace(/\D/g, ""); if (d.length === 9) return "996" + d; return /^996\d{9}$/.test(d) ? d : ""; };
const showName = (n, k) => n || "+" + k;
const myKey = () => state.profile.phone;
const chatIdWith = otherKey => [myKey(), otherKey].sort().join("__");
const fmtTime = ts => new Date(ts || Date.now()).toLocaleTimeString("ru-RU", { hour: "2-digit", minute: "2-digit" });

function saveRemote() {
  const p = state.profile;
  return set(ref(db, "users/" + p.phone), { uid: myUid, name: p.name || "", phone: p.phone, birth: p.birth, city: p.city, village: p.village, edu: p.edu, work: p.work, hobby: p.hobby, gender: p.gender, family: p.family });
}

function refreshAfterMessage(id) {
  if (currentView === "chatView" && state.activeContact === id) renderMessages();
  if (currentView === "chatsView") renderChats();
}

function listenChat(contact) {
  if (!db || listening[contact.id]) return;
  listening[contact.id] = true;
  const q = query(ref(db, "chats/" + chatIdWith(contact.id) + "/messages"), limitToLast(200));
  onChildAdded(q, snap => {
    const m = snap.val();
    if (!m || typeof m.text !== "string") return;
    const list = state.messages[contact.id] || (state.messages[contact.id] = []);
    if (list.some(x => x.key === snap.key)) return;
    list.push({ key: snap.key, text: m.text, mine: m.from === myKey(), time: fmtTime(m.ts) });
    refreshAfterMessage(contact.id);
  });
}

function listenInbox() {
  onChildAdded(ref(db, "inbox/" + myKey()), snap => {
    const v = snap.val();
    const id = snap.key;
    if (!v || id === myKey()) return;
    let contact = state.contacts.find(c => c.id === id);
    if (!contact) {
      contact = { id: id, name: showName(v.name, id), photo: "" };
      state.contacts.push(contact);
      saveState();
      renderContacts();
      renderChats();
    }
    listenChat(contact);
  });
}

// ---------- Хранение (локально: профиль и контакты) ----------
function loadState() {
  try {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) {
      const data = JSON.parse(saved);
      if (data && typeof data === "object") {
        state.profile = Object.assign(state.profile, data.profile || {});
        state.contacts = Array.isArray(data.contacts) ? data.contacts.filter(c => /^996\d{9}$/.test(c.id)) : [];
      }
    }
  } catch (error) {
    console.log("Не удалось загрузить сохранённые данные.");
  }
}

function saveState() {
  try {
    localStorage.setItem(STORAGE_KEY, JSON.stringify({ profile: state.profile, contacts: state.contacts }));
  } catch (error) {
    alert("Не удалось сохранить данные. Возможно, память браузера заполнена.");
  }
}

// ---------- Интерфейс ----------
function makeAvatar(photo, name) {
  const avatar = document.createElement("div");
  avatar.className = "avatar";
  if (photo) {
    const img = document.createElement("img");
    img.src = photo;
    img.alt = "";
    avatar.appendChild(img);
  } else {
    avatar.textContent = name ? name.trim().charAt(0).toUpperCase() : "👤";
  }
  return avatar;
}

function updateHeader() {
  $("headerName").textContent = showName(state.profile.name, state.profile.phone);
  const avatar = makeAvatar(state.profile.photo, state.profile.name);
  avatar.id = "headerAvatar";
  $("headerAvatar").replaceWith(avatar);

  $("profileName").value = state.profile.name;
  [["pCity","city"],["pVillage","village"],["pEdu","edu"],["pWork","work"],["pHobby","hobby"],["pGender","gender"],["pFamily","family"]].forEach(([i, k]) => { $(i).value = state.profile[k] || ""; });
  const bp = (state.profile.birth || "").split("-");
  $("pBY").value = +bp[0] || "";
  $("pBM").value = +bp[1] || "";
  $("pBD").value = +bp[2] || "";
  $("profilePhone").value = state.profile.phone ? "+" + state.profile.phone : "";
  const profileAvatar = makeAvatar(state.profile.photo, state.profile.name);
  profileAvatar.classList.add("profile-photo");
  profileAvatar.id = "profileAvatar";
  $("profileAvatar").replaceWith(profileAvatar);
}

function showView(viewId) {
  if (viewId === "chatView" && !state.activeContact) viewId = "chatsView";
  currentView = viewId;

  document.querySelectorAll(".view").forEach(v => v.classList.toggle("active", v.id === viewId));
  document.querySelectorAll(".nav-button[data-view]").forEach(b => b.classList.toggle("active", b.dataset.view === viewId));

  const status = {
    chatsView: "Личные сообщения",
    contactsView: "Ваши контакты",
    profileView: "Настройки профиля",
    moreView: "Меню"
  };
  if (status[viewId]) $("headerStatus").textContent = status[viewId];

  if (viewId === "chatsView") renderChats();
  if (viewId === "contactsView") renderContacts();
  if (viewId === "profileView") updateHeader();
}

function openContact(contactId) {
  const contact = state.contacts.find(c => c.id === contactId);
  if (!contact) return;
  state.activeContact = contactId;
  $("chatName").textContent = contact.name;
  const avatar = makeAvatar(contact.photo, contact.name);
  avatar.id = "chatAvatar";
  $("chatAvatar").replaceWith(avatar);
  $("headerStatus").textContent = "Переписка с контактом";
  showView("chatView");
  renderMessages();
}

function makeContactButton(contact, showLastMessage) {
  const button = document.createElement("button");
  button.className = "contact";
  button.appendChild(makeAvatar(contact.photo, contact.name));

  const info = document.createElement("div");
  info.className = "contact-info";
  const name = document.createElement("strong");
  name.textContent = contact.name;
  const last = document.createElement("small");
  const messages = state.messages[contact.id] || [];
  last.textContent = showLastMessage
    ? (messages.length ? messages[messages.length - 1].text : "Начните общение")
    : "Открыть переписку";
  info.append(name, last);
  button.appendChild(info);
  button.addEventListener("click", () => openContact(contact.id));
  return button;
}

function renderList(listId, emptyText, showLast) {
  const list = $(listId);
  list.replaceChildren();
  if (!state.contacts.length) {
    const empty = document.createElement("div");
    empty.className = "empty";
    empty.textContent = emptyText;
    list.appendChild(empty);
    return;
  }
  state.contacts.forEach(c => list.appendChild(makeContactButton(c, showLast)));
}

function renderChats() {
  renderList("contactList", "Пока нет контактов. Нажмите ➕, чтобы добавить первый контакт.", true);
}

function renderContacts() {
  renderList("allContacts", "Здесь пока нет контактов.", false);
}

function renderMessages() {
  const list = $("messageList");
  list.replaceChildren();
  const messages = state.messages[state.activeContact] || [];

  if (!messages.length) {
    const empty = document.createElement("div");
    empty.className = "empty";
    empty.textContent = "Начните общение с этим контактом ❤️";
    list.appendChild(empty);
    return;
  }

  messages.forEach(item => {
    const message = document.createElement("div");
    message.className = "message" + (item.mine ? " mine" : "");
    const text = document.createElement("div");
    text.textContent = item.text;
    const time = document.createElement("span");
    time.className = "message-time";
    time.textContent = item.time || "";
    message.append(text, time);
    list.appendChild(message);
  });
  list.scrollTop = list.scrollHeight;
}

// ---------- Действия ----------
async function addContact() {
  const key = normPhone($("newContactName").value);
  if (!key) { alert("Введи номер: 9 цифр после +996."); return; }
  if (!db) { alert("Firebase не настроен."); return; }

  if (key === myKey()) { alert("Это твой номер."); return; }
  if (state.contacts.some(c => c.id === key)) { alert("Такой контакт уже существует."); return; }

  try {
    const snap = await get(ref(db, "users/" + key));
    if (!snap.exists()) {
      alert("Пользователя с таким номером нет. Пусть он сначала войдёт в чат.");
      return;
    }
    const contact = { id: key, name: showName(snap.val().name, key), photo: "" };
    state.contacts.push(contact);
    saveState();
    listenChat(contact);

    $("newContactName").value = "";
    $("contactModal").classList.remove("show");
    renderChats();
    renderContacts();
    openContact(contact.id);
  } catch (e) {
    console.error(e);
    alert("Не удалось связаться с сервером.");
  }
}

function sendMessage(event) {
  event.preventDefault();
  if (!state.activeContact) return;
  if (!db) { alert("Firebase не настроен."); return; }

  const input = $("messageInput");
  const text = input.value.trim();
  if (!text) return;

  const id = state.activeContact;
  input.value = "";

  push(ref(db, "chats/" + chatIdWith(id) + "/messages"), {
    from: myKey(), text: text, ts: serverTimestamp()
  }).catch(() => alert("Не удалось отправить сообщение."));

  set(ref(db, "inbox/" + id + "/" + myKey()), {
    name: state.profile.name, ts: serverTimestamp()
  }).catch(() => {});
}

function openAddContact() {
  $("contactModal").classList.add("show");
  $("newContactName").focus();
}

function deleteCurrentContact() {
  if (!state.activeContact) { alert("Сначала открой контакт."); return; }
  const contact = state.contacts.find(c => c.id === state.activeContact);
  if (!contact) return;
  if (!confirm("Удалить контакт «" + contact.name + "»?")) return;

  state.contacts = state.contacts.filter(c => c.id !== state.activeContact);
  delete state.messages[state.activeContact];
  state.activeContact = null;
  saveState();

  $("menuModal").classList.remove("show");
  renderChats();
  renderContacts();
  showView("chatsView");
}

$("photoInput").addEventListener("change", function () {
  const file = this.files && this.files[0];
  if (!file) return;
  if (!file.type.startsWith("image/")) { alert("Выбери файл фотографии."); this.value = ""; return; }
  if (file.size > 2 * 1024 * 1024) { alert("Выбери фото размером до 2 МБ."); this.value = ""; return; }

  const reader = new FileReader();
  reader.onload = function () {
    state.profile.photo = reader.result;
    saveState();
    updateHeader();
  };
  reader.readAsDataURL(file);
  this.value = "";
});

$("saveProfileButton").addEventListener("click", async function () {
  const p = state.profile;
  p.name = $("profileName").value.trim();
  const by = $("pBY").value.trim(), bd = +$("pBD").value, bm = +$("pBM").value;
  if (by && (!/^\d{4}$/.test(by) || +by < 1900 || +by > new Date().getFullYear())) { alert("Проверь год рождения."); return; }
  if (bd && (bd < 1 || bd > 31)) { alert("Проверь день рождения."); return; }
  p.birth = by ? by + "-" + String(bm).padStart(2, "0") + "-" + String(bd).padStart(2, "0") : "";
  p.city = $("pCity").value.trim();
  p.village = $("pVillage").value.trim();
  p.edu = $("pEdu").value.trim();
  p.work = $("pWork").value.trim();
  p.hobby = $("pHobby").value.trim();
  p.gender = $("pGender").value;
  p.family = $("pFamily").value.trim();
  saveState();
  try { if (db) await saveRemote(); } catch (e) { alert("Не удалось сохранить на сервере."); return; }
  updateHeader();
  alert("Профиль сохранён! ❤️");
});

$("choosePhotoButton").addEventListener("click", () => $("photoInput").click());
$("messageForm").addEventListener("submit", sendMessage);

$("backButton").addEventListener("click", function () {
  state.activeContact = null;
  showView("chatsView");
});

$("addContactButton").addEventListener("click", openAddContact);
$("bottomAddContact").addEventListener("click", openAddContact);
$("moreAddContact").addEventListener("click", openAddContact);
$("cancelContact").addEventListener("click", () => $("contactModal").classList.remove("show"));
$("saveContact").addEventListener("click", addContact);
$("newContactName").addEventListener("keydown", e => { if (e.key === "Enter") addContact(); });

$("headerMenu").addEventListener("click", function () {
  $("menuDelete").style.display = currentView === "chatView" ? "block" : "none";
  $("menuModal").classList.add("show");
});

$("chatMenu").addEventListener("click", function () {
  $("menuDelete").style.display = "block";
  $("menuModal").classList.add("show");
});

$("closeMenu").addEventListener("click", () => $("menuModal").classList.remove("show"));
$("menuDelete").addEventListener("click", deleteCurrentContact);

function openProfile() {
  $("menuModal").classList.remove("show");
  showView("profileView");
}
$("menuProfile").addEventListener("click", openProfile);
$("moreProfile").addEventListener("click", openProfile);

document.querySelectorAll(".nav-button[data-view]").forEach(button => {
  button.addEventListener("click", function () {
    state.activeContact = null;
    showView(this.dataset.view);
  });
});

["contactModal", "menuModal"].forEach(id => {
  $(id).addEventListener("click", function (event) {
    if (event.target === this) this.classList.remove("show");
  });
});

// ---------- Запуск ----------
loadState();
updateHeader();
renderChats();
renderContacts();
showView("chatsView");

function startOnline() {
  $("loginModal").classList.remove("show");
  updateHeader();
  if (!db) { alert("Нет подключения к Firebase."); return; }
  get(ref(db, "users/" + state.profile.phone)).then(snap => {
    const v = snap.val();
    if (v) {
      ["name", "birth", "city", "village", "edu", "work", "hobby", "gender", "family"].forEach(k => { if (!state.profile[k] && v[k]) state.profile[k] = v[k]; });
      saveState();
      updateHeader();
    }
    return saveRemote();
  }).then(() => {
    listenInbox();
    state.contacts.forEach(listenChat);
  }).catch(() => alert("Нет связи с Firebase. Проверьте правила базы."));
}

$("loginButton").addEventListener("click", function () {
  const phone = normPhone($("loginPhone").value);
  if (!phone) { alert("Введи номер: 9 цифр после +996."); return; }
  state.profile.phone = phone;
  saveState();
  startOnline();
});

if (state.profile.phone) startOnline(); else $("loginModal").classList.add("show");

})();
</script>
</body>
</html>
