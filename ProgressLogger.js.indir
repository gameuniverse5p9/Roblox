window.ProgressLogger = class ProgressLogger {
    static logProgress(eventName, eventData) {
        window.parent.postMessage({ action: 'logProgress', content: { eventName, eventData } }, '*');
    }
};
