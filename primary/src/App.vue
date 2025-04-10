<template>
  <div class="container">
      <h1>URL Shortener</h1>
      <input v-model="originalUrl" type="text" placeholder="Enter a URL... "/>
      <button @click="shortenUrl">Shorten</button>

      <div v-if="shortenedUrl" class="result">
        <p>Shortened URL: </p>
        <div class="short-url">
        <a :href="shortenedUrl" target="_blank"> {{ shortenedUrl }}</a>
        <button @click="copzToClipboard">Copy</button>
        </div>
        <span v-if="copied" class="copued-text">Copied!</span>
      </div>
  </div>
</template>

<script>
export default{
  data(){
    return{
      originalUrl: "",
      shortenedUrl: "",
      copied:false,
    };
  },

  methods:{
    shortenUrl() {
      // Simulate URL shortening
      if (this.originalUrl.trim() === "") return;

      const randomId = Math.random().toString(36).substring(2, 8);
      this.shortenedUrl = `https://short.ly/${randomId}`
      this.copied = false;
    },
    copyToClipboard() {
      navigator.clipboard.writeText(this.shortenUrl).then(() =>{
        this.copied = true;
        setTimeout(() => {
          this.copied = false;
        }, 2000);
      });
    },
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