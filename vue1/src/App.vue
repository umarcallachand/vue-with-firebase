<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Game Wiki</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Add Game</h3>

      </div>
      <div class="panel-body">
        <form id="form" class="blue-grey lighten-4 form-inline" v-on:submit.prevent="addGame">
          <div class="form-group md-form">
            <label for="gameName">Name:</label>
            <input type="text" id="gameName" class="form-control" v-model="newGame.name">
          </div>
          <div class="form-group md-form">
            <label for="gameRating">Rating:</label>
            <input type="text" id="gameRating" class="form-control" v-model="newGame.rating">
          </div>
          <div class="form-group md-form">
            <label for="gameUrl">URL:</label>
            <input type="text" id="gameUrl" class="form-control" v-model="newGame.url">
          </div>
          <input type="submit" class="btn btn-default" value="Add Game">
        </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h2>Games</h2>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead class="blue-grey lighten-4">
            <tr>
              <th>
                Title
              </th>
              <th>
                Rating
              </th>
              <th>
                Delete
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="game in games">
              <td>
                <a v-bind:href="game.url" target="_blank">{{game.name}}</a>
              </td>
              <td>
                {{game.rating}}
              </td>
              <td>
                <span v-on:click="removeGame(game)"><i class="fa fa-trash" aria-hidden="true">delete</i></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

  </div>
</template>

<script>

// import Hello from './components/Hello'

import Firebase from 'firebase'
import toastr from 'toastr'

let config = {
  apiKey: 'AIzaSyCuBYkRs5Ds_6lQKJu9llRxvry7o6A-3Pc',
  authDomain: 'my-app-call.firebaseapp.com',
  databaseURL: 'https://my-app-call.firebaseio.com',
  projectId: 'my-app-call',
  storageBucket: 'my-app-call.appspot.com',
  messagingSenderId: '826723794007'
}

let app = Firebase.initializeApp(config)
let db = app.database()

let GameRef = db.ref('Games')

export default {
  name: 'app',
  firebase: {
    games: GameRef
  },
  data () {
    return {
      newGame: {
        name: '',
        rating: '',
        url: ''
      }
    }
  },
  methods: {
    addGame: function () {
      GameRef.push(this.newGame)
      this.newGame.name = ''
      this.newGame.rating = ''
      this.newGame.url = ''
    },
    removeGame: function (game) {
      GameRef.child(game['.key']).remove()
      toastr.success('Game Removed!')
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#form {
  padding: 20px;
  padding-left: 130px;

}
</style>
