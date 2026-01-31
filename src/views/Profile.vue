<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Cadastro de Cães</ion-title>
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
              @click="saveDog()"
              >Salvar</ion-button
            >
          </ion-col>
        </div>
      </div>
    </ion-content>
    <ion-item v-for="(dogItem, index) in dogs" :key="index">
      <ion-card>
        <ion-card-header>
          <ion-card-title> Nome do Dog: {{ dogItem.nome }}</ion-card-title>
          <ion-card-subtitle>Idade: {{ dogItem.idade }} anos</ion-card-subtitle>
        </ion-card-header>
        <ion-button color="danger" @click="removeDog(dogItem)">Deletar</ion-button>
        <ion-button color="primary" @click="editDog(dogItem)">Editar</ion-button>
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
    this.loadDogs();
  },
  methods: {
    loadDogs() {
      const dogs = localStorage.getItem("dogsApp");
      if (dogs) {
        this.dogs = JSON.parse(dogs);
      }
    },
    saveDog() { // Removed argument, uses this.dog
      const dogToSave = { ...this.dog }; // Clone to avoid binding issues

      if (!dogToSave.nome || !dogToSave.idade) {
        // Basic validation could go here
        return; 
      }

      const allDogs = this.dogs;

      if (dogToSave.id) {
        // Edit existing
        const index = allDogs.findIndex(d => d.id === dogToSave.id);
        if (index > -1) {
          allDogs[index] = dogToSave;
        }
      } else {
        // New dog
        dogToSave.id = new Date().getTime();
        allDogs.push(dogToSave);
      }

      this.dogs = allDogs;
      localStorage.setItem("dogsApp", JSON.stringify(allDogs));

      // Reset form
      this.dog = { id: "", nome: "", idade: "" };
    },
    removeDog(dogToRemove) {
      if (!confirm(`Deseja deletar ${dogToRemove.nome}?`)) return;

      this.dogs = this.dogs.filter((d) => d.id !== dogToRemove.id);
      localStorage.setItem("dogsApp", JSON.stringify(this.dogs));
    },
    editDog(dogToEdit) {
      this.dog = { ...dogToEdit }; // Copy to form
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