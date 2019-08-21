<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
      <div>
        <div>
            <h2>Who is coming?</h2>
            <input type="text">
            <span>Well done! The safe places are: </span>
        </div>
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

  created() {
    let recomendations = [];
      for(let user of this.users) {
        console.log("THIS IS THE USER", user);
        for(let venue of this.venues) {
          console.log("THIS IS THE VENUE", venue);
          let recomendation = this.canEatAndDrinkInVenue(user,venue)
          recomendations.push(recomendation);
            console.log("THIS IS THE ARRAY OF RECOMENDATIONS", recomendations)
        }
      }
  },

  methods: {
    canEatAndDrinkInVenue(user,venue) {
      let recomendation = this.createRecomendationForUser(this.users)
      console.log("recomendationnn", recomendation)
      return recomendation
  },

  createRecomendationForUser(user) {
    const canEat = this.canEatInVenue(user.wont_eat, venue.food);
    const canDrink = this.canDrinkInVenue(user.drinks, venue.drinks);

    let recomendation = {};

    recomendation.canGo = canEat && canDrink;

    if (!recomendation.canGo) {
      if(!canEat) {
        recomendation.reason = `Theres is nothing for ${user.name} to eat`;
      } else if (!canDrink) {
        recomendation.reason = `There is nothing for ${user.name} to drink`;
      } else {
        recomendation.reason = `There is nothing for ${user.name} to drink or eat`;
      }
    }
    return recomendation;
  },
  

  canEatInVenue(wontEat, food) {
      const canEat = wontEat.some(drink => !food.includes(drink));
      console.log(`can eat in venue ${canEat}!! because ${food} contains at least one element that is not in ${wontEat}`);
    return canEat;
  },

  canDrinkInVenue(userDrinks, venueDrinks) {
    const canDrink = userDrinks.some(drink => venueDrinks.includes(drink));
    console.log(`can drink in venue ${canDrink}!! because ${venueDrinks} contains at least one element of ${userDrinks}`);
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
