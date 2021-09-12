<template>
  <div id="app">
    <div class="app-header">
      
      <h1>Vietnamese Restaurants Directory</h1>
    </div>


    <amplify-authenticator>



      


      <div class="form-body">
        <form v-on:submit.prevent autocomplete="off">
          <div class='left'> 
            <label>Name: </label>
            <input v-model='form.name' class='input' autocomplete="off" />
          </div>
          <div class='left'>
            <label>Description: </label>
            <input v-model='form.description' class='input' autocomplete="off" />
          </div>
          <div class='left'>
            <label>Address: </label>
            <input v-model='form.address' class='input' autocomplete="off" />
          </div>
          <div class='left'>
            <label>City: </label>
            <input v-model='form.city' class='input' autocomplete="off" />
          </div>
          <div class='left'>
            <label>Zipcode: </label>
            <input v-model='form.zipcode' class='input' autocomplete="off" />
          </div>

          <div class='left'>
            <label>State: </label>
            <input v-model='form.state' class='input' autocomplete="off" />
          </div>

          <div class='left'>
            <label>Stars: </label>
            <input v-model='form.stars' class='input' autocomplete="off" />
          </div>

          <button @click='createRestaurant' class='button'>Create</button>

          <button @click='getData' class='button'>Refresh</button>
        
        </form>
      </div>


      <div class="app-body">
        <div v-if="loading" class="loading">Loading...</div>
        <div class="card-container" v-if="!loading">
          <div class="card" v-for="restaurant of sortedRestaurants" :key="restaurant.id">
            <div class="remove"><button @click='deleteRestaurant(restaurant)' class='button'>Delete</button></div>
            <div class="name">{{ restaurant.city }}</div>
            <div class="price">{{ restaurant.name }}</div>
            <div class="symbol">{{ restaurant.description }}</div>
            <div class="name">{{ restaurant.state }}</div>

          </div>
        </div>
      </div>

      <!-- <div id="LoginSignOut">
            <AmplifySignOut id="SignOutButton"/>
      </div> -->


      <div style="align-items: center;justify-content: center;">
        <div style="height:40px; width: 100px;margin-left:600px; background-color: #5900ff;">
          <amplify-sign-out></amplify-sign-out>
        </div>
      </div>
      

    </amplify-authenticator>
  </div>
</template>

<script>
import { AuthState, onAuthUIStateChange } from '@aws-amplify/ui-components'
import { API, graphqlOperation } from 'aws-amplify';
import { listRestaurants } from './graphql/queries';
import { createRestaurant, deleteRestaurant } from './graphql/mutations';
import { onCreateRestaurant, onDeleteRestaurant } from './graphql/subscriptions';

export default {
  name: 'app',
  data() {
    return {
      user: { },
      restaurants: [],
      form: { },
      loading: true
    }
  },
  computed: {
    sortedRestaurants() {
      let restaurants = [...this.restaurants];
      return restaurants.sort((a, b) => a.name.localeCompare(b.name));
    }
  },
  created() {
    // authentication state managament
    onAuthUIStateChange((state, user) => {
      // set current user and load data after login
      if (state === AuthState.SignedIn) {
        this.user = user;
        this.getData();
      }
    })
    //Subscribe to changes
    API.graphql(graphqlOperation(onCreateRestaurant))
    .subscribe((sourceData) => {
      const newRestaurant = sourceData.value.data.onCreateRestaurant
      if (newRestaurant) {
        // skip our own mutations and duplicates
        if (this.restaurants.some(r => r.id == newRestaurant.id)) return;
        this.restaurants = [newRestaurant, ...this.restaurants];
      } 
    });
    API.graphql(graphqlOperation(onDeleteRestaurant))
    .subscribe((sourceData) => {
      const deletedRestaurant = sourceData.value.data.onDeleteRestaurant
      if (deletedRestaurant) {
        this.restaurants = this.restaurants.filter((r) => r.id != deletedRestaurant.id);
      } 
    });
  },


  methods: {
    
    
    async getData() {
      try {
        this.loading = true;
        const response = await API.graphql(graphqlOperation(listRestaurants));
        this.restaurants = response.data.listRestaurants.items;
      }
      catch (error) {
        console.log('Error loading restaurants...', error);
      }
      finally {
        this.loading = false;
      }
    },


    async createRestaurant() {
      const { name, description, city } = this.form
      if (!name || !description || !city) return;
    
      const restaurant = { name, description, city, clientId: this.clientId };
      try {
        const response = await API.graphql(graphqlOperation(createRestaurant, { input: restaurant }))
        console.log('Item created!')
        this.restaurants = [...this.restaurants, response.data.createRestaurant];
        this.form = { name: '', description: '', city: '' };
      } catch (error) {
        console.log('Error creating restaurant...', error)
      }
    },


    async deleteRestaurant(restaurant) {
      if (restaurant) {
        try {
          const { id } = restaurant;
          await API.graphql(graphqlOperation(deleteRestaurant, { input: { id: id } }))
          console.log('Item deleted!')
          this.restaurants = this.restaurants.filter((r) => r.id != restaurant.id );
        } catch (error) {
          console.log('Error deleting restaurant...', error)
        }
      }
    }
  }
}
</script>

<style>

div#LoginSignOut amplify-sign-out#SignOutButton {
    min-width: 100;
    --padding: 0;
} 


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

html,
body {
  font-family: "Amazon Ember", "Helvetica", "sans-serif";
  margin: 0;
}
a {
  color: #ff9900;
}
h1 {
  font-weight: 300;
}
.app {
  width: 100%;
}
.app-header {
  color: white;
  text-align: center;
  background: linear-gradient(30deg, rgb(134, 153, 187) 55%, #8c00ff);
  width: 100%;
  margin: 0 0 1em 0;
  padding: 3em 0 3em 0;
  box-shadow: 1px 2px 4px rgba(0, 0, 0, 0.3);
}
.app-logo {
  width: 126px;
  margin: 0 auto;
}
.app-body {
  width: 60%;
  margin: 0 auto;
  text-align: center;
  min-height: 500px;
}
.form-body {
  display: flex;
  justify-content: center;
  align-items: center;
  display: -webkit-flex;
  -webkit-justify-content: center;
  -webkit-align-items: center;
  flex-direction: row;
  flex-wrap: wrap; 
}
.form-body button {
  background-color: #5900ff;
  font-size: 14px;
  color: white;
  text-transform: uppercase;
  padding: 1em;
  border: none;
  cursor: pointer;
  margin: 10px;
}
button:hover {
  opacity: 0.8;
}
.left {
  text-align: left;
}
input {
  width: 100px;
  padding: 6px;
  font-size: 14px;
  color: var(--input-color);
  background-color: var(--input-background-color);
  background-image: none;
  border: 1px solid var(--lightGrey);
  border-radius: 3px;
  box-sizing: border-box;
  margin: 10px;
}
.card-container {
  display: flex;
  justify-content: center;
  align-items: center;
  display: -webkit-flex;
  -webkit-justify-content: center;
  -webkit-align-items: center;
  flex-direction: row;
  flex-wrap: wrap;
}
.card {
  background-color: white;
  border-radius: 3px;
  box-shadow: 0 2px 8px 0 rgba(0,0,0,0.25);
  min-width: 180px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 20px;
  /* height: 100%; */
  transition: transform .2s ease, box-shadow .2s ease;
  backface-visibility: hidden;
  margin: 25px;
}
.card:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 14px 0 rgba(0,0,0,0.15);
}
.name {
  font-style: italic;
}
.symbol {
  color: #999;
}
.price, .loading {
  font-weight: bold;
  font-size: 2em;
  line-height: 0.9;
  margin: 10px;
}
.loading {
  margin-top: 35px;
}
/* remove blue highlight */
textarea:hover, 
input:hover:not([type="checkbox"]), 
textarea:active, 
input:active:not([type="checkbox"]), 
textarea:focus, 
input:focus:not([type="checkbox"]),
button:focus,
button:active,
button:hover,
label:focus,
.btn:active,
.btn.active,
select
{
  outline:0px !important;
  -webkit-appearance:none;
  box-shadow: none !important;
}
textarea {
  background-color: #eee;
  border-radius: 0 4px 4px 0;
}
textarea {
  border-radius: 4px 0 0 4px;
  border-right: 10px solid #dbdbdb;
}
.remove {
  top: -15px;
  position: relative;
  align-self: flex-end;
}
.remove button {
  background-color: #DD3F5B;
  color: white;
  border-radius: 31px;
  border: 0px;
}
.welcome {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
}
.welcome h1 {
  margin-right: 40px;
}
</style>