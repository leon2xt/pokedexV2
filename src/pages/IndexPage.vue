<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h3>{{ id }} - {{ name }}</h3>
      <q-img :src="url" width="240px" />
    </div>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Digite o nome de um Pokémon!" />
      <q-btn
        style="background: goldenrod; color: white"
        label="Pesquisar"
        @click="getPokemon"
      />
    </div>
    <!--Setas-->
    <div class="row justify-between absolute full-width container-arrows">
      <!-- Esquerdo-->
      <q-icon
        name="fa-solid fa-caret-left"
        color="primary"
        class="q-ml-sm cursor-pointer"
        size="50px"
        @click="getPokemon(id - 1)"
      >
        <q-tooltip transition-show="flip-left" transition-hide="flip-right">
          Anterior
        </q-tooltip>
      </q-icon>
      <!-- Direito-->
      <q-icon
        name="fa-solid fa-caret-right"
        color="primary"
        class="q-mr-sm cursor-pointer"
        size="50px"
        @click="getPokemon(id + 1)"
      >
        <q-tooltip transition-show="flip-right" transition-hide="flip-left">
          Próximo
        </q-tooltip>
      </q-icon>
    </div>
  </q-page>
</template>

<script>
import api from "../services/api";

export default {
  name: "IndexPage",

  data() {
    return {
      name: "",
      url: "",
      id: null,
      search: "bulbasaur",
    };
  },
  async beforeMount() {
    await this.getPokemon();
  },
  methods: {
    getPokemon(id) {
      api
        .get(id > 0 ? `/pokemon/${id}` : `/pokemon/${this.search}`)
        .then((response) => {
          // handle success
          this.id = response.data.id;
          this.name = response.data.name;
          this.search = response.data.name;
          this.url = response.data.sprites.other.dream_world.front_default;
        })
        .catch((error) => {
          // handle error
          this.triggerNegative();
        });
    },
    triggerPositive() {
      this.$q.notify({
        type: "positive",
        position: "top",
        message: `Pokémon encontrado!`,
      });
    },

    triggerNegative() {
      this.$q.notify({
        type: "negative",
        position: "top",
        message: `Pokémon não encontrado, verifique a ortografia. Nome tem que estar com a letra minuscula.`,
      });
    },
  },
};
</script>
<style lang="scss" scoped>

.container-arrows {
}

</style>
