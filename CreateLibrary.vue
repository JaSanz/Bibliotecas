<template>
  <div class="create" >
    <input v-if="name.length===0" class="model" v-model="name" placeholder="Nombre" @keydown.space.prevent @input="characterLimitName" @paste="characterLimitName"/>
    <input v-else v-model="name" class="model" placeholder="Nombre" @input="characterLimitName" @paste="characterLimitName"/>
    <span> {{getNameTam}} / 20 </span>
    <br><br>
    <textarea v-if="description.length===0" class="model" v-model="description" placeholder="Descripción" @keydown.space.prevent @keydown.enter.prevent @input="characterLimitDescription" @paste="characterLimitDescription"/>
    <textarea v-else v-model="description" class="model" placeholder="Descripción" @keydown.enter.prevent @input="characterLimitDescription" @paste="characterLimitDescription"/>
    <span> {{getDescriptionTam}} / 200</span>
    <br><br>
    <span>¿Cómo deseas la privacidad de tu biblioteca?</span>
    <br>
    <input type="radio" id="public" value="public" v-model="privacy">
    <label for="uno">Pública: La podrá ver todo el mundo</label>
    <br>
    <input type="radio" id="private" value="private" v-model="privacy">
    <label for="Dos">Privada: Solo la podrás ver tú</label>
    <br><br>
    <button @click="createButton()" :disabled="this.name.length <= 0 || checkNames() || !checkDescription()">Crear</button>
    <button @click="cancelButton()">Cancelar</button>
  </div>
</template>

<script>
export default {
  name: 'CreateLibrary',
  props: {
    librariesNamesList: Array
  },
  data () {
    return {
      name: '',
      description: '',
      privacy: 'public'
    }
  },
  computed: {
    getNameTam () {
      return this.name.length
    },
    getDescriptionTam () {
      return this.description.length
    }
  },
  methods: {
    characterLimitName () {
      let nameAux = this.name
      let nameLength = this.name.length
      if (nameAux.length > 20) {
        nameLength -= 20
        this.name = nameAux.slice(0, -nameLength)
      }
    },
    characterLimitDescription () {
      let descAux = this.description
      let descLength = this.description.length
      if (descAux.length > 200) {
        descLength -= 200
        this.description = descAux.slice(0, -descLength)
      }
    },
    checkNames () {
      let coincidence = false
      // Comprobamos si el usuario ya tiene una biblioteca con este nombre
      for (let i = 0; i < this.librariesNamesList.length; ++i) {
        if (this.name === this.librariesNamesList[i]) {
          coincidence = true
          break
        }
      }
      // Comprobamos si tiene espacios al final
      if (!coincidence) {
        if (this.name[this.name.length - 1] === ' ') coincidence = true
      }
      return coincidence
    },
    checkDescription () {
      // Comprobamos la longitud
      if (this.description.length > 0) {
        // Comprobamos que no haya espacios al final
        if (this.description[this.description.length - 1] === ' ') return false
        // Comprobamos que no haya intros al pegar un párrafo
        for (let i = 0; i < this.description.length; ++i) {
          if (this.description[i] === '\n') {
            return false
          }
        }
      }
      return true
    },
    createButton () {
      this.$emit('create', this.name, this.description, this.privacy)
    },
    cancelButton () {
      this.$emit('cancel')
    }
  }
}
</script>

<style scoped>
.create {
  padding: 10px;
}

.frame {
  background-color: beige;
  border: 1px solid lightpink;
}
</style>
