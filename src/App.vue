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
      <input type="checkbox" id="alan" value="Alan  Allen" v-model="checkedNames">
      <label for="allan">Alan Allen</label>
      <input type="checkbox" id="bobby" value="Bobby Robson" v-model="checkedNames">
      <label for="bobby">Bobby Robson</label>
      <input type="checkbox" id="david" value="David Lang" v-model="checkedNames">
      <label for="david">David Lang</label>
      <br>
      <p>People who are coming:</p>
      <div v-for="name in checkedNames"> {{name}} </div>
    
      <button>Find venues to avoid</button>

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
    let recommendations = this.createArrayOfRecommendations();
 
      let placesToAvoid = this.tellMeVenueToAvoid(recommendations.canGo, recommendations.name, recommendations.venue, recommendations.reason);
   
     
     console.log("THESE ARE THE PLACES TO AVOID", placesToAvoid)
  },

  updated() {
    var peopleWhoCome = this.usersWhoComeObj();
      console.log("PEOPLE WHO COMEEE", peopleWhoCome); 

      var filterd = this.filterUsers();
      console.log("filteredddd",filterd)
  },
 
  methods: {

    createArrayOfRecommendations() {
      let recommendations = [];
      for(var i = 0; i < this.users.length; i++) {
        for(var j = 0; j < this.venues.length; j++) {
          recommendations.push(this.canEatAndDrinkInVenue(this.users[i],this.venues[j]));
        }
        
      }
      return recommendations
   
    },


      // tellMeVenueToAvoid(canGo, name, venue, reason) {
      //   let recommendations = this.createArrayOfRecommendations();
      //     recommendations.forEach(recommendation => {
      //       if (recommendation.canGo == false) {
      //         var placesToAvoid = {};
      //         placesToAvoid.name = recommendation.name;
      //         placesToAvoid.venue = recommendation.venue;
      //         placesToAvoid.reason = recommendation.reason;
      //       });
            
      //       return placesToAvoid
            
      // },

          tellMeVenueToAvoid(canGo, name, venue, reason) {
              let recommendations = this.createArrayOfRecommendations();
              console.log("THIS IS MY ARRAYYYYYYYYYYYYYYYYYYYYYYYYYY", recommendations);
                for (var i of recommendations) {
                  if (i.canGo == false) {
                    console.log("THIS I I.CANGO", i.canGo)
                  var placesToAvoid = {};
                  placesToAvoid.name = i.name;
                  placesToAvoid.venue = i.venue;
                   placesToAvoid.reason = i.reason;
                }
               }
                return placesToAvoid;
            },

            
          // tellMeVenueToAvoid(canGo, name, venue, reason) {
          //     let recommendations = this.createArrayOfRecommendations();
          //     console.log("THIS IS MY ARRAYYYYYYYYYYYYYYYYYYYYYYYYYY", recommendations);
          //     let placesToAvoid = [];
          //    for (var i of recommendations) {
          //       if (i.canGo == false) {
          //         placesToAvoid.push(i.name, i.venue, i.reason);
          //       }
          //     }
          //       return placesToAvoid;
          //   },


      canEatAndDrinkInVenue(user,venue) {
        const canEat = this.canEatInVenue(user.wont_eat, venue.food);
        const canDrink = this.canDrinkInVenue(user.drinks, venue.drinks);
        let recommendation = this.createRecommendationForUser(user, canEat, canDrink, venue);
        console.log("THIS IS THE RECCOMENDATION", recommendation)
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

    filterUsers() {
      var peopleWhoCome = this.usersWhoComeObj();
      let newArray= [];
      this.users.map(function(obj, index){
        for(var i of peopleWhoCome) {
           if(obj.name === i.name){
             console.log("I AM THE OBJJJ", obj)
               console.log("I AM i nameee",  i.name)
            newArray.push(obj);
          }
        }
         
      });
        return this.users = newArray;
        
            //   var peopleWhoCome = this.usersWhoComeObj();
            //   for (var i of this.users) {
            //     for (var j of peopleWhoCome) {
            //       this.users.filter(function(person) {
            //         person.name != "Allan Allen"
            //           console.log("PERSOOON of iiiiii", person)
            //             console.log("PERSOOON of jjjjj", j.name )
                
            //       })
            //     }
                
            //   }

            //  return this.users
    
    },

    createRecommendationForUser(user, canEat, canDrink, venue) {
     
      let recommendation = {};

      recommendation.canGo = canEat && canDrink;
      recommendation.name = `${user.name}`;
       recommendation.venue = `${venue.name}`;
    
      if (!recommendation.canGo) {
        if(!canEat) {
          recommendation.reason = `Theres is nothing for ${user.name} to eat`;
        } else if (!canDrink) {
          recommendation.reason = `There is nothing for ${user.name} to drink`;
        } else {
          recommendation.reason = `There is nothing for ${user.name} to drink or eat`;
        }
      }
      return recommendation;
    },
    

    canEatInVenue(wontEat, food) {
          const canEat = food.some(foodItem => !wontEat.includes(foodItem));
        // para el array userWont_Eat dame ALGUN (some) elemento que no este incluido en el array venueFood
      //  console.log(`can eat in venue ${canEat}!! because ${food} contains at least one element that is not in ${wontEat}`);
      return canEat;
    },

    canDrinkInVenue(userDrinks, venueDrinks) {
      const canDrink = userDrinks.some(drinkElement => venueDrinks.includes(drinkElement));
      // console.log(`can drink in venue ${canDrink}!! because ${venueDrinks} contains at least one element of ${userDrinks}`);
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
