<template>

</template>

<script lang="js">
import {db} from "../db.js"
  export default  {

    name: 'lista-notas',
    props: [],
    mounted () {
    },
    data () {
      return {
        nota: "",
        listaNotas: [],
        empiezaPor: "",
        isButtonDisabled: true,
      }
    },

    methods: {

      anadirElemento: function () {
        if (this.nota.length > 0) {
          db.collection('notas').add({
            titulo: this.nota,
            prioridad: 0,
            fecha: new Date(),
            finalizada: false
          });
        this.nota = "";
        }
      },

      borrarTarea: function (n) {
        db.collection('notas').doc(this.listaNotas[n].id).delete();
      },

      cambiarEstadoTarea: function (n) {
        this.listaNotas[n].finalizada = !this.listaNotas[n].finalizada;
        db.collection('notas').doc(this.listaNotas[n].id).update({
          finalizada: this.listaNotas[n].finalizada
        });
      },
    },
    computed: {
        
      filtrarTareas: function () {
        if (this.empiezaPor) {
          return this.listaNotas.filter(nota => {
            return nota.titulo.indexOf(this.empiezaPor) != -1;
          });
        } else {
          return this.listaNotas;
        }
      },

      tareasPendientes: function () {
        return this.listaNotas.filter(nota => !nota.finalizada).length;
      },
    },
    firestore: {
      listaNotas: db.collection('notas')
    }
}
</script>

<style>
