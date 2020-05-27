<template>

    <div>
        <div class="ma-1">
            <h2>{{title}}</h2>
            <v-btn
            @click.stop="openDialog"
            block >
                <v-icon>mdi-plus</v-icon>
            </v-btn>
        </div>

        <Container
        @drop="e => onCardDrop(e, list)"
        :should-accept-drop="shouldAcceptDrop"
        :get-child-payload="e => getPayload(e, list)"
        :drag-class="'card-tilt'"
        :drop-class="'card-tilt-over'"
        :drop-placeholder="{className:'drop-preview',animationDuration: '150',
        showOnTop: true}"
    >
            <Draggable
            v-for="i in list"
            :key="i.text + 'b'"
            class="drag-item"
            >
                <v-card
                class="ma-1" >
                    <v-card-text  class="font-weight-medium card-font-size"  >
                        {{ i.text }}
                    </v-card-text>
                    <div class="pl-4 pb-4">
                   <v-avatar size="36">
                    <img
                      src="https://avatars0.githubusercontent.com/u/9064066?v=4&s=460"
                      alt="John"
                    >
                  </v-avatar>
                  <span class=" text--secondary ml-2">26 May</span>
                </div>
                </v-card>
            </Draggable>
        </Container>

        <v-dialog v-model="dialog" persistent max-width="520">

      <v-card >
        <v-card-title class="headline">Novo card: '{{ title }}'</v-card-title>

             <v-textarea
             class="mx-6"
             auto-grow
             outlined
             autofocus
             counter
             :counter-value="counterValue"
             v-model="modalContent"
             @input="updateCharCount"
            :rules="rules"

    ></v-textarea>
        <v-card-actions class="pt-0">
          <v-spacer></v-spacer>
          <v-btn  color="primary" text @click="closeDialog">Cancelar</v-btn>
          <v-btn :disabled="charCount > 250" color="primary" text @click="addNewCard">Adicionar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    </div>

</template>

<script>
import { Container, Draggable } from 'vue-smooth-dnd'

export default {
  props: ['title', 'list'],
  data () {
    return {
      charCount: 0,
      modalContent: '',
      rules: [(v = 0) => v.length <= 240 || 'Você excedeu 250 caracteres'],
      dialog: false
    }
  },
  components: { Container, Draggable },

  methods: {
    shouldAcceptDrop (sourceContainerOptions, payload) {
      return true
    },
    onCardDrop (dropResult, list) {
      const { removedIndex, addedIndex, payload } = dropResult
      console.log('On card drop', dropResult)
      // eslint-disable-next-line no-unused-expressions
      removedIndex !== null ? list.splice(removedIndex, 1) : null
      // eslint-disable-next-line no-unused-expressions
      addedIndex !== null ? list.splice(addedIndex, 0, payload) : null
    },
    getPayload (index, list) {
      // console.log('Payload WAWA')
      const { name, age } = list[index]

      console.log('Item', name, age)
      return list[index]
    },
    openDialog () {
      this.dialog = true
    },
    updateCharCount (e) {
      this.charCount = e.length
    },
    counterValue () {
      const charCount = this.charCount
      return `${charCount}/250`
    },
    addNewCard () {
      const newCard = {
        text: this.modalContent,
        author: 'User'
      }
      this.list.push(newCard)
      this.closeDialog()
    },
    closeDialog () {
      this.dialog = false
      this.charCount = 0
      this.modalContent = ''
    }
  }

}
</script>
<style lang="scss" scoped>

.drag-item{
    cursor:pointer;
}
    .card-font-size {
      font-size: 18px;
}

.card-tilt {
    animation-name: tiltIn;
    animation-duration: 500ms;
    animation-fill-mode: forwards;

}

.card-tilt-over {
    animation-name: tiltOut;
    animation-fill-mode: forwards;
}
.drop-preview {
  background-color: rgba(150, 150, 200, 0.1);
  border: 1px dashed #abc;
  margin: 5px;
}
@keyframes tiltIn {
    from { transform: rotate(0deg);}
    to { transform: rotate(8deg);}
}

@keyframes tiltOut {
    from {
        transform: rotate(8deg);
        }
    to {
        transform: rotate(0deg);

        }
}
</style>
