<script setup>
import { ref, onMounted } from 'vue'
import axios from "axios";

// modèle accessible via mon template
// pour créer des variables de mon modèle, 
// j'ai JUSTE à créer des variables javascript qui prennent une valeur ref(valeur_initiale)
// qensuite, je peux accéder à cette valeur dans le template via {{nom_variable}}
const listeGenres = ref([])
const genre = ref({titre:''})

/**
 * function creer()
 * c'est ici que je vais appeler l'API (via la librairie axios) pour creer un Genre
 * si on utilise await dans notre fonction, il faut la prefixer avec : async
 */
async function  creer() {
  // j'appelle l'API avec le genre de mon modèle mis à jour via mon formulaire
  // note : on utilise await afin d'attendre la réponse de la requête avant d'executer la ligne suivante
  await axios.post('http://localhost:8080/api/genres', genre.value)

  // une fois que j'ai créé un genre
  // je vide le formulaire
  genre.value = {titre:''}
  // je recharge la liste
  init()
}

/**
 * function supprimer()
 * c'est ici que je vais appeler l'API (via la librairie axios) pour supprimer un Genre
 * si on utilise await dans notre fonction, il faut la prefixer avec : async
 */
async function  supprimer(genreId) {
  // j'appelle l'API avec l'id du genre a supprimer
  // note : on utilise await afin d'attendre la réponse de la requête avant d'executer la ligne suivante
  await axios.delete('http://localhost:8080/api/genres/' + genreId)

  // une fois que j'ai supprimé le genre => je recharge la liste
  init()
}

/**
 * function init()
 * c'est ici que je vais appeler l'API (via la librairie axios) pour aller chercher la liste des genres
 * si on utilise await dans notre fonction, il faut la prefixer avec : async
 */
async function init(){
  // j'appelle l'API
  // note : on utilise await afin d'attendre la réponse de la requête avant d'executer la ligne suivante
  const reponse = await axios.get('http://localhost:8080/api/genres')

  // je met à jour mon modèle listeGenres avec les données de l'api
  // note2 : on doit faire .value pour mettre à jour une variable de notre modèle
  listeGenres.value = reponse.data;
}

// lifecycle hooks
onMounted(() => {
  init()
})
</script>

<template>
  <br><br>
  <div class="logo">
    <img src="../assets/film9.png" class="logoIcon"/>
  </div>
  <br><h6 class="headName" ></h6><br>

  <div class="welcome-container minHeight paddBtm">
    <h5>Nouveau genre</h5>




    <form  class="w-100 ">
      <div class="form-element mb-3">
        <input
            v-model="genre.titre"
            type="text"
            class="form-control "
            placeholder="Genre">
        <span class="error" ></span>
      </div>

      <div class="form-element">
        <button type="button" @click="creer" class="btn btn-warning btn-lg w-100">Ajouter</button>
      </div>

    </form>
    <br><br>

    <img v-if="listeGenres.length == 0" src="../assets/data.png" width="300px" height="300px"/>
  <section v-if="listeGenres.length > 0">


    <table class="w-100 p-3 ">
      <thead>
      <tr>
        <th>
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-chat-dots" viewBox="0 0 16 16">
            <path d="M5 8a1 1 0 1 1-2 0 1 1 0 0 1 2 0m4 0a1 1 0 1 1-2 0 1 1 0 0 1 2 0m3 1a1 1 0 1 0 0-2 1 1 0 0 0 0 2"/>
            <path d="m2.165 15.803.02-.004c1.83-.363 2.948-.842 3.468-1.105A9 9 0 0 0 8 15c4.418 0 8-3.134 8-7s-3.582-7-8-7-8 3.134-8 7c0 1.76.743 3.37 1.97 4.6a10.4 10.4 0 0 1-.524 2.318l-.003.011a11 11 0 0 1-.244.637c-.079.186.074.394.273.362a22 22 0 0 0 .693-.125m.8-3.108a1 1 0 0 0-.287-.801C1.618 10.83 1 9.468 1 8c0-3.192 3.004-6 7-6s7 2.808 7 6-3.004 6-7 6a8 8 0 0 1-2.088-.272 1 1 0 0 0-.711.074c-.387.196-1.24.57-2.634.893a11 11 0 0 0 .398-2"/>
          </svg>
        </th>
        <th><svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-pen" viewBox="0 0 16 16">
          <path d="m13.498.795.149-.149a1.207 1.207 0 1 1 1.707 1.708l-.149.148a1.5 1.5 0 0 1-.059 2.059L4.854 14.854a.5.5 0 0 1-.233.131l-4 1a.5.5 0 0 1-.606-.606l1-4a.5.5 0 0 1 .131-.232l9.642-9.642a.5.5 0 0 0-.642.056L6.854 4.854a.5.5 0 1 1-.708-.708L9.44.854A1.5 1.5 0 0 1 11.5.796a1.5 1.5 0 0 1 1.998-.001m-.644.766a.5.5 0 0 0-.707 0L1.95 11.756l-.764 3.057 3.057-.764L14.44 3.854a.5.5 0 0 0 0-.708z"/>
        </svg></th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="genre in listeGenres">
        <td>{{genre.titre}}</td>

        <td>
            <button @click="supprimer(genre.id)" class="deleteBtn">
              <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-trash-fill" viewBox="0 0 16 16">
                <path d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5M8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5m3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0"/>
              </svg>
            </button>
        </td>
      </tr>
      </tbody>
    </table>


  </section>
  </div>
</template>