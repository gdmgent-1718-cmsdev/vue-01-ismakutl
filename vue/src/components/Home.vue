<template>
  <div class="articles">
    <h2>Posts</h2>
    <form action="">
      <div class="form-group">
        <label for="">Title</label>
        <input type="text" placeholder="Title goes here" v-model="title">
      </div>
      <div class="form-group">
        <label for="">Body</label>
        <textarea rows="7" type="text" placeholder="Body goes here" v-model="body"></textarea>
      </div>
      <div class="form-group">
        <input @click="postArticle" type="submit" value="Submit">
      </div>
    </form>
    <ul class="posts">
      <li v-for="post in posts">
        <p><strong v-text="post.title"></strong></p>
        <small>published by admin on 16/10/2017</small>
        <p v-text="post.body"></p>
      </li>
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
      title: '',
      body: '',
      posts: [
        {
          title: 'Post #1',
          body: 'Curabitur arcu erat, accumsan id imperdiet et, porttitor at sem. Pellentesque in ipsum id orci porta dapibus. Vivamus magna justo, lacinia eget consectetur sed, convallis at tellus. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Donec velit neque, auctor sit amet aliquam vel, ullamcorper sit amet ligula. Praesent sapien massa, convallis a pellentesque nec, egestas non nisi. Donec sollicitudin molestie malesuada.'
        }
      ],
      pokes: []
    }
  },
  methods: {
    postArticle () {
      axios.post('http://cmsdev.localhost/node/',
        {
          '_links': {
            'type': {
              'href': 'http://cmsdev.localhost/rest/type/node/article'
            }
          },
          'title': {
            'value': 'Drupal Post!'
          },
          'type': {
            'target_id': 'article'
          }
        }, {
          headers: {
            'Accept': 'application/hal+json',
            'Content-Type': 'application/hal+json',
            'X-CSRF-Token': 'iKnpqGkq9jNSmZu-MxkAMxu_zYTJbN6bys1eL4uiBoA',
            'Authorization': 'basic Y21zZGV2LXVzZXI6Y21zZGV2LXBhc3M='
          }
        }
      )
      .then(() => {
        console.log('Posted!')
      })
      .catch(() => {
        console.log('Failed!')
      })
    //   axios({
    //     method: 'post',
    //     url: 'http://cmsdev.localhost/node/',
    //     // baseURL: 'http://cmsdev.localhost/node/',
    //     headers: {
    //       'Accept': 'application/hal+json',
    //       'Content-Type': 'application/hal+json',
    //       'X-CSRF-Token': 'CI2uo7FfX7SQ1qv4BBIUOuPxpr9aLcIbGWdQtASy-48'
    //     },
    //     auth: {
    //       username: 'cmsdev-user',
    //       password: 'cmsdev-pass'
    //     },
    //     body: {
    //       '_links': {
    //         'type': {
    //           'href': 'http://cmsdev.localhost/rest/type/node/article'
    //         }
    //       },
    //       'title': {
    //         'value': 'Drupal Post!'
    //       },
    //       'type': {
    //         'target_id': 'article'
    //       }
    //     }
    //   })
    //   .then(function (response) {
    //     console.log(response)
    //   })
    //   .catch(function (error) {
    //     console.log(error)
    //   })
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
    },
    date () {
      return console.log('test')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
	h1, h2 {
		font-weight: normal;
	}

  .articles {
    width: 100%;
    max-width: 960px;
    margin: 0 auto;
  }

	ul {
		padding: 0;
	}

	li {
		margin: 0 10px;
	}
  ul.posts li {
    padding: 16px 0;
    border-bottom: 1px solid #eee;
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
  color: white;
  width: 100%;
  max-width: 300px;
  border: none;
  padding: 8px 16px;
  background-color: #35495E;
}
form input[type="submit"]:hover,
button:hover {
  transition: cubic-bezier(0.075, 0.82, 0.165, 1);
  background-color: #2c3e50;
  cursor: pointer;
}
</style>
