<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Cadastro de cãe</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <div class="container">
        <div>
          <ion-col>
            <ion-item class="item">
              <ion-label position="floating">Nome do seu cão</ion-label>
              <ion-input type="text" v-model="dog.nome"></ion-input>
            </ion-item>
            <ion-item class="item">
              <ion-label position="floating">Idade do seu cão</ion-label>
              <ion-input type="number" v-model="dog.idade"></ion-input>
            </ion-item>
            <ion-button
              class="btn"
              expand="block"
              color="success"
              @click="saveDog(dog)"
              >Salvar</ion-button
            >
          </ion-col>
        </div>
      </div>
    </ion-content>
    <ion-item v-for="(dogsApp, index) in dogs" :key="index">
      <ion-card>
        <ion-card-header>
          <ion-card-title> Nome do Dog: {{ dogsApp.nome }}</ion-card-title>
          <ion-card-subtitle>Idade: {{ dogsApp.idade }} anos</ion-card-subtitle>
        </ion-card-header>
        <ion-button color="danger" @click="removeDog(dog)">Deletar</ion-button>
        <ion-button color="danger" @click="editDog(dog)">Editar</ion-button>
      </ion-card>
    </ion-item>
  </ion-page>
</template>

<script>
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonCol,
  IonItem,
  IonLabel,
  IonInput,
  IonButton,
  IonCard,
  IonCardHeader,
  IonCardSubtitle,
  IonCardTitle,
} from "@ionic/vue";
import { defineComponent } from "vue";
export default defineComponent({
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonCol,
    IonItem,
    IonLabel,
    IonInput,
    IonButton,
    IonCard,
    IonCardHeader,
    IonCardSubtitle,
    IonCardTitle,
  },

  data() {
    return {
      dogs: [],
      dog: {
        id: "",
        nome: "",
        idade: "",
      },
    };
  },
  created() {
    this.dogs = JSON.parse(localStorage.getItem("dogsApp"));
  },
  methods: {
    saveDog(dog) {
      let dogs = localStorage.getItem("dogsApp");

      dog.id = new Date().getTime();

      if (dogs) {
        dogs = JSON.parse(dogs);
        dogs.push(dog);
      } else {
        dogs = [dog];
      }

      this.dogs = dogs;

      // o atualizar local storage independemente de novo contato ou nova adição
      localStorage.setItem("dogsApp", JSON.stringify(dogs));

      location.reload();
    },
    removeDog(dogId) {
      let dogs = localStorage.getItem("dogsApp");
      
      if (!dogs) return;

      dogs = JSON.parse(dogs);

      dogs = dogs.filter((dog) => {
        return dog.id != dogId;
      });

      this.dogs = dogs;

      localStorage.setItem("dogsApp", JSON.stringify(dogs));
    },
    editDog(dog) {
      this.dog = dog;
      this.isEdit = true;
    },
  },
});
</script>


 
<style scoped>
.container {
  display: flex;
  justify-content: center;
}
.btn {
  margin: 30px 20px;
}
</style>