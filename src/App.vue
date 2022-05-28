<script>
cosnt API_URL =
`https://api.github.com/repos/vuejs/core/commits?per_page=3&sha=`

export default {
  data: ()=>({
    branches:["main","v2-compat"],
    currentBranch: "main",
    commits:null
  }),

  created(){
    this.fetchData()
  },

  watch: {
    currentBranch: "fecthdata"
  },
  methods: {
    async fetchData(){
      const url =`${API_URL}${this.currentBranch}`
      this.commits=await(await fetch(url)).json()
    },
    truncate(v){
      const newline=v.indexOf("\n")
      return newline > 0 ? v.slice(0,newline) :v
    },
    formatData(v){
      return v.replace(/T|Z/g, ' ')
    }
  }
}
</script>

<template>
<div>
  <h1>Latest Vue Core commits</h1>
  <template v-for="branch in branches">
  <div>
    <input type="radio"
    :id="branch"
    :value="branch"
    name="branch"
    v-model="currentBranch">
    <label :for="branch">{{branch}}</label>
  </div>
  </template>
  <p>Vuejs/vue@{{currentBranch}}</p>
  <ul>
    <li v-for="{heml_url,sha,author,commit} in commits">
    <a :href="html_url" target="_blank" class="commit">{{sha.slice(0,7)}}</a>
    -<span class="message">{{truncate(commit.message)}}</span><br>
    by <span class="author">
      <a :href="author_html_url" target="_blank">
      {{commit.author.name}}</a>
    </span>
    at <span class="date">{{formatData(commit.author.data)}}</span>
    </li>
  </ul>
</div>
</template>