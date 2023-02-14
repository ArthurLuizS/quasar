<template>
  <q-page padding>
    <!-- <q-list
      v-if="lista_pokes.length > 0"
      bordered
      separator
      style="width: 600px"
      class="body-dark"
    >
      <q-item clickable v-for="(p, i) in lista_pokes" :key="p.name">
        {{ p.name }}
        <q-btn icon="delete" @click="deletar(i)" />
      </q-item>
      <q-btn label="Limpar" color="warning" @click="limpar" />
    </q-list> -->

    <!-- 
    <q-card class="my-card">
      <q-img src="https://cdn.quasar.dev/img/parallax2.jpg">
        <div class="absolute-bottom text-subtitle2 text-center">
          Title
        </div>
      </q-img>
    </q-card> -->

    <div v-if="lista_pokes.length > 0" class="row" style="width: 100%">
      <div
        v-for="(p, i) in lista_pokes"
        :key="p.name"
        style="width: 200px; height: 100%"
        class="q-ma-xs"
      >
        <q-card>
          <q-img
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${++i}.png`"
          >
            <div class="absolute-bottom text-subtitle6 text-center">
              {{ p.name }}
            </div>
          </q-img>
        </q-card>
      </div>
    </div>
    <div v-else class="text-h4">
      Clique no botao para carregar pokemons
      <q-btn
        label="Carregar"
        color="primary"
        clickable
        @click="carregarPokemons"
      />
    </div>
  </q-page>
</template>

<script setup>
import { defineComponent } from "vue";
import { api } from "../boot/axios";
import { ref, onMounted } from "vue";
import { useQuasar } from "quasar";

const lista_pokes = ref([]);
const $q = useQuasar();
const img = ref("a");
const src = ref(1);
const url = ref("pokemon/1/");

async function carregarPokemons() {
  $q.loading.show();
  try {
    const resp = await api.get("/pokemon/?offset=0&limit=151");
    lista_pokes.value = resp.data.results;
  } catch (error) {
    $q.notify(`Erro ao carregar dados`);
  } finally {
    $q.loading.hide();
  }
}

// async function media_img(url) {
//   try {
//     const resp = await api.get(`/${url}`);
//     img.value = resp.data.sprites.front_default;
//     // console.log(img.value);
//   } catch (error) {}
// }

function limpar() {
  lista_pokes.value = [];
}
function deletar(id) {
  lista_pokes.value.splice(id, 1);
}

function regarregaimg(i) {
  img.value = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${i}.png`;
}
function qurl(url) {
  console.log(url);
}
// onMounted(() => {
//   media_img(url.value);
// });
</script>
