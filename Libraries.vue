<template>
  <div class="CreateLibrary">
    <button v-if="!opened" class="button" @click="btnCreateNewLib()">Crear nueva biblioteca</button>
    <CreateLibrary class="create" v-else :librariesNamesList="getLibrariesNameList()" @cancel="btnCreateNewLib" @create="createLibrary"/>
    <h3 v-if="librariesList.length===0">No hay librerias creadas</h3>
    <ul v-else>
        <div class='librariesList' v-for="library in librariesList" :key="library.name">
          <h5>Nombre: {{ library.name }}</h5>
          <span v-if="library.description!==''">Descripción: {{ library.description }}</span>
          <span v-else>Sin descripción</span>
          <br>
          Privacidad: {{ library.privacy}}
        </div>
    </ul>
  </div>
</template>

<script>
import CreateLibrary from '@/components/CreateLibrary.vue'

export default {
  name: 'libraries',
  components: {
    CreateLibrary
  },
  data () {
    return {
      librariesList: [
        { name: 'prueba 1', description: '4', privacy: 'public' },
        { name: 'prueba 2', description: '', privacy: 'private' }
      ],
      opened: false
    }
  },
  methods: {
    getLibrariesNameList () {
      let nameList = new Array(this.librariesList.length)
      for (let i = 0; i < this.librariesList.length; ++i) {
        nameList[i] = this.librariesList[i].name
      }
      return nameList
    },
    btnCreateNewLib () {
      this.opened = !this.opened
    },
    createLibrary (name, description, privacy) {
      this.librariesList.push({ name: name, description: description, privacy: privacy })
      this.btnCreateNewLib()
    }
  }
}
</script>

<style scoped>
.button {
  background-color: green;
  border: 1px solid darkgreen;
}

.create {
  background-color: #E9FFE2;
  border: 1px solid #DBECD5;
}

.librariesList {
  text-align: justify;
  background-color: #E9FFE2;
  border: 1px solid #DBECD5;
  margin-top: 2px;
}
</style>
