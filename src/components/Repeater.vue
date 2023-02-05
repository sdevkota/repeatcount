<script>
export default {
  data() {
    return {
      sentence: "",
      wordMap: null,
      loading: false
    };
  },
  methods: {
    countRepeatedWords() {
      try {
        if (this.sentence) {
          this.loading = true;
          const stripped = this.sentence
            .replace(/[,.!]/g, "")
            .toLowerCase()
            .trim();
          const wordMap = stripped.split(" ").reduce((acc, rec) => {
            return { ...acc, [rec]: (acc[rec] || 0) + 1 };
          }, {});

          const sortable = Object.entries(wordMap)
            .sort(([, a], [, b]) => b - a)
            .reduce((r, [k, v]) => ({ ...r, [k]: v }), {});
          this.wordMap = Object.fromEntries(
            Object.entries(sortable).filter(([_, v]) => v != null)
          );
          this.loading = false;
        }
      } catch (error) {
        alert("Something is wrong.");
      }
    }
  }
};
</script>

<template>
  <div>
    <small>swagtheme.com</small>
    <h1 style="margin:0 !important;line-height: 1.5 !important;">
      Word<span style="color:#535bf2; ">Repeat</span>
    </h1>

    <div>
      <textarea
        style="background:#535bf2"
        rows="20"
        cols="100"
        v-model="sentence"
      />
      <div>
        <br />
        <button @click="countRepeatedWords()">Find Repeated Words</button>
        <br />
      </div>
      <div>
        <br />

        <table v-if="loading">
          <tr>
            <th>...</th>
            <th>...</th>
          </tr>
          <tr v-for="n in 5" :key="n">
            <td>...</td>
            <td>...</td>
          </tr>
        </table>

        <table v-else-if="wordMap">
          <tr>
            <th>Word</th>
            <th>Repeat</th>
          </tr>
          <tr v-for="(item, key) in wordMap" :key="key">
            <td>{{ key }}</td>
            <td>{{ item }}</td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}
button {
  border-color: #535bf2;
}
td,
th {
  border: 2px solid #535bf2;
  text-align: left;
  color: white;
  padding: 8px;
}
</style>
