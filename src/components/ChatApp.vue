<template>
    <div v-if="!joined" class="parent-container">
        <div class="name-container">
            <h2 class="title">Welcome</h2>
            <input 
            type="text" 
            class="user-name" 
            placeholder="Enter your name..."
            v-model="currenUser"
            >

            <button class="join-button" @click="join">
            Join Chat
            </button>
        </div>
    </div>
</template>

<script>
export default {
    name: "ChatApp",
    data() {
        return {
            joined: false,
            currenUser: ""
        }
    },
    methods: {
        join() {
            console.log("User:", this.currenUser)
            this.joined = true
        }
    }
}
</script>

<style scoped>
/* Container utama */
.parent-container{
    width: 100%;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #f4f7f6;
}

/* Kotak form */
.name-container{
    display: flex;
    flex-direction: column;
    width: 280px;
    padding: 25px;
    background: white;
    border-radius: 12px;
    box-shadow: 0px 4px 12px rgba(0,0,0,0.1);
    text-align: center;
}

/* Judul */
.title{
    font-size: 24px;
    color: #35495e;
    font-weight: 700;
    margin-bottom: 15px;
}

/* Input username */
.user-name{
    height: 38px;
    font-size: 18px;
    padding: 8px;
    margin-bottom: 12px;
    border: 2px solid #42b883;
    border-radius: 8px;
    outline: none;
    text-align: center;
    transition: 0.2s ease;
}

.user-name:focus{
    box-shadow: 0 0 6px rgba(66,184,131,0.5);
}

/* Tombol Join */
.join-button{
    height: 38px;
    font-size: 18px;
    background: #42b883;
    border: none;
    color: white;
    font-weight: bold;
    border-radius: 8px;
    cursor: pointer;
    transition: 0.2s;
}

.join-button:hover{
    background: #3ca876;
    transform: translateY(-2px);
}

.join-button:active{
    transform: translateY(0px);
}
</style>
