const TELEGRAM_BOT_TOKEN = '8132651954:AAG5GdD63G0Gos8pC9z0bH6P1fsCimhxn9w';
const AI_API_URL = 'https://chatgpt.ashlynn.workers.dev/?question=';

addEventListener('fetch', event => {
   event.respondWith(handleRequest(event.request));
});

async function handleRequest(request) {
   if (request.method === 'POST') {
       const update = await request.json();

       // Log the full update for debugging purposes
       console.log(`Update received: ${JSON.stringify(update)}`);

       let chatId;
       let userMessage;

       if (update.message && update.message.chat && update.message.text) {
           chatId = update.message.chat.id;
           userMessage = update.message.text;

           // Log received message
           console.log(`Received message from ${chatId}: ${userMessage}`);

           const msg = userMessage.toLowerCase().trim();

           if (msg.startsWith('/start')) {
               await sendMessage(chatId, "Welcome.. sir 😊😊. I'm [YOUR] CINDRELLA  how can I assist you today.?.");
           } else if (msg.startsWith('/help')) {
               await sendMessage(chatId, "For support, contact us here: https://t.me/hey_devu ya https://t.me/cyndryk ");
           } else if (msg === 'hi' || msg === 'hello') {
               await sendMessage(chatId, "Hello! How can I help you today? 😊");
           } else {
               const aiResponse = await getAIResponse(userMessage);

               // Log AI response
               console.log(`AI Response for ${chatId}: ${JSON.stringify(aiResponse)}`);

               await sendMessage(chatId, aiResponse.gpt);
           }
       } else {
           console.log('Invalid message or unsupported update type.');
       }

       return new Response('ok', { status: 200 });
   }

   return new Response('i am alive baby ... ping - 32.45ms', { status: 405 });
}

async function getAIResponse(message) {
   const response = await fetch(AI_API_URL + encodeURIComponent(message));
   const data = await response.json();
   return data;
}

async function sendMessage(chatId, text) {
   const telegramApiUrl = `https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage`;
   await fetch(telegramApiUrl, {
       method: 'POST',
       headers: {
           'Content-Type': 'application/json'
       },
       body: JSON.stringify({
           chat_id: chatId,
           text: `${text}\n\n© ᴘᴏᴡᴇʀᴇᴅ ʙʏ ғʀᴏᴢᴇɴ ʙᴏᴛs,,`,
           parse_mode: 'Markdown'
       })
   });
}
