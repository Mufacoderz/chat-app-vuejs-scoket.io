<template>
    <div v-if="!joined" class="parent-container">
        <div class="name-container">
            <h2 class="title">Welcome</h2>
            <input type="text" class="user-name" placeholder="Enter your name..." v-model="currenUser">

            <button class="join-button" @click="join">
                Join Chat
            </button>
        </div>
    </div>
    <div v-if="joined">
        <div class="list-container">
            <div v-for="message in messages" :key="message.id">
                <b>
                    {{ message.user }}
                </b>
                : {{ message.text }}
            </div>
        </div>
        <div class="text-input-container">
            <textarea name="" id="" v-model="text" class="text-message" @keyup.enter="sendMessage">
            </textarea>
        </div>
    </div>
</template>

<script>
import { io } from 'socket.io-client';

export default {
    name: "ChatApp",
    data() {
        return {
            joined: false,
            currenUser: "",
            text: "",
            messages: [],
        }
    },
    methods: {
        join() {
            // console.log("User:", this.currenUser)
            this.joined = true

            this.socketInstance = io("http://localhost:3000")

            this.socketInstance.on("message:received", (data) => {
                console.log("Client received broadcast:", data)
                this.messages = this.messages.concat(data)
            });


        },
        sendMessage() {
            console.log(this.text)
            this.addMessage()

            this.text = ''
        },
        addMessage() {
            const message = {
                id: new Date().getTime(),
                text: this.text,
                user: this.currenUser
            }

            this.messages = this.messages.concat(message)
            this.socketInstance.emit('message', message);
        }
    }
}
</script>

<style scoped>
/* FULL SCREEN WRAPPER */
.parent-container {
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #e8eeee;
    padding: 20px;
}

/* LOGIN CARD */
.name-container {
    display: flex;
    flex-direction: column;
    width: 320px;
    padding: 30px;
    background: white;
    border-radius: 16px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
    text-align: center;
}

.title {
    font-size: 26px;
    color: #2c3e50;
    font-weight: 700;
    margin-bottom: 20px;
}

/* USERNAME INPUT */
.user-name {
    height: 42px;
    font-size: 17px;
    padding: 10px;
    margin-bottom: 14px;
    border: 2px solid #42b883;
    border-radius: 10px;
    outline: none;
    transition: 0.2s ease;
}

.user-name:focus {
    box-shadow: 0 0 8px rgba(66, 184, 131, 0.5);
}

/* JOIN BUTTON MODERN */
.join-button {
    height: 42px;
    font-size: 18px;
    background: #42b883;
    border: none;
    color: white;
    font-weight: bold;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.2s, transform 0.2s;
}

.join-button:hover {
    background: #37a670;
    transform: translateY(-2px);
}

.join-button:active {
    transform: translateY(0px);
}

/* CHAT SCREEN */
.list-container {
    height: calc(100vh - 90px);
    overflow-y: auto;
    padding: 15px;
    box-sizing: border-box;
    background: #f5f7f6;
}

/* MESSAGE BUBBLE */
.list-container div {
    max-width: 70%;
    padding: 10px 14px;
    margin-bottom: 12px;
    border-radius: 12px;
    line-height: 1.4rem;
    background: white;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

/* USERNAME */
.list-container b {
    color: #42b883;
}

/* INPUT AREA FIXED BOTTOM */
.text-input-container {
    position: fixed;
    bottom: 0;
    width: 100%;
    background: white;
    border-top: 1px solid #ddd;
    padding: 10px;
    box-sizing: border-box;
}

/* TEXTAREA MODERN */
.text-message {
    width: 100%;
    height: 60px;
    padding: 12px;
    resize: none;
    font-size: 15px;
    border: 2px solid #42b883;
    border-radius: 10px;
    outline: none;
    box-sizing: border-box;
}

.text-message:focus {
    box-shadow: 0 0 6px rgba(66, 184, 131, 0.4);
}
</style>
