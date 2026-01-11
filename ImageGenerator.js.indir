window.ImageGenerator = class ImageGenerator {
    static async requestEphemeralImageGeneration({ prompt, removeBackground = true } = {}) {
        const TIMEOUT_DURATION = 150000;

        const modelName = removeBackground ? 'Image (bg removed)' : 'Image';

        const payload = {
            prompt: prompt,
            modelName: modelName,
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
                        const imageURL = URL.createObjectURL(data.content.blob);
                        resolve(imageURL);
                    }
                }
            };

            window.addEventListener('message', handleMessage);

            window.parent.postMessage(
                {
                    action: 'requestEphemeralImageGeneration',
                    payload: payload,
                    requestId: requestId,
                },
                '*',
            );

            setTimeout(() => {
                window.removeEventListener('message', handleMessage);
                reject(new Error('Request timed out'));
            }, TIMEOUT_DURATION);
        });
    }
};
