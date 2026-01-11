class RosebudOGP {
    async submitPoints(points = 0) {
        const TIMEOUT_DURATION = 150000;

        return new Promise((resolve, reject) => {
            const requestId = self.crypto.randomUUID();
            const handleMessage = (event) => {
                const { data } = event;
                if (data.requestId === requestId) {
                    window.removeEventListener('message', handleMessage);
                    clearTimeout(timeoutId);
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
                    action: 'ogpSubmitPoints',
                    points: points,
                    requestId: requestId,
                },
                '*',
            );

            const timeoutId = setTimeout(() => {
                window.removeEventListener('message', handleMessage);
                reject(new Error('Request timed out'));
            }, TIMEOUT_DURATION);
        });
    }
}

window.RosebudOGP = new RosebudOGP();
