<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Claves</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Claves</ion-title>
        </ion-toolbar>
      </ion-header>
      <ion-grid>
        <ion-row>
          <ion-col>
            <div>Clave</div>
          </ion-col>
          <ion-col>
            <div>Status</div>
          </ion-col>
        </ion-row>
        <ion-row v-for = "(item, index) in listaClaves" :key = "index">
          <ion-col>
            <div>{{listaKeys[index]}}</div>
          </ion-col>
          <ion-col>
            <div>{{item.status}}</div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<style>
ion-col > div {
  border: solid 1px #ddd;
  padding: 10px;
}
</style>

<script>
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonCol, IonGrid, IonRow } from '@ionic/vue';
import { getDatabase, ref, onValue } from "firebase/database";

export default  {
  name: 'Tab1Page',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonCol, IonGrid, IonRow },
  mounted() {
    const db = getDatabase();
    const starCountRef = ref(db, "claves/");
    onValue(starCountRef, (snapshot) => {
      const data = snapshot.val();
      console.log(data);
      var cont = 0
      snapshot.forEach(element => {
        this.listaKeys[cont] = element.key
        this.listaClaves[cont] = element.toJSON()
        cont++
      });
    });
    console.log("Lista de Claves", this.listaClaves)
  },

  data(){ return {
    listaClaves: [{status: "", usuario: ""}],
    listaKeys:[]
  }}
};
</script>
