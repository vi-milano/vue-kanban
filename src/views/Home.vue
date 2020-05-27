<template>
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
    </v-row>
  </v-container>
</template>

<script>
// @ is an alias to /src

import KanbanColumn from '@/components/KanbanColumn.vue'

export default {
  name: 'Home',
  components: { KanbanColumn },

  data() {
    return {
      listaA: [
        {
          text:
            'Genius is one percent inspiration and ninety-nine percent perspiration.',
          priority: 'high',
          author: 'Thomas Edison'
        },
        {
          text: 'You can observe a lot just by watching.',
          priority: 'high',
          author: 'Yogi Berra'
        },
        {
          text: 'A house divided against itself cannot stand.',
          priority: 'low',
          author: 'Abraham Lincoln'
        },
        {
          text: 'Difficulties increase the nearer we get to the goal.',
          priority: 'low',
          author: 'Johann Wolfgang von Goethe'
        },
        {
          text: 'Fate is in your hands and no one elses',
          priority: 'low',
          author: 'Byron Pulsifer'
        }
      ],
      listaB: [
        {
          text: 'Be the chief but never the lord.',
          priority: 'high',
          author: 'Lao Tzu'
        },
        {
          text: 'Nothing happens unless first we dream.',
          priority: 'medium',
          author: 'Carl Sandburg'
        },
        {
          text: 'Well begun is half done.',
          priority: 'medium',
          author: 'Aristotle'
        },
        {
          text: 'Life is a learning experience, only if you learn.',
          priority: 'low',
          author: 'Yogi Berra'
        }
      ],
      listaC: [
        {
          text: 'Self-complacency is fatal to progress.',
          priority: 'low',
          author: 'Margaret Sangster'
        },
        {
          text: 'Peace comes from within. Do not seek it without.',
          priority: 'high',
          author: 'Buddha'
        },
        {
          text: 'What you give is what you get.',
          priority: 'medium',
          author: 'Byron Pulsifer'
        },
        {
          text: 'We can only learn to love by loving.',
          priority: 'medium',
          author: 'Iris Murdoch'
        },
        {
          text: 'Life is change. Growth is optional. Choose wisely.',
          priority: 'low',
          author: 'Karen Clark'
        },
        {
          text: "You'll see it when you believe it.",
          priority: 'medium',
          author: 'Wayne Dyer'
        }
      ]
    }
  },
  methods: {
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
