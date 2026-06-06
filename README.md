<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Send Unknownymous</title>

  <style>
    body {
      font-family: Arial;
      background: #0f0f0f;
      color: white;
      text-align: center;
      margin: 0;
      padding: 20px;
    }

    h2 {
      margin-bottom: 15px;
    }

    input, button {
      padding: 12px;
      margin: 8px;
      width: 85%;
      max-width: 350px;
      border-radius: 8px;
      border: none;
      outline: none;
    }

    button {
      background: #00c853;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }

    button:hover {
      background: #00e676;
    }

    .msg {
      background: #1e1e1e;
      margin: 10px auto;
      padding: 12px;
      width: 85%;
      max-width: 420px;
      border-radius: 10px;
      text-align: left;
    }
  </style>
</head>

<body>

  <h2>💬 Send Unknownymous</h2>

  <input id="text" placeholder="Write your anonymous message...">
  <button onclick="sendMsg()">Send</button>

  <h3>Messages</h3>
  <div id="messages">Loading...</div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/12.14.0/firebase-app.js";
    import { getDatabase, ref, push, onValue } from "https://www.gstatic.com/firebasejs/12.14.0/firebase-database.js";

    // 🔥 Firebase Config (your project)
    const firebaseConfig = {
      apiKey: "AIzaSyCdBqPWnWLLxFot4B17gL8khtLJwY-XEkc",
      authDomain: "store-de950.firebaseapp.com",
      databaseURL: "https://store-de950-default-rtdb.firebaseio.com",
      projectId: "store-de950",
      storageBucket: "store-de950.firebasestorage.app",
      messagingSenderId: "156999960612",
      appId: "1:156999960612:web:0fc18e5bb9752d832970b1"
    };

    const app = initializeApp(firebaseConfig);
    const db = getDatabase(app);

    const messagesRef = ref(db, "messages");

    // 📤 SEND MESSAGE
    window.sendMsg = function () {
      const input = document.getElementById("text");
      const text = input.value;

      if (!text.trim()) return;

      push(messagesRef, {
        text: text,
        timestamp: Date.now()
      });

      input.value = "";
    };

    // 📥 LOAD MESSAGES LIVE
    onValue(messagesRef, (snapshot) => {
      const box = document.getElementById("messages");
      box.innerHTML = "";

      const data = snapshot.val();

      if (!data) {
        box.innerHTML = "No messages yet...";
        return;
      }

      Object.values(data).reverse().forEach((msg) => {
        const div = document.createElement("div");
        div.className = "msg";
        div.innerText = msg.text;
        box.appendChild(div);
      });
    });

  </script>

</body>
</html>
