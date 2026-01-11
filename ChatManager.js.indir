window.ChatManager = class ChatManager {
    TIMEOUT_DURATION = 60000;
    chatHistory;
    characterDescription;

    constructor(characterDescription) {
        this.chatHistory = [];
        this.characterDescription = characterDescription;
    }

    addMessage(role, content) {
        this.chatHistory.push({ role, content });
    }

    getChatHistory() {
        return this.chatHistory;
    }

    cleanChatHistory() {
        this.chatHistory = [];
    }

    async getCharacterResponse(operation = 'chat', maxTokens = 384) {
        const payload = {
            character: this.characterDescription,
            messages: this.chatHistory,
            maxTokens: maxTokens,
            operation: operation,
        };

        return new Promise((resolve, reject) => {
            const requestId = self.crypto.randomUUID();
            const handleMessage = (event) => {
                const { data } = event;
                if (data.requestId === requestId) {
                    window.removeEventListener('message', handleMessage);
                    if (data.error) {
                        reject(new Error(data.error));
                    } else {
                        resolve(data.content.message);
                    }
                }
            };

            window.addEventListener('message', handleMessage);

            window.parent.postMessage(
                {
                    action: 'requestLLM',
                    payload: payload,
                    requestId: requestId,
                },
                '*',
            );

            setTimeout(() => {
                window.removeEventListener('message', handleMessage);
                reject(new Error('Request timed out'));
            }, this.TIMEOUT_DURATION);
        });
    }
};
