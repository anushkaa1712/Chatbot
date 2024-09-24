# Chatbot

#Chatbot Frontend

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Chat Application</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }

        #chat {
            width: 300px;
            height: 300px;
            border: 1px solid #ccc;
            overflow-y: scroll;
            padding: 10px;
            margin-bottom: 10px;
        }

        #message-input {
            width: 240px;
            padding: 5px;
        }

        #send-btn {
            padding: 5px 10px;
        }
    </style>
</head>

<body>
    <h1>Chat Application</h1>
    <div id="chat"></div>
    <input type="text" id="message-input" placeholder="Type a message...">
    <button id="send-btn">Send</button>

    <script>
        const chatBox = document.getElementById('chat');
        const messageInput = document.getElementById('message-input');
        const sendBtn = document.getElementById('send-btn');

        const socket = new WebSocket('ws://localhost:8765'); // WebSocket connection to the server

        // Append message to the chat box
        function appendMessage(message) {
            const messageElement = document.createElement('div');
            messageElement.textContent = message;
            chatBox.appendChild(messageElement);
            chatBox.scrollTop = chatBox.scrollHeight; // Auto scroll to bottom
        }

        // Receive messages from the WebSocket server
        socket.onmessage = (event) => {
            appendMessage(event.data);
        };

        // Send a message to the server
        sendBtn.addEventListener('click', () => {
            const message = messageInput.value;
            if (message) {
                socket.send(message);  // Send message to WebSocket server
                appendMessage('You: ' + message);
                messageInput.value = '';  // Clear the input field
            }
        });

        // Send message on "Enter" key press
        messageInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                sendBtn.click();
            }
        });
    </script>
</body>

</html>
