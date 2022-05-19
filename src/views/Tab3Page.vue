<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Generar Claves</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-item>
        <ion-label>Numero de Claves Nuevas</ion-label>
        <ion-input
          Type = "number"
          min = "1"
          :value = "claves"
          @ionInput="claves = parseInt($event.target.value)"
          ></ion-input>
      </ion-item>
      <ion-button expand = "block" @click = "agregarClaves()"
        >Agregar claves</ion-button>
      <alert-controller></alert-controller>
    </ion-content>
  </ion-page>
</template>

<script>
import { defineComponent } from 'vue';
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, alertController, IonButton, IonItem, IonInput, IonLabel } from '@ionic/vue';
import { getDatabase, ref, push } from "firebase/database";

export default defineComponent({
  name: 'Tab3Page',
  components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, alertController, IonButton, IonItem, IonInput, IonLabel },
  data() {
    return {
      claves: 1,
    };
  },
  methods: {
    async agregarClaves() {
      const db = getDatabase();
      var i;
      var errores = 0;
      for (i = 0; i < this.claves; i++) {
        push(ref(db, "claves/"), {
          status: "",
          usuario: "",
        })
         .then(async () => {
           //Data saves
         })
         .catch(async (error) => {
           console.log(error);
           errores++;
         });
      }

      if (errores > 0) {
        const alert = await alertController.create({
          cssClass: "Clase Claves no Agregadas",
          header: "Claves No Agregadas",
          subheader: "Error",
          message: "No se agregaron las claves",
          button: ["Aceptar"],
        });
        await alert.present();

        const { role } = await alert.onDidDismiss();
        console.log("onDidDismiss resolved with role", role);
      }
    },
  },
});
</script>
