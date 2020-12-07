<template>
  <div
    class="flex items-center justify-center h-screen"
    :style="{ backgroundColor: currentQuote.color }"
  >
    <div class="fixed top-10 right-8">
      <select
        id="quote-author"
        class="w-56 px-4 py-2 bg-white outline-none"
        v-model="currentAuthor"
        @change="setCurrentQuote"
      >
        <option value="">Select author</option>
        <option
          v-for="(author, index) in authors"
          v-bind:value="author"
          :key="index"
          >{{ author }}</option
        >
      </select>
    </div>
    <div
      id="quote-box"
      class="w-10/12 px-8 py-6 bg-white rounded sm:w-7/8 md:w-3/4 lg:w-1/2"
      :style="{ color: currentQuote.color }"
    >
      <div
        id="text"
        class="my-4 text-2xl"
        :style="{ color: currentQuote.color }"
      >
        {{ currentQuote.quote }}
      </div>
      <div id="author" class="text-right">- {{ currentQuote.author }}</div>
      <div class="flex items-center justify-between mt-4">
        <div>
          <a
            id="tweet-quote"
            target="_blank"
            rel="nofollow"
            :href="tweetQuoteLink(currentQuote)"
            :style="{ backgroundColor: currentQuote.color }"
            class="px-4 py-3 mr-2 text-white rounded-full focus:outline-none"
          >
            Tweet
          </a>

          <button
            id="copy-quote"
            :style="{ backgroundColor: currentQuote.color }"
            class="px-4 py-3 text-white rounded-full focus:outline-none"
            @click="copyQuote"
          >
            Copy
          </button>
        </div>

        <button
          href="javascript:void(0)"
          @click="setCurrentQuote"
          id="new-quote"
          class="px-4 py-3 text-white rounded-full focus:outline-none"
          :style="{ backgroundColor: currentQuote.color }"
        >
          New Quote
        </button>
      </div>
    </div>
  </div>
</template>

<script>
const quotes = require("./assets/quotes.json");

export default {
  name: "App",
  data() {
    return {
      quotes,
      currentQuote: {},
      authors: [],
      currentAuthor: "",
    };
  },
  methods: {
    random(items) {
      return items[Math.floor(Math.random() * items.length)];
    },
    setCurrentQuote() {
      if(this.currentAuthor != "") {
        this.currentQuote = this.random(this.quotes.filter(quote => quote.author == this.currentAuthor));
      } else {
        this.currentQuote = this.random(this.quotes);
      }
      this.currentQuote["color"] = this.generateRandColor();
    },
    setAuthors() {
      this.authors = [...new Set(this.quotes.map((item) => item.author))];
    },
    tweetQuoteLink(obj) {
      let encoded_text = encodeURIComponent(`${obj.quote} ${obj.author}`);
      return `https://twitter.com/intent/tweet?hashtags=quotes&related=freecodecamp&text=${encoded_text}`;
    },
    generateRandColor() {
      let color = "#";
      for (let i = 0; i < 6; i++) {
        color += Math.floor(Math.random() * 10);
      }
      return color;
    },
    copyQuote() {
      let quoteText = `${this.currentQuote.quote} -${this.currentQuote.author}`;
      this.$copyText(quoteText).then(
        (e) => {
          alert("Copied");
          console.log(e);
        },
        (e) => {
          alert("Cannot copy");
          console.log(e);
        }
      );
    },
  },
  mounted() {
    this.setAuthors();
    this.setCurrentQuote();
  },
};
</script>

<style></style>
