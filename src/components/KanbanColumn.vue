<template>
  <div>
    <div class="ma-1">
      <h2>{{ title }}</h2>
      <v-btn @click.stop="openDialog" block>
        <v-icon>mdi-plus</v-icon>
      </v-btn>
    </div>

    <Container
      @drop="e => onCardDrop(e, list)"
      :should-accept-drop="shouldAcceptDrop"
      :get-child-payload="e => getPayload(e, list)"
      :drag-class="'card-tilt'"
      :drop-class="'card-tilt-over'"
      :drop-placeholder="{
        className: 'drop-preview',
        animationDuration: '150',
        showOnTop: true
      }"
    >
      <Draggable v-for="i in list" :key="i.text + 'b'" class="drag-item">
        <v-card class="ma-1" :class="`priority-${i.priority}`">
          <v-card-text class="font-weight-medium card-font-size">
            {{ i.text }}
          </v-card-text>
          <div class="pl-4 pb-4">
            <v-avatar size="36">
              <img src="@/assets/user.jpg" alt="Author image" />
            </v-avatar>
            <span class=" text--secondary ml-2">{{ i.author }}</span>
          </div>
        </v-card>
      </Draggable>
    </Container>

    <v-dialog v-model="dialog" persistent max-width="520">
      <v-card>
        <v-card-title class="headline">Novo card: '{{ title }}'</v-card-title>

        <v-textarea
          ref="cardForm"
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

        <div class="mx-6 font-weight-medium">
          <span>Prioridade</span>

          <v-slider
            :ticks="'always'"
            v-model="modalPriority"
            :step="1"
            :max="2"
            :color="color"
            :track-color="'gray'"
            :tick-labels="['Baixa', 'Média', 'Alta']"
            tick-size="4"
          ></v-slider>
        </div>

        <v-card-actions class="pt-0">
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="closeDialog">Cancelar</v-btn>
          <v-btn
            :disabled="charCount <= 0 || charCount > 250"
            color="primary"
            text
            @click="addNewCard"
            >Adicionar</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import { Container, Draggable } from 'vue-smooth-dnd'

export default {
  props: ['title', 'list'],
  data() {
    return {
      charCount: 0,
      modalContent: '',
      modalPriority: 'low',
      rules: [
        (v = 0) => v.length > 0 || 'Insira algum conteúdo',
        (v = 0) =>
          (v.length <= 240 && v.length > 0) || 'Você excedeu 250 caracteres'
      ],
      dialog: false
    }
  },
  components: { Container, Draggable },
  computed: {
    color() {
      if (this.modalPriority === 0) return '#69dc2e'
      else if (this.modalPriority === 1) return '#f5b85c'
      else return 'red'
    }
  },
  methods: {
    checkPriority() {
      if (this.modalPriority === 0) return 'low'
      else if (this.modalPriority === 1) return 'medium'
      else return 'high'
    },
    shouldAcceptDrop(sourceContainerOptions, payload) {
      return true
    },
    onCardDrop(dropResult, list) {
      const { removedIndex, addedIndex, payload } = dropResult
      console.log('On card drop', dropResult)
      // eslint-disable-next-line no-unused-expressions
      removedIndex !== null ? list.splice(removedIndex, 1) : null
      // eslint-disable-next-line no-unused-expressions
      addedIndex !== null ? list.splice(addedIndex, 0, payload) : null
    },
    getPayload(index, list) {
      const { name, age } = list[index]
      console.log('Item', name, age)
      return list[index]
    },
    openDialog() {
      this.dialog = true
      this.charCount = 0
    },
    counterValue() {
      const charCount = this.charCount
      return `${charCount}/250`
    },
    updateCharCount(e = 0) {
      this.charCount = e.length
    },
    addNewCard() {
      const newCard = {
        text: this.modalContent,
        author: 'User',
        priority: this.checkPriority()
      }
      this.list.push(newCard)
      this.closeDialog()
    },
    closeDialog() {
      this.$refs.cardForm.reset()
      this.dialog = false
      this.charCount = 0
      this.modalContent = ''
      this.modalPriority = 'low'
    }
  }
}
</script>
<style lang="scss" scoped>
.priority-low {
  border-left: 4px solid #69dc2e;
}
.priority-medium {
  border-left: 4px solid #f5b85c;
}
.priority-high {
  border-left: 4px solid red;
}
.drag-item {
  cursor: pointer;
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
  animation-duration: 200ms;

  animation-fill-mode: forwards;
}
.drop-preview {
  background-color: rgba(150, 150, 200, 0.1);
  border: 1px dashed #abc;
  margin: 5px;
}
@keyframes tiltIn {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(8deg);
  }
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
