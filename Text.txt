<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Live Chat</title>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js"></script>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; background: #f4f4f4; padding: 20px; }
        #chatbox { width: 80%; height: 400px; border: 1px solid #ccc; overflow-y: auto; background: white; padding: 10px; margin: 10px auto; }
        input { width: 70%; padding: 10px; }
        button { padding: 10px; background: #007BFF; color: white; border: none; cursor: pointer; }
        button:hover { background: #0056b3; }
    </style>
</head>
<body>
    <h2>Live Chat für Alle</h2>
    <div id="chatbox"></div>
    <input type="text" id="username" placeholder="Dein Name">
    <input type="text" id="message" placeholder="Nachricht eingeben...">
    <button onclick="sendMessage()">Senden</button>

    <script>
        // Deine Firebase-Konfiguration (ersetze diese mit deinen echten Daten)
        const firebaseConfig = {
            apiKey: "DEIN_API_KEY",
            authDomain: "DEIN_PROJEKT.firebaseapp.com",
            projectId: "DEIN_PROJEKT",
            storageBucket: "DEIN_PROJEKT.appspot.com",
            messagingSenderId: "DEINE_ID",
            appId: "DEINE_APP_ID"
        };

        // Firebase initialisieren
        firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        const chatbox = document.getElementById("chatbox");

        // Nachrichten senden
        function sendMessage() {
            const username = document.getElementById("username").value;
            const message = document.getElementById("message").value;
            if (username.trim() !== "" && message.trim() !== "") {
                db.collection("messages").add({
                    user: username,
                    text: message,
                    timestamp: firebase.firestore.FieldValue.serverTimestamp()
                });
                document.getElementById("message").value = "";
            }
        }

        // Nachrichten in Echtzeit empfangen
        db.collection("messages").orderBy("timestamp").onSnapshot(snapshot => {
            chatbox.innerHTML = "";
            snapshot.forEach(doc => {
                const data = doc.data();
                const msgElement = document.createElement("p");
                msgElement.textContent = `${data.user}: ${data.text}`;
                chatbox.appendChild(msgElement);
            });
            chatbox.scrollTop = chatbox.scrollHeight;
        });
    </script>
</body>
</html>
