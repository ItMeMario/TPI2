<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Controle de Saúde</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Saúde</ion-title>
        </ion-toolbar>
      </ion-header>
      
      <div v-if="dogs.length === 0" class="ion-padding ion-text-center">
        <p>Nenhum cão cadastrado.</p>
        <p>Vá para a aba <strong>Cadastro</strong> para começar!</p>
      </div>

      <div v-else class="container">
        <ion-item>
          <ion-label position="floating">Selecione o Cão</ion-label>
          <ion-select v-model="form.dogId">
            <ion-select-option v-for="dog in dogs" :key="dog.id" :value="dog.id">
              {{ dog.nome }}
            </ion-select-option>
          </ion-select>
        </ion-item>

        <ion-item>
          <ion-label position="floating">Descrição (ex: Vacina Raiva)</ion-label>
          <ion-input v-model="form.description"></ion-input>
        </ion-item>

        <ion-item>
          <ion-label position="floating">Data</ion-label>
          <ion-datetime display-format="DD/MM/YYYY" v-model="form.date" cancel-text="Cancelar" done-text="Ok"></ion-datetime>
        </ion-item>

        <ion-button expand="block" class="ion-margin-top" @click="saveRecord" color="tertiary">Salvar Registro</ion-button>

        <ion-list class="ion-margin-top">
          <ion-list-header>
            <ion-label>Histórico</ion-label>
          </ion-list-header>
          <!-- Show newest first -->
          <ion-item v-for="(record, index) in healthRecords" :key="index">
            <ion-label>
              <h2>{{ getDogName(record.dogId) }}</h2>
              <h3>{{ record.description }}</h3>
              <p>{{ formatDate(record.date) }}</p>
            </ion-label>
            <ion-button slot="end" color="danger" fill="clear" @click="deleteRecord(index)">
               <ion-icon :icon="trash"></ion-icon>
            </ion-button>
          </ion-item>
        </ion-list>
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonItem, IonLabel, IonInput, IonSelect, IonSelectOption, IonDatetime, IonButton, IonList, IonListHeader, IonIcon } from '@ionic/vue';
import { trash } from 'ionicons/icons';
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'Tab3',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonItem, IonLabel, IonInput, IonSelect, IonSelectOption, IonDatetime, IonButton, IonList, IonListHeader, IonIcon },
  setup() {
    return { trash }
  },
  data() {
    return {
      dogs: [] as any[],
      healthRecords: [] as any[],
      form: {
        dogId: '',
        description: '',
        date: new Date().toISOString()
      }
    }
  },
  ionViewWillEnter() {
    this.loadData();
  },
  methods: {
    loadData() {
      const dogs = localStorage.getItem('dogsApp');
      if (dogs) this.dogs = JSON.parse(dogs);

      const health = localStorage.getItem('dogHealth');
      if (health) this.healthRecords = JSON.parse(health);
    },
    saveRecord() {
      if (!this.form.dogId || !this.form.description) {
        alert("Preencha o cão e a descrição!");
        return;
      }

      this.healthRecords.unshift({ ...this.form });
      localStorage.setItem('dogHealth', JSON.stringify(this.healthRecords));
      
      this.form.description = ''; 
    },
    deleteRecord(index: number) {
      if(!confirm("Tem certeza que deseja apagar?")) return;
      this.healthRecords.splice(index, 1);
      localStorage.setItem('dogHealth', JSON.stringify(this.healthRecords));
    },
    getDogName(id: any) {
      const dog = this.dogs.find(d => d.id == id);
      return dog ? dog.nome : 'Cão (removido)';
    },
    formatDate(value: string) {
      if(!value) return '';
      return new Date(value).toLocaleDateString('pt-BR');
    }
  }
});
</script>
<style scoped>
.container {
  padding: 16px;
}
</style>