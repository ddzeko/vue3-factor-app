<template>
  <div class="hello">
    <h1>Brojevi za Jakova</h1>
  </div>
  <div id="container">
    <div id="left">
      <img v-on:click="number -= 1" src="./assets/btn-one-minus.png"/><br/>
      <button class="plusminus" v-on:click="myMethod($event, 'minus')">Za 10 manji</button>
    </div>
    <div id="middle">
      <input v-model="number" class="numinput" type="number" />
    </div>
    <div id="right">
      <img v-on:click="number += 1" src="./assets/btn-one-plus.png"/><br/>
      <button class="plusminus" v-on:click="myMethod($event, 'plus')">Za 10 veći</button>
    </div>
  </div>
  <div id="numfacts">
    <p>Nešto o tom broju:</p>
    <transition appear name="fade">
      <p :key="number" class="numprops">{{ numberProperties(number) }}</p>
    </transition>
    <!-- <p class="numprops">{{ numprops }}</p> --> 
  </div>
</template>

<script>
export default {
  publicPath: process.env.NODE_ENV === 'production' ? '/factor-app/' : '/',       
  name: 'App',
  methods: {
    myMethod: function myMethod(ev, op) {
      //console.log(op, ev); // MouseEvent { ... }
      if (op === 'plus') {
        this.number = 0 | this.number;
        this.number += 10;
        this.show = false;
      }
      else if (op === 'minus') {
        this.number = 0 | this.number;
        this.number -= 10;
        this.show = true;
      }
    },
  } ,
  watch: {
      number: function (value) {
          // console.log(value, this.number);
          value = 0 | value;
          if (value < 0) { this.number = 0; }
          else if (value > 1111) { this.number = 1111; }
          else { this.number = value; }
      }
  },
  computed: {
    numberProperties() { return function(arg_number) {
      arg_number = 0 | arg_number;  
      var number = arg_number ? arg_number : this.number;
      var numprops = '';
      if (number > 0) {
        if (number % 2 == 0) {
          numprops = `${number} je paran broj`;
        }
        else {
          numprops = `${number} je neparan broj`;
        }
        let je_kvadrat = '';
        let je_kubus = '';
        let djeljiv_s = [];
        for (let n = 2; n <= number / 2; n++) {
            if (number % n == 0) {
              djeljiv_s.push(n);
            }
            if (n * n == number) {
              je_kvadrat = ` i kvadrat (${n}×${n})`;
            }
            if (n * n * n == number) {
              je_kubus = ` i kub (${n}×${n}×${n})`;
            }
        }
        if (djeljiv_s.length == 0)  {
          numprops += " a i prosti broj.";
        } else {
          numprops += (", (bez ostatka) djeljiv s: " + djeljiv_s.join(","));
          if (je_kvadrat || je_kubus) {
            numprops += `, uz to ${number} je ` + je_kvadrat + je_kubus + ".";
          }
        }
      }
      else {
        numprops = "Nula je paran broj.";
      }
      return numprops;
    }
  }   
  },
  data: function () {
    return {
      // publicPath: process.env.NODE_ENV === 'production' ? '/factor-app/' : '/',      
      publicPath: '/factor-app/',
      number: 10,
      numprops: '',
      show: true
    };
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.plusminus {
  margin-right: 20px;
  margin-left: 20px;
  padding-right: 20px;
  padding-left: 20px;
  width: 200px;
}
#container {min-width: 650px; max-width: 700px; display: flex; margin:0 auto; position:relative; font-size: 0px; overflow: hidden; text-align: center;}
#left, #middle, #right {float: left; vertical-align: middle; font-size: 12px; margin: 0 auto;}
#left {flex:20;}
#middle {flex:25;}
#right {flex:20;}
.numinput { font-size: 32px; width: 220px; text-align: center;} 
.numprops { font-size: 16px; color: rgb(96, 96, 211); margin-top: 20px; font-weight: 700;}

.fade-enter-active {
  transition: opacity 1.5s ease;  
}
.fade-leave-active {
  opacity: 0;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
