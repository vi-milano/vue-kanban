<template>
  <v-app>
    <v-content>
      <!-- Provides the application the proper gutter -->
      <v-app-bar
          app
          color="primary"
          dark
          dense
        >
        <v-spacer></v-spacer>
      <v-toolbar-title>Kanban Brasil de Fato</v-toolbar-title>
        <v-spacer></v-spacer>

    </v-app-bar>
      <v-container fluid>
        <v-row justify="center" class="mx-8">
          <v-col cols="3">
            <KanbanColumn :title="'Para fazer'" :list="listaA" />
          </v-col>
          <v-col cols="3">
            <KanbanColumn :title="'Em andamento'" :list="listaB" />
          </v-col>
          <v-col cols="3">
            <KanbanColumn :title="'Concluído'" :list="listaC" />
          </v-col>
          <v-col cols="3">
            <KanbanColumn :title="'Cancelado'" :list="listaD" />
          </v-col>
        </v-row>
      </v-container>

    </v-content>
  </v-app>
</template>

<script>
import KanbanColumn from '@/components/KanbanColumn.vue'

export default {
  name: 'App',
  components: { KanbanColumn },

  data () {
    return {

      listaA: [
        {
          text: 'Genius is one percent inspiration and ninety-nine percent perspiration.',
          author: 'Thomas Edison'

        },
        {
          text: 'You can observe a lot just by watching.',
          author: 'Yogi Berra'
        },
        {
          text: 'A house divided against itself cannot stand.',
          author: 'Abraham Lincoln'
        },
        {
          text: 'Difficulties increase the nearer we get to the goal.',
          author: 'Johann Wolfgang von Goethe'
        },
        {
          text: 'Fate is in your hands and no one elses',
          author: 'Byron Pulsifer'
        }
      ],
      listaB: [
        {
          text: 'Be the chief but never the lord.',
          author: 'Lao Tzu'
        },
        {
          text: 'Nothing happens unless first we dream.',
          author: 'Carl Sandburg'
        },
        {
          text: 'Well begun is half done.',
          author: 'Aristotle'
        },
        {
          text: 'Life is a learning experience, only if you learn.',
          author: 'Yogi Berra'
        }
      ],
      listaC: [
        {
          text: 'Self-complacency is fatal to progress.',
          author: 'Margaret Sangster'
        },
        {
          text: 'Peace comes from within. Do not seek it without.',
          author: 'Buddha'
        },
        {
          text: 'What you give is what you get.',
          author: 'Byron Pulsifer'
        },
        {
          text: 'We can only learn to love by loving.',
          author: 'Iris Murdoch'
        },
        {
          text: 'Life is change. Growth is optional. Choose wisely.',
          author: 'Karen Clark'
        },
        {
          text: "You'll see it when you believe it.",
          author: 'Wayne Dyer'
        }
      ],
      listaD: [
        {
          text: 'To succeed, we must first believe that we can.',
          author: 'Michael Korda'
        },
        {
          text: 'Learn from yesterday, live for today, hope for tomorrow.',
          author: 'Albert Einstein'
        },
        {
          text: 'A weed is no more than a flower in disguise.',
          author: 'James Lowell'
        },
        {
          text: 'Do, or do not. There is no try.',
          author: 'Yoda'
        }
      ]

    }
  },
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
    }

  }
}
</script>

<style>
.drop-preview {
  background-color: rgba(177, 150, 200, 0.1);
  border: 1px dashed #abc;
  margin: 5px;
}
.card-font-size {
      font-size: 18px;
}
</style>
