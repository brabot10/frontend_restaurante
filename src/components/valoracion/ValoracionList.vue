<script setup lang="ts">
import type { Valoracion } from '@/models/valoracion'
import { onMounted, ref } from 'vue'
import http from '@/plugins/axios'
import router from '@/router'

const props = defineProps<{
  //esto se copia 7-11
  ENDPOINT_API: string //variable que vien del view/valoracion
}>()

const ENDPOINT = props.ENDPOINT_API ?? ''
var valoraciones = ref<Valoracion[]>([]) //creamos la variable plural quie tomara loscalores de models/valoraciones

async function getValoracions() {
  valoraciones.value = await http.get(ENDPOINT).then((response) => response.data) //para listar hace get del backend
}

function toEdit(id: number) {
  router.push(`/valoraciones/editar/${id}`) //me direcciona a la url delswagger depath
}

async function toDelete(id: number) {
  var r = confirm('¿Está seguro que se desea eliminar la valoracion?')
  if (r == true) {
    await http.delete(`${ENDPOINT}/${id}`).then(() => getValoracions())
  }
}

onMounted(() => {
  getValoracions() //esto me llama para que cuando yo entre a valoraciones me muestre los datos
})
</script>

<template>
  <div class="container">
    <!--div general con bootstrap con diseño-->
    <nav aria-label="breadcrumb">
      <!--//clase propias de botstrap-->
      <ol class="breadcrumb">
        <!--//clase propias de botstrap-->
        <li class="breadcrumb-item"><RouterLink to="/">Inicio</RouterLink></li>
        <!--//clase propias de botstrap-->
        <li class="breadcrumb-item active" aria-current="page">Valoracion</li>
        <!--//clase propias de botstrap-->
      </ol>
    </nav>

    <div class="row">
      <h2>Lista de Valoracions</h2>
      <div class="col-12">
        <RouterLink to="/valoraciones/crear"
          ><!--Enlace deswwgaer para crear-->
          <font-awesome-icon icon="fa-solid fa-plus" />Crear Nueva Valoracion<!--implmetacion del icono-->
        </RouterLink>
      </div>
    </div>

    <div class="table-responsive">
      <!--tablas propias de bottstrap-->
      <table class="table table-bordered">
        <thead>
          <tr>
            <th scope="col">N°</th>
            <th scope="col">Descripción</th>
            <th scope="col">IdPlatillo</th>
            <th scope="col">IdPedido</th>
            <th scope="col">Editar/Cancelar</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(valoracion, index) in valoraciones" :key="valoracion.id">
            <!--el singular solo es una variable-->
            <th scope="row">{{ index + 1 }}</th>
            <!--cuando el intex comienza en 0 le damos mas 1-->
            <td>{{ valoracion.descripción }}</td>
            <td>{{ valoracion.idPlatillo }}</td>
            <td>{{ valoracion.idRepartidor }}</td>
            <td>
              <button class="btn text-success" @click="toEdit(valoracion.id)">
                <font-awesome-icon icon="fa-solid fa-edit" />
              </button>
              <button class="btn text-danger" @click="toDelete(valoracion.id)">
                <font-awesome-icon icon="fa-solid fa-trash" />
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped></style>
