<template>
    <main role="main" class="container">
        <h1 class="mt-5">Laravel Echo Web Sample</h1>
        <br />
        <p>
            Please enter a message that will be sent to the clients (this web
            application and android application) via Laravel Echo.
        </p>
        <form id="newMessageForm" @submit="onSubmit">
            <div class="form-group">
                <label for="messageInput">Message</label>
                <input
                    type="text"
                    class="form-control"
                    id="messageInput"
                    name="message"
                    placeholder="Enter some message"
                />
            </div>
            <button id="submitButton" type="submit" class="btn btn-primary">
                Submit
            </button>
            <br /><br />
            <div class="form-group">
                <label for="messagesList">Received messages:</label>
                <ul class="list-group">
                    <li v-for="message in messages">{{ message }}</li>
                </ul>
            </div>
        </form>
    </main>
</template>

<script>
export default {
    data() {
        return {
            messages: [],
        };
    },
    mounted() {
        let that = this;
        Echo.channel("message").listen("MessageCreated", (data) => {
            that.messages.push(data.message);
        });
    },
    methods: {
        onSubmit(event) {
            event.preventDefault();
            const _message = event.currentTarget.message.value;
            if (!_message) return;
            const _data = new URLSearchParams();
            _data.append("message", _message);
            fetch("/api/message", { method: "POST", body: _data });
        },
    },
};
</script>
