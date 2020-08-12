<template>
  <div id="app">
    <container>
        <label for="commons"> commons </label>
        <input name="commons" type="number" v-model="commons" />
        <label for="uncommons"> uncommons </label>
        <input name="uncommons" type="number" v-model="uncommons" />
        <label for="rares"> rares </label>
        <input name="rares" type="number" v-model="rares" />
        <label for="mythics"> mythics </label>
        <input name="mythics" type="number" v-model="mythics" />
        <button @click="calculate()"> calculate </button>
        <h2> Total Cost (USD): 
          {{cost}}
        </h2>
        <h3> if you were to buy the following gem bundles: </h3>
        <ul>
          <li v-if="gemBundles.tiny > 0"> {{gemBundles.tiny}} $5 Gem Bundle(s) (150 gems per $1) </li>
          <li v-if="gemBundles.small > 0"> {{gemBundles.small}} $10 Gem Bundle(s) (160 gems per $1) </li>
          <li v-if="gemBundles.medium > 0"> {{gemBundles.medium}} $20 Gem Bundle(s) (170 gems per $1) </li>
          <li v-if="gemBundles.big > 0"> {{gemBundles.big}} $50 Gem Bundle(s) (184 gems per $1) </li>
          <li v-if="gemBundles.huge > 0"> {{gemBundles.huge}} $100 Gem Bundle(s) (200 gems per $1) </li>
        </ul>
        <h3> Price in gems:
          {{gems}}
        </h3>
    </container>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      commons: 0,
      uncommons: 0,
      rares: 0,
      mythics: 0,
      includeRandomMythics: false,
      gems: 0,
      cost: 0,
      gemBundles: {
        tiny: 0,
        small: 0,
        medium: 0,
        big: 0,
        huge: 0
      }
    }
  },
  methods: {
    calculate() {
      //TODO: assign each rarity a "pull chance" to simplify this
      const commons = (this.commons*5)*0.667;
      const uncommons = (this.uncommons*5)*0.8;
      const rares = (this.rares*15)*0.958;
      const mythics = (this.mythics*30)*0.958;
      console.dir({commons, uncommons, rares, mythics});

      //todo: make gem bundles into {gems, price, count} to convert this into a loop
      this.gems = Math.max(commons, uncommons, rares, mythics) * 200;
      this.gemBundles.huge = Math.floor(this.gems/20000);
      let remainder = this.gems % 20000;
      this.gemBundles.big = Math.floor(remainder / 9200);
      remainder = remainder % 9200;
      this.gemBundles.medium = Math.floor(remainder / 3400);
      remainder = remainder % 3400;
      this.gemBundles.small = Math.floor(remainder / 1600);
      remainder = remainder % 1600;
      this.gemBundles.tiny = Math.ceil(remainder / 750);

      this.cost = (this.gemBundles.huge * 100) + (this.gemBundles.big * 50) + (this.gemBundles.medium * 20) + (this.gemBundles.small * 10) + (this.gemBundles.tiny * 5);
    }
  }
}
</script>

<style>

@media (orientation: landscape) and (min-width: 1281px) {
  html {
    font-size: 1vmax;
  }
  .mobile {
    display: none;
  }
}

@media (orientation: portrait), (max-width: 1280px) {
  html {
    font-size: 3vmax;
  }
  .pc {
    display: none;
  }
}
html {
  background-color: rgb(34, 34, 34);
}
.container {
  background-color: #E6E7ED;
  color:rgb(34, 34, 34);
  margin-top: 3em;
  margin-bottom: 3em;
}

button.round-big {
  border-radius: 50%;
  height: 2em;
  width: 2em;
  font-size: 2em;
  font-weight:bolder;
  font-family:"calibri light";
  color:rgb(34, 34, 34);
}
button.round-medium {
  border-radius: 50%;
  height: 2em;
  width: 2em;
}
button.round-small {
  border-radius: 50%;
  height: 1em;
  width: 1em;
}

button {
  border: 0;
  background-color:rgba(180, 180, 180, 0.8);
}

button.big {
  width: auto;
  padding: 0.25em;
  font-size: 2em;
  font-weight:bolder;
}

button:hover {
  background-color:rgba(48, 48, 48, 0.705);
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #c3c7ca;
}
</style>
