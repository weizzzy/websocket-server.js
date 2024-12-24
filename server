const WebSocket = require('ws');
const axios = require('axios');

const PORT = process.env.PORT || 8080;
const wss = new WebSocket.Server({ port: PORT }, () => {
    console.log(`WebSocket server is running on ws://localhost:${PORT}`);
});
wss.on('connection', (ws) => {
    console.log('Client connected');

    ws.on('message', async (message) => {
        try {

            const data = JSON.parse(message);
            const { botToken, chatId, text } = data;

            if (!botToken || !chatId || !text) {
                ws.send(JSON.stringify({ status: 'error', message: 'Missing parameters' }));
                return;
            }


            const telegramUrl = `https://api.telegram.org/bot${botToken}/sendMessage`;
            const response = await axios.post(telegramUrl, {
                chat_id: chatId,
                text: text,
            });

            ws.send(JSON.stringify({ status: 'success', data: response.data }));
        } catch (error) {
            console.error('Error:', error.message);
            ws.send(JSON.stringify({ status: 'error', message: error.message }));
        }
    });

    ws.on('close', () => {
        console.log('Client disconnected');
    });
});
