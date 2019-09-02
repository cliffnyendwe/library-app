<template>
    <div class="books">
        <h1 class="subheading grey--text"><v-icon>folder</v-icon>books</h1>
            <v-container class="my-5">
                <v-expansion-panel>
                    <v-expansion-panel-description v-for="book in mybooks" :key="book.author">
                        <div slot="header" class="py-1">{{ book.author }}</div>
                            <v-card>
                                <v-card-text class="px-4 grey--text">
                                    <div class="font-weight-bold">Due by {{ book.due }}</div>
                                    <div>{{ book.description }}</div>
                                </v-card-text>
                            </v-card>
                    </v-expansion-panel-description>
      </v-expansion-panel>
            </v-container>
    </div>
</template>

<script>
import db from '@/fb'

export default {
  data() {
    return {
      books: [
      ]
    }
  },
  computed: {
    mybooks() {
      return this.books.filter(book => {
        return book.book === 'User' && book.status === 'completed'
      })
    }
  },

  // Get method using firestore
created() {
  db.collection('books').onSnapshot(res =>{
    const changes = res.docChanges();
    changes.forEach(change =>{
      if (change.type === 'added'){
        this.books.push({
          ...change.doc.data(),
          id: change.doc.id
        })
      }
    })
  })
}
}
</script>
