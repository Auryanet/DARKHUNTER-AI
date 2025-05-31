<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>DarkHunter AI</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #1e1e2d;
            --secondary-color: #3b82f6;
            --accent-color: #8b5cf6;
            --dark-color: #0f172a;
            --light-color: #f8fafc;
            --text-color: #e2e8f0;
            --user-bubble: #334155;
            --ai-bubble: #1e293b;
            --background: #0f172a;
            --input-bg: #1e293b;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background-color: var(--background);
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: flex-start;
            min-height: 100vh;
            color: var(--text-color);
            overflow-x: hidden;
            touch-action: manipulation;
        }

        .chat-container {
            width: 100vw;
            height: 100vh;
            max-width: 100%;
            background-color: var(--dark-color);
            display: flex;
            flex-direction: column;
            overflow: hidden;
            border-radius: 0;
        }

        .chat-header {
            background: linear-gradient(135deg, var(--primary-color), var(--dark-color));
            color: white;
            padding: 14px 16px;
            text-align: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            flex-shrink: 0;
        }

        .chat-header h1 {
            margin: 0;
            font-size: 1.3rem;
            font-weight: 600;
            background: linear-gradient(to right, #8b5cf6, #3b82f6);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .chat-messages {
            flex: 1;
            padding: 12px;
            overflow-y: auto;
            -webkit-overflow-scrolling: touch;
            scroll-behavior: smooth;
        }

        .message {
            margin-bottom: 12px;
            display: flex;
            animation: messageIn 0.25s ease-out forwards;
            opacity: 0;
            transform: translateY(8px);
        }

        @keyframes messageIn {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .message-user {
            justify-content: flex-end;
        }

        .message-ai {
            justify-content: flex-start;
        }

        .message-bubble {
            max-width: 85%;
            padding: 10px 14px;
            border-radius: 14px;
            line-height: 1.4;
            font-size: 0.95rem;
            word-break: break-word;
        }

        .user-bubble {
            background-color: var(--user-bubble);
            border-top-right-radius: 4px;
            border-bottom-right-radius: 4px;
            border-left: 2px solid var(--accent-color);
        }

        .ai-bubble {
            background-color: var(--ai-bubble);
            border-top-left-radius: 4px;
            border-bottom-left-radius: 4px;
            border-right: 2px solid var(--accent-color);
        }

        .message-time {
            font-size: 0.6rem;
            opacity: 0.6;
            margin-top: 4px;
            text-align: right;
            color: #94a3b8;
        }

        .chat-input-container {
            padding: 10px 12px;
            background-color: var(--dark-color);
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            flex-shrink: 0;
        }

        .chat-input {
            display: flex;
            background-color: var(--input-bg);
            border-radius: 22px;
            padding: 3px;
        }

        .chat-input input {
            flex: 1;
            padding: 10px 14px;
            background: transparent;
            border: none;
            border-radius: 20px;
            outline: none;
            font-size: 0.9rem;
            color: var(--text-color);
            min-height: 20px;
        }

        .chat-input input::placeholder {
            color: #64748b;
            font-size: 0.9rem;
        }

        .chat-input button {
            background: linear-gradient(135deg, var(--accent-color), var(--secondary-color));
            color: white;
            border: none;
            border-radius: 50%;
            width: 36px;
            height: 36px;
            margin-left: 6px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            flex-shrink: 0;
        }

        .chat-input button:disabled {
            background: #475569;
            cursor: not-allowed;
        }

        .chat-input button i {
            font-size: 0.9rem;
        }

        .typing-indicator {
            display: flex;
            padding: 8px 14px;
            background-color: var(--ai-bubble);
            border-radius: 16px;
            max-width: 65px;
            margin-bottom: 12px;
            align-self: flex-start;
            border-right: 2px solid var(--accent-color);
        }

        .typing-indicator span {
            height: 7px;
            width: 7px;
            background-color: var(--accent-color);
            border-radius: 50%;
            display: inline-block;
            margin: 0 3px;
            animation: bounce 1.5s infinite ease-in-out;
        }

        @keyframes bounce {
            0%, 60%, 100% { transform: translateY(0); }
            30% { transform: translateY(-3px); }
        }

        /* Markdown formatting */
        .ai-bubble strong {
            font-weight: 600;
        }

        .ai-bubble code {
            font-family: monospace;
            background-color: rgba(0, 0, 0, 0.3);
            padding: 2px 4px;
            border-radius: 3px;
            font-size: 0.85em;
        }

        .ai-bubble pre {
            background-color: rgba(0, 0, 0, 0.3);
            padding: 10px;
            border-radius: 6px;
            overflow-x: auto;
            font-family: monospace;
            font-size: 0.85em;
            margin: 8px 0;
        }

        /* Mobile optimizations */
        @media (max-width: 480px) {
            .message-bubble {
                max-width: 90%;
                padding: 8px 12px;
                font-size: 0.9rem;
            }
            
            .chat-input input {
                padding: 8px 12px;
                font-size: 0.85rem;
            }
            
            .chat-input button {
                width: 34px;
                height: 34px;
            }
        }

        /* Scrollbar for mobile */
        ::-webkit-scrollbar {
            width: 4px;
        }
        
        ::-webkit-scrollbar-thumb {
            background: var(--accent-color);
            border-radius: 2px;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <header class="chat-header">
            <h1>DarkHunter AI</h1>
        </header>
        
        <div class="chat-messages" id="chat-messages">
            <!-- Messages will appear here -->
        </div>
        
        <div class="chat-input-container">
            <div class="chat-input">
                <input type="text" id="user-input" placeholder="Type a message..." autocomplete="off">
                <button id="send-button"><i class="fas fa-paper-plane"></i></button>
                <button id="mic-button"><i class="fas fa-microphone"></i></button>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const chatMessages = document.getElementById('chat-messages');
            const userInput = document.getElementById('user-input');
            const sendButton = document.getElementById('send-button');
            const micButton = document.getElementById('mic-button');
            
            // Google Gemini API Configuration
            const API_KEY = 'AIzaSyChPbiKtVSMgGynJFElfzxaTJs5lJ8vGQ8';
            const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${API_KEY}`;
            
            // Conversation history
            let conversationHistory = [
                { 
                    role: "user", 
                    parts: [{ text: "You are DarkHunter AI, a mobile-optimized AI assistant. Provide concise, helpful responses formatted for small screens. Use markdown sparingly (bold, lists). Keep responses brief but informative." }]
                }
            ];
            
            // Initialize the chat
            setTimeout(() => {
                addMessage('ai', 'Hello! How can I help you today?');
                conversationHistory.push({ 
                    role: "model", 
                    parts: [{ text: "Hello! How can I help you today?" }]
                });
            }, 600);
            
            // Event listeners
            sendButton.addEventListener('click', sendMessage);
            userInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault();
                    sendMessage();
                }
            });
            
            // Auto-resize input based on content
            userInput.addEventListener('input', function() {
                this.style.height = 'auto';
                this.style.height = (this.scrollHeight) + 'px';
            });
            
            // Voice recognition
            if ('webkitSpeechRecognition' in window) {
                const recognition = new webkitSpeechRecognition();
                recognition.continuous = false;
                recognition.interimResults = false;
                
                micButton.addEventListener('click', function() {
                    if (micButton.classList.contains('listening')) {
                        recognition.stop();
                        micButton.classList.remove('listening');
                        userInput.placeholder = "Type a message...";
                    } else {
                        recognition.start();
                        micButton.classList.add('listening');
                        userInput.placeholder = "Listening...";
                        sendButton.disabled = true;
                    }
                });
                
                recognition.onresult = function(event) {
                    const transcript = event.results[0][0].transcript;
                    userInput.value = transcript;
                    userInput.placeholder = "Type a message...";
                    micButton.classList.remove('listening');
                    sendButton.disabled = false;
                    sendMessage();
                };
                
                recognition.onerror = function(event) {
                    userInput.placeholder = "Type a message...";
                    micButton.classList.remove('listening');
                    sendButton.disabled = false;
                };
            } else {
                micButton.style.display = 'none';
            }
            
            function sendMessage() {
                const message = userInput.value.trim();
                if (message === '') return;
                
                // Disable input during processing
                sendButton.disabled = true;
                userInput.disabled = true;
                userInput.style.height = 'auto';
                
                addMessage('user', message);
                conversationHistory.push({ role: "user", parts: [{ text: message }] });
                userInput.value = '';
                showTypingIndicator();
                fetchAIResponse();
            }
            
            function addMessage(sender, text) {
                const messageDiv = document.createElement('div');
                messageDiv.classList.add('message', `message-${sender}`);
                
                const bubbleDiv = document.createElement('div');
                bubbleDiv.classList.add('message-bubble', `${sender}-bubble`);
                
                // Simple markdown formatting for mobile
                if (sender === 'ai') {
                    bubbleDiv.innerHTML = text
                        .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>')
                        .replace(/\*(.*?)\*/g, '<em>$1</em>')
                        .replace(/\n/g, '<br>');
                } else {
                    bubbleDiv.textContent = text;
                }
                
                const timeSpan = document.createElement('span');
                timeSpan.classList.add('message-time');
                timeSpan.textContent = getCurrentTime();
                
                bubbleDiv.appendChild(timeSpan);
                messageDiv.appendChild(bubbleDiv);
                chatMessages.appendChild(messageDiv);
                
                // Smooth scroll to bottom
                setTimeout(() => {
                    chatMessages.scrollTop = chatMessages.scrollHeight;
                }, 50);
            }
            
            function showTypingIndicator() {
                const typingDiv = document.createElement('div');
                typingDiv.classList.add('typing-indicator');
                typingDiv.id = 'typing-indicator';
                
                for (let i = 0; i < 3; i++) {
                    const dot = document.createElement('span');
                    typingDiv.appendChild(dot);
                }
                
                chatMessages.appendChild(typingDiv);
                chatMessages.scrollTop = chatMessages.scrollHeight;
            }
            
            function hideTypingIndicator() {
                const typingIndicator = document.getElementById('typing-indicator');
                if (typingIndicator) typingIndicator.remove();
            }
            
            function getCurrentTime() {
                return new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });
            }
            
            async function fetchAIResponse() {
                try {
                    const payload = {
                        contents: conversationHistory,
                        generationConfig: {
                            maxOutputTokens: 800 // Shorter responses for mobile
                        }
                    };
                    
                    const response = await fetch(API_URL, {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json'
                        },
                        body: JSON.stringify(payload)
                    });
                    
                    if (!response.ok) throw new Error('API request failed');
                    
                    const data = await response.json();
                    hideTypingIndicator();
                    
                    if (data.candidates?.[0]?.content?.parts?.[0]?.text) {
                        const aiReply = data.candidates[0].content.parts[0].text;
                        addMessage('ai', aiReply);
                        conversationHistory.push({ 
                            role: "model", 
                            parts: [{ text: aiReply }] 
                        });
                    } else {
                        throw new Error("No response from AI");
                    }
                    
                } catch (error) {
                    hideTypingIndicator();
                    addMessage('ai', "Sorry, I'm having trouble responding. Please try again.");
                    console.error('Error:', error);
                } finally {
                    sendButton.disabled = false;
                    userInput.disabled = false;
                    userInput.focus();
                }
            }
        });
    </script>
</body>
</html>
