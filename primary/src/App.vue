


<template>
  <div class="container">
      <h1>URL Shortener</h1>

      <input v-model="originalUrl" type="text" placeholder="Enter a URL... "/>
      <button @click="shortenUrl">Shorten</button>

      <div v-if="shortenedUrl" class="result">
        <p>Shortened URL: </p>
        <div class="short-url">
        <a :href="shortenedUrl" target="_blank" id="short-url"> {{ shortenedUrl }}</a>
        <button ref="copyBtn" data-clipboard-target="#short-url">Copy</button>
        </div>
        <span v-if="copied" class="copied-text">Copied!</span>
      </div>
      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </div>
</template>

<script>
import ClipboardJS from 'clipboard';
import axios from 'axios';
export default{
  data(){
    return{
      originalUrl: "",
      shortenedUrl: "",
      copied:false,
      clipboard: null,
      errorMessage: '',
    };
  },

  methods:{
    async shortenUrl() {
      this.errorMessage = '';
      this.shortenedUrl = '';
      this.copied = false;

      if (!this.originalUrl.trim()) return;

      try{
        const response = await axios.post(
          'https://api-ssl.bitly.com/v4/shorten',
          {
            long_url: this.originalUrl,
            domain: 'bit.ly',
          },
          {
            headers: {
              Authorization: 'Bearer 2442f02ab0859b904ac772d383bddc94941e48bf',
              'Content-Type' : 'application/json',
            },
          }
        );

        this.shortenedUrl = response.data.link;
        this.$nextTick(() => this.initClipboard());
      } catch (error) {
        console.error(error);
        this.errorMessage = 'Failed to shorten the URL. Please check the link on your API key';
      }
    },
    initClipboard() {
      if (this.clipboard) {
        this.clipboard.destroy();
      }

      this.clipboard = new ClipboardJS(this.$refs.copyBtn);

      this.clipboard.on('success', () => {
        this.copied = true;
        setTimeout(() => (this.copied = false), 2000);
      });

      this.clipboard.on('error', (e) => {
        console.error('Clipboard error: ', e);
      });
    }, 
  },
  beforeUnmount(){
    if (this.clipboard) {
      this.clipboard.destroy();
    }
  },
};
</script>

<style scoped>
.container{
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 8px;
}

input{
  width: 80%;
  padding: 10px;
  margin-bottom: 10px;
}

button{
  padding: 10px 15px;
  margin-left: 5px;
  cursor: pointer;
}

.result{
  margin-top: 20px;
}

.shortUrl{
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
}

.copied-text {
  color: green;
  margin-top: 10px;
  font-size: 0.9rem;
}

</style>