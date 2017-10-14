<template>
  <div class="articles">
    <h2>Posts</h2>
    <form action="">
      <div class="form-group">
        <label for="">Title</label>
        <input type="text" placeholder="Title goes here" v-model="newArticle">
      </div>
      <div class="form-group">
        <input @click="addArticle"type="submit" value="Submit">
      </div>
    </form>
    <ul>
      <li v-for="post in posts" v-text="post"></li>
    </ul>

    <hr>
    <h2>Show pokemons!</h2>
    <button @click="showAll">Show them all</button>
    
    <ul class="pokes">
      <li v-for="poke in pokes">
        <a :href="'http://pokemon.wikia.com/wiki/' + poke">{{ poke }}</a>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'articles',
  data () {
    return {
      msg: 'Welcome to Your Vue.js PWA',
      newArticle: '',
      posts: [
        'Post #1',
        'Post #2'
      ],
      pokes: []
    }
  },
  methods: {
    addArticle () {
      this.posts.push(this.newArticle)
      this.newArticle = ''
    },
    showAll () {
      let that = this
      axios.get('https://pokeapi.co/api/v2/pokemon/?limit=721')
        .then(function (response) {
          console.log('It\'s workin\'')
          console.log(response.data.results)
          let pokemons = response.data.results
          for (let i = 0; i <= 720; i++) {
            console.log(pokemons[i].name)
            that.pokes.push(pokemons[i].name)
          }
        })
        .catch(function (error) {
          console.log('ERROR!')
          console.log(error)
        })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
	h1, h2 {
		font-weight: normal;
	}

	ul {
		padding: 0;
	}

	li {
		margin: 0 10px;
	}
  ul.pokes {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    list-style: none;
    margin: 0;
  }
  ul.pokes li {
    margin-top: 30px;
    background: #eee;
    width: calc(33.33% - 36px);
    padding: 8px;
    float: left;
  }

	a {
		color: #35495E;
	}

form {
	width: 300px;
	margin: 0 auto;
}
form label,
form input,
form textarea {
	display: block;
	width: calc(100% - 32px);
	padding: 8px 16px;
	outline: none;
  margin-bottom: 8px;
}
form input[type="submit"],
button {
  width: 100%;
  max-width: 300px;
  border: none;
  padding: 8px 16px;
}
</style>
