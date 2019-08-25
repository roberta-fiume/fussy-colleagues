<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
      <div id='example-3'>
      <input type="checkbox" id="john" value="John Davis" v-model="checkedNames">
      <label for="john">John Davis</label>

      <input type="checkbox" id="gary" value="Gary Jones" v-model="checkedNames">
      <label for="gary">Gary Jones</label>

      <input type="checkbox" id="robert" value="Robert Webb" v-model="checkedNames">
      <label for="robert">Robert Webb</label>

      <input type="checkbox" id="gavin" value="Gavin Coulson" v-model="checkedNames">
      <label for="gavin">Gavin Coulson</label>

      <input type="checkbox" id="alan" value="Alan Allen" v-model="checkedNames">
      <label for="allan">Alan Allen</label>

      <input type="checkbox" id="bobby" value="Bobby Robson" v-model="checkedNames">
      <label for="bobby">Bobby Robson</label>

      <input type="checkbox" id="david" value="David Lang" v-model="checkedNames">
      <label for="david">David Lang</label>
      <br>
      <p>People who are coming:</p>
      <div v-for="name in checkedNames"> {{name}} </div>
    
      <button @click="recommendVenues()">Find venues to avoid</button>
       
      <p>Places to go: </p>
      <li v-for="place in this.venuesToGo"> 
        {{place}} 
      </li>

      <p>Places to avoid: </p>
      <li v-for="place in this.venuesToAvoid"> 
        {{place.venue}} : {{place.reason}}
      </li>
       


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
      console.log("PEOPLE WHO COMEEE", names); 
    var selectedNames = this.selectedNames();
      console.log("namesss", selectedNames)

  },

  watch: {
    recommendations() {
      this.recommendations =  this.createArrayOfRecommendations();
    },

    venuesToAvoid() {
      console.log("I AM CHANGEDDDDDD", this.venuesToAvoid)
    },

    venuesToGo() {
         console.log("I AM PLECES TO GO", this.venuesToGo);
    }
  },
 
  methods: {

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
      },

      recommendVenuesToAvoid(recommendation, placesToAvoid) {
        var negativePlaces = {};
        negativePlaces.venue = recommendation.venue;
        negativePlaces.reason = recommendation.reason;
        placesToAvoid.push(negativePlaces);
        console.log("THESE ARE THE PLACES TO AVOID", placesToAvoid);
      },

      recommendVenuesToGo(recommendation, placesToGo) {
        placesToGo.push(recommendation.venue);
      },

      canEatAndDrinkInVenue(person,venue) {
        const canEat = this.canEatInVenue(person.wont_eat, venue.food);
        const canDrink = this.canDrinkInVenue(person.drinks, venue.drinks);
        console.log("canEatAndDrinkInVenue", person, canEat, canDrink, venue);
        let recommendation = this.createRecommendationForUser(person, canEat, canDrink, venue);
        return recommendation 
    },

     usersWhoComeObj() {
       let names = [];
        for (var i of this.checkedNames) {
          var peopleWhoCome = {};
           peopleWhoCome.name = i;
           names.push(peopleWhoCome);
        }

        return names;  
    },

    selectedNames() {
      let selectedNames = [];
      var names = this.usersWhoComeObj();
         for(var i of names) {
            for (var j of this.users) {
              if (j.name === i.name) {
                selectedNames.push(j);
              }
            }
         }
         return selectedNames
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

    includesCaseInsensitive(array, string) {
      let includes = false;
      array.some(element => {
        if (element.toLowerCase() === string.toLowerCase()) {
          includes = true;
        }
      });
      return includes;
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
</style>
