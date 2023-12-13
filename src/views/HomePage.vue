<template>
  <ion-content>
    <ion-list>
      <ion-item v-for="(pokemon, index) in pokemons" :key="index">
        <ion-card>
          <ion-card-header>
            <ion-card-title>{{ pokemon.name }}</ion-card-title>
            <ion-card-subtitle>Experience: {{ pokemon.base_experience }}</ion-card-subtitle>
          </ion-card-header>

          <ion-card-content>
            Weight: {{ pokemon.weight }}kg. Height: {{ pokemon.height }}cm.
          </ion-card-content>
          <ion-card-content>
            Abilities: .
          </ion-card-content>
        </ion-card>
      </ion-item>
    </ion-list>
    <ion-infinite-scroll @ionInfinite="ionInfinite">
      <ion-infinite-scroll-content></ion-infinite-scroll-content>
    </ion-infinite-scroll>
  </ion-content>
</template>

<script>
import { IonContent, IonInfiniteScroll, IonInfiniteScrollContent, IonList, IonItem, IonAvatar, IonLabel, IonCard, IonCardContent, IonCardHeader, IonCardSubtitle, IonCardTitle } from '@ionic/vue';

export default {
  components: {
    IonContent,
    IonInfiniteScroll,
    IonInfiniteScrollContent,
    IonList,
    IonItem,
    IonAvatar,
    IonLabel,
    IonCard, IonCardContent, IonCardHeader, IonCardSubtitle, IonCardTitle
  },
  data() {
    return {
      pokemons: [],
      offset: 0,
    };
  },
  methods: {
    async fetchPokemons() {
      try {
        const response = await fetch(`https://pokeapi.co/api/v2/pokemon?limit=50&offset=${this.offset}`);
        const data = await response.json();
        // console.log('data', data)
        // data solo tiene el nombre del pokemon y la url a la api con su id, tengo que fetchear también esa data.

        const pokemonDetailsPromises = data.results.map(async (result) => {
          const pokemonResponse = await fetch(result.url);
          return await pokemonResponse.json();
        });

        const pokemonDetails = await Promise.all(pokemonDetailsPromises);
        console.log('details', pokemonDetails)

        // para obtener un orden aleatorio de los pokemons
        const shuffledPokemonDetails = pokemonDetails.sort(() => Math.random() - 0.5);

        this.pokemons = [...this.pokemons, ...shuffledPokemonDetails];

        // Aumento el offset para fetchear los siguientes 50
        this.offset += 50;
        // console.log(this.offset)
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
    ionInfinite(ev) {
      this.fetchPokemons();
      setTimeout(() => ev.target.complete(), 500);
    },
  },
  mounted() {
    this.fetchPokemons();
  },
};
</script>
