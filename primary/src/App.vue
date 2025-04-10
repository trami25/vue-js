<template>
  <div class="container">
      <h1>URL Shortener</h1>
      <input v-model="originalUrl" type="text" placeholder="Enter a URL... "/>
      <button @click="shortenUrl">Shorten</button>

      <div v-if="shortenedUrl" class="result">
        <p>Shortened URL: </p>
        <a :href="shortenedUrl" target="_blank"> {{ shortenedUrl }}</a>
      </div>
  </div>
</template>

<script>
export default{
  data(){
    return{
      originalUrl: "",
      shortenedUrl: "",
    };
  },

  methods:{
    shortenUrl() {
      // Simulate URL shortening
      if (this,originalUrl.trim() === "") return;

      const randomId = Math.random().toString(36).substring(2, 8);
      this.shortenUrl = `https://short.ly/${randomId}`
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
  padding: 10px 20px;
}

.result{
  margin-top: 20px;
}


</style>