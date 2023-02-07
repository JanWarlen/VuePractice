<script setup>

</script>
<template>
  <div>
    <label>website:</label>
    <input placeholder="input your website" v-model="originUrl">
    <button v-on:click="generate">generate short url</button>
    <br/>
    <label>shorten url:</label>
    <a v-bind:href="shortenUrl">{{ shortenUrl }}</a>
  </div>
</template>

<script>
export default {
  data() {
    return {
      originUrl: '',
      shortenUrl: '',
    }
  },
  methods: {
    async generate() {
      let reqOpt = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": "Bearer " + import.meta.env.VITE_BITLY_TOKEN
        },
        body: JSON.stringify({
          group_guid: import.meta.env.VITE_BITLY_GROUP_ID,
          domain: "bit.ly",
          long_url: this.originUrl
        })
      };
      let response = await fetch("https://api-ssl.bitly.com/v4/shorten", reqOpt);
      let data = await response.json();
      this.shortenUrl = data.link;
    },
  }
}
</script>