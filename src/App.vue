<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
      <div id='example-3'>
        <p>Select people who are coming:</p>
        <input type="checkbox" id="john" value="John Davis" v-model="checkedNames" class="person">
        <label for="john" >John Davis</label>

        <input type="checkbox" id="gary" value="Gary Jones" v-model="checkedNames" class="person">
        <label for="gary" >Gary Jones</label>

        <input type="checkbox" id="robert" value="Robert Webb" v-model="checkedNames" class="person">
        <label for="robert">Robert Webb</label>

        <input type="checkbox" id="gavin" value="Gavin Coulson" v-model="checkedNames" class="person">
        <label for="gavin">Gavin Coulson</label>

        <input type="checkbox" id="alan" value="Alan Allen" v-model="checkedNames" class="person">
        <label for="allan">Alan Allen</label>

        <input type="checkbox" id="bobby" value="Bobby Robson" v-model="checkedNames" class="person">
        <label for="bobby">Bobby Robson</label>

        <input type="checkbox" id="david" value="David Lang" v-model="checkedNames" class="person">
        <label for="david">David Lang</label>
        <br>
        <div v-for="name in checkedNames" :key="name.name"> {{name}} </div>
      
        <button @click="recommendVenues()" id="button">Find venues to go and avoid</button>
        
        <div  v-if="result">
          <p>Places to go: </p>
          <li v-for="place in this.venuesToGo"> 
            {{place}} 
          </li>

          <p>Places to avoid: </p>
          <li v-for="place in this.venuesToAvoid"> 
            {{place.venue}} : {{place.reason}}
          </li>   
        </div>
        <div></div>
      </div>
  </div>
</template>

<script>


export default {
  name: 'app',
  components: {

  },

  data() {
    return {

      checkedNames: [], 
      filteredArray: [],
      recommendations: "",
      venuesToAvoid: "",
      venuesToGo: "",
      result: false,
    
        users: [
          {
              "name": "John Davis",
              "wont_eat": ["Fish"],
              "drinks": ["Cider", "Rum", "Soft drinks"]
          },
          {
              "name": "Gary Jones",
              "wont_eat": ["Eggs", "Pasta"],
              "drinks": ["Tequila", "Soft drinks", "beer", "Coffee"]
          },
          {
              "name": "Robert Webb",
              "wont_eat": ["Bread", "Pasta"],
              "drinks": ["Vokda", "Gin", "Whisky", "Rum"]
          },
          {
              "name": "Gavin Coulson",
              "wont_eat": [],
              "drinks": ["Cider", "Beer", "Rum", "Soft drinks"]
          },
          {
              "name": "Alan Allen",
              "wont_eat": ["Meat", "Fish"],
              "drinks": ["Soft drinks", "Tea"]
          },
          {
              "name": "Bobby Robson",
              "wont_eat": ["Mexican"],
              "drinks": ["Vokda", "Gin", "whisky", "Rum", "Cider", "Beer", "Soft drinks"]
          },
          {
              "name": "David Lang",
              "wont_eat": ["Chinese"],
              "drinks": ["Beer", "cider", "Rum"]
          } 
      ],

     venues :
          [
            {
                "name": "El Cantina",
                "food": ["Mexican"],
                "drinks": ["Soft drinks", "Tequila", "Beer"]
            },
            {
                "name": "Twin Dynasty",
                "food": ["Chinese"],
                "drinks": ["Soft Drinks", "Rum", "Beer", "Whisky", "Cider"]
            },
            {
                "name": "Spice of life",
                "food": ["Eggs", "Meat", "Fish", "Pasta", "Dairy"],
                "drinks": ["Vokda", "Gin", "whisky", "Rum", "Cider", "Beer", "Soft drinks"]
            },
            {
                "name": "The Cambridge",
                "food": ["Eggs", "Meat", "Fish", "Pasta", "Dairy"],
                "drinks": ["Vokda", "Gin", "Cider", "Beer", "Soft drinks"]
            },
            {
                "name": "Wagamama",
                "food": ["Japanese"],
                "drinks": ["Beer", "Cider", "Soft Drinks", "Sake"]
            },
            {
                "name": "Sultan Sofrasi",
                "food": ["Meat", "Bread", "Fish"],
                "drinks": ["Beer", "Cider", "Soft Drinks"]
            },
            {
                "name": "Spirit House",
                "food": ["Nuts", "Cheese", "Fruit"],
                "drinks": ["Vodka", "Gin", "Rum", "Tequila"]
            },
            {
                "name": "Tally Joe",
                "food": ["Fish", "Meat", "Salad", "Deserts"],
                "drinks": ["Beer", "Cider", "Soft Drinks", "Sake"]
            },
            {
                "name": "Fabrique",
                "food": ["Bread", "Cheese", "Deli"],
                "drinks": ["Soft Drinks", "Tea", "Coffee"]
            },
        ]
    }
  },

  updated() {
    var names = this.usersWhoComeObj();
    var selectedNames = this.selectedNames();
  },

  watch: {
    recommendations() {
      this.recommendations =  this.createArrayOfRecommendations();
    }
  },
 
  methods: {

    selectedNames() {
      let selectedNames = [];
      var names = this.usersWhoComeObj();
        for(var name of names) {
          for (var user of this.users) {
            if (user.name === name.name) {
              selectedNames.push(user);
            }
          }
        }
      return selectedNames
    },

    usersWhoComeObj() {
      let names = [];
        for (var checkedName of this.checkedNames) {
          var peopleWhoCome = {};
          peopleWhoCome.name = checkedName;
          names.push(peopleWhoCome);
        }
      return names;  
    },

    createArrayOfRecommendations() {
      let recommendations = [];
      var selectedNames = this.selectedNames();
      for (var name of selectedNames) {
        for (var venue of this.venues) {
          recommendations.push(this.canEatAndDrinkInVenue(name,venue));
        }
      }
      return recommendations
    },

    canEatAndDrinkInVenue(person,venue) {
      const canEat = this.canEatInVenue(person.wont_eat, venue.food);
      const canDrink = this.canDrinkInVenue(person.drinks, venue.drinks);
      let recommendation = this.createRecommendationForUser(person, canEat, canDrink, venue);
      return recommendation 
    },

    canEatInVenue(wontEat, food) {
      // const canEat = food.some(foodItem => !wontEat.includes(foodItem));
      const canEat = food.some(foodItem => !this.includesCaseInsensitive(wontEat, foodItem));
      return canEat;
    },

    canDrinkInVenue(userDrinks, venueDrinks) {
      // const canDrink = userDrinks.some(drinkElement => venueDrinks.includes(drinkElement));
      const canDrink = userDrinks.some(drinkElement => this.includesCaseInsensitive(venueDrinks, drinkElement));
      return canDrink;
    }, 

    includesCaseInsensitive(array, string) {
      let includes = false;
      array.some(element => {
        if (element.toLowerCase() === string.toLowerCase()) {
          includes = true;
        }
      });
      return includes;
    },

    createRecommendationForUser(person, canEat, canDrink, venue) {
      let recommendation = {};
      recommendation.canGo = canEat && canDrink;
      recommendation.name = `${person.name}`;
      recommendation.venue = `${venue.name}`;
    
      if (!recommendation.canGo) {
        if(!canEat) {
          recommendation.reason = `Theres is nothing for ${person.name} to eat`;
        } else if (!canDrink) {
          recommendation.reason = `There is nothing for ${person.name} to drink`;
        } else {
          recommendation.reason = `There is nothing for ${person.name} to drink or eat`;
        }
      }
      return recommendation;
    },

    recommendVenues() {
        let recommendations = this.createArrayOfRecommendations();
        let placesToGo = [];
        let placesToAvoid = [];
          for (var recommendation of recommendations) {
            if (recommendation.canGo) {
              this.recommendVenuesToGo(recommendation, placesToGo);
            } else {
              this.recommendVenuesToAvoid(recommendation, placesToAvoid);
            }
          } 
        this.venuesToGo = placesToGo;
        this.venuesToAvoid = placesToAvoid;
        this.result =  true
      },

      recommendVenuesToAvoid(recommendation, placesToAvoid) {
        var negativePlaces = {};
        negativePlaces.venue = recommendation.venue;
        negativePlaces.reason = recommendation.reason;
        placesToAvoid.push(negativePlaces);
      },

      recommendVenuesToGo(recommendation, placesToGo) {
        placesToGo.push(recommendation.venue);
      },
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

.person {
  margin-left: 20px;
}

#button {
  margin-top: 20px;
}
</style>
